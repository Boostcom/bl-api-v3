##  Departments

### <a name="department-list"></a> List

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/stores/departments" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an array of department objects structured like below. Some values may be omitted if were not provided:

```json
[
  {
      "id": 12345,
      "department_id": 67890,
      "name": "Department/Mall name"
  }
]
``` 

**GET** `v3/:loyalty_club_slug/stores/departments`

List customer departments

<aside class="notice">
Requires <code>Stores:Api:Departments:Get</code> permit
</aside> 

### <a name="department-get"></a> Get

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/stores/departments/123456" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns department object structured like below. Some values may be omitted if were not provided:

```json
{
  "id": 12345,
  "department_id": 67890,
  "name": "Department/Mall name"
}
``` 

> When `id` is invalid (422), returns:

```json
{
    "error": "Department/Mall not found"
}
``` 

**GET** `v3/:loyalty_club_slug/stores/departments/:id`

**GET** `v3/:loyalty_club_slug/stores/departments/by_department_id/:department_id`

Return department by one of two identifiers: `id` or `department_id`


#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | ID internal | integer
department_id | Department id from customer system | integer

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Departments:Get</code> permit
</aside>

### <a name="department-create"></a> Create

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/stores/departments" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
        "name": "department name",
        "department_id": 12345
    }'
```

> When successful (200), returns a feedback object structured like this:

```json
{
    "id": 12345,
    "department_id": 6789,
    "status": "ok"
}
```  

> When request is invalid (422), returns:

```json
{
    "error": "Invalid format",
    "details": {
        "name": "The property {property} is required"
    }
}
``` 

Create store

**POST** `v3/:loyalty_club_slug/stores/departments`

Create store

#### POST Parameters (JSON)

Parameter      | Description            | Type
-------------  | ---------------------- | ------
name*          | department name             | string
department_id* | department/mall id          | integer

Parameters with `*` are required

#### Feedback statuses

Status | Description
---- | ----
ok   | Department was created
Department with this id already exists. | 

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Departments:Create</code>
</aside>

### <a name="department-update"></a> Update

> Example:

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/stores/departments/12345" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
        "name": "department name"
    }'
```

> When successful (200), returns a feedback object structured like this:

```json
{
    "id": 12345,
    "department_id": 6789,
    "status": "ok"
}
```  

> When request is invalid (422), returns:

```json
{
    "error": "Invalid format",
    "details": {
        "name": "The property {property} is required"
    }
}
``` 

> When `id` or `department_id` is invalid (422), returns:

```json
{
    "error": "Department not found"
}
``` 

Update department

**PUT** `v3/:loyalty_club_slug/stores/departments/:id`

**PUT** `v3/:loyalty_club_slug/stores/departments/by_department_id/:department_id`

#### URL Parameters

Parameter  | Description | Type
---------- | ----------- | ------
id         | ID          | integer
department_id   | department id    | integer

#### PUT Parameters (JSON)

Parameter     | Description            | Type
------------- | ---------------------- | ------
name*         | department name             | string

Parameters with `*` are required

#### Feedback statuses

Status | Description
------ | ----
ok     | Department updated
Department with this id does not exist. | 

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Departments:Update</code>
</aside> 