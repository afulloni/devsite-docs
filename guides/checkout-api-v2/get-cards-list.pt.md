# Consultar lista de cartões

É possível consultar a lista de cartões salvos para determinado cliente. Para isso, envie um **GET** com o `customer_id` do cliente ao endpoint [/v1/customers/{customer_id}/cards](/developers/pt/reference/cards/_customers_customer_id_cards/get) e execute a requisição ou, se preferir, utilize um de nossos SDKs abaixo.

[[[
```php
<?php
  $customer_client = new CustomerClient();
  $cards = $client->list("customer_id");
  echo implode ($cards);
?>
```
```node
const client = new MercadoPagoConfig({ accessToken: 'access_token' });
const customerClient = new Customer(client);

customerClient.listCards({ customerId: '123' })
	.then((result) => console.log(result));
```
```java

MercadoPagoConfig.setAccessToken("ENV_ACCESS_TOKEN");

CustomerCardClient customerCardClient = new CustomerCardClient();

MPResourceList<CustomerCard> list = customerCardClient.listAll("000000000-abcdEfghiJklM");
List<CustomerCard> customerCards = list.getResults();

```
```ruby

cards_response = sdk.card.list(customer_id)
cards = cards_response[:response]

```
```csharp

var customerClient = new CustomerClient();
ResourcesList<CustomerCard> customerCards = await customerClient.ListCardsAsync("CUSTOMER_ID");

```
```python

cards_response = sdk.card().list_all(customer_id)
cards = cards_response["response"]

```
```curl

curl -X GET \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  'https://api.mercadopago.com/v1/customers/CUSTOMER_ID/cards' \

```
]]]

A resposta trará o seguinte resultado:

```json
[{
    "id": "1490022319978",
    "expiration_month": 12,
    "expiration_year": 2020,
    "first_six_digits": "415231",
    "last_four_digits": "0001",
    ...
}]
```