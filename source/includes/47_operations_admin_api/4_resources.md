## <a name="operations-admin-resources"></a> Resources

### <a name="operations-admin-resource-model"></a> Resource model

> Resource example:

```json
{
    "id": 15,
    "resource_type": "Some resource type",
    "store_id": 3,
    "member_id": 33,
    "user_id": "some-user-uuid",
    "name": "a very important document",
    "marketing_usable": true,
    "file_url": "https://some.file/url",
    "state": "accepted",
    "properties": {
      "campaign_name": ["November", "October"],
      "used_so_far": false,
      "author_count": 3
    },
    "campaign_type": "some campaign type",
    "notes": "some notes about the resource",
    "created_at": "2020-12-30T17:12:46.435Z",
    "updated_at": "2020-12-30T17:12:50.235Z",
}
```

A resource is an entity that has a file attached to it. It comprises of a name, details (listed above) and a custom attribute list that matches its resource type attributes.
As an admin, one can list, accept and reject resources submitted by users.

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
resource_type | string | no | name of the ResourceType the resource is assigned to
store_id | integer | no | .id of the Store the resource is assigned to
member_id | integer | no | .member_id of the User the resource has been created by
user_id | string | no | uuid of the User the resource has been created by
name | string | no | name of the resource
marketing_usable | boolean | no | whether the resource will be allowed to be used for marketing purposes
file_url | string | no | URL of the file where the resource is located at
state | string | no | current state of the resource (could be `pending` (the default one), `accepted` or `rejected`)
properties | object | yes | custom attributes of the resource
campaign_type | string | yes | campaign type of the resource
notes | string | yes | notes about the resource - used to provide a reason for its acceptance/rejection/resubmission
created_at | datetime | no | time of creation
updated_at | datetime | no | time of last update

### <a name="operations-admin-list-resources"></a> List Resources

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/resources" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns object containing [resources](#operations-admin-resource-model) and [pagination_info](#pagination-model)

```json
{
  "resources": [], // List of resources - see 'Resource model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/resources`

Returns list of [Resources](#operations-admin-resource-model).

#### Query Parameters

Parameter                      | Type                            | Default   | Description
--------------                 | -----------                     | --------- | -----------
per_page                       | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no                        | integer                         | 1         | Number of results page
search_query                   | string                          | null      | If provided, only Resources that have any of their attribute values matching the provided string are returned
resource_type                  | string                          | null      | If provided, only Resources that belong to the resource type of provided name are returned
campaign_type                  | array[string]                   | null      | If provided, only Resources with the provided campaign types are returned
store_id                       | integer                         | null      | If provided, only Resources that belong to the store of provided id are returned
member_id                      | integer                         | null      | If provided, only Resources that belong to the member of provided id are returned

<aside class="notice">
Requires <code>Operations:Api:Resources:List</code> permit
</aside>

### <a name="operations-admin-show-resource"></a> Get Resource

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/resources/5" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns object containing [resource](#operations-admin-resource-model)

```json
{
  "resource": {} // see: 'Resource model'
}
````

**GET** `v1/operations/resources/:id`

Returns given [Resource](#operations-admin-resource-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Resource ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Resource not found

<aside class="notice">
Requires <code>Operations:Api:Resources:Show</code> permit
</aside>

### <a name="operations-show-resource"></a> Accept Resource

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/resources/5/accept" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns object containing [resource](#operations-resource-model)

```json
{
  "resource": {} // see: 'Resource model'
}
````

**PUT** `v1/operations/resources/:id/accept`

Accepts given [Resource](#operations-resource-model).

#### URL Parameters

Parameter  | Type     | Description
---------- | -------- | ------
id         | integer  | Resource ID
notes      | string   | Notes (thoughts) about the resource - will be visible to the author

#### Error responses

Status    | Description
--------- | -----------
`404`     | Resource not found
`405`     | Resource is not accepttable

<aside class="notice">
Requires <code>Operations:Api:Resources:Accept</code> accept
</aside>

### <a name="operations-show-resource"></a> Reject Resource

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/resources/5/reject" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns object containing [resource](#operations-resource-model)

```json
{
  "resource": {} // see: 'Resource model'
}
````

**PUT** `v1/operations/resources/:id/reject`

Rejects given [Resource](#operations-resource-model).

#### URL Parameters

Parameter  | Type     | Description
---------- | -------- | ------
id         | integer  | Resource ID
notes      | string   | Notes (thoughts) about the resource - will be visible to the author

#### Error responses

Status    | Description
--------- | -----------
`404`     | Resource not found
`405`     | Resource is not rejecttable

<aside class="notice">
Requires <code>Operations:Api:Resources:Reject</code> permit
</aside>

### <a name="operations-show-resource"></a> Create a Resource reminder

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/operations/resources/5/reminders" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns object containing [resource](#operations-resource-model)

```json
{
  "resource": {} // see: 'Resource model'
}
````

**POST** `v1/operations/resources/:id/reminders`

Creates a reminder for the given [Resource](#operations-resource-model).

#### URL Parameters

Parameter         | Type     | Description
----------        | -------- | ------
id                | integer  | Resource ID
reminder          | object   | Object containing a .message of the reminder
reminder[message] | string   | Message of the reminder

#### Error responses

Status    | Description
--------- | -----------
`404`     | Resource not found

<aside class="notice">
Requires <code>Operations:Api:Resources:Reminders:Create</code> permit
</aside>
