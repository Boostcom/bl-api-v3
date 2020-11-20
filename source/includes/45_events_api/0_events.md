# Events API

<aside class="warning">
This API is in development. Therefore, it may not be ready for use and is a subject to change at any time.
</aside>

##  Events

### <a name="events-list"></a> List

> Example:

```shell
curl -X GET \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/events" \
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
