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
  "consents": {
    "consent1": { "version": "", "required": true, "show_in_profile": true, "show_at_registration": true },
    "consent2": { "version": "", "required": true, "show_in_profile": true, "show_at_registration": true }
  },
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

## <a name="v3-loyalty-clubs-files-configuration"></a> Files configuration

> Example request

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/files/configuration" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> Example response

```json
{
  "kinds": [
    {
      "identifier": "collection_cover_default",
      "type": "IMAGE",
      "ratio": "2:1",
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": 300,
          "max_width": 600,
          "max_height": 300
        },
        {
          "identifier": "thumbnail",
          "min_width": 300,
          "min_height": 150,
          "max_width": 300,
          "max_height": 150
        },
        {
          "identifier": "original",
          "min_width": 600,
          "min_height": 300,
          "max_width": null,
          "max_height": null
        }
      ]
    },
    {
      "identifier": "offer_default",
      "type": "IMAGE",
      "ratio": "3:4",
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": 800,
          "max_width": 600,
          "max_height": 800
        },
        {
          "identifier": "thumbnail",
          "min_width": 225,
          "min_height": 300,
          "max_width": 225,
          "max_height": 300
        },
        {
          "identifier": "original",
          "min_width": 600,
          "min_height": 800,
          "max_width": null,
          "max_height": null
        }
      ]
    },
    {
      "identifier": "reward_default",
      "type": "IMAGE",
      "ratio": "3:4",
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": 800,
          "max_width": 600,
          "max_height": 800
        },
        {
          "identifier": "thumbnail",
          "min_width": 225,
          "min_height": 300,
          "max_width": 225,
          "max_height": 300
        },
        {
          "identifier": "original",
          "min_width": 600,
          "min_height": 800,
          "max_width": null,
          "max_height": null
        }
      ]
    },
    {
      // Hypothetical. Represents an image rescaled (up or down) to width: 600
      "identifier": "example_without_ratio",
      "type": "IMAGE",
      "ratio": null,
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": null,
          "max_width": 600,
          "max_height": null
        },
        {
          "identifier": "thumbnail",
          "min_width": 300,
          "min_height": null,
          "max_width": 300,
          "max_height": 300
        },
        {
          "identifier": "original",
          "min_width": null,
          "min_height": null,
          "max_width": null,
          "max_height": null
        }
      ]
    }
  ]
}
```

**GET** `v3/:loyalty_club_slug/files/configuration` (draft)

Returns a list of file kinds configured for the Loyalty Club.

### <a name="v3-file-standard-kinds"></a> Standard file kinds

There are some standard file kinds, their size definitions differ among Loyalty Clubs.

Identifier | Description
---  | -----------
`collection_cover_default` | Cover for Offer collections
`offer_default` | Default Offer image
`reward_default` | Default Reward image

### <a name="v3-file-kind-ratio"></a> Ratio

Currently, all images have fixed proportions ratio which is represented as a string: `"<width>:<height>"`

In future, there may be file kinds without fixed ratio defined. They may look like `"example_without_ratio"` on the right.

### <a name="v3-file-sizes"></a> File sizes

Every file has at least three file sizes available: 

#### `base`

Actual file size. Currently all file kinds have this size with fixed proportions (so `min` == `max`) that match the ratio.

#### `thumbnail`

Rescaled version of `base` size. Currently, rescaling always works this way:

* pick the higher dimension of file
* shrink the image so:
    * the higher dimension gets shrunk to `300px`
    * the lower dimensions gets shrunk so the image keeps the original ratio

Examples:

* File with resolution `600x800` gets shrunk down to `225x300`
* File with resolution `800x600` gets shrunk down to `300x225`
* File with resolution `400x300` gets shrunk down to `300x225`
* File with resolution `400x200` gets shrunk down to `300x150`
* File with resolution `300x250` is not shrunk down

#### `original`

Original file that that has been uploaded and was used for generating other file sizes.

Currently, we require all original files to have proper ratios and minimum sizes of at least the `base` size.
Because of that, you can expect them to have `min_height` and `min_width` that match the `base` size, 
but they never have `max_width` or `max_height` defined.   

### <a name="v3-file-kind-model"></a> File kind model

Key | Type | Description
--------- | ----------- | ---------
identifier | string | 
type | string | Currently, only `IMAGE` (mime: 'image/gif', 'image/jpeg', 'image/png') is supported
ratio | string | Describes ratio of an image. See: [Ratio](#v3-file-kind-ratio)
sizes| Array<Object> | See: [File sizes](#v3-file-sizes) and below

### <a name="v3-file-kind-size-model"></a> File kind size model

Key | Type | Optional
--------- | ----------- | -----
identifier | string | no
min_width | integer | yes
min_height | integer | yes
max_width | integer | yes
max_height | integer | yes

<aside class="notice">
Requires <code>Files:Api:GetConfiguration</code> permit
</aside>
