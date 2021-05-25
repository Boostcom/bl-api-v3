## <a name="operations-admin-revenue-reporting-policies"></a> Revenue reporting policies

### <a name="operations-admin-revenue-reporting-policy-model"></a> Revenue reporting policy model

> Revenue reporting policy example:

```json
{
  "revenue_reporting_policy": {
    "id": 89,
    "name": "Revenue reporting policy 1",
    "frequency_unit": "week",
    "is_common": null,
    "report_total_transaction_count": null,
    "tax_categories": [
      {
        "id": 3,
        "name": "25%",
        "tax_percentage": 25.00,
        "created_at": "2021-05-11T02:16:43.113Z",
        "updated_at": "2021-05-11T02:16:43.113Z"
      }
    ],
    "excluded_store_ids": [3],
    "created_at": "2021-05-11T02:16:43.113Z",
    "updated_at": "2021-05-11T02:16:43.113Z"
  }
}
```

A revenue reporting policy is an entity that represents a policy, according to which the revenue reports for the given store will be generated. It could be assigned to multiple (or none) stores at once.
There is a special kind of a policy - a *default* one, represented by the `is_common` flag set to true. There could only be one *default* policy per loyalty club at once. Other policies could inherit some of the properties from it.
As an admin, one can list, create, update and destroy such policies.

Key | Type | Optional | Description
--------------                 | -------    | --------- | ---------
id                             | integer    | no  |
name                           | string     | no  | name of the policy
frequency_unit                 | string     | no  | unit of the report generation frequency - could be one of `day, week, month, quarter, year`
is_common                      | boolean    | yes | marks the policy as the default one for the loyalty club
report_total_transaction_count | boolean    | yes | marks the reports generated accordingly to the policy as having to report total transaction count (or not)
tax_categories                 | object     | yes | represents tax categories represented in the reports generated according to the policy
excluded_store_ids             | array[int] | yes | represents the ids of the stores to which the policy is *NOT* assigned (all others are in by default)
created_at                     | datetime   | no  | time of the creation
updated_at                     | datetime   | no  | time of the last update

### <a name="operations-admin-list-revenue-reporting-policies"></a> List policies

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_policies" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns an object containing [policies](#operations-admin-revenue-reporting-policy-model) and [pagination_info](#pagination-model)

```json
{
  "revenue_reporting_policies": [], // List of policies - see 'Revenue reporting policy model'
  "pagination_info": {} // Pagination info - see 'Pagination info'
}
````

**GET** `v1/operations/revenue_reporting_policies`

Returns list of [policies](#operations-admin-revenue-reporting-policy-model).

#### Query Parameters

Parameter      | Type                            | Default   | Description
-------------- | -----------                     | --------- | -----------
per_page       | integer                         | 100       | Number of results to be returned per request (100 is the maximum)
page_no        | integer                         | 1         | Number of results page

<aside class="notice">
Requires <code>Operations:Api:RevenueReportingPolicies:List</code> permit
</aside>

### <a name="operations-admin-show-common-revenue-reporting-policy"></a> Get the common (default) policy

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_policies/common" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns object containing [policy](#operations-admin-revenue-reporting-policy-model)

```json
{
  "revenue_reporting_policy": {} // see: 'Revenue reporting policy model'
}
````

**GET** `v1/operations/revenue_reporting_policies/common`

Returns given [policy](#operations-admin-revenue-reporting-policy-model).

#### Error responses

Status    | Description
--------- | -----------
`404`     | Policy not found

<aside class="notice">
Requires <code>Operations:Api:RevenueReportingPolicies:Show</code> permit
</aside>

### <a name="operations-admin-show-revenue-reporting-policy"></a> Get policy

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_policies/5" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
```

> Returns object containing [policy](#operations-admin-revenue-reporting-policy-model)

```json
{
  "revenue_reporting_policy": {} // see: 'Revenue reporting policy model'
}
````

**GET** `v1/operations/revenue_reporting_policies/:id`

Returns given [policy](#operations-admin-revenue-reporting-policy-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Policy ID

#### Error responses

Status    | Description
--------- | -----------
`404`     | Policy not found

<aside class="notice">
Requires <code>Operations:Api:RevenueReportingPolicies:Show</code> permit
</aside>

### <a name="operations-admin-create-revenue-reporting-policy"></a> Create policy

> Example

```shell
curl -X POST \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_policies" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
  -d '
    {
      "revenue_reporting_policy": {
        "name": "Revenue reporting policy 1",
        "frequency_unit": "week",
        "is_common": null,
        "report_total_transaction_count": null,
        "tax_categories": [
          {
            "name": "25%",
            "tax_percentage": 25.00,
          }
        ],
        "excluded_store_ids": [3],
      }
    }
  '
```

> When successful, returns object containing created [policy](#operations-admin-revenue-reporting-policy-model)

```json
{
  "revenue_reporting_policy": {} // Policy - see: 'Revenue reporting policy model'
}
```

**POST** `v1/operations/revenue_reporting_policies`

Creates a new [policy](#operations-admin-revenue-reporting-policy-model)

#### POST Parameters (JSON)

Key                              | Type      | Description
---------                        | --------- | ---------
**revenue_reporting_policy.name                           | string     | no  | name of the policy
**revenue_reporting_policy.frequency_unit                 | string     | no  | unit of the report generation frequency - could be one of `day, week, month, quarter, year`
**revenue_reporting_policy.is_common                      | boolean    | yes | marks the policy as the default one for the loyalty club
**revenue_reporting_policy.report_total_transaction_count | boolean    | yes | marks the reports generated accordingly to the policy as having to report total transaction count (or not)
**revenue_reporting_policy.tax_categories                 | object     | yes | represents tax categories represented in the reports generated according to the policy
**revenue_reporting_policy.excluded_store_ids             | array[int] | yes | represents the ids of the stores to which the policy is *NOT* assigned (all others are in by default)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
revenue_reporting_policy | Policy | See: [Policy model](#operations-admin-revenue-reporting-policy-model)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:RevenueReportingPolicies:Create</code> permit
</aside>

### <a name="operations-admin-update-revenue-reporting-policy"></a> Update policy

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_policies/12" \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
  -d '
    {
      "revenue_reporting_policy": {
        "name": "Revenue reporting policy 1",
        "frequency_unit": "week",
        "is_common": null,
        "report_total_transaction_count": null,
        "tax_categories": [
          {
            "name": "25%",
            "tax_percentage": 25.00,
          }
        ],
        "excluded_store_ids": [3],
      }
    }
  '
```

> When successful, returns object containing updated [policy](#operations-admin-revenue-reporting-policy-model)

```json
{
  "revenue_reporting_policy": {} // Policy - see 'Revenue reporting policy model'
}
```

**PUT** `v1/operations/revenue_reporting_policies/:id`

Updates the [Policy](#operations-admin-revenue-reporting-policy-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Policy ID

#### POST Parameters (JSON)

Key                              | Type      | Description
---------                        | --------- | ---------
**revenue_reporting_policy.name                           | string     | no  | name of the policy
**revenue_reporting_policy.frequency_unit                 | string     | no  | unit of the report generation frequency - could be one of `day, week, month, quarter, year`
**revenue_reporting_policy.is_common                      | boolean    | yes | marks the policy as the default one for the loyalty club
**revenue_reporting_policy.report_total_transaction_count | boolean    | yes | marks the reports generated accordingly to the policy as having to report total transaction count (or not)
**revenue_reporting_policy.tax_categories                 | object     | yes | represents tax categories represented in the reports generated according to the policy
**revenue_reporting_policy.excluded_store_ids             | array[int] | yes | represents the ids of the stores to which the policy is *NOT* assigned (all others are in by default)

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
revenue_reporting_policy | Policy | See: [Policy model](#operations-admin-revenue-reporting-policy-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Policy not found
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:Users:RevenueReportingPolicies:Update</code> permit
</aside>

### <a name="operations-admin-destroy-revenue-reporting-policy"></a> Destroy policy

> Example

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v1/operations/revenue_reporting_policies/12 \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'x-user-agent: CURL manual test'
  -H 'x-b-store-id: 2' \
```

> When successful, returns object containing destroyed [policy](#operations-admin-revenue-reporting-policy-model)

```json
{
  "revenue_reporting_policy": {} // Policy - see: 'Revenue reporting policy model'
}
```

**DELETE** `v1/operations/revenue_reporting_policies/:id`

Destroys the [policy](#operations-admin-revenue-reporting-policy-model).

#### URL Parameters

Parameter  | Type    | Description
---------- | --------| ------
id         | integer | Policy ID

#### Response (JSON object)

Key | Type  | Description
---------- | -------- | ---------
revenue_reporting_policy | Policy | See: [Policy model](#operations-admin-revenue-reporting-policy-model)

#### Error responses

Status | Description
--------- | -----------
`404` | Policy not found

<aside class="notice">
Requires <code>Operations:Api:Users:RevenueReportingPolicies:Destroy</code> permit
</aside>
