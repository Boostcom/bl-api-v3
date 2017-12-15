# Common params and headers

## Content-Type header

This API supports `application/json` only. 

Both payload and response body are supposed to be a valid JSON so when making a request send a `Content-Type: application/json` header.

If payload is not a valid JSON, then `406 Not Acceptable` HTTP code and empty response body are returned.

## X-Client-Authorization header

> Example header: `X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM`

All of the endpoints **require** a client authorization header - `X-Client-Authorization`.

It should be used only on backend and never exposed in frontend code.

Each token has are permits assigned to it. Depending what permits are assigned, access to some of the endpoints
may be restricted.

If you miss your authentication token, please [let us know](http://boostcom.no).

## X-User-Agent header

> Example header: `X-User-Agent: Infinity Mall Android App `
  
`X-User-Agent` is **required** to distinguish specific client for information and debugging purposes so we better know who uses the service.

It should be arbitrarily chosen to represent client specifics (e.g. 'Infinity Mall Android App v1.2' or 'Infinity Mall Backend Service')

## X-Product-Name header

> Example header: `X-Product-Name: android-app`

Each system that is communicating with us should uniquely identify itself so it is possible to distinguish optin/update channels.
That will allow further targeting members by communication channel.

For that we use product name. It should be passed as **required** header `X-Product-Name` that is intended to provide the necessary granularity.

If you miss your product name, please [let us know](http://boostcom.no).

## Authorization header

> Example header: `Authorization: Bearer 8433d608645345a45ce5a0f5ba1225e57546e86ac49e5fec842159dc82218522`

See: [OAuth2](#v3-oauth2)

## Loyalty club slug param

> Example slug: 'infinity-mall'

As all of the API endpoints work in the context of specific Loyalty Club, they're scoped within it's identifier - `:loyalty_club_slug`

It's an unique slugified name of the Loyalty Club.

## Msisdn param

Members in our systems may be identified with their phone numbers (msisdns) according to [E.164](https://en.wikipedia.org/wiki/E.164)
We use format without leading `00` or `+` and without spaces, so that it contains only digits, i.e. `4740485124`.
