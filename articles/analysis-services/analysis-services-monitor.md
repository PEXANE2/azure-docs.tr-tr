---
title: Azure Analiz Hizmetleri sunucu ölçümlerini izleyin | Microsoft Dokümanlar
description: Analiz Hizmetlerinin, sunucularınızın performansını ve sistem durumunu izlemenize yardımcı olmak için portaldaki ücretsiz bir araç olan Azure Metrics Explorer'ı nasıl kullandığını öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: aaa3a6d128fe7dd466f6f60ab515f05fa38ba63b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252811"
---
# <a name="monitor-server-metrics"></a>Sunucu ölçümlerini izleme

Analiz Hizmetleri, sunucularınızın performansını ve sistem durumunu izlemenize yardımcı olmak için portaldaki ücretsiz bir araç olan Azure Ölçümleri Gezgini'nde ölçümler sağlar. Örneğin, bellek ve CPU kullanımını, istemci bağlantı sayısını ve kaynak tüketimini sorgula. Çözümleme Hizmetleri, diğer Azure hizmetleriyle aynı izleme çerçevesini kullanır. Daha fazla bilgi için Azure [Ölçümleri Gezgini'ne başlarken](../azure-monitor/platform/metrics-getting-started.md)bkz.

Daha ayrıntılı tanılama gerçekleştirmek, performansı izlemek ve kaynak grubundaki veya abonelikteki birden çok hizmet kaynağındaki eğilimleri belirlemek için [Azure Monitor'u](../azure-monitor/overview.md)kullanın. Azure Monitor (hizmet) faturalandırılabilir bir hizmete neden olabilir.


## <a name="to-monitor-metrics-for-an-analysis-services-server"></a>Analiz Hizmetleri sunucusunun ölçümlerini izlemek için

1. Azure portalında **Ölçümler'i**seçin.

    ![Azure portalında izleme](./media/analysis-services-monitor/aas-monitor-portal.png)

2. **Metrik'te,** grafiğinize eklemek için ölçüleri seçin. 

    ![Monitör grafiği](./media/analysis-services-monitor/aas-monitor-chart.png)

<a id="#server-metrics"></a>

## <a name="server-metrics"></a>Sunucu ölçümleri

İzleme senaryonuz için hangi ölçümlerin en iyi olduğunu belirlemek için bu tabloyu kullanın. Aynı grafikte yalnızca aynı birimin ölçümleri gösterilebilir.

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|
|---|---|---|---|---|
|CommandPoolJobQueueLength|Komut Havuzu İş Sıra Uzunluğu|Sayı|Ortalama|Komut iş parçacığı havuzunun kuyruğundaki iş sayısı.|
|Güncel Bağlantılar|Bağlantı: Geçerli bağlantılar|Sayı|Ortalama|Kurulan istemci bağlantılarının geçerli sayısı.|
|Geçerli Kullanıcı Oturumları|Geçerli Kullanıcı Oturumları|Sayı|Ortalama|Şu anda kurulan kullanıcı oturumları sayısı.|
|mashup_engine_memory_metric|M Motor Belleği|Bayt|Ortalama|Mashup motor prosesleri ile bellek kullanımı|
|mashup_engine_qpu_metric|M Motor QPU|Sayı|Ortalama|Mashup motor prosesleri ile QPU kullanımı|
|memory_metric|Bellek|Bayt|Ortalama|Bellek. S1 için 0-25 GB, S2 için 0-50 GB ve S4 için 0-100 GB aralığı|
|memory_thrashing_metric|Bellek Temizleme|Yüzde|Ortalama|Ortalama hafıza dayak.|
|CleanerCurrentPrice|Bellek: Temiz Geçerli Fiyat|Sayı|Ortalama|Geçerli bellek fiyatı, $/byte/time, 1000'e normalleştirildi.|
|TemizleyiciMemoryShrinkable|Bellek: Temiz Bellek küçültilemez|Bayt|Ortalama|Bellek miktarı, bayt, arka plan temizleyici tarafından tasfiye tabi değil.|
|TemizleyiciMemoryShrinkable|Bellek: Temiz Bellek küçültilebilir|Bayt|Ortalama|Bellek miktarı, bayt, arka plan temizleyici tarafından tasfiye tabi.|
|MemoryLimitHard|Bellek: Bellek Sınırı Sabit|Bayt|Ortalama|Yapılandırma dosyasından sabit bellek sınırı.|
|MemoryLimitHigh|Bellek: Bellek Sınırı Yüksek|Bayt|Ortalama|Yapılandırma dosyasından yüksek bellek sınırı.|
|MemoryLimitLow|Bellek: Bellek Sınırı Düşük|Bayt|Ortalama|Yapılandırma dosyasından düşük bellek sınırı.|
|MemoryLimitVertiPaq|Bellek: Bellek Limit VertiPaq|Bayt|Ortalama|Yapılandırma dosyasından bellek sınırı.|
|MemoryUsage|Bellek: Bellek Kullanımı|Bayt|Ortalama|Daha temiz bellek fiyatının hesaplanmasında kullanılan sunucu işleminin bellek kullanımı. Karşı İşlem\PrivateBytes artı bellek eşlenen veri boyutu, herhangi bir bellek yoksayarak eşit, hangi eşlenen veya motor Bellek Sınırı aşan bellek analiz motoru (VertiPaq) tarafından tahsis edildi.|
|private_bytes_metric|Özel Baytlar |Bayt|Ortalama|Analiz Hizmetleri motor süreci ve Mashup konteyner işlemlerinin, diğer işlemlerle paylaşılan bellek ler dahil olmak üzere toplam bellek miktarı ayrılmıştır.|
|virtual_bytes_metric|Sanal Bayt Sayısı |Bayt|Ortalama|Analiz Hizmetleri motor süreci ve Mashup konteyner süreçlerinin kullandığı sanal adres alanının geçerli boyutu.|
|mashup_engine_private_bytes_metric|M Motoru Özel Bayt |Bayt|Ortalama|Bellek Mashup kapsayıcı işlemlerinin toplam miktarı, diğer işlemlerle paylaşılan bellek dahil değil ayrılmıştır.|
|mashup_engine_virtual_bytes_metric|M Motor Sanal Bayt |Bayt|Ortalama|Sanal adres alanı Mashup konteyner süreçlerinin geçerli boyutu kullanıyor.|
|Kota|Bellek: Kota|Bayt|Ortalama|Geçerli bellek kotası, baytlar. Bellek kotası, bellek hibesi veya bellek rezervasyonu olarak da bilinir.|
|Kota Engellendi|Bellek: Kota Engellendi|Sayı|Ortalama|Diğer bellek kotaları serbest bırakılıncaya kadar engellenen geçerli kota isteği sayısı.|
|VertiPaqNonpaged|Bellek: VertiPaq Nonpaged|Bayt|Ortalama|Bellek içi motor tarafından kullanılmak üzere çalışma kümesinde kilitli bellek baytları.|
|VertiPaqPaged|Bellek: VertiPaq Paged|Bayt|Ortalama|Bellek içi veriler için kullanılan sayfalı bellek baytları.|
|ProcessingPoolJobQueueLength|İşlem havuzu Iş Sıra Uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzunun kuyruğundaki G/Ç olmayan işlerin sayısı.|
|SatırlarConvertedPerSec|İşleme: Satırlar saniye başına dönüştürülür|CountPerSecond|Ortalama|İşlem sırasında dönüştürülen satır ların oranı.|
|SatırlarReadPerSec|İşleme: Satırlar her saniye okunur|CountPerSecond|Ortalama|Tüm ilişkisel veritabanlarından okunan satır hızı.|
|SatırlarWrittenPerSec|İşleme: Satırlar saniyebaşına yazılmış|CountPerSecond|Ortalama|İşlem sırasında yazılan satırların oranı.|
|qpu_metric|QPU|Sayı|Ortalama|QPU. S1 için 0-100, S2 için 0-200 ve S4 için 0-400 aralığı|
|QueryPoolBusyThreads|Sorgu Havuzu Meşgul Konuları|Sayı|Ortalama|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|BaşarılıConnectionsPerSec|Sn Başına Başarılı Bağlantılar|CountPerSecond|Ortalama|Başarılı bağlantı tamamlanma oranı.|
|Komut HavuzuBusyThreads|İş parçacıkları: Komut havuzu meşgul konuları|Sayı|Ortalama|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|KomutPoolIdleThreads|İş parçacıkları: Komut havuzu boşta konuları|Sayı|Ortalama|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|
|LongParsingBusyThreads|İş parçacıkları: Uzun ayrışma meşgul konuları|Sayı|Ortalama|Uzun ayrışma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|LongParsingIdleThreads|İş parçacıkları: Uzun ayrışma boşta iş parçacıkları|Sayı|Ortalama|Uzun ayrışma iş parçacığı havuzundaki boştaki iş parçacığı sayısı.|
|LongParsingJobQueueLength|İş parçacıkları: Uzun ayrışma iş sırası uzunluğu|Sayı|Ortalama|Uzun ayrışma iş parçacığı havuzunun kuyruğundaki iş sayısı.|
|ProcessingPoolioJobQueueLength|İş parçacıkları: İşlem havuzu G/Ç iş sırası uzunluğu|Sayı|Ortalama|İşlemiş iş parçacığı havuzunun kuyruğundaki G/Ç iş sayısı.|
|ProcessingPoolBusyIOJobThreads|İş parçacıkları: İşlem havuzu meşgul G/Ç iş iş parçacıkları|Sayı|Ortalama|İşlemiş iş parçacığı havuzunda G/Ç işleri çalıştıran iş parçacığı sayısı.|
|ProcessingPoolBusyNonIOThreads|İş parçacıkları: İşlem havuzu meşgul olmayan G/Ç iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda G/Ç olmayan işleri çalıştıran iş parçacığı sayısı.|
|İşlemePoolIdleIOJobThreads|İş parçacıkları: İşlem havuzu boşta G/Ç iş iş parçacıkları|Sayı|Ortalama|İşlemiş iş parçacığı havuzundaki G/Ç işleri için boşta iş parçacığı sayısı.|
|ProcessingPoolIdleNonIOThreads|İş parçacıkları: İşlem havuzu boşta Olmayan G/Ç iş parçacıkları|Sayı|Ortalama|G/Ç olmayan işlere adanmış işleme iş parçacığı havuzundaki boştaki iş parçacığı sayısı.|
|QueryPoolIdleThreads|İş parçacıkları: Sorgu havuzu boşta konuları|Sayı|Ortalama|İşlemiş iş parçacığı havuzundaki G/Ç işleri için boşta iş parçacığı sayısı.|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş sırası uzunluğu|Sayı|Ortalama|Sorgu iş parçacığı havuzunun kuyruğundaki iş sayısı.|
|ShortParsingBusyThreads|İş parçacıkları: Kısa ayrışma meşgul konuları|Sayı|Ortalama|Kısa ayrışma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|ShortParsingIdleThreads|Konu: Kısa ayrışma boşta iş parçacıkları|Sayı|Ortalama|Kısa ayrışma iş parçacığı havuzundaki boştaki iş parçacığı sayısı.|
|ShortParsingJobQueueLength|İş parçacıkları: Kısa ayrışma iş sırası uzunluğu|Sayı|Ortalama|Kısa ayrışma iş parçacığı havuzunun kuyruğundaki iş sayısı.|
|ToplamBağlantı Hataları|Toplam Bağlantı Hataları|Sayı|Ortalama|Toplam başarısız bağlantı denemesi.|
|Toplam Bağlantı İstekleri|Toplam Bağlantı İstekleri|Sayı|Ortalama|Toplam bağlantı istekleri. |

## <a name="next-steps"></a>Sonraki adımlar
[Azure Monitör'e genel bakış](../azure-monitor/overview.md)      
[Azure Ölçümleri Gezgini ile başlarken](../azure-monitor/platform/metrics-getting-started.md)      
[Azure Monitor REST API'sındaki Ölçümler](/rest/api/monitor/metrics)
