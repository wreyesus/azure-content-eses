<properties
   pageTitle="Pila comunicación de WCF de Reliable Services | Microsoft Azure"
   description="La pila de comunicación de WCF integrada en Service Fabric ofrece comunicación de WCF del servicio de cliente para Reliable Services."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# Pila de comunicación basada en WCF de Reliable Services
El marco de Reliable Services permite a los autores de servicio elegir la pila de comunicación que desean usar para su servicio. Pueden conectar la pila de comunicaciones que deseen mediante la clase **ICommunicationListener** devuelta desde los métodos [CreateServiceReplicaListeners o CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md). El marco proporciona una implementación de la pila de comunicación basada en Windows Communication Foundation (WCF) para los autores de servicio que desean usar la comunicación basada en WCF.

## Agente de escucha de comunicación WCF
La implementación específica de WCF de **ICommunicationListener** la proporciona la clase **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener**.

Pongamos que tenemos un contrato de servicio del tipo `ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Podemos crear un agente de escucha de comunicación de WCF en el servicio de la siguiente manera.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## Escritura de clientes para la pila de comunicación de WCF
Para que los clientes de escritura se comuniquen con los servicios mediante WCF, el marco proporciona la clase **WcfClientCommunicationFactory**, que es la implementación específica de WCF de la clase [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Se puede acceder al canal de comunicación de WCF desde la clase **WcfCommunicationClient** creada por la clase **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

El código de cliente puede usar la clase **WcfCommunicationClientFactory** junto con **ServicePartitionClient** que implementa **ServicePartitionClient** para determinar el punto de conexión de servicio y comunicarse con el servicio.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] ServicePartitionResolver asume que el cliente se ejecuta en el mismo clúster que el servicio. Si no es así, cree un objeto ServicePartitionResolver y transfiera los puntos de conexión del clúster.

## Pasos siguientes
* [Llamada a procedimiento remoto con la comunicación remota de Reliable Services](service-fabric-reliable-services-communication-remoting.md)

* [Web API con OWIN en Reliable Services](service-fabric-reliable-services-communication-webapi.md)

* [Protección de la comunicación para Reliable Services](service-fabric-reliable-services-secure-communication.md)

<!---HONumber=AcomDC_0727_2016-->