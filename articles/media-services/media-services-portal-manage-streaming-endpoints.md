<properties 
	pageTitle="Administración de puntos de conexión de streaming con el Portal de Azure | Microsoft Azure" 
	description="En este tema se muestra cómo administrar los puntos de conexión de streaming mediante el Portal de Azure." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	writer="juliako" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/29/2016"
	ms.author="juliako"/>


#Administración de puntos de conexión de streaming con el Portal de Azure

## Información general

> [AZURE.NOTE] Para completar este tutorial, deberá tener una cuenta de Azure. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

En Servicios multimedia de Microsoft Azure, un **extremo de streaming** representa un servicio de streaming que puede entregar contenido directamente a una aplicación de reproducción de cliente o a una red de entrega de contenido (CDN) para la distribución posterior. Servicios multimedia también proporciona integración sin problemas de CDN de Azure. La secuencia de salida de un servicio de StreamingEndpoint puede ser una secuencia en vivo o un recurso de vídeo bajo demanda en su cuenta de Servicios multimedia.

Además, puede controlar la capacidad del servicio de punto de conexión de streaming para administrar las necesidades crecientes de ancho de banda mediante el ajuste de unidades de streaming. Se recomienda asignar una o más unidades de escalado para las aplicaciones en el entorno de producción. Las unidades de streaming proporcionan capacidad de salida específica que puede adquirirse en incrementos de 200 Mbps y funcionalidad adicional que incluye: [empaquetado dinámico](media-services-dynamic-packaging-overview.md), integración con red CDN y configuración avanzada.

>[AZURE.NOTE]Solo se le cobrará cuando StreamingEndpoint esté en estado en ejecución.

Este tema proporciona información general de las principales funcionalidades proporcionadas por los puntos de conexión de streaming. El tema también muestra cómo usar el Portal de Azure para administrar puntos de conexión de streaming. Para obtener información sobre cómo escalar el punto de conexión de streaming, consulte [este](media-services-portal-scale-streaming-endpoints.md) tema.

## Inicio de la administración de puntos de conexión de streaming

Para comenzar a administrar puntos de conexión de streaming de su cuenta, haga lo siguiente.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).
2. En la ventana **Configuración**, haga clic en **Extremos de streaming**.

	![Extremo de streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##Adición o eliminación de puntos de conexión de streaming

Para agregar/quitar el punto de conexión de streaming mediante el Portal de Azure, haga lo siguiente:

1. Para agregar un punto de conexión de streaming, haga clic en **+ Punto de conexión** en la parte superior de la página.
2. Para eliminar un punto de conexión de streaming, haga clic en el botón **Eliminar**.

	No se puede eliminar el punto de conexión de streaming predeterminado.
2. Haga clic en el botón **Iniciar** para iniciar el punto de conexión de streaming.

	![Extremo de streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

De forma predeterminada puede tener hasta dos extremos de streaming. Si necesita solicitar más, consulte [Cuotas y limitaciones](media-services-quotas-and-limitations.md).
	
##<a id="configure_streaming_endpoints"></a>Configuración del extremo de streaming

Extremo de streaming le permite configurar las siguientes propiedades cuando tenga al menos una unidad de escala:

- Control de acceso
- Control de caché
- Directivas de acceso en varios sitios

Para obtener información detallada acerca de estas propiedades, consulte [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx).

Puede configurar el punto de conexión de streaming haciendo lo siguiente:

1. Seleccione el extremo de streaming que desee configurar.
1. Haga clic en **Configuración**.
  
Aparecerá una breve descripción de los campos.

![Extremo de streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Directiva de caché máximo: se usa para configurar la duración de la caché de los recursos entregados a través de este punto de conexión de streaming. Si no se establece ningún valor, se utiliza el valor predeterminado. Los valores predeterminados también pueden definirse directamente en el almacenamiento de Azure. Si la red CDN de Azure está habilitada para el punto de conexión de streaming, no debe establecer el valor de la directiva de caché en menos de 600 segundos.

2. Direcciones IP permitidas: especifique las direcciones IP que podrán conectarse al punto de conexión de streaming publicado. Si no se especificaron direcciones IP, cualquier dirección IP se podrá conectar. Las direcciones IP se pueden especificar como una dirección IP única (por ejemplo, 10.0.0.1), un intervalo de IP que usa una dirección IP y una máscara de subred CIDR (por ejemplo, 10.0.0.1/22) o un intervalo de IP que usa una máscara de subred decimal con puntos; por ejemplo, 10.0.0.1(255.255.255.0).

3. Configuración de autenticación del encabezado de firma de Akamai: se utiliza para especificar cómo se configura la solicitud de autenticación de encabezado de firma desde servidores Akamai. La expiración está en formato UTC.



##<a id="enable_cdn"></a>Habilitación de la integración de CDN de Azure

Puede especificar habilitar la integración de CDN de Azure para un extremo de streaming(está deshabilitada de forma predeterminada).

Para configurar la integración de CDN de Azure en true:

- El punto de conexión de streaming debe tener al menos una unidad de streaming. Si más adelante desea establecer unidades de escala en 0, primero debe deshabilitar la integración de CDN. De forma predeterminada, cuando se crea un nuevo extremo de streaming, automáticamente se establece una unidad de streaming.

- El extremo de streaming debe estar en un estado detenido. Cuando se habilita CDN, puede iniciar el extremo de streaming.

Es posible que la integración de CDN de Azure tarde en habilitarse hasta 90 minutos. Se tarda hasta dos horas en activar los cambios en todos los POP de la red CDN.

La integración de red CDN está habilitada en todos los centros de datos de Azure: oeste de EE. UU., este de EE. UU., Europa del Norte, Europa Occidental, Japón Occidental, Japón Oriental, Asia Suroriental y Asia Oriental.

Una vez habilitada, se deshabilita la configuración de **Control de acceso**.

![Extremo de streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] La integración de Servicios multimedia de Azure con la red CDN de Azure se implementa en la **red CDN de Azure de Verizon**. Si quiere usar la **red CDN de Azure de Akamai** para Servicios multimedia de Azure, debe [configurar el punto de conexión manualmente](../cdn/cdn-create-new-endpoint.md). Para obtener más información sobre las características de la red CDN de Azure, consulte la [información general de la red CDN](../cdn/cdn-overview.md).

###Consideraciones adicionales

- Cuando la red CDN está habilitada para un extremo de streaming, los clientes no pueden solicitar contenido directamente desde el origen. Si necesita poder probar el contenido con o sin red CDN, puede crear otro punto de conexión de streaming que no tenga habilitada la red CDN.
- El nombre de host del extremo de streaming permanece igual después de habilitar la red CDN. No es necesario realizar ningún cambio en el flujo de trabajo de Servicios multimedia una vez habilitada la red CDN. Por ejemplo, si el nombre de host del extremo de streaming es strasbourg.streaming.mediaservices.windows.net, después de habilitar la red CDN se usa exactamente el mismo nombre de host.
- Para los nuevos puntos de conexión de streaming, puede habilitar la red CDN creando un nuevo punto de conexión; para los puntos de conexión de streaming existentes, deberá detener primero el punto de conexión y, después, habilitar la red CDN.
 

Para obtener más información, consulte [Anuncio de la integración de Servicios multimedia de Azure con la red CDN de Azure (red de entrega de contenido)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##Pasos siguientes

Consulte las rutas de aprendizaje de Servicios multimedia.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Envío de comentarios

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 

<!---HONumber=AcomDC_0831_2016-->