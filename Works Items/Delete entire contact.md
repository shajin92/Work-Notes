 
 - [x] create a client conf ✅ 2026-05-12
 - [x] handle the migration for the phonebook contact in the table. or try to keep the pref and handle them ✅ 2026-05-12
 - [x] nEED TO DO THE translation ✅ 2026-09-07
 - [x] handle ontime migration for the delete to fetch the contacts ✅ 2026-09-07
 - [x] need to check the seconday device flow for the delete contact . ✅ 2026-09-09
 - [ ] verify every case for the delete 
 - [ ] need to work on the phone book deleted contact in the arattai 
 - [ ] when a contact is cleared then deleteing a contact then the deleted contact is not updated
 - [ ] remove the existing contact delete feature 
 - [ ] handle the delete contact id 
 - [ ] validate the sync contact in the secondary device 






## Validate the secodary device 

- [x] handle the banner in m3 contacts ✅ 2026-09-15
- [x] handle the sync contacts in the settings page ✅ 2026-09-15
- [ ] handle the upload contact flow in the batch
- [ ] validate the import screen 
- [ ]  validate share contact 
- [ ] remove refresh contacts 




1. HomeActivity.kt:1152 — requestContactsPermission(), from the onboarding contact-sync consent sheet's onContinue (:1133)
2. ChatsFragment.kt:865 — requestContactPermission(permissionLauncher), from the chats-tab consent sheet onContinue (:812)
3. ContactSyncActivity.kt:92 — requestContactsPermission(), from onContinueClick() (:83)
4. SecurityActivity.java:485-515 — Settings → Privacy → "Sync contacts" switch enabled
5. ContactsListFragment.kt:95 — from overflow "Refresh contacts" (:212)
6. NewChatFragment.kt:160 — from new-chat overflow retry (:251)
7. ChatTranscriptFragment.kt:7943 — activity?.requestContactPermission() in handleContacts(), entered from the READ_CONTACTS branch (:7926) fed by ChatAttachmentSheet.kt:483
8. ImportChat.kt:198 — on ImportUIType.CONTACT_PERMISSION_REQUIRED









handle the add to contact cases to the also in them 


example code base 


  ```val ops = arrayListOf<ContentProviderOperation>(
    ContentProviderOperation.newInsert(ContactsContract.RawContacts.CONTENT_URI)
        .withValue(ContactsContract.RawContacts.ACCOUNT_TYPE, "com.google")
        .withValue(ContactsContract.RawContacts.ACCOUNT_NAME, accountName)
        .build(),
    ContentProviderOperation.newInsert(ContactsContract.Data.CONTENT_URI)
        .withValueBackReference(ContactsContract.Data.RAW_CONTACT_ID, 0)
        .withValue(ContactsContract.Data.MIMETYPE, StructuredName.CONTENT_ITEM_TYPE)
        .withValue(StructuredName.DISPLAY_NAME, displayName)
        .build(),
    ContentProviderOperation.newInsert(ContactsContract.Data.CONTENT_URI)
        .withValueBackReference(ContactsContract.Data.RAW_CONTACT_ID, 0)
        .withValue(ContactsContract.Data.MIMETYPE, Phone.CONTENT_ITEM_TYPE)
        .withValue(Phone.NUMBER, phoneNumber)
        .withValue(Phone.TYPE, Phone.TYPE_MOBILE)
        .build(),
)
val results = contentResolver.applyBatch(ContactsContract.AUTHORITY, ops)

Optionally nudge the sync instead of waiting for it:

ContentResolver.requestSync(
    Account(accountName, "com.google"),
    ContactsContract.AUTHORITY,
    Bundle().apply { putBoolean(ContentResolver.SYNC_EXTRAS_MANUAL, true) },
)
  ```