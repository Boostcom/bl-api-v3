# <a name="offers-api"></a> Offers API

This section describes endpoints destined for end user (e.g. for mobile apps) that work only within context of (signed in) member.
Navigate to [Offers Admin](#offers-admin) section to see docs for offers management endpoints.

## Introduction

### Member context

Offers API supports two distinct approaches to work with context of member.

#### <a name="offers-oauth-context"></a> Member ID OAuth (recommended)

In the first one member context is resolved from MPC [OAuth](#oauth2) member session.

This setup requires use of endpoints that have `/me/` URL segment and requires token provided with `authorization` header.

#### <a name="offers-member-id-context"></a> Member ID
 
In this approach member context is resolved from Member's ID provided within URL.

This setup requires use of endpoints that have `/:member_id/` URL segment and requires no authentication.

This approach is designed for implementations in which member is authenticated by your system and 
**should not be used on front-end implementations**, where API token is exposed to an end user. 

#### <a name="offers-guest-access"></a> Guest access

Some API client implementations may need to present content to a non-authenticated user.
 
In order to allow this, static endpoints (like offers list) may work without member context.

They exist in versions which use `/guest/` URL segment instead of `/me/` or `/member_id/` and may be used by both
aforementioned approaches.

### <a name="offer-model"></a> Offer model

> Offer example:

```json
{
  "id": 1000115,
  "name": "Buy two shoes, get one jacket",
  "type": "REGULAR",
  "description": null,
  "external_url": "https://example.com/foo",
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
  "display": {
    "read_more": { "button_title": "Button title", "url": null, "title": "Title", "text": "Text" },
    "activation_texts": { "modal": "Modal", "button": "Button", "description": null },
    "schemas": {
        "list": {
            "header": {
                "type": "reference",
                "value": "$.name"
            },
            "body": {
                "type": "reference",
                "value": "$.description"
            },
            "caption": {
                "type": "reference",
                "value": "$.stores"
            }
        },
        "details": {
            "header": {
                "type": "reference",
                "value": "$.description"
            },
            "body": {
                "type": "empty",
                "value": null
            },
            "caption": {
                "type": "inline",
                "value": "Some great offer we have here!"
            }
        }
    }
  },
  "liked": false,
  "usage": {
    "usable": true,
    "max_uses": 10,
    "uses_left": 3,
    "active_until": "2019-03-13T18:41:00.000Z",
    "period": null
  },
  "extras": {}
}
```

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
id | integer | no |
name | string| yes |
type | enum: ['REGULAR', 'EXCLUSIVE', 'BIRTHDAY', 'WELCOME'] | no |
description | string| yes |
external_url | string| yes | The URL the offer may be linked with 
usable_since | Date | yes | Time since the offer may be used. If null, there is no restriction. 
usable_until | Date | yes | Time until the offer may be used. If null, there is no restriction.
collection_ids | integer[] | no (may be empty) | List of collections ids the offer belongs to
tags | string[] | no (may be empty) | List of tags identifiers associated with the offer. Information from [Translations &bull; List](#list-translations) should be used to display the tags. 
stores | string[] | no (may be empty) | List of shop names associated with the offer
files | File[] | no | A list of offer files - see [File model](#file-model)
liked | Boolean | no | Has offer been liked by user?
usage | Usage | no | Usage information object - see [Offer usage model](#offer-usage-model)
extras | Object | no (may be empty) | Extendable container for any potential extra data
display | Object | no | Stores display information
display.schemas | Object | no | Offer displays schemas - see [Offer display schemas](#offers-display-schemas)
display.read_more | Object | yes |
display.read_more.button_title | string | yes |
display.read_more.url | string | yes |
display.read_more.title | string | yes |
display.read_more.text | string | yes |
display.read_more | Object | yes |
activation_texts.modal | string | yes |
activation_texts.button | string | yes |
activation_texts.description | string | yes |

### <a name="offer-usage-model"></a> Offer usage model

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
usable | Boolean | no | Is the offer usable?  
max_uses | integer | yes | Maximum number of times the specific member can use the offer. When null, there's no limit.
uses_left | integer | yes| How many more times the specific member can use the offer - this number can be also affected by global offer limit. When null, there's no limit.
active_until | Date | yes | The last time time the offer has been used at+ activation time (configurable per Loyalty Club, e.x. 30s). When null, the offer has not been activated (used) yet
period | Object | yes | If present, it describes limtations of using the offer by member within specific period
period.timespan | enum: ['MINUTE', 'HOUR', 'DAY', 'WEEK', 'MONTH', 'YEAR'] | no | Defines the period in which usage is limited
period.max_uses | integer | no | Defines how many times the offer may be used by member within period 
period.uses_left | integer | yes | Defines how many times member may use the offer until end of current period 

### <a name="offers-collection-model"></a> Collection model

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
id | integer | no | 
title | string | yes |  
description | string | yes | 
short_description | string | yes |
offers_order | integer[] | no (may be empty) | Describes order in which offers should be displayed inside this collection 
files | File[] | no | A list of Offer Files - see [File model](#file-model)

### <a name="offers-display-schemas"></a> Display schemas

> Example schemas:

```json
// (...)
"display": {
    "list": {
        "header": {
            "type": "reference",
            "value": "$.name"
        },
        "body": {
            "type": "reference",
            "value": "$.description"
        },
        "caption": {
            "type": "reference",
            "value": "$.stores"
        }
    },
    "details": {
        "header": {
            "type": "reference",
            "value": "$.description"
        },
        "body": {
            "type": "empty",
            "value": null
        },
        "caption": {
            "type": "inline",
            "value": "Some great offer we have here!"
        }
    }
}
// (...)
```

Display schemas are the offer attributes that describe how offer is meant to be displayed inside specific 
context - and there are two types of schemas:

  * `list` - describes how offer should look in an offers list view
  * `details` - describes how offer should look in an offer detail view

Each schema consists of fields with definitions of content that should be put inside the field - there are three of them:

  * `header`
  * `body`
  * `caption`

The fields interpretation (how they are utilized by view) is up to API client design and/or specific Loyalty Club standard.

Fields are described by two attributes: `type` and `value`. 
There are three types of fields:

  * `"reference"` - some offer attribute should be used for the field content, `value` contains [JSONPath](https://support.smartbear.com/readyapi/docs/testing/jsonpath-reference.html) reference to element, e.g. `$.name`  
  * `"inline"` - the field itself contains content that should be placed inside the field, which is stored in the `value` attribute
  * `"empty"` - the field should not be displayed at all

By default (when no schema is defined on offer by it's creator), all of the schema fields are references to:

  * `caption` => `stores`
  * `header` => `name`
  * `body` => `description`
 

## <a name="offers"></a> Offers

### <a name="offers-meta"></a> Get offers meta

> Example:

```shell
curl \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/offers/meta" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
    "tags": ["-50%", "great deal", "2 + 1"],
    "stores": ["M&H", "Doo Doo"],
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
    "activated_seconds_time": 35
}
```

**GET** `v3/:loyalty_club_slug/members/me/offers/meta` [[OAuth](#oauth2)]

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:GetMeta</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/:member_id/offers/meta`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:GetMetaByMemberId</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/guest/offers/meta`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:GetMetaPreview</code> permit
</aside>

Returns offers metadata which consists of:

* aggregated lists of records currently associated with offers (stores, tags, collections)
* Loyalty Club configuration

#### Response (JSON object)

Key | Type | Optional? | Description 
--------- | --------- | ---------- | ---------
tags | string[] | no (may be empty) | List of unique tags assigned to offers 
stores | string[] | no (may be empty) | List of unique stores assigned to offers
collections | Collection[] |  no (may be empty) | List of unique collections assigned to offers - see [Collection model](#offers-collection-model)
activated_seconds_time | integer | no | Number of seconds the offers stays active after member uses it 

### <a name="get-offer"></a> Get offer

**GET** `v3/:loyalty_club_slug/members/me/offers/:id` [[OAuth](#oauth2)]

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:Get</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/:member_id/offers/:id`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:GetByMemberId</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/guest/offers/:id`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:GetPreview</code> permit
</aside>


> Example:

```shell
curl \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/offers/1000016" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an Offer object under "offer" key. See [Offer model](#offer-model)

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
offer | Offer | no | See [Offer model](#offer-model) 

### <a name="list-offers"></a> List offers

> Example:

```shell
curl \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/offers" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns a list of offers and pagination info. 
  See [Offer model](#offer-model) and [Pagination info model](#pagination-json-model)

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

**GET** `v3/:loyalty_club_slug/members/me/offers` [[OAuth](#oauth2)]

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:ListVisible</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/:member_id/offers`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:ListVisibleByMemberId</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/guest/offers`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:ListVisiblePreview</code> permit
</aside>

Returns offers list.

#### Query Parameters

Parameter | Type | Default | Description
--------- | ----------- | --------- | -----------
per_page | integer | 100 | Number of results to be returned per request (100 is the maximum)
page_no | integer | 1 | Number of results page
include_pagination_info | boolean | false | When true, pagination info (containing info like total records count, next page) will be returned
order_by | string | "name" | See [`order_by` param](#offers-list-order-by)
collection_ids | integer[] | null |  When present, only offers belonging to least one of given collections will be returned
tags | string[] | null |  When present, only offers having at least one of given tags will be returned
stores | string[] | null | When present, only offers having at least one of given stores will be returned
campaign_id | integer[] | null | When present, only offers with this campaign_id will be returned
search | string | null | When present, only offers that match the query string will be returned
usable | boolean | false | When true, only offers that are usable will be returned
liked | boolean | false | When true, only offers that have been liked by member

All parameters are optional.

##### <a name="offers-list-order-by"></a> `order_by` param

Offers list may be sorted by `order_by` param, which has similar syntax as ORDER BY keyword in SQL statements.

For example `order_by=usable ASC, created_at DESC` will return offers sorted by their usability, then by their creation date.

Following offer attributes may be used for sorting  in general:

* name
* usable_since
* usable_until
* created_at

Only in member context (does not work with `/guest/` context: 

* usable 
* uses_left
* liked 

Also, when querying offers by *single* collection_id, the list may also be sorted by:

* collection_position

#### Response (JSON object)

Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
offers | Offer[] | no (may be empty)| See [Offer model](#offer-model) 
pagination_info | PaginationInfo | yes| See [Pagination info model](#pagination-json-model)

### <a name="use-offer"></a> Use offer

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/offers/1000016" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    -d '
    {
      "authorization_token": "a1cc4e0b3ab54b7d8"
    }
    '
```

> When successful (200), returns a Offer usage object - See [Offer usage model](#offer-usage-model)

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

**POST** `v3/:loyalty_club_slug/members/me/offers/:id/use` [[OAuth](#oauth2)]

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:Use</code> permit
</aside>

**POST** `v3/:loyalty_club_slug/members/:member_id/offers/:id/use`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:UseByMemberId</code> permit
</aside>

Uses (activates) the offer by member.

#### POST Parameters (JSON)

Key | Type | Optional? | Description
--- | ---- | --------- | -----------
authorization_token | string | yes | See [below](#offer-use-authorization)

##### <a name="offer-use-authorization"></a> Offer use authorization

Some features (like Rewards Program) may require from member to provide some code (scanned QR code, for example), so we can register
his physical presence in the store.

When token is not provided, the offer use is processed as usual, except that some benefits (like Rewards points)
tied to it may not be granted to the member.

However, when token is provided and happens to be invalid, the offer use is not registered, and 422 error (see below) is returned.  

#### Response (JSON object)

Key | Type  | Description
--------- | -------- | ---------
usage | OfferUsage | See [Offer usage model](#offer-usage-model)

#### Error responses

Status | Response body | Description
--------- | ----------- | -------- 
`404` | `{"error": "Offer#10000951 not found"}`| -
`422` | `{"error": "Already active"}` | The offer has been just used
`422` | `{"error": "Not granted"}` | The offer is not available to  member
`422` | `{"error": "Usage authorization token invalid"}` | Provided usage token is invalid
`422` | `{"error": "User limit exceeded"}` | Member exhausted his pool of uses
`422` | `{"error": "Periodic user limit exceeded"}` | Member exhausted his pool of uses within current time period 
`422` | `{"error": "Global limit exceeded"}` | There are no more offers available globally (stock is empty)
`422` | `{"error": "Not in usable timeframes"}` | Offer is not usable yet or anymore
`467` | `{"error": "Member is banned!", "banned_until": "2137-10-28T17:24:06.207Z"}` | Member is banned

### <a name="like-offer"></a> Like offer

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/offers/1000016/like" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an empty object:

```json
{
  // Empty object
}
```

**PUT** `v3/:loyalty_club_slug/members/me/offers/:id/like` [[OAuth](#oauth2)]

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:Like</code> permit
</aside>

**PUT** `v3/:loyalty_club_slug/members/:member_id/offers/:id/like`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:LikeByMemberId</code> permit
</aside>

Marks offer as "liked" by user.

### <a name="unlike-offer"></a> Unlike offer

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/offers/1000016/unlike" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an empty object:

```json
{
  // Empty object
}
```

**PUT** `v3/:loyalty_club_slug/members/me/offers/:id/unlike` [[OAuth](#oauth2)]

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:Like</code> permit
</aside>

**PUT** `v3/:loyalty_club_slug/members/:member_id/offers/:id/unlike`

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:LikeByMemberId</code> permit
</aside>

Revers marking offer as "liked" by user.

### <a name="grant-offer"></a> Grant offer

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/infinity-mall/members/7371713/offers/10000951/unlike" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an empty object:

```json
{
  // Empty object
}
```

**POST** `v3/:loyalty_club_slug/members/:member_id/members/:member_id/offers/:id/grant`

Some types of offers are not available to members until they are granted to them - this endpoint allows to do this. 

It may make sense to grant a same offer to one member multiple times when offer has limited number of uses. 
In such case, each consecutive grant will increase the number of uses for this member.

NOTE: This endpoint does not have [OAuth](#oauth2) version 

<aside class="notice">
Requires <code>Offers:Api:MemberOffers:GrantByMemberId</code> permit
</aside>

#### POST Parameters (JSON)

Key | Type | Optional? | Description
--- | ---- | --------- | -----------
usable_for_seconds | integer | yes | When present, the granted offer will be available for use by member only for the defined time

#### Error responses

Status | Response body | Description
--------- | ----------- | -------- 
`422` | `{"error": "Not grantable"}` | The offer has type not eligible for granting
`422` | `{"error": "Global limit exceeded"}` | The offer has no more uses available 
`422` | `{"error": "Member not in audience"}` | The offer has been specify to only be available to members in specific audience and the member does not belong to it
`467` | `{"error": "Member is banned!", "banned_until": "2137-10-28T17:24:06.207Z"}` | Member is banned
