#  Links

## <a name="v3-links-generate"></a> Generate link

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/links/offer_page/generate" \
    -H 'Content-Type: application/json' \
    -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'X-Product-Name: default' \
    -H 'X-User-Agent: CURL manual test'
    -d \
    '{
        "link_params": {
            "offer_id": 1000487
        }
    }'
```

> When successful (200), returns a generated link

```json
{
  "url": "https://member.dev.bstcm.no/infinity-mall/offers/1000487"
}
``` 

**POST** `v3/:loyalty_club_slug/links/(:link_slug)/generate`

Returns a link to given functionality in MPC, so it may be reached by member.

The returned link is generated in a "universal" form: it points to our Webforms, but it will be captured by mobile app if 
it's installed on member system.

A list of possible links depends on Loyalty Club configuration. 
An MPC API endpoint for returning it may be implemented in future.     

### URL Parameters

Parameter | Type | Description
--------- | ----------- | ------
link_slug | string | A slug that identifies the link to generate

### POST Parameters (JSON object)

Parameter | Type | Description
--------- | ----------- | ------
link_params | Object | Params for link generation dependent on specific link definition

### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
url | String | The generated link

### Error responses

Status | Description
--------- | ----------- 
`404` | Link could not be found
`422` | Invalid parameters

<aside class="notice">
    Requires <code>BL:Api:Links:Generate</code> permit
</aside> 
