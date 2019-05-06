---
title: Member Changes Notifications
layout: slate

toc_footers:
  - <a href='https://loyalty.boostcom.no'>Performance Cloud</a>
  - <a href='http://boostcom.com/'>Boostcom</a>
  - <hr/>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

---

# Members Changes Notifications

A Loyalty Club may be configured to send notifications (as HTTP POST requests) when a change occurs within its member 
database (e.g. when member registers).

If you wish to configure a Loyalty Club to have such notifications enabled, please contact us us.
You will need to provide us a HTTPS (for security reasons, we don't support HTTP) URL that notifications will
be sent to.

## Subscription

As it is possible to have multiple endpoints configured for receiving notifications in one Loyalty Club, for each 
endpoint we setup a Subscription.

Each Subscription has its own Secret Token (see below) and may be configured to send notifications only on specific 
changes (`import`, `update`, `delete`).

## Authentication token

Each notification request is signed with `X-Secret-Token` header that contains Secret Token configured for given 
Subscription.

In order to assure that notifications you receive are sent from us, you should check if its value matches 
the Secret Token that you received from us when the subscription has been set up.

## Notifications format

> Example

```json
    {
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

Changes are always sent in bulks (however, in most cases the `bulk` array will consist of only one element).

Each change in the `bulk` is identified by following attributes:

Parameter | Type | Description
--------- | --------- | -----------
customer_name | String | Name of Loyalty Club
customer_id | Integer | ID of Loyalty Club
event_type | `import`, `update`, or `delete` | A type of the change
external_id | Integer | ID of member
properties | Object | Member properties. Each of them has a `type <String>` and `value <String>`. Empty when `event_type` is `delete`
consents | Object | Member consents and their values. Empty when `event_type` is `delete`.
created_at | Date | When the change occurred
optin_date | Date | When the member has been created

## Notifications delivery

Your endpoint must return a `2xx` (`200` or `201` preferably) HTTP status code. Only then the notification 
will be treated as delivered.

If your server returns `5xx` code, the delivery will be treated as failed, and our system will make another attempt later.
On consequent failures, deliveries will be attempted up to 20 days.

If your server returns a `4xx` code, the delivery will be ignored.
