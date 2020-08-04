# General info

MPC API allows to manage Loyalty Club, its members and content.

## Hosts

Production: [https://bpc-api.boostcom.no](https://bpc-api.boostcom.no)

Production (APAC region): [https://bpc-api.apac.boostcom.com](https://bpc-api.apac.boostcom.com)

Staging: [https://bpc-api.dev.boostcom.no](https://bpc-api.dev.boostcom.no)

## Common params and headers

### Content-Type header

This API supports `application/json` only. 

Both payload and response body are supposed to be a valid JSON so when making a request send a `Content-Type: application/json` header.

If payload is not a valid JSON, then `406 Not Acceptable` HTTP code and empty response body are returned.

### X-Client-Authorization header

> Example header: `X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM`

All of the endpoints **require** a client authorization header - `X-Client-Authorization`.

It should be used only on backend and never exposed in frontend code.

Each token has are permits assigned to it. Depending what permits are assigned, access to some of the endpoints
may be restricted.

If you miss your authentication token, please [let us know](http://boostcom.no).

### X-User-Agent header

> Example header: `X-User-Agent: Infinity Mall Android App `
  
`X-User-Agent` is **required** to distinguish specific client for information and debugging purposes so we better know who uses the service.

It should be arbitrarily chosen to represent client specifics (e.g. 'Infinity Mall Android App v1.2' or 'Infinity Mall Backend Service')

### X-Product-Name header

> Example header: `X-Product-Name: android-app`

Each system that is communicating with us should uniquely identify itself so it is possible to distinguish optin/update channels.
That will allow further targeting members by communication channel.

For that we use product name. It should be passed as **required** header `X-Product-Name` that is intended to provide the necessary granularity.

If you miss your product name, please [let us know](http://boostcom.no).

### Authorization header

> Example header: `Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522`

See: [OAuth2](#v3-oauth2)

### Loyalty club slug param

> Example slug: 'infinity-mall'

As all of the API endpoints work in the context of specific Loyalty Club, they're scoped within it's identifier - `:loyalty_club_slug`

It's an unique slugified name of the Loyalty Club.

### Msisdn param

Members in our systems may be identified with their phone numbers (msisdns) according to [E.164](https://en.wikipedia.org/wiki/E.164)
We use format without leading `00` or `+` and without spaces, so that it contains only digits, i.e. `4740485124`.

## Common HTTP error codes

Status | Reason
-------|-----|-------
`401` | `X-Client-Authorization` is invalid (or doesn't match provided loyalty club or product)
`402` | Feature is not enabled for the Loyalty Club
`403` | Not authorized to perform this action (provided `X-Authorization-Token` doesn't have required permit)
`404` | The requested resource doesn't exist
`422` | Invalid parameters are provided (e.g. incorrect properties on member creation)
`460` | OAuth token required for the action is invalid (applies only to OAuth-related actions - see [OAuth2](#v3-oauth2))
`470` | Some of required header is missing

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
