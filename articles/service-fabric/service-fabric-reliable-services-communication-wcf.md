---
title: Güvenilir Hizmetler WCF iletişim yığını
description: Service Fabric'deki yerleşik WCF iletişim yığını, Güvenilir Hizmetler için istemci hizmeti WCF iletişimi sağlar.
author: BharatNarasimman
ms.topic: conceptual
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: 7f3b3974893316a488270f755b8f8822080658d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75433857"
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a>Güvenilir Hizmetler için WCF tabanlı iletişim yığını
Güvenilir Hizmetler çerçevesi, hizmet yazarlarının hizmetleri için kullanmak istedikleri iletişim yığınını seçmelerine olanak tanır. [CreateServiceReplicaListeners veya CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) yöntemlerinden döndürülen **ICommunicationListener** aracılığıyla istedikleri iletişim yığınını takabilirler. Çerçeve, WCF tabanlı iletişimi kullanmak isteyen hizmet yazarları için Windows Communication Foundation (WCF) tabanlı iletişim yığınının uygulanmasını sağlar.

## <a name="wcf-communication-listener"></a>WCF İletişim Dinleyicisi
**ICommunicationListener'ın** WCF'ye özel uygulaması **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** sınıfı tarafından sağlanmaktadır.

Diyelim ki bir hizmet sözleşmemiz var.`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Hizmette aşağıdaki şekilde bir WCF iletişim dinleyicisi oluşturabiliriz.

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

## <a name="writing-clients-for-the-wcf-communication-stack"></a>WCF iletişim yığını için istemci yazma
WCF kullanarak hizmetleri ile iletişim kurmak için istemciler yazma için, çerçeve **WcfClientCommunicationFactory**sağlar , [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md)WCF özel uygulamasıdır.

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

WCF iletişim kanalına **WcfCommunicationClient Tarafından**oluşturulan **WcfCommunicationClient'dan** erişilebilir.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

İstemci kodu, hizmet bitiş noktasını belirlemek ve hizmetle iletişim kurmak için **ServicePartitionClient'ı** uygulayan **WcfCommunicationClient** ile birlikte **WcfCommunicationClient'ı** kullanabilir.

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
> Varsayılan ServicePartitionResolver istemci hizmet olarak aynı kümede çalıştığını varsayar. Bu durumda değilse, bir ServicePartitionResolver nesnesi oluşturun ve küme bağlantısı uç noktalarında geçirin.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Hizmetler remoting ile uzaktan yordam çağrısı](service-fabric-reliable-services-communication-remoting.md)
* [Güvenilir Hizmetlerde OWIN ile Web API](service-fabric-reliable-services-communication-webapi.md)
* [Güvenilir Hizmetler için iletişimin sağlanması](service-fabric-reliable-services-secure-communication-wcf.md)

