## <a name="operations-admin-alert-templates"></a> Alert Templates

### Common models

#### <a name="operations-admin-alert-template-model"></a> Alert Template model

> Alert Template example:

```json
{
  "id": 4,
  "type": "Fire alert",
  "message": "Fire has broke out",
  "created_at": "2021-05-14T09:26:14.823Z",
  "updated_at": "2021-05-14T09:26:14.823Z"
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
type | string | no | One of available [alert types](#operations-admin-list-alert-types) names
message | string | no | Content of message sent to recipients
created_at | datetime | no | Time of creation
updated_at | datetime | no | Time of last update

#### <a name="operations-admin-alert-template-payload-model"></a> Alert Template Payload model

Key         | Type      | Description
---------   | --------- | ---------
**type**    | string    | Must match available [alert types](#operations-admin-list-alert-types) names
**message** | string    | Content of message sent to recipients

### <a name="operations-admin-list-alert-templates"></a> List Alert Templates

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/alert_templates" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [alert templates](#operations-admin-alert-template-model) and [pagination_info](#pagination-model)

```json
{
  "alert_templates": [], // List of alert_templates - see 'Alert Template model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/alert_templates`

Returns list of [Alert Templates](#operations-admin-alert-template-model).

<aside class="notice">
Requires <code>Operations:Api:AlertTemplates:List</code> permit
</aside>

### <a name="operations-admin-show-alert-template"></a> Get Alert Template

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/alert_templates/5" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [alert template](#operations-admin-alert-template-model)

```json
{
  "alert_template": {} // see: 'Alert Template model'
}
````

**GET** `v1/operations/alert_templates/:id`

Returns given [Alert Template](#operations-admin-alert-template-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert Template ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Alert Template not found

<aside class="notice">
Requires <code>Operations:Api:AlertTemplates:Show</code> permit
</aside>

### <a name="operations-admin-create-alert-template"></a> Create Alert Template

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/operations/alert_templates" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "alert_template": {
            "type": "Fire alert",
            "message": "Some message",
        }
    }
  '
```

> When successful, returns object containing created [alert template](#operations-admin-alert-template-model)

```json
{
  "alert_template": {} // Alert Template - see: 'Alert Template model'
}
```

**POST** `v1/operations/alert_templates`

Creates a new [Alert Template](#operations-admin-alert-template-model) and sends it to given recipients.

#### POST Parameters (JSON)

See [Alert Template Payload model](#operations-admin-alert-template-payload-model)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
alert_template | Alert Template | See: [Alert Template model](#operations-admin-alert-template-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:AlertTemplates:Create</code> permit
</aside>

### <a name="operations-admin-update-alert-template"></a> Update Alert Template

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/alert_templates/12" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "alert_template": {
            "type": "Fire alert",
            "message": "Some message"
        }
    }
  '
```

> When successful, returns object containing updated [alert template](#operations-admin-alert-template-model)

```json
{
  "alert_template": {} // Alert Template - see 'Alert Template model'
}
```

**PUT** `v1/operations/alert_templates/:id`

Updates the [Alert Template](#operations-admin-alert-template-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert Template ID

#### POST Parameters (JSON)

See [Alert Template Payload model](#operations-admin-alert-template-payload-model)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
alert_template | Alert Template | See: [Alert Template model](#operations-admin-alert-template-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Alert Template not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

### <a name="operations-admin-destroy-alert-template"></a> Destroy Alert Template

> Example

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v1/operations/alert_templates/12 \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, returns object containing destroyed [alert template](#operations-admin-alert-template-model)

```json
{
  "alert_template": {} // Alert Template - see: 'Alert Template model'
}
```

**DELETE** `v1/operations/alert_templates/:id`

Destroys the [Alert Template](#operations-admin-alert-template-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert Template ID

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
alert_template | Alert Template | See: [Alert Template model](#operations-admin-alert-template-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Alert Template not found

<aside class="notice">
Requires <code>Operations:Api:AlertTemplates:Destroy</code> permit
</aside>
