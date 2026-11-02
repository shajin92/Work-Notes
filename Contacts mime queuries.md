
1. To list the raw datas 
   adb -s RZGL12ERHQE shell "content query --uri content://com.android.contacts/data \  
  --projection _id:raw_contact_id:contact_id:mimetype:account_type:account_name:display_name:data1:data2:data3 \  
  --where \"mimetype LIKE '%chat.call.audio'\""
  
2. To invoke them use the adb qu 
   adb -s RZGL12ERHQE shell am start -W \                                                                                             -a android.intent.action.VIEW \                                                                                                           -d content://com.android.contacts/data/44606 \                                                                              -t vnd.android.cursor.item/vnd.com.aratai.chat.call.audio \                                                             -n com.aratai.chat/com.zoho.chat.ContactAction

3. wo