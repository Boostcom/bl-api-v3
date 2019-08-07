# Endpoints &bull; Files
## General info

### <a name="v3-file-cropping"></a> File cropping

> `crop_to` param interpretation may be visualized like this:

```text

                           x_top_left     x_bottom_right
                    |-------------------------------------
                    | ..........................
                    | ..........................
     y_top_left     | .....*---------------.....
                    | .....|..............|.....
                    | .....|..............|.....
                    | .....|..............|....
     y_bottom_right | .....---------------*.....
                    | ..........................
                    |

```

When file is uploaded and it's too large to match the required base size, it may get cropped.
This can be achieved by providing cropping param (`crop_to`).

The param has following syntax: `x_top_left,y_top_left,x_bottom_right,y_bottom_right` and represents boundaries of a cropping area. 

When uploaded file is too large and `crop_to` param is not provided, the file will be automatically cropped with gravity set in center of image.

#### Example

The kind's base size is 400x200px and the uploaded file is 1000x1000px. 

With crop_to="0,250,1000,750", a rectangle of "1000x500" size will be cropped, with 250px left offset.
The file will be then also resized to 400x200.

### <a name="v3-projected-file-versions"></a> Uploaded instances projections

When file is uploaded, only original file is physically processed and stored. `base` and other versions are processed asynchronously. 
So, the responses returned on upload contains only "projected" info about files.

Key | Type | Description
--------- | --------- | ---------
size_type | string |  
url | string | An URL the file be available on **after** it gets processed.
min_width | integer | 
max_width | integer |
min_height | integer |
max_height | integer |

## <a name="v3-get-file-schema"></a> Get file schema

> Example request

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/files/schema" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> Returns the schema as described [here](#v3-file-schema)

**GET** `v3/:loyalty_club_slug/files/schema`

Returns the Loyalty Club file schema. See: [Files &bull; Schema](#v3-file-schema)

<aside class="notice">
Requires <code>Files:Api:GetSchema</code> permit
</aside>

## <a name="v3-create-file"></a> Create file

> Example request

```shell
curl -X POST \
  https://bpc-api.boostcom.no/v3/infinity-mall/files/offers/1000289/offer_default \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
  -H 'Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F file=@some_file.png \
  -F 'crop_to=256,256,856,1056'

```

> Returns info about uploaded file and it's projected versions

```json
{
    "original": {
        "size_type": "original",
        "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000289-offer_default-original.png",
        "file_type": "IMAGE",
        "width": 1024,
        "height": 1024,
        "kind": "offer_default"
    },
    "projected_versions": [
        {
            "size_type": "base",
            "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000289-offer_default-base.png",
            "min_width": 600,
            "min_height": 800,
            "max_width": 600,
            "max_height": 800
        },
        {
            "size_type": "thumbnail",
            "url": "https://offers-api.s3.eu-central-1.amazonaws.com/offer-1000289-offer_default-thumbnail.png",
            "min_width": 225,
            "min_height": 300,
            "max_width": 225,
            "max_height": 300
        }
    ]
}
```

**POST** `v3/:loyalty_club_slug/files/:fileable_type/:fileable_id/kind`

Creates new file for given Fileable (e.g. Offer).

### <a name="v3-create-file-url-parameters"></a> URL Parameters

Parameter | Type | Description
--------- | ----------- | -----------
fileable_type | enum: `['offers', 'collections']` |
fileable_id | integer 
kind | enum | see [Standard file kinds](#v3-file-kinds)  

### <a name="v3-create-file-post-parameters"></a> POST Parameters

Parameter | Type | Required? | Description
--------- | ----------- | ------- | -----------
file | binary | yes | File to upload
crop_to | string | no | See: [cropping](#v3-file-cropping)

### <a name="v3-create-file-response"></a> Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
original | File | See [File model](#v3-file-model) 
projected_versions | ProjectedVersion[] | See [Projected file versions](#v3-projected-file-versions)

### Error responses

Status | Reason
--------- | ----------- 
`404` | The Fileable does not exist
`422` | Invalid parameters

<aside class="notice">
Requires <code>Files:Api:Create</code> permit
</aside>

## <a name="v3-update-file"></a> Update file

> Example request

```shell
curl -X PUT \
  https://bpc-api.boostcom.no/v3/infinity-mall/files/offers/1000289/offer_default \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test' \
  -H 'Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F file=@some_file.png \
  -F 'crop_to=256,256,856,1056'

```

> When successful (200), Returns info about uploaded file. Same as [Create file](#v3-create-file) example response

```json
{
  // (...) - see "Create file" example response
}
```

**PUT** `v3/:loyalty_club_slug/files/:fileable_type/:fileable_id/kind`

Updates file for given Fileable (e.g. Offer).

### URL Parameters

See: [Create File URL Parameters](#v3-create-file-url-parameters)

### POST Parameters

See: [Create File POST Parameters](#v3-create-file-post-parameters)

### Response (JSON object)

See: [Create File Response](#v3-create-file-response)

### Error responses

Status | Reason
--------- | ----------- 
`404` | Fileable not found
`404` | File not found
`422` | Invalid parameters

<aside class="notice">
Requires <code>Files:Api:Update</code> permit
</aside>

## <a name="v3-delete-file"></a> Delete file

> Example request

```shell
curl -X DELETE "https://bpc-api.boostcom.no/v3/infinity-mall/files/offers/1000289/offer_default \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> When successful (200), returns an empty object

```json
{
  // Empty object
}
```

**DELETE** `v3/:loyalty_club_slug/files/:fileable_type/:fileable_id/kind`

Deletes file of given Fileable (e.g. Offer).

### URL Parameters

See: [Create File URL Parameters](#v3-create-file-url-parameters)

### Error responses

Status | Reason
--------- | ----------- 
`404` | Fileable not found
`404` | File not found

<aside class="notice">
Requires <code>Files:Api:Delete</code> permit
</aside>
