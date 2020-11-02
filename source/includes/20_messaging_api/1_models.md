# <a name="messaging"></a> Messaging API

<aside class="warning">
This API is under development. Therefore, it may not be ready for use and is a subject to change at any time.
</aside>

## <a name="messaging-models"></a> Common models

### <a name="messaging-message-model"></a> Message model

> Message example:

```json
{
  "id": 758131,
  "name": "DD3",
  "shorten_urls": true,
  "track_in_shortener": false,
  "service": null,
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
        "wrapper_id": 3491,
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
**shorten_urls** | boolean | Should sendings for this message have URLs shortened with MPC Shortener? 
**track_in_shortener** | boolean | Should MPC Shortener track users?
service | enum | MPC [Service](#messaging-message-service) the Message is related to
campaign_id | integer | MPC's campaign ID (applicable to `campaigns` service)
**channels** | [Channel](#messaging-channel-model)[] | Channels the message is sent with
**sendings_status** | enum | [Aggregated Sendings status](#messaging-message-sendings-status)
**sendings_count** | integer | Number of Sendings of Message
**latest_sending** | [MessageSending](#messaging-message-sending-model) | Most recent Sending of Message
**created_at** | datetime | Time of creation
**updated_at** | datetime | Time of last update

##### <a name="messaging-message-sendings-status"></a> Aggregated Sendings status

If all Message's Sendings are `transmitted`, `cancelled`, `failed` or `skipped`, then `sendings_status` is `finished`.

If all Message's Sendings are `draft`, then `sendings_status` is `draft`.

If both above conditions are not true, then `sendings_status` is `scheduled`.

##### <a name="messaging-message-sending-model"></a> MessageSending model

Simplified version of [Sending](#messaging-sending-model)'s with limited set of attributes:

Key  | Description
--------- | ---------
**id** | [Sending#id](#messaging-sending-model)
audience_id | [Sending#audience_id](#messaging-sending-model)
**status** | [Sending#status](#messaging-sending-model)
**scheduled_at** | [Sending#scheduled_at](#messaging-sending-model)
**created_at** | [Sending#created_at](#messaging-sending-model) 
**updated_at** | [Sending#updated_at](#messaging-sending-model)
**recipients_count** | Number of recipients 

#### <a name="messaging-message-service"></a> Service

Service is used to provide special treatment for some types of Messages that are bound to specific MPC features.

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
wrapper_id | integer | ID of [wrapper template](#messaging-template-wrapping)  
**type** | enum: `['plain', 'email', 'bee_email', 'push', 'generic']` |
**content** | Object | Definition depends on the `type`. See below
**created_at** | datetime | Time of creation
**updated_at** | datetime | Time of last update

<div class="clear"></div>

#### <a name="messaging-template-model-plain"></a> `plain` template

> Plain Template example:

```json
{
  "id": 609193,
  "wrapper_id": null,
  "type": "plain",
  "content": { "subject": "Important message for {{name}}", "body": "Hei {{name}}" },
  "created_at": "2020-09-15T12:45:08.618Z",
  "updated_at": "2020-09-15T12:45:08.618Z"
}
```

This type of template contains only body and can be sent with any channel.

Key | Type | Description
--------- | --------- | ----------
**content.body** | string |

<div class="clear"></div>

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

<div class="clear"></div>

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

<div class="clear"></div>

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

<div class="clear"></div>

#### <a name="messaging-template-model-generic"></a> `generic` template

> Generic Template example:

```json
{
  "id": 609193,
  "wrapper_id": null,
  "type": "generic",
  "content": { "subject": "Welcome", "body": "Hi {{name}}!", "uri": "iml://app/cpn" },
  "created_at": "2020-09-15T12:45:08.618Z",
  "updated_at": "2020-09-15T12:45:08.618Z"
}
```

This type of template also can be assigned to any channel. But any possible content attribute can be set.

It's designed to be used as a generic wrapper or in appliances where the template needs to be utilized by messages
sent with various channels.

Key | Type 
--------- | ---------
content.body | string
content.subject | string
content.uri | URL

#### <a name="messaging-template-templating-system"></a> Templating system

We use [Liquid](https://shopify.github.io/liquid) template language for generating dynamic Message content.

NOTE: Currently, because of technical reasons, in e-mail messages very limited set of Liquid features is supported - 
it's demonstrated in examples of this section. However, in pushes and SMSes, full set of features is available.

The set of merge-tags available for given Message depends on recipient type the Message is being sent to.

> Example: Having this as a body of SMS sent to MPC members  

```text
{% if dmp.bonus_points %}
    Your points balance is {{ member.bonus_points }}! :)
{% else %}
    You have no points :(
{% endif %}
```    

> A following text will be received by member that has some points:

```text
Your points balance is 540 :)
```

> A the following text will be received by member that has no points:

```text
You have no points :(
```

For MPC members (fetched from [audience](#messaging-sending-audience-payload-model) or provided as [MPC members recipients](#messaging-mpc-recipient-model)),
their MPC data may be used as merge-tags:

merge-tag  | Description 
---------- | ------- 
`member.id` |  
`member.secret_id`  | Special member's ID of used to authorize member in some MPC services 
`member.msisdn`  | 
`member.email` | 
`member.app_token`| 
`member.*` | Member property (e.g. `member.first_name`) - the set depends on LC configuration
`dmp.*` | DMP value for member (e.g. `dmp.bonus_points`) - available for audience members only, <br />the set depends on LC configuration

<div class="clearfix"></div>

For [arbitrary recipients](#messaging-arbitrary-recipient-model), only their identifiers and arbitrary data provided 
within `properties`  may be used - see example.

> Having arbitrary recipients provided as follows: 

```json
[
  { "email": "piotr@example.com", "properties": { "first_name": "Piotr", "some_object": { "points": 15} } },
  { "email": "ola@example.com", "properties": { "first_name": "Ola", "some_object": { "points": 42 } } }
]
```

> A Template's content may be defined this way:

```text
Hello {{ first_name }}.
Your email is {{ email }}.
Your points balance is {{ some_object.points }}.
```

<div class="clear"></div>

##### <a name="messaging-template-wrapping"></a> Template wrapping

> Sample wrapper definition:

```json
{
  "id": 609193,
  "wrapper_id": null,
  "type": "generic",
  "content": { 
    "subject": "Important message for {{member.first_name}}", 
    "body": "Hi {{member.name}}! {{content}}",
    "uri": null // Empty, so it won't be used on wrapping
  }
}
```

> Consider this wrapper to be assigned to a template as follows:

```json
{
  "id": 609194,
  "wrapper_id": 609193,
  "type": "push",
  "content": { 
    "subject": "I will be replaced by wrapper, as wrapper has no {{content} defined for this property", 
    "body": "Your last name is {{member.last_name}}!",
    "uri": "foo://bar"
  }
}
```

> Effectively, following content will be used during sending execution after wrapping and merging:

```json
{ 
  "subject": "Important message for Piotr", 
  "body": "Hi Piotr! Your last name is Świtlicki!",
  "uri": "foo://bar"
}
```

Any template may be wrapped by another template by providing `wrapper_id`.

On sending execution, if channel's template has a wrapper assigned, each content attribute (body, subject, etc) 
of the template will be injected into special `{{ content }}` merge-tag of corresponding wrapper's attribute.

When wrapper has no specific content attribute defined, it will be ignored. 

Wrapping is not recursive, so if template A has a template B as a wrapper and template B has a template C as a wrapper,
template A will be wrapped by Template B, but template B will not be wrapped by Template C.

The type of wrapper that can be assigned to a template depends on given template type, as follows:

Template type | Wrappable with
------------- | -------------- 
`plain`       | `generic`, `email`, `bee_email`, `push`, `plain`
`generic`     | `generic`, `email`, `bee_email`, `push`
`email`       | `generic`, `email`, `bee_email`
`push`        | `generic`, `push`
`bee_email`   | `generic`

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

> Recipient example:

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
properties | Object | Arbitrary keys that can be used as [merge-tags](#messaging-templating-system) in the message template

<br />
Recipient may be an MPC member (identified by `member_id`) or an arbitrary sending receivers - `msisdn`, `email` or `push`.  

#### <a name="messaging-mpc-recipient-model"></a> MPC member recipient

When `member_id` is provided, other identifiers will be ignored and actual identifiers for sending execution will 
be fetched from MPC member's data (for example, for `sms` channel member's `msisdn` will be used as a sending receiver).

merge-tags for template will be fetched from MPC Members - but may be extended/replaced with provided `properties`.

#### <a name="messaging-arbtrary-recipient-model"></a> Arbitrary identifier recipient

Without `member_id`, at least one identifier relevant to message's channels definition is required.

For example, if `sms` and `email` channels are defined on message, `msisdn` or `email` is required.

Given `properties` will be used as template's merge-tags. 
