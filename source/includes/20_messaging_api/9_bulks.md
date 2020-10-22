## <a name="messaging-bulk-processing"></a> Bulk Operations

> Example - simultaneously updates Message, creates one Sending and updates another one

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v1/messages/bulks" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
      "operations": [
        {
          "type": "update",
          "entity": "message",
          "params": { "id": 80 },
          "payload": { "name": "my name" }
        },
        {
          "type": "update",
          "entity": "sending",
          "params": { "id": 3958 },
          "payload": { "schedule": { "type": "immediate" } }
        },
        {
          "type": "create",
          "entity": "sending",
          "params": { "message_id": 80 },
          "payload": { "recipients": [{ "member_id": 13 }] }
        },
      ]
    }
  '
```

> When all operations have been executed successfully, returns 200 and success response for each of them

```json
{
  "results": [
    {
      "type": "update",
      "entity": "message",
      "params": { "id": 41 },
      "response": {
        "status": 200,
        "body": {
          "message": {} // Updated Message
        }
      }
    },
    {
      "type": "update",
      "entity": "sending",
      "params": { "id": 3958 },
      "response": {
        "status": 200,
        "body": {
          "error": "Invalid parameters",
          "sending": {} // Updated Sending
        }
      }
    },
    {
      "type": "create",
      "entity": "sending",
      "params": { "message_id": 80 },
      "response": {
        "status": 200,
        "body": {
          "sending": {} // Created Sending
        }
      }
    }
  ]
}

```

> When one of operations fails, returns status 470 and results of operations up to the one that failed

```json
{
  "results": [
    {
      "type": "update",
      "entity": "message",
      "params": { "id": 41 },
      "response": {
        "status": 200,
        "body": {
          "message": {} // Updated Message
        }
      }
    },
    // Failed operation - previous ones are cancelled, despite the fact 
    // they've been rendered as successful
    {
      "type": "update",
      "entity": "sending",
      "params": { "id": 3958 },
      "response": {
        "status": 422,
        "body": {
          "error": "Invalid parameters",
          "details": {} // Validation errors
        }
      }
    }
    // Next operations are not executed, so there is no response for them
  ]
}

```

**POST** `v1/messages/bulks`

Allows to perform multiple operations (up to 10) on Messaging records (Messages, Sendings, Channels and Templates) 
in a single request.

Behavior and interface (payload, params and response) of each operation is based on specific API endpoint - 
[this](#messaging-bulk-processing-operations) table depicts it.

Operations are performed in a transactional way - if one of them fails, processing stops and other operations 
are cancelled. Special code - `470` - is returned as the status. 
Actual error for operation that failed is included in the returned `results` array.

If all operations are successful, `200` code is returned.

#### POST Parameters (JSON)

Key | Type | Description
----- | ---- | ---
operations | [Operations[]](#messaging-bulk-processing-operation-model) | Array of operations (max. 10) to perform

##### <a name="messaging-bulk-processing-operation-model"></a> Operation model

Key | Type  | Description
---------- | -------- | ---------
type | enum: ['create', 'update', 'delete'] | Type of operation to perform
entity | enum: ['message', 'sending', 'channel', 'template'] | Type of entity to change
params | object | Params identifying the entity (reflecting original HTTP URL params), specific to type and operation
payload | object | Attributes of identity, specific to type and operation 

##### <a name="messaging-bulk-processing-operations"></a> Supported operation types and their params and payloads

Entity | Operation | Params | Payload | Original endpoint
---------- | -------- | --------- | ----- | -----
`message` | `create` | none | [Message creation payload](#messaging-message-creation-payload) | [Create Message](#messaging-create-message) 
`message` | `update` | `id` | [Message update payload](#messaging-message-update-payload) | [Update Message](#messaging-update-message)
`sending` | `create` | `message_id` | [SendingPayload](#messaging-sending-payload-model) | [Create Sending](#messaging-create-sending)
`sending` | `update` | `id` | [SendingPayload](#messaging-sending-payload-model) | [Update Sending](#messaging-update-sending)

#### Response (JSON object)

Key | Type
---------- | -------- 
results | [Result[]](#messaging-bulk-processing-result) 

##### <a name="messaging-bulk-processing-result"></a> Result model

Key | Type  | Description
---------- | -------- | ---------
type | | Given operation type
entity | | Given type of entity
params | | Given params of operations
payload | object | Given payload
response.status | integer| HTTP status of operation 
response.body | object | JSON response of operation 

#### Error responses

Status | Description
--------- | ----------- 
`200` | All operations succeeded
`470` | At least one operation failed
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Messages:Api:Bulks:Process</code> permit
</aside>
