## <a name="cms-articles"></a> Articles

### <a name="cms-article-model"></a> Article model

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

### <a name="cms-list-articles"></a> List Articles

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/content/articles" \
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

Returns list of only currently published [Articles](#cms-admin-article-model).

#### Query Parameters

Parameter                      | Type                            | Default   | Description
--------------                 | -----------                     | --------- | -----------
per_page                       | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no                        | integer                         | 1         | Number of results page
search                         | string                          | null      | When true, returns only Articles with titles that match the given string

<aside class="notice">
Requires <code>Cms:Api:Users:Articles:List</code> permit
</aside>

### <a name="cms-show-article"></a> Get Article

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/content/article/5" \
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

Returns only currently published [Article](#cms-admin-article-model).

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