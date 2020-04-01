---
title: Redis için Azure Cache'in en iyi yöntemleri
description: Bu en iyi uygulamaları izleyerek Azure Önbelleğinizi Redis için nasıl kullanacağınızı öğrenin.
author: joncole
ms.service: cache
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: joncole
ms.openlocfilehash: 105a3996753a1d1c2d71846cc8bad574e4498acf
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478616"
---
# <a name="best-practices-for-azure-cache-for-redis"></a>Redis için Azure Cache'in en iyi yöntemleri 
Bu en iyi uygulamaları izleyerek, Redis örneğinde Azure Önbelleğinizin performansını ve uygun maliyetli kullanımını en üst düzeye çıkarmaya yardımcı olabilirsiniz.

## <a name="configuration-and-concepts"></a>Yapılandırma ve kavramlar
 * **Üretim sistemleri için Standart veya Premium katmanı kullanın.**  Temel katman, veri çoğaltma ve SLA olmayan tek bir düğüm sistemidir. Ayrıca en az C1 önbelleği kullanın.  C0 önbellekleri, paylaşılan bir CPU çekirdeğine, az belleğe sahip oldukları ndan ve "gürültülü komşu" sorunlarına eğilimli olduklarından basit geliştirme/test senaryoları içindir.

 * **Redis'in bir bellek içi veri deposu olduğunu unutmayın.**  [Bu makalede,](cache-troubleshoot-data-loss.md) veri kaybı oluşabilir bazı senaryolar özetliyor.

 * [Yama ve arıza nedeniyle](cache-failover.md) **bağlantı blips işleyebilir şekilde sisteminizi geliştirin.**

 * Bellek basıncı koşullarında **sistem yanıt verme durumunu iyileştirmek için maksimum bellek [rezerve edilmiş ayarınızı](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) yapılandırın.**  Yazma ağır iş yükleri için veya Redis'te daha büyük değerleri (100 KB veya daha fazla) depolıyorsanız, yeterli rezervasyon ayarı özellikle önemlidir. Önbelleğinizin boyutunun %10'uyla başlamalı ve yazma ağırlıklı yüklerin varsa bu yüzdeyi artırmalısınız.

 * **Redis daha küçük değerlerle en iyi şekilde çalışır,** bu nedenle daha büyük verileri birden çok tuşa doğramayı düşünün.  [Bu Redis tartışma,](https://stackoverflow.com/questions/55517224/what-is-the-ideal-value-size-range-for-redis-is-100kb-too-large/)bazı hususlar dikkatle dikkate almalısınız listelenir.  Büyük değerlerin neden olabileceği örnek bir sorun için [bu makaleyi](cache-troubleshoot-client.md#large-request-or-response-size) okuyun.

 * **Önbellek örneğini ve uygulamanızı aynı bölgede bulun.**  Farklı bir bölgedeki önbelleğe bağlanmak gecikme süresini önemli ölçüde artırabilir ve güvenilirliği azaltabilir.  Azure dışından bağlanabiliyor ken, *özellikle Redis'i önbellek olarak kullanırken*önerilmez.  Redis'i yalnızca bir anahtar/değer deposu olarak kullanıyorsanız, gecikme birincil sorun olmayabilir. 

 * **Bağlantıları yeniden kullanın.**  Yeni bağlantılar oluşturmak pahalıdır ve gecikme yi artırır, bu nedenle bağlantıları mümkün olduğunca yeniden kullanın. Yeni bağlantılar oluşturmayı seçerseniz, eski bağlantıları yayınlamadan önce kapattığınızdan emin olun (.NET veya Java gibi yönetilen bellek dillerinde bile).

 * **İstemci kitaplığınızı en az 15 *saniyelik bir bağlantı zaman aşını* kullanacak şekilde yapılandırArak**sisteme daha yüksek CPU koşullarında bile bağlanmak için zaman tanıyın.  Küçük bir bağlantı zaman aşımı değeri, bağlantının bu zaman diliminde kurulduğunu garanti etmez.  Bir şey yanlış giderse (yüksek istemci CPU, yüksek sunucu CPU, vb) sonra kısa bir bağlantı zaman dışarı değeri bağlantı girişimi başarısız neden olur. Bu davranış genellikle kötü bir durum daha kötü hale getirir.  Daha kısa zaman aşımları, yardımcı olmak yerine, sistemi yeniden bağlanmaya çalışma işlemini yeniden başlatmaya zorlayarak sorunu daha da kötüleştirebilir ve bu da *bir bağlantı -> başarısız -> yeniden deneme* döngüsüne yol açabilir. Genellikle bağlantı zaman dışından 15 saniye veya daha yüksek bir hızda ayrılmanızı öneririz. Bağlantı girişiminizin 15 veya 20 saniye sonra başarılı olmasına izin vermek, bağlantı girişiminizin hızlı bir şekilde yeniden denemede başarısız olmasından daha iyidir. Böyle bir yeniden deneme döngüsü, kesintinizin başlangıçta sistemin daha uzun sürmesine izin vermenizden daha uzun sürmesine neden olabilir.  
     > [!NOTE]
     > Bu *kılavuz, bağlantı girişimine* özgüdür ve GET veya SET gibi bir *işlemin* tamamlanmasını beklemek istediğiniz saatle ilgili değildir.
 
 * **Pahalı işlemlerden kaçının** - [KEYS](https://redis.io/commands/keys) komutu gibi bazı Redis işlemleri *çok* pahalıdır ve kaçınılmalıdır.  Daha fazla bilgi için, [uzun süren komutlar](cache-troubleshoot-server.md#long-running-commands) etrafında bazı hususlara bakın

 * **TLS şifrelemesi kullanın** - Redis için Azure Önbelleği varsayılan olarak TLS şifreli iletişim gerektirir.  TLS sürümleri 1.0, 1.1 ve 1.2 şu anda desteklenmiştir.  Ancak, TLS 1.0 ve 1.1 endüstri çapında amortismana giden bir yoldadır, bu nedenle mümkünse TLS 1.2'yi kullanın.  İstemci kitaplığınız veya aracınız TLS'yi desteklemiyorsa, şifrelenmemiş bağlantıları etkinleştirme Azure portalı veya [yönetim API'leri](https://docs.microsoft.com/rest/api/redis/redis/update) [aracılığıyla](cache-configure.md#access-ports) yapılabilir.  Şifreli bağlantıların mümkün olmadığı durumlarda, önbelleğinizi ve istemci uygulamanızı sanal bir ağa yerleştirmeniz önerilir.  Sanal ağ önbelleği senaryosunda hangi bağlantı noktalarının kullanıldığı hakkında daha fazla bilgi için bu [tabloya](cache-how-to-premium-vnet.md#outbound-port-requirements)bakın.
 
## <a name="memory-management"></a>Bellek yönetimi
Redis sunucu örneğinde dikkate almak isteyebileceğin bellek kullanımıyla ilgili birkaç şey vardır.  İşte birkaçı:

 * **Uygulamanız için çalışan bir [tahliye ilkesi](https://redis.io/topics/lru-cache) seçin.**  Azure Redis için varsayılan ilke *geçici-lru'dur,* bu da yalnızca TTL değeri kümesi olan anahtarların tahliye için uygun olacağı anlamına gelir.  Hiçbir anahtarın TTL değeri yoksa, sistem hiçbir anahtarı çıkarmaz.  Eğer sistem bellek baskısı altında herhangi bir anahtar tahliye edilmesine izin istiyorsanız, o zaman *allkeys-lru* ilkesini dikkate almak isteyebilirsiniz.

 * **Anahtarlarınızın son kullanma değerini ayarlayın.**  Son kullanma süresi, bellek basıncı olana kadar beklemek yerine anahtarları proaktif olarak kaldırır.  Bellek basıncı nedeniyle tahliye devreye girdiğinde, sunucunuza ek yük neden olabilir.  Daha fazla bilgi için [EXPIRE](https://redis.io/commands/expire) ve [EXPIREAT](https://redis.io/commands/expireat) komutları için belgelere bakın.
 
## <a name="client-library-specific-guidance"></a>İstemci kitaplığı özel rehberlik
 * [StackExchange.Redis (.NET)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-stackexchange-redis-md)
 * [Java - Hangi istemciyi kullanmalıyım?](https://gist.github.com/warrenzhu25/1beb02a09b6afd41dff2c27c53918ce7#file-azure-redis-java-best-practices-md)
 * [Marul (Java)](https://gist.github.com/warrenzhu25/181ccac7fa70411f7eb72aff23aa8a6a#file-azure-redis-lettuce-best-practices-md)
 * [Jedi'lar (Java)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-java-jedis-md)
 * [Node.js](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-node-js-md)
 * [PHP](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-php-md)
 * [Asp.Net Oturum Devlet Sağlayıcısı](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-session-state-provider-md)


## <a name="when-is-it-safe-to-retry"></a>Yeniden denemek ne zaman güvenli?
Ne yazık ki, kolay bir cevap yok.  Her uygulamanın hangi işlemlerin yeniden denenip hangilerini yeniden deneyeceğine karar vermesi gerekir.  Her işlemin farklı gereksinimleri ve anahtarlar arası bağımlılıkları vardır.  Göz önünde bulundurmanız gereken bazı şeyler şunlardır:

 * Redis çalışmasını istediğiniz komutu başarıyla çalıştırmış olsa bile istemci tarafı hataları alabilirsiniz.  Örnek:
     - Zaman zaman ları istemci tarafı bir kavramdır.  İşlem sunucuya ulaştıysa, istemci beklemeyi vazgeçse bile sunucu komutu çalıştıracaktır.  
     - Soket bağlantısında bir hata oluştuğunda, işlemin gerçekten sunucuda olup olmadığını bilmek mümkün değildir.  Örneğin, bağlantı hatası sunucu isteği işledikten sonra ancak istemci yanıtı almadan önce gerçekleşebilir.
 *  Yanlışlıkla aynı işlemi iki kez çalıştırarsam uygulamam nasıl tepki verir?  Örneğin, bir kere yerine iki kez bir kisa daarttırırsam ne olur?  Başvurum birden fazla yerden aynı anahtara mı yazıyor?  Yeniden deneme mantığım, uygulamamın başka bir bölümü tarafından ayarlanan bir değerin üzerine yazarsa ne olur?

Kodunuzu hata koşullarında nasıl çalıştığını test etmek istiyorsanız, [Yeniden Başlat özelliğini](cache-administration.md#reboot)kullanmayı düşünün. Yeniden başlatma, bağlantı blips'inin uygulamanızı nasıl etkilediğini görmenizi sağlar.

## <a name="performance-testing"></a>Performansı test etme
 * Kendi perf testlerinizi yazmadan önce olası bir iş sonu/gecikme sebebleri için bir his almak için ** `redis-benchmark.exe` kullanmaya başlayın.**  Redis-benchmark belgeleri [burada bulabilirsiniz.](https://redis.io/topics/benchmarks)  Redis-benchmark'ın TLS'yi desteklemediğini unutmayın, bu nedenle testi çalıştırmadan önce [Portal üzerinden TLS olmayan bağlantı noktasını etkinleştirmeniz](cache-configure.md#access-ports) gerekir.  [redis-benchmark.exe bir windows uyumlu sürümü burada bulabilirsiniz](https://github.com/MSOpenTech/redis/releases)
 * Sınama için kullanılan VM istemcisi Redis önbellek örneğinizle **aynı bölgede** olmalıdır.
 * Onlar daha iyi donanım alabildikleri ve en iyi sonuçları verecektir gibi biz müşteri için **Dv2 VM Serisi kullanmanızı öneririz.**
 * Kullandığınız istemci VM *en*az* önbellek test edilen kadar işlem ve bant genişliği ne kadar olduğundan emin olun. 
 * Windows'daysanız istemci makinesinde **VRSS'yi etkinleştirin.**  [Ayrıntılar için buraya bakın.](https://technet.microsoft.com/library/dn383582(v=ws.11).aspx)  Örnek powershell komut dosyası:
     >PowerShell -ExecutionPolicy Sınırsız Enable-NetAdapterRSS -Adı ( Get-NetAdapter). Adı 
     
 * **Premium katman Redis örneklerini kullanmayı düşünün.**  Hem CPU hem de Ağ için daha iyi donanımüzerinde çalıştıklarından, bu önbellek boyutları daha iyi ağ gecikmesi ve iş ortalığına sahip olur.
 
     > [!NOTE]
     > Gözlenen performans sonuçlarımız referansınız için [burada yayınlanmaktadır.](cache-faq.md#azure-cache-for-redis-performance)   Ayrıca, SSL/TLS'nin bazı ek ek yükü eklediğiiçin aktarım şifrelemesi kullanıyorsanız farklı gecikmeler ve/veya iş metotları alabilirsiniz.
 
### <a name="redis-benchmark-examples"></a>Redis-Benchmark örnekleri
**Ön test kurulumu**: Önbellek örneğini, aşağıda listelenen gecikme ve iş sonu test komutları için gerekli verilerle hazırlayın.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t SET -n 10 -d 1024 

**Gecikmeyi test etmek için**: 1k yük kullanarak GET isteklerini test edin.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t GET -d 1024 -P 50 -c 4

**Iş ın test etmek için:** 1k yükü ile pipelined GET istekleri.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t GET -n 1000000 -d 1024 -P 50 -c 50
