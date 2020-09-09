# Transactions API

##  Transactions

### <a name="v3-transactions-create"></a> Create

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
         "transactions": [
             {
                 "identifier_type": "id",
                 "identifier": 123456,
                 "store_id": "store_id",
                 "event_date": "2019-02-02T18:48:47+08:00",
                 "total": 10.0,
                 "receipt_id": "receipt id",
                 "tax": 1.0,
                 "currency": "NOK",
                 "items": [
                     {
                         "id": "1",
                         "discount": 0.0,
                         "tax": 1,
                         "amount": 1,
                         "price": 5.0,
                         "main_product_category": "category name",
                         "main_product_category_id": "category id",
                         "main_product_category_number": "1",
                         "product_category": "category name",
                         "product_category_id": "category id",
                         "product_category_number": "1",
                         "description": "item description"
                     },
                     {
                         "id": "2",
                         "discount": 0.0,
                         "tax": 1.0,
                         "amount": 1,
                         "price": 5,
                         "main_product_category": "category name",
                         "main_product_category_id": "category id"
                     }
                 ],
                 "loyalty_points": 100
             }
         ]
     }'
```

> When successful (200), returns a feedback object structured like this:

```json
[
  {
      "receipt_id": "id",
      "transaction_id": "43cb7852-5190-4d1d-9b1f-6d6cf552424b",
      "items": ["91cd14a0-b9b5-4e21-950c-a10b16934a91", "1ad96fb7-fb77-4981-b2a6-89549a6f5ed6"],
      "result": "ok"
  },
  {
      "receipt_id": "id",
      "transaction_id": "f4c2b7a6-e0fb-47a3-9b5a-02d3854fcea3",
      "items": ["5a8fd797-ba18-4c9c-9d70-c7acdd7805b6", "bc20e6a6-4792-4c94-8c30-4198bf8247c5"],
      "result": "Member doesn't exist"
  }
]
``` 

> When payload is invalid (422), returns an object structured like this:

```json
{
    "error": "Invalid format",
    "details": {
        "transactions[1].store_id": "This value should not be blank."
    }
}
``` 

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions?transaction_id=43cb7852-5190-4d1d-9b1f-6d6cf552424b" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an array of transactions objects structured like this:

```json
[
  {
      "transaction_id": "43cb7852-5190-4d1d-9b1f-6d6cf552424b",
      "member_id": 123456,
      "store_id": "store id",
      "receipt_id": "receipt id",
      "event_date": "2019-02-02T02:02:02+0000",
      "total": 10.0,
      "tax": 1.0,
      "currency": "NOK",
      "sales_person": "sales person"
  }
]
``` 

> When payload is invalid (422), returns an object structured like this:

```json
{
    "error": "Invalid format",
    "details": {
        "transactions[1].store_id": "This value should not be blank."
    }
}
``` 
**POST** `v3/:loyalty_club_slug/transactions`

Register transactions, stamps or bonus points will be calculated based on the configured loyalty program.

#### Messages POST Parameters (JSON array)

Expected payload is either object with `transactions` attribute containing array of `transaction` objects with attributes as described below or single `transaction` object:

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
identifier_type * | string (id, msisdn, email) | n/a |
identifier * | msisdn (string), email (string), member_id (int) | n/a |
store_id * | string | n/a |
receipt_id | string | n/a |
event_date * | string | use format with timezone |
total * | float | n/a |
tax * | float | n/a |
items * | array | n/a | array of `item` objects
calculate_rewards | bool | true | 
currency | string | Loyalty Club's currency | 
sales_person | string | n/a |
terminal_id | string | n/a |
store_tax_id | string | n/a |
store_msisdn | string | n/a |
loyalty_coupons | array of strings | n/a | list of coupons ids / codes
loyalty_giftcards | array of strings | n/a | list of codes
loyalty_points | integer | n/a
\* required

`item` object is a hash with following attributes:

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
id * | string | n/a | item id
price * | float | n/a | negative values will be treated as discounts |
amount * | integer | n/a | negative values will be treated as refunds |
discount * | float | n/a |
tax * | float | n/a |
calculate_rewards * | bool | true |
item_number | string | n/a |
description | string | n/a |
properties | hash key: value | n/a |
product_category | string | n/a |
product_category_id | string | n/a |
product_category_number | string | n/a |
main_product_category | string | n/a |
main_product_category_id | string | n/a |
main_product_category_number | string | n/a |
\* required

Negative price and amount will result in error.

You can provide `callback_url` in the root object. If it's available after processing we will send `POST` request to that URL with body as below:
```
{
    "transaction_id": "UUID",
    "receipt_id": "id",
    "items": ["UUID", "UUID"],
    "result" => "OK|error message"
}
```

#### Feedback statuses

Status | Description
---- | ----
ok | Message has been accepted for sending
Member doesn't exist | 

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Transactions:Create</code> and <code>BL:Api:Members:Get</code> permits
</aside>


### <a name="v3-transactions-list"></a> List

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions?transaction_id=43cb7852-5190-4d1d-9b1f-6d6cf552424b" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an array of transactions objects structured like below. Some values may be omitted if were not provided:

```json
[
  {
      "transaction_id": "43cb7852-5190-4d1d-9b1f-6d6cf552424b",
      "member_id": 123456,
      "store_id": "store id",
      "receipt_id": "receipt id",
      "event_date": "2019-02-02T02:02:02+0000",
      "total": 10.0,
      "tax": 1.0,
      "currency": "NOK",
      "sales_person": "sales person",
      "terminal_id": "terminal id",
      "store_tax_id": "store tax id",
      "discount": 0.0,
      "store_msisdn": "msisdn"
  }
]
``` 

> When parameters are invalid (422), returns an object structured like this:

```json
{
    "error": "Invalid format",
    "details": {
        "query": "At least one value required: {{ transactionId, storeId, receiptId, memberId, salesPerson, currency }}"
    }
}
``` 
**GET** `v3/:loyalty_club_slug/transactions`

List registered transactions

#### Request Parameters

Key | Type | Description
--------- | --------- | ---------
transaction_id * | string
store_id * | array
receipt_id * | string
member_id * | int
sales_person * | string 
currency * | string
ignore_loyalty | bool
min_total_gross | float
max_total_gross | float
sort | string | field to sort by
order | string (ASC, DESC)
\* required at least one of starred parameters 

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Transactions:Read</code> permit
</aside> 

### <a name="v3-transactions-get"></a> Get

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions/43cb7852-5190-4d1d-9b1f-6d6cf552424b" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns transactions object structured like below. Some values may be omitted if were not provided:

```json
{
  "transaction_id": "43cb7852-5190-4d1d-9b1f-6d6cf552424b",
  "member_id": 123456,
  "store_id": "store id",
  "receipt_id": "receipt id",
  "event_date": "2019-02-02T02:02:02+0000",
  "total": 10.0,
  "tax": 1.0,
  "currency": "NOK",
  "sales_person": "sales person",
  "terminal_id": "terminal id",
  "store_tax_id": "store tax id",
  "discount": 0.0,
  "store_msisdn": "msisdn",
  "items": [
    {
        "id": "1",
        "price": 10.0,
        "amount": 1,
        "discount": 0,
        "tax": 1.0,
        "description": "description",
        "product_category_id": "category id",
        "product_category_number": "12345",
        "product_category": "Category"
    }
  ]
}
``` 

> When `transaction_id` is invalid (422), returns:

```json
{
    "error": "Transaction not found"
}
``` 

**GET** `v3/:loyalty_club_slug/transactions/:id`

Get transaction with items

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Transactions:Read</code> permit
</aside> 

## Categories

### <a name="v3-transactions-categories-get"></a> List

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions/categories" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns categories objects structured like below:

```json
[
    {
        "id": "2276bb58-45a4-4d49-b25b-896a78b06730",
        "type": "main",
        "name": "category",
        "parent_id": null,
        "external_id": "ID",
        "number": "1"
    },
    {
        "id": "13b2a321-280e-48fc-bf18-54b61dc6d577",
        "type": "sub",
        "name": "subcategory",
        "parent_id": "2276bb58-45a4-4d49-b25b-896a78b06730",
        "external_id": "ID2",
        "number": "12"
    },
    {
        "id": "19c3f19c-f090-43f1-8439-824406449a2e",
        "type": "item",
        "name": "item",
        "parent_id": "13b2a321-280e-48fc-bf18-54b61dc6d577",
        "external_id": "2",
        "number": null
    }
]
``` 

> When parameters are invalid (422), returns:

```json
{
    "error": "Invalid format",
    "details": {
        "type": "The value you selected is not a valid choice.",
        "id": "This value should be of type array."
    }
}
``` 

**GET** `v3/:loyalty_club_slug/transactions/categories`

Get categories

#### Request Parameters

Key | Type | Default
--------- | --------- | ---------
type | string (main, sub, item) | 
parent_id | string | 
name | string | 
limit | int | 100
id | array | 

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Transactions:Read</code> permit
</aside> 

## Events

### <a name="v3-transactions-events-create"></a> Create events 

> Example:

> e-Vouchers

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions/events" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' 
    -d \
    '{
        { 
          "type": "use_evoucher",
          "bulk":
        	[
                {
                    "identifier": 1,
                    "identifier_type": "id",
                    "event_date": "2019-09-11T12:00:00+01:00",
                    "voucher_id": 1,
                    "store_id": "8",
                    "voucher_name": "name",
                    "serial_number": "serial_number"
                },
                {
                    "identifier": 2,
                    "identifier_type": "id",
                    "event_date": "2019-09-11T12:00:00+01:00",
                    "voucher_id": 2,
                    "store_id": "8",
                    "voucher_name": "name",
                    "serial_number": "serial_number"
                },
                {
                    "identifier": 3,
                    "identifier_type": "id",
                    "event_date": "2019-09-11T12:00:00+01:00",
                    "voucher_id": 3,
                    "store_id": "8",
                    "voucher_name": "name",
                    "serial_number": "serial_number"
                }
        	]
        }
    }'
```

> GWP

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions/categories" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' 
    -d \
    '{
        { 
          "type": "redeem_gwp",
          "bulk":
        	[
                {
                    "identifier": 44837522,
                    "identifier_type": "id",
                    "event_date": "2019-09-11T12:00:00+01:00",
                    "campaign_id": 1,
                    "item_id": "8",
                    "campaign_name": "campaign_name",
                    "item_name": "item_name",
                    "serial_number": "serial_number",
                    "detail_id": "detail_id",
                    "quantity": 1
                },
                {
                    "identifier": 44837522,
                    "identifier_type": "id",
                    "event_date": "2019-09-11T12:00:00+01:00",
                    "campaign_id": 1,
                    "item_id": "8",
                    "campaign_name": "campaign_name",
                    "item_name": "item_name",
                    "serial_number": "serial_number",
                    "detail_id": "detail_id",
                    "quantity": 1
                },
                {
                    "identifier": 44837522,
                    "identifier_type": "id",
                    "event_date": "2019-09-11T12:00:00+01:00",
                    "campaign_id": 1,
                    "item_id": "8",
                    "campaign_name": "campaign_name",
                    "item_name": "item_name",
                    "serial_number": "serial_number",
                    "detail_id": "detail_id",
                    "quantity": 1,
                    "additional_properties": 
                    {
                        "property_name": "property_value"
                    }                    
                }
        	]
        }
    }'
```

> When successful (200), result is ok for every event:

```json
[
    {
        "id": 1,
        "event_id": "4cca1661-7647-4a48-9664-a68b2075451d",
        "result": "ok"
    },
    {
        "id": 2,
        "event_id": "c3506406-b89e-4aef-8b33-c19aceb8ad6c",
        "result": "ok"
    },
    {
        "id": 3,
        "event_id": "eed0173c-a51d-4afb-a819-0320fabf77e9",
        "result": "ok"
    }
]
``` 

> When payload is invalid (422), returns an object structured like this:

```json
{
    "error": "Invalid format",
    "details": {
        "[0].store_id": "The property store_id is required",
        "[0].voucher_id": "The property voucher_id is required",
        "[1].voucher_name": "The property voucher_name is required",
        "[1].serial_number": "The property serial_number is required"
    }
}
``` 


**POST** `v3/:loyalty_club_slug/transactions/events`

Send different bonus events events

#### Request Parameters

Key | Type | Description
--------- | --------- | ---------
type | string | type of events (redeem_gwp, use_evoucher)
bulk | array | array of event objects

Bulk contains many events of specified type.

e-Vouchers event

Key | Type | Description
--------- | --------- | ---------
identifier | string | Member identifier
identifier_type | string | Member identifier type (id, msisdn, email)
event_date | string | datetime formatted according to ISO 8601, **use timezone**
voucher_id | integer | 
store_id | string | 
voucher_name | string | 
serial_number | string | 


GWP event

Key | Type | Description
--------- | --------- | ---------
identifier | string,integer | Member identifier
identifier_type | string | Member identifier type (id, msisdn, email)
event_date | string | datetime formatted according to ISO 8601, **use timezone**
campaign_id | integer | 
campaign_name | string | 
item_id | integer | 
item_name | string | 
serial_number | string | 
detail_id | string |
quantity | integer |
additional_properties | object |


#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Events:Create</code> permit
</aside> 