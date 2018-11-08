# Endpoints &bull; Push Notifications

## <a name="v3-push-notification-create"></a> Create

> Example:

```shell
curl -X POST "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/push_notifications" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
  -d '
    {
      "notifications":[
        {
          "id":"push_1",
          "member_ids":[1, 2],
          "notification":{ "title": "Hey man", "body": "Great offer for you!" }
        },
        {
          "id": "push_2",
          "member_ids": [3],
          "shortening_enabled": true,
          "notification": { "body":"Great offer!" }
        }
      ]
    }
  '
```

> When successful (200), returns a hash structured like this:

```json
[
    {
        "id": "push_1",
        "results": [
            { "member_id": 1, "result": "sent" },
            { "member_id": 2, "result": "no_device" }
        ]
    },
    {
        "id": "push_2",
        "results": [
            { "member_id": 3, "result": "sent" }
        ]
    }
]

```

> When payload is invalid (422), returns a hash structured like this:

```ruby
{
    "error": "At least one notification is invalid",
    "notifications_errors": {
        "notifications[0]": {
            "notification": [
                "can't be blank"
            ]
        },
        "notifications[1]": {
            "notification": [
                "can't be blank"
            ],
            "user_assigned_id": [
                "must be a String (but is a Integer)"
            ]
        }
    }
}
``` 

**POST** `api/v3/loyalty_clubs/:loyalty_club_slug/push_notifications`

Sends notifications (100 max) to target members.

The notifications are sent from our systems with Firebase API, so for more information on `notification` and `data` parameters, see [Firebase docs](https://firebase.google.com/docs/cloud-messaging/http-server-ref) 

Not all target members may be eligible for receiving a push notification (or may not actually exist), so for every recipient 
in every notification a result (`sent|no_device`) is returned - see an example on the right

### POST Parameters (JSON array)

The expected payload consists of `messages` object which has array of objects with following attributes:

Key | Type | Description
--------- | --------- | ---------
id* | string | ID of message. It's value is up to you, but we suggest <br /> to make it unique so the messages may be distinguished
member_ids* | array | IDs of members the notification should be sent to
shortening_enabled | bool | Should links in payload get shortened with Boostcom Shortener?
notification* | object | Predefined, user-visible key-value pairs of the notification payload. <br /> For more information, see [Firebase docs](https://firebase.google.com/docs/cloud-messaging/http-server-ref#notification-payload-support)
notification['body'] | string | Body of push notification
notification['title'] | string | Title to be displayed in push notification
data | object | Custom key-value pairs of the message's payload. <br /> For more information, see [Firebase docs](https://firebase.google.com/docs/cloud-messaging/http-server-ref#data)

\* required

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid payload (see example on the right)

<aside class="notice">
Requires <code>BL:Api:PushNotifications:Send</code> permit
</aside>
