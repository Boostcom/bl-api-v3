# Endpoints &bull; Points

## <a name="v3-points-create"></a> Create

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions/points" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
    -d \
    '[
        {
            "amount": 10,
            "event_type": "purchase",
            "event": "used coupon",
            "event_date": "2019-07-12T15:23:21Z",
            "message": "Black Friday",
            "store_id": "12345",
            "transaction_id": "f4c2b7a6-e0fb-47a3-9b5a-02d3854fcea3",
            "wallet": "default",
            "expiry_date": "2020-02-12T15:19:21Z"
        }
    ]'
```

> When successful (200), returns a feedback object structured like this:

```json
[
    {
        "event_type": "purchase",
        "amount": 100,
        "status": "ok"
    }
]
```  

> When request is invalid (422), returns:

```json
{
    "error": "Invalid format",
    "details": {
        "[1].property": "The property {property} is required"
    }
}
``` 

**POST** `v3/:loyalty_club_slug/transactions/points`

Add or remove points from member wallets. All unasigned points belongs to default wallet

### POST Parameters (JSON)

Parameter        | Description            | Type
---------------- | ---------------------- | ------
identifier_type* | Type of identifier     | string (id, msisdn, email)
identifier*      | Identifier             | msisdn (string), email (string), member_id (int)
event_type*      | Type of event          | string (purchase, issue, campaign, use, revoke, expire, other)
amount*          | store name             | int (not 0)
event            | description of event   | string
message          | additional remarks     | string
store_id         | store id               | string
transaction_id   | DMP transaction id     | string
wallet           | wallet                 | string 
event_date       | date of event allow adding past events   | datetime 
expiry_date      | Points expiry date     | datetime

Parameters with `*` are required


### Type of events
Type | Description
---- | -----------
purchase | points added by making a purchase (positive amount required)
campaign | points added due to campaign event (positive amount required)
issue | points added (positive amount required)
use | points used by a member (reward, discount, etc) (negative amount required)
revoke | points removed from member account due to refund etc. (negative amount required)
expire | points removed due to expiration (negative amount required)
other  | 

### Feedback statuses

Status | Description
---- | ----
ok   | Points saved
Member does not exist | 
Store does not exists | 
Incorrect amount | Positive amount when using points, negative amount when issuing points etc
Not enough points in the wallet | Not enough points when using a points

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transactions:Api:Points:Create</code>
</aside>

## <a name="v3-member-points"></a> Member points

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions/points/by_member_id/12345" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns a feedback object structured like this:

```json
    {
        "default_wallet": 100,
        "other_wallet": 10
    }
```  

**GET** `v3/:loyalty_club_slug/transactions/points/by_member_id/:member_id`

**GET** `v3/:loyalty_club_slug/transactions/points/by_msisdn/:msisdn`

**GET** `v3/:loyalty_club_slug/transactions/points/by_email/:email`

Return number of points that member has filtered by different wallets

### URL Parameters

Parameter | Description   | Type
--------- | ------------- | ------
member_id | Member id     | integer
msisdn    | Member msisdn | string
email     | Member email  | string

### Error responses

Status    | Description
--------- | ----------- 
`422`     | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transactions:Api:Points:Get</code> permit
</aside>