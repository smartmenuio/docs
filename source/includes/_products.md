
# Products
  
A product is the core element in the menu - it represents a dish, drink or maybe a bottle of wine. Products can be grouped into `categories` and `menus` or they can exist standalone. Products are generally referenced by their ID, however there's also a `product_code` available which can be used to synchronise and modify the product by an external service, such as a POS or a delivery service. Each `product` can further be tagged with `tags` describing it, such as *vegetarian* or *spicy*. Similarly `allergies` are also available.

**Prices are handled as an Integer in cents.**  
This is because each language uses different precision and data types to store floating point numbers (BigDecimal, Numeric, double, float) and floats and doubles cannot accurately represent a currency.
You'll need to parse the price into an appropriate datatype for you. 
 
      
## Get a short list of all the Products 

```shell
curl https://my.smartmenu.io/api/v2/products/list \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept: application/json; charset=utf-8' \
  --verbose
```

``` php
try {
    $web_api = new Smartmenu\WebAPI($api_client);
    // return Array of ProductList (model)
    $response = $web_api->getProductsList();
    print_r($response);
} catch (Exception $e) {
    echo 'Caught exception: ', $e->getMessage(), "\n";
}
```

```ruby
begin
  # return Array of ProductList (model)
  response = Smartmenu::WebAPI.get_products_list()
  p response
rescue Smartmenu::HTTPError => e
  puts "Failed to call API! Error: #{e}"
  puts "Response code: #{e.response.code}"
  puts "Response headers: #{e.response.headers}"
  puts "Response body: #{e.response.body}"
end
```


> The above command returns JSON structured like this:

```json
[
  {
    "id": 23,
    "name": "German Potato Soup",
    "i18n_name": {
      "en": "German Potato Soup",
      "de": "Kartoffelsuppe"
      },
    "price": 550,
    "allergy_codes": "A, C, G"
  }
]
```

Get a nice list representation of all the products for the current business. Consider using this query when showing the list of products.


### Try it out in Swagger:

[`GET https://my.smartmenu.io/api/v2/products/list`](https://my.smartmenu.io/swagger/#!/products/getProductsList)

### Code examples:

[`GET https://my.smartmenu.io/api/v2/products/list`](http://restunited.com/docs/5blsoeb1bwhd#httpmethod_Web_getProductsList)


## Get an extended list of all the Products 

```shell
curl https://my.smartmenu.io/api/v2/products/list \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept: application/json; charset=utf-8' \
  --verbose
```

``` php
try {
    $web_api = new Smartmenu\WebAPI($api_client);
    // return Array of Product (model)
    $response = $web_api->getProducts();
    print_r($response);
} catch (Exception $e) {
    echo 'Caught exception: ', $e->getMessage(), "\n";
}
```

```ruby
begin
  # return Array of Product (model)
  response = Smartmenu::WebAPI.get_products()
  p response
rescue Smartmenu::HTTPError => e
  puts "Failed to call API! Error: #{e}"
  puts "Response code: #{e.response.code}"
  puts "Response headers: #{e.response.headers}"
  puts "Response body: #{e.response.body}"
end
```


> The above command returns JSON structured like this:

```json
[
  {
    "id": 71,
    "name": "German Potato Soup",
    "description": "Some more info and description.",
    "subtitle": "With fresh basil leaves and a pinch of curry.",
    "information": "Containing the best natural ingredients this soup will make your arms look like diamonds!",
    "i18n_name": {
      "en": "German Potato Soup"
    },
    "i18n_description": {
      "en": "Some more info and description."
    },
    "i18n_subtitle": {
      "en": "With fresh basil leaves and a pinch of curry."
    },
    "i18n_information": {
      "en": "Containing the best natural ingredients this soup will make your arms look like diamonds!"
    },
    "t_created": "2015-07-15T16:06:26Z",
    "t_updated": "2015-07-15T16:06:26Z",
    "product_code": "P90X3",
    "price": 550,
    "image_big_url": "https://media.smartmenu.io/images/todorsburgers_product_image_big_default.jpg",
    "image_tile_url": "https://media.smartmenu.io/images/todorsburgers_product_image_tile_default.jpg",
    "image_info_url": "https://media.smartmenu.io/images/todorsburgers_product_image_info_default.jpg",
    "tags": [
      {
        "id": 2,
        "name": "Spicy",
        "description": "Spicy dish",
        "thumbnail_url": null,
        "image_url": null
      }
    ],
    "allergies": [
      {
        "id": 2,
        "name": "Sesame seed",
        "description": "Sesame seed",
        "thumbnail_url": null,
        "image_url": null,
        "code": "N"
      }
    ],
    "allergy_codes": "N"
  }
]
```

Get a full representation of **all** the products for the current business. **Use sparsely**. 
Although this entity is always cached on our side this representation will cost you greater loading and processing times.  


### Try it out in Swagger:

[`GET https://my.smartmenu.io/api/v2/products`](https://my.smartmenu.io/swagger/#!/products/getProducts)

### Code examples:

[`GET https://my.smartmenu.io/api/v2/products`](http://restunited.com/docs/5blsoeb1bwhd#httpmethod_Web_getProducts)


      
## Create a new Product 

> Create a new product with a price, name and description:

```shell
curl /api/v2/products \
  --request POST \
  --data '{"description":"Popeye edition.","name":"German Spinach Soup","price":950}' \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept: application/json; charset=utf-8' \
  --verbose
```

```php
// initialize the API client
$api_client = new Smartmenu\APIClient('https://my.smartmenu.io');
// authentication setting using api key/token
$api_client->apiKey = 'YOUR_API_KEY';

$name = "German Spinach Soup";  // A name for this product. Max. 254 characters. 
$price = 950;  // The price in cents. 
$description = "Popeye edition.";  // A detailed description. Max 1000 characters. 

try {
    $web_api = new Smartmenu\WebAPI($api_client);
    // return Product (model)
    $response = $web_api->addProduct($name, $price, $description);
    print_r($response);
} catch (Exception $e) {
    echo 'Caught exception: ', $e->getMessage(), "\n";
}
```

```ruby
name = "German Spinach Soup"  # A name for this product. Max. 254 characters. 
price = 950  # The price in cents. 

begin
  # return Product (model)
  response = Smartmenu::WebAPI.add_product(name, price, :description => "Popeye edition.")
  p response
rescue Smartmenu::HTTPError => e
  puts "Failed to call API! Error: #{e}"
  puts "Response code: #{e.response.code}"
  puts "Response headers: #{e.response.headers}"
  puts "Response body: #{e.response.body}"
end
```

> This will create the product:

```json
{
    "id": 71,
    "name": "German Spinach Soup",
    "description": "Popeye edition.",
    "subtitle": "",
    "information": "",
    "i18n_name": {
      "en": "German Spinach Soup"
    },
    "i18n_description": {
      "en": "Popeye edition."
    },
    "i18n_subtitle": {
      "en": ""
    },
    "i18n_information": {
      "en": ""
    },
    "t_created": "2015-07-15T16:06:26Z",
    "t_updated": "2015-07-15T16:06:26Z",
    "product_code": "",
    "price": 950,
    "image_big_url": "https://media.smartmenu.io/images/todorsburgers_product_image_big_default.jpg",
    "image_tile_url": "https://media.smartmenu.io/images/todorsburgers_product_image_tile_default.jpg",
    "image_info_url": "https://media.smartmenu.io/images/todorsburgers_product_image_info_default.jpg",
    "tags": [],
    "allergies": [],
    "allergy_codes": ""
}
```
Create a new product for the current business.

### Try it out in Swagger:

[`POST https://my.smartmenu.io/api/v2/products`](https://my.smartmenu.io/swagger/#!/products/addProduct)

### Code examples:

[`POST https://my.smartmenu.io/api/v2/products/`](http://restunited.com/docs/5blsoeb1bwhd#httpmethod_Web_addProduct)


### URL Parameters

| Required | Name | Type | Description |
|:--------:|:-----|:-----|:------------|
| * | name |  String | A name for this product. Max. 254 characters. |
| * | price |  Integer | The price in cents. |
|  | allergy_ids |  String | The IDs of the Allergies this product has been tagged with (comma separated). |
|  | category_ids |  String | The IDs of the categories this product belongs to (comma separated). |
|  | description |  String | A detailed description. Max 1000 characters. |
|  | information |  String | More information about the product such as nutrition information or origin, max. 1000 characters. |
|  | menu_ids |  String | The IDs of the menus this product/category belongs to (comma separated). |
|  | product_code |  String | The product code. Max 254 characters. |
|  | subtitle |  String | A nice subtitle for this product, max. 254 characters. |
|  | tag_ids |  String | The IDs of the Tags this product has been tagged with (comma separated). |
|  | visible |  Boolean | Controls the visibility |

         
         
         
      
## Get a Product by its ID

```shell
curl /api/v2/products/71 \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept: application/json; charset=utf-8' \
  --verbose
```

```php
$id = 71;  // The ID. 

try {
    $web_api = new Smartmenu\WebAPI($api_client);
    // return Product (model)
    $response = $web_api->getProduct($id);
    print_r($response);
} catch (Exception $e) {
    echo 'Caught exception: ', $e->getMessage(), "\n";
}
```

```ruby
id = 71 # The ID. 

begin
  # return Product (model)
  response = Smartmenu::WebAPI.get_product(id)
  p response
rescue Smartmenu::HTTPError => e
  puts "Failed to call API! Error: #{e}"
  puts "Response code: #{e.response.code}"
  puts "Response headers: #{e.response.headers}"
  puts "Response body: #{e.response.body}"
end
```


> The above command returns JSON structured like this:

```json
{
    "id": 71,
    "name": "German Potato Soup",
    "description": "Some more info and description.",
    "subtitle": "With fresh basil leaves and a pinch of curry.",
    "information": "Containing the best natural ingredients this soup will make your arms look like diamonds!",
    "i18n_name": {
      "en": "German Potato Soup"
    },
    [...]
    "allergy_codes": "N"
}
```
Get a product for the current business.



### Try it out in Swagger:

[`GET https://my.smartmenu.io/api/v2/products/:id`](https://my.smartmenu.io/swagger/#!/products/getProduct)

### Code examples:

[`GET https://my.smartmenu.io/api/v2/products/:id`](http://restunited.com/docs/5blsoeb1bwhd#httpmethod_Web_getProduct)

### URL Parameters

| Required | Name | Type | Description |
|:--------:|:-----|:-----|:------------|
| * | id |  Integer | The ID. |

      
## Update a Product by its ID

> Change just the name, description and price of a product:

```shell
curl /api/v2/products/71 \
  --request PUT \
  --data '{"description":"Corrected info and description.","name":"Another German Potato Soup","price":950}' \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept: application/json; charset=utf-8' \
  --verbose
```

```php
$id = @@26;  // The ID. 
$name = "Another German Potato Soup";  // A name for this product. Max. 254 characters. 
$price = 950;  // The price in cents. 
$description = "Corrected info and description.";  // A detailed description. Max 1000 characters. 

try {
    $web_api = new Smartmenu\WebAPI($api_client);
    // return Product (model)
    $response = $web_api->updateProduct($id, $name, $price, $description);
    print_r($response);
} catch (Exception $e) {
    echo 'Caught exception: ', $e->getMessage(), "\n";
}
```

```ruby
id = 71  # The ID. 

begin
  # return Product (model)
  response = Smartmenu::WebAPI.update_product(id, :name => "Another German Potato Soup", :price => 950, :description => "Corrected info and description.")
  p response
rescue Smartmenu::HTTPError => e
  puts "Failed to call API! Error: #{e}"
  puts "Response code: #{e.response.code}"
  puts "Response headers: #{e.response.headers}"
  puts "Response body: #{e.response.body}"
end
```

> This will only change the name, price and description:

```json
{
    "id": 71,
    "name": "Another German Potato Soup",
    "description": "Corrected info and description.",
    "subtitle": "With fresh basil leaves and a pinch of curry.",
    "information": "Containing the best natural ingredients this soup will make your arms look like diamonds!",
    "i18n_name": {
      "en": "Another German Potato Soup"
    },
    "i18n_description": {
      "en": "Corrected info and description."
    },
    "i18n_subtitle": {
      "en": "With fresh basil leaves and a pinch of curry."
    },
    "i18n_information": {
      "en": "Containing the best natural ingredients this soup will make your arms look like diamonds!"
    },
    "t_created": "2015-07-15T16:06:26Z",
    "t_updated": "2015-07-15T16:06:26Z",
    "product_code": "P90X3",
    "price": 950,
    "image_big_url": "https://media.smartmenu.io/images/todorsburgers_product_image_big_default.jpg",
    "image_tile_url": "https://media.smartmenu.io/images/todorsburgers_product_image_tile_default.jpg",
    "image_info_url": "https://media.smartmenu.io/images/todorsburgers_product_image_info_default.jpg",
    "tags": [],
    "allergies": [],
    "allergy_codes": ""
}
```
Change a product for the current business.

### Try it out in Swagger:

[`PUT https://my.smartmenu.io/api/v2/products/:id`](https://my.smartmenu.io/swagger/#!/products/updateProduct)

### Code examples:

[`PUT https://my.smartmenu.io/api/v2/products/:id`](http://restunited.com/docs/5blsoeb1bwhd#httpmethod_Web_updateProduct)


### URL Parameters

| Required | Name | Type | Description |
|:--------:|:-----|:-----|:------------|
| * | id |  Integer | The ID. |
|  | allergy_ids |  String | The IDs of the Allergies this product has been tagged with (comma separated). |
|  | category_ids |  String | The IDs of the categories this product belongs to (comma separated). |
|  | description |  String | A detailed description. Max 1000 characters. |
|  | information |  String | More information about the product such as nutrition information or origin, max. 1000 characters. |
|  | menu_ids |  String | The IDs of the menus this product/category belongs to (comma separated). |
|  | name |  String | A name for this product. Max. 254 characters. |
|  | price |  Integer | The price in cents. |
|  | product_code |  String | The product code. Max 254 characters. |
|  | subtitle |  String | A nice subtitle for this product, max. 254 characters. |
|  | tag_ids |  String | The IDs of the Tags this product has been tagged with (comma separated). |
|  | visible |  Boolean | Controls the visibility |





      
## Delete a Product


```shell
curl /api/v2/products/34 \
  --request DELETE \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept: application/json; charset=utf-8' \
  --verbose
```

```php
$id = 34;  // The ID. 

try {
    $web_api = new Smartmenu\WebAPI($api_client);
    // return Success (model)
    $response = $web_api->deleteProduct($id);
    print_r($response);
} catch (Exception $e) {
    echo 'Caught exception: ', $e->getMessage(), "\n";
}
```

```ruby
id = 34  # The ID. 

begin
  # return Success (model)
  response = Smartmenu::WebAPI.delete_product(id)
  p response
rescue Smartmenu::HTTPError => e
  puts "Failed to call API! Error: #{e}"
  puts "Response code: #{e.response.code}"
  puts "Response headers: #{e.response.headers}"
  puts "Response body: #{e.response.body}"
end
```

> The above command returns JSON structured like this:

```json
{
  "success": true
}
```

Deletes a product for the current business. 

<aside class="warning">
This action is permanent.
</aside>


### Try it out in Swagger:

[`DELETE https://my.smartmenu.io/api/v2/products/:id`](https://my.smartmenu.io/swagger/#!/products/deleteProduct)

### Code examples:

[`DELETE https://my.smartmenu.io/api/v2/products/:id`](http://restunited.com/docs/5blsoeb1bwhd#httpmethod_Web_deleteProduct)

### URL Parameters

| Required | Name | Type | Description |
|:--------:|:-----|:-----|:------------|
| * | id |  Integer | The ID. |


# Other Products Actions
      
## Assign a Tag to a Product

```shell
curl /api/v2/products/71/tags/2 \
  --request POST \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept: application/json; charset=utf-8' \
  --verbose
```

> The above command returns JSON structured like this:

```json
  {
    "id": 71,
    "name": "Another German Potato Soup",
    "description": "Corrected info and description.",
    "subtitle": "With fresh basil leaves and a pinch of curry.",
    [...]
    "tags": [
      {
        "id": 2,
        "name": "Spicy",
        "description": "Spicy dish",
        "thumbnail_url": null,
        "image_url": null
      }
    ],
    "allergies": [],
    "allergy_codes": "N"
  }
```

Assign Tags to a product using their IDs. You can pass one more tags

### HTTP Method

`POST`

### URL Parameters

| Required | Name | Type | Description |
|:--------:|:-----|:-----|:------------|
| * | id |  Integer | The ID. |
| * | tag_ids |  String | The IDs of the Tags this product has been tagged with (comma separated). |


## Assign an allergy to a Product


```shell
curl /api/v2/products/71/allergies/2 \
  --request POST \
  --header 'Authorization: smartmenu.signed.token' \
  --header 'Accept: application/json; charset=utf-8' \
  --verbose
```


> The above command returns JSON structured like this:

```json
{
  "id": 71,
  "name": "Another German Potato Soup",
  "description": "Corrected info and description.",
  "subtitle": "With fresh basil leaves and a pinch of curry.",
   [...]
  "tags": [],
  "allergies": [
    {
      "id": 2,
      "name": "Sesame seed",
      "description": "Sesame seed",
      "thumbnail_url": null,
      "image_url": null,
      "code": "N"
    }
  ],
  "allergy_codes": "N"
}
```

Assign Allergies to a product using their IDs. You can pass multiple allergies by separating the IDs with comma.

### HTTP Method

`POST`

### URL Parameters

| Required | Name | Type | Description |
|:--------:|:-----|:-----|:------------|
| * | allergy_ids |  String | The IDs of the Allergies this product has been tagged with (comma separated). |
| * | id |  Integer | The ID. |


