# <a name="v3-members-groups"></a> Endpoints &bull; Members Groups

Members may be grouped into groups. It's up to API client / customer how they are utilized. 

## <a name="v3-automatic-members-groups"></a> Automatic groups

Group is automatic (and has `"automatic": true` attribute ) when it has been created with `audience_id` or 
`audience_conditions` param.

Automatic group doesn't allow API client to manually manage its members - instead, those are fetched from the given audience.

See more [here](#v3-members-groups-create)

## System groups

System groups (with `"system": true` attribute) are created automatically by system and are not editable
by API client - only MPC system can create, edit and destroy system groups.

The set of system groups present in given Loyalty Club depends on its configuration. An example of system group could be
"All members" group that would contain all Loyalty Club members.

## <a name="v3-members-group-model"></a> MembersGroup model

> JSON example:

```json
{
    "id": 6813,
    "name": "Red",
    "type": "Color",
    "description": "Lorem ipsum dolor est",
    "automatic": false,
    "system": false,
    "audience_id": 351,
    "audience_conditions": [
        {
            "type": "array",
            "field": "members_group_ids",
            "value": [6813],
            "operator": "any",
            "condition_group": "member_properties"
        }
    ],
    "members_count": 42
}

```
Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
id | integer | no |
name | string| yes |
type | string| yes | Types are not predefined. It's up to API client to define them.
automatic | boolean | no | Is the group automatic?
system | boolean | no | Is the group system group?
description | string| yes |
members_count | integer| yes |
audience_id | integer | no| ID of related audience
audience_conditions | Object | no| Conditions of related audience - See [DMP docs](https://dmp.boostcom.no/docs/#conditions)

## <a name="v3-members-group-payload-model"></a> MembersGroup payload model

Used for as an input of creations and updates.

> JSON example:

```json
{
    "name": "Red",
    "type": "Color",
    "description": "Lorem ipsum dolor est",
    "system": false,
    "audience_id": 351,
    "audience_conditions": [
        {
            "type": "array",
            "field": "members_group_ids",
            "value": [6813],
            "operator": "any",
            "condition_group": "member_properties"
        }
    ]
}

```
Key | Type | Optional? | Description
--------- | --------- | -------- | ---------
name | string| yes |
type | string| yes | Types are not predefined. It's up to API client to define them.
system | boolean | no | Is the group system group?
description | string| yes |
audience_id | integer | no| ID of related audience
audience_conditions | Object | no| Conditions of related audience

## <a name="v3-members-groups-list"></a> List

> Example:

```shell
curl --request GET 'https://bpc-api.boostcom.no/v3/infinity-mall/members_groups' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing'
```

> Returns object containing [members groups](#v3-members-group-model) and [pagination_info](#v3-pagination-model)

```json
{
  "members_groups": [], // List of members groups - See: "MemberGroup model"
  "pagination_info": {} // Pagination info - see "Pagination info"
}
```

**GET** `v3/:loyalty_club_slug/members_groups`

Returns groups defined within Loyalty Club.

### Query Parameters

Parameter | Type | Default | Description
--------- | ----------- | --------- | -----------
per_page | integer | 1000 | Number of results to be returned per request (1000 is the maximum)
page_no | integer | 1 | Number of results page

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members_groups | Array<MembersGroup> | Array of [MembersGroup](#v3-members-group-model)
pagination_info | Object | [Pagination](#v3-pagination-model) object

<aside class="notice">
Requires <code>BL:Api:MembersGroups:Index</code> permit
</aside>

## <a name="v3-members-groups-get"></a> Get

> Example:

```shell
curl --request GET 'https://bpc-api.boostcom.no/v3/infinity-mall/members_groups' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing'
```

> Returns object containing [members group](#v3-members-group-model)

```json
{
  "members_group": {}, // Members groups - See: "MemberGroup model"
}
```

**GET** `v3/:loyalty_club_slug/members_groups/:members_group_id`

Returns group identified by id.

### URL Parameters

Key | Type 
--- | ----
members_group_id | Integer

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members_group | MembersGroup | [MembersGroup](#v3-members-group-model)

### Error responses

Status | Description
--------- | ----------- 
`404` | MembersGroup does not exist

<aside class="notice">
Requires <code>BL:Api:MembersGroups:Get</code> permit
</aside>

## <a name="v3-members-groups-create"></a> Create

> Example:

```shell
curl --request POST 'https://bpc-api.boostcom.no/v3/infinity-mall/members_groups' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing' \
--header 'Content-Type: application/json' \
--data-raw '{
	"members_group": {
		"name": "My Group 1",
		"type": "Store",
		"description": "Oh long johnson"
	}
}'
```

> Returns object containing created [members group](#v3-members-group-model)

```json
{
  "members_group": {}, // Members groups - See: "MemberGroup model"
}
```

**POST** `v3/:loyalty_club_slug/members_groups`

Creates a new group.

When `audience_id` or `audience_conditions` param is provided, the group is treated 
as [automatic](#v3-automatic-members-groups).

Specifically:

* when `audience_id` is given, the group relates to existing audience
* when `audience_conditions` is given, a new audience is created with specified conditions 
* when none of those parameters is given, a new audience is created with conditions that will return only members 
  manually assigned to the group by API client

### POST Parameters (JSON object)

Parameter | Type
--------- |  -----
members_group | [MembersGroup Payload](#v3-members-group-payload-model) 
members_group.audience_id | ID of audience that the group should relate to  
members_group.audience_conditions | Conditions of audience that the group should relate to

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members_group | MembersGroup | Created [MembersGroup](#v3-members-group-model)

### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters - see [Invalid parameters errors model](#v3-invalid-parameters-errors-model)

<aside class="notice">
Requires <code>BL:Api:MembersGroups:Create</code> permit
</aside>

## <a name="v3-members-groups-update"></a> Update

> Example:

```shell
curl --request PUT 'https://bpc-api.boostcom.no/v3/infinity-mall/members_groups/4321' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing' \
--header 'Content-Type: application/json' \
--data-raw '{
	"members_group": {
		"name": "My Group 1",
		"type": "Store",
		"description": "Oh long johnson"
	}
}'
```

> Returns object containing updated [members group](#v3-members-group-model)

```json
{
  "members_group": {}, // Members groups - See: "MemberGroup model"
}
```

**PUT** `v3/:loyalty_club_slug/members_groups/:members_group_id`

Updates given group.

### POST Parameters (JSON object)

Parameter | Type
--------- |  -----
members_group | [MembersGroup Payload](#v3-members-group-payload-model) 

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members_group | MembersGroup | Created [MembersGroup](#v3-members-group-model)

### Error responses

Status | Description
--------- | ----------- 
`404` | MembersGroup does not exist
`422` | Invalid parameters - see [Invalid parameters errors model](#v3-invalid-parameters-errors-model)

<aside class="notice">
Requires <code>BL:Api:MembersGroups:Update</code> permit
</aside>

## <a name="v3-members-groups-destroy"></a> Destroy

> Example:

```shell
curl --request DELETE 'https://bpc-api.boostcom.no/v3/infinity-mall/members_groups/4321' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing' \
--header 'Content-Type: application/json'
```

> Returns object containing deleted [members group](#v3-members-group-model)

```json
{
  "members_group": {}, // Members groups - See: "MemberGroup model"
}
```

**DELETE** `v3/:loyalty_club_slug/members_groups/:members_group_id`

Deletes given group.

### URL Parameters

Key | Type 
--- | ----
members_group_id | Integer

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members_group | MembersGroup | The deleted [MembersGroup](#v3-members-group-model)

### Error responses

Status | Description
--------- | ----------- 
`404` | MembersGroup does not exist

<aside class="notice">
Requires <code>BL:Api:MembersGroups:Destroy</code> permit
</aside>

## <a name="v3-members-groups-add-member"></a> Add member

> Example:

```shell
curl --request POST 'https://bpc-api.boostcom.no/v3/infinity-mall/members_groups/512/members/123' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing' \
--data-raw ''
```

**POST** `v3/:loyalty_club_slug/members_groups/:members_group_id/members/:member_id`

Adds member to the group.

### URL Parameters

Key | Type 
--- | ----
members_group_id | Integer
member_id | Integer

### Response (JSON object)

Returns empty object.

### Error responses

Status | Description
--------- | ----------- 
`404` | MembersGroup does not exist
`404` | Member does not exist
`422` | Invalid parameters - see [Invalid parameters errors model](#v3-invalid-parameters-errors-model)

<aside class="notice">
Requires <code>BL:Api:MembersGroups:Members:Add</code> permit
</aside>

## <a name="v3-members-groups-remove-member"></a> Remove member

> Example:

```shell
curl --request DELETE 'https://bpc-api.boostcom.no/v3/infinity-mall/members_groups/512/members/123' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing' \
--data-raw ''
```

**DELETE** `v3/:loyalty_club_slug/members_groups/:members_group_id/members/:member_id`

Removes member from the group.

### URL Parameters

Key | Type 
--- | ----
members_group_id | Integer
member_id | Integer

### Response (JSON object)

Returns empty object.

### Error responses

Status | Description
--------- | ----------- 
`404` | MembersGroup does not exist
`404` | Member does not exist
`422` | Invalid parameters - see [Invalid parameters errors model](#v3-invalid-parameters-errors-model)

<aside class="notice">
Requires <code>BL:Api:MembersGroups:Members:Remove</code> permit
</aside>

## <a name="v3-members-groups-list-group-members"></a> List members of a group

> Example:

```shell
curl --request GET 'https://bpc-api.boostcom.no/v3/infinity-mall/members_groups/1/members' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing' \
--data-raw ''
```

> Returns object containing [members](#v3-member-model) and [pagination_info](#v3-pagination-model)

```json
{
  "members": [], // List of members - See: "Member model"
  "pagination_info": {} // Pagination info - see "Pagination info"
}

```

**GET** `v3/:loyalty_club_slug/members_groups/:members_group_id/members`

### Query Parameters

Parameter | Type | Default | Description
--------- | ----------- | --------- | -----------
per_page | integer | 1000 | Number of results to be returned per request (1000 is the maximum)
page_no | integer | 1 | Number of results page

### URL Parameters

Key | Description | Type
--- | ---- | ---
members_group_id | MembersGroup ID | integer

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members | Array<Member> | Array of [Members](#v3-member-model)
pagination_info | Object | [Pagination](#v3-pagination-model) object

<aside class="notice">
Requires <code>BL:Api:MembersGroups:Members:List</code> permit
</aside>

## <a name="v3-members-groups-list-member-groups"></a> List groups of a member

> Example: 

```shell
curl --request GET 'https://bpc-api.boostcom.no/v3/infinity-mall/members/549123/groups' \
--header 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'X-Product-Name: default' \
--header 'X-User-Agent: cURL Manual Testing'
```

> Returns object containing [members groups](#v3-members-group-model) and [pagination_info](#v3-pagination-model)

```json
{
  "members_groups": [], // List of members groups - See: "MemberGroup model"
  "pagination_info": {} // Pagination info - see "Pagination info"
}
```

**GET** `v3/:loyalty_club_slug/members/:member_id/groups`

**GET** `v3/:loyalty_club_slug/members/by_email/:member_email/groups`

**GET** `v3/:loyalty_club_slug/members/by_msisdn/:member_msisdn/groups`

Returns groups of given member.

### Query Parameters

Parameter | Type | Default | Description
--------- | ----------- | --------- | -----------
per_page | integer | 1000 | Number of results to be returned per request (1000 is the maximum)
page_no | integer | 1 | Number of results page

### URL Parameters

Key | Description | Type
--- | ---- | ---
member_id | Member's ID | integer
member_msisdn | Member's msisdn | string (format as defined [here](#msisdn-member-identifier) - example: `4740485124`)
member_email | Member's email | string (email)

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
members_groups | Array<MembersGroup> | Array of [MembersGroup](#v3-members-group-model)
pagination_info | Object | [Pagination](#v3-pagination-model) object

### Error responses

Status | Description
--------- | ----------- 
`404` | Member does not exist

<aside class="notice">
Requires <code>BL:Api:Members:Groups</code> permit
</aside>
