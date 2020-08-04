# Endpoints &bull; Translations

## <a name="v3-list-translations"></a> List 

> Example request

```shell
curl "https://bpc-api.boostcom.no/v3/infinity-mall/translations?language=en" \
  -H 'Content-Type: application/json' \
  -H 'X-Client-Authorization: B7t9U9tsoWsGhrv2ouUoSqpM' \
  -H 'X-Product-Name: default' \
  -H 'X-User-Agent: CURL manual test'
```

> Returns a response structured like this:

```json
{
    "member_properties": {
        "email": "E-mail",
        "gender": "Gender",
        "msisdn": "msisdn",
        "birthday": "Birthday",
        "zip_code": "Zip code",
        "interests": "Interests",
        "last_name": "Last name",
        "first_name": "First name",
        "gender.man": "Man",
        "gender.woman": "Woman",
        "bonus_points": "Bonus points",
        "interests.skincare": "Skincare",
        "interests.baby_child": "Baby/Child",
        "interests.good_offers": "Good offers",
        "interests.travel_and_leisure": "Travel and leisure",
        "interests.dietary_and_nutrition": "Dietary and nutrition",
        "interests.health_and_pharmaceuticals": "Health and pharmaceuticals"
    },
    "consents": {
        "dmp_profiling": "Would you like to receive personalised offers?",
        "dmp_profiling.title": "",
        "dmp_profiling.no_alt": "Are you sure? Ved å si nei kan mottar du ikke informasjon og tilbud i andre medier",
        "dmp_profiling.read_more": "Profiling is our way of personalising offers, info and discounts. Please see our Privacy Policy for more info on how we use data for profiling.",
        "dmp_profiling.__yes__": "Yes, I accept profiling to receive personalised exclusive offers, info and discounts",        
        "dmp_profiling.__no__": "No, I do not accept profiling to receive personalised exclusive offers, info and discounts",                
        "sms_marketing": "Do you want to receive exclusive offers via SMS?",
        "sms_marketing.title": "SMS Marketing",
        "sms_marketing.no_alt": "Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? Are you sure? ",        
        "sms_marketing.read_more": "",                
        "sms_marketing.__yes__": "Yes, I want to receive exclusive offers via SMS.",
        "sms_marketing.__no__": "No, I don't want to receive exclusive offers.",        
        "email_marketing": "Do you want to receive exclusive offers via email? ",
        "email_marketing.title": "Email Marketing",
        "email_marketing.no_alt": "Are you sure? Ved å si nei kan mottar du ikke informasjon og tilbud i andre medier",        
        "email_marketing.__yes__": "Yes",
        "email_marketing.__no__": "No, I don't want to receive exclusive offers.",        
        "email_marketing.read_more": ""
    },
    "tags": {
        "doggo": "Dog",
        "piggo": "Pig"
    },
    "terms": "<p><h3>Privacy Policy</h3>We strive to protect your privacy</p>"
}
```

**GET** `v3/:loyalty_club_slug/translations`

Returns translations for the Loyalty Club.

### Query Parameters

Parameter | Type | Default | Type
--------- | ----------- | --------- | ------
language | String | (default language configured for Loyalty Club) | Desired language of translations 

### Response (JSON object)

Key | Type | Optional? | Description 
--------- | --------- | ---------- | ---------
member_properties | Object | no (may be empty) | Translations of member properties 
consents | Object | no (may be empty) | Translations of member consents
tags | Object |  no (may be empty) | Translations of Loyalty Club tags
terms | string | yes | Translation of Loyalty Club terms 

<aside class="notice">
Requires <code>Translations:Api:Index</code> permit
</aside>
