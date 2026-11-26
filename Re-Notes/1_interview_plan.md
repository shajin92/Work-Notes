# RevenueCat: hiring manager call, the plan

Only what you need for this one call. Pay, contract, later rounds: later.

Full answer texts are in `revenuecat_interview_prep.md`; product knowledge is in `revenuecat_dse_knowledge.md`.

---

## The call

| | |
|---|---|
| When | **Thursday 10 Dec 2026, 7:30–8:15 PM IST** (2:00–2:45 PM UK time). Confirmed 26 Nov |
| Who | Guilherme Mota, Developer Support Engineer, Portugal. Ten years as a mobile engineer, started on Android |
| How long | 45 minutes, **Zoom** (link and passcode in your calendar invite), recorded with BrightHire |
| Type | Conversation, no coding. "A casual chat with some questions relevant to the position" (a hired candidate) |
| Not allowed | Your own recording or AI tools during the call |

## What he's checking

1. **Can you do the job?** Debugging with little information, Android knowledge, real RevenueCat experience.
2. **Can you explain clearly?** The whole job is writing and talking to developers.
3. **Do you fit their values?** Customer Obsession, Always Be Shipping, Own It, Balance.
4. **Are you honest and self-aware?** Real stories, real mistakes.

## How the 45 minutes will probably go

| Minutes | Part | You |
|---|---|---|
| 0–5 | Hello, he explains the role | Listen; note anything to ask about |
| 5–8 | "Tell me about yourself" | 90 seconds, no more |
| 8–30 | Your experience and stories | 1–2 minutes per answer, STAR order |
| 30–35 | How the job works day to day | Listen, ask follow-ups |
| 35–45 | Your questions | Ask 3 or 4 |

---

## The answers to have ready

Practise these aloud. Each one: what happened, what you did, what changed.

| # | Question | Your answer in one line | Ready? |
|---|---|---|---|
| 1 | Tell me about yourself | Android at Zoho on Arattai; best at investigation; built Health Decoder with RevenueCat | Yes |
| 2 | Why are you switching roles? / Why support and not pure Android? | The work I enjoy most is investigating and explaining; here it's the whole job, and I keep coding | Full answer below |
| 3 | Why RevenueCat? | I've been on the developer's side; your customers are people like me | Yes |
| 4 | What would you bring? | Debugging with little information, Android and KMP, India and APAC payments | Yes |
| 5 | Experience with RevenueCat / in-app purchases? | Health Decoder integration, and the three things I learned the hard way | Yes |
| 6 | A difficult support case | The contacts bug: logs, the exact app version, the 500-contact page, workaround then fix | Needs: user numbers |
| 7 | Going out of your way | The sync loop uploading 80–100 times a day | Yes |
| 8 | A time you gave feedback | Told the server team their encrypted send endpoint rejected a field the plain one accepted, with the code path and a suggested fix | Draft below: **confirm you raised it** |
| 9 | A mistake you made (Own It) | My emoji-size change crashed in release 1.29.0; I fixed it the same day | Draft below: **confirm how it was spotted** |
| 10 | Something you shipped fast (Always Be Shipping) | Server reported OTP call failures; I scoped the fix to one gate and it was in the fast-track release the same afternoon | Draft below |
| 11 | A new problem: how do you handle it? | Reply fast, gather logs and versions, reproduce, escalate with everything, keep them updated, write it up | Yes |
| 12 | Telling a developer their way isn't recommended | Ask why first, show the better way with an example, respect their choice | Yes |
| 13 | Is the customer always right? | No, but their problem is always real | Yes |
| 14 | Remote work experience? | Not full-time remote, but I already work through writing | Check the farm-client detail |
| 15 | Notice period? | 15 days at Zoho | Yes |
| 16 | A project you're proud of / tell me about something technical you built | The queue-based download engine (the filetransfer module) | Draft below: **confirm the why and the status** |

## Answer 2 in full: "Why are you switching roles?"

He will almost certainly ask this, because he made the same switch. Keep it positive, about what you're moving **towards**, never about what's wrong at Zoho.

> At Zoho, the work I look forward to most isn't building a new screen. It's when someone says "this user's contacts are wrong and we don't know why", and I trace it through the app and the server logs until I find the real cause, then explain it so the right team can fix it. That's become most of what I'm good at.
>
> In this role, that's the job itself, not the part I fit in between features. And it's still engineering: I'd be reading code, debugging Android apps and building tools, just pointed at helping other developers instead of one app.
>
> I'm also ready for a change of environment after [four] years at Zoho on one product. Working with developers from many countries, on a product I've used myself, is where I'll learn the most over the next few years.

**If he follows up:**

- **"Won't you miss building features?"**
  > Some, yes. But your blog says DSEs build internal tools and apps to reproduce issues, and I'll keep building my own apps. I'm not stopping coding; I'm changing what it's for.
- **"Is this a step back from a developer role?"**
  > I don't see it that way. Debugging someone else's integration with only their logs is harder than debugging my own code. And the people who've done this role at RevenueCat went on to lead the team or into technical account management.
- **"Why leave Zoho?"**
  > I've learned a lot there and I'm grateful for it. I joined through Zoho Schools and grew into the role. But it's been [four] years on one product, and I want to work with many apps and a global team.
- **"How long would you stay?"**
  > I'm looking for somewhere to grow for the next several years, not a short stop.

**Check:** "four years" counts from August 2022, which overlapped with Zoho Schools. If you only started on Arattai later, say the real number, e.g. "over two years on Arattai, four at Zoho".

**Don't say:**
- Anything negative about Zoho, your team or your manager.
- Money as the reason. It's fine that it matters, but it's not the answer to this question.
- "I'm bad at DSA" or "developer interviews are hard". True or not, it sounds like running away.
- "Support is easier."

## Stories 8, 9 and 10 (drafted from your own records, 25 Nov)

Found in your git history and past Claude Code sessions. Everything below is what the records show; the [brackets] are what they don't, so fill them from memory. Tell them as **your** decisions (what you checked, how you scoped it), not who typed the code. Keep them general: no names of colleagues, user IDs or internal links.

### 8. A time you gave feedback

**What the records show (19 Nov 2026):** while building multi-contact sharing, the encrypted send failed because the server's encrypted endpoint rejected a `message_id` field the plain endpoint accepts. You checked the server code yourself, found the endpoint's request template only allowed a few keys, drafted a fix based on how another endpoint already handled it, and the server team said they would fix it. You then cross-checked the whole client API against the server and listed the other mismatches.

> While building contact sharing, encrypted sends kept failing with an "extra key" error. Instead of just reporting "it's broken", I read the server code and found the encrypted endpoint only accepted a few fields, so it rejected one the plain endpoint takes. I went to the server team with the exact request, the line that rejected it, and a suggested fix modelled on another endpoint that already did it right. [How you raised it, e.g. a message or a call.] They agreed and took the fix. I also checked every other call against the server and sent them the remaining mismatches in one list, so they could fix them together. What I learned: feedback lands better with evidence and a proposed fix than with a complaint.

**Confirm before using:** that you raised it yourself, and how. The records show "server team said they will check and they will fix this", not your message.

**Backup (28 Oct 2026):** the server team proposed sending deleted contact IDs inside the contacts batch call. You worked out the client-side impact (a medium change, as long as fetching contacts still returns IDs) and agreed the limits per request. Use it only if you remember what you told them.

### 9. A mistake you made (Own It)

**What the records show (July 2025):** you changed how emoji sizes render in text stories (1 and 14 Jul). That code was in the 1.29.0 build tagged on 31 Jul. The same day you committed "fixed the 1.29.0.1 crashes" (so the crash may have been found in a test or staged build before most users got it), replacing the emoji library call with the app's own emoji parser, and the fix went out in 1.29.1 on 5 Aug.

> In mid-2025 I changed how emojis were sized in text stories. I tested it, but I used an emoji library call that wasn't safe in every state the app could be in, and it crashed in the 1.29.0 build. [How you found out: crash reports, Play Console, QA.] I owned it: I found the cause the same day, replaced the call with our own emoji parser that the rest of the app already used, and it went out in the next release, 1.29.1, five days later. What I changed after: before using a library call in a shared screen, I check whether the app has its own tested helper for it, and I test on a fresh install where libraries may not be ready yet.

**Confirm before using:** how the crash was spotted, how many users it hit, and whether 1.29.0 reached everyone or only a staged rollout. The records don't say. Also check the "fresh install" lesson matches the real cause; the likely cause (the library not initialised) is a guess.

**Backup (30 Oct 2026):** you changed error handling so locally generated errors no longer showed an untranslated "User not logged in" message during SIM re-authentication. Testing then showed a new error when opening a group link, because that screen depended on the old fallback. You fixed it the same day, before any release. A good "caught my own regression in testing" story.

### 10. Something you shipped fast (Always Be Shipping)

**What the records show (25 Nov 2026):** the server team reported a large number of failing OTP calls: the app was calling them from secondary devices and before encryption key registration had finished. You pasted the report at about 12:19, committed a fix by 12:43, added a retry after review, and it was merged into the fast-track release branch at 16:22.

> Our server team reported a large number of failing calls to one login endpoint. I traced it to the app calling it too early: from secondary devices, and before key registration had finished. Rather than rework the login flow, I put a single check in the one place that makes the call: only call it on the primary device once registration is done. That was ready within about half an hour. In review we added a retry once registration completes, so nothing was lost, and it was merged into the fast-track release the same afternoon. [Outcome: did the failures drop?] The lesson I take from it: find the smallest change that removes the cause, ship it, then improve it.

**Confirm before using:** the release date of that fast-track build and whether the failures dropped.

**Backup (19–25 Nov 2026):** video transcoding failures. You found from telemetry that long file names in Telugu, Hindi and Tamil went over the 255-byte limit, split that fix into its own small change and got it merged the same day, then shipped the broader transcoding fixes a few days later.

## Story 16: the queue-based download engine (filetransfer module)

RevenueCat's hiring page gives "a shipped project you're proud of" as an example question. This is your proudest project, but **it isn't shipped yet**, so say that plainly, and keep a shipped one as backup: SIM-bound login or the personal-to-business account migration (both on your resume). It is also your best technical deep-dive if he asks how something works.

**What the records show:** you started a new download manager in October 2025 ("download manager with priority handling and network execution", 5 Oct) and kept building it through 2026: queue-based transfers (Oct 2025), streaming downloads with byte ranges (Jan 2026), download and upload queue refactor, resume state and ETag support (Jul 2026). About 114 of your commits touch it, from Sep 2025 to Oct 2026. On 19 Nov 2026 it was brought into the main code as its own `:filetransfer` module on the fast-track release branch. No release tag contains it yet.

**How it works (be able to explain each in one sentence):**

- **Three queues by file size:** under 5 MB, 5–50 MB, and 50 MB or more, so one large video never holds up small images. A file of unknown size starts in the small queue and moves once the server says how big it is.
- **Four priority levels:** streaming (a video player waiting for a range) always runs and takes no queue slot; then a download the user tapped; then a file on screen; then automatic downloads off screen.
- **Fair ordering:** within a level, the newer request goes first, but a job never jumps ahead of an equal one, because that would pause a running download for no gain.
- **A limited number run at once per queue,** and all starting and pausing happens on one scheduler thread, so there are no race conditions between them.
- **Resume:** it records which byte ranges have arrived, merges them, and asks the server only for the missing part, using HTTP range requests checked against the file's ETag.
- **Safety:** if the server sends the whole file instead of the range (the file changed, or the server ignores ranges), the stored bytes are thrown away, because they may belong to a different file. Data is written to a separate partial file; the real path only ever holds a finished file.
- **Retries:** up to 3 attempts per run, with a growing wait between them.
- **Streaming:** a video can play while it downloads, by fetching the range the player needs first.
- **Tested:** unit tests for the queue, the range maths, the executors and the partial-file handling.

**Spoken answer (about 2 minutes):**

> The project I'm proudest of is the download engine I built for Arattai's media. [The problem: e.g. large videos blocked small images, downloads restarted from zero after a network drop, and you couldn't play a video until it had fully downloaded.]
>
> I designed it as a set of queues. Files are split by size into three queues, so a big video never blocks a photo. Inside each queue there are priorities: a video the user is watching streams first, then what the user tapped, then what's on screen, then background downloads. Only a few run at once, and all the scheduling happens on one thread, which removed a whole class of race conditions.
>
> For reliability, it remembers exactly which byte ranges have arrived and resumes from the first missing byte, using range requests checked against the file's ETag. If the server sends the whole file instead, it throws away what it had, because those bytes may belong to a different version of the file. And it writes to a partial file, so anything at the final path is always complete.
>
> I built it over about a year alongside other work, with unit tests for the tricky parts like range merging. It's now being integrated into our release as its own module. [Result: what improved, or "it goes out in the next release".]

**Why it helps for RevenueCat:** it shows you understand HTTP, retries, caching and partial failures from the inside, which is what you'd debug in developers' apps every day.

**Confirm before using:**
1. The problem it solved, in your words: what was wrong with the old downloads.
2. Whether it's live for users yet. The records show it on the fast-track release branch from 19 Nov, not in a tagged release. If it's not out yet, say "going out in the next release".
3. Any number you can give (faster downloads, fewer failures, smaller memory use). Don't invent one.
4. A teammate brought it into the main code as a module on 19 Nov. If he asks, describe it as your design and build, integrated with a teammate.

## Extra: what big tech asks for this kind of role (online, 26 Nov)

Support and developer-support interviews at Meta, Amazon, Google and Stripe, from candidate reports on Glassdoor and prep sites. Not RevenueCat's process, but the same kinds of questions can come up, so have short answers ready. Lower priority than answers 1–16.

**What these companies test**

| Company | Role | What candidates report |
|---|---|---|
| Meta | Developer Support Engineer | Phone screen of about 10 quick technical questions: HTTP, HTML, CSS, 400 errors; then live coding |
| Amazon | Support Engineer | Hiring manager round of STAR questions on their values; a customer scenario to root-cause; live coding |
| Google | Technical Solutions Engineer | Customer-situation questions; a role play walking a customer through a network problem |
| Stripe | Support Engineer | Troubleshooting simulation with a mock user, a written communication exercise, API debugging, a values round |

**Questions that come up again and again, with your answers**

**Explain HTTP status codes you see when debugging.**
> 2xx means it worked; 206 is a partial response, which I used for resumable downloads. 4xx is the client's fault: 400 bad request, 401 not authenticated, 403 authenticated but not allowed, 404 not found, 429 too many requests, meaning rate-limited. Our own server signals throttling with a 400 and an error code instead, which I saw when a device re-uploaded contacts in a loop, so I always read the error body, not just the code. 5xx is the server's fault: 500 an error, 502 or 503 a server or gateway that's down or overloaded. For a support ticket, the code tells me which side to look at first.

**Walk me through debugging a request that fails only for some users.**
> First find what the failing users share: app version, OS version, country, network, account type. Then compare one failing request with one working one: URL, headers, body, response. Most of the time the difference shows the cause. In my OTP fix, the shared factor was secondary devices and timing, not the network.

**Tell me about a difficult or angry customer.**
Use the contacts story (answer 6), told from the user's side:
> Users were frustrated because their saved contacts looked like strangers. The first thing they needed wasn't the root cause, it was a way back: log out and in fixed it. So that went out first, and the real fix followed. With an upset customer I acknowledge the problem, give them something that works today, and tell them when they'll hear from me next.

**A time you had to convince someone.**
> [Use the feedback story, answer 8: you convinced the server team with the exact request, the line that rejected it, and a proposed fix. Evidence beats opinion.]

**A time you disagreed with a teammate or your manager.**
> [Needs a real example. A design choice in review, or how to scope a fix. Say what you disagreed on, how you discussed it, and how it ended, including if you were wrong.]

**A time you couldn't deliver what was asked.**
> [Needs a real example. Something you descoped or a deadline you moved. Say how early you warned people and what you delivered instead.]

**Explain something technical to a non-technical person.**
Use the poultry farm client (story 4 in the prep sheet), or explain resumable downloads:
> It's like a book delivered in chapters. If the delivery stops at chapter 7, we only send chapters 8 onwards, instead of the whole book again.

**What happens when you open a URL in an app?**
> The app looks up the server's address through DNS, opens a connection, sets up TLS for HTTPS, sends the request with headers, and gets back a status code, headers and body. Things can fail at any of those steps, and the error usually says which: a DNS error, a timeout, a certificate error, or a status code.

**Write a reply to this ticket** (Stripe's written exercise; a likely shape for RevenueCat's take-home too)

Practise once on paper. Example ticket: *"Your SDK is broken. My offerings are empty on Android and I'm losing sales!!"*

> Hi [name], sorry about this, I know empty offerings mean you can't sell right now, so let's get it fixed.
>
> This is almost always a setup mismatch, not the SDK itself. Could you check these three things first?
> 1. In Play Console, are your subscription products **Active**?
> 2. Do the product IDs in RevenueCat match Play Console exactly, including base plans?
> 3. Is the app's package name the same in both, with no `.debug` suffix?
>
> If those look right, please send me your debug logs from app launch (set `Purchases.logLevel = LogLevel.DEBUG`), your SDK version, and your app user ID, and I'll look straight away.

What makes it good: it acknowledges the impact, doesn't blame them, gives the three most likely causes first, and asks for exactly what's needed next.

**Sources:** [Meta DSE interviews](https://www.glassdoor.com/Interview/Meta-Developer-Support-Engineer-Interview-Questions-EI_IE40772.0,4_KO5,31.htm) · [Amazon Support Engineer interviews](https://www.glassdoor.com/Interview/Amazon-Support-Engineer-Interview-Questions-EI_IE6036.0,6_KO7,23.htm) · [Amazon Support Engineer 2](https://www.glassdoor.co.in/Interview/Amazon-Support-Engineer-2-Interview-Questions-EI_IE6036.0,6_KO7,25.htm) · [Meta Partner Engineer](https://www.glassdoor.com/Interview/Meta-Partner-Engineer-Interview-Questions-EI_IE40772.0,4_KO5,21.htm) · [Stripe interview process (interviewing.io)](https://interviewing.io/stripe-interview-questions). All self-reported or third-party; formats change.

## Your questions for him

1. You moved from mobile engineering into support. What made you switch, and what was harder than you expected?
2. What does a normal ticket look like, and how much of the day is debugging versus writing?
3. What hours would the APAC role cover, and how does it fit with the rest of the team?
4. What are the next steps after this call?

---

## Plan to the call (full effort)

The trip stays a holiday. The work happens before it, on the flights, and in the eight days after. Weekdays are after Zoho hours.

| Date | Do | Time |
|---|---|---|
| **26 Nov** | Break Health Decoder on purpose (wrong entitlement name, no API key, unpublished paywall) and read the logs each time. Download the trip zip to your phone | 1 hour |
| **26 Nov flight** | Knowledge file sections 1–4 | 40 min |
| **27–30 Nov** | Holiday | — |
| **1 Dec flight** | Knowledge file sections 5–13, then the answers table | 2 hours |
| **2 Dec (Wed)** | Fill every [bracket] in 8, 9, 10 and 16; user numbers for 6; write the two missing stories (a disagreement, something you couldn't deliver) | 2 hours |
| **3 Dec (Thu)** | Say answers 1–7 aloud, timed; rewrite any sentence that doesn't sound like you. Post one answer on the RevenueCat community forum | 2 hours |
| **4 Dec (Fri)** | Say answers 8–16 aloud; explain the download engine on paper with a diagram, no notes | 2 hours |
| **5 Dec (Sat)** | Record all 16 answers on your phone, listen back, fix the weak ones. Optional: create a Google Play Console account ($25) and put Health Decoder on a closed test track with real products | 4 hours |
| **6 Dec (Sun)** | Mock interview 1 with me, full 45 minutes, then go through the feedback. Write the reply to the sample ticket on paper | 3 hours |
| **7 Dec (Mon)** | Knowledge file sections 7, 8 and 13 again; explain the empty-offerings checklist from memory | 1.5 hours |
| **8 Dec (Tue)** | Mock interview 2, harder follow-up questions | 1.5 hours |
| **9 Dec (Wed)** | Resume line by line; questions for him; install or update Zoom and do a test call | 1 hour |
| **Thu 10 Dec (call day)** | Finish work early if you can. Light review of the one-line answers at 6 PM; at your desk by 7:10 PM; join at 7:25 PM | 20 min |

**The three things that matter most, in order:**
1. **Speaking clearly and slowly.** Every answer said aloud at least three times before the call.
2. **Honest, specific stories.** Every [bracket] filled with something true.
3. **Knowing your own work cold.** `ProAccess.kt` and the download engine, explained without notes.

**Two optional moves that strengthen the "shipped RevenueCat" requirement**, if you have the time and the $25:
- A Health Decoder closed test track with real Google Play products, so "integrated and shipped to a test track" is true.
- One answer posted on the RevenueCat community forum, so you can say you've already helped a developer.

DSA: paused until after the call.

## Call-day checklist

- [ ] Invite accepted (click Yes in the calendar); time is **7:30 PM IST, Thu 10 Dec**
- [ ] Zoom installed and updated; test call done; your display name shows "Shajin Hari"
- [ ] Laptop charged, camera and microphone tested, headphones on
- [ ] Phone hotspot ready in case the internet drops
- [ ] Quiet room, plain background, good light on your face
- [ ] On paper: the answers table and your four questions
- [ ] Every AI tool and note-taker closed
- [ ] Join 5 minutes early
- [ ] Speak slowly; if you don't understand a question, ask him to repeat it
