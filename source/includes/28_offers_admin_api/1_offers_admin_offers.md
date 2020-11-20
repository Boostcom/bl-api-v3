## <a name="offer-admin-offers"></a> Offers

### <a name="get-offer"></a> Get offer

**GET** `v3/:loyalty_club_slug/offers/:id`

> Example:

```shell
curl \
"https://api.mpc.placewise.com/v3/infinity-mall/offers/1000016" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an Offer object under "offer" key. See [Offer model](#admin-offer-model)

```json
{
  "offer": {
    // (...) - see Offer model
  }
}
```

Returns details of the specified offer.

#### Response (JSON object)

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
offer | Offer | no | See [Offer model](#admin-offer-model) 

<aside class="notice">
Requires <code>Offers:Api:Offers:Get</code> permit
</aside>

### <a name="list-offers"></a> List offers

> Example:

```shell
curl \
"https://api.mpc.placewise.com/v3/infinity-mall/offers" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns a list of offers and pagination info. 
  See [Offer model](#admin-offer-model) and [Pagination info model](#pagination-json-model)

```json
{
  "offers": [
    {
      // (...) - see Offer model
    }
  ],
  "pagination_info": {
      // (...) - see Pagination info model
  }
}
```

**GET** `v3/:loyalty_club_slug/offers`

Returns offers list.

#### Query Parameters

Parameter | Type | Default | Description
--------- | ----------- | --------- | -----------
per_page | integer | 100 | Number of results to be returned per request (100 is the maximum)
page_no | integer | 1 | Number of results page
include_total | boolean | false | When true, detailed pagination info (containing info like total records count, next page) will be returned
sort_by | string | "name" | What attribute should results be sorted by? Supported attributes are: `["name", "usable_since", "usable_until", "visible_since", "visible_until", "created_at", "updated_at", "archived_at"]`
sort_direction | string | "asc" | Direction of sorting: "asc" or "desc"
collection_ids | integer[] | null |  When present, only offers belonging to least one of given collections will be returned
tags | string[] | null |  When present, only offers having at least one of given tags will be returned
stores | string[] | null | When present, only offers having at least one of given stores will be returned
campaign_id | integer[] | null | When present, only offers with this campaign_id will be returned
audience_id | integer[] | null | When present, only offers with this audience_id will be returned
search | string | null | When present, only offers that match the query string will be returned
ids | integer[] | null |  When present, only offers having one of provided IDs will be returned
without_collection | boolean | false | When present, only offers without collections will be returned
without_tag | boolean | false | When present, only offers without tags will be returned
without_audience_id | boolean | false | When present, only offers without audiences will be returned
without_campaign_id | boolean | false | When present, only offers without campaigns will be returned
without_shop | boolean | false | When present, only offers without stores will be returned
with_archived | boolean | false | When present, also archived offers will be returned
only_archived | boolean | false | When present, only archived offers will be returned

All parameters are optional.

#### Response (JSON object)

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
offers | Offer[] | no (may be empty)| See [Offer model](#admin-offer-model) 
pagination_info | PaginationInfo | no| See [Pagination info model](#pagination-json-model)

<aside class="notice">
Requires <code>Offers:Api:Offers:List</code> permit
</aside>

### <a name="create-offers"></a> Create offers

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/infinity-mall/offers" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    -d '
        {
            "offers":[
                {
                    "name": "Offer 1"
                },
                {
                    "name": "Offer 2"
                }
            ]
        }
    '
```

> Returns object structured like this: 

```json
{
    "success": true,
    "errors": {
        "1": {
            "collection_ids": [
                "list must contain unique values"
            ]
        }
    },
    "ids": [1000284]
}
```

**POST** `v3/:loyalty_club_slug/offers`

Creates offer(s) with given lists of attributes (OfferPayloads). 

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
offers | OfferPayload[] | List of offers to create. See [Offer payload](#admin-offer-payload)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
success | boolean | True if any offer has been created
ids | integer[] | IDs of offers that have been created 
errors | Object | Errors of offers that couldn't be created 

<aside class="notice">
Requires <code>Offers:Api:Offers:Create</code> permit
</aside>

### <a name="update-offers"></a> Update offers

> Example:

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v3/infinity-mall/offers" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    -d '
        {
            "offers":[
                {
                    "id": 1000284,
                    "name": "Offer 1"
                },
                {
                    "id": 1000285,
                    "name": "Offer 2"
                }
            ]
        }
    '
```

> Returns object structured like this: 

```json
{
    "success": true,
    "errors": {
        "1": {
            "collection_ids": [
                "list must contain unique values"
            ]
        }
    }
}
```

**PUT** `v3/:loyalty_club_slug/offers`

Updates offer(s) with given lists of attributes (OfferPayloads). Also, each payload record must contain offer ID that is meant to be updated.
Update is not partial, so all attributes that aren't present in the payload will be nullified. 

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
offers | OfferPayload[] | List of offers to update. See [Offer payload](#admin-offer-payload).

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
success | boolean | True if any offer has been created
ids | integer[] | IDs of offers that have been created 
errors | Object | Errors of offers that couldn't be created 

<aside class="notice">
Requires <code>Offers:Api:Offers:Update</code> permit
</aside>

### <a name="clone-offers"></a> Clone offers

> Example:

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v3/infinity-mall/offers/clone" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    -d '
        {
            "ids":[1000284, 1000285]
        }
    '
```

> When successful (200), returns IDs of offers that were created by cloning 

```json
{
    "ids": [1000301, 1000302]
}
```

**POST** `v3/:loyalty_club_slug/offers/clone`

Clones given offers (by their IDs)

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
ids | integer[] | List of offers ids to clone

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
ids | integer[] | IDs of offers that have been cloned 

<aside class="notice">
Requires <code>Offers:Api:Offers:Clone</code> permit
</aside>

### <a name="grant-offers"></a> Grant offers

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/infinity-mall/offers/grant" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    -d '
        {
            "offers": [
                {
                    "id": 47,
                    "audience_id": 572,
                    "usable_for_seconds": 60
                },
                {
                    "id": 49,
                    "audience_id": 572,
                }
            ]
        }
    '
```

> When successful (200), returns an empty JSON

```json
{}
```

**POST** `v3/:loyalty_club_slug/offers/grant`

Grants offer(s) to members of given audience(s).

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
offers[].id | integer | ID of offer to grant
offers[].audience_id | integer | ID of audience which members should be granted with the offer
offers[].usable_for_seconds | integer | (optional) When present, the granted offer will be available for use by member only for the defined time

<aside class="notice">
Requires <code>Offers:Api:Offers:Grant</code> permit
</aside>

### <a name="update-offers"></a> Delete offers

> Example:

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v3/infinity-mall/offers" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    -d '
        {
            "ids":[1000284, 1000285]
        }
    '
```

> When successful (200), returns an empty object

```json
{
  // Empty object
}
```

**DELETE** `v3/:loyalty_club_slug/offers`

Deletes given offers (by their IDs).

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
ids | integer[] | List of offers ids to delete

<aside class="notice">
Requires <code>Offers:Api:Offers:Destroy</code> permit
</aside>
