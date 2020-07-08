---
title: Azure Service Fabric ile bağlanma ve hizmetlerle iletişim kurma
description: Service Fabric 'de Hizmetleri çözmeyi, bağlamayı ve iletişim kurmayı öğrenin.
author: vturecek
ms.topic: conceptual
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: e57d169decf482f8b8be1e3b31a07690bc222c5d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75458235"
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Service Fabric ile bağlanma ve hizmetlerle iletişim kurma
Service Fabric, bir hizmet, genellikle birden çok VM arasında dağıtılan bir Service Fabric kümesinde bir yerde çalışır. Hizmet sahibine veya Service Fabric göre otomatik olarak bir yerden diğerine taşınabilir. Hizmetler, belirli bir makineye veya adrese statik olarak bağlı değildir.

Service Fabric uygulama genellikle her hizmetin özelleşmiş bir görev gerçekleştirdiği birçok farklı hizmetten oluşur. Bu hizmetler, bir Web uygulamasının farklı parçalarını işlemek gibi bir bütün işlev oluşturmak için birbirleriyle iletişim kurabilir. Ayrıca, hizmetlere bağlanan ve hizmetleriyle iletişim kuran istemci uygulamaları da vardır. Bu belgede, Service Fabric Hizmetleri ile ve arasında iletişimin nasıl ayarlanacağı açıklanır.

## <a name="bring-your-own-protocol"></a>Kendi protokolünü getir
Service Fabric, hizmetlerinizin yaşam döngüsünün yönetilmesine yardımcı olur, ancak hizmetleriniz ne işe yarar konusunda kararlar vermez. Buna iletişim dahildir. Hizmetiniz Service Fabric tarafından açıldığında, sizin istediğiniz protokol veya iletişim yığınını kullanarak gelen istekler için bir uç nokta ayarlama fırsatıdır. Hizmetiniz, bir URI gibi herhangi bir adresleme şeması kullanarak normal **IP: bağlantı noktası** adresini dinler. Birden çok hizmet örneği veya çoğaltması bir konak işlemini paylaşabilir, bu durumda farklı bağlantı noktaları kullanmaları veya Windows 'da http.sys çekirdek sürücüsü gibi bir bağlantı noktası paylaşım mekanizması kullanması gerekir. Her iki durumda da, bir konak işlemindeki her bir hizmet örneği veya çoğaltma benzersiz şekilde adreslenebilir olmalıdır.

![hizmet uç noktaları][1]

## <a name="service-discovery-and-resolution"></a>Hizmet bulma ve çözümleme
Dağıtılmış bir sistemde, hizmetler bir makineden diğerine zaman içinde geçebilir. Bu, kaynak Dengeleme, yükseltmeler, yük devretme veya genişleme gibi çeşitli nedenlerden kaynaklanabilir. Bu, hizmet uç noktası adreslerinin farklı IP adreslerine sahip düğümlere taşındığı ve hizmet dinamik olarak seçilen bir bağlantı noktasını kullanıyorsa farklı bağlantı noktalarında açık olabileceği anlamına gelir.

![Hizmetlerin dağıtılması][7]

Service Fabric, Adlandırma Hizmeti adlı bir bulma ve çözümleme hizmeti sağlar. Adlandırma Hizmeti, adlandırılmış hizmet örneklerini dinlediği uç nokta adresleriyle eşleştiren bir tabloyu tutar. Service Fabric tüm adlandırılmış hizmet örnekleri, URI olarak temsil edilen benzersiz adlara sahiptir, örneğin `"fabric:/MyApplication/MyService"` . Hizmetin adı, hizmetin kullanım ömrü boyunca değişmez ve yalnızca hizmetler taşındığında değiştirebileceğinizi belirten uç nokta adresleridir. Bu, sabit URL 'Leri olan, ancak IP adresinin değiştirebildiği Web sitelerine benzerdir. Web sitesi URL 'Lerini IP adreslerine çözümleyen ve Web 'de DNS 'ye benzer şekilde, Service Fabric hizmet adlarını uç nokta adresleriyle eşleyen bir kaydedici vardır.

![hizmet uç noktaları][2]

Hizmetlere çözümleme ve bağlanma, aşağıdaki adımları bir döngüde çalıştırmayı içerir:

* **Çözümle**: bir hizmetin adlandırma hizmeti yayımladığı uç noktayı alın.
* **Bağlan**: hizmete, bu uç noktada kullandığı herhangi bir protokol üzerinden bağlanın.
* **Yeniden deneme**: bir bağlantı girişimi herhangi bir sayıda nedenden dolayı başarısız olabilir, örneğin, uç nokta adresinin çözümlenmesinden bu yana hizmet taşındığında. Bu durumda, önceki Resolve ve Connect adımlarının yeniden denenmesi gerekir ve bu döngüyle bağlantı başarılı olana kadar yinelenir.

## <a name="connecting-to-other-services"></a>Diğer hizmetlere bağlanma
Bir kümedeki düğümler aynı yerel ağda olduğundan, bir küme içindeki birbirlerine bağlanan hizmetler genellikle diğer hizmetlerin uç noktalarına erişebilir. Hizmetler arasında bağlantı kurmak daha kolay hale getirmek için Service Fabric Adlandırma Hizmeti kullanan ek hizmetler sağlar. Bir DNS hizmeti ve ters proxy hizmeti.


### <a name="dns-service"></a>DNS hizmeti
Birçok hizmet, özellikle Kapsayıcılı hizmetler, var olan bir URL adına sahip olduğundan, bu işlemi standart DNS Protokolü (Adlandırma Hizmeti protokolü yerine) kullanarak çözebiliyor, özellikle uygulamanın "yükseltme ve kaydırma" senaryolarında çok kullanışlı hale getirilir. Bu, tam olarak DNS hizmetinin yaptığı şeydir. DNS adlarını bir hizmet adıyla eşlemenizi sağlar ve bu nedenle uç nokta IP adreslerini çözümleyebilir. 

Aşağıdaki diyagramda gösterildiği gibi, Service Fabric kümesinde çalışan DNS hizmeti, DNS adlarını, daha sonra bağlantı kurmak üzere uç nokta adreslerini döndürmek için Adlandırma Hizmeti tarafından çözümlenen hizmet adlarıyla eşler. Hizmetin DNS adı, oluşturma sırasında sağlanır. 

![hizmet uç noktaları][9]

DNS hizmetini kullanma hakkında daha fazla ayrıntı için bkz. [Azure Service Fabric 'de DNS hizmeti](service-fabric-dnsservice.md) .

### <a name="reverse-proxy-service"></a>Ters proxy hizmeti
Ters proxy, HTTPS de dahil olmak üzere HTTP uç noktalarını kullanıma sunan kümedeki hizmetleri ele alınmaktadır. Ters proxy, belirli bir URI biçimine sahip olan ve Adlandırma Hizmeti kullanarak bir hizmetin birbirleriyle iletişim kurması için gerekli olan çözümle, Bağlan, yeniden deneme adımlarını ele alarak diğer hizmetleri ve bunların yöntemlerini çağırmayı büyük ölçüde basitleştirir. Diğer bir deyişle, URL 'YI çağırmak kadar basit hale getirerek diğer hizmetleri çağırırken Adlandırma Hizmeti gizler.

![hizmet uç noktaları][10]

Ters proxy hizmetini kullanma hakkında daha fazla bilgi için bkz. [Azure 'Da ters proxy Service Fabric](service-fabric-reverseproxy.md) makalesi.

## <a name="connections-from-external-clients"></a>Dış istemcilerden gelen bağlantılar
Bir kümedeki düğümler aynı yerel ağda olduğundan, bir küme içindeki birbirlerine bağlanan hizmetler genellikle diğer hizmetlerin uç noktalarına erişebilir. Ancak bazı ortamlarda, küme, dış giriş trafiğini sınırlı bir bağlantı noktası kümesiyle yönlendiren bir yük dengeleyicinin arkasında olabilir. Bu durumlarda, hizmetler hala birbirleriyle iletişim kurabilir ve Adlandırma Hizmeti kullanarak adresleri çözümleyebilir, ancak dış istemcilerin hizmetlere bağlanmasına izin vermek için ek adımların alınması gerekir.

## <a name="service-fabric-in-azure"></a>Azure 'da Service Fabric
Azure 'daki bir Service Fabric kümesi bir Azure Load Balancer arkasına yerleştirilir. Kümenin tüm dış trafiği yük dengeleyiciden geçmelidir. Yük dengeleyici, belirli bir bağlantı noktası üzerinden gelen trafiği, aynı bağlantı noktası açık olan rastgele bir *düğüme* otomatik olarak iletecektir. Azure Load Balancer yalnızca *düğümlerde*açık olan bağlantı noktalarını bilir, bireysel *Hizmetler*tarafından açık olan bağlantı noktalarını bilmez.

![Azure Load Balancer ve Service Fabric topolojisi][3]

Örneğin, **80**numaralı bağlantı noktasında dış trafiği kabul etmek için aşağıdaki noktalar yapılandırılmalıdır:

1. 80 numaralı bağlantı noktasını dinleyen bir hizmet yazın. Hizmetin ServiceManifest.xml bağlantı noktası 80 ' ü yapılandırın ve hizmette bir dinleyici açın, örneğin, şirket içinde barındırılan bir Web sunucusu.

    ```xml
    <Resources>
        <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>
    ```
    ```csharp
        class HttpCommunicationListener : ICommunicationListener
        {
            ...

            public Task<string> OpenAsync(CancellationToken cancellationToken)
            {
                EndpointResourceDescription endpoint =
                    serviceContext.CodePackageActivationContext.GetEndpoint("WebEndpoint");

                string uriPrefix = $"{endpoint.Protocol}://+:{endpoint.Port}/myapp/";

                this.httpListener = new HttpListener();
                this.httpListener.Prefixes.Add(uriPrefix);
                this.httpListener.Start();

                string publishUri = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
                return Task.FromResult(publishUri);
            }

            ...
        }

        class WebService : StatelessService
        {
            ...

            protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
            {
                return new[] { new ServiceInstanceListener(context => new HttpCommunicationListener(context))};
            }

            ...
        }
    ```
    ```java
        class HttpCommunicationlistener implements CommunicationListener {
            ...

            @Override
            public CompletableFuture<String> openAsync(CancellationToken arg0) {
                EndpointResourceDescription endpoint =
                    this.serviceContext.getCodePackageActivationContext().getEndpoint("WebEndpoint");
                try {
                    HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress(endpoint.getPort()), 0);
                    server.start();

                    String publishUri = String.format("http://%s:%d/",
                        this.serviceContext.getNodeContext().getIpAddressOrFQDN(), endpoint.getPort());
                    return CompletableFuture.completedFuture(publishUri);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

            ...
        }

        class WebService extends StatelessService {
            ...

            @Override
            protected List<ServiceInstanceListener> createServiceInstanceListeners() {
                <ServiceInstanceListener> listeners = new ArrayList<ServiceInstanceListener>();
                listeners.add(new ServiceInstanceListener((context) -> new HttpCommunicationlistener(context)));
                return listeners;       
            }

            ...
        }
    ```
2. Azure 'da bir Service Fabric kümesi oluşturun ve hizmeti barındıracak düğüm türü için özel bir uç nokta bağlantı noktası olarak **80** numaralı bağlantı noktasını belirtin. Birden fazla düğüm türüne sahipseniz, yalnızca özel uç nokta bağlantı noktası açık olan düğüm türünde çalıştığından emin olmak için hizmette bir *yerleştirme kısıtlaması* ayarlayabilirsiniz.

    ![Düğüm türünde bir bağlantı noktası açma][4]
3. Küme oluşturulduktan sonra, bağlantı noktası 80 ' deki trafiği iletmek için kümenin kaynak grubundaki Azure Load Balancer yapılandırın. Azure portal aracılığıyla bir küme oluştururken, bu, yapılandırılmış olan her özel uç nokta bağlantı noktası için otomatik olarak ayarlanır.

    ![Azure Load Balancer trafiği iletme][5]
4. Azure Load Balancer belirli bir düğüme trafik gönderilmesi gerekip gerekmediğini anlamak için bir araştırma kullanır. Araştırma, düğümün yanıt verip vermediğini anlamak için her düğümdeki bir uç noktayı düzenli olarak denetler. Araştırma, yapılandırılan sayıda kez Yanıt alamadığında, yük dengeleyici bu düğüme trafik göndermeyi durduruyor. Azure portal aracılığıyla bir küme oluştururken, yapılandırılan her özel uç nokta bağlantı noktası için otomatik olarak bir araştırma ayarlanır.

    ![Azure Load Balancer trafiği iletme][8]

Azure Load Balancer ve araştırmanın düğümler üzerinde çalışan *Hizmetleri* değil, yalnızca *düğümleri*hakkında bilgi sahibi olduğunu unutmamak önemlidir. Azure Load Balancer her zaman, araştırmasına yanıt veren düğümlere trafik gönderir, bu nedenle araştırmayı yanıtlayabilecek düğümlerde hizmetlerin kullanılabilir olmasını sağlamak için dikkatli olunması gerekir.

## <a name="reliable-services-built-in-communication-api-options"></a>Reliable Services: yerleşik iletişim API 'SI seçenekleri
Reliable Services Framework önceden oluşturulmuş çeşitli iletişim seçenekleriyle birlikte gelir. Kendisi için en iyi çalışma kararı, programlama modelinin, iletişim çerçevesinin ve hizmetlerinizin yazıldığı programlama dilinin seçimine bağlıdır.

* **Özel protokol yok:**  Belirli bir iletişim çerçevesi seçeneğiniz yoksa, ancak hızlı bir şekilde çalışmaya başlayın, sonra, Reliable Services ve Reliable Actors için kesin olarak yazılmış uzak yordam çağrılarına izin veren [hizmet uzaktan iletişim](service-fabric-reliable-services-communication-remoting.md)kurun. Bu, hizmet iletişimini kullanmaya başlamanın en kolay ve en hızlı yoludur. Hizmet uzaktan iletişim hizmeti adresleri, bağlantı, yeniden deneme ve hata işleme çözümlemesini yönetir. Bu, hem C# hem de Java uygulamalarında kullanılabilir.
* **Http**: dilden bağımsız ILETIŞIM için http, her türlü Service Fabric tarafından desteklenen birçok farklı dilde araçlar ve http sunucularıyla sunulan endüstri standardı bir seçenek sunar. Hizmetler, C# uygulamaları için [ASP.NET Web API 'si](service-fabric-reliable-services-communication-webapi.md) de dahil olmak üzere kullanılabilir HERHANGI bir HTTP yığınını kullanabilir. C# dilinde yazılan istemciler `ICommunicationClient` ve `ServicePartitionClient` sınıflarından yararlanarak, Java için, `CommunicationClient` `FabricServicePartitionClient` [hizmet çözümlemesi, http bağlantıları ve yeniden deneme döngüleri için](service-fabric-reliable-services-communication.md)ve sınıflarını kullanın.
* **WCF**: iletişim çerçeve'niz olarak WCF kullanan mevcut kodunuz varsa, `WcfCommunicationListener` sunucu tarafı ve `WcfCommunicationClient` istemci sınıfları için öğesini kullanabilirsiniz `ServicePartitionClient` . Bu ancak yalnızca Windows tabanlı kümelerde C# uygulamaları için kullanılabilir. Daha fazla ayrıntı için, [iletişim yığınının WCF tabanlı uygulamasıyla](service-fabric-reliable-services-communication-wcf.md)ilgili bu makaleye bakın.

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Özel protokoller ve diğer iletişim çerçeveleri kullanma
Hizmetler, iletişim için herhangi bir protokol veya çerçeveyi, TCP yuvaları üzerinde özel bir ikili protokol mi yoksa [azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) veya [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)aracılığıyla akış olayları mı kullanabilir. Service Fabric, iletişim yığınınızı bağlayabileceğiniz iletişim API 'Leri sağlar, ancak keşfedilecek ve bağlanan tüm işler sizin için soyutlandır. Daha fazla ayrıntı için [güvenilir hizmet iletişim modeliyle](service-fabric-reliable-services-communication.md) ilgili bu makaleye bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Reliable Services iletişim modelinde](service-fabric-reliable-services-communication.md)bulunan kavramlar ve API 'ler hakkında daha fazla bilgi edinin, ardından [hizmet uzaktan](service-fabric-reliable-services-communication-remoting.md) iletişim kurarak hızlı bir şekilde çalışmaya başlayın veya [Web API 'si ile owın Self-Host](service-fabric-reliable-services-communication-webapi.md)kullanarak bir iletişim dinleyicisi yazmayı öğrenmek için derinlemesine ilerleyin.

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
