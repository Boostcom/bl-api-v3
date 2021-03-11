## Policies

### <a name="tokens-admin-policy-model"></a> Policy model

> Policy example:

```json
{
  "name": "MessagesApiRegularAccess",
  "description": null,
  "internal": false,
  "for_loyalty_club_types": null,
  "taint_produced_permits_with": "NONE",
  "permits": [
    "Messages:Api:Bulks:Process",
    "Messages:Api:Dispatches:Index",
    "Messages:Api:Messages:Create",
    "Messages:Api:Messages:Get",
    "Messages:Api:Messages:Index",
    "Messages:Api:Messages:Update",
    "Messages:Api:Sendings:Create",
    "Messages:Api:Sendings:Get",
    "Messages:Api:Sendings:Update",
    "Messages:Api:Settings:Show"
  ]
}
```

Key | Type | Description
--------- | ------ | ------
name | string | 
description | string |
internal | boolean |
for_loyalty_club_types | string[] |
taint_produced_permits_with | string |
permits | string[] |

### <a name="tokens-admin-index-policies"></a> List Policies

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/tokens/policies" \
  -H 'content-type: application/json' \
  -H 'Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [policies](#tokens-admin-policy-model) 

```json
{
  "policies": [] // List of policies - see 'Policy model'
}
````

**GET** `v1/tokens/policies`

Returns list of [Policies](#tokens-admin-policy-model).

#### Response (JSON object)

Key | Type 
--------- | ---------
policies | [Policies](#tokens-admin-policy-model)[]

<aside class="notice">
Requires <code>Policies:Api:Policies:Index</code> policy
</aside>
