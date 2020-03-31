---
title: Redis için Azure Önbelleği nasıl izlenir?
description: Redis örnekleri için Azure Önbelleğinizin sistem durumunu ve performansını nasıl izleyeceğinizi öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 07/13/2017
ms.openlocfilehash: 86cbeddba699e89ce1127dbac72dac81dcc41449
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76547498"
---
# <a name="how-to-monitor-azure-cache-for-redis"></a>Redis için Azure Önbelleği nasıl izlenir?

Redis için Azure Önbelleği, önbellek örneklerinizi izlemek için çeşitli seçenekler sunmak için [Azure Monitor'u](../azure-monitor/index.yml) kullanır. Ölçümleri görüntüleyebilir, ölçüm grafiklerini Başlangıç Panosuna sabitleyebilir, izleme grafiklerinin tarih ve saat aralığını özelleştirebilir, grafiklerden ölçümler ekleyebilir ve kaldırabilir ve belirli koşullar yerine getirildiğinde uyarılar ayarlayabilirsiniz. Bu araçlar, Redis örnekleri için Azure Önbelleğinizin durumunu izlemenize ve önbelleğe alma uygulamalarınızı yönetmenize yardımcı olur.

Redis örnekleri için Azure Önbelleği ölçümleri dakikada yaklaşık iki kez Redis [INFO](https://redis.io/commands/info) komutu kullanılarak toplanır ve 30 gün boyunca otomatik olarak depolanır (farklı bir bekletme ilkesini yapılandırmak için dışa aktarma [önbellek ölçümlerine](#export-cache-metrics) bakın) böylece ölçümler grafiklerinde görüntülenebilir ve uyarı kurallarıyla değerlendirilebilir. Her önbellek ölçümü için kullanılan farklı BİlGİ değerleri hakkında daha fazla bilgi için [bkz.](#available-metrics-and-reporting-intervals)

<a name="view-cache-metrics"></a>

Önbellek ölçümlerini görüntülemek için [Azure portalındaki](https://portal.azure.com)önbellek örneğinize [göz atın.](cache-configure.md#configure-azure-cache-for-redis-settings)  Redis için Azure Önbelleği, Genel **Bakış** bıçağı nda ve **Redis ölçüm** bıçağında bazı yerleşik grafikler sağlar. Her grafik, ölçümler ekleyerek veya kaldırarak ve raporlama aralığını değiştirerek özelleştirilebilir.

![Redis ölçümleri](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Önceden yapılandırılmış ölçüm grafiklerini görüntüleme

**Genel Bakış** bıçağı, önceden yapılandırılmış aşağıdaki izleme grafiklerini sıralar.

* [Grafikleri izleme](#monitoring-charts)
* [Kullanım grafikleri](#usage-charts)

### <a name="monitoring-charts"></a>Grafikleri izleme

**Genel Bakış** **bıçaktaki İzleme** bölümünde Hits and **Misses**, Gets **and Sets**, **Connections**ve **Total Commands** grafikleri vardır.

![Grafikleri izleme](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Kullanım grafikleri

**Genel Bakış** bıçak **Kullanım** bölümünde **Redis Server Load**, **Bellek Kullanımı**, Ağ Bant **Genişliği**ve **CPU Kullanım** grafikleri vardır ve ayrıca önbellek örneği için **Fiyatlandırma katmanı** görüntüler.

![Kullanım grafikleri](./media/cache-how-to-monitor/redis-cache-usage-part.png)

**Fiyatlandırma katmanı** önbellek fiyatlandırma katmanını görüntüler ve önbelleği farklı bir fiyatlandırma katmanına [ölçeklendirmek](cache-how-to-scale.md) için kullanılabilir.

## <a name="view-metrics-with-azure-monitor"></a>Azure monitörü yle ölçümleri görüntüleme

Redis ölçümlerini görüntülemek ve Azure Monitor'u kullanarak özel grafikler oluşturmak için **Kaynak menüsünden** **Ölçümler'i** tıklatın ve istediğiniz ölçümleri, raporlama aralığını, grafik türünü ve daha fazlasını kullanarak grafiğinizi özelleştirin.

![Redis ölçümleri](./media/cache-how-to-monitor/redis-cache-monitor.png)

Azure Monitor'u kullanarak ölçümlerle çalışma hakkında daha fazla bilgi için Microsoft [Azure'daki ölçümlere Genel Bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md)bölümüne bakın.

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Önbellek ölçümlerini dışa aktarma

Varsayılan olarak, Azure Monitor'daki önbellek ölçümleri [30 gün boyunca saklanır](../azure-monitor/platform/data-platform-metrics.md) ve ardından silinir. Önbellek ölçümlerinizi 30 günden uzun süre devam etmek için [bir depolama hesabı belirleyebilir](../azure-monitor/platform/archive-diagnostic-logs.md) ve önbellek ölçümleriniz için bir **Bekletme (gün)** ilkesi belirtebilirsiniz. 

Önbellek ölçümleriniz için bir depolama hesabı yapılandırmak için:

1. **Redis için Azure Önbelleği** sayfasında, **İzleme** başlığı altında **Tanılama'yı**seçin.
2. Select **+ Tanılama ayarı ekle.**
3. Ayarları adlandırın.
4. **Arşiv'i bir depolama hesabına**denetleyin. Tanılamayı bir depolama hesabına gönderdiğinde depolama ve işlemler için normal veri hızları tahsil edilir.
4. Önbellek ölçümlerini depolamak için depolama hesabını seçmek için **Yapıla'yı** seçin.
5. Tablo başlığı **metrik**altında, **AllMetrics**gibi depolamak istediğiniz satır öğelerinin yanındaki onay kutusu. **Bekletme (gün)** ilkesi belirtin. Belirtebileceğiniz en büyük gün bekletme **365 gündür.** Ancak, ölçüm verilerini sonsuza kadar saklamak istiyorsanız, **Bekletme (gün)** ile **0'ı**ayarlayın.
6. **Kaydet**'e tıklayın.


![Redis tanılama](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>Önbellek ölçümlerinizi depolamaalanına arşivlemenin yanı sıra, [bunları bir Etkinlik hub'ına](../azure-monitor/platform/rest-api-walkthrough.md#retrieve-metric-values)aktarabilir veya Azure Monitor günlüklerine gönderebilirsiniz.
>

Ölçümlerinize erişmek için, bunları azure portalında bu makalede daha önce açıklandığı şekilde görüntüleyebilir ve [Azure Monitör Ölçümleri REST API'sini](../azure-monitor/platform/stream-monitoring-data-event-hubs.md)kullanarak bunlara erişebilirsiniz.

> [!NOTE]
> Depolama hesaplarını değiştirirseniz, önceden yapılandırılmış depolama hesabındaki veriler karşıdan yüklenmeye devam eder, ancak Azure portalında görüntülenmez.  
> 

## <a name="available-metrics-and-reporting-intervals"></a>Kullanılabilir ölçümler ve raporlama aralıkları

Önbellek **ölçümleri, Geçmiş saat,** **Bugün**, **Geçen hafta**ve **Özel**dahil olmak üzere çeşitli raporlama aralıkları kullanılarak raporlanır. Her ölçüm grafiği için **Metrik** bıçak, grafikteki her bir metrik için ortalama, minimum ve maksimum değerleri görüntüler ve bazı ölçümler raporlama aralığı için bir toplam görüntüler. 

Her metrik iki sürüm içerir. Bir metrik, tüm önbellek için performansı ölçer ve [kümeleme](cache-how-to-premium-clustering.md)kullanan önbellekler `(Shard 0-9)` için, önbellekteki tek bir parçaiçin performansı ad ölçülerinde içeren ölçütün ikinci bir sürümü. Örneğin, bir önbelleğin dört `Cache Hits` parçası varsa, önbelleğin tamamı için `Cache Hits (Shard 3)` toplam isabet sayısıdır ve önbelleğin bu parçasının isabetleri sadece isabetdir.

> [!NOTE]
> Önbellek bağlı etkin istemci uygulamaları olmadan boşta olsa bile, bağlı istemciler, bellek kullanımı ve gerçekleştirilen işlemler gibi bazı önbellek etkinliği görebilirsiniz. Bu etkinlik Redis örneği için bir Azure Önbelleği çalışması sırasında normaldir.
> 
> 

| Ölçüm | Açıklama |
| --- | --- |
| Önbellek Hits |Belirtilen raporlama aralığı sırasında başarılı anahtar aramalarının sayısı. Bu numara `keyspace_hits` Redis [INFO](https://redis.io/commands/info) komutundan eşlenir. |
| Önbellek Gecikmesi (Önizleme) | Önbelleğin gecikmesi, önbelleğin internodu gecikmesi üzerinden hesaplanır. Bu metrik mikrosaniye cinsinden ölçülür `Avg`ve `Min`üç `Max`boyutu vardır: , ve , belirtilen raporlama aralığı sırasında sırasıyla önbelleğin ortalama, minimum ve maksimum gecikmesini temsil eder. |
| Önbellek Misses |Belirtilen raporlama aralığı sırasında başarısız anahtar aramalarının sayısı. Bu numara `keyspace_misses` Redis INFO komutundan eşlenir. Önbellek misses mutlaka önbellek ile ilgili bir sorun olduğu anlamına gelmez. Örneğin, önbellek kenara programlama deseni kullanırken, bir uygulama önce bir öğenin önbelleğinde görünür. Öğe orada değilse (önbellek miss), öğe veritabanından alınır ve bir sonraki kez önbelleğe eklenir. Önbellek misses önbellek kenara programlama deseni için normal davranış vardır. Önbellek kaçıranların sayısı beklenenden yüksekse, önbellekten dolduran ve okuyan uygulama mantığını inceleyin. Öğeler bellek basıncı nedeniyle önbellekten tahliye ediliyorsa, bazı önbellek misses olabilir, ancak bellek basıncı için `Used Memory` `Evicted Keys`izlemek için daha iyi bir metrik olacaktır veya . |
| Önbellek Okuma |Belirtilen raporlama aralığı nda megabayt başına saniyede (MB/s) önbellekten okunan veri miktarı. Bu değer, önbelleği barındıran sanal makineyi destekleyen ve Redis'e özgü olmayan ağ arabirimi kartlarından türetilir. **Bu değer, bu önbellek tarafından kullanılan ağ bant genişliğine karşılık gelir. Sunucu tarafındaki ağ bant genişliği sınırları için uyarılar ayarlamak istiyorsanız, bu `Cache Read` sayacı kullanarak oluşturun. Çeşitli önbellek fiyatlandırma katmanları ve boyutları için gözlenen bant genişliği sınırları için [bu tabloya](cache-faq.md#cache-performance) bakın.** |
| Önbellek Yazma |Belirtilen raporlama aralığı nda megabayt başına saniye (MB/s) cinsinden önbelleğe yazılan veri miktarı. Bu değer, önbelleği barındıran sanal makineyi destekleyen ve Redis'e özgü olmayan ağ arabirimi kartlarından türetilir. Bu değer, istemciden önbelleğe gönderilen verilerin ağ bant genişliğine karşılık gelir. |
| Bağlanan İstemciler |Belirtilen raporlama aralığı sırasında önbelleğe istemci bağlantı sayısı. Bu numara `connected_clients` Redis INFO komutundan eşlenir. Bağlantı [sınırına](cache-configure.md#default-redis-server-configuration) ulaşıldıktan sonra önbelleğe sonraki bağlantı denemeleri başarısız olur. Etkin istemci uygulamaları olmasa bile, iç işlemler ve bağlantılar nedeniyle bağlı istemcilerin birkaç örneği olabilir. |
| CPU |Redis sunucusu için Azure Önbelleğinin, belirtilen raporlama aralığında yüzde olarak kullanılması. Bu değer, işletim sistemi `\Processor(_Total)\% Processor Time` performans sayacıyla eşlenir. |
| Hatalar | Önbelleğin belirli bir raporlama aralığında karşılaşabileceği belirli hatalar ve performans sorunları. Bu metrik, farklı hata türlerini temsil eden sekiz boyutu vardır, ancak gelecekte daha fazla eklenmiştir. Şimdi temsil edilen hata türleri aşağıdaki gibidir: <br/><ul><li>**Failover** – bir önbellek üzerinde başarısız olduğunda (ast ana tanıtıcı)</li><li>**Dataloss** – önbellekte veri kaybı olduğunda</li><li>**Yanıt vermeyen Istemciler** – istemciler sunucudan yeterince hızlı veri okumuyorsa</li><li>**AOF** – AOF kalıcılığı ile ilgili bir sorun olduğunda</li><li>**RDB** – RDB kalıcılığı ile ilgili bir sorun olduğunda</li><li>**İthalat** – İthalat RDB ile ilgili bir sorun olduğunda</li><li>**İhracat** - İhracat RDB ile ilgili bir sorun olduğunda</li></ul> |
| Tahliye Edilen Anahtarlar |`maxmemory` Sınır nedeniyle belirtilen raporlama aralığı nda önbellekten çıkarılan madde sayısı. Bu numara `evicted_keys` Redis INFO komutundan eşlenir. |
| Süresi Dolmuş Anahtarlar |Belirtilen raporlama aralığı sırasında önbellekten süresi dolan madde sayısı. Bu değer `expired_keys` Redis INFO komutundan eşlenir.|
| Alır |Belirtilen raporlama aralığı sırasında önbellekten alma işlemleri sayısı. Bu değer Redis INFO tüm komutaşağıdaki değerlerin toplamıdır: `cmdstat_get`, `cmdstat_mget` `cmdstat_getbit` `cmdstat_hget`, `cmdstat_hgetall` `cmdstat_hmget` `cmdstat_getrange`, , , ve , ve raporlama aralığı sırasında önbellek isabet ve misses toplamına eşdeğerdir. |
| Saniye başına işlemler | Belirtilen raporlama aralığı sırasında önbellek sunucusu tarafından saniyede işlenen toplam komut sayısı.  Bu değer Redis INFO komutundan "instantaneous_ops_per_sec" ile eşlenir. |
| Redis Sunucu Yükü |Redis sunucusunun işleme yle meşgul olduğu ve iletileri boşta beklemediği döngülerin yüzdesi. Bu sayaç 100'e ulaşırsa, Redis sunucusu nun performans tavanına çarptığı ve CPU'nun çalışmayı daha hızlı işlemeyebileceği anlamına gelir. Yüksek Redis Server Load görüyorsanız, istemcide zaman alakart özel durumları görürsünüz. Bu durumda, verilerinizi ölçekleme veya birden çok önbelleğe bölme yi düşünmelisiniz. |
| Ayarlar |Belirtilen raporlama aralığı sırasında önbelleğe ayarlanan işlem sayısı. Bu değer Redis INFO tüm komut aşağıdaki değerlerin `cmdstat_set`toplamıdır: `cmdstat_hsetnx` `cmdstat_lset`, `cmdstat_mset` `cmdstat_msetnx`, `cmdstat_setbit` `cmdstat_hset` `cmdstat_hmset` `cmdstat_setex`, `cmdstat_setrange`, `cmdstat_setnx`, , , , ve . |
| Toplam Anahtar  | Geçmiş raporlama süresi boyunca önbellekteki maksimum anahtar sayısı. Bu numara `keyspace` Redis INFO komutundan eşlenir. Temel ölçüm sisteminin sınırlaması nedeniyle, kümeleme etkin olan önbellekler için Toplam Anahtarlar, raporlama aralığı nda en fazla anahtar sayısına sahip parçanın en fazla anahtar sayısını döndürür.  |
| Toplam İşlemler |Belirtilen raporlama aralığı sırasında önbellek sunucusu tarafından işlenen toplam komut sayısı. Bu değer `total_commands_processed` Redis INFO komutundan eşlenir. Redis için Azure Önbelleği tamamen pub/alt için kullanıldığında, `Cache Hits` `Cache Misses`pub/alt `Sets`işlemleri için `Total Operations` önbellek kullanımını yansıtan ölçümler , , `Gets`, , , , |
| Kullanılmış Bellek |Belirtilen raporlama aralığı sırasında MB'deki önbellekteki anahtar/değer çiftleri için kullanılan önbellek belleği miktarı. Bu değer `used_memory` Redis INFO komutundan eşlenir. Bu değer meta veri veya parçalanma içermez. |
| Kullanılan Bellek Yüzdesi | Belirtilen raporlama aralığı nda kullanılan toplam belleğin %'si.  Bu değer, `used_memory` yüzdeyi hesaplamak için Redis INFO komutundaki değere başvurur. |
| İkinci el bellek RSS |Parçalanma ve meta veriler de dahil olmak üzere belirtilen raporlama aralığı sırasında MB'de kullanılan önbellek belleği miktarı. Bu değer `used_memory_rss` Redis INFO komutundan eşlenir. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Uyarılar

Ölçümlere ve etkinlik günlüklerine göre uyarı alacak şekilde yapılandırmanızı ayarlayabilirsiniz. Azure İzleyici, tetiklendiğinde aşağıdaki işlemleri yapmak üzere bir uyarı yapılandırmanıza olanak tanır:

* E-posta bildirimi gönderme
* Web kancası çağırma
* Bir Azure Mantıksal Uygulamasını çağırma

Önbelleğiniz için Uyarı kurallarını yapılandırmak için **Kaynak menüsünden** **Uyarı kurallarını** tıklatın.

![İzleme](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Uyarıları yapılandırma ve kullanma hakkında daha fazla bilgi için [Uyarılara Genel Bakış](../monitoring-and-diagnostics/insights-alerts-portal.md)bölümüne bakın.

## <a name="activity-logs"></a>Etkinlik Günlükleri
Etkinlik günlükleri, Redis örnekleri için Azure Önbelleğinizde gerçekleştirilen işlemlerhakkında bilgi sağlar. Daha önce "denetim günlükleri" veya "operasyonel günlükleri" olarak biliniyordu. Etkinlik günlüklerini kullanarak, Redis örnekleri için Azure Önbelleğinizde alınan herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" olduğunu belirleyebilirsiniz. 

> [!NOTE]
> Etkinlik günlükleri okuma (GET) işlemlerini içermez.
>

Önbelleğinizin etkinlik günlüklerini görüntülemek için **Kaynak menüsünden** **Etkinlik günlüklerini** tıklatın.

Etkinlik günlükleri hakkında daha fazla bilgi için Azure [Etkinlik Günlüğüne Genel Bakış'a](../azure-monitor/platform/platform-logs-overview.md)bakın.
