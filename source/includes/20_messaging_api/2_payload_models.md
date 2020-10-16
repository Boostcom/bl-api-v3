
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
campaign_id | integer | MPC's Campaign ID
service     | string | [Service](#messaging-message-service) the Message is related to 
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

> Example #1 - Creates Sending without schedule to audience defined by given conditions

```json
{
  // Schedule is not provided, so message will not be scheduled
  "audience": {
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
draft | bool | If true (false, by default), the Sending is not scheduled for execution, even if schedule is present  
priority | Integer (1-10) | Requires `Messages:Api:Sendings:SetPriority` permit 
schedule | [SendingSchedulePayload](#messaging-sending-schedule-payload-model) |
audience | [SendingAudiencePayload](#messaging-sending-audience-payload-model) |
recipients | [Recipient](#messaging-recipient-model) | 

<br />

There are two methods to define recipients of the sending:

* `audience` - members of the audience will be used as recipients
* `recipients` - inline receivers
 
Both methods can be used at the same time.

`schedule` is optional - when not provided, sending will not be scheduled (will have `draft` status).

#### <a name="messaging-sending-schedule-payload-model"></a> SendingSchedulePayload model

> Example #1 - immediate schedule

```json
{ 
  "type": "immediate" 
}
```

> Example #2 - absolute schedule

```json
{ 
  "type": "absolute", 
  "at": "2020-10-18T10:59:32.340Z" 
}
```

> Example #3 - relative schedule

```json
{ 
  "type": "relative",
  "in": "5 minutes" 
}
```

Key | Type | Description
--------- | --------- | ---------
type | enum: `['immediate', 'absolute', 'relative']` |
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
