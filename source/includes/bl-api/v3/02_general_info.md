
# General info

## Hosts

Production: [https://bpc-api.boostcom.no](https://bpc-api.boostcom.no)

Production (APAC region): [https://bpc-api.apac.boostcom.com](https://bpc-api.apac.boostcom.com)

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

## <a name="v3-pagination-model"></a> Pagination JSON model

> Example:

```json
{
    "total_count": 2050,
    "per_page": 1000,
    "total_pages": 3,
    "current_page": 1,
    "next_page": 2,
    "prev_page": null,
    "is_first_page": true,
    "is_last_page": false,
    "is_out_of_range": false
}
```

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

## <a name="v3-file-model"></a> File model

Every file attached to some entity (Offer, Reward, Offer Collection) follows the file schema and is defined like this:

Key | Type | Description
--- | --- | -----------
url | URL | Link to the file
width | integer | Image width
height | integer | Image height
kind | string | File kind identifier (e.x. `offer_default`), see [Standard file kinds](#v3-file-kinds)
size_type | string | e.x. `base`, see: [File sizes](#v3-file-sizes)

## <a name="v3-file-schema"></a> File schema

File schema defines a list of files kinds available in the Loyalty club. 
Each file kind is also available in multiple sizes. 

The schema may get retrieved with [Files&bull; Get schema](#v3-get-file-schema) endpoint.

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
      "identifier": "example_without_ratio",
      "type": "IMAGE",
      "ratio": null,
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": 600,
          "max_width": null,
          "max_height": null
        },
        {
          "identifier": "thumbnail",
          "min_width": null,
          "min_height": null,
          "max_width": 300,
          "max_height": 300
        },
        {
          "identifier": "original",
          "min_width": 600,
          "min_height": 600,
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

Images may have fixed proportions ratio which is represented as a string: `"<width>:<height>"`

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

## <a name="v3-invalid-parameters-errors-model"></a> Invalid parameters errors (422)

> Example:

```js
// Considering that request with following payload has been sent:
{
  "member": {
    "first_name": "s",
    "msisdns": [
      "123",
      "4740769126",
      ""
    ]
  }
}

// Following response is returned: 
{
  "error": "Invalid parameters",
  "details": {
    "member": {
      "name": [
        {
          "value": "s",
          "error": "too_short",
          "options": {
            "min_length": 0
          },
          "message": "is too short"
        },
        {
          "value": "s",
          "error": "invalid_format",
          "options": {
            "format": "/[A-Z][a-z]+/"
          },
          "message": "has invalid format"
        }
      ],
      "msisdns": {
        "0": [
          {
            "value": "32",
            "error": "invalid_msisdn",
            "options": {},
            "message": "is not a valid MSISDN"
          }
        ],
        "2": [
          {
            "value": "",
            "error": "blank",
            "options": {},
            "message": "cannot be blank"
          }
        ]
      }
    }
  }
}

// It means that in given payload:
// * member.name is too short 
// * member.name has invalid format
// * member.msisdns[0] is not a valid MSISDN
// * member.msisdns[2] is blank
```

Some of the endpoints (it's noted on specific request's description) return invalid parameters errors in the standardized way.

For them, when some of parameter (either JSON body or URL/Query) is invalid, server responds with 422 with a following 
object:  

key | Type | Description
--- | ---- | -----------
error | String | Always "Invalid parameters"
details | Errors object | See [Errors](#v3-errors-object) below
details.(path.0.to.)parameter_name | Array<ParameterError> | See [ParameterError](#v3-parameter-error-object) below

### <a name="v3-errors-object"></a> Errors object

It's a recursive tree structure, corresponding to the structure original payload.
Objects being parts of arrays are represented by their array indexes.

Each leaf node is an array of [ParameterError](#v3-parameter-error-object) objects for given parameter.

### <a name="v3-parameter-error-object""></a> ParameterError object

key | Type | Description
--- | ---- | -----------
value | Mixed | Value of given parameter
error | String | Symbol of error that relates to this parameter
options | Object| Contains options of specific validation rule. For example `{"max": 15}`.
message | String | Human-readable error message
