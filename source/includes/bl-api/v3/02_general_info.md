
# General info

## Hosts

Production: [https://bpc-api.boostcom.no](https://bpc-api.boostcom.no)

Staging: [https://bpc-api.dev.boostcom.no](https://bpc-api.dev.boostcom.no)

## Common HTTP error codes

Status | Reason
-------|-----|-------
`400` | Some of required header is missing
`401` | `X-Client-Authorization` is invalid (or doesn't match provided loyalty club or product)
`403` | Not authorized to perform this action (provided `X-Authorization-Token` doesn't have required permit)
`404` | The requested resource doesn't exist
`422` | Invalid parameters are provided (e.g. incorrect properties on member creation)
`460` | OAuth token required for the action is invalid (applies only to OAuth-related actions - see [OAuth2](#v3-oauth2))

Also, most of handled errors have JSON response body like this:

`{"error": "Message describing what went wrong"}`

## Validation on members

All members' endpoints have properties validation. Properties are validated to conform loyalty club's schema.

If something is wrong, you will receive explanation of errors in response.

<aside class="warning">
Errors format and strings are not yet stable and may change without further notice.
</aside>

### Schema validation errors

```shell
curl -X PUT -H "X-Product-Name: custom-product-name" \
    -H "X-Customer-Private-Token: token" \
    -H "Content-Type: application/json" -d '{
	"properties": {
		"first_name": "",
		"last_name": "err",
		"gender": "man",
		"birthday": "201X-01-01"
	}
}' "https://tbp.bstcm.no/api/v2/loyalty_clubs/:loyalty_club_slug/members/:msisdn"
```

> Example response (with code 400):

```json
{
  "email": [
    {
      "error": "invalid",
      "property": "email"
    }
  ],
  "properties": [
    {
      "error": {
        "language": [
          {
            "error": "value_not_match",
            "value": "een",
            "values": "en, no",
            "property": "language"
          }
        ],
        "optin_channel": [
          {
            "error": "not_contain_required_property",
            "property": "optin_channel"
          }
        ]
      }
    }
  ]
}
```

Error | Description
-------|------------
invalid_date_format | Invalid date format
invalid_time_format | Invalid time format
invalid_date_time_format | Invalid date with time format
must_be_valid_RFC3339_date_time_string | Format beyond the RFC3339 standard
invalid_URI | Invalid URI
additional_array_elements | Additional elements in the array
additional_properties | Additional properties
property_not_match_all_of | The property did not match all of the required schemas
property_not_match_any_of | The property did not match any of the required schema
property_matched_more_than_one | The property matched more than one of the required schemas
depends_on_a_missing_property | The validated property has a property that depends on the other missing property
value_not_match | The property value did not match one of the given values.
schema_cannot_be_found | The extended schema cannot be found
not_a_valid_schema | The property was not a valid schema
minimum_string_length | The property was not of a minimum string length of minimum limit.
maximum_string_length | The property was not of a maximum string length of maximum limit.
less_item_than_minimum | The property did not contain a minimum number of minimum items limit 
more_item_than_maximum | The property had more items than the allowed items limit
less_properties_than_minimum | The property did not contain a minimum number of properties
more_properties_than_maximum | The property had more properties than allowed
not_have_value_of_exclusively | The property did not have value of exclusively
not_have_value_of_inclusively | The property did not have value of inclusively
more_decimal_places_than_maximum | The property had more decimal places than the allowed maximum
matched_the_disallowed_schema | The property matched the disallowed schema
the_regex_not_match | The property did not match the regex 
not_contain_required_property | The validated property did not contain a required property
contained_undefined_properties | The validated property contained undefined properties
referenced_schema_cannot_be_found | The referenced schema cannot be found
invalid_schema | The property was not a valid schema
matched_one_or_more_types | The property matched one or more of the given types
one_or_more_types_not_match | The property did not match one or more of the given types
type_not_match | The property did not match the given type
contained_duplicated_array_values | The property contained duplicated array values
invalid_email | Invalid email format
disposable_email | The email was [disposable](https://github.com/lisinge/valid_email2/blob/master/vendor/disposable_emails.yml)
duplicated_email | The email was duplicated in community

## <a name="v3-member-model"></a> Member JSON model

> Example:

```json
{
  "id": 42,
  "properties": {
    "first_name": "Ola",
    "last_name": "Nordmann",
    "birthday": "1990-10-23",
    "interests": [
      "bikes_and_cars",
      "sportwear"
    ],
    "child_birth_years": [
      2010,
      2011,
      2011
    ],
    "language": "no"
  },
  "consents": {
    "consent1": { "status": true, "updated_at": "2018-12-14T21:57:20.063Z" },
    "consent2": { "status": false, "updated_at": "2018-10-25T21:57:43.738Z" }
  },
  "sms_status": "enabled",
  "email_status": "hard_bounced",
  "push_status": "disabled",
  "optin_channel": "webforms",
  "optin_subchannel": "campaign-10-2017",
  "created_at": "2017-01-19T10:07:08.336+01:00",
  "updated_at": "2017-04-03T09:35:19.313+02:00",
  "person_id": 99
}
```

Standard member JSON model returned from many API endpoints.

Key | Description | Type
--------- | ----------- | ---------
id | Member ID | integer
properties | Object with member's properties | JSON Object
properties\['language'\] | Language used by user | string
consents | Member's consents JSON model (see below) | JSON Object
sms_status | Status of sms channel | string
email_status | Status of email channel | string
push_status | Status of push channel | string
optin_channel | Channel (`X-Product-Name` header) which had been used to register member | string
optin_subchannel | Subchannel (`X-Subproduct-Name` header) which had been used to register member | string
created_at | Time when the user was firstly created | string
updated_at | Time when the user was last updated | string
person_id | Unique Member's identifier (set internally) | string

## <a name="v3-member-consents-model"></a> Member's consents JSON model

JSON model for consents. Keys can be dynamically created based on customer's need.

It consists of: `'consent-slug': { "status": <boolean_value>, "updated_at": <time> }`

* If status is `true` - Member approved consent.
* If status is `false` - Member disapproved consent.
* If you can't find slug on Member: he/she haven't disapproved or approved consent.

`updated_at` is the time of last consent update. It can be also not provided or null.

Available consents for Loyalty Club are described in [schema](#v3-loyalty-clubs-schema).

## <a name="v3-pagination-model"></a> Pagination JSON model

Used to present information about paginated pages

Key | Type | Description
--- | ---- | -----------
total_count | integer | Number of all available results
per_page | integer | Number of results returned per request
total_pages | integer | Number of all available pages
current_page | integer |
next_page | integer | Next available page (`null` when it's a last page)
prev_page | integer | Previous available page (`null` when it's a first page)
is_first_page | boolean | 
is_last_page | boolean | 
is_out_of_range | boolean | Is given `per_page` param out of range?

## <a name="v3-file-model"></a> File model (WIP)

Every file attached to some entity (Offer, Reward, Offer Collection) follows the files schema and is defined like this:

Key | Type | Description
--- | --- | -----------
url | URL | Link to the file
width | integer | Image width
height | integer | Image height
kind | string | File kind identifier (e.x. `offer_default`), see [Standard file kinds](#v3-file-kinds)
size_type | string | e.x. `base`, see: [File sizes](#v3-file-sizes)

## <a name="v3-file-schema"></a> Files schema (WIP)

Files schema defines a list of files kinds available in the Loyalty club. 
Each file kind is also available in multiple sizes. 

The schema may get retrieved with [Files&bull; Get schema](#v3-get-files-schema) endpoint.

> Example loyalty club file schema

```json
{
  "files_kinds": [
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

### <a name="v3-file-kind-model"></a> File kind model

Key | Type | Description
--------- | ----------- | ---------
identifier | string | See: [Standard file kinds](#v3-file-kinds)
type | string | Currently, only `IMAGE` (MIME: `image/gif`, `image/jpeg`, `image/png`) is supported
ratio | string | Describes ratio of an image. See: [Ratio](#v3-file-kind-ratio) (optional)
sizes| Array<Object> | See: [File sizes](#v3-file-sizes)
sizes[]['identifier'] | string | 
sizes[]['min_width'] | integer | (optional)
sizes[]['min_height'] | integer | (optional)
sizes[]['max_width'] | integer | (optional)
sizes[]['max_height'] | integer | (optional)

### <a name="v3-file-kinds"></a> Standard file kinds

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

Actual file size. Currently all file kinds have this size with fixed proportions (so `min_*` == `max_*`) that match the ratio.

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

Original file that that has been uploaded and used for generating other file sizes.

Currently, we require all original files to have proper ratios and minimum sizes of at least the `base` size.
Because of that, you can expect them to have `min_height` and `min_width` that match the `base` size, 
but they never have `max_width` or `max_height` defined.
