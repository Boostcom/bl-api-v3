# <a name="operations-admin"></a> Operations Admin API

## <a name="operations-admin-models"></a> Common models

### <a name="operations-admin-document-model"></a> Document

> Document example:

```json
{
    "id": 15,
    "confirmable": true,
    "title": "Fire instructions",
    "deadline_at": "2020-12-15T15:43:32.000Z",
    "message_channel": "sms",
    "first_reminder": {
        "days_before_deadline": 5,
        "scheduled_at": "2020-12-10T08:00:00.000Z",
        "sent_at": null
    },
    "second_reminder": {
        "days_before_deadline": 2,
        "scheduled_at": "2020-12-13T08:00:00.000Z",
        "sent_at": null
    },
    "created_at": "2020-11-30T17:12:46.435Z",
    "updated_at": "2020-11-30T17:12:46.435Z",
    "recipients_count": 2,
    "recipients_confirmation_status": "some"
}
```

The document is an entity that has some files (with `type: "document"` and Document's ID as `identifier`) attached to it. 

Upon creation, a notification containing the document is sent via specified channel to given recipients (members of Tenant community). 

When document is `confirmable`, recipients may need to confirm it. In such case, a `deadline_at` must be specified.
Also, it is possible to configure automated [reminder](#operations-admin-document-reminder-model) messages that
are sent to recipients which didn't confirm the document.

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |   
confirmable | boolean | no | Is document meant to be confirmed by recipients?   
title | string | no |
deadline_at | datetime | when not confirmable | Time until document is meant to be confirmed by recipients - only for `confirmable` documents 
message_channel | enum: `['sms', 'email', 'push']` | no | Channel that the document (and reminders) should be sent with
first_reminder | [DocumentReminder](#operations-admin-document-reminder-model) | yes | 
second_reminder | [DocumentReminder](#operations-admin-document-reminder-model)| yes | Can only be specified after the first reminder 
created_at | datetime | no | Time of creation
updated_at | datetime | no | Time of last update
recipients_count | integer | no | Number of recipients
recipients_confirmation_status | enum: `['all', 'some', 'none']` | no | Describes how many recipients have confirmed the document 

#### <a name="operations-admin-document-reminder-model"></a> DocumentReminder model

> Document reminder example:

```json
{
    "days_before_deadline": 2,
    "scheduled_at": "2020-12-13T08:00:00.000Z",
    "sent_at": "2020-12-13T08:00:01.540Z"
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
days_before_deadline | integer | no | When the reminder should be sent - number of days before the specified deadline   
scheduled_at | datetime | no | Actual calculated time for reminder sending - at 09:00 in the timezone specific for the Loyalty Club 
sent_at | datetime | yes | When the reminder has been sent

### <a name="operations-admin-document-recipient-model"></a> DocumentRecipient model

> DocumentRecipient example:

```json
{
    "id": 44,
    "member_id": 52,
    "last_seen_at": "2020-11-30T17:21:42.350Z",
    "confirmed_at": "2020-11-30T17:21:50.420Z",
    "created_at": "2020-11-30T16:46:28.200Z",
    "updated_at": "2020-11-30T16:46:28.200Z"
}
```

Key          | Type     | Optional | Description
---------    | -------- | -------- | ---------
id           | integer  | no       |   
member_id    | integer  | no       | ID of MPC member
last_seen_at | datetime | yes      | Last time when the recipient retrieved the Document record
confirmed_at | datetime | yes      | Time of confirmation
created_at   | datetime | no       | Time of creation
updated_at   | datetime | no       | Time of last update
