# Files API (deprecated)

## Introduction

### <a name="file-model"></a> File model

Every file attached to some entity (Offer, Reward, Offer Collection) follows the file schema and is defined like this:

Key | Type | Description
--- | --- | -----------
url | URL | Link to the file
width | integer | Image width
height | integer | Image height
kind | string | File kind identifier (e.x. `offer_default`), see [Standard file kinds](#file-kinds)
size_type | string | e.x. `base`, see: [File sizes](#file-sizes)

### <a name="file-schema"></a> File schema

File schema defines a list of files kinds available in the Loyalty club.
Each file kind is also available in multiple sizes.

The schema may get retrieved with [Files> Get schema](#get-file-schema) endpoint.

> Example loyalty club file schema

```json
{
  "files_kinds": [
    {
      "identifier": "collection_cover_default",
      "type": "IMAGE",
      "ratio": "2:1",
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": 300,
          "max_width": 600,
          "max_height": 300
        },
        {
          "identifier": "thumbnail",
          "min_width": 300,
          "min_height": 150,
          "max_width": 300,
          "max_height": 150
        },
        {
          "identifier": "original",
          "min_width": 600,
          "min_height": 300,
          "max_width": null,
          "max_height": null
        }
      ]
    },
    {
      "identifier": "offer_default",
      "type": "IMAGE",
      "ratio": "3:4",
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": 800,
          "max_width": 600,
          "max_height": 800
        },
        {
          "identifier": "thumbnail",
          "min_width": 225,
          "min_height": 300,
          "max_width": 225,
          "max_height": 300
        },
        {
          "identifier": "original",
          "min_width": 600,
          "min_height": 800,
          "max_width": null,
          "max_height": null
        }
      ]
    },
    {
      "identifier": "reward_default",
      "type": "IMAGE",
      "ratio": "3:4",
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": 800,
          "max_width": 600,
          "max_height": 800
        },
        {
          "identifier": "thumbnail",
          "min_width": 225,
          "min_height": 300,
          "max_width": 225,
          "max_height": 300
        },
        {
          "identifier": "original",
          "min_width": 600,
          "min_height": 800,
          "max_width": null,
          "max_height": null
        }
      ]
    },
    {
      "identifier": "example_without_ratio",
      "type": "IMAGE",
      "ratio": null,
      "sizes": [
        {
          "identifier": "base",
          "min_width": 600,
          "min_height": 600,
          "max_width": null,
          "max_height": null
        },
        {
          "identifier": "thumbnail",
          "min_width": null,
          "min_height": null,
          "max_width": 300,
          "max_height": 300
        },
        {
          "identifier": "original",
          "min_width": 600,
          "min_height": 600,
          "max_width": null,
          "max_height": null
        }
      ]
    }
  ]
}
```

### <a name="file-kind-model"></a> File kind model

Key | Type | Description
--------- | ----------- | ---------
identifier | string | See: [Standard file kinds](#file-kinds)
type | string | Currently, only `IMAGE` (MIME: `image/gif`, `image/jpeg`, `image/png`) is supported
ratio | string | Describes ratio of an image. See: [Ratio](#file-kind-ratio) (optional)
sizes| Array<Object> | See: [File sizes](#file-sizes)
sizes[]['identifier'] | string |
sizes[]['min_width'] | integer | (optional)
sizes[]['min_height'] | integer | (optional)
sizes[]['max_width'] | integer | (optional)
sizes[]['max_height'] | integer | (optional)

### <a name="file-kinds"></a> Standard file kinds

There are some standard file kinds, their size definitions differ among Loyalty Clubs.

Identifier | Description
---  | -----------
`collection_cover_default` | Cover for Offer collections
`offer_default` | Default Offer image
`reward_default` | Default Reward image

### <a name="file-kind-ratio"></a> Ratio

Images may have fixed proportions ratio which is represented as a string: `"<width>:<height>"`

### <a name="file-sizes"></a> File sizes

Every file has at least three file sizes available:

#### `base`

Actual file size. Currently all file kinds have this size with fixed proportions (so `min_*` == `max_*`) that match the ratio.

#### `thumbnail`

Rescaled version of `base` size. Currently, rescaling always works this way:

* pick the higher dimension of file
* shrink the image so:
    * the higher dimension gets shrunk to `300px`
    * the lower dimensions gets shrunk so the image keeps the original ratio

Examples:

* File with resolution `600x800` gets shrunk down to `225x300`
* File with resolution `800x600` gets shrunk down to `300x225`
* File with resolution `400x300` gets shrunk down to `300x225`
* File with resolution `400x200` gets shrunk down to `300x150`
* File with resolution `300x250` is not shrunk down

#### `original`

Original file that that has been uploaded and used for generating other file sizes.

Currently, we require all original files to have proper ratios and minimum sizes of at least the `base` size.
Because of that, you can expect them to have `min_height` and `min_width` that match the `base` size,
but they never have `max_width` or `max_height` defined.

#### <a name="file-cropping"></a> File cropping

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

##### Example

The kind's base size is 400x200px and the uploaded file is 1000x1000px.

With crop_to="0,250,1000,750", a rectangle of "1000x500" size will be cropped, with 250px left offset.
The file will be then also resized to 400x200.

#### <a name="projected-file-versions"></a> Uploaded instances projections

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

##  Files

### <a name="get-file-schema"></a> Get file schema

> Example request

```shell
curl "https://api.mpc.placewise.com/v3/infinity-mall/files/schema" \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns the schema as described [here](#file-schema)

**GET** `v3/:loyalty_club_slug/files/schema`

Returns the Loyalty Club file schema. See: [Files > Schema](#file-schema)

<aside class="notice">
Requires <code>Files:Api:GetSchema</code> permit
</aside>

### <a name="create-file"></a> Create file

> Example request

```shell
curl -X POST \
  https://api.mpc.placewise.com/v3/infinity-mall/files/offers/1000289/offer_default \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
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

#### <a name="create-file-url-parameters"></a> URL Parameters

Parameter | Type | Description
--------- | ----------- | -----------
fileable_type | enum: `['offers', 'collections']` |
fileable_id | integer
kind | enum | see [Standard file kinds](#file-kinds)

#### <a name="create-file-post-parameters"></a> POST Parameters

Parameter | Type | Required? | Description
--------- | ----------- | ------- | -----------
file | binary | yes | File to upload
crop_to | string | no | See: [cropping](#file-cropping)

#### <a name="create-file-response"></a> Response (JSON object)

Key | Type | Description
--------- | --------- | ---------
original | File | See [File model](#file-model)
projected_versions | ProjectedVersion[] | See [Projected file versions](#projected-file-versions)

#### Error responses

Status | Reason
--------- | -----------
`404` | The Fileable does not exist
`422` | Invalid parameters

<aside class="notice">
Requires <code>Files:Api:Create</code> permit
</aside>

### <a name="update-file"></a> Update file

> Example request

```shell
curl -X PUT \
  https://api.mpc.placewise.com/v3/infinity-mall/files/offers/1000289/offer_default \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F file=@some_file.png \
  -F 'crop_to=256,256,856,1056'

```

> When successful (200), Returns info about uploaded file. Same as [Create file](#create-file) example response

```json
{
  // (...) - see 'Create file' example response
}
```

**PUT** `v3/:loyalty_club_slug/files/:fileable_type/:fileable_id/kind`

Updates file for given Fileable (e.g. Offer).

#### URL Parameters

See: [Create File URL Parameters](#create-file-url-parameters)

#### POST Parameters

See: [Create File POST Parameters](#create-file-post-parameters)

#### Response (JSON object)

See: [Create File Response](#create-file-response)

#### Error responses

Status | Reason
--------- | -----------
`404` | Fileable not found
`404` | File not found
`422` | Invalid parameters

<aside class="notice">
Requires <code>Files:Api:Update</code> permit
</aside>

### <a name="reprocess-file"></a> Reprocess file

> Example request

```shell
curl -X PUT \
  https://api.mpc.placewise.com/v3/infinity-mall/files/offers/1000289/offer_default/reprocess \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F 'crop_to=256,256,856,1056'

```

> When successful (200), Returns info about uploaded file. Same as [Create file](#create-file) example response

```json
{
  // (...) - see 'Create file' example response
}
```

**PUT** `v3/:loyalty_club_slug/files/:fileable_type/:fileable_id/kind/reprocess`

Reprocesses file for given Fileable (e.g. Offer).
It means that no file needs to be uploaded, instead the original file will be reprocessed according to given parameters.

#### URL Parameters

See: [Create File URL Parameters](#create-file-url-parameters)

#### POST Parameters

See: [Create File POST Parameters](#create-file-post-parameters) - except no `file` param is accepted here

#### Response (JSON object)

See: [Create File Response](#create-file-response)

#### Error responses

Status | Reason
--------- | -----------
`404` | Fileable not found
`404` | File not found
`422` | Invalid parameters

<aside class="notice">
Requires <code>Files:Api:Reprocess</code> permit
</aside>

### <a name="delete-file"></a> Delete file

> Example request

```shell
curl -X DELETE "https://api.mpc.placewise.com/v3/infinity-mall/files/offers/1000289/offer_default \
  -H 'content-type: application/json' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful (200), returns an empty object

```json
{
  // Empty object
}
```

**DELETE** `v3/:loyalty_club_slug/files/:fileable_type/:fileable_id/kind`

Deletes file of given Fileable (e.g. Offer).

#### URL Parameters

See: [Create File URL Parameters](#create-file-url-parameters)

#### Error responses

Status | Reason
--------- | -----------
`404` | Fileable not found
`404` | File not found

<aside class="notice">
Requires <code>Files:Api:Delete</code> permit
</aside>
