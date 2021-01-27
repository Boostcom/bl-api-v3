## <a name="operations-admin-resource-types"></a> Resource types

### <a name="operations-admin-resource-type-schema"></a> Resource type schema

> Resource type schema example:

```json
{
  "$id": "/v1/users/me/operations/resource_types/schema",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "description": "Description of available ResourceTypes (with their properties)",
  "type": "object",
  "properties": {
    "ResourceType 2": {
      "description": "image/jpeg",
      "type": "object",
      "properties": {
        "ResourceTypeAttribute 3": {
          "type": "string"
        },
        "ResourceTypeAttribute 4": {
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      },
      "required": [
        "ResourceTypeAttribute 4"
      ]
    },
    "ResourceType 1": {
      "description": "image/jpeg",
      "type": "object",
      "properties": {
        "ResourceTypeAttribute 1": {
          "type": "string"
        },
        "ResourceTypeAttribute 2": {
          "type": "array",
          "items": {
            "type": "string"
          }
        }
      },
      "required": [
        "ResourceTypeAttribute 2"
      ]
    }
  }
}
```

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
$id | string | yes | path of this schema (auto-generated)
$schema | string | yes | draft which this schema is compatible with (auto-generated)
description | string | yes | description of this schema (auto-generated)
type | string | yes | type of this schema (auto-generated)
properties | object | no | description of how the resource type properties look like for each of the resource types and their attributes (and whether they are required)

### <a name="operations-admin-show-resource-type-schema"></a> Show resource type schema

> Example

```shell
curl -X GET \
"https://api.mpc.placewise.com/v1/operations/resource_types/schema" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test'
```

> Returns object containing [resource type schema](#operations-admin-resource-type-schema)

```json
  // see 'Resource type schema'
````

**GET** `v1/operations/resource_types/schema`
Returns requested [Resource type schema](#operations-admin-resource-type-schema)

<aside class="notice">
Requires <code>Operations:Api:ResourceTypes:Schema::Show</code> permit
</aside>

### <a name="operations-admin-update-resource-type-schema"></a> Update resource type schema

> Example

```shell
curl -X PUT \
"https://api.mpc.placewise.com/v1/operations/resource_types/schema" \
  -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIyOE' \
  -H 'content-type: application/json' \
  -H 'x-loyalty-club-slug: infinity-mall' \
  -H 'x-user-agent: CURL manual test' \
  -H 'x-b-store-id: 2' \
  -d '
    {
      "$id": "/v1/users/me/operations/resource_types/schema",
      "$schema": "http://json-schema.org/draft-07/schema#",
      "description": "Description of available ResourceTypes (with their properties)",
      "type": "object",
      "properties": {
        "ResourceType 2": {
          "description": "image/jpeg",
          "type": "object",
          "properties": {
            "ResourceTypeAttribute 3": {
              "type": "string"
            },
            "ResourceTypeAttribute 4": {
              "type": "array",
              "items": {
                "type": "string"
              }
            }
          },
          "required": [
            "ResourceTypeAttribute 4"
          ]
        },
        "ResourceType 1": {
          "description": "image/jpeg",
          "type": "object",
          "properties": {
            "ResourceTypeAttribute 1": {
              "type": "string"
            },
            "ResourceTypeAttribute 2": {
              "type": "array",
              "items": {
                "type": "string"
              }
            }
          },
          "required": [
            "ResourceTypeAttribute 2"
          ]
        }
      }
    }
  '
```

> Returns object containing [resource type schema](#operations-admin-resource-type-schema)

```json
  // see 'Resource type schema'
````

**PUT** `v1/operations/resource_types/schema`

Updates the [resource type schema](#operations-admin-resource-type-schema)

#### POST Parameters (JSON)

Key | Type | Optional | Description
--------- | --------- | --------- | ---------
$id | string | yes | path of this schema (auto-generated)
$schema | string | yes | draft which this schema is compatible with (auto-generated)
description | string | yes | description of this schema (auto-generated)
type | string | yes | type of this schema (auto-generated)
properties | object | no | description of how the resource type properties look like for each of the resource types and their attributes (and whether they are required)

#### Response (JSON object)
see [resource type schema](#operations-admin-resource-type-schema)

#### Error responses

Status | Description
--------- | -----------
`422` | Invalid parameters - see [Invalid parameters errors model](#invalid-parameters-errors-model)

<aside class="notice">
Requires <code>Operations:Api:ResourceTypes:Schema:Update</code> permit
</aside>
