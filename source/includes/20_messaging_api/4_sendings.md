## <a name="messaging-sendings"></a> Sendings

### <a name="messaging-get-sending"></a> Get Sending

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/messages/sendings/32" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing the requested [sending](#messaging-sending-model)

```json
{
  "sending": {} // Sending - see 'Sending model'
}
```

**GET** `v1/messages/sendings/:id`

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

### <a name="messaging-list-sending-dispatches"></a> List Sending Dispatches

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/messages/sendings/32/dispatches" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [dispatches](#messaging-dispatch-model) and [pagination_info](#pagination-model)

```json
{
  "dispatches": [], // List of messages - see 'Dispatch model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
```

**GET** `v1/messages/sendings/:id`

Returns [Dispatches](#messaging-dispatch-model) created for Sending.

#### URL Parameters

Parameter  |                   Type                | Description
---------- | -------------------------------------------- | ------
id | integer                                   | Sending ID

#### Query Parameters

Parameter            | Type        | Default   | Description
--------------       | ----------- | --------- | -----------
per_page             | integer     | 100       | Number of results to be returned per request (100 is the maximum)
page_no              | integer     | 1         | Number of results page
transmission_status  | enum        | null      | When present, returns only Messages having given [transmission_status](#messaging-dispatch-model)
delivery_status      | enum        | null      | When present, returns only Dispatches with given [delivery_status](#messaging-dispatch-model)
member_id            | integer     | null      | When present, returns only Dispatches sent to recipient identified by given id
msisdn               | string      | null      | When present, returns only Dispatches sent to recipient identified by given msisdn
email                | string      | null      | When present, returns only Dispatches sent to recipient identified by given email
app_token            | string      | null      | When present, returns only Dispatches sent to recipient identified by given app_token

#### Error responses

Status | Description
--------- | -----------
`404` | Sending not found

<aside class="notice">
Requires <code>Messages:Api:Dispatches:List</code> permit
</aside>

### <a name="messaging-list-sending-dispatch-members"></a> List Sending Members

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/messages/sendings/32/dispatches/members" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [Dispatch_member](#messaging-dispatch-member-model) and [pagination_info](#pagination-model)

```json
{
  "dispatch_members": [], // List of distinct members - see 'Dispatch model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
```

**GET** `v1/messages/sendings/:id/dispatches/members`

Returns [Dispatch_member](#messaging-dispatch-member-model) created for Sending.

#### URL Parameters

Parameter  |                   Type                | Description
---------- | -------------------------------------------- | ------
id | integer                                   | Sending ID

#### Query Parameters

Parameter            | Type        | Default   | Description
--------------       | ----------- | --------- | -----------
per_page             | integer     | 100       | Number of results to be returned per request (100 is the maximum)
page_no              | integer     | 1         | Number of results page
transmission_status  | enum        | null      | When present, returns only Messages having given [transmission_status](#messaging-dispatch-model)
delivery_status      | enum        | null      | When present, returns only Dispatches with given [delivery_status](#messaging-dispatch-model)
member_id            | integer     | null      | When present, returns only Dispatches sent to recipient identified by given id
msisdn               | string      | null      | When present, returns only Dispatches sent to recipient identified by given msisdn
email                | string      | null      | When present, returns only Dispatches sent to recipient identified by given email
app_token            | string      | null      | When present, returns only Dispatches sent to recipient identified by given app_token

#### Error responses

Status | Description
--------- | -----------
`404` | Sending not found

<aside class="notice">
Requires <code>Messages:Api:Dispatches:Members:Index</code> permit
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
  "sending": {} // Sending - see 'Sending model'
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
"https://api.mpc.placewise.com/v1/messages/sendings/42" \
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
  "sending": {} // Sending - see 'Sending model'
}
```

**PUT** `v1/messages/sendings/:id`

Updates Sending attributes.

The update is partial - only attributes present in the payload are updated.
If attribute removal is intended, it should be provided with `null` value.

##### Updating recipients

If `recipients` are given, the Sending Recipients are replaced with given ones.
When the array is empty, all Recipients are removed from Sending.

#### URL Parameters

Parameter  | Type    | Description
---        | ---     | ----
id         | integer | Sending ID

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

### <a name="messaging-cancel-sending"></a> Cancel Sending

> Example - cancels the Sending execution

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/messages/sendings/42/cancel" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, returns object containing cancelled [sending](#messaging-sending-model)

```json
{
  "sending": {} // Sending - see 'Sending model'
}
```

**PUT** `v1/messages/sendings/:id`

Cancels Sending execution process. See [Sending status](#messaging-sending-status).

#### URL Parameters

Parameter  | Type    | Description
---------- | ---     | ------
id         | integer | Sending ID

#### Response (JSON object)

Key     | Type    | Description
------- | ------- | ---------
sending | Sending | See: [Sending model](#messaging-sending-model)

#### Error responses

Status | Description
-------| -----------
`404`  | Sending not found
`405`  | Sending is not cancellable - execution is not in progress

<aside class="notice">
Requires <code>Messages:Api:Sendings:Update</code> permit
</aside>
