# Loyalty Clubs API

##  Loyalty Clubs

### <a name="loyalty-club-model"></a> Loyalty Club model

> Loyalty Club example:

```json
{
    "id": 235,
    "community_id": 3433,
    "customer_id": 447,
    "main_slug": "infinity-mall",
    "short_slug": "iml",
    "other_slugs": ["infinity"],
    "name": "Infinity Mall",
    "country": "NO",
    "timezone": "Europe/Oslo",
    "member_identifiers": [
        "msisdn"
    ],
    "new_offers_api_enabled": true
}
```

Key | Type | Description
--------- | ----------- | ---------
id | integer |
community_id | integer | ID of community assigned to the Loyalty Club
customer_id | integer | ID of customer that the Loyalty Club belongs to
main_slug | slug | Main LC's slug
short_slug | slug | Short LC's slug, used in Boostcom Shortener
other_slugs | array<string> | May be empty
name | string | 
country | string | [ISO 3166-1 alpha-2 code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements)
timezone | string | [TZ database name](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
member_identifiers | array<string> | May consist of following values: `msisdn` and `email`
new_offers_api_enabled | bool | Is [New Offers API](#endpoints-offers) enabled for this Loyalty Club?

### <a name="loyalty-clubs-list"></a> List

> Example

```shell
curl "https://bpc-api.boostcom.no/v3/loyalty_clubs" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, the above command returns JSON structured like this:

```json
{
  "loyalty_clubs": [
    {
      // (...) - see Loyalty Club model
    }
  ],
  "pagination_info": {
      // (...) - see Pagination info model
  }
}
```

**GET** `v3/loyalty_clubs/`
 
Returns [Loyalty Clubs](#loyalty-club-model) accessible by API client.

#### Query Parameters

Parameter | Type | Default | Description
--------- | ----------- | --------- | -----------
per_page | integer | 1000 | Number of results to be returned per request (1000 is the maximum)
page_no | integer | 1 | Number of results page
type | enum: ['REGULAR', 'TENANTS', 'ICOM'] | null | When present, filters results by this type
customer_id | integer | null | When present, filters results by this customer

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
Loyalty Club | Array<Loyalty Club> | Array of [Loyalty Clubs](#loyalty-club-model)
pagination_info | Object | [Pagination](#pagination-model) object

<aside class="notice">
Requires <code>BL:Api:LoyaltyClubs:ListMine</code> permit
</aside>

### <a name="loyalty-clubs-get"></a> Get

> Example

```shell
curl "https://bpc-api.boostcom.no/v3/loyalty_clubs/infinity-mall/" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, the above command returns JSON structured like this:

```json
{
  // (...) - see Loyalty Club model
}
```

**GET** `v3/loyalty_clubs/:loyalty_club_slug`

**GET** `v3/loyalty_clubs/by_community_id/:community_id`
 
Returns basic information about the [Loyalty Clubs](#loyalty-club-model), by one of it's slugs or by it's `community_id`.

<aside class="notice">
Requires <code>BL:Api:LoyaltyClubs:Get</code> permit
</aside>

## Schema

### <a name="loyalty-clubs-schema"></a> Get

> Example

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/member_schema" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
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

#### Response (JSON object)

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

## Products

### <a name="loyalty-clubs-products"></a> List

> Example

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/products" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
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

#### Product response model (in JSON array)

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

## Translations

### <a name="list-translations"></a> List

> Example request

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/translations?language=en" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns a response structured like this:

```json
{
    "member_properties": {
        "email": "E-mail",
        "gender": "Gender",
        "msisdn": "msisdn",
        "birthday": "Birthday",
        "zip_code": "Zip code",
        "interests": "Interests",
        "last_name": "Last name",
        "first_name": "First name",
        "gender.man": "Man",
        "gender.woman": "Woman",
        "bonus_points": "Bonus points",
        "interests.skincare": "Skincare",
        "interests.baby_child": "Baby/Child",
        "interests.good_offers": "Good offers",
        "interests.travel_and_leisure": "Travel and leisure",
        "interests.dietary_and_nutrition": "Dietary and nutrition",
        "interests.health_and_pharmaceuticals": "Health and pharmaceuticals"
    },
    "consents": {
        "dmp_profiling": "Would you like to receive personalised offers?",
        "dmp_profiling.title": "",
        "dmp_profiling.no_alt": "Are you sure? Ved å si nei kan mottar du ikke informasjon og tilbud i andre medier",
        "dmp_profiling.read_more": "Profiling is our way of personalising offers, info and discounts. Please see our Privacy Policy for more info on how we use data for profiling.",
        "dmp_profiling.__yes__": "Yes, I accept profiling to receive personalised exclusive offers, info and discounts",        
        "dmp_profiling.__no__": "No, I do not accept profiling to receive personalised exclusive offers, info and discounts",                
        "sms_marketing": "Do you want to receive exclusive offers via SMS?",
        "sms_marketing.title": "SMS Marketing",
        "sms_marketing.no_alt": "Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? ",        
        "sms_marketing.read_more": "",                
        "sms_marketing.__yes__": "Yes, I want to receive exclusive offers via SMS.",
        "sms_marketing.__no__": "No, I don't want to receive exclusive offers.",        
        "email_marketing": "Do you want to receive exclusive offers via email? ",
        "email_marketing.title": "Email Marketing",
        "email_marketing.no_alt": "Are you sure? Ved å si nei kan mottar du ikke informasjon og tilbud i andre medier",        
        "email_marketing.__yes__": "Yes",
        "email_marketing.__no__": "No, I don't want to receive exclusive offers.",        
        "email_marketing.read_more": ""
    },
    "tags": {
        "doggo": "Dog",
        "piggo": "Pig"
    },
    "terms": "<p><h3>Privacy Policy</h3>We strive to protect your privacy</p>"
}
```

**GET** `v3/:loyalty_club_slug/translations`

Returns translations for the Loyalty Club.

#### Query Parameters

Parameter | Type | Default | Type
--------- | ----------- | --------- | ------
language | String | (default language configured for Loyalty Club) | Desired language of translations 

#### Response (JSON object)

Key | Type | Optional? | Description 
--------- | --------- | ---------- | ---------
member_properties | Object | no (may be empty) | Translations of member properties 
consents | Object | no (may be empty) | Translations of member consents
tags | Object |  no (may be empty) | Translations of Loyalty Club tags
terms | string | yes | Translation of Loyalty Club terms 

<aside class="notice">
Requires <code>Translations:Api:Index</code> permit
</aside>
