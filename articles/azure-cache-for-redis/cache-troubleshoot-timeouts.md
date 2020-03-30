---
title: Redis için Azure Cache zaman aşımı sorunlarını giderme
description: Redis sunucu düzeltme ve StackExchange.Redis zaman ara verme özel durumları gibi Redis için Azure Önbellek ile sık karşılaşılan zaman anına ilişkin sorunları nasıl çözeceğinizi öğrenin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 10/18/2019
ms.openlocfilehash: 4b8cfed883ffef780de2e82e3f309e97bcb5515c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278252"
---
# <a name="troubleshoot-azure-cache-for-redis-timeouts"></a>Redis için Azure Cache zaman aşımı sorunlarını giderme

Bu bölümde, Redis için Azure Önbelleği'ne bağlanırken oluşan sorun giderme zaman giderme sorunları anlatılmaktadır.

- [Redis sunucu yama](#redis-server-patching)
- [StackExchange.Redis zaman arası özel durumlar](#stackexchangeredis-timeout-exceptions)

> [!NOTE]
> Bu kılavuzdaki sorun giderme adımlarından bazıları Redis komutlarını çalıştırmak ve çeşitli performans ölçümlerini izlemek için yönergeler içerir. Daha fazla bilgi ve talimatlar için [Ek bilgi](#additional-information) bölümündeki makalelere bakın.
>

## <a name="redis-server-patching"></a>Redis sunucu yama

Redis için Azure Önbelleği, sunucu yazılımını sağladığı yönetilen hizmet işlevselliğinin bir parçası olarak düzenli olarak güncelleştirir. Bu [yama](cache-failover.md) etkinliği büyük ölçüde sahnenin arkasında gerçekleşir. Redis sunucu düğümlerinin yaması sırasında, bu düğümlere bağlı Redis istemcileri, bu düğümler arasında bağlantı geçiş yaptığı için geçici zaman aparatları yaşayabilir. Bkz. Bir hata, yama nın uygulamanız üzerinde ne gibi yan etkilere sahip olabileceği ve yama olaylarının işlenmesini nasıl iyileştirebileceğiniz hakkında daha fazla bilgi için [müşteri başvurumu nasıl etkiler.](cache-failover.md#how-does-a-failover-affect-my-client-application)

## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis zaman arası özel durumlar

StackExchange.Redis varsayılan değeri `synctimeout` 1000 ms olan senkron işlemler için adlandırılmış bir yapılandırma ayarı kullanır. Bu süre içinde eşzamanlı bir arama tamamlanmazsa, StackExchange.Redis istemcisi aşağıdaki örneğe benzer bir zaman alakart hatası atar:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)

Bu hata iletisi, sorunun nedeni ve olası çözümüne işaret yardımcı olabilecek ölçümler içerir. Aşağıdaki tabloda hata iletisi ölçümleri hakkında ayrıntılar yer alıyor.

| Hata iletisi ölçümü | Ayrıntılar |
| --- | --- |
| ınst |Son kez dilimde: 0 komutları verildi |
| Mgr |Soket yöneticisi `socket.select`yapıyor, bu da işletim sistemi'nden yapacak bir şeyi olan bir soketi göstermesini istediği anlamına geliyor. Okuyucu ağdan aktif olarak okumuyor çünkü yapacak bir şey olmadığını düşünüyor. |
| kuyruk |Toplam 73 devam etmekte olan operasyonlar var |
| Qu |Devam eden işlemlerden 6'sı gönderilmemiş sırada ve giden ağa henüz yazılmadı |
| Qs |Devam eden işlemlerin 67'si sunucuya gönderildi, ancak henüz bir yanıt bulunamadı. Yanıt, `Not yet sent by the server``sent by the server but not yet processed by the client.` |
| Qc |Devam eden işlemlerin 0'ı yanıtları gördü, ancak tamamlanma döngüsünde bekledikleri için henüz tam olarak işaretlenmedi |
| Wr |Etkin bir yazar (yani gönderilmemiş 6 istek göz ardı edilmiyor) baytlar/activewriters |
| in |Hiçbir aktif okuyucu ve sıfır bayt NIC bayt / activereaders okumak için kullanılabilir |

Olası kök nedenlerini araştırmak için aşağıdaki adımları kullanabilirsiniz.

1. En iyi yöntem olarak StackExchange.Redis istemcisini kullanırken bağlanmak için aşağıdaki deseni kullandığınızdan emin olun.

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

    Daha fazla bilgi için [StackExchange.Redis kullanarak önbelleğe bağlan'a](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)bakın.

1. Sunucunuzun ve istemci uygulamanızın Azure'da aynı bölgede olduğundan emin olun. Örneğin, önbelleğiniz Doğu ABD'deyken, ancak istemci Batı ABD'deyken ve istek `synctimeout` aralıkta tamamlanmadığında veya yerel geliştirme makinenizden hata ayıklarken zaman aşımları alıyor olabilirsiniz. 

    Önbelleğin aynı Azure bölgesinde ve istemcide olması önerilir. Bölgeler arası çağrıları içeren bir senaryonuz varsa, `synctimeout` bağlantı dizesine bir `synctimeout` özellik ekleyerek aralığı varsayılan 1000 ms aralığından daha yüksek bir değere ayarlamanız gerekir. Aşağıdaki örnek, StackExchange.Redis için 2000 ms ile Redis için Azure `synctimeout` Önbelleği tarafından sağlanan bir bağlantı dizesinin bir snippet'ini gösterir.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
1. [StackExchange.Redis NuGet paketinin](https://www.nuget.org/packages/StackExchange.Redis/)en son sürümünü kullandığınızdan emin olun. En son sürüme sahip olmak önemlidir, bu yüzden zaman ekmeleri daha sağlam hale getirmek için kod sürekli sabit hatalar vardır.
1. İstekleriniz sunucu veya istemcideki bant genişliği sınırlamalarına bağlıysa, bunların tamamlanması daha uzun sürer ve zaman aşımına neden olabilir. Zaman aşımınızın sunucudaki ağ bant genişliğinden dolayı olup olmadığını görmek için [Sunucu tarafındaki bant genişliği sınırlamasına](cache-troubleshoot-server.md#server-side-bandwidth-limitation)bakın. Zaman aşımınızın istemci ağ bant genişliğinden dolayı olup olmadığını görmek [için, Istemci tarafındaki bant genişliği sınırlamasına](cache-troubleshoot-client.md#client-side-bandwidth-limitation)bakın.
1. CPU'yu sunucuya mı yoksa istemciye mi bağlı hale getirin?

   - Müşterinizin CPU'ya bağlanıp bağlanmadığınızı kontrol edin. Yüksek CPU, isteğin `synctimeout` aralık içinde işlenmemesi ve isteğin zaman alabına neden olabilir. Daha büyük bir istemci boyutuna taşıma veya yükü dağıtmak bu sorunu denetlemeye yardımcı olabilir.
   - CPU [önbellek performans ölçümünü](cache-how-to-monitor.md#available-metrics-and-reporting-intervals)izleyerek sunucuda CPU bağlanıp bağlanmadığınızı kontrol edin. Redis CPU bağlıyken gelen istekler bu isteklerin zaman alabına neden olabilir. Bu durumu gidermek için, yükü premium önbellekteki birden çok parçaya dağıtabilir veya daha büyük bir boyuta veya fiyatlandırma katmanına yükseltebilirsiniz. Daha fazla bilgi için [Sunucu tarafındaki bant genişliği sınırlaması'na](cache-troubleshoot-server.md#server-side-bandwidth-limitation)bakın.
1. Sunucuda işlenmesi uzun zaman alan komutlar var mı? Redis sunucusunda işlenmesi uzun zaman alan uzun süreli komutlar zaman ayarı yapabilir. Uzun süren komutlar hakkında daha fazla bilgi [için, Uzun süren komutlara](cache-troubleshoot-server.md#long-running-commands)bakın. Redis-cli istemcisini veya [Redis Konsolunu](cache-configure.md#redis-console)kullanarak Redis örneği için Azure Önbelleğinize bağlanabilirsiniz. Ardından, beklenenden daha yavaş istekolup olmadığını görmek için [SLOWLOG](https://redis.io/commands/slowlog) komutunu çalıştırın. Redis Server ve StackExchange.Redis, daha az büyük istek yerine birçok küçük istek için optimize edilebistir. Verilerinizi daha küçük parçalara bölmek burada bazı şeyleri geliştirebilir.

    Redis-cli ve stunnel kullanarak önbelleğinizin SSL bitiş noktasına bağlanma hakkında daha fazla bilgi için, [Redis Preview Release için Oturum Devlet Sağlayıcısı ASP.NET duyuran](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)blog yazısına bakın.
1. Yüksek Redis sunucu yükü zaman adabına neden olabilir. `Redis Server Load` [Önbellek performans ölçümünü](cache-how-to-monitor.md#available-metrics-and-reporting-intervals)izleyerek sunucu yükünü izleyebilirsiniz. 100 sunucu yükü (maksimum değer) redis sunucusunun meşgul olduğunu, boşta kalmadan istekleri ni işlemediğini belirtir. Belirli isteklerin tüm sunucu kapasitesini alıp almamalarını görmek için, önceki paragrafta açıklandığı gibi SlowLog komutunu çalıştırın. Daha fazla bilgi için Yüksek CPU kullanımı / Sunucu Yükü'ne bakın.
1. İstemci tarafında ağ blip'ine neden olabilecek başka bir olay var mıydı? Sık karşılaşılan olaylar şunlardır: istemci örneklerinin sayısını yukarı veya aşağı ölçeklendirme, istemcinin yeni bir sürümünü dağıtma veya otomatik ölçeklendirme etkin. Testlerimizde, otomatik ölçeklendirme veya yukarı/aşağı ölçeklendirmenin giden ağ bağlantısının birkaç saniye liğine kaybolmasına neden olabileceğini bulduk. StackExchange.Redis kodu bu tür olaylara karşı dayanıklıdır ve yeniden bağlanır. Yeniden bağlanma sırasında, kuyruktaki tüm istekler zaman dışında olabilir.
1. Önbelleğe gelen birkaç küçük istekten önce zaman lanmış büyük bir istek var mıydı? Hata iletisindeki parametre, `qs` istemciden sunucuya kaç istek gönderildiğini gösterir, ancak bir yanıtı işlememiş. StackExchange.Redis tek bir TCP bağlantısı kullandığından ve aynı anda yalnızca bir yanıtı okuyabildiği için bu değer büyümeye devam edebilir. İlk işlem zamanlanmış olsa bile, sunucuya veya sunucudan daha fazla verinin gönderilmesini durdurmaz. Diğer istekler, büyük istek tamamlanana kadar engellenir ve zaman alacaktır. Bir çözüm, önbelleğinizin iş yükünüz için yeterince büyük olmasını ve büyük değerleri daha küçük parçalara bölmesini sağlayarak zaman ayırma olasılığını en aza indirmektir. Başka bir olası çözüm istemcinizde `ConnectionMultiplexer` nesnelerin bir havuz kullanmak ve `ConnectionMultiplexer` yeni bir istek gönderirken en az yüklü seçin. Birden çok bağlantı nesnesi arasında yükleme, tek bir zaman anındiğer isteklerin de zaman alamasına neden olmasını önleymelidir.
1. Kullanıyorsanız, `RedisSessionStateProvider`yeniden deneme zaman öncesini doğru ayarladığınızdan emin olun. `retryTimeoutInMilliseconds`daha `operationTimeoutInMilliseconds`yüksek olmalıdır, aksi takdirde hiçbir yeniden deneme oluşur. Aşağıdaki örnekte `retryTimeoutInMilliseconds` 3000 olarak ayarlanır. Daha fazla bilgi [için, Redis için Azure Önbelleği için ASP.NET Oturum Devlet Sağlayıcısı](cache-aspnet-session-state-provider.md) ve Oturum Durumu Sağlayıcısı ve Çıktı [Önbellek Sağlayıcısı'nın yapılandırma parametrelerinin nasıl kullanılacağı](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration)bölümüne bakın.

    ```xml
    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />
    ```

1. Redis sunucusu için Azure Önbelleğinde bellek kullanımını `Used Memory` [kontrol edin](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` ve . Bir çıkarma ilkesi varsa, Redis önbellek boyutuna `Used_Memory` ulaştığında anahtarları tahliye etmeye başlar. İdeal olarak, `Used Memory RSS` sadece biraz daha `Used memory`yüksek olmalıdır . Büyük bir fark bellek parçalanması (iç veya dış) olduğu anlamına gelir. Daha `Used Memory RSS` az `Used Memory`olduğunda, önbellek belleğinin bir parçası nın işletim sistemi tarafından değiştirildiğini gösterir. Bu takas oluşursa, bazı önemli gecikmeler bekleyebilirsiniz. Redis'in ayırmalarının bellek sayfalarına nasıl eşlendiği üzerinde denetimi `Used Memory RSS` olmadığından, yüksek genellikle bellek kullanımındaki ani bir artışın sonucudur. Redis sunucusu belleği boşalttığında, ayırıcı belleği alır, ancak belleği sisteme geri verebilir veya vermeyebilir. İşletim sistemi tarafından `Used Memory` bildirilen değer ve bellek tüketimi arasında bir tutarsızlık olabilir. Bellek Redis tarafından kullanılmış ve serbest bırakılmış olabilir ama sisteme geri verilmedi. Bellek sorunlarını azaltmaya yardımcı olmak için aşağıdaki adımları yapabilirsiniz:

   - Sistemdeki bellek sınırlamalarına karşı çalışmamak için önbelleği daha büyük bir boyuta yükseltin.
   - Eski değerlerin proaktif olarak tahliye edilebilmeleri için anahtarlarda son kullanma sürelerini ayarlayın.
   - Önbellek `used_memory_rss` ölçümünü izleyin. Bu değer önbelleklerinin boyutuna yaklaştığında, performans sorunlarını görmeye başlama olasılığınız yüksektir. Premium önbellek kullanıyorsanız verileri birden çok parçaya dağıtın veya daha büyük bir önbellek boyutuna yükseltin.

   Daha fazla bilgi için [Redis sunucusundaki Bellek basıncına](cache-troubleshoot-server.md#memory-pressure-on-redis-server)bakın.

## <a name="additional-information"></a>Ek bilgiler

- [Redis için Azure Cache istemci tarafı sorunlarını giderme](cache-troubleshoot-client.md)
- [Redis için Azure Cache sunucu tarafı sorunlarını giderme](cache-troubleshoot-server.md)
- [Önbelleğimin performansını nasıl kıyaslayabilir ve test edebilirim?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
- [Redis için Azure Önbelleği nasıl izlenir?](cache-how-to-monitor.md)
