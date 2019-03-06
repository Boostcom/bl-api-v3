# Endpoints &bull; Files (WIP)

## <a name="v3-get-files-schema"></a> Get files schema

> Example request

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/files/schema" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> Example response

```json
// see example schema in [Files -> Schema]
```

**GET** `v3/:loyalty_club_slug/files/schema`

Returns the Loyalty Club files schema. See: [Files &bull; Schema](#v3-file-schema)

<aside class="notice">
Requires <code>Files:Api:GetSchema</code> permit
</aside>
