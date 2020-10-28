# Members API

##  Members

### Introduction

#### Authentication

Actions related to specific member (the one that is using your application) require to have member authenticated and we implement
OAuth2 flow for this.

See [Members OAuth](#oauth2) to see how to authenticate the member with OAuth2.

#### <a name="member-model"></a> Member model

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
  "banned_since": "2137-04-03T09:35:19.313+02:00",
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
optin_channel | Channel (`x-product-name` header) which had been used to register member | string
optin_subchannel | Subchannel (`X-Subproduct-Name` header) which had been used to register member | string
created_at | Time when the user was firstly created | string
updated_at | Time when the user was last updated | string
banned_since | Time since member was/is banned (not updatable through OAuth) | string
person_id | Unique Member's identifier (set internally) | string

#### <a name="member-consents-model"></a> Member's consents JSON model

JSON model for consents. Keys can be dynamically created based on customer's need.

It consists of: `'consent-slug': { "status": <boolean_value>, "updated_at": <time> }`

* If status is `true` - Member approved consent.
* If status is `false` - Member disapproved consent.
* If you can't find slug on Member: he/she haven't disapproved or approved consent.

`updated_at` is the time of last consent update. It can be also not provided or null.

Available consents for Loyalty Club are described in [schema](#loyalty-clubs-schema).

#### Validation on members

All members' endpoints have properties validation. Properties are validated to conform loyalty club's schema.

If something is wrong, you will receive explanation of errors in response.

##### Schema validation errors

```shell
curl -X PUT -H "x-product-name: custom-product-name" \
    -H "X-Customer-Private-Token: token" \
    -H "content-type: application/json" -d '{
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
invalid_mx | Invalid (inaccessible) email domain
disposable_email | The email was [disposable](https://github.com/lisinge/valid_email2/blob/master/vendor/disposable_emails.yml)
duplicated_email | The email was duplicated in community

### <a name="members-index"></a> List

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members?per_page=100&page=1&ids[]=1&ids[]=2" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [members](#member-model) and [pagination_info](#pagination-model)

```json
{
  "members": [], // List of members - See: "Member model"
  "pagination_info": {} // Pagination info - see "Pagination info"
}

```

**GET** `v3/:loyalty_club_slug/members?per_page=:per_page&page=:page`

Returns paginated list of all Loyalty Club members, sorted by `created_at ASC`.

Every returned member is represented by [Member model](#member-model).

#### Query Parameters

Parameter | Type | Required? | Default | Description
--------- | ----------- | ----------- | --------- | -----------
per_page | integer | no | 1000 | Number of results to be returned per request (1000 is the maximum)
page_no | integer | no | 1 | Number of results page
ids | Array<integer> | no | null | IDs of members that will be returned
properties[:property_name] | Array<any> | no | null | Properties of members that will be returned. A property must be enabled for querying in the Loyalty Club configuration before it can be used here. Please contact us if you need this feature.

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members | Array<Member> | Array of [Members](#member-model)
pagination_info | Object | [Pagination](#pagination-model) object

#### Error responses

Status | Reason
--------- | ----------- 
`422` | :per_page param exceeds the limit

#### Example queries

* `/members?properties[gender]=man&properties[language][]=en&properties[language][]=pl` - returns members with `gender="man" AND language IN ("en", "pl")`
* `/members?ids[]=1&ids[]=2` - returns members with `id IN (1, 2)`

<aside class="notice">
Requires <code>BL:Api:Members:Index</code> permit
</aside>

### <a name="members-get"></a> Get

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/:id" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, returns member object as depicted [here](#member-model)

**GET** `v3/:loyalty_club_slug/members/:id`

<aside class="notice">
Requires <code>BL:Api:Members:Get</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/by_msisdn/:msisdn`

<aside class="notice">
Requires <code>BL:Api:Members:Get</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/by_email/:email`

<aside class="notice">
Requires <code>BL:Api:Members:Get</code> permit
</aside>

**GET** `v3/:loyalty_club_slug/members/me`

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:Get</code> permit
</aside>

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer
msisdn | Member's msisdn | string (format as defined [here](#msisdn-param) - example: `4740485124`)
email | Member's email | string (email)

#### Response (JSON object)

See: [Member model](#member-model)

##### Error responses

Status | Reason
--------- | ----------- 
`404` | Member with given identifier could not found 
`422` | Invalid MSISDN param

### <a name="members-public-info"></a> Get public info

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/:id/public_info" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns hash structured like this:

```json
{
  "exists": true, 
  "can_login": true, // deprecated
  "available_identifiers": ["email"],
  "has_password": true
}
```

**GET** `v3/:loyalty_club_slug/members/:id/public_info`

**GET** `v3/:loyalty_club_slug/members/by_msisdn/:msisdn/public_info`

**GET** `v3/:loyalty_club_slug/members/by_email/:email/public_info`

Returns basic info for given member. If member does not exist, returns `null'.


#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer
msisdn | Member's msisdn | string (format as defined [here](#msisdn-param) - example: `4740485124`)
email | Member's email | string (email)

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
exists | boolean | Does member with given identifier exist in given Loyalty Club? 
can_login | boolean | Is this member able to sign? (deprecated)
available_identifiers | string[] | Identifiers that are set on member
has_password | boolean | Was password set by member?

<aside class="notice">
Requires <code>BL:Api:Members:Check</code> permit
</aside>

#### Error responses

Status | Reason
--------- | ----------- 
`422` | Invalid MSISDN param

<!--- ############################################################################################################# --->

### <a name="members-person-id"></a> Get person id

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/:id/person_id" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns hash structured like this:

```json
{
  "source": "storage", 
  "person_id": 92 
}
```

**GET** `v3/:loyalty_club_slug/members/:id/person_id`

**GET** `v3/:loyalty_club_slug/members/by_msisdn/:msisdn/person_id`

**GET** `v3/:loyalty_club_slug/members/by_email/:email/person_id`

Returns `person_id` for given member. Please note that `person_id` cannot be preset in member payload and will be silently ignored. Also, it cannot be updated.

> There are 4 possible successful responses:

> If member currently exists in database:

```json
{
  "success": true,
  "source": "db", 
  "person_id": 92 
}
```

> If member does not exist but was deleted in last 30 days:

```json
{
  "success": true,
  "source": "storage", 
  "person_id": 92 
}
```

> If member had existed, was deleted (in last 30 days) and was created again so it exists now:

```json
{
  "success": true,
  "source": "db_and_cache", 
  "person_id": null 
}
```

> If member does not exist and never existed or did not exist in last 30 days:

```json
{
  "success": true,
  "source": "not_found", 
  "person_id": null 
}
```

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer
msisdn | Member's msisdn | string (format as defined [here](#msisdn-param) - example: `4740485124`)
email | Member's email | string (email)

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
success | boolean | If query was successful?
source | string | Source of `person_id` (possible values are: `not_found`, `db`, `storage`)
person_id | integer | Member's `person_id`

<aside class="notice">
Requires <code>BL:Api:Members:Check</code> permit
</aside>

#### Error responses

Status | Reason
--------- | ----------- 
`422` | Invalid MSISDN param


<!--- ############################################################################################################# --->

### <a name="members-create"></a> Create

> Example:

```shell

curl -X POST \
  https://bpc-api.boostcom.no/v3/infinity-mall/members \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: facebook' \
  -H 'X-Subproduct-Name: campaign-10-2017' \
  -H 'x-user-agent: CURL manual test' \
  -d '{
	"properties": {
		"email": "dev+6@example.com",
		"msisdn": "4740485124",
		"first_name": "The",
		"last_name": "Doge"
	},
	"consents": {
	    "consent1": { "status": true },
	    "consent2": { "status": false }
	},
	"send_sms_welcome_message": false
}'

```

> When successful, the above command returns created member object as depicted [here](#member-model)

> When payload is invalid, validation errors like this are returned:

```json
{
    "email": [
        {
        
            "property": "email",
            "error": "duplicated_email_in_community"
        }
    ]
}
```

**POST** `v3/:loyalty_club_slug/members`

Create member with given properties.

Available properties and their validation rules are defined by Loyalty Club schema (see: [here](#loyalty-clubs-schema)).

Actual welcome messages sending depends on Loyalty Club and Product configuration.
For example, even if send_email_welcome_message:true param is provided, message may not be sent because either Product 
or Loyalty has disabled welcome messages or Loyalty Club has no e-mails configured at all.

There is also a possibility to have multiple SMS welcome messages sent. The one that matches Product or default one will be sent.

#### <a name="members-create-registration-password"></a> Registration Password

Some API clients (depends on Permit assigned to given `x-client-authorization` token) may be required to provide 
valid `registration_password` param that is sent to user with [Members &bull; Send registration password](#members-send-registration-password) 

#### Headers

Header name | Required? | Description
------ | --------- | -----------
X-Subproduct-Name | no | Additional source/optin channel information

#### <a name="members-create-post-parameters"></a> POST Parameters (JSON)

Parameter | Required? | Default | Description | Type
--------- | ----------- | ----------- | --------- | -----------
properties | **yes** | none | JSON with properties for member | JSON Object
properties\['language'\] | no | "default_language" from schema | Language used by user | string
properties\['msisdn'\] | yes* | none | Unique member's msisdn as defined [here](#msisdn-param)) Example: `4740485124`.| string
properties\['email'\] | yes* | none | Member's email | string
consents | no | {} | Member's consents (similar to [Member's consents JSON model](#member-consents-model)) | JSON Object
sms_enabled | no | true | Should SMS channel be enabled for member? | Boolean
email_enabled | no | true | Should email channel be enabled for member? | Boolean
push_enabled | no | true | Should push channel be enabled for member? | Boolean
send_sms_welcome_message | no | true | Should SMS welcome message be sent to member? | Boolean
send_email_welcome_message | no | true | Should email welcome be sent to member | Boolean
password | depends | none | Member's password. Not required, but user won't be able to log in without this when only its email is provided | string
registration_password | depends | none | Password for registration for member MSISDN verification, see [above](#members-create-registration-password)

&ast; At least one of those properties must be provided
 
#### Response (JSON object)

Created member properties - see: [Member model](#member-model)

#### Error responses

Status | Description
--------- | ----------- 
`422` | [validation errors](#validation-on-members) JSON object.
`465` | `registration_password` or `password` param is missing
`466` | `registration_password` param is invalid
`467` | `{"error": "Member is banned!", "banned_since": "2020-10-28T17:24:06.207Z"}` | -

<aside class="notice">
Requires <code>BL:Api:Members:Create</code> or <code>BL:Api:Members:CreateWithVerification</code>permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-update"></a> Update

> Example:

```shell

curl -X PUT \
  https://bpc-api.boostcom.no/v3/infinity-mall/members/:id \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
  '{
    "properties": {
      "last_name": "Doge"
    },
    "password": "new_password"
  }'
```

> When successful, the above command returns updated member object as depicted [here](#member-model)

> When payload is invalid, validation errors as depicted [here](#members-create)

**PUT** `v3/:loyalty_club_slug/members/:id`

<aside class="notice">
Requires <code>BL:Api:Members:Update</code> permit
</aside>

**PUT** `v3/:loyalty_club_slug/members/me` [OAuth](#oauth2)

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:Update</code> permit.
</aside>

Update member's properties, consents and other with given ones.

It is intended for partial updates:
* not given properties are neither deleted nor overwritten,
* not given attributes (like consents) won't be changed in any way.

If deleting properties is intended, their value should be sent as `null`.

This endpoint may return validation errors (`422`) even for current member properties. 
That's because loyalty club schema may get changed over time which results in invalidating existing users.

To bypass such validation errors, you may provide `validate_partially: true` param which will validate and update only 
attributes that were provided in payload. 

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer

#### PUT Parameters (JSON)

Parameter | Description | Type | Default
--------- | --------- | ----------- | -----------
properties | JSON with properties for member | JSON Object | null
consents | Member's consents (similar to [Member's consents JSON model](#member-consents-model)) | JSON Object | null
password | Member's password | string | null
sms_enabled | Should SMS channel be enabled for member? | Boolean | null
email_enabled | Should email channel be enabled for member? | Boolean | null
push_enabled | Should push channel will be enabled for member? | Boolean | null
validate_partially | Should only provided data be validated? | Boolean | false
event_occurred_at | See: [`event_occurred_at` param](#member-event-occured-at-param) | Date | (current time)

##### <a name="member-event-occured-at-param"></a> `event_occurred_at` param

When actual member data change occurred at the time different than request's time, you can utilize the `event_occurred_at' param to pass
the actual update time.

This may be relevant when you want this to be reflected in the member's changes history.

#### Response (JSON object)

Member properties after update - see: [Member model](#member-model)

#### Error responses

Status | Description
--------- | ----------- 
`404` | Member could not be found
`422` | [validation errors](#validation-on-members) JSON object.


<!--- ############################################################################################################# --->

### <a name="me-update-password"></a> Update password

> Example:

```shell
curl -X PUT \
  https://bpc-api.boostcom.no/v3/infinity-mall/members/me/update_password \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
  -d \
  '{
	  "current_password": "123"
	  "password": "SuperStrongP4ssw0rd"
  }'
```

> Always returns an empty object

```json
{
  // Empty object
}
```

**PUT** `v3/:loyalty_club_slug/members/update_password` [OAuth](#oauth2)

Updates members's password.

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
password | Member's new password | string
current_password | Member's current password | string

#### Error responses

Status | Reason
--------- | -----------
`404` | Member associated with given Authorization token does not exist
`422` | Invalid password - returns [validation errors](#validation-on-members) JSON object.
`460` | Member not authorized (Invalid or expired OAuth token)
`464` | Invalid current_password

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:UpdatePassword</code> permit.
</aside>

### <a name="members-update-app-token"></a> Update app token

> Example:

```shell

curl -X PUT \
  https://bpc-api.boostcom.no/v3/infinity-mall/members/:id/update_app_token \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
  '{
     "app_token": "tok3n",
     "app_platform": "android"
   }'
```

> When successful, returns an empty object

```json
{
  // Empty object
}
```

> When payload is invalid, returns error response (422)

```json
{
    "error": "Invalid parameters",
    "details": {
        "app_token": [
            "must be a string"
        ],
        "app_platform": [
            "must be one of: android, ios"
        ]
    }
}
```

**PUT** `v3/:loyalty_club_slug/members/:id/update_app_token`

<aside class="notice">
Requires <code>BL:Api:Members:UpdateAppToken</code> permit
</aside>

**PUT** `v3/:loyalty_club_slug/members/me/update_app_token` [OAuth](#oauth2)

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:UpdateAppToken</code> permit
</aside>

Updates member's Firebase token, along with platform it's that it's associated with. 

Token may be removed by sending it as empty string or null, empty payload will also be treated as token removal.
Platform is automatically removed when token is empty.

#### PUT Parameters (JSON)

Parameter | Type | Description
--------- | ----------- | ------
app_token | string | Firebase push token
app_platform | string | One of: `['android', 'ios']` 

#### Error responses

Status | Description
--------- | ----------- 
`404` | Member could not be found
`422` | Invalid parameters

<!--- ############################################################################################################# --->

### <a name="members-destroy"></a> Destroy

**DELETE** `v3/:loyalty_club_slug/members/:id`

<aside class="notice">
Requires <code>BL:Api:Members:Destroy</code> permit
</aside>

**DELETE** `v3/:loyalty_club_slug/members/me` [OAuth](#oauth2)

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:Destroy</code> permit.
</aside>

Permanently removes member.

It is possible to trigger optout message by setting `send_unsubscribe_message=true` query string parameter.
Same as welcome messages, optout messages sending also depends on Loyalty Club and Product configuration.

> Example:

```shell
curl -X DELETE \
    "https://bpc-api.boostcom.no/v3/infinity-mall/members/:id" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful, the above command returns destroyed member object as depicted [here](#member-model)

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer

#### Query Parameters

Parameter | Required? | Default | Description | Type
--------- | ----------- | ----------- | --------- | -----------
send_email_unsubscribe_message | no | true | Should optout email be sent to member? | Boolean

#### Response (JSON object)

Properties of member that has been destroyed - see: [Member model](#member-model)

#### Error responses

Status | Reason
--------- | ----------- 
`404` | Member could not be found
`422` | [validation errors](#validation-on-members) JSON object.

<!--- ############################################################################################################# --->


### <a name="members-validate"></a> Validate

> Example:

```shell

curl -X POST \
  https://bpc-api.boostcom.no/v3/infinity-mall/members/validate \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: android-app' \
  -H 'X-Subproduct-Name: campaign-10-2017' \
  -H 'x-user-agent: CURL manual test' \
  -d '{
	"properties": {
		"gender": "wrong",
		"email": "foo@ba.r.ba.z"
	},
	"registration_password": "a145"
  }'

```

> When data is valid, returns JSON object structured like this 

```json
{
    "valid": true,
    "errors": null
}
```

> When data is not valid, returns JSON object structured like this 

```json
{
    "valid": false,
    "errors": {
        "properties": [
            {
                "error": {
                    "gender": [
                        { "error": "value_not_match", "property": "gender", "value": "wrong", "values": "man, woman" }
                    ]
                }
            }
        ],
        "email": [{ "error": "invalid_mx", "property": "email"}
        ],
        "registration_password": [
            {
                "error": "invalid"
            }
        ]
    }
}
```

**POST** `v3/:loyalty_club_slug/members/validate`

Checks if given data is valid for new member registration. Returns successful response (200), regardless of data validity.

#### About partial validation mechanism

Only properties which keys are present in given payload will be checked against presence validation.

For example, let's say that Loyalty Club configuration requires member to have `msisdn` property set.

* if `"msisdn": ""` or`"msisdn": null`" is present in the payload, the validation will return error stating that `msisdn` must be present.
* however, when payload doesn't include the `msisdn` key, such error will not be returned.

This behaviour has been designed to allow validate member partially, property by property. This way you can choose 
what values should be validated at specific point of registration.

#### <a name="members-validate-post-parameters"></a> POST Parameters (JSON)

Following keys of [Members &bull; Create](#members-create-post-parameters) payload are supported when validating:

* properties
* consents
* registration_password

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
valid | boolean | Is the data valid?
errors | object | [validation errors](#validation-on-members) JSON object, `null` when data is valid

<aside class="notice">
Requires <code>BL:Api:Members:Validate</code> permit
</aside>

### <a name="members-reset-password"></a> Reset password

> Example:

```shell
curl -X PUT \
  "https://bpc-api.boostcom.no/v3/:loyalty_club_slug/members/by_email/:email/reset_password" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
    '{
      "password": "new_password",
      "token": "k4wort03j2"
    }'
```

> When successful, returns an empty object:

```json
{
  // Empty object
}
```

**PUT** `/v3/:loyalty_club_slug/members/by_email/:email/reset_password`

Updates given member password if token is valid.

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
password | Member's new password | string
token | Confirmation token generated and sent with [Members &bull; Send password reset token](#members-send-password-reset-token) | string

#### Error responses

Status | Reason
--------- | ----------- 
`400` | `password` or `token` param is missing
`404` | Member could not be found
`422` | Invalid password - returns [validation errors](#validation-on-members) JSON object.
`463` | Invalid confirmation token

<aside class="notice">
Requires <code>BL:Api:Members:ResetPassword</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-send-password-reset-token"></a> Send password reset token

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_email/joe@example.com/send_password_reset_token" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Always returns an empty JSON object

```json
{
  // Empty object
}
```

**POST** `/v3/:loyalty_club_slug/members/by_email/:email/send_password_reset_token`

Sends password reset link to given e-mail address if it is associated with member in given loyalty club.

The e-mail contains a token generated for member, valid for 24 hours.

The token can be then used to reset password with [Members &bull; Reset password](#members-reset-password).
It also may be verified with [Members &bull; Verify token](#members-verify-token)

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
email | Member's email | string (email)

<aside class="notice">
Requires <code>BL:Api:Members:Tokens:Create</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-send-one-time-password"></a> Send one time password SMS

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_msisdn/4740485124/send_one_time_password" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Always returns an empty JSON object

```json
{
  // Empty object
}
```

**POST** `/v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_one_time_password`

Sends an SMS to given msisdn if it is associated with member in given loyalty club.

The SMS contains 4-digit One-Time-Password generated for member, valid for 1 hour.

This password then can be used to sign in, just as "regular" member password - see:  [OAuth Token &bull; Create](#token-create).

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
msisdn | Member's msisdn | string (format as defined [here](#msisdn-param) - example: `4740485124`)

<aside class="notice">
Requires <code>BL:Api:Members:CreateOneTimePassword</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-send-one-time-password-email"></a> Send one time password E-mail

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_email/user@example.com/send_one_time_password" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Always returns an empty JSON object

```json
{
  // Empty object
}
```

**POST** `/v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_one_time_password`

Sends a "Login" e-mail to given address if it is associated with member in given loyalty club. The e-mail contains a deep link to the mobile app.
 
The link scheme is: `https://<app_scheme>.al.bstcm.no/lgn?member_id=<member_id>&otp=<otp>` and contains member id and 16-digit password generated for member, valid for 1 hour.  

Those params then can be used to sign in the user, just as "regular" member password - see:  [OAuth Token &bull; Create](#token-create).

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
email | Member's email | e-mail

#### Error responses

Status | Reason
--------- | ----------- 
`422` | Invalid e-mail param

<aside class="notice">
Requires <code>BL:Api:Members:CreateOneTimePassword</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-send-registration-password"></a> Send registration password

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_msisdn/4740485124/send_registration_password" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
    '{
      "language": "en"
     }'
```

> Always returns an empty JSON object

```json
{
  // Empty object
}
```

**POST** `/v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_registration_password`

Sends SMS to given MSISDN.

The sent message contains 4-digit registration password, valid for 10 minutes.

This password may be required for member registration - see: [Registration password](#members-create-registration-password)

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
msisdn | MSISDN | string (format as defined [here](#msisdn-param) - example: `4740485124`)

#### POST Parameters

Parameter | Description | Type
--------- | ----------- | ------
language | Language of the registration password message. When not provided, default LC language will be used | string

#### Error responses

Status | Reason
--------- | ----------- 
`422` | Invalid MSISDN param

<aside class="notice">
Requires <code>BL:Api:Members:CreateRegistrationPassword</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-verify-token"></a> Verify token

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_email/joe@example.com/verify_token/password_reset/k4wort03j2" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Example response:

```json
{
  "valid": true
}
```

**GET** `/v3/:loyalty_club_slug/members/by_email/:email/verify_token/:type/:token`

Verifies given member token. Returns response indicating if given token is (still) valid.

#### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
email | Member's email | string (email)
type | Token type | `password_reset` (more types in future) 
token | Token | string

#### Response (JSON object)

Key | Type
--------- | ---------
valid | boolean

#### Error responses

Status | Reason
--------- | ----------- 
`400` | Invalid `type`

<aside class="notice">
Requires <code>BL:Api:Members:Tokens:Verify</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-send-verification-sms"></a> Send MSISDN verification SMS

> Example:

```shell
curl -X PUT \
  "https://bpc-api.boostcom.no/v3/infinity-mall/members/channels/msisdn/4740485124/send_verification" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
    '{
      "for_mobile_app": false
    }'
```

**PUT** `/v3/:loyalty_club_slug/members/channels/msisdn/:msisdn/send_verification`

Generates a verification token for member identified by given MSISDN and sends it to him with an SMS message.
The token is valid for 30 days.

By default, a link to Webforms is sent.
When `for_mobile_app = true` param is provided, a deep link to mobile app is generated instead.

#### PUT body parameters

Parameter | Type | Required? | Default | Description
--------- | ----------- | ------ | ------ | ------
for_mobile_app | boolean | no | false | Should a deep-link to mobile app be generated?

#### Response

Always returns `204 (No content)`

<aside class="notice">
Requires <code>BL:Api:Members:Msisdns:Verify</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-verify-msisdn"></a> Verify MSISDN

> Example:

```shell
curl -X PUT \
  "https://bpc-api.boostcom.no/v3/infinity-mall/members/channels/msisdn/4740485124/verify" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
    '{
      "token": "shNWPezzYoIuJV2qDIcQrdFQqUdlVdquwUJrNb2Nuw"
    }'
```

> When successful, returns an empty object:

```json
{
  // Empty object
}
```

**PUT** `/v3/:loyalty_club_slug/members/channels/msisdn/:msisdn/verify`

Uses given token (sent with [Members &bull; Send MSISDN verification SMS](#members-send-verification-sms)) to verify
user identified with given MSISDN.

#### PUT body parameters

Parameter | Type | Required? | Description
--------- | ----------- | ------ | ------
token | string | yes | Token generated and sent with [Members &bull; Send MSISDN verification SMS](#members-send-verification-sms)

#### Error responses

Status | Reason
--------- | ----------- 
`400` | Token missing
`463` | Invalid token (it may not exist, or not match the user or be expired)

<aside class="notice">
Requires <code>BL:Api:Members:Msisdns:Verify</code> permit
</aside>
