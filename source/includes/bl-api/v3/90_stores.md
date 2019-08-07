# Endpoints &bull; Stores

## <a name="v3-store-list"></a> List

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores?mall_id=123&parent_id=456&limit=100&sort=name&order=asc" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an array of mall objects structured like below. Some values may be omitted if were not provided:

```json
[
    {
        "id": 1234,
        "parent_id": 1234,
        "store_id": "1234",
        "mall_id": 1234,
        "name": "Store name",
        "categories": ["category"],
        "unit": "location"
    }
]
``` 

**GET** `v3/:loyalty_club_slug/stores`

List of all active stores

### URL Parameters

Parameter | Description                  | Type
--------- | ---------------------------- | ------
parent_id | internal mall id             | integer
mall_id   | mall id                      | string
limit     | max number of results        | string
sort      | field to sort by (name,id,store_id) | string
order     | order (asc,desc)             | string

All parameters are optional

<aside class="notice">
Requires <code>Stores:Api:Stores:Get</code> permit
</aside> 

## <a name="v3-store-get"></a> Get

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/123456" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns mall object structured like below. Some values may be omitted if were not provided:

```json
{
    "id": 1234,
    "parent_id": 1234,
    "store_id": "1234",
    "mall_id": 1234,
    "name": "Store name",
    "categories": ["category"],
    "unit": "location"
}
``` 

> When `id` is invalid (422), returns:

```json
{
    "error": "Store not found"
}
``` 

**GET** `v3/:loyalty_club_slug/stores/:id`

**GET** `v3/:loyalty_club_slug/stores/by_store_id/:store_id`

Return store by one of two identifiers: `id` or `store_id`


### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | ID internal | integer
store_id | Store id from customer system | string

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Stores:Get</code> permit
</aside>

## <a name="v3-store-create"></a> Create

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
    -d \
    '{
        "name": "store name",
        "store_id": "6789",
        "mall_id": 12345,
        "parent_id": 12345,
        "categories": ["category"],
        "unit": "location"
    }'
```

> When successful (200), returns a feedback object structured like this:

```json
{
    "id": 12345,
    "store_id": "6789",
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

**POST** `v3/:loyalty_club_slug/stores`

Create store

### POST Parameters (JSON)

Parameter | Description       | Type
---------- | ---------------- | ------
store_id*  | store id         | string
name*      | store name       | string
mall_id    | mall id          | string
parent_id  | internal mall id | string
category   | categories       | array
unit       | unit             | string

Parameters with `*` are required

### Feedback statuses

Status | Description
---- | ----
ok   | Message has been accepted for sending
Store with this id already exists. | 
Mall with this id does not exist. | 

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Stores:Create</code>
</aside>

## <a name="v3-store-update"></a> Update

> Example:

```shell
curl -X PUT \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/12345" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
    -d \
    '{
        "name": "store name",
        "mall_id": 12345,
        "parent_id": 6789,
        "categories": ["category"],
        "unit": "location"
    }'
```

> When successful (200), returns a feedback object structured like this:

```json
{
    "id": 12345,
    "store_id": "6789",
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

> When `id` or `store_id` is invalid (422), returns:

```json
{
    "error": "Store not found"
}
``` 

Update store

**PUT** `v3/:loyalty_club_slug/stores/:id`

**PUT** `v3/:loyalty_club_slug/stores/by_store_id/:store_id`

### URL Parameters

Parameter  | Description | Type
---------- | ----------- | ------
id         | ID          | integer
store_id   | store id    | string

### PUT Parameters (JSON)

Parameter | Description       | Type
---------- | ---------------- | ------
name*      | store name       | string
mall_id    | mall id          | string
parent_id  | internal mall id | string
category   | categories       | array
unit       | unit             | string

Parameters with `*` are required

### Feedback statuses

Status | Description
---- | ----
ok   | Message has been accepted for sending
Mall with this id does not exist. | 

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Stores:Update</code>
</aside>

## <a name="v3-store-delete"></a> Delete

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/12345" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns a feedback object structured like this:

```json
{
    "id": 1113,
    "store_id": null,
    "status": "ok"
}
```  

> When `id` or `store_id` is invalid (422), returns:

```json
{
    "error": "Store not found"
}
``` 

Delete store

**DELETE** `v3/:loyalty_club_slug/stores/:id`

**DELETE** `v3/:loyalty_club_slug/stores/by_store_id/:store_id`

### URL Parameters

Parameter  | Description | Type
---------- | ----------- | ------
id         | ID          | integer
store_id   | store id    | string

### Feedback statuses

Status | Description
------ | ----
ok     | Store was deleted


### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Stores:Delete</code>
</aside>