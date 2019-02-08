# Endpoints &bull; Rewards Program (draft)

## Introduction

Boostcom Rewards is a Loyalty Program that awards members for achieving some defined goals with points.

The points may be spent on acquiring (purchasing) special rewards and then using them.
Points may expire after some time.

Participation in program is optional, so member can enroll (join) to it and leave it.

### Achievements

Every Loyalty Club that has Boostcom Rewards enabled, has defined list of achievements that have rules attached to them.
The rules are described below.

### <a name="v3-rewards-program-achievements"></a> Achievements

Key | Type | Description
--------- | --------- | ---------
type | string | see [Achievement types](#v3-rewards-program-achievement-types)
points | integer | How many points the achievement grants
maximum_frequency | @todo | How often the achievement may be granted to member
limit | integer | Maximum number of times the member can have the achievement granted  

### <a name="v3-rewards-program-achievement-types"></a> Achievement types

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

## <a name="v3-rewards-program-info"></a> Get info

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/rewards-program/info" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
  "achievements": [
    {
      "type": "app_opened",
      "point": 50,
      "limit": 20,
      "maximum_frequency": "@todo"
    },
    {
      "type": "consent_granted",
      "point": 1000,
      "limit": 1,
      "maximum_frequency": "@todo"
    }
  ],
  "reward_activation_time": 35
}
```

**GET** `v3/infinity-mall/rewards-program/info`

Returns information about Rewards Program in Loyalty Club. 

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
achievements | Array| List of Achievement objects - see [Achievements](#v3-rewards-program-achievements)
reward_activation_time | integer| Time in seconds describing how long the reward is active after use

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Program:GetInfo</code> permit
</aside>

## <a name="v3-rewards-program-join"></a> Join

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/join" \
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

**POST** `v3/infinity-mall/members/me/rewards-program/join`

Registers current member in Rewards Program. This results granting "rewards_enrollment" consent to user.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Memberships:Create</code> permit
</aside>

## <a name="v3-rewards-program-leave"></a> Leave

> Example:

```shell
curl -X DELETE \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/leave" \
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

**DELETE** `v3/infinity-mall/members/me/rewards-program/leave`

Removes current member from Rewards Program. This results in removing "rewards_enrollment" consent from member
and **deletion of all points and transaction from Rewards Program.**

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Memberships:Delete</code> permit
</aside>

## <a name="v3-rewards-program-status"></a> Get status

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/members/me/rewards-program/status" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an object structured like this:

```json
{
  "membership": true,
  "balance": 180,
  "transactions": [
    {
      "type": "expiration",
      "date": "2019-12-14T08:00:15063Z",
      "amount": -110,
      "expired_at": null,
      "details": {}
    },
    {
      "type": "usage",
      "date": "2019-12-14T08:00:15063Z",
      "amount": -20,
      "expired_at": null,
      "details": { "reward_name": "Awesome Toaster" }
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
  ]
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

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
membership | boolean | Does member participate the Rewards Program?
balance | integer | Member's current points number
transactions | Array| List of Transactions objects, ordered by date descending. See below

### Transaction object

Key | Type | Description
--------- | --------- | ---------
type | One of: 'achievement', 'usage', 'expiration' and 'correction' | see "Transaction types" below
date | Date | When the transaction has been made
balance | integer | How many points the transaction added or subtracted  
expired_at | Date | (optional) When the points granted by the transaction expired. When null, they're not expired
details | Object | Transaction-specific details - see "Transaction details" below

### Transaction types

Type | Description |
----- | ----------- 
achievement | Addition of points triggered by fulfilling an achievement goal
usage | Reduction of points caused by purchasing a reward by member
expiration | Reduction of points caused by automatic expiration of old transactions
correction | Change of points caused by admin's manual correction

### Transaction details

Depending on transaction type, the details contain different kind of data.

Transaction type | Key | Description
--- | --- | ---
achievement | achievement_type | Type of achievement that granted the points - see [Achievement types](#v3-rewards-program-achievement-types)
usage | reward_name | Name of reward that the points have been spent on
correction | comment | (optional) Admin's notes

<aside class="notice">
Requires <code>Rewards:Api:OAuth:Memberships:CheckStatus</code> permit
</aside>
