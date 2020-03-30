---
title: Azure Hizmet Kumaşı'ndaki hizmetlerle bağlantı kurun ve onlarla iletişim kurun
description: Service Fabric'teki hizmetleri nasıl çözeceğinizi, bağkurup kurabileceğinizi ve iletişim kurabileceğinizi öğrenin.
author: vturecek
ms.topic: conceptual
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: e57d169decf482f8b8be1e3b31a07690bc222c5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458235"
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Service Fabric'teki hizmetlerle bağlantı kurun ve iletişim kurun
Hizmet Kumaşı'nda, bir hizmet genellikle birden çok VM arasında dağıtılan Bir Hizmet Kumaşı kümesinde bir yerde çalışır. Bir yerden başka bir yere, servis sahibi tarafından veya otomatik olarak Service Fabric tarafından taşınabilir. Hizmetler statik olarak belirli bir makineye veya adrese bağlı değildir.

Hizmet Kumaşı uygulaması genellikle her hizmetin özel bir görev yürüttüğü birçok farklı hizmetlerden oluşur. Bu hizmetler, bir web uygulamasının farklı bölümlerini oluşturma gibi tam bir işlev oluşturmak için birbirleriyle iletişim kurabilir. Hizmetlere bağlanan ve hizmetlerle iletişim kuran istemci uygulamaları da vardır. Bu belgede, Service Fabric'teki hizmetlerinizle ve hizmetleriarasında iletişimin nasıl ayarlır?

## <a name="bring-your-own-protocol"></a>Kendi protokolünüze getirin
Service Fabric, hizmetlerinizin yaşam döngüsünü yönetmenize yardımcı olur ancak hizmetlerinizin ne yaptığına karar vermez. Bu iletişim içerir. Hizmetiniz Service Fabric tarafından açıldığında, istediğiniz protokol veya iletişim yığınını kullanarak hizmetinizin gelen istekler için bir bitiş noktası ayarlama fırsatıdır. Hizmetiniz, URI gibi herhangi bir adresleme düzenini kullanarak normal bir **IP:port** adresinden dinlenir. Birden çok hizmet örneği veya yineleme, ana bilgisayar işlemini paylaşabilir ve bu durumda windows'daki http.sys çekirdek sürücüsü gibi bir bağlantı noktası paylaşım mekanizması kullanmaları gerekir. Her iki durumda da, bir ana bilgisayar işlemindeki her hizmet örneği veya yineleme benzersiz bir şekilde ele alınabiliyor olmalıdır.

![hizmet bitiş noktaları][1]

## <a name="service-discovery-and-resolution"></a>Hizmet bulma ve çözüm
Dağıtılmış bir sistemde, hizmetler zaman içinde bir makineden diğerine geçebilir. Bu, kaynak dengeleme, yükseltmeler, başarısızlıklar veya ölçeklendirme dahil olmak üzere çeşitli nedenlerle gerçekleşebilir. Bu, hizmet farklı IP adreslerine sahip düğümlere taşınırken hizmet bitiş noktası adreslerinin değiştiği ve hizmet dinamik olarak seçilmiş bir bağlantı noktası kullanıyorsa farklı bağlantı noktalarında açılabileceği anlamına gelir.

![Hizmetlerin dağıtımı][7]

Service Fabric, Adlandırma Hizmeti adı verilen bir bulma ve çözüm hizmeti sağlar. Adlandırma Hizmeti, hizmet örneklerini dinledikleri uç nokta adresleriyle eşleyen bir tablo tutar. Service Fabric'teki tüm adlandırılmış hizmet örnekleri, örneğin URI `"fabric:/MyApplication/MyService"`olarak temsil edilen benzersiz adlara sahiptir. Hizmetin adı hizmetin ömrü boyunca değişmez, hizmetler hareket ettiğinde yalnızca uç nokta adresleri değişir. Bu, sabit URL'leri olan ancak IP adresinin değişebileceği web sitelerine benzer. Web sitesi URL'lerini IP adreslerine göre çözen web'deki DNS'ye benzer şekilde, Service Fabric'in hizmet adlarını bitiş noktası adresleriyle eşleyen bir kayıt şirketi vardır.

![hizmet bitiş noktaları][2]

Hizmetleri çözme ve hizmetlere bağlanma, bir döngüde çalıştırılaçalışan aşağıdaki adımları içerir:

* **Çözüm :** Bir hizmetin Adlandırma Hizmeti'nden yayımlandığı bitiş noktasını alın.
* **Connect**: Bu uç noktada kullandığı protokol üzerinden servise bağlanın.
* **Yeniden Deneme**: Bir bağlantı denemesi, örneğin hizmet bitiş noktası adresinin son kez çözüldüğü tarihten bu yana taşınmışsa, çeşitli nedenlerle başarısız olabilir. Bu durumda, önceki çözüm ve bağlantı adımlarının yeniden denenmesi gerekir ve bağlantı başarılı olana kadar bu döngü yinelenir.

## <a name="connecting-to-other-services"></a>Diğer hizmetlere bağlanma
Kümedeki düğümler aynı yerel ağda olduğundan, küme içinde birbirine bağlanan hizmetler genellikle diğer hizmetlerin uç noktalarına doğrudan erişebilir. Hizmetler arasında bağlantı kurmak daha kolay hale getirmek için, Hizmet Dokusu Adlandırma Hizmeti'ni kullanan ek hizmetler sağlar. Bir DNS hizmeti ve ters proxy hizmeti.


### <a name="dns-service"></a>DNS hizmeti
Birçok hizmet, özellikle kapsayıcı hizmetler, varolan bir URL adı olabilir, standart DNS protokolü (Adlandırma Hizmeti protokolü yerine) kullanarak bunları çözmek için özellikle uygulama "asansör ve kaydırma" senaryoları çok uygundur. DNS hizmeti nin yaptığı tam olarak budur. DNS adlarını bir hizmet adı ile eşlenebilmenizi ve dolayısıyla uç nokta IP adreslerini çözmenizi sağlar. 

Aşağıdaki diyagramda gösterildiği gibi, Hizmet Kumaşı kümesinde çalışan DNS hizmeti, DNS adlarını, bağlanmak için bitiş noktası adreslerini döndürmek için Adlandırma Hizmeti tarafından çözülen hizmet adlarıyla eşler. Hizmetin DNS adı oluşturuldurma sırasında sağlanır. 

![hizmet bitiş noktaları][9]

DNS hizmetinin nasıl kullanılacağı hakkında daha fazla bilgi için [Azure Hizmet Kumaşı makalesinde DNS hizmetine](service-fabric-dnsservice.md) bakın.

### <a name="reverse-proxy-service"></a>Ters proxy hizmeti
Ters proxy, https dahil olmak üzere HTTP uç noktalarını ortaya çıkaran kümedeki hizmetleri ele adamaktadır. Ters proxy, belirli bir URI biçimine sahip olarak diğer hizmetleri ve yöntemleri aramayı büyük ölçüde kolaylaştırır ve bir hizmetin Adlandırma Hizmeti'ni kullanarak başka bir hizmetle iletişim kurması için gereken kararlılığı, bağlanmayı, yeniden deneme adımlarını işler. Başka bir deyişle, diğer hizmetleri ararken, url çağırmak kadar basit hale getirerek Adlandırma Hizmetini sizden gizler.

![hizmet bitiş noktaları][10]

Ters proxy hizmetinin nasıl kullanılacağı hakkında daha fazla bilgi için [Azure Service Fabric makalesinde Ters proxy'ye](service-fabric-reverseproxy.md) bakın.

## <a name="connections-from-external-clients"></a>Harici istemcilerden gelen bağlantılar
Kümedeki düğümler aynı yerel ağda olduğundan, küme içinde birbirine bağlanan hizmetler genellikle diğer hizmetlerin uç noktalarına doğrudan erişebilir. Ancak bazı ortamlarda, bir küme, sınırlı bir bağlantı noktası kümesi nden dış giriş trafiğini yönlendirir bir yük dengeleyicisinin arkasında olabilir. Bu gibi durumlarda, hizmetler yine de birbirleriyle iletişim kurabilir ve Adlandırma Hizmeti'ni kullanarak adresleri çözümleyebilir, ancak dış istemcilerin hizmetlere bağlanmasına izin vermek için ek adımlar atılması gerekir.

## <a name="service-fabric-in-azure"></a>Azure'da Hizmet Kumaşı
Azure'daki Hizmet Kumaşı kümesi, Azure Yük Dengeleyicisi'nin arkasına yerleştirilir. Kümeye gelen tüm dış trafiğin yük dengeleyiciden geçmesi gerekir. Yük dengeleyicisi, belirli bir bağlantı noktasındagelen trafiği otomatik olarak aynı bağlantı noktası açık olan rasgele bir *düğüme* iletir. Azure Yük Dengeleyicisi yalnızca *düğümlerde*açık olan bağlantı noktalarını bilir, tek tek *hizmetler*tarafından açılan bağlantı noktalarını bilmez.

![Azure Yük Dengeleyici ve Servis Kumaşı topolojisi][3]

Örneğin, bağlantı noktası **80'deki**dış trafiği kabul etmek için aşağıdaki şeylerin yapılandırılması gerekir:

1. Bağlantı noktası 80'de dinleyen bir hizmet yazın. Hizmetin ServiceManifest.xml'inde bağlantı noktası 80'i yapılandırın ve hizmette bir dinleyici açın, örneğin, kendi barındırılan bir web sunucusu.

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
2. Azure'da bir Hizmet Kumaş Kümesi oluşturun ve hizmete ev sahipliği yapacak düğüm türü için bağlantı noktası **80'i** özel bir bitiş noktası bağlantı noktası olarak belirtin. Birden fazla düğüm türünüz varsa, hizmette yalnızca özel bitiş noktası bağlantı noktası açık olan düğüm türünde çalıştığından emin olmak için bir *yerleşim kısıtlaması* ayarlayabilirsiniz.

    ![Düğüm türünde bağlantı noktası açma][4]
3. Küme oluşturulduktan sonra, kümenin Kaynak Grubu'ndaki Azure Yük Dengeleyicisini 80. Azure portalı üzerinden bir küme oluştururken, bu, yapılandırılan her özel uç nokta bağlantı noktası için otomatik olarak ayarlanır.

    ![Azure Yük Dengeleyicisi'nde ileri trafik][5]
4. Azure Yük Dengeleyicisi, belirli bir düğüme trafik gönderip göndermeyeceklerini belirlemek için bir sonda kullanır. Sonda, düğümün yanıt verip vermeyemeyeceğini belirlemek için her düğümde bir bitiş noktasını düzenli olarak denetler. Sonda, yapılandırılmış sayıdan sonra yanıt alamazsa, yük dengeleyicisi bu düğüme trafik göndermeyi durdurur. Azure portalı üzerinden küme oluştururken, yapılandırılan her özel bitiş noktası bağlantı noktası için otomatik olarak bir sonda ayarlanır.

    ![Azure Yük Dengeleyicisi'nde ileri trafik][8]

Azure Yük Dengeleyicisi ve sondasının düğümler üzerinde çalışan *hizmetleri* değil, yalnızca *düğümleri*bildiğini unutmamak önemlidir. Azure Yük Dengeleyicisi her zaman sondaya yanıt veren düğümlere trafik gönderir, bu nedenle sondaya yanıt verebilen düğümlerde hizmetlerin kullanılabilir olduğundan emin olmak için dikkatli olunmalıdır.

## <a name="reliable-services-built-in-communication-api-options"></a>Güvenilir Hizmetler: Yerleşik iletişim API seçenekleri
Güvenilir Hizmetler çerçeve, önceden oluşturulmuş birkaç iletişim seçeneğiyle gemiler. Hangisinin sizin için en iyi şekilde çalışacağına karar, programlama modelinin seçimine, iletişim çerçevesine ve hizmetlerinizin yazıldığı programlama diline bağlıdır.

* **Belirli bir protokol yok:**  Eğer iletişim çerçevesi belirli bir seçim yoksa, ancak bir şey almak ve hızlı bir şekilde çalışan istiyorsanız, o zaman sizin için ideal seçenek [hizmet remoting](service-fabric-reliable-services-communication-remoting.md), hangi güçlü güvenilir hizmetler ve Güvenilir Aktörler için güçlü bir şekilde yazılmış uzaktan yordam çağrıları sağlar. Bu, hizmet iletişimine başlamanın en kolay ve en hızlı yoludur. Hizmet remoting hizmet adresleri, bağlantı, yeniden deneme ve hata işleme çözümünü işler. Bu hem C# hem de Java uygulamaları için kullanılabilir.
* **HTTP**: Dil-agnostik iletişim için HTTP, tümü Service Fabric tarafından desteklenen birçok farklı dilde kullanılabilen araçlar ve HTTP sunucuları ile endüstri standardı bir seçim sunar. Hizmetler, C# uygulamaları için [ASP.NET Web API'si](service-fabric-reliable-services-communication-webapi.md) de dahil olmak üzere kullanılabilir herhangi bir HTTP yığınını kullanabilir. C# `ICommunicationClient` ile yazılmış istemciler, Java için ve `ServicePartitionClient` `CommunicationClient` `FabricServicePartitionClient` sınıfları kullanırken, hizmet [çözümü, HTTP bağlantıları ve döngüleri yeniden denemek için](service-fabric-reliable-services-communication.md)ve sınıfları kullanabilir.
* **WCF**: İletişim çerçeveniz olarak WCF kullanan varolan kodunuz `WcfCommunicationListener` varsa, istemci `WcfCommunicationClient` `ServicePartitionClient` için sunucu tarafı ve sınıflar için kullanabilirsiniz. Ancak bu, yalnızca Windows tabanlı kümelerde C# uygulamaları için kullanılabilir. Daha fazla bilgi için, [iletişim yığınının WCF tabanlı uygulaması](service-fabric-reliable-services-communication-wcf.md)hakkında bu makaleye bakın.

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Özel protokolleri ve diğer iletişim çerçevelerini kullanma
Hizmetler, ister TCP soketleri üzerinden özel bir ikili protokol olsun, ister [Azure Etkinlik Hub'ları](https://azure.microsoft.com/services/event-hubs/) veya [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)üzerinden olay akışı olsun, iletişim için herhangi bir protokolü veya çerçeveyi kullanabilir. Service Fabric, iletişim yığınınızı takabileceğiniz iletişim API'leri sağlarken, keşfetmek ve bağlamak için yapılan tüm çalışmalar sizden soyutlanır. Daha fazla ayrıntı için [Güvenilir Hizmet iletişim modeli](service-fabric-reliable-services-communication.md) hakkında bu makaleye bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Güvenilir Hizmetler iletişim modelinde](service-fabric-reliable-services-communication.md)bulunan kavramlar ve API'ler hakkında daha fazla bilgi edinin, ardından hızlı bir şekilde [hizmet remoting'e](service-fabric-reliable-services-communication-remoting.md) başlayın veya [OWIN self-host ile Web API'yi](service-fabric-reliable-services-communication-webapi.md)kullanarak bir iletişim dinleyicisi yazmayı öğrenmek için derinlemesine gidin.

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
