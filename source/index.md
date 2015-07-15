---
title: Smartmenu.io - API Reference

language_tabs:
  - shell
  - php
  - ruby

toc_footers:
  - <a href='https://my.smartmenu.io/swagger/'>Test the API in Swagger </a>
  - <a href='http://restunited.com/docs/5blsoeb1bwhd'>Download API SDKs 2.0.2</a>
  - <a href='http://creativecommons.org/licenses/by-sa/4.0/'>Licensed under (CC BY-SA 4.0) </a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - products
  - errors

search: true
---

# Introduction

```
  ______ _______ _______ ______ _______ _______ _______ _______ _     _ 
 / _____|_______|_______|_____ (_______|_______|_______|_______|_)   (_)
( (____  _  _  _ _______ _____) )  _    _  _  _ _____   _     _ _     _ 
 \____ \| ||_|| |  ___  |  __  /  | |  | ||_|| |  ___) | |   | | |   | |
 _____) ) |   | | |   | | |  \ \  | |  | |   | | |_____| |   | | |___| |
(______/|_|   |_|_|   |_|_|   |_| |_|  |_|   |_|_______)_|   |_|\_____/ 
                                                                                                
```
> This pane will show example API queries and responses but you can test each one using swagger.

```
     _______.____    __    ____  ___       _______   _______  _______ .______      
    /       |\   \  /  \  /   / /   \     /  _____| /  _____||   ____||   _  \     
   |   (----` \   \/    \/   / /  ^  \   |  |  __  |  |  __  |  |__   |  |_)  |    
    \   \      \            / /  /_\  \  |  | |_ | |  | |_ | |   __|  |      /     
.----)   |      \    /\    / /  _____  \ |  |__| | |  |__| | |  |____ |  |\  \----.
|_______/        \__/  \__/ /__/     \__\ \______|  \______| |_______|| _| `._____|
```  

Welcome to the Smartmenu.io API documentation! This is the API used by the official [Smartmenu.io](https://my.smartmenu.io) web interface. 
Most of the things available on there can also be accomplished via the API.

Currently we have examples in Shell using `curl`! You can view code examples in the dark area to the right.


This documentation applies solely to the **version 2** of the API.

### Testing it out

                                                                                 


You can try out each and every request yourself! Just head to our **[Swagger](https://my.smartmenu.io/swagger/)** tool.

You'll need to be logged in to perform most requests. It's recommended that you log in to Smartmenu by using the *Keep me signed in* option. 
This way your login token will persist for 30 days. 

Each documented entity will have link to their respective counterpart in swagger so that you can quickly access it.

<aside class="warning">
Be careful when using Swagger! Most actions cannot be reversed! We suggest you create a separate account just for testing.
</aside>


### SDKs

We have SDKs in several programming languages:

 * PHP
 * Java
 * Ruby
 * Python v2
 * Android
 * C#
 * Objective-C

You can find the SDK documentation and even more sample code **[here](http://restunited.com/docs/5blsoeb1bwhd)**

The SDKs are currently in Beta and are maintained by RestUnited.

# Entry Points

> you can use both `https://my.smartmenu.io/api/v2` and `https://customers-domain.smartmenu.io/api/v2` to query the API.

The entry point for the API is:

`https://my.smartmenu.io/api/v2`

In order to achieve a full *RESTful* compliance, each **Smartmenu** business has a different subdomain that can be additionally used to query the API. 
A business with a domain name `bobsburgers` will also have the following API entry point:

`https://bobsburgers.smartmenu.io/api/v2`


# Obtaining API Keys

## Integration Hub

> Once your user inputs an `activation_code` you can retrieve the API key:

```shell
curl "https://my.smartmenu.io/api/v2/activate/device" \
  --request POST \
  --data '{"activation_code":THE-ACTIVATION-CODE-PROVIDED-BY-THE-USER}' \
  --header 'Accept-Charset: utf-8' \
  --verbose
```

``` php
// initialize the API client
$api_client = new Smartmenu\APIClient('https://my.smartmenu.io');

$activationCode = "sampleActivationCode";  // The activation code. 

try {
    $web_api = new Smartmenu\WebAPI($api_client);
    // return Token (model)
    $response = $web_api->getApiKey($activationCode);
    print_r($response);
} catch (Exception $e) {
    echo 'Caught exception: ', $e->getMessage(), "\n";
}
```

```ruby
activation_code = "sample_activation_code"  # The activation code. 

begin
  # return Token (model)
  response = Smartmenu::WebAPI.get_api_key(activation_code)
  p response
rescue Smartmenu::HTTPError => e
  puts "Failed to call API! Error: #{e}"
  puts "Response code: #{e.response.code}"
  puts "Response headers: #{e.response.headers}"
  puts "Response body: #{e.response.body}"
end
```

> The above command returns a JSON with the API Key (token):

```json
{
  "token": "smartmenu.signed.token"
}
```

To create a better experience for our users we created an [Integrations hub](https://my.smartmenu.io/mgmt/integrations/overview) 
where they can easily connect **Smartmenu** with other systems without the need for generating or mentioning the concept of API Keys to them.

This is very useful for transparently integrating devices such as tablets, terminals, cash registers and various mobile apps.
 
This would enable your users to integrate **Smartmenu** with your system by generating a temporary `activation code`.
This code can then be typed or scanned in your app so your system can retrieve the actual API Key.

Once you've retrieved the API Key you can then store it in your system and use it for subsequent requests. 
Please note that different businesses have different keys, so an API key provided by one business won't work with another business.

<aside class="notice">
Make sure you store API Keys safely in your database. Do not publish or share them with anybody.
</aside>

### Example Integration Scenario

Consider we have a business owner called **Bob**. 
Bob has just opened a burger joint down the street called "Bob's Burgers" and he uses **Smartmenu** to manage his menus.
Integrating other devices or apps would look something like this:

  1. Bob wants to connect a device such as a POS to **Smartmenu** so that he can synchronise his products on the menu.
  2. Bob proceeds to the *Integrations Hub* on **Smartmenu** to create the respective integration. 
  3. The Integration Hub then generates an activation code and prompts Bob to input the code in the respective system.
  4. Bob manually inputs the code in the app (or scans the code with the mobile device). 
  5. The app gets the API Keys with the activation token and stores it for future use.
  6. The integration is complete and Bob is informed about it in the **Smartmenu** Integrations hub.
  
We'd be more than happy to integrate and feature your app in **Smartmenu** - just drop us a line.

## Blacklisting keys

If one or more of the API Keys becomes exposed you must blacklist them in **Smartmenu**. 
Be sure to inform your users about it so that they can disable the respective Integrations in the hub.
This will mark the tokens as invalid and will disable the API access until a new integration is performed.


# Authentication

> To authorize, pass the token in the `Authorization` header:

```shell
# you must pass the correct header with each request
curl "https://bobsburgers.smartmenu.io/api/v2/status"
  -H "Authorization: smartmenu.signed.token"
```

``` php
// initialize the API client
$api_client = new Smartmenu\APIClient('https://my.smartmenu.io');
// authentication setting using api key/token
$api_client->apiKey = 'YOUR_API_KEY';
// you can now make queries with $api_client
```

```ruby
# authentication setting using api key/token
Smartmenu::Swagger.configure do |config|
  config.api_key = 'YOUR_API_KEY'
end
# you can now perform queries
```

> Make sure to replace `smartmenu.signed.token` with your user's API key.   
> Alternatively you can pass the token as a parameter:

```shell
# you should **not** pass the API Key as a URL parameter (bad!)
curl "https://bobsburgers.smartmenu.io/api/v2/status?token=smartmenu.signed.token"
# but rather pass it as data (ok!)
curl "https://bobsburgers.smartmenu.io/api/v2/status"
  -d "token=smartmenu.signed.token"
```

### Authorization Header 

You can pass the API Key with each request in the `Authorization` header. 

`Authorization: smartmenu.signed.token`

<aside class="notice">
You must replace <code>smartmenu.signed.token</code> with the API key.
</aside>

### Token URL parameter
 
Alternatively you can pass the API Key as a parameter in the `token` field.
 
Passing the API Key as a parameter is not recommended as URL parameters could be logged by servers or proxies and may become exposed. 
Consider using the `Authorization` header whenever possible. 

### JSON Web Token

The API Key is a **signed** [JSON Web Token](http://jwt.io/) which does not contain any user sensitive data in its payload.
However you can extract the token's payload to determine the user's `domain`. 

<aside class="notice">
Each Smartmenu business (user) has a different API key. Make sure you use the corresponding key when making requests. 
</aside>


# Making Requests

As per RESTful design patterns, the **Smartmenu** API implements the following HTTP methods:

* **`GET`** - Read resources
* **`POST`** - Create new resources
* **`PUT`** - Modify existing resources
* **`DELETE`** - Remove resources
* **`HEAD`** - Perform a HEAD request 

When making requests, arguments can be passed as params, form data or JSON with correct `Content-Type` header.

Each response is `application/json` and it's encoded in `UTF-8`

## HTTPS 

> A happy request is a valid `HTTPS` request.

All requests must be made over `HTTPS`. Any `HTTP` requests are met with a HTTP `302 Found` to its HTTPS equivalent.

## Languages

> This will get the english version of a product:

```shell
curl /api/v1/products/33 \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept-Language: en' \
  --verbose
```

> This will update the **french** *name* and subtitle of a product. Every other field will remain unchanged:

```shell
curl /api/v1/products/25 \
  --request PUT \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept-Language: fr' \
  --data '{"subtitle":"Tomates multicolores, sélection du chef","name":"DECLINAISON DE TOMATES"}' \
  --verbose
```

> Some entities such as products have `i18n` fields:

```json
  {
    "id": 43,
    "name": "Chocolate Cream",
    "i18n_name": {
      "en": "Chocolate Cream",
      "de": "Schokocreme"
    },
    "price": "5.0"
  }
  ```


Some businesses manage their menus in more than one language. When making requests to create or update products, categories, and menus 
the default language set by the business owner is used, so there's no need to adjust anything. 

### Setting the language

If However you want to edit resources in a different language you can pass the `Accept-Language` header or `lang=xx` as a parameter.
Smartmenu uses two-letter [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) codes for each language. Some examples include 
`en` for English, `de` for German, `fr` for French, `es` for Spanish and `it` for Italian.

### Using the `lang` parameter

The `lang` parameter takes precedence over the `Accept-Language` so if both of them are passed the `lang` parameter will be applied
and the header value will be ignored. 

### Locale and country

Language codes with region such as `en-US` or `de-DE` are stripped down to their language code. Making a request with 
`Accept-Language: en-GB` will be treated as `en`. 

### Entities using more than one language

Each multilingual field is marked as such in this documentation. 

* `Product`: name, subtitle, description, information 
* `Menu`: name, description
* `Category`: name, description

### i18n hashes

In addition to the normal response most entities contain `i18n` fields encompassing all available languages. 

This is useful if you want to display multilingual content in your app. 
This way for example you don't need to retrieve the products for each language - you can simply switch languages on the fly.

## ETags and caching

```shell
curl /api/v1/products/23 \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'ETag: 000b19131cf99d00ec98fe0065a81fc6' \
  --verbose
```

> This request will result in an empty `304 Not Modified` response if the product has not changed or a `200 OK` response with the product.  


Each response contains an `ETag` header. It is highly recommended to pass a `If-None-Match` header with your request
so as to save resources and bandwidth. After all, faster response times means better user experience.

Apart from performing `conditional GET`s you can also perform `HEAD` requests to check for any changes.
 
ETags are different for each language and take into account the usage of the `Accept-Language` header or `lang` parameters. 
Proxies and caches should take the `Vary` header into account which also lists `Accept-Language`.

Requests with `If-Match` are currently not supported.
  
To learn more about ETags and conditional GETs please visit the [Wikipedia Article](https://en.wikipedia.org/wiki/HTTP_ETag#Typical_usage)

## Rate-Limits and throttling 
 
 > When exceeding the rate limit you'll get a `429` Response:
 
 ```json
 {
   "error": "TooManyRequestsError",
   "status_code": 429,
   "message": "You have sent too many requests. Please enhance your calm."
 }
 ```
 
There is a rate limit of **1000 requests per hour** for each API key after which every request will be met with a `429 Too Many Requests` response.
The rate limit is reset every 60 minutes. There are certain headers with each request that can help you track your limit:
 
 header | Description
 --------- | ------- 
 `X-RateLimit-Limit` | The total number of requests allowed in 60 minutes.
 `X-RateLimit-Remaining` | The number of remaining requests for the next 60 minutes 

 
## Fields

> This will get only a nice list of all products instead of a full-blown JSON

```shell
curl "http://my.smartmenu.io/api/v2/products/list"
  -H "Authorization: smartmenu.signed.token"
```

```json
[
  {
    "id": 55,
    "name": "Kartofelsuppe",
    "i18n_name": {
      "de": "Kartofelsuppe"
    },
    "price": "3.5"
  },
  {
    "id": 31,
    "name": "Spargelsuppe",
    "i18n_name": {
      "de": "Spargelsuppe"
    },
    "price": "4.5"
  }
]
```

### IDs and references

Every `ID` for an object is a unique, positive integer with a `bigint` range.    
That means that no two businesses can share the same product IDs or menus IDs; these values and references are unique to each business.

### Timestamps

All timestamp are returned in `ISO8601` format in `UTC` with fields beginning with `t_`. 

Example: `"t_created": "2015-05-12T01:04:38Z"`

### URLs

Every URL includes an absolute path beginning with `http://` or `https://`. There are no relative paths.
 
Example: `"image_big_url": "https://media.smartmenu.io/images/some_image.jpg"`

### Lists

Most of the entities have a compact list view that is smaller and very useful when displaying lists.
Consider using those when caching is not available.

Examples:

  * `GET` on `api/v2/products/list`
  * `GET` on `api/v2/categories/list`

### Prices

**Prices are handled as an Integer in Cents.**  
This is because each language uses different precision and data types to store floating point numbers (BigDecimal, Numeric, double, float) and floats and doubles cannot accurately represent a currency.
You'll need to parse the price into an appropriate datatype for you. 



