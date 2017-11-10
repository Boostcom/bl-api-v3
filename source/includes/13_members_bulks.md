# Endpoints &bull; Members Bulks

## <a name="v3-members-bulk-create-or-update"></a> Create or Update (ASync, work in progress)

**This may change**

**POST** `api/v3/loyalty_clubs/:loyalty_club_slug/members/bulks/create_or_update`

Creates new members or updates existing ones (based on unique identifiers). Job will be done in background.


It is possible to trigger sms and email welcome message by setting `send_sms_welcome_message: true` or `send_email_welcome_message: true`.
It will be only send when member is created (nothing will be send to members that are being updated). 
Welcome messages depends on Loyalty Club and Product configuration.

> Example:

```shell
curl -X POST \
    "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/members/bulks/create_or_update" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful, JSON will be returned:

```json
{
  "success": true,
  "job_id": "23388281912"
}
```

### Members Array

Requirements:

* all members must have unique identifiers (and also email and msisdn)
* maximum number of items in array is 5000

Key | Required? | Default | Description | Type
--- | --- | --- | --- | ---
properties | yes | - | JSON with properties for member (similar as in Members Create endpoint) | JSON Object
password | no | null | Member’s password. Not required, but user won’t be able to log in without this | String
sms_enabled | no | true | Should SMS channel be enabled for member? | Boolean
email_enabled | no | true | Should email channel be enabled for member? | Boolean
push_enabled | no | true | Should push channel will be enabled for member? | Boolean
source | no | "Product" header | Source of member. Used only when creating member | String
subsource | no | "Subproduct" header | Subsource of member. Used only when creating member | String

### POST Parameters (JSON)

Parameter | Required? | Default | Description | Type
--------- | ----------- | ----------- | --------- | -----------
members | yes | - | Array with members payloads | Array<JSON Object>
only_create | no | false | Only create members. Don't update existing ones | Boolean
job_id | no | auto-generated | When you're making more than 1 call you can consider adding it. Otherwise it will be auto-generated | String
request_number | no | auto-generated | Number of request to identify errors in "Status" endpoint easier. Useless without specifying `job_id` | Integer
send_sms_welcome_message | no | true | Should SMS welcome message be sent to member? | Boolean
send_email_welcome_message | no | true | Should email welcome be sent to member? | Boolean

### Response (JSON object)

Key | Description | Type
--- | --- | ---
success | Job was successfully scheduled | Boolean
job_id | ID for checking batch job status | String


### Error responses

Status | Reason
--------- | ----------- 
`422` | Most likely you have duplicated identifiers in `members` param.

<aside class="notice">
Requires <code>BL:Api:MemberBulks:CreateOrUpdate</code> permit
</aside>

## <a name="v3-members-bulk-create-or-update-status"></a> Create or Update Status (work in progress)

**This may change**

**GET** `api/v3/loyalty_clubs/:loyalty_club_slug/members/bulks/create_or_update/:job_id`

Returns status about "Bulk Create or Update" background job.

Access jobs' statuses is scoped by client authorization token.

> Example:

```shell
curl -X GET \
    "https://bpc-api.boostcom.no/api/v3/loyalty_clubs/infinity-mall/member_bulks/create_or_update/:job_id" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
```

> When successful, JSON will be returned:

### URL Parameters

Parameter | Description
--------- | ----------- |
job_id | ID of bulk job |

### Response (JSON object)

Key | Description | Type
--- | --- | ---
status | One of: ['finished', 'waiting', 'in_progress', 'fatal_error'] | String
bulk_jobs | Number of bulk jobs scheduled internally | Integer
bulk_jobs_done | Number of bulk jobs already done | Integer
members_created_number | Number of members that were successfully created | Integer
members_updated_number | Number of members that were successfully updated | Integer
members_with_validation_errors_number | Number of members that weren't create nor updated due to validation errors | Integer
errors | Array with errors | Array<JSON Object>

### Errors JSON Object

Key | Description | Type
--- | --- | ---
request_number | Number of request specified while making bulk for creating bulk job | Integer 
position | Member payload position in members' array | Integer
errors | [validation errors](#validation-on-members) JSON object. | JSON Object

### Error responses

Status | Reason
--------- | ----------- 
`404` | Job with such ID wasn't found.

<aside class="notice">
Requires <code>BL:Api:MemberBulks:CreateOrUpdate</code> permit
</aside>
