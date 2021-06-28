## <a name="operations-admin-alerts"></a> Alerts

### Common models

#### <a name="operations-admin-alert-model"></a> Alert model

> Alert example:

```json
{
  "id": 10,
  "audience_id": 572,
  "type": "Fire alert",
  "state": "pending",
  "message": "Fire has broke out",
  "location": "Parking",
  "created_at": "2021-05-14T09:26:14.823Z",
  "updated_at": "2021-05-14T09:26:14.823Z"
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
audience_id | integer | no | Audience the alert is sent to
type | string | no | One of available [alert types](#operations-admin-list-alerts) names
state |  enum: `['pending', 'accepted', 'rejected']` | no | 
message | string | no | Content of message sent to recipients
location | string | yes | 
created_at | datetime | no | Time of creation
updated_at | datetime | no | Time of last update

#### <a name="operations-admin-alert-payload-model"></a> AlertPayload model

Key                   | Type      | Description
---------             | --------- | ---------
**audience_id** | integer[] | ID of audience that should receive the alert
**type**        | string    | Must match available [alert types](#operations-admin-list-alerts) names
**message**     | string    | Content of message sent to recipients 
location        | string    | 

### <a name="operations-admin-list-alert-types"></a> List Alert Types

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/alert_types" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing available alert types

```json
{
  "alerts_types": [
    {
      "name": "Fire alert",
      "message": "The fire has broke out"
    }
  ]
}
````

**GET** `v1/operations/alerts`

Returns list of available alert types with default messages.

<aside class="notice">
Requires <code>Operations:Api:AlertTypes:List</code> permit
</aside>

### <a name="operations-admin-list-alerts"></a> List Alerts

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/alerts" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [alerts](#operations-admin-alert-model) and [pagination_info](#pagination-model)

```json
{
  "alerts": [], // List of alerts - see 'Alert model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/alerts`

Returns list of [Alerts](#operations-admin-alert-model).

#### Query Parameters

Parameter      | Type                                        | Default   | Description
-------------- | -----------                                 | --------- | -----------
per_page       | integer                                     | 100       | Number of results to be returned per request (100 is the maximum)
page_no        | integer                                     | 1         | Number of results page
state          | enum: `['pending', 'accepted', 'rejected']` | null      | When present, returns only Alerts having given `state`

<aside class="notice">
Requires <code>Operations:Api:Alerts:List</code> permit
</aside>

### <a name="operations-admin-show-alert"></a> Get Alert

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/alerts/5" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing detailed [alert](#operations-admin-alert-model)

```json
{
  "alert": {
    "id": 1,
    "audience_id": 572,
    "type": "Fire alert",
    "state": "accepted",
    "message": "Hi",
    "location": "Parking",
    "created_at": "2021-05-15T10:09:48.559Z",
    "updated_at": "2021-05-15T10:10:21.868Z",
    "created_by": { "entity_id": 277, "entity_type": "user" },
    "reviewed_at": "2021-05-15T10:09:48.559Z",
    "reviewed_by": { "entity_id": "", "entity_type": "user" },
    "sent_at": "2021-05-15T10:10:21.868Z"
  }
}
```

**GET** `v1/operations/alerts/:id`

Returns given [Alert](#operations-admin-alert-model) with additional details

#### Additional alert details

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
created_by | [API entity](#api-entity-model) | no | Entity that created the alert
reviewed_at | datetime | yes | Time when the alert has been reviewed (accepted or rejected)
reviewed_by | [API entity](#api-entity-model) | yes | Entity that reviewed the alert 
sent_at | datetime | yes | Time when the alert has been sent

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Alert not found

<aside class="notice">
Requires <code>Operations:Api:Alerts:Show</code> permit
</aside>

### <a name="operations-admin-list-alert-recipients"></a> List Alert Recipients

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/alerts/15/recipients" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [alert recipients](#operations-admin-alert-recipient-model) and [pagination_info](#pagination-model)

```json
{
  "alert_recipients": [], // List of alert recipients
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/alerts/:id/recipients`

Returns list of recipients that are Alert has been sent to.

#### Alert Recipient model

> DocumentRecipient example:

```json
{
  "id": 137,
  "member_id": 46849981,
  "delivery_status": "delivered",
  "report_status": "safe",
  "reported_at": "2021-05-15T12:05:15.639Z",
  "last_seen_at": "2021-05-15T12:06:02.470Z",
  "created_at": "2021-05-15T11:41:03.574Z",
  "updated_at": "2021-05-15T12:06:02.471Z"
}
```

Key             | Type                                       | Optional | Description
---------       | --------                                   | -------- | ---------
id              | integer                                    | no       |
member_id       | integer                                    | no       | ID of MPC member
delivery_status | enum: `['pending', 'delivered', 'failed']` | no       | Describes if member has received actual message
report_status   | enum: `['safe', 'need_help']`              | yes      | Status reported by member  
reported_at     | datetime                                   | yes      | Time of report
last_seen_at    | datetime                                   | yes      | Last time when the recipient retrieved the Alert record
created_at      | datetime                                   | no       | Time of creation
updated_at      | datetime                                   | no       | Time of last update

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert ID

#### Query Parameters

Parameter | Type    | Default   | Description
--------- | ------- | --------- | -----------
per_page  | integer | 100       | Number of results to be returned per request (100 is the maximum)
page_no   | integer | 1         | Number of results page

#### Error responses

Status    | Description
--------- | -----------
`404`     | Alert not found

<aside class="notice">
Requires <code>Operations:Api:AlertRecipients:List</code> permit
</aside>

### <a name="operations-admin-create-alert"></a> Create Alert

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/operations/alerts" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "alert": {
            "audience_id": 572,
            "type": "Fire alert",
            "message": "Hi",
            "location": "Parking"
        }
    }
  '
```

> When successful, returns object containing created [alert](#operations-admin-alert-model)

```json
{
  "alert": {} // Alert - see: 'Alert model'
}
```

**POST** `v1/operations/alerts`

Creates a new [Alert](#operations-admin-alert-model) (with `accepted` state) and sends it to given audience.

#### POST Parameters (JSON)

See [AlertPayload model](#operations-admin-alert-payload-model)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
alert | Alert | See: [Alert model](#operations-admin-alert-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Alerts:Create</code> permit
</aside>

### <a name="operations-admin-accept-alert"></a> Accept Alert

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/alerts/12/accept" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "alert": {
            "audience_id": 572,
            "type": "Fire alert",
            "message": "Hi",
            "location": "Parking"
        }
    }
  '
```

> When successful, returns object containing updated [alert](#operations-admin-alert-model)

```json
{
  "alert": {} // Alert - see 'Alert model'
}
```

**PUT** `v1/operations/alerts/:id/accept`

Accepts the [Alert](#operations-admin-alert-model) which results in message sending. It also allow to
update the alert before sending.

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert ID

#### PUT Parameters (JSON)

See [AlertPayload model](#operations-admin-alert-payload-model)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
alert | Alert | See: [Alert model](#operations-admin-alert-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Alert not found
`405` | Alert cannot be accepted
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Alerts:Accept</code> permit
</aside>

### <a name="operations-admin-reject-alert"></a> Reject Alert

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/alerts/12/reject" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
  '
```

> When successful, returns object containing [alert](#operations-admin-alert-model)

```json
{
  "alert": {} // Alert - see 'Alert model'
}
```

**PUT** `v1/operations/alerts/:id/reject`

Rejects the [Alert](#operations-admin-alert-model) which results in ignoring message sending.

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert ID

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
alert | Alert | See: [Alert model](#operations-admin-alert-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Alert not found
`405` | Alert cannot be rejected
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Alerts:Reject</code> permit
</aside>
