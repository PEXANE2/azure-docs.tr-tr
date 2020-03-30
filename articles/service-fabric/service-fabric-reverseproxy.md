---
title: Azure Hizmet Kumaş ters proxy
description: Kümenin içinden ve dışından mikro hizmetlere iletişim için Service Fabric'in ters proxy'sini kullanın.
author: BharatNarasimman
ms.topic: conceptual
ms.date: 11/03/2017
ms.author: bharatn
ms.openlocfilehash: 4fa4c6e46dd786b833087f892d995e85b5d2ea47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282230"
---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Azure Hizmet Kumaşında ters proxy
Azure Hizmet Kumaşı'nda yerleşik ters proxy, Hizmet Kumaşı kümesinde çalışan mikro hizmetlerin http uç noktaları olan diğer hizmetleri keşfetmesine ve onlarla iletişim kurmasına yardımcı olur.

## <a name="microservices-communication-model"></a>Microservices iletişim modeli
Hizmet Kumaşındaki Microservices kümedeki düğümlerin bir alt kümesinde çalışır ve çeşitli nedenlerle düğümler arasında geçiş yapabilir. Sonuç olarak, mikro hizmetlerin uç noktaları dinamik olarak değişebilir. Kümedeki diğer hizmetleri keşfetmek ve onlarla iletişim kurmak için mikro hizmet aşağıdaki adımları aşmalıdır:

1. Adlandırma hizmeti aracılığıyla hizmet konumunu çözümle.
2. Hizmete bağlanın.
3. Önceki adımları hizmet çözümlemesi uygulayan bir döngüye sarın ve bağlantı hatalarında uygulanacak ilkeleri yeniden deneyin

Daha fazla bilgi için [Connect'e](service-fabric-connect-and-communicate-with-services.md)bakın ve hizmetlerle iletişim kurun.

### <a name="communicating-by-using-the-reverse-proxy"></a>Ters proxy kullanarak iletişim kurma
Ters proxy, her düğümde çalışan ve istemci hizmetleri adına uç nokta çözümlemesi, otomatik yeniden deneme ve diğer bağlantı hatalarını işleyen bir hizmettir. Ters proxy, istemci hizmetlerinden gelen istekleri işlerken çeşitli ilkeler uygulamak üzere yapılandırılabilir. Ters proxy kullanmak istemci hizmetinin istemci tarafındaki http iletişim kitaplıklarını kullanmasına izin verir ve hizmette özel çözünürlük ve yeniden deneme mantığı gerektirmez. 

Ters proxy, istemci hizmetlerinin diğer hizmetlere istek göndermek için kullanması için yerel düğümde bir veya daha fazla uç noktayı ortaya çıkarır.

![İç iletişim][1]

> [!NOTE]
> **Desteklenen Platformlar**
>
> Service Fabric'te ters proxy şu anda aşağıdaki platformları destekler
> * *Windows Cluster*: Windows 8 ve sonraki veya Windows Server 2012 ve sonrası
> * *Linux Cluster*: Ters Proxy şu anda Linux kümeleri için kullanılamıyor
>

## <a name="reaching-microservices-from-outside-the-cluster"></a>Küme dışından mikro hizmetlere ulaşma
Mikro hizmetler için varsayılan dış iletişim modeli, her hizmete doğrudan dış istemcilerden erişilemediği bir opt-in modelidir. Mikro hizmetler ve dış istemciler arasında bir ağ sınırı olan [Azure Yük Dengeleyici,](../load-balancer/load-balancer-overview.md)ağ adresi çevirisini gerçekleştirir ve dış istekleri dahili IP:port uç noktalarına iletir. Bir mikro hizmetin bitiş noktasını dış istemciler tarafından doğrudan erişilebilir hale getirmek için, öncelikle yükü hizmetin kümede kullandığı her bağlantı noktasına iletecek Şekilde Yük Dengeleyicisi'ni yapılandırmanız gerekir. Ayrıca, çoğu mikro hizmet, özellikle de devlete özel mikro hizmetler, kümenin tüm düğümlerinde yaşamaz. Mikro hizmetler, başarısız olundaki düğümler arasında hareket edebilir. Bu gibi durumlarda, Yük Dengeleyici, trafiği iletmesi gereken yinelemelerin hedef düğümünün konumunu etkili bir şekilde belirleyemez.

### <a name="reaching-microservices-via-the-reverse-proxy-from-outside-the-cluster"></a>Küme nin dışından ters proxy ile mikro hizmetlere ulaşma
Yük Dengeleyici'de tek bir hizmetin bağlantı noktasını yapılandırmak yerine, Yük Dengeleyici'nde yalnızca ters proxy bağlantı noktasını yapılandırabilirsiniz. Bu yapılandırma, küme dışındaki istemcilerin ek yapılandırma olmadan ters proxy'yi kullanarak küme içindeki hizmetlere ulaşmasını sağlar.

![Dış iletişim][0]

> [!WARNING]
> Ters proxy'nin Yük Dengeleyicisi'ndeki bağlantı noktasını yapılandırırken, kümedeki bir HTTP bitiş noktasını ortaya çıkaran tüm mikro hizmetler kümenin dışından adreslenebilir. Bu, dahili olması amaçedilen mikro hizmetlerin kararlı bir kötü niyetli kullanıcı tarafından keşfedilebilir olduğu anlamına gelir. Bu, yararlanılabilen ciddi güvenlik açıkları sunar; örneğin:
>
> * Kötü niyetli bir kullanıcı, yeterince sertleştirilmiş bir saldırı yüzeyine sahip olmayan bir dahili hizmeti defalarca arayarak hizmet reddi saldırısı başlatabilir.
> * Kötü amaçlı bir kullanıcı, istenmeyen davranışlarla sonuçlanan bozuk paketleri dahili bir hizmete sunabilir.
> * Dahili olması gereken bir hizmet, küme dışındaki hizmetlere maruz kalması amaçlanmayan özel veya hassas bilgileri döndürerek bu hassas bilgileri kötü amaçlı bir kullanıcıya sunabilir. 
>
> Ters proxy bağlantı noktasını herkese açık hale getirmeden önce kümenizin ve üzerinde çalışan uygulamaların olası güvenlik sonuçlarını tam olarak anladığınızdan ve azaltladığınızdan emin olun. 
>


## <a name="uri-format-for-addressing-services-by-using-the-reverse-proxy"></a>Ters proxy kullanarak hizmetleri adresleme için URI biçimi
Ters proxy, gelen isteğin iletilmesi gereken hizmet bölümlerini tanımlamak için belirli bir tek biçimli kaynak tanımlayıcısı (URI) biçimini kullanır:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **http(ler):** Ters proxy HTTP veya HTTPS trafiğini kabul etmek üzere yapılandırılabilir. HTTPS iletme için, HTTPS'yi dinlemek için ters proxy kurulumu yaptıktan sonra [ters proxy ile güvenli bir hizmete bağlan'a](service-fabric-reverseproxy-configure-secure-communication.md) bakın.
* **Küme tam nitelikli etki alanı adı (FQDN) | dahili IP:** Dış istemciler için, ters proxy'yi küme etki alanı üzerinden mycluster.eastus.cloudapp.azure.com gibi erişilebilen şekilde yapılandırabilirsiniz. Varsayılan olarak, ters proxy her düğümüzerinde çalışır. İç trafik için, ters proxy'ye localhost'ta veya 10.0.0.1 gibi herhangi bir iç düğüm IP'sinde ulaşılabilir.
* **Bağlantı noktası:** Bu, ters proxy için belirtilen 19081 gibi bağlantı noktasıdır.
* **ServiceInstanceName:** Bu, "kumaş:/" olmadan ulaşmaya çalıştığınız dağıtılan hizmet örneğinin tam nitelikli adıdır. Düzeni. Örneğin, kumaş ulaşmak *için:/myapp/myservice/service,* *myapp/myservice'i*kullanırsınız.

    Hizmet örneği adı büyük/küçük harf duyarlıdır. URL'de servis örneği adı için farklı bir kasa kullanılması, isteklerin 404 (Bulunamadı) ile başarısız olmasını neden olur.
* **Sonek yolu:** Bu, bağlanmak istediğiniz hizmet için *myapi/values/add/3*gibi gerçek URL yoludur.
* **PartitionKey:** Bölümlenmiş bir hizmet için bu, ulaşmak istediğiniz bölümün hesaplanmış bölüm anahtarıdır. Bunun bölüm kimliği GUID *olmadığını* unutmayın. Bu parametre, singleton bölümleme düzenini kullanan hizmetler için gerekli değildir.
* **PartitionKind:** Bu hizmet bölümleme düzenidir. Bu 'Int64Range' veya 'Named' olabilir. Bu parametre, singleton bölümleme düzenini kullanan hizmetler için gerekli değildir.
* **DinleyiciAdı** Hizmetten uç noktalar {"Endpoints":{"Listener1":"Endpoint1","Listener2":"Endpoint2" ...}} şeklindedir. Hizmet birden çok uç nokta ortaya çıktığında, bu istemci isteğinin iletilmesi gereken bitiş noktasını tanımlar. Hizmetin yalnızca bir dinleyicisi varsa bu atlanabilir.
* **TargetReplicaSelector** Bu, hedef yinelemenin veya örneğin nasıl seçileceği belirtilir.
  * Hedef hizmet durumlu olduğunda, TargetReplicaSelector aşağıdakilerden biri olabilir: 'PrimaryReplica', 'RandomSecondaryReplica', veya 'RandomReplica'. Bu parametre belirtilmediğinde, varsayılan 'PrimaryReplica' olur.
  * Hedef hizmet durum siz olduğunda, ters proxy isteği iletmek için hizmet bölümü rasgele bir örnek seçer.
* **Zaman araları:**  Bu, istemci isteği adına hizmete ters proxy tarafından oluşturulan HTTP isteği için zaman arasını belirtir. Varsayılan değer 60 saniyedir. Bu isteğe bağlı bir parametredir.

### <a name="example-usage"></a>Örnek kullanım
Örnek olarak, aşağıdaki URL'de bir HTTP dinleyicisi açan kumaşı ele *alalım:/MyApp/MyService* hizmeti:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Hizmet için kaynaklar şunlardır:

* `/index.html`
* `/api/users/<userId>`

Hizmet singleton bölümleme düzenini kullanıyorsa, *PartitionKey* ve *PartitionKind* sorgu dize parametreleri gerekli değildir ve hizmet ağ geçidi kullanılarak ulaşılabilir:

* Harici:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Dahili:`http://localhost:19081/MyApp/MyService`

Hizmet, Uniform Int64 bölümleme düzenini kullanıyorsa, hizmetin bir bölümüne ulaşmak için *PartitionKey* ve *PartitionKind* sorgu dize parametreleri kullanılmalıdır:

* Harici:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Dahili:`http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Hizmetin ortaya çıkardığı kaynaklara ulaşmak için, kaynak yolunu hizmet adından sonra URL'ye yerleştirmemiz yeterlidir:

* Harici:`http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Dahili:`http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Ağ geçidi daha sonra bu istekleri hizmetin URL'sine iletir:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Bağlantı noktası paylaşım hizmetleri için özel kullanım
Hizmet Dokusu ters proxy bir hizmet adresini yeniden çözmek ve bir hizmete ulaşılamadığınızda isteği yeniden denemek için çalışır. Genellikle, bir hizmete ulaşılamıyorsa, hizmet örneği veya yineleme normal yaşam döngüsünün bir parçası olarak farklı bir düğüme taşınır. Bu durumda, ters proxy, özgün olarak çözülen adreste bir bitiş noktasının artık açık olmadığını belirten bir ağ bağlantısı hatası alabilir.

Ancak, yinelemeler veya hizmet örnekleri bir ana bilgisayar işlemini paylaşabilir ve http.sys tabanlı bir web sunucusu tarafından barındırıldığında bir bağlantı noktasını da paylaşabilir:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [ASP.NET Çekirdek WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

Bu durumda, web sunucusuana bilgisayar işleminde ve isteklere yanıt verirken kullanılabilir, ancak çözümlenmiş hizmet örneği veya yineleme artık ana bilgisayarda kullanılamaz. Bu durumda, ağ geçidi web sunucusundan bir HTTP 404 yanıtı alır. Böylece, bir HTTP 404 yanıtı iki farklı anlamları olabilir:

- Büyük/küçük harf #1: Servis adresi doğru, ancak kullanıcının istediği kaynak yok.
- Büyük/#2: Servis adresi yanlıştır ve kullanıcının istediği kaynak farklı bir düğümüzerinde bulunabilir.

İlk durumda bir kullanıcı hatası olarak kabul edilir normal bir HTTP 404, olduğunu. Ancak, ikinci durumda, kullanıcı var olan bir kaynak istedi. Ters proxy, hizmetin kendisi taşındığı için onu bulamadı. Ters proxy'nin adresi yeniden çözmesi ve isteği yeniden denemesi gerekir.

Ters proxy böylece bu iki durum arasında ayrım yapmak için bir yol gerekir. Bu ayrımı yapmak için sunucudan bir ipucu gereklidir.

* Varsayılan olarak, ters proxy büyük/#2 varsayar ve isteği yeniden çözmek ve vermek için çalışır.
* Ters proxy'ye #1 büyük/#1 belirtmek için, hizmetaşağıdaki HTTP yanıt üstbilgisini döndürmelidir:

  `X-ServiceFabric : ResourceNotFound`

Bu HTTP yanıt üstbilgisi, istenen kaynağın olmadığı normal bir HTTP 404 durumunu gösterir ve ters proxy hizmet adresini yeniden çözmeye çalışmaz.

## <a name="special-handling-for-services-running-in-containers"></a>Konteynerlerde çalışan hizmetler için özel taşıma

Kapsayıcıların içinde çalışan hizmetler için, aşağıdaki `Fabric_NodeIPOrFQDN` kodda olduğu gibi [ters proxy URL'sini](#uri-format-for-addressing-services-by-using-the-reverse-proxy) oluşturmak için ortam değişkenini kullanabilirsiniz:

```csharp
    var fqdn = Environment.GetEnvironmentVariable("Fabric_NodeIPOrFQDN");
    var serviceUrl = $"http://{fqdn}:19081/DockerSFApp/UserApiContainer";
```
Yerel küme için `Fabric_NodeIPOrFQDN` varsayılan olarak "localhost" olarak ayarlanır. Kapsayıcıların `-UseMachineName` düğüm üzerinde çalışan ters proxy'ye erişebileceğinden emin olmak için yerel kümeyi parametreyle başlatın. Daha fazla bilgi için, [kapsayıcıları hata ayıklamak için geliştirici ortamınızı yapılandır' ına](service-fabric-how-to-debug-windows-containers.md#configure-your-developer-environment-to-debug-containers)bakın.

Docker Compose kapları içinde çalışan Service Fabric hizmetleri özel bir docker-compose.yml *Bağlantı Noktaları bölümü* ister http: veya https: yapılandırma. Daha fazla bilgi için [bkz.](service-fabric-docker-compose.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Küme üzerinde ters proxy'yi ayarlama ve yapılandırma.](service-fabric-reverseproxy-setup.md)
* [TERS proxy ile HTTP hizmetini güvenli hale getirmek için yönlendirmeyi ayarlama](service-fabric-reverseproxy-configure-secure-communication.md)
* [Ters proxy olaylarını tanılama](service-fabric-reverse-proxy-diagnostics.md)
* [GitHub'daki örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)bir projedeki hizmetler arasındaki HTTP iletişimörneğine bakın.
* [Güvenilir Hizmetler remoting ile uzaktan yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [Güvenilir Hizmetlerde OWIN kullanan Web API'sı](service-fabric-reliable-services-communication-webapi.md)
* [Güvenilir Hizmetler kullanarak WCF iletişimi](service-fabric-reliable-services-communication-wcf.md)

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
