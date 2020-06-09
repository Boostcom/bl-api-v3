# Endpoints &bull; Stores

## <a name="v3-store-list"></a> List

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores?department_id=123&search=test&category=cat&limit=100&sort=name&order=asc" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an array of store objects structured like below. Some values may be omitted if were not provided:

```json
[
    {
        "id": 1234,
        "parent_id": 1234,
        "store_id": "1234",
        "department_id": 1234,
        "name": "Store name",
        "department": "Departament name",
        "categories": [
          "category 1",
          "category 2"
          ],
        "floor": "floor",
        "location": "location",
        "area": 1000,
        "zones": [
          "zone"
        ],
        "opening_hours": [
            {
                "name": "monday",
                "hours": "10:00-20:00"
            },
            {
                "name": "tuesday",
                "hours": "10:00-20:00"
            },
            {
                "name": "wednesday",
                "hours": "10:00-20:00"
            },
            {
                "name": "thursday",
                "hours": "10:00-20:00"
            },
            {
                "name": "friday",
                "hours": "10:00-20:00"
            },
            {
                "name": "saturday",
                "hours": "10:00-18:00"
            },
            {
                "name": "sunday",
                "hours": "closed"
            }
        ],
        "logo_url": "http://foo.foo/foo.png",
        "description": "Description"
    }
]
``` 

**GET** `v3/:loyalty_club_slug/stores`

List of all active stores

### URL Parameters

Parameter     | Description                         | Type
------------- | ----------------------------------- | ------
department_id | department id                       | integer
category      | filter by single category           | string
zone          | filter by single zone               | string
name          | filter by part of store name        | string
search        | filter by all searchable fields     | string
limit         | max number of results               | integer
sort          | field to sort by (name,id,store_id) | string
order         | order (asc,desc)                    | string

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

> When successful (200), returns department object structured like below. Some values may be omitted if were not provided:

```json
{
        "id": 1234,
        "parent_id": 1234,
        "store_id": "1234",
        "department_id": 1234,
        "name": "Store name",
        "department": "Departament name",
        "categories": [
          "category 1",
          "category 2"
          ],
        "floor": "floor",
        "location": "location",
        "area": 1000,
        "zones": [
          "zone"
        ],
        "opening_hours": [
            {
                "name": "monday",
                "hours": "10:00-20:00"
            },
            {
                "name": "tuesday",
                "hours": "10:00-20:00"
            },
            {
                "name": "wednesday",
                "hours": "10:00-20:00"
            },
            {
                "name": "thursday",
                "hours": "10:00-20:00"
            },
            {
                "name": "friday",
                "hours": "10:00-20:00"
            },
            {
                "name": "saturday",
                "hours": "10:00-18:00"
            },
            {
                "name": "sunday",
                "hours": "closed"
            }
        ],
        "logo_url": "http://foo.foo/foo.png",
        "description": "Description"
    }
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
id        | ID internal | integer
store_id  | Store id from customer system | string

### Error responses

Status    | Description
--------- | ----------- 
`422`     | Invalid parameters (see example on the right)

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
        "department_id": 12345,
        "categories": [
            "category"
        ],
        "floor": "2nd Floor",
        "location": "12a",
        "area": 50,
        "zones": [
            "zone"
        ],
        "opening_hours": [
			{
			    "name":"monday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"tuesday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"wednesday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"thursday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"friday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"saturday",
			    "hours":"10:00-18:00"
			},
			{
			    "name":"sunday",
			    "hours":"closed"
			}
        	],
        "description": "Description"
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

Parameter     | Description            | Type
------------- | ---------------------- | ------
store_id     | store id               | string
name*         | store name             | string
department_id | department/mall id          | string
categories    | categories             | array
zones         | zones                  | array
floor         | foor                   | string
location      | location               | string
area          | area                   | int
opening_hours | opening hours          | array of day objects
description   | description            | string

Day object for opening hours

Parameter     | Description            | Type
------------- | ---------------------- | ------
name*         | day of the week        | string
hours*        | opening hours          | string

Parameters with `*` are required

### Feedback statuses

Status | Description
---- | ----
ok   | Store was created
This store already exists. | 

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
        "department_id": 12345,
        "categories": [
            "category"
        ],
        "floor": "2nd Floor",
        "location": "12a",
        "area": 50,
        "zones": [
            "zone"
        ],
        "opening_hours": [
			{
			    "name":"monday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"tuesday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"wednesday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"thursday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"friday",
			    "hours":"10:00-20:00"
			},
			{
			    "name":"saturday",
			    "hours":"10:00-18:00"
			},
			{
			    "name":"sunday",
			    "hours":"closed"
			}
        	],
        "description": "Description"
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

Parameter     | Description            | Type
------------- | ---------------------- | ------
name*         | store name             | string
department_id | department/mall id     | string
categories    | categories             | array
zones         | zones                  | array
floor         | foor                   | string
location      | location               | string
area          | area                   | int
opening_hours | opening hours          | array of day objects
description   | description            | string

Day object for opening hours

Parameter     | Description            | Type
------------- | ---------------------- | ------
name*         | day of the week        | string
hours*        | opening hours          | string

Parameters with `*` are required

### Feedback statuses

Status | Description
------ | ----
ok     | Store updated
Store with this id does not exist. | 

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
Store with this id does not exist. | 


### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Stores:Delete</code>
</aside>

## <a name="v3-store-categories-list"></a> Categories

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/categories" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an array of store objects structured like below. Some values may be omitted if were not provided:

```json
[
    "category A",
    "category B",
    "category C"
]
``` 

**GET** `v3/:loyalty_club_slug/stores/categories`

List of all store categories


<aside class="notice">
Requires <code>Stores:Api:Zones:Get</code> permit
</aside> 

## <a name="v3-store-zones-list"></a> Zones

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/zones" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an array of store objects structured like below. Some values may be omitted if were not provided:

```json
[
    "zone A",
    "zone B",
    "zone C"
]
``` 

**GET** `v3/:loyalty_club_slug/stores/zones`

List of all zones


<aside class="notice">
Requires <code>Stores:Api:Zones:Get</code> permit
</aside>