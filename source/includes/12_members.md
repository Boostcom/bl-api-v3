# Endpoints &bull; Members

## <a name="v3-members-public-info"></a> Get public info

> Example:

```shell
curl "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/:id/public_info" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> Returns hash structured like this:

```json
{
  "exists": true, 
  "can_login": true 
}
```

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/:id/public_info`

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/by_msisdn/:msisdn/public_info`

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/by_email/:email/public_info`

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
can_login | boolean | Is this member able to sign? 

<aside class="notice">
Requires <code>BL:Api:Members:Check</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-get"></a> Get

> Example:

```shell
curl "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/:id" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> When successful, the above command returns member object as depicted [here](#v3-member-model)

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/:id`

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/by_msisdn/:msisdn`

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/by_email/:email`

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

<aside class="notice">
Requires <code>BL:Api:Members:Get</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-create"></a> Create

> Example:

```shell

curl -X POST \
  https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members \
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

**POST** `api/v3/loyalty_clubs/:loyalty_club_slug/members`

Create member with given properties.

Available properties and their validation rules are defined by Loyalty Club schema (see: [here](#v3-loyalty-clubs-schema)).

Actual welcome messages sending depends on Loyalty Club and Product configuration.
For example, even if send_email_welcome_message:true param is provided, message may not be sent because either Product 
or Loyalty has disabled welcome messages or Loyalty Club has no e-mails configured at all.

There is also a possibility to have multiple SMS welcome messages sent. The one that matches Product or default one will be sent.

### Headers

Header name | Required? | Description
------ | --------- | -----------
X-Subproduct-Name | no | Additional source/optin channel information

### POST Parameters (JSON)

Parameter | Required? | Default | Description | Type
--------- | ----------- | ----------- | --------- | -----------
properties | **yes** | none | JSON with properties for member | JSON Object
properties\['language'\] | no | "default_language" from schema | Language used by user | string
properties\['msisdn'\] | yes* | none | Unique member's msisdn as defined [here](#msisdn-member-identifier)) Example: `4740485124`.| string
properties\['email'\] | yes* | none | Member's email | string
password | no | none | Member's password. Not required, but user won't be able to log in without this | string
sms_enabled | no | true | Should SMS channel be enabled for member? | Boolean
email_enabled | no | true | Should email channel be enabled for member? | Boolean
push_enabled | no | true | Should push channel will be enabled for member? | Boolean
send_sms_welcome_message | no | true | Should SMS welcome message be sent to member? | Boolean
send_email_welcome_message | no | true | Should email welcome be sent to member | Boolean

&ast; At least one of those properties must be provided

### Response (JSON object)

Created member properties - see: [Member model](#v3-member-model)

### Error responses

Status | Reason
--------- | ----------- 
`422` | [validation errors](#validation-on-members) JSON object.

<aside class="notice">
Requires <code>BL:Api:Members:Create</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-update"></a> Update

> Example:

```shell

curl -X PUT \
  https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/:id \
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

**PUT** `api/v3/loyalty_clubs/:loyalty_club_slug/members/:id`

Update member's properties with given ones.

It is intended for partial updates - not given properties are neither deleted nor overwritten.

If deleting attribute is intended, it's value should be sent as `null`.

This endpoint may return validation errors (`422`) because of current member data invalidity 
as loyalty club schema may get changed over time and make existing users invalid.   

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
id | Member's ID | integer

### PUT Parameters (JSON)

Parameter | Description | Type
--------- | --------- | -----------
properties | JSON with properties for member | JSON Object
password | Member's password | string
sms_enabled | Should SMS channel be enabled for member? | Boolean
email_enabled | Should email channel be enabled for member? | Boolean
push_enabled | Should push channel will be enabled for member? | Boolean

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

**DELETE** `api/v3/loyalty_clubs/:loyalty_club_slug/members/:id`

Permanently removes member.

It is possible to trigger optout message by setting `send_unsubscribe_message=true` query string parameter.
Same as welcome messages, optout messages sending also depends on Loyalty Club and Product configuration.

> Example:

```shell
curl -X DELETE \
    "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/:id" \
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

## <a name="v3-members-reset-password"></a> Reset password

> Example:

```shell
curl -X PUT \
  "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/:loyalty_club_slug/members/by_email/:email/reset_password" \
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

**PUT** `/api/v3/loyalty_clubs/:loyalty_club_slug/members/by_email/:email/reset_password`

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
curl "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/by_email/joe@example.com/send_password_reset_token" \
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

**POST** `/api/v3/loyalty_clubs/:loyalty_club_slug/members/by_email/:email/send_password_reset_token`

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

## <a name="v3-members-send-one-time-password"></a> Send one time password

> Example:

```shell
curl "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/by_msisdn/4740485124/send_one_time_password" \
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

**POST** `/api/v3/loyalty_clubs/:loyalty_club_slug/members/by_msisdn/:msisdn/send_one_time_password`

Sends an SMS to given msisdn if it is associated with member in given loyalty club.

The SMS contains a token generated for member, valid for 10 minutes.

@todo: Describe signing in using the OTP 

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
msisdn | Member's msisdn | string (format as defined [here](#msisdn-member-identifier) - example: `4740485124`)

<aside class="notice">
Requires <code>BL:Api:Members:CreateOneTimePassword</code> permit
</aside>

<!--- ############################################################################################################# --->

## <a name="v3-members-verify-token"></a> Verify token

> Example:

```shell
curl "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/by_email/joe@example.com/verify_token/password_reset/k4wort03j2" \
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

**GET** `/api/v3/loyalty_clubs/:loyalty_club_slug/members/by_email/:email/verify_token/:type/:token`

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
