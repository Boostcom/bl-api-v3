## <a name="image-templates"></a> ImageTemplates

This API allows managing [ImageTemplates records](#offer-image-template-record).

### <a name="offer-admin-image-templates-list"></a> List

> Example:

```shell
curl --request GET 'https://api.mpc.placewise.com/v3/infinity-mall/offers/image_templates' \
--header 'x-client-authorization: B7t``9U9tsoWsGhrv2ouUoSqpM' \
--header 'x-product-name: default' \
--header 'x-user-agent: cURL Manual Testing'
```

> Returns object containing [image_templates](#offer-image-template-record) and [pagination_info](#pagination-model)

```json
{
  "image_templates": [],// List of image_templates - See: "ImageTemplate record"
  "pagination_info": {} // Pagination info - see "Pagination info"
}
```

**GET** `v3/:loyalty_club_slug/offers/image_templates`

Returns templates defined within Loyalty Club.

#### Query Parameters

Parameter | Type | Default | Description
--------- | ----------- | --------- | -----------
per_page | integer | 1000 | Number of results to be returned per request (1000 is the maximum)
page_no | integer | 1 | Number of results page
default | boolean | null | When true - returns only default template 

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
image_templates | Array<ImageTemplate> | Array of [ImageTemplate](#offer-image-template-record)
pagination_info | Object | [Pagination](#pagination-model) object

<aside class="notice">
Requires <code>Offers:Api:ImageTemplates:List</code> permit
</aside>

### <a name="offer-admin-image-templates-get"></a> Get

> Example:

```shell
curl --request GET 'https://api.mpc.placewise.com/v3/infinity-mall/offers/image_templates' \
--header 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'x-product-name: default' \
--header 'x-user-agent: cURL Manual Testing'
```

> Returns object containing [image template](#offer-image-template-record)

```json
{
  "image_template": {}, // Image template - See: "ImageTemplate record"
}
```

**GET** `v3/:loyalty_club_slug/offers/image_templates/:image_template_id`

Returns template identified by id.

#### URL Parameters

Key | Type 
--- | ----
image_template_id | Integer

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
image_template | ImageTemplate | [ImageTemplate](#offer-image-template-record)

#### Error responses

Status | Description
--------- | ----------- 
`404` | ImageTemplate does not exist

<aside class="notice">
Requires <code>Offers:Api:ImageTemplates:Get</code> permit
</aside>

### <a name="offer-admin-image-templates-create"></a> Create

> Example:

```shell
curl --request POST 'https://api.mpc.placewise.com/v3/infinity-mall/offers/image_templates' \
--header 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'x-product-name: default' \
--header 'x-user-agent: cURL Manual Testing' \
--header 'content-type: application/json' \
--data-raw '{
	"image_template": {
		"name": "My Group 1",
        "rectangles": [
            {
                "number": 1,
                "name": "Title",
                "offset_top": 100,
                "offset_left": 150,
                "width": 300,
                "height": 200,
                "font_size": 40,
                "font_family": "'Playfair Display', serif",
                "font_url": "https://fonts.googleapis.com/css?family=Raleway:400,700&display=swap",
                "color": "#FFFFFF",
                "text_align": "center",
                "content": "Oh my {{name}}!"
            }
        ]
	}
}'
```

> Returns object containing created [image template](#offer-image-template-record)

```json
{
  "image_template": {}, // Image templates - See: "ImageTemplate record"
}
```

**POST** `v3/:loyalty_club_slug/offers/image_templates`

Creates a new template.

#### POST Parameters (JSON object)

Parameter | Type
--------- |  -----
image_template | [ImageTemplate Payload](#offer-image-template-record-payload) 

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
image_template | ImageTemplate | Created [ImageTemplate](#offer-image-template-record)

#### Error responses

Status | Description
--------- | ----------- 
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Offers:Api:ImageTemplates:Create</code> permit
</aside>

### <a name="offer-admin-image-templates-update"></a> Update

> Example:

```shell
curl --request PUT 'https://api.mpc.placewise.com/v3/infinity-mall/offers/image_templates/4321' \
--header 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'x-product-name: default' \
--header 'x-user-agent: cURL Manual Testing' \
--header 'content-type: application/json' \
--data-raw '{
	"image_template": {
		"name": "My Group 1",
        "rectangles": [
            {
                "number": 1,
                "name": "Title",
                "offset_top": 100,
                "offset_left": 150,
                "width": 300,
                "height": 200,
                "font_size": 40,
                "font_family": "'Playfair Display', serif",
                "font_url": "https://fonts.googleapis.com/css?family=Raleway:400,700&display=swap",
                "color": "#FFFFFF",
                "text_align": "center",
                "content": "Oh my {{name}}!"
            }
        ]
	}
}'
```

> Returns object containing updated [image template](#offer-image-template-record)

```json
{
  "image_template": {}, // Image templates - See: "ImageTemplate record"
}
```

**PUT** `v3/:loyalty_club_slug/offers/image_templates/:image_template_id`

Updates given template.

#### POST Parameters (JSON object)

Parameter | Type
--------- |  -----
image_template | [ImageTemplate Payload](#offer-image-template-record-payload) 

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
image_template | ImageTemplate | Created [ImageTemplate](#offer-image-template-record)

#### Error responses

Status | Description
--------- | ----------- 
`404` | ImageTemplate does not exist
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Offers:Api:ImageTemplates:Update</code> permit
</aside>

### <a name="offer-admin-image-templates-destroy"></a> Destroy

> Example:

```shell
curl --request DELETE 'https://api.mpc.placewise.com/v3/infinity-mall/offers/image_templates/4321' \
--header 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
--header 'x-product-name: default' \
--header 'x-user-agent: cURL Manual Testing' \
--header 'content-type: application/json'
```

> Returns object containing deleted [image template](#offer-image-template-record)

```json
{
  "image_template": {}, // Image templates - See: "ImageTemplate record"
}
```

**DELETE** `v3/:loyalty_club_slug/offers/image_templates/:image_template_id`

Deletes given template.

#### URL Parameters

Key | Type 
--- | ----
image_template_id | Integer

#### Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
image_template | ImageTemplate | The deleted [ImageTemplate](#offer-image-template-record)

#### Error responses

Status | Description
--------- | ----------- 
`404` | ImageTemplate does not exist

<aside class="notice">
Requires <code>Offers:Api:ImageTemplates:Destroy</code> permit
</aside>
