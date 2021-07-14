## <a name="cms-admin-articles"></a> Articles

### <a name="cms-admin-article-model"></a> Article model

> Document example:

```json
{
   "id": 15,
   "status": "draft",
   "title": "Title",
   "description": "<html>Html content</html>",
   "lead": null,
   "image_url": "http://image.url",
   "published_at": "2021-07-14T12:54:07.076Z",
   "created_at": "2021-07-14T12:54:07.076Z"
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
status | enum: `['draft', 'published']` | no |
title | string | no |
description | string | no |
lead | string | yes | currently not used
created_at | datetime | no | Time of creation
published_at | datetime | yes | Time of publication
image_url | string | yes |

### <a name="cms-admin-list-articles"></a> List Articles

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/content/articles" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [article](#cms-admin-article-model) and [pagination_info](#pagination-model)

```json
{
  "articles": [], // List of articles - see 'Article model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/content/articles`

Returns list of [Articles](#cms-admin-article-model).

#### Query Parameters

Parameter                      | Type                            | Default   | Description
--------------                 | -----------                     | --------- | -----------
per_page                       | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no                        | integer                         | 1         | Number of results page
status                         | enum `['draft', 'published']`   | null      | When present, returns only Articles having given status
search                         | string                          | null      | When true, returns only Articles with titles that match the given string
currently_published            | bool                            | null      | When present, returns only published articles (visible for tenants)

<aside class="notice">
Requires <code>Cms:Api:Articles:List</code> permit
</aside>

### <a name="cms-admin-show-article"></a> Get Article

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/content/article/5" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [article](#cms-admin-article-model)

```json
{
  "article": {} // see: 'Article model'
}
````

**GET** `v1/content/articles/:id`

Returns given [Article](#cms-admin-article-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Article ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Article not found

<aside class="notice">
Requires <code>Cms:Api:Articles:Show</code> permit
</aside>

### <a name="cms-admin-create-article"></a> Create Article

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/content/articles" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        {
            "title": "title",
            "status": "draft",
            "description": "description"
        }
    }
  '
```

> When successful, returns object containing created [article](#cms-admin-article-model)

```json
{
  "article": {} // Article - see: 'Article model'
}
```

**POST** `v1/content/articles`

Creates a new [Article](#cms-admin-article-model)

#### POST Parameters (JSON)

Key                          | Type                             | Description
---------                    | ---------                        | ---------
**title**                    | string                           |
**description**              | string                           |
**status**                   | enum: `['draft', 'published']`   |

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
article | Article | See: [Article model](#cms-admin-article-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)


<aside class="notice">
Requires <code>Cms:Api:Articles:Create</code> permit
</aside>

### <a name="cms-admin-update-article"></a> Update Article

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/content/articles/12" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test' \
  -d '
    {
        {
            "title": "title",
            "status": "draft",
            "description": "description"
        }
    }
  '
```

> When successful, returns object containing updated [article](#cms-admin-article-model)

```json
{
  "article": {} // Article - see 'Article model'
}
```

**PUT** `v1/content/articles/:id`

Updates the [Article](#cms-admin-article-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Article ID

#### POST Parameters (JSON)

Key                          | Type                    | Description
---------                    | ---------               | ---------
**title**           | string                           |
**description**     | string                           |
**status**          | enum: `['draft', 'published']`   |

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
article | Article | See: [Article model](#cms-admin-article-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Article not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Cms:Api:Articles:Update</code> permit
</aside>

### <a name="cms-admin-destroy-article"></a> Destroy Article

> Example

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v1/contnet/articles/12 \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-product-name: default' \
  -H 'x-user-agent: CURL manual test'
```

> When successful, returns object containing destroyed [article](#cms-admin-article-model)

```json
{
  "article": {} // Article - see: 'Article model'
}
```

**DELETE** `v1/content/articles/:id`

Destroys the [Article](#cms-admin-article-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Article ID

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
article | Article | See: [Article model](#content-admin-article-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Article not found

<aside class="notice">
Requires <code>Cms:Api:Articles:Destroy</code> permit
</aside>
