## <a name="operations-admin-settings"></a> Settings

### Common models

#### <a name="operations-admin-settings-model"></a> Settings model

> Alert example:

```json
{
  "alert_reviewers": ["7db2c508-1758-4b84-ba68-50b5c8e1f458"],
  "updated_at": "2021-05-14T09:26:14.823Z"
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
alert_reviewers | string[] | no | IDs of users responsible for accepting alerts
updated_at | datetime | no | When null, the default values are returned.

#### <a name="operations-admin-settings-payload-model"></a> SettingsPayload model

Key                   | Type      | Description
---------             | --------- | ---------
**alert_reviewers** | string[] | IDs of users responsible for accepting alerts

### <a name="operations-admin-show-settings"></a> Get Settings

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/settings" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing Operations [settings](#operations-admin-settings-model)

```json
{
  "settings": {} // Settings - see 'Settings model'
}
```

**GET** `v1/operations/settings`

Returns Operations [settings](#operations-admin-settings-model).

When settings haven't been set up yet, returns default values.

<aside class="notice">
Requires <code>Operations:Api:Settings:Show</code> permit
</aside>

### <a name="operations-admin-update-settings"></a> Update Settings

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/settings" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "settings": {
            "alert_reviewers": ["7db2c508-1758-4b84-ba68-50b5c8e1f458"]
        }
    }
  '
```

> When successful, returns object containing updated [settings](#operations-admin-settings-model)

```json
{
  "settings": {} // Settings - see 'Settings model'
}
```

**PUT** `v1/operations/settings`

Updates the [Settings](#operations-admin-settings-model)

#### PUT Parameters (JSON)

See [SettingsPayload model](#operations-admin-settings-payload-model)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
settings | Settings | See: [Settings model](#operations-admin-settings-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

#### Specific validation errors

Key                                              | Error
---                                              | ----------------
`settings.alert_reviewers`                       | `user_not_found`
`settings.alert_reviewers`                       | `user_msisdn_missing`

<aside class="notice">
Requires <code>Operations:Api:Settings:Update</code> permit
</aside>
