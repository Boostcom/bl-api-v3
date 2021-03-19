##  Stamps

### <a name="stamps-create"></a> Create

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/transactions/stamps" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '[
        {
            "identifier_type": "id",
            "identifier": 3456,
            "amount": 1,
            "event_type": "purchase",
            "event_date": "2019-07-12T15:23:21Z",
            "message": "Black Friday",
            "store_id": "12345",
            "transaction_id": "f4c2b7a6-e0fb-47a3-9b5a-02d3854fcea3",
            "expiry_date": "2020-02-12"
        }
    ]'
```

> When successful (200), returns a feedback object structured like this:

```json
[
    {
        "event_type": "issue",
        "amount": 1,
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

**POST** `v3/:loyalty_club_slug/transactions/stamps`

Add or remove stamps

#### POST Parameters (JSON)

Parameter        | Description            | Type
---------------- | ---------------------- | ------
identifier_type* | Type of identifier     | string (id, msisdn, email)
identifier*      | Identifier             | msisdn (string), email (string), member_id (int)
event_type*      | Type of event          | string (issue, campaign, use, revoke, expire)
amount*          | Stamps amount             | int (not 0)
event            | description of event   | string
message          | additional remarks     | string
store_id         | store id               | string
transaction_id   | DMP transaction id     | string
event_date       | date of event allow adding past events   | datetime
expiry_date | Points expiry date  (used only by integration without loyalty 

Parameters with `*` are required


#### Type of events
Type | Description
---- | -----------
campaign | stamps added due to campaign event (positive amount required)
issue | stamps added (positive amount required)
use | stamps used by a member (reward, discount, etc) (negative amount required)
revoke | stamps removed from member account due to refund etc. (negative amount required)
expire | stamps removed due to expiration (negative amount required)


#### Feedback statuses

Status | Description
---- | ----
ok   | Stamps saved
Member does not exist |
Store does not exists |
Incorrect amount | Positive amount when using stamps, negative amount when issuing stamps etc

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Stamps:Create</code>
</aside>
