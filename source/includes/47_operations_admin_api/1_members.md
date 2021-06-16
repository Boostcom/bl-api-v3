## <a name="operations-admin-members"></a> Members⠀

This section describes Member management endpoints that are specific to Operations API.

### <a name="operations-admin-members-delete"></a> Delete contact

> Example:

```shell
curl -X GET \
  "https://api.mpc.placewise.com/v1/operations/members/12345" \
  -H 'content-type: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, returns an empty object:

```json
{}
```

**GET** `/v1/operations/members/:member_id`

Delete contact/member. It will delete user at the same time if contact has MPC account


#### Error responses

Status | Reason
--------- | ----------- 
`403` | Not authorized to perform this action
`470` | Some of required header is missing
`404` | Member not found

<aside class="notice">
Requires <code>Operations:Api:Members:Destroy</code> permit
</aside>