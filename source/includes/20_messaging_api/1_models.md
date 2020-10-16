# <a name="messaging"></a> Messaging API

## <a name="messaging-models"></a> Models

### <a name="messaging-message-model"></a> Message model

> Message example:

```json
{
  "id": 758131,
  "name": "DD3",
  "shorten_urls": true,
  "track_in_shortener": false,
  "campaign_id": null,
  "service": null,
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
  "sendings_status": "scheduled",
  "sendings_count": 3,
  "latest_sending": {
    "id": 3920,
    "status": "scheduled",
    "audience_id": 107103,
    "recipients_count": 83,
    "scheduled_at": "2020-10-15T15:27:02.853Z",
    "created_at": "2020-09-15T12:45:08.625Z",
    "updated_at": "2020-09-15T12:45:08.625Z"
  }
}
```

Message stores business-level information and configuration.

It has [Templates](#messaging-template-model) through [Channels](#messaging-channel-model) (currently only one is supported). 

It may be sent, more than once, by scheduling [Sendings](#messaging-sending-model) to specific recipients.

Key | Type | Description
--------- | --------- | ---------
**id** | integer |   
**name** | string |
**shorten_urls** | boolean | Should sendings for this message have URLs shortened with MPC's Shortener? 
**track_in_shortener** | boolean | Should MPC's Shortener track users?
campaign_id | string | MPC's campaign ID
service | string| MPC [Service](#messaging-message-service) the Message is related to
**channels** | [Channel](#messaging-channel-model)[] | Channels the message is sent with
**sendings_status** | string | [Aggregated Sendings status](#messaging-message-sendings-status)
**sendings_count** | integer | Number of Sendings of Message
**latest_sending** | [MessageSending](#messaging-message-sending-model)[] | Most recent Sending of Message
**created_at** | datetime | Time of creation
**updated_at** | datetime | Time of last update

##### <a name="messaging-message-sendings-status"></a> Aggregated Sendings status

If all Message's Sendings are `transmitted`, `cancelled`, `failed` or `skipped`, then `sending_status` is `finished`.

If all Message's Sendings are `draft`, then `sending_status` is `draft`.

If both above conditions are not true, then `sending_status` is `scheduled`

##### <a name="messaging-message-sendings-status"></a> MessageSending model

Contains limited set of [Sending](#messaging-sending-model)'s attributes:

* id
* status
* audience_id
* scheduled_at
* created_at
* updated_at

Also, returns `recipients_count` instead of presenting actual Recipients.

#### <a name="messaging-message-service"></a> Service

Service is used to provide special treatment for some types of Messages.

A permit is required for managing Messages of each Service.

Service   | Permit                                     | Description
--------- | -----------------------------------------  | -----------------------------------------
campaigns | Messages:Api:Messages:UseService:Campaigns | For MPC Campaigns Messages
tenants   | Messages:Api:Messages:UseService:Tenants   | For MPC Tenant Engagement Module Messages

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

Channel defines how [Message](#messaging-message-model) may be sent along with the content (defined by [Template](#messaging-template-model)) that 
should be sent through this channel.

Please note that while we allow to set multiple channels on single message for future usage, 
currently just one (the first) is utilized as the Message's single channel.

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

Template describes content of the [Message](#messaging-message-model). 

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
  "content": { "subject": "Welcome", "body": "Hi {{name}}!", "uri": "iml://app/cpn" },
  "created_at": "2020-09-15T12:45:08.618Z",
  "updated_at": "2020-09-15T12:45:08.618Z"
}
```

This type of template can be assigned to `push` channel.

Key | Type 
--------- | ---------
**content.body** | string
content.subject | string
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
  "status": "scheduled",
  "scheduled_at": "2020-09-18T09:28:59.918Z",
  "scheduled_with": {
    "in": "15 minutes",
    "type": "relative"
  },
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
**status** | string | See: [Sending status](#messaging-sending-status)
**scheduled_at** | datetime | Time of sending execution
scheduled_with | Object | Params the sending has been scheduled with - See [SendingSchedulePayload](#messaging-sending-schedule-payload-model)
**created_at** | datetime | Time of creation
**updated_at** | datetime | Time of update
**recipients** | [Recipient](#messaging-recipient-model)[] | Inline recipients the sending is directed to

#### <a name="messaging-sending-status"></a> Sending status

The Sending may have one of following statuses, which may be grouped into **initial states**, **execution steps** and the **final outcomes**:

* **initial states**:
    * **draft** - When API client didn't't define the schedule (or has `draft: true` flag)
    * **scheduled** - When scheduled to be sent at specific time by API client

Sending may have status changed from `draft` to `scheduled`  and vice versa, as long as the execution haven't started yet.  

* **execution steps**:
    * **preparing** - Building Dispatches for Recipients and Audience members
    * **prepared** - Dispatches are built, ready for transmission
    * **transmitting** - Dispatches are being transmitted

Preparing is a process that builds Dispatches for given recipients. Potentially Sendings may have very large Audiences assigned, 
so it may take some time to build Dispatches for all of them. Therefore, the preparation starts 5 minutes before the actual transmission.

* **final outcomes**: 
    * **transmitted** - When the Dispatches have been transmitted
    * **skipped** - When no recipients could be resolved for it (for example, Audience may lose its members over time).
    * **cancelled** - When cancelled by API client at any point of execution (see: [Sending cancellation](#todo-cancel-endpoint).
    * **failed** - When the execution couldn't be finished in about 20 minutes after the desired time 

Whenever Sending is `cancelled` or `failed`, it cannot be rescheduled again. 
It must be created again, for example with [Clone sending endpoint](#todo). 

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
