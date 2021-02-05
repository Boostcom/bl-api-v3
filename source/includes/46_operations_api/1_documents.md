## <a name="operations-documents"></a> Documents

### <a name="operations-document-model"></a> Document model

> Document example:

```json
{
    "id": 15,
    "confirmable": true,
    "title": "Fire instructions",
    "deadline_at": "2020-12-15T15:43:32.000Z",
    "created_at": "2020-11-30T17:12:46.435Z",
    "updated_at": "2020-11-30T17:12:50.235Z",
    "confirmed_at": "2020-11-30T17:12:50.235Z"
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
confirmable | boolean | no | Is document meant to be confirmed by recipient?
title | string | no |
deadline_at | datetime | when not confirmable | Time until document is meant to be confirmed by recipients - only for `confirmable` documents
created_at | datetime | no | Time of creation
updated_at | datetime | no | Time of last update
confirmed_at | datetime | yes| Time of confirmation by current user

For more details, see [Document model](#operations-admin-document-model) in Operations Admin API

### <a name="operations-list-documents"></a> List Documents

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/documents" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [documents](#operations-document-model) and [pagination_info](#pagination-model)

```json
{
  "documents": [], // List of documents - see 'Document model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/users/me/operations/documents`

Returns list of [Documents](#operations-document-model) shared with current user.

#### Query Parameters

Parameter              | Type        | Default   | Description
--------------         | ----------- | --------- | -----------
per_page               | integer     | 100       | Number of results to be returned per request (100 is the maximum)
page_no                | integer     | 1         | Number of results page
confirmable            | boolean     | null      | When true, returns only confirmable Documents - and vice versa
title                  | string      | null      | When true, returns only Documents that have title matching to given string
requiring_confirmation | boolean     | null      | When present, returns only Documents that require confirmation (are confirmable and are not confirmed)

<aside class="notice">
Requires <code>Operations:Api:Users:Documents:List</code> permit
</aside>

### <a name="operations-show-document"></a> Get Document

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/documents/5" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [document](#operations-document-model)

```json
{
  "document": {} // see 'Document model'
}
````

**GET** `v1/users/me/operations/documents/:id`

Returns given [Document](#operations-document-model) shared with current user.

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Document ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Document not found

<aside class="notice">
Requires <code>Operations:Api:Users:Documents:Show</code> permit
</aside>

### <a name="operations-show-document"></a> Confirm Document

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/users/me/operations/documents/5/confirm" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [document](#operations-document-model)

```json
{
  "document": {} // see: 'Document model'
}
````

**PUT** `v1/users/me/operations/documents/:id/confirm`

Confirms given [Document](#operations-document-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Document ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Document not found
`405`     | Document is not confirmable

<aside class="notice">
Requires <code>Operations:Api:Users:Documents:Confirm</code> permit
</aside>
