<properties
   pageTitle="Uso de la aplicación de API de JavaScript en una aplicación lógica | Microsoft Azure"
   description="Conector o aplicación de API de JavaScript"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# Aplicación de API de JavaScript

>[AZURE.NOTE] Esta versión del artículo se aplica a la versión de esquema 2014-12-01-preview de aplicaciones lógicas.

La aplicación de API de JavaScript proporciona una manera sencilla de ejecutar expresiones sencillas de JavaScript *mientras se ejecuta una aplicación lógica*.

## ¿Cuándo debe usar esta aplicación de API?
El escenario clave para el uso de esta aplicación de API es cuando quiere que el ciclo de vida del código que escribe sea el mismo que el de la aplicación lógica, y *no* quiere que se llame al código en otros escenarios.

Por otro lado, si desea un fragmento de código reutilizable que tenga un ciclo de vida independiente de la aplicación lógica, debe usar la aplicación de API de WebJobs para crear expresiones de código simples y llamarlas desde la aplicación lógica.

Por último, si desea incluir paquetes adicionales, también necesita usar la aplicación de API de WebJobs, ya que no puede agregar bibliotecas mediante la aplicación de API de JavaScript.

Utilice la [aplicación de API de C#](app-service-logic-cs-api.md) si prefiere escribir las expresiones en C#.

## Creación de una aplicación de API de JavaScript
Para usar la aplicación de API de JavaScript, deberá crear primero una instancia de ella. Esta tarea puede realizarse en línea mediante la creación de una aplicación lógica, o bien seleccionando la aplicación de API en JavaScript Azure Marketplace.

## Uso de la aplicación de API de JavaScript en la superficie del diseñador de aplicaciones lógicas
### Desencadenador
Puede crear un desencadenador que el servicio de Aplicaciones lógicas sondee (en el intervalo que defina) y, si devuelve cualquier otro contenido, la aplicación lógica se ejecuta. De lo contrario, espera hasta el siguiente intervalo de sondeo para volver a comprobarlo.

Las entradas para el desencadenador son las siguientes:
- **Expresión de JavaScript**: una expresión que se evalúa. Se invoca dentro de una función y debe devolver `false` cuando no desee que se ejecute la aplicación lógica, mientras que podría devolver cualquier otro valor cuando sí desee su ejecución. Puede usar el contenido de la respuesta en las acciones de la aplicación lógica.
- **Objeto de contexto**: un objeto opcional que se puede pasar al desencadenador. Puede definir tantas propiedades como desee, pero la entidad de nivel superior debe ser un objeto, por ejemplo, `{ "bar" : 0}`.

Por ejemplo, podría disponer de un desencadenador simple que solo ejecute la aplicación lógica entre los 15 y los 30 minutos de cada hora:

```
var d = new Date(); return (d.getMinutes() > 15) && (d.getMinutes() < 30);
```

### Acción

Del mismo modo, puede proporcionar la acción que desea ejecutar.

Las entradas para la acción son las siguientes:
- **Expresión de JavaScript**: una expresión que se evalúa. Debe incluir la instrucción `return` para obtener cualquier contenido.
- **Objeto de contexto**: un objeto opcional que se puede pasar al desencadenador. Puede definir tantas propiedades como desee, pero la entidad de nivel superior debe ser un objeto, por ejemplo, `{ "bar" : 0}`.

Por ejemplo, imagine que utiliza el desencadenador de Office 365 **Nuevo correo electrónico**. Que devuelve el siguiente objeto:
```
{
	...
	"Attachments" : [
		{
			"name" : "Picture.png",
			"content" : {
				"ContentData" : "iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFAQMAAAC3obSmAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAGUExURf///wAAAFXC034AAAASSURBVAjXY2BgCGBgYOhgKAAABEIBSWDJEbYAAAAASUVORK5CYII=",
				"ContentType" : "image/png",
				"ContentTransferEncoding" : "Base64"
			}
		},	
		{
			"name" : "File.txt",
			"content" : {
				"ContentData" : "Don't worry, be happy!",
				"ContentType" : "text/plain",
				"ContentTransferEncoding" : "None"
			}
		}	
	]
}
```

Sin embargo, desea cargar estos archivos adjuntos en una publicación de Yammer. Desafortunadamente, el esquema para los datos adjuntos de Yammer es ligeramente diferente. Ahora, puede analizar esto dentro de la aplicación lógica. Para el objeto de contexto, solo pase: `@triggerBody()`. Para la expresión, pase:

```
return Attachments.map(function(obj){var a = obj.Content; a.FileName = obj.Name; return a;})
```

La acción devuelve el JSON que devolvió a partir de la función. Por lo tanto, en la aplicación de API de Yammer, puede hacer referencia a `@body('javascriptapi')` para la propiedad **Datos adjuntos**.

## Aplicaciones adicionales del conector
Después de crear el conector, puede agregarlo a un flujo empresarial mediante una aplicación lógica. Consulte [¿Qué son las aplicaciones lógicas?](app-service-logic-what-are-logic-apps.md)

 

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md

<!---HONumber=AcomDC_0907_2016-->