## <a name="messaging-messages"></a> Messages

<aside class="warning">
This API is under development. Therefore, it may not be ready for use and is a subject to change at any time.
</aside>

### <a name="messaging-create-message"></a> Create message

> Example - creates an SMS message, scheduled in 15 minutes, sent to given audience and one inline recipient.

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v1/infinity-mall/messages" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
      "message": {
        "name": "My message", "shorten_urls": false,
        "channels": [
          {
            "type": "sms", 
            "sender": { type: "alphanumeric", value: "Infinity" },
            "template": {
              "type": "inline",
              "data": { "type": "plain", "content": { "body": "Hi {{first_name}}!" } }
            }
          }
        ],
        "sending": {
          "schedule": { "type": "relative", "in": "15 minutes" },
          "audience": {
            "type": "inline",
            "conditions": [
              {
                "condition_group": "member_properties", "field": "first_name",
                "operator": "match", "value": "Piotr"
              }
            ]
          },
          "recipients": [
            { "member_id": 150, "properties": { "first_name": "Piotr" } }
          ]
        }
      }
    }
  '
```

> When successful, returns object containing created [message](#messaging-message-model)

```json
{
  "message": {} // Message - See: "Message model"
}
```

**POST** `v1/:loyalty_club_slug/messages`

Creates a new Message and optionally schedules Sending for it.  

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
message | MessagePayload | See: [MessagePayload model](#messaging-message-payload-model)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
message | Message | See: [Message model](#messaging-message-model)

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Messages:Api:Messages:Create</code> permit
</aside>

### <a name="messaging-get-message"></a> Get message

> Example - creates an SMS message, scheduled in 15 minutes, sent to given audience and one inline recipient.

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v1/infinity-mall/messages/83" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing the requested [message](#messaging-message-model)

```json
{
  "message": {} // Message - See: "Message model"
}
```

**GET** `v1/:loyalty_club_slug/messages/:message_id`

Returns [Message](#messaging-message-model).  

#### URL Parameters

Parameter  | Description                                  | Type
---------- | -------------------------------------------- | ------
message_id | Message ID                                   | integer

<aside class="notice">
Requires <code>Messages:Api:Messages:Get</code> permit
</aside>
