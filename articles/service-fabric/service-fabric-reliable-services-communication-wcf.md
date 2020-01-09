---
title: Reliable Services WCF iletişim yığını
description: Service Fabric 'daki yerleşik WCF iletişim yığını, Reliable Services için istemci hizmeti WCF iletişimi sağlar.
author: BharatNarasimman
ms.topic: conceptual
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: 7f3b3974893316a488270f755b8f8822080658d9
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75433857"
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a>Reliable Services için WCF tabanlı iletişim yığını
Reliable Services Framework, hizmet yazarlarının kendi hizmetleri için kullanmak istedikleri iletişim yığınını seçmesine olanak sağlar. [CreateServiceReplicaListeners veya Createserviceınstancelisteners](service-fabric-reliable-services-communication.md) metotlarından döndürülen **ıcommunicationlistener** aracılığıyla seçtikleri iletişim yığınını yükleyebilir. Framework, WCF tabanlı iletişim kullanmak isteyen hizmet yazarları için Windows Communication Foundation (WCF) temelinde iletişim yığınının bir uygulamasını sağlar.

## <a name="wcf-communication-listener"></a>WCF Iletişim dinleyicisi
**Idimmunicationlistener** 'ın WCF 'e özgü uygulanması **Microsoft. Servicefabric. Services. Communication. WCF. Runtime. wcfcommunicationlistener** sınıfı tarafından sağlanır.

En uzun, `ICalculator` türünde bir hizmet sözleşmemiz var

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Hizmette aşağıdaki şekilde bir WCF iletişim dinleyicisi oluşturarız.

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

## <a name="writing-clients-for-the-wcf-communication-stack"></a>WCF iletişim yığını için istemciler yazma
İstemcileri WCF kullanarak hizmetlerle iletişim kuracak şekilde yazmak için **Framework,** [Clientcommunicationfactorybase](service-fabric-reliable-services-communication.md)'in WCF 'e özgü uygulamasıdır.

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

WCF iletişim kanalına **Wcfcommunicationclientfactory**tarafından oluşturulan **Wcfcommunicationclient** üzerinden erişilebilir.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

İstemci kodu, hizmet uç noktasını belirleyebilmek ve hizmetle iletişim kurmak için **Servicepartitionclient** uygulayan **Wcfcommunicationclient** Ile **Wcfcommunicationclientfactory** ' i kullanabilir.

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
> [!NOTE]
> Varsayılan ServicePartitionResolver, istemcinin hizmet olarak aynı kümede çalıştığını varsayar. Böyle bir durum söz konusu değilse, bir ServicePartitionResolver nesnesi oluşturun ve küme bağlantı uç noktalarında geçiş yapın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services uzaktan iletişim ile uzak yordam çağrısı](service-fabric-reliable-services-communication-remoting.md)
* [Reliable Services 'de OWIN ile Web API 'SI](service-fabric-reliable-services-communication-webapi.md)
* [Reliable Services için iletişimin güvenliğini sağlama](service-fabric-reliable-services-secure-communication-wcf.md)

