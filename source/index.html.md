---
title: API Reference
toc_footers:
  - <a href='https://uaccount.asia/'>Documentation Powered by <br> uAccount </a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the uAccount API! You can use our API to access Wi Eco-System API endpoints.
We developed our system using RESTful APIs that we use OAuth 2.0 for authorization.
All request and response bodies are formatted in JSON.

# Authentication & Authorization

To Authenticate you have to use uAccount API.
The uAccount REST API uses the OAuth 2.0 protocol to authorize calls. OAuth is an open standard that many companies use to provide secure access to protected resources.

When you create an app,
<code>
  uAccount generates a set of **OAuth client ID** and **secret Key** for your app.
</code>

Now, you have to hit in uAccount <code>/oauth/authorize</code> with some specific keys for Authorization.
<aside class="warning">
You must replace <code>client_id</code> <code>redirect_uri</code> <code>device_id.</code>
</aside>
<aside class="notice">
You must have to generate unique **<code>device_id</code>** for every login session and have to keep in session for further request.

**<code>device_id</code>** at least 64 character long and must contain **numbers** and **letters** only.
</aside>

### Authorization
Example in **PHP**

<div class="center-column"></div>
```php
<?php
use GuzzleHttp;

$query = http_build_query([
'client_id' => 'YOUR_CLIENT_ID',
'redirect_uri' => 'YOUR_CALLBACK_URI',
'response_type' => 'code',
'device_id' => 'UNIQUE_DEVICE_ID',
'device_info' => $_SERVER['HTTP_USER_AGENT'],
'scope' => '',
]);

return redirect('http://account.witoken.global/oauth/authorize?' . $query);
?>
```
You will get an **<code>authorization_code</code>** for the next login request.

###Authentication
After Authorization you have to send login request for your user.

<div class="center-column"></div>
```php
<?php
$http = new GuzzleHttp\Client;

$response = $http->post('http://account.witoken.global/oauth/token', [
'form_params' => [
    'grant_type' => 'authorization_code',
    'client_id' => 'YOUR_CLIENT_ID',
    'client_secret' => 'YOUR_CLIENT_SECRET',
    'redirect_uri' => 'YOUR_REDIRECT_URL',
    'code' => 'AUTHORIZATION_CODE',
    'device_id' => 'USER_DEVICE_ID',
    'device_info' => $_SERVER['HTTP_USER_AGENT'],
],
]);
?>
```
You will get **<code>Access_Token</code>** for further request.


###Authentication(for mobile)
<aside class="notice">
  You Don't need any authorization code for this step.
</aside>

For Mobile App Development any Authorization code is not needed. So send direct request to API with user credentials.

<aside class="notice">
Method : **POST**
</aside>

<div class="center-column"></div>
```javascript
fetch('https://account.witoken.global/oauth/token', {
			method: 'POST',
			headers: {
				'Accept': 'application/json',
				'Content-Type': 'application/json',
			},
			body: JSON.stringify( {
				grant_type: 'password',
				client_id: 'YOUR_CLIENT_ID',
				client_secret: 'YOUR_CLIENT_SECRET',
				username: 'USER_EMAIL',
				password: 'USER_PASSWORD',
				device_id: 'USER_DEVICE_ID',
				device_info: 'USER_DEVICE_INFORMATION',
				scope: '',
			})
```

# Public API

## User Register
<aside class="notice">
**Method : POST**
</aside>

<div class="center-column"></div>
```ruby
/register
```

Parameter | Required | Description
--------- | ------- | -----------
device_id | Yes | user specific device_id.
client_id | Yes | client_id which is assigned to that specific app.
first_name | Yes | User first Name
last_name | Yes | User last Name
date_of_birth | No | User date of birth
username | Yes | user name
email | Yes | user email
password | Yes | user password (should be 6+ character)
password_confirmation | Yes | user password to confirm
avatar | No | User Avatar
nid | No | User National ID card
mobile_no | Yes | User Mobile No
mobile_code | Yes | Mobile Country Code
country_id | Yes | Country ID (From our Country URL)
zip_code | No | zip code
gender | No | User Gender
address | No | Address

## Seller Register
<aside class="notice">
**Method : POST**
</aside>

<aside class="warning">
<strong>Required Fields</strong> may vary depends of Seller Application Selection from our eco-system.
</aside>

<div class="center-column"></div>
```ruby
/seller/register
```

Parameter | Required | Description
--------- | ------- | -----------
device_id | Yes | user specific device_id.
client_id | Yes | client_id which is assigned to that specific app.
first_name | Yes | User first Name
last_name | Yes | User last Name
date_of_birth | No | User date of birth
username | Yes | user name
email | Yes | user email
password | Yes | user password (should be 6+ character)
password_confirmation | Yes | user password to confirm
avatar | No | User Avatar (Image)
nid | No | User National ID card (Image)
mobile_no | Yes | User Mobile No
mobile_code | Yes | Mobile Country Code
country_id | Yes | Country ID (From our Country URL)
zip_code | No | zip code
gender | No | User Gender
address | No | Address
business_licence | N/A | seller Business Licence Image
shop_title | N/A | Seller Shop title
welcome_msg | N/A | Shop welcome message
shop_address | N/A | Shop address
shop_zip | N/A | Shop Zip Code
shop_mobile | N/A | Shop Mobile
shop_mobile_code | N/A | Mobile Country code
vat | N/A | vat
logo | N/A | Image of shop (Image)
header_img | N/A | Cover Image of Shop (Image)
business_country_id | N/A | Business country ID ( From our country URL)
bank_country_id | N/A | Seller Bank ID (From our Selected Bank Lists)
currency | N/A | Seller Business Currency
beneficiary_name | N/A | Bank beneficiary name
branch_id | N/A | Bank Branch ID (From our branch URL)
account_no | N/A | Bank account No.


## Get All Countries

Get all countries from our table
<aside class="success">
**Method : GET**
</aside>

<div class="center-column"></div>
```ruby
countries
```

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
countries: [
    {
        country_id: 4,
        country_name: "Afghanistan",
        capital: "Kabul",
        citizenship: "Afghan",
        country_code: "4",
        currency: "afghani",
        currency_code: "AFN",
        currency_sub_unit: "pul",
        full_name: "Islamic Republic of Afghanistan",
        short_name: "AF",
        iso_3166_3: "AFG",
        region_code: "142",
        sub_region_code: "34",
        eea: "False",
        country_mobile_code: "93",
        currency_symbol: "؋",
        currency_decimals: 2,
        flag: "AF.png"
    }
]
```

## Get Specific Country Information

Get all countries from our table
<aside class="notice">
**Method : POST**
</aside>

<div class="center-column"></div>
```ruby
countries/:{id}
```

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
{
    country_id: 4,
    country_name: "Afghanistan",
    capital: "Kabul",
    citizenship: "Afghan",
    country_code: "4",
    currency: "afghani",
    currency_code: "AFN",
    currency_sub_unit: "pul",
    full_name: "Islamic Republic of Afghanistan",
    short_name: "AF",
    iso_3166_3: "AFG",
    region_code: "142",
    sub_region_code: "34",
    eea: "False",
    country_mobile_code: "93",
    currency_symbol: "؋",
    currency_decimals: 2,
    flag: "AF.png"
}
```
## Get Specific Country's Bank Information

Get all Banks from Specific Country.
<aside class="success">
**Method : GET**
</aside>

<div class="center-column"></div>
```ruby
country/:{id}/banks
```

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
[
    {
        "bank_id": 1,
        "name": "Bangladesh Bank",
        "country_id": 50,
        "swift_code": "BANGLASWIFTCODE"
    }
]
```

## Get Specific Bank's Branches

Get all Branches's from Specific Bank.
<aside class="success">
**Method : GET**
</aside>

<div class="center-column"></div>
```ruby
bank/{id}/branches
```

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
[
    {
        "branch_id": 1,
        "branch_name": "BD BANK Branch 1",
        "bank_id": 1,
        "address": "Some address",
        "zip_code": 1230,
        "swift_code": "BDBRANCH1",
        "country_id": 50,
        "country": {
            "country_id": 50,
            "country_name": "Bangladesh",
            "capital": "Dhaka",
            "citizenship": "Bangladeshi",
            "country_code": "50",
            "currency": "taka (inv.)",
            "currency_code": "BDT",
            "currency_sub_unit": "poisha (inv.)",
            "full_name": "People’s Republic of Bangladesh",
            "short_name": "BD",
            "iso_3166_3": "BGD",
            "region_code": "142",
            "sub_region_code": "34",
            "eea": "False",
            "country_mobile_code": "880",
            "currency_symbol": "BDT",
            "currency_decimals": 2,
            "flag": "BD.png"
        }
    }
]
```

## Get all Banks

Get all Banks.
<aside class="success">
**Method : GET**
</aside>

<div class="center-column"></div>
```ruby
/banks
```

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
{
    "banks": [
        {
            "bank_id": 1,
            "name": "Bangladesh Bank",
            "country_id": 50,
            "swift_code": "BANGLASWIFTCODE"
        }
    ]
}
```

## Get User Status
Check Any User is active logged in or not.
<aside class="notice">
**Method : POST**
</aside>

<div class="center-column"></div>
```ruby
/user/status
```

Parameter | Type | Description
--------- | ------- | -----------
device_id | false | user specific device_id.
client_id | false | client_id which is assigned to that specific app.

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
{
    "status": "active",
    "user_id": 16
}
```


#API With Authentication

## Get User Details
<aside class="notice">
Method : POST
</aside>

<div class="center-column"></div>
```ruby
/details
```

**Headers**

Key | Value
--------- | -------
Accept | 'application/json'
Content-type | 'application/json'
Authorization | 'Bearer 'USER_ACCESS_TOKEN

**Parameters**

Parameter | Type | Description
--------- | ------- | -----------
device_id | false | user specific device_id.
client_id | false | client_id which is assigned to that specific app.

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
{
    "user_id": 32,
    "first_name": "admin",
    "last_name": "vai",
    "date_of_birth": "2000-12-12",
    "email": "admin@admin.com",
    "avatar": "http://account.witoken.global/storage/users/images/dgZXn2WmbTCQGO5qmCyNHZv8mdTMn5I6sDSqK5xy.png",
    "mobile_code": 65,
    "mobile_no": 1234567890,
    "zip_code": 1212,
    "address": "50, DOHS, Mirpur",
    "country": {
        "country_id": 50,
        "country_name": "Bangladesh",
    },
    "gender": "male",
    "nid": "http://account.witoken.global/storage/users/images/4EYgq37ms1VodqCTxTdmulB6PQqkth2zOISUDHtB.png",
    "nid_verified": 'Unverified/Verified/Pending/Rejected',
    "status": 1,
    "is_admin": 1,
    "seller_status": null,
    "seller_info": null,
    "bank_info": [],
    "approved_app": [],
    "qrcode_img": "http://account.witoken.global/storage/qrcodes/5d72489c09e2c.png",
}
```

## Update User Details
<aside class="notice">
Method : POST
</aside>
<aside class="warning">
You must have to pass current password of the user.
But user cannot change current Password and Email from any application.
<b>Only from uAccount any user can change password. </b>
</aside>
<div class="center-column"></div>
```ruby
/users
```

**Headers**

Key | Value
--------- | -------
Accept | 'application/json'
Content-type | 'application/json'
Authorization | 'Bearer 'USER_ACCESS_TOKEN

**Parameters**

Parameter | Type | Description
--------- | ------- | -----------
device_id | false | user specific device_id.
client_id | false | client_id which is assigned to that specific app.
....      |...    | All your desired parameters will be added.

## User Addresses
<aside class="notice">
Method : POST
</aside>

<div class="center-column"></div>
```ruby
/user/addresses
```

**Headers**

Key | Value
--------- | -------
Accept | 'application/json'
Content-type | 'application/json'
Authorization | 'Bearer 'USER_ACCESS_TOKEN

**Parameters**

Parameter | Type | Description
--------- | ------- | -----------
device_id | false | user specific device_id.
client_id | false | client_id which is assigned to that specific app.

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
[
    {
        "id": 4,
        "contact": "admin vai",
        "user_id": 32,
        "country_id": 50,
        "country_name": "Bangladesh",
        "zip_code": 1212,
        "address": "50, DOHS, Mirpur",
        "mobile_no": 1234567890,
        "email": null,
        "is_primary": 1,
        "mobile_code": 65,
        "title": ""
    }
]
```

## Generate OTP
Generate OTP and send to client
<aside class="notice">
**Method : GET**
</aside>

<div class="center-column"></div>
```ruby
/otp/generate
```

**Headers**

Key | Value
--------- | -------
Accept | 'application/json'
Content-type | 'application/json'
Authorization | 'Bearer 'USER_ACCESS_TOKEN

**Parameters**

Parameter | Type | Description
--------- | ------- | -----------
type      | string | OTP type "message"/ "email"
device_id | false | user specific device_id.
client_id | false | client_id which is assigned to that specific app.

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
{

    "success": true,
    "message"  : OTP successfully send.
}
```

## Get OTP List
Get platform based OTP Sending list
<aside class="success">
**Method : GET**
</aside>

<div class="center-column"></div>
```ruby
/otp/get-list
```

**Headers**

Key | Value
--------- | -------
Accept | 'application/json'
Content-type | 'application/json'
Authorization | 'Bearer 'USER_ACCESS_TOKEN

**Parameters**

Parameter | Type | Description
--------- | ------- | -----------
device_id | false | user specific device_id.
client_id | false | client_id which is assigned to that specific app.

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
{

    "message": true,
    "email"  : false
}
```

## Check OTP
Check OTP
<aside class="notice">
**Method : GET**
</aside>

<div class="center-column"></div>
```ruby
/otp/check
```

**Headers**

Key | Value
--------- | -------
Accept | 'application/json'
Content-type | 'application/json'
Authorization | 'Bearer 'USER_ACCESS_TOKEN

**Parameters**

Parameter | Type | Description
--------- | ------- | -----------
otp_key   | numeric | User given OTP key
device_id | false | user specific device_id.
client_id | false | client_id which is assigned to that specific app.

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
{

    "success": true,
    "message"  : Successful
}
```


## User Logout
<aside class="notice">
Method : POST
</aside>

<aside class="warning">
  You must have to hit <code>/logout</code> before make logged out from your native app.
</aside>

<div class="center-column"></div>
```ruby
/logout
```

**Headers**

Key | Value
--------- | -------
Accept | 'application/json'
Content-type | 'application/json'
Authorization | 'Bearer 'USER_ACCESS_TOKEN

**Parameters**

Parameter | Type | Description
--------- | ------- | -----------
device_id | false | user specific device_id.
client_id | false | client_id which is assigned to that specific app.
user_id   | false | user id.

The above command returns JSON structured like this:

<div class="center-column"></div>
```json
{
    "status": "success",
    "message": "Request completed successfully"
}
```
