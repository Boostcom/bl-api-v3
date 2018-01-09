# Changelog

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
