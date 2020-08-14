---
title: Redsıs yönetimi hakkında Azure önbelleği SSS
description: Redsıs için Azure önbelleğini yönetmenize yardımcı olacak yaygın soruların yanıtlarını öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 08/06/2020
ms.openlocfilehash: 5c5c7a5adae9891f764f714d1700c6024376de02
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88205391"
---
# <a name="azure-cache-for-redis-management-faqs"></a>Redsıs yönetimi hakkında Azure önbelleği SSS
Bu makalede, redin için Azure önbelleğini yönetme hakkında sık sorulan soruların yanıtları sağlanır.

## <a name="common-questions-and-answers"></a>Yaygın sorular ve yanıtları
Bu bölümde aşağıdaki SSS ele alınmaktadır:

* [Redsıs 'e bağlanmak için TLS olmayan/SSL bağlantı noktasını ne zaman etkinleştirmem gerekir?](#when-should-i-enable-the-non-tlsssl-port-for-connecting-to-redis)
* [Üretim en iyi uygulamaları nelerdir?](#what-are-some-production-best-practices)
* [Ortak Redi komutları kullanılırken dikkat edilecek noktalar nelerdir?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Önbelleğim performansını nasıl kıyaslarım ve test edebilirim?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [İş parçacığı büyümesi hakkında önemli ayrıntılar](#important-details-about-threadpool-growth)
* [StackExchange. Redsıs kullanırken istemci üzerinde daha fazla aktarım sağlamak için sunucu GC 'yi etkinleştirin](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Bağlantılar etrafında performans konuları](#performance-considerations-around-connections)

### <a name="when-should-i-enable-the-non-tlsssl-port-for-connecting-to-redis"></a>Redsıs 'e bağlanmak için TLS olmayan/SSL bağlantı noktasını ne zaman etkinleştirmem gerekir?
Redsıs sunucusu, TLS 'yi yerel olarak desteklemez, ancak Redsıs için Azure önbelleği olur. Redsıs için Azure önbelleğine bağlanıyorsanız ve istemciniz, StackExchange. Redsıs gibi TLS 'yi destekliyorsa, TLS kullanmanız gerekir.

>[!NOTE]
>TLS olmayan bağlantı noktası, Redsıs örnekleri için yeni Azure önbelleği için varsayılan olarak devre dışıdır. İstemciniz TLS 'yi desteklemiyorsa, [redin Için Azure önbelleğinde önbellek yapılandırma](cache-configure.md) makalesinin [erişim bağlantı noktaları](cache-configure.md#access-ports) bölümündeki yönergeleri izleyerek TLS olmayan bağlantı noktasını etkinleştirmeniz gerekir.
>
>

Bu gibi redsıs araçları `redis-cli` , TLS bağlantı noktasıyla çalışmaz, ancak `stunnel` [Redsıs önizleme sürümü blog gönderisine Ilişkin ASP.NET oturum durumu sağlayıcısında](https://devblogs.microsoft.com/aspnet/announcing-asp-net-session-state-provider-for-redis-preview-release/) sunulan YÖNERGELERI izleyerek, araçları TLS bağlantı noktasına güvenli bir şekilde bağlamak için gibi bir yardımcı programı kullanabilirsiniz.

Redsıs araçlarını indirme yönergeleri için bkz. [redsıs komutlarını nasıl çalıştırabilirim?](cache-development-faq.md#how-can-i-run-redis-commands) bölümü.

### <a name="what-are-some-production-best-practices"></a>Üretim en iyi uygulamaları nelerdir?
* [StackExchange. Redsıs en iyi uygulamaları](#stackexchangeredis-best-practices)
* [Yapılandırma ve kavramlar](#configuration-and-concepts)
* [Performansı test etme](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange. Redsıs en iyi uygulamaları
* Yanlış olarak ayarlayın `AbortConnect` , ardından Connectionçoğullayıcı 'nın otomatik olarak yeniden bağlanmasına izin verin. [Ayrıntılar için buraya bakın](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* Connectionçoğullayıcısı yeniden kullanma-her istek için yeni bir tane oluşturmayın. `Lazy<ConnectionMultiplexer>` [Burada gösterilen](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) model önerilir.
* Red, daha küçük değerlerle çalışır, bu nedenle daha büyük verileri birden fazla anahtara chopping değerlendirin. [Bu redin tartışmasında](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)100 KB büyük olarak değerlendirilir. Büyük değerlerden kaynaklanmış olabilecek örnek bir sorun için [Bu makaleyi](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) okuyun.
* Zaman aşımlarını önlemek için iş [parçacığı ayarlarınızı](#important-details-about-threadpool-growth) yapılandırın.
* En azından 5 saniyelik varsayılan connectTimeout değerini kullanın. Bu Aralık, StackExchange 'e, bir ağ BLIP durumunda bağlantıyı yeniden kurmak için yeterli zaman sağlar.
* Çalıştırdığınız farklı işlemlerle ilişkili performans maliyetlerinden haberdar olun. Örneğin, `KEYS` komut bir O (n) işlemidir ve kaçınılması gerekir. [Redis.io sitesinde](https://redis.io/commands/) , desteklediği her bir işlem için zaman karmaşıklığı etrafında ayrıntılar bulunur. Her bir işlemin karmaşıklığını görmek için her bir komuta tıklayın.

#### <a name="configuration-and-concepts"></a>Yapılandırma ve kavramlar
* Üretim sistemleri için standart veya Premium katmanını kullanın. Temel Katman, veri çoğaltma ve SLA bulunmayan tek düğümlü bir sistemdir. Ayrıca en az C1 önbelleği kullanın. C0 önbellekleri genellikle basit geliştirme ve test senaryoları için kullanılır.
* Redthe 'ın **bellek içi** veri deposu olduğunu unutmayın. Veri kaybının gerçekleşebileceği senaryolara dikkat etmeniz için [Bu makaleyi](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) okuyun.
* [Düzeltme eki uygulama ve yük devretme nedeniyle](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md), sisteminizi, bağlantı bliıt 'leri işleyebilecek şekilde geliştirin.

#### <a name="performance-testing"></a>Performansı test etme
* `redis-benchmark.exe`Kendi performans testlerinizi yazmadan önce olası aktarım hızına yönelik bir fikir sahibi olmak için kullanarak başlayın. `redis-benchmark`, TLS 'yi desteklemediğinden, testi çalıştırmadan önce [Azure Portal aracılığıyla TLS olmayan bağlantı noktasını etkinleştirmeniz](cache-configure.md#access-ports) gerekir. Örnekler için bkz. [önbelleğim performansını nasıl ücretlendirilirim ve test edebilirim?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* Test için kullanılan istemci sanal makinesi, Redsıs örneği için Azure önbelleğiniz ile aynı bölgede olmalıdır.
* Daha iyi donanımlar olduğundan ve en iyi sonuçları vermesi için istemciniz için dv2 VM serisini kullanmanızı öneririz.
* Seçtiğiniz önbellek için en az bilgi işlem ve bant genişliği özelliği olan seçtiğiniz istemci VM 'nizin olduğundan emin olun.
* Windows kullanıyorsanız, istemci makinesinde VRSS 'i etkinleştirin. [Ayrıntılar için buraya bakın](https://technet.microsoft.com/library/dn383582.aspx).
* Premium katman Redsıs örnekleri, hem CPU hem de ağ için daha iyi donanımlar üzerinde çalıştıkları için daha iyi ağ gecikmesi ve üretilen iş hızına sahiptir.

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Ortak Redi komutları kullanılırken dikkat edilecek noktalar nelerdir?

* Bu komutların etkisini tam olarak anlamadığınız sürece, tamamlanması uzun süren bazı REDI komutlarının kullanmaktan kaçının. Örneğin, üretimde [anahtarlar](https://redis.io/commands/keys) komutunu çalıştırmayın. Anahtar sayısına bağlı olarak, döndürülmesi uzun sürebilir. Redsıs tek iş parçacıklı bir sunucusudur ve komutları tek seferde işler. Anahtarlar sonrasında verilen başka komutlarınız varsa, Reduntil anahtarlar komutunu işleyene kadar bunlar işlenmeyecektir. [Redis.io sitesinde](https://redis.io/commands/) , desteklediği her bir işlem için zaman karmaşıklığı etrafında ayrıntılar bulunur. Her bir işlemin karmaşıklığını görmek için her bir komuta tıklayın.
* Anahtar boyutları-küçük anahtar/değer veya büyük anahtar/değer kullanmalı mıyım? Senaryoya bağlıdır. Senaryonuz daha büyük anahtarlar gerektiriyorsa, ConnectionTimeout 'yi ayarlayabilir, sonra değerleri yeniden deneyebilir ve yeniden deneme mantığınızı ayarlayabilirsiniz. Redsıs sunucu perspektifinden, daha küçük değerler daha iyi performans sağlar.
* Bu noktalar, daha büyük değerleri redin içinde depolayamayacağınız anlamına gelmez; Aşağıdaki noktalara dikkat etmeniz gerekir. Gecikme süreleri daha yüksek olacaktır. Daha büyük bir veri kümesi ve daha küçük olan bir veri kümesine sahipseniz, her biri farklı bir zaman aşımı kümesiyle ve yeniden deneme değerleriyle yapılandırılmış ve bu, önceki [StackExchange. redsıs yapılandırma seçenekleri](cache-development-faq.md#what-do-the-stackexchangeredis-configuration-options-do) bölümünde açıklandığı gibi, birden çok connectionçoğullayıcı örneği kullanabilirsiniz.

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Önbelleğim performansını nasıl kıyaslarım ve test edebilirim?
* Önbelleğinizin sistem durumunu [izleyebilmeniz](cache-how-to-monitor.md) için [önbellek tanılamayı etkinleştirin](cache-how-to-monitor.md#enable-cache-diagnostics). Azure portalında ölçümleri görüntüleyebilir ve ayrıca istediğiniz araçları kullanarak bunları [indirebilir ve gözden geçirebilirsiniz](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring).
* Redsıs sunucunuza yük testi uygulamak için redis-benchmark.exe kullanabilirsiniz.
* Redin yük testi istemcisinin ve Azure önbelleğinin aynı bölgede bulunduğundan emin olun.
* redis-cli.exe kullanın ve ıNFO komutunu kullanarak önbelleği izleyin.
* Yüklemeniz yüksek bellek parçalanmasına neden oluyorsa, daha büyük bir önbellek boyutuna kadar ölçeklendirmelisiniz.
* Redsıs araçlarını indirme yönergeleri için bkz. [redsıs komutlarını nasıl çalıştırabilirim?](cache-development-faq.md#how-can-i-run-redis-commands) bölümü.

Aşağıdaki komutlar redis-benchmark.exe kullanımına bir örnek sağlar. Doğru sonuçlar için, bu komutları önbelleğiniz ile aynı bölgedeki bir VM 'den çalıştırın.

* Bir 1000 yükü kullanarak ardışık, ardışık iş kümesi isteklerini test etme

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Test ardışık düzen bir 1 k yük kullanarak istekleri al.

>[!NOTE]
> Önbelleği doldurmak için önce yukarıda gösterilen küme testini çalıştırın
>

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

### <a name="important-details-about-threadpool-growth"></a>İş parçacığı büyümesi hakkında önemli ayrıntılar
CLR ThreadPool 'ın iki tür iş parçacığı vardır-"Worker" ve "g/ç tamamlama bağlantı noktası" (IOCP) iş parçacıkları.

* Çalışan iş parçacıkları, veya yöntemlerini işlemek gibi şeyler için kullanılır `Task.Run(…)` `ThreadPool.QueueUserWorkItem(…)` . Bu iş parçacıkları, bir arka plan iş parçacığında iş olması gerektiğinde CLR içindeki çeşitli bileşenler tarafından da kullanılır.
* ICP iş parçacıkları, ağdan okurken olduğu gibi zaman uyumsuz GÇ gerçekleştiğinde kullanılır.

İş parçacığı havuzu, her bir iş parçacığı türü için "En düşük" ayarına ulaşıncaya kadar isteğe bağlı olarak (herhangi bir daraltma olmadan) yeni çalışan iş parçacıkları veya g/ç Tamamlama iş parçacıkları sağlar. Varsayılan olarak, en az sayıda iş parçacığı bir sistemdeki işlemci sayısına ayarlanır.

Mevcut (meşgul) iş parçacıklarının sayısı "en az" iş parçacığı sayısına ulaştıktan sonra, ThreadPool, yeni iş parçacıklarını 500 milisaniye başına bir iş parçacığına çıkarma oranını ortadan caktır. Genellikle, sisteminiz bir ıOCP iş parçacığına ihtiyaç duyan bir iş patlaması alırsa, bu çalışmayı hızla işler. Ancak, iş patlaması yapılandırılmış "En düşük" ayarından daha büyükse, iş parçacığı, iki şeyin oluşmasını beklediği için bazı işlerin işlenmesinde bir gecikme olur.

* Mevcut bir iş parçacığı çalışmayı işlemek için ücretsiz hale gelir.
* 500 MS için mevcut bir iş parçacığı yok, bu nedenle yeni bir iş parçacığı oluşturulur.

Temel olarak, meşgul iş parçacıklarının sayısı en az iş parçacığından fazlaysa, büyük olasılıkla ağ trafiğinin uygulama tarafından işlenebilmesi için 500-ms gecikme süresi ödemelerinizin olması anlamına gelir. Ayrıca, var olan bir iş parçacığının 15 saniyeden uzun süre boşta kaldığı durumlarda (hatırlıyorum), temizlendiğini ve bu büyüme ve azalma döngüsünün yinelenebileceğini unutmayın.

StackExchange. Redsıs 'den bir örnek hata iletisi görüyoruz (derleme 1.0.450 veya sonrası), şimdi de iş parçacığı istatistiklerini (bkz. ıOCP ve çalışan ayrıntıları aşağıda verilmiştir) yazdıracaksınız.

```
System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
IOCP: (Busy=6,Free=994,Min=4,Max=1000),
WORKER: (Busy=3,Free=997,Min=4,Max=1000)
```

Önceki örnekte, ıOCP iş parçacığında altı meşgul iş parçacığı olduğunu ve sistemin dört en düşük iş parçacığına izin verecek şekilde yapılandırıldığını görebilirsiniz. Bu durumda, 6 > 4 nedeniyle istemci muhtemelen 2 500 ms gecikme süresi görmüştür.

ICP veya çalışan iş parçacıklarının büyümesi kısıtlandığında, StackExchange. redin zaman aşımları ile karşılaşacağını unutmayın.

#### <a name="recommendation"></a>Öneri

Bu bilgiler verildiğinde, müşterilerin IOCP ve çalışan iş parçacıkları için en düşük yapılandırma değerini varsayılan değerden daha büyük bir değere ayarlaması önemle önerilir. Bir uygulamanın doğru değeri başka bir uygulama için büyük olasılıkla çok yüksek veya düşük olacağı için, bu değerin ne olması gerektiğine ilişkin bir veya daha fazla uygun olmayan tek bir kılavuza izin vermiyoruz. Bu ayar ayrıca, karmaşık uygulamaların diğer bölümlerinin performansını etkileyebilir, böylece her müşterinin bu ayarı kendi özel gereksinimlerine göre hassas bir şekilde ayarlayabilmeleri gerekir. İyi bir başlangıç yeri 200 veya 300, sonra da test ve ince ayar gerekir.

Bu ayar nasıl yapılandırılır:

* İçindeki [ThreadPool. SetMinThreads (...)](/dotnet/api/system.threading.threadpool.setminthreads#System_Threading_ThreadPool_SetMinThreads_System_Int32_System_Int32_) metodunu kullanarak bu ayarı programlı bir şekilde değiştirmenizi öneririz `global.asax.cs` . Örnek:

    ```csharp
    private readonly int minThreads = 200;
    void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on application startup
        AreaRegistration.RegisterAllAreas();
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ThreadPool.SetMinThreads(minThreads, minThreads);
    }
    ```

    > [!NOTE]
    > Bu yöntem tarafından belirtilen değer genel bir ayardır ve tüm AppDomain etki alanı etkilendi. Örneğin, 4 çekirdekli bir makineniz varsa ve çalışma zamanı sırasında *MinWorkerThreads* ve *MINIOTHREADS* 'i CPU başına 50 olarak ayarlamak Istiyorsanız, **ThreadPool. SetMinThreads (200, 200)** kullanın.

* En düşük iş parçacığı ayarlarını, içindeki yapılandırma öğesinin altında bulunan, [ *miniothreads* veya *MinWorkerThreads* yapılandırma ayarı](https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx) kullanılarak belirlemek de mümkündür `<processModel>` `Machine.config` `%SystemRoot%\Microsoft.NET\Framework\[versionNumber]\CONFIG\` . **Bu şekilde en düşük iş parçacığı sayısını bu şekilde ayarlamak, sistem genelinde bir ayar olduğundan genellikle önerilmez.**

  > [!NOTE]
  > Bu yapılandırma öğesinde belirtilen değer, *çekirdek başına* ayardır. Örneğin, 4 çekirdekli bir makineniz varsa ve *Miniothreads* ayarınızı çalışma zamanında 200 olarak istiyorsanız, kullanabilirsiniz `<processModel minIoThreads="50"/>` .
  >

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>StackExchange. Redsıs kullanırken istemci üzerinde daha fazla aktarım sağlamak için sunucu GC 'yi etkinleştirin
Sunucu GC 'yi etkinleştirmek, istemcisini iyileştirebilirler ve StackExchange. Redsıs kullanırken daha iyi performans ve verimlilik sağlar. Sunucu GC ve nasıl etkinleştirileceği hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Sunucu GC 'yi etkinleştirmek için](/dotnet/framework/configure-apps/file-schema/runtime/gcserver-element)
* [Atık Toplamanın Temelleri](/dotnet/standard/garbage-collection/fundamentals)
* [Çöp toplama ve performans](/dotnet/standard/garbage-collection/performance)

### <a name="performance-considerations-around-connections"></a>Bağlantılar etrafında performans konuları

Her fiyatlandırma katmanı, istemci bağlantıları, bellek ve bant genişliği için farklı sınırlara sahiptir. Her önbellek boyutu belirli sayıda bağlantıya *izin verdiğinden,* redde her bağlantı ile ilişkili ek yük vardır. Bu ek yükün bir örneği, TLS/SSL şifreleme sonucu olarak CPU ve bellek kullanımı olabilir. Belirli bir önbellek boyutu için en fazla bağlantı sınırı, hafif bir şekilde yüklenmiş önbelleği varsayar. Bağlantı yüklerinden yükleme, istemci *işlemlerinden yükleme,* sistem kapasitesini aşarsa, geçerli önbellek boyutu için bağlantı sınırını aşmasanız bile önbellek, kapasite sorunları yaşayabilir.

Her katmana ait farklı bağlantı sınırları hakkında daha fazla bilgi için bkz. [redsıs fiyatlandırması Için Azure önbelleği](https://azure.microsoft.com/pricing/details/cache/). Bağlantılar ve diğer varsayılan yapılandırmalar hakkında daha fazla bilgi için bkz. [varsayılan redsıs sunucu yapılandırması](cache-configure.md#default-redis-server-configuration).

## <a name="next-steps"></a>Sonraki adımlar

[Redsıs SSS için diğer Azure önbelleği](cache-faq.md)hakkında bilgi edinin.
