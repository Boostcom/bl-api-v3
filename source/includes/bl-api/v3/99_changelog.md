# Changelog

### 27.02.2019 | Piotr Świtlicki:
* Add `GET /v3/infinity-mall/files/configuration` ([Loyalty Clubs &bull; Files Configuration](#v3-loyalty-clubs-files-configuration))

### 27.02.2019 | Jakub Kruczek:
* Extend consent definitions example ([Loyalty Clubs &bull; Get schema](#v3-loyalty-clubs-schema))

### 27.02.2019 | Adam Kuś:
* Add `POST /v3/infinity-mall/transactions` ([Transactions &bull; Create](#v3-transactions-create))
* Add `GET /v3/infinity-mall/transactions` ([Transactions &bull; List](#v3-transactions-list))
* Add `GET /v3/infinity-mall/transactions/:id` ([Transactions &bull; Get](#v3-transactions-get))

### 19.02.2019 | Piotr Świtlicki:
* Add `GET /v3/infinity-mall/members/:id/rewards-program/status` ([Rewards Program &bull; Get status by member ID](#v3-rewards-program-status-by-member-id))

### 08.02.2019 | Piotr Świtlicki:

Boostcom Rewards draft

* Add `GET /v3/infinity-mall/rewards-program/info` ([Rewards Program &bull; Get Info](#v3-rewards-program-info))
* Add `POST /v3/infinity-mall/members/me/rewards-program/join` ([Rewards Program &bull; Join](#v3-rewards-program-join))
* Add `DELETE /v3/infinity-mall/members/me/rewards-program/leave` ([Rewards Program &bull; Leave](#v3-rewards-program-leave))
* Add `GET /v3/infinity-mall/members/me/rewards-program/status` ([Rewards Program &bull; Get status](#v3-rewards-program-status))
* Add `GET /v3/infinity-mall/members/me/rewards-program/rewards` ([Rewards &bull; List](#v3-rewards-list))
* Add `GET /v3/infinity-mall/members/me/rewards-program/rewards/purchased` ([Rewards &bull; List purchased](#v3-rewards-list-purchased))
* Add `POST /v3/infinity-mall/members/me/rewards-program/rewards/:id/purchase` ([Rewards &bull; Purchase](#v3-rewards-purchase))
* Add `POST /v3/infinity-mall/members/me/rewards-program/rewards/:id/use` ([Rewards &bull; Use](#v3-rewards-use))

### 07.02.2019 | Dominik Halat:

* Remove `shortening_enabled` param from Emails API ([Emails &bull; Create](#v3-emails-create))

### 07.02.2019 | Piotr Świtlicki:

* Replace paths: `/v3/loyalty_clubs/:loyalty_club_slug/` with `/v3/:loyalty_clubs/` (old ones will still work)

### 25.01.2019 | Dominik Halat:

* Add `POST /v3/:loyalty_club_slug/emails` ([Emails &bull; Create](#v3-emails-create))
* Add `db_and_cache` response type for Member#person_id ([Get person id](#v3-members-person-id))

### 14.12.2018 | Jakub Kruczek:

* Add `updated_at` to consents value on member

### 04.12.2018 | Piotr Świtlicki:

* Add `language` param to `/v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_registration_password`

### 13.11.2018 | Piotr Świtlicki:

* Add `PUT /v3/:loyalty_club_slug/members/channels/msisdn/:msisdn/send_verification`([Members &bull; Send MSISDN verification SMS](#v3-members-send-verification-sms))
* Add `PUT /v3/:loyalty_club_slug/members/channels/msisdn/:msisdn/verify` ([Members &bull; Verify MSISDN](#v3-members-verify-msisdn))

### 08.11.2018 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/smses` ([SMSes &bull; Create](#v3-smses-create))

### 12.10.2018 | Piotr Świtlicki:

* Add `GET /v3/:loyalty_club_slug/members/imports/:import_id/bulks//by_request_number/:request_number`

### 03.08.2018 | Piotr Świtlicki:

* Add [Members Import](#v3-members-imports-import-flow)

### 27.04.2018 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/members/by_email/:email/send_one_time_password` ([Members &bull; Send one time password E-mail](#v3-members-send-one-time-password-email))

### 27.04.2018 | Piotr Świtlicki:

* Add `channel` and `subchannel` attributes to `Member`
* Add `validate_partially` param in ([Members &bull; Update](v3-members-update) and ([Me &bull; Update](#v3-me-update)
* Add `GET /v3/:loyalty_club_slug/products` ([Loyalty Clubs &bull; List Products](#v3-loyalty-clubs-products))
* Add `POST /v3/:loyalty_club_slug/push_notifications` ([Push Notifications &bull; Create](#v3-push-notification-create))

### 11.04.2018 | Dominik Halat

* Add `person_id` to `Member`
* Add new endpoint `GET /v3/:loyalty_club_slug/members/person_id` ([Members &bull; List](#v3-members-index))

### 01.02.2017 | Piotr Świtlicki:

* Add `ids` query param to `GET /v3/:loyalty_club_slug/members` ([Members &bull; List](#v3-members-index))

### 29.01.2017 | Piotr Świtlicki:

* Add `GET /v3/:loyalty_club_slug/members` ([Members &bull; List](#v3-members-index))

### 22.01.2017 | Jakub Kruczek:

* Update staging domain to: `https://bpc-api.dev.boostcom.no/`

### 9.01.2017 | Piotr Świtlicki:

* Update staging domain to: `https://bl-api.bl-stg.boostcom.cc/`

### 4.01.2017 | Jakub Kruczek:

* Add `consents` fields to schema and Member payload

### 15.12.2017 | Piotr Świtlicki:

* Add `POST /v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_registration_password`
* Add information about `registration_password` in [Members &bull; Create](#v3-members-create)
* Add some other minor fixes

### 10.11.2017 | Piotr Świtlicki:

* Restructurize the docs
* Add `POST /v3/:loyalty_club_slug/members/by_msisdn/:msisdn/send_one_time_password`

### 27.10.2017 | Piotr Świtlicki: 

* Rename `/check_existance` endpoints to `/public_info`
* Add :can_login attribute to `/public_info` response
* Remove :exists attribute from `/public_info` response and make it to return nil when member does not exist
