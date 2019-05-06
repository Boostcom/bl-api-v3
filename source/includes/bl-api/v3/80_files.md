# Endpoints &bull; Files

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
