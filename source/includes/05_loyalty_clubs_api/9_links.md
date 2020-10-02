## <a name="links"> Links

### <a name="link-model"></a> Link model

> Example

```json
        {
            "slug": "about",
            "name": "About",
            "description": null,
            "url": "/about",
            "push_message_data": { "foo": "bar" },
            "optional_params": [],
            "system": false,
            "global": true,
            "enabled": true
        }
```

Key               | Type          | Optional? | Description
----------------- | ------------- | --------- | ---------
slug              | string        | no        | Identifier or Link. Generated from name.
name              | string        | no        | Must be unique
description       | string        | yes       |
url               | string        | no        | Must be unique
scheme            | string        | no        | When present, link will be generated with this scheme
push_message_data | Object        | yes       | When present, it will be sent along push messages that contain this link
optional_params   | array<string> | yes       | Contains information which link params are optional
system            | boolean       | no        | Is this a system link?
global            | boolean       | no        | Is this a global link?
enabled           | boolean       | no        | Is this link enabled for specific LC?


#### <a name="enabled-links"></a> Enabled links

When link is enabled, it's available in MPC's UI for being used within sending (unless the link is parameterized).

#### Global links

Global links are standard links used within MPC apps. Every LC may be configured to have each of them [enabled](#links-enable).

Non-global links are links created specifically for the Loyalty Club.

#### System links

System links are global links that are essential for LC to operate and are always [enabled](#links-enable)..

### <a name="links-list"></a> List

Returns all links available within Loyalty Club.

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/links" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns list of [links](#link-model):

```json
{
  "links": [] // List of links - See: "Link model"
}
```

**GET** `v3/infinity-mall/links`

#### Response (JSON object)

Key | Type 
--------- | --------- 
links | [Links](#link-model)[] 

<aside class="notice">
Requires <code>BL:Api:Links:Index</code> permit
</aside>

### <a name="links-list"></a> Get

Returns specific link.

> Example:

```shell
curl \
"https://bpc-api.boostcom.no/v3/infinity-mall/links/about" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns [link](#link-model):

```json
{
  "link": [] // Link - See: "Link model"
}
```

**GET** `v3/infinity-mall/links/:link_slug`

#### Response (JSON object)

Key | Type 
--------- | --------- 
link | [Link](#link-model)

#### Error responses

Status | Description
--------- | ----------- 
`404` | Link could not be found

<aside class="notice">
Requires <code>BL:Api:Links:Get</code> permit
</aside>

### <a name="links-enable"></a> Enable

Enables given link - see [enabled links](#enabled-links)

> Example:

```shell
curl -X PUT \
"https://bpc-api.boostcom.no/v3/infinity-mall/links/about/enable" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns [link](#link-model):

```json
{
  "link": [] // Link - See: "Link model"
}
```

**PUT** `v3/infinity-mall/links/:link_slug/enable`

#### Response (JSON object)

Key | Type 
--------- | --------- 
link | [Link](#link-model)

#### Error responses

Status | Description
--------- | ----------- 
`404` | Link could not be found

<aside class="notice">
Requires <code>BL:Api:Links:Enable</code> permit
</aside>

### <a name="links-disable"></a> Disable

Disables given link - see [enabled links](#enabled-links)

> Example:

```shell
curl -X PUT \
"https://bpc-api.boostcom.no/v3/infinity-mall/links/about/disable" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns [link](#link-model):

```json
{
  "link": [] // Link - See: "Link model"
}
```

**PUT** `v3/infinity-mall/links/:link_slug/disable`

#### Response (JSON object)

Key | Type 
--------- | --------- 
link | [Link](#link-model)

#### Error responses

Status | Description
--------- | ----------- 
`404` | Link could not be found

<aside class="notice">
Requires <code>BL:Api:Links:Enable</code> permit
</aside>


### <a name="links-create"></a> Create

Create a new link for the Loyalty Club. Created link is always [enabled](#enabled-links) by default.

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/infinity-mall/links" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test' \
    --data-raw '{
        "link": {
            "name": "My Link",
            "url": "some/url",
            "description": "Oh long johnson",
            "scheme": "my-app"
        }
    }'
```

> When successful (200), returns created [link](#link-model):

```json
{
  "link": [] // Link - See: "Link model"
}
```

> This link [generated](#links-generate) by MPC backend will then look like this:

```text
my-app://some/url
```  

**POST** `v3/infinity-mall/links`

#### POST Parameters (JSON object)

Parameter        | Type   | Required?
---------------- | -----  | ----- 
link.name        | string | yes
link.url         | string | yes
link.scheme      | string | no
link.description | string | no

#### Response (JSON object)

Key | Type 
--------- | --------- 
link | [Link](#link-model)

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>BL:Api:Links:Create</code> permit
</aside>

### <a name="links-delete"></a> Delete

Destroys Loyalty Club link.

> Example:

```shell
curl -X DELETE \
"https://bpc-api.boostcom.no/v3/infinity-mall/links/articles" \
    -H 'content-type: application/json' \
    -H 'authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns deleted [link](#link-model):

```json
{
  "link": [] // Link - See: "Link model"
}
```

**DELETE** `v3/infinity-mall/links/:link_slug`

#### Response (JSON object)

Key | Type 
--------- | --------- 
link | [Link](#link-model)

#### Error responses

Status | Description
--------- | ----------- 
`404` | Link could not be found
`405` | Link cannot be deleted

<aside class="notice">
Requires <code>BL:Api:Links:Delete</code> permit
</aside>

### <a name="links-generate"></a> Generate

> Example:

```shell
curl -X POST \
"https://bpc-api.boostcom.no/v3/:loyalty_club_slug/links/offer_page/generate" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
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

Returns a generated link to given functionality in MPC, so it may be reached by member.

In standard setup, the returned link is generated in a "universal" form: it points to our Webforms, but it will be captured by mobile app if 
it's installed on member system.

#### URL Parameters

Parameter | Type | Description
--------- | ----------- | ------
link_slug | string | A slug that identifies the link to generate

#### POST Parameters (JSON object)

Parameter | Type | Description
--------- | ----------- | ------
link_params | Object | Params for link generation dependent on specific link definition

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
url | String | The generated link

#### Error responses

Status | Description
--------- | ----------- 
`404` | Link could not be found
`422` | Invalid parameters

<aside class="notice">
    Requires <code>BL:Api:Links:Generate</code> permit
</aside> 
