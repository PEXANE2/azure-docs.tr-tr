---
title: Azure Izleyici platformu ölçümleri, Tanılama ayarları aracılığıyla dışarı aktarılabilir
description: Azure Izleyici ile her kaynak türü için kullanılabilen ölçümlerin listesi.
services: azure-monitor
ms.topic: reference
ms.date: 02/10/2020
ms.subservice: metrics
ms.openlocfilehash: 7a75655d1707dd2491065974ed8addc4c2da1a6a
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77661371"
---
# <a name="azure-monitor-platform-metrics-exportable-via-diagnostic-settings"></a>Azure Izleyici platformu ölçümleri, Tanılama ayarları aracılığıyla dışarı aktarılabilir

Azure Izleyici, varsayılan olarak hiçbir yapılandırma olmadan [Platform ölçümleri](data-platform-metrics.md) sağlar. Bu, portalda grafik oluşturma, REST API aracılığıyla erişme veya PowerShell veya CLı kullanarak sorgulama dahil olmak üzere platform ölçümleriyle etkileşimde bulunmak için çeşitli yollar sağlar. Bkz. [ölçümler-](metrics-supported.md) Azure izleyicisinin birleştirilmiş ölçüm işlem hattı ile Şu anda kullanılabilir olan platform ölçümlerinin tam listesi için desteklenir. Bu ölçümleri sorgulamak ve bunlara erişmek için lütfen [2018-01-01 api sürümünü](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)kullanın. Diğer ölçümler portalda veya eski API 'Ler kullanılarak kullanılabilir.

Azure izleyici ardışık düzeninde bulunan Platform ölçümlerini iki şekilde başka konumlara dışarı aktarabilirsiniz.
1. Log Analytics, Event Hubs veya Azure Storage 'a göndermek için [tanılama ayarlarını](diagnostic-settings.md) kullanma.
2. Ölçümleri kullanın [REST API](https://docs.microsoft.com/rest/api/monitor/metrics/list)

Azure Izleyici arka ucunun anormal olması nedeniyle, tüm ölçümler Tanılama ayarları kullanılarak dışarı aktarılabilir değildir. Aşağıdaki tabloda, Tanılama ayarları kullanılarak verilebilen ve verilemeyen listeler listelenmiştir.

## <a name="change-to-behavior-for-nulls-and-zero-values"></a>Null değerler ve sıfır değerleri için davranışa değiştirme 
 
Tanılama ayarları aracılığıyla verilebilen platform ölçümleri için, Azure Izleyici 'nin ' 0s ' öğesini ' nulls ' olarak yorumlaması için birkaç ölçüm vardır. Bu, gerçek ' 0s ' (kaynağa göre yayılan) ve yorumlanan ' 0s ' (null değerler) arasında karışıklık oluşmasına neden oldu. **1 nisan 2020** ' den itibaren, Tanılama ayarları aracılığıyla dışarı aktarılmış platform ölçümleri, temel alınan kaynak tarafından gerçekten yayılmadığı takdirde artık ' 0s ' öğesini dışarı aktarmayacaktır. Lütfen unutmayın:

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


| Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?  | Null değerleri yayar |  ResourceType  |  Ölçüm  |  MetricDisplayName  |  Birim  |  Toplamatürü | 
|---|---| ---- | ----- | ------ | ---- | ---- | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  CleanerCurrentPrice  |  Bellek: temizleyici geçerli fiyatı  |  Sayı  |  Ortalama | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Cleanermemorynonınkable  |  Bellek: temizleyici bellek daraltılamaz  |  Bayt  |  Ortalama | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Cleanermemoryshrınkable  |  Bellek: temizleyici bellek shrınılabilir  |  Bayt  |  Ortalama | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Commandpoolbusyıthreads  |  İş parçacıkları: komut havuzu meşgul iş parçacıkları  |  Sayı  |  Ortalama | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  CommandPoolIdleThreads  |  İş parçacıkları: komut havuzu boşta iş parçacıkları  |  Sayı  |  Ortalama | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  CommandPoolJobQueueLength  |  Komut havuzu Iş kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  CurrentConnections  |  Bağlantı: geçerli bağlantılar  |  Sayı  |  Ortalama | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  CurrentUserSessions  |  Geçerli Kullanıcı oturumları  |  Sayı  |  Ortalama | 
| Evet * * * *  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Longparsingbusyıthreads  |  İş parçacıkları: uzun ayrıştırma meşgul iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  LongParsingIdleThreads  |  İş parçacıkları: uzun ayrıştırma boşta iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  LongParsingJobQueueLength  |  İş parçacıkları: uzun ayrıştırma işi sıra uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  mashup_engine_memory_metric  |  D motoru belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  mashup_engine_private_bytes_metric  |  D motoru özel baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  mashup_engine_qpu_metric  |  A motoru QPU  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  mashup_engine_virtual_bytes_metric  |  D motoru sanal bayt sayısı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  memory_metric  |  Bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  memory_thrashing_metric  |  Bellek dağılması  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  MemoryLimitHard  |  Bellek: bellek sınırı sabit  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Memoryhighlimit  |  Bellek: bellek sınırı yüksek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  MemoryLimitLow  |  Bellek: bellek sınırı düşük  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  MemoryLimitVertiPaq  |  Bellek: bellek sınırı VertiPaq  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  MemoryUsage  |  Bellek: bellek kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  private_bytes_metric  |  Özel Baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Processingpoolbusyıiojobthreads  |  İş parçacıkları: Işleme havuzu meşgul g/ç işi iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Processingpoolbusınonıothreads  |  İş parçacıkları: Işleme havuzu meşgul olmayan g/ç iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Processingpokaydliiojobthreads  |  İş parçacıkları: Işleme havuzu boşta g/ç işi iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Processingpokaydlenoniothreads  |  İş parçacıkları: Işleme havuzu boşta g/ç olmayan iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  ProcessingPoolIOJobQueueLength  |  İş parçacıkları: Işleme havuzu g/ç iş kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  ProcessingPoolJobQueueLength  |  İşleme havuzu Iş kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  qpu_metric  |  QPU  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Querypoolbusyıthreads  |  Sorgu havuzu meşgul Iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  QueryPoolIdleThreads  |  İş parçacıkları: sorgu havuzu boşta iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  QueryPoolJobQueueLength  |  İş parçacıkları: sorgu havuzu iş kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Kota  |  Bellek: kota  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Quotabkilitli  |  Bellek: kota engellendi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  RowsConvertedPerSec  |  İşleme: dönüştürülen satır sayısı/saniye  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  RowsReadPerSec  |  İşleme: okunan satır sayısı/saniye  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  RowsWrittenPerSec  |  İşleme: yazılan satır sayısı/saniye  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Shortparsingbusyıthreads  |  İş parçacıkları: kısa ayrıştırma meşgul iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  ShortParsingIdleThreads  |  İş parçacıkları: kısa ayrıştırma boşta iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  ShortParsingJobQueueLength  |  İş parçacıkları: kısa ayrıştırma iş kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Başarılı bir bağlantı Spersec  |  Saniyedeki başarılı bağlantı sayısı  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Totalconnectionarızaları  |  Toplam bağlantı başarısızlığı sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  TotalConnectionRequests  |  Toplam bağlantı Isteği sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Vertipaqdisk belleksiz  |  Bellek: VertiPaq disk belleksiz  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  Vertipaqdisk  |  Bellek: VertiPaq disk belleğine  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AnalysisServices/sunucuları  |  virtual_bytes_metric  |  Sanal Bayt Sayısı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  BackendDuration  |  Arka uç Isteklerinin süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ApiManagement/service  |  Kapasite  |  Kapasite  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Süre  |  Ağ Geçidi Isteklerinin genel süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubDroppedEvents  |  Bırakılan EventHub olayları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubRejectedEvents  |  Reddedilen EventHub olayları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Eventhubbaşarılı Futavents  |  Başarılı EventHub olayları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubThrottledEvents  |  Kısıtlanmış EventHub olayları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTimedoutEvents  |  Zaman aşımına uğrayan EventHub olayları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalBytesSent  |  EventHub olaylarının boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalEvents  |  Toplam EventHub olayları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalFailedEvents  |  Başarısız EventHub olayları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  FailedRequests  |  Başarısız ağ geçidi Istekleri (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Diğer Istekler  |  Diğer ağ geçidi Istekleri (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  SuccessfulRequests  |  Başarılı ağ geçidi Istekleri (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Toplam Istek sayısı  |  Toplam ağ geçidi Isteği sayısı (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  UnauthorizedRequests  |  Yetkisiz ağ geçidi Istekleri (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  AppCpuUsagePercentage  |  Uygulama CPU kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  AppMemoryCommitted  |  Atanan uygulama belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  AppMemoryMax  |  En fazla uygulama belleği  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  Appmemorykullanıldı  |  Kullanılan uygulama belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  GCPauseTotalCount  |  GC duraklatma sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  GCPauseTotalTime  |  GC duraklatma toplam süre  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  MaxOldGenMemoryPoolBytes  |  En fazla kullanılabilir eski nesil veri boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  OldGenMemoryPoolBytes  |  Eski nesil veri boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  OldGenPromotedBytes  |  Eski nesil veri boyutuna yükselt  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  SystemCpuUsagePercentage  |  Sistem CPU kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatErrorCount  |  Tomcat genel hatası  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  Tomcatreceived baytları  |  Tomcat alınan toplam bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatRequestMaxTime  |  Tomcat Isteği maksimum zamanı  |  Milisaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatRequestTotalCount  |  Tomcat Isteği toplam sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatRequestTotalTime  |  Tomcat Isteği toplam kez  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatResponseAvgTime  |  Tomcat Isteği ortalama zamanı  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatSentBytes  |  Tomcat gönderilen toplam bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatSessionActiveCurrentCount  |  Tomcat oturum etkin sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatSessionActiveMaxCount  |  Tomcat oturumu en fazla etkin sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatSessionAliveMaxTime  |  Tomcat oturumu maksimum etkin süresi  |  Milisaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatSessionCreatedCount  |  Tomcat oturum oluşturma sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatSessionExpiredCount  |  Tomcat oturumunun süre dolma sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  TomcatSessionRejectedCount  |  Tomcat oturumu reddedildi sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. AppPlatform/yay  |  YoungGenPromotedBytes  |  Küçük ölçekli oluşturma veri boyutuna yükselt  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Automation/automationAccounts  |  TotalJob  |  Toplam Iş sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Automation/automationAccounts  |  Totalupdatedeploymentmachinerçalıştırır  |  Toplam güncelleştirme dağıtım makinesi çalıştırması  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Automation/automationAccounts  |  TotalUpdateDeploymentRuns  |  Toplam güncelleştirme dağıtımı çalıştırmaları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  CoreCount  |  Adanmış çekirdek sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  CreatingNodeCount  |  Düğüm sayısı oluşturuluyor  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  IdleNodeCount  |  Boştaki düğüm sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  JobDeleteCompleteEvent  |  İş silme Tamam olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  JobDeleteStartEvent  |  İş silme başlangıç olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  JobDisableCompleteEvent  |  İş tüm olayları devre dışı bırak  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  JobDisableStartEvent  |  İş devre dışı başlatma olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  JobStartEvent  |  İş başlatma olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  JobTerminateCompleteEvent  |  İş sonlandırma tamamlanma olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  JobTerminateStartEvent  |  İş sonlandırma başlangıç olayları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  LeavingPoolNodeCount  |  Havuz düğüm sayısından çıkılıyor  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  LowPriorityCoreCount  |  LowPriority çekirdek sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  OfflineNodeCount  |  Çevrimdışı düğüm sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  PoolCreateEvent  |  Havuz oluşturma olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  PoolDeleteCompleteEvent  |  Havuz silme Tamam olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  PoolDeleteStartEvent  |  Havuz silme başlangıç olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  PoolResizeCompleteEvent  |  Havuz yeniden boyutlandırma Tamam olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  PoolResizeStartEvent  |  Havuz yeniden boyutlandırma başlangıç olayları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  PreemptedNodeCount  |  Önden düğüm sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  RebootingNodeCount  |  Düğüm sayısı yeniden başlatılıyor  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  ReimagingNodeCount  |  Yeniden Imaging düğüm sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  RunningNodeCount  |  Çalışan düğüm sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  StartingNodeCount  |  Başlangıç düğüm sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  StartTaskFailedNodeCount  |  Başlangıç görevi başarısız düğüm sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  TaskCompleteEvent  |  Görev tamamlanma olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  TaskFailEvent  |  Görev başarısız olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchAccounts  |  TaskStartEvent  |  Görev başlatma olayları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  TotalLowPriorityNodeCount  |  Düşük öncelikli düğüm sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  TotalNodeCount  |  Adanmış düğüm sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  UnusableNodeCount  |  Kullanılamayan düğüm sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchAccounts  |  WaitingForStartTaskNodeCount  |  Başlangıç görevi düğüm sayısı bekleniyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Etkin çekirdekler  |  Etkin çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Etkin düğümler  |  Etkin düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Boştaki çekirdekler  |  Boştaki çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Boştaki düğümler  |  Boştaki düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  İş tamamlandı  |  İş tamamlandı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  İş gönderildi  |  İş gönderildi  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Çekirdekleri bırakma  |  Çekirdekleri bırakma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Düğümlerden çıkılıyor  |  Düğümlerden çıkılıyor  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Önden çekirdekler  |  Önden çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Önden düğümler  |  Önden düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Kota kullanım yüzdesi  |  Kota kullanım yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Toplam çekirdek sayısı  |  Toplam çekirdek sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Toplam düğüm sayısı  |  Toplam düğüm sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Kullanılamayan çekirdekler  |  Kullanılamayan çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Batchaı/çalışma alanları  |  Kullanılamayan düğümler  |  Kullanılamayan düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  Connectionkabul edildi  |  Kabul edilen bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  ConnectionActive  |  Etkin bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  Connectionişlenmiş  |  İşlenmiş bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  CpuUsagePercentageInDouble  |  CPU kullanım yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  Ioreadbytes  |  GÇ okuma baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  IOWriteBytes  |  GÇ Yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  MemoryLimit  |  Bellek sınırı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  MemoryUsage  |  Bellek Kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  MemoryUsagePercentageInDouble  |  Bellek kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  PendingTransactions  |  Bekleyen Işlemler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  Processedbkilitler  |  İşlenen bloklar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  ProcessedTransactions  |  İşlenen Işlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  QueuedTransactions  |  Kuyruğa alınmış Işlemler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  RequestHandled  |  İşlenmiş Istekler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Blockzincirine/blockchainMembers  |  StorageUsage  |  Depolama alanı kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits  |  Önbellek ısabetleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits0  |  Önbellek ısabetleri (parça 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits1  |  Önbellek ısabetleri (parça 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits2  |  Önbellek ısabetleri (parça 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits3  |  Önbellek ısabetleri (parça 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits4  |  Önbellek ısabetleri (parça 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits5  |  Önbellek ısabetleri (parça 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits6  |  Önbellek ısabetleri (parça 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits7  |  Önbellek ısabetleri (parça 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits8  |  Önbellek ısabetleri (parça 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachehits9  |  Önbellek ısabetleri (parça 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheLatency  |  Önbellek gecikmesi mikrosaniye (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheisabetsizlik  |  Önbellek Isabetsizliği  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses0  |  Önbellekte bulunamayanlar (parça 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses1  |  Önbellekte bulunamayanlar (parça 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses2  |  Önbellekte bulunamayanlar (parça 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses3  |  Önbellekte bulunamayanlar (parça 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses4  |  Önbellekte bulunamayanlar (parça 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses5  |  Önbellekte bulunamayanlar (parça 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses6  |  Önbellekte bulunamayanlar (parça 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses7  |  Önbellekte bulunamayanlar (parça 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses8  |  Önbellekte bulunamayanlar (parça 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cachemisses9  |  Önbellekte bulunamayanlar (parça 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead  |  Önbellek Okuma  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead0  |  Önbellek Okuma (parça 0)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead1  |  Önbellek Okuma (parça 1)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead2  |  Önbellek Okuma (parça 2)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead3  |  Önbellek Okuma (parça 3)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead4  |  Önbellek Okuma (parça 4)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead5  |  Önbellek Okuma (parça 5)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead6  |  Önbellek Okuma (parça 6)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead7  |  Önbellek Okuma (parça 7)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead8  |  Önbellek Okuma (parça 8)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheRead9  |  Önbellek Okuma (parça 9)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite  |  Önbellek yazma  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite0  |  Önbellek yazma (parça 0)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite1  |  Önbellek yazma (parça 1)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite2  |  Önbellek yazma (parça 2)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite3  |  Önbellek yazma (parça 3)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite4  |  Önbellek yazma (parça 4)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite5  |  Önbellek yazma (parça 5)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite6  |  Önbellek yazma (parça 6)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite7  |  Önbellek yazma (parça 7)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite8  |  Önbellek yazma (parça 8)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  cacheWrite9  |  Önbellek yazma (parça 9)  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedistemcileri  |  Bağlı Istemciler  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients0  |  Bağlı Istemciler (parça 0)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients1  |  Bağlı Istemciler (parça 1)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients2  |  Bağlı Istemciler (parça 2)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients3  |  Bağlı Istemciler (parça 3)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients4  |  Bağlı Istemciler (parça 4)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients5  |  Bağlı Istemciler (parça 5)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients6  |  Bağlı Istemciler (parça 6)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients7  |  Bağlı Istemciler (parça 7)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients8  |  Bağlı Istemciler (parça 8)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  connectedclients9  |  Bağlı Istemciler (parça 9)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  hatalar  |  Hatalar  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  çıkarılan anahtarlar  |  Çıkarılan anahtarlar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys0  |  Çıkarılan anahtarlar (parça 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys1  |  Çıkarılan anahtarlar (parça 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys2  |  Çıkarılan anahtarlar (parça 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys3  |  Çıkarılan anahtarlar (parça 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys4  |  Çıkarılan anahtarlar (parça 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys5  |  Çıkarılan anahtarlar (parça 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys6  |  Çıkarılan anahtarlar (parça 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys7  |  Çıkarılan anahtarlar (parça 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys8  |  Çıkarılan anahtarlar (parça 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  evictedkeys9  |  Çıkarılan anahtarlar (parça 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys  |  Süre dolma anahtarları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys0  |  Süre dolma anahtarları (parça 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys1  |  Süre dolma anahtarları (parça 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys2  |  Süre dolma anahtarları (parça 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys3  |  Süre dolma anahtarları (parça 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys4  |  Süre dolma anahtarları (parça 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys5  |  Süre dolma anahtarları (parça 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys6  |  Süre dolma anahtarları (parça 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys7  |  Süre dolma anahtarları (parça 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys8  |  Süre dolma anahtarları (parça 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  expiredkeys9  |  Süre dolma anahtarları (parça 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  GetCommands  |  İyorsa  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands0  |  Alır (parça 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands1  |  Alır (parça 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands2  |  Alır (parça 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands3  |  Alır (parça 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands4  |  Alır (parça 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands5  |  Alır (parça 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands6  |  Alır (parça 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands7  |  Alır (parça 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands8  |  Al (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  getcommands9  |  Alır (parça 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond  |  İşlem/saniye  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond0  |  Saniyedeki işlem (parça 0)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond1  |  Saniyedeki işlem (parça 1)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond2  |  Saniyedeki işlem (parça 2)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond3  |  Saniyedeki işlem (parça 3)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond4  |  Saniyedeki işlem (parça 4)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond5  |  Saniyedeki işlem (parça 5)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond6  |  Saniyedeki işlem (parça 6)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond7  |  Saniyedeki işlem (parça 7)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond8  |  Saniyedeki işlem (parça 8)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  operationsPerSecond9  |  Saniyedeki işlem (parça 9)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime  |  CPU  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime0  |  CPU (parça 0)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime1  |  CPU (parça 1)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime2  |  CPU (parça 2)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime3  |  CPU (parça 3)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime4  |  CPU (parça 4)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime5  |  CPU (parça 5)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime6  |  CPU (parça 6)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime7  |  CPU (parça 7)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime8  |  CPU (parça 8)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  percentProcessorTime9  |  CPU (parça 9)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  Sunucuyükü  |  Sunucu yükü  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad0  |  Sunucu yükü (parça 0)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad1  |  Sunucu yükü (parça 1)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad2  |  Sunucu yükü (parça 2)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad3  |  Sunucu yükü (parça 3)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad4  |  Sunucu yükü (parça 4)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad5  |  Sunucu yükü (parça 5)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad6  |  Sunucu yükü (parça 6)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad7  |  Sunucu yükü (parça 7)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad8  |  Sunucu yükü (parça 8)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  serverLoad9  |  Sunucu yükü (parça 9)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  SetCommands  |  Yapar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands0  |  Kümeler (parça 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands1  |  Kümeler (parça 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands2  |  Kümeler (parça 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands3  |  Kümeler (parça 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands4  |  Kümeler (parça 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands5  |  Kümeler (parça 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands6  |  Kümeler (parça 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands7  |  Kümeler (parça 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands8  |  Kümeler (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  setcommands9  |  Kümeler (parça 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsişlendi  |  İşlemler Toplamı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed0  |  Toplam Işlem (parça 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed1  |  Toplam Işlem (parça 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed2  |  Toplam Işlem (parça 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed3  |  Toplam Işlem (parça 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed4  |  Toplam Işlem (parça 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed5  |  Toplam Işlem (parça 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed6  |  Toplam Işlem (parça 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed7  |  Toplam Işlem (parça 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed8  |  Toplam Işlem (parça 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalcommandsprocessed9  |  Toplam Işlem (parça 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys  |  Toplam anahtar sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys0  |  Toplam anahtar (parça 0)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys1  |  Toplam anahtar (parça 1)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys2  |  Toplam anahtar (parça 2)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys3  |  Toplam anahtar (parça 3)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys4  |  Toplam anahtar (parça 4)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys5  |  Toplam anahtar (parça 5)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys6  |  Toplam anahtar (parça 6)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys7  |  Toplam anahtar (parça 7)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys8  |  Toplam anahtar (Shard 8)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  totalkeys9  |  Toplam anahtar (parça 9)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory  |  Kullanılan bellek  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory0  |  Kullanılan bellek (parça 0)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory1  |  Kullanılan bellek (parça 1)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory2  |  Kullanılan bellek (parça 2)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory3  |  Kullanılan bellek (parça 3)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory4  |  Kullanılan bellek (parça 4)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory5  |  Kullanılan bellek (parça 5)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory6  |  Kullanılan bellek (parça 6)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory7  |  Kullanılan bellek (parça 7)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory8  |  Kullanılan bellek (parça 8)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemory9  |  Kullanılan bellek (parça 9)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemorypercentage  |  Kullanılan bellek yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss  |  Kullanılan bellek RSS  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss0  |  Kullanılan bellek RSS (parça 0)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss1  |  Kullanılan bellek RSS (parça 1)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss2  |  Kullanılan bellek RSS (parça 2)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss3  |  Kullanılan bellek RSS (parça 3)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss4  |  Kullanılan bellek RSS (parça 4)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss5  |  Kullanılan bellek RSS (parça 5)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss6  |  Kullanılan bellek RSS (parça 6)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss7  |  Kullanılan bellek RSS (parça 7)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss8  |  Kullanılan bellek RSS (parça 8)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Cache/redsıs  |  usedmemoryRss9  |  Kullanılan bellek RSS (parça 9)  |  Bayt  |  Maksimum | 
| Hayır  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Disk okuma bayt/sn  |  Disk okuma  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Disk yazma bayt/sn  |  Disk yazma  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/domainNames/yuvalar/roller  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  Disk okuma bayt/sn  |  Disk okuma  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  Disk yazma bayt/sn  |  Disk yazma  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicCompute/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts  |  UsedCapacity  |  Kullanılan kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  BlobCapacity  |  Blob Kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  BLOB sayısı  |  Blob Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  ContainerCount  |  Blob Kapsayıcı Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Dizin kapasitesi  |  Dizin kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/blobServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Dosya kapasitesi  |  Dosya kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  FileCount  |  Dosya sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Dosya ShareCount  |  Dosya paylaşma sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Dosya Sharequota  |  Dosya paylaşımının kota boyutu  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Dosya paylaşımının anlık görüntü sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Dosya paylaşımının anlık görüntü boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/fileServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueCapacity  |  Kuyruk Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueCount  |  Kuyruk Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  QueueMessageCount  |  Kuyruk İleti Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/queueServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  TableCapacity  |  Tablo Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  TableCount  |  Tablo Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  TableEntityCount  |  Tablo Varlık Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ClassicStorage/storageAccounts/tableServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Blockedçağrılarında  |  Engellenen çağrılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Karakterçeli  |  Eğitilen karakterler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Karakter Yabanslamuş  |  Çevrilen karakterler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  ClientErrors  |  İstemci hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Dataın  |  Veri Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Veri çıkışı  |  Giden Veriler  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Gecikme süresi  |  Gecikme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  ServerErrors  |  Sunucu hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  SpeechSessionDuration  |  Konuşma oturumu süresi  |  Saniyeden  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Başarılı çağrılar  |  Başarılı çağrılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Toplam çağrılar  |  Toplam çağrı sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  Toplam hata sayısı  |  Hatalar Toplamı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  TotalTokenCalls  |  Toplam belirteç çağrısı sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/accounts  |  TotalTransactions  |  Toplam Işlem sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Tüketilen CPU kredileri  |  Tüketilen CPU kredileri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Kalan CPU kredileri  |  Kalan CPU kredileri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri diski sıra derinliği  |  Veri diski sıra derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri diski okuma bayt/sn  |  Veri diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri diski okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri diski yazma bayt/sn  |  Veri diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri diski yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk okuma bayt sayısı  |  Disk okuma bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk yazma baytları  |  Disk yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Gelen akışlar  |  Gelen akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Gelen akışlar en yüksek oluşturma oranı  |  Gelen akışlar en yüksek oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Ağ Girişi  |  Faturalanabilir (kullanım dışı) ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Toplam ağ  |  Toplam ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Ağ Çıkışı  |  Ağdan çıkış faturalandırılabilir (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Toplam ağ çıkışı  |  Toplam ağ çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski sıra derinliği  |  İşletim sistemi diski sıra derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk başına işletim sistemi QD  |  İşletim sistemi diski QD (kullanım dışı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi disk başına okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski başına okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi disk başına yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim sistemi diski başına yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Giden akışlar  |  Giden akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Giden akış maksimum oluşturma oranı  |  Giden akışlar maksimum oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk başına QD  |  Veri diski QD (kullanım dışı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk başına okunan bayt/sn  |  Veri diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk başına okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk başına yazma bayt/sn  |  Veri diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk başına yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Premium veri diski önbelleği okuma Isabeti  |  Premium veri diski önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Premium veri diski önbelleği okuma Isabetsizliği  |  Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Premium işletim sistemi disk önbelleği okuma Isabeti  |  Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Tüketilen CPU kredileri  |  Tüketilen CPU kredileri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Kalan CPU kredileri  |  Kalan CPU kredileri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski sıra derinliği  |  Veri diski sıra derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski okuma bayt/sn  |  Veri diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski yazma bayt/sn  |  Veri diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri diski yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk okuma bayt sayısı  |  Disk okuma bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk yazma baytları  |  Disk yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Gelen akışlar  |  Gelen akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Gelen akışlar en yüksek oluşturma oranı  |  Gelen akışlar en yüksek oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Ağ Girişi  |  Faturalanabilir (kullanım dışı) ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Toplam ağ  |  Toplam ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Ağ Çıkışı  |  Ağdan çıkış faturalandırılabilir (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Toplam ağ çıkışı  |  Toplam ağ çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski sıra derinliği  |  İşletim sistemi diski sıra derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına işletim sistemi QD  |  İşletim sistemi diski QD (kullanım dışı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi disk başına okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski başına okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi disk başına yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim sistemi diski başına yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Giden akışlar  |  Giden akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Giden akış maksimum oluşturma oranı  |  Giden akışlar maksimum oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına QD  |  Veri diski QD (kullanım dışı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına okunan bayt/sn  |  Veri diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına yazma bayt/sn  |  Veri diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk başına yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Premium veri diski önbelleği okuma Isabeti  |  Premium veri diski önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Premium veri diski önbelleği okuma Isabetsizliği  |  Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Premium işletim sistemi disk önbelleği okuma Isabeti  |  Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Tüketilen CPU kredileri  |  Tüketilen CPU kredileri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Kalan CPU kredileri  |  Kalan CPU kredileri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri diski sıra derinliği  |  Veri diski sıra derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri diski okuma bayt/sn  |  Veri diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri diski okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri diski yazma bayt/sn  |  Veri diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri diski yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk okuma bayt sayısı  |  Disk okuma bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk yazma baytları  |  Disk yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Gelen akışlar  |  Gelen akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Gelen akışlar en yüksek oluşturma oranı  |  Gelen akışlar en yüksek oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Ağ Girişi  |  Faturalanabilir (kullanım dışı) ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Toplam ağ  |  Toplam ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Ağ Çıkışı  |  Ağdan çıkış faturalandırılabilir (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Toplam ağ çıkışı  |  Toplam ağ çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski sıra derinliği  |  İşletim sistemi diski sıra derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk başına işletim sistemi QD  |  İşletim sistemi diski QD (kullanım dışı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi disk başına okuma bayt/sn  |  İşletim sistemi diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski başına okuma Işlemi/sn  |  İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi disk başına yazma bayt/sn  |  İşletim sistemi diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim sistemi diski başına yazma Işlemi/sn  |  İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Giden akışlar  |  Giden akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Giden akış maksimum oluşturma oranı  |  Giden akışlar maksimum oluşturma oranı (Önizleme)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk başına QD  |  Veri diski QD (kullanım dışı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk başına okunan bayt/sn  |  Veri diski okuma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk başına okuma Işlemi/sn  |  Veri diski okuma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk başına yazma bayt/sn  |  Veri diski yazma bayt/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk başına yazma Işlemi/sn  |  Veri diski yazma Işlemi/sn (kullanım dışı)  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Premium veri diski önbelleği okuma Isabeti  |  Premium veri diski önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Premium veri diski önbelleği okuma Isabetsizliği  |  Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Premium işletim sistemi disk önbelleği okuma Isabeti  |  Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik  |  Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Containerınstance/containerGroups  |  CpuUsage  |  CPU kullanımı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Containerınstance/containerGroups  |  MemoryUsage  |  Bellek Kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Containerınstance/containerGroups  |  NetworkBytesReceivedPerSecond  |  Saniye başına alınan ağ bayt sayısı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Containerınstance/containerGroups  |  NetworkBytesTransmittedPerSecond  |  Saniye başına aktarılan ağ baytları  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ContainerRegistry/kayıt defterleri  |  RunDuration  |  Çalışma süresi  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. ContainerRegistry/kayıt defterleri  |  Başarılı sayı  |  Başarılı çekme sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ContainerRegistry/kayıt defterleri  |  Başarılı Pushcount  |  Başarılı gönderme sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ContainerRegistry/kayıt defterleri  |  TotalPullCount  |  Toplam çekme sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. ContainerRegistry/kayıt defterleri  |  Toplam Pushcount  |  Toplam gönderme sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_node_status_allocatable_cpu_cores  |  Yönetilen kümede toplam kullanılabilir CPU çekirdeği sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_node_status_allocatable_memory_bytes  |  Yönetilen kümede toplam kullanılabilir bellek miktarı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_node_status_condition  |  Çeşitli düğüm koşullarına yönelik durumlar  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_pod_status_phase  |  Aşamasına göre Pod sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. ContainerService/Managedkümeler  |  kube_pod_status_ready  |  Hazırlık durumundaki Pod sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  AvailableCapacity  |  Kullanılabilir kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloud  |  Karşıya yüklenen bulut baytları (cihaz)  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloudPerShare  |  Karşıya yüklenen bulut baytları (paylaşma)  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Cloudreadüretilen Iş  |  Bulut Indirme performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughputPerShare  |  Bulut Indirme verimlilik (paylaşma)  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Clouduploadüretilen Iş  |  Bulut karşıya yükleme performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughputPerShare  |  Bulut karşıya yükleme üretilen Işi (paylaşma)  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Hipervmemoryutilileştirme  |  Edge Işlem-bellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Hypervvirtualprocessorkullanım  |  Edge hesaplama-CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Nicreadüretilen Iş  |  Okuma performansı (ağ)  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  Nicwriteüretilen Iş  |  Yazma Işleme (ağ)  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataBoxEdge/dataBoxEdgeDevices  |  TotalCapacity  |  Toplam Kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataFactory/DataFactory  |  Failedçalıştırmaları  |  Başarısız çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/DataFactory  |  Başarılı çalıştırmalar  |  Başarılı çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Activitycancelledçalıştırmaları  |  İptal edilen etkinlik ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Activityfailedçalıştırmaları  |  Başarısız etkinlik çalıştırma ölçümleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  ActivitySucceededRuns  |  Başarılı etkinlik ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Factorysizeingbits  |  Toplam fabrika boyutu (GB birimi)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Integrationruntimekullanılabilirliği Blememory  |  Tümleştirme çalışma zamanı kullanılabilir belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Integrationruntimeaveragetaskpickupdelay  |  Tümleştirme çalışma zamanı sıra süresi  |  Saniyeden  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Integrationruntimecpuyüzdesi  |  Tümleştirme çalışma zamanı CPU kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Integrationruntimequeuelength  |  Tümleştirme çalışma zamanı sıra uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Maxallowedfactorysizeingbits  |  İzin verilen en yüksek fabrika boyutu (GB birimi)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  MaxAllowedResourceCount  |  İzin verilen en fazla varlık sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Ardışık düzen ınecancelledçalıştırmaları  |  İptal edilen işlem hattı çalışma ölçümleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Pipelinefailedçalıştırmaları  |  Başarısız işlem hattı çalıştırmaları ölçümleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Ardışık düzen Inesucceededçalıştırmaları  |  Başarılı işlem hattı çalışma ölçümleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  ResourceCount  |  Toplam varlık sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Triggercancelledçalıştırmaları  |  İptal edilen tetikleyici çalışan ölçümleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  Triggerfailedçalıştırmaları  |  Başarısız tetikleyici çalıştırma ölçümleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DataFactory/Factory  |  TriggerSucceededRuns  |  Başarılı tetikleyici çalışan ölçümleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/accounts  |  Jobauendediptal edildi  |  İptal edilen AU Saati  |  Saniyeden  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedFailure  |  Başarısız AU Saati  |  Saniyeden  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/accounts  |  JobAUEndedSuccess  |  Başarılı AU Saati  |  Saniyeden  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/accounts  |  Jobendediptal edildi  |  İptal edilen Işler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedFailure  |  Başarısız Işler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/accounts  |  JobEndedSuccess  |  Başarılı Işler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/accounts  |  DataRead  |  Okunan veriler  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/accounts  |  Veri yazıldı  |  Yazılan veriler  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/accounts  |  ReadRequests  |  Okuma Istekleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/accounts  |  TotalStorage  |  Toplam depolama alanı  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/accounts  |  WriteRequests  |  Yazma Istekleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  active_connections  |  Etkin bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  backup_storage_used  |  Kullanılan yedekleme depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  connections_failed  |  Başarısız Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  io_consumption_percent  |  GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  seconds_behind_master  |  Saniye cinsinden çoğaltma gecikmesi  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  serverlog_storage_limit  |  Sunucu günlüğü depolama sınırı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  serverlog_storage_percent  |  Sunucu günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  serverlog_storage_usage  |  Kullanılan sunucu günlüğü depolaması  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  storage_percent  |  Depolama alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformarıdb/sunucular  |  storage_used  |  Kullanılan depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  active_connections  |  Etkin bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  backup_storage_used  |  Kullanılan yedekleme depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  connections_failed  |  Başarısız Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  io_consumption_percent  |  GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  seconds_behind_master  |  Saniye cinsinden çoğaltma gecikmesi  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  serverlog_storage_limit  |  Sunucu günlüğü depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  serverlog_storage_percent  |  Sunucu günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  serverlog_storage_usage  |  Kullanılan sunucu günlüğü depolaması  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  storage_percent  |  Depolama alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Dbformyısql/sunucuları  |  storage_used  |  Kullanılan depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  active_connections  |  Etkin bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  backup_storage_used  |  Kullanılan yedekleme depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  connections_failed  |  Başarısız Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  io_consumption_percent  |  GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  pg_replica_log_delay_in_bytes  |  Çoğaltmalar genelinde en fazla gecikme  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  pg_replica_log_delay_in_seconds  |  Çoğaltma gecikmesi  |  Saniyeden  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  serverlog_storage_limit  |  Sunucu günlüğü depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  serverlog_storage_percent  |  Sunucu günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  serverlog_storage_usage  |  Kullanılan sunucu günlüğü depolaması  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  storage_percent  |  Depolama alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/sunucuları  |  storage_used  |  Kullanılan depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/serversv2  |  active_connections  |  Etkin bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/serversv2  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/serversv2  |  'ye  |  IOPS  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/serversv2  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/serversv2  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/serversv2  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/serversv2  |  storage_percent  |  Depolama alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. DBforPostgreSQL/serversv2  |  storage_used  |  Kullanılan depolama alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Devices/Account  |  digitaltwins. telemetri. düğümleri  |  Dijital TWINS düğümü telemetri yer tutucusu  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  c2d.commands.egress.abandon.success  |  C2D iletileri bırakıldı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  c2d.commands.egress.complete.success  |  C2D ileti teslimleri tamamlandı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  c2d.commands.egress.reject.success  |  C2D iletileri reddedildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  C2D. Methods. Failure  |  Başarısız doğrudan yöntem etkinleştirmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  c2d.methods.requestSize  |  Doğrudan yöntem etkinleştirmeleri istek boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  c2d.methods.responseSize  |  Doğrudan yöntem etkinleştirmeleri yanıt boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  c2d.methods.success  |  Başarılı doğrudan yöntem etkinleştirmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  C2D. ikizi. Read. Failure  |  Arka uçtan başarısız ikizi okumaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  C2D. ikizi. Read. size  |  Arka uçtan gelen ikizi okumaların yanıt boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  c2d.twin.read.success  |  Arka uçtan başarılı ikizi okumaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  C2D. ikizi. Update. Failure  |  Arka uçtan başarısız ikizi güncelleştirmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  c2d.twin.update.size  |  Arka uçtan ikizi güncelleştirmelerinin boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  C2D. ikizi. Update. Success  |  Arka uçtan başarılı ikizi güncelleştirmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  C2DMessagesExpired  |  C2D Iletilerinin süre dolma (Önizleme)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  yapılandırmalar  |  Yapılandırma ölçümleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Devices/ıothubs  |  connectedDeviceCount  |  Bağlı cihazlar (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. endpoints. çıkış. builtIn. Events  |  Yönlendirme: iletilere/olaylara teslim edilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. endpoints. çıkış. eventHubs  |  Yönlendirme: Olay Hub 'ına teslim edilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.endpoints.egress.serviceBusQueues  |  Yönlendirme: Service Bus kuyruğuna teslim edilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.endpoints.egress.serviceBusTopics  |  Yönlendirme: Service Bus konuya teslim edilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. endpoints. çıkış. Storage  |  Yönlendirme: depolamaya teslim edilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. endpoints. çıkış. Storage. blob 'ları  |  Yönlendirme: depolamaya teslim edilen Bloblar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. endpoints. çıkış. Storage. Bytes  |  Yönlendirme: depolamaya teslim edilen veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. endpoints. Latency. yerleik. Events  |  Yönlendirme: iletiler/olaylar için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. endpoints. Latency. eventHubs  |  Yönlendirme: Olay Hub 'ı için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.endpoints.latency.serviceBusQueues  |  Yönlendirme: Service Bus kuyruğu için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.endpoints.latency.serviceBusTopics  |  Yönlendirme: Service Bus konusu için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. endpoints. Latency. Storage  |  Yönlendirme: depolama için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. telemetri. çıkış. bırakıldı  |  Yönlendirme: telemetri iletileri bırakıldı   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.telemetry.egress.fallback  |  Yönlendirme: geri dönüşe teslim edilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. telemetri. çıkış. geçersiz  |  Yönlendirme: telemetri iletileri uyumsuz  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.telemetry.egress.orphaned  |  Yönlendirme: telemetri iletileri yalnız bırakılmış   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. telemetri. çıkış. başarılı  |  Yönlendirme: teslim edilen telemetri iletileri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.telemetry.ingress.allProtocol  |  Telemetri iletisi gönderme denemeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.telemetry.ingress.sendThrottle  |  Daraltma hatası sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. telemetri. giriş. başarılı  |  Gönderilen telemetri iletileri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.twin.read.failure  |  Cihazlardan başarısız ikizi okumaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. ikizi. Read. size  |  Cihazlardan gelen ikizi okumaların yanıt boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.twin.read.success  |  Cihazlardan başarılı ikizi okumaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.twin.update.failure  |  Cihazlardan ikizi güncelleştirmeleri başarısız oldu  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  d2c.twin.update.size  |  Cihazlardan ikizi güncelleştirmelerinin boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  D2C. ikizi. Update. Success  |  Cihazlardan başarılı ikizi güncelleştirmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Günlükmessagequotakullanıldı  |  Kullanılan toplam ileti sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  deviceDataUsage  |  Toplam cihaz verisi kullanımı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  deviceDataUsageV2  |  Toplam cihaz verisi kullanımı (Önizleme)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  devices.connectedDevices.allProtocol  |  Bağlı cihazlar (kullanım dışı)   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Devices. totalDevices  |  Toplam cihaz (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Eventgridteslimler  |  Event Grid teslimler (Önizleme)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  EventGridLatency  |  Event Grid gecikme süresi (Önizleme)  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Jobs. cancelJob. Failure  |  Başarısız iş iptalleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Jobs. cancelJob. Success  |  Başarılı iş iptalleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  işler. tamamlandı  |  Tamamlanan İşler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Jobs. createDirectMethodJob. Failure  |  Yöntem çağırma işlerinin oluşturma işlemi başarısız oldu  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Jobs. createDirectMethodJob. Success  |  Yöntem çağırma işlerinin başarılı oluşturmaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  jobs.createTwinUpdateJob.failure  |  İkizi Update işlerinin başarısız oluşturmaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  jobs.createTwinUpdateJob.success  |  İkizi Update işlerinin başarılı oluşturmaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  işler. başarısız  |  Başarısız işler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Jobs. listJobs. hata  |  İşleri listelemek için başarısız çağrılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  işler. listJobs. başarılı  |  İşleri listelemek için başarılı çağrılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  Jobs. queryJobs. hata  |  Başarısız iş sorguları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  işler. queryJobs. başarılı  |  Başarılı iş sorguları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Devices/ıothubs  |  totalDeviceCount  |  Toplam cihaz (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  twinQueries. Failure  |  Başarısız ikizi sorguları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  twinQueries.resultSize  |  İkizi sorguları sonuç boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/ıothubs  |  twinQueries. Success  |  Başarılı ikizi sorguları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/provisioningServices  |  AttestationAttempts  |  Kanıtlama denemeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/provisioningServices  |  Deviceasyleri  |  Atanan cihazlar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Devices/provisioningServices  |  Registrationdenemeleri  |  Kayıt denemeleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  AvailableStorage  |  Kullanılabilir Depolama Alanı'na  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  Cassandraconnectionkapanışları  |  Cassandra bağlantı kapanışları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  Cassandrarequestücretleri  |  Cassandra Istek ücretleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  CassandraRequests  |  Cassandra Istekleri  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  Veri kullanımı  |  Veri Kullanımı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  DeleteVirtualNetwork  |  DeleteVirtualNetwork  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  DocumentCount  |  Belge sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  DocumentQuota  |  Belge kotası  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  Indexusage  |  Dizin kullanımı  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  Meta veri datarequests  |  Meta veri Istekleri  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft. DocumentDB/databaseAccounts  |  MongoRequestCharge  |  Mongo Istek ücreti  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. DocumentDB/databaseAccounts  |  MongoRequests  |  Mongo Istekleri  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  MongoRequestsCount  |  Mongo Istek hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  MongoRequestsDelete  |  Mongo silme Isteği hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  Mongorequestsınsert  |  Mongo ekleme Isteği oranı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  MongoRequestsQuery  |  Mongo sorgu Isteği hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  MongoRequestsUpdate  |  Mongo güncelleştirme Isteği hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  Provisionedüretilen Iş  |  Sağlanan İşleme Hızı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  ReplicationLatency  |  P99 çoğaltma gecikmesi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. DocumentDB/databaseAccounts  |  ServiceAvailability  |  Hizmet kullanılabilirliği  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. DocumentDB/databaseAccounts  |  Toplam Istek sayısı  |  Toplam İstek Sayısı  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft. DocumentDB/databaseAccounts  |  TotalRequestUnits  |  Toplam Istek birimleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EnterpriseKnowledgeGraph/hizmetler  |  FailureCount  |  Hata sayısı  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft. EnterpriseKnowledgeGraph/hizmetler  |  Başarılı sayısı  |  Başarı sayısı  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft. EnterpriseKnowledgeGraph/hizmetler  |  Başarılı gecikme  |  Başarı gecikmesi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EnterpriseKnowledgeGraph/hizmetler  |  Işlem sayısı  |  İşlem sayısı  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  DeadLetteredCount  |  Kullanılmayan olaylar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventGrid/Domains  |  Ityattemptfailcount  |  Teslim başarısız olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  Deliverybaşarılı sayısı  |  Teslim edilen olaylar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventGrid/Domains  |  DestinationProcessingDurationInMs  |  Hedef Işleme süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  DroppedEventCount  |  Bırakılan olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  MatchedEventCount  |  Eşleşen olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  PublishFailCount  |  Başarısız olayları Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  Publishbaşarılı sayısı  |  Yayımlanan olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Domains  |  Publishbaşarıyla Latencınms  |  Yayımlama başarı gecikmesi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Eventabonelikleri  |  DeadLetteredCount  |  Kullanılmayan olaylar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventGrid/Eventabonelikleri  |  Ityattemptfailcount  |  Teslim başarısız olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Eventabonelikleri  |  Deliverybaşarılı sayısı  |  Teslim edilen olaylar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventGrid/Eventabonelikleri  |  DestinationProcessingDurationInMs  |  Hedef Işleme süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Eventabonelikleri  |  DroppedEventCount  |  Bırakılan olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Eventabonelikleri  |  MatchedEventCount  |  Eşleşen olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Extensionkonuları  |  PublishFailCount  |  Başarısız olayları Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Extensionkonuları  |  Publishbaşarılı sayısı  |  Yayımlanan olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Extensionkonuları  |  Publishbaşarıyla Latencınms  |  Yayımlama başarı gecikmesi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/Extensionkonuları  |  UnmatchedEventCount  |  Eşleşmeyen olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/konuları  |  PublishFailCount  |  Başarısız olayları Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/konuları  |  Publishbaşarılı sayısı  |  Yayımlanan olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/konuları  |  Publishbaşarıyla Latencınms  |  Yayımlama başarı gecikmesi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventGrid/konuları  |  UnmatchedEventCount  |  Eşleşmeyen olaylar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  ActiveConnection sayısı  |  ActiveConnection sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  Kullanılabilirlik Blememory  |  Kullanılabilir Bellek  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  CaptureBacklog  |  Biriktirme listesini yakala.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  CapturedBytes  |  Yakalanan baytlar.  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  CapturedMessages  |  Yakalanan Iletiler.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  ConnectionsClosed  |  Bağlantı kapatıldı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  ConnectionsOpened  |  Bağlantılar açıldı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  CPU  |  CPU  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  Incomingbytes  |  Gelen bayt sayısı.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  Incomingmessages  |  Gelen İletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  Incomingrequests  |  Gelen Istekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  OutgoingBytes  |  Giden bayt sayısı.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. EventHub/kümeler  |  OutgoingMessages  |  Giden İletiler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  QuotaExceededErrors  |  Kota, hataları aştı.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  ServerErrors  |  Sunucu hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  SuccessfulRequests  |  Başarılı Istekler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  OperationName  |  Kısıtlanmış Istekler.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. EventHub/kümeler  |  UserErrors  |  Kullanıcı hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  ActiveConnection sayısı  |  ActiveConnection sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  CaptureBacklog  |  Biriktirme listesini yakala.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  CapturedBytes  |  Yakalanan baytlar.  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  CapturedMessages  |  Yakalanan Iletiler.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  ConnectionsClosed  |  Bağlantı kapatıldı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  ConnectionsOpened  |  Bağlantılar açıldı.  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHABL  |  Biriktirme listesi iletilerini Arşivle (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHAMB  |  Arşiv iletisi verimlilik (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHAMSGS  |  İletileri Arşivle (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHINBYTES  |  Gelen baytlar (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHINMB  |  Gelen bayt (geçersiz) (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHINILETISI  |  Gelen Iletiler (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHOUTBYTES  |  Giden baytlar (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHOUTMB  |  Giden baytlar (eski) (kullanım dışı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHODıKILETISI  |  Giden Iletiler (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  FAILREQ  |  Başarısız Istekler (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Incomingbytes  |  Gelen bayt sayısı.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Incomingmessages  |  Gelen İletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Incomingrequests  |  Gelen Istekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Giriş iletileri  |  Gelen Iletiler (eski) (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  INREQS  |  Gelen Istekler (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  INTERR  |  İç sunucu hataları (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Hatalı cerr  |  Diğer hatalar (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  OutgoingBytes  |  Giden bayt sayısı.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  OutgoingMessages  |  Giden İletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Dış mesaj  |  Giden Iletiler (eski) (kullanım dışı)  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  QuotaExceededErrors  |  Kota, hataları aştı.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  ServerErrors  |  Sunucu hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  Boyut  |  Boyut  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  SuccessfulRequests  |  Başarılı Istekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  SUCCREQ  |  Başarılı Istekler (kullanım dışı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  SVRBSY  |  Sunucu meşgul hataları (kullanım dışı)  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  OperationName  |  Kısıtlanmış Istekler.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  UserErrors  |  Kullanıcı hataları.  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. HDInsight/kümeler  |  Kategorizedgatewayrequests  |  Kategorilere ayrılmış ağ geçidi Istekleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. HDInsight/kümeler  |  GatewayRequests  |  Ağ Geçidi Istekleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. HDInsight/kümeler  |  Numactiveçalışanları  |  Etkin çalışan sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. HDInsight/kümeler  |  ScalingRequests  |  Ölçeklendirme istekleri  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Insights/oto Scalesettings  |  MetricThreshold  |  Ölçüm eşiği  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/oto Scalesettings  |  ObservedCapacity  |  Gözlenen kapasite  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/oto Scalesettings  |  ObservedMetricValue  |  Gözlemlenen ölçüm değeri  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/oto Scalesettings  |  Scaleactionsınıated  |  Ölçek eylemleri başlatıldı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  Kullanılabilirlik sonuçları/kullanılabilirliği yüzdesi  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  Kullanılabilirlik sonuçları/sayısı  |  Kullanılabilirlik testleri  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  Kullanılabilirlik sonuçları/süresi  |  Kullanılabilirlik testi süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/networkDuration  |  Sayfa yükleme ağ bağlantı süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/processingDuration  |  İstemci işlem süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/receiveDuration  |  Yanıt süresini alma  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/sendDuration  |  İstek gönderme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  Browserzamanlamalar/totalDuration  |  Tarayıcı sayfa yükleme süresi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  Bağımlılıklar/sayı  |  Bağımlılık çağrıları  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  Bağımlılıklar/süre  |  Bağımlılık süresi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  Bağımlılıklar/başarısız  |  Bağımlılık çağrısı sorunları  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  özel durumlar/tarayıcı  |  Tarayıcı özel durumları  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  özel durumlar/say  |  Özel durumlar  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  özel durumlar/sunucu  |  Sunucu özel durumları  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  pageViews/Count  |  Sayfa görünümleri  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  pageViews/Duration  |  Sayfa görünümü yükleme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  performanceCounters/exceptionsPerSecond  |  Özel durum oranı  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  performanceCounters/memoryAvailableBytes  |  Available memory  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  performanceCounters/processCpuPercentage  |  İşlem CPU 'SU  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  performanceCounters/Processıobitespersecond  |  İşlem GÇ oranı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  performanceCounters/Processorcpuyüzdesi  |  İşlemci zamanı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  performanceCounters/processPrivateBytes  |  İşlem özel baytları  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  performanceCounters/requestExecutionTime  |  HTTP isteği yürütme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  performanceCounters/Requestsınqueue  |  Uygulama kuyruğundaki HTTP istekleri  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  performanceCounters/requestsPerSecond  |  HTTP istek hızı  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  istek/sayı  |  Sunucu istekleri  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft. Insights/bileşenler  |  istek/süre  |  Sunucu yanıt süresi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  istek/başarısız  |  Başarısız istekler  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft. Insights/bileşenler  |  istek/hız  |  Sunucu isteği hızı  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Insights/bileşenler  |  izlemeler/say  |  Lerin  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.KeyVault/vaults  |  ServiceApiHit  |  Toplam hizmet API 'Si Isabet sayısı  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.KeyVault/vaults  |  Hizmet Apilatesi  |  Genel hizmet API 'Si gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.KeyVault/vaults  |  ServiceApiResult  |  Toplam hizmet API 'Si sonuçları  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Cachekullanım  |  Önbellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  ContinuousExportMaxLatenessMinutes  |  Sürekli dışarı aktarma en fazla dakika  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Continuousexportnumofrecordsexports  |  Sürekli dışa aktarma-dışarı aktarılmış kayıtların sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  ContinuousExportPendingCount  |  Sürekli dışarı aktarma bekleyen sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  ContinuousExportResult  |  Sürekli dışarı aktarma sonucu  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  CPU  |  CPU  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Processedforeventhubs olayları  |  İşlenen Olaylar (olay/IoT Hub 'Ları için)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Dışarı Aktaramstilileştirme  |  Kullanım verme  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Inestionlatencınseconds  |  Alma gecikmesi (saniye)  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Inestionresult  |  Alım sonucu  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Alım kullanımı  |  Alım kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Inestionvolumeınmb  |  Alım birimi (MB)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  Alı  |  Canlı tut  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  QueryDuration  |  Sorgu süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  SteamingIngestRequestRate  |  Akış alma Isteği oranı  |  Sayı  |  Kterequestspersecond | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  StreamingIngestDataRate  |  Akış alma verileri oranı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  StreamingIngestDuration  |  Akış alma süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. kusto/kümeler  |  StreamingIngestResults  |  Akış alma sonucu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionLatency  |  Eylem gecikmesi   |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsCompleted  |  Tamamlanan eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsFailed  |  Başarısız eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsSkipped  |  Atlanan eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsStarted  |  Başlatılan eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionsSucceeded  |  İşlemler başarılı oldu   |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Actionbaşarılı gecikme süresi  |  Eylem başarı gecikmesi   |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  ActionThrottledEvents  |  Eylem kısıtlanmış olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Integrationserviceenvironmentconnectormemoryusage  |  Tümleştirme Hizmeti Ortamı için bağlayıcı bellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Integrationserviceenvironmentconnectorprocessorusage  |  Tümleştirme Hizmeti Ortamı için bağlayıcı Işlemci kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Integrationserviceenvironmentworkflowmemoryusage  |  Tümleştirme Hizmeti Ortamı için iş akışı bellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Integrationserviceenvironmentworkflowprocessorusage  |  Tümleştirme Hizmeti Ortamı için iş akışı Işlemcisi kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunFailurePercentage yüzdesi  |  Çalıştırma hatası yüzdesi  |  Yüzde  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunLatency  |  Çalıştırma gecikmesi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsCancelled  |  Çalıştırma Iptal edildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsCompleted  |  Tamamlanan çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsFailed  |  Çalıştırma başarısız  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsStarted  |  Çalışma başlatıldı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunsSucceeded  |  Çalışma başarılı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunStartThrottledEvents  |  Kısıtlanmış başlangıç olaylarını Çalıştır  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Runbaşarılı gecikme süresi  |  Çalıştırma başarılı gecikme süresi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  RunThrottledEvents  |  Kısıtlanmış olayları Çalıştır  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Triggerfirelatlik  |  Tetikleyici Tetikleme gecikmesi   |  Saniyeden  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggerLatency  |  Tetikleme gecikmesi   |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersCompleted  |  Tamamlanan Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersFailed  |  Başarısız Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Triggersharekete geçirildi  |  Tetiklenen Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersSkipped  |  Atlanan Tetikleyiciler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersStarted  |  Başlatılan Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggersSucceeded  |  Başarılı Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Logic/ıntegrationserviceortamortamları  |  Triggerbaşarılı gecikme  |  Tetikleyici başarı gecikme süresi   |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Logic/ıntegrationserviceortamortamları  |  TriggerThrottledEvents  |  Kısıtlanmış olayları tetikleyin  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/workflows  |  ActionLatency  |  Eylem gecikmesi   |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  ActionsCompleted  |  Tamamlanan eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  ActionsFailed  |  Başarısız eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  ActionsSkipped  |  Atlanan eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  ActionsStarted  |  Başlatılan eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  ActionsSucceeded  |  İşlemler başarılı oldu   |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/workflows  |  Actionbaşarılı gecikme süresi  |  Eylem başarı gecikmesi   |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  ActionThrottledEvents  |  Eylem kısıtlanmış olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  Billableactionyürütmeler  |  Faturalanabilir eylem yürütmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  Billabletriggeryürütmeler  |  Faturalanabilir tetikleyici yürütmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  BillingUsageNativeOperation  |  Yerel Işlem yürütmeleri için faturalandırma kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  BillingUsageNativeOperation  |  Yerel Işlem yürütmeleri için faturalandırma kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  BillingUsageStandardConnector  |  Standart bağlayıcı yürütmeleri için faturalandırma kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  BillingUsageStandardConnector  |  Standart bağlayıcı yürütmeleri için faturalandırma kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  Billingusagestorampatüketim  |  Depolama tüketimi yürütmeleri için faturalama kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  Billingusagestorampatüketim  |  Depolama tüketimi yürütmeleri için faturalama kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  RunFailurePercentage yüzdesi  |  Çalıştırma hatası yüzdesi  |  Yüzde  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/workflows  |  RunLatency  |  Çalıştırma gecikmesi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  RunsCancelled  |  Çalıştırma Iptal edildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  RunsCompleted  |  Tamamlanan çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  RunsFailed  |  Çalıştırma başarısız  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  RunsStarted  |  Çalışma başlatıldı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  RunsSucceeded  |  Çalışma başarılı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  RunStartThrottledEvents  |  Kısıtlanmış başlangıç olaylarını Çalıştır  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/workflows  |  Runbaşarılı gecikme süresi  |  Çalıştırma başarılı gecikme süresi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  RunThrottledEvents  |  Kısıtlanmış olayları Çalıştır  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  Totalbillableyürütmeleri  |  Toplam faturalandırılabilir yürütmeler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/workflows  |  Triggerfirelatlik  |  Tetikleyici Tetikleme gecikmesi   |  Saniyeden  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Logic/workflows  |  TriggerLatency  |  Tetikleme gecikmesi   |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  TriggersCompleted  |  Tamamlanan Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  TriggersFailed  |  Başarısız Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  Triggersharekete geçirildi  |  Tetiklenen Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  TriggersSkipped  |  Atlanan Tetikleyiciler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  TriggersStarted  |  Başlatılan Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  TriggersSucceeded  |  Başarılı Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/workflows  |  Triggerbaşarılı gecikme  |  Tetikleyici başarı gecikme süresi   |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/workflows  |  TriggerThrottledEvents  |  Kısıtlanmış olayları tetikleyin  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Etkin çekirdekler  |  Etkin çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Etkin düğümler  |  Etkin düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Tamamlanan çalıştırmalar  |  Tamamlanan çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Başarısız çalıştırmalar  |  Başarısız çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Boştaki çekirdekler  |  Boştaki çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Boştaki düğümler  |  Boştaki düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Çekirdekleri bırakma  |  Çekirdekleri bırakma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Düğümlerden çıkılıyor  |  Düğümlerden çıkılıyor  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Model Dağıtımı başarısız oldu  |  Model Dağıtımı başarısız oldu  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Model Dağıtımı başladı  |  Model Dağıtımı başladı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Model Dağıtımı başarılı oldu  |  Model Dağıtımı başarılı oldu  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Model kaydı başarısız oldu  |  Model kaydı başarısız oldu  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Model kaydı başarılı oldu  |  Model kaydı başarılı oldu  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Önden çekirdekler  |  Önden çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Önden düğümler  |  Önden düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Kota kullanım yüzdesi  |  Kota kullanım yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Başlatılan çalıştırmalar  |  Başlatılan çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Toplam çekirdek sayısı  |  Toplam çekirdek sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Toplam düğüm sayısı  |  Toplam düğüm sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Kullanılamayan çekirdekler  |  Kullanılamayan çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. MachineLearningServices/çalışma alanları  |  Kullanılamayan düğümler  |  Kullanılamayan düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Maps/hesaplar  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Maps/hesaplar  |  Kullanım  |  Kullanım  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  AssetCount  |  Varlık sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  AssetQuota  |  Varlık kotası  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  AssetQuotaUsedPercentage  |  Kullanılan varlık kotası yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  ContentKeyPolicyCount  |  İçerik anahtarı Ilke sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  ContentKeyPolicyQuota  |  İçerik anahtarı Ilke kotası  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  ContentKeyPolicyQuotaUsedPercentage  |  İçerik anahtarı Ilke kotası kullanılan yüzde  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  Streammingpolicycount  |  Akış Ilkesi sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  StreamingPolicyQuota  |  Akış Ilkesi kotası  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices  |  StreamingPolicyQuotaUsedPercentage  |  Akış Ilkesi kotası kullanılan yüzde  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices/streamingEndpoints  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices/streamingEndpoints  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Media/mediaservices/streamingEndpoints  |  Başarı E2e  |  Başarılı uçtan uca gecikme süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  GCPauseTotalCount  |  GC duraklatma sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  GCPauseTotalTime  |  GC duraklatma toplam süre  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  MaxOldGenMemoryPoolBytes  |  En fazla kullanılabilir eski nesil veri boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  OldGenMemoryPoolBytes  |  Eski nesil veri boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  OldGenPromotedBytes  |  Eski nesil veri boyutuna yükselt  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  ServiceCpuUsagePercentage  |  Hizmet CPU kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  ServiceMemoryCommitted  |  Hizmet belleği atandı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  ServiceMemoryMax  |  En fazla hizmet belleği  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  Servicememorykullanıldı  |  Kullanılan hizmet belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  SystemCpuUsagePercentage  |  Sistem CPU kullanım yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatErrorCount  |  Tomcat genel hatası  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  Tomcatreceived baytları  |  Tomcat alınan toplam bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatRequestMaxTime  |  Tomcat Isteği maksimum zamanı  |  Milisaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatRequestTotalCount  |  Tomcat Isteği toplam sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatRequestTotalTime  |  Tomcat Isteği toplam kez  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatResponseAvgTime  |  Tomcat Isteği ortalama zamanı  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSentBytes  |  Tomcat gönderilen toplam bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionActiveCurrentCount  |  Tomcat oturum etkin sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionActiveMaxCount  |  Tomcat oturumu en fazla etkin sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionAliveMaxTime  |  Tomcat oturumu maksimum etkin süresi  |  Milisaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionCreatedCount  |  Tomcat oturum oluşturma sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionExpiredCount  |  Tomcat oturumunun süre dolma sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  TomcatSessionRejectedCount  |  Tomcat oturumu reddedildi sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Microservices4Spring/Appkümeler  |  YoungGenPromotedBytes  |  Küçük ölçekli oluşturma veri boyutuna yükselt  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları  |  VolumePoolAllocatedUsed  |  Ayrılan birim havuzu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları  |  VolumePoolTotalLogicalSize  |  Birim havuzu toplam mantıksal boyut  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  AverageReadLatency  |  Ortalama okuma gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  AverageWriteLatency  |  Ortalama yazma gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  ReadIops  |  IOPS 'yi oku  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  Birimlogicalsize  |  Birim mantıksal boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  VolumeSnapshotSize  |  Birim anlık görüntü boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri  |  Writeıops  |  IOPS yaz  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  ApplicationGatewayTotalTime  |  Application Gateway toplam süre  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  Avgrequestcountperhealthyıhost  |  Sağlıklı ana bilgisayar başına dakika başına istek  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  BackendConnectTime  |  Arka uç bağlantı saati  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  BackendFirstByteResponseTime  |  Arka uç Ilk bayt yanıt süresi  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  BackendLastByteResponseTime  |  Arka uç son bayt yanıt süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  BackendResponseStatus  |  Arka uç yanıt durumu  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  BlockedCount  |  Web uygulaması güvenlik duvarı engellenen Istek kuralı dağıtımı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  BlockedReqCount  |  Web uygulaması güvenlik duvarı engellenen Istek sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  BytesReceived  |  Alınan bayt  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  BytesSent  |  Gönderilen bayt  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  CapacityUnits  |  Geçerli kapasite birimleri  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  ClientRtt  |  İstemci RTT  |  Mayacak  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  ComputeUnits  |  Geçerli Işlem birimleri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  CurrentConnections  |  Geçerli bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  FailedRequests  |  Başarısız İstekler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Healthyıhostcount  |  Sağlıklı konak sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  MatchedCount  |  Web uygulaması güvenlik duvarı toplam kural dağıtımı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  ResponseStatus  |  Yanıt durumu  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  Aktarım hızı  |  Aktarım hızı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  TlsProtocol  |  İstemci TLS protokolü  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Toplam Istek sayısı  |  Toplam İstek Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Unhealthyıhostcount  |  Sağlıksız konak sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/azurefirewalls  |  ApplicationRuleHit  |  Uygulama kuralları isabet sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/azurefirewalls  |  Veri Işlendi  |  İşlenen veri  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/azurefirewalls  |  FirewallHealth  |  Güvenlik duvarı sistem durumu  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/azurefirewalls  |  NetworkRuleHit  |  Ağ kuralları isabet sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/azurefirewalls  |  SNATPortUtilization  |  SNAT bağlantı noktası kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Connections  |  BitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Connections  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/dnszones  |  QueryVolume  |  Sorgu hacmi  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Network/dnszones  |  Recordsetkapaıyutilileştirme  |  Kayıt kümesi kapasite kullanımı  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/dnszones  |  Kayıt kümesi sayısı  |  Kayıt kümesi sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  ArpAvailability  |  ARP kullanılabilirliği  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  BgpAvailability  |  BGP kullanılabilirliği  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  BitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  GlobalReachBitsInPerSecond  |  GlobalReachBitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  GlobalReachBitsOutPerSecond  |  GlobalReachBitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  QosDropBitsInPerSecond  |  DroppedInBitsPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutedevreleri  |  QosDropBitsOutPerSecond  |  DroppedOutBitsPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Expressroutedevreleri/eşayarları  |  BitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Expressroutedevreleri/eşayarları  |  BitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutegateway 'ler  |  ErGatewayConnectionBitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Network/Expressroutegateway 'ler  |  ErGatewayConnectionBitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/expressRoutePorts  |  AdminState  |  AdminState  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/expressRoutePorts  |  LineProtocol  |  LineProtocol  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/expressRoutePorts  |  PortBitsInPerSecond  |  BitsInPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/expressRoutePorts  |  PortBitsOutPerSecond  |  BitsOutPerSecond  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/expressRoutePorts  |  RxLightLevel  |  RxLightLevel  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/expressRoutePorts  |  TxLightLevel  |  TxLightLevel  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  BackendHealthPercentage  |  Arka uç sistem durumu yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  BackendRequestCount  |  Arka uç Istek sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  BackendRequestLatency  |  Arka uç Isteği gecikmesi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  Billilableresponsesize  |  Faturalanabilir yanıt boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  RequestCount  |  İstek Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  RequestSize  |  İstek boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  Yanıt boyutu  |  Yanıt boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  TotalLatency  |  Toplam gecikme süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Network/frontkapaklı  |  WebApplicationFirewallRequestCount  |  Web uygulaması güvenlik duvarı Istek sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/loadBalancers  |  AllocatedSnatPorts  |  Ayrılan SNAT bağlantı noktaları (Önizleme)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  ByteCount  |  Bayt sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  DipAvailability  |  Durum araştırma durumu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  PacketCount  |  Paket sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  SnatConnectionCount  |  SNAT bağlantı sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  SYNCount  |  SYN sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/loadBalancers  |  UsedSnatPorts  |  Kullanılan SNAT bağlantı noktaları (Önizleme)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  VipAvailability  |  Veri yolu kullanılabilirliği  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/NetworkInterfaces  |  BytesReceivedRate  |  Alınan bayt  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/NetworkInterfaces  |  BytesSentRate  |  Gönderilen bayt  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/NetworkInterfaces  |  PacketsReceivedRate  |  Alınan paketler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/NetworkInterfaces  |  PacketsSentRate  |  Gönderilen paketler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/networkWatchers/Connectionmonitörleri  |  AverageRoundtripMs  |  Ort. gidiş dönüş süresi (MS)  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/networkWatchers/Connectionmonitörleri  |  ChecksFailedPercent  |  Başarısız denetimler yüzdesi (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/networkWatchers/Connectionmonitörleri  |  ProbesFailedPercent  |  % Yoklama başarısız oldu  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/networkWatchers/Connectionmonitörleri  |  Roundroundtimems  |  Gidiş dönüş süresi (MS) (Önizleme)  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  ByteCount  |  Bayt sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  BytesDroppedDDoS  |  Gelen bayt bırakıldı DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  BytesForwardedDDoS  |  İletilen gelen bayt sayısı  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  BytesInDDoS  |  Gelen bayt sayısı DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  Ddobir Ggersynpackets  |  DDoS azaltma tetiklenmesi için gelen SYN paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  DDO, Ggertcppackets  |  DDoS azaltma tetiklemeye yönelik gelen TCP paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  DDO, Ggerudppaketleri  |  DDoS risk azaltma tetiklenecek gelen UDP paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  IfUnderDDoSAttack  |  DDoS saldırısı altında  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  PacketCount  |  Paket sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  PacketsDroppedDDoS  |  DDoS bırakılan gelen paketler  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  PacketsForwardedDDoS  |  DDoS iletilen gelen paketler  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  PacketsInDDoS  |  Gelen paketler DDoS  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  SynCount  |  SYN sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  TCPBytesDroppedDDoS  |  Gelen TCP bayt bırakılan DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  TCPBytesForwardedDDoS  |  İletilen gelen TCP baytları  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  TCPBytesInDDoS  |  Gelen TCP bayt DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  TCPPacketsDroppedDDoS  |  Gelen TCP paketleri bırakıldı DDoS  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  TCPPacketsForwardedDDoS  |  DDoS iletilen gelen TCP paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  TCPPacketsInDDoS  |  Gelen TCP paketleri DDoS  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  UDPBytesDroppedDDoS  |  Gelen UDP bayt bırakılan DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  UDPBytesForwardedDDoS  |  İletilen gelen UDP baytları  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  UDPBytesInDDoS  |  Gelen UDP bayt sayısı DDoS  |  BytesPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  UDPPacketsDroppedDDoS  |  Gelen UDP paketleri bırakıldı  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  UDPPacketsForwardedDDoS  |  DDoS iletilen gelen UDP paketleri  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  UDPPacketsInDDoS  |  Gelen UDP paketleri DDoS  |  Sayaçpersaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Publicıpaddresses  |  VipAvailability  |  Veri yolu kullanılabilirliği  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/trafficManagerProfiles  |  ProbeAgentCurrentEndpointStateByProfileResourceId  |  Uç nokta tarafından uç nokta durumu  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Network/trafficManagerProfiles  |  QpsByEndpoint  |  Bitiş noktasına göre sorgular döndürüldü  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  AverageBandwidth  |  Ağ Geçidi S2S bant genişliği  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  P2SBandwidth  |  Ağ Geçidi P2S bant genişliği  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  P2SConnectionCount  |  P2S bağlantı sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelAverageBandwidth  |  Tünel bant genişliği  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelEgressBytes  |  Tünel çıkış baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelEgressPacketDropTSMismatch  |  Tünel çıkış uyumsuzluğu paket bırakma  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelEgressPackets  |  Tünel çıkış paketleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelIngressBytes  |  Tünel giriş baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelIngressPacketDropTSMismatch  |  Tünel girişi uyumsuz paket bırakma  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/Virtualnetworkgateway 'ler  |  TunnelIngressPackets  |  Tünel giriş paketleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Network/virtualNetworks  |  PingMeshAverageRoundtripMs  |  Bir VM 'ye ping için gidiş dönüş süresi  |  Mayacak  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Network/virtualNetworks  |  PingMeshProbesFailedPercent  |  VM 'ye ping işlemi başarısız oldu  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen  |  Gelen İletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen. tüm. failedistekleri  |  Tüm gelen başarısız Istekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen. ALL. Requests  |  Tüm gelen Istekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen. zamanlandı  |  Zamanlanan anında Iletme bildirimleri gönderildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  gelen. zamanlandı. iptal  |  Zamanlanan anında Iletme bildirimleri Iptal edildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. tümü  |  Yükleme yönetimi Işlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. Delete  |  Yükleme Işlemlerini silme  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. Get  |  Yükleme Işlemlerini al  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. Patch  |  Düzeltme eki yükleme Işlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  yükleme. upsert  |  Yükleme Işlemlerini oluşturma veya güncelleştirme  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  notificationhub. gönderimleri  |  Tüm giden bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.allpns.badorexpiredchannel  |  Hatalı veya süre dolma kanalı hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.allpns.channelerror  |  Kanal hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. allpns. ınvalidpayload  |  Yük Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.allpns.pnserror  |  Harici Bildirim Sistemi Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. allpns. Success  |  Başarılı bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. badchannel  |  APNS Geçersiz Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.apns.expiredchannel  |  APNS Süresi Dolan Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. ınvalidcredentials  |  APNS yetkilendirme hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. ınvalidnotificationsize  |  APNS Geçersiz Bildirim Boyutu Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. pnserror  |  APNS Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. APNs. Success  |  APNS Başarılı Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. authenticationerror  |  GCM kimlik doğrulama hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. badchannel  |  GCM Geçersiz Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. expiredchannel  |  GCM Süresi Dolan Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. ınvalidcredentials  |  GCM Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. ınvalidnotificationformat  |  GCM Geçersiz Bildirim Biçimi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. ınvalidnotificationsize  |  GCM Geçersiz Bildirim Boyutu Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.gcm.pnserror  |  GCM Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. Success  |  GCM Başarılı Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. gcm. kısıtlanıyor  |  GCM Kısıtlanan Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.gcm.wrongchannel  |  GCM Yanlış Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.mpns.authenticationerror  |  MPNS kimlik doğrulama hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. badchannel  |  MPNS Geçersiz Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.mpns.channeldisconnected  |  MPNS Kanalın Bağlantısı Kesildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. bırakıldı  |  MPNS Bırakılan Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. ınvalidcredentials  |  MPNS Geçersiz Kimlik Bilgileri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. ınvalidnotificationformat  |  MPNS Geçersiz Bildirim Biçimi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.mpns.pnserror  |  MPNS Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. Success  |  MPNS Başarılı Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. MPNS. kısıtlanıyor  |  MPNS Kısıtlanan Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.wns.authenticationerror  |  WNS Kimlik Doğrulama Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. badchannel  |  WNS Geçersiz Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.wns.channeldisconnected  |  WNS Kanal Bağlantısı Kesildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. channelkısıtlanıyor  |  WNS Kanal Kısıtlandı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. bırakıldı  |  WNS Bırakılan Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.wns.expiredchannel  |  WNS Süresi Dolan Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. ınvalidcredentials  |  WNS Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. ınvalidnotificationformat  |  WNS Geçersiz Bildirim Biçimi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. ınvalidnotificationsize  |  WNS Geçersiz Bildirim Boyutu Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. ınvalidtoken  |  WNS Yetkilendirme Hataları (Geçersiz Belirteç)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  outgoing.wns.pnserror  |  WNS Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. Success  |  WNS Başarılı Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. kısıtlanıyor  |  WNS Kısıtlanan Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. tokenproviderunreachable  |  WNS Yetkilendirme Hataları (Ulaşılamıyor)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  giden. WNS. yanlışlıkla gtoken  |  WNS Yetkilendirme Hataları (Yanlış Belirteç)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. tümü  |  Kayıt İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. oluştur  |  Kayıt Oluşturma İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. Sil  |  Kayıt Silme İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. Get  |  Kayıt Okuma İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  kayıt. güncelleştirme  |  Kayıt Güncelleme İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar  |  scheduled.pending  |  Bekleyen zamanlanmış bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Kullanılabilir% Average_ bellek  |  % Kullanılabilir bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% kullanılabilir takas alanı  |  % Kullanılabilir takas alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  % Average_ kaydedilmiş bayt kullanımda  |  Kullanılan kaydedilmiş bayt yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% DPC Zamanı  |  % DPC Zamanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% boş ınomdes  |  % Boş ınomdes  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  % Average_ boş alan  |  Boş Alan Yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  % Average_ boş alan  |  Boş Alan Yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% boşta kalma süresi  |  % Boş zaman  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Kesme zamanı Average_%  |  Kesme zamanı yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% GÇ bekleme süresi  |  % GÇ bekleme süresi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% Iyi zaman  |  % İyi zaman  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% ayrıcalıklı zaman  |  % Ayrıcalıklı zaman  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% Işlemci zamanı  |  % İşlemci Zamanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% Işlemci zamanı  |  % İşlemci Zamanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% kullanılan ınomdes  |  % Kullanılan ınomdes  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Kullanılan% Average_ bellek  |  % Kullanılan bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Kullanılan Average_ alan yüzdesi  |  % Kullanılan alan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% kullanılan takas alanı  |  % Kullanılan takas alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_% Kullanıcı Zamanı  |  % Kullanıcı Zamanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Available MBayt  |  Kullanılabilir MBayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Available MBayt bellek  |  Kullanılabilir MBayt belleği  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Available MBayt takas  |  Kullanılabilir MBayt takas  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Okuma  |  Ortalama Disk sn/okuma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Okuma  |  Ortalama Disk sn/okuma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Aktarım  |  Ortalama Disk sn/Aktarım  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Yazma  |  Ortalama Disk sn/yazma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Avg. Disk sn/Yazma  |  Ortalama Disk sn/yazma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Alınan Average_Bytes/sn  |  Alınan Bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Gönderilen Average_Bytes/sn  |  Gönderilen bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Toplam Average_Bytes/sn  |  Toplam bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Current disk kuyruğu uzunluğu  |  Geçerli disk sırası uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk okunan bayt/sn  |  Disk Okuma Bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Okuma/sn Average_Disk  |  Disk Okuma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Okuma/sn Average_Disk  |  Disk Okuma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk aktarım sayısı/sn  |  Disk aktarımı/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk aktarım sayısı/sn  |  Disk aktarımı/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk yazma bayt/sn  |  Disk Yazma Bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk yazma/sn  |  Disk Yazma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Disk yazma/sn  |  Disk Yazma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Free megabayt  |  Boş megabayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Free megabayt  |  Boş megabayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Fiziksel bellek Average_Free  |  Boş fiziksel bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Disk belleği dosyalarında Average_Free alanı  |  Disk belleği dosyalarındaki boş alan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Sanal bellek Average_Free  |  Boş sanal bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Logical disk bayt/sn  |  Mantıksal Disk Bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Okuma/sn Average_Page  |  Sayfa Okuma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Page yazma/sn  |  Sayfa Yazma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Pages/sn  |  Sayfa/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Pct ayrıcalıklı saat  |  Ayrıcalıklı zaman yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Pct Kullanıcı saati  |  Kullanım süresi yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Physical disk bayt/sn  |  Fiziksel disk bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Processes  |  İşlemler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Processor kuyruğu uzunluğu  |  İşlemci kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Disk belleği dosyalarında depolanan Average_Size  |  Disk belleği dosyalarında depolanan boyut  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total bayt  |  Toplam bayt sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total alınan bayt  |  Alınan toplam bayt sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total aktarılan baytlar  |  Aktarılan toplam bayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total çakışmaları  |  Toplam çakışma sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Alınan Average_Total paketleri  |  Alınan toplam paket sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Iletilen Average_Total paketleri  |  Aktarılan toplam paket sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total RX hataları  |  Toplam RX hatası sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Total TX hataları  |  Toplam TX hatası sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Uptime  |  Çalışma süresi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Used MBayt alanı değiştirme  |  Kullanılan MBayt değiştirme alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Used belleği Kbayt  |  Kullanılan bellek Kbayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Used belleği MBayt  |  Kullanılan bellek MBayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Users  |  Kullanıcılar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Average_Virtual paylaşılan bellek  |  Sanal paylaşılan bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Olay  |  Olay  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Operationalınsights/çalışma alanları  |  Sinyal  |  Sinyal  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Operationalınsights/çalışma alanları  |  Güncelleştir  |  Güncelleştir  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Powerbiadanmış/kapasiteler  |  memory_metric  |  Bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Powerbiadanmış/kapasiteler  |  memory_thrashing_metric  |  Bellek kullanımı (veri kümeleri)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Powerbiadanmış/kapasiteler  |  qpu_high_utilization_metric  |  QPU yüksek kullanım  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Powerbiadanmış/kapasiteler  |  QueryDuration  |  Sorgu süresi (veri kümeleri)  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Powerbiadanmış/kapasiteler  |  QueryPoolJobQueueLength  |  Sorgu havuzu Iş kuyruğu uzunluğu (veri kümeleri)  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ActiveConnection sayısı  |  ActiveConnection sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ActiveListeners  |  ActiveListeners  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Relay/Namespace  |  BytesTransferred  |  BytesTransferred  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ListenerConnections-ClientError  |  ListenerConnections-ClientError  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ListenerConnections-Sunucuhatası  |  ListenerConnections-Sunucuhatası  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ListenerConnections-başarılı  |  ListenerConnections-başarılı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  ListenerConnections-TotalRequests  |  ListenerConnections-TotalRequests  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  Listenerdisconnect  |  Listenerdisconnect  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  SenderConnections-ClientError  |  SenderConnections-ClientError  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  SenderConnections-Sunucuhatası  |  SenderConnections-Sunucuhatası  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  SenderConnections-başarılı  |  SenderConnections-başarılı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  SenderConnections-TotalRequests  |  SenderConnections-TotalRequests  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Relay/Namespace  |  Gönderenin bağlantısı kesiliyor  |  Gönderenin bağlantısı kesiliyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Search/searchServices  |  SearchLatency  |  Arama gecikmesi  |  Saniyeden  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Search/searchServices  |  SearchQueriesPerSecond  |  Sorgu arama/saniye  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Search/searchServices  |  Tüm Searchqueriespercentage 'ı kısıtlar  |  Kısıtlanmış arama sorguları yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  ActiveConnection sayısı  |  ActiveConnection sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  ActiveMessages  |  Bir kuyruktaki/konudaki etkin ileti sayısı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  ConnectionsClosed  |  Bağlantı kapatıldı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  ConnectionsOpened  |  Bağlantılar açıldı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  CPUXNS  |  CPU (kullanım dışı)  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  DeadletteredMessages  |  Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı.  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ServiceBus/namespaces  |  Incomingmessages  |  Gelen İletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ServiceBus/namespaces  |  Incomingrequests  |  Gelen Istekler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  İletiler  |  Kuyruktaki/konudaki iletilerin sayısı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  NamespaceCpuUsage  |  CPU  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  NamespaceMemoryUsage  |  Bellek Kullanımı  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.ServiceBus/namespaces  |  OutgoingMessages  |  Giden İletiler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  ScheduledMessages  |  Bir kuyruktaki/konudaki zamanlanan ileti sayısı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  ServerErrors  |  Sunucu hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  Boyut  |  Boyut  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  SuccessfulRequests  |  Başarılı Istekler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  OperationName  |  Kısıtlanmış Istekler.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  UserErrors  |  Kullanıcı hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  WSXNS  |  Bellek kullanımı (kullanım dışı)  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ActualCpu  |  ActualCpu  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ActualMemory  |  ActualMemory  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Ayrılatedcpu  |  Ayrılatedcpu  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Ayrılan bellek  |  Ayrılan bellek  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ApplicationStatus  |  ApplicationStatus  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ContainerStatus  |  ContainerStatus  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Cpukullanımı  |  Cpukullanımı  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Memoryutilileştirme  |  Memoryutilileştirme  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  RestartCount  |  RestartCount  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  Servicereperepstatus  |  Servicereperepstatus  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Servicefabrickafes/uygulamaları  |  ServiceStatus  |  ServiceStatus  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  ConnectionCount  |  Bağlantı sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  Inboundtraffic  |  Gelen Trafik  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  MessageCount  |  İleti sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  OutboundTraffic  |  Giden Trafik  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  SystemErrors  |  Sistem hataları  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SignalRService/SignalR  |  UserErrors  |  Kullanıcı hataları  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft. SQL/ManagedInstances  |  avg_cpu_percent  |  Ortalama CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. SQL/ManagedInstances  |  io_bytes_read  |  Okunan GÇ baytları  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. SQL/ManagedInstances  |  io_bytes_written  |  Yazılan GÇ baytları  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. SQL/ManagedInstances  |  io_requests  |  GÇ istek sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. SQL/ManagedInstances  |  reserved_storage_mb  |  Ayrılan depolama alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. SQL/ManagedInstances  |  storage_space_used_mb  |  Kullanılan depolama alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. SQL/ManagedInstances  |  virtual_core_count  |  Sanal çekirdek sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/sunucuları  |  database_dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. SQL/sunucuları  |  database_storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/sunucuları  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. SQL/sunucuları  |  dtu_used  |  Kullanılan DTU  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. SQL/sunucuları  |  storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  app_cpu_billed  |  Faturalandırılan uygulama CPU 'SU  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  app_cpu_percent  |  Uygulama CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  app_memory_percent  |  Uygulama belleği yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  blocked_by_firewall  |  Güvenlik duvarı tarafından engellendi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  cache_hit_percent  |  İsabetli önbellek okuması yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  cache_used_percent  |  Önbellek kullanılan yüzde  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  connection_failed  |  Başarısız Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  connection_successful  |  Başarılı bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  cpu_limit  |  CPU sınırı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  cpu_used  |  Kullanılan CPU  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  ölü kilit  |  Çık  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  dtu_limit  |  DTU sınırı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  dtu_used  |  Kullanılan DTU  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  dwu_consumption_percent  |  DWU yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  dwu_limit  |  DWU sınırı  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  dwu_used  |  Kullanılan DWU  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  local_tempdb_usage_percent  |  Yerel tempdb yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  log_write_percent  |  Günlük GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  memory_usage_percent  |  Bellek yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  sessions_percent  |  Oturum yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  Depo  |  Kullanılan veri alanı  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  storage_percent  |  Kullanılan veri alanı yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  tempdb_data_size  |  Tempdb veri dosyası boyutu kilobayt  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  tempdb_log_size  |  Tempdb günlük dosyası boyutu kilobayt  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/databases  |  tempdb_log_used_percent  |  Kullanılan tempdb günlüğü yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  workers_percent  |  Çalışan yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/databases  |  xtp_storage_percent  |  Bellek içi OLTP depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage_percent  |  Ayrılan veri alanı yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  cpu_limit  |  CPU sınırı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  cpu_used  |  Kullanılan CPU  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_cpu_limit  |  CPU sınırı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_cpu_used  |  Kullanılan CPU  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_eDTU_used  |  eDTU kullanıldı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_log_write_percent  |  Günlük GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_sessions_percent  |  Oturum yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_workers_percent  |  Çalışan yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  eDTU_limit  |  eDTU sınırı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  eDTU_used  |  eDTU kullanıldı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  log_write_percent  |  Günlük GÇ yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  sessions_percent  |  Oturum yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  storage_limit  |  Veri boyutu üst sınırı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  storage_percent  |  Kullanılan veri alanı yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  tempdb_data_size  |  Tempdb veri dosyası boyutu kilobayt  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_size  |  Tempdb günlük dosyası boyutu kilobayt  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_used_percent  |  Kullanılan tempdb günlüğü yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  workers_percent  |  Çalışan yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  xtp_storage_percent  |  Bellek içi OLTP depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts  |  UsedCapacity  |  Kullanılan kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  BlobCapacity  |  Blob Kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  BLOB sayısı  |  Blob Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  ContainerCount  |  Blob Kapsayıcı Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Dizin kapasitesi  |  Dizin kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Dosya kapasitesi  |  Dosya kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  FileCount  |  Dosya sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Dosya ShareCount  |  Dosya paylaşma sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Dosya Sharequota  |  Dosya paylaşımının kota boyutu  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Dosya paylaşımının anlık görüntü sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Dosya paylaşımının anlık görüntü boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Storage/storageAccounts/fileServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  QueueCapacity  |  Kuyruk Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  QueueCount  |  Kuyruk Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  QueueMessageCount  |  Kuyruk İleti Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Başarı E2e  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  TableCapacity  |  Tablo Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  TableCount  |  Tablo Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  TableEntityCount  |  Tablo Varlık Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Clienentiops  |  Toplam Istemci ıOPS 'si  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  ClientLatency  |  Ortalama Istemci gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Clientlockıops  |  İstemci kilidi ıOP  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  ClientMetadataReadIOPS  |  İstemci meta verileri okuma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Clientmetadatawriteıops  |  İstemci meta verileri yazma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  ClientReadIOPS  |  İstemci okuma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Clientreadüretilen Iş  |  Ortalama önbellek okuma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  ClientWriteIOPS  |  İstemci yazma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  ClientWriteThroughput  |  Ortalama önbellek yazma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetasyncwriteüretilen Iş  |  StorageTarget zaman uyumsuz yazma üretilen Işi  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetfillüretilen Iş  |  StorageTarget dolgusu Işleme  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargethegizli  |  Depolama hedefi durumu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetıops  |  Toplam StorageTarget ıOPS  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  StorageTargetLatency  |  StorageTarget gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetmetadatareadıops  |  StorageTarget meta verileri okuma ıOPS 'si  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetmetadatawriteıops  |  StorageTarget meta verileri yazma ıOPS  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetreadaheadüretilen Iş  |  StorageTarget Ileriye yönelik okuma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetreadıops  |  Storagetaral okuma ıOPS  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetsyncwriteüretilen Iş  |  StorageTarget zaman uyumlu yazma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargettotalreadüretilen Iş  |  StorageTarget toplam okuma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargettotalwriteverimini  |  StorageTarget Toplam yazma performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Storagetargetwriteıops  |  StorageTarget yazma ıOPS  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. StorageCache/önbellekler  |  Çalışma süresi  |  Çalışma süresi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  ServerSyncSessionResult  |  Eşitleme oturumu sonucu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  StorageSyncBatchTransferredFileBytes  |  Eşitlenen baytlar  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  StorageSyncRecalledNetworkBytesByApplication  |  Uygulamaya göre bulut katmanlama geri çağırma boyutu  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  Storagesyncyeniden hesaplama Ledtotalnetworkbytes  |  Bulut katmanlama geri çağırma boyutu  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  Storagesyncyeniden hesaplama Liototalsizebytes  |  Bulut katmanlama geri çağırma  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  StorageSyncRecallThroughputBytesPerSecond  |  Bulut katmanlama geri çekme performansı  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  Storagesyncserversinyal  |  Sunucu çevrimiçi durumu  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  StorageSyncSyncSessionAppliedFilesCount  |  Eşitlenen dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices  |  StorageSyncSyncSessionPerItemErrorsCount  |  Dosyalar eşitlenmiyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices/registeredServers  |  Sunucusinyal  |  Sunucu çevrimiçi durumu  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices/registeredServers  |  Sunucu yeniden hesaplama Liototalsizebytes  |  Bulut katmanlama geri çağırma  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices/syncGroups  |  SyncGroupBatchTransferredFileBytes  |  Eşitlenen baytlar  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionAppliedFilesCount  |  Eşitlenen dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionPerItemErrorsCount  |  Dosyalar eşitlenmiyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointBatchTransferredFileBytes  |  Eşitlenen baytlar  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionAppliedFilesCount  |  Eşitlenen dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionPerItemErrorsCount  |  Dosyalar eşitlenmiyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  AMLCalloutFailedRequests  |  Başarısız İşlev İstekleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  AMLCalloutInputEvents  |  İşlev Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  AMLCalloutRequests  |  İşlev İstekleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Dönüştürme hataları  |  Veri Dönüştürme Hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  DeserializationError  |  Giriş Serileştirme Kaldırma Hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Droppedorayarlantedevents  |  Sıra dışı olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  EarlyInputEvents  |  Erken Giriş Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Hatalar  |  Çalışma Zamanı Hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Inputeventbytes  |  Giriş Olayı Bayt Sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Inputevents  |  Giriş Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Inputeventssourcesbackgünlüğe kaydediliyor  |  Biriktirme Listesindeki Giriş Olayları  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Inputeventssourcespersecond  |  Alınan Giriş Kaynakları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  LateInputEvents  |  Geç Giriş Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  OutputEvents  |  Çıkış Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Outputsulu Markdelayseconds  |  Eşik Gecikmesi  |  Saniyeden  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. StreamAnalytics/streamingjobs  |  Resourcekullanım  |  SU Kullanım Yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Disk okuma bayt sayısı  |  Disk okuma bayt sayısı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Disk okuma Işlemi/sn  |  Disk okuma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Disk yazma baytları  |  Disk yazma baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Disk yazma Işlemi/sn  |  Disk yazma Işlemi/sn  |  Sayaçpersaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskReadBytesPerSecond  |  Disk okuma bayt/sn  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskReadLatency  |  Disk okuma gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskReadOperations  |  Disk okuma Işlemleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskWriteBytesPerSecond  |  Disk yazma bayt/sn  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskWriteLatency  |  Disk yazma gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  DiskWriteOperations  |  Disk yazma Işlemleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  MemoryActive  |  Bellek etkin  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Memoryverildi  |  Verilen bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  MemoryUsed  |  Kullanılan bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Networkınbytespersecond  |  Bayt/sn cinsinden ağ  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  NetworkOutBytesPerSecond  |  Ağ çıkış bayt/sn  |  BytesPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft. Vmwarechoparlör basit/virtualMachines  |  Yüztagecpuready  |  CPU için hazırlanma yüzdesi  |  Milisaniye  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  ActiveRequests  |  Etkin Istekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  AverageResponseTime  |  Ortalama Yanıt Süresi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  BytesReceived  |  Veri Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  BytesSent  |  Giden Veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Cpuyüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  DiskQueueLength  |  Disk kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http101  |  Http 101  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http2xx  |  Http 2xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http3xx  |  Http 3xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http401  |  Http 401  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http403  |  Http 403  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http404  |  HTTP 404  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http406  |  Http 406  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http4xx  |  Http 4xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Http5xx  |  Http Sunucu Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  HttpQueueLength  |  Http kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Largeappserviceplanınstances  |  Büyük App Service çalışanları planlıyor  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Mediumappserviceplanınstances  |  Orta App Service çalışanları planlıyor  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  MemoryPercentage  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Smallappserviceplanınstances  |  Küçük App Service çalışanları planlıyor  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/multiRolePools  |  Toplam ön uçlar  |  Toplam ön uçlar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  Cpuyüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  MemoryPercentage  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  WorkersAvailable  |  Kullanılabilir çalışanlar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  WorkersTotal  |  Toplam çalışan sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft. Web/hostingEnvironments/workerPools  |  WorkersUsed  |  Kullanılan çalışanlar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  BytesReceived  |  Veri Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  BytesSent  |  Giden Veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  Cpuyüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  DiskQueueLength  |  Disk kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  HttpQueueLength  |  Http kuyruğu uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  MemoryPercentage  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpCloseWait  |  TCP kapatma bekleme  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpClosing  |  TCP kapatma  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  Tcpkurdu  |  TCP oluşturuldu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpFinWait1  |  TCP Fin bekleme 1  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpFinWait2  |  TCP Fin bekleme 2  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpLastAck  |  TCP son ACK  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  Tcpsynalındı  |  TCP SYN alındı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpSynSent  |  TCP SYN gönderilen  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpTimeWait  |  TCP zaman bekleme  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  AppConnections  |  Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  AverageMemoryWorkingSet  |  Ortalama bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  AverageResponseTime  |  Ortalama Yanıt Süresi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  BytesReceived  |  Veri Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  BytesSent  |  Giden Veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  CpuTime  |  CPU Süresi  |  Saniyeden  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  CurrentAssemblies  |  Geçerli derlemeler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Işlev yürütme sayısı  |  İşlev yürütme sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  FunctionExecutionUnits  |  İşlev yürütme birimleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Gen0Collections  |  Gen 0 çöp koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Gen1Collections  |  Gen 1 çöp koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Gen2Collections  |  Gen 2 çöp koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  İşlendiğini  |  Tanıtıcı Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  HealthCheckStatus  |  Durum denetimi durumu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http101  |  Http 101  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http2xx  |  Http 2xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http3xx  |  Http 3xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http401  |  Http 401  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http403  |  Http 403  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http404  |  HTTP 404  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http406  |  Http 406  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http4xx  |  Http 4xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Http5xx  |  Http Sunucu Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  HttpResponseTime  |  Yanıt Süresi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Iootherbytespersecond  |  GÇ diğer bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Iootheroperationspersecond  |  GÇ diğer Işlem/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Ioreadbytespersecond  |  GÇ okuma bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  IoReadOperationsPerSecond  |  GÇ okuma Işlemi/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Iowritebytespersecond  |  GÇ yazma bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  Iowriteoperationspersecond  |  GÇ Yazma Işlemi/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  MemoryWorkingSet  |  Bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  PrivateBytes  |  Özel Baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  RequestsInApplicationQueue  |  Uygulama kuyruğundaki istekler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  İş Parçacıkları  |  İş Parçacığı Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  TotalAppDomains  |  Toplam uygulama etki alanları  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites  |  TotalAppDomainsUnloaded  |  Toplam yüklenmeyen uygulama etki alanları  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  AppConnections  |  Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  AverageMemoryWorkingSet  |  Ortalama bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  AverageResponseTime  |  Ortalama Yanıt Süresi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  BytesReceived  |  Veri Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  BytesSent  |  Giden Veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  CpuTime  |  CPU Süresi  |  Saniyeden  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  CurrentAssemblies  |  Geçerli derlemeler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Işlev yürütme sayısı  |  İşlev yürütme sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  FunctionExecutionUnits  |  İşlev yürütme birimleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Gen0Collections  |  Gen 0 çöp koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Gen1Collections  |  Gen 1 çöp koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Gen2Collections  |  Gen 2 çöp koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  İşlendiğini  |  Tanıtıcı Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  HealthCheckStatus  |  Durum denetimi durumu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http101  |  Http 101  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http2xx  |  Http 2xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http3xx  |  Http 3xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http401  |  Http 401  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http403  |  Http 403  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http404  |  HTTP 404  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http406  |  Http 406  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http4xx  |  Http 4xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Http5xx  |  Http Sunucu Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  HttpResponseTime  |  Yanıt Süresi  |  Saniyeden  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Iootherbytespersecond  |  GÇ diğer bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Iootheroperationspersecond  |  GÇ diğer Işlem/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Ioreadbytespersecond  |  GÇ okuma bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  IoReadOperationsPerSecond  |  GÇ okuma Işlemi/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Iowritebytespersecond  |  GÇ yazma bayt/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  Iowriteoperationspersecond  |  GÇ Yazma Işlemi/saniye  |  BytesPerSecond  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  MemoryWorkingSet  |  Bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  PrivateBytes  |  Özel Baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  RequestsInApplicationQueue  |  Uygulama kuyruğundaki istekler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  İş Parçacıkları  |  İş Parçacığı Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  TotalAppDomains  |  Toplam uygulama etki alanları  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/sites/slots  |  TotalAppDomainsUnloaded  |  Toplam yüklenmeyen uygulama etki alanları  |  Sayı  |  Ortalama | 
