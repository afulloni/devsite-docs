# Create customer and card

To create a customer and associate it with your card, you must send the email field, the type of payment method, the ID of the issuing bank and the generated token. Each customer will be saved with the value `customer` and each card with the value `card`.

In addition, we recommend storing your card data whenever a payment is successfully completed. This allows the correct data to be stored for future purchases and streamlines the payment process for the buyer.

To create a customer and card, use one of the codes below.

[[[
```php
<?php
  MercadoPagoConfig::setAccessToken("YOUR_ACCESS_TOKEN");
  
  $client_customer = new CustomerClient();
  $customer = $client_customer->create(["email" => "my.user@example.com"]);
  $client = new CustomerCardClient();
  $customer_card = $client->create($customer->id, ["token" => "your_card_token"]);
?>
```
```node
const client = new MercadoPagoConfig({ accessToken: 'YOUR_ACCESS_TOKEN' });


const customerClient = new Customer(client);

const customerBody = {
  email: "my.user@example.com"
};

customerClient.create({ customerBody }).then(result) => {
  const cardClient = new CustomerCard(client);

  const body = {
  	token : result.token,
  };

  customerClient.create({ customerId: 'customer_id', customerCardBody :   body }).then((result) => console.log(result));
};
```
```java

MercadoPagoConfig.setAccessToken("ENV_ACCESS_TOKEN");

CustomerClient customerClient = new CustomerClient();
CustomerCardClient customerCardClient = new CustomerCardClient();

CustomerRequest customerRequest = CustomerRequest.builder()
.email("john@test.com")
.build();
Customer customer = customerClient.create(customerRequest);

CustomerCardIssuer issuer = CustomerCardIssuer.builder()
.id("3245612")
.build();

CustomerCardCreateRequest cardCreateRequest = CustomerCardCreateRequest.builder()
.token("9b2d63e00d66a8c721607214cedaecda")
.issuer(issuer)
.paymentMethodId("debit_card")
.build();

customerCardClient.create(customer.getId(), cardCreateRequest);

```
```ruby

require 'mercadopago'

sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

customer_request = {
email: 'john@yourdomain.com'
}
customer_response = sdk.customer.create(customer_request)
customer = customer_response[:response]

card_request = {
token: '9b2d63e00d66a8c721607214cedaecda',
issuer_id: '3245612',
payment_method_id: 'visa'
}
card_response = sdk.card.create(customer['id'], card_request)
card = card_response[:response]

```
```csharp

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var customerRequest = new CustomerRequest
{
Email = "test_payer_12345@testuser.com",
};
var customerClient = new CustomerClient();
Customer customer = await customerClient.CreateAsync(customerRequest);

var cardRequest = new CustomerCardCreateRequest
{
Token = "9b2d63e00d66a8c721607214cedaecda"
};
CustomerCard card = await customerClient.CreateCardAsync(customer.Id, cardRequest);

```
```python

import market
sdk = Mercadopago.SDK("ENV_ACCESS_TOKEN")

customer_data = {
"email": "test_payer_12345@testuser.com"
}
customer_response = sdk.customer().create(customer_data)
customer = customer_response["response"]

card_data = {
"token": "9b2d63e00d66a8c721607214cedaecda",
"issuer_id": "3245612",
"payment_method_id": "visa"
}
card_response = sdk.card().create(customer["id"], card_data)
card = card_response["response"]

```
```curl

curl -X POST \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
'https://api.mercadopago.com/v1/customers/CUSTOMER_ID/cards' \
-d '{"token": "9b2d63e00d66a8c721607214cedaecda", "issuer_id": "3245612", "payment_method_id": "visa"}'

```
]]]

The response will bring the following result.

```json
{
"id": "123456789-jxOV430go9fx2e",
"email": "test_payer_12345@testuser.com",
...
"default_card": "1490022319978",
"default_address": null,
"cards": [{
"id": "1490022319978",
"expiration_month": 12,
"expiration_year": 2020,
"first_six_digits": "415231",
"last_four_digits": "0001",
...
}],
"addresses": [],
"live_mode": false
}
```

> WARNING
>
> Attention
>
> If the request response returns an error like `invalid parameter` with HTTP code 400, review the `payment_method_id` and `issuer_id` parameters to ensure that the values have been entered correctly. Also, when using test users, keep in mind the following format for customer email: `test_payer_[0-9]{1,10}@testuser.com` For example: `test_payer_12345@testuser.com`.