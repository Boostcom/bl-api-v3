# Endpoints &bull; Members

## <a name="v3-members-index"></a> List

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members?per_page=100&page=1&ids[]=1&ids[]=2" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> Returns hash structured like this:

```json
{
  "members": [], // List of members - See: "General Info -> Member JSON model"
  "pagination_info": {
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
}

```

**GET** `v3/:loyalty_club_slug/members?per_page=:per_page&page=:page&ids[]=1&ids[]=2`

Returns paginated list of all Loyalty Club members, sorted by `created_at ASC`.

Every returned member is represented by [Member model](#v3-member-model).

### Query Parameters

Parameter | Type | Required? | Default | Description
--------- | ----------- | ----------- | --------- | -----------
per_page | integer | no | 1000 | Number of results to be returned per request (1000 is the maximum)
page_no | integer | no | 1 | Number of results page
ids | Array<integer> | no | null | IDs of members to return 

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members | Array<Member> | Array of [Members](#v3-member-model)
pagination_info | Object | [Pagination](#v3-pagination-model) object

<aside class="notice">
Requires <code>BL:Api:Members:Index</code> permit
</aside>

### Error responses

Status | Reason
--------- | ----------- 
`400` | :per_page param exceeds the limit

<!--- ############################################################################################################# --->

## <a name="v3-members-public-info"></a> Get public info

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/:id/public_info" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
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


### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer
msisdn | Member's msisdn | string (format as defined [here](#msisdn-member-identifier) - example: `4740485124`)
email | Member's email | string (email)

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
exists | boolean | Does member with given identifier exist in given Loyalty Club? 
can_login | boolean | Is this member able to sign? (deprecated)
available_identifiers | string[] | Identifiers that are set on member
has_password | boolean | Was password set by member?

<aside class="notice">
Requires <code>BL:Api:Members:Check</code> permit
</aside>

### Error responses

Status | Reason
--------- | ----------- 
`422` | Invalid MSISDN param

<!--- ############################################################################################################# --->

## <a name="v3-members-person-id"></a> Get person id

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/:id/person_id" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
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

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer
msisdn | Member's msisdn | string (format as defined [here](#msisdn-member-identifier) - example: `4740485124`)
email | Member's email | string (email)

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
success | boolean | If query was successful?
source | string | Source of `person_id` (possible values are: `not_found`, `db`, `storage`)
person_id | integer | Member's `person_id`

<aside class="notice">
Requires <code>BL:Api:Members:Check</code> permit
</aside>

### Error responses

Status | Reason
--------- | ----------- 
`422` | Invalid MSISDN param

<!--- ############################################################################################################# --->


## <a name="v3-members-get"></a> Get

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/:id" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> When successful, the above command returns member object as depicted [here](#v3-member-model)

**GET** `v3/:loyalty_club_slug/members/:id`

**GET** `v3/:loyalty_club_slug/members/by_msisdn/:msisdn`

**GET** `v3/:loyalty_club_slug/members/by_email/:email`

Returns member by one of three identifier types: `id`, `member` or `email`

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer
msisdn | Member's msisdn | string (format as defined [here](#msisdn-member-identifier) - example: `4740485124`)
email | Member's email | string (email)

### Response (JSON object)

See: [Member model](#v3-member-model)

### Error responses

Status | Reason
--------- | ----------- 
`404` | Member with given identifier could not found 
`422` | Invalid MSISDN param

<aside class="notice">
Requires <code>BL:Api:Members:Get</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-create"></a> Create

> Example:

```shell

curl -X POST \
  https://bpc-api.boostcom.no/v3/infinity-mall/members \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: facebook' \
  -H 'X-Subproduct-Name: campaign-10-2017' \
  -H 'X-User-Agent: CURL manual test' \
  -d '{
	"properties": {
		"email": "dev+6@test.com",
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

> When successful, the above command returns created member object as depicted [here](#v3-member-model)

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

Available properties and their validation rules are defined by Loyalty Club schema (see: [here](#v3-loyalty-clubs-schema)).

Actual welcome messages sending depends on Loyalty Club and Product configuration.
For example, even if send_email_welcome_message:true param is provided, message may not be sent because either Product 
or Loyalty has disabled welcome messages or Loyalty Club has no e-mails configured at all.

There is also a possibility to have multiple SMS welcome messages sent. The one that matches Product or default one will be sent.

### <a name="v3-members-create-registration-password"></a> Registration Password

Some API clients (depends on Permit assigned to given `X-Client-Authorization` token) may be required to provide 
valid `registration_password` param that is sent to user with [Members &bull; Send registration password](#v3-members-send-registration-password) 

### Headers

Header name | Required? | Description
------ | --------- | -----------
X-Subproduct-Name | no | Additional source/optin channel information

### <a name="v3-members-create-post-parameters"></a> POST Parameters (JSON)

Parameter | Required? | Default | Description | Type
--------- | ----------- | ----------- | --------- | -----------
properties | **yes** | none | JSON with properties for member | JSON Object
properties\['language'\] | no | "default_language" from schema | Language used by user | string
properties\['msisdn'\] | yes* | none | Unique member's msisdn as defined [here](#msisdn-member-identifier)) Example: `4740485124`.| string
properties\['email'\] | yes* | none | Member's email | string
consents | no | {} | Member's consents (similar to [Member's consents JSON model](#v3-member-consents-model)) | JSON Object
sms_enabled | no | true | Should SMS channel be enabled for member? | Boolean
email_enabled | no | true | Should email channel be enabled for member? | Boolean
push_enabled | no | true | Should push channel be enabled for member? | Boolean
send_sms_welcome_message | no | true | Should SMS welcome message be sent to member? | Boolean
send_email_welcome_message | no | true | Should email welcome be sent to member | Boolean
password | depends | none | Member's password. Not required, but user won't be able to log in without this when only its email is provided | string
registration_password | depends | none | Password for registration for member MSISDN verification, see [above](#v3-members-create-registration-password)

&ast; At least one of those properties must be provided
 
### Response (JSON object)

Created member properties - see: [Member model](#v3-member-model)

### Error responses

Status | Description
--------- | ----------- 
`422` | [validation errors](#validation-on-members) JSON object.
`465` | `registration_password` or `password` param is missing
`466` | `registration_password` param is invalid

<aside class="notice">
Requires <code>BL:Api:Members:Create</code> or <code>BL:Api:Members:CreateWithVerification</code>permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-update"></a> Update

> Example:

```shell

curl -X PUT \
  https://bpc-api.boostcom.no/v3/infinity-mall/members/:id \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
  -d \
  '{
    "properties": {
      "last_name": "Doge"
    },
    "password": "new_password"
  }'
```

> When successful, the above command returns updated member object as depicted [here](#v3-member-model)

> When payload is invalid, validation errors as depicted [here](#v3-members-create)

**PUT** `v3/:loyalty_club_slug/members/:id`

Update member's properties, consents and other with given ones.

It is intended for partial updates:
* not given properties are neither deleted nor overwritten,
* not given attributes (like consents) won't be changed in any way.

If deleting properties is intended, their value should be sent as `null`.

This endpoint may return validation errors (`422`) even for current member properties. 
That's because loyalty club schema may get changed over time which results in invalidating existing users.

To bypass such validation errors, you may provide `validate_partially: true` param which will validate and update only 
attributes that were provided in payload. 

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer

### PUT Parameters (JSON)

Parameter | Description | Type | Default
--------- | --------- | ----------- | -----------
properties | JSON with properties for member | JSON Object | null
consents | Member's consents (similar to [Member's consents JSON model](#v3-member-consents-model)) | JSON Object | null
password | Member's password | string | null
sms_enabled | Should SMS channel be enabled for member? | Boolean | null
email_enabled | Should email channel be enabled for member? | Boolean | null
push_enabled | Should push channel will be enabled for member? | Boolean | null
validate_partially | Should only provided data be validated? | Boolean | false

### Response (JSON object)

Member properties after update - see: [Member model](#v3-member-model)

### Error responses

Status | Description
--------- | ----------- 
`404` | Member could not be found
`422` | [validation errors](#validation-on-members) JSON object.

<aside class="notice">
Requires <code>BL:Api:Members:Update</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-destroy"></a> Destroy

**DELETE** `v3/:loyalty_club_slug/members/:id`

Permanently removes member.

It is possible to trigger optout message by setting `send_unsubscribe_message=true` query string parameter.
Same as welcome messages, optout messages sending also depends on Loyalty Club and Product configuration.

> Example:

```shell
curl -X DELETE \
    "https://bpc-api.boostcom.no/v3/infinity-mall/members/:id" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful, the above command returns destroyed member object as depicted [here](#v3-member-model)

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer

### Query Parameters

Parameter | Required? | Default | Description | Type
--------- | ----------- | ----------- | --------- | -----------
send_email_unsubscribe_message | no | true | Should optout email be sent to member? | Boolean

### Response (JSON object)

Properties of member that has been destroyed - see: [Member model](#v3-member-model)

### Error responses

Status | Reason
--------- | ----------- 
`404` | Member could not be found
`422` | [validation errors](#validation-on-members) JSON object.

<aside class="notice">
Requires <code>BL:Api:Members:Destroy</code> permit
</aside>


<!--- ############################################################################################################# --->


## <a name="v3-members-validate"></a> Validate

> Example:

```shell

curl -X POST \
  https://bpc-api.boostcom.no/v3/infinity-mall/members/validate \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: android-app' \
  -H 'X-Subproduct-Name: campaign-10-2017' \
  -H 'X-User-Agent: CURL manual test' \
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

Validates if given data is valid for new member registration.

Only properties which keys are present in given payload, will be validated against presence validation.

For example, let's say that Loyalty Club configuration requires member to have `msisdn` property set.

* if `"msisdn": ""` or`"msisdn": null`" is present in the payload, the validation will return error stating that `msisdn` must be present.
* however, when payload doesn't include the `msisdn` key, such error will not be returned.

This behaviour has been designed to allow validate member partially, property by property. This way you can choose 
what values should be validated at specific point of registration.

### <a name="v3-members-validate-post-parameters"></a> POST Parameters (JSON)

Following keys of [Members &bull; Create](#v3-members-create-post-parameters) payload are supported when validating:

* properties
* consents
* registration_password

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
valid | boolean | Is the data valid?
errors | object | [validation errors](#validation-on-members) JSON object, `null` when data is valid

<aside class="notice">
Requires <code>BL:Api:Members:Validate</code> permit
</aside>

## <a name="v3-members-reset-password"></a> Reset password

> Example:

```shell
curl -X PUT \
  "https://bpc-api.boostcom.no/v3/:loyalty_club_slug/members/by_email/:email/reset_password" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
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

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
password | Member's new password | string
token | Confirmation token generated and sent with [Members &bull; Send password reset token](#v3-members-send-password-reset-token) | string

### Error responses

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

## <a name="v3-members-send-password-reset-token"></a> Send password reset token

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_email/joe@example.com/send_password_reset_token" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
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

The token can be then used to reset password with [Members &bull; Reset password](#v3-members-reset-password).
It also may be verified with [Members &bull; Verify token](#v3-members-verify-token)

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
email | Member's email | string (email)

<aside class="notice">
Requires <code>BL:Api:Members:Tokens:Create</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-send-one-time-password"></a> Send one time password SMS

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_msisdn/4740485124/send_one_time_password" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
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

This password then can be used to sign in, just as "regular" member password - see:  [OAuth Token &bull; Create](#v3-token-create).

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
msisdn | Member's msisdn | string (format as defined [here](#msisdn-member-identifier) - example: `4740485124`)

<aside class="notice">
Requires <code>BL:Api:Members:CreateOneTimePassword</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-send-one-time-password-email"></a> Send one time password E-mail

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_email/user@example.com/send_one_time_password" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
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

Those params then can be used to sign in the user, just as "regular" member password - see:  [OAuth Token &bull; Create](#v3-token-create).

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
email | Member's email | e-mail

### Error responses

Status | Reason
--------- | ----------- 
`422` | Invalid e-mail param

<aside class="notice">
Requires <code>BL:Api:Members:CreateOneTimePassword</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-send-registration-password"></a> Send registration password

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_msisdn/4740485124/send_registration_password" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
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

This password may be required for member registration - see: [Registration password](#v3-members-create-registration-password)

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
msisdn | MSISDN | string (format as defined [here](#msisdn-member-identifier) - example: `4740485124`)

### POST Parameters

Parameter | Description | Type
--------- | ----------- | ------
language | Language of the registration password message. When not provided, default LC language will be used | string

### Error responses

Status | Reason
--------- | ----------- 
`422` | Invalid MSISDN param

<aside class="notice">
Requires <code>BL:Api:Members:CreateRegistrationPassword</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-verify-token"></a> Verify token

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/by_email/joe@example.com/verify_token/password_reset/k4wort03j2" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> Example response:

```json
{
  "valid": true
}
```

**GET** `/v3/:loyalty_club_slug/members/by_email/:email/verify_token/:type/:token`

Verifies given member token. Returns response indicating if given token is (still) valid.

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
email | Member's email | string (email)
type | Token type | `password_reset` (more types in future) 
token | Token | string

### Response (JSON object)

Key | Type
--------- | ---------
valid | boolean

### Error responses

Status | Reason
--------- | ----------- 
`400` | Invalid `type`

<aside class="notice">
Requires <code>BL:Api:Members:Tokens:Verify</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-send-verification-sms"></a> Send MSISDN verification SMS

> Example:

```shell
curl -X PUT \
  "https://bpc-api.boostcom.no/v3/infinity-mall/members/channels/msisdn/4740485124/send_verification" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
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

### PUT body parameters

Parameter | Type | Required? | Default | Description
--------- | ----------- | ------ | ------ | ------
for_mobile_app | boolean | no | false | Should a deep-link to mobile app be generated?

### Response

Always returns `204 (No content)`

<aside class="notice">
Requires <code>BL:Api:Members:Msisdns:Verify</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-verify-msisdn"></a> Verify MSISDN

> Example:

```shell
curl -X PUT \
  "https://bpc-api.boostcom.no/v3/infinity-mall/members/channels/msisdn/4740485124/verify" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
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

Uses given token (sent with [Members &bull; Send MSISDN verification SMS](#v3-members-send-verification-sms)) to verify
user identified with given MSISDN.

### PUT body parameters

Parameter | Type | Required? | Description
--------- | ----------- | ------ | ------
token | string | yes | Token generated and sent with [Members &bull; Send MSISDN verification SMS](#v3-members-send-verification-sms)

### Error responses

Status | Reason
--------- | ----------- 
`400` | Token missing
`463` | Invalid token (it may not exist, or not match the user or be expired)

<aside class="notice">
Requires <code>BL:Api:Members:Msisdns:Verify</code> permit
</aside>
