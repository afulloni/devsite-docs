# Crear orden

Puede crear un pedido para asociarlo con la preferencia de pago y obtener la URL necesaria para iniciar el flujo de pago a través del SDK a continuación. Para detalles de los parámetros de la solicitud, acceda a la API [Crear pedido](/developers/es/reference/merchant_orders/_merchant_orders/post)


[[[
```node
const client = new MercadoPagoConfig({ accessToken: 'access_token' });
const merchantOrder = new MerchantOrder(client);
const item = {
   id: 'item id',
   category_id: 'item category',
   currency_id: 'BRL',
   description: 'item description',
   picture_url: 'item picture',
   quantity: 1,
   unit_price: 5,
   title: 'item title'
};

const createRequest = {
   external_reference: 'default',
   preference_id: 'Preference identification',
   payer: {
 	id: 123,
 	nickname: 'JOHN'
   },
   site_id: 'MLA',
   items: items,
   preference_id: "Preference identification",
   application_id: "10000000000000000"
};
merchantOrder.create(createRequest)
    .then((result) => { console.log(result); })
    .catch((error) => { console.error(error); });
```
]]]

# Buscar órdenes

Puede encontrar toda la información de los pedidos creados a través de filtros específicos o por un rango de fechas específico a través del SDK a continuación. Para detalles de los parámetros de la solicitud, acceda a la API [Buscar pedidos](/developers/es/reference/merchant_orders/_merchant_orders_search/get).

[[[
```node
const client = new MercadoPagoConfig({ accessToken: '' });

const merchantOrder = new MerchantOrder(client);

merchantOrder.search({
  application_id: '10000000000000000'
})
  .then((result) => { console.log(result); })
  .catch((error) => { console.error(error); });
```
]]]

# Obtener orden

Puede obtener toda la información de pago de un producto o servicio con el ID de pedido de su elección a través del SDK a continuación. Para detalles de los parámetros de la solicitud, acceda a la API [Obtener pedido](/developers/es/reference/merchant_orders/_merchant_orders_id/get).

[[[
```node
const client = new MercadoPagoConfig({ accessToken: 'access_token' });

const merchantOrder = new MerchantOrder(client);

merchantOrder.get('123456789')
  .then((result) => { console.log(result); })
  .catch((error) => { console.error(error); });
```
]]]

# Actualizar orden

Puede actualizar los detalles de un pago indicando el ID del pedido y enviando la información que actualiza utilizando el SDK a continuación. Para detalles de los parámetros de la solicitud, acceda a la API [Actualizar Pedido](/developers/es/reference/merchant_orders/_merchant_orders_id/put).

[[[
```node
const client = new MercadoPagoConfig({ accessToken: 'access_token' });
const merchantOrder = new MerchantOrder(client);

const updateRequest = {
    external_reference: "default"
};

merchantOrder.update(123456789, updateRequest)
  .then((result) => { console.log(result); })
  .catch((error) => { console.error(error); });
```
]]]
