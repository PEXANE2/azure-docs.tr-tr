---
title: Azure Izleyici platformu ölçümleri, Tanılama ayarları aracılığıyla dışarı aktarılabilir
description: Azure Izleyici ile her kaynak türü için kullanılabilen ölçümlerin listesi.
services: azure-monitor
ms.topic: reference
ms.date: 03/30/2020
ms.subservice: metrics
ms.openlocfilehash: f0a8fd186862cf95ebdbb2d5bd92d8ff860b3ba1
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515487"
---
# <a name="azure-monitor-platform-metrics-exportable-via-diagnostic-settings"></a>Azure Izleyici platformu ölçümleri, Tanılama ayarları aracılığıyla dışarı aktarılabilir

Azure Izleyici, varsayılan olarak hiçbir yapılandırma olmadan [Platform ölçümleri](data-platform-metrics.md) sağlar. Bu, portalda grafik oluşturma, REST API aracılığıyla erişme veya PowerShell veya CLı kullanarak sorgulama dahil olmak üzere platform ölçümleriyle etkileşimde bulunmak için çeşitli yollar sağlar. Bkz. [ölçümler-](metrics-supported.md) Azure izleyicisinin birleştirilmiş ölçüm işlem hattı ile Şu anda kullanılabilir olan platform ölçümlerinin tam listesi için desteklenir. Bu ölçümleri sorgulamak ve bunlara erişmek için lütfen [2018-01-01 api sürümünü](/rest/api/monitor/metricdefinitions)kullanın. Diğer ölçümler portalda veya eski API 'Ler kullanılarak kullanılabilir.

Azure izleyici ardışık düzeninde bulunan Platform ölçümlerini iki şekilde başka konumlara dışarı aktarabilirsiniz.
1. Log Analytics, Event Hubs veya Azure Storage 'a göndermek için [tanılama ayarlarını](diagnostic-settings.md) kullanma.
2. Ölçümleri kullanın [REST API](/rest/api/monitor/metrics/list)

Azure Izleyici arka ucunun anormal olması nedeniyle, tüm ölçümler Tanılama ayarları kullanılarak dışarı aktarılabilir değildir. Aşağıdaki tabloda, Tanılama ayarları kullanılarak verilebilen ve verilemeyen listeler listelenmiştir.

## <a name="change-to-behavior-for-nulls-and-zero-values"></a>Null değerler ve sıfır değerleri için davranışa değiştirme 
 
Tanılama ayarları aracılığıyla verilebilen platform ölçümleri için, Azure Izleyici 'nin ' 0s ' öğesini ' nulls ' olarak yorumlaması için birkaç ölçüm vardır. Bu, gerçek ' 0s ' (kaynağa göre yayılan) ve yorumlanan ' 0s ' (null değerler) arasında karışıklık oluşmasına neden oldu. Kısa süre içinde bir değişiklik gerçekleşir ve Tanılama ayarları aracılığıyla dışarı aktarılmış platform ölçümleri, temel alınan kaynak tarafından gerçekten yayılmadığı sürece artık ' 0s ' öğesini dışarı aktarmayacaktır. 

> [!CAUTION]
> Yukarıda açıklanan davranıştaki değişiklik 1 Haziran 2020 ' de gerçekleşecek şekilde zamanlanır.

Lütfen unutmayın:

1.  Bir kaynak grubunu veya belirli bir kaynağı silerseniz, etkilenen kaynaklardaki ölçüm verileri artık dışarı aktarma hedeflerine tanılama ayarına gönderilmez. Diğer bir deyişle, artık Event Hubs, depolama hesapları ve Log Analytics çalışma alanlarında görünmez.
2.  Bu geliştirme, tüm genel ve özel bulutlarda kullanılabilir.
3.  Bu değişiklik aşağıdaki deneyimlerden herhangi birinin davranışını etkilemez: 
   - Tanılama ayarları aracılığıyla içe aktarılmış platform kaynak günlükleri
   - Ölçüm grafik Ölçüm Gezgini
   - Platform ölçümleri üzerinde uyarı verme
 
## <a name="metrics-exportable-table"></a>Ölçümler dışarı aktarılabilir tablosu 

Tabloda aşağıdaki sütunlar bulunur. 
- Tanılama ayarları aracılığıyla dışarı aktarılabilir mi? 
- NULL/0 tarafından etkilendi 
- ResourceType 
- Ölçüm 
- MetricDisplayName
- Birim 
- Toplamatürü


> [!NOTE]
> Aşağıdaki tabloda, alt kısımdaki bir yatay kaydırma çubuğu olabilir. Bilgilerin eksik olduğunu düşünüyorsanız, kaydırma çubuğunun sola doğru olup olmadığını kontrol edin.  


| Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?  | Null değerleri zaten yay |  ResourceType  |  Ölçüm  |  MetricDisplayName  |  Birim  |  Toplamatürü | 
|---|---| ---- | ----- | ------ | ---- | ---- | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  CleanerCurrentPrice  |  Bellek: temizleyici geçerli fiyatı  |  Count  |  Ortalama | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  Cleanermemorynonınkable  |  Bellek: temizleyici bellek daraltılamaz  |  Bayt  |  Ortalama | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  Cleanermemoryshrınkable  |  Bellek: temizleyici bellek shrınılabilir  |  Bayt  |  Ortalama | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  Commandpoolbusyıthreads  |  İş parçacıkları: komut havuzu meşgul iş parçacıkları  |  Count  |  Ortalama | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  Commandpokaydlithreads  |  İş parçacıkları: komut havuzu boşta iş parçacıkları  |  Count  |  Ortalama | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  CommandPoolJobQueueLength  |  Komut havuzu Iş kuyruğu uzunluğu  |  Count  |  Ortalama | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  CurrentConnections  |  Bağlantı: geçerli bağlantılar  |  Count  |  Ortalama | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  CurrentUserSessions  |  Geçerli Kullanıcı oturumları  |  Count  |  Ortalama | 
| Evet * * * *  | No |  Microsoft. AnalysisServices/sunucuları  |  Longparsingbusyıthreads  |  İş parçacıkları: uzun ayrıştırma meşgul iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  LongParsingIdleThreads  |  İş parçacıkları: uzun ayrıştırma boşta iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  LongParsingJobQueueLength  |  İş parçacıkları: uzun ayrıştırma işi sıra uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  mashup_engine_memory_metric  |  D motoru belleği  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  mashup_engine_private_bytes_metric  |  D motoru özel baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  mashup_engine_qpu_metric  |  A motoru QPU  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  mashup_engine_virtual_bytes_metric  |  D motoru sanal bayt sayısı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  memory_metric  |  Bellek  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  memory_thrashing_metric  |  Bellek Temizleme  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  MemoryLimitHard  |  Bellek: bellek sınırı sabit  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Memoryhighlimit  |  Bellek: bellek sınırı yüksek  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  MemoryLimitLow  |  Bellek: bellek sınırı düşük  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  MemoryLimitVertiPaq  |  Bellek: bellek sınırı VertiPaq  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  MemoryUsage  |  Bellek: bellek kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  private_bytes_metric  |  Özel baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Processingpoolbusyıiojobthreads  |  İş parçacıkları: Işleme havuzu meşgul g/ç işi iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Processingpoolbusınonıothreads  |  İş parçacıkları: Işleme havuzu meşgul olmayan g/ç iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Processingpokaydliiojobthreads  |  İş parçacıkları: Işleme havuzu boşta g/ç işi iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Processingpokaydlenoniothreads  |  İş parçacıkları: Işleme havuzu boşta g/ç olmayan iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Processingpokayojobqueuelength  |  İş parçacıkları: Işleme havuzu g/ç iş kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  ProcessingPoolJobQueueLength  |  İşleme havuzu Iş kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  qpu_metric  |  QPU  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Querypoolbusyıthreads  |  Sorgu havuzu meşgul Iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Querypokaydlithreads  |  İş parçacıkları: sorgu havuzu boşta iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  QueryPoolJobQueueLength  |  İş parçacıkları: sorgu havuzu iş kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Kota  |  Bellek: kota  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Quotabkilitli  |  Bellek: kota engellendi  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  RowsConvertedPerSec  |  İşleme: dönüştürülen satır sayısı/saniye  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  RowsReadPerSec  |  İşleme: okunan satır sayısı/saniye  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  RowsWrittenPerSec  |  İşleme: yazılan satır sayısı/saniye  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Shortparsingbusyıthreads  |  İş parçacıkları: kısa ayrıştırma meşgul iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  ShortParsingIdleThreads  |  İş parçacıkları: kısa ayrıştırma boşta iş parçacıkları  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  ShortParsingJobQueueLength  |  İş parçacıkları: kısa ayrıştırma iş kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Başarılı bir bağlantı Spersec  |  Saniyedeki başarılı bağlantı sayısı  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Totalconnectionarızaları  |  Toplam bağlantı başarısızlığı sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  TotalConnectionRequests  |  Toplam bağlantı Isteği sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Vertipaqdisk belleksiz  |  Bellek: VertiPaq disk belleksiz  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  Vertipaqdisk  |  Bellek: VertiPaq disk belleğine  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AnalysisServices/sunucuları  |  virtual_bytes_metric  |  Sanal Bayt Sayısı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  BackendDuration  |  Arka uç Isteklerinin süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft.ApiManagement/service  |  Kapasite  |  Kapasite  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Süre  |  Ağ Geçidi Isteklerinin genel süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubDroppedEvents  |  Bırakılan EventHub olayları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubRejectedEvents  |  Reddedilen EventHub olayları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Eventhubbaşarılı Futavents  |  Başarılı EventHub olayları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubThrottledEvents  |  Kısıtlanmış EventHub olayları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTimedoutEvents  |  Zaman aşımına uğrayan EventHub olayları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalBytesSent  |  EventHub olaylarının boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalEvents  |  Toplam EventHub olayları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalFailedEvents  |  Başarısız EventHub olayları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  FailedRequests  |  Başarısız ağ geçidi Istekleri (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Diğer Istekler  |  Diğer ağ geçidi Istekleri (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  İstekler  |  İstekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  SuccessfulRequests  |  Başarılı ağ geçidi Istekleri (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  TotalRequests  |  Toplam ağ geçidi Isteği sayısı (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  UnauthorizedRequests  |  Yetkisiz ağ geçidi Istekleri (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  AppCpuUsagePercentage  |  Uygulama CPU kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  AppMemoryCommitted  |  Atanan uygulama belleği  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  AppMemoryMax  |  En fazla uygulama belleği  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  Appmemorykullanıldı  |  Kullanılan uygulama belleği  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  GCPauseTotalCount  |  GC duraklatma sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  GCPauseTotalTime  |  GC duraklatma toplam süre  |  Mayacak  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  MaxOldGenMemoryPoolBytes  |  En fazla kullanılabilir eski nesil veri boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  OldGenMemoryPoolBytes  |  Eski nesil veri boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  OldGenPromotedBytes  |  Eski nesil veri boyutuna yükselt  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  SystemCpuUsagePercentage  |  Sistem CPU kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatErrorCount  |  Tomcat genel hatası  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  Tomcatreceived baytları  |  Tomcat alınan toplam bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatRequestMaxTime  |  Tomcat Isteği maksimum zamanı  |  Mayacak  |  Maksimum | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatRequestTotalCount  |  Tomcat Isteği toplam sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatRequestTotalTime  |  Tomcat Isteği toplam kez  |  Mayacak  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatResponseAvgTime  |  Tomcat Isteği ortalama zamanı  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatSentBytes  |  Tomcat gönderilen toplam bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatSessionActiveCurrentCount  |  Tomcat oturum etkin sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatSessionActiveMaxCount  |  Tomcat oturumu en fazla etkin sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatSessionAliveMaxTime  |  Tomcat oturumu maksimum etkin süresi  |  Mayacak  |  Maksimum | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatSessionCreatedCount  |  Tomcat oturum oluşturma sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatSessionExpiredCount  |  Tomcat oturumunun süre dolma sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  TomcatSessionRejectedCount  |  Tomcat oturumu reddedildi sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. AppPlatform/yay  |  YoungGenPromotedBytes  |  Küçük ölçekli oluşturma veri boyutuna yükselt  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Automation/automationAccounts  |  TotalJob  |  Toplam Iş sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Automation/automationAccounts  |  Totalupdatedeploymentmachinerçalıştırır  |  Toplam güncelleştirme dağıtım makinesi çalıştırması  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Automation/automationAccounts  |  TotalUpdateDeploymentRuns  |  Toplam güncelleştirme dağıtımı çalıştırmaları  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  CoreCount  |  Adanmış çekirdek sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  CreatingNodeCount  |  Düğüm sayısı oluşturuluyor  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  Idıdnodecount  |  Boştaki düğüm sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  JobDeleteCompleteEvent  |  İş silme Tamam olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  JobDeleteStartEvent  |  İş silme başlangıç olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  JobDisableCompleteEvent  |  İş tüm olayları devre dışı bırak  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  JobDisableStartEvent  |  İş devre dışı başlatma olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  JobStartEvent  |  İş başlatma olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  JobTerminateCompleteEvent  |  İş sonlandırma tamamlanma olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  JobTerminateStartEvent  |  İş sonlandırma başlangıç olayları  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  LeavingPoolNodeCount  |  Havuz düğüm sayısından çıkılıyor  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  LowPriorityCoreCount  |  LowPriority çekirdek sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  OfflineNodeCount  |  Çevrimdışı düğüm sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  PoolCreateEvent  |  Havuz oluşturma olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  PoolDeleteCompleteEvent  |  Havuz silme Tamam olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  PoolDeleteStartEvent  |  Havuz silme başlangıç olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  PoolResizeCompleteEvent  |  Havuz yeniden boyutlandırma Tamam olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  PoolResizeStartEvent  |  Havuz yeniden boyutlandırma başlangıç olayları  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  PreemptedNodeCount  |  Önden düğüm sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  RebootingNodeCount  |  Düğüm sayısı yeniden başlatılıyor  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  ReimagingNodeCount  |  Yeniden Imaging düğüm sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  RunningNodeCount  |  Çalışan düğüm sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  StartingNodeCount  |  Başlangıç düğüm sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  StartTaskFailedNodeCount  |  Başlangıç görevi başarısız düğüm sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  TaskCompleteEvent  |  Görev tamamlanma olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  TaskFailEvent  |  Görev başarısız olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Batch/batchAccounts  |  TaskStartEvent  |  Görev başlatma olayları  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  TotalLowPriorityNodeCount  |  Düşük öncelikli düğüm sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  TotalNodeCount  |  Adanmış düğüm sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  UnusableNodeCount  |  Kullanılamayan düğüm sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  Waitingforstarttasnot Decount  |  Başlangıç görevi düğüm sayısı bekleniyor  |  Count  |  Toplam | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Etkin çekirdekler  |  Etkin çekirdekler  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Etkin düğümler  |  Etkin düğümler  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Boştaki çekirdekler  |  Boştaki çekirdekler  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Boştaki düğümler  |  Boştaki düğümler  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  İş tamamlandı  |  İş tamamlandı  |  Count  |  Toplam | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  İş gönderildi  |  İş gönderildi  |  Count  |  Toplam | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Çekirdekleri bırakma  |  Çekirdekleri bırakma  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Düğümlerden çıkılıyor  |  Düğümlerden çıkılıyor  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Önden çekirdekler  |  Önden çekirdekler  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Önden düğümler  |  Önden düğümler  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Kota kullanım yüzdesi  |  Kota kullanım yüzdesi  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Toplam çekirdek sayısı  |  Toplam çekirdek sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Toplam düğüm sayısı  |  Toplam düğüm sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Kullanılamayan çekirdekler  |  Kullanılamayan çekirdekler  |  Count  |  Ortalama | 
| **Evet**  | No |  ChAI/çalışma alanlarını Microsoft.Bat  |  Kullanılamayan düğümler  |  Kullanılamayan düğümler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  Connectionkabul edildi  |  Kabul edilen bağlantılar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  ConnectionActive  |  Etkin Bağlantılar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  Connectionişlenmiş  |  İşlenmiş bağlantılar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  CpuUsagePercentageInDouble  |  CPU kullanım yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  Ioreadbytes  |  GÇ okuma baytları  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  IOWriteBytes  |  GÇ Yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  MemoryLimit  |  Bellek sınırı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  MemoryUsage  |  Bellek Kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  MemoryUsagePercentageInDouble  |  Bellek kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  PendingTransactions  |  Bekleyen Işlemler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  Processedbkilitler  |  İşlenen bloklar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  ProcessedTransactions  |  İşlenen Işlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  QueuedTransactions  |  Kuyruğa alınmış Işlemler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  RequestHandled  |  İşlenmiş Istekler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Blockzincirine/blockchainMembers  |  StorageUsage  |  Depolama alanı kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits  |  İsabetli Önbellek Okuma Sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits0  |  Önbellek ısabetleri (parça 0)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits1  |  Önbellek ısabetleri (parça 1)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits2  |  Önbellek ısabetleri (parça 2)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits3  |  Önbellek ısabetleri (parça 3)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits4  |  Önbellek ısabetleri (parça 4)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits5  |  Önbellek ısabetleri (parça 5)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits6  |  Önbellek ısabetleri (parça 6)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits7  |  Önbellek ısabetleri (parça 7)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits8  |  Önbellek ısabetleri (parça 8)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachehits9  |  Önbellek ısabetleri (parça 9)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheLatency  |  Önbellek gecikmesi mikrosaniye (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheisabetsizlik  |  İsabetsiz Önbellek Okuma Sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses0  |  Önbellekte bulunamayanlar (parça 0)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses1  |  Önbellekte bulunamayanlar (parça 1)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses2  |  Önbellekte bulunamayanlar (parça 2)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses3  |  Önbellekte bulunamayanlar (parça 3)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses4  |  Önbellekte bulunamayanlar (parça 4)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses5  |  Önbellekte bulunamayanlar (parça 5)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses6  |  Önbellekte bulunamayanlar (parça 6)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses7  |  Önbellekte bulunamayanlar (parça 7)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses8  |  Önbellekte bulunamayanlar (parça 8)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cachemisses9  |  Önbellekte bulunamayanlar (parça 9)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead  |  Önbellek Okuması  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead0  |  Önbellek Okuma (parça 0)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead1  |  Önbellek Okuma (parça 1)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead2  |  Önbellek Okuma (parça 2)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead3  |  Önbellek Okuma (parça 3)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead4  |  Önbellek Okuma (parça 4)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead5  |  Önbellek Okuma (parça 5)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead6  |  Önbellek Okuma (parça 6)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead7  |  Önbellek Okuma (parça 7)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead8  |  Önbellek Okuma (parça 8)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheRead9  |  Önbellek Okuma (parça 9)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite  |  Önbellek Yazması  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite0  |  Önbellek yazma (parça 0)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite1  |  Önbellek yazma (parça 1)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite2  |  Önbellek yazma (parça 2)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite3  |  Önbellek yazma (parça 3)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite4  |  Önbellek yazma (parça 4)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite5  |  Önbellek yazma (parça 5)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite6  |  Önbellek yazma (parça 6)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite7  |  Önbellek yazma (parça 7)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite8  |  Önbellek yazma (parça 8)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  cacheWrite9  |  Önbellek yazma (parça 9)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedistemcileri  |  Bağlı İstemciler  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients0  |  Bağlı Istemciler (parça 0)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients1  |  Bağlı Istemciler (parça 1)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients2  |  Bağlı Istemciler (parça 2)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients3  |  Bağlı Istemciler (parça 3)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients4  |  Bağlı Istemciler (parça 4)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients5  |  Bağlı Istemciler (parça 5)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients6  |  Bağlı Istemciler (parça 6)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients7  |  Bağlı Istemciler (parça 7)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients8  |  Bağlı Istemciler (parça 8)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  connectedclients9  |  Bağlı Istemciler (parça 9)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  hatalar  |  Hatalar  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  çıkarılan anahtarlar  |  Çıkarılan Anahtarlar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys0  |  Çıkarılan anahtarlar (parça 0)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys1  |  Çıkarılan anahtarlar (parça 1)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys2  |  Çıkarılan anahtarlar (parça 2)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys3  |  Çıkarılan anahtarlar (parça 3)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys4  |  Çıkarılan anahtarlar (parça 4)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys5  |  Çıkarılan anahtarlar (parça 5)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys6  |  Çıkarılan anahtarlar (parça 6)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys7  |  Çıkarılan anahtarlar (parça 7)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys8  |  Çıkarılan anahtarlar (parça 8)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  evictedkeys9  |  Çıkarılan anahtarlar (parça 9)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys  |  Süresi Dolan Anahtarlar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys0  |  Süre dolma anahtarları (parça 0)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys1  |  Süre dolma anahtarları (parça 1)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys2  |  Süre dolma anahtarları (parça 2)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys3  |  Süre dolma anahtarları (parça 3)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys4  |  Süre dolma anahtarları (parça 4)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys5  |  Süre dolma anahtarları (parça 5)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys6  |  Süre dolma anahtarları (parça 6)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys7  |  Süre dolma anahtarları (parça 7)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys8  |  Süre dolma anahtarları (parça 8)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  expiredkeys9  |  Süre dolma anahtarları (parça 9)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  GetCommands  |  Alınanlar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands0  |  Alır (parça 0)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands1  |  Alır (parça 1)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands2  |  Alır (parça 2)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands3  |  Alır (parça 3)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands4  |  Alır (parça 4)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands5  |  Alır (parça 5)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands6  |  Alır (parça 6)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands7  |  Alır (parça 7)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands8  |  Al (Shard 8)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  getcommands9  |  Alır (parça 9)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond  |  Saniye Başına İşlem  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond0  |  Saniyedeki işlem (parça 0)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond1  |  Saniyedeki işlem (parça 1)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond2  |  Saniyedeki işlem (parça 2)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond3  |  Saniyedeki işlem (parça 3)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond4  |  Saniyedeki işlem (parça 4)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond5  |  Saniyedeki işlem (parça 5)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond6  |  Saniyedeki işlem (parça 6)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond7  |  Saniyedeki işlem (parça 7)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond8  |  Saniyedeki işlem (parça 8)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  operationsPerSecond9  |  Saniyedeki işlem (parça 9)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime  |  CPU  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime0  |  CPU (parça 0)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime1  |  CPU (parça 1)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime2  |  CPU (parça 2)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime3  |  CPU (parça 3)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime4  |  CPU (parça 4)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime5  |  CPU (parça 5)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime6  |  CPU (parça 6)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime7  |  CPU (parça 7)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime8  |  CPU (parça 8)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  percentProcessorTime9  |  CPU (parça 9)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  Sunucuyükü  |  Sunucu Yükü  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad0  |  Sunucu yükü (parça 0)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad1  |  Sunucu yükü (parça 1)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad2  |  Sunucu yükü (parça 2)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad3  |  Sunucu yükü (parça 3)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad4  |  Sunucu yükü (parça 4)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad5  |  Sunucu yükü (parça 5)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad6  |  Sunucu yükü (parça 6)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad7  |  Sunucu yükü (parça 7)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad8  |  Sunucu yükü (parça 8)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  serverLoad9  |  Sunucu yükü (parça 9)  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  SetCommands  |  Kümeler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands0  |  Kümeler (parça 0)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands1  |  Kümeler (parça 1)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands2  |  Kümeler (parça 2)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands3  |  Kümeler (parça 3)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands4  |  Kümeler (parça 4)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands5  |  Kümeler (parça 5)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands6  |  Kümeler (parça 6)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands7  |  Kümeler (parça 7)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands8  |  Kümeler (Shard 8)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  setcommands9  |  Kümeler (parça 9)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsişlendi  |  Toplam İşlem Sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed0  |  Toplam Işlem (parça 0)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed1  |  Toplam Işlem (parça 1)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed2  |  Toplam Işlem (parça 2)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed3  |  Toplam Işlem (parça 3)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed4  |  Toplam Işlem (parça 4)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed5  |  Toplam Işlem (parça 5)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed6  |  Toplam Işlem (parça 6)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed7  |  Toplam Işlem (parça 7)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed8  |  Toplam Işlem (parça 8)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalcommandsprocessed9  |  Toplam Işlem (parça 9)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys  |  Toplam anahtar sayısı  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys0  |  Toplam anahtar (parça 0)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys1  |  Toplam anahtar (parça 1)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys2  |  Toplam anahtar (parça 2)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys3  |  Toplam anahtar (parça 3)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys4  |  Toplam anahtar (parça 4)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys5  |  Toplam anahtar (parça 5)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys6  |  Toplam anahtar (parça 6)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys7  |  Toplam anahtar (parça 7)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys8  |  Toplam anahtar (Shard 8)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  totalkeys9  |  Toplam anahtar (parça 9)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory  |  Kullanılan Bellek  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory0  |  Kullanılan bellek (parça 0)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory1  |  Kullanılan bellek (parça 1)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory2  |  Kullanılan bellek (parça 2)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory3  |  Kullanılan bellek (parça 3)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory4  |  Kullanılan bellek (parça 4)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory5  |  Kullanılan bellek (parça 5)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory6  |  Kullanılan bellek (parça 6)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory7  |  Kullanılan bellek (parça 7)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory8  |  Kullanılan bellek (parça 8)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemory9  |  Kullanılan bellek (parça 9)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemorypercentage  |  Kullanılan Bellek Yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss  |  Kullanılan bellek RSS  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss0  |  Kullanılan bellek RSS (parça 0)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss1  |  Kullanılan bellek RSS (parça 1)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss2  |  Kullanılan bellek RSS (parça 2)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss3  |  Kullanılan bellek RSS (parça 3)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss4  |  Kullanılan bellek RSS (parça 4)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss5  |  Kullanılan bellek RSS (parça 5)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss6  |  Kullanılan bellek RSS (parça 6)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss7  |  Kullanılan bellek RSS (parça 7)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss8  |  Kullanılan bellek RSS (parça 8)  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Cache/redsıs  |  usedmemoryRss9  |  Kullanılan bellek RSS (parça 9)  |  Bayt  |  Maksimum | 
| Hayır  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Disk okuma bayt/sn  |  Disk okuma  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Disk yazma bayt/sn  |  Disk yazma  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  Disk okuma bayt/sn  |  Disk okuma  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicCompute/virtualMachines  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  Disk yazma bayt/sn  |  Disk yazma  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicCompute/virtualMachines  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicCompute/virtualMachines  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicCompute/virtualMachines  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicCompute/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts  |  UsedCapacity  |  Kullanılan kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  BlobCapacity  |  Blob kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  BlobCount  |  BLOB sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  ContainerCount  |  Blob kapsayıcı sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Dizin kapasitesi  |  Dizin kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Dosya kapasitesi  |  Dosya kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  FileCount  |  Dosya sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Dosya ShareCount  |  Dosya paylaşma sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Dosya Sharequota  |  Dosya paylaşımının kota boyutu  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Dosya paylaşımının anlık görüntü sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Dosya paylaşımının anlık görüntü boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueCapacity  |  Kuyruk kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueCount  |  Sıra sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueMessageCount  |  Kuyruk Iletisi sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Tablokapasitesi  |  Tablo kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Tablosayısı  |  Tablo sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  TableEntityCount  |  Tablo varlık sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Blockedçağrılarında  |  Engellenen çağrılar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Karakterçeli  |  Eğitilen karakterler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Karakter Yabanslamuş  |  Çevrilen karakterler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  ClientErrors  |  İstemci hataları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Dataın  |  Içindeki veriler  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Veri çıkışı  |  Giden veriler  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Gecikme süresi  |  Gecikme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  ServerErrors  |  Sunucu hataları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  SpeechSessionDuration  |  Konuşma oturumu süresi  |  Saniye  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Başarılı çağrılar  |  Başarılı çağrılar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Toplam çağrılar  |  Toplam çağrı sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  Toplam hata sayısı  |  Toplam Hata Sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  TotalTokenCalls  |  Toplam belirteç çağrısı sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Biliveservices/hesapları  |  TotalTransactions  |  Toplam Işlem sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Tüketilen CPU kredileri  |  Tüketilen CPU kredileri  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Kalan CPU kredileri  |  Kalan CPU kredileri  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Veri Diski Kuyruk Derinliği  |  Veri diski sıra derinliği (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Veri diski okuma bayt/sn  |  Veri diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Veri diski okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Veri diski yazma bayt/sn  |  Veri diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Veri diski yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk okuma bayt sayısı  |  Disk okuma bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk yazma baytları  |  Disk yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Gelen Akışlar  |  Gelen Akışlar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Gelen akışlar en yüksek oluşturma oranı  |  Gelen akışlar en yüksek oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Ağ Girişi  |  Faturalanabilir (kullanım dışı) ağ  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Toplam ağ  |  Toplam ağ  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Ağ Çıkışı  |  Ağdan çıkış faturalandırılabilir (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Toplam ağ çıkışı  |  Toplam ağ çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim Sistemi Diski Kuyruk Derinliği  |  İşletim sistemi diski sıra derinliği (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk başına işletim sistemi QD  |  İşletim sistemi diski QD (kullanım dışı)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim sistemi disk başına okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski başına okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim sistemi disk başına yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski başına yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Giden akışlar  |  Giden akışlar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Giden akış maksimum oluşturma oranı  |  Giden akışlar maksimum oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk başına QD  |  Veri diski QD (kullanım dışı)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk başına okunan bayt/sn  |  Veri diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk başına okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk başına yazma bayt/sn  |  Veri diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Disk başına yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Premium veri diski önbelleği okuma Isabeti  |  Premium veri diski önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Premium veri diski önbelleği okuma Isabetsizliği  |  Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Premium işletim sistemi disk önbelleği okuma Isabeti  |  Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachines  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Tüketilen CPU kredileri  |  Tüketilen CPU kredileri  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Kalan CPU kredileri  |  Kalan CPU kredileri  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Veri Diski Kuyruk Derinliği  |  Veri diski sıra derinliği (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski okuma bayt/sn  |  Veri diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski yazma bayt/sn  |  Veri diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk okuma bayt sayısı  |  Disk okuma bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk yazma baytları  |  Disk yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Gelen Akışlar  |  Gelen Akışlar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Gelen akışlar en yüksek oluşturma oranı  |  Gelen akışlar en yüksek oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Ağ Girişi  |  Faturalanabilir (kullanım dışı) ağ  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Toplam ağ  |  Toplam ağ  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Ağ Çıkışı  |  Ağdan çıkış faturalandırılabilir (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Toplam ağ çıkışı  |  Toplam ağ çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim Sistemi Diski Kuyruk Derinliği  |  İşletim sistemi diski sıra derinliği (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına işletim sistemi QD  |  İşletim sistemi diski QD (kullanım dışı)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi disk başına okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski başına okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi disk başına yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski başına yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Giden akışlar  |  Giden akışlar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Giden akış maksimum oluşturma oranı  |  Giden akışlar maksimum oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına QD  |  Veri diski QD (kullanım dışı)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına okunan bayt/sn  |  Veri diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına yazma bayt/sn  |  Veri diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Premium veri diski önbelleği okuma Isabeti  |  Premium veri diski önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Premium veri diski önbelleği okuma Isabetsizliği  |  Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Premium işletim sistemi disk önbelleği okuma Isabeti  |  Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft.Compute/virtualMachineScaleSets  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Tüketilen CPU kredileri  |  Tüketilen CPU kredileri  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Kalan CPU kredileri  |  Kalan CPU kredileri  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Veri Diski Kuyruk Derinliği  |  Veri diski sıra derinliği (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Veri diski okuma bayt/sn  |  Veri diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Veri diski okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Veri diski yazma bayt/sn  |  Veri diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Veri diski yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk okuma bayt sayısı  |  Disk okuma bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk yazma baytları  |  Disk yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Gelen Akışlar  |  Gelen Akışlar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Gelen akışlar en yüksek oluşturma oranı  |  Gelen akışlar en yüksek oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Ağ Girişi  |  Faturalanabilir (kullanım dışı) ağ  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Toplam ağ  |  Toplam ağ  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Ağ Çıkışı  |  Ağdan çıkış faturalandırılabilir (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Toplam ağ çıkışı  |  Toplam ağ çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim Sistemi Diski Kuyruk Derinliği  |  İşletim sistemi diski sıra derinliği (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk başına işletim sistemi QD  |  İşletim sistemi diski QD (kullanım dışı)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi disk başına okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski başına okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi disk başına yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski başına yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Giden akışlar  |  Giden akışlar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Giden akış maksimum oluşturma oranı  |  Giden akışlar maksimum oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk başına QD  |  Veri diski QD (kullanım dışı)  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk başına okunan bayt/sn  |  Veri diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk başına okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk başına yazma bayt/sn  |  Veri diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Disk başına yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Premium veri diski önbelleği okuma Isabeti  |  Premium veri diski önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Premium veri diski önbelleği okuma Isabetsizliği  |  Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Premium işletim sistemi disk önbelleği okuma Isabeti  |  Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Containerınstance/containerGroups  |  CpuUsage  |  CPU Kullanımı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Containerınstance/containerGroups  |  MemoryUsage  |  Bellek Kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Containerınstance/containerGroups  |  NetworkBytesReceivedPerSecond  |  Saniye başına alınan ağ bayt sayısı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Containerınstance/containerGroups  |  NetworkBytesTransmittedPerSecond  |  Saniye başına aktarılan ağ baytları  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. ContainerRegistry/kayıt defterleri  |  RunDuration  |  Çalışma süresi  |  Mayacak  |  Toplam | 
| **Evet**  | No |  Microsoft. ContainerRegistry/kayıt defterleri  |  Başarılı sayı  |  Başarılı çekme sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ContainerRegistry/kayıt defterleri  |  Başarılı Pushcount  |  Başarılı gönderme sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ContainerRegistry/kayıt defterleri  |  TotalPullCount  |  Toplam çekme sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. ContainerRegistry/kayıt defterleri  |  Toplam Pushcount  |  Toplam gönderme sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_node_status_allocatable_cpu_cores  |  Yönetilen kümede toplam kullanılabilir CPU çekirdeği sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_node_status_allocatable_memory_bytes  |  Yönetilen kümede toplam kullanılabilir bellek miktarı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_node_status_condition  |  Çeşitli düğüm koşullarına yönelik durumlar  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_pod_status_phase  |  Aşamasına göre Pod sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_pod_status_ready  |  Hazırlık durumundaki Pod sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  AvailableCapacity  |  Kullanılabilir kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloud  |  Karşıya yüklenen bulut baytları (cihaz)  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloudPerShare  |  Karşıya yüklenen bulut baytları (paylaşma)  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Cloudreadüretilen Iş  |  Bulut Indirme performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughputPerShare  |  Bulut Indirme verimlilik (paylaşma)  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Clouduploadüretilen Iş  |  Bulut karşıya yükleme performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughputPerShare  |  Bulut karşıya yükleme üretilen Işi (paylaşma)  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Hipervmemoryutilileştirme  |  Edge Işlem-bellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Hypervvirtualprocessorkullanım  |  Edge hesaplama-CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Nicreadüretilen Iş  |  Okuma performansı (ağ)  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Nicwriteüretilen Iş  |  Yazma Işleme (ağ)  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  TotalCapacity  |  Toplam kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataFactory/DataFactory  |  Failedçalıştırmaları  |  Başarısız çalıştırmalar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/DataFactory  |  Başarılı çalıştırmalar  |  Başarılı çalıştırmalar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Activitycancelledçalıştırmaları  |  İptal edilen etkinlik ölçümleri çalıştırıyor  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Activityfailedçalıştırmaları  |  Başarısız etkinlik çalıştırma ölçümleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  ActivitySucceededRuns  |  Başarılı etkinlik ölçümleri çalıştırıyor  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Factorysizeingbits  |  Toplam fabrika boyutu (GB birimi)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Integrationruntimekullanılabilirliği Blememory  |  Tümleştirme çalışma zamanı kullanılabilir belleği  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Integrationruntimeaveragetaskpickupdelay  |  Tümleştirme çalışma zamanı sıra süresi  |  Saniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Integrationruntimecpuyüzdesi  |  Tümleştirme çalışma zamanı CPU kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Integrationruntimequeuelength  |  Tümleştirme çalışma zamanı sıra uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Maxallowedfactorysizeingbits  |  İzin verilen en yüksek fabrika boyutu (GB birimi)  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  MaxAllowedResourceCount  |  İzin verilen en fazla varlık sayısı  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Ardışık düzen ınecancelledçalıştırmaları  |  İptal edilen işlem hattı çalışma ölçümleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Pipelinefailedçalıştırmaları  |  Başarısız işlem hattı çalıştırmaları ölçümleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Ardışık düzen Inesucceededçalıştırmaları  |  Başarılı işlem hattı çalışma ölçümleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  ResourceCount  |  Toplam varlık sayısı  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Triggercancelledçalıştırmaları  |  İptal edilen tetikleyici çalışan ölçümleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  Triggerfailedçalıştırmaları  |  Başarısız tetikleyici çalıştırma ölçümleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataFactory/Factory  |  TriggerSucceededRuns  |  Başarılı tetikleyici çalışan ölçümleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeAnalytics/hesapları  |  Jobauendediptal edildi  |  İptal edilen AU Saati  |  Saniye  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeAnalytics/hesapları  |  JobAUEndedFailure  |  Başarısız AU Saati  |  Saniye  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeAnalytics/hesapları  |  JobAUEndedSuccess  |  Başarılı AU Saati  |  Saniye  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeAnalytics/hesapları  |  Jobendediptal edildi  |  İptal edilen Işler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeAnalytics/hesapları  |  JobEndedFailure  |  Başarısız Işler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeAnalytics/hesapları  |  JobEndedSuccess  |  Başarılı Işler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeStore/accounts  |  DataRead  |  Okunan veriler  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeStore/accounts  |  Veri yazıldı  |  Yazılan veriler  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeStore/accounts  |  ReadRequests  |  Okuma Istekleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DataLakeStore/accounts  |  TotalStorage  |  Toplam Depolama Alanı  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. DataLakeStore/accounts  |  WriteRequests  |  Yazma Istekleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  active_connections  |  Etkin Bağlantılar  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  backup_storage_used  |  Kullanılan yedekleme depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  connections_failed  |  Başarısız Bağlantılar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  io_consumption_percent  |  GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  seconds_behind_master  |  Saniye cinsinden çoğaltma gecikmesi  |  Count  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  serverlog_storage_limit  |  Sunucu günlüğü depolama sınırı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  serverlog_storage_percent  |  Sunucu günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  serverlog_storage_usage  |  Kullanılan sunucu günlüğü depolaması  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  storage_percent  |  Depolama alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  storage_used  |  Kullanılan depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  active_connections  |  Etkin Bağlantılar  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  backup_storage_used  |  Kullanılan yedekleme depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  connections_failed  |  Başarısız Bağlantılar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  io_consumption_percent  |  GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  seconds_behind_master  |  Saniye cinsinden çoğaltma gecikmesi  |  Count  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  serverlog_storage_limit  |  Sunucu günlüğü depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  serverlog_storage_percent  |  Sunucu günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  serverlog_storage_usage  |  Kullanılan sunucu günlüğü depolaması  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  storage_percent  |  Depolama alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  storage_used  |  Kullanılan depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  active_connections  |  Etkin Bağlantılar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  backup_storage_used  |  Kullanılan yedekleme depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  connections_failed  |  Başarısız Bağlantılar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  io_consumption_percent  |  GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  pg_replica_log_delay_in_bytes  |  Çoğaltmalar genelinde en fazla gecikme  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  pg_replica_log_delay_in_seconds  |  Çoğaltma gecikmesi  |  Saniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  serverlog_storage_limit  |  Sunucu günlüğü depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  serverlog_storage_percent  |  Sunucu günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  serverlog_storage_usage  |  Kullanılan sunucu günlüğü depolaması  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  storage_percent  |  Depolama alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/sunucuları  |  storage_used  |  Kullanılan depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/serversv2  |  active_connections  |  Etkin Bağlantılar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/serversv2  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/serversv2  |  'ye  |  IOPS  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/serversv2  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/serversv2  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/serversv2  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/serversv2  |  storage_percent  |  Depolama alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. DBforPostgreSQL/serversv2  |  storage_used  |  Kullanılan depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/Account  |  digitaltwins. telemetri. düğümleri  |  Dijital TWINS düğümü telemetri yer tutucusu  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. Commands. çıkış. Abandon. Success  |  C2D iletileri bırakıldı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. Commands. çıkış. tamamlandı. başarılı  |  C2D ileti teslimleri tamamlandı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. Commands. çıkış. Red. Success  |  C2D iletileri reddedildi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. Methods. Failure  |  Başarısız doğrudan yöntem etkinleştirmeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. Methods. requestSize  |  Doğrudan yöntem etkinleştirmeleri istek boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. Methods. responseSize  |  Doğrudan yöntem etkinleştirmeleri yanıt boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. Methods. Success  |  Başarılı doğrudan yöntem etkinleştirmeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. ikizi. Read. Failure  |  Arka uçtan başarısız ikizi okumaları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. ikizi. Read. size  |  Arka uçtan gelen ikizi okumaların yanıt boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. ikizi. Read. Success  |  Arka uçtan başarılı ikizi okumaları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. ikizi. Update. Failure  |  Arka uçtan başarısız ikizi güncelleştirmeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. ikizi. Update. size  |  Arka uçtan ikizi güncelleştirmelerinin boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2D. ikizi. Update. Success  |  Arka uçtan başarılı ikizi güncelleştirmeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  C2DMessagesExpired  |  C2D Iletilerinin süre dolma (Önizleme)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  konfigürasyonları  |  Yapılandırma ölçümleri  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Devices/IotHubs  |  connectedDeviceCount  |  Bağlı cihazlar (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. çıkış. builtIn. Events  |  Yönlendirme: iletilere/olaylara teslim edilen iletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. çıkış. eventHubs  |  Yönlendirme: Olay Hub 'ına teslim edilen iletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. çıkış. serviceBusQueues  |  Yönlendirme: Service Bus kuyruğuna teslim edilen iletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. çıkış. Servicebuskonular  |  Yönlendirme: Service Bus konuya teslim edilen iletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. çıkış. Storage  |  Yönlendirme: depolamaya teslim edilen iletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. çıkış. Storage. blob 'ları  |  Yönlendirme: depolamaya teslim edilen Bloblar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. çıkış. Storage. Bytes  |  Yönlendirme: depolamaya teslim edilen veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. Latency. yerleik. Events  |  Yönlendirme: iletiler/olaylar için ileti gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. Latency. eventHubs  |  Yönlendirme: Olay Hub 'ı için ileti gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. Latency. serviceBusQueues  |  Yönlendirme: Service Bus kuyruğu için ileti gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. Latency. Servicebuskonular  |  Yönlendirme: Service Bus konusu için ileti gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. endpoints. Latency. Storage  |  Yönlendirme: depolama için ileti gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. telemetri. çıkış. bırakıldı  |  Yönlendirme: telemetri iletileri bırakıldı   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. telemetri. çıkış. geri dönüş  |  Yönlendirme: geri dönüşe teslim edilen iletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. telemetri. çıkış. geçersiz  |  Yönlendirme: telemetri iletileri uyumsuz  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. telemetri. çıkış. yalnız bırakılmış  |  Yönlendirme: telemetri iletileri yalnız bırakılmış   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. telemetri. çıkış. başarılı  |  Yönlendirme: teslim edilen telemetri iletileri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. telemetri. ınress. allProtocol  |  Telemetri iletisi gönderme denemeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. telemetri. ınress. Sendvaliz  |  Daraltma hatası sayısı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. telemetri. giriş. başarılı  |  Gönderilen telemetri iletileri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. ikizi. Read. Failure  |  Cihazlardan başarısız ikizi okumaları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. ikizi. Read. size  |  Cihazlardan gelen ikizi okumaların yanıt boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. ikizi. Read. Success  |  Cihazlardan başarılı ikizi okumaları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. ikizi. Update. Failure  |  Cihazlardan ikizi güncelleştirmeleri başarısız oldu  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. ikizi. Update. size  |  Cihazlardan ikizi güncelleştirmelerinin boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  D2C. ikizi. Update. Success  |  Cihazlardan başarılı ikizi güncelleştirmeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Günlükmessagequotakullanıldı  |  Kullanılan toplam ileti sayısı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  deviceDataUsage  |  Toplam cihaz verisi kullanımı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  deviceDataUsageV2  |  Toplam cihaz verisi kullanımı (Önizleme)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Devices. connectedDevices. allProtocol  |  Bağlı cihazlar (kullanım dışı)   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Devices. totalDevices  |  Toplam cihaz (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Eventgridteslimler  |  Event Grid teslimler (Önizleme)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  EventGridLatency  |  Event Grid gecikme süresi (Önizleme)  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Jobs. cancelJob. Failure  |  Başarısız iş iptalleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Jobs. cancelJob. Success  |  Başarılı iş iptalleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  işler. tamamlandı  |  Tamamlanan İşler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Jobs. createDirectMethodJob. Failure  |  Yöntem çağırma işlerinin oluşturma işlemi başarısız oldu  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Jobs. createDirectMethodJob. Success  |  Yöntem çağırma işlerinin başarılı oluşturmaları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Jobs. createTwinUpdateJob. Failure  |  İkizi Update işlerinin başarısız oluşturmaları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Jobs. createTwinUpdateJob. Success  |  İkizi Update işlerinin başarılı oluşturmaları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  işler. başarısız  |  Başarısız işler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Jobs. listJobs. hata  |  İşleri listelemek için başarısız çağrılar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  işler. listJobs. başarılı  |  İşleri listelemek için başarılı çağrılar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  Jobs. queryJobs. hata  |  Başarısız iş sorguları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  işler. queryJobs. başarılı  |  Başarılı iş sorguları  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Devices/IotHubs  |  totalDeviceCount  |  Toplam cihaz (Önizleme)  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  twinQueries. Failure  |  Başarısız ikizi sorguları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  twinQueries. resultSize  |  İkizi sorguları sonuç boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/IotHubs  |  twinQueries. Success  |  Başarılı ikizi sorguları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/provisioningServices  |  AttestationAttempts  |  Kanıtlama denemeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/provisioningServices  |  Deviceasyleri  |  Atanan cihazlar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/provisioningServices  |  Registrationdenemeleri  |  Kayıt denemeleri  |  Count  |  Toplam | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  AvailableStorage  |  Kullanılabilir Depolama Alanı  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  Cassandraconnectionkapanışları  |  Cassandra bağlantı kapanışları  |  Count  |  Toplam | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  Cassandrarequestücretleri  |  Cassandra Istek ücretleri  |  Count  |  Toplam | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  CassandraRequests  |  Cassandra Istekleri  |  Count  |  Count | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  Veri kullanımı  |  Veri Kullanımı  |  Bayt  |  Toplam | 
| **Evet**  | No |  UmentDB/databaseAccounts Microsoft.Doc  |  DeleteVirtualNetwork  |  DeleteVirtualNetwork  |  Count  |  Count | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  DocumentCount  |  Belge sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  DocumentQuota  |  Belge kotası  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  Indexusage  |  Dizin Kullanımı  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  Meta veri datarequests  |  Meta veri Istekleri  |  Count  |  Count | 
| **Evet**  | **Evet** |  UmentDB/databaseAccounts Microsoft.Doc  |  Mongorequestşarj  |  Mongo Istek ücreti  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  UmentDB/databaseAccounts Microsoft.Doc  |  MongoRequests  |  Mongo Istekleri  |  Count  |  Count | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  MongoRequestsCount  |  Mongo Istek hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  MongoRequestsDelete  |  Mongo silme Isteği hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  Mongorequestsınsert  |  Mongo ekleme Isteği oranı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  MongoRequestsQuery  |  Mongo sorgu Isteği hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  MongoRequestsUpdate  |  Mongo güncelleştirme Isteği hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  Provisionedüretilen Iş  |  Sağlanan İşleme Hızı  |  Count  |  Maksimum | 
| **Evet**  | No |  UmentDB/databaseAccounts Microsoft.Doc  |  ReplicationLatency  |  P99 çoğaltma gecikmesi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  UmentDB/databaseAccounts Microsoft.Doc  |  ServiceAvailability  |  Hizmet kullanılabilirliği  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  UmentDB/databaseAccounts Microsoft.Doc  |  TotalRequests  |  Toplam İstek Sayısı  |  Count  |  Count | 
| **Evet**  | **Evet** |  UmentDB/databaseAccounts Microsoft.Doc  |  TotalRequestUnits  |  Toplam Istek birimleri  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EnterpriseKnowledgeGraph/hizmetler  |  FailureCount  |  Başarısız İşlem Sayısı  |  Count  |  Count | 
| Hayır  | Hayır |  Microsoft. EnterpriseKnowledgeGraph/hizmetler  |  Başarılı sayısı  |  Başarılı İşlem Sayısı  |  Count  |  Count | 
| Hayır  | Hayır |  Microsoft. EnterpriseKnowledgeGraph/hizmetler  |  Başarılı gecikme  |  Başarı gecikmesi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EnterpriseKnowledgeGraph/hizmetler  |  Işlem sayısı  |  İşlem sayısı  |  Count  |  Count | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  DeadLetteredCount  |  Kullanılmayan olaylar  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventGrid/Domains  |  Ityattemptfailcount  |  Teslim başarısız olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  Deliverybaşarılı sayısı  |  Teslim edilen olaylar  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventGrid/Domains  |  DestinationProcessingDurationInMs  |  Hedef Işleme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  DroppedEventCount  |  Bırakılan olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  MatchedEventCount  |  Eşleşen olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  PublishFailCount  |  Başarısız olayları Yayımla  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  Publishbaşarılı sayısı  |  Yayımlanan olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  Publishbaşarıyla Latencınms  |  Yayımlama başarı gecikmesi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Eventabonelikleri  |  DeadLetteredCount  |  Kullanılmayan olaylar  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventGrid/Eventabonelikleri  |  Ityattemptfailcount  |  Teslim başarısız olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Eventabonelikleri  |  Deliverybaşarılı sayısı  |  Teslim edilen olaylar  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventGrid/Eventabonelikleri  |  DestinationProcessingDurationInMs  |  Hedef Işleme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Eventabonelikleri  |  DroppedEventCount  |  Bırakılan olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Eventabonelikleri  |  MatchedEventCount  |  Eşleşen olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Extensionkonuları  |  PublishFailCount  |  Başarısız olayları Yayımla  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Extensionkonuları  |  Publishbaşarılı sayısı  |  Yayımlanan olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Extensionkonuları  |  Publishbaşarıyla Latencınms  |  Yayımlama başarı gecikmesi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Extensionkonuları  |  UnmatchedEventCount  |  Eşleşmeyen olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/konuları  |  PublishFailCount  |  Başarısız olayları Yayımla  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/konuları  |  Publishbaşarılı sayısı  |  Yayımlanan olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/konuları  |  Publishbaşarıyla Latencınms  |  Yayımlama başarı gecikmesi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/konuları  |  UnmatchedEventCount  |  Eşleşmeyen olaylar  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  ActiveConnections  |  ActiveConnections  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  Kullanılabilirlik Blememory  |  Kullanılabilir Bellek  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  CaptureBacklog  |  Biriktirme listesini yakala.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  CapturedBytes  |  Yakalanan baytlar.  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  CapturedMessages  |  Yakalanan Iletiler.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  ConnectionsClosed  |  Kapatılan Bağlantılar.  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  ConnectionsOpened  |  Açılan Bağlantılar.  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  CPU  |  CPU  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  Incomingbytes  |  Gelen Bayt Miktarı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  Incomingmessages  |  Gelen İletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  Incomingrequests  |  Gelen İstekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  OutgoingBytes  |  Giden bayt sayısı.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  OutgoingMessages  |  Giden İletiler  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  QuotaExceededErrors  |  Kota Aşıldı Hataları.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  ServerErrors  |  Sunucu Hataları.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  SuccessfulRequests  |  Başarılı İstekler  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  OperationName  |  Kısıtlanan İstekler.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  UserErrors  |  Kullanıcı Hataları.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  ActiveConnections  |  ActiveConnections  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  CaptureBacklog  |  Biriktirme listesini yakala.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  CapturedBytes  |  Yakalanan baytlar.  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  CapturedMessages  |  Yakalanan Iletiler.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  ConnectionsClosed  |  Kapatılan Bağlantılar.  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  ConnectionsOpened  |  Açılan Bağlantılar.  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHABL  |  Biriktirme listesi iletilerini Arşivle (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHAMB  |  Arşiv iletisi verimlilik (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHAMESAJ  |  İletileri Arşivle (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHINBYTES  |  Gelen baytlar (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHINMB  |  Gelen bayt (geçersiz) (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHINILETISI  |  Gelen Iletiler (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHOUTBYTES  |  Giden baytlar (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHOUTMB  |  Giden baytlar (eski) (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  EHODıKILETISI  |  Giden Iletiler (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  FAILREQ  |  Başarısız Istekler (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  Incomingbytes  |  Gelen Bayt Miktarı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  Incomingmessages  |  Gelen İletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  Incomingrequests  |  Gelen İstekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  Giriş iletileri  |  Gelen Iletiler (eski) (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  INREQS  |  Gelen Istekler (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  INTERR  |  İç sunucu hataları (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  Hatalı cerr  |  Diğer hatalar (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  OutgoingBytes  |  Giden bayt sayısı.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  OutgoingMessages  |  Giden İletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  Dış mesaj  |  Giden Iletiler (eski) (kullanım dışı)  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  QuotaExceededErrors  |  Kota Aşıldı Hataları.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  ServerErrors  |  Sunucu Hataları.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  Boyut  |  Boyut  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  SuccessfulRequests  |  Başarılı İstekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  SUCCREQ  |  Başarılı Istekler (kullanım dışı)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/ad alanları  |  SVRBSY  |  Sunucu meşgul hataları (kullanım dışı)  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  OperationName  |  Kısıtlanan İstekler.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/ad alanları  |  UserErrors  |  Kullanıcı Hataları.  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. HDInsight/kümeler  |  Kategorizedgatewayrequests  |  Kategorilere ayrılmış ağ geçidi Istekleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. HDInsight/kümeler  |  GatewayRequests  |  Ağ Geçidi Istekleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. HDInsight/kümeler  |  Numactiveçalışanları  |  Etkin çalışan sayısı  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. HDInsight/kümeler  |  ScalingRequests  |  Ölçeklendirme istekleri  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Insights/oto Scalesettings  |  MetricThreshold  |  Ölçüm eşiği  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/oto Scalesettings  |  ObservedCapacity  |  Gözlenen kapasite  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/oto Scalesettings  |  ObservedMetricValue  |  Gözlemlenen ölçüm değeri  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/oto Scalesettings  |  Scaleactionsınıated  |  Ölçek eylemleri başlatıldı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  Kullanılabilirlik sonuçları/kullanılabilirliği yüzdesi  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  Kullanılabilirlik sonuçları/sayısı  |  Kullanılabilirlik testleri  |  Count  |  Count | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  Kullanılabilirlik sonuçları/süresi  |  Kullanılabilirlik testi süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/networkDuration  |  Sayfa yükleme ağ bağlantı süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/processingDuration  |  İstemci işlem süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/receiveDuration  |  Yanıt süresini alma  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/sendDuration  |  İstek gönderme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/totalDuration  |  Tarayıcı sayfa yükleme süresi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  Bağımlılıklar/sayı  |  Bağımlılık çağrıları  |  Count  |  Count | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  Bağımlılıklar/süre  |  Bağımlılık süresi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  Bağımlılıklar/başarısız  |  Bağımlılık çağrısı sorunları  |  Count  |  Count | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  özel durumlar/tarayıcı  |  Tarayıcı özel durumları  |  Count  |  Count | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  özel durumlar/say  |  Özel Durumlar  |  Count  |  Count | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  özel durumlar/sunucu  |  Sunucu özel durumları  |  Count  |  Count | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  pageViews/Count  |  Sayfa görünümleri  |  Count  |  Count | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  pageViews/Duration  |  Sayfa görünümü yükleme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  performanceCounters/exceptionsPerSecond  |  Özel durum oranı  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  performanceCounters/memoryAvailableBytes  |  Kullanılabilir bellek  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  performanceCounters/processCpuPercentage  |  İşlem CPU 'SU  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  performanceCounters/Processıobitespersecond  |  İşlem GÇ oranı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  performanceCounters/Processorcpuyüzdesi  |  İşlemci zamanı  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  performanceCounters/processPrivateBytes  |  İşlem özel baytları  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  performanceCounters/requestExecutionTime  |  HTTP isteği yürütme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  performanceCounters/Requestsınqueue  |  Uygulama kuyruğundaki HTTP istekleri  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  performanceCounters/requestsPerSecond  |  HTTP istek hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  istek/sayı  |  Sunucu istekleri  |  Count  |  Count | 
| **Evet**  | No |  Microsoft. Insights/bileşenler  |  istek/süre  |  Sunucu yanıt süresi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  istek/başarısız  |  Başarısız istekler  |  Count  |  Count | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  istek/hız  |  Sunucu isteği hızı  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  izlemeler/say  |  İzlemeler  |  Count  |  Count | 
| **Evet**  | No |  Microsoft. Keykasası/kasa  |  ServiceApiHit  |  Toplam hizmet API 'Si Isabet sayısı  |  Count  |  Count | 
| **Evet**  | No |  Microsoft. Keykasası/kasa  |  Hizmet Apilatesi  |  Genel hizmet API 'Si gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Keykasası/kasa  |  ServiceApiResult  |  Toplam hizmet API 'Si sonuçları  |  Count  |  Count | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Cachekullanım  |  Önbellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  ContinuousExportMaxLatenessMinutes  |  Sürekli dışarı aktarma en fazla dakika  |  Count  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Continuousexportnumofrecordsexports  |  Sürekli dışa aktarma-dışarı aktarılmış kayıtların sayısı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  ContinuousExportPendingCount  |  Sürekli dışarı aktarma bekleyen sayısı  |  Count  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  ContinuousExportResult  |  Sürekli dışarı aktarma sonucu  |  Count  |  Count | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  CPU  |  CPU  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Processedforeventhubs olayları  |  İşlenen Olaylar (olay/IoT Hub 'Ları için)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Dışarı Aktaramstilileştirme  |  Dışarı Aktarma Kullanımı  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Inestionlatencınseconds  |  Alma gecikmesi (saniye)  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Inestionresult  |  Alım sonucu  |  Count  |  Count | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Alım kullanımı  |  Alım kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Inestionvolumeınmb  |  Alım birimi (MB)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Alı  |  Canlı tut  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  QueryDuration  |  Sorgu süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  SteamingIngestRequestRate  |  Akış alma Isteği oranı  |  Count  |  Kterequestspersecond | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  StreamingIngestDataRate  |  Akış alma verileri oranı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  StreamingIngestDuration  |  Akış alma süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  StreamingIngestResults  |  Akış alma sonucu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionLatency  |  Eylem gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsCompleted  |  Tamamlanan eylemler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsFailed  |  Başarısız eylemler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsSkipped  |  Atlanan eylemler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsStarted  |  Başlatılan eylemler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsSucceeded  |  İşlemler başarılı oldu   |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Actionbaşarılı gecikme süresi  |  Eylem başarı gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionThrottledEvents  |  Eylem kısıtlanmış olaylar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Integrationserviceenvironmentconnectormemoryusage  |  Tümleştirme Hizmeti Ortamı için bağlayıcı bellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Integrationserviceenvironmentconnectorprocessorusage  |  Tümleştirme Hizmeti Ortamı için bağlayıcı Işlemci kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Integrationserviceenvironmentworkflowmemoryusage  |  Tümleştirme Hizmeti Ortamı için iş akışı bellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Integrationserviceenvironmentworkflowprocessorusage  |  Tümleştirme Hizmeti Ortamı için iş akışı Işlemcisi kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunFailurePercentage yüzdesi  |  Çalıştırma hatası yüzdesi  |  Yüzde  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunLatency  |  Çalıştırma gecikmesi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsCancelled  |  Çalıştırma Iptal edildi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsCompleted  |  Tamamlanan çalıştırmalar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsFailed  |  Çalıştırma başarısız  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsStarted  |  Çalışma başlatıldı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsSucceeded  |  Çalışma başarılı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunStartThrottledEvents  |  Kısıtlanmış başlangıç olaylarını Çalıştır  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Runbaşarılı gecikme süresi  |  Çalıştırma başarılı gecikme süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunThrottledEvents  |  Kısıtlanmış olayları Çalıştır  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Triggerfirelatlik  |  Tetikleyici Tetikleme gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggerLatency  |  Tetikleme gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersCompleted  |  Tamamlanan Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersFailed  |  Başarısız Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Triggersharekete geçirildi  |  Tetiklenen Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersSkipped  |  Atlanan Tetikleyiciler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersStarted  |  Başlatılan Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersSucceeded  |  Başarılı Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Triggerbaşarılı gecikme  |  Tetikleyici başarı gecikme süresi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggerThrottledEvents  |  Kısıtlanmış olayları tetikleyin  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/iş akışları  |  ActionLatency  |  Eylem gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  ActionsCompleted  |  Tamamlanan eylemler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  ActionsFailed  |  Başarısız eylemler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  ActionsSkipped  |  Atlanan eylemler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  ActionsStarted  |  Başlatılan eylemler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  ActionsSucceeded  |  İşlemler başarılı oldu   |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/iş akışları  |  Actionbaşarılı gecikme süresi  |  Eylem başarı gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  ActionThrottledEvents  |  Eylem kısıtlanmış olaylar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  Billableactionyürütmeler  |  Faturalanabilir eylem yürütmeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  Billabletriggeryürütmeler  |  Faturalanabilir tetikleyici yürütmeleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  BillingUsageNativeOperation  |  Yerel Işlem yürütmeleri için faturalandırma kullanımı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  BillingUsageNativeOperation  |  Yerel Işlem yürütmeleri için faturalandırma kullanımı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  BillingUsageStandardConnector  |  Standart bağlayıcı yürütmeleri için faturalandırma kullanımı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  BillingUsageStandardConnector  |  Standart bağlayıcı yürütmeleri için faturalandırma kullanımı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  Billingusagestorampatüketim  |  Depolama tüketimi yürütmeleri için faturalama kullanımı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  Billingusagestorampatüketim  |  Depolama tüketimi yürütmeleri için faturalama kullanımı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  RunFailurePercentage yüzdesi  |  Çalıştırma hatası yüzdesi  |  Yüzde  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/iş akışları  |  RunLatency  |  Çalıştırma gecikmesi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  RunsCancelled  |  Çalıştırma Iptal edildi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  RunsCompleted  |  Tamamlanan çalıştırmalar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  RunsFailed  |  Çalıştırma başarısız  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  RunsStarted  |  Çalışma başlatıldı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  RunsSucceeded  |  Çalışma başarılı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  RunStartThrottledEvents  |  Kısıtlanmış başlangıç olaylarını Çalıştır  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/iş akışları  |  Runbaşarılı gecikme süresi  |  Çalıştırma başarılı gecikme süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  RunThrottledEvents  |  Kısıtlanmış olayları Çalıştır  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  Totalbillableyürütmeleri  |  Toplam faturalandırılabilir yürütmeler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/iş akışları  |  Triggerfirelatlik  |  Tetikleyici Tetikleme gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Logic/iş akışları  |  TriggerLatency  |  Tetikleme gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  TriggersCompleted  |  Tamamlanan Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  TriggersFailed  |  Başarısız Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  Triggersharekete geçirildi  |  Tetiklenen Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  TriggersSkipped  |  Atlanan Tetikleyiciler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  TriggersStarted  |  Başlatılan Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  TriggersSucceeded  |  Başarılı Tetikleyiciler   |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Logic/iş akışları  |  Triggerbaşarılı gecikme  |  Tetikleyici başarı gecikme süresi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/iş akışları  |  TriggerThrottledEvents  |  Kısıtlanmış olayları tetikleyin  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Etkin çekirdekler  |  Etkin çekirdekler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Etkin düğümler  |  Etkin düğümler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Tamamlanan çalıştırmalar  |  Tamamlanan çalıştırmalar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Başarısız çalıştırmalar  |  Başarısız çalıştırmalar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Boştaki çekirdekler  |  Boştaki çekirdekler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Boştaki düğümler  |  Boştaki düğümler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Çekirdekleri bırakma  |  Çekirdekleri bırakma  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Düğümlerden çıkılıyor  |  Düğümlerden çıkılıyor  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Model Dağıtımı başarısız oldu  |  Model Dağıtımı başarısız oldu  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Model Dağıtımı başladı  |  Model Dağıtımı başladı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Model Dağıtımı başarılı oldu  |  Model Dağıtımı başarılı oldu  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Model kaydı başarısız oldu  |  Model kaydı başarısız oldu  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Model kaydı başarılı oldu  |  Model kaydı başarılı oldu  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Önden çekirdekler  |  Önden çekirdekler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Önden düğümler  |  Önden düğümler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Kota kullanım yüzdesi  |  Kota kullanım yüzdesi  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Başlatılan çalıştırmalar  |  Başlatılan çalıştırmalar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Toplam çekirdek sayısı  |  Toplam çekirdek sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Toplam düğüm sayısı  |  Toplam düğüm sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Kullanılamayan çekirdekler  |  Kullanılamayan çekirdekler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. MachineLearningServices/çalışma alanları  |  Kullanılamayan düğümler  |  Kullanılamayan düğümler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Maps/hesaplar  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Maps/hesaplar  |  Kullanım  |  Kullanım  |  Count  |  Count | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  AssetCount  |  Varlık sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  AssetQuota  |  Varlık kotası  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  AssetQuotaUsedPercentage  |  Kullanılan varlık kotası yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  ContentKeyPolicyCount  |  İçerik anahtarı Ilke sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  ContentKeyPolicyQuota  |  İçerik anahtarı Ilke kotası  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  ContentKeyPolicyQuotaUsedPercentage  |  İçerik anahtarı Ilke kotası kullanılan yüzde  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  Streammingpolicycount  |  Akış Ilkesi sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  StreamingPolicyQuota  |  Akış Ilkesi kotası  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices  |  StreamingPolicyQuotaUsedPercentage  |  Akış Ilkesi kotası kullanılan yüzde  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Media/mediaservices/streamingEndpoints  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Media/mediaservices/streamingEndpoints  |  İstekler  |  İstekler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Media/mediaservices/streamingEndpoints  |  SuccessE2ELatency  |  Başarılı uçtan uca gecikme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  GCPauseTotalCount  |  GC duraklatma sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  GCPauseTotalTime  |  GC duraklatma toplam süre  |  Mayacak  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  MaxOldGenMemoryPoolBytes  |  En fazla kullanılabilir eski nesil veri boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  OldGenMemoryPoolBytes  |  Eski nesil veri boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  OldGenPromotedBytes  |  Eski nesil veri boyutuna yükselt  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  ServiceCpuUsagePercentage  |  Hizmet CPU kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  ServiceMemoryCommitted  |  Hizmet belleği atandı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  ServiceMemoryMax  |  En fazla hizmet belleği  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  Servicememorykullanıldı  |  Kullanılan hizmet belleği  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  SystemCpuUsagePercentage  |  Sistem CPU kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatErrorCount  |  Tomcat genel hatası  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  Tomcatreceived baytları  |  Tomcat alınan toplam bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatRequestMaxTime  |  Tomcat Isteği maksimum zamanı  |  Mayacak  |  Maksimum | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatRequestTotalCount  |  Tomcat Isteği toplam sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatRequestTotalTime  |  Tomcat Isteği toplam kez  |  Mayacak  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatResponseAvgTime  |  Tomcat Isteği ortalama zamanı  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSentBytes  |  Tomcat gönderilen toplam bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionActiveCurrentCount  |  Tomcat oturum etkin sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionActiveMaxCount  |  Tomcat oturumu en fazla etkin sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionAliveMaxTime  |  Tomcat oturumu maksimum etkin süresi  |  Mayacak  |  Maksimum | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionCreatedCount  |  Tomcat oturum oluşturma sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionExpiredCount  |  Tomcat oturumunun süre dolma sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionRejectedCount  |  Tomcat oturumu reddedildi sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Microservices4Spring/Appkümeler  |  YoungGenPromotedBytes  |  Küçük ölçekli oluşturma veri boyutuna yükselt  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları  |  VolumePoolAllocatedUsed  |  Ayrılan birim havuzu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları  |  VolumePoolTotalLogicalSize  |  Birim havuzu toplam mantıksal boyut  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  AverageReadLatency  |  Ortalama okuma gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  AverageWriteLatency  |  Ortalama yazma gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  ReadIops  |  IOPS 'yi oku  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  Birimlogicalsize  |  Birim mantıksal boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  VolumeSnapshotSize  |  Birim anlık görüntü boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  Writeıops  |  IOPS yaz  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  ApplicationGatewayTotalTime  |  Application Gateway toplam süre  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  Avgrequestcountperhealthyıhost  |  Sağlıklı ana bilgisayar başına dakika başına istek  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  BackendConnectTime  |  Arka uç bağlantı saati  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  BackendFirstByteResponseTime  |  Arka uç Ilk bayt yanıt süresi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  BackendLastByteResponseTime  |  Arka uç son bayt yanıt süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  BackendResponseStatus  |  Arka uç yanıt durumu  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  BlockedCount  |  Web uygulaması güvenlik duvarı engellenen Istek kuralı dağıtımı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  BlockedReqCount  |  Web uygulaması güvenlik duvarı engellenen Istek sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  BytesReceived  |  Alınan bayt  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  BytesSent  |  Gönderilen bayt  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  CapacityUnits  |  Geçerli kapasite birimleri  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  ClientRtt  |  İstemci RTT  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  ComputeUnits  |  Geçerli Işlem birimleri  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  CurrentConnections  |  Geçerli bağlantılar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  FailedRequests  |  Başarısız İstekler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  Healthyıhostcount  |  Sağlıklı konak sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  MatchedCount  |  Web uygulaması güvenlik duvarı toplam kural dağıtımı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  ResponseStatus  |  Yanıt durumu  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Network/Applicationgateway 'ler  |  Aktarım hızı  |  Aktarım hızı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  TlsProtocol  |  İstemci TLS protokolü  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  TotalRequests  |  Toplam İstek Sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Applicationgateway 'ler  |  Unhealthyıhostcount  |  Sağlıksız konak sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/azurefirewalls  |  ApplicationRuleHit  |  Uygulama kuralları isabet sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/azurefirewalls  |  Veri Işlendi  |  İşlenen veri  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/azurefirewalls  |  FirewallHealth  |  Güvenlik duvarı sistem durumu  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/azurefirewalls  |  NetworkRuleHit  |  Ağ kuralları isabet sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/azurefirewalls  |  SNATPortUtilization  |  SNAT bağlantı noktası kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Connections  |  BitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Connections  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/dnszones  |  QueryVolume  |  Sorgu hacmi  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Network/dnszones  |  Recordsetkapaıyutilileştirme  |  Kayıt kümesi kapasite kullanımı  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/dnszones  |  Kayıt kümesi sayısı  |  Kayıt kümesi sayısı  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Expressroutedevreleri  |  ArpAvailability  |  ARP kullanılabilirliği  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Expressroutedevreleri  |  BgpAvailability  |  BGP kullanılabilirliği  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  BitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  GlobalReachBitsInPerSecond  |  GlobalReachBitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  GlobalReachBitsOutPerSecond  |  GlobalReachBitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  QosDropBitsInPerSecond  |  DroppedInBitsPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  QosDropBitsOutPerSecond  |  DroppedOutBitsPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Expressroutedevreleri/eşayarları  |  BitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Expressroutedevreleri/eşayarları  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutegateway 'ler  |  ErGatewayConnectionBitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutegateway 'ler  |  ErGatewayConnectionBitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/expressRoutePorts  |  AdminState  |  AdminState  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/expressRoutePorts  |  LineProtocol  |  LineProtocol  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/expressRoutePorts  |  PortBitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/expressRoutePorts  |  PortBitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/expressRoutePorts  |  RxLightLevel  |  RxLightLevel  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/expressRoutePorts  |  TxLightLevel  |  TxLightLevel  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  BackendHealthPercentage  |  Arka uç sistem durumu yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  BackendRequestCount  |  Arka uç Istek sayısı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  BackendRequestLatency  |  Arka uç Isteği gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  Billilableresponsesize  |  Faturalanabilir yanıt boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  Istek sayısı  |  İstek Sayısı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  RequestSize  |  İstek boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  Yanıt boyutu  |  Yanıt boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  TotalLatency  |  Toplam gecikme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  WebApplicationFirewallRequestCount  |  Web uygulaması güvenlik duvarı Istek sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Network/loadBalancers  |  AllocatedSnatPorts  |  Ayrılan SNAT bağlantı noktaları (Önizleme)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/loadBalancers  |  ByteCount  |  Bayt sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/loadBalancers  |  DipAvailability  |  Durum Yoklaması Durumu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/loadBalancers  |  PacketCount  |  Paket Sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/loadBalancers  |  SnatConnectionCount  |  SNAT bağlantı sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/loadBalancers  |  SYNCount  |  SYN sayısı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Network/loadBalancers  |  UsedSnatPorts  |  Kullanılan SNAT bağlantı noktaları (Önizleme)  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/loadBalancers  |  VipAvailability  |  Veri Yolu Kullanılabilirliği  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/NetworkInterfaces  |  BytesReceivedRate  |  Alınan bayt  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/NetworkInterfaces  |  BytesSentRate  |  Gönderilen bayt  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/NetworkInterfaces  |  PacketsReceivedRate  |  Alınan paketler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/NetworkInterfaces  |  PacketsSentRate  |  Gönderilen paketler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/networkWatchers/Connectionmonitörleri  |  AverageRoundtripMs  |  Ort. gidiş dönüş süresi (MS)  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/networkWatchers/Connectionmonitörleri  |  ChecksFailedPercent  |  Başarısız denetimler yüzdesi (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/networkWatchers/Connectionmonitörleri  |  ProbesFailedPercent  |  % Yoklama başarısız oldu  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/networkWatchers/Connectionmonitörleri  |  Roundroundtimems  |  Gidiş dönüş süresi (MS) (Önizleme)  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  ByteCount  |  Bayt sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  BytesDroppedDDoS  |  Gelen bayt bırakıldı DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  BytesForwardedDDoS  |  İletilen gelen bayt sayısı  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  BytesInDDoS  |  Gelen bayt sayısı DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  Ddobir Ggersynpackets  |  DDoS azaltma tetiklenmesi için gelen SYN paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  DDO, Ggertcppackets  |  DDoS azaltma tetiklemeye yönelik gelen TCP paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  DDO, Ggerudppaketleri  |  DDoS risk azaltma tetiklenecek gelen UDP paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  Iunderddosattack  |  DDoS saldırısı altında  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  PacketCount  |  Paket Sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  PacketsDroppedDDoS  |  DDoS bırakılan gelen paketler  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  PacketsForwardedDDoS  |  DDoS iletilen gelen paketler  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  PacketsInDDoS  |  Gelen paketler DDoS  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  SynCount  |  SYN sayısı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  TCPBytesDroppedDDoS  |  Gelen TCP bayt bırakılan DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  TCPBytesForwardedDDoS  |  İletilen gelen TCP baytları  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  TCPBytesInDDoS  |  Gelen TCP bayt DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  TCPPacketsDroppedDDoS  |  Gelen TCP paketleri bırakıldı DDoS  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  TCPPacketsForwardedDDoS  |  DDoS iletilen gelen TCP paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  TCPPacketsInDDoS  |  Gelen TCP paketleri DDoS  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  UDPBytesDroppedDDoS  |  Gelen UDP bayt bırakılan DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  UDPBytesForwardedDDoS  |  İletilen gelen UDP baytları  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  UDPBytesInDDoS  |  Gelen UDP bayt sayısı DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  UDPPacketsDroppedDDoS  |  Gelen UDP paketleri bırakıldı  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  UDPPacketsForwardedDDoS  |  DDoS iletilen gelen UDP paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  UDPPacketsInDDoS  |  Gelen UDP paketleri DDoS  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Publicıpaddresses  |  VipAvailability  |  Veri Yolu Kullanılabilirliği  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/trafficManagerProfiles  |  Probeagentcurrentendpointstatebyprofileresourceıd  |  Uç nokta tarafından uç nokta durumu  |  Count  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Network/trafficManagerProfiles  |  QpsByEndpoint  |  Bitiş noktasına göre sorgular döndürüldü  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  AverageBandwidth  |  Ağ Geçidi S2S bant genişliği  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  P2SBandwidth  |  Ağ Geçidi P2S bant genişliği  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  P2SConnectionCount  |  P2S Bağlantı Sayısı  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelAverageBandwidth  |  Tünel Bant Genişliği  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelEgressBytes  |  Tünel Çıkış Baytları  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelEgressPacketDropTSMismatch  |  Tünel Çıkış TS Uyumsuzluğu Paket Bırakma  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelEgressPackets  |  Tünel Çıkış Paketleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelIngressBytes  |  Tünel Giriş Baytları  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelIngressPacketDropTSMismatch  |  Tünel Giriş TS Uyumsuzluğu Paket Bırakma  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelIngressPackets  |  Tünel giriş paketleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Network/virtualNetworks  |  PingMeshAverageRoundtripMs  |  Bir VM 'ye ping için gidiş dönüş süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Network/virtualNetworks  |  PingMeshProbesFailedPercent  |  VM 'ye ping işlemi başarısız oldu  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen  |  Gelen İletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen. tüm. failedistekleri  |  Tüm gelen başarısız Istekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen. ALL. Requests  |  Tüm gelen Istekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen. zamanlandı  |  Zamanlanan anında Iletme bildirimleri gönderildi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen. zamanlandı. iptal  |  Zamanlanan anında Iletme bildirimleri Iptal edildi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. tümü  |  Yükleme yönetimi Işlemleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. Delete  |  Yükleme Işlemlerini silme  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. Get  |  Yükleme Işlemlerini al  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. Patch  |  Düzeltme eki yükleme Işlemleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. upsert  |  Yükleme Işlemlerini oluşturma veya güncelleştirme  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  notificationhub. gönderimleri  |  Tüm giden bildirimler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. allpns. badorexpiredchannel  |  Hatalı veya süre dolma kanalı hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. allpns. channelerror  |  Kanal hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. allpns. ınvalidpayload  |  Yük hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. allpns. pnserror  |  Dış bildirim sistemi hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. allpns. Success  |  Başarılı bildirimler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. badchannel  |  APNS geçersiz kanal hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. expiredchannel  |  APNS süre dolma kanalı hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. ınvalidcredentials  |  APNS yetkilendirme hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. ınvalidnotificationsize  |  APNS geçersiz bildirim boyutu hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. pnserror  |  APNS hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. Success  |  APNS başarılı bildirimler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. authenticationerror  |  GCM kimlik doğrulama hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. badchannel  |  GCM hatalı kanal hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. expiredchannel  |  GCM süre aşımına uğradı kanal hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. ınvalidcredentials  |  GCM yetkilendirme hataları (geçersiz kimlik bilgileri)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. ınvalidnotificationformat  |  GCM geçersiz bildirim biçimi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. ınvalidnotificationsize  |  GCM geçersiz bildirim boyutu hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. pnserror  |  GCM hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. Success  |  GCM başarılı bildirimleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. kısıtlanıyor  |  GCM kısıtlanan bildirimler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. yanlışlıkla gchannel  |  GCM yanlış kanal hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. authenticationerror  |  MPNS kimlik doğrulama hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. badchannel  |  MPNS hatalı kanal hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. channelconnected bağlantısı kesildi  |  MPNS kanalının bağlantısı kesildi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. bırakıldı  |  MPNS bırakılan bildirimler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. ınvalidcredentials  |  MPNS geçersiz kimlik bilgileri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. ınvalidnotificationformat  |  MPNS geçersiz bildirim biçimi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. pnserror  |  MPNS hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. Success  |  MPNS başarılı bildirimleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. kısıtlanıyor  |  MPNS kısıtlanan bildirimler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. authenticationerror  |  WNS kimlik doğrulama hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. badchannel  |  WNS hatalı kanal hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. channelconnected bağlantısı kesildi  |  WNS kanal bağlantısı kesildi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. channelkısıtlanıyor  |  WNS kanal kısıtlandı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. bırakıldı  |  WNS bırakılan bildirimler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. expiredchannel  |  WNS süre dolduğunda kanal hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. ınvalidcredentials  |  WNS yetkilendirme hataları (geçersiz kimlik bilgileri)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. ınvalidnotificationformat  |  WNS geçersiz bildirim biçimi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. ınvalidnotificationsize  |  WNS geçersiz bildirim boyutu hatası  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. ınvalidtoken  |  WNS yetkilendirme hataları (geçersiz belirteç)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. pnserror  |  WNS hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. Success  |  WNS başarılı bildirimleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. kısıtlanıyor  |  WNS kısıtlanıyor bildirimleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. tokenproviderunreachable  |  WNS yetkilendirme hataları (ulaşılamıyor)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. yanlışlıkla gtoken  |  WNS yetkilendirme hataları (yanlış belirteç)  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. tümü  |  Kayıt Işlemleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. oluştur  |  Kayıt oluşturma Işlemleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. Sil  |  Kayıt silme Işlemleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. Get  |  Kayıt okuma Işlemleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. güncelleştirme  |  Kayıt güncelleştirme Işlemleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  zamanlandı. bekliyor  |  Bekleyen zamanlanmış bildirimler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Kullanılabilir% Average_ bellek  |  % Kullanılabilir bellek  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% kullanılabilir takas alanı  |  % Kullanılabilir takas alanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  % Average_ kaydedilmiş bayt kullanımda  |  Kullanılan kaydedilmiş bayt yüzdesi  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% DPC Zamanı  |  % DPC Zamanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% boş ınomdes  |  % Boş ınomdes  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  % Average_ boş alan  |  % Boş alan  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  % Average_ boş alan  |  % Boş alan  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% boşta kalma süresi  |  % Boşta kalma süresi  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Kesme zamanı Average_%  |  Kesme zamanı yüzdesi  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% GÇ bekleme süresi  |  % GÇ bekleme süresi  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% Iyi zaman  |  % İyi zaman  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% ayrıcalıklı zaman  |  % Ayrıcalıklı zaman  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% Işlemci zamanı  |  % İşlemci zamanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% Işlemci zamanı  |  % İşlemci zamanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% kullanılan ınomdes  |  % Kullanılan ınomdes  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Kullanılan% Average_ bellek  |  % Kullanılan bellek  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Kullanılan Average_ alan yüzdesi  |  % Kullanılan alan  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% kullanılan takas alanı  |  % Kullanılan takas alanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% Kullanıcı Zamanı  |  % Kullanıcı Zamanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Available MBayt  |  Kullanılabilir MBayt  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Available MBayt bellek  |  Kullanılabilir MBayt belleği  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Available MBayt takas  |  Kullanılabilir MBayt takas  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Okuma  |  Ortalama Disk sn/okuma  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Okuma  |  Ortalama Disk sn/okuma  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Aktarım  |  Ortalama Disk sn/Aktarım  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Yazma  |  Ortalama Disk sn/yazma  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Yazma  |  Ortalama Disk sn/yazma  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Alınan Average_Bytes/sn  |  Alınan bayt/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Gönderilen Average_Bytes/sn  |  Gönderilen bayt/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Toplam Average_Bytes/sn  |  Toplam bayt/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Current disk kuyruğu uzunluğu  |  Geçerli disk sırası uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk okunan bayt/sn  |  Disk okuma bayt/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Okuma/sn Average_Disk  |  Disk Okuma/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Okuma/sn Average_Disk  |  Disk Okuma/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk aktarım sayısı/sn  |  Disk aktarımı/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk aktarım sayısı/sn  |  Disk aktarımı/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk yazma bayt/sn  |  Disk yazma bayt/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk yazma/sn  |  Disk yazma/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk yazma/sn  |  Disk yazma/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Free megabayt  |  Boş megabayt  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Free megabayt  |  Boş megabayt  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Fiziksel bellek Average_Free  |  Boş fiziksel bellek  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Disk belleği dosyalarında Average_Free alanı  |  Disk belleği dosyalarındaki boş alan  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Sanal bellek Average_Free  |  Boş sanal bellek  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Logical disk bayt/sn  |  Mantıksal disk bayt/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Okuma/sn Average_Page  |  Sayfa Okuma/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Page yazma/sn  |  Sayfa yazma/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Pages/sn  |  Sayfa/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Pct ayrıcalıklı saat  |  Ayrıcalıklı zaman yüzdesi  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Pct Kullanıcı saati  |  Kullanım süresi yüzdesi  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Physical disk bayt/sn  |  Fiziksel disk bayt/sn  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Processes  |  İşlemler  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Processor kuyruğu uzunluğu  |  İşlemci kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Disk belleği dosyalarında depolanan Average_Size  |  Disk belleği dosyalarında depolanan boyut  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total bayt  |  Toplam bayt sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total alınan bayt  |  Alınan toplam bayt  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total aktarılan baytlar  |  Aktarılan toplam bayt sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total çakışmaları  |  Toplam çakışma sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Alınan Average_Total paketleri  |  Alınan toplam paket sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Iletilen Average_Total paketleri  |  Aktarılan toplam paket sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total RX hataları  |  Toplam RX hatası sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total TX hataları  |  Toplam TX hatası sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Uptime  |  Hizmet  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Used MBayt alanı değiştirme  |  Kullanılan MBayt değiştirme alanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Used belleği Kbayt  |  Kullanılan bellek Kbayt  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Used belleği MBayt  |  Kullanılan bellek MBayt  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Users  |  Kullanıcılar  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Virtual paylaşılan bellek  |  Sanal paylaşılan bellek  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Olay  |  Olay  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Operationalınsights/çalışma alanları  |  Sinyal  |  Sinyal  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Operationalınsights/çalışma alanları  |  Güncelleştir  |  Güncelleştir  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Powerbiadanmış/kapasiteler  |  memory_metric  |  Bellek  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Powerbiadanmış/kapasiteler  |  memory_thrashing_metric  |  Bellek kullanımı (veri kümeleri)  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Powerbiadanmış/kapasiteler  |  qpu_high_utilization_metric  |  QPU Yüksek Kullanımı  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Powerbiadanmış/kapasiteler  |  QueryDuration  |  Sorgu süresi (veri kümeleri)  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Powerbiadanmış/kapasiteler  |  QueryPoolJobQueueLength  |  Sorgu havuzu Iş kuyruğu uzunluğu (veri kümeleri)  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ActiveConnections  |  ActiveConnections  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ActiveListeners  |  ActiveListeners  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Relay/Namespace  |  BytesTransferred  |  BytesTransferred  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ListenerConnections-ClientError  |  ListenerConnections-ClientError  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ListenerConnections-Sunucuhatası  |  ListenerConnections-Sunucuhatası  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ListenerConnections-başarılı  |  ListenerConnections-başarılı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ListenerConnections-TotalRequests  |  ListenerConnections-TotalRequests  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  Listenerdisconnect  |  Listenerdisconnect  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  SenderConnections-ClientError  |  SenderConnections-ClientError  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  SenderConnections-Sunucuhatası  |  SenderConnections-Sunucuhatası  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  SenderConnections-başarılı  |  SenderConnections-başarılı  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  SenderConnections-TotalRequests  |  SenderConnections-TotalRequests  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  Gönderenin bağlantısı kesiliyor  |  Gönderenin bağlantısı kesiliyor  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Search/searchServices  |  SearchLatency  |  Arama gecikmesi  |  Saniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Search/searchServices  |  SearchQueriesPerSecond  |  Sorgu arama/saniye  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Search/searchServices  |  Tüm Searchqueriespercentage 'ı kısıtlar  |  Kısıtlanmış arama sorguları yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  ActiveConnections  |  ActiveConnections  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  ActiveMessages  |  Bir kuyruktaki/konudaki etkin ileti sayısı.  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  ConnectionsClosed  |  Kapatılan Bağlantılar.  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  ConnectionsOpened  |  Açılan Bağlantılar.  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  CPUXNS  |  CPU (kullanım dışı)  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  DeadletteredMessages  |  Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı.  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. ServiceBus/ad alanları  |  Incomingmessages  |  Gelen İletiler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. ServiceBus/ad alanları  |  Incomingrequests  |  Gelen İstekler  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  İletiler  |  Kuyruktaki/Konudaki iletilerin sayısı.  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  NamespaceCpuUsage  |  CPU  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  NamespaceMemoryUsage  |  Bellek Kullanımı  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. ServiceBus/ad alanları  |  OutgoingMessages  |  Giden İletiler  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  ScheduledMessages  |  Bir kuyruktaki/konudaki zamanlanan ileti sayısı.  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  ServerErrors  |  Sunucu Hataları.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  Boyut  |  Boyut  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  SuccessfulRequests  |  Başarılı İstekler  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  OperationName  |  Kısıtlanan İstekler.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  UserErrors  |  Kullanıcı Hataları.  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ServiceBus/ad alanları  |  WSXNS  |  Bellek kullanımı (kullanım dışı)  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ActualCpu  |  ActualCpu  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ActualMemory  |  ActualMemory  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Ayrılatedcpu  |  Ayrılatedcpu  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Ayrılan bellek  |  Ayrılan bellek  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ApplicationStatus  |  ApplicationStatus  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ContainerStatus  |  ContainerStatus  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Cpukullanımı  |  Cpukullanımı  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Memoryutilileştirme  |  Memoryutilileştirme  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  RestartCount  |  RestartCount  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Servicereperepstatus  |  Servicereperepstatus  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ServiceStatus  |  ServiceStatus  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  ConnectionCount  |  Bağlantı sayısı  |  Count  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  Inboundtraffic  |  Gelen Trafik  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  MessageCount  |  İleti Sayısı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  OutboundTraffic  |  Giden Trafik  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  SystemErrors  |  Sistem hataları  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  UserErrors  |  Kullanıcı hataları  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SQL/ManagedInstances  |  avg_cpu_percent  |  Ortalama CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/ManagedInstances  |  io_bytes_read  |  Okunan GÇ baytları  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/ManagedInstances  |  io_bytes_written  |  Yazılan GÇ baytları  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/ManagedInstances  |  io_requests  |  GÇ istek sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/ManagedInstances  |  reserved_storage_mb  |  Ayrılan depolama alanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/ManagedInstances  |  storage_space_used_mb  |  Kullanılan depolama alanı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/ManagedInstances  |  virtual_core_count  |  Sanal çekirdek sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/sunucuları  |  database_dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/sunucuları  |  database_storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/sunucuları  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/sunucuları  |  dtu_used  |  Kullanılan DTU  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/sunucuları  |  storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  app_cpu_billed  |  Faturalandırılan uygulama CPU 'SU  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  app_cpu_percent  |  Uygulama CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  app_memory_percent  |  Uygulama belleği yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  blocked_by_firewall  |  Güvenlik duvarı tarafından engellendi  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  cache_hit_percent  |  İsabetli önbellek okuması yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  cache_used_percent  |  Önbellek kullanılan yüzde  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  connection_failed  |  Başarısız Bağlantılar  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  connection_successful  |  Başarılı bağlantılar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  cpu_limit  |  CPU sınırı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  cpu_used  |  Kullanılan CPU  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  Çözül  |  Çık  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  dtu_limit  |  DTU sınırı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  dtu_used  |  Kullanılan DTU  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  dwu_consumption_percent  |  DWU yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  dwu_limit  |  DWU sınırı  |  Count  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  dwu_used  |  Kullanılan DWU  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  local_tempdb_usage_percent  |  Yerel tempdb yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  log_write_percent  |  Günlük GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  memory_usage_percent  |  Bellek yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  sessions_percent  |  Oturum yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  depolama  |  Kullanılan veri alanı  |  Bayt  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  storage_percent  |  Kullanılan veri alanı yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  tempdb_data_size  |  Tempdb veri dosyası boyutu kilobayt  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  tempdb_log_size  |  Tempdb günlük dosyası boyutu kilobayt  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/veritabanları  |  tempdb_log_used_percent  |  Kullanılan tempdb günlüğü yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  workers_percent  |  Çalışan yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/veritabanları  |  xtp_storage_percent  |  Bellek içi OLTP depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  allocated_data_storage_percent  |  Ayrılan veri alanı yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  cpu_limit  |  CPU sınırı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/Elaun havuzları  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  cpu_used  |  Kullanılan CPU  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_cpu_limit  |  CPU sınırı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_cpu_used  |  Kullanılan CPU  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_eDTU_used  |  eDTU kullanıldı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_log_write_percent  |  Günlük GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_sessions_percent  |  Oturum yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/Servers/Elaun havuzları  |  database_workers_percent  |  Çalışan yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/Elaun havuzları  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  eDTU_limit  |  eDTU sınırı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/Elaun havuzları  |  eDTU_used  |  eDTU kullanıldı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/Elaun havuzları  |  log_write_percent  |  Günlük GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/Elaun havuzları  |  physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/Elaun havuzları  |  sessions_percent  |  Oturum yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  storage_limit  |  Veri boyutu üst sınırı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  storage_percent  |  Kullanılan veri alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  tempdb_data_size  |  Tempdb veri dosyası boyutu kilobayt  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  tempdb_log_size  |  Tempdb günlük dosyası boyutu kilobayt  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. SQL/Servers/Elaun havuzları  |  tempdb_log_used_percent  |  Kullanılan tempdb günlüğü yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/Elaun havuzları  |  workers_percent  |  Çalışan yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/Servers/Elaun havuzları  |  xtp_storage_percent  |  Bellek içi OLTP depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts  |  UsedCapacity  |  Kullanılan kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/blobServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/blobServices  |  BlobCapacity  |  Blob kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/blobServices  |  BlobCount  |  BLOB sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/blobServices  |  ContainerCount  |  Blob kapsayıcı sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/blobServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/blobServices  |  Dizin kapasitesi  |  Dizin kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/blobServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/blobServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/blobServices  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/blobServices  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/fileServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/fileServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Dosya kapasitesi  |  Dosya kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  FileCount  |  Dosya sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Dosya ShareCount  |  Dosya paylaşma sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Dosya Sharequota  |  Dosya paylaşımının kota boyutu  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Dosya paylaşımının anlık görüntü sayısı  |  Count  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Dosya paylaşımının anlık görüntü boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/fileServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/fileServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/fileServices  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/fileServices  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  QueueCapacity  |  Kuyruk kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  QueueCount  |  Sıra sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  QueueMessageCount  |  Kuyruk Iletisi sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/queueServices  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  Başarılı Sunucugecikmesi  |  Başarı sunucusu gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  Tablokapasitesi  |  Tablo kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  Tablosayısı  |  Tablo sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  TableEntityCount  |  Tablo varlık sayısı  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. Storage/storageAccounts/tableServices  |  İşlemler  |  İşlemler  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Clienentiops  |  Toplam Istemci ıOPS 'si  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  ClientLatency  |  Ortalama Istemci gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Clientlockıops  |  İstemci kilidi ıOP  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  ClientMetadataReadIOPS  |  İstemci meta verileri okuma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Clientmetadatawriteıops  |  İstemci meta verileri yazma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  ClientReadIOPS  |  İstemci okuma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Clientreadüretilen Iş  |  Ortalama önbellek okuma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  ClientWriteIOPS  |  İstemci yazma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  ClientWriteThroughput  |  Ortalama önbellek yazma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetasyncwriteüretilen Iş  |  StorageTarget zaman uyumsuz yazma üretilen Işi  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetfillüretilen Iş  |  StorageTarget dolgusu Işleme  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargethegizli  |  Depolama hedefi durumu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetıops  |  Toplam StorageTarget ıOPS  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  StorageTargetLatency  |  StorageTarget gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetmetadatareadıops  |  StorageTarget meta verileri okuma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetmetadatawriteıops  |  StorageTarget meta verileri yazma ıOPS  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetreadaheadüretilen Iş  |  StorageTarget Ileriye yönelik okuma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetreadıops  |  Storagetaral okuma ıOPS  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetsyncwriteüretilen Iş  |  StorageTarget zaman uyumlu yazma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargettotalreadüretilen Iş  |  StorageTarget toplam okuma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargettotalwriteverimini  |  StorageTarget Toplam yazma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Storagetargetwriteıops  |  StorageTarget yazma ıOPS  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. StorageCache/önbellekler  |  Hizmet  |  Hizmet  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  ServerSyncSessionResult  |  Eşitleme oturumu sonucu  |  Count  |  Ortalama | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  StorageSyncBatchTransferredFileBytes  |  Eşitlenen baytlar  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  StorageSyncRecalledNetworkBytesByApplication  |  Uygulamaya göre bulut katmanlama geri çağırma boyutu  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  Storagesyncyeniden hesaplama Ledtotalnetworkbytes  |  Bulut katmanlama geri çağırma boyutu  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  Storagesyncyeniden hesaplama Liototalsizebytes  |  Bulut katmanlama geri çağırma  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  StorageSyncRecallThroughputBytesPerSecond  |  Bulut katmanlama geri çekme performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  Storagesyncserversinyal  |  Sunucu çevrimiçi durumu  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  StorageSyncSyncSessionAppliedFilesCount  |  Eşitlenen dosyalar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices  |  StorageSyncSyncSessionPerItemErrorsCount  |  Dosyalar eşitlenmiyor  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices/registeredServers  |  Sunucusinyal  |  Sunucu çevrimiçi durumu  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices/registeredServers  |  Sunucu yeniden hesaplama Liototalsizebytes  |  Bulut katmanlama geri çağırma  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices/syncGroups  |  SyncGroupBatchTransferredFileBytes  |  Eşitlenen baytlar  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionAppliedFilesCount  |  Eşitlenen dosyalar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionPerItemErrorsCount  |  Dosyalar eşitlenmiyor  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointBatchTransferredFileBytes  |  Eşitlenen baytlar  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionAppliedFilesCount  |  Eşitlenen dosyalar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionPerItemErrorsCount  |  Dosyalar eşitlenmiyor  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  AMLCalloutFailedRequests  |  Başarısız Işlev Istekleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  AMLCalloutInputEvents  |  İşlev olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  AMLCalloutRequests  |  İşlev Istekleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Dönüştürme hataları  |  Veri dönüştürme hataları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  DeserializationError  |  Giriş seri kaldırma hataları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Droppedorayarlantedevents  |  Sıra dışı olaylar  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  EarlyInputEvents  |  Erken giriş olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Hatalar  |  Çalışma zamanı hataları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Inputeventbytes  |  Olay baytlarını gir  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Inputevents  |  Giriş olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Inputeventssourcesbackgünlüğe kaydediliyor  |  Biriktirme listesindeki giriş olayları  |  Count  |  Maksimum | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Inputeventssourcespersecond  |  Alınan giriş kaynakları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  LateInputEvents  |  Geç giriş olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  OutputEvents  |  Çıkış olayları  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Outputsulu Markdelayseconds  |  Filigran gecikmesi  |  Saniye  |  Maksimum | 
| **Evet**  | No |  Microsoft. StreamAnalytics/streamingjobs  |  Resourcekullanım  |  SU kullanımı yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Disk okuma bayt sayısı  |  Disk okuma bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Disk yazma baytları  |  Disk yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskReadBytesPerSecond  |  Disk okuma bayt/sn  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskReadLatency  |  Disk okuma gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskReadOperations  |  Disk okuma Işlemleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskWriteBytesPerSecond  |  Disk yazma bayt/sn  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskWriteLatency  |  Disk yazma gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskWriteOperations  |  Disk yazma Işlemleri  |  Count  |  Toplam | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  MemoryActive  |  Bellek etkin  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Memoryverildi  |  Verilen bellek  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  MemoryUsed  |  Kullanılan bellek  |  Bayt  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Networkınbytespersecond  |  Bayt/sn cinsinden ağ  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  NetworkOutBytesPerSecond  |  Ağ çıkış bayt/sn  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | No |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Yüztagecpuready  |  CPU için hazırlanma yüzdesi  |  Mayacak  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  ActiveRequests  |  Etkin Istekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  AverageResponseTime  |  Ortalama yanıt süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  BytesReceived  |  Içindeki veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  BytesSent  |  Giden veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Cpuyüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  DiskQueueLength  |  Disk kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http101  |  Http 101  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http2xx  |  Http 2xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http3xx  |  Http 3xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http401  |  Http 401  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http403  |  Http 403  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http404  |  HTTP 404  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http406  |  Http 406  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http4xx  |  Http 4xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http5xx  |  Http sunucu hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  HttpQueueLength  |  Http kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Largeappserviceplanınstances  |  Büyük App Service çalışanları planlıyor  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Mediumappserviceplanınstances  |  Orta App Service çalışanları planlıyor  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  MemoryPercentage  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  İstekler  |  İstekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Smallappserviceplanınstances  |  Küçük App Service çalışanları planlıyor  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Toplam ön uçlar  |  Toplam ön uçlar  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  Cpuyüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  MemoryPercentage  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  WorkersAvailable  |  Kullanılabilir çalışanlar  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  WorkersTotal  |  Toplam çalışan sayısı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  WorkersUsed  |  Kullanılan çalışanlar  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  BytesReceived  |  Içindeki veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  BytesSent  |  Giden veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  Cpuyüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  DiskQueueLength  |  Disk kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  HttpQueueLength  |  Http kuyruğu uzunluğu  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  MemoryPercentage  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  TcpCloseWait  |  TCP kapatma bekleme  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  TcpClosing  |  TCP kapatma  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  Tcpkurdu  |  TCP oluşturuldu  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  TcpFinWait1  |  TCP Fin bekleme 1  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  TcpFinWait2  |  TCP Fin bekleme 2  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  TcpLastAck  |  TCP son ACK  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  Tcpsynalındı  |  TCP SYN alındı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  TcpSynSent  |  TCP SYN gönderilen  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/sunucugrupları  |  TcpTimeWait  |  TCP zaman bekleme  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  AppConnections  |  Bağlantılar  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  AverageMemoryWorkingSet  |  Ortalama bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  AverageResponseTime  |  Ortalama yanıt süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  BytesReceived  |  Içindeki veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  BytesSent  |  Giden veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  CpuTime  |  CPU süresi  |  Saniye  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  CurrentAssemblies  |  Geçerli derlemeler  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Işlev yürütme sayısı  |  İşlev yürütme sayısı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  FunctionExecutionUnits  |  İşlev yürütme birimleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Gen0Collections  |  Gen 0 çöp koleksiyonları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Gen1Collections  |  Gen 1 çöp koleksiyonları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Gen2Collections  |  Gen 2 çöp koleksiyonları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Handles  |  Tanıtıcı Sayısı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  HealthCheckStatus  |  Durum denetimi durumu  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http101  |  Http 101  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http2xx  |  Http 2xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http3xx  |  Http 3xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http401  |  Http 401  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http403  |  Http 403  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http404  |  HTTP 404  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http406  |  Http 406  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http4xx  |  Http 4xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Http5xx  |  Http sunucu hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  HttpResponseTime  |  Yanıt süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Iootherbytespersecond  |  GÇ diğer bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Iootheroperationspersecond  |  GÇ diğer Işlem/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Ioreadbytespersecond  |  GÇ okuma bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  IoReadOperationsPerSecond  |  GÇ okuma Işlemi/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Iowritebytespersecond  |  GÇ yazma bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  Iowriteoperationspersecond  |  GÇ Yazma Işlemi/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  MemoryWorkingSet  |  Bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  PrivateBytes  |  Özel baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  İstekler  |  İstekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  RequestsInApplicationQueue  |  Uygulama kuyruğundaki istekler  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  İş Parçacıkları  |  İş parçacığı sayısı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  TotalAppDomains  |  Toplam uygulama etki alanları  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/siteler  |  TotalAppDomainsUnloaded  |  Toplam yüklenmeyen uygulama etki alanları  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  AppConnections  |  Bağlantılar  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  AverageMemoryWorkingSet  |  Ortalama bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  AverageResponseTime  |  Ortalama yanıt süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  BytesReceived  |  Içindeki veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  BytesSent  |  Giden veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  CpuTime  |  CPU süresi  |  Saniye  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  CurrentAssemblies  |  Geçerli derlemeler  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Işlev yürütme sayısı  |  İşlev yürütme sayısı  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  FunctionExecutionUnits  |  İşlev yürütme birimleri  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Gen0Collections  |  Gen 0 çöp koleksiyonları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Gen1Collections  |  Gen 1 çöp koleksiyonları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Gen2Collections  |  Gen 2 çöp koleksiyonları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Handles  |  Tanıtıcı Sayısı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  HealthCheckStatus  |  Durum denetimi durumu  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http101  |  Http 101  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http2xx  |  Http 2xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http3xx  |  Http 3xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http401  |  Http 401  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http403  |  Http 403  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http404  |  HTTP 404  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http406  |  Http 406  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http4xx  |  Http 4xx  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Http5xx  |  Http sunucu hataları  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  HttpResponseTime  |  Yanıt süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Iootherbytespersecond  |  GÇ diğer bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Iootheroperationspersecond  |  GÇ diğer Işlem/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Ioreadbytespersecond  |  GÇ okuma bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  IoReadOperationsPerSecond  |  GÇ okuma Işlemi/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Iowritebytespersecond  |  GÇ yazma bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  Iowriteoperationspersecond  |  GÇ Yazma Işlemi/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  MemoryWorkingSet  |  Bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  PrivateBytes  |  Özel baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  İstekler  |  İstekler  |  Count  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  RequestsInApplicationQueue  |  Uygulama kuyruğundaki istekler  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  İş Parçacıkları  |  İş parçacığı sayısı  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  TotalAppDomains  |  Toplam uygulama etki alanları  |  Count  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/Sites/Yuvaları  |  TotalAppDomainsUnloaded  |  Toplam yüklenmeyen uygulama etki alanları  |  Count  |  Ortalama | 
