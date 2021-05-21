## <a name="operations-app-tokens"></a>Push App Tokens

### <a name="operations-app-token-payload"></a> App Token Payload model

> App Token Payload example:

```json
{
  "token": "token",
  "platform": "ios",
  "firebase_project_slug": "slug"
}
```

Key | Type | Description
--------- | ------ | ------
token | string | 
platform | string (`ios`, `android`) | App platform
firebase_project_slug | string | Firebase project slug

### <a name="operations-add-users-app-tokens"></a> Add app tokens

> Example:

```shell

curl -X POST \
  https://api.mpc.placewise.com/v1/users/me/app_tokens \
  -H 'content-type: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
  -d '{
	    "token": "token",
	    "platform": "ios",
        "firebase_project_slug": "project slug"
    }'

```

> When successful, returns 200

**POST** `v1/users/me/app_tokens`

<aside class="notice">
Requires <code>UsersAPI:Users:AppTokens:Create</code> permit
</aside>

#### Request parameters (JSON) 
[App Token Payload] (#operations-app-token-payload) 

#### Response (JSON object)

Empty object {}

##### Error responses

Status | Reason
--------- | ----------- 
`404` | User with given token could not found 
`422` | Invalid parameters
