# Events API

<aside class="warning">
This API is in development. Therefore, it may not be ready for use and is a subject to change at any time.
</aside>

##  Events

### <a name="events-list"></a> List

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events?menuItem=slug" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an array of events objects structured like below:

```json
[
  {
      "id": 12345,
      "name": "Event name",
      "start_time": "2016-01-02 12:00:00",
      "end_time": "2016-01-02 18:00:00",
      "singup_time": "2016-01-01 12:00:00",
      "place": "Place",
      "open": true,
      "registered_members": 0,
      "spots": 0,
      "checkin_members": 10,
      "checkin_time_limit": 15,
      "status": "active"
  }
]
``` 

**GET** `v3/:loyalty_club_slug/events`

List of events

#### Parameters

Parameter | Description | Type
--------- | ----------- | ------
menuItem  | Menu item slug | string

All parameters are optional

<aside class="notice">
Requires <code>Events:Api:Events:Get</code> permit
</aside>

### <a name="event-get"></a> Get

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/:event_id" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an event object structured like below. Some values may be omitted if were not provided:

```json
  {
      "id": 12345,
      "name": "Event name",
      "start_time": "2016-01-02 12:00:00",
      "end_time": "2016-01-02 18:00:00",
      "singup_time": "2016-01-01 12:00:00",
      "place": "Place",
      "open": true,
      "registered_members": 0,
      "spots": 0,
      "checkin_members": 10,
      "checkin_time_limit": 15,
      "status": "active"
  }
``` 

**GET** `v3/:loyalty_club_slug/events/:event_id`

Single event information

<aside class="notice">
Requires <code>Events:Api:Events:Get</code> permit
</aside>

### <a name="events-validate-member"></a> Member validation

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/validation" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
        "member_id": 12345,
        "event_id": 1
    }
```

> When successful (200), returns an object structured as below

```json
  {
    "status": "ok",
    "member_id": 12345,
    "event_id": 1
  }
``` 

> When `event_id` is invalid (422), returns:

```json
{
    "error": "Event not found"
}
``` 

**POST** `v3/:loyalty_club_slug/events/validate`

Check if member is registered to the event, should be uses only for closed events

<aside class="notice">
Requires <code>Events:Api:Validation:Create</code> permit
</aside>

#### Parameters

Parameter | Description | Type
--------- | ----------- | ------
event_id  | Event id    | integer
member_id | Member id   | integer

#### Error responses

Status    | Description
--------- | ----------- 
`422`     | Invalid parameters (see example on the right)

## Checkins

### <a name="events-checkin-member"></a> Create

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/checkin" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
        "member_id": 12345,
        "event_id": 1,
        "error": null
    }
```

> When successful (200), returns an object structured as below:


```json
  {
    "member_id": 12345,
    "event_id": 1,
    "status": "ok",
    "checkin_members": 100,
    "checkin_time": "2016-02-02T02:02:02+0000",
    "checkin_time_alert": false,
    "error": null
  }
```

> When `event_id` is invalid (422), returns:

```json
{
    "error": "Event not found"
}
``` 

Checkin/scan member to the event

<aside class="notice">
Requires <code>Events:Api:Checkin:Create</code> permit
</aside> 

#### Parameters

Parameter | Description | Type         | Comments
--------- | ----------- | -------      | ---------
event_id  | Event id    | integer      |
member_id | Member id   | integer      |
error     | Error code  | integer/null | 1 - not a member, 2 - member not registered to the event, 3 - member scanned in the time limit 

#### Error responses

Status    | Description
--------- | ----------- 
`422`     | Invalid parameters (see example on the right)


**GET** `v3/:loyalty_club_slug/events/checkin`

### <a name="events-checkin-list"></a> List

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/checkin/:event_id" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), When successful (200), returns an array of members objects structured as below.


```json
[
  {
    "member_id": 12345,
    "checkin_time": "2016-02-02T02:02:02+0000",
    "error": null
  }
]
``` 

**GET** `v3/:loyalty_club_slug/events/checkin/:event_id`

List scanned/checked in events

<aside class="notice">
Requires <code>Events:Api:Checkin:Get</code> permit
</aside>

## Invitations

### <a name="events-invitation"></a> Send invitation

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/invitation" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
        "email": "email@example.com",
        "event_id": 1
    }
```

> When successful (200), returns an object structured like below:

```json
    {
      "status": "ok"
    }
```

> When `event_id` is invalid (422), returns:

```json
{
    "error": "Event not found"
}
```  

**GET** `v3/:loyalty_club_slug/events/invitation`

Send email invitation to loyalty club

<aside class="notice">
Requires <code>Events:Api:Invitation:Create</code> permit
</aside>

#### Payload Parameters

Parameter | Description | Type
--------- | ----------- | ------
event_id  | Event id    | integer
email     | Non-member email   | string


#### Error responses

Status    | Description
--------- | ----------- 
`422`     | Invalid parameters (see example on the right)


## <a name="events-registration"></a> Registration

### <a name="events-registration-list"></a> List

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/registration" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an array of objects structured like below:

```json
[
  {
    "id": 1,
    "name": "Events",
    "events": [
      {
        "id": 1,
        "name": "Test",
        "article_id": 1097,
        "title": "Test",
        "lead": "Test Lead",
        "start_date": "2020-09-01 16:24:00",
        "end_date": "2020-09-01 16:24:00",
        "calendar_url": "/v3/infinity-mall/events/registration/1/calendar",
        "image": "image-url.jpg",
        "place": "1st",
        "open": false
      }
    ]
  },
  {
    "id": 2,
    "name": "News",
    "events": [
      {
        "id": 3,
        "name": "Test 2",
        "article_id": 1099,
        "title": "Test 2",
        "lead": "Test 2 Lead",
        "start_date": "2020-09-02 11:25:00",
        "end_date": "2020-09-02 11:25:00",
        "calendar_url": "/v3/infinity-mall/events/registration/3/calendar",
        "image": "image-url.jpg",
        "place": "test2",
        "open": false
      }
    ]
  }
]
```


**GET** `v3/:loyalty_club_slug/events/registration?menuItem=:menu_item&photoSize=:photo_size`

Get events list for registration

Fetch events grouped by categories. List could be filtered by menu item

<aside class="notice">
Requires <code>Events:Api:Registration:Get</code> permit
</aside>

#### Payload Parameters

Parameter | Description | Type
--------- | ----------- | ------
menu_item  | Configured menu item    | string
photo_size     | Image size  | enum: ["thumb", "large", "medium", "original"]


### <a name="events-registration-details"></a> Get

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/registration/:event_id \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an object structured like below:

```json
{
  "id": 1,
  "name": "Test",
  "article_id": 1,
  "title": "Title",
  "lead": "Lead",
  "content": "<p>Article content</p>",
  "start_date": "2020-09-01 16:24:00",
  "end_date": "2020-09-01 16:24:00",
  "calendar_url": "/v3/infinity-mall/events/registration/1/calendar",
  "image": "image-url.jpg",
  "place": "1st",
  "open": false,
  "type": "first_come",
  "available_spots": 5,
  "signup_deadline": "2021-02-12 16:24:00"
}
```

> When `event_id` is invalid (422), returns:

```json
{
  "error": "Event does not exist"
}
```  

**GET** `v3/:loyalty_club_slug/events/registration/:event_id`

Get event details

<aside class="notice">
Requires <code>Events:Api:Registration:Get</code> permit
</aside>

#### Payload Parameters

Parameter | Description | Type
--------- | ----------- | ------
event_id  | Event ID    | integer


#### Error responses

Status    | Description
--------- | -----------
`422`     | Invalid parameters (see example on the right)


### <a name="events-registration-calendar"></a> Calendar

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/registration/:event_id/calendar \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an event.ics file:

```yaml
BEGIN:VCALENDAR
VERSION:2.0
BEGIN:VEVENT
DTSTAMP:20210122T114500Z
LOCATION:1st
DTSTART:20200901T162400
DTEND:20200901T162400
SUMMARY:Test
END:VEVENT
END:VCALENDAR
```

> When `event_id` is invalid (422), returns:

```json
{
  "error": "Event does not exist"
}
```  

**GET** `v3/:loyalty_club_slug/events/registration/:event_id/calendar`

Get event calendar file

<aside class="notice">
Requires <code>Events:Api:Registration:Get</code> permit
</aside>

#### Payload Parameters

Parameter | Description | Type
--------- | ----------- | ------
event_id  | Event ID    | integer


#### Error responses

Status    | Description
--------- | -----------
`422`     | Invalid parameters (see example on the right)

### <a name="events-registration-register"></a> Register

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/registration/:event_id/:member_id \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
        "action": "register"
    }
```

> When successful (200), returns an object structured like below:

```json
{
  "status": "ok"
}
```

> When `event_id` is invalid (422), returns:

```json
{
  "error": "Event does not exist"
}
```  

**POST** `v3/:loyalty_club_slug/events/registration/:event_id/:member_id`

Register on event

<aside class="notice">
Requires <code>Events:Api:Registration:Create</code> permit
</aside>

#### Payload Parameters

Parameter | Description | Type
--------- | ----------- | ------
event_id  | Event ID    | integer
member_id  | Member ID    | integer
action    | Register type |     enum: ["register","waiting_list"]

#### Error responses

Status    | Description
--------- | -----------
`422`     | Invalid parameters (see example on the right)

### <a name="events-registration-unregister"></a> Unregister

> Example:

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/registration/:event_id/:member_id \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an object structured like below:

```json
{
  "status": "ok"
}
```

> When `event_id` is invalid (422), returns:

```json
{
  "error": "Event does not exist"
}
```  

**DELETE** `v3/:loyalty_club_slug/events/registration/:event_id/:member_id`

Unregister from an event

<aside class="notice">
Requires <code>Events:Api:Registration:Delete</code> permit
</aside>

#### Payload Parameters

Parameter | Description | Type
--------- | ----------- | ------
event_id  | Event ID    | integer
member_id  | Member ID    | integer
action    | enum | string

#### Error responses

Status    | Description
--------- | -----------
`422`     | Invalid parameters (see example on the right)

### <a name="events-registration-actions"></a> Event - Member available actions

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events/registration/:event_id/:member_id \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an object structured like below: (non-registered member)

```json
{
  "member_status": null,
  "actions": [
    {
      "name": "register",
      "default_text": "Register",
      "request_type": "POST",
      "options": [
        {
          "type": "checkbox",
          "name": "invite",
          "available_values": [
            "y",
            "n"
          ],
          "default_value": "n",
          "required": false
        }
      ]
    }
  ]
}
```

> When successful (200), returns an object structured like below: (registered member)

```json
{
  "member_status": {
    "status": "coming",
    "guest": false
  },
  "actions": [
    {
      "name": "register",
      "default_text": "Unregister",
      "request_type": "DELETE",
      "options": null
    }
  ]
}
```

> When `event_id` is invalid (422), returns:

```json
{
  "error": "Event does not exist"
}
```  

**GET** `v3/:loyalty_club_slug/events/registration/:event_id/:member_id`

Get available actions

<aside class="notice">
Requires <code>Events:Api:Registration:Get</code> permit
</aside>

#### Payload Parameters

Parameter | Description | Type
--------- | ----------- | ------
event_id  | Event ID    | integer
member_id  | Member ID    | integer

#### Error responses

Status    | Description
--------- | -----------
`422`     | Invalid parameters (see example on the right)