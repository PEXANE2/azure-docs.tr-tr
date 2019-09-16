---
title: Azure Analysis Services Server ölçümlerini izleme | Microsoft Docs
description: Azure portal ' de Analysis Services Server ölçümlerini nasıl izleyeceğinizi öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7c7aaf911930f83775f66c47377bc68edb059519
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70958979"
---
# <a name="monitor-server-metrics"></a>Sunucu ölçümlerini izleme

Analysis Services, portaldaki ücretsiz bir araç olan Azure Ölçüm Gezgini ve sunucularınızın performansını ve sistem durumunu izlemenize yardımcı olmak için ölçümler sağlar. Örneğin, bellek ve CPU kullanımını, istemci bağlantı sayısını ve sorgu kaynak tüketimini izleyin. Analysis Services, diğer Azure hizmetleriyle aynı izleme çerçevesini kullanır. Daha fazla bilgi için bkz. [Azure Ölçüm Gezgini](../azure-monitor/platform/metrics-getting-started.md)kullanmaya başlama.

Daha ayrıntılı tanılama gerçekleştirmek, performansı izlemek ve bir kaynak grubunda veya abonelikte birden çok hizmet kaynağı arasındaki eğilimleri belirlemek için [Azure izleyici](../azure-monitor/overview.md)'yi kullanın. Azure Izleyici (hizmet), faturalandırılabilir bir hizmetle sonuçlanabilir.


## <a name="to-monitor-metrics-for-an-analysis-services-server"></a>Analysis Services sunucuya yönelik ölçümleri izlemek için

1. Azure portal, **ölçümler**' i seçin.

    ![Azure portalında izleme](./media/analysis-services-monitor/aas-monitor-portal.png)

2. **Kullanılabilir ölçümler**' de, grafiğinizde içerilecek ölçümleri seçin. 

    ![Grafiği izle](./media/analysis-services-monitor/aas-monitor-chart.png)

<a id="#server-metrics"></a>

## <a name="server-metrics"></a>Sunucu ölçümleri

İzleme senaryolarınız için hangi ölçümlerin en iyi olduğunu anlamak için bu tabloyu kullanın. Aynı grafikte yalnızca aynı birimin ölçümleri görüntülenebilir.

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|
|---|---|---|---|---|
|CommandPoolJobQueueLength|Komut havuzu Iş kuyruğu uzunluğu|Count|Average|Komut iş parçacığı havuzu sırasındaki iş sayısı.|
|CurrentConnections|Bağlantı: Geçerli bağlantılar|Count|Average|Kurulan istemci bağlantılarının geçerli sayısı.|
|CurrentUserSessions|Geçerli Kullanıcı oturumları|Count|Average|Geçerli kullanıcı oturumlarının sayısı.|
|mashup_engine_memory_metric|D motoru belleği|Bayt|Average|Karma altyapı işlemlerine göre bellek kullanımı|
|mashup_engine_qpu_metric|A motoru QPU|Count|Average|Karma altyapı işlemlerine göre QPU kullanımı|
|memory_metric|Bellek|Bayt|Average|Bellek. S1 için 0-25 GB, S2 için 0-50 GB ve S4 için 0-100 GB|
|memory_thrashing_metric|Bellek dağılması|Percent|Average|Ortalama bellek miktarı.|
|CleanerCurrentPrice|Bellek: Temizleyici geçerli fiyat|Count|Average|Geçerli bellek fiyatı, $/Byte/Time, 1000 olarak normalleştirilemez.|
|Cleanermemorynonınkable|Bellek: Temizleyici bellek daraltılamaz|Bayt|Average|Arka plan temizleyici tarafından temizlenme konusu değil, bayt cinsinden bellek miktarı.|
|Cleanermemoryshrınkable|Bellek: Temizleyici bellek shrınılabilir|Bayt|Average|Arka plan temizleyici tarafından temizlenme konusu bayt cinsinden bellek miktarı.|
|MemoryLimitHard|Bellek: Bellek sınırı sabit|Bayt|Average|Yapılandırma dosyasından sabit bellek sınırı.|
|Memoryhighlimit|Bellek: Bellek sınırı yüksek|Bayt|Average|Yapılandırma dosyasından yüksek bellek sınırı.|
|MemoryLimitLow|Bellek: Bellek sınırı düşük|Bayt|Average|Yapılandırma dosyasından düşük bellek sınırı.|
|MemoryLimitVertiPaq|Bellek: Bellek sınırı VertiPaq|Bayt|Average|Yapılandırma dosyasından bellek içi sınır.|
|MemoryUsage|Bellek: Bellek Kullanımı|Bayt|Average|Sunucu işleminin temizleyici bellek fiyatını hesaplamada kullanılan bellek kullanımı. Altyapı bellek sınırının fazla olması halinde bellek içi analiz altyapısı (VertiPaq) tarafından eşlenen veya ayrılan belleği yok sayan, bellek eşlemeli verilerin boyutuna eşit.|
|private_bytes_metric|Özel Baytlar |Bayt|Average|Analysis Services altyapısı işleminin ve karma kapsayıcı işlemlerinin ayırdığı, diğer işlemlerle paylaşılan bellek dahil değil toplam bellek miktarı.|
|virtual_bytes_metric|Sanal baytlar |Bayt|Average|Analysis Services altyapısı işleminin ve mashup kapsayıcı işlemlerinin kullandığı sanal adres alanının geçerli boyutu.|
|mashup_engine_private_bytes_metric|D motoru özel baytlar |Bayt|Average|Diğer işlemlerle paylaşılan bellek dahil değil, ayrılan bellek karma kapsayıcı işlemlerinin toplam miktarı.|
|mashup_engine_virtual_bytes_metric|D motoru sanal bayt sayısı |Bayt|Average|Sanal adres alanı karma kapsayıcı işlemlerinin geçerli boyutu kullanıyor.|
|Kota|Bellek: Kota|Bayt|Average|Bayt cinsinden geçerli bellek kotası. Bellek kotası, bellek verme veya bellek ayırma olarak da bilinir.|
|Quotabkilitli|Bellek: Kota engellendi|Count|Average|Diğer bellek kotaları boşaltılana kadar engellenen kota isteklerinin geçerli sayısı.|
|Vertipaqdisk belleksiz|Bellek: VertiPaq disk belleksiz|Bayt|Average|Bellek içi altyapı tarafından kullanılmak üzere çalışma kümesinde kilitlenen bellek baytları.|
|Vertipaqdisk|Bellek: VertiPaq disk belleğine|Bayt|Average|Bellek içi veriler için kullanılan disk belleğine alınan bellek miktarı.|
|ProcessingPoolJobQueueLength|İşleme havuzu Iş kuyruğu uzunluğu|Count|Average|İşleme iş parçacığı havuzu kuyruğundaki g/ç dışı iş sayısı.|
|RowsConvertedPerSec|İşlerken Dönüştürülen satır sayısı/saniye|CountPerSecond|Average|İşlem sırasında dönüştürülen satır oranı.|
|RowsReadPerSec|İşlerken Okunan satır sayısı/saniye|CountPerSecond|Average|Tüm ilişkisel veritabanlarından okunan satır oranı.|
|RowsWrittenPerSec|İşlerken Saniyede yazılan satır sayısı|CountPerSecond|Average|İşlem sırasında yazılan satır oranı.|
|qpu_metric|QPU|Count|Average|QPU. S1 için 0-100 aralığı, S2 için 0-200 ve S4 için 0-400|
|Querypoolbusyıthreads|Sorgu havuzu meşgul Iş parçacıkları|Count|Average|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|Başarılı bir bağlantı Spersec|Saniyedeki başarılı bağlantı sayısı|CountPerSecond|Average|Başarılı bağlantı tamamlama oranı.|
|Commandpoolbusyıthreads|Akışları Komut havuzu meşgul iş parçacıkları|Count|Average|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|CommandPoolIdleThreads|Akışları Komut havuzu boşta iş parçacıkları|Count|Average|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|
|Longparsingbusyıthreads|Akışları Uzun ayrıştırma meşgul iş parçacıkları|Count|Average|Uzun ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|LongParsingIdleThreads|Akışları Uzun ayrıştırma boşta iş parçacıkları|Count|Average|Uzun ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|
|LongParsingJobQueueLength|Akışları Uzun süre ayrıştırması iş kuyruğu uzunluğu|Count|Average|Uzun ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|
|ProcessingPoolIOJobQueueLength|Akışları İşlem havuzu g/ç iş kuyruğu uzunluğu|Count|Average|İşleme iş parçacığı havuzu kuyruğundaki g/ç işlerinin sayısı.|
|Processingpoolbusyıiojobthreads|Akışları İşlem havuzu meşgul g/ç işi iş parçacıkları|Count|Average|İşleme iş parçacığı havuzunda g/ç işleri çalıştıran iş parçacığı sayısı.|
|Processingpoolbusınonıothreads|Akışları İşlem havuzu meşgul olmayan g/ç iş parçacıkları|Count|Average|İşleme iş parçacığı havuzunda g/ç olmayan işler çalıştıran iş parçacığı sayısı.|
|Processingpokaydliiojobthreads|Akışları İşlem havuzu boşta g/ç işi iş parçacıkları|Count|Average|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|
|Processingpokaydlenoniothreads|Akışları İşlem havuzu boşta g/ç olmayan iş parçacıkları|Count|Average|İşleme iş parçacığı havuzunda g/ç dışı işlere adanmış boşta iş parçacığı sayısı.|
|QueryPoolIdleThreads|Akışları Sorgu havuzu boşta iş parçacıkları|Count|Average|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|
|QueryPoolJobQueueLength|Akışları Sorgu havuzu iş kuyruğu uzunluğu|Count|Average|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|
|Shortparsingbusyıthreads|Akışları Kısa ayrıştırma meşgul iş parçacıkları|Count|Average|Kısa ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|
|ShortParsingIdleThreads|Akışları Kısa ayrıştırma boşta iş parçacıkları|Count|Average|Kısa ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|
|ShortParsingJobQueueLength|Akışları Kısa ayrıştırma iş kuyruğu uzunluğu|Count|Average|Kısa ayrıştırma iş parçacığı havuzu sırasındaki iş sayısı.|
|Totalconnectionarızaları|Toplam bağlantı başarısızlığı sayısı|Count|Average|Toplam başarısız bağlantı denemesi.|
|TotalConnectionRequests|Toplam bağlantı Isteği sayısı|Count|Average|Toplam bağlantı isteği sayısı. |

## <a name="next-steps"></a>Sonraki adımlar
[Azure Izleyicisine genel bakış](../azure-monitor/overview.md)      
[Azure Ölçüm Gezgini kullanmaya başlama](../azure-monitor/platform/metrics-getting-started.md)      
[Azure Izleyici 'de ölçümler REST API](/rest/api/monitor/metrics)
