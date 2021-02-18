# Introduction

This documentation describes Placewise's MPC APIs that allow to manage Loyalty Club, its members and content.

## Hosts

Production: [https://api.mpc.placewise.com](https://api.mpc.placewise.com)

Production (APAC region): [https://api.mpc.apac.placewise.com](https://api.mpc.apac.placewise.com)

Staging: [https://api.mpc.dev.placewise.com](https://api.mpc.dev.placewise.com)

### HTTPS (TLS) requirement

To make connection to Placewise's MPC APIs you have to use HTTPS with TLS encryption.
Your client has to support TLS1.2+ with strong ciphers.

## Common params and headers

### "content-type" header

This API supports `application/json` only. 

Both payload and response body are supposed to be a valid JSON so when making a request send a `content-type: application/json` header.

If payload is not a valid JSON, then `406 Not Acceptable` HTTP code and empty response body are returned.

### "x-client-authorization" header

> Example header: `x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM`

All of the endpoints **require** a client authorization header - `x-client-authorization`.

It should be used only on backend and never exposed in frontend code.

Each token has are permits assigned to it. Depending what permits are assigned, access to some of the endpoints
may be restricted.

If you miss your authentication token, please [let us know](https://placewise.com).

### "x-user-agent" header

> Example header: `x-user-agent: Infinity Mall Android App `
  
`x-user-agent` is **required** to distinguish specific client for information and debugging purposes so we better know who uses the service.

It should be arbitrarily chosen to represent client specifics (e.g. 'Infinity Mall Android App v1.2' or 'Infinity Mall Backend Service')

### "x-product-name" header

> Example header: `x-product-name: android-app`

Each system that is communicating with us should uniquely identify itself so it is possible to distinguish optin/update channels.
That will allow further targeting members by communication channel.

For that we use product name. It should be passed as **required** header `x-product-name` that is intended to provide the necessary granularity.

If you miss your product name, please [let us know](https://placewise.com).

### "authorization" header

> Example header: `authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522`

See: [OAuth2](#oauth2)

### Loyalty club slug param

> Example slug: 'infinity-mall'

As all of the API endpoints work in the context of specific Loyalty Club, they're scoped within it's identifier - `:loyalty_club_slug`

It's an unique slugified name of the Loyalty Club.

### <a name="msisdn-param"></a> Msisdn param

Members in our systems may be identified with their phone numbers (msisdns) according to [E.164](https://en.wikipedia.org/wiki/E.164)
We use format without leading `00` or `+` and without spaces, so that it contains only digits, i.e. `4740769126`.

## Common HTTP error codes

Status | Reason
-------|-----|-------
`401` | `x-client-authorization` is invalid (or doesn't match provided loyalty club or product)
`402` | Feature is not enabled for the Loyalty Club
`403` | Not authorized to perform this action (provided `x-client-authorization` doesn't have required permit)
`404` | The requested resource doesn't exist
`422` | Invalid parameters are provided (e.g. incorrect properties on member creation)
`460` | OAuth token required for the action is invalid (applies only to OAuth-related actions - see [OAuth2](#oauth2))
`470` | Some of required header is missing
`471` | Some of headers is incorrect
`503` | Server is down for maintenance

Also, most of handled errors have JSON response body like this:

`{"error": "Message describing what went wrong"}`

## <a name="pagination-model"></a> Pagination JSON model

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

## <a name="invalid-parameters-errors-model"></a> Invalid parameters errors (422)

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
details | Errors object | See [Errors](#errors-object) below
details.(path.0.to.)parameter_name | Array<ParameterError> | See [ParameterError](#parameter-error-object) below

#### <a name="errors-object"></a> Errors object

It's a recursive tree structure, corresponding to the structure original payload.
Objects being parts of arrays are represented by their array indexes.

Each leaf node is an array of [ParameterError](#parameter-error-object) objects for given parameter.

#### <a name="parameter-error-object""></a> ParameterError object

key | Type | Description
--- | ---- | -----------
value | Mixed | Value of given parameter
error | String | Symbol of error that relates to this parameter
options | Object| Contains options of specific validation rule. For example `{"max": 15}`.
message | String | Human-readable error message

#### <a name="common-validation-errors"></a> Common validation errors

Error key                        | Options                            | Description
---                              | ---                                | ---                                                         
presence                         |                                    | When not present 
inclusion                        | `in` - contains permitted values   | When doesn't match list of permitted values
must_be_an_object                |                                    | 
must_be_an_array                 |                                    |
must_be_an_array_of_objects      |                                    |
must_have_elements_of_type       |  `type` - required type            |
not_an_integer                   |                                    |
invalid_msisdn                   |                                    | When cannot be processed as a valid [MSISDN](#msisdn-param)
invalid_email                    |                                    |     
invalid_iso_8601_time            |                                    | When not valid according to [ISO 8601](#https://en.wikipedia.org/wiki/ISO_8601)
attribute_cannot_be_present_with | `with` - other attribute name      | Mutually exclusive with another attribute
size_too_small                   | `number` - min size                | When array has not enough elements
size_too_large                   | `number` - max size                | When array has too many elements
size_not_match                   | `number` - exact size              | When array has not exactly required number of elements
taken                            |                                    | When another record already uses this value
not_a_number                     |                                    |    
greater_than                     | `number` - min value               | When not > `number`
greater_or_equal_to              | `number` - min value               | When not >= `number`
less_than                        | `number` - max value               | When not < `number`
less_or_equal_to                 | `number` - max value               | When not <= `number`
invalid                          | `format` - regex                   | When does not match regex
liquid_syntax_error              | `syntax_error` - error description | When not valid [liquid](https://shopify.github.io/liquid/) syntax
must_be_in_the_future            |                                    | When date is not in the future
must_be_in_the_past              |                                    | When date is not in the past
