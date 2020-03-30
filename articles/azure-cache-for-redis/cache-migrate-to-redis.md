---
title: Yönetilen Önbellek Hizmeti uygulamalarını Redis'e geçir - Azure
description: Yönetilen Önbellek Hizmeti ve Rol İçi Önbellek uygulamalarını Redis için Azure Önbelleğine nasıl geçireceğinizi öğrenin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 05/30/2017
ms.author: yegu
ms.openlocfilehash: 9596b8cb771f114cb09c5d6c6ae33b4fc4a8cada
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74122692"
---
# <a name="migrate-from-managed-cache-service-to-azure-cache-for-redis"></a>Yönetilen Önbellek Hizmeti’nden Redis için Azure Cache’ye Geçiş
Önbelleğe alma uygulamanız tarafından kullanılan Yönetilen Önbellek Hizmeti özelliklerine bağlı olarak, Azure Yönetilen Önbellek Hizmetini kullanan uygulamalarınızı Redis için Azure Önbelleğine geçirmek, uygulamanızda en az değişiklikle gerçekleştirilebilir. API'ler tam olarak aynı olmasa da benzerler ve önbelleğe erişmek için Yönetilen Önbellek Hizmeti'ni kullanan varolan kodunuzuzun çoğu en az değişiklikle yeniden kullanılabilir. Bu makalede, Redis için Azure Önbelleği kullanmak için Yönetilen Önbellek Hizmeti uygulamalarınızı geçirmek için gerekli yapılandırma ve uygulama değişikliklerinin nasıl yapılacağı ve Redis için Azure Önbelleğinin bazı özelliklerinin kullanımının kullanılabilir Yönetilen Önbellek Hizmeti önbelleği.

>[!NOTE]
>Yönetilen Önbellek Hizmeti ve Rol İçi Önbellek 30 Kasım 2016'da [kullanımdan kaldırıldı.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) Redis için Azure Önbelleğine geçirmek istediğiniz rol içi önbellek dağıtımlarınız varsa, bu makaledeki adımları izleyebilirsiniz.

## <a name="migration-steps"></a>Geçiş Adımları
Redis için Azure Önbelleğini kullanmak için yönetilen önbellek hizmeti uygulamasını geçirmek için aşağıdaki adımlar gereklidir.

* Redis için Azure Önbelleği'ne Harita Yönetilen Önbellek Hizmeti özellikleri
* Önbellek Teklifi Seçin
* Önbellek Oluşturma
* Önbellek İstemlerini Yapılandırma
  * Yönetilen Önbellek Hizmeti Yapılandırmasını Kaldırma
  * StackExchange.Redis NuGet Paketini kullanarak önbellek istemcisini yapılandırın
* Yönetilen Önbellek Hizmeti kodunu geçir
  * ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlanma
  * Önbellekte ilkel veri türlerine erişin
  * Önbellekte .NET nesneleriyle çalışma
* ASP.NET Oturum Durumu ve Çıktı önbelleğe alma 

## <a name="map-managed-cache-service-features-to-azure-cache-for-redis"></a>Redis için Azure Önbelleği'ne Harita Yönetilen Önbellek Hizmeti özellikleri
Azure Yönetilen Önbellek Hizmeti ve Redis için Azure Önbelleği benzerdir, ancak bazı özelliklerini farklı şekillerde uygular. Bu bölümde bazı farklılıklar açıklanır ve Redis için Azure Önbelleğinde Yönetilen Önbellek Hizmetinin özelliklerini uygulama konusunda kılavuz sağlar.

| Yönetilen Önbellek Hizmeti özelliği | Yönetilen Önbellek Hizmeti desteği | Redis desteği için Azure Önbelleği |
| --- | --- | --- |
| Adlandırılmış önbellekler |Varsayılan önbellek yapılandırılır ve Standart ve Premium önbellek tekliflerinde, istenirse en fazla dokuz ek adlandırılmış önbellek yapılandırılabilir. |Redis için Azure Önbelleği, adlandırılmış önbelleklere benzer bir işlevselliği uygulamak için kullanılabilecek yapılandırılabilir sayıda veritabanına (varsayılan 16) sahiptir. Daha fazla bilgi için bkz. [Redis veritabanı nedir?](cache-faq.md#what-are-redis-databases) ve [Varsayılan Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration). |
| Yüksek Kullanılabilirlik |Standart ve Premium önbellek tekliflerindeki önbellekteki öğeler için yüksek kullanılabilirlik sağlar. Öğeler bir hata nedeniyle kaybolursa, önbellekteki öğelerin yedek kopyaları hala kullanılabilir. İkincil önbelleğe yazılar eşzamanlı olarak yapılır. |Yüksek kullanılabilirlik, iki düğüm Birincil/Çoğaltma yapılandırması olan Standart ve Premium önbellek tekliflerinde kullanılabilir (Premium önbellekteki her parçanın birincil/çoğaltma çifti vardır). Çoğaltma için yazar eşzamanlı yapılır. Daha fazla bilgi [için Redis fiyatlandırması için Azure Önbelleği'ne](https://azure.microsoft.com/pricing/details/cache/)bakın. |
| Bildirimler |Adlandırılmış bir önbellekte çeşitli önbellek işlemleri gerçekleştiğinde istemcilerin eşzamanlı bildirimler almasını sağlar. |İstemci uygulamaları, bildirimlere benzer bir işlevsellik elde etmek için Redis pub/alt veya [Keyspace bildirimlerini](cache-configure.md#keyspace-notifications-advanced-settings) kullanabilir. |
| Yerel önbellek |Önbelleğe alınmış nesnelerin bir kopyasını yerel olarak istemcide depolar ve ekstra hızlı erişim için. |İstemci uygulamaları bir sözlük veya benzer veri yapısı kullanarak bu işlevselliği uygulamak gerekir. |
| Tahliye Politikası |Yok ya da LRU. Varsayılan ilke LRU'dur. |Redis için Azure Önbelleği aşağıdaki tahliye ilkelerini destekler: uçucu-lru, allkeys-lru, uçucu-rasgele, allkeys-random, volatile-ttl, noeviction. Varsayılan ilke geçici-lru'dur. Daha fazla bilgi için Varsayılan [Redis sunucu](cache-configure.md#default-redis-server-configuration)yapılandırması'na bakın. |
| Sona Erme Politikası |Varsayılan son kullanma ilkesi Mutlak'tır ve varsayılan son kullanma aralığı 10 dakikadır. Sürgülü ve Asla ilkeleri de mevcuttur. |Önbellekteki varsayılan öğelerin süresi dolmaz, ancak önbellek kümesi aşırı yüklenmeleri kullanılarak son kullanma tarihi her yazma bazında yapılandırılabilir. |
| Bölgeler ve Etiketleme |Bölgeler önbelleğe alınmış öğeler için alt gruplardır. Bölgeler ayrıca önbelleğe alınmış öğelerin ek açıklamalarını etiketler olarak adlandırılan ek açıklayıcı dizeleri destekler. Bölgeler, o bölgedeki etiketli öğelerde arama işlemleri gerçekleştirme yeteneğini destekler. Bir bölgedeki tüm öğeler önbellek kümesinin tek bir düğümü içinde bulunur. |Redis için bir Azure Önbelleği tek bir düğümden oluşur (Redis kümesi etkinleştirilmediği sürece) yönetilen Önbellek Hizmeti bölgeleri kavramı geçerli değildir. Redis, anahtar adlarının içine gömülü ve daha sonra öğeleri almak için kullanılabilen açıklayıcı etiketlerin anahtarları alırken arama ve joker karakter işlemlerini destekler. Redis kullanarak etiketleme çözümünün uygulanmasına örnek [olarak,](https://stackify.com/implementing-cache-tagging-redis/)bkz. |
| Serileştirme |Yönetilen Önbellek NetDataContractSerializer, BinaryFormatter ve özel serializers kullanımını destekler. Varsayılan netdatacontractserializer olduğunu. |.NET nesnelerini önbelleğe yerleştirmeden önce seri hale getirmek, istemci uygulama geliştiricisine kadar serileştirici seçimi ile .NET nesnelerini seri hale getirmek istemci uygulamasının sorumluluğundadır. Daha fazla bilgi ve örnek kod için [önbellekteki .NET nesneleriile çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)konusuna bakın. |
| Önbellek emülatörü |Yönetilen Önbellek yerel bir önbellek emülatörü sağlar. |Redis için Azure Önbelleği'nde emülatör yoktur, ancak emülatör deneyimi sağlamak için [MSOpenTech redis-server.exe'nin oluşturulmasını yerel olarak çalıştırabilirsiniz.](cache-faq.md#cache-emulator) |

## <a name="choose-a-cache-offering"></a>Önbellek Teklifi Seçin
Redis için Microsoft Azure Önbelleği aşağıdaki katmanlarda kullanılabilir:

* **Temel** – Tek düğümlü. 53 GB'a kadar birden çok boyut.
* **Standart** – İki düğümlü Birincil/Çoğaltma. 53 GB'a kadar birden çok boyut. %99,9 SLA.
* **Premium** – İki düğümlü Birincil/Çoğaltma, En fazla 10 parça. 6 GB'dan 1,2 TB'ye kadar birden fazla boyut. [Redis kümesi](cache-how-to-premium-clustering.md), [Redis kalıcılığı](cache-how-to-premium-persistence.md), and [Azure Virtual Network](cache-how-to-premium-vnet.md) dahil tüm Standart katman özellikleri ve fazlası. %99,9 SLA.

Her katman özellikler ve fiyatlandırma açısından farklıdır. Özellikler daha sonra bu kılavuzda ele alınmıştır ve fiyatlandırma hakkında daha fazla bilgi için [Önbellek Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/cache/)bakın.

Geçiş için bir başlangıç noktası, önceki Yönetilen Önbellek Hizmeti önbelleğinin boyutuyla eşleşen boyutu seçmek ve uygulamanızın gereksinimlerine bağlı olarak ölçeklendirmek veya küçültmektir. Redis için doğru Azure Önbelleğini seçme hakkında daha fazla bilgi için, [Redis için hangi Azure Önbelleğini ve boyutunu kullanmam gerektiğini](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)görün.

## <a name="create-a-cache"></a>Önbellek Oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Önbellek İstemlerini Yapılandırma
Önbellek oluşturulduktan ve yapılandırıldıktan sonra, bir sonraki adım Yönetilen Önbellek Hizmeti yapılandırmasını kaldırmak ve önbellek istemcilerinin önbelleğe erişebilmeleri için Redis yapılandırması ve başvuruları için Azure Önbelleği eklemektir.

* Yönetilen Önbellek Hizmeti Yapılandırmasını Kaldırma
* StackExchange.Redis NuGet Paketini kullanarak önbellek istemcisini yapılandırın

### <a name="remove-the-managed-cache-service-configuration"></a>Yönetilen Önbellek Hizmeti Yapılandırmasını Kaldırma
İstemci uygulamaları Redis için Azure Önbellek için yapılandırılmadan önce, yönetilen Önbellek Hizmeti NuGet paketini kaldırarak varolan Yönetilen Önbellek Hizmeti yapılandırması ve derleme başvurularıkaldırılmalıdır.

Yönetilen Önbellek Hizmeti NuGet paketini kaldırmak için Solution **Explorer'daki** istemci projesini sağ tıklatın ve **NuGet Paketlerini Yönet'i**seçin. Yüklü **paketler** düğümünü seçin ve Arama yüklü paketler kutusuna W**indowsAzure.Caching** yazın. **Windows** **Azure Önbelleği'ni** (veya NuGet paketinin sürümüne bağlı olarak **Windows** **Azure Önbelleğe Alma'yı)** seçin), **Kaldır'ı**tıklatın ve ardından **Kapat'ı**tıklatın.

![Azure Yönetilen Önbellek Hizmeti NuGet Paketi'ni Kaldır](./media/cache-migrate-to-redis/IC757666.jpg)

Yönetilen Önbellek Hizmeti NuGet paketinin kaldırılması, yönetilen Önbellek Hizmeti derlemelerini ve istemci uygulamasının app.config veya web.config'indeki Yönetilen Önbellek Hizmeti girişlerini kaldırır. NuGet paketini yüklerken bazı özelleştirilmiş ayarlar kaldırılmayabileceğinden, web.config veya app.config'i açın ve aşağıdaki öğelerin kaldırıldığından emin olun.

Girişin `dataCacheClients` öğeden kaldırıldığından `configSections` emin olun. Öğenin tamamını `configSections` kaldırmayın; varsa girişi `dataCacheClients` kaldırman.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Bölümün kaldırıldığından `dataCacheClients` emin olun. Bölüm `dataCacheClients` aşağıdaki örneğe benzer olacaktır.

```xml
<dataCacheClients>
  <dataCacheClientname="default">
    <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
    <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
    <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

    <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
    <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
    <!--<securityProperties mode="Message" sslEnabled="true">
      <messageSecurity authorizationInfo="[Authentication Key]" />
    </securityProperties>-->
  </dataCacheClient>
</dataCacheClients>
```

Yönetilen Önbellek Hizmeti yapılandırması kaldırıldıktan sonra, önbellek istemcisini aşağıdaki bölümde açıklandığı şekilde yapılandırabilirsiniz.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>StackExchange.Redis NuGet Paketini kullanarak önbellek istemcisini yapılandırın
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Yönetilen Önbellek Hizmeti kodunu geçir
Redis istemcisi için StackExchange.Azure Önbelleği'nin API'si Yönetilen Önbellek Hizmeti'ne benzer. Bu bölümde farklılıklara genel bir bakış sağlar.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlanma
Yönetilen Önbellek Hizmeti'nde, önbelleğe yapılan `DataCacheFactory` bağlantılar `DataCache` ve sınıflar tarafından işlenir. Redis için Azure Önbelleğinde, bu bağlantılar `ConnectionMultiplexer` sınıf tarafından yönetilir.

Önbelleğe erişmek istediğiniz herhangi bir dosyanın üstüne aşağıdaki ifadesini kullanarak ifadesini ekleyin.

```csharp
using StackExchange.Redis
```

Bu ad alanı çözülmezse, Quickstart'ta açıklandığı gibi StackExchange.Redis NuGet paketini eklediğinizden emin [olun: Redis için Azure Önbelleğini bir .NET uygulamasıyla kullanın.](cache-dotnet-how-to-use-azure-redis-cache.md)

> [!NOTE]
> StackExchange.Redis istemcisinin .NET Framework 4 veya daha yüksek gerektirmesini unutmayın.
> 
> 

Redis örneği için bir Azure Önbelleğine `ConnectionMultiplexer.Connect` bağlanmak için statik yöntemi arayın ve bitiş noktası ve anahtardan geçirin. Uygulamanızda bir `ConnectionMultiplexer` örneği paylaşmaya ilişkin bir yaklaşım, aşağıdaki örneğe benzer bir bağlı örnek döndüren statik özelliğe sahip olmaktır. Bu yaklaşım, bağlı `ConnectionMultiplexer` tek bir örneği başlatmanın iş parçacığı güvenli bir yolunu sağlar. Bu örnekte `abortConnect` yanlış olarak ayarlanır, bu da önbelleğe bağlantı kurulmasa bile aramanın başarılı olacağı anlamına gelir. `ConnectionMultiplexer` temel özelliklerinden biri ağ sorunu ya da diğer nedenler çözümlendiğinde önbellek bağlantısını otomatik olarak geri yüklemesidir.

```csharp
private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
{
    return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
});

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```

Önbellek bitiş noktası, anahtarlar ve bağlantı noktaları önbellek örneğiniz için Redis için **Azure Önbelleği'nden** elde edilebilir. Daha fazla bilgi [için Redis özellikleri için Azure Önbelleği'ne](cache-configure.md#properties)bakın.

Bağlantı kurulduktan sonra, yöntemi çağırarak Redis için Azure Önbelleği veritabanına bir başvuru döndürün. `ConnectionMultiplexer.GetDatabase` `GetDatabase` yönteminden döndürülen nesne küçük, geçişli bir nesnedir ve depolanması gerekmez.

```csharp
IDatabase cache = Connection.GetDatabase();

// Perform cache operations using the cache object...
// Simple put of integral data types into the cache
cache.StringSet("key1", "value");
cache.StringSet("key2", 25);

// Simple get of data types from the cache
string key1 = cache.StringGet("key1");
int key2 = (int)cache.StringGet("key2");
```

StackExchange.Redis istemcisi, önbellekteki öğelere erişmek ve depolamak için ve `RedisKey` `RedisValue` türleri kullanır. Bu tür, dize de dahil olmak üzere en ilkel dil türlerine eşlenir ve genellikle doğrudan kullanılmaz. Redis Strings Redis değerinin en temel türüdür ve serileştirilmiş ikili akışlar da dahil olmak üzere birçok veri türü içerebilir ve türü `String` doğrudan kullanmasanız da, adında içeren yöntemleri kullanırsınız. Çoğu ilkel veri türü için, önbellekte koleksiyonları `StringSet` veya `StringGet` diğer Redis veri türlerini depolamadığınız sürece, öğeleri önbellekten ve yöntemleri kullanarak depolar ve alırsınız. 

`StringSet`ve `StringGet` Yönetilen Önbellek Hizmeti `Put` ve `Get` yöntemlerine benzer, önemli bir fark, bir .NET nesnesini ayarlamadan önce ve önbelleğe girmeden önce onu seri hale getirmelisiniz. 

Çağrı `StringGet`yaparken , nesne varsa, döndürülür ve değilse, null döndürülür. Bu durumda, değeri istenilen veri kaynağından alabilir ve sonraki kullanım için önbellekte depolayabilirsiniz. Bu desen önbellek kenara desen olarak bilinir.

Bir öğenin önbellekte sona erme tarihini belirtmek için, `StringSet` dizesine ait `TimeSpan` parametresini kullanın.

```csharp
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Redis için Azure Önbelleği ,NET nesnelerinin yanı sıra ilkel veri türleri ile de çalışabilir, ancak bir .NET nesnesi önbelleğe alınamadan önce seri hale getirilmelidir. Bu serileştirme uygulama geliştiricisinin sorumluluğundadır ve geliştiriciye serileştirici seçiminde esneklik sağlar. Daha fazla bilgi ve örnek kod için [önbellekteki .NET nesneleriile çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)konusuna bakın.

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-cache-for-redis"></a>ASP.NET Oturum Durumu ve Çıktı önbelleğe alma
Redis için Azure Önbelleği'nde hem ASP.NET oturum durumu hem de Sayfa Çıktısı önbelleğe alma sağlayıcıları vardır. Bu sağlayıcıların Yönetilen Önbellek Hizmeti sürümlerini kullanan uygulamanızı geçirmek için önce web.config'inizden varolan bölümleri kaldırın ve ardından sağlayıcıların Redis sürümleri için Azure Önbelleği'ni yapılandırın. Redis ASP.NET sağlayıcıları için Azure Önbelleğini kullanma yla ilgili talimatlar [için, Redis için Azure Önbelleği için ASP.NET Oturum Durumu Sağlayıcısı](cache-aspnet-session-state-provider.md) ve [Redis için Azure Önbelleği için ASP.NET Çıktı Önbelleği Sağlayıcısı'na](cache-aspnet-output-cache-provider.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Öğreticiler, örnekler, videolar ve daha fazlası [için Redis belgeleri için Azure Önbelleğini](https://azure.microsoft.com/documentation/services/cache/) keşfedin.

