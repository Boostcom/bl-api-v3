# POS Campaign API


##<a name="pos-campaigns-config-create"></a> Create

> Example:

```shell
curl -X POST \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/pos-campaigns/points" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
         
         "name": "Default Campaign",
         "bonus": {
             "type": "amount",
             "value": 50
         },
         "conditions": 
         {
             "condition":"Or",
             "arg": [
                 {
                     "condition":"StoreId",
                     "arg": "910"
                 },
                 {
                     "condition":"ItemId",
                     "arg" : "112"
                 }
             ]
         },
         "start":  "2019-02-02T18:48:47+08:00",
         "end" :  "2022-02-02T18:48:47+08:00"
     }'
```

> When successful (200), returns a feedback object structured like this:

```json
{"id":1}
``` 

> When payload is invalid (422), returns an object structured like this:

```json
{
    "error":"Invalid format",
    "details":
    {
        "bonus.value":"String value found, but an integer is required"
    }
}
``` 

**POST** `v3/:loyalty_club_slug/pos-campaigns/:type`

Create POS Campaign.

#### URL Parameters

Parameter | Description                                  | Type
--------- | -------------------------------------------- | ------
type |        POS Campaign type                          | enum ["points", "stamps"]   

#### Messages POST Parameters (JSON)


Key | Type | Default | Description
--------- | --------- | --------- | --------- 
name* | string | n/a |
bonus* | Bonus | n/a |
conditions* | Condition | n/a |
start* | string | use format with timezone |
end* | string | use format with timezone |
\* required

`Bonus` object is a hash with following attributes:

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
type* | enum: ["amount", "percentage", "extra"] | n/a |
value* | integer | n/a |  |
\* required

`Condition` object is a hash with following attributes:

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
condition* | string | n/a | Class name
args* | Condition, Condition[], integer, string, array | n/a |  |
\* required

### <a name="pos-campaigns-config-conditions"></a> Conditions List
Condition | Args | Type | Meets requirements when
--------- | --------- | --------- | ---------  
`Or` | Condition[] | One of| One of conditions meets requirements
`And` | Condition[] | All of |All of conditions meet requirements
`Not` | Condition | Negation |Condition does not meet requirements
`ItemId` | string | Equal | Item ID is equal to arg
`CategoryId` | string | Equal | Category ID is equal to arg
`StoreId` | string | Equal | Store ID is equal to arg

> Example:

```json
{
    "condition":"Or",
    "arg": 
    [
        {
            "condition":"StoreId",
            "arg": "910"
        },
        {
            "condition":"And",
            "arg" : 
            [
                {
                    "condition":"ItemId",
                    "arg" : "112"
                },
                {
                    "condition":"Not",
                    "arg" : 
                    {
                        "condition":"StoreId",
                        "arg": "910"
                    }
                }
            ]     
        }
    ]
}
```

## <a name="pos-campaigns-config-delete"></a> Delete

> Example:

```shell
curl -X DELETE \
"https://api.mpc.placewise.com/v3/:loyalty_club_slug/pos-campaigns/points" \
    -H 'content-type: application/json' \
    -H 'x-client-authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
    -H 'x-product-name: default' \
    -H 'x-user-agent: CURL manual test'
    -d \
    '{
         "id":1
     }'
```

> When successful (200), returns empty array:

```json
[]
``` 

> When payload is invalid (422), returns an object structured like this:

```json
{
    "error":"Can not delete campaign. Campaign does not exist."
}
``` 

**DELETE** `v3/:loyalty_club_slug/pos-campaigns/:type`

Remove POS Campaign.


#### URL Parameters

Parameter | Description                                  | Type
--------- | -------------------------------------------- | ------
type |        POS Campaign type                          | enum ["points", "stamps"]   


#### Messages DELETE Parameters (JSON)

Key | Type | Default | Description
--------- | --------- | --------- | --------- 
id* | integer | n/a | ID obtained by [CREATE](#pos-campaigns-config-create)  method
\* required