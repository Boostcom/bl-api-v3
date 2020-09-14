# Rewards API

## Introduction

Boostcom Rewards is a Loyalty Program that awards members for achieving some defined goals with points.

The points may be spent on acquiring (purchasing) special rewards and then using them.
Points may expire after some time.

Participation in program is optional, so member can enroll (join) to it and leave it.

### Achievements

Every Loyalty Club that has Boostcom Rewards enabled, has defined list of achievements that have rules attached to them.
The rules are described below.

### <a name="rewards-program-achievements"></a> Achievement model

Key | Type | Description
--------- | --------- | ---------
type | string | see [Achievement types](#rewards-program-achievement-types)
points | integer | How many points the achievement grants
limit | integer | Maximum number of times the member can have the achievement granted (when null, there's no limit)  
frequency | Object | How often the achievement may be granted to member (when null, there are no constraints). For example, achievement with frequency defined as `{"timespan": "day", "limit": 5}` means that member may get points for it at most 5 times a day.
frequency['timespan'] | string | One of: `['hour', 'day', 'week', 'month', 'year']`
frequency['limit'] | integer |

#### <a name="rewards-program-achievement-types"></a> Achievement types

type | Event
---- | -----------
app_opened | The member opened the loyalty club mobile application
coupon_used | Member used some coupon
link_clicked | Member clicked on link that has been sent from us
email_opened | Member opened an email that has been sent from us
push_opened | Member opened a push message that has been sent from us
wifi_approached | Member approached (logged in to) loyalty club's WiFi
geofence_approached | Member approached geofence defined by loyalty club mobile application
beacon_approached | Member approached one of loyalty club's beacons
consent_granted | Member granted one of loyalty club's consents

### <a name="rewards-program-levels-program"></a> Member levels

> Example configuration for regular levels

```json
{
    "type": "regular",
    "levels": [
        {
            "name": "1",
            "points_threshold": 0
        },
        {
            "name": "2",
            "points_threshold": 300
        },
        {
            "name": "3",
            "points_threshold": 500
        },
        {
            "name": "4",
            "points_threshold": 500
        }
    ]
}
```

> Example configuration for virtual level

```json
{
    "type": "virtual_level",
    "maximum_points": 1500
}
```

Loyalty Club may have members level defined (available in [Rewards Program &bull; Get info](#rewards-program-info) response).
There are two types of them: `regular` and `virtual_level`.

#### Regular levels

When regular levels are enabled, members get levels based on the amount of points that they have earned 
in program (in **total**, not the current balance) and may need to have minimal level to purchase some rewards. 

Each level has `points_threshold` value (integer) that describes how many points are needed to reach the level (first level always requires 0 points to have).
Levels are returned in ascending order, sorted by the `points_threshold` attribute. 

#### Single virtual level

When single virtual level is enabled, it is just used to show member how he performs in the Rewards Program.

It defines `maximum_points` value, that is meant to be compared with member's **current** points balance.

#### Common error responses

Status | Response body
--------- | ----------- 
`480` | `{"error": "Member is not participating in Rewards Program"}` | -

##  Rewards Program

### <a name="rewards-program-info"></a> Get info

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/rewards-program/info" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
  "achievements": [
    {
      "type": "app_opened",
      "points": 50,
      "limit": 20,
      "frequency": {
        "limit": 10,
        "timespan": "day"
      }
    },
    {
      "type": "consent_granted",
      "points": 1000,
      "limit": null,
      "frequency": null
    }
  ],
  "levels_program": {
    "type": "regular",
    "levels": [
        {
          "name": "1",
          "points_threshold": 100
        },
        {
          "name": "2",
          "points_threshold": 300
        },
        {
          "name": "3",
          "points_threshold": 500
        }
    ]
  },
  "reward_activation_time": 35
}
```

**GET** `v3/infinity-mall/rewards-program/info`

Returns information about Rewards Program in Loyalty Club. 

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
achievements | Array<Achievement> | List of Achievement objects - see [Achievements](#rewards-program-achievements)
reward_activation_time | integer | Time in seconds describing how long the reward is active after use
levels_program | object |  Definition of member levels - see [Levels](#rewards-program-levels-program). When null, there are no levels.

<aside class="notice">
Requires <code>Rewards:Api:Program:GetInfo</code> permit
</aside>

### <a name="rewards-program-join"></a> Join

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/join" \
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

**POST** `v3/infinity-mall/members/me/rewards-program/join`

Registers current member in Rewards Program. This results granting "rewards_enrollment" consent to user.

As a member-related action, it requires member authorization. See [OAuth](#oauth2).

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Memberships:Create</code> permit
</aside>

### <a name="rewards-program-leave"></a> Leave

> Example:

```shell
curl -X DELETE \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/leave" \
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

**DELETE** `v3/infinity-mall/members/me/rewards-program/leave`

Removes current member from Rewards Program. This results in removing "rewards_enrollment" consent from member
and **deletion of all points and transaction from Rewards Program.**

As a member-related action, it requires member authorization. See [OAuth](#oauth2).

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Memberships:Delete</code> permit
</aside>

### <a name="rewards-program-status"></a> Get status

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/status" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
  "membership": true,
  "membership_started_at": "2019-05-15T12:43:22:41063Z",
  "balance": 180,
  "transactions": [
    {
      "type": "reward_purchase",
      "date": "2019-12-15T08:00:15063Z",
      "amount": -20,
      "expired_at": null,
      "details": { "reward_name": "Awesome Toaster" }
    },
    {
      "type": "expiration",
      "date": "2019-12-14T08:00:15063Z",
      "amount": -110,
      "expired_at": null,
      "details": {}
    },
    {
      "type": "achievement",
      "date": "2019-12-10T08:00:15063Z",
      "amount": 150,
      "expired_at": null,
      "details": { "achievement_type": "app_opened" }
    },
    {
      "type": "correction",
      "date": "2019-12-05T08:00:15063Z",
      "amount": 50,
      "expired_at": null,
      "details": { "comment":  "Just for you" }
    },
    {
      "type": "achievement",
      "date": "2019-06-41T08:00:15063Z",
      "amount": 110,
      "expired_at": "2019-12-15T08:00:15063Z",
      "details": { "achievement_type": "coupon_used" }
    }
  ],
  "pagination_info": {
    "total_count": 2050,
    "per_page": 1000,
    "total_pages": 3,
    "current_page": 1,
    "next_page": 2,
    "prev_page": null,
    "is_first_page": true,
    "is_last_page": false,
    "is_out_of_range": false
  }  
}
```

> When member hasn't joined the program, returns a response having only one key:

```json
{
  "membership": false
}
```

**GET** `v3/infinity-mall/members/me/rewards-program/status`

Returns Rewards Program status for current member.

As a member-related action, it requires member authorization. See [OAuth](#oauth2).

#### Query Parameters

Parameter | Type | Required? | Default | Description
--------- | ----------- | ----------- | --------- | -----------
per_page | integer | no | 1000 | Number of transactions to be returned per request (1000 is the maximum)
page | integer | no | 1 | Number of transactions page

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
membership | boolean | Does member participate the Rewards Program?
membership_started_at | Date | When the member joined the Rewards Program
balance | integer | Member's current points number
transactions | Array| List of Transactions objects, ordered by date descending. See below
pagination_info | Object | [Pagination](#pagination-model) object describing transactions list

#### Transaction object

Key | Type | Description
--------- | --------- | ---------
type | One of: 'achievement', 'reward_purchase', 'expiration' and 'correction' | see "Transaction types" below
date | Date | When the transaction has been made
amount | integer | How many points the transaction added or subtracted  
expired_at | Date | (optional) When the points granted by the transaction expired. When null, they're not expired
details | Object | Transaction-specific details - see "Transaction details" below

#### Transaction types

Type | Description |
----- | ----------- 
achievement | Addition of points triggered by fulfilling an achievement goal
reward_purchase | Reduction of points caused by purchasing a reward by member
expiration | Reduction of points caused by automatic expiration of old transactions
correction | Change of points caused by admin's manual correction

#### Transaction details

Depending on transaction type, the details contain different kind of data.

Transaction type | Key | Description
--- | --- | ---
achievement | achievement_type | Type of achievement that granted the points - see [Achievement types](#rewards-program-achievement-types)
reward_purchase | reward_name | Name of reward that the points have been spent on
correction | comment | (optional) Admin's notes

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Memberships:CheckStatus</code> permit
</aside>

### <a name="rewards-program-status-by-member-id"></a> Get status by member ID

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/837182/rewards-program/status" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> Returns response as in [Rewards Program &bull; Get Status]($rewards-program-status)

**GET** `v3/infinity-mall/members/:id/rewards-program/status`

Returns Rewards Program status for member identified by given ID.
Works just the same as [Rewards Program &bull; Get Status](#rewards-program-status).

<aside class="notice">
Requires <code>Rewards:Api:Memberships:CheckStatus</code> permit
</aside>

### <a name="rewards-program-achievements-summary"></a> Achievements summary

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/status/achievements_summary" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
    "achievements_by_type": [
        {
            "type": "app_opened",
            "times_achieved": 3,
            "total_amount_earned": 30
        },
        {
            "type": "beacon_approached",
            "times_achieved": 3,
            "total_amount_earned": 150
        },
        {
            "type": "link_clicked",
            "times_achieved": 4,
            "total_amount_earned": 80
        }
    ],
    "achievements_by_month": [
        {
            "year": 2019,
            "month": 2,
            "achievements_made": 20,
            "total_amount_earned": 800
        },
        {
            "year": 2019,
            "month": 1,
            "achievements_made": 35,
            "total_amount_earned": 950
        },
        {
            "year": 2018,
            "month": 12,
            "achievements_made": 12,
            "total_amount_earned": 580
        }
        // (...) another 9 months
    ]
}
```

**GET** `v3/infinity-mall/members/me/rewards-program/status/achievements_summary`

Returns a summary for achievements made by member, grouped by type and month.

As a member-related action, it requires member authorization. See [OAuth](#oauth2).

#### Query Parameters

Parameter | Type | Required? | Default | Description
--------- | ----------- | ----------- | --------- | -----------
months_no | integer | no | 12 | Number of last months to fetch. Must be in (0..999) range

#### Achievement by type JSON object

Key | Type | Description
--------- | --------- | ---------
type | string | See: [Achievement types](#rewards-program-achievement-types)
times_achieved | integer | Number of times member made the achievements of this type
total_amount_earned | integer | Sum of points member get with this achievement type 

#### Achievement by month JSON object

Key | Type | Description
--------- | --------- | ---------
year | integer | four-digit year, e.x. 2019 
month | integer | 1-12
achievements_made | integer |  Number of achievements made by member in that month
total_amount_earned | integer | Sum of points member achieved in that month

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Memberships:CheckStatus</code> permit
</aside>
