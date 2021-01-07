## <a name="operations-admin-members"></a> Members⠀

This section describes Member management endpoints that are specific to Operations API.

### <a name="members-stores-get"></a> Get id of stores assigned to member

> Example:

```shell
curl -X GET \
  "https://api.mpc.placewise.com/v3/infinity-mall/members/12345/stores" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, returns an empty object:

```json
{
  "stores_ids": [1, 2]
}
```

**GET** `/v3/:loyalty_club_slug/members/:member_id/stores`

Show id of stores assigned to member

<aside class="notice">
Requires <code>BL:Api:Members:Stores:Index</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-stores-add"></a> Assign member to stores

> Example:

```shell
curl -X POST \
  "https://api.mpc.placewise.com/v3/infinity-mall/members/12345/stores" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
    '{
      "stores_ids": [1, 2]
    }'
```

> When successful, returns an empty object:

```json
{
  "stores_ids": [1, 2]
}
```

**POST** `/v3/:loyalty_club_slug/members/:member_id/stores`

Assing stores to member using stores ids

#### POST body parameters

Parameter | Type | Required? | Description
--------- | ----------- | ------ | ------
stores_ids | array | yes | Id of stores

#### Error responses

Status | Reason
--------- | ----------- 
`400` | Stores ids missing

<aside class="notice">
Requires <code>BL:Api:Members:Stores:Create</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-stores-remove"></a> Remove stores to member

> Example:

```shell
curl -X DELETE \
  "https://api.mpc.placewise.com/v3/infinity-mall/members/12345/stores" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
    '{
      "stores_ids": [1, 2]
    }'
```

> When successful, returns an empty object:

```json
{
  "stores_ids": [3]
}
```

**DELETE** `/v3/:loyalty_club_slug/members/:member_id/stores`

Remove stores assigned from member

#### DELETE body parameters

Parameter | Type | Required? | Description
--------- | ----------- | ------ | ------
stores_ids | array | yes | Id of stores

#### Error responses

Status | Reason
--------- | ----------- 
`400` | Stores ids missing

<aside class="notice">
Requires <code>BL:Api:Members:Stores:Destroy</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-user-create"></a> Transform member to user

> Example:

```shell
curl -X POST \
  "https://api.mpc.placewise.com/v3/infinity-mall/members/12345/user" \
  -H 'content-type: application/json' \
  -H 'Authorization: Bearer B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d \
    '{
      "role": "role"
    }'
```

> When successful, returns an empty object:

```json
{
  "user_id": "created-user-id"
}
```

**POST** `/v3/:loyalty_club_slug/members/:member_id/user`

Transform member (tenant contact) to user and return user id

#### POST body parameters

Parameter | Type | Required? | Description
--------- | ----------- | ------ | ------
role | string | yes | role

#### Error responses

Status | Reason
--------- | ----------- 
`400` | Role missing

<aside class="notice">
Requires <code>BL:Api:Members:User:Create</code> permit
</aside>
