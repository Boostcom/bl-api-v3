## Permits

### <a name="tokens-admin-permit-model"></a> Permit model

> Permit example:

```json
{
  "name": "Rewards:Api:Program:GetInfo",
  "description": "Allows to see Rewards Program Info",
  "internal": false,
  "required_customer_features": []
}
```

Key | Type | Description
--------- | ------ | ------
name | string | 
description | string |
internal | boolean |
required_customer_features | string[] |

### <a name="tokens-admin-index-permits"></a> List Permits

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/tokens/permits" \
  -H 'content-type: application/json' \
  -H 'Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [permits](#tokens-admin-permit-model) 

```json
{
  "permits": [] // List of permits - see 'Permit model'
}
````

**GET** `v1/tokens/permits`

Returns list of [Permits](#tokens-admin-permit-model).

#### Response (JSON object)

Key | Type 
--------- | ---------
permits | [Permits](#tokens-admin-permit-model)[]

<aside class="notice">
Requires <code>Permits:Api:Permits:Index</code> permit
</aside>
