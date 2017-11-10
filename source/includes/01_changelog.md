# Changelog

### 27.10.2017: 

* Rename `/check_exsistance` endpoints to `/public_info`
* Add :can_login attribute to `/public_info` response
* Remove :exists attribute from `/public_info` response and make it to return nil when member does not exist
* Add `PUT /api/v3/loyalty_clubs/:loyalty_club_slug/members/emails/:email/verify`
* Add `PUT /api/v3/loyalty_clubs/:loyalty_club_slug/members/by_email/:email/unsubscribe`
