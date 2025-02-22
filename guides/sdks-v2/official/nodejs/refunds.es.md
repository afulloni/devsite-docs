## Crear reembolso total

Puede crear un reembolso total con el SDK a continuación. Para detalles sobre los parámetros de solicitud, consulta la API [Crear reembolsos](/developers/es/reference/chargebacks/_payments_id_refunds/post).  

[[[
```node
const client = new MercadoPagoConfig({ accessToken: 'access_token', options: { timeout: 5000 } });

const refund = new PaymentsRefunds(client);

refund.create({
	payment_id: '123456789',
	body: {
		amount: 5
	}
}).then((result) => console.log(result))
  .catch((error) => console.log(error));
```
]]]

## Crear reembolso parcial

Puede crear un reembolso parcial con el SDK a continuación. Para detalles sobre los parámetros de solicitud, consulta la API [Crear reembolsos](/developers/es/reference/chargebacks/_payments_id_refunds/post). 

[[[
```node
var refund = {
 payment_id: payment_id,
 amount: 0.0
};
mercadopago.configure({
 access_token: 'YOUR_ACCESS_TOKEN'
});
 
mercadopago.refund.create(refund).then((result) => {
 console.log(result.response.id)
});
```
]]]

## Obtener reembolso específico

Puede obtener reembolsos específicos para ciertos pagos utilizando el SDK a continuación. Para detalles sobre los parámetros de solicitud, consulta la API de [Obtener reembolso específico](/developers/es/reference/chargebacks/_payments_id_refunds_refund_id/get).

[[[
```node
const client = new MercadoPagoConfig({ accessToken: 'access_token', options: { timeout: 5000 } });

const refund = new PaymentsRefunds(client);

refund.get({
	payment_id: '123',
	refund_id: '456'
}).then((result) => console.log(result))
	.catch((error) => console.log(error));
```
]]]

## Obtener lista de reembolsos

Puede ver todos los reembolsos de un pago específico utilizando el SDK a continuación. Para obtener detalles de los parámetros de la solicitud, acceda a la API [Obtener lista de reembolsos](/developers/es/reference/chargebacks/_payments_id_refunds/get)

[[[
```node
const client = new MercadoPagoConfig({ accessToken: 'access_token', options: { timeout: 5000 } });

const refund = new PaymentsRefunds(client);

refund.list({
	payment_id: '123456789'
}).then((result) => console.log(result))
	.catch((error) => console.log(error));
```
]]]