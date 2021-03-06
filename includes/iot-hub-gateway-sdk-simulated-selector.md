> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

Este tutorial del [ejemplo de carga en la nube de dispositivos simulados] muestra cómo usar el [SDK de puerta de enlace de IoT de Microsoft Azure][lnk-sdk] para enviar los datos de telemetría que envían los dispositivos a la nube al Centro de IoT desde dispositivos simulados.

En este tutorial, se describen los siguientes procedimientos:

1. **Arquitectura**: información de arquitectura importante sobre el ejemplo de carga en la nube de dispositivos simulados.

2. **Compilación y ejecución**: los pasos necesarios para compilar y ejecutar el ejemplo.

## Arquitectura

El ejemplo de carga en la nube de dispositivos simulados muestra cómo usar el SDK para crear una puerta de enlace que envíe datos de telemetría desde dispositivos simulados a un Centro de IoT. Los dispositivos simulados no se pueden conectar directamente al Centro de IoT por los siguientes motivos:

- Los dispositivos no utilizan un protocolo de comunicaciones que entienda el Centro de IoT.
- Los dispositivos no son lo suficientemente inteligentes como para recordar la identidad que les ha asignado el Centro de IoT.

La puerta de enlace soluciona estos problemas para los dispositivos simulados de las maneras siguientes:

- La puerta de enlace comprende el protocolo utilizado por los dispositivos simulados, recibe la telemetría que envían los dispositivos a la nube y reenvía esos mensajes al Centro de IoT mediante un protocolo que entiende el centro.
- La puerta de enlace almacena las identidades del Centro de IoT en nombre de los dispositivos simulados y actúa como proxy cuando estos envían mensajes al Centro de IoT.

En el siguiente diagrama se muestran los componentes principales del ejemplo, incluidos los módulos de puerta de enlace:

![][1]


> [AZURE.NOTE] Los módulos no se pasan mensajes directamente. Publican mensajes en un bus de mensajes interno que entrega los mensajes a los demás módulos mediante un mecanismo de suscripción, como se muestra en el diagrama siguiente. Para más información, consulte [Empezar a trabajar con el SDK de puerta de enlace][lnk-gw-getstarted].

### Módulo de ingesta de protocolos

Este módulo es el punto de partida para obtener datos de dispositivos mediante la puerta de enlace y depositarlos en la nube. En el ejemplo, el módulo realiza cuatro tareas:

1.  Crea datos de temperatura simulados.
    
    Nota: Si fuera a usar dispositivos reales, el módulo leería los datos de esos dispositivos físicos.

2.  Coloca los datos de temperatura simulados en el contenido de un mensaje.

3.  Agrega una propiedad con una dirección MAC falsa al mensaje que contiene los datos de temperatura simulados.

4.  Permite que el mensaje esté disponible para el siguiente módulo de la cadena.

> [AZURE.NOTE] El módulo denominado **Ingesta de protocolos X** en el diagrama anterior se denomina **Dispositivo simulado** en el código fuente.

### Módulo de identificación MAC &lt;-&gt; Centro de IoT

Este módulo busca los mensajes que incluyen una propiedad que contiene la dirección MAC, agregada por el módulo de ingesta de protocolos del dispositivo simulado. Si el módulo detecta esta propiedad, agrega otra propiedad con una clave de dispositivo del Centro de IoT al mensaje y luego hace que el mensaje esté disponible para el siguiente módulo de la cadena. Así es cómo en el ejemplo se asocian las identidades de dispositivos del Centro de IoT a los dispositivos simulados. El desarrollador configura la asignación entre las direcciones MAC y las identidades del centro de IoT manualmente como parte de la configuración del módulo.

> [AZURE.NOTE]  En este ejemplo se usa una dirección MAC como identificador único del dispositivo y se correlaciona con una identidad de dispositivo del Centro de IoT. Sin embargo, puede escribir su propio módulo para que utilice un identificador único diferente. Por ejemplo, podría tener dispositivos con números de serie únicos o datos de telemetría que tengan insertado un nombre de dispositivo único que podría usar para determinar la identidad del dispositivo del Centro de IoT.

### Módulo de comunicación del Centro de IoT

Este módulo toma los mensajes que tienen una identidad de dispositivo del Centro de IoT asignada por el módulo anterior y envía el contenido del mensaje al Centro de IoT mediante HTTPS. HTTPS es uno de los tres protocolos que comprende el Centro de IoT.

En lugar de abrir una conexión al Centro de IoT para cada dispositivo simulado, este módulo abre una única conexión HTTP desde la puerta de enlace al Centro de IoT y multiplexa las conexiones desde todos los dispositivos simulados a través de esa conexión. Esto permite que una sola puerta de enlace conecte muchos más dispositivos, simulados o no, de lo que sería posible si abriera una conexión única para cada uno.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[ejemplo de carga en la nube de dispositivos simulados]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md

