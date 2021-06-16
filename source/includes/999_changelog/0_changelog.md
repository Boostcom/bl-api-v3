# Changelog

### 16.06.2021 | Natalia Styrska

* Add [Operationas Admin API -> Delete contact](#operations-members-delete) 
* Move member actions from Operations Admin API to [Members API] (#members-stores-get)

### 14.06.2021 | Piotr Świtlicki

* [MPC-2711] Add preheader to [Messages API -> Template](#messaging-template-model)

### 13.06.2021 | Piotr Świtlicki

* [MPC-3045] Add to get member by user_id in [Members -> Get](#members-get)

### 13.06.2021 | Piotr Świtlicki

* [MPC-2999] Add new endpoint variants to [Members -> Send OTP](#members-send-one-time-password)

### 25.05.2021 | Natalia Styrska

* Add [Stores -> List of floors](#store-floors-list) 

#### 18.05.2021 | Piotr Świtlicki
* Add [Rewards API -> Rewards Program -> Cancel](#rewards-program-cancel-transaction)
* Update [Rewards API -> Rewards Program -> Grant Points](#rewards-program-grant-points) to return created transaction
* Add `cancellation` transaction type in [RewardsAPI -> Transactions](#rewards-transaction-object)

### 17.05.2021 | Natalia Styrska

* Add description to [Operations -> Add user app tokens](#operations-app-tokens) 

#### 17.05.2021 | Piotr Świtlicki

* Add [Operations API -> Alerts](#operations-alerts)
* Add [Operations Admin API -> Alerts](#operations-admin-alerts)

#### 02.12.2021 | Piotr Świtlicki
* Add [Operations API -> Alerts](#operations-alerts)
* Add [Operations Admin API -> Alerts](#operations-admin-alerts)

#### 18.05.2021 | Piotr Świtlicki
* Add [Rewards API > Rewards Program > Cancel](#rewards-program-cancel-transaction)
* Update [Rewards API > Rewards Program > Grant Points](#rewards-program-grant-points) to return created transaction
* Add `cancellation` transaction type in [RewardsAPI > Transactions](#rewards-transaction-object)

#### 17.05.2021 | Piotr Świtlicki
* Add [Operations API > Alerts](#operations-alerts)
* Add [Operations Admin API > Alerts](#operations-admin-alerts)

### 14.05.2021 | Natalia Styrska
* Add description to [Members > Add app tokens](#members-add-app-tokens) 

### 07.05.2021 | Piotr Świtlicki
* Add description to [Operations > Documents](#operations-documents) 

#### 19.03.2021 | Paweł Buczkowski
* Add [Transaction Stamps > Create](#stamps-create)

#### 11.03.2021 | Piotr Świtlicki
* [Tokens Admin API](#tokens-admin) 

#### 05.03.2021 | Piotr Świtlicki
* Add `login_url` query param in [Members > Send one time password E-mail](#members-send-one-time-password-email) 

#### 23.02.2021 | Piotr Świtlicki
* Add support for direct update in [Members imports](#members-imports) 

#### 10.02.2021 | Paweł Buczkowski
* Add [POS Campaign API > List](#pos-campaigns-config-list)

#### 09.02.2021 | Jakub Kruczek
* Destroy File Records documentation [Files API](#files-api)

#### 08.02.2021 | Jakub Kruczek
* Initial Files API documentation [Files API](#files-api)

#### 02.12.2020 | Piotr Świtlicki
* Add `title` param to [Operations API > Documents > Index](#operations-admin-list-documents)

#### 22.01.2021 | Paweł Buczkowski
* Add [Events registration](#events-registration)

#### 15.01.2021 | Piotr Świtlicki
* Add [Rewards API > Grant Points](#rewards-program-grant-points)

#### 14.01.2021 | Piotr Świtlicki
* Add `include_content` and `type` query params to [List Member Messages](#members-messages-list)

#### 18.12.2020 | Piotr Świtlicki
* Add [Members API > Member Lookup](#members-lookup)

#### 18.12.2020 | Piotr Świtlicki
* Rework [Offers Admin API > ImageTemplates > Preview](#offer-admin-image-templates-preview)

#### 15.12.2020 | Piotr Świtlicki
* Add [Messaging API > Settings > Show](#messaging-show-settings)

#### 15.12.2020 | Piotr Świtlicki
* Add [Messaging API > Sendings > Cancel](#messaging-cancel-sending)

#### 08.12.2020 | Piotr Świtlicki
* Add `background_url` to `ImageTemplate` model in [Offers Admin API](#offers-admin)

#### 08.12.2020 | Piotr Świtlicki
* Add `dispatches_count` to `Sending` and `MessageSending` models in [Messaging API](#messaging)

#### 04.12.2020 | Paweł Buczkowski
* Update [POS Campaigns](#pos-campaign-api)

#### 02.12.2020 | Piotr Świtlicki
* Add [Operations API > Documents](#operations-documents)

#### 01.12.2020 | Piotr Świtlicki
* Add [Operations Admin API > Documents](#operations-admin-documents)

#### 23.11.2020 | Piotr Świtlicki
* Add [Offers Admin API > Image Templates > Preview](#offer-admin-image-templates-preview)

#### 23.11.2020 | Piotr Świtlicki
* Add [Messaging API > Sending](#messaging-list-sending-dispatches)

#### 23.11.2020 | Piotr Świtlicki
* Add periodic limits feature to [Offers](#offers-api)

#### 20.11.2020 | Jakub Kruczek
* New API hosts after rebranding to Placewise
* Rename Boostcom references to Placewise
* Add new Placewise logo

#### 09.11.2020 | Piotr Świtlicki
* Add [Offers > Grant](#grant-offers)

#### 30.10.2020 | Paweł Buczkowski
* Add [POS Campaigns > PointsConfig](#points-config)

#### 29.10.2020 | Natalia Styrska
* Add [Operations Admin API > Members > Stores](#members-stores-get)

#### 09.09.2020 | Piotr Świtlicki
* Add [Offers Admin API > ImageTemplates API](#image-templates)
* Add [Offer ImageTemplate model](#offer-image-template)

#### 09.09.2020 | Natalia Styrska
* Update [Transactions > Create events](#transactions-events-create)

#### 17.08.2020 | Piotr Świtlicki
* Add [Links](#links)

#### 17.08.2020 | Jakub Kruczek
* Use lowercase header keys to match HTTP/2 standard
* Add HTTPS (TLS) section [Introduction > Hosts > HTTPS (TLS) requirement](#https-tls-requirement)
* Add new common HTTP code for incorrect headers [Introduction > Common HTTP error codes](#common-http-error-codes)

#### 13.08.2020 | Piotr Świtlicki
* Restructurize docs (group endpoints into APIs)

#### 07.08.2020 | Natalia Styrska
* Add [Points > List](#member-points-list)

#### 02.07.2020 | Natalia Styrska
* Add [Events > Index](#events-list)
* Add [Events > Get](#event-get)
* Add [Events > Validate](#events-validate-member)
* Add [Events > Checkin](#events-checkin-member)
* Add [Events > Invitation](#events-invitation)

#### 02.06.2020 | Piotr Świtlicki

* Extend [Members Groups > Index](#members-groups-list) with filtering parameters
* Add [Members Groups > Types](#members-groups-types)

#### 28.05.2020 | Natalia Styrska

* Extend ([#  Stores](#store-list)) all endpoints with new properties
* Add ([Stores > Zones](#store-zones-list)) endpoint 

#### 21.05.2020 | Piotr Świtlicki
* Extend [Members Groups](#members-groups) to work with audiences (automatic groups)

#### 20.04.2020 | Piotr Świtlicki

* Add `usable_for_seconds` param to [Grant offer](#grant-offer) 
* Add `type` to [Offer](#offer-model) 

#### 09.04.2020 | Piotr Świtlicki

* Add [Invalid parameters errors](#invalid-parameters-errors-model)
* Add [Members Groups](#members-groups)

#### 13.03.2020 | Natalia Styrska

* Extend [Transaction Events > Create](#transactions-events-create) endpoint 

#### 12.03.2020 | Natalia Styrska

* Extend [Points > Create](#points-create) endpoint 

#### 05.03.2020 | Piotr Świtlicki

* Extend [Loyalty Clubs > List](#loyalty-clubs-list) endpoint 

#### 01.03.2020 | Piotr Świtlicki

* Extend [Offers](#offers) API to work within [Member context](offers-oauth-context)
* Rework [Offers](#offers) API to work within [Guest context](offers-guest-access) 

#### 11.02.2020 | Piotr Świtlicki

* Add [Offers > Grant offer](#grant-offer)

#### 03.02.2020 | Natalia Styrska

* Add ([Transactions > Events](#transactions-events-create)) endpoint

#### 11.12.2019 | Piotr Świtlicki

* Update [Offers > Display schemas](#offers-display-schemas) to have [JSONPath](https://support.smartbear.com/readyapi/docs/testing/jsonpath-reference.html) references

#### 05.12.2019 | Piotr Świtlicki

* Add [Links > Generate](#links-generate) 

#### 22.11.2019 | Piotr Świtlicki

* Add [Loyalty Clubs > Get](#loyalty-clubs-get)
* Add info about new `client` [display schema](#offers-display-schemas) item type 

#### 22.10.2019 | Piotr Świtlicki

* Add `collection_position` order_by key to [Offers > List offers](#list-offers)

#### 18.10.2019 | Piotr Świtlicki

* Add [Offer display schemas](#offers-display-schemas)

#### 16.10.2019 | Natalia Styrska

* Add ([Stores > Categories](#store-categories-list)) endpoint
* Add ([Departments > Create](#department-create)) endpoint
* Add ([Departments > Update](#department-update)) endpoint

#### 23.09.2019 | Piotr Świtlicki

* Add `event_occurred_at` param to [Members > Update](#member-event-occured-at-param) 

#### 21.09.2019 | Piotr Świtlicki

* Add `membership_started_at` to [Rewards Program > Get Status](#rewards-program-status) 

#### 19.09.2019 | Natalia Styrska

* Add ([Points > Member points](#member-points)) endpoint
* Add ([Points > Create](#points-create)) endpoint

#### 02.09.2019 | Natalia Styrska
* Change ([Stores > List](#store-list)) rename malls to departments
* Change ([Stores > Get](#store-get)) rename malls to departments
* Change ([Stores > Create](#store-create)) rename malls to departments
* Change ([Stores > Update](#store-update)) rename malls to departments
* Add ([Departments > List](#department-list)) rename malls to departments
* Add ([Departments > Get](#department-get)) rename malls to departments

#### 29.08.2019 | Piotr Świtlicki
* Add [Members > Update app token](#members-update-app-token) endpoint
* Add [OAuth > Update app token](#me-update-app-token) endpoint

#### 27.08.2019 | Piotr Świtlicki
* Add [Offers Events](#offers-events) section

#### 23.08.2019 | Piotr Świtlicki
* Add [Members > Validate](#members-validate) endpoint
* Change [Offers > List](#offers-list) to accept `include_pagination_info` instead of `include_total` param

#### 19.08.2019 | Piotr Świtlicki
* Add [Offers > Reprocess](#reprocess-file)
* Add [`order_by` param](#offers-list-order-by) to Offers List

#### 09.08.2019 | Piotr Świtlicki
* Add [Offers Admin](#offers-admin) section
* Add [Files > Create](#create-file)
* Add [Files > Update](#update-file)
* Add [Files > Delete](#delete-file)

#### 08.08.2019 | Natalia Styrska
Stores

* Add `GET /v3/:loyalty_club_slug/stores` ([Stores > List](#store-list))
* Add `GET /v3/:loyalty_club_slug/stores/:id` ([Stores > Get](#store-get))
* Add `POST /v3/:loyalty_club_slug/stores` ([Stores > Create](#store-create))
* Add `PUT /v3/:loyalty_club_slug/stores` ([Stores > Update](#store-update))
* Add `DELETE /v3/:loyalty_club_slug/stores` ([Stores > Delete](#store-delete))
* Add `GET /v3/:loyalty_club_slug/stores/malls` ([Malls > List](#mall-list))
* Add `GET /v3/:loyalty_club_slug/stores/malls/:id` ([Malls > Get](#mall-get))


#### 27.06.2019 | Piotr Świtlicki
* Introduce [Levels](#rewards-program-levels-program) 

#### 13.05.2019 | Piotr Świtlicki:
* Add `GET /v3/:loyalty_club_slug/translations` ([Translations > List](#list-translations))

#### 06.05.2019 | Piotr Świtlicki:
Offers

* Add `GET /v3/:loyalty_club_slug/members/me/offers/meta` ([Offers > Get offers meta](#offers-meta))
* Add `GET /v3/:loyalty_club_slug/members/me/offers/:id` ([Offers > Get offer](#get-offer))
* Add `GET /v3/:loyalty_club_slug/members/me/offers` ([Offers > List offers](#list-offers))
* Add `POST /v3/:loyalty_club_slug/members/me/offers/:id/use` ([Offers > Use offer](#use-offer))
* Add `PUT /v3/:loyalty_club_slug/members/me/offers/:id/like` ([Offers > Like offer](#like-offer))
* Add `PUT /v3/:loyalty_club_slug/members/me/offers/:id/unlike` ([Offers > Unlike offer](#unlike-offer))

#### 16.04.2019 | Piotr Świtlicki:
* Add `X-Usage-Token` header to [Rewards > Use](#rewards-use)

#### 07.03.2019 | Piotr Świtlicki:
* Add `GET /v3/:loyalty_club_slug/rewards-program/status/achievements_summary` ([Rewards Program > Achievements summary](#rewards-program-achievements-summary))

#### 06.03.2019 | Piotr Świtlicki:
* Add file schema info ([General info> Files schema](#file-schema))
* Add `GET /v3/:loyalty_club_slug/files/schema` ([Files> Get schema](#get-file-schema))

#### 27.02.2019 | Jakub Kruczek:
* Extend consent definitions example ([Loyalty Clubs > Get schema](#loyalty-clubs-schema))

#### 27.02.2019 | Adam Kuś:
* Add `POST /v3/:loyalty_club_slug/transactions` ([Transactions > Create](#transactions-create))
* Add `GET /v3/:loyalty_club_slug/transactions` ([Transactions > List](#transactions-list))
* Add `GET /v3/:loyalty_club_slug/transactions/:id` ([Transactions > Get](#transactions-get))

#### 19.02.2019 | Piotr Świtlicki:
* Add `GET /v3/:loyalty_club_slug/members/:id/rewards-program/status` ([Rewards Program > Get status by member ID](#rewards-program-status-by-member-id))

#### 08.02.2019 | Piotr Świtlicki:

Boostcom Rewards draft

* Add `GET /v3/:loyalty_club_slug/rewards-program/info` ([Rewards Program > Get Info](#rewards-program-info))
* Add `POST /v3/:loyalty_club_slug/members/me/rewards-program/join` ([Rewards Program > Join](#rewards-program-join))
* Add `DELETE /v3/:loyalty_club_slug/members/me/rewards-program/leave` ([Rewards Program > Leave](#rewards-program-leave))
* Add `GET /v3/:loyalty_club_slug/members/me/rewards-program/status` ([Rewards Program > Get status](#rewards-program-status))
* Add `GET /v3/:loyalty_club_slug/members/me/rewards-program/rewards` ([Rewards > List](#rewards-list))
* Add `GET /v3/:loyalty_club_slug/members/me/rewards-program/rewards/purchased` ([Rewards > List purchased](#rewards-list-purchased))
* Add `POST /v3/:loyalty_club_slug/members/me/rewards-program/rewards/:id/purchase` ([Rewards > Purchase](#rewards-purchase))
* Add `POST /v3/:loyalty_club_slug/members/me/rewards-program/rewards/:id/use` ([Rewards > Use](#rewards-use))

#### 07.02.2019 | Dominik Halat:

* Remove `shortening_enabled` param from Emails API ([Emails > Create](#emails-create))

#### 07.02.2019 | Piotr Świtlicki:

* Replace paths: `/v3/loyalty_clubs/:loyalty_club_slug/` with `/v3/:loyalty_clubs/` (old ones will still work)

#### 25.01.2019 | Dominik Halat:

* Add `POST /v3/:loyalty_club_slug/emails` ([Emails > Create](#emails-create))
* Add `db_and_cache` response type for Member#person_id ([Get person id](#members-person-id))

#### 14.12.2018 | Jakub Kruczek:

* Add `updated_at` to consents value on member

#### 04.12.2018 | Piotr Świtlicki:

* Add `language` param to `/v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_registration_password`

#### 13.11.2018 | Piotr Świtlicki:

* Add `PUT /v3/:loyalty_club_slug/members/channels/msisdn/:msisdn/send_verification`([Members > Send MSISDN verification SMS](#members-send-verification-sms))
* Add `PUT /v3/:loyalty_club_slug/members/channels/msisdn/:msisdn/verify` ([Members > Verify MSISDN](#members-verify-msisdn))

#### 08.11.2018 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/smses` ([SMSes > Create](#smses-create))

#### 12.10.2018 | Piotr Świtlicki:

* Add `GET /v3/:loyalty_club_slug/members/imports/:import_id/bulks//by_request_number/:request_number`

#### 03.08.2018 | Piotr Świtlicki:

* Add [Members Import](#members-imports-import-flow)

#### 27.04.2018 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/members/by_email/:email/send_one_time_password` ([Members > Send one time password E-mail](#members-send-one-time-password-email))

#### 27.04.2018 | Piotr Świtlicki:

* Add `channel` and `subchannel` attributes to `Member`
* Add `validate_partially` param in ([Members > Update](members-update) and ([Me > Update](#me-update)
* Add `GET /v3/:loyalty_club_slug/products` ([Loyalty Clubs > List Products](#loyalty-clubs-products))
* Add `POST /v3/:loyalty_club_slug/push_notifications` ([Push Notifications > Create](#push-notification-create))

#### 11.04.2018 | Dominik Halat

* Add `person_id` to `Member`
* Add new endpoint `GET /v3/:loyalty_club_slug/members/person_id` ([Members > List](#members-index))

#### 01.02.2017 | Piotr Świtlicki:

* Add `ids` query param to `GET /v3/:loyalty_club_slug/members` ([Members > List](#members-index))

#### 29.01.2017 | Piotr Świtlicki:

* Add `GET /v3/:loyalty_club_slug/members` ([Members > List](#members-index))

#### 22.01.2017 | Jakub Kruczek:

* Update staging domain to: `https://api.mpc.dev.placewise.com/`

#### 9.01.2017 | Piotr Świtlicki:

* Update staging domain to: `https://bl-api.bl-stg.boostcom.cc/`

#### 4.01.2017 | Jakub Kruczek:

* Add `consents` fields to schema and Member payload

#### 15.12.2017 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_registration_password`
* Add information about `registration_password` in [Members > Create](#members-create)
* Add some other minor fixes

#### 10.11.2017 | Piotr Świtlicki:

* Restructurize the docs
* Add `POST /v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_one_time_password`

#### 27.10.2017 | Piotr Świtlicki: 

* Rename `/check_existance` endpoints to `/public_info`
* Add :can_login attribute to `/public_info` response
* Remove :exists attribute from `/public_info` response and make it to return nil when member does not exist
