# Modificar cliente

Si se produce un cambio en los datos de un cliente, como por ejemplo la dirección, la tarjeta o el email de la cuenta, es posible realizar esta alteración a través de la API de clientes o de nuestros SDKs.

En la siguiente tabla se describen todos los atributos que se pueden modificar, y para realizar el cambio, envía un **PUT** con el `customer_id` y los atributos que deseas modificar al endpoint [/v1/customers/{id}](/developers/es/reference/customers/_customers_id/put) y ejecuta la solicitud o, si lo prefieres, utiliza uno de los siguientes SDKs.


> NOTE
>
> Importante
>
> Si no tienes el `customer_id`, sigue los pasos indicados en la sección [Buscar cliente](/developers/es/docs/checkout-api/cards-and-customers-management/search-customers) para obtener la información. Además, el campo `email` solo puede actualizarse si el cliente aún no tiene un email asociado.


| Atributo  | Descripción  |
| --- | --- |
| `address`  | Dirección  |
| `default_address`  | Dirección estándar  |
| `default_card`  | Tarjeta estándar  |
| `description`  | Descripción  |
| `emai`  | Email de la cuenta |
| `first_name`  | Nombre  |
| `last_name`  | Apellido  |
| `phone`  | Teléfono registrado  |
| `identification`  | Tipo y número del documento  |


[[[
```php
<?php
  MercadoPagoConfig::setAccessToken("YOUR_ACCESS_TOKEN");
  
  $client = new CustomerClient();

  $customer = $client->update("user_id", [
    "email" => "my.user@example.com",
    "first_name" => "john",
    "last_name" => "wagner",
    "phone" => array(
      "area_code" => "11",
      "number" => "001234567"
    ),
    "identification" => array(
      "type" => "CPF",
      "number" => "12341234"
    ),
    "default_address" => "Casa",
    "address" => array(
      "zip_code" => "52",
      "street_name" => "Av. das Nações Unidas"
      "street_number" => "3033"
    )
  ]);
?>
```
```node
const client = new MercadoPagoConfig({ accessToken: 'YOUR_ACCESS_TOKEN' });
const customerClient = new Customer(client);

const customerBody = {
  email: "my.user@example.com"
  first_name: "john",
  last_name: "wagner",
  phone: {
    area_code: "11",
    number: "001234567"
  }
  identification: {
    type: "CPF",
    number: "12341234"
  },
  default_address: "Casa",
  address: {
    zip_code: "52",
    street_name: "Av. das Nações Unidas"
    street_number: "3033"
  }
};

customerClient.update({ customerBody }).then((result) => console.log(result));
```

```java

MercadoPagoConfig.setAccessToken("ENV_ACCESS_TOKEN");

CustomerClient client = new CustomerClient();

CustomerRequest request = CustomerRequest.builder()
   .email("user@user.com")
   .firstName("John")
   .lastName("Wagner")
   .defaultAddress("Casa")
   .phone(PhoneRequest.builder()
       .areaCode("11")
       .number("001234567")
       .build())
   .identification(IdentificationRequest.builder()
       .type("CPF")
       .number("12341234")
       .build())
   .description("Información del cliente")
   .defaultCard("None")
   .address(CustomerAddressRequest.builder()
       .zipCode("52")
       .streetName("Av. Caseros")
       .streetNumber(2)
       .build())
   .build();

client.update("247711297-jxOV430go9fx2e", request);

```
```ruby

require 'mercadopago'

sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

customer_request = {
  email: 'user@user.com',
  first_name: 'john',
  last_name: 'wagner',
  default_address: 'Casa',
  phone: {
    area_code: '[FAKER][PHONE_NUMBER][AREA_CODE]',
    number: '001234567'
  },
  identification: {
    type: '[FAKER][IDENTIFICATION][TYPE]',
    number: '12341234'
  },
  address: {
    zip_code: '[FAKER][ADDRESS][ZIP_CODE]',
    street_name: '[FAKER][ADDRESS][STREET_NAME]',
    street_number: '2'
  },
  description: 'Información del cliente',
  default_card: 'None'
}
customer_response = sdk.customer.update(customer_id ,customer_request)
customer = customer_response[:response]

```
```csharp

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";
var phoneRequest = new PhoneRequest
{
  AreaCode = "[FAKER][PHONE_NUMBER][AREA_CODE]",
  Number = "001234567"
};

var identificationRequest = new IdentificationRequest
{
  Type = "[FAKER][IDENTIFICATION][TYPE]",
  Number = "12341234"
};

var addressRequest = new AddressRequest
{
  ZipCode = "[FAKER][ADDRESS][ZIP_CODE]",
  StreetName = "[FAKER][ADDRESS][STREET_NAME]",
  StreetNumber = "2"
};

var customerRequest = new CustomerRequest
{
    Email = "test_payer_12345@testuser.com",
    FirstName = "john",
    LastName = "wagner",
    DefaultAddress = "home",
    Description = "Información del cliente",
    DefaultCard = "None",
    Phone = phoneRequest,
    Identification = identificationRequest,
    Address = addressRequest

};
var customerClient = new CustomerClient();
Customer customer = await customerClient.Update(customerRequest);

```
```python

import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

customer_data = {
  "email": 'user@user.com',
  "first_name": 'john',
  "last_name": 'wagner',
  "default_address": 'Casa',
  "phone": {
    "area_code": '[FAKER][PHONE_NUMBER][AREA_CODE]',
    "number": '001234567'
  },
  "identification": {
    "type": '[FAKER][IDENTIFICATION][TYPE]',
    "number": '12341234'
  },
  "address": {
    "zip_code": '[FAKER][ADDRESS][ZIP_CODE]',
    "street_name": '[FAKER][ADDRESS][STREET_NAME]',
    "street_number": '2'
  },
  "description": 'Información del cliente',
  "default_card": 'None'
}
customer_response = sdk.customer().update(customer_id, customer_data)
customer = customer_response["response"]

```
```curl

curl -X PUT \
    'https://api.mercadopago.com/v1/customers/{id}' \
    -H 'Authorization: Bearer ACCESS_TOKEN_ENV' \ 
    -d '{
  "email": "user@user.com",
  "first_name": "john",
  "last_name": "wagner",
  "address": {
    "zip_code": "[FAKER][ADDRESS][ZIP_CODE]",
    "street_name": "[FAKER][ADDRESS][STREET_NAME]",
    "street_number": "2"
  },
  "phone": {
    "area_code": "[FAKER][PHONE_NUMBER][AREA_CODE]",
    "number": "001234567"
  },
  "identification": {
    "type": "[FAKER][IDENTIFICATION][TYPE]",
    "number": "12341234"
  },
  "description": "Información del cliente" 
}'

```
]]]

