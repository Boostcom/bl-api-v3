## <a name="operations-admin-documents"></a> Documents

### <a name="operations-admin-document-model"></a> Document model

> Document example:

```json
{
    "id": 15,
    "confirmable": true,
    "title": "Fire instructions",
    "description": "Describes how to behave in case of fire",
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
    "created_by": { "entity_type": "token", "entity_id": 491 },
    "created_at": "2020-11-30T17:12:46.435Z",
    "updated_at": "2020-11-30T17:12:46.435Z",
    "recipients_count": 2,
    "recipients_confirmation_status": "some"
}
```

The document is an entity that has some files (with `type: "document"` and Document's ID as `identifier`) attached to it.

Upon creation, a notification containing a link to the document is sent via specified channel to given recipients (members of Tenant community).

When document is `confirmable`, recipients may need to confirm it. In such case, a `deadline_at` must be specified.
Also, it is possible to configure automated [reminder](#operations-admin-document-reminder-model) messages that
are sent to recipients which didn't confirm the document.

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
confirmable | boolean | no | Is document meant to be confirmed by recipients?
title | string | no |
description | string | no |
deadline_at | datetime | when not confirmable | Time until document is meant to be confirmed by recipients - only for `confirmable` documents
message_channel | enum: `['sms', 'email', 'push']` | no | Channel that the document (and reminders) should be sent with
first_reminder | [DocumentReminder](#operations-admin-document-reminder-model) | yes |
second_reminder | [DocumentReminder](#operations-admin-document-reminder-model)| yes | Can only be specified after the first reminder
created_by | [API entity](#api-entity-model) | no | Author of document
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

### <a name="operations-admin-list-documents"></a> List Documents

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/documents" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [documents](#operations-admin-document-model) and [pagination_info](#pagination-model)

```json
{
  "documents": [], // List of documents - see 'Document model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/documents`

Returns list of [Documents](#operations-admin-document-model).

#### Query Parameters

Parameter                      | Type                            | Default   | Description
--------------                 | -----------                     | --------- | -----------
per_page                       | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no                        | integer                         | 1         | Number of results page
confirmable                    | boolean                         | null      | When true, returns only confirmable Documents - and vice versa
search                         | string                          | null      | When true, returns only Documents that match the given string
recipients_confirmation_status | enum: `['all', 'some', 'none']` | null      | When present, returns only Documents having given `recipients_confirmation_status`

<aside class="notice">
Requires <code>Operations:Api:Documents:List</code> permit
</aside>

### <a name="operations-admin-show-document"></a> Get Document

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/documents/5" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [document](#operations-admin-document-model)

```json
{
  "document": {} // see: 'Document model'
}
````

**GET** `v1/operations/documents/:id`

Returns given [Document](#operations-admin-document-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Document ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Document not found

<aside class="notice">
Requires <code>Operations:Api:Documents:Show</code> permit
</aside>

### <a name="operations-admin-list-document-recipients"></a> List Document Recipients

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/documents/15/recipients" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [document recipients](#operations-admin-document-recipient-model) and [pagination_info](#pagination-model)

```json
{
  "document_recipients": [], // List of document recipients - see 'DocumentRecipient model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/documents/:id/recipients`

Returns list of [DocumentRecipients](#operations-admin-document-recipient-model) that are assigned to given Document

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Document ID

#### Query Parameters

Parameter | Type    | Default   | Description
--------- | ------- | --------- | -----------
per_page  | integer | 100       | Number of results to be returned per request (100 is the maximum)
page_no   | integer | 1         | Number of results page
confirmed | boolean | null      | When true, returns only recipients that confirmed - and vice versa

#### Error responses

Status    | Description
--------- | -----------
`404`     | Document not found

<aside class="notice">
Requires <code>Operations:Api:Recipients:List</code> permit
</aside>

### <a name="operations-admin-create-document"></a> Create Document

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/operations/documents" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "document": {
            "title": "My name",
            "description": "Some description",
            "message_channel": "sms",
            "confirmable": true,
            "deadline_at": "2020-12-15T15:43:32",
            "recipients": [14, 423],
            "first_reminder": { "days_before_deadline": 5 },
            "second_reminder": { "days_before_deadline": 2 }
        }
    }
  '
```

> When successful, returns object containing created [document](#operations-admin-document-model)

```json
{
  "document": {} // Document - see: 'Document model'
}
```

**POST** `v1/operations/documents`

Creates a new [Document](#operations-admin-document-model) and sends it to given recipients.

#### POST Parameters (JSON)

Key                          | Type                             | Description
---------                    | ---------                        | ---------
**document.title**           | string                           |
**document.description**     | string                           |
**document.message_channel** | enum: `['sms', 'email', 'push']` | The channel for sending a notification and reminders
**document.recipients**      | integer[]                        | IDs of members that should receive the document - max. 1000
document.confirmable         | boolean                          | Is document meant to be confirmed by recipients? Default: `false`

When the document is confirmable:

Key                                           | Type      | Description
---------                                     | --------- | ---------
**document.deadline_at**                      | datetime  | Date until the document should be confirmed
document.first_reminder.days_before_deadline  | integer   | Number of days before the deadline the first reminder should be sent
document.second_reminder.days_before_deadline | integer   | Number of days before the deadline the second reminder should be sent

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
document | Document | See: [Document model](#operations-admin-document-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

#### Specific validation errors

Key                                             | Error
---                                             | ----------------
`document.second_reminder`                      | `cannot_be_present_without_first_reminder`
`document.second_reminder.days_before_deadline` | `cannot_be_before_first_reminder`

<aside class="notice">
Requires <code>Operations:Api:Documents:Create</code> permit
</aside>

### <a name="operations-admin-update-document"></a> Update Document

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/documents/12" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "document": {
            "title": "My name",
            "description": "Some description",
            "deadline_at": "2020-12-15T15:43:32",
            "first_reminder": { "days_before_deadline": 5 },
            "second_reminder": { "days_before_deadline": 2 }
        }
    }
  '
```

> When successful, returns object containing updated [document](#operations-admin-document-model)

```json
{
  "document": {} // Document - see 'Document model'
}
```

**PUT** `v1/operations/documents/:id`

Updates the [Document](#operations-admin-document-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Document ID

#### POST Parameters (JSON)

Key                          | Type                             | Description
---------                    | ---------                        | ---------
**document.title**           | string                           |
**document.description**     | string                           |

When the document is confirmable:

Key                                           | Type      | Description
---------                                     | --------- | ---------
**document.deadline_at**                      | datetime  | Date until the document should be confirmed
document.first_reminder.days_before_deadline  | integer   | Number of days before the deadline the first reminder should be sent
document.second_reminder.days_before_deadline | integer   | Number of days before the deadline the second reminder should be sent

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
document | Document | See: [Document model](#operations-admin-document-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Document not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

#### Specific validation errors

Key                                             | Error
---                                             | ----------------
`document.first_reminder`                       | `already_sent`
`document.second_reminder`                      | `already_sent`
`document.second_reminder`                      | `cannot_be_present_without_first_reminder`
`document.second_reminder.days_before_deadline` | `cannot_be_before_first_reminder`

<aside class="notice">
Requires <code>Operations:Api:Documents:Update</code> permit
</aside>

### <a name="operations-admin-add-document-recipients"></a> Add recipients to the Document

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/operations/documents/12" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "recipients": [4, 2, 42]
    }
  '
```

> Returns object containing created [document recipients](#operations-admin-document-recipient-model)

```json
{
  "document_recipients": [] // List of document recipients - see: 'DocumentRecipient model'
}
```

**PUT** `v1/operations/documents/:id`

Adds [recipients](#operations-admin-document-recipient-model) to the [Document](#operations-admin-document-model) and sends them a notification.

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Document ID

#### POST Parameters (JSON)

Key            | Type      | Description
---------      | --------- | ---------
**recipients** | integer[] | IDs of members that should receive the document - max. 1000

#### Error responses

Status | Description
--------- | -----------
`404` | Document not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:DocumentRecipients:Create</code> permit
</aside>

### <a name="operations-admin-destroy-document"></a> Destroy Document

> Example

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v1/operations/documents/12 \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, returns object containing destroyed [document](#operations-admin-document-model)

```json
{
  "document": {} // Document - see: 'Document model'
}
```

**DELETE** `v1/operations/documents/:id`

Destroys the [Document](#operations-admin-document-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Document ID

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
document | Document | See: [Document model](#operations-admin-document-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Document not found

<aside class="notice">
Requires <code>Operations:Api:Documents:Destroy</code> permit
</aside>
