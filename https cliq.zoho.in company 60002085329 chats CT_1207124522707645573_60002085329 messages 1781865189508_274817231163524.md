





https://cliq.zoho.in/company/60002085329/chats/CT_1207124522707645573_60002085329/messages/1781865189508_274817231163524




# Findings — Large audio files displayed and played as video

**Ticket:** "Large Audio Files Automatically Converted to Video"
**Component:** Chat attachments — outgoing media classification
**Status:** Root cause identified; reporter's exact file format unconfirmed

---

## Summary

Audio files sent through Arattai can be renamed to a video extension during the file-import step. The message type is derived from that extension, so the attachment is stored, sent, and rendered as a video. Because the wrong MIME type is what goes up to the server, the recipient's device classifies it as video too — which is why the reporter sees it on both the send and receive side.

This is not a rendering bug. The message is genuinely typed `Video` in the database and in the server payload.

---

## The chain

### 1. Message type comes from the file extension, nothing else

`app/src/main/kotlin/com/arattai/chats/domain/usecases/SendAttachmentUseCase.kt:101`

```kotlin
val mimeType = file.mimeType.takeIf { type -> type != "application/octet-stream" }
    ?: "file/${file.extension}"
```

stored as both `content` and `contentType` on the attachment (`:125-126`).

`app/src/main/kotlin/com/arattai/chats/presentation/utils/FileUtil.kt:135`

```kotlin
val File.mimeType: String
    get() = MimeTypeMap.getSingleton().getMimeTypeFromExtension(extension)
        ?: "application/octet-stream"
```

`app/src/main/kotlin/com/arattai/chats/data/repository/DefaultChatsRepository.kt:4877-4884`

```kotlin
val mimeType = message.content ?: message.contentType
if (mimeType?.contains("image/", …) == true && !mimeType.contains("svg", …)) Message.Type.Image
else if (mimeType?.contains("video/", …) == true) Message.Type.Video
else if (mimeType?.contains("audio/", …) == true) Message.Type.Audio
else Message.Type.File
```

The same extension-only derivation is repeated on the forward path (`DefaultChatsRepository.kt:10458-10501`).

### 2. The extension is rewritten during import

`FileUtil.copyToTemp()` renames the temp copy after the stream is written — both overloads:

- `app/src/main/kotlin/com/arattai/chats/presentation/utils/FileUtil.kt:65` (content-URI path)
- `app/src/main/kotlin/com/arattai/chats/presentation/utils/FileUtil.kt:93` (file-path path)

`app/src/main/kotlin/com/arattai/commons/ktx/FileExtensions.kt:47-66`

```kotlin
fun File.renameWithCorrectExtension(): File {
    val androidMimeType = MimeTypeMap.getSingleton().getMimeTypeFromExtension(extension)
    val transformerMimeType = mimeType          // arattaix.media.transformer
    if (androidMimeType == transformerMimeType || transformerMimeType == "application/octet-stream") return this
    val fileExtension = MimeTypeMap.getSingleton().getExtensionFromMimeType(transformerMimeType)
    …renameTo("$nameWithoutExtension.$fileExtension")
}
```

The two MIME sources only diverge when `MimeTypeMap` doesn't recognise the extension — the transformer's own `getFileMimeType` also starts from `MimeTypeMap` and only falls through to header sniffing on `application/octet-stream`. So this rename fires exactly for files whose extension Android doesn't know.

### 3. The header sniffer classifies audio containers as video

Decompiled from `arattaix.media.transformer` **1.0.0-beta1** (`gradle/libs.versions.toml:15`), `TransformerUtils.detectFileType` — the signature tables are matched **photo → video → audio, first match wins**, and the video table is reached first with signatures that are not video-exclusive:

| Header bytes | ASCII | Video table returns | Also the container for |
|---|---|---|---|
| `66747970` | `ftyp` | `mp4` / `mov` / `3gp` | **every M4A / M4B / AAC-in-MP4 audio file** |
| `4F676753` | `OggS` | `ogg` (as video) | Ogg Vorbis / Opus audio |
| `52494646` | `RIFF` | `avi` | WAV, AIFF |
| `00000018`, `00000020`, `00000014` | ISO-BMFF box size | `mp4` / `mov` | M4A and friends |

`OggS` and `RIFF` appear in the audio table too (`ogg`, `aiff`), but those entries are unreachable — the video table is consulted first.

**Net effect:** an audio file arrives as `recording.m4b` → `MimeTypeMap` returns `null` → transformer sniffs `ftyp` → `video/mp4` → file renamed `recording.mp4` → `content = "video/mp4"` → `Message.Type.Video`.

### 4. It propagates to the recipient

The bad MIME is sent to the server as `content`. On receive, `app/src/main/kotlin/com/arattai/chats/data/utils/ChatsDataUtils.kt:88-94` classifies from that same field, so the recipient independently arrives at `Video`.

---

## Reproduction (predicted)

1. Put an audio file with an extension `MimeTypeMap` doesn't know on the device — `.m4b`, `.oga`, `.aax`, `.opus` on older API levels, or a file with no extension at all.
2. Attach it in a chat via the document/file picker.
3. The attachment shows a video icon, opens in the video player, and appears the same way for the recipient.

Known-good extensions (`.mp3`, `.m4a`, `.wav`, `.aac`, `.ogg`, `.flac`) are unaffected — `MimeTypeMap` resolves them and the rename short-circuits.

---

## On the "large file" correlation — unverified

Nothing in the send path branches on audio file size. The only size-sensitive audio code is the compression gate at `app/src/main/kotlin/com/arattai/attachments/data/repository/DefaultAttachmentsRepository.kt:1861-1935`, and it writes its output back over `finalFile`, which keeps the original filename — so it cannot change the message type.

My reading is that size is incidental rather than causal: large audio files are typically music and audiobook files (M4A/M4B/OGG) pulled from a file manager, while short ones are voice notes and `.mp3`, whose extensions resolve cleanly. **This should be confirmed against the reporter's actual file before the ticket is closed** — if they hit this with a plain `.mp3`, there is a second cause not yet found.

---

## Impact

- Wrong icon and wrong player for affected audio attachments, for sender and recipient.
- The file on disk is renamed, so the wrong extension persists into the media store and into any later export or forward.
- Affects media-gallery grouping, which reads the same type (`MessageUiUtils.kt:1998`).
- No data loss — the bytes are untouched; only the extension and type label are wrong.

---

## Recommended fix

Container signatures cannot distinguish audio from video — only the track list can. `com.zoho.media.ZohoMedia.getTrackInfo` is already available in the codebase and used for diagnostics at `DefaultAttachmentsRepository.kt:1967`.

In `renameWithCorrectExtension()`: when the sniffer returns `video/*`, check for an actual video track. If there is none, either map to the audio equivalent (`m4a` / `ogg` / `wav`) or leave the extension untouched. Leaving it untouched is the lower-risk option — the file falls back to `Message.Type.File`, which is wrong but not misleading, and no file on disk gets renamed.

The ordering bug in `arattaix.media.transformer`'s signature tables is worth reporting upstream separately: every consumer of that library misclassifies MP4-container audio, not just Arattai.

---
