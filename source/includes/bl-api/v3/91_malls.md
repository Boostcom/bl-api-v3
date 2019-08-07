# Endpoints &bull; Malls

## <a name="v3-mall-list"></a> List

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/malls" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an array of mall objects structured like below. Some values may be omitted if were not provided:

```json
[
  {
      "id": 12345,
      "mall_id": 67890,
      "name": "Mall name"
  }
]
``` 

**GET** `v3/:loyalty_club_slug/stores/malls`

List customer malls

<aside class="notice">
Requires <code>Stores:Api:Malls:Get</code> permit
</aside> 

## <a name="v3-mall-get"></a> Get

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/stores/malls/123456" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns mall object structured like below. Some values may be omitted if were not provided:

```json
{
  "id": 12345,
  "mall_id": 67890,
  "name": "Mall name"
}
``` 

> When `id` is invalid (422), returns:

```json
{
    "error": "Mall not found"
}
``` 

**GET** `v3/:loyalty_club_slug/stores/malls/:id`

**GET** `v3/:loyalty_club_slug/stores/malls/by_mall_id/:mall_id`

Return mall by one of two identifiers: `id` or `mall_id`


### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | ID internal | integer
mall_id | Mall id from customer system | integer

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Stores:Api:Malls:Get</code> permit
</aside> 