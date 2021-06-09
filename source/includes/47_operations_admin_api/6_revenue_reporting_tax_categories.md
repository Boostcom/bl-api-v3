## <a name="operations-admin-revenue-reporting-tax-categories"></a> Revenue reporting tax categories

### <a name="operations-admin-revenue-reporting-tax-category-model"></a> Revenue reporting tax category model

> Revenue reporting tax category example:

```json
{
  "revenue_reporting_tax_category": {
    "id": 89,
    "name": "Revenue reporting tax_category 1",
    "tax_percentage": 23.8,
    "created_at": "2021-05-11T02:16:43.113Z",
    "updated_at": "2021-05-11T02:16:43.113Z"
  }
}
```

A revenue reporting tax_category is an entity that represents a category of tax to which a revenue report will be assigned. It comprises of name and tax percentage - the name has to be unique within a loyalty club, but the tax percentage is optional and currently not used anywhere.

Key | Type | Optional | Description
--------------                 | -------    | --------- | ---------
id                             | integer    | no  |
name                           | string     | no  | name of the tax category
tax_percentage                 | double     | yes | tax percentage of the tax category
created_at                     | datetime   | no  | time of the creation
updated_at                     | datetime   | no  | time of the last update

### <a name="operations-admin-list-revenue-reporting-tax-categories"></a> List tax categories

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_tax_categories" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns an object containing [tax categories](#operations-admin-revenue-reporting-tax-category-model) and [pagination_info](#pagination-model)

```json
{
  "revenue_reporting_tax_categories": [], // List of tax categories - see 'Revenue reporting tax category model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/revenue_reporting_tax_categories`

Returns list of [tax categories](#operations-admin-revenue-reporting-tax-category-model).

#### Query Parameters

Parameter      | Type                            | Default   | Description
-------------- | -----------                     | --------- | -----------
per_page       | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no        | integer                         | 1         | Number of results page

<aside class="notice">
Requires <code>Operations:Api:RevenueReportingTaxCategories:List</code> permit
</aside>

### <a name="operations-admin-show-revenue-reporting-tax-category"></a> Get tax category

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_tax_categories/5" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns object containing [tax category](#operations-admin-revenue-reporting-tax-category-model)

```json
{
  "revenue_reporting_tax_category": {} // see: 'Revenue reporting tax category model'
}
````

**GET** `v1/operations/revenue_reporting_tax_categories/:id`

Returns given [tax category](#operations-admin-revenue-reporting-tax-category-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | tax category ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | tax category not found

<aside class="notice">
Requires <code>Operations:Api:RevenueReportingTaxCategories:Show</code> permit
</aside>

### <a name="operations-admin-create-revenue-reporting-tax-category"></a> Create tax category

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_tax_categories" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
  -d '
    {
      "revenue_reporting_tax_category": {
        "name": "Revenue reporting tax category 1",
        "tax_percentage": 25.00
      }
    }
  '
```

> When successful, returns object containing created [tax category](#operations-admin-revenue-reporting-tax-category-model)

```json
{
  "revenue_reporting_tax_category": {} // tax_category - see: 'Revenue reporting tax category model'
}
```

**POST** `v1/operations/revenue_reporting_tax_categories`

Creates a new [tax category](#operations-admin-revenue-reporting-tax-category-model)

#### POST Parameters (JSON)

Key                                             | Type      | Description
---------                                       | --------- | ---------
**revenue_reporting_tax_category.name           | string    | no  | name of the tax category
**revenue_reporting_tax_category.tax_percentage | double    | no  | tax percentage of the tax category

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
revenue_reporting_tax_category | tax_category | See: [tax category model](#operations-admin-revenue-reporting-tax-category-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:RevenueReportingTaxCategories:Create</code> permit
</aside>

### <a name="operations-admin-update-revenue-reporting-tax-category"></a> Update tax category

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_tax_categories/12" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
  -d '
    {
      "revenue_reporting_tax_category": {
        "name": "Revenue reporting tax category 1",
        "tax_percentage": 25.00
      }
    }
  '
```

> When successful, returns object containing updated [tax category](#operations-admin-revenue-reporting-tax-category-model)

```json
{
  "revenue_reporting_tax_category": {} // tax_category - see 'Revenue reporting tax category model'
}
```

**PUT** `v1/operations/revenue_reporting_tax_categories/:id`

Updates the [tax category](#operations-admin-revenue-reporting-tax-category-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | tax category ID

#### POST Parameters (JSON)

Key                                             | Type       | Description
---------                                       | ---------  | ---------
**revenue_reporting_tax_category.name           | string     | no  | name of the tax category
**revenue_reporting_tax_category.tax_percentage | string     | no  | tax percentage of the tax category

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
revenue_reporting_tax_category | tax_category | See: [tax category model](#operations-admin-revenue-reporting-tax-category-model)

#### Error responses

Status | Description
--------- | -----------
`404` | tax category not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Users:RevenueReportingTaxCategories:Update</code> permit
</aside>

### <a name="operations-admin-destroy-revenue-reporting-tax-category"></a> Destroy tax category

> Example

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_tax_categories/12 \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test'
  -H 'x-b-store-id: 2' \
```

> When successful, returns object containing destroyed [tax category](#operations-admin-revenue-reporting-tax-category-model)

```json
{
  "revenue_reporting_tax_category": {} // tax_category - see: 'Revenue reporting tax-category model'
}
```

**DELETE** `v1/operations/revenue_reporting_tax_categories/:id`

Destroys the [tax category](#operations-admin-revenue-reporting-tax-category-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | tax category ID

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
revenue_reporting_tax_category | tax_category | See: [tax category model](#operations-admin-revenue-reporting-tax-category-model)

#### Error responses

Status | Description
--------- | -----------
`404` | tax category not found

<aside class="notice">
Requires <code>Operations:Api:Users:RevenueReportingTaxCategories:Destroy</code> permit
</aside>
