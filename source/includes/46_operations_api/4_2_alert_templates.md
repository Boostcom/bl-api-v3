## <a name="operations-alert-templates"></a> Alert Templates

### <a name="operations-list-alert-templates"></a> List Alert Templates

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/alert_templates" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing alert templates and [pagination_info](#pagination-model)

```json
{
  "alert_templates": [
    {
      "id": 4,
      "type": "Fire alert",
      "message": "Fire has broke out",
      "created_at": "2021-05-14T09:26:14.823Z",
      "updated_at": "2021-05-14T09:26:14.823Z"
    }
  ],
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/alert_templates`

Returns list of Alert Templates.

#### Response

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
alert_templates[].id | integer | no |
alert_templates[].type | string | no | One of available [alert types](#operations-list-alert-types) names
alert_templates[].message | string | no | Content of message sent to recipients
alert_templates[].created_at | datetime | no | Time of creation
alert_templates[].updated_at | datetime | no | Time of last update

<aside class="notice">
Requires <code>Operations:Api:Users:AlertTemplates:List</code> permit
</aside>
