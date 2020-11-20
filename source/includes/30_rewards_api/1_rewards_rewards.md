##  Rewards
### <a name="rewards-list"></a> List

> Example:

```shell
curl \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/rewards-program/rewards" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
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
      "required_member_level": "2",
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

As a member-related action, it requires member authorization. See [OAuth](#oauth2).

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
rewards | Array | List of Rewards - see below

#### <a name="reward-model"></a> Reward

Key | Type | Description
--------- | --------- | ---------
id | integer | 
name | string | (optional) 
description | string | (optional)
price | integer | Number of points that must be spent to purchase the reward
required_member_level | string| (optional) Minimal level name the members needs to have to be able to purchase the reward. See [Levels](#rewards-program-levels-program)
remaining_stock | integer | (optional) Number of items that is available for purchase. When null, there is no limit
files | Array | A list of Reward Files - see [File model](#file-model)

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Rewards:List</code> permit
</aside>

### <a name="rewards-list-purchased"></a> List purchased

```shell
curl \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/rewards-program/rewards/purchased" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
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

As a member-related action, it requires member authorization. See [OAuth](#oauth2).

#### Response (JSON object)

Response looks as in [Reward list](#rewards-list), but with reward usage instead of purchasing info

#### <a name="rewards-usage"></a> Usage

Key | Type | Description
--------- | --------- | ---------
left | integer | (optional) How many more times the member can use the Reward. Null, when there's no usage limit
active_until | Date | (optional) The last time time the reward has been used at + activation time (configurable per Loyalty Club, e.x. 30s). When null, the reward has not been activated (used) yet
usable | boolean | Will the reward be still usable after activation time passes?

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Rewards:ListPurchased</code> permit
</aside>

### <a name="rewards-purchase"></a> Purchase

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/rewards-program/rewards/153/purchase" \
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

**POST** `v3/infinity-mall/members/me/rewards-program/rewards/:id/purchase`

Adds one of available rewards to member's purchased list and reduces member's balance by price.

As a member-related action, it requires member authorization. See [OAuth](#oauth2).

#### Error responses

Status | Response body | Description
--------- | ----------- | -------- 
`404` | `{"error": "Reward#10000951 not found"}`| -
`422` | `{"error": "Not enough points"}` | Member has no enough points to purchase the reward
`422` | `{"error": "Member level is not high enough"}` | Member level is not high enough to purchase the reward
`422` | `{"error": "Global limit exceeded"}` | There are no more rewards available to purchase
`467` | `{"error": "Member is banned!", "banned_until": "2137-10-28T17:24:06.207Z"}` | Member is banned

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Rewards:Purchase</code> permit
</aside>

### <a name="rewards-use"></a> Use

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/infinity-mall/members/me/rewards-program/rewards/153/use" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an empty object:

```json
{
  "usage": {
    "left": 3,
    "usable": false,
    "active_until": "2019-01-31T11:24:15.278Z"
  }
}
```

**POST** `v3/infinity-mall/members/me/rewards-program/rewards/:id/use`

Uses (activates) one of usable rewards purchased by member.

As a member-related action, it requires member authorization. See [OAuth](#oauth2).

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
usage | Object| See [Reward Usage](#rewards-usage)

#### Error responses

Status | Response body | Description
--------- | ----------- | -------- 
`404` | `{"error": "Reward#10000951 not found"}`| -
`422` | `{"error": "Already active"}` | The reward has been just used
`422` | `{"error": "Not granted"}` | The reward has not been purchased by member
`422` | `{"error": "User limit exceeded"}` | There are no more rewards available to purchase for the member
`467` | `{"error": "Member is banned!", "banned_until": "2137-10-28T17:24:06.207Z"}` | Member is banned

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Rewards:Use</code> permit
</aside>
