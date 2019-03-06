# Endpoints &bull; Transactions

## <a name="v3-transactions-create"></a> Create

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
    -d \
    '{
         "transactions": [
             {
                 "identifier_type": "id",
                 "identifier": 123456,
                 "store_id": "store_id",
                 "event_date": "2019-02-02 02:02:02",
                 "total": 10.0,
                 "receipt_id": "receipt id",
                 "discount": 0.0,
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
      "result": "ok"
  },
  {
      "receipt_id": "id",
      "transaction_id": "f4c2b7a6-e0fb-47a3-9b5a-02d3854fcea3",
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
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
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
  },
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

### Messages POST Parameters (JSON array)

Expected payload is either object with `transactions` attribute containing array of `transaction` objects with attributes as described below or single `transaction` object:

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
identifier_type * | string (id, msisdn, email) | n/a |
identifier * | msisdn (string), email (string), member_id (int) | n/a |
store_id * | string | n/a |
receipt_id | string | n/a |
event_date * | string | n/a |
total * | float | n/a |
tax * | float | n/a |
items * | array | n/a | array of `item` objects
calculate_rewards | bool | true | 
currency | string | Loyalty Club's currency | 
sales_person | string | n/a |
terminal_id | string | n/a |
store_tax_id | string | n/a |
discount | float | n/a |
store_msisdn | string | n/a |
loyalty_coupons | array of strings | n/a | list of coupons ids / codes
loyalty_giftcards | array of strings | n/a | list of codes
loyalty_points | integer | n/a
\* required

`item` object is a hash with following attributes:

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
item_id * | string | n/a | 
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

Negative price and amount will result in error

### Feedback statuses

Status | Description
---- | ----
ok | Message has been accepted for sending
Member doesn't exist | 

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Transactions:Create</code> permit
</aside>


## <a name="v3-transactions-list"></a> List

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions?transaction_id=43cb7852-5190-4d1d-9b1f-6d6cf552424b" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
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

### Request Parameters

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

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Transactions:Read</code> permit
</aside> 

## <a name="v3-transactions-get"></a> Get

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/transactions/43cb7852-5190-4d1d-9b1f-6d6cf552424b" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
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
        "productCategoryId": "category id",
        "productCategoryNumber": "12345",
        "productCategory": "Category"
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

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>Transaction:Api:Transactions:Read</code> permit
</aside> 