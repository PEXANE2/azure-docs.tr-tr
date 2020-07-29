---
title: Azure 'da Java kullanarak hizmet uzaktan iletişim Service Fabric
description: Service Fabric uzaktan iletişim, istemcilerin ve hizmetlerin uzak yordam çağrısı kullanarak Java hizmetleriyle iletişim kurmasına olanak tanır.
author: PavanKunapareddyMSFT
ms.topic: conceptual
ms.date: 06/30/2017
ms.custom: devx-track-java
ms.author: pakunapa
ms.openlocfilehash: d53d20510db70d81aab796efab48de40c880bb3a
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87316133"
---
# <a name="service-remoting-in-java-with-reliable-services"></a>Reliable Services ile Java 'da hizmet uzaktan iletişimi
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-communication-remoting.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Belirli bir iletişim protokolüne veya örneğin, WebAPI, Windows Communication Foundation (WCF) veya diğer bir yığına bağlı olmayan hizmetler için, Reliable Services Framework Hizmetleri için uzak yordam çağrılarını hızlı ve kolay bir şekilde ayarlamak için bir uzaktan iletişim mekanizması sağlar.  Bu makalede, Java ile yazılmış hizmetler için uzak yordam çağrılarının nasıl ayarlanacağı açıklanır.

## <a name="set-up-remoting-on-a-service"></a>Bir hizmette uzaktan iletişim kurma
Bir hizmet için uzaktan iletişim kurulması iki basit adımda yapılır:

1. Hizmetinizin uygulanması için bir arabirim oluşturun. Bu arabirim, hizmetinize bir uzak yordam çağrısı için kullanılabilen yöntemleri tanımlar. Yöntemler görev döndüren zaman uyumsuz yöntemler olmalıdır. Arabirimin, `microsoft.serviceFabric.services.remoting.Service` hizmetin uzaktan iletişim arabirimine sahip olduğunu bildirmek için uygulaması gerekir.
2. Hizmetinizde uzaktan iletişim dinleyicisi kullanın. Bu, `CommunicationListener` Uzaktan iletişim özellikleri sağlayan bir uygulamasıdır. `FabricTransportServiceRemotingListener`, varsayılan uzaktan iletişim aktarım protokolü kullanılarak uzaktan iletişim dinleyicisi oluşturmak için kullanılabilir.

Örneğin, aşağıdaki durum bilgisi olmayan hizmet bir uzak yordam çağrısı üzerinden "Merhaba Dünya" almak için tek bir yöntem sunar.

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
> Hizmet arabirimindeki bağımsız değişkenler ve dönüş türleri herhangi bir basit, karmaşık veya özel tür olabilir, ancak seri hale getirilebilir olmaları gerekir.
>
>

## <a name="call-remote-service-methods"></a>Uzak hizmet yöntemlerini çağırma
Uzaktan iletişim yığınını kullanarak bir hizmette yöntemleri çağırmak, sınıfı aracılığıyla hizmete yerel bir proxy kullanılarak yapılır `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` . `ServiceProxyBase`Yöntemi, hizmetin uyguladığı arabirimi kullanarak bir yerel ara sunucu oluşturur. Bu proxy ile, yalnızca arabirimdeki yöntemleri uzaktan çağırabilirsiniz.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Remoting Framework, hizmette oluşturulan özel durumları istemciye yayar. Bu nedenle, kullanarak istemcideki özel durum işleme mantığı `ServiceProxyBase` , hizmetin oluşturduğu özel durumları doğrudan işleyebilir.

## <a name="service-proxy-lifetime"></a>Hizmet proxy 'Si ömrü
ServiceProxy oluşturma hafif bir işlemdir, bu sayede ihtiyacınız olan kadar oluşturabilirsiniz. Hizmet proxy örnekleri, gerektiği sürece yeniden kullanılabilir. Bir uzak yordam çağrısı bir özel durum oluşturursa, yine de aynı proxy örneğini yeniden kullanabilirsiniz. Her ServiceProxy, iletileri kablo üzerinden göndermek için kullanılan bir iletişim istemcisi içerir. Uzaktan aramalar çağrılırken, iletişim istemcisinin geçerli olup olmadığını anlamak için iç denetimler gerçekleştirilir. Bu denetimlerin sonuçlarına bağlı olarak, iletişim istemcisi gerektiğinde yeniden oluşturulur. Bu nedenle, bir özel durum oluşursa, yeniden oluşturmanız gerekmez `ServiceProxy` .

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory ömrü
[Fabricserviceproxyfactory](/java/api/microsoft.servicefabric.services.remoting.client.fabricserviceproxyfactory) , farklı uzaktan iletişim arabirimleri için ara sunucu oluşturan bir fabrikadır. `ServiceProxyBase.create`Proxy oluşturmak IÇIN API kullanıyorsanız, Framework bir oluşturur `FabricServiceProxyFactory` .
[Serviceremotingclientfactory](/java/api/microsoft.servicefabric.services.remoting.client.serviceremotingclientfactory) özelliklerini geçersiz kılmanız gerektiğinde el ile bir tane oluşturmak yararlı olur.
Fabrika, pahalı bir işlemdir. `FabricServiceProxyFactory`iletişim istemcilerinin önbelleğini korur.
En iyi yöntem, mümkün olduğunca uzun süre önbelleğe alma yöntemidir `FabricServiceProxyFactory` .

## <a name="remoting-exception-handling"></a>Uzaktan iletişim özel durum Işleme
Service API tarafından oluşturulan tüm uzak özel durum, istemciye RuntimeException veya FabricException olarak geri gönderilir.

ServiceProxy, için oluşturulduğu hizmet bölümünün tüm yük devretme özel durumunu işler. Yük devretme özel durumları (geçici olmayan özel durumlar) varsa uç noktaları yeniden çözer ve çağrıyı doğru uç noktayla yeniden dener. Yük devretme özel durumu için yeniden deneme sayısı sonsuz.
Geçişli Entexcepler söz konusu olduğunda yalnızca çağrıyı yeniden dener.

Varsayılan yeniden deneme parametreleri, [Operationretrysettings](/java/api/microsoft.servicefabric.services.communication.client.operationretrysettings)tarafından kısımları.
Bu değerleri, OperationRetrySettings nesnesini ServiceProxyFactory oluşturucusuna geçirerek yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services için iletişimin güvenliğini sağlama](service-fabric-reliable-services-secure-communication-java.md)
