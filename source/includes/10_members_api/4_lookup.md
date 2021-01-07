## <a name="members-lookup"></a> Members Lookup

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/members/by_msisdn/:msisdn/lookup" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When found, returns looked-up properties of given person

```json
{
  "properties": {
    "msisdn": "4740769126",
    "first_name": "Johannes",
    "last_name": "Gutenberg",
    "gender": "man",
    "birthday": "1400-10-06",
    "address": "Høytunet 63",
    "city": "Mainz",
    "zip_code": "5582",
    "country": "Germany"
  }
}
```

> When not found, returns an empty object:

```json
{}
```

**GET** `v3/:loyalty_club_slug/members/by_msisdn/:msisdn/person_id`

#### URL Parameters

Parameter |             Type                | Description
--------- | ------------------------------- | -----------
msisdn    | string ([msisdn](#msisdn-param) |

#### Error responses

Status  | Description
------- | ----------- 
`402`   | Not enough funds on customer account. Returned only with `BL:Api:Members:Lookup:`.
`404`   | Data not found
`422`   | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>BL:Api:Members:Lookup</code> permit
</aside>
