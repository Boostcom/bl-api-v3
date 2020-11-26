## Members Webhooks

A Loyalty Club may be configured to send notifications (as HTTP POST requests) when a change occurs within its member 
database (e.g. when member registers).

If you wish to configure a Loyalty Club to have such notifications enabled, please contact us us.
You will need to provide us a HTTPS (for security reasons, we don't support HTTP) URL that notifications will
be sent to.

### Subscription

As it is possible to have multiple endpoints configured for receiving notifications in one Loyalty Club, for each 
endpoint we setup a Subscription.

Each Subscription has its own Secret Token (see below) and may be configured to send notifications only on specific 
changes (`import`, `update`, `delete`).

### Authentication token

Each notification request is signed with `X-Secret-Token` header that contains Secret Token configured for given 
Subscription.

In order to assure that notifications you receive are sent from us, you should check if its value matches 
the Secret Token that you received from us when the subscription has been set up.

### Notifications format

Currently, notifications are generated in two versions. 
Version 1 is deprecated and new subscriptions are going to be configured to receive v2 notifications.

#### Notifications v2

> Example for v2

```json
{
  "version": 2,
  "events": [
    {
      "event": { "type": "update", "date": "2020-02-05T14:31:15.520Z" },
      "loyalty_club": { "id": 32, "slug": "infinity-mall" },
      "member": {
        "id": 11256,
        "person_id": 89103,
        "msisdn": null,
        "email": "john@example.com",
        "properties": { "first_name": "John", "last_name": "Doe", "zip_code": null },
        "consents": {
          "sms_marketing": { "updated_at": "2020-01-21T10:23:34.982Z", "value": true },
          "email_marketing": { "updated_at": "2020-02-05T14:31:15.520Z", "value": false }
        },
        "sms_status": "verified",
        "email_status": "enabled",
        "push_status": "disabled",
        "optin_channel": "ios",
        "optin_subchannel": null,
        "created_at": "2020-01-21T10:23:34.982Z",
        "updated_at": "2020-02-05T14:31:15.520Z"
      },
      "member_changes": {
        "msisdn": { "change": "-", "was": "4740769126", "is": null },
        "email": { "change": "+", "was": null, "is": "john@example.com" },
        "sms_status": { "change": "~", "was": "enabled", "is": "verified" },
        "push_status": { "change": "~", "was": "enabled", "is": "disabled" },
        "properties": {
          "first_name": { "change": "~", "was": "Mark", "is": "John" },
          "last_name": { "change": "+", "was": null,  "is": "Doe" },
          "zip_code": { "change": "-", "was": "3300", "is": null }
        },
        "consents": {
          "sms_marketing": { "change": "~", "was": false, "is": true }
        }
      }
    }
  ]
}
```

Each change in the `events` array consists of following objects:

Name | Description
---------  | -----------
event | See [Event model](#v2-event)
loyalty_club | Information about member's Loyalty Club - it's ID and slug. 
member  | Member attributes after the event - See [Member JSON model](https://docs.mpc.placewise.com/#introduction36). For `delete` events it contains only `id` and `person_id`
member_changes | See [Event model](#v2-event). It's empty for `delete` events

##### <a name="v2-event"></a> Event model

Contains meta information about the change event. 

Attribute | Type | Description
--------- | --------- | -----------
type | `import`, `update`, or `delete` | A type of the change event
date | Date | When the event occurred

##### <a name="v2-changes"></a> Changes object

Represents changes that happened to member within the event. Includes only properties that have been actually changed
within the event.

Changes object may contain following fields:

  * `msisdn`
  * `email`
  * `sms_status`
  * `email_status`
  * `push_status`
  * `optin_channel`
  * `optin_subchannel`
  * `consents`, which contains changes for consents 
  * `properties`, which contains changes for properties

Each change consists of three fields:

Attribute | Type | Description
--------- | --------- | -----------
change | `~`, `+` or `-` | Whether the property has been changed (`~`), added (`+`) or removed (`-`)
before | mixed | The value of property before the change
after | mixed | The value of property after the change

#### Notifications v1 (deprecated)

> Example for v1

```json
{
  "version": 1,
  "bulk": [
    {
      "customer_name": "Infinity Mall",
      "customer_id": 447,
      "event_type": "update",
      "external_id": 44882174,
      "properties": {
        "birthday":{ "type":"string","value":"1990-01-01" },
        "gender": { "type": "string", "value": "woman" },
        "language": { "type": "string", "value": "en" },
        "first_name": { "type": "string", "value": "John" },
        "last_name": { "type": "string", "value": "Doe" },
        "optin_channel": { "type": "string", "value": "webforms" },
        "msisdn": { "type": "string", "value": "4740769126" },
        "email": { "type": "string", "value": "j.doe@example.com" },
        "sms_status": { "type": "string", "value": "verified" },
        "email_status": { "type": "string", "value": "unsubscribed" },
        "push_status": { "type": "string", "value": "enabled" }
      },
      "consents": {
        "dmp_profiling": { "value": true },
        "sms_marketing": { "value": true },
        "email_marketing": { "value": false },
        "custom_consent": { "value": true }
      },
      "created_at": "2018-05-28T08:34:59.231Z",
      "optin_date": "2018-05-24T11:23:41.231Z"
    }
  ]
}
```

Each change in the `bulk` is identified by following attributes:

Attribute | Type | Description
--------- | --------- | -----------
customer_name | String | Name of Loyalty Club
customer_id | Integer | ID of Loyalty Club
event_type | `import`, `update`, or `delete` | A type of the change
external_id | Integer | ID of member
properties | Object | Member properties. Each of them has a `type <String>` and `value <String>`. When `event_type` is `delete`, contains only member identifiers (`msisdn`, `email`)
consents | Object | Member consents and their values. Empty when `event_type` is `delete`.
created_at | Date | When the change occurred
optin_date | Date | When the member has been created

### Notifications delivery

Your endpoint must return a `2xx` (`200` or `201` preferably) HTTP status code. Only then the notification 
will be treated as delivered.

If your server returns `5xx` code, the delivery will be treated as failed, and our system will make another attempt later.
On consequent failures, deliveries will be attempted up to 20 days.

If your server returns a `4xx` code, the delivery will be ignored.
