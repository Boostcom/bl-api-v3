## <a name="operations-alerts"></a> Alerts

### Common models

#### <a name="operations-alert-model"></a> Alert model

> Alert example:

```json
{
  "alert": {
    "id": 3,
    "type": "Fire alert",
    "message": "Hi",
    "location": "Parking",
    "report_status": "safe",
    "reported_at": "2021-05-15T12:05:15.639Z",
    "created_at": "2021-05-15T11:41:02.543Z",
    "updated_at": "2021-05-15T11:41:04.049Z",
    "last_seen_at": "2021-05-15T13:00:27.966Z"
  }
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
type | string | no | One of available [alert types](#operations-list-alerts) names
message | string | no | Content of message sent to recipients
location | string | yes |
report_status   | enum: `['safe', 'need_help']`              | yes      | Status reported by member
reported_at | datetime | yes | Time of report
created_at | datetime | no | Time of creation
updated_at | datetime | no | Time of last update
last_seen_at | datetime | yes | Last time when the recipient retrieved the Alert record

### <a name="operations-list-alert-types"></a> List Alert Types

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/alert_types" \
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

**GET** `v1/users/me/operations/alerts`

Returns list of available alert types with default messages.

<aside class="notice">
Requires <code>Operations:Api:Users:AlertTypes:List</code> permit
</aside>

### <a name="operations-list-alerts"></a> List Alerts

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/alerts" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [alerts](#operations-alert-model) and [pagination_info](#pagination-model)

```json
{
  "alerts": [], // List of alerts - see 'Alert model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/users/me/operations/alerts`

Returns list of [Alerts](#operations-alert-model).

#### Query Parameters

Parameter     | Type                         | Default   | Description
------------- | -----------                  | --------- | -----------
per_page      | integer                      | 100       | Number of results to be returned per request (100 is the maximum)
page_no       | integer                      | 1         | Number of results page
report_status | enum: `['safe', 'need_help]` | null      | When present, returns only Alerts having given `report_status`
seen          | boolean                      | null      | When true, returns only seen Alerts - and vice versa
reported      | boolean                      | null      | When true, returns only Alerts the member has reported to - and vice versa

<aside class="notice">
Requires <code>Operations:Api:Users:Alerts:List</code> permit
</aside>

### <a name="operations-show-alert"></a> Get Alert

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/alerts/5" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [alert](#operations-alert-model)

```json
{
  "alert": {} // Alert - see: 'Alert model'
}
```

**GET** `v1/users/me/operations/alerts/:id`

Returns given [Alert](#operations-alert-model) with additional details

#### Additional alert details

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
reviewed_at | datetime | yes | Time when the alert has been reviewed (accepted or rejected)

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Alert not found

<aside class="notice">
Requires <code>Operations:Api:Users:Alerts:Show</code> permit
</aside>

### <a name="operations-create-alert"></a> Create Alert

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/users/me/operations/alerts" \
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

> When successful, returns object containing created [alert](#operations-alert-model)

```json
{
  "alert": {} // Alert - see: 'Alert model'
}
```

**POST** `v1/users/me/operations/alerts`

Creates a new [Alert](#operations-alert-model). Alert then must be [accepted](#operations-admin-accept-alert)

#### POST Parameters (JSON)

Key                   | Type      | Description
---------             | --------- | ---------
**alert.audience_id** | integer[] | ID of audience that should receive the alert
**alert.type**        | string    | Must match available [alert types](#operations-admin-list-alerts) names
**alert.message**     | string    | Content of message sent to recipients
alert.location        | string    |

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
alert | Alert | See: [Alert model](#operations-alert-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Users:Alerts:Create</code> permit
</aside>

### <a name="operations-report-alert"></a> Report to Alert

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/users/me/operations/alerts/12/report" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        "report_status": "safe"
    }
  '
```

> When successful, returns object containing updated [alert](#operations-alert-model)

```json
{
  "alert": {} // Alert - see 'Alert model'
}
```

**PUT** `v1/users/me/operations/alerts/:id/report`

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Alert ID

#### PUT Parameters (JSON)

Parameter     | Type                          | Required? | Description
----------    | --------                      | ------    | ---
report_status | enum: `['safe', 'need_help']` | yes       | 

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
alert | Alert | See: [Alert model](#operations-alert-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Alert not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Users:Alerts:Report</code> permit
</aside>
