# Files API

## Introduction

### <a name="file-record"></a> FileRecord model

```json
{
  "id": 9,
  "type": "logo",
  "identifier": "42-sth",
  "customer_id": 447,
  "loyalty_club_id": 583,
  "user_id": "xxx-56b1-xxx-a9ca-xxxx",
  "url": null,
  "filename": "this-is-my-file.png",
  "mime_type": "image/png",
  "size": 23172,
  "created_at": "2020-10-13T10:01:35.586Z",
  "updated_at": "2020-10-13T10:01:35.586Z",
  "active": true,
  "last_active_at": null,
  "duplicated_from_file_record_id": null
}
```

Key | Type | Optional | Description
--- | ---  | --- | -----------
id | integer | no |
type | string | no | Type of resource to which file is attributed to
identifier | string | no | ID of resource to which file is attributed to
customer_id | integer | yes | ID of customer which owns the file
loyalty_club_id | integer | yes | ID of loyalty club which owns the file
user_id | string | yes | ID of user which uploaded file
url | string | yes | URL to file
filename | string | no | File's name
mime_type | string | no | File's mime type
size | integer | no | Size of uploaded file
created_at | datetime | no | Time of creation
updated_at | datetime | no | Time of last update
active | boolean | no | Is file currently active
last_active_at | datetime | yes | Time when file was deactivated
duplicated_from_file_record_id | integer | yes | ID of file record from which current one was duplicated

## <a name="list-files"></a> List File Records

> Example request

```shell
curl --location --request GET 'https://api.mpc.dev.placewise.com/v1/files' \
--header 'x-product-name: default' \
--header 'x-user-agent: CURL manual test' \
--header 'x-customer: 447' \
--header 'authorization: Bearer JWTTOKEN'
```

> Returns object containing [file record](#file-record)

```json
{
  "file_records": {} // see: FileRecord model
}
```

**GET** `v1/files`

Returns list of file records.

#### Query Parameters

Parameter              | Type        | Default   | Description
--------------         | ----------- | --------- | -----------
per_page               | integer     | 100       | Number of results to be returned per request (100 is the maximum)
page_no                | integer     | 1         | Number of results page
ids                    | integer[]   | null      | When given, returned file records are filtered by id
file_identifiers       | string[]    | null      | When given, returned file records are filtered by file's type & identifier. Format: `#{type}:#{identifier}`.
with_inactive          | boolean     | false     | When true fetches also inactive file records


<aside class="notice">
Requires <code>Files:Api:Files:List</code> permit
</aside>
