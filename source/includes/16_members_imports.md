# Endpoints &bull; Members Imports

## <a name="v3-members-imports-import-flow"></a> Import flow

#### Import bulks

At most 1000 member records may be sent in one request.

If you intend to import more than that, you may group them by `import_id`, so multiple bulks will be grouped into one import.

Nevertheless, keep in mind, that import always consists of at least one bulk.

#### Import statuses

Each bulk is processed by an asynchronous job, so you need to check it's status some time after sending a request.

For that, you should firstly check the import status [here](#v3-members-imports-status) and then 
check bulks details for each bulk [here](#v3-members-imports-bulk-status).

Please note that:

* We recommend to check status in intervals not smaller than 5 seconds
* In extreme cases, it make take even few hours for bulk to be fully processed
* 24 hours after import request any potential personal data (contained in `member_payloads` and `errors`) is permanently removed from server

#### Bulk statuses

When bulk is created, it gets `waiting` status.

When bulk has been picked by asynchronous job, it transitions into `working` status.
Depending on the server load, it may get picked instantly or even after few hours in extreme cases.

When the job has been executed successfully (which make take up to few minutes), bulk will have `finished` status assigned.
Please note that it doesn't mean that members have been successfully processed (there may be even none), you'll need to check bulk status
for errors.

When job failed for some internal reason, it will have either `failed` or `waiting_for_retry` status. See below
for more details.  

#### Asynchronous job failures

Jobs that couldn't be processed because of some internal reasons will be retried up to 5 times.

After each failure, the bulk will have `waiting_for_retry` status assigned and it's `retries` attribute will be incremented.

After 5th failure, the bulk will have `failed` status assigned. In such case, please contact us, so we may identify 
and resolve the problem.

## <a name="v3-members-imports"></a> Send import request

**POST** `api/v3/loyalty_clubs/:loyalty_club_slug/members/imports`

Creates new members or updates existing ones (based on `msisdn` and `e-mail` identifiers).

There is a maximum number of members that can be sent in one request (1000). So, if you intend to import more than 
that, you may want to identify all bulks with the same `import_id` and pass consequent `request_number` to each of them.

> Example:

```shell
curl -X POST \
    "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/imports" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
  -d \
    '{
      "import_id": "import_21",
      "request_number": 3,
      "only_create": false,
      "members": [
        {
          "properties": { "email": "foo@bar.baz" },
          "password": "pass",
          "sms_enabled": true,
          "email_enabled": true,
          "push_enabled": true,
          "optin_channel": true,
          "optin_subchannel": true,
          "created_at": "foo",
          "updated_at": "bar",
          "consents": { "email_marketing": { "status": true } }
        }
      ]
    }'    
```

> When successful, JSON will be returned:

```json
{
  "import_id": "a9a00143-6488-4d4f-87d4-29c2da02d678"
}
```

> When input is invalid, returns the error object:

```json
{
  "error": "duplicated_msisdns"
}
```

### POST Parameters (JSON)

Parameter | Required? | Default | Description | Type
--------- | ----------- | ----------- | --------- | -----------
import_id | no | auto-generated | When you're making more than one call you can consider adding it. Otherwise it will be auto-generated | String
request_number | no | - | A number that you can use for bulk identification in "Status" endpoint. It's up to you to provide it's uniqueness. Useless without specifying `import_id` | Integer
members | yes | - | Array with members payloads - See below | Array <JSON Object>
only_create | no | false | When true, it does not update members | Boolean

#### Members Array 

General requirements:

* all members must have unique identifiers (`msisdn` or `email`)
* maximum number of items in array is 1000

Key | Required? | Default | Description | Type
--- | --- | --- | --- | ---
properties | yes | - | JSON with properties for member (see: [Member model](#v3-member-model) | JSON Object
properties\['msisdn'\] | yes* | none | Unique member's msisdn as defined [here](#msisdn-member-identifier)) Example: `4740485124`.| string
properties\['email'\] | yes* | none | Member's email | string
password | no | null | Member’s password. Not required, but user may not be able to log in without this | String
sms_enabled | no | true | Should SMS channel be enabled for member? | Boolean
email_enabled | no | true | Should email channel be enabled for member? | Boolean
push_enabled | no | true | Should push channel be enabled for member? | Boolean
optin_channel | no | "import"** | Source of member | String
optin_subchannel | no | import's id** | Subsource of member| String
consents | no | {} | Members consents | See [Member's consents JSON model](#v3-member-consents-model) | JSON object

&ast; At least one of those properties must be provided

&ast;&ast; When those member attributes are missing on creation, defaults will be used. When on update, they won't be overwritten.

### Response (JSON object)

Key | Description | Type
--- | --- | ---
import_id | Identifier for import status checking (either provided by you or auto-generated) | String

### Error responses

Status | Reason | Description
--------- | ----------- | -----------
`422` | Invalid payload | Returns error object - see below

#### Invalid Payload Error (JSON object)

When payload is invalid, an object is returned: `{"error" => <reason>}`

Possible reasons are:

Code | Description
---- | -----------
`duplicated_emails` | At least two members in payload share same email
`duplicated_msisdns` | At least two members in payload share same msisdn
`members_size_incorrect` | Members number in payload exceeds maximum (1000)
`members_empty` | Payload is empty
`missing_identifier` | At least one member in payload misses required identifier (depends on Community)

<aside class="notice">
Requires <code>BL:Api:Members:Imports:Create</code> permit
</aside>

## <a name="v3-members-imports-status"></a> Get import status

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/imports/:import_id`

Returns status of import with a list of references to bulks (`id` and `request_number`). 

You need to call [Members Imports &bull; Get bulk status](#v3-members-import-bulk-status) to see bulks details.

> Example:

```shell
curl -X GET \
    "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/imports/:import_id" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```


> When successful, JSON will be returned:

```json
{
    "members_in_payloads_number": 4000,
    "members_created_number": 3020,
    "members_updated_number": 531,
    "members_with_validation_errors_number": 449,
    "created_at": "2018-07-30T11:23:08.417Z",
    "bulks": [
        { "id": 83473, "request_number": 1, "status": "finished" },
        { "id": 83474, "request_number": 2, "status": "failed" },
        { "id": 83475, "request_number": 3, "status": "working" },
        { "id": 83476, "request_number": 4, "status": "waiting" }
    ]
}
```

### URL Parameters

Parameter | Description
--------- | ----------- |
import_id | Import identifier |

### Response (JSON object)

Key | Description | Type
--- | --- | ---
members_in_payload_number | A total number of members that were requested for importing | Integer
members_created_number | A total number of members that were successfully created | Integer
members_updated_number | A total number of members that were successfully updated | Integer
members_with_validation_errors_number | A total number of members that had validation errors | Integer
created_at | The date of import creation | Date
bulks | A list of bulks in the import | Array<Object>, where Object consists: of `id`, `request_number` and `status` attributes

### Error responses

Status | Reason
--------- | ----------- 
`404` | Job with such ID wasn't found.

<aside class="notice">
Requires <code>BL:Api:Members:Imports:Get</code> permit
</aside>

## <a name="v3-members-imports-bulk-status"></a> Get bulk status

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/imports/:import_id/bulks/:bulk_id`

Returns status of import bulk.

> Example:

```shell
curl -X GET \
    "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/imports/:import_id/bulks/:bulk_id" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```


> When successful, JSON will be returned:

```json
{
    "id": 11,
    "import_id": "89405ae3-2dd5-46cb-a13b-840bc588199b",
    "request_number": 1,
    "only_create": false,
    "status": "finished",
    "members_in_payload_number": 13,
    "members_created_number": 10,
    "members_updated_number": 2,
    "members_with_validation_errors_number": 1,
    "retries": 0,
    "members_errors": {
        "joe@example.com": {
            "properties": [
                {
                    "error": {
                        "language": [
                            { 
                              "error": "value_not_match",
                              "value": "pl",
                              "values": "en, no",
                              "property": "language"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "created_at": "2018-07-30T17:14:03.738Z"
}
```

### URL Parameters

Parameter | Description
--------- | ----------- |
import_id | Import identifier |
bulk_id | ID of bulk |

### Response (JSON object)

Key | Description | Type
--- | --- | ---
id | ID of bulk| Integer
import_id | Import identifier | String
request_number | The number of the bulk | Integer
status | Job status - one of: 'waiting', 'working', 'finished', 'failed' 'waiting_for_retry' - see more [here](#v3-members-imports-import-flow) in "Bulk statuses" section | String
members_in_payload_number | A number of members that were requested for importing| Integer
members_created_number | A number of members that were successfully created | Integer
members_updated_number | A number of members that were successfully updated | Integer
members_with_validation_errors_number | A number of members that had validation errors  | Integer
retries | A number of background job retries (max 5) | Integer
members_errors | Members errors. Each invalid member will be returned in this object, where key is his identifier (`email` or `msisdn`) and the value is a list of it's [validation errors](#validation-on-members) | Object
created_at | The date of bulk creation | Date

### Error responses

Status | Reason
--------- | ----------- 
`404` | Bulk with such ID wasn't found.

<aside class="notice">
Requires <code>BL:Api:Members:Imports:Get</code> permit
</aside>

