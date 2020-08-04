#  SMSes

## <a name="v3-smses-create"></a> Create

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/smses" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
    -d \
    '{
      "messages": [
		{ "id": "message:124", "member_id": 1, "content": "Hello" },
		{ "id": "message:125", "member_id": 2, "content": "What is going on?", "sender": "Marvin Gaye" }
      ]
    }'
```

> When successful (200), returns a feedback object structured like this:

```json
[
    {
        "id": "message:1",
        "result": "ok"
    },
    {
        "id": "message:2",
        "result": "no_consent"
    },
    {
        "id": "message:3",
        "result": "member_not_found"
    }
]

``` 

> When payload is invalid (422), returns an object structured like this:

```json
{
    "error": "Invalid parameters",
    "details": {
        "messages": {
            "0": {
                "sender": [
                    "it must have between 2-11 characters"
                ]
            },
            "2": {
                "ignore_consents": [
                    "must be boolean"
                ]
            }
        }
    }
}
``` 

**POST** `v3/:loyalty_club_slug/smses`

Sends SMSes (100 max) to target members.

Not all target members may be eligible for receiving an SMS, so for every recipient in every notification 
a feedback is returned - see an example on the right.

Please note that successful feedback does not mean that message has been sent right away.
Messages are being sent asynchronously. Also, it cannot be guaranteed that the message will be delivered to user.

In future, a feature for checking messages delivery statuses will be added.

### Messages POST Parameters (JSON array)

The expected payload consists of `messages` object which has array of objects with following attributes:

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
id * | string  | n/a | ID of message. It's value is up to you, but we suggest to make it unique so the messages may be distinguished
member_id * | integer | n/a | IDs of member the message should be sent to
content * | string  | n/a | The message content
sender | string  | Loyalty Club settings will be applied. | Text which will be perceived by user as message sender (2-11 characters). 
ignore_sending_window | bool | false | Should Loyalty Club sending window be ignored?
ignore_consents | bool | false | Should member consents be ignored?
ignore_msisdn_verification_status | bool | false | Should member MSISDN verification status be ignored?
shortening_enabled | bool | true | Should links in payload get shortened with Boostcom Shortener?

\* required

### Feedback statuses

Key | Description
---- | ----
ok | Message has been accepted for sending
member_not_found | Message with given ID does not exist
no_consent | Member has no `sms_marketing` consent - this may be ignored by enabling `ignore_consents` option 
msisdn_missing | Member has no MSISDN
msisdn_barred | Member's MSISDN is barred
msisdn_unverified | Member's MSISDN is not verified - this may be ignored by enabling `ignore_msisdn_verification_status` option
channel_disabled | Member has SMS channel disabled

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters (see example on the right)

<aside class="notice">
Requires <code>BL:Api:Smses:Send</code> permit
</aside>
