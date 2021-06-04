## <a name="operations-revenue-reports"></a> Revenue reports

### <a name="operations-revenue-report-model"></a> Revenue report model

> Revenue report example:

```json
{
  "revenue_report": {
    "id": 128,
    "revenue_reporting_policy": {
      "id": 282,
      "name": "Some policy",
      "frequency_unit": "week",
      "is_common": null,
      "report_total_transaction_count": null,
      "tax_categories": [
        {
          "id": 192,
          "name": "Some tax category",
          "tax_percentage": 23.0,
          "created_at": "2021-06-01T14:12:37.658Z",
          "updated_at": "2021-06-01T14:12:37.679Z"
        }
      ],
      "excluded_store_ids": [2, 8],
      "created_at": "2021-06-01T14:12:37.610Z",
      "updated_at": "2021-06-01T14:12:37.610Z"
    },
    "revenue_reporting_policy_tax_category": {
      "id": 192,
      "name": "Some tax category",
      "tax_percentage": 23.0,
      "created_at": "2021-06-01T14:12:37.658Z",
      "updated_at": "2021-06-01T14:12:37.679Z"
    },
    "transaction_count": 2,
    "value_cents": 10,
    "value": 10,
    "total": 240,
    "user_id": "some_user_id",
    "member_id": 3,
    "deadline": "2021-06-03T14:12:37.661Z",
    "created_at": "2021-06-01T14:12:37.735Z",
    "updated_at": "2021-06-01T14:12:37.735Z"
  }
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
id | integer | no |
revenue_reporting_policy | object | no | see [Revenue Reporting policy model](#operations-admin-revenue-reporting-policy-model)
revenue_reporting_policy_tax_category | object | no | see [Revenue Reporting policy/tax categories model](#operations-admin-revenue-reporting-policy-model)
transaction_count | integer | no | total count of the transactions for the given day
value_cents | integer | no | total value of the transactions for the given day in cents
value | integer | no | same as value_cents - used for convenience
total | integer | no | value_cents multiplied by the associated tax category tax_percentage
user_id | string | yes | user_id of the tenant that has most recently updated the record (could be blank)
member_id | integer | yes | member_id of the tenant that has most recently updated the record (could be blank)
deadline | datetime | no | time of the deadline by which the report has to be fulfilled
created_at | datetime | no | time of the report creation
updated_at | datetime | no | time of the last report update

For more details, see [Revenue report model](#operations-admin-revenue-report-model) in Operations Admin API

### <a name="operations-list-revenue-reports"></a> List Revenue reports

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/revenue_reports" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [reports](#operations-revenue-report-model) and [pagination_info](#pagination-model)

```json
{
  "revenue_reports": [], // List of revenue reports - see 'Revenue report model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/users/me/operations/revenue_reports`

Returns list of [Revenue reports](#operations-revenue-report-model) shared with current user.

#### Query Parameters

Parameter              | Type                            | Default   | Description
--------------         | -----------                     | --------- | -----------
per_page               | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no                | integer                         | 1         | Number of results page
start_date             | datetime                        | null      | Starting range of the created_at report filtering
end_date               | datetime                        | null      | Ending range of the created_at report filtering
tax_category_ids       | array[integer]                  | []        | comma-separated ids of the revenue_reporting_policy_tax_categories to filter the entries by

<aside class="notice">
Requires <code>Operations:Api:Users:RevenueReports:List</code> permit
</aside>

### <a name="operations-list-store-revenue-reports"></a> List store Revenue reports

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/revenue_reports/store/3?tax_category_ids=2,3&start_date=2021-01-02&end_date=2021-01-03" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [reports](#operations-revenue-report-model) and [pagination_info](#pagination-model)

```json
{
  "revenue_reports": [], // List of revenue reports - see 'Revenue report model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/users/me/operations/revenue_reports/store/:store_id`

Returns list of [Revenue reports](#operations-revenue-report-model) shared with current user.

#### Query Parameters

Parameter              | Type                            | Default   | Description
--------------         | -----------                     | --------- | -----------
store_id               | integer                         | null      | .id of the store entries belonging to are to be returned
per_page               | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no                | integer                         | 1         | Number of results page
start_date             | datetime                        | null      | Starting range of the created_at report filtering
end_date               | datetime                        | null      | Ending range of the created_at report filtering
tax_category_ids       | array[integer]                  | []        | comma-separated ids of the revenue_reporting_policy_tax_categories to filter the entries by

<aside class="notice">
Requires <code>Operations:Api:Users:RevenueReports:List</code> permit
</aside>

### <a name="operations-show-revenue-report"></a> Get Revenue report

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/users/me/operations/revenue_reports/5" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [revenue report](#operations-revenue-report-model)

```json
{
  "revenue_report": {} // see: 'Revenue report model'
}
````

**GET** `v1/users/me/operations/revenue_reports/:id`

Returns given [Revenue report](#operations-revenue-report-model) shared with current user.

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Revenue report ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Revenue report not found

<aside class="notice">
Requires <code>Operations:Api:Users:RevenueReports:Show</code> permit
</aside>

### <a name="operations-admin-update-revenue-report"></a> Update Revenue report

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/revenue_reports/12" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
  -d '
    {
      "revenue_report": {
        "transaction_count": 2,
        "value_cents": 10,
      }
    }
  '
```

> When successful, returns object containing updated [report](#operations-admin-revenue-report-model)

```json
{
  "revenue_report": {} // Report - see 'Revenue report model'
}
```

**PUT** `v1/operations/revenue_reports/:id`

Updates the [Revenue report](#operations-admin-revenue-report-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Revenue report ID

#### POST Parameters (JSON)

Key                                  | Type       | Description
---------                            | ---------- | ---------
**revenue_report.transaction_count** | integer    | total count of the transactions for the given day
**revenue_report.value_cents**       | integer    | total value of the transactions for the given day in cents

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
revenue_report | Revenue report | See: [Revenue report model](#operations-admin-revenue-report-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Revenue report not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Users:RevenueReports:Update</code> permit
</aside>
