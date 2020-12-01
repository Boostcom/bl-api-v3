## <a name="operations-admin-documents"></a> Documents

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
  "documents": [], // List of documents - See: "Document model"
  "pagination_info": {} // Pagination info - see "Pagination info"
}
````

**GET** `v1/operations/documents`

Returns list of [Documents](#operations-admin-document-model).  

#### Query Parameters

Parameter                      | Type                            | Default   | Description
--------------                 | -----------                     | --------- | -----------
per_page                       | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no                        | integer                         | 1         | Number of results page 
confirmable                    | boolean                         | null      | When present, returns only confirmable Documents - and vice versa
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
  "document": {} // See: "Document model"
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
  "document_recipients": [], // List of document recipients - See: "DocumentRecipient model"
  "pagination_info": {} // Pagination info - see "Pagination info"
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
  "document": {} // Document - See: "Document model"
}
```

**POST** `v1/operations/documents`

Creates a new [Document](#operations-admin-document-model) and sends it to given recipients.

#### POST Parameters (JSON)

Key                          | Type                             | Description
---------                    | ---------                        | ---------
**document.title**           | string                           |   
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

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

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
  "document": {} // Document - See: "Document model"
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
  "document_recipients": [] // List of document recipients - See: "DocumentRecipient model"
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
  "document": {} // Document - See: "Document model"
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
