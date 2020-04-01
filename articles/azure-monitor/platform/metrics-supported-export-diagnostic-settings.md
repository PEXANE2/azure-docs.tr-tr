---
title: Azure Monitor platform ölçümleri Tanılama Ayarları ile dışa aktarılabilir
description: Azure Monitor ile her kaynak türü için kullanılabilen ölçümlerin listesi.
services: azure-monitor
ms.topic: reference
ms.date: 03/30/2020
ms.subservice: metrics
ms.openlocfilehash: 6be8cb1b7e74301d16a1174f5ca2b774334dac3f
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422119"
---
# <a name="azure-monitor-platform-metrics-exportable-via-diagnostic-settings"></a>Azure Monitor platform ölçümleri Tanılama Ayarları ile dışa aktarılabilir

Azure Monitor, yapılandırma olmadan varsayılan olarak [platform ölçümleri](data-platform-metrics.md) sağlar. Platform ölçümleri ile etkileşim de dahil olmak üzere portalda grafik, REST API üzerinden erişme veya PowerShell veya CLI kullanarak bunları sorgulama dahil olmak üzere çeşitli yollar sağlar. Azure Monitor'un konsolide metrik ardışık boru hattıyla şu anda kullanılabilen platform ölçümlerinin tam listesi için [desteklenen ölçümlere](metrics-supported.md) bakın. Bu ölçümleri sorgulamak ve bunlara erişmek için lütfen [2018-01-01 api sürümünü](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)kullanın. Diğer ölçümler portalda veya eski API'ler kullanılarak kullanılabilir.

Platform ölçümlerini Azure monitöründen diğer konumlara iki şekilde dışa aktarabilirsiniz.
1. Günlük Analizi, Etkinlik Hub'ları veya Azure Depolama'ya göndermek için [tanılama ayarlarını](diagnostic-settings.md) kullanma.
2. Ölçümleri kullanın [REST API](https://docs.microsoft.com/rest/api/monitor/metrics/list)

Azure Monitor arka ucundaki karışıklıklar nedeniyle, tüm ölçümler tanılama ayarları kullanılarak dışa aktarılamaz. Aşağıdaki tabloda tanılama ayarları kullanılarak dışa aktarılabilen ve veremeyen listelenmiştir.

## <a name="change-to-behavior-for-nulls-and-zero-values"></a>NULL'ler ve Sıfır değerleri için davranışdeğişikliği 
 
Tanılama ayarları yla dışa aktarılabilen platform ölçümleri için, Azure Monitor'un '0'ları 'Nulls' olarak yorumladığı birkaç ölçüm vardır. Bu gerçek '0s' (kaynak tarafından yayılan) ve '0s' (Nulls) yorumlanan arasında bazı karışıklığa neden oldu. Yakında bir değişiklik meydana gelecek ve tanılama ayarları üzerinden dışa aktarılan platform ölçümleri, temel kaynak tarafından gerçekten yayıldığı sürece artık '0'ları dışa aktarmayacaktır. Değişiklik 1 Nisan 2020'de planlandı, ancak COVID-19 nedeniyle öncelik değişimleri nedeniyle ertelendi. 

Lütfen unutmayın:

1.  Bir kaynak grubunu veya belirli bir kaynağı silerseniz, etkilenen kaynaklardan gelen metrik veriler artık tanılama verme hedeflerine gönderilmez. Diğer bir deyişle, artık Olay Hub'larında, Depolama Hesaplarında ve Günlük Analizi Çalışma Alanlarında görünmez.
2.  Bu gelişme tüm genel ve özel bulutlarda kullanılabilir olacaktır.
3.  Bu değişiklik, aşağıdaki deneyimlerden herhangi birinin davranışını etkilemez: 
   - Platform Kaynak Günlükleri Tanılama Ayarları ile dışa aktarıla
   - Ölçümler Gezgini'nde grafik ölçümleri
   - Platform ölçümleri hakkında uyarı
 
## <a name="metrics-exportable-table"></a>Dışa aktarılabilir ölçümler tablosu 

Tablo aşağıdaki sütunları içerir. 
- Tanılama Ayarları ile Dışa Aktarılabilir mi? 
- NULL tarafından etkilenir / 0 
- ResourceType 
- Ölçüm 
- MetricDisplayName
- Birim 
- Aggregationtype


> [!NOTE]
> Aşağıdaki tablonun alt kısmında yatay bir kaydırma çubuğu olabilir. Bilgilerin eksik olduğunu düşünüyorsanız, kaydırma çubuğunun tamamen sola doğru olup olmadığını kontrol edin.  


| Tanılama Ayarları ile Dışa Aktarılabilir mi?  | Zaten NULLs yontun |  ResourceType  |  Ölçüm  |  MetricDisplayName  |  Birim  |  Aggregationtype | 
|---|---| ---- | ----- | ------ | ---- | ---- | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  CleanerCurrentPrice  |  Bellek: Temiz Geçerli Fiyat  |  Sayı  |  Ortalama | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  TemizleyiciMemoryShrinkable  |  Bellek: Temiz Bellek küçültilemez  |  Bayt  |  Ortalama | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  TemizleyiciMemoryShrinkable  |  Bellek: Temiz Bellek küçültilebilir  |  Bayt  |  Ortalama | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  Komut HavuzuBusyThreads  |  İş parçacıkları: Komut havuzu meşgul konuları  |  Sayı  |  Ortalama | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  KomutPoolIdleThreads  |  İş parçacıkları: Komut havuzu boşta konuları  |  Sayı  |  Ortalama | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  CommandPoolJobQueueLength  |  Komut Havuzu İş Sıra Uzunluğu  |  Sayı  |  Ortalama | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  Güncel Bağlantılar  |  Bağlantı: Geçerli bağlantılar  |  Sayı  |  Ortalama | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  Geçerli Kullanıcı Oturumları  |  Geçerli Kullanıcı Oturumları  |  Sayı  |  Ortalama | 
| Evet****  | Hayır |  Microsoft.AnalysisServices/sunucular  |  LongParsingBusyThreads  |  İş parçacıkları: Uzun ayrışma meşgul konuları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  LongParsingIdleThreads  |  İş parçacıkları: Uzun ayrışma boşta iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  LongParsingJobQueueLength  |  İş parçacıkları: Uzun ayrışma iş sırası uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  mashup_engine_memory_metric  |  M Motor Belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  mashup_engine_private_bytes_metric  |  M Motoru Özel Bayt  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  mashup_engine_qpu_metric  |  M Motor QPU  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  mashup_engine_virtual_bytes_metric  |  M Motor Sanal Bayt  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  memory_metric  |  Bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  memory_thrashing_metric  |  Bellek Temizleme  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  MemoryLimitHard  |  Bellek: Bellek Sınırı Sabit  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  MemoryLimitHigh  |  Bellek: Bellek Sınırı Yüksek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  MemoryLimitLow  |  Bellek: Bellek Sınırı Düşük  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  MemoryLimitVertiPaq  |  Bellek: Bellek Limit VertiPaq  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  MemoryUsage  |  Bellek: Bellek Kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  private_bytes_metric  |  Özel Baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ProcessingPoolBusyIOJobThreads  |  İş parçacıkları: İşlem havuzu meşgul G/Ç iş iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ProcessingPoolBusyNonIOThreads  |  İş parçacıkları: İşlem havuzu meşgul olmayan G/Ç iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  İşlemePoolIdleIOJobThreads  |  İş parçacıkları: İşlem havuzu boşta G/Ç iş iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ProcessingPoolIdleNonIOThreads  |  İş parçacıkları: İşlem havuzu boşta Olmayan G/Ç iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ProcessingPoolioJobQueueLength  |  İş parçacıkları: İşlem havuzu G/Ç iş sırası uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ProcessingPoolJobQueueLength  |  İşlem havuzu Iş Sıra Uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  qpu_metric  |  QPU  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  QueryPoolBusyThreads  |  Sorgu Havuzu Meşgul Konuları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  QueryPoolIdleThreads  |  İş parçacıkları: Sorgu havuzu boşta konuları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  QueryPoolJobQueueLength  |  İş parçacıkları: Sorgu havuzu iş sırası uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  Kota  |  Bellek: Kota  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  Kota Engellendi  |  Bellek: Kota Engellendi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  SatırlarConvertedPerSec  |  İşleme: Satırlar saniye başına dönüştürülür  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  SatırlarReadPerSec  |  İşleme: Satırlar her saniye okunur  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  SatırlarWrittenPerSec  |  İşleme: Satırlar saniyebaşına yazılmış  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ShortParsingBusyThreads  |  İş parçacıkları: Kısa ayrışma meşgul konuları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ShortParsingIdleThreads  |  Konu: Kısa ayrışma boşta iş parçacıkları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ShortParsingJobQueueLength  |  İş parçacıkları: Kısa ayrışma iş sırası uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  BaşarılıConnectionsPerSec  |  Sn Başına Başarılı Bağlantılar  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  ToplamBağlantı Hataları  |  Toplam Bağlantı Hataları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  Toplam Bağlantı İstekleri  |  Toplam Bağlantı İstekleri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  VertiPaqNonpaged  |  Bellek: VertiPaq Nonpaged  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  VertiPaqPaged  |  Bellek: VertiPaq Paged  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AnalysisServices/sunucular  |  virtual_bytes_metric  |  Sanal Bayt Sayısı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Arka uçSüresi  |  Arka Uç İsteklerinin Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ApiManagement/service  |  Kapasite  |  Kapasite  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Süre  |  Ağ Geçidi İsteklerinin Genel Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubDroppedEtkinlikler  |  Bırakılan EventHub Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubRejectedOlaylar  |  Reddedilen EventHub Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubSuccessfulEvents  |  Başarılı EventHub Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  OlayHubThrottledEtkinlikler  |  Daraltılmış EventHub Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTimedoutEtkinlikler  |  Zamanlanmış OlayHub Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalBytesSent  |  EventHub Etkinliklerinin Boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalOlaylar  |  Toplam EventHub Etkinliği  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  EventHubTotalFailedEvents  |  Başarısız EventHub Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Başarısız İstekler  |  Başarısız Ağ Geçidi İstekleri (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Diğer İstekler  |  Diğer Ağ Geçidi İstekleri (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Başarılı İstekler  |  Başarılı Ağ Geçidi İstekleri (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  TotalRequests  |  Toplam Ağ Geçidi İsteği (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ApiManagement/service  |  Yetkisiz İstekler  |  Yetkisiz Ağ Geçidi İstekleri (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  AppCpuKullanım Yüzdesi  |  Uygulama CPU Kullanım Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  AppMemoryCommitted  |  Atanan Uygulama Belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  AppMemoryMax  |  Uygulama Bellek Max  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  AppMemoryUsed  |  Kullanılan Uygulama Belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  GCPauseToplam Sayısı  |  GC Duraklatma Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  GCPauseTotalTime  |  GC Duraklama Toplam Süresi  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  MaxOldGenMemoryPoolBytes  |  Maksimum Kullanılabilir Eski Nesil Veri Boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  OldGenMemoryPoolBytes  |  Eski Nesil Veri Boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  OldGenPromotedBytes  |  Eski Nesil Veri Boyutuna Tanıtın  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  SystemCpuKullanım Yüzdesi  |  Sistem CPU Kullanım Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatErrorCount  |  Tomcat Global Hatası  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatReceivedBytes  |  Tomcat Toplam Alınan Baytlar  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatRequestMaxTime  |  Tomcat İstek Max Zaman  |  Milisaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatRequestTotalSayısı  |  Tomcat İstek Toplam Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatRequestTotalTime  |  Tomcat İstek Toplam Kez  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatResponseAvgTime  |  Tomcat İstek Ortalama Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatSentBytes  |  Tomcat Toplam Gönderilen Bayt  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatSessionActiveCurrentCount  |  Tomcat Oturum Canlı Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatSessionActiveMaxCount  |  Tomcat Oturum Max Etkin Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatSessionAliveMaxTime  |  Tomcat Oturum Max Alive Time  |  Milisaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatSessionCreatedCount  |  Tomcat Oturum Oluşturulan Sayı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatSessionExpiredCount  |  Tomcat Oturumu Süresi Dolmuş Sayı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  TomcatSessionReddedildiSayısı  |  Tomcat Oturumu Reddedilen Sayı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.AppPlatform/Bahar  |  YoungGenPromotedBytes  |  Genç Nesil Veri Boyutuna Tanıtın  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Automation/automationHesapları  |  Toplam İş  |  Toplam İşler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Automation/automationHesapları  |  TotalUpdateDeploymentMachineRuns  |  Toplam Güncelleme Dağıtım Makinesi Çalışır  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Automation/automationHesapları  |  ToplamUpdateDeploymentRuns  |  Toplam Güncelleştirme Dağıtım Çalışır  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  Çekirdek Sayısı  |  Özel Çekirdek Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  OluşturmaNodeCount  |  Düğüm Sayısı Oluşturma  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  IdleNodeCount  |  Boşta Düğüm Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  JobDeleteCompleteOlay  |  İş Silme Etkinlikleri tamamlandı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  JobDeleteStartOlay  |  İş Silme Başlangıç Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  JobDisableCompleteEvent  |  İş Devre Dışı Tam Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  JobDisableStartEvent  |  İş Devre dışı Başlangıç Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  JobStartOlay  |  İş Başlangıç Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  JobTerminateCompleteOlay  |  İş Sonlandırma Etkinlikleri Tamamlandı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  JobTerminateStartOlay  |  İş Sonlandırma Başlangıç Olayları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  LeavingPoolNodeCount  |  Havuz Düğümü Sayısını Bırakma  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  Düşük ÖncelikliÇekirdek Sayısı  |  Düşük Öncelikli Çekirdek Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  ÇevrimdışıNodeCount  |  Çevrimdışı Düğüm Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  PoolCreateOlay  |  Havuz Oluşturma Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  PoolDeleteCompleteOlay  |  Havuz Silme Etkinlikleri Tamamla  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  PoolDeleteStartOlay  |  Havuz Silme Başlangıç Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  PoolResizeCompleteOlay  |  Havuz Resize Komple Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  PoolResizeStartEvent  |  Havuz Yeniden Boyutlandırma Etkinlikleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  PreemptedNodeCount  |  Preempted Düğüm Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  Yeniden BaşlatmaNodeCount  |  Düğüm Sayısını Yeniden Başlatma  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  ReimagingNodeCount  |  Regörüntüleme Düğümü Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  RunningNodeCount  |  Çalışan Düğüm Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  BaşlangıçNodeCount  |  Başlangıç Düğümü Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  BaşlangıçTaskFailedNodeCount  |  Görev Başarısız Düğüm Sayısını Başlat  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  Görev Tamamlama Etkinliği  |  Görev Tamamlama Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  TaskFailOlay  |  Görev Başarısız Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Batch/batchHesapları  |  TaskStartOlay  |  Görev Başlangıç Etkinlikleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  ToplamLowPriorityNodeCount  |  Düşük Öncelikli Düğüm Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  TotalNodeCount  |  Özel Düğüm Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  KullanılamazSayı  |  Kullanılamaz Düğüm Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Batch/batchHesapları  |  WaitingForStartTaskNodeCount  |  Başlangıç Görev Düğümü Sayısını Beklerken  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Aktif Çekirdekler  |  Aktif Çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Etkin Düğümler  |  Etkin Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  BoşTa Çekirdekler  |  BoşTa Çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Boşta Düğümler  |  Boşta Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Tamamlanan İş  |  Tamamlanan İş  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Gönderilen İş  |  Gönderilen İş  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Çekirdekleri Bırakma  |  Çekirdekleri Bırakma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Düğümleri Bırakma  |  Düğümleri Bırakma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Preempted Çekirdekler  |  Preempted Çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Preempted Düğümler  |  Preempted Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Kota Kullanım Yüzdesi  |  Kota Kullanım Yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Toplam Çekirdek  |  Toplam Çekirdek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Toplam Düğümler  |  Toplam Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Kullanılmaz Çekirdekler  |  Kullanılmaz Çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.BatchAI/çalışma alanları  |  Kullanılamaz Düğümler  |  Kullanılamaz Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  BağlantıKabul Edildi  |  Kabul Edilen Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  BağlantıAktif  |  Etkin Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  Bağlantı Ele  |  İşlenen Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  CpuUsagePercentageinDouble  |  CPU Kullanım Yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  IOReadBytes  |  IO Okuma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  IOWriteBytes  |  IO Bayt yaz  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  MemoryLimit  |  Bellek Sınırı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  MemoryUsage  |  Bellek Kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  MemoryUsagePercentageinDouble  |  Bellek Kullanım Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  Bekleyen İşlemler  |  Bekleyen İşlemler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  İşlenmiş Bloklar  |  İşlenmiş Bloklar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  İşlenmiş İşlemler  |  İşlenen İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  Sıraya Ait İşlemler  |  Sıraya Ait İşlemler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  İstek Ele  |  Ele Alınması İstekleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Blockchain/blockchainÜyeler  |  DepolamaKullanımı  |  Depolama Kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekler  |  Önbellek Hits  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek0  |  Önbellek Hits (Shard 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek1  |  Önbellek Hits (Shard 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek2  |  Önbellek Hits (Shard 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek3  |  Önbellek Hits (Shard 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek4  |  Önbellek Hits (Shard 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek5  |  Önbellek Hits (Shard 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek6  |  Önbellek Hits (Shard 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek7  |  Önbellek Hits (Shard 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek8  |  Önbellek Hits (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek9  |  Önbellek Hits (Shard 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekLatency  |  Önbellek Gecikme Mikrosaniye (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekler  |  Önbellek Misses  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekmisses0  |  Önbellek Misses (Shard 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekmisses1  |  Önbellek Misses (Shard 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekler2  |  Önbellek Misses (Shard 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek3  |  Önbellek Misses (Shard 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekler4  |  Önbellek Misses (Shard 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekler5  |  Önbellek Misses (Shard 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek6  |  Önbellek Misses (Shard 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekler7  |  Önbellek Misses (Shard 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellek8  |  Önbellek Misses (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekler9  |  Önbellek Misses (Shard 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekOku  |  Önbellek Okuma  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead0  |  Önbellek Okuma (Shard 0)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead1  |  Önbellek Okuma (Shard 1)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead2  |  Önbellek Okuma (Shard 2)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead3  |  Önbellek Okuma (Shard 3)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead4  |  Önbellek Okuma (Shard 4)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead5  |  Önbellek Okuma (Shard 5)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead6  |  Önbellek Okuma (Shard 6)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead7  |  Önbellek Okuma (Shard 7)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead8  |  Önbellek Okuma (Shard 8)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekRead9  |  Önbellek Okuma (Shard 9)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekYazma  |  Önbellek Yazma  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekYazma0  |  Önbellek Yazma (Shard 0)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekYazma1  |  Önbellek Yazma (Shard 1)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekWrite2  |  Önbellek Yazma (Shard 2)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekYazma3  |  Önbellek Yazma (Shard 3)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekWrite4  |  Önbellek Yazma (Shard 4)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekWrite5  |  Önbellek Yazma (Shard 5)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekWrite6  |  Önbellek Yazma (Shard 6)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekWrite7  |  Önbellek Yazma (Shard 7)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekWrite8  |  Önbellek Yazma (Shard 8)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  önbellekWrite9  |  Önbellek Yazma (Shard 9)  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler  |  Bağlanan İstemciler  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler0  |  Bağlı İstemciler (Shard 0)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler1  |  Bağlı İstemciler (Shard 1)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler2  |  Bağlı İstemciler (Shard 2)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler3  |  Bağlı İstemciler (Shard 3)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler4  |  Bağlı İstemciler (Shard 4)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler5  |  Bağlı İstemciler (Shard 5)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler6  |  Bağlı İstemciler (Shard 6)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler7  |  Bağlı İstemciler (Shard 7)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler8  |  Bağlı İstemciler (Shard 8)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  bağlı istemciler9  |  Bağlı İstemciler (Shard 9)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  hatalar  |  Hatalar  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları  |  Tahliye Edilen Anahtarlar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları0  |  Tahliye Edilen Anahtarlar (Shard 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları1  |  Tahliye Edilen Anahtarlar (Shard 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları2  |  Tahliye Edilen Anahtarlar (Shard 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları3  |  Tahliye Edilen Anahtarlar (Shard 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları4  |  Tahliye Edilen Anahtarlar (Shard 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları5  |  Tahliye Edilen Anahtarlar (Shard 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları6  |  Tahliye Edilen Anahtarlar (Shard 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları7  |  Tahliye Edilen Anahtarlar (Shard 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları8  |  Tahliye Edilen Anahtarlar (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  tahliye tuşları9  |  Tahliye Edilen Anahtarlar (Shard 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş anahtarlar  |  Süresi Dolmuş Anahtarlar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş tuşlar0  |  Süresi Dolmuş Anahtarlar (Shard 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş tuşlar1  |  Süresi Dolmuş Anahtarlar (Shard 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş tuşlar2  |  Süresi Dolmuş Anahtarlar (Shard 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş tuşlar3  |  Süresi Dolmuş Anahtarlar (Shard 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş tuşlar4  |  Süresi Dolmuş Anahtarlar (Shard 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş tuşlar5  |  Süresi Dolmuş Anahtarlar (Shard 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş tuşlar6  |  Süresi Dolmuş Anahtarlar (Shard 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş anahtarlar7  |  Süresi Dolmuş Anahtarlar (Shard 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş tuşlar8  |  Süresi Dolmuş Anahtarlar (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  süresi dolmuş anahtarlar9  |  Süresi Dolmuş Anahtarlar (Shard 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  komutları alma  |  Alır  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları0  |  Gets (Shard 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları1  |  Gets (Shard 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları2  |  Gets (Shard 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları3  |  Gets (Shard 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları4  |  Gets (Shard 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları5  |  Gets (Shard 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları6  |  Gets (Shard 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları7  |  Gets (Shard 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları8  |  Gets (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  getkomutları9  |  Gets (Shard 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond  |  Saniye başına işlemler  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond0  |  Saniye başına işlemler (Shard 0)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond1  |  Saniye başına işlemler (Shard 1)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond2  |  Saniye başına işlemler (Shard 2)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond3  |  Saniye başına işlemler (Shard 3)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond4  |  Saniye başına işlemler (Shard 4)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond5  |  Saniye başına işlemler (Shard 5)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond6  |  Saniye başına işlemler (Shard 6)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond7  |  Saniye başına operasyonlar (Shard 7)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond8  |  Saniye başına işlemler (Shard 8)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  operasyonlarPerSecond9  |  Saniye başına işlemler (Shard 9)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime  |  CPU  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime0  |  Cpu (Shard 0)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime1  |  Cpu (Shard 1)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime2  |  Cpu (Shard 2)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime3  |  Cpu (Shard 3)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime4  |  Cpu (Shard 4)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime5  |  Cpu (Shard 5)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime6  |  Cpu (Shard 6)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime7  |  Cpu (Shard 7)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime8  |  Cpu (Shard 8)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  yüzdeProcessorTime9  |  Cpu (Shard 9)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad  |  Sunucu Yükü  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad0  |  Sunucu Yükü (Shard 0)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad1  |  Sunucu Yükü (Shard 1)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad2  |  Sunucu Yükü (Shard 2)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad3  |  Sunucu Yükü (Shard 3)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad4  |  Sunucu Yükü (Shard 4)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad5  |  Sunucu Yükü (Shard 5)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad6  |  Sunucu Yükü (Shard 6)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad7  |  Sunucu Yükü (Shard 7)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad8  |  Sunucu Yükü (Shard 8)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  serverLoad9  |  Sunucu Yükü (Shard 9)  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları  |  Ayarlar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları0  |  Setler (Shard 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları1  |  Setler (Shard 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları2  |  Setler (Shard 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları3  |  Setler (Shard 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları4  |  Setler (Shard 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları5  |  Setler (Shard 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları6  |  Setler (Shard 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları7  |  Setler (Shard 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları8  |  Setler (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  setkomutları9  |  Setler (Shard 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalkomutlar işlendi  |  Toplam İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalcommandsprocessed0  |  Toplam İşlemler (Shard 0)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalkomutlar işlenmiş1  |  Toplam İşlemler (Shard 1)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalcommandsprocessed2  |  Toplam İşlemler (Shard 2)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalcommandsprocessed3  |  Toplam İşlemler (Shard 3)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalkomutlar işlenmiş4  |  Toplam İşlemler (Shard 4)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalcommandsişlenmiş5  |  Toplam İşlemler (Shard 5)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalkomutlar işlenmiş6  |  Toplam İşlemler (Shard 6)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalcommandsprocessed7  |  Toplam İşlemler (Shard 7)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalcommandsprocessed8  |  Toplam İşlemler (Shard 8)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  totalcommandsprocessed9  |  Toplam İşlemler (Shard 9)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar  |  Toplam Anahtar  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar0  |  Toplam Anahtar (Shard 0)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar1  |  Toplam Anahtar (Shard 1)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar2  |  Toplam Anahtar (Shard 2)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar3  |  Toplam Anahtar (Shard 3)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar4  |  Toplam Anahtar (Shard 4)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar5  |  Toplam Anahtar (Shard 5)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar6  |  Toplam Anahtar (Shard 6)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar7  |  Toplam Anahtar (Shard 7)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar8  |  Toplam Anahtar (Shard 8)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  toplam tuşlar9  |  Toplam Anahtar (Shard 9)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory  |  Kullanılmış Bellek  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory0  |  İkinci El Bellek (Shard 0)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory1  |  İkinci El Bellek (Shard 1)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory2  |  İkinci El Bellek (Shard 2)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory3  |  İkinci El Bellek (Shard 3)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory4  |  İkinci El Bellek (Shard 4)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory5  |  İkinci El Bellek (Shard 5)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory6  |  İkinci El Bellek (Shard 6)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory7  |  İkinci El Bellek (Shard 7)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory8  |  İkinci El Bellek (Shard 8)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemory9  |  İkinci El Bellek (Shard 9)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemorypercentage  |  Kullanılan Bellek Yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss  |  İkinci el bellek RSS  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss0  |  İkinci El Bellek RSS (Shard 0)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss1  |  İkinci El Bellek RSS (Shard 1)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss2  |  İkinci El Bellek RSS (Shard 2)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss3  |  İkinci El Bellek RSS (Shard 3)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss4  |  İkinci El Bellek RSS (Shard 4)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss5  |  İkinci El Bellek RSS (Shard 5)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss6  |  İkinci el Bellek RSS (Shard 6)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss7  |  İkinci el Bellek RSS (Shard 7)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss8  |  İkinci el Bellek RSS (Shard 8)  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Önbellek/redis  |  usedmemoryRss9  |  İkinci el Bellek RSS (Shard 9)  |  Bayt  |  Maksimum | 
| Hayır  | Hayır |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Okuma Bayt/Sn  |  Disk Okuma  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Okuma İşlemleri/Sn  |  Disk Okuma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Bayt/Sn Yazma  |  Disk Yazma  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Disk Yazma İşlemleri/Sn  |  Disk Yazma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/domainNames/slots/roles  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/domainNames/slots/roles  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicCompute/virtualMachines  |  Disk Okuma Bayt/Sn  |  Disk Okuma  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/virtualMachines  |  Disk Okuma İşlemleri/Sn  |  Disk Okuma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicCompute/virtualMachines  |  Disk Bayt/Sn Yazma  |  Disk Yazma  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/virtualMachines  |  Disk Yazma İşlemleri/Sn  |  Disk Yazma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/virtualMachines  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/virtualMachines  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicCompute/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageHesapları  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageHesapları  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageHesapları  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageHesapları  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageHesapları  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageHesapları  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageHesapları  |  Kullanılmış Kapasite  |  Kullanılmış kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  BlobKapasite  |  Blob Kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  BlobCount  |  Blob Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Konteyner Sayısı  |  Blob Konteyner Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Dizin Kapasitesi  |  Dizin Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/blobServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Dosya Kapasitesi  |  Dosya Kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Dosya Sayısı  |  Dosya Sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareCount  |  Dosya Paylaşım Sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareQuota  |  Dosya paylaşımı kota boyutu  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Dosya Paylaşımı Anlık Görüntü Sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Dosya Paylaşımı Anlık Görüntü Boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/fileServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Sıra Kapasitesi  |  Sıra Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  Sıra Sayısı  |  Sıra Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  QueueMessageCount  |  Sıra İleti Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/queueServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Tablo Kapasitesi  |  Tablo Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  Tablo Sayısı  |  Tablo Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  TabloEntityCount  |  Tablo Varlık Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ClassicStorage/storageAccounts/tableServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Engellenen Aramalar  |  Engellenen Aramalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  KarakterlerEğitilmiş  |  Karakterler Eğitilmiş  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  KarakterlerÇevrilen  |  Çevrilen Karakterler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  İstemci Hataları  |  İstemci Hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  DataIn  |  Veri In  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Veri Out  |  Veri Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Gecikme süresi  |  Gecikme süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Sunucu Hataları  |  Sunucu Hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Konuşma Süresi  |  Konuşma Oturumu Süresi  |  Saniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Başarılı Aramalar  |  Başarılı Aramalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Toplam Aramalar  |  Toplam Aramalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Toplam Hatalar  |  Toplam Hatalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  ToplamTokenAramalar  |  Toplam Belirteç Çağrıları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.CognitiveServices/hesapları  |  Toplam İşlemler  |  Toplam İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Cpu Kredileri Tüketilen  |  Cpu Kredileri Tüketilen  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  CPU Kredileri Kalan  |  CPU Kredileri Kalan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri Diski Kuyruk Derinliği  |  Veri DiskI Sıra Derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri Diski Okuma Bayt/sn  |  Veri Diski Okuma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri Diski Okuma İşlemleri/Sn  |  Veri Diski Okuma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri Diski Bayt/sn Yazma  |  Veri Diski Yazma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Veri Disk ihyası Işlemleri/Sn  |  Veri Disk ihyası işlemleri/sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Okuma Baytları  |  Disk Okuma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Okuma İşlemleri/Sn  |  Disk Okuma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Yazma Baytları  |  Disk Yazma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Yazma İşlemleri/Sn  |  Disk Yazma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Gelen Akışlar  |  Gelen Akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Gelen Akışlar Maksimum Oluşturma Oranı  |  Gelen Akışlar Maksimum Oluşturma Oranı (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Ağ Girişi  |  Faturalandırılabilir Ağ (Amortismana Tabi)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Toplam Ağ  |  Toplam Ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Ağ Çıkışı  |  Ağ Out Faturalanabilir (Deprecated)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Ağ Çıkış Toplamı  |  Ağ Çıkış Toplamı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim Sistemi Diski Kuyruk Derinliği  |  İşletim Sistemi Disk Sıra Derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim Sistemi Disk II Bayt/sn  |  İşletim Sistemi Disk Okuma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim Sistemi Disk Okuma İşlemleri/Sn  |  İşletim Sistemi Disk Okuma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim Sistemi Disk IYazma Bayt/sn  |  İşletim Sistemi Disk I Yazma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  İşletim Sistemi Disk Yazma İşlemleri/Sn  |  İşletim Sistemi Disk Yazma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına İşletim Sistemi QD  |  İşletim Sistemi Disk QD (Amortismana Uğradı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına İşletim Sistemi Okuma Bayt/sn  |  İşletim Sistemi Disk Okuma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına İşletim Sistemi Okuma İşlemleri/Sn  |  İşletim Sistemi Disk Okuma İşlemleri/Sn (Amortismana Uğradı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına İşletim Sistemi Bayt/sn Yazma  |  İşletim Sistemi Disk I Yazma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına İşletim Sistemi Yazma İşlemleri/Sn  |  İşletim Sistemi Disk Yazma İşlemleri/Sn (Amortismana Uğradı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Giden Akışlar  |  Giden Akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Giden Akışlar Maksimum Oluşturma Oranı  |  Giden Akışlar Maksimum Oluşturma Oranı (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına QD  |  Veri Diski QD (Deprecated)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına Okuma Bayt/sn  |  Veri Diski Okuma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına Okuma İşlemleri/Sn  |  Veri Diski Okuma İşlemleri/Sn (Amortismana Alındı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına Bayt/sn Yazma  |  Veri Diski Yazma Bayt/Sn (Amortismana Eritilmiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Disk Başına Yazma İşlemleri/Sn  |  Veri Diski Yazma İşlemleri/Sn (Amortismana Alındı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Premium Veri Disk Önbelleği Oku Hiti  |  Premium Veri Disk Önbelleği Oku Hiti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Premium Veri Disk Önbelleği Okuma Miss  |  Premium Veri Disk Önbelleği Okuma Miss (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Premium OS Disk Önbelleği Hit Oku  |  Premium İşletim Sistemi Disk Önbelleği Hit'i Oku (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachines  |  Premium OS Disk Önbelleği Okuma Miss  |  Premium İşletim Sistemi Disk Önbelleği Okuma Miss (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Cpu Kredileri Tüketilen  |  Cpu Kredileri Tüketilen  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  CPU Kredileri Kalan  |  CPU Kredileri Kalan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri Diski Kuyruk Derinliği  |  Veri DiskI Sıra Derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri Diski Okuma Bayt/sn  |  Veri Diski Okuma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri Diski Okuma İşlemleri/Sn  |  Veri Diski Okuma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri Diski Bayt/sn Yazma  |  Veri Diski Yazma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Veri Disk ihyası Işlemleri/Sn  |  Veri Disk ihyası işlemleri/sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Okuma Baytları  |  Disk Okuma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Okuma İşlemleri/Sn  |  Disk Okuma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Yazma Baytları  |  Disk Yazma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Yazma İşlemleri/Sn  |  Disk Yazma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Gelen Akışlar  |  Gelen Akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Gelen Akışlar Maksimum Oluşturma Oranı  |  Gelen Akışlar Maksimum Oluşturma Oranı (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Ağ Girişi  |  Faturalandırılabilir Ağ (Amortismana Tabi)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Toplam Ağ  |  Toplam Ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Ağ Çıkışı  |  Ağ Out Faturalanabilir (Deprecated)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Ağ Çıkış Toplamı  |  Ağ Çıkış Toplamı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim Sistemi Diski Kuyruk Derinliği  |  İşletim Sistemi Disk Sıra Derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim Sistemi Disk II Bayt/sn  |  İşletim Sistemi Disk Okuma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim Sistemi Disk Okuma İşlemleri/Sn  |  İşletim Sistemi Disk Okuma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim Sistemi Disk IYazma Bayt/sn  |  İşletim Sistemi Disk I Yazma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  İşletim Sistemi Disk Yazma İşlemleri/Sn  |  İşletim Sistemi Disk Yazma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına İşletim Sistemi QD  |  İşletim Sistemi Disk QD (Amortismana Uğradı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına İşletim Sistemi Okuma Bayt/sn  |  İşletim Sistemi Disk Okuma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına İşletim Sistemi Okuma İşlemleri/Sn  |  İşletim Sistemi Disk Okuma İşlemleri/Sn (Amortismana Uğradı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına İşletim Sistemi Bayt/sn Yazma  |  İşletim Sistemi Disk I Yazma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına İşletim Sistemi Yazma İşlemleri/Sn  |  İşletim Sistemi Disk Yazma İşlemleri/Sn (Amortismana Uğradı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Giden Akışlar  |  Giden Akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Giden Akışlar Maksimum Oluşturma Oranı  |  Giden Akışlar Maksimum Oluşturma Oranı (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına QD  |  Veri Diski QD (Deprecated)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına Okuma Bayt/sn  |  Veri Diski Okuma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına Okuma İşlemleri/Sn  |  Veri Diski Okuma İşlemleri/Sn (Amortismana Alındı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına Bayt/sn Yazma  |  Veri Diski Yazma Bayt/Sn (Amortismana Eritilmiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Disk Başına Yazma İşlemleri/Sn  |  Veri Diski Yazma İşlemleri/Sn (Amortismana Alındı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Premium Veri Disk Önbelleği Oku Hiti  |  Premium Veri Disk Önbelleği Oku Hiti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Premium Veri Disk Önbelleği Okuma Miss  |  Premium Veri Disk Önbelleği Okuma Miss (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Premium OS Disk Önbelleği Hit Oku  |  Premium İşletim Sistemi Disk Önbelleği Hit'i Oku (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets  |  Premium OS Disk Önbelleği Okuma Miss  |  Premium İşletim Sistemi Disk Önbelleği Okuma Miss (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Cpu Kredileri Tüketilen  |  Cpu Kredileri Tüketilen  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  CPU Kredileri Kalan  |  CPU Kredileri Kalan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri Diski Kuyruk Derinliği  |  Veri DiskI Sıra Derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri Diski Okuma Bayt/sn  |  Veri Diski Okuma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri Diski Okuma İşlemleri/Sn  |  Veri Diski Okuma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri Diski Bayt/sn Yazma  |  Veri Diski Yazma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Veri Disk ihyası Işlemleri/Sn  |  Veri Disk ihyası işlemleri/sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Okuma Baytları  |  Disk Okuma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Okuma İşlemleri/Sn  |  Disk Okuma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Yazma Baytları  |  Disk Yazma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Yazma İşlemleri/Sn  |  Disk Yazma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Gelen Akışlar  |  Gelen Akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Gelen Akışlar Maksimum Oluşturma Oranı  |  Gelen Akışlar Maksimum Oluşturma Oranı (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Ağ Girişi  |  Faturalandırılabilir Ağ (Amortismana Tabi)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Toplam Ağ  |  Toplam Ağ  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Ağ Çıkışı  |  Ağ Out Faturalanabilir (Deprecated)  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Ağ Çıkış Toplamı  |  Ağ Çıkış Toplamı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim Sistemi Diski Kuyruk Derinliği  |  İşletim Sistemi Disk Sıra Derinliği (Önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim Sistemi Disk II Bayt/sn  |  İşletim Sistemi Disk Okuma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim Sistemi Disk Okuma İşlemleri/Sn  |  İşletim Sistemi Disk Okuma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim Sistemi Disk IYazma Bayt/sn  |  İşletim Sistemi Disk I Yazma Bayt/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  İşletim Sistemi Disk Yazma İşlemleri/Sn  |  İşletim Sistemi Disk Yazma İşlemleri/Sn (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına İşletim Sistemi QD  |  İşletim Sistemi Disk QD (Amortismana Uğradı)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına İşletim Sistemi Okuma Bayt/sn  |  İşletim Sistemi Disk Okuma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına İşletim Sistemi Okuma İşlemleri/Sn  |  İşletim Sistemi Disk Okuma İşlemleri/Sn (Amortismana Uğradı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına İşletim Sistemi Bayt/sn Yazma  |  İşletim Sistemi Disk I Yazma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına İşletim Sistemi Yazma İşlemleri/Sn  |  İşletim Sistemi Disk Yazma İşlemleri/Sn (Amortismana Uğradı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Giden Akışlar  |  Giden Akışlar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Giden Akışlar Maksimum Oluşturma Oranı  |  Giden Akışlar Maksimum Oluşturma Oranı (Önizleme)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına QD  |  Veri Diski QD (Deprecated)  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına Okuma Bayt/sn  |  Veri Diski Okuma Bayt/Sn (Amortismana Ermiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına Okuma İşlemleri/Sn  |  Veri Diski Okuma İşlemleri/Sn (Amortismana Alındı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına Bayt/sn Yazma  |  Veri Diski Yazma Bayt/Sn (Amortismana Eritilmiş)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Disk Başına Yazma İşlemleri/Sn  |  Veri Diski Yazma İşlemleri/Sn (Amortismana Alındı)  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Premium Veri Disk Önbelleği Oku Hiti  |  Premium Veri Disk Önbelleği Oku Hiti (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Premium Veri Disk Önbelleği Okuma Miss  |  Premium Veri Disk Önbelleği Okuma Miss (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Premium OS Disk Önbelleği Hit Oku  |  Premium İşletim Sistemi Disk Önbelleği Hit'i Oku (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Compute/virtualMachineScaleSets/virtualMachines  |  Premium OS Disk Önbelleği Okuma Miss  |  Premium İşletim Sistemi Disk Önbelleği Okuma Miss (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ContainerInstance/containerGroups  |  CpuKullanımı  |  CPU Kullanımı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ContainerInstance/containerGroups  |  MemoryUsage  |  Bellek Kullanımı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ContainerInstance/containerGroups  |  AğBaytReceivedPerSecond  |  Ağ Baytları Saniyede Alındı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ContainerInstance/containerGroups  |  AğBaytTransmittedPerSecond  |  Saniyede Aktarılan Ağ Baytları  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ContainerRegistry/registries  |  Çalışma Süresi  |  Çalışma Süresi  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.ContainerRegistry/registries  |  Başarılı PullCount  |  Başarılı Çekme Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ContainerRegistry/registries  |  Başarılı PushCount  |  Başarılı Push Count  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ContainerRegistry/registries  |  Toplam PullCount  |  Toplam Çekme Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.ContainerRegistry/registries  |  Toplam PushCount Sayısı  |  Toplam İtme Sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ContainerService/managedClusters  |  kube_node_status_allocatable_cpu_cores  |  Yönetilen kümedeki kullanılabilir işlemci çekirdeğinin toplam sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ContainerService/managedClusters  |  kube_node_status_allocatable_memory_bytes  |  Yönetilen kümedeki kullanılabilir bellek toplam miktarı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ContainerService/managedClusters  |  kube_node_status_condition  |  Çeşitli düğüm koşulları için durumlar  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ContainerService/managedClusters  |  kube_pod_status_phase  |  Faza göre bölme sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ContainerService/managedClusters  |  kube_pod_status_ready  |  Hazır durumdaki bölme sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  Kullanılabilir Kapasite  |  Kullanılabilir Kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloud  |  Bulut Baytyüklenen (Aygıt)  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  BytesUploadedToCloudPerShare  |  Cloud Bayt Yüklendi (Paylaş)  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughput  |  Bulut İndirme İşi  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudReadThroughputPerShare  |  Bulut İndirme İşİ (Paylaş)  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughput  |  Bulut Yükleme İşi  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  CloudUploadThroughputPerShare  |  Bulut Yükleme İşI (Paylaş)  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  HyperVMemoryKullanımı  |  Kenar Hesaplama - Bellek Kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  HyperVVirtualProcessorKullanım  |  Kenar İşlemi - Yüzde Cpu  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  NICReadThroughput  |  Okuma Throughput (Ağ)  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  NICWriteThroughput  |  Yazma Throughput (Ağ)  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataBoxEdge/dataBoxEdgeDevices  |  Toplam Kapasite  |  Toplam Kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataFactory/datafactorys  |  Başarısız Çalıştırmalar  |  Başarısız Çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/datafactorys  |  Başarılı Çalışır  |  Başarılı Koşular  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  ActivityCancelledRuns  |  İptal edilen etkinlik ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  EtkinlikFailedRuns  |  Başarısız etkinlik ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  ActivitySucceededRuns  |  Başarılı etkinlik ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  Fabrikasizeingbunits  |  Toplam fabrika büyüklüğü (GB birimi)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  IntegrationRuntimeKullanılabilirBellek  |  Tümleştirme çalışma zamanı kullanılabilir bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  EntegrasyonRuntimeAverageTaskPickupDelay  |  Tümleştirme çalışma zamanı sıra süresi  |  Saniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  IntegrationRuntimeCpuPercentage  |  Entegrasyon çalışma zamanı CPU kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  EntegrasyonRuntimeQueueLength  |  Tümleştirme çalışma zamanı sıra uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  MaxallowedFactorySizeingbunits  |  İzin verilen maksimum fabrika boyutu (GB birimi)  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  MaxAllowedKaynak Sayısı  |  İzin verilen maksimum varlık sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  PipelineCancelledRuns  |  İptal edilen ardışık hatlar ölçümleri çalışır  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  Boru HattıFailedRuns  |  Başarısız ardışık hatlar ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  PipelineSucceededRuns  |  Başarılı ardışık ardışık hatlar ölçümleri çalışır  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  Kaynak Sayısı  |  Toplam varlıklar sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  TriggerCancelledRuns  |  İptal edilen tetikleyici ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  TriggerFailedRuns  |  Başarısız tetikleyici ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataFactory/fabrikalar  |  TriggerSucceededRuns  |  Başarılı tetikleyici ölçümleri çalıştırıyor  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/hesapları  |  JobAUEndedİptal edildi  |  İptal EDILEN AU Saati  |  Saniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/hesapları  |  JobAUEndedFailure  |  Başarısız AU Süresi  |  Saniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/hesapları  |  JobAUEndedSuccess  |  Başarılı AU Time  |  Saniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/hesapları  |  İşSon İptal Edildi  |  İptal Edilen İşler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/hesapları  |  İş Sonu Hatası  |  Başarısız İşler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeAnalytics/hesapları  |  JobEndedSuccess  |  Başarılı İşler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/hesapları  |  Veri Okuma  |  Veri Okuma  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/hesapları  |  Veri Yazısı  |  Yazılı Veriler  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/hesapları  |  Okuma İstekleri  |  İstekleri Okuma  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/hesapları  |  Toplam Depolama  |  Toplam Depolama Alanı  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DataLakeStore/hesapları  |  Yazma İstekleri  |  İstek Yazma  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  active_connections  |  Etkin Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  backup_storage_used  |  Yedek Depolama kullanılan  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  connections_failed  |  Başarısız Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  io_consumption_percent  |  IO yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  seconds_behind_master  |  Saniye cinsinden çoğaltma gecikmesi  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  serverlog_storage_limit  |  Sunucu Günlüğü depolama sınırı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  serverlog_storage_percent  |  Sunucu Günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  serverlog_storage_usage  |  Kullanılan Server Log depolama  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  storage_percent  |  Depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMariaDB/sunucular  |  storage_used  |  Kullanılan depolama  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  active_connections  |  Etkin Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  backup_storage_used  |  Yedek Depolama kullanılan  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  connections_failed  |  Başarısız Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  io_consumption_percent  |  IO yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  seconds_behind_master  |  Saniye cinsinden çoğaltma gecikmesi  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  serverlog_storage_limit  |  Sunucu Günlüğü depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  serverlog_storage_percent  |  Sunucu Günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  serverlog_storage_usage  |  Kullanılan Server Log depolama  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  storage_percent  |  Depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DBforMySQL/sunucular  |  storage_used  |  Kullanılan depolama  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  active_connections  |  Etkin Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  backup_storage_used  |  Yedek Depolama kullanılan  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  connections_failed  |  Başarısız Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  io_consumption_percent  |  IO yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  pg_replica_log_delay_in_bytes  |  Çoğaltmalar Arasında Max Lag  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  pg_replica_log_delay_in_seconds  |  Çoğaltma Gecikmesi  |  Saniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  serverlog_storage_limit  |  Sunucu Günlüğü depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  serverlog_storage_percent  |  Sunucu Günlüğü depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  serverlog_storage_usage  |  Kullanılan Server Log depolama  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  storage_limit  |  Depolama sınırı  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  storage_percent  |  Depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/sunucular  |  storage_used  |  Kullanılan depolama  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/serversv2  |  active_connections  |  Etkin Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/serversv2  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/serversv2  |  ıop  |  IOPS  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/serversv2  |  memory_percent  |  Bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/serversv2  |  network_bytes_egress  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/serversv2  |  network_bytes_ingress  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/serversv2  |  storage_percent  |  Depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.DBforPostgreSQL/serversv2  |  storage_used  |  Kullanılan depolama  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/Account  |  digitaltwins.telemetri.nodes  |  Dijital İkizler Düğümü Telemetri Yer Tutucu  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.abandon.success  |  C2D iletileri terk edildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.complete.success  |  C2D ileti teslimatları tamamlandı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.commands.egress.reject.success  |  C2D iletileri reddedildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.methods.failure  |  Başarısız doğrudan yöntem çağrıları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.methods.requestSize  |  Doğrudan yöntem çağrılarının istek boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.methods.responseSize  |  Doğrudan yöntem çağrılarının yanıt boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.methods.success  |  Başarılı doğrudan yöntem çağrıları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.failure  |  Başarısız ikiz arka uçtan okur  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.size  |  İkiz okumanın yanıt boyutu arka uçtan okur  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.twin.read.success  |  Başarılı ikiz arka uçtan okur  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.failure  |  Arka uçtan başarısız ikiz güncelleştirmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.size  |  Arka uçtan ikiz güncellemeleriboyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  c2d.twin.update.success  |  Arka uçtan başarılı ikiz güncellemeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  C2DMesajlarSüresi Doldu  |  C2D İletilerinin Süresi Doldu (önizleme)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  Yapılandırma  |  Yapılandırma Ölçümleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Devices/IotHubs  |  bağlıCihaz Sayısı  |  Bağlı aygıtlar (önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.builtIn.events  |  Yönlendirme: iletilere/etkinliklere iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.eventHubs  |  Yönlendirme: Event Hub'a iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.serviceBusQueues  |  Yönlendirme: Servis Veri Servisi Kuyruğuna gönderilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.serviceBusTopics  |  Yönlendirme: Servis Veri Servisi Konusuna iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.depolama  |  Yönlendirme: depolama alanına teslim edilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage.blobs  |  Yönlendirme: depoya teslim edilen lekeler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.egress.storage.bytes  |  Yönlendirme: depolamaya teslim edilen veriler  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.builtIn.events  |  Yönlendirme: iletiler/olaylar için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.eventHubs  |  Yönlendirme: Olay Hub'ı için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.serviceBusQueues  |  Yönlendirme: Servis Veri Servisi Sırası için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.serviceBusTopics  |  Yönlendirme: Servis Veri Servisi Konusu için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.endpoints.latency.storage  |  Yönlendirme: depolama için ileti gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.düştü  |  Yönlendirme: telemetri iletileri düştü   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.fallback  |  Yönlendirme: geri dönüşe teslim edilen iletiler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.çıkış.geçersiz  |  Yönlendirme: telemetri iletileri uyumsuz  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.orphaned  |  Yönlendirme: telemetri mesajları öksüz   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.telemetry.egress.success  |  Yönlendirme: telemetri iletileri teslim  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.telemetri.ingress.allProtocol  |  Telemetri iletisi gönderme girişimleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.telemetri.ingress.sendThrottle  |  Azaltma hatalarının sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.telemetri.ingress.success  |  Gönderilen Telemetri iletileri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.failure  |  Cihazlardan başarısız ikiz okuma  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.size  |  Cihazlardan gelen ikiz okumaların yanıt boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.twin.read.success  |  Cihazlardan başarılı ikiz okuma  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.failure  |  Aygıtlardan başarısız ikiz güncelleştirmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.size  |  Cihazlardan gelen ikiz güncellemelerin boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  d2c.twin.update.success  |  Cihazlardan başarılı ikiz güncellemeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  dailyMessageQuotaUsed  |  Kullanılan toplam ileti sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  deviceDataUsage  |  Toplam cihaz veri kullanımı  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  cihazDataUsageV2  |  Toplam cihaz veri kullanımı (önizleme)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  devices.connectedDevices.allProtocol  |  Bağlı aygıtlar (amortismana uğradı)   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  devices.totalCihazlar  |  Toplam aygıtlar (amortismana uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  EventGridDeliveries  |  Olay Izgara teslimatları(önizleme)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  EventGridLatency  |  Olay Izgara gecikmesi (önizleme)  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.cancelJob.failure  |  Başarısız iş iptalleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.cancelJob.success  |  Başarılı iş iptalleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.completed  |  Tamamlanan işler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.createDirectMethodJob.failure  |  Yöntem çağırma işlerinin başarısız oluşturmaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.createDirectMethodJob.success  |  Yöntem çağırma işlerinin başarılı oluşturulması  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.createTwinUpdateJob.failure  |  İkiz güncelleştirme işlerinin başarısız oluşturmaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.createTwinUpdateJob.success  |  İkiz güncelleştirme işlerinin başarılı oluşturmaları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.failed  |  Başarısız işler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.listJobs.failure  |  İşleri listelemek için başarısız çağrılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.listJobs.success  |  İşleri listelemek için başarılı çağrılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.queryJobs.failure  |  Başarısız iş sorguları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  jobs.queryJobs.success  |  Başarılı iş sorguları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Devices/IotHubs  |  totalDeviceCount  |  Toplam aygıtlar (önizleme)  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  twinQueries.failure  |  Başarısız çift sorgular  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  twinQueries.resultSize  |  İkiz sorgular sonuç boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Devices/IotHubs  |  twinQueries.success  |  Başarılı ikiz sorgular  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/provisioningServices  |  AttestationGirişimleri  |  Attestation girişimleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/provisioningServices  |  Aygıt Atamaları  |  Atanan aygıtlar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Devices/provisioningServices  |  Kayıt Denemeleri  |  Kayıt denemeleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  Kullanılabilir Depolama  |  Kullanılabilir Depolama  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  CassandraConnectionClosures  |  Cassandra Bağlantı Kapakları  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  CassandraRequestÜcretleri  |  Cassandra İstek Ücretleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  Cassandraİstek'ler  |  Cassandra İstekleri  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  VeriKullanımı  |  Veri Kullanımı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  SilmeVirtualNetwork  |  SilmeVirtualNetwork  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  Belge Sayısı  |  Belge Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  BelgeKota  |  Belge Kotası  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  IndexKullanım  |  Dizin Kullanımı  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  Meta dataRequests  |  Meta veri istekleri  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft.DocumentDB/databaseHesapları  |  MongoRequestCharge  |  Mongo İstek Ücreti  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.DocumentDB/databaseHesapları  |  Mongoİstek'ler  |  Mongo İstekleri  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  MongoRequestsCount  |  Mongo İstek Oranı  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  MongoRequestsDelete  |  Mongo Silme İstek Oranı  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  MongoRequestsInsert  |  Mongo Ekle İstek Oranı  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  MongoRequestsQuery  |  Mongo Sorgu İstek Oranı  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  MongoRequestsUpdate  |  Mongo Güncelleme İstek Oranı  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  ProvisionedThroughput  |  Sağlanan İşleme Hızı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  ReplicationLatency  |  P99 Çoğaltma Gecikmesi  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.DocumentDB/databaseHesapları  |  Servis Kullanılabilirliği  |  Hizmet Kullanılabilirliği  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.DocumentDB/databaseHesapları  |  TotalRequests  |  Toplam İstek Sayısı  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft.DocumentDB/databaseHesapları  |  Toplam İstek Birimleri  |  Toplam İstek Birimleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EnterpriseKnowledgeGraph/hizmetleri  |  Hata Sayısı  |  Hata Sayısı  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft.EnterpriseKnowledgeGraph/hizmetleri  |  Başarı Sayısı  |  Başarı Sayısı  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft.EnterpriseKnowledgeGraph/hizmetleri  |  SuccessLatency  |  Başarı Gecikmesi  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EnterpriseKnowledgeGraph/hizmetleri  |  İşlem Sayısı  |  İşlem Sayısı  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/etki alanları  |  DeadLetteredSayısı  |  Ölü Harfli Etkinlikler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventGrid/etki alanları  |  TeslimatGirişimiFailCount  |  Teslim Başarısız Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/etki alanları  |  TeslimatBaşarı Sayısı  |  Teslim Etkinlikleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventGrid/etki alanları  |  Hedef İşlemlerDurationinms  |  Hedef İşlem Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/etki alanları  |  BırakılanEventCount  |  Bırakılan Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/etki alanları  |  EşleşenEventSayısı  |  Eşleşen Etkinlikler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/etki alanları  |  YayınFailCount  |  Başarısız Olayları Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/etki alanları  |  YayınBaşarı Sayısı  |  Yayınlanan Etkinlikler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/etki alanları  |  YayınBaşarıLatencyInMs  |  Başarı Gecikmesi Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/eventAbonelikler  |  DeadLetteredSayısı  |  Ölü Harfli Etkinlikler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventGrid/eventAbonelikler  |  TeslimatGirişimiFailCount  |  Teslim Başarısız Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/eventAbonelikler  |  TeslimatBaşarı Sayısı  |  Teslim Etkinlikleri  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventGrid/eventAbonelikler  |  Hedef İşlemlerDurationinms  |  Hedef İşlem Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/eventAbonelikler  |  BırakılanEventCount  |  Bırakılan Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/eventAbonelikler  |  EşleşenEventSayısı  |  Eşleşen Etkinlikler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/extensionTopics  |  YayınFailCount  |  Başarısız Olayları Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/extensionTopics  |  YayınBaşarı Sayısı  |  Yayınlanan Etkinlikler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/extensionTopics  |  YayınBaşarıLatencyInMs  |  Başarı Gecikmesi Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/extensionTopics  |  EşleşmemişEventSayısı  |  Eşsiz Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/konular  |  YayınFailCount  |  Başarısız Olayları Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/konular  |  YayınBaşarı Sayısı  |  Yayınlanan Etkinlikler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/konular  |  YayınBaşarıLatencyInMs  |  Başarı Gecikmesi Yayımla  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventGrid/konular  |  EşleşmemişEventSayısı  |  Eşsiz Olaylar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  Aktif Bağlantılar  |  Aktif Bağlantılar  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  Kullanılabilir Bellek  |  Kullanılabilir Bellek  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  CaptureBacklog  |  Biriktirme Listesini Yakalayın.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  Yakalanan Baytlar  |  Ele geçirilen Baytlar.  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  Yakalanan Mesajlar  |  Yakalanan Mesajlar.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  BağlantılarKapalı  |  Bağlantılar Kapalı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  BağlantılarAçıldı  |  Bağlantılar Açıldı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  CPU  |  CPU  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.EventHub/kümeleri  |  Gelen Baytlar  |  Gelen Baytlar.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/kümeleri  |  Gelen Mesajlar  |  Gelen Mesajlar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/kümeleri  |  Gelen İstekler  |  Gelen İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/kümeleri  |  Giden Baytlar  |  Giden Baytlar.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/kümeleri  |  Giden Mesajlar  |  Giden Mesajlar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  KotaAşıldı Hatalar  |  Kota Hataları Aştı.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  Sunucu Hataları  |  Sunucu Hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  Başarılı İstekler  |  Başarılı İstekler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  OperationName  |  Daraltılmış İstekler.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/kümeleri  |  Kullanıcı Hataları  |  Kullanıcı Hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  Aktif Bağlantılar  |  Aktif Bağlantılar  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  CaptureBacklog  |  Biriktirme Listesini Yakalayın.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  Yakalanan Baytlar  |  Ele geçirilen Baytlar.  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  Yakalanan Mesajlar  |  Yakalanan Mesajlar.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  BağlantılarKapalı  |  Bağlantılar Kapalı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  BağlantılarAçıldı  |  Bağlantılar Açıldı.  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHABL  |  Biriktirme listesi iletilerini arşivle (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHAMBS  |  Arşiv iletisi girişi (Amortismana uğradı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHAMSGS  |  Arşiv iletileri (Amortismana uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHINBYTES  |  Gelen baytlar (Amortismana uğradı)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHINMBS  |  Gelen baytlar (eski) (Amortismana ermiş)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHINMSGS  |  Gelen İletiler (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHOUTBYTES  |  Giden baytlar (Deprecated)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHOUTMBS  |  Giden baytlar (eski) (Amortismana ermiş)  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  EHOUTMSGS  |  Giden İletiler (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  FAILREQ  |  Başarısız İstekler (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Gelen Baytlar  |  Gelen Baytlar.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Gelen Mesajlar  |  Gelen Mesajlar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Gelen İstekler  |  Gelen İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  INMSGS  |  Gelen İletiler (eski) (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  INREQS  |  Gelen İstekler (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  INTERR  |  Dahili Sunucu Hataları (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  MISCERR  |  Diğer Hatalar (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Giden Baytlar  |  Giden Baytlar.  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  Giden Mesajlar  |  Giden Mesajlar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  OUTMSGS  |  Giden İletiler (eski) (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  KotaAşıldı Hatalar  |  Kota Hataları Aştı.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  Sunucu Hataları  |  Sunucu Hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  Boyut  |  Boyut  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  Başarılı İstekler  |  Başarılı İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  SUCCREQ  |  Başarılı İstekler (Amortismana Uğradı)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.EventHub/namespaces  |  SVRBSY  |  Sunucu Meşgul Hataları (Deprecated)  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  OperationName  |  Daraltılmış İstekler.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.EventHub/namespaces  |  Kullanıcı Hataları  |  Kullanıcı Hataları.  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.HDInsight/kümeler  |  KategorizeAğ İstekleri  |  Kategorize Ağ Geçidi İstekleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.HDInsight/kümeler  |  Ağ Geçidi İstekleri  |  Ağ Geçidi İstekleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.HDInsight/kümeler  |  NumActiveWorkers  |  Aktif Çalışan Sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.HDInsight/kümeler  |  Ölçekleme İstekleri  |  İstekleri ölçekleme  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Insights/Otomatik Ölçeklendirme Ayarları  |  Metrik Eşik  |  Metrik Eşik  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Otomatik Ölçeklendirme Ayarları  |  Gözlemlenen Kapasite  |  Gözlenen Kapasite  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Otomatik Ölçeklendirme Ayarları  |  Gözlenen Metrik Değer  |  Gözlenen Metrik Değer  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Insights/Otomatik Ölçeklendirme Ayarları  |  ScaleActionsBaşlatılan  |  Başlatılan Ölçeklendirme Eylemleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  availabilitySonuçlar/kullanılabilirlikPercentage  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Insights/Components  |  availabilitySonuçlar/sayım  |  Kullanılabilirlik testleri  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  availabilitySonuçlar/süre  |  Kullanılabilirlik test süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  browserTimings/networkDuration  |  Sayfa yükleme ağı bağlama süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  browserTimings/processingDuration  |  İstemci işleme süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  browserTimings/receiveDuration  |  Yanıt süresi alma  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  browserTimings/sendDuration  |  İstek süresi gönder  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  browserZamanlamas/totalDuration  |  Tarayıcı sayfası yükleme süresi  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Insights/Components  |  bağımlılıklar/sayım  |  Bağımlılık çağrıları  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  bağımlılıklar/süre  |  Bağımlılık süresi  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Insights/Components  |  bağımlılıklar/başarısız  |  Bağımlılık çağrı hataları  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft.Insights/Components  |  özel durumlar/tarayıcı  |  Tarayıcı özel durumları  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft.Insights/Components  |  özel durumlar/sayım  |  Özel durumlar  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft.Insights/Components  |  özel durumlar/sunucu  |  Sunucu özel durumları  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft.Insights/Components  |  sayfaGörünümler/sayım  |  Sayfa görünümleri  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  sayfaGörünümler/süre  |  Sayfa görünümü yükleme süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Insights/Components  |  performanceCounters/exceptionsPerSecond  |  Özel durum oranı  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  performanceSayaçlar/memoryAvailableBytes  |  Kullanılabilir bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  performanceSayaçlar/prosesCpuPercentage  |  İşlem CPU'su  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  performansSayaçlar/processIOBytesPerSecond  |  Proses IO oranı  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  performanceSayaçlar/işlemciCpuPercentage  |  İşlemci süresi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  performanceSayaçlar/prosesPrivateBytes  |  Özel baytları işleme  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  performanceCounters/requestExecutionTime  |  HTTP yürütme süresi isteği  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Insights/Components  |  performanceCounters/requestsInQueue  |  Uygulama kuyruğundaki HTTP istekleri  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Insights/Components  |  performanceCounters/requestsPerSecond  |  HTTP istek oranı  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Insights/Components  |  istekleri/sayısı  |  Sunucu istekleri  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.Insights/Components  |  istekler/süre  |  Sunucu yanıt süresi  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Insights/Components  |  istekleri/başarısız  |  Başarısız istekler  |  Sayı  |  Sayı | 
| Hayır  | Hayır |  Microsoft.Insights/Components  |  istekler/oran  |  Sunucu istek oranı  |  CountPerSecond  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Insights/Components  |  izler/sayım  |  İzlemeler  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.KeyVault/vaults  |  ServiceApiHit  |  Toplam Servis Api Hits  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.KeyVault/vaults  |  ServiceApiLatency  |  Genel Hizmet Api Gecikme  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.KeyVault/vaults  |  ServiceApiResult  |  Toplam Hizmet Api Sonuçları  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  ÖnbellekKullanımı  |  Önbellek kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  ContinuousExportMaxLatenessMinutes  |  Sürekli İhracat Max Gecikme Dakikası  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  SürekliExportNumOfRecordsExported  |  Sürekli ihracat - ihraç edilen kayıtların num  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  SürekliExportPendingCount  |  Sürekli İhracat Bekleyen Sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  Sürekli İhracat Sonucu  |  Sürekli İhracat Sonucu  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  CPU  |  CPU  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  EtkinliklerProcessedForEventHubs  |  İşlenen olaylar (Olay/IoT Hub'ları için)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  İhracatKullanım  |  İhracat Kullanımı  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  IngestionLatencyInSeconds  |  Yutma gecikmesi (saniye cinsinden)  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  Yutma Sonucu  |  Yutma sonucu  |  Sayı  |  Sayı | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  YutmaKullanımı  |  Yutma kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  Yutma HacimInMB  |  Yutma hacmi (MB'de)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  Keepalive  |  Canlı tutun  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  Sorgu Süresi  |  Sorgu süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  SteamingIngestRequestRate  |  Akış Alma İstek Oranı  |  Sayı  |  RateRequestsPerSecond | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  StreamingingestDataRate  |  Veri Hızı Akış  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  StreamingingestDuration  |  Akış Alma Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Kusto/Kümeler  |  StreamingingestSonuçlar  |  Akış Ingest Sonucu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  ActionLatency  |  Eylem Gecikme   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  Tamamlanan İşlemler  |  Tamamlanan İşlemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  EylemlerBaşarısız  |  Başarısız Olan Eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  EylemlerAtlatı  |  Atlanan Eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  Eylemler Başlatıldı  |  Eylemler Başladı   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  İşlemlerBaşarılı  |  Başarılı Olan Eylemler   |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  ActionSuccessLatency  |  Eylem Başarı Gecikme   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  ActionThrottledEtkinlikler  |  Eylem Daraltılmış Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  EntegrasyonServiceEnvironmentConnectorMemoryUsage  |  Entegrasyon Hizmet Ortamı için Bağlayıcı Bellek Kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  EntegrasyonServiceEnvironmentConnectorProcessorUsage  |  Entegrasyon Hizmet Ortamı için Bağlayıcı İşlemci Kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  EntegrasyonServiceEnvironmentWorkflowMemoryUsage  |  Entegrasyon Hizmet Ortamı için İş Akışı Bellek Kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  EntegrasyonServiceEnvironmentWorkflowProcessorUsage  |  Entegrasyon Hizmet Ortamı için İş Akışı İşlemci Kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  RunFailure Yüzdesi  |  Çalıştır Hatası Yüzdesi  |  Yüzde  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  RunLatency  |  Run Gecikme  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  Runsİptal edildi  |  İptal Edilen Çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  Tamamlanan Çalıştırmalar  |  Tamamlanan Çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsFailed  |  Çalıştırmalar Başarısız Oldu  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  Çalıştırmalar Başladı  |  Çalıştırmalar Başladı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  RunsBaşarılı  |  Başarılı Çalışır  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  RunStartThrottledEtkinlikler  |  Çalıştır Başlangıç Daraltılmış Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  RunSuccessLatency  |  Run Başarı Gecikmesi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  RunThrottledEtkinlikler  |  Daraltılmış Olayları Çalıştır  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerFireLatency  |  Tetik Yangın Gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerLatency  |  Tetik Gecikme   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  TetikleyicilerTamamlandı  |  Tamamlanan Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  TetikleyicilerBaşarısız  |  Tetikleyiciler Başarısız Oldu   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  TetikleyicilerAteşlendi  |  Ateşlenen Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  TetikleyicilerAtlanır  |  Atlanan Tetikleyiciler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  TetikleyicilerBaşlatıldı  |  Tetikleyiciler Başladı   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  TetikleyicilerBaşarılı  |  Başarılı Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerSuccessLatency  |  Tetik Başarı Gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/integrationServiceEnvironments  |  TriggerThrottledOlaylar  |  Tetikleyici Daraltılmış Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/iş akışları  |  ActionLatency  |  Eylem Gecikme   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  Tamamlanan İşlemler  |  Tamamlanan İşlemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  EylemlerBaşarısız  |  Başarısız Olan Eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  EylemlerAtlatı  |  Atlanan Eylemler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  Eylemler Başlatıldı  |  Eylemler Başladı   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  İşlemlerBaşarılı  |  Başarılı Olan Eylemler   |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/iş akışları  |  ActionSuccessLatency  |  Eylem Başarı Gecikme   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  ActionThrottledEtkinlikler  |  Eylem Daraltılmış Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  FaturalanabilirEylemYürütmeler  |  Faturalandırılabilir Eylem Yürütmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  FaturalandırılabilirTetikleyiciLer  |  Faturalandırılabilir Tetikleyici Yürütmeler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  FaturalamaKullanımıNativeOperation  |  Yerel İşlem Yürütmeleri için FaturaLandırma Kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  FaturalamaKullanımıNativeOperation  |  Yerel İşlem Yürütmeleri için FaturaLandırma Kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  FaturalamaKullanımStandartBağlantı  |  Standart Bağlayıcı Yürütmeler için Faturalandırma Kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  FaturalamaKullanımStandartBağlantı  |  Standart Bağlayıcı Yürütmeler için Faturalandırma Kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  FaturalamaKullanımStorageConsumption  |  Depolama Tüketim Uygulamaları için Faturalandırma Kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  FaturalamaKullanımStorageConsumption  |  Depolama Tüketim Uygulamaları için Faturalandırma Kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  RunFailure Yüzdesi  |  Çalıştır Hatası Yüzdesi  |  Yüzde  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/iş akışları  |  RunLatency  |  Run Gecikme  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  Runsİptal edildi  |  İptal Edilen Çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  Tamamlanan Çalıştırmalar  |  Tamamlanan Çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  RunsFailed  |  Çalıştırmalar Başarısız Oldu  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  Çalıştırmalar Başladı  |  Çalıştırmalar Başladı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  RunsBaşarılı  |  Başarılı Çalışır  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  RunStartThrottledEtkinlikler  |  Çalıştır Başlangıç Daraltılmış Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/iş akışları  |  RunSuccessLatency  |  Run Başarı Gecikmesi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  RunThrottledEtkinlikler  |  Daraltılmış Olayları Çalıştır  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  Toplam Faturalı Yürütmeler  |  Toplam Faturalandırılabilir Yürütmeler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/iş akışları  |  TriggerFireLatency  |  Tetik Yangın Gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Logic/iş akışları  |  TriggerLatency  |  Tetik Gecikme   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  TetikleyicilerTamamlandı  |  Tamamlanan Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  TetikleyicilerBaşarısız  |  Tetikleyiciler Başarısız Oldu   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  TetikleyicilerAteşlendi  |  Ateşlenen Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  TetikleyicilerAtlanır  |  Atlanan Tetikleyiciler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  TetikleyicilerBaşlatıldı  |  Tetikleyiciler Başladı   |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  TetikleyicilerBaşarılı  |  Başarılı Tetikleyiciler   |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Logic/iş akışları  |  TriggerSuccessLatency  |  Tetik Başarı Gecikmesi   |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Logic/iş akışları  |  TriggerThrottledOlaylar  |  Tetikleyici Daraltılmış Olaylar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Aktif Çekirdekler  |  Aktif Çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Etkin Düğümler  |  Etkin Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Tamamlanan Çalıştırmalar  |  Tamamlanan Çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Başarısız Çalıştırmalar  |  Başarısız Çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  BoşTa Çekirdekler  |  BoşTa Çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Boşta Düğümler  |  Boşta Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Çekirdekleri Bırakma  |  Çekirdekleri Bırakma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Düğümleri Bırakma  |  Düğümleri Bırakma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Model Dağıtma Başarısız Oldu  |  Model Dağıtma Başarısız Oldu  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Model Dağıtımı Başladı  |  Model Dağıtımı Başladı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Model Dağıtma Başarılı  |  Model Dağıtma Başarılı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Model Kaydı Başarısız Oldu  |  Model Kaydı Başarısız Oldu  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Model Kaydı Başarılı  |  Model Kaydı Başarılı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Preempted Çekirdekler  |  Preempted Çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Preempted Düğümler  |  Preempted Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Kota Kullanım Yüzdesi  |  Kota Kullanım Yüzdesi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Başlatılan Çalıştırmalar  |  Başlatılan Çalıştırmalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Toplam Çekirdek  |  Toplam Çekirdek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Toplam Düğümler  |  Toplam Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Kullanılmaz Çekirdekler  |  Kullanılmaz Çekirdekler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.MachineLearningServices/çalışma alanları  |  Kullanılamaz Düğümler  |  Kullanılamaz Düğümler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Haritalar/hesapları  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Haritalar/hesapları  |  Kullanım  |  Kullanım  |  Sayı  |  Sayı | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  Varlık Sayısı  |  Varlık sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  Varlık Kotası  |  Varlık kotası  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  Varlık KotasıKullanılma Yüzdesi  |  Varlık kotası kullanılan yüzde  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  İçerikKeyPolicyCount  |  İçerik Anahtar İlkesi sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  İçerikKeyPolicyKota  |  İçerik Anahtar İlkesi kotası  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  İçerikKeyPolicyQuotaUsedPercentage  |  İçerik Anahtar İlkesi kotası kullanılan yüzde  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  Akış Politikası Sayısı  |  Akış İlkesi sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  StreamingPolicyQuota  |  Akış İlkesi kotası  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices  |  StreamingPolicyQuotaUsedPercentage  |  Akış İlkesi kotası kullanılan yüzde  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices/streamingEndpoints  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices/streamingEndpoints  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Media/mediaservices/streamingEndpoints  |  SuccessE2ELatency  |  Gecikme sonu için başarı sona erer  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  GCPauseToplam Sayısı  |  GC Duraklatma Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  GCPauseTotalTime  |  GC Duraklama Toplam Süresi  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  MaxOldGenMemoryPoolBytes  |  Maksimum Kullanılabilir Eski Nesil Veri Boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  OldGenMemoryPoolBytes  |  Eski Nesil Veri Boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  OldGenPromotedBytes  |  Eski Nesil Veri Boyutuna Tanıtın  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  ServiceCpuKullanım Yüzdesi  |  Hizmet CPU Kullanım Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  HizmetMemoryCommitted  |  Hizmet Belleği Atandı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  ServiceMemoryMax  |  Servis Bellek Max  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  Hizmet Bellekleri Kullanılır  |  Kullanılan Servis Belleği  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  SystemCpuKullanım Yüzdesi  |  Sistem CPU Kullanım Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatErrorCount  |  Tomcat Global Hatası  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatReceivedBytes  |  Tomcat Toplam Alınan Baytlar  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestMaxTime  |  Tomcat İstek Max Zaman  |  Milisaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestTotalSayısı  |  Tomcat İstek Toplam Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatRequestTotalTime  |  Tomcat İstek Toplam Kez  |  Milisaniye  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatResponseAvgTime  |  Tomcat İstek Ortalama Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatSentBytes  |  Tomcat Toplam Gönderilen Bayt  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionActiveCurrentCount  |  Tomcat Oturum Canlı Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionActiveMaxCount  |  Tomcat Oturum Max Etkin Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionAliveMaxTime  |  Tomcat Oturum Max Alive Time  |  Milisaniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionCreatedCount  |  Tomcat Oturum Oluşturulan Sayı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionExpiredCount  |  Tomcat Oturumu Süresi Dolmuş Sayı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  TomcatSessionReddedildiSayısı  |  Tomcat Oturumu Reddedilen Sayı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Microservices4Spring/appClusters  |  YoungGenPromotedBytes  |  Genç Nesil Veri Boyutuna Tanıtın  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.NetApp/netAppAccounts/capacityPools  |  VolumePoolAllocatedUsed  |  Kullanılan hacim havuzu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.NetApp/netAppAccounts/capacityPools  |  VolumePoolTotalLogicalSize  |  Birim havuzu toplam mantıksal boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  AverageReadLatency  |  Ortalama okuma gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  AverageWriteLatency  |  Ortalama yazma gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  ReadIops  |  Iops'u okuyun  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  Ciltleme Boyutu  |  Birim mantıksal boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  VolumeSnapshotSize  |  Birim anlık görüntü boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.NetApp/netAppAccounts/capacityPools/volumes  |  Yazma  |  iops yaz  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  UygulamaGatewayTotalTime  |  Uygulama Ağ Geçidi Toplam Süresi  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  AvgRequestCountPerHealthyHost  |  Sağlıklı Ana Bilgisayar Başına dakika başına istekler  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  BackendConnectTime  |  Arka Uç Bağlantı Süresi  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  BackendFirstByteResponseTime  |  Backend İlk Bayt Yanıt Süresi  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  BackendLastByteResponseTime  |  Backend Son Bayt Yanıt Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  BackendResponseStatus  |  Arka Uç Yanıt Durumu  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Engellenen Sayım  |  Web Uygulaması Güvenlik Duvarı Engellenen İstekler Kural Dağıtımı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  BlokeReqCount  |  Web Uygulaması Güvenlik Duvarı Engellenen İstek Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  BaytAlınan  |  Alınan Baytlar  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Baytgönderildi  |  Gönderilen Baytlar  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  Kapasite Birimleri  |  Mevcut Kapasite Birimleri  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  MüşteriRtt  |  İstemci RTT  |  Milisaniye  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  İşlem Birimleri  |  Geçerli İşlem Birimleri  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Güncel Bağlantılar  |  Geçerli Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Başarısız İstekler  |  Başarısız İstekler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Sağlıklı HostCount  |  Sağlıklı Ev Sahibi Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Eşleşen Sayı  |  Web Uygulama Güvenlik Duvarı Toplam Kural Dağılımı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  Yanıt Durumu  |  Yanıt Durumu  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/applicationGateways  |  Aktarım hızı  |  Aktarım hızı  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  TlsProtokol  |  İstemci TLS Protokolü  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  TotalRequests  |  Toplam İstek Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/applicationGateways  |  SağlıksızHostCount  |  Sağlıksız Ev Sahibi Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/azurefirewalls  |  UygulamaRuleHit  |  Uygulama kuralları sayısına isabet  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/azurefirewalls  |  Veriİşlendi  |  İşlenen veriler  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/azurefirewalls  |  Güvenlik DuvarıSağlık  |  Güvenlik duvarı sistem durumu  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/azurefirewalls  |  NetworkRuleHit  |  Ağ kuralları isabet sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/azurefirewalls  |  SNATPortKullanım  |  SNAT bağlantı noktası kullanımı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/bağlantılar  |  BitsInPerİkinci  |  BitsInPerİkinci  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/bağlantılar  |  BitsOutPerİkinci  |  BitsOutPerİkinci  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/dnszones  |  SorguHacmi  |  Sorgu Hacmi  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/dnszones  |  RecordSetCapacityKullanımation  |  Kayıt Seti Kapasite Kullanımı  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/dnszones  |  Kayıt Seti Sayısı  |  Kayıt Kümesi Sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/expressRouteCircuits  |  ArpAvailability  |  Arp Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRouteCircuits  |  BgpKullanılabilirlik  |  Bgp Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/expressRouteCircuits  |  BitsInPerİkinci  |  BitsInPerİkinci  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/expressRouteCircuits  |  BitsOutPerİkinci  |  BitsOutPerİkinci  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/expressRouteCircuits  |  GlobalReachBitsInPerSecond  |  GlobalReachBitsInPerSecond  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/expressRouteCircuits  |  GlobalReachBitsOutPerSecond  |  GlobalReachBitsOutPerSecond  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/expressRouteCircuits  |  QosDropBitsInPerİkinci  |  DüştüInBitsPerSecond  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/expressRouteCircuits  |  QosDropBitsOutPerSecond  |  BırakılanOutBitsPerSecond  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRouteCircuits/peerings  |  BitsInPerİkinci  |  BitsInPerİkinci  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRouteCircuits/peerings  |  BitsOutPerİkinci  |  BitsOutPerİkinci  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/expressRouteGateways  |  ErGatewayConnectionBitsInPerİkinci  |  BitsInPerİkinci  |  CountPerSecond  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Network/expressRouteGateways  |  ErGatewayConnectionBitsOutPerSecond  |  BitsOutPerİkinci  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRoutePorts  |  Yönetici Durumu  |  Yönetici Durumu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRoutePorts  |  LineProtokolü  |  LineProtokolü  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRoutePorts  |  PortBitsInPerİkinci  |  BitsInPerİkinci  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRoutePorts  |  PortBitsOutPerİkinci  |  BitsOutPerİkinci  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRoutePorts  |  RxLightLevel  |  RxLightLevel  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/expressRoutePorts  |  TxLightLevel  |  TxLightLevel  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  BackendHealthPercentage  |  Arka Uç Sağlık Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  BackendRequestCount  |  Arka Uç İstek Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  BackendRequestLatency  |  Arka Uç İstek GecikmeSi  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  FaturalanabilirYanıtBoyutu  |  Faturalandırılabilir Yanıt Boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  İstek Sayısı  |  İstek Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  İstek Boyutu  |  İstek Boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  YanıtBoyutu  |  Yanıt Boyutu  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  TotalLatency  |  Toplam Gecikme  |  Milisaniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Network/ön kapılar  |  WebApplicationFirewallRequestCount  |  Web Uygulaması Güvenlik Duvarı İstek Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/loadBalancers  |  Tahsis edilenSnatPort'lar  |  Tahsis edilmiş SNAT Bağlantı Noktaları (Önizleme)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  Bytecount  |  Bayt Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  DipAvailability  |  Sağlık Sondası Durumu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  Paket Sayısı  |  Paket Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  SnatConnectionCount  |  SNAT Bağlantı Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  SYNCount  |  SYN Sayısı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Network/loadBalancers  |  UsedSnatPorts  |  İkinci El SNAT Bağlantı Noktaları (Önizleme)  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/loadBalancers  |  VipAvailability  |  Veri Yolu Kullanılabilirliği  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/networkInterfaces  |  BaytReceivedRate  |  Alınan Baytlar  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/networkInterfaces  |  BaytSentRate  |  Gönderilen Baytlar  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/networkInterfaces  |  PaketlerReceivedRate  |  Alınan Paketler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/networkInterfaces  |  PaketlerSentRate  |  Gönderilen Paketler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/networkWatchers/connectionMonitors  |  AverageRoundtripMs  |  Avg. Gidiş-dönüş Saati (ms)  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/networkWatchers/connectionMonitors  |  ChecksFailedPercent  |  Denetler Başarısız Yüzde (Önizleme)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/networkWatchers/connectionMonitors  |  ProbesFailedPercent  |  % Sondalar Başarısız Oldu  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/networkWatchers/connectionMonitors  |  Gidiş-Dönüş Zamanları  |  Gidiş-Dönüş Süresi (ms) (Önizleme)  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  Bytecount  |  Bayt Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  BytesDroppedDDoS  |  Gelen baytd DDoS düştü  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  BaytForwardedDDoS  |  Gelen baytlar iletilen DDoS  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  BaytInDDoS  |  Gelen baytlar DDoS  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  DDoSTriggerSYNPaketleri  |  DDoS azaltmayı tetiklemek için gelen SYN paketleri  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  DDoSTriggerTCPPaketleri  |  DDoS azaltmayı tetiklemek için gelen TCP paketleri  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  DDoSTriggerUDPPackets  |  DDoS azaltmayı tetiklemek için gelen UDP paketleri  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  IfunderDdosAttack  |  DDoS saldırısı altında veya değil  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  Paket Sayısı  |  Paket Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  PaketlerDroppedDDoS  |  Gelen paketler DDoS düştü  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  PaketlerForwardedDDoS  |  Gelen paketler iletilen DDoS  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  PaketlerInDDoS  |  Gelen paketler DDoS  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  SynCount  |  SYN Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  TCPBytesDroppedDDoS  |  Gelen TCP baytDDoS düştü  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  TCPBytesForwardedDDoS  |  Gelen TCP baytları iletilen DDoS  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  TCPBytesInDDoS  |  Gelen TCP baytd DDoS  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  TCPPacketsDroppedDDoS  |  Gelen TCP paketleri DDoS düştü  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  TCPPacketsForwardedDDoS  |  Gelen TCP paketleri iletilmiş DDoS  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  TCPPacketsInDDoS  |  Gelen TCP paketleri DDoS  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  UDPBytesDroppedDDoS  |  Gelen UDP bayt DDoS düştü  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  UDPBytesForwardedDDoS  |  Gelen UDP baytları iletilen DDoS  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  UDPBytesInDDoS  |  Gelen UDP bayt DDoS  |  BaytPerİkinci  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  UDPPacketsDroppedDDoS  |  Gelen UDP paketleri DDoS düştü  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  UDPPacketsForwardedDDoS  |  Gelen UDP paketleri iletilen DDoS  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  UDPPacketsInDDoS  |  Gelen UDP paketleri DDoS  |  CountPerSecond  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/publicIPAdresleri  |  VipAvailability  |  Veri Yolu Kullanılabilirliği  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/trafficManagerProfiles  |  ProbeAgentCurrentEndpointStateByProfileResourceId  |  Bitiş Noktasına Göre Uç Noktası Durumu  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Network/trafficManagerProfiles  |  QpsByEndpoint  |  Bitiş Noktası İadesi'ne göre sorgular  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  Ortalama Bant Genişliği  |  Ağ Geçidi S2S Bant Genişliği  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  P2SBant Genişliği  |  Ağ Geçidi P2S Bant Genişliği  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  P2SConnectionCount  |  P2S Bağlantı Sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  TünelOrtalamaBant Genişliği  |  Tünel Bant Genişliği  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  TunnelEgressBayt  |  Tünel Çıkış Bayt  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  TunnelEgressPacketDropTSMismatch  |  Tünel Çıkış TS Uyumsuzluk Paket Bırak  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  TunnelEgressPackets  |  Tünel Çıkış Paketleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  TunnelIngressBayt  |  Tünel Girişi Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  Tünel IngressPacketDropTSMismatch  |  Tünel Girişi TS Uyuşmazlık Paket Bırakma  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworkAğ Ağ Geçidi  |  TunnelIngressPaketleri  |  Tünel Giriş Paketleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworks  |  PingMeshAverageRoundtripMs  |  Ping'ler için VM'ye gidiş dönüş süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Network/virtualNetworks  |  PingMeshProbesFailedPercent  |  VM'de Başarısız Ping  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  Gelen  |  Gelen Mesajlar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  gelen.all.failedrequests  |  Tüm Gelen Başarısız İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  gelen.all.requests  |  Tüm Gelen İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  gelen.zamanlanmış  |  Gönderilen Zamanlanmış Anında Iletme Bildirimleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  gelen.scheduled.cancel  |  Zamanlanan Anında Iletme Bildirimleri İptal Edildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  yükleme.all  |  Kurulum Yönetimi İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.delete  |  Yükleme Işlemlerini Sil  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  yükleme.get  |  Kurulum İşlemleri alın  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.patch  |  Yama Kurulum İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  installation.upsert  |  Kurulum İşlemleri Oluşturma veya Güncelleştirme  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  notificationhub.pushs  |  Tüm Giden Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.allpns.badorexpiredchannel  |  Hatalı veya Süresi Dolmuş Kanal Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.allpns.channelerror  |  Kanal Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.allpns.geçersizpayload  |  Yük Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.allpns.pnserror  |  Dış Bildirim Sistemi Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.allpns.başarı  |  Başarılı bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.apns.badchannel  |  APNS Kötü Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.apns.expiredchannel  |  APNS Süresi Dolmuş Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.apns.geçersiz kimlik bilgileri  |  APNS Yetkilendirme Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.apns.geçersizbildirimboyutu  |  APNS Geçersiz Bildirim Boyutu Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.apns.pnserror  |  APNS Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.apns.success  |  APNS Başarılı Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.authenticationerror  |  GCM Kimlik Doğrulama Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.badchannel  |  GCM Kötü Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.expiredchannel  |  GCM Süresi Dolmuş Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.geçersiz kimlik bilgileri  |  GCM Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.geçersizbildirim formatı  |  GCM Geçersiz Bildirim Biçimi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.geçersizbildirimboyutu  |  GCM Geçersiz Bildirim Boyutu Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.pnserror  |  GCM Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.başarı  |  GCM Başarılı Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.throttled  |  GCM Daraltılmış Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.gcm.wrongchannel  |  GCM Yanlış Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.kimlik doğrulama hatası  |  MPNS Kimlik Doğrulama Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.badchannel  |  MPNS Kötü Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.channeldisconnected  |  MPNS Kanalı Kesildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.düştü  |  MPNS Bırakılan Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.geçersiz kimlik bilgileri  |  MPNS Geçersiz Kimlik Bilgileri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.geçersizbildirim formatı  |  MPNS Geçersiz Bildirim Biçimi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.pnserror  |  MPNS Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.success  |  MPNS Başarılı Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.mpns.throttled  |  MPNS Daraltılmış Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.authenticationerror  |  WNS Kimlik Doğrulama Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.badchannel  |  WNS Kötü Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.channeldisconnected  |  WNS Kanalı Kesildi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.channelthrottled  |  WNS Kanal Daraltılmış  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.düştü  |  WNS Bırakılan Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.expiredchannel  |  WNS Süresi Dolmuş Kanal Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.geçersiz kimlik bilgileri  |  WNS Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.geçersizbildirim formatı  |  WNS Geçersiz Bildirim Biçimi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.geçersizbildirimboyutu  |  WNS Geçersiz Bildirim Boyutu Hatası  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.invalidtoken  |  WNS Yetkilendirme Hataları (Geçersiz Belirteç)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.pnserror  |  WNS Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.success  |  WNS Başarılı Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.throttled  |  WNS Daraltılmış Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.tokenproviderunreachable  |  WNS Yetkilendirme Hataları (Erişilemez)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  giden.wns.wrongtoken  |  WNS Yetkilendirme Hataları (Yanlış Belirteç)  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  kayıt.all  |  Kayıt İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  kayıt.create  |  Kayıt Oluşturma İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  kayıt.delete  |  Kayıt Silme İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  kayıt.get  |  Kayıt Okuma İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  kayıt.update  |  Kayıt Güncelleme İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.NotificationHubs/Namespaces/NotificationHubs  |  zamanlandı.beklemede  |  Bekleyen Zamanlanan Bildirimler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kullanılabilir Bellek  |  % Kullanılabilir Bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kullanılabilir Takas Alanı  |  % Kullanılabilir Takas Alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kullanımda Taahhüt Edilen Baytlar  |  % Taahhüt Bayt Lar Kullanımda  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ DPC Süresi  |  % DPC Süresi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Ücretsiz Inodes  |  % Ücretsiz Inodes  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Boş Alan  |  % Boş Alan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Boş Alan  |  % Boş Alan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Boşta Kalma Süresi  |  % Boşta Kalma Süresi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kesme Süresi  |  % Kesme Süresi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ IO Bekleme Süresi  |  % IO Bekleme Süresi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Güzel Zaman  |  % Güzel Zaman  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Ayrıcalıklı Zaman  |  % Ayrıcalıklı Zaman  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ İşlemci Süresi  |  % İşlemci zamanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ İşlemci Süresi  |  % İşlemci zamanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kullanılmış Inodes  |  % Kullanılmış Inodes  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kullanılmış Bellek  |  % Kullanılan Bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kullanılmış Alan  |  % Kullanılan Alan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kullanılmış Takas Alanı  |  % Kullanılan Takas Alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  %Average_ Kullanıcı Süresi  |  % Kullanıcı Süresi  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Available MBytes  |  Kullanılabilir MBytes  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Available MBytes Bellek  |  Kullanılabilir MBytes Bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Available MBytes Takas  |  Mevcut MBytes Swap  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Avg. Disk sn/Okuma  |  Avg. Disk sn/Oku  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Avg. Disk sn/Okuma  |  Avg. Disk sn/Oku  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Avg. Disk sn/Transfer  |  Avg. Disk sn/Transfer  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Avg. Disk sn/Yazma  |  Avg. Disk sn/Yazma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Avg. Disk sn/Yazma  |  Avg. Disk sn/Yazma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  alınan/sn Average_Bytes  |  Alınan baytlar/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Bytes Gönderildi/sn  |  Gönderilen bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Bytes Toplam/sn  |  Bayt Toplam/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Current Disk Sıra Uzunluğu  |  Geçerli Disk Sıra Uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Disk Bayt/sn oku  |  Disk Okuma Bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Disk Okur/sn  |  Disk Okuma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Disk Okur/sn  |  Disk Okuma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Disk Transferleri/sn  |  Disk Transferleri/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Disk Transferleri/sn  |  Disk Transferleri/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Disk Bayt/sn Yaz  |  Disk Bayt/sn Yazma  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Disk Yazıyor/sn  |  Disk Yazma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Disk Yazıyor/sn  |  Disk Yazma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Free Megabayt  |  Ücretsiz Megabayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Free Megabayt  |  Ücretsiz Megabayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Free Fiziksel Bellek  |  Ücretsiz Fiziksel Bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Sayfalama Dosyalarında Average_Free Alanı  |  Sayfalama Dosyalarında Boş Alan  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Free Sanal Bellek  |  Ücretsiz Sanal Bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Logical Disk Bayt/sn  |  Mantıksal Disk Baytları/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Page Okuma/sn  |  Sayfa Okuma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Page Yazıyor/sn  |  Sayfa Yazma/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Pages/sn  |  Sayfalar/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Pct Ayrıcalıklı Zaman  |  Pct Ayrıcalıklı Zaman  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Pct Kullanıcı Süresi  |  Pct Kullanıcı Saati  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Physical Disk Bayt/sn  |  Fiziksel Disk Bayt/sn  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Processes  |  İşlemler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Processor Sıra Uzunluğu  |  İşlemci Sıra Uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Size Sayfalama Dosyalarında Depolanır  |  Sayfalama Dosyalarında Depolanan Boyut  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Total Bayt  |  Toplam Bayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  alınan Average_Total Bayt  |  Alınan Toplam Bayt Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Total Bayt Lar Aktarıldı  |  Aktarılan Toplam Bayt  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Total Çarpışmaları  |  Toplam Çarpışmalar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  alınan Average_Total Paket  |  Alınan Toplam Paket Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Total Paketler İletilen  |  Aktarılan Toplam Paketler  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Total Rx Hataları  |  Toplam Rx Hataları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Total Tx Hataları  |  Toplam Tx Hataları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Uptime  |  Uptime  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Used MByteS Takas Alanı  |  İkinci El MByteS Takas Alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Used Bellek kByte  |  İkinci El Bellek kByte'ları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Used Bellek MBytes  |  İkinci El Bellek MByte'ları  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Users  |  Kullanıcılar  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Average_Virtual Paylaşılan Bellek  |  Sanal Paylaşılan Bellek  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Olay  |  Olay  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.OperationalInsights/çalışma alanları  |  Sinyal  |  Sinyal  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.OperationalInsights/çalışma alanları  |  Güncelleştirme  |  Güncelleştirme  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.PowerBIDedicated/kapasiteleri  |  memory_metric  |  Bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.PowerBIDedicated/kapasiteleri  |  memory_thrashing_metric  |  Bellek Thrashing (Datasets)  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.PowerBIDedicated/kapasiteleri  |  qpu_high_utilization_metric  |  QPU Yüksek Kullanımı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.PowerBIDedicated/kapasiteleri  |  Sorgu Süresi  |  Sorgu Süresi (Datasets)  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.PowerBIDedicated/kapasiteleri  |  QueryPoolJobQueueLength  |  Sorgu Havuzu İş Sırası Uzunluğu (Datasets)  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  Aktif Bağlantılar  |  Aktif Bağlantılar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  ActiveListeners  |  ActiveListeners  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Relay/namespaces  |  BaytTransfer  |  BaytTransfer  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  DinleyiciConnections-ClientError  |  DinleyiciConnections-ClientError  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  DinleyiciConnections-ServerError  |  DinleyiciConnections-ServerError  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  DinleyiciBağlantıları-Başarı  |  DinleyiciBağlantıları-Başarı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  DinleyiciBağlantıları-TotalRequests  |  DinleyiciBağlantıları-TotalRequests  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  Dinleyici Keser  |  Dinleyici Keser  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  GönderenBağlantılar-İstemci Hatası  |  GönderenBağlantılar-İstemci Hatası  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  GönderenConnections-ServerError  |  GönderenConnections-ServerError  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  GönderenConnections-Başarı  |  GönderenConnections-Başarı  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  GönderenBağlantılar-Toplam İstekler  |  GönderenBağlantılar-Toplam İstekler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Relay/namespaces  |  Gönderen Keser  |  Gönderen Keser  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Search/searchServices  |  Arama Latency  |  Arama Gecikmesi  |  Saniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Search/searchServices  |  AramaSorgularPerSecond  |  Saniyede arama sorguları  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Search/searchServices  |  DaraltılmışAramaSorgularıYüzdesi  |  Daraltılmış arama sorguları yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  Aktif Bağlantılar  |  Aktif Bağlantılar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  ActiveMessages  |  Bir Sıra/Konu'daki etkin iletisayısı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  BağlantılarKapalı  |  Bağlantılar Kapalı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  BağlantılarAçıldı  |  Bağlantılar Açıldı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  CPUXNS  |  Cpu (Amortismana Uğradı)  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  DeadletteredMesajlar  |  Sıra/Konu'daki ölü harflerle yazılmış iletilerin sayısı.  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.ServiceBus/namespaces  |  Gelen Mesajlar  |  Gelen Mesajlar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.ServiceBus/namespaces  |  Gelen İstekler  |  Gelen İstekler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  İletiler  |  Sıra/Konu'daki ileti sayısı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  NamespaceCpuKullanım  |  CPU  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  NamespaceMemoryUsage  |  Bellek Kullanımı  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.ServiceBus/namespaces  |  Giden Mesajlar  |  Giden Mesajlar  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  Zamanlanmış Mesajlar  |  Bir Sıra/Konu'daki zamanlanmış iletisayısı.  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  Sunucu Hataları  |  Sunucu Hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  Boyut  |  Boyut  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  Başarılı İstekler  |  Başarılı İstekler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  OperationName  |  Daraltılmış İstekler.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  Kullanıcı Hataları  |  Kullanıcı Hataları.  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.ServiceBus/namespaces  |  WSXNS  |  Bellek Kullanımı (Amortismana Uğradı)  |  Yüzde  |  Maksimum | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  ActualCpu  |  ActualCpu  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  Fiili Bellek  |  Fiili Bellek  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  TahsisCpu  |  TahsisCpu  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  Ayrılmış Bellek  |  Ayrılmış Bellek  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  Başvuru Durumu  |  Başvuru Durumu  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  Konteyner Durumu  |  Konteyner Durumu  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  CpuKullanımı  |  CpuKullanımı  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  MemoryKullanımation  |  MemoryKullanımation  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  Yeniden BaşlatmaSayısı  |  Yeniden BaşlatmaSayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  ServiceReplicaStatus  |  ServiceReplicaStatus  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.ServiceFabricMesh/uygulamaları  |  Hizmet Durumu  |  Hizmet Durumu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.SignalRService/SignalR  |  Bağlantı Sayısı  |  Bağlantı Sayısı  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.SignalRService/SignalR  |  Gelen Trafik  |  Gelen Trafik  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.SignalRService/SignalR  |  İleti Sayısı  |  İleti Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.SignalRService/SignalR  |  GidenTrafik  |  Giden Trafik  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.SignalRService/SignalR  |  Sistem Hataları  |  Sistem Hataları  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.SignalRService/SignalR  |  Kullanıcı Hataları  |  Kullanıcı Hataları  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/managedInstances  |  avg_cpu_percent  |  Ortalama CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/managedInstances  |  io_bytes_read  |  IO bayt okuyun  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/managedInstances  |  io_bytes_written  |  IO bayt lar yazılı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/managedInstances  |  io_requests  |  IO istekleri sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/managedInstances  |  reserved_storage_mb  |  Depolama alanı ayrılmıştır  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/managedInstances  |  storage_space_used_mb  |  Kullanılan depolama alanı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/managedInstances  |  virtual_core_count  |  Sanal çekirdek sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/sunucular  |  database_dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/sunucular  |  database_storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/sunucular  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/sunucular  |  dtu_used  |  Kullanılan DTU  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/sunucular  |  storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  app_cpu_billed  |  Uygulama CPU faturalı  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  app_cpu_percent  |  Uygulama CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  app_memory_percent  |  Uygulama bellek yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  blocked_by_firewall  |  Güvenlik Duvarı tarafından engellendi  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  cache_hit_percent  |  Önbellek isabet yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  cache_used_percent  |  Önbellek kullanılan yüzde  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  connection_failed  |  Başarısız Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  connection_successful  |  Başarılı Bağlantılar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  cpu_limit  |  CPU sınırı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  cpu_used  |  Kullanılan CPU  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  Çıkmaz  |  Kilitlenmeleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  dtu_limit  |  DTU Limiti  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  dtu_used  |  Kullanılan DTU  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  dwu_consumption_percent  |  DWU yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  dwu_limit  |  DWU sınırı  |  Sayı  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  dwu_used  |  Kullanılan DWU  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  local_tempdb_usage_percent  |  Yerel tempdb yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  log_write_percent  |  Günlük IO yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  memory_usage_percent  |  Bellek yüzdesi  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  sessions_percent  |  Oturumyüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  depolama  |  Kullanılan veri alanı  |  Bayt  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  storage_percent  |  Yüzde kullanılan veri alanı  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  tempdb_data_size  |  Tempdb Veri Dosya Boyutu Kilobayt  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  tempdb_log_size  |  Tempdb Log Dosya Boyutu Kilobayt  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/veritabanları  |  tempdb_log_used_percent  |  Tempdb Yüzde Günlük Kullanılan  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  workers_percent  |  İşçi yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/veritabanları  |  xtp_storage_percent  |  Bellek IÇI OLTP depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  allocated_data_storage_percent  |  Yüzde ayrılan veri alanı  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  cpu_limit  |  CPU sınırı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  cpu_used  |  Kullanılan CPU  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_allocated_data_storage  |  Ayrılan veri alanı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_cpu_limit  |  CPU sınırı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_cpu_percent  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_cpu_used  |  Kullanılan CPU  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_eDTU_used  |  eDTU kullanılan  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_log_write_percent  |  Günlük IO yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_sessions_percent  |  Oturumyüzdesi  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Sql/servers/elasticPools  |  database_workers_percent  |  İşçi yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  dtu_consumption_percent  |  DTU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  eDTU_limit  |  eDTU sınırı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  eDTU_used  |  eDTU kullanılan  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  log_write_percent  |  Günlük IO yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  physical_data_read_percent  |  Veri G/Ç yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  sessions_percent  |  Oturumyüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  storage_limit  |  Veri max boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  storage_percent  |  Yüzde kullanılan veri alanı  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  storage_used  |  Kullanılan veri alanı  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  tempdb_data_size  |  Tempdb Veri Dosya Boyutu Kilobayt  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_size  |  Tempdb Log Dosya Boyutu Kilobayt  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.Sql/servers/elasticPools  |  tempdb_log_used_percent  |  Tempdb Yüzde Günlük Kullanılan  |  Yüzde  |  Maksimum | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  workers_percent  |  İşçi yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Sql/servers/elasticPools  |  xtp_storage_percent  |  Bellek IÇI OLTP depolama yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageHesapları  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageHesapları  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageHesapları  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageHesapları  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageHesapları  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageHesapları  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Storage/storageHesapları  |  Kullanılmış Kapasite  |  Kullanılmış kapasite  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  BlobKapasite  |  Blob Kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  BlobCount  |  Blob Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Konteyner Sayısı  |  Blob Konteyner Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Dizin Kapasitesi  |  Dizin Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/blobServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  Dosya Kapasitesi  |  Dosya Kapasitesi  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  Dosya Sayısı  |  Dosya Sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareCount  |  Dosya Paylaşım Sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareQuota  |  Dosya paylaşımı kota boyutu  |  Bayt  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareSnapshotCount  |  Dosya paylaşımı anlık görüntü sayısı  |  Sayı  |  Ortalama | 
| Hayır  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  FileShareSnapshotSize  |  Dosya paylaşımı anlık görüntü boyutu  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/fileServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Sıra Kapasitesi  |  Sıra Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  Sıra Sayısı  |  Sıra Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  QueueMessageCount  |  Sıra İleti Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/queueServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Kullanılabilirlik  |  Kullanılabilirlik  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Çıkış  |  Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Giriş  |  Giriş  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessE2ELatency  |  Başarı E2E Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  SuccessServerLatency  |  Başarı Sunucu Gecikme Süresi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Tablo Kapasitesi  |  Tablo Kapasitesi  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  Tablo Sayısı  |  Tablo Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  TabloEntityCount  |  Tablo Varlık Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.Storage/storageAccounts/tableServices  |  İşlemler  |  İşlemler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  Müşteri Likleri  |  Toplam İstemci IOPS  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  Müşteri Gecikmesi  |  Ortalama İstemci Gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  MüşteriLockiOPS  |  İstemci Kilidi IOPS  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  MüşteriMetadataReadIOPS  |  İstemci Meta veri IOPS okuyun  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  MüşteriMetadataWriteIOPS  |  İstemci Meta veri Yazma IOPS  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  MüşteriReadIOPS  |  İstemci IOPS Okuyun  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  MüşteriReadThroughput  |  Ortalama Önbellek Okuma Throughput  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  Müşteri Yazma  |  İstemci IOPS Yaz  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  ClientWriteThroughput  |  Ortalama Önbellek Yazma Throughput  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetAsyncYazma  |  StorageTarget Asynchronous Yazma Throughput  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaHedefDoldurmaThroughput  |  DepolamaHedef Dolgu Throughput  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaHedefSağlık  |  Depolama Hedef Sağlık  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetIOPS  |  Toplam DepolamaHedef IOPS  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetLatency  |  DepolamaHedef Gecikme  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetMetadataReadIOPS  |  StorageTarget Meta data Oku IOPS  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetMetadataWriteIOPS  |  StorageTarget Meta data Write IOPS  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetReadAheadThroughput  |  StorageTarget İleriyi Oku Bilgi  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaHedefReadIOPS  |  StorageTarget IOPS Okuyun  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetSyncWriteThroughput  |  StorageTarget Synchronous Yazma Throughput  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetTotalReadThroughput  |  DepolamaHedef Toplam Okuma İş İlerletme  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaTargetTotalWriteThroughput  |  StorageTarget Toplam Yazma İş Ilik  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  DepolamaHedefWriteIOPS  |  StorageTarget Yazma IOPS  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.StorageÖnbellek/önbellekler  |  Uptime  |  Uptime  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  ServerSyncSessionResult  |  Eşitleme Oturum Sonucu  |  Sayı  |  Ortalama | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  DepolamaSyncBatchTransferredFileBytes  |  Bayt senkronize  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  StorageSyncRecalledNetworkBytesByApplication  |  Uygulamaya göre bulut katmanlama geri çağırma boyutu  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  DepolamaSyncRecalledTotalNetworkBytes  |  Bulut katmanlama geri çağırma boyutu  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  DepolamaSyncRecallIOTotalSizeBytes  |  Bulut katmanlama geri çağırma  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  DepolamaSyncRecallThroughputBytesPerSecond  |  Bulut katmanlama geri çağırma iş  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  DepolamaSyncServerKalp Atışı  |  Sunucu Çevrimiçi Durumu  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  StorageSyncSyncSessionAppliedFilesCount  |  Eşitlenen Dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices  |  StorageSyncSyncSessionPeritemErrorsCount  |  Eşitleme olmayan dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices/registeredServers  |  SunucuKalp Atışı  |  Sunucu Çevrimiçi Durumu  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices/registeredServers  |  ServerRecallIOTotalSizeBytes  |  Bulut katmanlama geri çağırma  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupBatchTransferredFileBytes  |  Bayt senkronize  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionAppliedFilesCount  |  Eşitlenen Dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices/syncGroups  |  SyncGroupSyncSessionPeritemErrorsCount  |  Eşitleme olmayan dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointBatchTransferredFileBytes  |  Bayt senkronize  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionAppliedFilesCount  |  Eşitlenen Dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints  |  ServerEndpointSyncSessionPerItemErrorsCount  |  Eşitleme olmayan dosyalar  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutFailedRequests  |  Başarısız Fonksiyon İstekleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutInputEtkinlikler  |  Fonksiyon Etkinlikleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  AMLCalloutRequests  |  Fonksiyon İstekleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  Dönüşüm Hataları  |  Veri Dönüştürme Hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  DeserializationHatası  |  Giriş Deserialization Hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  BırakılanOrAdjustedEvents  |  Sıra dışı Etkinlikler  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  EarlyInputOlaylar  |  Erken Giriş Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  Hatalar  |  Çalışma Zamanı Hataları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventBytes  |  Giriş Olay Bayt  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  GirişEtkinlikleri  |  Giriş Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventsSourcesBacklogged  |  Backlogged Giriş Olayları  |  Sayı  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  InputEventsSourcesPerSecond  |  Alınan Giriş Kaynakları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  LateInputEtkinlikler  |  Geç Giriş Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  OutputEvents  |  Çıktı Olayları  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  OutputWatermarkDelaySeconds  |  Filigran Gecikmesi  |  Saniye  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.StreamAnalytics/streamingjobs  |  Kaynak Kullanımı  |  SU % Kullanım  |  Yüzde  |  Maksimum | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  Disk Okuma Baytları  |  Disk Okuma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  Disk Okuma İşlemleri/Sn  |  Disk Okuma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  Disk Yazma Baytları  |  Disk Yazma Baytları  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  Disk Yazma İşlemleri/Sn  |  Disk Yazma İşlemleri/Sn  |  CountPerSecond  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  DiskReadBytesPerSecond  |  Disk Okuma Bayt/Sn  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  DiskReadLatency  |  Disk Okuma Gecikmesi  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  DiskReadİşlemleri  |  Disk Okuma İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  DiskWriteBytesPerSecond  |  Disk Bayt/Sn Yazma  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  DiskWriteLatency  |  Disk Yazma Gecikme  |  Milisaniye  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  DiskYazma İşlemleri  |  Disk Yazma İşlemleri  |  Sayı  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  MemoryActive  |  Bellek Etkin  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  MemoryVerilen  |  Bellek Verilen  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  BellekLer Kullanıldı  |  Kullanılan Bellek  |  Bayt  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  Ağ Girişi  |  Ağ Girişi  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  Ağ Çıkışı  |  Ağ Çıkışı  |  Bayt  |  Toplam | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  NetworkInBytesPerSecond  |  Ağ Bayt/Sn  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  AğOutBytesPerSecond  |  Ağ Çıkış Baytları/Sn  |  BaytPerİkinci  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  CPU yüzdesi  |  CPU yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | Hayır |  Microsoft.VMwareCloudBasit/virtualMachines  |  YüzdeCpuReady  |  Yüzde CPU Hazır  |  Milisaniye  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Aktif İstekler  |  Etkin İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Ortalama Yanıt Süresi  |  Ortalama Yanıt Süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  BaytAlınan  |  Veri In  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Baytgönderildi  |  Veri Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  CpuYüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  DiskQueueLength  |  Disk Sıra Uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http101 ile  |  Http 101  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http2xx  |  Http 2xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http3xx  |  Http 3xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http401 sayılı  |  Http 401  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http403 sayılı  |  Http 403  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http404  |  Http 404  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http406 sayılı  |  Http 406  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http4xx  |  Http 4xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Http5xx  |  Http Sunucu Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  httpQueueLength  |  Http Sıra Uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  LargeAppServicePlanInstances  |  Büyük Uygulama Hizmet Planı Çalışanları  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  MediumAppServicePlanInstances  |  Orta Uygulama Hizmet Planı İşçileri  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Bellek Yüzdesi  |  Bellek Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  SmallAppServicePlanInstances  |  Küçük Uygulama Hizmet Planı Çalışanları  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/multiRolePools  |  Toplam FrontEnds  |  Toplam Ön Uç  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/işçiHavuzları  |  CpuYüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/işçiHavuzları  |  Bellek Yüzdesi  |  Bellek Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/işçiHavuzları  |  İşçiler Kullanılabilir  |  Mevcut İşçiler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/işçiHavuzları  |  İşçiToplam  |  Toplam İşçi  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/hostingOrtamlar/işçiHavuzları  |  İşçiler Kullanılmış  |  İkinci El İşçiler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  BaytAlınan  |  Veri In  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  Baytgönderildi  |  Veri Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  CpuYüzdesi  |  CPU Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  DiskQueueLength  |  Disk Sıra Uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  httpQueueLength  |  Http Sıra Uzunluğu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  Bellek Yüzdesi  |  Bellek Yüzdesi  |  Yüzde  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpCloseWait  |  TCP Kapanış Bekle  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpKapanış  |  TCP Kapanış  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpKuruldu  |  TCP Kuruldu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpFinWait1  |  TCP Fin Bekle 1  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpFinWait2  |  TCP Fin Bekle 2  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpLastAck  |  TCP Son Ack  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpSynReceived  |  TCP Syn Alındı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpSynSent  |  TCP Syn Gönderildi  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/serverfarms  |  TcpTimeWait  |  TCP Zaman Bekle  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  AppConnections  |  Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  OrtalamaMemoryWorkingSet  |  Ortalama bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Ortalama Yanıt Süresi  |  Ortalama Yanıt Süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  BaytAlınan  |  Veri In  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Baytgönderildi  |  Veri Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  CpuTime  |  CPU Süresi  |  Saniye  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Güncel Montajlar  |  Geçerli Meclisler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  FonksiyonYürütme Sayısı  |  İşlev Yürütme Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Fonksiyonyürütme Birimleri  |  Fonksiyon Yürütme Birimleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Gen0Koleksiyonlar  |  Gen 0 Çöp Koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Gen1Koleksiyonlar  |  Gen 1 Çöp Koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Gen2Koleksiyonlar  |  Gen 2 Çöp Koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Handles  |  Tanıtıcı Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  HealthCheckDurumu  |  Sağlık kontrolü durumu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http101 ile  |  Http 101  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http2xx  |  Http 2xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http3xx  |  Http 3xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http401 sayılı  |  Http 401  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http403 sayılı  |  Http 403  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http404  |  Http 404  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http406 sayılı  |  Http 406  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http4xx  |  Http 4xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Http5xx  |  Http Sunucu Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  httpResponseTime  |  Yanıt Süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  IoOtherBytesPerSecond  |  IO Saniyede Diğer Baytlar  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  IoOtherOperationsPerSecond  |  IO Saniyede Diğer İşlemler  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  IoReadBytesPerSecond  |  IO Saniyede Bayt Oku  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  IoReadOperationsPerSecond  |  IO Saniyede Okuma İşlemleri  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  IoWriteBytesPerSecond  |  IO Saniyede Bayt Yaz  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  IoWriteOperationsPerSecond  |  IO Saniyede Yazma İşlemleri  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  MemoryWorkingSet  |  Bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  Özel Baytlar  |  Özel Baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  RequestsInApplicationQueue  |  Uygulama Kuyruğundaki İstekler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  İş Parçacıkları  |  Ilmekli  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  TotalAppDomains  |  Toplam Uygulama Etki Alanı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler  |  TotalAppDomainsUnloaded  |  Toplam Uygulama Etki Alanı Boşaltıldı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  AppConnections  |  Bağlantılar  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  OrtalamaMemoryWorkingSet  |  Ortalama bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Ortalama Yanıt Süresi  |  Ortalama Yanıt Süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  BaytAlınan  |  Veri In  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Baytgönderildi  |  Veri Çıkış  |  Bayt  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  CpuTime  |  CPU Süresi  |  Saniye  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Güncel Montajlar  |  Geçerli Meclisler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  FonksiyonYürütme Sayısı  |  İşlev Yürütme Sayısı  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Fonksiyonyürütme Birimleri  |  Fonksiyon Yürütme Birimleri  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Gen0Koleksiyonlar  |  Gen 0 Çöp Koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Gen1Koleksiyonlar  |  Gen 1 Çöp Koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Gen2Koleksiyonlar  |  Gen 2 Çöp Koleksiyonları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Handles  |  Tanıtıcı Sayısı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  HealthCheckDurumu  |  Sağlık kontrolü durumu  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http101 ile  |  Http 101  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http2xx  |  Http 2xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http3xx  |  Http 3xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http401 sayılı  |  Http 401  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http403 sayılı  |  Http 403  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http404  |  Http 404  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http406 sayılı  |  Http 406  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http4xx  |  Http 4xx  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Http5xx  |  Http Sunucu Hataları  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  httpResponseTime  |  Yanıt Süresi  |  Saniye  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  IoOtherBytesPerSecond  |  IO Saniyede Diğer Baytlar  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  IoOtherOperationsPerSecond  |  IO Saniyede Diğer İşlemler  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  IoReadBytesPerSecond  |  IO Saniyede Bayt Oku  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  IoReadOperationsPerSecond  |  IO Saniyede Okuma İşlemleri  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  IoWriteBytesPerSecond  |  IO Saniyede Bayt Yaz  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  IoWriteOperationsPerSecond  |  IO Saniyede Yazma İşlemleri  |  BaytPerİkinci  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  MemoryWorkingSet  |  Bellek çalışma kümesi  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  Özel Baytlar  |  Özel Baytlar  |  Bayt  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  İstekler  |  İstekler  |  Sayı  |  Toplam | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  RequestsInApplicationQueue  |  Uygulama Kuyruğundaki İstekler  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  İş Parçacıkları  |  Ilmekli  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  TotalAppDomains  |  Toplam Uygulama Etki Alanı  |  Sayı  |  Ortalama | 
| **Evet**  | **Evet** |  Microsoft.Web/siteler/yuvalar  |  TotalAppDomainsUnloaded  |  Toplam Uygulama Etki Alanı Boşaltıldı  |  Sayı  |  Ortalama | 
