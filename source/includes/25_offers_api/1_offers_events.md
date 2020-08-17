## <a name="v3-offers-events"></a> Offers Events

### <a name="v3-offer-event-model"></a> OfferEvent model

> Offer Event example:

```json
{
    "offer_id": "14",
    "created_at": "2019-08-27T10:20:11.810Z",
    "context": {
        "collections_ids": [1, 2],
        "tags": ["t1"],
        "stores": ["a"],
        "search_string": "foo",
        "utm": "campaign-2018-05-01"
    }
}
```

Key | Type | Required? | Description
--------- | --------- | -------- | ---------
offer_id | integer | yes |  
created_at | Date| yes | Date of the event occurrence 
context | Object| no | Context of the event occurrence.   
context.collections_ids | Array<ID> | no |  
context.tags | Array<string> | no |  
context.stores | Array<string> | no |
context.search_string | string | no |
context.utm | string | no |

Context describes in what circumstances the event has occurred. 
For example, member clicked on a coupon that was displayed inside collection, the context should include this collection_id. 

### <a name="v3-offers-clicked"></a> Register "clicked" event

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/offers/events/offers_clicked" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -d '
    {
        "events": [
            {
                "offer_id": "14",
                "created_at": "2019-08-27T10:20:11.810Z",
                "context": {
                    "collections_ids": [1, 2],
                    "tags": ["t1"],
                    "stores": ["a"],
                    "search_string": "foo",
                    "utm": "campaign-2018-05-01"
                }
            }
        ]
    
    }
    '
```

> When successful (200), returns an empty object.

```json
{}
```

**POST** `v3/:loyalty_club_slug/members/me/events/offers_clicked` [[OAuth](#v3-oauth2)]

Registers that OAuth member clicked on given offers.

#### Post Parameters

Parameter | Type | Description
--------- | --------- | ------
events | Array<OfferEvent> | See [Offer Event](#v3-offer-event-model)

<aside class="notice">
Requires <code>Offers:Api:Events:Clicked</code> permit
</aside>

### <a name="v3-offers-seen"></a> Register "seen" event

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/offers/events/offers_seen" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -d '
    {
        "events": [
            {
                "offer_id": "14",
                "created_at": "2019-08-27T10:20:11.810Z",
                "context": {
                    "collections_ids": [1, 2],
                    "tags": ["t1"],
                    "stores": ["a"],
                    "search_string": "foo",
                    "utm": "campaign-2018-05-01"
                }
            }
        ]
    
    }
    '
```

> When successful (200), returns an empty object.

```json
{}
```

**POST** `v3/:loyalty_club_slug/members/me/events/offers_seen` [[OAuth](#v3-oauth2)]

Registers that OAuth member saw given offers.

<aside class="warning">
Seen events are automatically tracked when retrieving offer image/file within member context. Make sure you are not duplicating events!
</aside>

#### Post Parameters

Parameter | Type | Description
--------- | --------- | ------
events | Array <OfferEvent> | See [Offer Event](#v3-offer-event-model)

<aside class="notice">
Requires <code>Offers:Api:Events:Seen</code> permit
</aside>
