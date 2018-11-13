# Changelog

### 13.11.2018 | Piotr Świtlicki:

* Add `PUT /api/v3/loyalty_clubs/:loyalty_club_slug/members/channels/msisdn/:msisdn/send_verification`([Members &bull; Send MSISDN verification SMS](#v3-members-send-verification-sms))
* Add `PUT /api/v3/loyalty_clubs/:loyalty_club_slug/members/channels/msisdn/:msisdn/verify` ([Members &bull; Verify MSISDN](#v3-members-verify-msisdn))

### 08.11.2018 | Piotr Świtlicki:

* Add `POST /api/v3/loyalty_clubs/:loyalty_club_slug/smses` ([SMSes &bull; Create](#v3-smses-create))

### 12.10.2018 | Piotr Świtlicki:

* Add `GET /api/v3/loyalty_clubs/:loyalty_club_slug/members/imports/:import_id/bulks//by_request_number/:request_number`

### 03.08.2018 | Piotr Świtlicki:

* Add [Members Import](#v3-members-imports-import-flow)

### 27.04.2018 | Piotr Świtlicki:

* Add `POST /api/v3/loyalty_clubs/:loyalty_club_slug/members/by_email/:email/send_one_time_password` ([Members &bull; Send one time password E-mail](#v3-members-send-one-time-password-email))

### 27.04.2018 | Piotr Świtlicki:

* Add `channel` and `subchannel` attributes to `Member`
* Add `validate_partially` param in ([Members &bull; Update](v3-members-update) and ([Me &bull; Update](#v3-me-update)
* Add `GET /api/v3/loyalty_clubs/:loyalty_club_slug/products` ([Loyalty Clubs &bull; List Products](#v3-loyalty-clubs-products))
* Add `POST /api/v3/loyalty_clubs/:loyalty_club_slug/push_notifications` ([Push Notifications &bull; Create](#v3-push-notification-create))

### 11.04.2018 | Dominik Halat

* Add `person_id` to `Member`
* Add new endpoint `GET /api/v3/loyalty_clubs/:loyalty_club_slug/members/person_id` ([Members &bull; List](#v3-members-index))

### 01.02.2017 | Piotr Świtlicki:

* Add `ids` query param to `GET /api/v3/loyalty_clubs/:loyalty_club_slug/members` ([Members &bull; List](#v3-members-index))

### 29.01.2017 | Piotr Świtlicki:

* Add `GET /api/v3/loyalty_clubs/:loyalty_club_slug/members` ([Members &bull; List](#v3-members-index))

### 22.01.2017 | Jakub Kruczek:

* Update staging domain to: `https://bpc-api.dev.boostcom.no/`

### 9.01.2017 | Piotr Świtlicki:

* Update staging domain to: `https://bl-api.bl-stg.boostcom.cc/`

### 4.01.2017 | Jakub Kruczek:

* Add `consents` fields to schema and Member payload

### 15.12.2017 | Piotr Świtlicki:

* Add `POST /api/v3/loyalty_clubs/:loyalty_club_slug/members/by_msisdn/:msisdn/send_registration_password`
* Add information about `registration_password` in [Members &bull; Create](#v3-members-create)
* Add some other minor fixes

### 10.11.2017 | Piotr Świtlicki:

* Restructurize the docs
* Add `POST /api/v3/loyalty_clubs/:loyalty_club_slug/members/by_msisdn/:msisdn/send_one_time_password`

### 27.10.2017 | Piotr Świtlicki: 

* Rename `/check_existance` endpoints to `/public_info`
* Add :can_login attribute to `/public_info` response
* Remove :exists attribute from `/public_info` response and make it to return nil when member does not exist