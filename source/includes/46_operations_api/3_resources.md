## <a name="operations-resources"></a> Resources

### <a name="operations-resource-model"></a> Resource model

> Resource example:

```json
{
    "id": 15,
    "resource_type_id": 2,
    "store_id": 3,
    "member_id": 33,
    "user_id": 34,
    "name": "a very important document",
    "marketing_usable": true,
    "state": "accepted",
    "resource_attributes": {
      "campaign_name": ["November", "October"],
      "used_so_far": false,
      "author_count": 3
    },
    "created_at": "2020-12-30T17:12:46.435Z",
    "updated_at": "2020-12-30T17:12:50.235Z",
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
resource_type_id | integer | no | .id of the ResourceType the resource is assigned to
store_id | integer | no | .id of the Store the resource is assigned to
member_id | integer | no | .member_id of the User the resource has been created by
user_id | integer | no | .id of the User the resource has been created by
name | string | no | name of the resource
marketing_usable | boolean | no | whether the resource will be allowed to be used for marketing purposes
state | string | no | current state of the resource (could be `pending` (the default one), `approved` or `rejected`)
resource_attributes | object | yes | custom attributes of the resource
created_at | datetime | no | time of creation
updated_at | datetime | no | time of last update

For more details, see [Resource model](#operations-admin-resource-model) in Operations Admin API

### <a name="operations-list-resources"></a> List Resources

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/resources" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [resources](#operations-resource-model) and [pagination_info](#pagination-model)

```json
{
  "resources": [], // List of resources - see 'Resource model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/users/me/operations/resources`

Returns list of [Resources](#operations-resource-model) shared with current user.

#### Query Parameters

Parameter              | Type                            | Default   | Description
--------------         | -----------                     | --------- | -----------
per_page               | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no                | integer                         | 1         | Number of results page

<aside class="notice">
Requires <code>Operations:Api:Users:Resources:List</code> permit
</aside>

### <a name="operations-show-resource"></a> Get Resource

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/resources/5" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [resource](#operations-resource-model)

```json
{
  "resource": {} // see: 'Resource model'
}
````

**GET** `v1/users/me/operations/resources/:id`

Returns given [Resource](#operations-resource-model) shared with current user.

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Resource ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Resource not found

<aside class="notice">
Requires <code>Operations:Api:Users:Resources:Show</code> permit
</aside>

### <a name="operations-create-resource"></a> Create Resource

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/operations/resources" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
  -d '
    {
        "resource": {
            "resource_type_id": 2,
            "name": "a very important document",
            "marketing_usable": true,
            "resource_attributes": {
              "campaign_name": ["November", "October"],
              "used_so_far": false,
              "author_count": 3
            }
        }
    }
  '
```

> When successful, returns object containing created [resource](#operations-resource-model)

```json
{
  "resource": {} // Resource - see: 'Resource model'
}
```

**POST** `v1/operations/resources`

Creates a new [Resource](#operations-resource-model)

#### POST Parameters (JSON)

Key                              | Type      | Description
---------                        | --------- | ---------
**resource.resource_type_id**    | integer   | id of the resource type the resource will be associated to
**resource.name**                | string    | name of the resource
**resource.marketing_usable**    | boolean   | whether the resource will be allowed to be used for marketing purposes
**resource.resource_attributes** | object    | custom attributes of the resource

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
resource | Resource | See: [Resource model](#operations-resource-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Resources:Create</code> permit
</aside>

### <a name="operations-admin-update-resource"></a> Update Resource

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/resources/12" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
  -d '
    {
        "resource": {
            "resource_type_id": 2,
            "name": "a very important document",
            "marketing_usable": true,
            "resource_attributes": {
              "campaign_name": ["November", "October"],
              "used_so_far": false,
              "author_count": 3
            }
        }
    }
  '
```

> When successful, returns object containing updated [resource](#operations-admin-resource-model)

```json
{
  "resource": {} // Resource - see 'Resource model'
}
```

**PUT** `v1/operations/resources/:id`

Updates the [Resource](#operations-admin-resource-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Resource ID

#### POST Parameters (JSON)

Key                              | Type      | Description
---------                        | --------- | ---------
**resource.resource_type_id**    | integer   | id of the resource type the resource will be associated to
**resource.name**                | string    | name of the resource
**resource.marketing_usable**    | boolean   | whether the resource will be allowed to be used for marketing purposes
**resource.resource_attributes** | object    | custom attributes of the resource

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
resource | Resource | See: [Resource model](#operations-admin-resource-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Resource not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Resources:Update</code> permit
</aside>

### <a name="operations-admin-destroy-resource"></a> Destroy Resource

> Example

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v1/operations/resources/12 \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test'
  -H 'x-b-store-id: 2' \
```

> When successful, returns object containing destroyed [resource](#operations-admin-resource-model)

```json
{
  "resource": {} // Resource - see: 'Resource model'
}
```

**DELETE** `v1/operations/resources/:id`

Destroys the [Resource](#operations-admin-resource-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Resource ID

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
resource | Resource | See: [Resource model](#operations-admin-resource-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Resource not found

<aside class="notice">
Requires <code>Operations:Api:Resources:Destroy</code> permit
</aside>

### <a name="operations-show-resource"></a> Resubmit Resource

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/users/me/operations/resources/5/resubmit" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [resource](#operations-resource-model)

```json
{
  "resource": {} // see: 'Resource model'
}
````

**PUT** `v1/users/me/operations/resources/:id/resubmit`

Resubmits given [Resource](#operations-resource-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Resource ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Resource not found
`405`     | Resource is not resubmittable

<aside class="notice">
Requires <code>Operations:Api:Users:Resources:Resubmit</code> permit
</aside>
