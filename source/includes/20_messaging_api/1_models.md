# <a name="messaging"></a> Messaging API

## Models

### <a name="messaging-message-model"></a> Message model

> Message example:

```json
{
  "id": 758131,
  "name": "DD3",
  "shorten_urls": true,
  "track_in_shortener": false,
  "campaign_id": null,
  "created_at": "2020-09-15T10:01:09.467Z",
  "updated_at": "2020-09-15T10:01:09.467Z",
  "channels": [
    {
      "id": 55781,
      "type": "sms",
      "sender": { "type": "alphanumeric", "value": "Infinity" },
      "template": {
        "id": 609193,
        "type": "plain",
        "content": { "body": "Hei {{name}}" },
        "created_at": "2020-09-15T12:45:08.618Z",
        "updated_at": "2020-09-15T12:45:08.618Z"
      },
      "created_at": "2020-09-15T12:45:08.625Z",
      "updated_at": "2020-09-15T12:45:08.625Z"
    }
  ],
  "sendings": [
    {
      "id": 1000016,
      "audience_id": 106642,
      "scheduled_with": {
        "in": "15 minutes",
        "type": "relative"
      },
      "scheduled_at": "2020-09-18T09:28:59.918Z",
      "created_at": "2020-09-18T09:13:59.937Z",
      "updated_at": "2020-09-18T09:13:59.937Z",
      "recipients": [
        {
          "member_id": 150,
          "msisdn": null,
          "email": null,
          "app_token": null,
          "properties": { "first_name": "Piotr" }
        }
      ]
    }
  ]
}
```

Key | Type | Description
--------- | --------- | ---------
**id** | integer |   
**name** | string |
**shorten_urls** | boolean | Should sendings for this message have URLs shortened with MPC's Shortener? 
**track_in_shortener** | boolean | Should MPC's Shortener track users?
campaign_id | string | MPC's campaign ID
**channels** | [Channel](#messaging-channel-model)[] | Channels the message is sent with
**sendings** | [Sending](#messaging-sending-model)[] | Sending of the message
**created_at** | datetime | Time of creation
**updated_at** | datetime | Time of last update

### <a name="messaging-channel-model"></a> Channel model

> Channel example:

```json
{
  "id": 55781,
  "type": "sms",
  "sender": { "type": "alphanumeric", "value": "Infinity" },
  "template": {
     "id": 609193,
     "type": "plain",
     "content": { "body": "Hei {{name}}" },
     "created_at": "2020-09-15T12:45:08.618Z",
     "updated_at": "2020-09-15T12:45:08.618Z"
  },
  "created_at": "2020-09-15T12:45:08.625Z",
  "updated_at": "2020-09-15T12:45:08.625Z"
}
```

Defines channel the message should be sent with along with the content that should be sent through this channel. 

Please note that while we allow to set multiple channels on single message (for future usage), 
currently just one (the first) is utilized as the Message's only channel.

Key | Type
----- | --------- 
**id** | integer 
**type** | enum: `['sms', 'email', 'push']`
**created_at** | datetime
**updated_at** | datetime
**template** | [Template](#messaging-template-model)
[(channel-specific attributes)](#messaging-channel-specific-attributes) |

#### <a name="messaging-channel-specific-attributes"></a> Channel-specific attributes

When not provided explicitly, values of all the following attributes are taken from Loyalty Club configuration.

Key | Channel type | Type
--------- | ----- | --------- 
sender.type | `sms` | enum: `['alphanumeric', 'international']` 
sender.value | `sms` | string
from_email | `email` | string
from_name | `email` | string

The `sender` describes how SMS sender will be displayed to the receiver of SMS.
By default, a value from Loyalty Club configuration is used.

Depending on selected `sender.type`, `sender.value` can be provided according to the following table:  

Type | Value
---- | ---------------
`alphanumeric` | String consisting of 2-11 characters
`international` | Any MSISDN

### <a name="messaging-template-model"></a> Template model

> Template example:

```json
{
  "id": 609193,
  "wrapper_id": 5913,
  "type": "plain",
  "content": { "body": "Hei {{name}}" },
  "created_at": "2020-09-15T12:45:08.618Z",
  "updated_at": "2020-09-15T12:45:08.618Z"
}
```
Key | Type | Description
--------- | --------- | -----
**id** | integer |
wrapper_id | integer | ID of [wrapping template](#messaging-template-wrapping)  
**type** | enum: `['plain', 'email', 'bee_email', 'push']` |
**content** | Object | Definition depends on the `type`. See below
**created_at** | datetime | Time of creation
**updated_at** | datetime | Time of last update

#### <a name="messaging-template-model-plain"></a> `plain` template

> Plain Template example:

```json
{
  "id": 609193,
  "wrapper_id": null,
  "type": "plain",
  "content": { "body": "Hei {{name}}" },
  "created_at": "2020-09-15T12:45:08.618Z",
  "updated_at": "2020-09-15T12:45:08.618Z"
}
```

This type of template can be assigned to any type.

Content of such template consists of single (required) property - `body`.

#### <a name="messaging-template-model-email"></a> `email` template

> Email Template example:

```json
{
  "id": 609194,
  "wrapper_id": null,
  "type": "email",
  "content": { "subject": "Welcome!", "body": "<b>Hei {{name}}</b>" },
  "created_at": "2020-09-15T12:45:08.618Z",
  "updated_at": "2020-09-15T12:45:08.618Z"
}
```

This type of template can be assigned to `email` channel.

Key | Type 
--------- | ---------
**content.body** | HTML
content.subject | string

#### <a name="messaging-template-model-bee-email"></a> `bee_email` template

> BeeEmail Template example:

```json
{
  "id": 609193,
  "wrapper_id": null,
  "type": "bee_email",
  "content": { "subject": "Welcome!", "body": "<b>Hei {{name}}</b>", "bee_json": {} },
  "created_at": "2020-09-15T12:45:08.618Z",
  "updated_at": "2020-09-15T12:45:08.618Z"
}
```

This type of template is used internally by MPC UI. 

It differs from `email` by containing JSON source which is used by [Bee Plugin](https://docs.beefree.io/) to generate HTML `body`. 

Key | Type 
--------- | ---------
**content.body** | HTML
**content.bee_json** | Object
content.subject | string

#### <a name="messaging-template-model-push"></a> `push` template

> Push Template example:

```json
{
  "id": 609193,
  "wrapper_id": null,
  "type": "push",
  "content": { "title": "Welcome", "body": "Hi {{name}}!", "uri": "iml://app/cpn" },
  "created_at": "2020-09-15T12:45:08.618Z",
  "updated_at": "2020-09-15T12:45:08.618Z"
}
```

This type of template can be assigned to `push` channel.

Key | Type 
--------- | ---------
**content.body** | string
content.title | string
content.uri | URL

#### <a name="messaging-template-wrapping"></a> Wrapping

`@todo`

#### <a name="messaging-template-generating"></a> Template generating

`@todo`

### <a name="messaging-sending-model"></a> Sending model

> Sending example:

```json
{
  "id": 1000016,
  "audience_id": 106642,
  "scheduled_with": {
    "in": "15 minutes",
    "type": "relative"
  },
  "scheduled_at": "2020-09-18T09:28:59.918Z",
  "created_at": "2020-09-18T09:13:59.937Z",
  "updated_at": "2020-09-18T09:13:59.937Z",
  "recipients": [
    {
      "member_id": 150,
      "msisdn": null,
      "email": null,
      "app_token": null,
      "properties": { "first_name": "Piotr" }
    }
  ]
}
```

Key | Type | Description
--------- | --------- | ---------
**id** | integer |   
audience_id | integer | ID of audience the sending is directed to  
scheduled_with | Object | Params the sending has been scheduled with - See [SendingSchedulePayload](#messaging-sending-schedule-payload-model)
**scheduled_at** | datetime | Time of sending execution
**created_at** | datetime | Time of creation
**updated_at** | datetime | Time of update
**recipients** | [Recipient](#messaging-recipient-model)[] | Inline recipients the sending is directed to

### <a name="messaging-recipient-model"></a> Recipient model

> Sending example:

```json
{
  "member_id": 150,
  "msisdn": "4740485124",
  "email": "recipient@example.com",
  "app_token": "c6OE_joK28g:APA91bFkT5fzhdWJMJswPo3btVLxtMmhi7jKdOE_VL1IhkSmuoz3iEQMg4Cq",
  "properties": { "first_name": "Piotr" }
}
```

Key | Type | Description
--------- | --------- | ---------
member_id | integer | ID of MPC member
msisdn | [MSISDN](#msisdn-param) |
email | email |
app_token | email | 
properties | Object | Arbitrary keys that can be used as merge-fields in the message template

<br />
Recipient may be an MPC member (identified by `member_id`) or an arbitrary sending receivers - `msisdn`, `email` or `push`.  

#### MPC member recipient

When `member_id` is provided, other identifiers will be ignored and actual identifiers for sending execution will 
be fetched from MPC member's data (for example, for `sms` channel member's `msisdn` will be used as a sending receiver).

Merge-fields for template will be fetched from MPC - extended by provided `properties`.

#### Arbitrary identifier recipient

Without `member_id`, at least one identifier relevant to message's channels definition is required.

For example, if `sms` and `email` channels are defined on message, `msisdn` or `email` is required.

Given `properties` will be used as template's merge-fields. 

### <a name="messaging-message-payload-model"></a> MessagePayload model

> MessagePayload example - it creates an SMS message which will be sent at given time to members of given audience:

```json
{
  "name": "My message", "shorten_urls": true, "track_in_shortener": true, "campaign_id" : 32,
  "channels": [
    {
      "type": "sms",
      "sender": { "type": "alphanumeric", "value": "Infinity" },
      "template": {
        "type": "inline",
        "data": { "type": "plain", "content": { "body": "Hi {{name}}" } }
      }
    }
  ],
  "sending": {
    "schedule": { "type": "absolute", "at": "2020-10-18T10:59:32.340Z" },
    "audience": { "type": "reference", "id" : 15 } 
  }
}
```

Key | Type | Description
--------- | -------- | ---------
**name** | string | 
shorten_urls | boolean | Should sendings for this message have URLs shortened? Default: `true`  
shorten_urls | boolean | Should MPC's Shortener track users? Default: `false`  
campaign_id | string | MPC's campaign ID
**channels** | [ChannelPayload](#messaging-channel-payload-model)[] | Channels the message should be sent with.
sending | [SendingPayload](#messaging-sending-payload-model) | Sending to schedule for the message 
<br />

### <a name="messaging-channel-payload-model"></a> ChannelPayload model

> ChannelPayload example:

```json
{
  "type": "sms",
  "sender": { "type": "alphanumeric", "value": "Infinity" },
  "template": {
    "type": "inline",
    "data": { "type": "plain", "content": { "body": "Hi {{first_name}}" } }
  }
}
```

Key | Type | Description
--------- | ---------  | ----
**type** | enum: `['sms', 'email', 'push']` |
**template.type** | enum: `['inline', 'reference']` |
template.id | integer | ID of Template to assign (when `reference`)
template.definition | [TemplatePayload](#messaging-template-payload-model) | Definition of Template to assign (when `inline`)
[(channel-specific attributes)](#messaging-channel-specific-attributes) | |

There are two ways to provide template:

* `inline` - you will need provide new template `definition` 
* `reference` - template definition will be copied from template identified by `id` 

### <a name="messaging-template-payload-model"></a> TemplatePayload model

> Template example:

```json
{
  "type": "plain",
  "wrapper_id": 9193,
  "content": { "body": "Hi {{first_name}}" }
}
```

Key | Type | Description
--------- | ---------  | --------
**type** | enum: `['plain', 'email', 'bee_email', 'push']` |  
**content** | Object | Depends on type. See: [Template model](#messaging-template-model)
wrapper_id | integer | ID of [wrapping template](#messaging-template-wrapping)


### <a name="messaging-sending-payload-model"></a> SendingPayload model

> Example #1 - schedules sending ASAP to audience defined by given conditions

```json
{
  "audience": {
    // Schedule is not provided, so message will be sent ASAP
    "type": "inline",
    "conditions": [
      {
        "condition_group": "member_properties", "field": "first_name", 
        "operator": "match", "value": "Piotr"
      }
    ]
  }
}
```

> Example #2 - schedules message sending 2 days from now to members of audience defined by ID and 2 arbitrary recipients

```json
{
  "schedule": { "type": "relative", "in": "2 days" },
  "audience": { "type": "inline", "id": 15 },
  "recipients": [
    { "member_id": 150, "properties": { "name": "Piotr" } },
    { "email": "jan@example.com", "properties": { "name": "Jan" } }
  ]
}
```

Key | Type | Description
--------- | --------- | -----
schedule | [SendingSchedulePayload](#messaging-sending-schedule-payload-model) |
audience | [SendingAudiencePayload](#messaging-sending-schedule-audience-model) |
recipients | [Recipient](#messaging-recipient-model) | 
priority | Integer (1-10) | Requires `Messages:Api:Sendings:SetPriority` permit 

<br />

There are two methods to define recipients of the sending:

* `audience` - members of the audience will be used as recipients
* `recipients` - inline receivers
 
Both methods can be used at the same time.

`schedule` is optional - when not provided, sending will be scheduled ASAP.

#### <a name="messaging-sending-schedule-payload-model"></a> SendingSchedulePayload model

> Example #1 - absolute schedule

```json
{ 
  "type": "absolute", 
  "at": "2020-10-18T10:59:32.340Z" 
}
```

> Example #2 - relative schedule

```json
{ 
  "type": "relative",
  "in": "5 minutes" 
}
```

Key | Type | Description
--------- | --------- | ---------
type | enum: `['absolute', 'relative']` |
at | ISO 8601 DateTime | For `absolute` type - defines exact time the sending should be executed at
in | string | For `relative` type - see below
<br />

**`in` format**

You can provide `number` of time `unit`s the sending will be scheduled at in format described by following 
regex: 
`^(?<number>\d+) (?<unit>second|minute|hour|day|week|month|year)s?$`.

Examples:

  * `30 minutes`
  * `1 day`
  * `5 months`

#### <a name="messaging-sending-audience-payload-model"></a> SendingAudiencePayload model

> Example #1 - audience by reference

```json
{ 
  "type": "inline", 
  "id": 532 
}
```

> Example #2 - audience by inline conditions

```json
{ 
  "type": "inline",
  "conditions": [
    {
      "condition_group": "member_properties", "field": "first_name", 
      "operator": "match", "value": "Piotr"
    }
  ]
}
```

Key | Type | Description
--------- | --------- | ---------
type | enum: `['reference', 'inline']` |
id | integer | For `reference` type - ID of audience the sending should be sent to
conditions | Object[] | For `inline` type - See [DMP docs](https://dmp.boostcom.no/docs/#conditions)
