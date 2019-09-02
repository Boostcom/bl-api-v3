# Endpoints &bull; Departments

## <a name="v3-department-list"></a> List

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/departments" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
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

## <a name="v3-department-get"></a> Get

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/departments/123456" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
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


### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | ID internal | integer
department_id | Department id from customer system | integer

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Departments:Get</code> permit
</aside> 