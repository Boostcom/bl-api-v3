## Tokens

### <a name="tokens-admin-token-model"></a> Token model

> Token example:

```json
{
  "id": 1000010,
  "name": "Token",
  "token": "PpdfXpwqDyh7yavEwXxKTVe3",
  "permits": ["Stores:Api:Stores:Get", "Stores:Api:Categories:Get"],
  "policies": ["OffersRegularAccess", "RewardsRegularAccess"],
  "loyalty_club_ids": [235],
  "customers": ["language=pl"],
  "product": "android",
  "internal": true,
  "created_at": "2021-03-11T09:27:09.561Z",
  "updated_at": "2021-03-11T09:27:09.561Z",
  "created_by": { "entity_type": "user", "entity_id": "7db2c508-1758-4b84-bb68-50b5c8e1f458" },
  "updated_by": { "entity_type": "user", "entity_id": "7db2c508-1758-4b84-bb68-50b5c8e1f458" }
}
```

Key | Type | Description
--------- | ------ | ------
id | integer |
name | string |
token | string |
permits | string[] | Names of permits the token has
policies | string[] | Names of policies the token has
loyalty_club_ids | integer[] | List of Loyalty Clubs the token is permitted to. When null, the token is not limited to specific club(s).
customers | string | List of Customers (in CustomerParam format) the token is permitted to. When null, the token is not limited to specific customers.
product | string | API product the token is permitted to. When null, the token is not limited to specific product.
internal | boolean | Is token internal for Placewise?
created_at | datetime | Time of creation
updated_at | datetime | Time of last update
created_by | Object | Author of creation
updated_by | Object | Author of last update

### <a name="tokens-admin-token-payload-model"></a> TokenPayload model

> TokenPayload example:

```json
{
  "name": "Token",
  "permits": ["Stores:Api:Stores:Get", "Stores:Api:Categories:Get"],
  "policies": ["OffersRegularAccess", "RewardsRegularAccess"],
  "loyalty_club_ids": [235],
  "customers": ["language=pl"],
  "product": "android",
  "internal": true
}
```

Key | Type | Description
--------- | ------ | ------
name | string |
permits | string[] | Names of permits the token has
policies | string[] | Names of policies the token has
loyalty_club_ids | integer[] | List of Loyalty Clubs the token is permitted to. When null, the token is not limited to specific club(s).
customers | string | List of Customers (in CustomerParam format) the token is permitted to. When null, the token is not limited to specific customers.
product | string | API product the token is permitted to. When null, the token is not limited to specific product.
internal | boolean | Is token internal for Placewise?

### <a name="tokens-admin-index-tokens"></a> List Tokens

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/tokens" \
  -H 'content-type: application/json' \
  -H 'Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [tokens](#tokens-admin-token-model) and [pagination_info](#pagination-model)

```json
{
  "tokens": [], // List of tokens - see 'Token model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/tokens`

Returns list of [Tokens](#tokens-admin-token-model).

#### Query Parameters

Parameter       | Type        | Default   | Description
--------------  | ----------- | --------- | -----------
per_page        | integer     | 20        | Number of results to be returned per request (20 is the maximum)
page_no         | integer     | 1         | Number of results page
loyalty_club_id | integer     | null      | When true, returns only Tokens with access scoped to given LC
global          | boolean     | null      | When true, returns only Tokens that haven't access scoped by LC
internal        | boolean     | null      | When present, returns either only internal or non-internal Tokens
product         | string      | null      | When present, returns only Tokens scoped by given product
policy          | string      | null      | When present, returns only Tokens having given policy (direct or through policy)
permit          | string      | null      | When present, returns only Tokens having given policy
search          | string      | null      | When present, returns only Tokens with name matched to given search string

#### Response (JSON object)

Key | Type 
--------- | ---------
tokens | [Tokens](#tokens-admin-token-model)[]
pagination_info | [PaginationInfo](#pagination-model)

<aside class="notice">
Requires <code>Tokens:Api:Tokens:Index</code> permit
</aside>

### <a name="tokens-admin-show-token"></a> Get Token

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/tokens/5" \
  -H 'content-type: application/json' \
  -H 'Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [token](#tokens-admin-token-model)

```json
{
  "token": {} // see: 'Token model'
}
````

**GET** `v1/tokens/:id_or_token`

Returns given [Token](#tokens-admin-token-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | mixed (integer or string) | Token ID or token

#### JSON resposne

Key | Type
---------- | --------
token | [Token](#tokens-admin-token-model)

#### Error responses

Status    | Description
--------- | -----------
`404`     | Token not found

<aside class="notice">
Requires <code>Tokens:Api:Tokens:Show</code> permit
</aside>

### <a name="tokens-admin-create-token"></a> Create Token

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/tokens" \
  -H 'content-type: application/json' \
  -H 'Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "token": {
          "name": "Token",
          "permits": ["Stores:Api:Stores:Get", "Stores:Api:Categories:Get"],
          "policies": ["OffersRegularAccess", "RewardsRegularAccess"],
          "loyalty_club_ids": [235],
          "customers": ["language=pl"],
          "product": "android",
          "internal": true
        }
    }
  '
```

> When successful, returns object containing created [token](#tokens-admin-token-model)

```json
{
  "token": {} // Token - see: 'Token model'
}
```

**POST** `v1/tokens`

Creates a new [Token](#tokens-admin-token-model).

#### POST Parameters (JSON)

Key | Type
----- | ----
token | [TokenPayload](#tokens-admin-token-payload-model)

#### Response (JSON object)

Key | Type
---------- | --------
token | [Token](#tokens-admin-token-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Tokens:Api:Tokens:Create</code> permit
</aside>

### <a name="tokens-admin-update-token"></a> Update Token

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/tokens/12" \
  -H 'content-type: application/json' \
  -H 'Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "token": {
            "name": "Token",
            "permits": ["Stores:Api:Stores:Get", "Stores:Api:Categories:Get"],
            "policies": ["OffersRegularAccess", "RewardsRegularAccess"],
            "loyalty_club_ids": [235],
            "customers": ["language=pl"],
            "product": "android",
            "internal": true
        }
    }
  '
```

> When successful, returns object containing updated [token](#tokens-admin-token-model)

```json
{
  "token": {} // Token - see 'Token model'
}
```

**PUT** `v1/tokens/:id`

Updates the [Token](#tokens-admin-token-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | mixed (integer or string) | Token ID or token

#### POST Parameters (JSON)

Key | Type
----- | ----
token | [TokenPayload](#tokens-admin-token-payload-model)

#### Response (JSON object)

Key | Type
---------- | --------
token | [Token](#tokens-admin-token-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Tokens:Api:Tokens:Update</code> permit
</aside>

### <a name="tokens-admin-destroy-token"></a> Destroy Token

> Example

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v1/tokens/12 \
  -H 'content-type: application/json' \
  -H 'Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, returns object containing destroyed [token](#tokens-admin-token-model)

```json
{
  "token": {} // Token - see: 'Token model'
}
```

**DELETE** `v1/tokens/:id`

Destroys the [Token](#tokens-admin-token-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | mixed (integer or string) | Token ID or token

#### Response (JSON object)

Key | Type 
---------- | --------
token | [Token](#tokens-admin-token-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Token not found

<aside class="notice">
Requires <code>Tokens:Api:Tokens:Destroy</code> permit
</aside>
