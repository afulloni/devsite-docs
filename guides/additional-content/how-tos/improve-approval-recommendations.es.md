# Recomendaciones para mejorar tu aprobación

> WARNING
> 
> Atención
>
> Recomendamos  evaluar la [calidad de tu integración](/developers/es/guides/additional-content/homologator/homologator) antes de salir a producción para que puedas validar si estás cumpliendo con los estándares de calidad y seguridad de Mercado Pago  que pueden mejorar tu tasa de aprobación de pagos.

Para evitar que un pago real sea rechazado porque no cumple con las validaciones de seguridad, **es necesario incluir toda la información posible** al realizar la transacción y también prestar atención a algunos requisitos de seguridad, como nuestro **Código de seguridad** y el **device ID**.

> NOTE
>
> Importante
>
> Si utilizas Checkout Pro, ya cuentas con toda la seguridad para prevenir fraude.

## Detallar toda la información sobre el pago

Para optimizar la validación de la seguridad de los pagos y mejorar las aprobaciones, es valioso enviar la mayor cantidad posible de datos del comprador y del ítem. 
 
Presta atención a todos los atributos disponibles al crear un pago con el método [Crear pago](/developers/es/reference/payments/_payments/post). 

## Configurar preferencias

Puedes adaptar la **integración de Checkout Pro** a tu modelo de negocio configurando atributos de preferencia, que ayudan a mejorar las aprobaciones.

Para obtener más detalles, visite la documentación de configuración de preferencias de [Checkout Pro](/developers/es/docs/checkout-pro/checkout-customization/preferences).

## Mejorar la experiencia del usuario

A menudo, el comprador puede cometer un error al completar sus datos al finalizar la compra, así que vale la pena revisar cada paso, interacciones, e incluso diseño, para comprobar que todo está tan claro como debe ser. Realiza los cambios necesarios para evitar confusiones o problemas.

## Ayuda a tus clientes con sus pagos rechazados

Es importante que expliques a tus clientes el motivo de la denegación del pago y qué medidas pueden tomar para solucionarlo. Tus clientes tendrán toda la información que necesitan para pagar sin problemas.

Por ejemplo, si se rechaza un pago por fondos insuficientes, puedes recomendarles que intenten nuevamente con otro método de pago para completar la transacción.

> Si utilizas el Checkout Pro, no te preocupes, ya tienes configurados los mensajes según cada caso. Y si usas otro de nuestros productos, te recomendamos mostrar un **mensaje específico por cada motivo de rechazo**.

## Agrega nuestro código de seguridad en tu sitio

Agrega el código de seguridad de Mercado Pago a tu sitio, reemplazando el valor de `view` con el nombre de la sección en la que deseas agregarlo (Ej.: home, search, item).

```html
<script src="https://www.mercadopago.com/v2/security.js" view="home"></script>
```

> NOTE
>
> Importante
>
> En caso de no tener un valor disponible para la sección, puedes dejarlo vacío.

## Uso del device ID en la web

La identificación del dispositivo es una información importante para garantizar una mejor seguridad y, en consecuencia, una mejor tasa de aprobación de pagos. Representa **un identificador único para el dispositivo de cada comprador** en el momento de la compra.

Si un comprador frecuente hace una compra desde un dispositivo diferente al habitual, esto podría representar un comportamiento falso e indicarnos que esta transacción no debe realizarse.

Para usarlo en la web, debes seguir los siguientes pasos:

### 1. Agrega nuestro código de seguridad

Para implementar la generación del device ID en tu sitio, agrega el siguiente código a su página de Checkout:

```html
<script src="https://www.mercadopago.com/v2/security.js" view="checkout"></script>
```

### 2. Obteniendo el device ID

Una vez que hayas agregado el código de seguridad de Mercado Pago a tu sitio, automáticamente se crea una variable JavaScript global con el nombre `MP_DEVICE_SESSION_ID`, cuyo valor es el ID del dispositivo. 

  - Si prefieres asignarlo a otra variable, indica el nombre agregando el atributo `output` al script de seguridad, como en el siguiente ejemplo:  
  ```html
  <script src="https://www.mercadopago.com/v2/security.js" view="checkout" output="deviceId"></script>
  ```

  - También puedes crear tu propia variable agregando una etiqueta HTML a tu sitio con el identificador `id="deviceID"`, como en el siguiente ejemplo:
  ```html
  <input type="hidden" id="deviceId">
  ```

### 3. Uso del device ID

Una vez que tengas el valor de ID del dispositivo, debes enviarlo a nuestros servidores al crear un pago. Para hacer esto, agrega el siguiente encabezado (header) a la solicitud:

```http
X-meli-session-id: device_id
```

> WARNING
> 
> Atención
>
> Recuerda reemplazar `device_id` con el nombre de la variable que contiene su valor de ID del dispositivo.

## Implementa el device ID en tu aplicación móvil nativa

Si tienes una aplicación nativa, puedes capturar la información del dispositivo con nuestro SDK y enviarla al momento de crear el token. Sigue estos pasos:

### 1. Agrega la dependencia

[[[
```ios
===
Agrega el siguiente código en el archivo **Podfile**.
===
use_frameworks!
pod ‘MercadoPagoDevicesSDK’
```
```android
===
Necesitas agregar el repositorio y la dependencia en el archivo **build.gradle**.
===
repositories {
    maven {
        url "https://artifacts.mercadolibre.com/repository/android-releases"
    }
}
dependencies {
   implementation 'com.mercadolibre.android.device:sdk:3.1.0'
}
```
]]]

### 2. Inicializa el módulo

[[[
```swift
===
Te recomendamos iniciarlo en el evento didFinishLaunchingWithOptions del AppDelegate.
===
import MercadoPagoDevicesSDK
...
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        ...        
        MercadoPagoDevicesSDK.shared.execute()
        ...
}
```
```objective-c
===
Te recomendamos iniciarlo en el evento didFinishLaunchingWithOptions del AppDelegate.
===
@import ‘MercadoPagoDevicesSDK’;
...
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    ...
    [[MercadoPagoDevicesSDK shared] execute];
    ...
}
```
```java
===
Te recomendamos iniciarlo en la clase MainApplication.
===
import com.mercadolibre.android.device.sdk.DeviceSDK;


DeviceSDK.getInstance().execute(this);
```

]]]

### 3. Captura la información

Ejecuta alguna de estas funciones para obtener la información en el formato que prefieras.

[[[
```swift
MercadoPagoDevicesSDK.shared.getInfo() // devuelve un objeto Device que es Codable
MercadoPagoDevicesSDK.shared.getInfoAsJson() // devuelve un objeto Data de la librería de JSON
MercadoPagoDevicesSDK.shared.getInfoAsJsonString() // devuelve el json en formato de String
MercadoPagoDevicesSDK.shared.getInfoAsDictionary() // devuelve un Dictionary<String,Any>
```
```objective-c
[[[MercadoPagoDevicesSDK] shared] getInfoAsJson] // devuelve un objeto Data de la librería JSON
[[[MercadoPagoDevicesSDK] shared] getInfoAsJsonString] // devuelve el json en formato de String
[[[MercadoPagoDevicesSDK] shared] getInfoAsDictionary] // devuelve un Dictionary<String,Any>
```
```java
Device device = DeviceSDK.getInstance().getInfo() // devuelve un objeto Device, serializable
Map deviceMap = DeviceSDK.getInstance().getInfoAsMap()  // devuelve un Map<String, Object>
String jsonString = DeviceSDK.getInstance().getInfoAsJsonString() // devuelve un String de tipo Json
```
]]]

### 4. Envía la información

Por último, envía la información en el campo `device` al crear el `card_token`.

```
{
	...,
	 "device":{
	  "fingerprint":{
	     "os":"iOS",
	     "system_version":"8.3",
	     "ram":18446744071562067968,
	     "disk_space":498876809216,
	     "model":"MacBookPro9,2",
	     "free_disk_space":328918237184,
	     "vendor_ids":[
	        {
	           "name":"vendor_id",
	           "value":"C2508642-79CF-44E4-A205-284A4F4DE04C"
	        },
	        {
	           "name":"uuid",
	           "value":"AB28738B-8DC2-4EC2-B514-3ACF330482B6"
	        }
	     ],
	     "vendor_specific_attributes":{
	        "feature_flash":false,
	        "can_make_phone_calls":false,
	        "can_send_sms":false,
	        "video_camera_available":true,
	        "cpu_count":4,
	        "simulator":true,
	        "device_languaje":"en",
	        "device_idiom":"Phone",
	        "platform":"x86_64",
	        "device_name":"iPhone Simulator",
	        "device_family":4,
	        "retina_display_capable":true,
	        "feature_camera":false,
	        "device_model":"iPhone Simulator",
	        "feature_front_camera":false
	     },
	     "resolution":"375x667"
	  }
}
```