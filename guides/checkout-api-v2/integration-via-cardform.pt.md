# Cartão

A integração de pagamentos via cartão é feita via cardform. Neste modo de integração, o **MercadoPago.js** é responsável pelos fluxos necessários para obtenção das informações obrigatórias para a criação de um pagamento. Quando inicializado, uma busca é realizada para recolher os tipos de documentos disponíveis para o país em questão.

À medida que os dados do cartão são inseridos, ocorre uma busca automática das informações de emissor e parcelas disponíveis para aquele meio de pagamento. Com isso, a implementação do fluxo é transparente para quem realiza a integração.

----[mlb]----
> NOTE
>
> Importante
>
> Além das opções disponíveis nesta documentação, também é possível integrar **pagamentos com cartão** utilizando o **Brick de CardPayment**. Veja a documentação [Renderização padrão](/developers/pt/docs/checkout-bricks/card-payment-brick/default-rendering#editor_2) do CardPayment para mais detalhes. Também recomendamos a adoção do protocolo 3DS 2.0 para aumentar a probabilidade de aprovação dos seus pagamentos. Para obter mais informações, consulte a documentação sobre [Como integrar 3DS com Checkout Transparente.](/developers/pt/docs/checkout-api/how-tos/integrate-3ds)

------------
----[mla, mlm, mpe, mlc]----
> NOTE
>
> Importante
>
> Além das opções disponíveis nesta documentação, também é possível integrar **pagamentos com cartão** utilizando o **Brick de CardPayment**. Veja a documentação [Renderização padrão](/developers/pt/docs/checkout-bricks/card-payment-brick/default-rendering#editor_2) do CardPayment para mais detalhes. Também recomendamos a adoção do protocolo 3DS 2.0 para aumentar a probabilidade de aprovação dos seus pagamentos. Para obter mais informações, consulte a documentação sobre [Como integrar 3DS com Checkout API.](/developers/pt/docs/checkout-api/how-tos/integrate-3ds)

------------
----[mlu, mco]----
> NOTE
>
> Importante
>
> Além das opções disponíveis nesta documentação, também é possível integrar **pagamentos com cartão** utilizando o **Brick de CardPayment**. Veja a documentação [Renderização padrão](/developers/pt/docs/checkout-bricks/card-payment-brick/default-rendering#editor_2) do CardPayment para mais detalhes.

------------

Confira abaixo o diagrama que ilustra o processo de pagamento via cartão utilizando o Card Form.

![API-integration-flowchart](/images/api/api-integration-flowchart-cardform-2-pt.png)

Para integrar pagamentos com cartão no Checkout Transparente siga as etapas abaixo.

## Importar MercadoPago.js

A primeira etapa do processo de integração de pagamentos com cartões é a captura de dados do cartão. Esta captura é feita a partir da inclusão da biblioteca MercadoPago.js em seu projeto, seguida do formulário de pagamento. Utilize o código abaixo para importar a biblioteca MercadoPago.js antes de adicionar o formulário de pagamento.

[[[
```html
<body>
  <script src="https://sdk.mercadopago.com/js/v2"></script>
</body>
```
```bash

npm install @mercadopago/sdk-js

```
]]]

## Configurar credencial

As credenciais são chaves únicas com as quais identificamos uma integração na sua conta. Servem para capturar pagamentos em lojas virtuais e outras aplicações de forma segura.

Esta é a primeira etapa de uma estrutura completa de código que deverá ser seguida para a correta integração do pagamento via cartão. Atente-se aos blocos abaixo para adicionar aos códigos conforme indicado.

[[[
```html
<script>
  const mp = new MercadoPago("YOUR_PUBLIC_KEY");
</script>
```
```javascript
import { loadMercadoPago } from "@mercadopago/sdk-js";

await loadMercadoPago();
const mp = new window.MercadoPago("YOUR_PUBLIC_KEY");

```
]]]

## Adicionar formulário de pagamento

A captura dos dados do cartão é feita através do CardForm da biblioteca MercadoPago.js. Nosso CardForm se conectará ao seu formulário de pagamento HTML, facilitando a obtenção e validação de todos os dados necessários para processar o pagamento.

> WARNING
>
> Atenção
>
> O cardtoken pode ser usado **somente uma vez** e expira dentro de **7 dias**.

Para adicionar o formulário de pagamento, insira o HTML abaixo diretamente no projeto. 

----[mla, mlu, mpe, mco, mlb, mlc]----
[[[
```html
  <style>
    #form-checkout {
      display: flex;
      flex-direction: column;
      max-width: 600px;
    }

    .container {
      height: 18px;
      display: inline-block;
      border: 1px solid rgb(118, 118, 118);
      border-radius: 2px;
      padding: 1px 2px;
    }
  </style>
  <form id="form-checkout">
    <div id="form-checkout__cardNumber" class="container"></div>
    <div id="form-checkout__expirationDate" class="container"></div>
    <div id="form-checkout__securityCode" class="container"></div>
    <input type="text" id="form-checkout__cardholderName" />
    <select id="form-checkout__issuer"></select>
    <select id="form-checkout__installments"></select>
    <select id="form-checkout__identificationType"></select>
    <input type="text" id="form-checkout__identificationNumber" />
    <input type="email" id="form-checkout__cardholderEmail" />

    <button type="submit" id="form-checkout__submit">Pagar</button>
    <progress value="0" class="progress-bar">Carregando...</progress>
  </form>
```
]]]

------------
----[mlm]----
[[[
```html
  <style>
    #form-checkout {
      display: flex;
      flex-direction: column;
      max-width: 600px;
    }

    .container {
      height: 18px;
      display: inline-block;
      border: 1px solid rgb(118, 118, 118);
      border-radius: 2px;
      padding: 1px 2px;
    }
  </style>
  <form id="form-checkout">
    <div id="form-checkout__cardNumber" class="container"></div>
    <div id="form-checkout__expirationDate" class="container"></div>
    <div id="form-checkout__securityCode" class="container"></div>
    <input type="text" id="form-checkout__cardholderName" />
    <select id="form-checkout__issuer"></select>
    <select id="form-checkout__installments"></select>
    <input type="email" id="form-checkout__cardholderEmail" />

    <button type="submit" id="form-checkout__submit">Pagar</button>
    <progress value="0" class="progress-bar">Carregando...</progress>
  </form>
```
]]]

------------

## Inicializar formulário de pagamento

Após adicionar o formulário de pagamento, é preciso inicializá-lo. Esta etapa consiste em relacionar o ID de cada campo do formulário com os atributos correspondentes. A biblioteca será responsável pelo preenchimento, obtenção e validação de todos os dados necessários no momento de confirmação do pagamento. 

> NOTE
>
> Importante
>
> Ao enviar o formulário, um token é gerado representando, de forma segura, os dados do cartão. É possível acessá-lo através da função `cardForm.getCardFormData()`, como mostrado abaixo no callback `onSubmit`. Além disso, este token também é armazenado em um input oculto dentro do formulário no qual poderá ser encontrado com a nomenclatura `MPHiddenInputToken`.

----[mla, mlu, mpe, mco, mlb, mlc]----
[[[
```javascript

    const cardForm = mp.cardForm({
      amount: "100.5",
      iframe: true,
      form: {
        id: "form-checkout",
        cardNumber: {
          id: "form-checkout__cardNumber",
          placeholder: "Número do cartão",
        },
        expirationDate: {
          id: "form-checkout__expirationDate",
          placeholder: "MM/YY",
        },
        securityCode: {
          id: "form-checkout__securityCode",
          placeholder: "Código de segurança",
        },
        cardholderName: {
          id: "form-checkout__cardholderName",
          placeholder: "Titular do cartão",
        },
        issuer: {
          id: "form-checkout__issuer",
          placeholder: "Banco emissor",
        },
        installments: {
          id: "form-checkout__installments",
          placeholder: "Parcelas",
        },        
        identificationType: {
          id: "form-checkout__identificationType",
          placeholder: "Tipo de documento",
        },
        identificationNumber: {
          id: "form-checkout__identificationNumber",
          placeholder: "Número do documento",
        },
        cardholderEmail: {
          id: "form-checkout__cardholderEmail",
          placeholder: "E-mail",
        },
      },
      callbacks: {
        onFormMounted: error => {
          if (error) return console.warn("Form Mounted handling error: ", error);
          console.log("Form mounted");
        },
        onSubmit: event => {
          event.preventDefault();

          const {
            paymentMethodId: payment_method_id,
            issuerId: issuer_id,
            cardholderEmail: email,
            amount,
            token,
            installments,
            identificationNumber,
            identificationType,
          } = cardForm.getCardFormData();

          fetch("/process_payment", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              token,
              issuer_id,
              payment_method_id,
              transaction_amount: Number(amount),
              installments: Number(installments),
              description: "Descrição do produto",
              payer: {
                email,
                identification: {
                  type: identificationType,
                  number: identificationNumber,
                },
              },
            }),
          });
        },
        onFetching: (resource) => {
          console.log("Fetching resource: ", resource);

          // Animate progress bar
          const progressBar = document.querySelector(".progress-bar");
          progressBar.removeAttribute("value");

          return () => {
            progressBar.setAttribute("value", "0");
          };
        }
      },
    });
```
]]]

------------
----[mlm]----
[[[
```javascript

    const cardForm = mp.cardForm({
      amount: "100.5",
      iframe: true,
      form: {
        id: "form-checkout",
        cardNumber: {
          id: "form-checkout__cardNumber",
          placeholder: "Número do cartão",
        },
        expirationDate: {
          id: "form-checkout__expirationDate",
          placeholder: "MM/YY",
        },
        securityCode: {
          id: "form-checkout__securityCode",
          placeholder: "Código de segurança",
        },
        cardholderName: {
          id: "form-checkout__cardholderName",
          placeholder: "Titular do cartão",
        },
        issuer: {
          id: "form-checkout__issuer",
          placeholder: "Banco emissor",
        },
        installments: {
          id: "form-checkout__installments",
          placeholder: "Parcelas",
        },        
        cardholderEmail: {
          id: "form-checkout__cardholderEmail",
          placeholder: "E-mail",
        },
      },
      callbacks: {
        onFormMounted: error => {
          if (error) return console.warn("Form Mounted handling error: ", error);
          console.log("Form mounted");
        },
        onSubmit: event => {
          event.preventDefault();

          const {
            paymentMethodId: payment_method_id,
            issuerId: issuer_id,
            cardholderEmail: email,
            amount,
            token,
            installments,
            identificationNumber,
            identificationType,
          } = cardForm.getCardFormData();

          fetch("/process_payment", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              token,
              issuer_id,
              payment_method_id,
              transaction_amount: Number(amount),
              installments: Number(installments),
              description: "Descrição do produto",
              payer: {
                email,
                identification: {
                  type: identificationType,
                  number: identificationNumber,
                },
              },
            }),
          });
        },
        onFetching: (resource) => {
          console.log("Fetching resource: ", resource);

          // Animate progress bar
          const progressBar = document.querySelector(".progress-bar");
          progressBar.removeAttribute("value");

          return () => {
            progressBar.setAttribute("value", "0");
          };
        }
      },
    });
```
]]]

------------

> NOTE
>
> Importante
>
> Caso necessite adicionar ou modificar alguma lógica no fluxo dos métodos do Javascript consulte a documentação [Integração via Métodos Core](/developers/pt/docs/checkout-api/integration-configuration/card/integrate-via-core-methods)

## Enviar pagamento

Para continuar o processo de integração de pagamento via cartão, é necessário que o backend receba a informação do formulário com o token gerado e os dados completos conforme indicado nas etapas anteriores.

No exemplo da seção anterior, enviamos todos os dados necessários para criar o pagamento para o endpoint `process_payment` do backend.

Com todas as informações coletadas no backend, envie um POST com os atributos necessários, atentando-se aos parâmetros `token`, `transaction_amount`, `installments`, `payment_method_id` e o `payer.email` ao endpoint [/v1/payments](/developers/pt/reference/payments/_payments/post) e execute a requisição ou, se preferir, faça o envio das informações utilizando nossos SDKs.

> NOTE
>
> Importante
>
> Para aumentar as chances de aprovação do pagamento e evitar que a análise antifraude não autorize a transação, recomendamos inserir o máximo de informação sobre o comprador ao realizar a requisição. Para mais detalhes sobre como aumentar as chances de aprovação, veja [Como melhorar a aprovação dos pagamentos](/developers/pt/docs/checkout-api/how-tos/improve-payment-approval).

[[[
```php
<?php
  use MercadoPago\Client\Payment\PaymentClient;
  use MercadoPago\MercadoPagoConfig;


  MercadoPagoConfig::setAccessToken("YOUR_ACCESS_TOKEN");

  $client = new PaymentClient();
  $request_options = new MPRequestOptions();
  $request_options->setCustomHeaders(["X-Idempotency-Key: <SOME_UNIQUE_VALUE>"]);

  $payment = $client->create([
    "transaction_amount" => (float) $_POST['transactionAmount'],
    "token" => $_POST['token'],
    "description" => $_POST['description'],
    "installments" => $_POST['installments'],
    "payment_method_id" => $_POST['paymentMethodId'],
    "issuer_id" => $_POST['issuer'],
    "payer" => [
      "email" => $_POST['email'],
      "identification" => [
        "type" => $_POST['identificationType'],
        "number" => $_POST['number']
      ]
    ]
  ], $request_options);
  echo implode($payment);
?>
```
```node
import { MercadoPagoConfig, Payments } from 'mercadopago';

const client = new MercadoPagoConfig({ accessToken: 'YOUR_ACCESS_TOKEN' });
const payments = new Payments(client);

payments.create({
  transaction_amount: req.transaction_amount,
  token: req.token,
  description: req.description,
  installments: req.installments,
  payment_method_id: req.paymentMethodId,
  issuer_id: req.issuer,
  payer: {
    email: req.email,
    identification: {
      type: req.identificationType,
      number: req.number
    }
  } 
}, { idempotencyKey: '<SOME_UNIQUE_VALUE>' })
  .then((result) => console.log(result))
  .catch((error) => console.log(error));
```
```java
===
Encontre o estado do pagamento no campo _status_.
===

PaymentClient client = new PaymentClient();

PaymentCreateRequest paymentCreateRequest =
   PaymentCreateRequest.builder()
       .transactionAmount(request.getTransactionAmount())
       .token(request.getToken())
       .description(request.getDescription())
       .installments(request.getInstallments())
       .paymentMethodId(request.getPaymentMethodId())
       .payer(
           PaymentPayerRequest.builder()
               .email(request.getPayer().getEmail())
               .firstName(request.getPayer().getFirstName())
               .identification(
                   IdentificationRequest.builder()
                       .type(request.getPayer().getIdentification().getType())
                       .number(request.getPayer().getIdentification().getNumber())
                       .build())
               .build())
       .build();

client.create(paymentCreateRequest);

```
```ruby
===
Encontre o estado do pagamento no campo _status_.
===
require 'mercadopago'
sdk = Mercadopago::SDK.new('YOUR_ACCESS_TOKEN')
 
payment_data = {
 transaction_amount: params[:transactionAmount].to_f,
 token: params[:token],
 description: params[:description],
 installments: params[:installments].to_i,
 payment_method_id: params[:paymentMethodId],
 payer: {
   email: params[:email],
   identification: {----[mla, mlb, mlu, mlc, mpe, mco]----
     type: params[:identificationType],------------
     number: params[:identificationNumber]
   }
 }
}
 
payment_response = sdk.payment.create(payment_data)
payment = payment_response[:response]
 
puts payment
 
```
```csharp
===
Encontre o status do pagamento no campo _status_.
===
using System;
using MercadoPago.Client.Common;
using MercadoPago.Client.Payment;
using MercadoPago.Config;
using MercadoPago.Resource.Payment;
 
MercadoPagoConfig.AccessToken = "YOUR_ACCESS_TOKEN";
 
var paymentRequest = new PaymentCreateRequest
{
   TransactionAmount = decimal.Parse(Request["transactionAmount"]),
   Token = Request["token"],
   Description = Request["description"],
   Installments = int.Parse(Request["installments"]),
   PaymentMethodId = Request["paymentMethodId"],
   Payer = new PaymentPayerRequest
   {
       Email = Request["email"],
       Identification = new IdentificationRequest
       {----[mla, mlb, mlu, mlc, mpe, mco]----
           Type = Request["identificationType"],------------
           Number = Request["identificationNumber"],
       },
   },
};
 
var client = new PaymentClient();
Payment payment = await client.CreateAsync(paymentRequest);
 
Console.WriteLine(payment.Status);
 
```
```python
===
Encontre o status do pagamento no campo _status_.
===
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")
 
payment_data = {
   "transaction_amount": float(request.POST.get("transaction_amount")),
   "token": request.POST.get("token"),
   "description": request.POST.get("description"),
   "installments": int(request.POST.get("installments")),
   "payment_method_id": request.POST.get("payment_method_id"),
   "payer": {
       "email": request.POST.get("email"),
       "identification": {----[mla, mlb, mlu, mlc, mpe, mco]----
           "type": request.POST.get("type"), ------------
           "number": request.POST.get("number")
       }
   }
}
 
payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
 
print(payment)
```
```curl
===
Encontre o status do pagamento no campo _status_.
===
 
curl -X POST \
   -H 'accept: application/json' \
   -H 'content-type: application/json' \
   -H 'Authorization: Bearer YOUR_ACCESS_TOKEN' \
   'https://api.mercadopago.com/v1/payments' \
   -d '{
         "transaction_amount": 100,
         "token": "ff8080814c11e237014c1ff593b57b4d",
         "description": "Blue shirt",
         "installments": 1,
         "payment_method_id": "visa",
         "issuer_id": 310,
         "payer": {
           "email": "PAYER_EMAIL"
         }
   }'
 
```
]]]

A resposta trará o seguinte resultado

```json
{
   "status": "approved",
   "status_detail": "accredited",
   "id": 3055677,
   "date_approved": "2019-02-23T00:01:10.000-04:00",
   "payer": {
       ...
   },
   "payment_method_id": "visa",
   "payment_type_id": "credit_card",
   "refunds": [],
   ...
}
```
> WARNING
>
> Atenção
>
> Os pagamentos criados possuem os seguintes status: "Pendente", "Rejeitado" e "Aprovado". Para acompanhar as atualizações é necessário configurar seu sistema para receber as notificações de pagamentos e outras atualizações de status. Veja [Notificações](/developers/pt/docs/checkout-api/additional-content/your-integrations/notifications) para mais detalhes.

## Exemplo de código

----[mlb]----
> GIT
>
> Checkout Transparente
>
> Para exemplos completos de código, confira os [exemplos completos de integração](https://github.com/mercadopago/card-payment-sample) disponíveis no GitHub.

------------
----[mla, mlm, mpe, mco, mlu, mlc]----
> GIT
>
> Checkout API
>
> Para exemplos completos de código, confira os [exemplos completos de integração](https://github.com/mercadopago/card-payment-sample) disponíveis no GitHub.

------------