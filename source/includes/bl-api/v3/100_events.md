# Endpoints &bull; Events (WIP)

## <a name="v3-events-list"></a> List

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/events" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
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
      "open": true,
      "registered_members": 0,
      "checkin_members": 10,
      "checkin_time_limit": 15
  }
]
``` 

**GET** `v3/:loyalty_club_slug/events`

List of active events

<aside class="notice">
Requires <code>?</code> permit
</aside>

## <a name="v3-event-get"></a> Get

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/events/:event_id" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an event object structured like below. Some values may be omitted if were not provided:

```json
  {
      "id": 12345,
      "name": "Event name",
      "start_time": "2016-01-02 12:00:00",
      "end_time": "2016-01-02 18:00:00",
      "singup_time": "2016-01-01 12:00:00",
      "open": true,
      "registered_members": 0,
      "checkin_members": 10,
      "checkin_time_limit": 15
  }
``` 

**GET** `v3/:loyalty_club_slug/events`

Single event information

<aside class="notice">
Requires <code>?</code> permit
</aside>

## <a name="v3-events-validate-member"></a> Member validation

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/events/validation" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
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

**POST** `v3/:loyalty_club_slug/events/validate`

Check if member is registered to the event, should be uses only for closed events

<aside class="notice">
Requires <code>?</code> permit
</aside>

## <a name="v3-events-checkin-member"></a> Checkin

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/events/checkin" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
    -d \
    '{
        "member_id": 12345,
        "event_id": 1
    }
```

> When successful (200), returns an object structured as below:


```json
  {
    "member_id": 12345,
    "event_id": 1,
    "status": "ok",
    "checkin_members": 100,
    "last_checkin_time": "2016-02-02T02:02:02+0000",
    "checkin_time_alert": false
  }
``` 

**GET** `v3/:loyalty_club_slug/events/checkin`

Checkin/scan member to the event

<aside class="notice">
Requires <code>?</code> permit
</aside>

## <a name="v3-events-members-list"></a> Members list

> Example:

```shell
curl -X GET \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/events/members/:event_id" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful (200), When successful (200), returns an array of members objects structured as below.


```json
[
  {
    "member_id": 12345,
    "event_id": 1,
    "last_checkin_time": "2016-02-02T02:02:02+0000"
  }
]
``` 

**GET** `v3/:loyalty_club_slug/events/members`

List members who were scanned/checked in

<aside class="notice">
Requires <code>?</code> permit
</aside>

## <a name="v3-events-invitation"></a> Invitation

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/events/invitation" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
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

**GET** `v3/:loyalty_club_slug/events/invitation`

Send email invitation to loyalty club

<aside class="notice">
Requires <code>?</code> permit
</aside>