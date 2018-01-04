
# General info

## Hosts

Production: [https://bpc-api.boostcom.no](https://bpc-api.boostcom.no)

Staging: [https://bpc-api.stg.boostcom.no](https://bpc-api.stg.boostcom.no)

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
  "errors": {
    "properties": {
      "birthday": [
        {
          "error": "invalid_date_format",
          "property": "birthday"
        }
      ],
      "gender": [
        {
          "error": "value_not_match",
          "property": "gender",
          "value": "man",
          "values": "Mann, Kvinne"
        }
      ],
      "first_name": [
        {
          "error": "not_contain_required_property",
          "property": "first_name"
        }
      ]
    }
  }
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
    "consent1": { "status": true },
    "consent2": { "status": false }
  },
  "sms_status": "enabled",
  "email_status": "hard_bounced",
  "push_status": "disabled",
  "created_at": "2017-01-19T10:07:08.336+01:00",
  "updated_at": "2017-04-03T09:35:19.313+02:00"
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
created_at | Time when the user was firstly created | string
updated_at | Time when the user was last updated | string

## <a name="v3-member-consents-model"></a> Member's consents JSON model

JSON model for consents. Keys can be dynamically created based on customer's need.

It consists of: `'consent-slug': { status: <boolean value> }`

* If status is `true` - Member approved consent.
* If status is `false` - Member disapproved consent.
* If you can find slug on Member: he/she haven't disapproved or approved consent.
