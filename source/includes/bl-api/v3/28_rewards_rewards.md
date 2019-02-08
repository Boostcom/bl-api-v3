# Endpoints &bull; Rewards (draft)
## <a name="v3-rewards-list"></a> List

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/rewards" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
  "rewards": [
    {
      "id": 1000062,
      "name": "Awesome Toaster",
      "description": "It has incredible features.",
      "price": 300,
      "remaining_stock": 15,
      "files": [
        {
          "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000062-default-mobile.jpg",
          "width": 225,
          "height": 300,
          "kind": "reward_default",
          "size_type": "thumbnail"
        },
        {
          "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000062-default-mobile.jpg",
          "width": 300,
          "height": 400,
          "kind": "reward_default",
          "size_type": "base"
        },
        {
          "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000062-default-mobile.jpg",
          "width": 600,
          "height": 800,
          "kind": "reward_default",
          "size_type": "original"
        }                
      ]
    }
  ]
}

```

**GET** `v3/infinity-mall/members/me/rewards-program/rewards`

Returns list of rewards available to purchase by member.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
rewards | Array | List of Rewards - see below

### <a name="v3-reward-model"></a> Reward

Key | Type | Description
--------- | --------- | ---------
id | integer | 
name | string | (optional) 
description | string | (optional)
price | integer | Number of points that must be spent to purchase the reward
remaining_stock | integer | (optional) Number of items that is available for purchase. When null, there is no limit
files | Array | A list of Reward Files - see below

### <a name="v3-rewards-file"></a> Reward File

(@todo: describe File Model globally)

Key | Type | Description
--- | --- | -----------
url | URL | Link to image
width | integer | Image width
height | integer | Image height
kind | string | Kind of image, always "reward_default"
size_type | string | One of: 'original', 'base', 'thumbnail' 

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Rewards:List</code> permit
</aside>

## <a name="v3-rewards-list-purchased"></a> List purchased

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/rewards/purchased" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
  "rewards": [
    {
      "id": 1000062,
      "name": "Awesome Toaster",
      "description": "It has incredible features.",
      "files": [
        {
          "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000062-default-mobile.jpg",
          "width": 225,
          "height": 300,
          "kind": "reward_default",
          "size_type": "thumbnail"
        },
        {
          "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000062-default-mobile.jpg",
          "width": 300,
          "height": 400,
          "kind": "reward_default",
          "size_type": "base"
        },
        {
          "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000062-default-mobile.jpg",
          "width": 600,
          "height": 800,
          "kind": "reward_default",
          "size_type": "original"
        }                
      ],
      "usage": {
        "limit": 5,
        "left": 3,
        "usable": true,
        "active_until": "2019-01-31T11:24:15.278Z"
      }
    }
  ]
}
```

**GET** `v3/infinity-mall/members/me/rewards-program/rewards/purchased`

Returns list of rewards purchased by member.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

### Response (JSON object)

Response looks as in [Reward list](#v3-reward-list), but - additionally - each reward includes reward usage information

### <a name="v3-rewards-usage"></a> Usage

Key | Type | Description
--------- | --------- | ---------
limit | integer | (optional) How many times member can use the reward in total. If null, there's no limit
left | integer | (optional) How many usages are left for member. Null, when there's no usage limit - see above
usable | boolean | Is the reward usable currently?
active_until | Date | (optional) The last time time the coupon has been used + activation time (configurable per Loyalty Club, e.x. 30s). When null, the reward has not been activated yet

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Rewards:ListPurchased</code> permit
</aside>

## <a name="v3-rewards-purchase"></a> Purchase

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/rewards/153/purchase" \
    -H 'Content-Type: application/json' \
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

**POST** `v3/infinity-mall/members/me/rewards-program/rewards/:id/purchase`

Adds one of available rewards to member's purchased list.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Rewards:Purchase</code> permit
</aside>

## <a name="v3-rewards-use"></a> Use

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/rewards/153/use" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an empty object:

```json
{
  "usage": {
    "limit": 5,
    "left": 3,
    "usable": false,
    "active_until": "2019-01-31T11:24:15.278Z"
  }
}
```

**POST** `v3/infinity-mall/members/me/rewards-program/rewards/:id/use`

Uses (activates) one of usable rewards purchased by member.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
usage | Object| See [Reward Usage](#v3-rewards-usage)

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Rewards:Use</code> permit
</aside>
