---
title: Azure Hizmet Kumaşında Java kullanarak hizmet remotingi
description: Service Fabric remoting, istemcilerin ve hizmetlerin uzaktan yordam çağrısı kullanarak Java hizmetleriyle iletişim kurmasını sağlar.
author: PavanKunapareddyMSFT
ms.topic: conceptual
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: eef63d7a2c8a4b15938dfbffd7db5f9d1b22d426
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426631"
---
# <a name="service-remoting-in-java-with-reliable-services"></a>Güvenilir Hizmetlerle Java'da Hizmet Remoting
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-communication-remoting.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-communication-remoting-java.md)
>
>

WebAPI, Windows Communication Foundation (WCF) veya diğerleri gibi belirli bir iletişim protokolüne veya yığınına bağlı olmayan hizmetler için Güvenilir Hizmetler çerçevesi, uzaktan yordam çağrılarının hızlı ve kolay bir şekilde ayarlanması için bir yeniden moting mekanizması sağlar Hizmetleri.  Bu makalede, Java ile yazılmış hizmetler için uzaktan yordam çağrılarının nasıl ayarlır.

## <a name="set-up-remoting-on-a-service"></a>Bir hizmette remoting ayarlama
Bir hizmet için remoting ayarlama iki basit adımda yapılır:

1. Hizmetinizin uygulayacağı bir arayüz oluşturun. Bu arabirim, hizmetinizde uzaktan yordam çağrısı için kullanılabilen yöntemleri tanımlar. Yöntemler görev döndüren eşzamanlı yöntemler olmalıdır. Arabirimin, `microsoft.serviceFabric.services.remoting.Service` hizmetin bir remoting arabirimi olduğunu bildirmek için uygulaması gerekir.
2. Hizmetinizde bir remoting dinleyici kullanın. Bu, `CommunicationListener` remoting yetenekleri sağlayan bir uygulamadır. `FabricTransportServiceRemotingListener`varsayılan remoting aktarım protokolünü kullanarak bir remoting dinleyici oluşturmak için kullanılabilir.

Örneğin, aşağıdaki stateless hizmeti uzak bir yordam çağrısı üzerinden "Hello World" almak için tek bir yöntem ortaya çıkarır.

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> Hizmet arabirimindeki bağımsız değişkenler ve iade türleri basit, karmaşık veya özel türler olabilir, ancak serileştirilebilir olmalıdır.
>
>

## <a name="call-remote-service-methods"></a>Uzaktan servis yöntemlerini arayın
Remoting yığınını kullanarak bir hizmetteki arama yöntemlerini `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` sınıf aracılığıyla hizmete yerel bir proxy kullanılarak yapılır. Yöntem, `ServiceProxyBase` hizmetin uyguladığı aynı arabirimi kullanarak yerel bir proxy oluşturur. Bu proxy ile, sadece uzaktan arayüzü yöntemleri arayabilirsiniz.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Remoting çerçevesi, hizmette atılan özel durumları istemciye yayıltır. Bu nedenle istemcide özel durum `ServiceProxyBase` işleme mantığı nı kullanarak hizmetin attığı özel durumları doğrudan işleyebilir.

## <a name="service-proxy-lifetime"></a>Hizmet Proxy Ömür Boyu
ServiceProxy oluşturma hafif bir işlemdir, böylece istediğiniz kadar çok oluşturabilirsiniz. Hizmet Proxy örnekleri, gerekli oldukları sürece yeniden kullanılabilir. Uzak yordam çağrısı bir Özel Durum atarsa, yine de aynı proxy örneğini yeniden kullanabilirsiniz. Her ServiceProxy, iletileri kablo üzerinden göndermek için kullanılan bir iletişim istemcisi içerir. Uzaktan arama çağırırken, iletişim istemcisinin geçerli olup olmadığını belirlemek için iç denetimler gerçekleştirilir. Bu denetimlerin sonuçlarına bağlı olarak, gerekirse iletişim istemcisi yeniden oluşturulur. Bu nedenle, bir özel durum oluşursa, `ServiceProxy`yeniden oluşturmanız gerekmez.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory Ömür Boyu
[FabricServiceProxyFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client.fabricserviceproxyfactory) farklı remoting arayüzleri için proxy oluşturan bir fabrikadır. Proxy oluşturmak için `ServiceProxyBase.create` API kullanıyorsanız, çerçeve `FabricServiceProxyFactory`bir .
[ServiceRemotingClientFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client.serviceremotingclientfactory) özelliklerini geçersiz kılmanız gerektiğinde el ile bir tane oluşturmak yararlıdır.
Fabrika pahalı bir operasyondur. `FabricServiceProxyFactory`iletişim istemcilerinin önbelleğini korur.
En iyi yöntem, `FabricServiceProxyFactory` mümkün olduğunca uzun süre önbelleğe almaktır.

## <a name="remoting-exception-handling"></a>Özel Durum İşleme
Hizmet API tarafından atılan tüm uzak özel durum, RuntimeException veya FabricException olarak istemciye geri gönderilir.

ServiceProxy, oluşturulduğu hizmet bölümü için tüm Failover Exception'ı işler. Failover Exceptions (Geçici Olmayan Özel Durumlar) varsa uç noktaları yeniden çözer ve aramayı doğru bitiş noktasıyla yeniden dener. Başarısız Özel Durum için yeniden deneme sayısı belirsizdir.
Geçici Özel Durumlar durumunda, yalnızca aramayı yeniden dener.

Varsayılan yeniden deneme parametreleri [OperationRetrySettings](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.communication.client.operationretrysettings)tarafından provied.
Bu değerleri OperationRetrySettings nesnesini ServiceProxyFactory oluşturucuya geçirerek yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Hizmetler için iletişimin sağlanması](service-fabric-reliable-services-secure-communication-java.md)
