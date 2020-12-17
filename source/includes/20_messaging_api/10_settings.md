## <a name="messaging-settings"></a> Settings

### <a name="messaging-show-settings"></a> Show Settings

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/messages/settings" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing settings 

```json
{
  "settings": {
    "channels": {
      "sms": {
        "sender_type": "alphanumeric",
        "sender_value": "Infinity"
      },
      "email": {
        "from_name": "Infinity",
        "from_email": "infinity-mall@placewise.com"
      },
      "push": {
        "android_app_id": "no.bstcm.loyaltyapp.infinity_mall",
        "ios_app_id": "1231452117"
      }
    }
  }
}
```

**GET** `v1/messages/settings`

Returns Loyalty Club settings related to Message sending.

#### Response (JSON object)

Key                                     | Type   | Description
---                                     | ---    | ---
`settings.channels`                     | Object | Contains information about channels. When given channel is not available for LC, it's `null`.
`settings.channels.sms.sender_type`     | string | See [Channel-specific attributes](#messaging-channel-specific-attributes)
`settings.channels.sms.sender_value`    | string | See [Channel-specific attributes](#messaging-channel-specific-attributes)
`settings.channels.email.from_name`     | string | See [Channel-specific attributes](#messaging-channel-specific-attributes)
`settings.channels.email.from_email`    | string | See [Channel-specific attributes](#messaging-channel-specific-attributes)
`settings.channels.push.android_app_id` | string | ID of android app configured for push messages receiving
`settings.channels.push.ios_app_id`     | string | ID of ios app configured for push messages receiving

<aside class="notice">
Requires <code>Messages:Api:Settings:Show</code> permit
</aside>
