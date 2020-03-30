---
title: Redis için Azure Önbelleği SSS
description: Redis için Azure Önbelleği için sık sorulan soruların, desenlerin ve en iyi uygulamaların yanıtlarını öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: ddf7999153e9d9722e627d148b116750fe3aaecf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278720"
---
# <a name="azure-cache-for-redis-faq"></a>Redis için Azure Önbelleği SSS
Redis için Azure Önbelleği için sık sorulan soruların, desenlerin ve en iyi uygulamaların yanıtlarını öğrenin.

## <a name="what-if-my-question-isnt-answered-here"></a>Ya sorum burada cevaplandırılmazsa?
Sorunuz burada listelenmemişse, bize bildirin ve bir yanıt bulmanıza yardımcı olalım.

* Bu SSS'nin sonundaki yorumlarda bir soru yayınlayabilir ve bu makale hakkında Azure Önbellek ekibi ve diğer topluluk üyeleriyle iletişim kurabilirsiniz.
* Daha geniş bir kitleye ulaşmak için [Azure Önbellek MSDN Forumu'nda](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) bir soru yayınlayabilir ve Azure Önbellek ekibi yle ve topluluğun diğer üyeleriyle etkileşimkurabilirsiniz.
* Bir özellik isteği nde bulunmak istiyorsanız, isteklerinizi ve fikirlerinizi [Redis Kullanıcı Sesi için Azure Önbelleğine](https://feedback.azure.com/forums/169382-cache)gönderebilirsiniz.
* Ayrıca [Azure Önbellek Dış Geribildirim](mailto:azurecache@microsoft.com)de bize bir e-posta gönderebilirsiniz.

## <a name="azure-cache-for-redis-basics"></a>Redis temelleri için Azure Önbelleği
Bu bölümdeki SSS' ler, Redis için Azure Önbelleği'nin bazı temellerini kapsar.

* [Redis için Azure Cache nedir?](#what-is-azure-cache-for-redis)
* [Redis için Azure Önbelleği'ne nasıl başlarım?](#how-can-i-get-started-with-azure-cache-for-redis)

Aşağıdaki SSS'ler, Redis için Azure Önbelleği ile ilgili temel kavramları ve soruları kapsar ve diğer SSS bölümlerinden birinde yanıtlanır.

* [Redis sunan ve boyutu için hangi Azure Önbelleğini kullanmalıyım?](#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Redis istemcileri için hangi Azure Önbelleğini kullanabilirim?](#what-azure-cache-for-redis-clients-can-i-use)
* [Redis için Azure Önbellek için yerel bir emülatör var mı?](#is-there-a-local-emulator-for-azure-cache-for-redis)
* [Önbelleğimin durumunu ve performansını nasıl izleyebilirim?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Planlama SSS
* [Redis sunan ve boyutu için hangi Azure Önbelleğini kullanmalıyım?](#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Redis performansı için Azure Önbelleği](#azure-cache-for-redis-performance)
* [Önbelleğimi hangi bölgede bulmalıyım?](#in-what-region-should-i-locate-my-cache)
* [Redis için Azure Önbelleği için nasıl faturalandırılırım?](#how-am-i-billed-for-azure-cache-for-redis)
* [Azure Kamu Bulutu, Azure Çin Bulutu veya Microsoft Azure Almanya ile Redis için Azure Önbelleğini kullanabilir miyim?](#can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>Geliştirme SSS'leri
* [StackExchange.Redis yapılandırma seçenekleri ne yapar?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Redis istemcileri için hangi Azure Önbelleğini kullanabilirim?](#what-azure-cache-for-redis-clients-can-i-use)
* [Redis için Azure Önbellek için yerel bir emülatör var mı?](#is-there-a-local-emulator-for-azure-cache-for-redis)
* [Redis komutlarını nasıl çalıştırabilirim?](#how-can-i-run-redis-commands)
* [Redis için Azure Önbelleği'nde neden diğer Azure hizmetleri gibi BIR MSDN sınıfı kitaplığı başvurusu yok?](#why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Redis için Azure Önbelleğini PHP oturum önbelleği olarak kullanabilir miyim?](#can-i-use-azure-cache-for-redis-as-a-php-session-cache)
* [Redis veritabanları nedir?](#what-are-redis-databases)

## <a name="security-faqs"></a>Güvenlik SSS'leri
* [Redis'e bağlanmak için SSL olmayan bağlantı noktasını ne zaman etkinleştirmeliyim?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Üretim SSS'leri
* [Bazı üretim en iyi uygulamaları nelerdir?](#what-are-some-production-best-practices)
* [Ortak Redis komutlarını kullanırken dikkat edilmesi gereken hususlar nelerdir?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Önbelleğimin performansını nasıl kıyaslayabilir ve test edebilirim?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [ThreadPool büyüme hakkında önemli ayrıntılar](#important-details-about-threadpool-growth)
* [StackExchange.Redis kullanırken istemci üzerinde daha fazla iş elde etmek için sunucu GC etkinleştirin](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Bağlantılar etrafında performans değerlendirmeleri](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>SSS'leri izleme ve sorun giderme
Bu bölümdeki SSS'ler sık izleme ve sorun giderme sorularını kapsar. Redis örnekleri için Azure Önbelleğinizi izleme ve sorun giderme hakkında daha fazla bilgi için [Redis için Azure Önbelleğini](cache-how-to-monitor.md) ve çeşitli sorun giderme kılavuzlarını nasıl izleyebilirsiniz' e bakın.

* [Önbelleğimin durumunu ve performansını nasıl izleyebilirim?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Neden zaman dilimleri görüyorum?](#why-am-i-seeing-timeouts)
* [Müvekkilimin önbellekten bağlantısı neden kesildi?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Önceki Önbellek sunan SSS'ler
* [Hangi Azure Önbellek teklifi benim için doğru?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-cache-for-redis"></a>Redis için Azure Cache nedir?
Redis için Azure Önbellek popüler açık kaynak yazılım [Redis](https://redis.io/)dayanmaktadır. Microsoft tarafından yönetilen ve Azure'daki tüm uygulamalardan erişilebilen güvenli ve özel bir Azure Önbelleğine erişmenizi sağlar. Daha ayrıntılı bir genel bakış için, Azure.com'daki Redis için [Azure Önbelleği](https://azure.microsoft.com/services/cache/) sayfasına bakın.

### <a name="how-can-i-get-started-with-azure-cache-for-redis"></a>Redis için Azure Önbelleği'ne nasıl başlarım?
Redis için Azure Önbelleği'ne başlamanın birkaç yolu vardır.

* [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET,](cache-web-app-howto.md) [Java,](cache-java-get-started.md) [Node.js](cache-nodejs-get-started.md)ve [Python](cache-python-get-started.md)için mevcut olan eğitimlerimizden birine göz atabilirsiniz.
* [Redis için Microsoft Azure Önbelleğini Kullanarak Yüksek Performanslı Uygulamalar Oluşturmayı](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/)izleyebilirsiniz.
* Redis'in nasıl kullanılacağını görmek için projenizin geliştirme diliyle eşleşen istemci belgelerine göz atabilirsiniz. Redis için Azure Önbellek ile kullanılabilecek birçok Redis istemcisi vardır. Redis istemcilerinin listesi için [https://redis.io/clients](https://redis.io/clients)bkz.

Zaten bir Azure hesabınız yoksa şunları yapabilirsiniz:

* [Ücretsiz bir Azure hesabı açın](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Ücretli Azure hizmetlerini denemek için kullanabileceğiniz krediler alırsınız. Krediler bitmiş olsa bile hesabı sürdürebilir ve ücretsiz Azure hizmet ve özelliklerinden faydalanabilirsiniz.
* [Visual Studio abone avantajları etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir.

<a name="cache-size"></a>

### <a name="what-azure-cache-for-redis-offering-and-size-should-i-use"></a>Redis sunan ve boyutu için hangi Azure Önbelleğini kullanmalıyım?
Redis için her Azure Önbelleği, farklı **boyut**düzeyleri, **bant genişliği,** **yüksek kullanılabilirlik**ve **SLA** seçenekleri sağlar.

Önbellek teklifi seçmek için göz önünde bulundurulması gereken hususlar şunlardır.

* **Bellek**: Temel ve Standart katmanlar 250 MB – 53 GB sunar. Premium katman 1,2 TB 'a (küme olarak) veya 120 GB'a (kümelenmemiş) kadar teklif sunar. Daha fazla bilgi [için Redis Fiyatlandırması için Azure Önbelleği'ne](https://azure.microsoft.com/pricing/details/cache/)bakın.
* **Ağ Performansı**: Yüksek iş yükü gerektiren bir iş yükünüz varsa, Premium katmanı Standart veya Temel'e göre daha fazla bant genişliği sunar. Ayrıca her katmaniçinde, önbelleği barındıran temel VM nedeniyle daha büyük boyut önbellekleri daha fazla bant genişliğine sahiptir. Daha fazla bilgi için [aşağıdaki tabloya](#cache-performance)bakın.
* **İş Verimi**: Premium katmanı mevcut maksimum verimi sunar. Önbellek sunucusu veya istemci bant genişliği sınırlarına ulaşırsa, istemci tarafında zaman acısı alabilirsiniz. Daha fazla bilgi için aşağıdaki tabloya bakın.
* **Yüksek Kullanılabilirlik/SLA**: Redis için Azure Önbelleği, standart/premium önbelleğin en az %99,9 oranında kullanılabileceğini garanti eder. SLA'mız hakkında daha fazla bilgi edinmek [için Redis Fiyatlandırması için Azure Önbelleği'ne](https://azure.microsoft.com/support/legal/sla/cache/v1_0/)bakın. SLA yalnızca Önbellek uç noktalarına bağlantı kapsar. SLA veri kaybına karşı korumayı kapsamaz. Veri kaybına karşı dayanıklılığı artırmak için Premium katmanındaki Redis veri kalıcılığı özelliğini kullanmanızı öneririz.
* **Redis Veri Kalıcılığı**: Premium katmanı önbellek verilerini bir Azure Depolama hesabında kalıcı olarak sürebilirsiniz. Temel/Standart önbelleğinde, tüm veriler yalnızca bellekte depolanır. Altta yatan altyapı sorunları olası veri kaybına neden olabilir. Veri kaybına karşı dayanıklılığı artırmak için Premium katmanındaki Redis veri kalıcılığı özelliğini kullanmanızı öneririz. Redis için Azure Önbellek, Redis kalıcılığında RDB ve AOF (yakında) seçeneklerini sunar. Daha fazla bilgi için [Redis için Premium Azure Önbelleği için kalıcılığı nasıl yapılandıracağınız](cache-how-to-premium-persistence.md)abakın.
* **Redis Cluster**: 120 GB'dan büyük önbellekler oluşturmak veya birden çok Redis düğümünde veri parçaları oluşturmak için, Premium katmanında bulunan Redis kümeleme'yi kullanabilirsiniz. Her düğüm, yüksek kullanılabilirlik için bir birincil/çoğaltma önbellek çiftinden oluşur. Daha fazla bilgi için [Redis için Premium Azure Önbelleği için kümeleme yapılandırmanın nasıl yapılacağını](cache-how-to-premium-clustering.md)öğrenin.
* **Gelişmiş güvenlik ve ağ yalıtımı**: Azure Sanal Ağı (VNET) dağıtımı, erişimi daha da kısıtlamak için Azure Önbelleğiniz redis için gelişmiş güvenlik ve yalıtımın yanı sıra alt ağları, erişim denetim ilkeleri ve diğer özellikleri sağlar. Daha fazla bilgi için [Redis için Premium Azure Önbelleği için Sanal Ağ desteğini nasıl yapılandıracağınız](cache-how-to-premium-vnet.md)abakın.
* **Redis'i Yapılandır**: Hem Standart hem de Premium katmanlarda Keyspace bildirimleri için Redis'i yapılandırabilirsiniz.
* **Maksimum istemci bağlantısı sayısı**: Premium katman, daha büyük ölçekli önbellekler için daha yüksek sayıda bağlantıyla Redis'e bağlanabilen maksimum istemci sayısını sunar. Kümeleme, kümelenmiş önbellek için kullanılabilir bağlantı sayısını artırmaz. Daha fazla bilgi [için Redis fiyatlandırması için Azure Önbelleği'ne](https://azure.microsoft.com/pricing/details/cache/)bakın.
* **Redis Server için Özel Çekirdek**: Premium katmanında, tüm önbellek boyutları Redis için özel bir çekirdeğe sahiptir. Temel/Standart katmanlarda, C1 boyutu ve üstü Redis sunucusu için özel bir çekirdek vardır.
* **Redis tek dişli,** böylece ikiden fazla çekirdek olması sadece iki çekirdek olan üzerinde ek fayda sağlamaz, ancak daha büyük VM boyutları genellikle küçük boyutlarda daha fazla bant genişliğine sahip. Önbellek sunucusu veya istemci bant genişliği sınırlarına ulaşırsa, istemci tarafında zaman zaman acısı alırsınız.
* **Performans iyileştirmeleri**: Premium katmandaki önbellekler, temel veya standart katmana göre daha iyi performans sağlayan daha hızlı işlemcilere sahip donanımlarda dağıtılır. Premium katman önbellekleri daha yüksek iş sahibi ve daha düşük gecikme süreleri vardır.

<a name="cache-performance"></a>

### <a name="azure-cache-for-redis-performance"></a>Redis performansı için Azure Önbelleği
Aşağıdaki tablo, IaaS `redis-benchmark.exe` VM'den Redis bitiş noktası için Azure Önbelleği'ne karşı çeşitli standart ve premium önbellekboyutlarını sınarken gözlenen maksimum bant genişliği değerlerini gösterir. SSL iş çıkışı için redis-benchmark, Redis uç noktası için Azure Önbelleğine bağlanmak için stunnel ile birlikte kullanılır.

>[!NOTE] 
>Bu değerler garanti edilmez ve bu sayılar için SLA yoktur, ancak tipik olmalıdır. Uygulamanız için doğru önbellek boyutunu belirlemek için kendi uygulamanızı yüklemeniz gerekir.
>Bu sayılar, yeni sonuçları periyodik olarak yayınladığımız için değişebilir.
>

Bu tablodan aşağıdaki sonuçları çıkarabiliriz:

* Aynı boyuttaki önbellekler için üretim, Premium katmanda Standart katmana kıyasla daha yüksektir. Örneğin, 6 GB Önbellek ile P1'in iş kısmı saniyede 180.000 istek (RPS) olup, C3 için 100.000 RPS'dir.
* Redis kümeleme ile, kümedeki parça (düğüm) sayısını artırdıkça üretim doğrusal olarak artar. Örneğin, 10 parçadan oluşan bir P4 kümesi oluşturursanız, kullanılabilir iş parçacığı 400.000 * 10 = 4 milyon RPS'dir.
* Daha büyük anahtar boyutları için üretim, Premium katmanda Standart Katmana göre daha yüksektir.

| Fiyatlandırma katmanı | Boyut | CPU çekirdekleri | Kullanılabilir bant genişliği | 1-KB değer boyutu | 1-KB değer boyutu |
| --- | --- | --- | --- | --- | --- |
| **Standart önbellek boyutları** | | |**Sn başına megabit (Mb/s) / Sn başına Megabayt (MB/s)** |**Saniyedeki istekler (RPS) SSL dışı** |**Saniyedeki istekler (RPS) SSL** |
| C0 | 250 MB | Paylaşımlı | 100 / 12.5  |  15.000 |   7.500 |
| C1 |   1 GB | 1      | 500 / 62.5  |  38,000 |  20,720 |
| C2 | 2,5 GB | 2      | 500 / 62.5  |  41,000 |  37,000 |
| C3 |   6 GB | 4      | 1000 / 125  | 100.000 |  90,000 |
| C4 |  13 GB | 2      | 500 / 62.5  |  60.000 |  55,000 |
| C5 |  26 GB | 4      | 1,000 / 125 | 102,000 |  93,000 |
| C6 |  53 GB | 8      | 2,000 / 250 | 126,000 | 120,000 |
| **Premium önbellek boyutları** | |**Parça başına CPU çekirdekleri** | **Sn başına megabit (Mb/s) / Sn başına Megabayt (MB/s)** |**Saniyede istekler (RPS) SSL olmayan, parça başına** |**Saniyede istek (RPS) SSL, parça başına** |
| P1 |   6 GB |  2 | 1,500 / 187.5 | 180,000 | 172,000 |
| P2 |  13 GB |  4 | 3,000 / 375   | 350,000 | 341,000 |
| P3 |  26 GB |  4 | 3,000 / 375   | 350,000 | 341,000 |
| P4 |  53 GB |  8 | 6,000 / 750   | 400,000 | 373,000 |
| P5 | 120 GB | 20 | 6,000 / 750   | 400,000 | 373,000 |

Stunnel kurma veya [Redis](#cache-commands) araçlarını indirme ile `redis-benchmark.exe`ilgili talimatlar için , bkz.

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>Önbelleğimi hangi bölgede bulmalıyım?
En iyi performans ve en düşük gecikme süresi için, Redis için Azure Önbelleğinizi önbellek istemci uygulamanızla aynı bölgede bulun.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-cache-for-redis"></a>Redis için Azure Önbelleği için nasıl faturalandırılırım?
Redis fiyatlandırması için Azure Önbelleği [burada.](https://azure.microsoft.com/pricing/details/cache/) Fiyatlandırma sayfası fiyatlandırmayı saatlik ücret olarak listeler. Önbellekler, önbelleğin oluşturulduğu andan önbelleğin silindiği zamana kadar dakika başına faturalandırılır. Önbelleğin faturalandırmasını durdurma veya duraklatma seçeneği yoktur.

### <a name="can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany"></a>Azure Kamu Bulutu, Azure Çin Bulutu veya Microsoft Azure Almanya ile Redis için Azure Önbelleğini kullanabilir miyim?
Evet, Redis için Azure Önbelleği Azure Kamu Bulutu, Azure China 21Vianet Cloud ve Microsoft Azure Almanya'da kullanılabilir. Redis için Azure Önbelleği'ne erişmek ve yönetmek için URL'ler bu bulutlarda Azure Genel Bulut'a göre farklıdır.

| Bulut   | Redis için Dns Soneki            |
|---------|---------------------------------|
| Genel  | *.redis.cache.windows.net       |
| US Gov  | *.redis.cache.usgovcloudapi.net |
| Almanya | *.redis.cache.cloudapi.de       |
| Çin   | *.redis.cache.chinacloudapi.cn  |

Redis için Azure Önbelleğini diğer bulutlarla birlikte kullanırken dikkat edilmesi gereken ler hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın.

- [Azure Devlet Veritabanları - Redis için Azure Önbelleği](../azure-government/documentation-government-services-database.md#azure-cache-for-redis)
- [Azure China 21Vianet Cloud - Redis için Azure Önbellek](https://www.azure.cn/home/features/redis-cache/)
- [Microsoft Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)

Azure Kamu Bulutu, Azure Çin 21Vianet Bulutu ve Microsoft Azure Almanya'da PowerShell ile Redis için Azure Önbelleği kullanma hakkında bilgi için diğer bulutlara nasıl bağlanacağınız hakkında bilgi için [( Redis PowerShell için Azure Önbelleği](cache-how-to-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds)).

<a name="cache-configuration"></a>

### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>StackExchange.Redis yapılandırma seçenekleri ne yapar?
StackExchange.Redis birçok seçenek vardır. Bu bölümde bazı ortak ayarlar dan bahsedilmelidir. StackExchange.Redis seçenekleri hakkında daha ayrıntılı bilgi için [StackExchange.Redis yapılandırmasına](https://stackexchange.github.io/StackExchange.Redis/Configuration)bakın.

| ConfigurationOptions | Açıklama | Öneri |
| --- | --- | --- |
| AbortOnConnectFail |Doğru ayarlandığında, bağlantı ağ hatasından sonra yeniden bağlanmaz. |False olarak ayarlayın ve StackExchange.Redis'in otomatik olarak yeniden bağlanmasına izin verin. |
| ConnectRetry |İlk bağlantı sırasında bağlantı denemelerini tekrarlama sayısı. |Rehberlik için aşağıdaki notlara bakın. |
| ConnectTimeout |Bağlantı işlemleri için ms'de zaman alakart. |Rehberlik için aşağıdaki notlara bakın. |

Genellikle istemcinin varsayılan değerleri yeterlidir. İş yükünüze bağlı olarak seçenekleri ince ayaryapabilirsiniz.

* **Yeniden deneme sayısı**
  * ConnectRetry ve ConnectTimeout için genel kılavuz hızlı başarısız olmak ve yeniden denemektir. Bu kılavuz, iş yükünüze ve müşterinizin redis komutu çıkarabilmek ve yanıt alması için ortalama ne kadar zaman aldığına bağlıdır.
  * StackExchange.Redis'in bağlantı durumunu kontrol etmek ve kendinizi yeniden bağlamak yerine otomatik olarak yeniden bağlanmasına izin verin. **ConnectionMultiplexer.IsConnected özelliğini kullanmaktan kaçının.**
  * Kartopu - bazen yeniden çalışıyorsunuz ve yeniden kartopu ve kurtarır asla bir sorun içine çalıştırabilirsiniz. Kartopu oluşursa, Microsoft Patterns & Uygulamaları grubu tarafından yayınlanan [Yeniden Deneme genel kılavuzunda](../best-practices-retry-general.md) açıklandığı gibi üstel bir geri dönüş yeniden deneme algoritması kullanmayı düşünmelisiniz.
  
* **Zaman ara değerleri**
  * İş yükünüzü göz önünde bulundurun ve değerleri buna göre ayarlayın. Büyük değerleri depolıyorsanız, zaman arasını daha yüksek bir değere ayarlayın.
  * Yanlış `AbortOnConnectFail` ayarlayın ve StackExchange.Redis sizin için yeniden izin.
  * Uygulama için tek bir ConnectionMultiplexer örneği kullanın. [ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlan'da](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)gösterildiği gibi, Bir Bağlantı özelliği tarafından döndürülen tek bir örnek oluşturmak için LazyConnection kullanabilirsiniz.
  * `ConnectionMultiplexer.ClientName` Özelliği tanılama amacıyla bir uygulama örneği benzersiz adı olarak ayarlayın.
  * Özel `ConnectionMultiplexer` iş yükleri için birden çok örnek kullanın.
      * Uygulamanızda değişen bir yük varsa bu modeli takip edebilirsiniz. Örnek:
      * Büyük tuşları ile başa çıkmak için bir multiplexer olabilir.
      * Küçük tuşları ile başa çıkmak için bir multiplexer olabilir.
      * Bağlantı zaman ları için farklı değerler ayarlayabilir ve kullandığınız her ConnectionMultiplexer için mantığı yeniden deneyebilirsiniz.
      * Tanılama `ClientName` ile yardımcı olmak için her multiplexer özelliği ayarlayın.
      * Bu kılavuz, her bir kılavuz da `ConnectionMultiplexer`daha düzenli gecikmeye yol açabilir.

### <a name="what-azure-cache-for-redis-clients-can-i-use"></a>Redis istemcileri için hangi Azure Önbelleğini kullanabilirim?
Redis hakkında büyük şeylerden biri birçok farklı geliştirme dilleri destekleyen birçok müşteri olmasıdır. Geçerli bir istemci listesi için [Redis istemcilerine](https://redis.io/clients)bakın. Birkaç farklı dili ve istemciyi kapsayan öğreticiler [için Redis için Azure Önbelleği'nin nasıl kullanılacağına](cache-dotnet-how-to-use-azure-redis-cache.md) bakın ve bu makaleler içindekiler tablosundaki kardeş makalelerdir.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-cache-for-redis"></a>Redis için Azure Önbellek için yerel bir emülatör var mı?
Redis için Azure Önbelleği için yerel bir emülatör yoktur, ancak redis-server.exe'nin MSOpenTech sürümünü yerel makinenizdeki [Redis komut satırı araçlarından](https://github.com/MSOpenTech/redis/releases/) çalıştırabilir ve aşağıdaki örnekte gösterildiği gibi yerel bir önbellek emülatörüne benzer bir deneyim elde etmek için ona bağlanabilirsiniz:

    private static Lazy<ConnectionMultiplexer>
          lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });

        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Redis için çevrimiçi Azure [Önbelleğinizin varsayılan önbellek ayarlarıyla](cache-configure.md#default-redis-server-configuration) daha yakından eşleşecek şekilde isteğe bağlı olarak [redis.conf](https://redis.io/topics/config) dosyasını yapılandırabilirsiniz.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Redis komutlarını nasıl çalıştırabilirim?
Redis için [Azure Önbelleğinde desteklenmeyen Redis komutlarında](cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis)listelenen komutlar dışında [Redis komutlarında](https://redis.io/commands#) listelenen komutlardan herhangi birini kullanabilirsiniz. Redis komutlarını çalıştırmak için birkaç seçeneğiniz vardır.

* Standart veya Premium önbelleğiniz varsa [Redis Konsolu'nu](cache-configure.md#redis-console)kullanarak Redis komutlarını çalıştırabilirsiniz. Redis konsolu, Azure portalında Redis komutlarını çalıştırmak için güvenli bir yol sağlar.
* Redis komut satırı araçlarını da kullanabilirsiniz. Bunları kullanmak için aşağıdaki adımları gerçekleştirin:
* [Redis komut satırı araçlarını](https://github.com/MSOpenTech/redis/releases/)indirin.
* Önbelleğe 'yi `redis-cli.exe`kullanarak bağlan. -h anahtarını ve -a tuşunu kullanarak önbellek bitiş noktasını aşağıdaki örnekte gösterildiği gibi geçirin:
* `redis-cli -h <Azure Cache for Redis name>.redis.cache.windows.net -a <key>`

> [!NOTE]
> Redis komut satırı araçları SSL bağlantı noktasıyla çalışmaz, ancak Redis makalesi [için Azure Önbelleği ile Redis komut satırı aracının nasıl kullanılacağı](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-redis-cli-tool) nasıI yönergeleri izleyerek araçları SSL bağlantı noktasına güvenli bir şekilde bağlamak gibi bir yardımcı program `stunnel` kullanabilirsiniz.
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Redis için Azure Önbelleği'nde neden diğer Azure hizmetleri gibi BIR MSDN sınıfı kitaplığı başvurusu yok?
Redis için Microsoft Azure Önbelleği, Redis için popüler açık kaynak Azure Önbelleğini temel adatır. Birçok programlama dilleri için [Redis istemcileri](https://redis.io/clients) geniş bir yelpazede tarafından erişilebilir. Her istemcinin Redis komutlarını kullanarak Redis örneği için Azure Önbelleği'ni çağıran kendi [API'si](https://redis.io/commands)vardır.

Her istemci farklı olduğundan, MSDN'de tek bir merkezi sınıf başvurusu yoktur ve her istemci kendi başvuru belgelerini tutar. Başvuru belgelerine ek olarak, farklı diller ve önbellek istemcileri kullanarak Redis için Azure Önbelleği'ne nasıl başlanacağını gösteren çeşitli öğreticiler vardır. Bu öğreticilere erişmek [için Redis için Azure Önbelleği'ni nasıl kullanacağınız](cache-dotnet-how-to-use-azure-redis-cache.md) konusuna bakın ve bu makaleler içindekiler tablosundaki kardeş makalelerdir.

### <a name="can-i-use-azure-cache-for-redis-as-a-php-session-cache"></a>Redis için Azure Önbelleğini PHP oturum önbelleği olarak kullanabilir miyim?
Evet, REDIs için Azure Önbelleğini PHP oturum önbelleği olarak kullanmak için, Redis `session.save_path`örneğindeki Azure Önbelleğinize bağlantı dizesini belirtin.

> [!IMPORTANT]
> Redis için Azure Önbelleğini PHP oturum önbelleği olarak kullanırken, önbelleğe bağlanmak için kullanılan güvenlik anahtarını aşağıdaki örnekte gösterildiği gibi URL olarak kodlamanız gerekir:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Anahtar URL kodlanmış değilse, aşağıdaki gibi bir ileti ile ilgili bir özel durum alabilirsiniz:`Failed to parse session.save_path`
>
>

PhpRedis istemcisi ile PHP oturum önbelleği olarak Redis için Azure Önbelleği kullanma hakkında daha fazla bilgi için [PHP Oturum işleyicisi'ne](https://github.com/phpredis/phpredis#php-session-handler)bakın.

### <a name="what-are-redis-databases"></a>Redis veritabanları nedir?

Redis Veritabanları, aynı Redis örneğindeki verilerin mantıksal bir ayrımıdır. Önbellek belleği tüm veritabanları arasında paylaşılır ve belirli bir veritabanının gerçek bellek tüketimi, bu veritabanında depolanan anahtarlara/değerlere bağlıdır. Örneğin, Bir C6 önbelleği 53 GB belleğe ve Bir P5'te 120 GB bellek vardır. Tüm 53 GB / 120 GB'ı tek bir veritabanına koymayı seçebilir veya birden çok veritabanları arasında bölebilirsiniz. 

> [!NOTE]
> Kümeleme etkin redis için Premium Azure Önbelleği kullanırken, yalnızca veritabanı 0 kullanılabilir. Bu sınırlama içsel bir Redis sınırlamasıdır ve Redis için Azure Önbelleğine özgü değildir. Daha fazla bilgi için bkz. Kümeleme kullanmak [için istemci uygulamamda herhangi bir değişiklik yapmam gerekiyor mu?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Redis'e bağlanmak için SSL olmayan bağlantı noktasını ne zaman etkinleştirmeliyim?
Redis sunucusu yerel olarak SSL'yi desteklemez, ancak Redis için Azure Önbelleği desteklemez. Redis için Azure Önbelleğine bağlanıyorsanız ve istemciniz StackExchange.Redis gibi SSL'yi destekliyorsa, SSL'yi kullanmalısınız.

>[!NOTE]
>Redis örnekleri için yeni Azure Önbelleği varsayılan olarak SSL olmayan bağlantı noktası devre dışı bırakılır. İstemciniz SSL'yi desteklemiyorsa, Redis makalesi [için Azure Önbelleğindeki Önbelleği Yapılandır'ın](cache-configure.md) [Erişim bağlantı noktaları](cache-configure.md#access-ports) bölümündeki yönergeleri izleyerek SSL olmayan bağlantı noktasını etkinleştirmeniz gerekir.
>
>

Redis gibi `redis-cli` Redis araçları SSL bağlantı noktası ile çalışmaz, `stunnel` ancak Redis Preview Release blog gönderisi için [Duyuru ASP.NET Oturum Devlet Sağlayıcısı'ndaki](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) yönergeleri izleyerek araçları SSL bağlantı noktasına güvenli bir şekilde bağlamak gibi bir yardımcı program kullanabilirsiniz.

Redis araçlarını indirme yle ilgili talimatlar için [Redis komutlarını nasıl çalıştırabilirim bölümüne bakın.](#cache-commands)

### <a name="what-are-some-production-best-practices"></a>Bazı üretim en iyi uygulamaları nelerdir?
* [StackExchange.Redis en iyi uygulamaları](#stackexchangeredis-best-practices)
* [Yapılandırma ve kavramlar](#configuration-and-concepts)
* [Performansı test etme](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange.Redis en iyi uygulamaları
* False `AbortConnect` olarak ayarlayın, ardından ConnectionMultiplexer'ın otomatik olarak yeniden bağlanmasına izin verin. [Ayrıntılar için buraya bakın.](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md)
* ConnectionMultiplexer'ı yeniden kullanın - her istek için yeni bir istek oluşturmayın. Burada `Lazy<ConnectionMultiplexer>` [gösterilen](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) desen önerilir.
* Redis en iyi küçük değerlerle çalışır, bu nedenle daha büyük verileri birden çok tuşa doğramayı düşünün. [Bu Redis tartışma](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), 100 kb büyük olarak kabul edilir. Büyük değerlerin neden olabileceği örnek bir sorun için [bu makaleyi](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) okuyun.
* Zaman aşımlarını önlemek için [ThreadPool ayarlarınızı](#important-details-about-threadpool-growth) yapılandırın.
* En azından varsayılan 5 saniyelik timeout'u kullanın. Bu aralık StackExchange.Redis bir ağ blip durumunda bağlantıyı yeniden kurmak için yeterli zaman verir.
* Çalıştırdığınız farklı işlemlerle ilişkili performans maliyetlerine dikkat edin. Örneğin, `KEYS` komut bir O(n) işlemidir ve kaçınılmalıdır. [redis.io sitesinde,](https://redis.io/commands/) desteklediği her işlem için zaman karmaşıklığı hakkında ayrıntılar vardır. Her işlem için karmaşıklığı görmek için her komutu tıklatın.

#### <a name="configuration-and-concepts"></a>Yapılandırma ve kavramlar
* Üretim sistemleri için Standart veya Premium Katman'ı kullanın. Temel Katman, veri çoğaltma ve SLA bulunmayan tek düğümlü bir sistemdir. Ayrıca en az C1 önbelleği kullanın. C0 önbellekleri genellikle basit geliştirme/test senaryoları için kullanılır.
* Redis'in bir **In-Memory** veri deposu olduğunu unutmayın. Veri kaybının meydana gelebileceği senaryolardan haberdar olmak için [bu makaleyi](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) okuyun.
* Sisteminizi, yama ve arıza nedeniyle bağlantı hatalarını işleyebilir şekilde [geliştirin.](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md)

#### <a name="performance-testing"></a>Performansı test etme
* Kendi perf testlerinizi yazmadan önce olası bir iş için bir his almak için kullanarak `redis-benchmark.exe` başlayın. `redis-benchmark` SSL'yi desteklemediği için, testi çalıştırmadan önce [SSL olmayan bağlantı noktasını Azure portalı üzerinden etkinleştirmeniz](cache-configure.md#access-ports) gerekir. Örneğin, [önbelleğimin performansını nasıl kıyaslayabilirim ve test edebilirim?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* Sınama için kullanılan VM istemcisi, Redis örneğiniz için Azure Önbelleğiile aynı bölgede olmalıdır.
* Onlar daha iyi donanım alabildikleri ve en iyi sonuçları vermeleri gerektiğinden, müşteriniz için Dv2 VM Serisi kullanmanızı öneririz.
* Seçtiğiniz istemci VM'nin test ettiğiniz önbellek kadar en az bilgi işlem ve bant genişliği özelliğine sahip olduğundan emin olun.
* Windows'daysanız istemci makinesinde VRSS'yi etkinleştirin. [Ayrıntılar için buraya bakın.](https://technet.microsoft.com/library/dn383582.aspx)
* Premium katman Redis örnekleri, hem CPU hem de Ağ için daha iyi donanımüzerinde çalıştıkları ndan daha iyi ağ gecikmesi ve iş elde etme gereksinimine sahiptir.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Ortak Redis komutlarını kullanırken dikkat edilmesi gereken hususlar nelerdir?

* Bu komutların etkisini tam olarak anlamadığınız sürece, tamamlanması uzun zaman alan bazı Redis komutlarını kullanmaktan kaçının. Örneğin, üretimde [KEYS](https://redis.io/commands/keys) komutunu çalıştırmayın. Anahtarların sayısına bağlı olarak, geri dönmek uzun zaman alabilir. Redis tek iş parçacığı sunucusudur ve komutları birer birer işler. KEYS'den sonra verilen başka komutlarınız varsa, Redis KEYS komutunu işleyene kadar bunlar işlenmez. [redis.io sitesinde,](https://redis.io/commands/) desteklediği her işlem için zaman karmaşıklığı hakkında ayrıntılar vardır. Her işlem için karmaşıklığı görmek için her komutu tıklatın.
* Anahtar boyutları - küçük anahtar/değerler mi yoksa büyük anahtar/değerler mi kullanmalıyım? Senaryoya bağlı. Senaryonuz daha büyük anahtarlar gerektiriyorsa, ConnectionTimeout'ı ayarlayabilir, ardından değerleri yeniden deneyebilir ve yeniden deneme mantığınızı ayarlayabilirsiniz. Redis sunucu açısından bakıldığında, daha küçük değerler daha iyi performans sağlar.
* Bu hususlar, Redis'te daha büyük değerleri depolayamadığınız anlamına gelmez; aşağıdaki hususlara dikkat etmelidir. Gecikme süreleri daha yüksek olacak. Daha büyük ve daha küçük bir veri kümeniz varsa, [stackexchange.redis yapılandırma seçenekleri](#cache-configuration) nin önceki bölümünde açıklandığı gibi, her biri farklı bir zaman acısı kümesiyle yapılandırılan birden çok ConnectionMultiplexer örneğini kullanabilirsiniz.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Önbelleğimin performansını nasıl kıyaslayabilir ve test edebilirim?
* Önbelleğinizin sistem durumunu [izleyebilmeniz](cache-how-to-monitor.md) için [önbellek tanılamayı etkinleştirin](cache-how-to-monitor.md#enable-cache-diagnostics). Azure portalında ölçümleri görüntüleyebilir ve ayrıca istediğiniz araçları kullanarak bunları [indirebilir ve gözden geçirebilirsiniz](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring).
* Redis sunucunuzu yüklemek için redis-benchmark.exe'yi kullanabilirsiniz.
* Redis için yük testi istemcisi ve Azure Önbelleği'nin aynı bölgede olduğundan emin olun.
* redis-cli.exe kullanın ve INFO komutunu kullanarak önbelleği izleyin.
* Yükünüz yüksek bellek parçalanmasına neden oluyorsa, daha büyük bir önbellek boyutuna kadar ölçeklendirmeniz gerekir.
* Redis araçlarını indirme yle ilgili talimatlar için [Redis komutlarını nasıl çalıştırabilirim bölümüne bakın.](#cache-commands)

Aşağıdaki komutlar redis-benchmark.exe kullanarak bir örnek sağlar. Doğru sonuçlar elde etmek için, bu komutları önbelleğinizle aynı bölgedeki bir VM'den çalıştırın.

* 1k yük kullanarak Pipelined SET isteklerini test edin

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* 1k yük kullanarak Pipelined GET isteklerini test edin.
  NOT: Önbelleği doldurmak için önce yukarıda gösterilen SET testini çalıştırın

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>ThreadPool büyüme hakkında önemli ayrıntılar
CLR ThreadPool iki tür iş parçacığı vardır : "İşçi" ve "I/O Tamamlama Bağlantı Noktası" (IOCP) iş parçacıkları.

* Alt iş `Task.Run(…)`parçacıkları, işleme veya `ThreadPool.QueueUserWorkItem(…)` yöntemleri gibi şeyler için kullanılır. Bu iş parçacıkları, bir arka plan iş parçacığı üzerinde iş olması gerektiğinde CLR'deki çeşitli bileşenler tarafından da kullanılır.
* IOCP iş parçacıkları, ağdan okurken olduğu gibi eşzamanlı IO olduğunda kullanılır.

İş parçacığı havuzu, her iş parçacığı türü için "Minimum" ayarına ulaşana kadar isteğe bağlı olarak (herhangi bir azaltma olmadan) yeni alt iş parçacıkları veya G/Ç tamamlama iş parçacığı sağlar. Varsayılan olarak, en az iş parçacığı sayısı bir sistemdeki işlemci sayısına ayarlanır.

Varolan (meşgul) iş parçacığı sayısı "minimum" iş parçacığı sayısına ulaştığında, ThreadPool 500 milisaniye başına bir iş parçacığına yeni iş parçacığı enjekte etme hızını daraltır. Genellikle, sisteminiz bir IOCP iş parçacığına ihtiyaç duyan bir iş patlaması elde ederse, bu işi hızlı bir şekilde işler. Ancak, iş patlaması yapılandırılan "Minimum" ayarından daha fazlaysa, ThreadPool iki şeyden birinin gerçekleşmesini beklediği için bazı işlerin işlenmesinde bazı gecikmeler olacaktır.

1. Varolan bir iş parçacığı, çalışmayı işlemek için özgür hale gelir.
2. Varolan iş parçacığı 500 ms için ücretsiz hale gelir, bu nedenle yeni bir iş parçacığı oluşturulur.

Temel olarak, Meşgul iş parçacığı sayısı Min iş parçacığı daha büyük olduğunda, büyük olasılıkla ağ trafiği uygulama tarafından işlenmeden önce 500 ms gecikme ödüyoruz anlamına gelir. Ayrıca, varolan bir iş parçacığı 15 saniyeden daha uzun süre boşta kaldığında (hatırladığım kadarıyla) temizlenecek ve bu büyüme ve büzülme döngüsünütekrar edebileceğiunutulmamalıdır.

StackExchange.Redis'ten gelen örnek bir hata iletisi (1.0.450 veya sonrası oluşturma) bakarsanız, şimdi ThreadPool istatistiklerini yazdırdığını görürsünüz (aşağıdaki IOCP ve WORKER ayrıntılarına bakın).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
    IOCP: (Busy=6,Free=994,Min=4,Max=1000),
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Önceki örnekte, IOCP iş parçacığı için altı meşgul iş parçacığı olduğunu ve sistemin dört minimum iş parçacığına izin verecek şekilde yapılandırıldığı görebilirsiniz. Bu durumda, istemci büyük olasılıkla iki 500-ms gecikmeler görmüş olurdu, çünkü 6 > 4.

StackExchange.Redis ya IOCP veya WORKER konuları büyüme daraltılmış alırsa zaman zaman ları vurabilirsiniz unutmayın.

### <a name="recommendation"></a>Öneri

Bu bilgiler göz önüne alındığında, müşterilerin IOCP ve WORKER iş parçacıkları için minimum yapılandırma değerini varsayılan değerden daha büyük bir değere ayarlamalarını önemle öneririz. Bir uygulama için doğru değer büyük olasılıkla başka bir uygulama için çok yüksek veya düşük olacağından, bu değerin ne olması gerektiği konusunda her şeye uyan bir kılavuz veremeyiz. Bu ayar karmaşık uygulamaların diğer bölümlerinin performansını da etkileyebilir, bu nedenle her müşterinin bu ayarı kendi özel gereksinimlerine göre hassas ayarlamayapması gerekir. İyi bir başlangıç yeri 200 veya 300, sonra test ve gerektiği gibi çimdik.

Bu ayarı yapılandırma:

* [ThreadPool.SetMinThreads (...)](/dotnet/api/system.threading.threadpool.setminthreads#System_Threading_ThreadPool_SetMinThreads_System_Int32_System_Int32_) yöntemini kullanarak bu ayarı programlı olarak değiştirmenizi `global.asax.cs`öneririz. Örnek:

```cs
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
  > Bu yöntem tarafından belirtilen değer, tüm AppDomain etkileyen küresel bir ayardır. Örneğin, 4 çekirdekli bir makineniz varsa ve çalışma süresi boyunca *minWorkerThreads* ve *minIoThreads'i* CPU başına 50 olarak ayarlamak istiyorsanız **ThreadPool.SetMinThreads(200, 200)** kullanırsınız.

* `<processModel>` `Machine.config` `%SystemRoot%\Microsoft.NET\Framework\[versionNumber]\CONFIG\` [ *MinIoThreads* veya *minWorkerThreads* yapılandırma ayarını](https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx) yapılandırma elemanı altında kullanarak minimum iş parçacığı ayarını belirtmek de mümkündür , genellikle . **Sistem çapında bir ayar olduğundan, en küçük iş parçacığı sayısını bu şekilde ayarlamak genellikle önerilmez.**

  > [!NOTE]
  > Bu yapılandırma öğesinde belirtilen değer *çekirdek başına* bir ayardır. Örneğin, 4 çekirdekli bir makineniz varsa ve *minioThread'lerinizin* çalışma zamanında 200 `<processModel minIoThreads="50"/>`olmasını istiyorsanız, bunu kullanırsınız.
  >

<a name="server-gc"></a>

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>StackExchange.Redis kullanırken istemci üzerinde daha fazla iş elde etmek için sunucu GC etkinleştirin
Sunucu GC'yi etkinleştirmek, StackExchange.Redis'i kullanırken istemciyi optimize edebilir ve daha iyi performans ve iş gücü sağlayabilir. Sunucu GC hakkında daha fazla bilgi ve nasıl etkinleştirilir için aşağıdaki makalelere bakın:

* [Sunucu GC'yi etkinleştirmek için](/dotnet/framework/configure-apps/file-schema/runtime/gcserver-element)
* [Atık Toplamanın Temelleri](/dotnet/standard/garbage-collection/fundamentals)
* [Çöp Toplama ve Performans](/dotnet/standard/garbage-collection/performance)


### <a name="performance-considerations-around-connections"></a>Bağlantılar etrafında performans değerlendirmeleri

Her fiyatlandırma katmanı istemci bağlantıları, bellek ve bant genişliği için farklı sınırlara sahiptir. Önbelleğin her boyutu belirli sayıda *bağlantıya* izin verirken, Redis'e yapılan her bağlantının yükü yle ilişkilidir. Bu tür ek yükün bir örneği, TLS/SSL şifrelemesi sonucunda CPU ve bellek kullanımı olacaktır. Belirli bir önbellek boyutu için maksimum bağlantı sınırı, hafif yüklü bir önbellek varsayar. Bağlantı yükünden gelen yük *artı* istemci işlemlerinden gelen yük sistem kapasitesini aşıyorsa, geçerli önbellek boyutu için bağlantı sınırını aşmamış olsanız bile önbellek kapasite sorunlarıyla karşılaşabilir.

Her katman için farklı bağlantı sınırları hakkında daha fazla bilgi için [Redis fiyatlandırması için Azure Önbelleği'ne](https://azure.microsoft.com/pricing/details/cache/)bakın. Bağlantılar ve diğer varsayılan yapılandırmalar hakkında daha fazla bilgi için [Varsayılan Redis sunucu](cache-configure.md#default-redis-server-configuration)yapılandırması'na bakın.

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Önbelleğimin durumunu ve performansını nasıl izleyebilirim?
Redis örnekleri için Microsoft Azure Önbelleği [Azure portalında](https://portal.azure.com)izlenebilir. Ölçümleri görüntüleyebilir, ölçüm grafiklerini Başlangıç Panosuna sabitleyebilir, izleme grafiklerinin tarih ve saat aralığını özelleştirebilir, grafiklerden ölçümler ekleyebilir ve kaldırabilir ve belirli koşullar yerine getirildiğinde uyarılar ayarlayabilirsiniz. Daha fazla bilgi için [Redis için Azure Önbelleğini İzle'ye](cache-how-to-monitor.md)bakın.

Redis Kaynak için Azure Önbelleği **menüsü,** önbelleklerinizi izlemek ve sorun giderme için çeşitli araçlar da içerir.

* **Sorunları tanılamak ve çözmek,** sorunları çözmek için sık karşılaşılan sorunlar ve stratejiler hakkında bilgi sağlar.
* **Kaynak durumu** kaynağınızı izler ve beklendiği gibi çalışır mı size bildirir. Azure Kaynak sistem durumu hizmeti hakkında daha fazla bilgi için Azure [Kaynak sistem durumu genel bakışı'na](../resource-health/resource-health-overview.md)bakın.
* **Yeni destek isteği** önbelleğiniz için bir destek isteği açmak için seçenekler sağlar.

Bu araçlar, Redis örnekleri için Azure Önbelleğinizin durumunu izlemenize ve önbelleğe alma uygulamalarınızı yönetmenize yardımcı olur. Daha fazla bilgi için, [Redis için Azure Önbelleğini yapılandırma nın "Destek](cache-configure.md)& sorun giderme ayarları" bölümüne bakın.

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Neden zaman dilimleri görüyorum?
Zaman ekmeleri Redis'le konuşmak için kullandığın istemcide olur. Redis sunucusuna bir komut gönderildiğinde, komut sıraya alınır ve Redis sunucusu sonunda komutu alır ve çalıştırılır. Ancak istemci bu işlem sırasında zaman dışarı olabilir ve bunu yaparsa bir özel durum çağıran tarafında yükseltilir. Sorun giderme zaman ayarı sorunları hakkında daha fazla bilgi için [istemci tarafı sorun giderme](cache-troubleshoot-client.md) ve [StackExchange.Redis zaman dışarı özel durumlara](cache-troubleshoot-timeouts.md#stackexchangeredis-timeout-exceptions)bakın.

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-the-cache"></a>Müvekkilimin önbellekten bağlantısı neden kesildi?
Önbellek bağlantısının kesilmesinin yaygın bir nedeni aşağıda veda edilebedilir.

* İstemci tarafı nedenleri
  * İstemci uygulaması yeniden dağıtıldı.
  * İstemci uygulaması bir ölçekleme işlemi gerçekleştirdi.
    * Bulut Hizmetleri veya Web Uygulamaları söz konusu olduğunda, bunun nedeni otomatik ölçekleme olabilir.
  * İstemci tarafındaki ağ katmanı değişti.
  * Geçici hatalar istemci de veya istemci ve sunucu arasında ağ düğümleri oluştu.
  * Bant genişliği eşik sınırlarına ulaşıldı.
  * CPU bağlı işlemleri tamamlamak için çok uzun sürdü.
* Sunucu tarafı nedenleri
  * Standart önbellek teklifinde, Redis için Azure Önbelleği hizmeti birincil düğümden ikincil düğüme bir hata başlatma başlattı.
  * Azure, önbelleğin dağıtıldığı örneği yamalıyor
    * Bu Redis sunucu güncellemeleri veya genel VM bakım için olabilir.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Hangi Azure Önbellek teklifi benim için doğru?
> [!IMPORTANT]
> Geçen yılki [duyuruya](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)göre Azure Yönetilen Önbellek Hizmeti ve Azure Rolü İçi Önbellek hizmeti 30 Kasım 2016'da **kullanımdan kaldırıldı.** Tavsiyemiz [Redis için Azure Önbelleği'ni](https://azure.microsoft.com/services/cache/)kullanmaktır. Geçiş hakkında bilgi için [bkz.](cache-migrate-to-redis.md)
>
>

### <a name="azure-cache-for-redis"></a>Redis için Azure Önbelleği
Redis için Azure Önbelleği Genellikle 120 GB'a kadar boyutlarda kullanılabilir ve kullanılabilirlik SLA'sı %99,9'dur. Yeni [premium katman](cache-premium-tier-intro.md) 1,2 TB'a kadar boyutlar ve kümeleme, VNET ve kalıcılık için %99,9 SLA ile destek sunar.

Redis için Azure Önbelleği, müşterilere Microsoft tarafından yönetilen redis için güvenli ve özel bir Azure Önbelleği kullanma olanağı sağlar. Bu teklifle, Redis tarafından sağlanan zengin özellik kümesi ve ekosistemden ve Microsoft'un güvenilir barındırma ve izlemelerinden yararlanabilirsiniz.

Redis, yalnızca anahtar değerli çiftleri ile ilgili geleneksel önbelleklerin aksine, yüksek performanslı veri türleri için popülerdir. Redis ayrıca bu tür atomik işlemleri yürütmeyi de destekler, bir dize ekleme gibi; bir karma daki değeri artım; bir listeye iterek; bilgisayar seti kavşak, birlik ve fark; veya sıralanmış bir sette en yüksek rütbeli üye almak. Diğer özellikler arasında işlemler için destek, pub/alt, Lua komut dosyası, sınırlı bir zamana sahip anahtarlar ve Redis'in geleneksel bir önbellek gibi hareket etmesini sağlamak için yapılandırma ayarları yer almaktadır.

Redis başarısının bir diğer önemli yönü de etrafında inşa edilen sağlıklı, canlı açık kaynak ekosistemidir. Bu, birden çok dilde kullanılabilen çeşitli Redis istemcileri kümesine yansır. Bu ekosistem ve çok çeşitli istemciler, Redis için Azure Önbelleğinin Azure içinde oluşturacağınız neredeyse tüm iş yükleri tarafından kullanılmasına olanak tanır.

Redis için Azure Önbelleği'ni kullanmaya başlama hakkında daha fazla bilgi için [Redis için Azure Önbelleği](cache-dotnet-how-to-use-azure-redis-cache.md) ve [Redis belgeleri için Azure Önbelleği](index.yml)nasıl kullanılır' a bakın.

### <a name="managed-cache-service"></a>Yönetilen Önbellek hizmeti
[Yönetilen Önbellek hizmeti 30 Kasım 2016'da kullanımdan kaldırıldı.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Arşivlenmiş belgeleri görüntülemek için [Arşivlenmiş Yönetilen Önbellek Hizmet Belgeleri'ne](/previous-versions/azure/azure-services/dn386094(v=azure.100))bakın.

### <a name="in-role-cache"></a>Rol Önbelleği
[In-Role Cache 30 Kasım 2016'da emekliye ayrıldı.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Arşivlenmiş belgeleri görüntülemek [için, Arşivlenmiş Rol Önbellek Belgeleri'ne](/previous-versions/azure/azure-services/dn386103(v=azure.100))bakın.

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
