<properties
	pageTitle="Introducción a Azure Mobile Engagement para iOS en Objective C | Microsoft Azure"
	description="Aprenda a usar Azure Mobile Engagement con los análisis y las notificaciones push en aplicaciones iOS."
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-ios"
	ms.devlang="objective-c"
	ms.topic="hero-article"
	ms.date="09/14/2016"
	ms.author="piyushjo" />

# Introducción a Azure Mobile Engagement para aplicaciones iOS en Objective C

[AZURE.INCLUDE [Modificador del tutorial de imagen prominente](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

En este tema se muestra cómo utilizar Azure Mobile Engagement para conocer el uso de las aplicaciones y enviar notificaciones push a usuarios segmentados en una aplicación de iOS. En este tutorial, puede crear una aplicación iOS vacía que recopile datos básicos y reciba notificaciones push mediante el sistema de notificaciones push de Apple (APN).

Este tutorial requiere lo siguiente:

+ XCode 8, que se puede instalar desde la tienda de aplicaciones para Mac
+ [SDK de Mobile Engagement iOS]

Completar este tutorial es un requisito previo para todos los tutoriales de Mobile Engagement para aplicaciones iOS.

> [AZURE.NOTE] Para completar este tutorial, deberá tener una cuenta de Azure activa. En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fes-ES%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Configuración de Mobile Engagement para una aplicación iOS

[AZURE.INCLUDE [Creación de la aplicación de Mobile Engagement en el portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Conectar la aplicación al backend de Mobile Engagement

En este tutorial se presenta una "integración básica", que es el conjunto mínimo necesario para recopilar los datos y enviar una notificación de inserción. Toda la información sobre la integración se encuentra en la [Integración del SDK de Mobile Engagement para iOS](mobile-engagement-ios-sdk-overview.md)

Crearemos una aplicación básica con XCode para demostrar la integración:

###Creación de un nuevo proyecto iOS

[AZURE.INCLUDE [Creación de un nuevo proyecto iOS](../../includes/mobile-engagement-create-new-ios-app.md)]

###Conectar la aplicación al backend de Mobile Engagement

1. Descargue el [SDK de iOS para Mobile Engagement].
2. Extraiga el archivo .tar.gz en una carpeta del equipo.
3. Haga clic con el botón derecho en el proyecto y luego seleccione **Agregar archivos a**.

	![][1]

4. Navegue hasta la carpeta donde extrajo el SDK, seleccione la carpeta `EngagementSDK` y luego haga clic en **Aceptar**.

	![][2]

5. Abra la pestaña **Fases de compilación** y, en el menú **Vincular binarios con bibliotecas**, agregue los marcos de trabajo, como se muestra a continuación:

	![][3]

6. Vuelva al Portal de Azure en la página **Información de conexión** de la aplicación y copie la cadena de conexión.

	![][4]

7. Agregue la siguiente línea de código al archivo **AppDelegate.m**.

		#import "EngagementAgent.h"

8. Pegue la cadena de conexión en el delegado `didFinishLaunchingWithOptions`.

		- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
		{
  			[...]   
  			[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
  			[...]
		}

9. `setTestLogEnabled` es una instrucción opcional que habilita los registros del SDK para identificar problemas.

##<a id="monitor"></a>Habilitación de la supervisión en tiempo real

Para comenzar a enviar datos y asegurarse de que los usuarios estén activos, debe enviar al menos una pantalla (Actividad) al back-end de Mobile Engagement.

1. Abra el archivo **ViewController.h** e importe **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Reemplace ahora la superclase de la interfaz de **ViewController** por `EngagementViewController`:

	`@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Conexión de la aplicación con la supervisión en tiempo real

[AZURE.INCLUDE [Conectar la aplicación con la supervisión en tiempo real](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Habilitación de las notificaciones push y la mensajería en aplicación

Mobile Engagement permite interactuar y llegar mediante notificaciones push y mensajería en la aplicación en el contexto de las campañas. Este módulo se denomina REACH en el portal de Mobile Engagement. En las secciones siguientes se instala la aplicación para recibirlos.

### Habilitación de la aplicación para recibir notificaciones push silenciosas

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### Adición de la biblioteca de cobertura al proyecto

1. Haga clic con el botón derecho en el proyecto.
2. Seleccione **Agregar archivo a**.
3. Navegue hasta la carpeta donde extrajo el SDK.
4. Seleccione la carpeta `EngagementReach`.
5. Haga clic en **Agregar**.

### Modifique su delegado de la aplicación

1. En el archivo **AppDeletegate.m**, importe el módulo Engagement Reach.

		#import "AEReachModule.h"
		#import <UserNotifications/UserNotifications.h>

2. Dentro del método `application:didFinishLaunchingWithOptions`, cree un módulo de Reach y páselo a la línea de inicialización de Engagement existente:

		- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
			AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
			[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
			[...]
			return YES;
		}

###Habilite su aplicación para recibir notificaciones push de APN.

1. Agregue la siguiente línea al método `application:didFinishLaunchingWithOptions`:

		if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
		{
			if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
			{
				[UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
			}else
			{
				[application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
			}
			[application registerForRemoteNotifications];
		}
		else
		{
			[application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
		}

2. Agregue el método `application:didRegisterForRemoteNotificationsWithDeviceToken` de la siguiente forma:

		- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
		{
 			[[EngagementAgent shared] registerDeviceToken:deviceToken];
			NSLog(@"Registered Token: %@", deviceToken);
		}

3. Agregue el método `didFailToRegisterForRemoteNotificationsWithError` de la siguiente forma:

		- (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
		{
		   
		   NSLog(@"Failed to get token, error: %@", error);
		}

4. Agregue el método `didReceiveRemoteNotification:fetchCompletionHandler` de la siguiente forma:

		- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
		{
			[[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
		}

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[SDK de Mobile Engagement iOS]: http://aka.ms/qk2rnj
[SDK de iOS para Mobile Engagement]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

<!---HONumber=AcomDC_0928_2016-->