## <a name="oauth2"></a> Members OAuth

> Example header: `authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522`

Actions related to specific member (the one that is using your application) require to have member authenticated and we implement
OAuth2 flow for this.

To authorize those actions, we **require** `authorization` header that should contain: `Bearer :access_token`. 

Look at [OAuth Token &bull; Create](#token-create) to see how to obtain the :access_token.

### <a name="token-create"></a> Create token

> Create token example:

```shell
curl -X POST "https://bpc-api.boostcom.no/v3/infinity-mall/members/oauth/token" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '{
      "grant_type": "password",
      "identifier_type": "id",
      "identifier": 42,
      "password": "123"
    }'
```

> Refresh token example:

```shell
curl -X POST "https://bpc-api.boostcom.no/v3/infinity-mall/members/oauth/token" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '{
  	  "grant_type": "refresh_token",
  	  "refresh_token": "36c636e4290d28488a13691afce351397bec21b1246c2c7896a8262d9bfbc4c4"
    }'
```

> When successful, both commands return JSON structured like this:

```json
{
  "access_token": "af9e5361cd7e083dfa4132df3ea7ab82fac21496991632a9994a8c2a9f33884f",
  "token_type": "bearer",
  "expires_in": 86400,
  "refresh_token": "36c636e4290d28488a13691afce351397bec21b1246c2c7896a8262d9bfbc4c4",
  "created_at": 1506523094,
  "resource_owner_id": 42
}
```

**POST** `v3/:loyalty_club_slug/members/oauth/token`

Creates a new access token by one of two methods (grant types):

* `password` - token is issued by providing user credentials
* `refresh_token` - token is issued by providing refresh token obtained from `password` grant type 

#### Creating token (`password` grant)

When creating new access token, `"grant_type": "password"` should be given along with member credentials (see example on the right).

In response, two tokens are returned:

* `access_token` that is valid for **24 hours** - may be used to authenticate member in member-related actions (see: [OAuth2](#oauth2)).
* `refresh_token` which is valid for **1 year** - may be used to get a new access token (with `refresh_token` grant)

Also, `resource_owner_id` is returned, is an ID of member that the token has been issued for. 

#### Refreshing token (`refresh_token` grant)

You can obtain a new token after (or before) it's expiration time, by using `refresh_token` grant.

Param `grant_type: "refresh_token"` must be provided along with `refresh_token: ":refresh_token"`  (see example on the right).

It returns a token response, same as for `password` grant, but with new tokens.

#### POST Parameters (JSON object)

Parameter | Description | Type | For grant type
--------- | ------- | ----- | ----- 
grant_type | type of grant | enum: `password`, `refresh_token` | -
identifier_type | identifier type that user should be retrieved by | enum: `id`, `email`, `msisdn` | `password` 
identifier | value of member identifier | mixed (e.g. `134123123`, `+47123456789` or `alice@example.com`) | `password`
password | member password, One-Time-Password or Registration Password | string | `password`
refresh_token | refresh token | string | `access_token`

#### Response (JSON object)

Key | Description | Type
--------- | ----------- | ---------
access_token | Token that member can be authenticated with | string
token_type | Always "bearer" | string
expires_in | Seconds for how long token will be valid | integer (seconds)
refresh_token | Token that may be used to issue a new :access_token | string
created_at | When the token has been created | integer (timestamp)
resource_owner_id | ID of member that the token has been for | integer

#### Error responses

Status | Reason
--------- | ----------- 
`461` | Invalid member credentials provided for `password` grant. Either member could not be found or password is wrong
`462` | Invalid refresh_token provided for `refresh_token` grant (may be expired)

<aside class="notice">
Requires <code>BL:Api:Members:OAuth</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="token-revoke"></a> Revoke token

> Example:

```shell
curl -X POST "https://bpc-api.boostcom.no/v3/infinity-mall/members/oauth/revoke" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '{
      "token": "36c636e4290d28488a13691afce351397bec21b1246c2c7896a8262d9bfbc4c4" 
    }'
```

> Always returns an empty JSON object

```json
{
  // Empty object
}
```

**POST** `v3/:loyalty_club_slug/members/oauth/revoke`

Revokes a token (access or refresh). 

Always returns an empty JSON object (even if given token is invalid).

#### POST Parameters (JSON)

Parameter | Description | Type
--------- | ------- | -------
token | access or refresh token obtained from [OAuth Token &bull; Create](#token-create) | string

<aside class="notice">
Requires <code>BL:Api:Members:OAuth</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="token-info"></a> Get token info

> Example:

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/members/oauth/token/info" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522'

```

> When successful, return JSON structured like this:

```json
{
    "resource_owner_id": 42, // ID of member that the token has been issued for
    "scopes": [],
    "expires_in_seconds": 73614, // Seconds until expiration
    "application": {
        "uid": null
    },
    "created_at": 1506516784
}
```

**GET** `v3/:loyalty_club_slug/members/oauth/token/info`

Returns info of given token (not refresh token) from `authorization` header.

#### Response (JSON object)

Key | Description | Type
--------- | ----------- | ---------
resource_owner_id | ID of member that the token has been for | integer
scopes | Not implemented | Array<string>
expires_in_seconds | Seconds for how long token will be valid | integer (seconds)
application | Not implemented | Object
created_at | When the token has been created | integer (timestamp)

#### Error responses

Status | Reason
--------- | ----------- 
`460` | Token from `authorization` is invalid (or expired)

<aside class="notice">
Requires <code>BL:Api:Members:OAuth</code> permit
</aside>

<!--- ############################################################################################################# --->
