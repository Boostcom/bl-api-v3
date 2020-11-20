## <a name="members-messages"></a> Members Messages

### <a name="member-messages-introduction"> Introduction

#### <a name="member-message-model"></a> MemberMessage model

> Example:

```json
{
  "id": "MTE0NTc1Mg",
  "type": "push",
  "created_at": "2020-08-05T05:38:12",
  "content": "You have just received 9 points."
}
```

Key        | Description                         | Required? | Type
---------  | ----------------------------------- | --------- | ---------
id         | Message ID                          | yes       | string
type       | Object with member's properties     | yes       | enum: ["sms", "email", "push"]
created_at | Time when the message has been sent | yes       | string
content    |                                     | yes       | string

### <a name="members-messages-list"></a> List Member Messages

> Example:

```shell
curl "https://api.mpc.placewise.com/v3/infinity-mall/members/509134/messages" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [member messages](#member-message-model) and [pagination_info](#pagination-model)

```json
{
  "messages": [], // List of members - See: "Member message"
  "pagination_info": {} // Pagination info - see "Pagination info"
}
```

**GET** `v3/:loyalty_club_slug/members/:member_id/messages`

Returns messages sent to given Member from our system, after merging properties and shortening urls so this is the content Member received. 
Does not translate id to other identifiers and vice versa. If no messages are found, returns an empty `messages` array. 

**Messages are removed when Member is deleted.**

#### URL Parameters

Parameter | Description                                  | Required? | Type
--------- | -------------------------------------------- | ----------| ------
member_id | Member ID                                    | yes       | integer
per_page  | Number of results to be returned per request | no        | integer 
page_no   | Number of results page                       | no        | integer

#### Response (JSON object)

Key             | Type                 | Description
--------------- | -------------------- | ---------------------------------------------------
messages        | Array<MemberMessage> | Array of [MemberMessages](#member-message-model)
pagination_info | Pagination           | [Pagination](#pagination-model) object

<aside class="notice">
Requires <code>BL:Api:Members:MessagesHistory:List</code> permit
</aside>

<!--- ############################################################################################################# --->

### <a name="members-message-get"></a> Get Member Message

> Example:

```shell
curl "https://api.mpc.placewise.com/v3/infinity-mall/members/509134/messages/MTE0NTc1Mg" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns [member message](#member-message-model)

```json
{
  "messages": {} // MemberMessage record - See: "MemberMessage model"
}
```

**GET** `v3/:loyalty_club_slug/members/:member_id/messages/:id`

Returns specific message sent to member.

#### URL Parameters

Parameter | Description | Required? | Type
--------- | ----------- | --------  | ------
member_id | Member ID   | yes       | integer
id        | Message ID  | yes       | string

#### Response (JSON object)

Key     | Type          | Description
------- | ------------- | ---------
message | MemberMessage | [MemberMessage](#member-message-model)

#### Error responses

Status    | Reason
--------- | ----------- 
`404`     | Message not found

<aside class="notice">
Requires <code>BL:Api:Members:MessagesHistory:Get</code> permit
</aside>

<!--- ############################################################################################################# --->
