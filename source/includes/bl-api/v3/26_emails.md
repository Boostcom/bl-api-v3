# Endpoints &bull; Emails

## <a name="v3-emails-create"></a> Create

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/emails" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
    -d \
    '{ 
      "emails": {
          "id": "email_1", 
          "content": "Hello",
          "subject": "from api", 
          "from_name":"Boost",
          "from_email":"noreply@boost.no",
          "members_ids": [4175287, 123], 
        }
      }'
```

> When successful (200), returns a feedback object structured like this:

```json
[
  {
      "id": "email_1",
      "member_id": 4175287,
      "result": "ok"
  },
  {
      "id": "email_1",
      "member_id": 123,
      "result": "member_not_found"
  }
]
``` 

> When payload is invalid (422), returns an object structured like this:

```json
{"error": "Invalid parameters",
    "details": {
        "emails": {
            "members_ids": [
                "size must be within 1 - 100"
            ], 
            "from_email": [
                "is in invalid format"
            ]
        }
    }
}
``` 

**POST** `v3/:loyalty_club_slug/emails`

Sends Emails (100 max) to target members.

Not all target members may be eligible for receiving an Email, so for every recipient in every notification 
a feedback is returned - see an example on the right.

Please note that successful feedback does not mean that message has been sent right away.
Messages are being sent asynchronously. Also, it cannot be guaranteed that the message will be delivered to user.

In future, a feature for checking messages delivery statuses will be added.

### Messages POST Parameters (JSON array)

The expected payload consists of `emails` object which has an hash of objects with following attributes:

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
id * | string  | n/a | ID of message. It's value is up to you, but we suggest to make it unique so the messages may be distinguished
members_ids * | int[] | n/a | IDs of members the message should be sent to
content * | string  | n/a | The message content
subject* | string  | n/a | *Subject* field. 
from_name | string | Loyalty Club settings wil be applied.| *From* field.
from_email | string | Loyalty Club settings wil be applied.| *Sender* field. Must be a valid (and verified - contact support@boostcom.no in case you don't have any verified domain) email address.
ignore_consents | bool | false | Should member consents be ignored?
ignore_email_verification_status | bool | false | Should member Email verification status be ignored?
shortening_enabled | bool | Loyalty Club settings will be applied. | Should links in payload get shortened with Boostcom Shortener?

\* required

### Feedback statuses

Key | Description
---- | ----
ok | Message has been accepted for sending
member_not_found | Message with given ID does not exist
no_consent | Member has no `email_marketing` consent - this may be ignored by enabling `ignore_consents` option 
email_missing | Member has no Email
email_unsubscribed | Members has unsubscribed from receiving emails
email_hard_bounced | Member's Email is marked as a hard bounced
email_unverified | Member's Email is not verified - this may be ignored by enabling `ignore_email_verification_status` option
channel_disabled | Member has Email channel disabled

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>BL:Api:Emails:Send</code> permit
</aside>
