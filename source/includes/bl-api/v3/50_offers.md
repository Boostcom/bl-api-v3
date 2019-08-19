# Endpoints &bull; Offers (WIP)

NOTE: The Offers API is in development state. Changes (rather small) may occur during our mobile apps development & testing process.

## Common params

### <a name="v3-offers-preview"></a> `preview` query param

It is possible to use some endpoints without authenticated user by adding `preview=true` query param.

This allows to preview the endpoint's content without requiring the user to be signed-in.

With this param given, authentication will not be required (`Authorization` header may be skipped) and therefore, 
response will be user-agnostic.

## Common error responses

Status | Response body
--------- | ----------- 
`485` | `{"error": "Offers API is not enabled for the Loyalty Club"}` |

## Common models

### <a name="v3-offer-model"></a> Offer

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
  "shops": ["M&H"],
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
external_url | string| yes | The URL the offer may be linked with 
read_more | Object | no | Additional offer display data
read_more.button_title | string | yes |  
read_more.url | string | yes |  
read_more.title | string | yes |
read_more.text | string | yes |
usable_since | Date | yes | Time since the offer may be used. If null, there is no restriction. 
usable_until | Date | yes | Time until the offer may be used. If null, there is no restriction.
collection_ids | integer[] | no (may be empty) | List of collections ids the offer belongs to
tags | string[] | no (may be empty) | List of tags identifiers associated with the offer. Information from [Translations &bull; List](#v3-list-translations) should be used to display the tags. 
shops | string[] | no (may be empty) | List of shop names associated with the offer
files | File[] | no | A list of offer files - see [File model](#v3-file-model)
liked | Boolean | no | Has offer been liked by user?
usage | Usage | no | Usage information object - see [Offer usage model](#v3-offer-usage-model)
extras | Object | no (may be empty) | Extendable container for any potential extra data 

### <a name="v3-offer-usage-model"></a> Offer usage

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
usable | Boolean | no | Is the offer usable?  
max_uses | integer | yes | Maximum number of times the specific member can use the offer. When null, there's no limit.
uses_left | integer | yes| How many more times the specific member can use the offer - this number can be also affected by global offer limit. When null, there's no limit.
active_until | Date | yes | The last time time the offer has been used at+ activation time (configurable per Loyalty Club, e.x. 30s). When null, the offer has not been activated (used) yet

### <a name="v3-offers-collection-model"></a> Collection

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
id | integer | no | 
title | string | yes |  
description | string | yes | 
short_description | string | yes |
offers_order | integer[] | no (may be empty) | Describes order in which offers should be displayed inside this collection 
files | File[] | no | A list of Offer Files - see [File model](#v3-file-model)

## <a name="v3-offers-meta"></a> Get offers meta

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/offers/meta" \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
    "tags": ["-50%", "great deal", "2 + 1"],
    "shops": ["M&H", "Doo Doo"],
    "collections": [
        {
            "id": 1000016,
            "title": "Collection #1",
            "short_title": null,
            "description": null,
            "offers_order": [1000115, 1000112, 1000114],
            "files": [
                {
                    "url": "https://offers-api.s3.eu-central-1.amazonaws.com/collection-1000016-collection_cover_default-original.jpg",
                    "width": 600,
                    "height": 300,
                    "kind": "collection_cover_default",
                    "size_type": "original"
                },
                {
                    "url": "https://offers-api.s3.eu-central-1.amazonaws.com/collection-1000016-collection_cover_default-thumbnail.jpg",
                    "width": 300,
                    "height": 150,
                    "kind": "collection_cover_default",
                    "size_type": "thumbnail"
                },
                {
                    "url": "https://offers-api.s3.eu-central-1.amazonaws.com/collection-1000016-collection_cover_default-base.jpg",
                    "width": 600,
                    "height": 300,
                    "kind": "collection_cover_default",
                    "size_type": "base"
                }
            ]
        }
    ],
    "activated_seconds_time": 35,
    "usage_authorization_tokens": ["secret", "another-secret", "06edd7397efa83b1e4d07a195324b8a5a"]
}
```

**GET** `v3/infinity-mall/members/me/offers/meta` [[OAuth](#v3-oauth2)]

Returns offers metadata which consists of:

* aggregated lists of records currently associated with offers (shops, tags, collections)
* Loyalty Club configuration

### Query Parameters

Parameter | Type | Default | Type
--------- | ----------- | --------- | ------
preview | Boolean | false | See [`preview` param](#v3-offers-preview)

### Response (JSON object)

Key | Type | Optional? | Description 
--------- | --------- | ---------- | ---------
tags | string[] | no (may be empty) | List of unique tags assigned to offers 
shops | string[] | no (may be empty) | List of unique shops assigned to offers
collections | Collection[] |  no (may be empty) | List of unique collections assigned to offers - see [Collection model](#v3-offers-collection-model)
activated_seconds_time | integer | no | Number of seconds the offers stays active after member uses it 
usage_authorization_tokens | string[] | no (may be empty) | List of tokens permitted to use the offers. See [`X-Usage-Token` header](#v3-usage-token)

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:GetMeta</code> permit
</aside>

## <a name="v3-get-offer"></a> Get offer

**GET** `v3/infinity-mall/members/me/offers/:id` [[OAuth](#v3-oauth2)]

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/offers/1000016" \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an Offer object under "offer" key. See [Offer model](#v3-offer-model)

```json
{
  "offer": {
    // (...) - see Offer model
  }
}
```

Returns details of the specified offer.

### Query Parameters

Parameter | Type | Default | Type
--------- | ----------- | --------- | ------
preview | Boolean | false | See [`preview` param](#v3-offers-preview)

### Response (JSON object)

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
offer | Offer | no | See [Offer model](#v3-offer-model) 

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:Get</code> permit
</aside>

## <a name="v3-list-offers"></a> List offers

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/offers" \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns a list of offers and pagination info. 
  See [Offer model](#v3-offer-model) and [Pagination info model](#pagination-json-model)

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

**GET** `v3/infinity-mall/members/me/offers` [[OAuth](#v3-oauth2)]

Returns offers list.

### Query Parameters

Parameter | Type | Default | Description
--------- | ----------- | --------- | -----------
per_page | integer | 100 | Number of results to be returned per request (100 is the maximum)
page_no | integer | 1 | Number of results page
preview | boolean | false | See [`preview` param](#v3-offers-preview)
include_total | boolean | false | When true, detailed pagination info (containing info like total records count, next page) will be returned
order_by | string | "name" | See [`order_by` param](#v3-offers-list-order-by)
collection_ids | integer[] | null |  When present, only offers belonging to least one of given collections will be returned
tags | string[] | null |  When present, only offers having at least one of given tags will be returned
shops | string[] | null | When present, only offers having at least one of given shops will be returned
campaign_id | integer[] | null | When present, only offers with this campaign_id will be returned
search | string | null | When present, only offers that match the query string will be returned
only_usable | boolean | false | When true, only offers that are usable will be returned

All parameters are optional.

#### <a name="v3-offers-list-order-by"></a> `order_by` param

Offers list may be sorted by `order_by` param, which has similar syntax as ORDER BY keyword in SQL statements.

For example `order_by=usable ASC, created_at DESC` will return offers sorted by their usability, then by their creation date.

Following offer attributes may be used for ordering:

* name
* usable_since
* usable_until
* created_at
* usable 

### Response (JSON object)

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
offers | Offer[] | no (may be empty)| See [Offer model](#v3-offer-model) 
pagination_info | PaginationInfo | no| See [Pagination info model](#pagination-json-model)

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:ListVisibleByMemberId</code> permit
</aside>

## <a name="v3-use-offer"></a> Use offer

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/offers/1000016" \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns a Offer usage object - See [Offer usage model](#v3-offer-usage-model)

```json
{
    "usage": {
        "usable": true,
        "max_uses": 100,
        "uses_left": 68,
        "active_until": "2019-05-08T14:47:22.323Z"
    }
}
```

**POST** `v3/infinity-mall/members/me/offers/:id/use` [[OAuth](#v3-oauth2)]

Uses (activates) the offer by member.

### <a name="v3-usage-token"></a> `X-Usage-Token` header

It is possible to require user to provide a token (for example, encoded as a QR code) before he is able to use the offer.

In such case, the token should be sent as a `X-Usage-Token` header. When such header is present, it's value 
is validated against the list of tokens configured for Loyalty Club. 
A corresponding error message will be returned when token happens to be invalid.

### Error responses

Status | Response body | Description
--------- | ----------- | -------- 
`404` | `{"error": "Offer#10000951 not found"}`| -
`422` | `{"error": "Already active"}` | The offer has been just used
`422` | `{"error": "Not granted"}` | The offer has not been purchased by member
`422` | `{"error": "Usage authorization token invalid"}` | Provided usage token is invalid
`422` | `{"error": "User limit exceeded"}` | There are no more offers available to purchase for the member
`422` | `{"error": "Global limit exceeded"}` | There are no more offers available globally (stock is empty)
`422` | `{"error": "Not in usable timeframes"}` | Offer is not usable yet or anymore

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:Use</code> permit
</aside>

## <a name="v3-like-offer"></a> Like offer

> Example

```shell
curl -X PUT \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/offers/1000016/like" \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an empty object:

```json
{
  // Empty object
}
```

**PUT** `v3/infinity-mall/members/me/offers/:id/like` [[OAuth](#v3-oauth2)]

Marks offer as "liked" by user.

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:Like</code> permit
</aside>

## <a name="v3-unlike-offer"></a> Unlike offer

> Example

```shell
curl -X PUT \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/offers/1000016/unlike" \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an empty object:

```json
{
  // Empty object
}
```

**PUT** `v3/infinity-mall/members/me/offers/:id/unlike` [[OAuth](#v3-oauth2)]

Marks offer as "unliked" by user.

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:Like</code> permit
</aside>
