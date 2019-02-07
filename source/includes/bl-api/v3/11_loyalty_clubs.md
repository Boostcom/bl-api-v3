# Endpoints &bull; Loyalty Clubs

## <a name="v3-loyalty-clubs-schema"></a> Get schema

> Example

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/member_schema" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> When successful, the above command returns JSON structured like this:

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "identifiers": ["email"],
  "consents": { "consent1": { "version": "" }, "consent2": { "version": "" } },
  "languages": ["en", "no"],
  "default_language": "no",
  "version": "v2",
  "properties": {
    "email": {
      "type": "string",
      "format": "email"
    },
    "first_name": {
      "type": "string"
    },
    "last_name": {
      "type": "string"
    },
    "birthday": {
      "type": "string",
      "format": "date"
    },
    "interests": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": [
          "bikes_and_cars",
          "sportwear"
        ]
      }
    }
  },
  "required": [
    "first_name",
    "last_name",
    "birthday"
  ]
}
```

**GET** `v3/:loyalty_club_slug/member_schema`

Properties for members are defined as part of the loyalty club they belong to.

To describe member properties we use [JSON Schema](http://json-schema.org/documentation.html) definition.
Properties of each member must conform to the defined schema.

We support **JSON schema Draft V4** with format extension for `date` (YYYY-MM-DD).

### Response (JSON object)

Key | Description | Type
--------- | ----------- | ---------
identifiers | Fields that are set to identify member | Array<string>
consents | Definition of available consents. Versions are now `''`. | JSON Object
languages | Languages set up in loyalty club | Array<string>
default_language | Default language used in loyalty club and also used for mappings to Api v2 | string
version | Version of schema, currently the newest is `v2` | string
products | Properties scoping and ordering by product name, default one is `default` | object
required | Required properties for member | integer (timestamp)
properties | Properties describing member | object

<aside class="notice">
Requires <code>BL:Api:Schema:Get</code> permit
</aside>

## <a name="v3-loyalty-clubs-products"></a> List products

> Example

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/products" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> When successful, the above command returns JSON structured like this:

```json
[
    {
        "name": "Webforms",
        "slug": "webforms",
        "filtered_member_properties": ["msisdn", "email", "first_name", "last_name"],
        "subproducts": [],
        "is_webform": true
    },
    {
        "name": "Default",
        "slug": "default",
        "filtered_member_properties": [],
        "subproducts": [],
        "is_webform": false
    }
]
```

**GET** `v3/:loyalty_club_slug/products`

Returns list of Products (AKA Channels, AKA Sources) defined for Loyalty Club.

### Product response model (in JSON array)

Key | Description | Type
--------- | ----------- | ---------
name | Product name | string
slug | Product slug | string
filtered_member_properties | Member that are available to access (CRUD) for this product. When empty, all properties are available. | Array<string>
subproducts | Subproducts/Subchannels available for the product | Array<string>
is_webform | Is the product also a webform? | boolean

<aside class="notice">
Requires <code>BL:Api:Products:Index</code> permit
</aside>
