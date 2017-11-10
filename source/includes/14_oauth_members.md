# Endpoints &bull; Me

## <a name="v3-oauth2"></a> Introduction

> Example header: `Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522`

Actions related to specific member (the one that is using your application) require to have member authenticated and we implement
OAuth2 flow for this.

To authorize those actions, we **require** `Authorization` header that should contain: `Bearer :access_token`. 

Look at [OAuth Token &bull; Create](#v3-token-create) to see how to obtain the :access_token.

## <a name="v3-me-get"></a> Get

> Example:

```shell
curl "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/me" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
  -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522'
```

> Responses as same as in [Members &bull; Get](#v3-members-get)

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/me`

Returns "current" member - the one that given Authorization token has been issued for.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

### Response (JSON object)

See: [Member model](#v3-member-model)

### Error responses

Status | Reason
--------- | ----------- 
`404` | Member associated with given Authorization token does not exist
`460` | Member not authorized (Invalid or expired OAuth token)

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:Get</code> permit.
</aside>

## <a name="v3-me-update"></a> Update

> Example:

```shell
curl -X PUT \
  https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/me \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
  -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
  -d \
  '{
	  "properties": {
		  "last_name": "Doge"
	  },
	  "password": "new_password"
  }'
```

> Responses are same as in [Members &bull; Update](#v3-members-update)

**PUT** `api/v3/loyalty_clubs/:loyalty_club_slug/members/me`

Updates "current" member - the one that given Authorization token has been issued for.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

This endpoint may return validation errors (`422`) because of current member data invalidity 
as loyalty club schema may get changed over time and make existing users invalid.   

### Parameters

It accepts same PUT and URL parameters as in [Members &bull; Update](#v3-members-update).

### Response (JSON object)

Same as in [Members &bull; Update](#v3-members-update).

### Error responses

Status | Reason
--------- | -----------
`404` | Member associated with given Authorization token does not exist
`422` | [Validation errors](#validation-on-members) JSON object.
`460` | Member not authorized (Invalid or expired OAuth token)

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:Update</code> permit.
</aside>

## <a name="v3-me-update-password"></a> Update password

> Example:

```shell
curl -X PUT \
  https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/me/update_password \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
  -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
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

**PUT** `api/v3/loyalty_clubs/:loyalty_club_slug/members/update_password`

Updates "current" member (the one that given Authorization token has been issued for) password.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

In opposition to [Me &bull; Update](#v3-me-update), it does not validate current user data, only given password.

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ------
password | Member's new password | string
current_password | Member's current password | string

### Error responses

Status | Reason
--------- | -----------
`404` | Member associated with given Authorization token does not exist
`422` | Invalid password - returns [validation errors](#validation-on-members) JSON object.
`460` | Member not authorized (Invalid or expired OAuth token)
`464` | Invalid current_password

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:UpdatePassword</code> permit.
</aside>

## <a name="v3-me-destroy"></a> Destroy

> Example:

```shell
curl -X DELETE \
    "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/me" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test' \
    -H 'Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
```

> Response as same as in [Members &bull; Destroy](#v3-members-destroy)

**DELETE** `api/v3/loyalty_clubs/:loyalty_club_slug/members/me`

Permanently removes "current" member - the one that given Authorization token has been issued for.

As a member-related action, it requires member authorization. See [OAuth](#v3-oauth2).

### Parameters

It accepts same URL & query parameters as in [Members &bull; Destroy](#v3-members-destroy).

### Response (JSON object)

Same as in [Members &bull; Destroy](#v3-members-destroy).

### Error responses

Status | Reason
--------- | -----------
`404` | Member associated with given Authorization token does not exist
`460` | Member not authorized (Invalid or expired OAuth token)

<aside class="notice">
Requires <code>BL:Api:Members:OAuth:Destroy</code> permit.
</aside>
