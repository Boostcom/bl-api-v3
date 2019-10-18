# <a name="v3-offers-admin"></a> Endpoints &bull; Offers Admin (WIP)

This section describes endpoints destined for offers management. 
Navigate to [Offers](#v3-offers) section to see docs for member-related endpoints.

NOTE: The Offers Admin API is in development state. Changes (rather small) may occur during our mobile apps development & testing process.

## Common error responses

Status | Response body
--------- | ----------- 
`485` | `{"error": "Offers API is not enabled for the Loyalty Club"}` |

## Common models

### <a name="v3-admin-offer-model"></a> Offer

> Offer example:

```json
{
  "id": 1000115,
  "name": "Buy two shoes, get one jacket",
  "description": null,
  "external_url": "https://example.com/foo",
  "read_more": { "button_title": "Button title", "url": null, "title": "Title", "text": "Text" },
  "usable_since": "2019-03-13T17:32:00.000Z",
  "usable_until": null,
  "collection_ids": [1000016, 1000017],
  "tags": ["2 + 1"],
  "stores": ["M&H"],
  "files": [
    {
      "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000115-offer_default-original.jpg",
      "width": 600,
      "height": 800,
      "kind": "offer_default",
      "size_type": "base"
    }
  ],
  "liked": false,
  "usage": {
    "usable": true,
    "max_uses": 10,
    "uses_left": 3,
    "active_until": "2019-03-13T18:41:00.000Z"
  },
  "extras": {}
}
```

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
id | integer | no |  
name | string| yes | 
description | string| yes | 
internal_name | string| yes | 
internal_description | string| yes | 
external_url | string| yes | The URL the offer may be linked with 
read_more | Object | no | Additional offer display data
read_more.button_title | string | yes |  
read_more.url | string | yes |  
read_more.title | string | yes |
read_more.text | string | yes |
status | enum: `['draft', 'published']` | no | Offer is visible only when status is `'published'` 
usable_since | Date | yes | Time since the offer may be used. If null, there is no restriction. 
usable_until | Date | yes | Time until the offer may be used. If null, there is no restriction.
visible_since | Date | yes | Time since the offer is visible to members. If null, there is no restriction. 
visible_until | Date | yes | Time until the offer is visible to members. If null, there is no restriction.
collection_ids | integer[] | no (may be empty) | List of collections ids the offer belongs to
tags | string[] | no (may be empty) | List of tags identifiers associated with the offer. Information from [Translations &bull; List](#v3-list-translations) should be used to display the tags. 
stores | string[] | no (may be empty) | List of stores names associated with the offer. See [Stores &bull; List](#v3-store-list). NOTE: this attribute is called "shops" in member-facing API
audience_id | Integer | yes | DMP's audience ID. If it is set, the offer is available only to members belonging to this audience
campaign_id | Integer | yes | MPC's campaign ID
files | File[] | no | A list of offer files - see [File model](#v3-file-model)
extras | Object | no (may be empty) | Extendable container for any potential extra data
display_schemas | Object | no | Offer displays schemas - see [Offer display schemas](#v3-offer-display-schemas)
maximum_uses_per_user | Integer | yes | Limit of of uses individual member may perform. When empty, there's no limit.
stock | Integer | yes | Global limit of member uses. When empty, there's no limit.
created_at | Date | no | When the offer has been created 
updated_at | Date | no | Last time when the offer has been updated  
archived_at | Date | yes | Time when the offer has been archived
uses_count | Date | no | Total number of times the offer has been used by members   

### <a name="v3-admin-offer-payload"></a> OfferPayload

Following Offer attributes (described above) are available to set when creating or updating offers.
     
* name
* description
* internal_name
* internal_description
* external_url
* read_more
* status
* usable_since
* usable_until
* visible_since
* visible_until
* collection_ids
* tags
* stores
* audience_id
* campaign_id
* extras
* display_schemas
* maximum_uses_per_user
* stock

## <a name="v3-get-offer"></a> Get offer

**GET** `v3/:loyalty_club_slug/offers/:id`

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/offers/1000016" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an Offer object under "offer" key. See [Offer model](#v3-admin-offer-model)

```json
{
  "offer": {
    // (...) - see Offer model
  }
}
```

Returns details of the specified offer.

### Response (JSON object)

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
offer | Offer | no | See [Offer model](#v3-admin-offer-model) 

<aside class="notice">
Requires <code>Offers:Api:Offers:Get</code> permit
</aside>

## <a name="v3-list-offers"></a> List offers

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/offers" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns a list of offers and pagination info. 
  See [Offer model](#v3-admin-offer-model) and [Pagination info model](#pagination-json-model)

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

### Query Parameters

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

### Response (JSON object)

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
offers | Offer[] | no (may be empty)| See [Offer model](#v3-admin-offer-model) 
pagination_info | PaginationInfo | no| See [Pagination info model](#pagination-json-model)

<aside class="notice">
Requires <code>Offers:Api:Offers:List</code> permit
</aside>

## <a name="v3-create-offers"></a> Create offers

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/offers" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test' \
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

### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
offers | OfferPayload[] | List of offers to create. See [Offer payload](#v3-admin-offer-payload)

### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
success | boolean | True if any offer has been created
ids | integer[] | IDs of offers that have been created 
errors | Object | Errors of offers that couldn't be created 

<aside class="notice">
Requires <code>Offers:Api:Offers:Create</code> permit
</aside>

## <a name="v3-update-offers"></a> Update offers

> Example:

```shell
curl -X PUT \
"https://bpc-api.boostcom.no/v3/infinity-mall/offers" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test' \
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

### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
offers | OfferPayload[] | List of offers to update. See [Offer payload](#v3-admin-offer-payload).

### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
success | boolean | True if any offer has been created
ids | integer[] | IDs of offers that have been created 
errors | Object | Errors of offers that couldn't be created 

<aside class="notice">
Requires <code>Offers:Api:Offers:Update</code> permit
</aside>

## <a name="v3-clone-offers"></a> Clone offers

> Example:

```shell
curl -X DELETE \
"https://bpc-api.boostcom.no/v3/infinity-mall/offers/clone" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test' \
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

**POST** `v3/:loyalty_club_slug/offers/clones`

Clones given offers (by their IDs)

### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
ids | integer[] | List of offers ids to clone

### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
ids | integer[] | IDs of offers that have been cloned 

<aside class="notice">
Requires <code>Offers:Api:Offers:Clone</code> permit
</aside>

## <a name="v3-update-offers"></a> Delete offers

> Example:

```shell
curl -X DELETE \
"https://bpc-api.boostcom.no/v3/infinity-mall/offers" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test' \
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

### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
ids | integer[] | List of offers ids to delete

<aside class="notice">
Requires <code>Offers:Api:Offers:Destroy</code> permit
</aside>
