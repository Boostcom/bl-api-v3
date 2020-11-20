## <a name="messaging-sendings"></a> Sendings

### <a name="messaging-get-sending"></a> Get Sending

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/sendings/32" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing the requested [sending](#messaging-sending-model)

```json
{
  "sending": {} // Sending - See: "Sending model"
}
```

**GET** `v1/sendings/:id`

Returns [Sending](#messaging-sending-model).  

#### URL Parameters

Parameter  |                   Type                | Description
---------- | -------------------------------------------- | ------
id | integer                                   | Sending ID 

#### Error responses

Status | Description
--------- | ----------- 
`404` | Sending not found

<aside class="notice">
Requires <code>Messages:Api:Sendings:Get</code> permit
</aside>

### <a name="messaging-create-sending"></a> Create Sending

> Example - creates a Sending scheduled immediately, sent to given audience and one inline recipient.

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/messages/42/sendings" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
      "sending": {
        "schedule": { "type": "immediate" },
        "audience": {
          "type": "inline",
          "id": 150
        },
        "recipients": [
          { "member_id": 150, "properties": { "first_name": "Piotr" } }
        ]
      }
    }
  '
```

> When successful, returns object containing created [sending](#messaging-sending-model)

```json
{
  "sending": {} // Sending - See: "Sending model"
}
```

**POST** `v1/messages/:message_id/sendings`

Creates (and schedules if demanded) Sending for given Message.

#### URL Parameters

Parameter  |                   Type                | Description
---------- | -------------------------------------------- | ------
message_id | integer                                   | Message ID 

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
sending | SendingPayload | See: [SendingPayload model](#messaging-sending-payload-model)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
sending | Sending | See: [Sending model](#messaging-sending-model)

#### Error responses

Status | Description
--------- | ----------- 
`403` | Unauthorized to set Sending priority
`404` | Message not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Messages:Api:Sendings:Create</code> permit
</aside>

### <a name="messaging-update-sending"></a> Update Sending

> Example - updates the Sending - it's unscheduled and audience is removed

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/sendings/42" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
      "sending": {
        "schedule": nil,
        "audience": nil
      }
    }
  '
```

> When successful, returns object containing updated [sending](#messaging-sending-model)

```json
{
  "sending": {} // Sending - See: "Sending model"
}
```

**PUT** `v1/sendings/:id`

Updates Sending attributes. 

The update is partial - only attributes present in the payload are updated. 
If attribute removal is intended, it should be provided with `null` value.

##### Updating recipients

If `recipients` are given, the Sending Recipients are replaced with given ones. 
When the array is empty, all Recipients are removed from Sending.

#### URL Parameters

Parameter  |                   Type                | Description
---------- | -------------------------------------------- | ------
id | integer                                   | Sending ID 

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
sending | SendingPayload | See: [SendingPayload model](#messaging-sending-payload-model)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
sending | Sending | See: [Sending model](#messaging-sending-model)

#### Error responses

Status | Description
--------- | ----------- 
`403` | Unauthorized to set Sending priority
`404` | Sending not found
`405` | Sending is not editable - execution process has started or finished
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Messages:Api:Sendings:Update</code> permit
</aside>
