# Changelog

#### 15.01.2021 | Piotr Świtlicki
* Add [Rewards API &bull; Grant Points](#rewards-program-grant-points)

#### 14.01.2021 | Piotr Świtlicki
* Add `include_content` and `type` query params to [List Member Messages](#members-messages-list)

#### 18.12.2020 | Piotr Świtlicki
* Add [Members API &bull; Member Lookup](#members-lookup)

#### 18.12.2020 | Piotr Świtlicki
* Rework [Offers Admin API &bull; ImageTemplates &bull; Preview](#offer-admin-image-templates-preview)

#### 15.12.2020 | Piotr Świtlicki
* Add [Messaging API &bull; Settings &bull; Show](#messaging-show-settings)

#### 15.12.2020 | Piotr Świtlicki
* Add [Messaging API &bull; Sendings &bull; Cancel](#messaging-cancel-sending)

#### 08.12.2020 | Piotr Świtlicki
* Add `background_url` to `ImageTemplate` model in [Offers Admin API](#offers-admin)

#### 08.12.2020 | Piotr Świtlicki
* Add `dispatches_count` to `Sending` and `MessageSending` models in [Messaging API](#messaging)

#### 04.12.2020 | Paweł Buczkowski
* Update [POS Campaigns](#pos-campaign-api)

#### 02.12.2020 | Piotr Świtlicki
* Add [Operations API &bull; Documents](#operations-documents)

#### 01.12.2020 | Piotr Świtlicki
* Add [Operations Admin API &bull; Documents](#operations-admin-documents)

#### 23.11.2020 | Piotr Świtlicki
* Add [Offers Admin API &bull; Image Templates &bull; Preview](#offer-admin-image-templates-preview)

#### 23.11.2020 | Piotr Świtlicki
* Add [Messaging API &bull; Sending](#messaging-list-sending-dispatches)

#### 23.11.2020 | Piotr Świtlicki
* Add periodic limits feature to [Offers](#offers-api)

#### 20.11.2020 | Jakub Kruczek
* New API hosts after rebranding to Placewise
* Rename Boostcom references to Placewise
* Add new Placewise logo

#### 09.11.2020 | Piotr Świtlicki
* Add [Offers &bull; Grant](#grant-offers)

#### 30.10.2020 | Paweł Buczkowski
* Add [POS Campaigns &bull; PointsConfig](#points-config)

#### 29.10.2020 | Natalia Styrska
* Add [Operations Admin API &bull; Members &bull; Stores](#members-stores-get)

#### 09.09.2020 | Piotr Świtlicki
* Add [Offers Admin API &bull; ImageTemplates API](#image-templates)
* Add [Offer ImageTemplate model](#offer-image-template)

#### 09.09.2020 | Natalia Styrska
* Update [Transactions &bull; Create events](#transactions-events-create)

#### 17.08.2020 | Piotr Świtlicki
* Add [Links](#links)

#### 17.08.2020 | Jakub Kruczek
* Use lowercase header keys to match HTTP/2 standard
* Add HTTPS (TLS) section [Introduction &bull; Hosts &bull; HTTPS (TLS) requirement](#https-tls-requirement)
* Add new common HTTP code for incorrect headers [Introduction &bull; Common HTTP error codes](#common-http-error-codes)

#### 13.08.2020 | Piotr Świtlicki
* Restructurize docs (group endpoints into APIs)

#### 07.08.2020 | Natalia Styrska
* Add [Points &bull; List](#member-points-list)

#### 02.07.2020 | Natalia Styrska
* Add [Events &bull; Index](#events-list)
* Add [Events &bull; Get](#event-get)
* Add [Events &bull; Validate](#events-validate-member)
* Add [Events &bull; Checkin](#events-checkin-member)
* Add [Events &bull; Invitation](#events-invitation)

#### 02.06.2020 | Piotr Świtlicki

* Extend [Members Groups &bull; Index](#members-groups-list) with filtering parameters
* Add [Members Groups &bull; Types](#members-groups-types)

#### 28.05.2020 | Natalia Styrska

* Extend ([#  Stores](#store-list)) all endpoints with new properties
* Add ([Stores &bull; Zones](#store-zones-list)) endpoint 

#### 21.05.2020 | Piotr Świtlicki
* Extend [Members Groups](#members-groups) to work with audiences (automatic groups)

#### 20.04.2020 | Piotr Świtlicki

* Add `usable_for_seconds` param to [Grant offer](#grant-offer) 
* Add `type` to [Offer](#offer-model) 

#### 09.04.2020 | Piotr Świtlicki

* Restructure members docs
* Add [Invalid parameters errors](#invalid-parameters-errors-model)
* Add [Members Groups](#members-groups)

#### 13.03.2020 | Natalia Styrska

* Extend [Transaction Events &bull; Create](#transactions-events-create) endpoint 

#### 12.03.2020 | Natalia Styrska

* Extend [Points &bull; Create](#points-create) endpoint 

#### 05.03.2020 | Piotr Świtlicki

* Extend [Loyalty Clubs &bull; List](#loyalty-clubs-list) endpoint 

#### 01.03.2020 | Piotr Świtlicki

* Extend [Offers](#offers) API to work within [Member context](offers-oauth-context)
* Rework [Offers](#offers) API to work within [Guest context](offers-guest-access) 

#### 11.02.2020 | Piotr Świtlicki

* Add [Offers &bull; Grant offer](#grant-offer)

#### 03.02.2020 | Natalia Styrska

* Add ([Transactions &bull; Events](#transactions-events-create)) endpoint

#### 11.12.2019 | Piotr Świtlicki

* Update [Offers &bull; Display schemas](#offers-display-schemas) to have [JSONPath](https://support.smartbear.com/readyapi/docs/testing/jsonpath-reference.html) references


#### 05.12.2019 | Piotr Świtlicki

* Add [Links &bull; Generate](#links-generate) 

#### 22.11.2019 | Piotr Świtlicki

* Add [Loyalty Clubs &bull; Get](#loyalty-clubs-get)
* Add info about new `client` [display schema](#offers-display-schemas) item type 

#### 22.10.2019 | Piotr Świtlicki

* Add `collection_position` order_by key to [Offers &bull; List offers](#list-offers)

#### 18.10.2019 | Piotr Świtlicki

* Add [Offer display schemas](#offers-display-schemas)

#### 16.10.2019 | Natalia Styrska

* Add ([Stores &bull; Categories](#store-categories-list)) endpoint
* Add ([Departments &bull; Create](#department-create)) endpoint
* Add ([Departments &bull; Update](#department-update)) endpoint

#### 23.09.2019 | Piotr Świtlicki

* Add `event_occurred_at` param to [Members &bull; Update](#member-event-occured-at-param) 

#### 21.09.2019 | Piotr Świtlicki

* Add `membership_started_at` to [Rewards Program &bull; Get Status](#rewards-program-status) 

#### 19.09.2019 | Natalia Styrska

* Add ([Points &bull; Member points](#member-points)) endpoint
* Add ([Points &bull; Create](#points-create)) endpoint

#### 02.09.2019 | Natalia Styrska
* Change ([Stores &bull; List](#store-list)) rename malls to departments
* Change ([Stores &bull; Get](#store-get)) rename malls to departments
* Change ([Stores &bull; Create](#store-create)) rename malls to departments
* Change ([Stores &bull; Update](#store-update)) rename malls to departments
* Add ([Departments &bull; List](#department-list)) rename malls to departments
* Add ([Departments &bull; Get](#department-get)) rename malls to departments

#### 29.08.2019 | Piotr Świtlicki
* Add [Members &bull; Update app token](#members-update-app-token) endpoint
* Add [OAuth &bull; Update app token](#me-update-app-token) endpoint

#### 27.08.2019 | Piotr Świtlicki
* Add [Offers Events](#offers-events) section

#### 23.08.2019 | Piotr Świtlicki
* Add [Members &bull; Validate](#members-validate) endpoint
* Change [Offers &bull; List](#offers-list) to accept `include_pagination_info` instead of `include_total` param

#### 19.08.2019 | Piotr Świtlicki
* Add [Offers &bull; Reprocess](#reprocess-file)
* Add [`order_by` param](#offers-list-order-by) to Offers List

#### 09.08.2019 | Piotr Świtlicki
* Add [Offers Admin](#offers-admin) section
* Add [Files &bull; Create](#create-file)
* Add [Files &bull; Update](#update-file)
* Add [Files &bull; Delete](#delete-file)

#### 08.08.2019 | Natalia Styrska
Stores

* Add `GET /v3/:loyalty_club_slug/stores` ([Stores &bull; List](#store-list))
* Add `GET /v3/:loyalty_club_slug/stores/:id` ([Stores &bull; Get](#store-get))
* Add `POST /v3/:loyalty_club_slug/stores` ([Stores &bull; Create](#store-create))
* Add `PUT /v3/:loyalty_club_slug/stores` ([Stores &bull; Update](#store-update))
* Add `DELETE /v3/:loyalty_club_slug/stores` ([Stores &bull; Delete](#store-delete))
* Add `GET /v3/:loyalty_club_slug/stores/malls` ([Malls &bull; List](#mall-list))
* Add `GET /v3/:loyalty_club_slug/stores/malls/:id` ([Malls &bull; Get](#mall-get))


#### 27.06.2019 | Piotr Świtlicki
* Introduce [Levels](#rewards-program-levels-program) 

#### 13.05.2019 | Piotr Świtlicki:
* Add `GET /v3/:loyalty_club_slug/translations` ([Translations &bull; List](#list-translations))

#### 06.05.2019 | Piotr Świtlicki:
Offers

* Add `GET /v3/:loyalty_club_slug/members/me/offers/meta` ([Offers &bull; Get offers meta](#offers-meta))
* Add `GET /v3/:loyalty_club_slug/members/me/offers/:id` ([Offers &bull; Get offer](#get-offer))
* Add `GET /v3/:loyalty_club_slug/members/me/offers` ([Offers &bull; List offers](#list-offers))
* Add `POST /v3/:loyalty_club_slug/members/me/offers/:id/use` ([Offers &bull; Use offer](#use-offer))
* Add `PUT /v3/:loyalty_club_slug/members/me/offers/:id/like` ([Offers &bull; Like offer](#like-offer))
* Add `PUT /v3/:loyalty_club_slug/members/me/offers/:id/unlike` ([Offers &bull; Unlike offer](#unlike-offer))

#### 16.04.2019 | Piotr Świtlicki:
* Add `X-Usage-Token` header to [Rewards &bull; Use](#rewards-use)

#### 07.03.2019 | Piotr Świtlicki:
* Add `GET /v3/:loyalty_club_slug/rewards-program/status/achievements_summary` ([Rewards Program &bull; Achievements summary](#rewards-program-achievements-summary))

#### 06.03.2019 | Piotr Świtlicki:
* Add file schema info ([General info&bull; Files schema](#file-schema))
* Add `GET /v3/:loyalty_club_slug/files/schema` ([Files&bull; Get schema](#get-file-schema))

#### 27.02.2019 | Jakub Kruczek:
* Extend consent definitions example ([Loyalty Clubs &bull; Get schema](#loyalty-clubs-schema))

#### 27.02.2019 | Adam Kuś:
* Add `POST /v3/:loyalty_club_slug/transactions` ([Transactions &bull; Create](#transactions-create))
* Add `GET /v3/:loyalty_club_slug/transactions` ([Transactions &bull; List](#transactions-list))
* Add `GET /v3/:loyalty_club_slug/transactions/:id` ([Transactions &bull; Get](#transactions-get))

#### 19.02.2019 | Piotr Świtlicki:
* Add `GET /v3/:loyalty_club_slug/members/:id/rewards-program/status` ([Rewards Program &bull; Get status by member ID](#rewards-program-status-by-member-id))

#### 08.02.2019 | Piotr Świtlicki:

Boostcom Rewards draft

* Add `GET /v3/:loyalty_club_slug/rewards-program/info` ([Rewards Program &bull; Get Info](#rewards-program-info))
* Add `POST /v3/:loyalty_club_slug/members/me/rewards-program/join` ([Rewards Program &bull; Join](#rewards-program-join))
* Add `DELETE /v3/:loyalty_club_slug/members/me/rewards-program/leave` ([Rewards Program &bull; Leave](#rewards-program-leave))
* Add `GET /v3/:loyalty_club_slug/members/me/rewards-program/status` ([Rewards Program &bull; Get status](#rewards-program-status))
* Add `GET /v3/:loyalty_club_slug/members/me/rewards-program/rewards` ([Rewards &bull; List](#rewards-list))
* Add `GET /v3/:loyalty_club_slug/members/me/rewards-program/rewards/purchased` ([Rewards &bull; List purchased](#rewards-list-purchased))
* Add `POST /v3/:loyalty_club_slug/members/me/rewards-program/rewards/:id/purchase` ([Rewards &bull; Purchase](#rewards-purchase))
* Add `POST /v3/:loyalty_club_slug/members/me/rewards-program/rewards/:id/use` ([Rewards &bull; Use](#rewards-use))

#### 07.02.2019 | Dominik Halat:

* Remove `shortening_enabled` param from Emails API ([Emails &bull; Create](#emails-create))

#### 07.02.2019 | Piotr Świtlicki:

* Replace paths: `/v3/loyalty_clubs/:loyalty_club_slug/` with `/v3/:loyalty_clubs/` (old ones will still work)

#### 25.01.2019 | Dominik Halat:

* Add `POST /v3/:loyalty_club_slug/emails` ([Emails &bull; Create](#emails-create))
* Add `db_and_cache` response type for Member#person_id ([Get person id](#members-person-id))

#### 14.12.2018 | Jakub Kruczek:

* Add `updated_at` to consents value on member

#### 04.12.2018 | Piotr Świtlicki:

* Add `language` param to `/v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_registration_password`

#### 13.11.2018 | Piotr Świtlicki:

* Add `PUT /v3/:loyalty_club_slug/members/channels/msisdn/:msisdn/send_verification`([Members &bull; Send MSISDN verification SMS](#members-send-verification-sms))
* Add `PUT /v3/:loyalty_club_slug/members/channels/msisdn/:msisdn/verify` ([Members &bull; Verify MSISDN](#members-verify-msisdn))

#### 08.11.2018 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/smses` ([SMSes &bull; Create](#smses-create))

#### 12.10.2018 | Piotr Świtlicki:

* Add `GET /v3/:loyalty_club_slug/members/imports/:import_id/bulks//by_request_number/:request_number`

#### 03.08.2018 | Piotr Świtlicki:

* Add [Members Import](#members-imports-import-flow)

#### 27.04.2018 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/members/by_email/:email/send_one_time_password` ([Members &bull; Send one time password E-mail](#members-send-one-time-password-email))

#### 27.04.2018 | Piotr Świtlicki:

* Add `channel` and `subchannel` attributes to `Member`
* Add `validate_partially` param in ([Members &bull; Update](members-update) and ([Me &bull; Update](#me-update)
* Add `GET /v3/:loyalty_club_slug/products` ([Loyalty Clubs &bull; List Products](#loyalty-clubs-products))
* Add `POST /v3/:loyalty_club_slug/push_notifications` ([Push Notifications &bull; Create](#push-notification-create))

#### 11.04.2018 | Dominik Halat

* Add `person_id` to `Member`
* Add new endpoint `GET /v3/:loyalty_club_slug/members/person_id` ([Members &bull; List](#members-index))

#### 01.02.2017 | Piotr Świtlicki:

* Add `ids` query param to `GET /v3/:loyalty_club_slug/members` ([Members &bull; List](#members-index))

#### 29.01.2017 | Piotr Świtlicki:

* Add `GET /v3/:loyalty_club_slug/members` ([Members &bull; List](#members-index))

#### 22.01.2017 | Jakub Kruczek:

* Update staging domain to: `https://api.mpc.dev.placewise.com/`

#### 9.01.2017 | Piotr Świtlicki:

* Update staging domain to: `https://bl-api.bl-stg.boostcom.cc/`

#### 4.01.2017 | Jakub Kruczek:

* Add `consents` fields to schema and Member payload

#### 15.12.2017 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_registration_password`
* Add information about `registration_password` in [Members &bull; Create](#members-create)
* Add some other minor fixes

#### 10.11.2017 | Piotr Świtlicki:

* Restructurize the docs
* Add `POST /v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_one_time_password`

#### 27.10.2017 | Piotr Świtlicki: 

* Rename `/check_existance` endpoints to `/public_info`
* Add :can_login attribute to `/public_info` response
* Remove :exists attribute from `/public_info` response and make it to return nil when member does not exist
