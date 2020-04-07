---
title: Azure Monitor kaynak türüne göre desteklenen ölçümler
description: Azure Monitor ile her kaynak türü için kullanılabilen ölçümlerin listesi.
author: rboucher
services: azure-monitor
ms.topic: reference
ms.date: 04/06/2020
ms.author: robb
ms.subservice: metrics
ms.openlocfilehash: 846e8da889e3913c4a8eaab7808495bbd8afad29
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754671"
---
# <a name="supported-metrics-with-azure-monitor"></a>Azure Monitor ile desteklenen ölçümler

> [!NOTE]
> Bu liste büyük ölçüde Azure Monitor Ölçümleri REST API'sinden otomatik olarak oluşturulur. Github üzerinden bu listede yapılan herhangi bir değişiklik uyarı olmadan üzerine yazılabilir. Kalıcı güncelleştirmelerin nasıl yapılacağını öğrenmek için bu makalenin yazarına başvurun.

Azure Monitor, ölçümleri portalda grafiklendirmek, REST API'den erişmek veya PowerShell veya CLI kullanarak sorgulamak gibi ölçümlerle etkileşim kurmanın çeşitli yollarını sağlar. Aşağıda, Şu anda Azure Monitor'un metrik ardışık boru hattıyla kullanılabilen tüm ölçümlerin tam listesi verilmiştir. Diğer ölçümler portalda veya eski API'ler kullanılarak kullanılabilir. Aşağıdaki liste yalnızca birleştirilmiş Azure Monitor metrik ardışık ardışık hattını kullanarak kullanılabilen ölçümleri içerir. Ölçümler ad alanına göre düzenlenir. Hizmetlerin listesi ve bunlara ait ad alanları [için Azure hizmetleri için Kaynak sağlayıcılarına](../../azure-resource-manager/management/azure-services-resource-providers.md)bakın. Bu ölçümleri programlı olarak sorgulamak ve bunlara erişmek için lütfen [2018-01-01 api sürümünü](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) kullanın

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
> Tanılama ayarları yla dışa aktarılabilen platform ölçümlerinin listesi için [bu makaleye](metrics-supported-export-diagnostic-settings.md)bakın.


## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/sunucular

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|qpu_metric|QPU|Sayı|Ortalama|QPU. S1 için 0-100, S2 için 0-200 ve S4 için 0-400 aralığı|ServerResourceType|
|memory_metric|Bellek|Bayt|Ortalama|Bellek. S1 için 0-25 GB, S2 için 0-50 GB ve S4 için 0-100 GB aralığı|ServerResourceType|
|private_bytes_metric|Özel Baytlar|Bayt|Ortalama|Özel baytlar.|ServerResourceType|
|virtual_bytes_metric|Sanal Bayt Sayısı|Bayt|Ortalama|Sanal baytlar.|ServerResourceType|
|Toplam Bağlantı İstekleri|Toplam Bağlantı İstekleri|Sayı|Ortalama|Toplam bağlantı istekleri. Bunlar gelenler.|ServerResourceType|
|BaşarılıConnectionsPerSec|Sn Başına Başarılı Bağlantılar|CountPerSecond|Ortalama|Başarılı bağlantı tamamlanma oranı.|ServerResourceType|
|ToplamBağlantı Hataları|Toplam Bağlantı Hataları|Sayı|Ortalama|Toplam başarısız bağlantı denemesi.|ServerResourceType|
|Geçerli Kullanıcı Oturumları|Geçerli Kullanıcı Oturumları|Sayı|Ortalama|Şu anda kurulan kullanıcı oturumları sayısı.|ServerResourceType|
|QueryPoolBusyThreads|Sorgu Havuzu Meşgul Konuları|Sayı|Ortalama|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolJobQueueLength|Komut Havuzu İş Sıra Uzunluğu|Sayı|Ortalama|Komut iş parçacığı havuzunun kuyruğundaki iş sayısı.|ServerResourceType|
|ProcessingPoolJobQueueLength|İşlem havuzu Iş Sıra Uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzunun kuyruğundaki G/Ç olmayan işlerin sayısı.|ServerResourceType|
|Güncel Bağlantılar|Bağlantı: Geçerli bağlantılar|Sayı|Ortalama|Kurulan istemci bağlantılarının geçerli sayısı.|ServerResourceType|
|CleanerCurrentPrice|Bellek: Temiz Geçerli Fiyat|Sayı|Ortalama|Geçerli bellek fiyatı, $/byte/time, 1000'e normalleştirildi.|ServerResourceType|
|TemizleyiciMemoryShrinkable|Bellek: Temiz Bellek küçültilebilir|Bayt|Ortalama|Bellek miktarı, bayt, arka plan temizleyici tarafından tasfiye tabi.|ServerResourceType|
|TemizleyiciMemoryShrinkable|Bellek: Temiz Bellek küçültilemez|Bayt|Ortalama|Bellek miktarı, bayt, arka plan temizleyici tarafından tasfiye tabi değil.|ServerResourceType|
|MemoryUsage|Bellek: Bellek Kullanımı|Bayt|Ortalama|Daha temiz bellek fiyatının hesaplanmasında kullanılan sunucu işleminin bellek kullanımı. Karşı Proses\PrivateBytes artı bellek eşlenen veri boyutuna eşit, xVelocity motor Bellek Sınırı aşan xVelocity bellek analiz motoru (VertiPaq) tarafından eşlenen veya tahsis edilmiş herhangi bir bellek yoksayarak.|ServerResourceType|
|MemoryLimitHard|Bellek: Bellek Sınırı Sabit|Bayt|Ortalama|Yapılandırma dosyasından sabit bellek sınırı.|ServerResourceType|
|MemoryLimitHigh|Bellek: Bellek Sınırı Yüksek|Bayt|Ortalama|Yapılandırma dosyasından yüksek bellek sınırı.|ServerResourceType|
|MemoryLimitLow|Bellek: Bellek Sınırı Düşük|Bayt|Ortalama|Yapılandırma dosyasından düşük bellek sınırı.|ServerResourceType|
|MemoryLimitVertiPaq|Bellek: Bellek Limit VertiPaq|Bayt|Ortalama|Yapılandırma dosyasından bellek sınırı.|ServerResourceType|
|Kota|Bellek: Kota|Bayt|Ortalama|Geçerli bellek kotası, baytlar. Bellek kotası, bellek hibesi veya bellek rezervasyonu olarak da bilinir.|ServerResourceType|
|Kota Engellendi|Bellek: Kota Engellendi|Sayı|Ortalama|Diğer bellek kotaları serbest bırakılıncaya kadar engellenen geçerli kota isteği sayısı.|ServerResourceType|
|VertiPaqNonpaged|Bellek: VertiPaq Nonpaged|Bayt|Ortalama|Bellek içi motor tarafından kullanılmak üzere çalışma kümesinde kilitli bellek baytları.|ServerResourceType|
|VertiPaqPaged|Bellek: VertiPaq Paged|Bayt|Ortalama|Bellek içi veriler için kullanılan sayfalı bellek baytları.|ServerResourceType|
|SatırlarReadPerSec|İşleme: Satırlar her saniye okunur|CountPerSecond|Ortalama|Tüm ilişkisel veritabanlarından okunan satır hızı.|ServerResourceType|
|SatırlarConvertedPerSec|İşleme: Satırlar saniye başına dönüştürülür|CountPerSecond|Ortalama|İşlem sırasında dönüştürülen satır ların oranı.|ServerResourceType|
|SatırlarWrittenPerSec|İşleme: Satırlar saniyebaşına yazılmış|CountPerSecond|Ortalama|İşlem sırasında yazılan satırların oranı.|ServerResourceType|
|Komut HavuzuBusyThreads|İş parçacıkları: Komut havuzu meşgul konuları|Sayı|Ortalama|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|KomutPoolIdleThreads|İş parçacıkları: Komut havuzu boşta konuları|Sayı|Ortalama|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|LongParsingBusyThreads|İş parçacıkları: Uzun ayrışma meşgul konuları|Sayı|Ortalama|Uzun ayrışma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|LongParsingIdleThreads|İş parçacıkları: Uzun ayrışma boşta iş parçacıkları|Sayı|Ortalama|Uzun ayrışma iş parçacığı havuzundaki boştaki iş parçacığı sayısı.|ServerResourceType|
|LongParsingJobQueueLength|İş parçacıkları: Uzun ayrışma iş sırası uzunluğu|Sayı|Ortalama|Uzun ayrışma iş parçacığı havuzunun kuyruğundaki iş sayısı.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|İş parçacıkları: İşlem havuzu meşgul G/Ç iş iş parçacıkları|Sayı|Ortalama|İşlemiş iş parçacığı havuzunda G/Ç işleri çalıştıran iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|İş parçacıkları: İşlem havuzu meşgul olmayan G/Ç iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda G/Ç olmayan işleri çalıştıran iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolioJobQueueLength|İş parçacıkları: İşlem havuzu G/Ç iş sırası uzunluğu|Sayı|Ortalama|İşlemiş iş parçacığı havuzunun kuyruğundaki G/Ç iş sayısı.|ServerResourceType|
|İşlemePoolIdleIOJobThreads|İş parçacıkları: İşlem havuzu boşta G/Ç iş iş parçacıkları|Sayı|Ortalama|İşlemiş iş parçacığı havuzundaki G/Ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|İş parçacıkları: İşlem havuzu boşta Olmayan G/Ç iş parçacıkları|Sayı|Ortalama|G/Ç olmayan işlere adanmış işleme iş parçacığı havuzundaki boştaki iş parçacığı sayısı.|ServerResourceType|
|QueryPoolIdleThreads|İş parçacıkları: Sorgu havuzu boşta konuları|Sayı|Ortalama|İşlemiş iş parçacığı havuzundaki G/Ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolJobQueueLength|Konu: Sorgu havuzu iş sırası lengt|Sayı|Ortalama|Sorgu iş parçacığı havuzunun kuyruğundaki iş sayısı.|ServerResourceType|
|ShortParsingBusyThreads|İş parçacıkları: Kısa ayrışma meşgul konuları|Sayı|Ortalama|Kısa ayrışma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|ShortParsingIdleThreads|Konu: Kısa ayrışma boşta iş parçacıkları|Sayı|Ortalama|Kısa ayrışma iş parçacığı havuzundaki boştaki iş parçacığı sayısı.|ServerResourceType|
|ShortParsingJobQueueLength|İş parçacıkları: Kısa ayrışma iş sırası uzunluğu|Sayı|Ortalama|Kısa ayrışma iş parçacığı havuzunun kuyruğundaki iş sayısı.|ServerResourceType|
|memory_thrashing_metric|Bellek Temizleme|Yüzde|Ortalama|Ortalama hafıza dayak.|ServerResourceType|
|mashup_engine_qpu_metric|M Motor QPU|Sayı|Ortalama|Mashup motor prosesleri ile QPU kullanımı|ServerResourceType|
|mashup_engine_memory_metric|M Motor Belleği|Bayt|Ortalama|Mashup motor prosesleri ile bellek kullanımı|ServerResourceType|
|mashup_engine_private_bytes_metric|M Motoru Özel Bayt|Bayt|Ortalama|Mashup motor prosesleri ile özel bayt kullanımı.|ServerResourceType|
|mashup_engine_virtual_bytes_metric|M Motor Sanal Bayt|Bayt|Ortalama|Mashup motor süreçleri ile sanal bayt kullanımı.|ServerResourceType|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalRequests|Toplam Ağ Geçidi İsteği (Amortismana Uğradı)|Sayı|Toplam|Ağ geçidi isteklerinin sayısı - GatewayResponseCodeCategory boyutu yerine çok boyutlu istek metrik kullanın|Konum,Hostname|
|Başarılı İstekler|Başarılı Ağ Geçidi İstekleri (Amortismana Uğradı)|Sayı|Toplam|Başarılı ağ geçidi isteklerinin sayısı - GatewayResponseCodeCategory boyutu yerine çok boyutlu istek metrik kullanın|Konum,Hostname|
|Yetkisiz İstekler|Yetkisiz Ağ Geçidi İstekleri (Amortismana Uğradı)|Sayı|Toplam|Yetkisiz ağ geçidi isteklerinin sayısı - GatewayResponseCodeCategory boyutu yerine çok boyutlu istek metrik kullanın|Konum,Hostname|
|Başarısız İstekler|Başarısız Ağ Geçidi İstekleri (Amortismana Uğradı)|Sayı|Toplam|Ağ geçidi isteklerindeki hata sayısı - GatewayResponseCodeCategory boyutu yerine çok boyutlu istek metrik kullanın|Konum,Hostname|
|Diğer İstekler|Diğer Ağ Geçidi İstekleri (Amortismana Uğradı)|Sayı|Toplam|Diğer ağ geçidi isteklerinin sayısı - GatewayResponseCodeCategory boyutu yerine çok boyutlu istek metrik kullanın|Konum,Hostname|
|Süre|Ağ Geçidi İsteklerinin Genel Süresi|Milisaniye|Ortalama|Milisaniye cinsinden Ağ Geçidi İsteklerinin Toplam Süresi|Konum,Hostname|
|Arka uçSüresi|Arka Uç İsteklerinin Süresi|Milisaniye|Ortalama|Milisaniye cinsinden Arka Uç İsteklerinin Süresi|Konum,Hostname|
|Kapasite|Kapasite|Yüzde|Ortalama|ApiManagement hizmeti için kullanım ölçümü|Konum|
|EventHubTotalOlaylar|Toplam EventHub Etkinliği|Sayı|Toplam|EventHub'a gönderilen etkinlik sayısı|Konum|
|EventHubSuccessfulEvents|Başarılı EventHub Etkinlikleri|Sayı|Toplam|Başarılı EventHub etkinliklerinin sayısı|Konum|
|EventHubTotalFailedEvents|Başarısız EventHub Etkinlikleri|Sayı|Toplam|Başarısız EventHub olaylarının sayısı|Konum|
|EventHubRejectedOlaylar|Reddedilen EventHub Etkinlikleri|Sayı|Toplam|Reddedilen EventHub olaylarının sayısı (yanlış yapılandırma veya yetkisiz)|Konum|
|OlayHubThrottledEtkinlikler|Daraltılmış EventHub Etkinlikleri|Sayı|Toplam|Daraltılmış EventHub olaylarının sayısı|Konum|
|EventHubTimedoutEtkinlikler|Zamanlanmış OlayHub Etkinlikleri|Sayı|Toplam|Zamanlanmış EventHub olaylarının sayısı|Konum|
|EventHubDroppedEtkinlikler|Bırakılan EventHub Etkinlikleri|Sayı|Toplam|Sıra boyutu sınırına ulaşıldığı için atlanan olay sayısı|Konum|
|EventHubTotalBytesSent|EventHub Etkinliklerinin Boyutu|Bayt|Toplam|Bayttaki EventHub olaylarının toplam boyutu|Konum|
|İstekler|İstekler|Sayı|Toplam|Birden çok boyuta sahip ağ geçidi istek ölçümleri|Konum,Hostname,LastErrorReason,BackendResponseCode,GatewayResponseCode,BackendResponseCodeCategory,GatewayResponseCodeCategory|
|Ağ Bağlantısı|Kaynakların Ağ Bağlantısı Durumu (Önizleme)|Sayı|Toplam|API Yönetimi hizmetinden bağımlı kaynak türlerinin Ağ Bağlantısı durumu|Konum,Kaynak Türü|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft.AppConfiguration/configurationStores

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|HttpIncomingRequestCount|HttpIncomingRequestCount|Sayı|Sayı|Toplam gelen http istekleri sayısı.|Statuscode|
|HttpIncomingRequestDuration|HttpIncomingRequestDuration|Sayı|Ortalama|Bir http isteği üzerine gecikme.|Statuscode|


## <a name="microsoftappplatformspring"></a>Microsoft.AppPlatform/Bahar

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SystemCpuKullanım Yüzdesi|Sistem CPU Kullanım Yüzdesi|Yüzde|Ortalama|Tüm sistem için son işlemci kullanımı|Appname,Pod|
|AppCpuKullanım Yüzdesi|Uygulama CPU Kullanım Yüzdesi|Yüzde|Ortalama|Uygulama JVM CPU Kullanım Yüzdesi|Appname,Pod|
|AppMemoryCommitted|Atanan Uygulama Belleği|Bayt|Ortalama|JVM'ye baytolarak atanan bellek|Appname,Pod|
|AppMemoryUsed|Kullanılan Uygulama Belleği|Bayt|Ortalama|Baytlarda Kullanılan Uygulama Belleği|Appname,Pod|
|AppMemoryMax|Uygulama Bellek Max|Bayt|Maksimum|Bellek yönetimi için kullanılabilecek baytlarda maksimum bellek miktarı|Appname,Pod|
|MaxOldGenMemoryPoolBytes|Maksimum Kullanılabilir Eski Nesil Veri Boyutu|Bayt|Ortalama|Eski nesil bellek havuzunun maksimum boyutu|Appname,Pod|
|OldGenMemoryPoolBytes|Eski Nesil Veri Boyutu|Bayt|Ortalama|Tam bir GC sonra eski nesil bellek havuzu boyutu|Appname,Pod|
|OldGenPromotedBytes|Eski Nesil Veri Boyutuna Tanıtın|Bayt|Maksimum|GC'den gc'den sonra eski nesil bellek havuzunun boyutundaki olumlu artışların sayısı|Appname,Pod|
|YoungGenPromotedBytes|Genç Nesil Veri Boyutuna Tanıtın|Bayt|Maksimum|Bir GC'den sonra genç nesil bellek havuzunun büyüklüğünde bir artış için bir sonraki|Appname,Pod|
|GCPauseToplam Sayısı|GC Duraklatma Sayısı|Sayı|Toplam|GC Duraklatma Sayısı|Appname,Pod|
|GCPauseTotalTime|GC Duraklama Toplam Süresi|Milisaniye|Toplam|GC Duraklama Toplam Süresi|Appname,Pod|
|TomcatSentBytes|Tomcat Toplam Gönderilen Bayt|Bayt|Toplam|Tomcat Toplam Gönderilen Bayt|Appname,Pod|
|TomcatReceivedBytes|Tomcat Toplam Alınan Baytlar|Bayt|Toplam|Tomcat Toplam Alınan Baytlar|Appname,Pod|
|TomcatRequestTotalTime|Tomcat İstek Toplam Kez|Milisaniye|Toplam|Tomcat İstek Toplam Kez|Appname,Pod|
|TomcatRequestTotalSayısı|Tomcat İstek Toplam Sayısı|Sayı|Toplam|Tomcat İstek Toplam Sayısı|Appname,Pod|
|TomcatResponseAvgTime|Tomcat İstek Ortalama Süresi|Milisaniye|Ortalama|Tomcat İstek Ortalama Süresi|Appname,Pod|
|TomcatRequestMaxTime|Tomcat İstek Max Zaman|Milisaniye|Maksimum|Tomcat İstek Max Zaman|Appname,Pod|
|TomcatErrorCount|Tomcat Global Hatası|Sayı|Toplam|Tomcat Global Hatası|Appname,Pod|
|TomcatSessionActiveMaxCount|Tomcat Oturum Max Etkin Sayısı|Sayı|Toplam|Tomcat Oturum Max Etkin Sayısı|Appname,Pod|
|TomcatSessionAliveMaxTime|Tomcat Oturum Max Alive Time|Milisaniye|Maksimum|Tomcat Oturum Max Alive Time|Appname,Pod|
|TomcatSessionActiveCurrentCount|Tomcat Oturum Canlı Sayısı|Sayı|Toplam|Tomcat Oturum Canlı Sayısı|Appname,Pod|
|TomcatSessionCreatedCount|Tomcat Oturum Oluşturulan Sayı|Sayı|Toplam|Tomcat Oturum Oluşturulan Sayı|Appname,Pod|
|TomcatSessionExpiredCount|Tomcat Oturumu Süresi Dolmuş Sayı|Sayı|Toplam|Tomcat Oturumu Süresi Dolmuş Sayı|Appname,Pod|
|TomcatSessionReddedildiSayısı|Tomcat Oturumu Reddedilen Sayı|Sayı|Toplam|Tomcat Oturumu Reddedilen Sayı|Appname,Pod|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationHesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Toplam İş|Toplam İşler|Sayı|Toplam|Toplam iş sayısı|Runbook,Durum|
|ToplamUpdateDeploymentRuns|Toplam Güncelleştirme Dağıtım Çalışır|Sayı|Toplam|Toplam yazılım güncelleştirme dağıtım çalışır|SoftwareUpdateConfigurationName,Durum|
|TotalUpdateDeploymentMachineRuns|Toplam Güncelleme Dağıtım Makinesi Çalışır|Sayı|Toplam|Toplam yazılım güncelleştirme dağıtım makinesi bir yazılım güncelleştirme dağıtım çalıştırır|SoftwareUpdateConfigurationName,Durum,TargetComputer,SoftwareUpdateConfigurationRunid|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchHesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Çekirdek Sayısı|Özel Çekirdek Sayısı|Sayı|Toplam|Toplu iş hesabındaki toplam ayrılmış çekirdek sayısı|None|
|TotalNodeCount|Özel Düğüm Sayısı|Sayı|Toplam|Toplu iş hesabındaki toplam adanmış düğüm sayısı|None|
|Düşük ÖncelikliÇekirdek Sayısı|Düşük Öncelikli Çekirdek Sayısı|Sayı|Toplam|Toplu iş hesabındaki toplam düşük öncelikli çekirdek sayısı|None|
|ToplamLowPriorityNodeCount|Düşük Öncelikli Düğüm Sayısı|Sayı|Toplam|Toplu iş hesabındaki toplam düşük öncelikli düğüm sayısı|None|
|OluşturmaNodeCount|Düğüm Sayısı Oluşturma|Sayı|Toplam|Oluşturulan düğüm sayısı|None|
|BaşlangıçNodeCount|Başlangıç Düğümü Sayısı|Sayı|Toplam|Başlayan düğüm sayısı|None|
|WaitingForStartTaskNodeCount|Başlangıç Görev Düğümü Sayısını Beklerken|Sayı|Toplam|Başlangıç Görevi'nin tamamlanmasını bekleyen düğüm sayısı|None|
|BaşlangıçTaskFailedNodeCount|Görev Başarısız Düğüm Sayısını Başlat|Sayı|Toplam|Başlangıç Görevi'nin başarısız olduğu düğüm sayısı|None|
|IdleNodeCount|Boşta Düğüm Sayısı|Sayı|Toplam|Boşta düğüm sayısı|None|
|ÇevrimdışıNodeCount|Çevrimdışı Düğüm Sayısı|Sayı|Toplam|Çevrimdışı düğüm sayısı|None|
|Yeniden BaşlatmaNodeCount|Düğüm Sayısını Yeniden Başlatma|Sayı|Toplam|Yeniden başlatma düğümü sayısı|None|
|ReimagingNodeCount|Regörüntüleme Düğümü Sayısı|Sayı|Toplam|Regörüntüleme düğümlerinin sayısı|None|
|RunningNodeCount|Çalışan Düğüm Sayısı|Sayı|Toplam|Çalışan düğüm sayısı|None|
|LeavingPoolNodeCount|Havuz Düğümü Sayısını Bırakma|Sayı|Toplam|Havuzdan ayrılan düğüm sayısı|None|
|KullanılamazSayı|Kullanılamaz Düğüm Sayısı|Sayı|Toplam|Kullanılamaz düğüm sayısı|None|
|PreemptedNodeCount|Preempted Düğüm Sayısı|Sayı|Toplam|Preempted düğüm sayısı|None|
|TaskStartOlay|Görev Başlangıç Etkinlikleri|Sayı|Toplam|Başlatılan toplam görev sayısı|poolId,jobId|
|Görev Tamamlama Etkinliği|Görev Tamamlama Etkinlikleri|Sayı|Toplam|Tamamlanan toplam görev sayısı|poolId,jobId|
|TaskFailOlay|Görev Başarısız Olaylar|Sayı|Toplam|Başarısız durumda tamamlanan toplam görev sayısı|poolId,jobId|
|PoolCreateOlay|Havuz Oluşturma Etkinlikleri|Sayı|Toplam|Oluşturulan toplam havuz sayısı|poolId|
|PoolResizeStartEvent|Havuz Yeniden Boyutlandırma Etkinlikleri|Sayı|Toplam|Başlatılan toplam havuz yeniden boyutlandırma sayısı|poolId|
|PoolResizeCompleteOlay|Havuz Resize Komple Olaylar|Sayı|Toplam|Tamamlanan toplam havuz yeniden boyutlandırma sayısı|poolId|
|PoolDeleteStartOlay|Havuz Silme Başlangıç Olayları|Sayı|Toplam|Başlatılan toplam havuz silme sayısı|poolId|
|PoolDeleteCompleteOlay|Havuz Silme Etkinlikleri Tamamla|Sayı|Toplam|Tamamlanan toplam havuz silme sayısı|poolId|
|JobDeleteCompleteOlay|İş Silme Etkinlikleri tamamlandı|Sayı|Toplam|Başarıyla silinen toplam iş sayısı.|Jobıd|
|JobDeleteStartOlay|İş Silme Başlangıç Olayları|Sayı|Toplam|Silinmek istenen toplam iş sayısı.|Jobıd|
|JobDisableCompleteEvent|İş Devre Dışı Tam Olaylar|Sayı|Toplam|Başarıyla devre dışı bırakılan toplam iş sayısı.|Jobıd|
|JobDisableStartEvent|İş Devre dışı Başlangıç Etkinlikleri|Sayı|Toplam|Devre dışı bırakılmak istenen toplam iş sayısı.|Jobıd|
|JobStartOlay|İş Başlangıç Etkinlikleri|Sayı|Toplam|Başarıyla başlatılan toplam iş sayısı.|Jobıd|
|JobTerminateCompleteOlay|İş Sonlandırma Etkinlikleri Tamamlandı|Sayı|Toplam|Başarıyla sonlandırılan toplam iş sayısı.|Jobıd|
|JobTerminateStartOlay|İş Sonlandırma Başlangıç Olayları|Sayı|Toplam|Sonlandırılması istenen toplam iş sayısı.|Jobıd|


## <a name="microsoftbatchaiworkspaces"></a>Microsoft.BatchAI/çalışma alanları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Gönderilen İş|Gönderilen İş|Sayı|Toplam|Gönderilen iş sayısı|Senaryo,ClusterName|
|Tamamlanan İş|Tamamlanan İş|Sayı|Toplam|Tamamlanan iş sayısı|Senaryo,ClusterName,Sonuç Türü|
|Toplam Düğümler|Toplam Düğümler|Sayı|Ortalama|Toplam düğüm sayısı|Senaryo,ClusterName|
|Etkin Düğümler|Etkin Düğümler|Sayı|Ortalama|Çalışan düğüm sayısı|Senaryo,ClusterName|
|Boşta Düğümler|Boşta Düğümler|Sayı|Ortalama|Boşta düğüm sayısı|Senaryo,ClusterName|
|Kullanılamaz Düğümler|Kullanılamaz Düğümler|Sayı|Ortalama|Kullanılamaz düğüm sayısı|Senaryo,ClusterName|
|Preempted Düğümler|Preempted Düğümler|Sayı|Ortalama|Preempted düğüm sayısı|Senaryo,ClusterName|
|Düğümleri Bırakma|Düğümleri Bırakma|Sayı|Ortalama|Ayrılan düğüm sayısı|Senaryo,ClusterName|
|Toplam Çekirdek|Toplam Çekirdek|Sayı|Ortalama|Toplam çekirdek sayısı|Senaryo,ClusterName|
|Aktif Çekirdekler|Aktif Çekirdekler|Sayı|Ortalama|Aktif çekirdek sayısı|Senaryo,ClusterName|
|BoşTa Çekirdekler|BoşTa Çekirdekler|Sayı|Ortalama|Boşta kalan çekirdek sayısı|Senaryo,ClusterName|
|Kullanılmaz Çekirdekler|Kullanılmaz Çekirdekler|Sayı|Ortalama|Kullanışsız çekirdek sayısı|Senaryo,ClusterName|
|Preempted Çekirdekler|Preempted Çekirdekler|Sayı|Ortalama|Preempted çekirdek sayısı|Senaryo,ClusterName|
|Çekirdekleri Bırakma|Çekirdekleri Bırakma|Sayı|Ortalama|Ayrılan çekirdek sayısı|Senaryo,ClusterName|
|Kota Kullanım Yüzdesi|Kota Kullanım Yüzdesi|Sayı|Ortalama|Kullanılan kota yüzdesi|Senaryo,ClusterName,VmFamilyName,VmPriority|

## <a name="microsoftblockchainblockchainmembers"></a>Microsoft.Blockchain/blockchainÜyeler

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuUsagePercentageinDouble|CPU Kullanım Yüzdesi|Yüzde|Maksimum|CPU Kullanım Yüzdesi|Node|
|MemoryUsage|Bellek Kullanımı|Bayt|Ortalama|Bellek Kullanımı|Node|
|MemoryLimit|Bellek Sınırı|Bayt|Ortalama|Bellek Sınırı|Node|
|MemoryUsagePercentageinDouble|Bellek Kullanım Yüzdesi|Yüzde|Ortalama|Bellek Kullanım Yüzdesi|Node|
|DepolamaKullanımı|Depolama Kullanımı|Bayt|Ortalama|Depolama Kullanımı|Node|
|IOReadBytes|IO Okuma Baytları|Bayt|Toplam|IO Okuma Baytları|Node|
|IOWriteBytes|IO Bayt yaz|Bayt|Toplam|IO Bayt yaz|Node|
|BağlantıKabul Edildi|Kabul Edilen Bağlantılar|Sayı|Toplam|Kabul Edilen Bağlantılar|Node|
|Bağlantı Ele|İşlenen Bağlantılar|Sayı|Toplam|İşlenen Bağlantılar|Node|
|BağlantıAktif|Etkin Bağlantılar|Sayı|Ortalama|Etkin Bağlantılar|Node|
|İstek Ele|Ele Alınması İstekleri|Sayı|Toplam|Ele Alınması İstekleri|Node|
|İşlenmiş Bloklar|İşlenmiş Bloklar|Sayı|Toplam|İşlenmiş Bloklar|Node|
|İşlenmiş İşlemler|İşlenen İşlemler|Sayı|Toplam|İşlenen İşlemler|Node|
|Bekleyen İşlemler|Bekleyen İşlemler|Sayı|Ortalama|Bekleyen İşlemler|Node|
|Sıraya Ait İşlemler|Sıraya Ait İşlemler|Sayı|Ortalama|Sıraya Ait İşlemler|Node|



## <a name="microsoftcacheredis"></a>Microsoft.Önbellek/redis

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|bağlı istemciler|Bağlanan İstemciler|Sayı|Maksimum||ShardId|
|totalkomutlar işlendi|Toplam İşlemler|Sayı|Toplam||ShardId|
|önbellekler|Önbellek Hits|Sayı|Toplam||ShardId|
|önbellekler|Önbellek Misses|Sayı|Toplam||ShardId|
|önbellek|Önbellek Miss Oranı|Yüzde|önbellek||ShardId|
|komutları alma|Alır|Sayı|Toplam||ShardId|
|setkomutları|Ayarlar|Sayı|Toplam||ShardId|
|operasyonlarPerSecond|Saniye başına işlemler|Sayı|Maksimum||ShardId|
|tahliye tuşları|Tahliye Edilen Anahtarlar|Sayı|Toplam||ShardId|
|toplam tuşlar|Toplam Anahtar|Sayı|Maksimum||ShardId|
|süresi dolmuş anahtarlar|Süresi Dolmuş Anahtarlar|Sayı|Toplam||ShardId|
|usedmemory|Kullanılmış Bellek|Bayt|Maksimum||ShardId|
|usedmemorypercentage|Kullanılan Bellek Yüzdesi|Yüzde|Maksimum||ShardId|
|usedmemoryRss|İkinci el bellek RSS|Bayt|Maksimum||ShardId|
|serverLoad|Sunucu Yükü|Yüzde|Maksimum||ShardId|
|önbellekYazma|Önbellek Yazma|BaytPerİkinci|Maksimum||ShardId|
|önbellekOku|Önbellek Okuma|BaytPerİkinci|Maksimum||ShardId|
|yüzdeProcessorTime|CPU|Yüzde|Maksimum||ShardId|
|önbellekLatency|Önbellek Gecikme Mikrosaniye (Önizleme)|Sayı|Ortalama||ShardId|
|hatalar|Hatalar|Sayı|Maksimum||Shardid,ErrorType|
|bağlı istemciler0|Bağlı İstemciler (Shard 0)|Sayı|Maksimum||None|
|totalcommandsprocessed0|Toplam İşlemler (Shard 0)|Sayı|Toplam||None|
|önbellek0|Önbellek Hits (Shard 0)|Sayı|Toplam||None|
|önbellekmisses0|Önbellek Misses (Shard 0)|Sayı|Toplam||None|
|getkomutları0|Gets (Shard 0)|Sayı|Toplam||None|
|setkomutları0|Setler (Shard 0)|Sayı|Toplam||None|
|operasyonlarPerSecond0|Saniye başına işlemler (Shard 0)|Sayı|Maksimum||None|
|tahliye tuşları0|Tahliye Edilen Anahtarlar (Shard 0)|Sayı|Toplam||None|
|toplam tuşlar0|Toplam Anahtar (Shard 0)|Sayı|Maksimum||None|
|süresi dolmuş tuşlar0|Süresi Dolmuş Anahtarlar (Shard 0)|Sayı|Toplam||None|
|usedmemory0|İkinci El Bellek (Shard 0)|Bayt|Maksimum||None|
|usedmemoryRss0|İkinci El Bellek RSS (Shard 0)|Bayt|Maksimum||None|
|serverLoad0|Sunucu Yükü (Shard 0)|Yüzde|Maksimum||None|
|önbellekYazma0|Önbellek Yazma (Shard 0)|BaytPerİkinci|Maksimum||None|
|önbellekRead0|Önbellek Okuma (Shard 0)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime0|Cpu (Shard 0)|Yüzde|Maksimum||None|
|bağlı istemciler1|Bağlı İstemciler (Shard 1)|Sayı|Maksimum||None|
|totalkomutlar işlenmiş1|Toplam İşlemler (Shard 1)|Sayı|Toplam||None|
|önbellek1|Önbellek Hits (Shard 1)|Sayı|Toplam||None|
|önbellekmisses1|Önbellek Misses (Shard 1)|Sayı|Toplam||None|
|getkomutları1|Gets (Shard 1)|Sayı|Toplam||None|
|setkomutları1|Setler (Shard 1)|Sayı|Toplam||None|
|operasyonlarPerSecond1|Saniye başına işlemler (Shard 1)|Sayı|Maksimum||None|
|tahliye tuşları1|Tahliye Edilen Anahtarlar (Shard 1)|Sayı|Toplam||None|
|toplam tuşlar1|Toplam Anahtar (Shard 1)|Sayı|Maksimum||None|
|süresi dolmuş tuşlar1|Süresi Dolmuş Anahtarlar (Shard 1)|Sayı|Toplam||None|
|usedmemory1|İkinci El Bellek (Shard 1)|Bayt|Maksimum||None|
|usedmemoryRss1|İkinci El Bellek RSS (Shard 1)|Bayt|Maksimum||None|
|serverLoad1|Sunucu Yükü (Shard 1)|Yüzde|Maksimum||None|
|önbellekYazma1|Önbellek Yazma (Shard 1)|BaytPerİkinci|Maksimum||None|
|önbellekRead1|Önbellek Okuma (Shard 1)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime1|Cpu (Shard 1)|Yüzde|Maksimum||None|
|bağlı istemciler2|Bağlı İstemciler (Shard 2)|Sayı|Maksimum||None|
|totalcommandsprocessed2|Toplam İşlemler (Shard 2)|Sayı|Toplam||None|
|önbellek2|Önbellek Hits (Shard 2)|Sayı|Toplam||None|
|önbellekler2|Önbellek Misses (Shard 2)|Sayı|Toplam||None|
|getkomutları2|Gets (Shard 2)|Sayı|Toplam||None|
|setkomutları2|Setler (Shard 2)|Sayı|Toplam||None|
|operasyonlarPerSecond2|Saniye başına işlemler (Shard 2)|Sayı|Maksimum||None|
|tahliye tuşları2|Tahliye Edilen Anahtarlar (Shard 2)|Sayı|Toplam||None|
|toplam tuşlar2|Toplam Anahtar (Shard 2)|Sayı|Maksimum||None|
|süresi dolmuş tuşlar2|Süresi Dolmuş Anahtarlar (Shard 2)|Sayı|Toplam||None|
|usedmemory2|İkinci El Bellek (Shard 2)|Bayt|Maksimum||None|
|usedmemoryRss2|İkinci El Bellek RSS (Shard 2)|Bayt|Maksimum||None|
|serverLoad2|Sunucu Yükü (Shard 2)|Yüzde|Maksimum||None|
|önbellekWrite2|Önbellek Yazma (Shard 2)|BaytPerİkinci|Maksimum||None|
|önbellekRead2|Önbellek Okuma (Shard 2)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime2|Cpu (Shard 2)|Yüzde|Maksimum||None|
|bağlı istemciler3|Bağlı İstemciler (Shard 3)|Sayı|Maksimum||None|
|totalcommandsprocessed3|Toplam İşlemler (Shard 3)|Sayı|Toplam||None|
|önbellek3|Önbellek Hits (Shard 3)|Sayı|Toplam||None|
|önbellek3|Önbellek Misses (Shard 3)|Sayı|Toplam||None|
|getkomutları3|Gets (Shard 3)|Sayı|Toplam||None|
|setkomutları3|Setler (Shard 3)|Sayı|Toplam||None|
|operasyonlarPerSecond3|Saniye başına işlemler (Shard 3)|Sayı|Maksimum||None|
|tahliye tuşları3|Tahliye Edilen Anahtarlar (Shard 3)|Sayı|Toplam||None|
|toplam tuşlar3|Toplam Anahtar (Shard 3)|Sayı|Maksimum||None|
|süresi dolmuş tuşlar3|Süresi Dolmuş Anahtarlar (Shard 3)|Sayı|Toplam||None|
|usedmemory3|İkinci El Bellek (Shard 3)|Bayt|Maksimum||None|
|usedmemoryRss3|İkinci El Bellek RSS (Shard 3)|Bayt|Maksimum||None|
|serverLoad3|Sunucu Yükü (Shard 3)|Yüzde|Maksimum||None|
|önbellekYazma3|Önbellek Yazma (Shard 3)|BaytPerİkinci|Maksimum||None|
|önbellekRead3|Önbellek Okuma (Shard 3)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime3|Cpu (Shard 3)|Yüzde|Maksimum||None|
|bağlı istemciler4|Bağlı İstemciler (Shard 4)|Sayı|Maksimum||None|
|totalkomutlar işlenmiş4|Toplam İşlemler (Shard 4)|Sayı|Toplam||None|
|önbellek4|Önbellek Hits (Shard 4)|Sayı|Toplam||None|
|önbellekler4|Önbellek Misses (Shard 4)|Sayı|Toplam||None|
|getkomutları4|Gets (Shard 4)|Sayı|Toplam||None|
|setkomutları4|Setler (Shard 4)|Sayı|Toplam||None|
|operasyonlarPerSecond4|Saniye başına işlemler (Shard 4)|Sayı|Maksimum||None|
|tahliye tuşları4|Tahliye Edilen Anahtarlar (Shard 4)|Sayı|Toplam||None|
|toplam tuşlar4|Toplam Anahtar (Shard 4)|Sayı|Maksimum||None|
|süresi dolmuş tuşlar4|Süresi Dolmuş Anahtarlar (Shard 4)|Sayı|Toplam||None|
|usedmemory4|İkinci El Bellek (Shard 4)|Bayt|Maksimum||None|
|usedmemoryRss4|İkinci El Bellek RSS (Shard 4)|Bayt|Maksimum||None|
|serverLoad4|Sunucu Yükü (Shard 4)|Yüzde|Maksimum||None|
|önbellekWrite4|Önbellek Yazma (Shard 4)|BaytPerİkinci|Maksimum||None|
|önbellekRead4|Önbellek Okuma (Shard 4)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime4|Cpu (Shard 4)|Yüzde|Maksimum||None|
|bağlı istemciler5|Bağlı İstemciler (Shard 5)|Sayı|Maksimum||None|
|totalcommandsişlenmiş5|Toplam İşlemler (Shard 5)|Sayı|Toplam||None|
|önbellek5|Önbellek Hits (Shard 5)|Sayı|Toplam||None|
|önbellekler5|Önbellek Misses (Shard 5)|Sayı|Toplam||None|
|getkomutları5|Gets (Shard 5)|Sayı|Toplam||None|
|setkomutları5|Setler (Shard 5)|Sayı|Toplam||None|
|operasyonlarPerSecond5|Saniye başına işlemler (Shard 5)|Sayı|Maksimum||None|
|tahliye tuşları5|Tahliye Edilen Anahtarlar (Shard 5)|Sayı|Toplam||None|
|toplam tuşlar5|Toplam Anahtar (Shard 5)|Sayı|Maksimum||None|
|süresi dolmuş tuşlar5|Süresi Dolmuş Anahtarlar (Shard 5)|Sayı|Toplam||None|
|usedmemory5|İkinci El Bellek (Shard 5)|Bayt|Maksimum||None|
|usedmemoryRss5|İkinci El Bellek RSS (Shard 5)|Bayt|Maksimum||None|
|serverLoad5|Sunucu Yükü (Shard 5)|Yüzde|Maksimum||None|
|önbellekWrite5|Önbellek Yazma (Shard 5)|BaytPerİkinci|Maksimum||None|
|önbellekRead5|Önbellek Okuma (Shard 5)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime5|Cpu (Shard 5)|Yüzde|Maksimum||None|
|bağlı istemciler6|Bağlı İstemciler (Shard 6)|Sayı|Maksimum||None|
|totalkomutlar işlenmiş6|Toplam İşlemler (Shard 6)|Sayı|Toplam||None|
|önbellek6|Önbellek Hits (Shard 6)|Sayı|Toplam||None|
|önbellek6|Önbellek Misses (Shard 6)|Sayı|Toplam||None|
|getkomutları6|Gets (Shard 6)|Sayı|Toplam||None|
|setkomutları6|Setler (Shard 6)|Sayı|Toplam||None|
|operasyonlarPerSecond6|Saniye başına işlemler (Shard 6)|Sayı|Maksimum||None|
|tahliye tuşları6|Tahliye Edilen Anahtarlar (Shard 6)|Sayı|Toplam||None|
|toplam tuşlar6|Toplam Anahtar (Shard 6)|Sayı|Maksimum||None|
|süresi dolmuş tuşlar6|Süresi Dolmuş Anahtarlar (Shard 6)|Sayı|Toplam||None|
|usedmemory6|İkinci El Bellek (Shard 6)|Bayt|Maksimum||None|
|usedmemoryRss6|İkinci el Bellek RSS (Shard 6)|Bayt|Maksimum||None|
|serverLoad6|Sunucu Yükü (Shard 6)|Yüzde|Maksimum||None|
|önbellekWrite6|Önbellek Yazma (Shard 6)|BaytPerİkinci|Maksimum||None|
|önbellekRead6|Önbellek Okuma (Shard 6)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime6|Cpu (Shard 6)|Yüzde|Maksimum||None|
|bağlı istemciler7|Bağlı İstemciler (Shard 7)|Sayı|Maksimum||None|
|totalcommandsprocessed7|Toplam İşlemler (Shard 7)|Sayı|Toplam||None|
|önbellek7|Önbellek Hits (Shard 7)|Sayı|Toplam||None|
|önbellekler7|Önbellek Misses (Shard 7)|Sayı|Toplam||None|
|getkomutları7|Gets (Shard 7)|Sayı|Toplam||None|
|setkomutları7|Setler (Shard 7)|Sayı|Toplam||None|
|operasyonlarPerSecond7|Saniye başına operasyonlar (Shard 7)|Sayı|Maksimum||None|
|tahliye tuşları7|Tahliye Edilen Anahtarlar (Shard 7)|Sayı|Toplam||None|
|toplam tuşlar7|Toplam Anahtar (Shard 7)|Sayı|Maksimum||None|
|süresi dolmuş anahtarlar7|Süresi Dolmuş Anahtarlar (Shard 7)|Sayı|Toplam||None|
|usedmemory7|İkinci El Bellek (Shard 7)|Bayt|Maksimum||None|
|usedmemoryRss7|İkinci el Bellek RSS (Shard 7)|Bayt|Maksimum||None|
|serverLoad7|Sunucu Yükü (Shard 7)|Yüzde|Maksimum||None|
|önbellekWrite7|Önbellek Yazma (Shard 7)|BaytPerİkinci|Maksimum||None|
|önbellekRead7|Önbellek Okuma (Shard 7)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime7|Cpu (Shard 7)|Yüzde|Maksimum||None|
|bağlı istemciler8|Bağlı İstemciler (Shard 8)|Sayı|Maksimum||None|
|totalcommandsprocessed8|Toplam İşlemler (Shard 8)|Sayı|Toplam||None|
|önbellek8|Önbellek Hits (Shard 8)|Sayı|Toplam||None|
|önbellek8|Önbellek Misses (Shard 8)|Sayı|Toplam||None|
|getkomutları8|Gets (Shard 8)|Sayı|Toplam||None|
|setkomutları8|Setler (Shard 8)|Sayı|Toplam||None|
|operasyonlarPerSecond8|Saniye başına işlemler (Shard 8)|Sayı|Maksimum||None|
|tahliye tuşları8|Tahliye Edilen Anahtarlar (Shard 8)|Sayı|Toplam||None|
|toplam tuşlar8|Toplam Anahtar (Shard 8)|Sayı|Maksimum||None|
|süresi dolmuş tuşlar8|Süresi Dolmuş Anahtarlar (Shard 8)|Sayı|Toplam||None|
|usedmemory8|İkinci El Bellek (Shard 8)|Bayt|Maksimum||None|
|usedmemoryRss8|İkinci el Bellek RSS (Shard 8)|Bayt|Maksimum||None|
|serverLoad8|Sunucu Yükü (Shard 8)|Yüzde|Maksimum||None|
|önbellekWrite8|Önbellek Yazma (Shard 8)|BaytPerİkinci|Maksimum||None|
|önbellekRead8|Önbellek Okuma (Shard 8)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime8|Cpu (Shard 8)|Yüzde|Maksimum||None|
|bağlı istemciler9|Bağlı İstemciler (Shard 9)|Sayı|Maksimum||None|
|totalcommandsprocessed9|Toplam İşlemler (Shard 9)|Sayı|Toplam||None|
|önbellek9|Önbellek Hits (Shard 9)|Sayı|Toplam||None|
|önbellekler9|Önbellek Misses (Shard 9)|Sayı|Toplam||None|
|getkomutları9|Gets (Shard 9)|Sayı|Toplam||None|
|setkomutları9|Setler (Shard 9)|Sayı|Toplam||None|
|operasyonlarPerSecond9|Saniye başına işlemler (Shard 9)|Sayı|Maksimum||None|
|tahliye tuşları9|Tahliye Edilen Anahtarlar (Shard 9)|Sayı|Toplam||None|
|toplam tuşlar9|Toplam Anahtar (Shard 9)|Sayı|Maksimum||None|
|süresi dolmuş anahtarlar9|Süresi Dolmuş Anahtarlar (Shard 9)|Sayı|Toplam||None|
|usedmemory9|İkinci El Bellek (Shard 9)|Bayt|Maksimum||None|
|usedmemoryRss9|İkinci el Bellek RSS (Shard 9)|Bayt|Maksimum||None|
|serverLoad9|Sunucu Yükü (Shard 9)|Yüzde|Maksimum||None|
|önbellekWrite9|Önbellek Yazma (Shard 9)|BaytPerİkinci|Maksimum||None|
|önbellekRead9|Önbellek Okuma (Shard 9)|BaytPerİkinci|Maksimum||None|
|yüzdeProcessorTime9|Cpu (Shard 9)|Yüzde|Maksimum||None|




## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft.Cdn/cdnwebapplicationfirewallpolicies

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|WebApplicationFirewallRequestCount|Web Uygulaması Güvenlik Duvarı İstek Sayısı|Sayı|Toplam|Web Uygulaması Güvenlik Duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName,RuleName,Eylem|


## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makine(ler) tarafından şu anda kullanılmakta olan ayrılan bilgi işlem birimlerinin yüzdesi.|None|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makine(ler) (Gelen Trafik) tarafından tüm ağ arabirimlerinde alınan bayt sayısı.|None|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makine(ler) (Giden Trafik) tarafından tüm ağ arabirimlerinde bayt sayısı.|None|
|Disk Okuma Bayt/Sn|Disk Okuma|BaytPerİkinci|Ortalama|İzleme döneminde diskten okunan ortalama baytsayısı.|None|
|Disk Bayt/Sn Yazma|Disk Yazma|BaytPerİkinci|Ortalama|İzleme döneminde diske yazılan ortalama baytlar.|None|
|Disk Okuma İşlemleri/Sn|Disk Okuma İşlemleri/Sn|CountPerSecond|Ortalama|Disk IOPS okuyun.|None|
|Disk Yazma İşlemleri/Sn|Disk Yazma İşlemleri/Sn|CountPerSecond|Ortalama|Disk IOPS yazın.|None|


## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makine(ler) tarafından şu anda kullanılmakta olan ayrılan bilgi işlem birimlerinin yüzdesi.|RoleInstanceId|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal Makine(ler) (Gelen Trafik) tarafından tüm ağ arabirimlerinde alınan bayt sayısı.|RoleInstanceId|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal Makine(ler) (Giden Trafik) tarafından tüm ağ arabirimlerinde bayt sayısı.|RoleInstanceId|
|Disk Okuma Bayt/Sn|Disk Okuma|BaytPerİkinci|Ortalama|İzleme döneminde diskten okunan ortalama baytsayısı.|RoleInstanceId|
|Disk Bayt/Sn Yazma|Disk Yazma|BaytPerİkinci|Ortalama|İzleme döneminde diske yazılan ortalama baytlar.|RoleInstanceId|
|Disk Okuma İşlemleri/Sn|Disk Okuma İşlemleri/Sn|CountPerSecond|Ortalama|Disk IOPS okuyun.|RoleInstanceId|
|Disk Yazma İşlemleri/Sn|Disk Yazma İşlemleri/Sn|CountPerSecond|Ortalama|Disk IOPS yazın.|RoleInstanceId|



## <a name="microsoftclassicstoragestorageaccounts"></a>Microsoft.ClassicStorage/storageHesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanılmış Kapasite|Kullanılmış kapasite|Bayt|Ortalama|Hesap kullanılan kapasite|None|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Kimlik Doğrulama|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure Depolama tarafından başarılı bir isteği milisaniye cinsinden işlemek için kullanılan gecikme sonu. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama|

## <a name="microsoftclassicstoragestorageaccountsblobservices"></a>Microsoft.ClassicStorage/storageAccounts/blobServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobKapasite|Blob Kapasitesi|Bayt|Ortalama|Depolama hesabının Blob hizmeti tarafından baytlarda kullanılan depolama miktarı.|BlobType,Seviye|
|BlobCount|Blob Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType,Seviye|
|Konteyner Sayısı|Blob Konteyner Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki kapsayıcı sayısı.|None|
|Dizin Kapasitesi|Dizin Kapasitesi|Bayt|Ortalama|ADLS Gen2 (Hiyerarşik) Endeksi tarafından bayt cinsinden kullanılan depolama miktarı.|None|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Kimlik Doğrulama|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure Depolama tarafından başarılı bir isteği milisaniye cinsinden işlemek için kullanılan gecikme sonu. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama|

## <a name="microsoftclassicstoragestorageaccountstableservices"></a>Microsoft.ClassicStorage/storageAccounts/tableServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Tablo Kapasitesi|Tablo Kapasitesi|Bayt|Ortalama|Depolama hesabının Tablo hizmeti tarafından baytlarda kullanılan depolama miktarı.|None|
|Tablo Sayısı|Tablo Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo sayısı.|None|
|TabloEntityCount|Tablo Varlık Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo varlıkların sayısı.|None|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Kimlik Doğrulama|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure Depolama tarafından başarılı bir isteği milisaniye cinsinden işlemek için kullanılan gecikme sonu. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama|

## <a name="microsoftclassicstoragestorageaccountsfileservices"></a>Microsoft.ClassicStorage/storageAccounts/fileServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Dosya Kapasitesi|Dosya Kapasitesi|Bayt|Ortalama|Depolama hesabının Dosya hizmeti tarafından baytlarda kullanılan depolama alanı miktarı.|Fileshare|
|Dosya Sayısı|Dosya Sayısı|Sayı|Ortalama|Depolama hesabının Dosya hizmetindeki dosya sayısı.|Fileshare|
|FileShareCount|Dosya Paylaşım Sayısı|Sayı|Ortalama|Depolama hesabının Dosya hizmetindeki dosya paylaşımlarının sayısı.|None|
|FileShareSnapshotCount|Dosya Paylaşımı Anlık Görüntü Sayısı|Sayı|Ortalama|Depolama hesabının Dosyalar Hizmeti'ndeki paylaşımda bulunan anlık görüntü sayısı.|Fileshare|
|FileShareSnapshotSize|Dosya Paylaşımı Anlık Görüntü Boyutu|Bayt|Ortalama|Depolama hesabının Dosya hizmetindeki anlık görüntüler tarafından baytlarda kullanılan depolama alanı miktarı.|Fileshare|
|FileShareQuota|Dosya paylaşımı kota boyutu|Bayt|Ortalama|Azure Dosyalar Hizmeti tarafından baytolarak kullanılabilecek depolama alanı miktarının üst sınırı.|Fileshare|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Authentication,FileShare|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama,FileShare|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama,FileShare|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure Depolama tarafından başarılı bir isteği milisaniye cinsinden işlemek için kullanılan gecikme sonu. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama,FileShare|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama,FileShare|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama,FileShare|

## <a name="microsoftclassicstoragestorageaccountsqueueservices"></a>Microsoft.ClassicStorage/storageAccounts/queueServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Sıra Kapasitesi|Sıra Kapasitesi|Bayt|Ortalama|Depolama hesabının Sıra hizmeti tarafından baytlarda kullanılan depolama alanı miktarı.|None|
|Sıra Sayısı|Sıra Sayısı|Sayı|Ortalama|Depolama hesabının Sıra hizmetindeki sıra sayısı.|None|
|QueueMessageCount|Sıra İleti Sayısı|Sayı|Ortalama|Depolama hesabının Sıra hizmetindeki yaklaşık sıra ileti sayısı.|None|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Kimlik Doğrulama|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure Depolama tarafından başarılı bir isteği milisaniye cinsinden işlemek için kullanılan gecikme sonu. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/hesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Toplam Aramalar|Toplam Aramalar|Sayı|Toplam|Toplam arama sayısı.|ApiName,OperationName,Bölge|
|Başarılı Aramalar|Başarılı Aramalar|Sayı|Toplam|Başarılı aramaların sayısı.|ApiName,OperationName,Bölge|
|Toplam Hatalar|Toplam Hatalar|Sayı|Toplam|Hata yanıtı içeren toplam çağrı sayısı (HTTP yanıt kodu 4xx veya 5xx).|ApiName,OperationName,Bölge|
|Engellenen Aramalar|Engellenen Aramalar|Sayı|Toplam|Oran veya kota sınırını aşan arama ların sayısı.|ApiName,OperationName,Bölge|
|Sunucu Hataları|Sunucu Hataları|Sayı|Toplam|Hizmet iç hatası olan arama ların sayısı (HTTP yanıt kodu 5xx).|ApiName,OperationName,Bölge|
|İstemci Hataları|İstemci Hataları|Sayı|Toplam|İstemci tarafı hatası olan arama ların sayısı (HTTP yanıt kodu 4xx).|ApiName,OperationName,Bölge|
|DataIn|Veri In|Bayt|Toplam|Baytlarda gelen verilerin boyutu.|ApiName,OperationName,Bölge|
|Veri Out|Veri Çıkış|Bayt|Toplam|Baytlarda giden verilerin boyutu.|ApiName,OperationName,Bölge|
|Gecikme süresi|Gecikme süresi|Milisaniye|Ortalama|Milisaniye cinsinden gecikme.|ApiName,OperationName,Bölge|
|ToplamTokenAramalar|Toplam Belirteç Çağrıları|Sayı|Toplam|Toplam belirteç aramasayısı.|ApiName,OperationName,Bölge|
|KarakterlerÇevrilen|Çevrilen Karakterler|Sayı|Toplam|Gelen metin isteğindeki toplam karakter sayısı.|ApiName,OperationName,Bölge|
|KarakterlerEğitilmiş|Karakterler Eğitilmiş|Sayı|Toplam|Toplam karakter sayısı eğitildi.|ApiName,OperationName,Bölge|
|Konuşma Süresi|Konuşma Oturumu Süresi|Saniye|Toplam|Saniye cinsinden toplam konuşma oturumu süresi.|ApiName,OperationName,Bölge|
|Toplam İşlemler|Toplam İşlemler|Sayı|Toplam|Toplam hareket sayısı.|None|
|İşlenmiş Görüntüler|İşlenmiş Görüntüler|Sayı|Toplam| Görüntü işleme için Hareket Sayısı.|ApiName,FeatureName,Kanal,Bölge|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makine(ler) tarafından şu anda kullanılmakta olan ayrılan bilgi işlem birimlerinin yüzdesi|None|
|Ağ Girişi|Faturalandırılabilir Ağ (Amortismana Tabi)|Bayt|Toplam|Sanal Makine(ler) (Gelen Trafik) (Deprecated) tarafından tüm ağ arabirimlerinde alınan faturalı bayt sayısı|None|
|Ağ Çıkışı|Ağ Out Faturalanabilir (Deprecated)|Bayt|Toplam|Sanal Makine(ler) (Giden Trafik) (Deprecated) tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı|None|
|Disk Okuma Baytları|Disk Okuma Baytları|Bayt|Toplam|İzleme süresi boyunca diskten okunan baytlar|None|
|Disk Yazma Baytları|Disk Yazma Baytları|Bayt|Toplam|İzleme döneminde diske yazılan baytlar|None|
|Disk Okuma İşlemleri/Sn|Disk Okuma İşlemleri/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|None|
|Disk Yazma İşlemleri/Sn|Disk Yazma İşlemleri/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|None|
|CPU Kredileri Kalan|CPU Kredileri Kalan|Sayı|Ortalama|Patlamaiçin kullanılabilir toplam kredi sayısı|None|
|Cpu Kredileri Tüketilen|Cpu Kredileri Tüketilen|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|None|
|Disk Başına Okuma Bayt/sn|Veri Diski Okuma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca tek bir diskten okunan bayt/Sn|Tembellik|
|Disk Başına Bayt/sn Yazma|Veri Diski Yazma Bayt/Sn [(Amortismana Eritilmiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan bayt/Sn|Tembellik|
|Disk Başına Okuma İşlemleri/Sn|Veri Diski Okuma İşlemleri/Sn [(Amortismana Alındı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS'yi okuyun|Tembellik|
|Disk Başına Yazma İşlemleri/Sn|Veri Diski Yazma İşlemleri/Sn [(Amortismana Alındı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS yazma|Tembellik|
|Disk Başına QD|Veri Diski QD [(Deprecated)](portal-disk-metrics-deprecation.md)](portal-disk-metrics-deprecation.md)|Sayı|Ortalama|Veri DiskI Sıra Derinliği(veya Sıra Uzunluğu)|Tembellik|
|Disk Başına İşletim Sistemi Okuma Bayt/sn|İşletim Sistemi Disk Okuma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diskten okunan bayt/Sn|None|
|Disk Başına İşletim Sistemi Bayt/sn Yazma|İşletim Sistemi Disk I Yazma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diske yazılan bayt/Sn|None|
|Disk Başına İşletim Sistemi Okuma İşlemleri/Sn|İşletim Sistemi Disk Okuma İşlemleri/Sn [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|Os diski için izleme süresi boyunca IOPS'yi tek bir diskten okuyun|None|
|Disk Başına İşletim Sistemi Yazma İşlemleri/Sn|İşletim Sistemi Disk Yazma İşlemleri/Sn [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|OS diski için izleme süresi boyunca tek bir diskten IOPS yazma|None|
|Disk Başına İşletim Sistemi QD|İşletim Sistemi Disk QD [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|Sayı|Ortalama|İşletim Sistemi Disk Sıra Derinliği(veya Sıra Uzunluğu)|None|
|Veri Diski Okuma Bayt/sn|Veri Diski Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca tek bir diskten okunan bayt/Sn|Lun|
|Veri Diski Bayt/sn Yazma|Veri Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan bayt/Sn|Lun|
|Veri Diski Okuma İşlemleri/Sn|Veri Diski Okuma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS'yi okuyun|Lun|
|Veri Disk ihyası Işlemleri/Sn|Veri Disk ihyası işlemleri/sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS yazma|Lun|
|Veri Diski Kuyruk Derinliği|Veri DiskI Sıra Derinliği (Önizleme)|Sayı|Ortalama|Veri DiskI Sıra Derinliği(veya Sıra Uzunluğu)|Lun|
|İşletim Sistemi Disk II Bayt/sn|İşletim Sistemi Disk Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diskten okunan bayt/Sn|None|
|İşletim Sistemi Disk IYazma Bayt/sn|İşletim Sistemi Disk I Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diske yazılan bayt/Sn|None|
|İşletim Sistemi Disk Okuma İşlemleri/Sn|İşletim Sistemi Disk Okuma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|Os diski için izleme süresi boyunca IOPS'yi tek bir diskten okuyun|None|
|İşletim Sistemi Disk Yazma İşlemleri/Sn|İşletim Sistemi Disk Yazma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|OS diski için izleme süresi boyunca tek bir diskten IOPS yazma|None|
|İşletim Sistemi Diski Kuyruk Derinliği|İşletim Sistemi Disk Sıra Derinliği (Önizleme)|Sayı|Ortalama|İşletim Sistemi Disk Sıra Derinliği(veya Sıra Uzunluğu)|None|
|Gelen Akışlar|Gelen Akışlar|Sayı|Ortalama|Gelen Akışlar, gelen yöndeki akım akışlarının sayısıdır (VM'ye giden trafik)|None|
|Giden Akışlar|Giden Akışlar|Sayı|Ortalama|Giden Akışlar, giden yöndeki akım akışlarının sayısıdır (VM'den çıkan trafik)|None|
|Gelen Akışlar Maksimum Oluşturma Oranı|Gelen Akışlar Maksimum Oluşturma Oranı|CountPerSecond|Ortalama|Gelen akışların maksimum oluşturma hızı (VM'ye giden trafik)|None|
|Giden Akışlar Maksimum Oluşturma Oranı|Giden Akışlar Maksimum Oluşturma Oranı|CountPerSecond|Ortalama|Giden akışların maksimum oluşturma oranı (VM'den çıkan trafik)|None|
|Premium Veri Disk Önbelleği Oku Hiti|Premium Veri Disk Önbelleği Oku Hiti (Önizleme)|Yüzde|Ortalama|Premium Veri Disk Önbelleği Oku Hiti|Lun|
|Premium Veri Disk Önbelleği Okuma Miss|Premium Veri Disk Önbelleği Okuma Miss (Önizleme)|Yüzde|Ortalama|Premium Veri Disk Önbelleği Okuma Miss|Lun|
|Premium OS Disk Önbelleği Hit Oku|Premium İşletim Sistemi Disk Önbelleği Hit'i Oku (Önizleme)|Yüzde|Ortalama|Premium OS Disk Önbelleği Hit Oku|None|
|Premium OS Disk Önbelleği Okuma Miss|Premium İşletim Sistemi Disk Önbelleği Okuma Miss (Önizleme)|Yüzde|Ortalama|Premium OS Disk Önbelleği Okuma Miss|None|
|Toplam Ağ|Toplam Ağ|Bayt|Toplam|Sanal Makine(ler) (Gelen Trafik) tarafından tüm ağ arabirimlerinde alınan bayt sayısı|None|
|Ağ Çıkış Toplamı|Ağ Çıkış Toplamı|Bayt|Toplam|Sanal Makine(ler) (Giden Trafik) tarafından tüm ağ arabirimlerinde bayt sayısı|None|


## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makine(ler) tarafından şu anda kullanılmakta olan ayrılan bilgi işlem birimlerinin yüzdesi|VMName|
|Ağ Girişi|Faturalandırılabilir Ağ (Amortismana Tabi)|Bayt|Toplam|Sanal Makine(ler) (Gelen Trafik) (Deprecated) tarafından tüm ağ arabirimlerinde alınan faturalı bayt sayısı|VMName|
|Ağ Çıkışı|Ağ Out Faturalanabilir (Deprecated)|Bayt|Toplam|Sanal Makine(ler) (Giden Trafik) (Deprecated) tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı|VMName|
|Disk Okuma Baytları|Disk Okuma Baytları|Bayt|Toplam|İzleme süresi boyunca diskten okunan baytlar|VMName|
|Disk Yazma Baytları|Disk Yazma Baytları|Bayt|Toplam|İzleme döneminde diske yazılan baytlar|VMName|
|Disk Okuma İşlemleri/Sn|Disk Okuma İşlemleri/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|VMName|
|Disk Yazma İşlemleri/Sn|Disk Yazma İşlemleri/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|VMName|
|CPU Kredileri Kalan|CPU Kredileri Kalan|Sayı|Ortalama|Patlamaiçin kullanılabilir toplam kredi sayısı|None|
|Cpu Kredileri Tüketilen|Cpu Kredileri Tüketilen|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|None|
|Disk Başına Okuma Bayt/sn|Veri Diski Okuma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca tek bir diskten okunan bayt/Sn|Tembellik|
|Disk Başına Bayt/sn Yazma|Veri Diski Yazma Bayt/Sn [(Amortismana Eritilmiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan bayt/Sn|Tembellik|
|Disk Başına Okuma İşlemleri/Sn|Veri Diski Okuma İşlemleri/Sn [(Amortismana Alındı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS'yi okuyun|Tembellik|
|Disk Başına Yazma İşlemleri/Sn|Veri Diski Yazma İşlemleri/Sn [(Amortismana Alındı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS yazma|Tembellik|
|Disk Başına QD|Veri Diski QD [(Deprecated)](portal-disk-metrics-deprecation.md)|Sayı|Ortalama|Veri DiskI Sıra Derinliği(veya Sıra Uzunluğu)|Tembellik|
|Disk Başına İşletim Sistemi Okuma Bayt/sn|İşletim Sistemi Disk Okuma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diskten okunan bayt/Sn|None|
|Disk Başına İşletim Sistemi Bayt/sn Yazma|İşletim Sistemi Disk I Yazma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diske yazılan bayt/Sn|None|
|Disk Başına İşletim Sistemi Okuma İşlemleri/Sn|İşletim Sistemi Disk Okuma İşlemleri/Sn [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|Os diski için izleme süresi boyunca IOPS'yi tek bir diskten okuyun|None|
|Disk Başına İşletim Sistemi Yazma İşlemleri/Sn|İşletim Sistemi Disk Yazma İşlemleri/Sn [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|OS diski için izleme süresi boyunca tek bir diskten IOPS yazma|None|
|Disk Başına İşletim Sistemi QD|İşletim Sistemi Disk QD [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|Sayı|Ortalama|İşletim Sistemi Disk Sıra Derinliği(veya Sıra Uzunluğu)|None|
|Veri Diski Okuma Bayt/sn|Veri Diski Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca tek bir diskten okunan bayt/Sn|LUN,VMName|
|Veri Diski Bayt/sn Yazma|Veri Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan bayt/Sn|LUN,VMName|
|Veri Diski Okuma İşlemleri/Sn|Veri Diski Okuma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS'yi okuyun|LUN,VMName|
|Veri Disk ihyası Işlemleri/Sn|Veri Disk ihyası işlemleri/sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS yazma|LUN,VMName|
|Veri Diski Kuyruk Derinliği|Veri DiskI Sıra Derinliği (Önizleme)|Sayı|Ortalama|Veri DiskI Sıra Derinliği(veya Sıra Uzunluğu)|LUN,VMName|
|İşletim Sistemi Disk II Bayt/sn|İşletim Sistemi Disk Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diskten okunan bayt/Sn|VMName|
|İşletim Sistemi Disk IYazma Bayt/sn|İşletim Sistemi Disk I Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diske yazılan bayt/Sn|VMName|
|İşletim Sistemi Disk Okuma İşlemleri/Sn|İşletim Sistemi Disk Okuma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|Os diski için izleme süresi boyunca IOPS'yi tek bir diskten okuyun|VMName|
|İşletim Sistemi Disk Yazma İşlemleri/Sn|İşletim Sistemi Disk Yazma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|OS diski için izleme süresi boyunca tek bir diskten IOPS yazma|VMName|
|İşletim Sistemi Diski Kuyruk Derinliği|İşletim Sistemi Disk Sıra Derinliği (Önizleme)|Sayı|Ortalama|İşletim Sistemi Disk Sıra Derinliği(veya Sıra Uzunluğu)|VMName|
|Gelen Akışlar|Gelen Akışlar|Sayı|Ortalama|Gelen Akışlar, gelen yöndeki akım akışlarının sayısıdır (VM'ye giden trafik)|VMName|
|Giden Akışlar|Giden Akışlar|Sayı|Ortalama|Giden Akışlar, giden yöndeki akım akışlarının sayısıdır (VM'den çıkan trafik)|VMName|
|Gelen Akışlar Maksimum Oluşturma Oranı|Gelen Akışlar Maksimum Oluşturma Oranı|CountPerSecond|Ortalama|Gelen akışların maksimum oluşturma hızı (VM'ye giden trafik)|VMName|
|Giden Akışlar Maksimum Oluşturma Oranı|Giden Akışlar Maksimum Oluşturma Oranı|CountPerSecond|Ortalama|Giden akışların maksimum oluşturma oranı (VM'den çıkan trafik)|VMName|
|Premium Veri Disk Önbelleği Oku Hiti|Premium Veri Disk Önbelleği Oku Hiti (Önizleme)|Yüzde|Ortalama|Premium Veri Disk Önbelleği Oku Hiti|LUN,VMName|
|Premium Veri Disk Önbelleği Okuma Miss|Premium Veri Disk Önbelleği Okuma Miss (Önizleme)|Yüzde|Ortalama|Premium Veri Disk Önbelleği Okuma Miss|LUN,VMName|
|Premium OS Disk Önbelleği Hit Oku|Premium İşletim Sistemi Disk Önbelleği Hit'i Oku (Önizleme)|Yüzde|Ortalama|Premium OS Disk Önbelleği Hit Oku|VMName|
|Premium OS Disk Önbelleği Okuma Miss|Premium İşletim Sistemi Disk Önbelleği Okuma Miss (Önizleme)|Yüzde|Ortalama|Premium OS Disk Önbelleği Okuma Miss|VMName|
|Toplam Ağ|Toplam Ağ|Bayt|Toplam|Sanal Makine(ler) (Gelen Trafik) tarafından tüm ağ arabirimlerinde alınan bayt sayısı|VMName|
|Ağ Çıkış Toplamı|Ağ Çıkış Toplamı|Bayt|Toplam|Sanal Makine(ler) (Giden Trafik) tarafından tüm ağ arabirimlerinde bayt sayısı|VMName|


## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makine(ler) tarafından şu anda kullanılmakta olan ayrılan bilgi işlem birimlerinin yüzdesi|None|
|Ağ Girişi|Faturalandırılabilir Ağ (Amortismana Tabi)|Bayt|Toplam|Sanal Makine(ler) (Gelen Trafik) (Deprecated) tarafından tüm ağ arabirimlerinde alınan faturalı bayt sayısı|None|
|Ağ Çıkışı|Ağ Out Faturalanabilir (Deprecated)|Bayt|Toplam|Sanal Makine(ler) (Giden Trafik) (Deprecated) tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı|None|
|Disk Okuma Baytları|Disk Okuma Baytları|Bayt|Toplam|İzleme süresi boyunca diskten okunan baytlar|None|
|Disk Yazma Baytları|Disk Yazma Baytları|Bayt|Toplam|İzleme döneminde diske yazılan baytlar|None|
|Disk Okuma İşlemleri/Sn|Disk Okuma İşlemleri/Sn|CountPerSecond|Ortalama|Disk Okuma IOPS|None|
|Disk Yazma İşlemleri/Sn|Disk Yazma İşlemleri/Sn|CountPerSecond|Ortalama|Disk Yazma IOPS|None|
|CPU Kredileri Kalan|CPU Kredileri Kalan|Sayı|Ortalama|Patlamaiçin kullanılabilir toplam kredi sayısı|None|
|Cpu Kredileri Tüketilen|Cpu Kredileri Tüketilen|Sayı|Ortalama|Sanal Makine tarafından tüketilen toplam kredi sayısı|None|
|Disk Başına Okuma Bayt/sn|Veri Diski Okuma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca tek bir diskten okunan bayt/Sn|Tembellik|
|Disk Başına Bayt/sn Yazma|Veri Diski Yazma Bayt/Sn [(Amortismana Eritilmiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan bayt/Sn|Tembellik|
|Disk Başına Okuma İşlemleri/Sn|Veri Diski Okuma İşlemleri/Sn [(Amortismana Alındı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS'yi okuyun|Tembellik|
|Disk Başına Yazma İşlemleri/Sn|Veri Diski Yazma İşlemleri/Sn [(Amortismana Alındı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS yazma|Tembellik|
|Disk Başına QD|Veri Diski QD [(Deprecated)](portal-disk-metrics-deprecation.md)|Sayı|Ortalama|Veri DiskI Sıra Derinliği(veya Sıra Uzunluğu)|Tembellik|
|Disk Başına İşletim Sistemi Okuma Bayt/sn|İşletim Sistemi Disk Okuma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diskten okunan bayt/Sn|None|
|Disk Başına İşletim Sistemi Bayt/sn Yazma|İşletim Sistemi Disk I Yazma Bayt/Sn [(Amortismana Ermiş)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diske yazılan bayt/Sn|None|
|Disk Başına İşletim Sistemi Okuma İşlemleri/Sn|İşletim Sistemi Disk Okuma İşlemleri/Sn [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|Os diski için izleme süresi boyunca IOPS'yi tek bir diskten okuyun|None|
|Disk Başına İşletim Sistemi Yazma İşlemleri/Sn|İşletim Sistemi Disk Yazma İşlemleri/Sn [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|CountPerSecond|Ortalama|OS diski için izleme süresi boyunca tek bir diskten IOPS yazma|None|
|Disk Başına İşletim Sistemi QD|İşletim Sistemi Disk QD [(Amortismana Uğradı)](portal-disk-metrics-deprecation.md)|Sayı|Ortalama|İşletim Sistemi Disk Sıra Derinliği(veya Sıra Uzunluğu)|None|
|Veri Diski Okuma Bayt/sn|Veri Diski Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca tek bir diskten okunan bayt/Sn|Lun|
|Veri Diski Bayt/sn Yazma|Veri Diski Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme döneminde tek bir diske yazılan bayt/Sn|Lun|
|Veri Diski Okuma İşlemleri/Sn|Veri Diski Okuma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS'yi okuyun|Lun|
|Veri Disk ihyası Işlemleri/Sn|Veri Disk ihyası işlemleri/sn (Önizleme)|CountPerSecond|Ortalama|İzleme süresi boyunca Tek bir diskten IOPS yazma|Lun|
|Veri Diski Kuyruk Derinliği|Veri DiskI Sıra Derinliği (Önizleme)|Sayı|Ortalama|Veri DiskI Sıra Derinliği(veya Sıra Uzunluğu)|Lun|
|İşletim Sistemi Disk II Bayt/sn|İşletim Sistemi Disk Okuma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diskten okunan bayt/Sn|None|
|İşletim Sistemi Disk IYazma Bayt/sn|İşletim Sistemi Disk I Yazma Bayt/Sn (Önizleme)|CountPerSecond|Ortalama|İşletim sistemi diski için izleme süresi boyunca tek bir diske yazılan bayt/Sn|None|
|İşletim Sistemi Disk Okuma İşlemleri/Sn|İşletim Sistemi Disk Okuma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|Os diski için izleme süresi boyunca IOPS'yi tek bir diskten okuyun|None|
|İşletim Sistemi Disk Yazma İşlemleri/Sn|İşletim Sistemi Disk Yazma İşlemleri/Sn (Önizleme)|CountPerSecond|Ortalama|OS diski için izleme süresi boyunca tek bir diskten IOPS yazma|None|
|İşletim Sistemi Diski Kuyruk Derinliği|İşletim Sistemi Disk Sıra Derinliği (Önizleme)|Sayı|Ortalama|İşletim Sistemi Disk Sıra Derinliği(veya Sıra Uzunluğu)|None|
|Gelen Akışlar|Gelen Akışlar|Sayı|Ortalama|Gelen Akışlar, gelen yöndeki akım akışlarının sayısıdır (VM'ye giden trafik)|None|
|Giden Akışlar|Giden Akışlar|Sayı|Ortalama|Giden Akışlar, giden yöndeki akım akışlarının sayısıdır (VM'den çıkan trafik)|None|
|Gelen Akışlar Maksimum Oluşturma Oranı|Gelen Akışlar Maksimum Oluşturma Oranı|CountPerSecond|Ortalama|Gelen akışların maksimum oluşturma hızı (VM'ye giden trafik)|None|
|Giden Akışlar Maksimum Oluşturma Oranı|Giden Akışlar Maksimum Oluşturma Oranı|CountPerSecond|Ortalama|Giden akışların maksimum oluşturma oranı (VM'den çıkan trafik)|None|
|Premium Veri Disk Önbelleği Oku Hiti|Premium Veri Disk Önbelleği Oku Hiti (Önizleme)|Yüzde|Ortalama|Premium Veri Disk Önbelleği Oku Hiti|Lun|
|Premium Veri Disk Önbelleği Okuma Miss|Premium Veri Disk Önbelleği Okuma Miss (Önizleme)|Yüzde|Ortalama|Premium Veri Disk Önbelleği Okuma Miss|Lun|
|Premium OS Disk Önbelleği Hit Oku|Premium İşletim Sistemi Disk Önbelleği Hit'i Oku (Önizleme)|Yüzde|Ortalama|Premium OS Disk Önbelleği Hit Oku|None|
|Premium OS Disk Önbelleği Okuma Miss|Premium İşletim Sistemi Disk Önbelleği Okuma Miss (Önizleme)|Yüzde|Ortalama|Premium OS Disk Önbelleği Okuma Miss|None|
|Toplam Ağ|Toplam Ağ|Bayt|Toplam|Sanal Makine(ler) (Gelen Trafik) tarafından tüm ağ arabirimlerinde alınan bayt sayısı|None|
|Ağ Çıkış Toplamı|Ağ Çıkış Toplamı|Bayt|Toplam|Sanal Makine(ler) (Giden Trafik) tarafından tüm ağ arabirimlerinde bayt sayısı|None|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuKullanımı|CPU Kullanımı|Sayı|Ortalama|Milicores tüm çekirdeklerde CPU kullanımı.|containerName|
|MemoryUsage|Bellek Kullanımı|Bayt|Ortalama|Bayt toplam bellek kullanımı.|containerName|
|AğBaytReceivedPerSecond|Ağ Baytları Saniyede Alındı|Bayt|Ortalama|Ağ baytları saniyede alındı.|None|
|AğBaytTransmittedPerSecond|Saniyede Aktarılan Ağ Baytları|Bayt|Ortalama|Ağ baytları saniyede aktarılır.|None|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Toplam PullCount|Toplam Çekme Sayısı|Sayı|Ortalama|Toplam da görüntü çekme sayısı|None|
|Başarılı PullCount|Başarılı Çekme Sayısı|Sayı|Ortalama|Başarılı görüntü çekme sayısı|None|
|Toplam PushCount Sayısı|Toplam İtme Sayısı|Sayı|Ortalama|Toplam görüntü itme sayısı|None|
|Başarılı PushCount|Başarılı Push Count|Sayı|Ortalama|Başarılı görüntü itme sayısı|None|
|Çalışma Süresi|Çalışma Süresi|Milisaniye|Toplam|Süreyi milisaniye cinsinden çalıştırın|None|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Yönetilen kümedeki kullanılabilir işlemci çekirdeğinin toplam sayısı|Sayı|Ortalama|Yönetilen kümedeki kullanılabilir işlemci çekirdeğinin toplam sayısı|None|
|kube_node_status_allocatable_memory_bytes|Yönetilen kümedeki kullanılabilir bellek toplam miktarı|Bayt|Ortalama|Yönetilen kümedeki kullanılabilir bellek toplam miktarı|None|
|kube_pod_status_ready|Hazır durumdaki bölme sayısı|Sayı|Ortalama|Hazır durumdaki bölme sayısı|namespace,pod|
|kube_node_status_condition|Çeşitli düğüm koşulları için durumlar|Sayı|Ortalama|Çeşitli düğüm koşulları için durumlar|durum,durum,durum2,düğüm|
|kube_pod_status_phase|Faza göre bölme sayısı|Sayı|Ortalama|Faza göre bölme sayısı|faz,ad alanı,pod|



## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft.CustomProviders/resourceproviders

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Başarılı İstekler|Başarılı İstekler|Sayı|Toplam|Özel sağlayıcı tarafından yapılan başarılı istekler|httpMethod,CallPath,StatusCode|
|Başarısız İstekler|Başarısız İstekler|Sayı|Toplam|Özel Kaynak Sağlayıcıları için kullanılabilir günlükleri alır|httpMethod,CallPath,StatusCode|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|NICReadThroughput|Okuma Throughput (Ağ)|BaytPerİkinci|Ortalama|Ağ geçidindeki tüm birimler için raporlama döneminde aygıttaki ağ arabiriminin okuma iş hacmi.|Örnekadı|
|NICWriteThroughput|Yazma Throughput (Ağ)|BaytPerİkinci|Ortalama|Ağ geçidindeki tüm birimler için raporlama döneminde aygıttaki ağ arabiriminin yazma iş hacmi.|Örnekadı|
|CloudReadThroughputPerShare|Bulut İndirme İşİ (Paylaş)|BaytPerİkinci|Ortalama|Raporlama döneminde bir paylaşımdan Azure'a indirme işlemi.|Paylaş|
|CloudUploadThroughputPerShare|Bulut Yükleme İşI (Paylaş)|BaytPerİkinci|Ortalama|Raporlama döneminde bir paylaşımdan Azure'a yükleme girişi.|Paylaş|
|BytesUploadedToCloudPerShare|Cloud Bayt Yüklendi (Paylaş)|Bayt|Ortalama|Raporlama döneminde bir paylaşımdan Azure'a yüklenen toplam bayt sayısı.|Paylaş|
|Toplam Kapasite|Toplam Kapasite|Bayt|Ortalama|Toplam Kapasite|None|
|Kullanılabilir Kapasite|Kullanılabilir Kapasite|Bayt|Ortalama|Raporlama döneminde baytlarda kullanılabilir kapasite.|None|
|CloudUploadThroughput|Bulut Yükleme İşi|BaytPerİkinci|Ortalama|Bulut, raporlama döneminde azure'a iş elde etme tarihine yüklenir.|None|
|CloudReadThroughput|Bulut İndirme İşi|BaytPerİkinci|Ortalama|Raporlama döneminde Azure'a bulut indirme iş girişi.|None|
|BytesUploadedToCloud|Bulut Baytyüklenen (Aygıt)|Bayt|Ortalama|Raporlama döneminde bir cihazdan Azure'a yüklenen toplam bayt sayısı.|None|
|HyperVVirtualProcessorKullanım|Kenar İşlemi - Yüzde Cpu|Yüzde|Ortalama|Yüzde CPU Kullanımı|Örnekadı|
|HyperVMemoryKullanımı|Kenar Hesaplama - Bellek Kullanımı|Yüzde|Ortalama|Kullanımdaki RAM Miktarı|Örnekadı|


## <a name="microsoftdatacatalogdatacatalogs"></a>Microsoft.DataCatalog/datacatalogs

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Varlık Dağılımı ByClassification|Sınıflandırmaya göre varlık dağılımı|Sayı|Toplam|Belirli bir sınıflandırma atanmış varlıkların sayısını gösterir, yani bu etiketle sınıflandırılır.|Sınıflandırma, Kaynak|
|Varlık DağılımıByStorageType|Depolama türüne göre varlık dağılımı|Sayı|Toplam|Belirli bir depolama türüne sahip varlıkların sayısını gösterir.|Depolama Türü|
|NumberOfAssetsWithClassifications|En az bir sınıflandırmaya sahip varlıkların sayısı|Sayı|Ortalama|En az bir etiket sınıflandırması olan varlıkların sayısını gösterir.|None|
|Scanİptal edildi|Tarama İptal Edildi|Sayı|Toplam|İptal edilen tarama sayısını gösterir.|None|
|ScanTamamlandı|Tsama Tamamlandı|Sayı|Toplam|Başarıyla tamamlanan tarama sayısını gösterir.|None|
|ScanFailed|Tolan Başarısız|Sayı|Toplam|Başarısız olan tarama sayısını gösterir.|None|
|ScanTimeTaken|Alınan süreyi tazyikle|Saniye|Toplam|Saniyecinsinden toplam tbmksüresini gösterir.|None|
|CatalogActiveUsers|Günlük Aktif Kullanıcılar|Sayı|Toplam|Günlük aktif kullanıcı sayısı|None|
|KatalogKullanımı|Operasyona Göre Kullanım Dağılımı|Sayı|Toplam|Kullanıcının katalogda yaptığı işlem sayısını, yani Erişim, Arama, Sözlük'ü belirtin.|İşlem|


## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactorys

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Başarısız Çalıştırmalar|Başarısız Çalıştırmalar|Sayı|Toplam||pipelineName,activityName|
|Başarılı Çalışır|Başarılı Koşular|Sayı|Toplam||pipelineName,activityName|


## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/fabrikalar

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Boru HattıFailedRuns|Başarısız ardışık hatlar ölçümleri çalıştırıyor|Sayı|Toplam||FailureType,Adı|
|PipelineSucceededRuns|Başarılı ardışık ardışık hatlar ölçümleri çalışır|Sayı|Toplam||FailureType,Adı|
|PipelineCancelledRuns|İptal edilen ardışık hatlar ölçümleri çalışır|Sayı|Toplam||FailureType,Adı|
|EtkinlikFailedRuns|Başarısız etkinlik ölçümleri çalıştırıyor|Sayı|Toplam||ActivityType,PipelineName,FailureType,Name|
|ActivitySucceededRuns|Başarılı etkinlik ölçümleri çalıştırıyor|Sayı|Toplam||ActivityType,PipelineName,FailureType,Name|
|ActivityCancelledRuns|İptal edilen etkinlik ölçümleri çalıştırıyor|Sayı|Toplam||ActivityType,PipelineName,FailureType,Name|
|TriggerFailedRuns|Başarısız tetikleyici ölçümleri çalıştırıyor|Sayı|Toplam||Ad,FailureType|
|TriggerSucceededRuns|Başarılı tetikleyici ölçümleri çalıştırıyor|Sayı|Toplam||Ad,FailureType|
|TriggerCancelledRuns|İptal edilen tetikleyici ölçümleri çalıştırıyor|Sayı|Toplam||Ad,FailureType|
|IntegrationRuntimeCpuPercentage|Entegrasyon çalışma zamanı CPU kullanımı|Yüzde|Ortalama||IntegrationRuntimeName,DüğümName|
|IntegrationRuntimeKullanılabilirBellek|Tümleştirme çalışma zamanı kullanılabilir bellek|Bayt|Ortalama||IntegrationRuntimeName,DüğümName|
|EntegrasyonRuntimeAverageTaskPickupDelay|Tümleştirme çalışma zamanı sıra süresi|Saniye|Ortalama||IntegrationRuntimeName|
|EntegrasyonRuntimeQueueLength|Tümleştirme çalışma zamanı sıra uzunluğu|Sayı|Ortalama||IntegrationRuntimeName|
|IntegrationRuntimeAvailableNodeNumber|Tümleştirme çalışma zamanı kullanılabilir düğüm sayısı|Sayı|Ortalama||IntegrationRuntimeName|
|MaxAllowedKaynak Sayısı|İzin verilen maksimum varlık sayısı|Sayı|Maksimum||None|
|MaxallowedFactorySizeingbunits|İzin verilen maksimum fabrika boyutu (GB birimi)|Sayı|Maksimum||None|
|Kaynak Sayısı|Toplam varlıklar sayısı|Sayı|Maksimum||None|
|Fabrikasizeingbunits|Toplam fabrika büyüklüğü (GB birimi)|Sayı|Maksimum||None|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/hesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|JobEndedSuccess|Başarılı İşler|Sayı|Toplam|Başarılı işlerin sayısı.|None|
|İş Sonu Hatası|Başarısız İşler|Sayı|Toplam|Başarısız işlerin sayısı.|None|
|İşSon İptal Edildi|İptal Edilen İşler|Sayı|Toplam|İptal edilen işlerin sayısı.|None|
|JobAUEndedSuccess|Başarılı AU Time|Saniye|Toplam|Başarılı işler için toplam AU süresi.|None|
|JobAUEndedFailure|Başarısız AU Süresi|Saniye|Toplam|Başarısız işler için toplam AU süresi.|None|
|JobAUEndedİptal edildi|İptal EDILEN AU Saati|Saniye|Toplam|İptal edilen işler için toplam AU süresi.|None|
|İş Aşaması|Sahnedeki İşler|Sayı|Toplam|Her aşamada iş sayısı.|None|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/hesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Toplam Depolama|Toplam Depolama Alanı|Bayt|Maksimum|Hesapta depolanan toplam veri miktarı.|None|
|Veri Yazısı|Yazılı Veriler|Bayt|Toplam|Hesaba yazılan toplam veri miktarı.|None|
|Veri Okuma|Veri Okuma|Bayt|Toplam|Hesaptan okunan toplam veri miktarı.|None|
|Yazma İstekleri|İstek Yazma|Sayı|Toplam|Hesapa veri yazma istekleri sayısı.|None|
|Okuma İstekleri|İstekleri Okuma|Sayı|Toplam|Hesaptaki istekleri okuyan veri sayısı.|None|


## <a name="microsoftdatashareaccounts"></a>Microsoft.DataShare/hesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Hisse Sayısı|Gönderilen Paylaşımlar|Sayı|Maksimum|Hesapta gönderilen hisse sayısı|Paylaşımadı|
|ShareSubscriptionCount|Alınan Hisseler|Sayı|Maksimum|Hesaptan alınan hisse sayısı|ShareSubscriptionName|
|BaşarılıShareSynchronizations|Gönderilen Paylaşım Başarılı Anlık Görüntüler|Sayı|Sayı|Hesapta başarılı olan paylaşım sayısı|None|
|FailedShareSynchronizations|Gönderilen Paylaşım Başarısız Anlık Görüntüler|Sayı|Sayı|Hesapta gönderilen hisse sayısı başarısız anlık görüntü|None|
|BaşarılıShareSubscriptionSynchronizations|Alınan Paylaşım Başarılı Anlık Görüntüler|Sayı|Sayı|Hesapta başarılı olan hisse sayısı|None|
|FailedShareSubscriptionSynchronizations|Alınan Paylaşım Başarısız Anlık Görüntüler|Sayı|Sayı|Hesapta alınan hisse sayısı başarısız anlık görüntü|None|


## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/sunucular

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|None|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|None|
|io_consumption_percent|IO yüzdesi|Yüzde|Ortalama|IO yüzdesi|None|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|None|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|None|
|storage_limit|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|None|
|serverlog_storage_percent|Sunucu Günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu Günlüğü depolama yüzdesi|None|
|serverlog_storage_usage|Kullanılan Server Log depolama|Bayt|Ortalama|Kullanılan Server Log depolama|None|
|serverlog_storage_limit|Sunucu Günlüğü depolama sınırı|Bayt|Ortalama|Sunucu Günlüğü depolama sınırı|None|
|active_connections|Etkin Bağlantılar|Sayı|Ortalama|Etkin Bağlantılar|None|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|None|
|seconds_behind_master|Saniye cinsinden çoğaltma gecikmesi|Sayı|Maksimum|Saniye cinsinden çoğaltma gecikmesi|None|
|backup_storage_used|Yedek Depolama kullanılan|Bayt|Ortalama|Yedek Depolama kullanılan|None|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında Ağ Dışında|None|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Ağ Etkin bağlantılar arasında|None|


## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/sunucular

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|None|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|None|
|io_consumption_percent|IO yüzdesi|Yüzde|Ortalama|IO yüzdesi|None|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|None|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|None|
|storage_limit|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|None|
|serverlog_storage_percent|Sunucu Günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu Günlüğü depolama yüzdesi|None|
|serverlog_storage_usage|Kullanılan Server Log depolama|Bayt|Ortalama|Kullanılan Server Log depolama|None|
|serverlog_storage_limit|Sunucu Günlüğü depolama sınırı|Bayt|Maksimum|Sunucu Günlüğü depolama sınırı|None|
|active_connections|Etkin Bağlantılar|Sayı|Ortalama|Etkin Bağlantılar|None|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|None|
|seconds_behind_master|Saniye cinsinden çoğaltma gecikmesi|Sayı|Maksimum|Saniye cinsinden çoğaltma gecikmesi|None|
|backup_storage_used|Yedek Depolama kullanılan|Bayt|Ortalama|Yedek Depolama kullanılan|None|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında Ağ Dışında|None|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Ağ Etkin bağlantılar arasında|None|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/sunucular

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|None|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|None|
|io_consumption_percent|IO yüzdesi|Yüzde|Ortalama|IO yüzdesi|None|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|None|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|None|
|storage_limit|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|None|
|serverlog_storage_percent|Sunucu Günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu Günlüğü depolama yüzdesi|None|
|serverlog_storage_usage|Kullanılan Server Log depolama|Bayt|Ortalama|Kullanılan Server Log depolama|None|
|serverlog_storage_limit|Sunucu Günlüğü depolama sınırı|Bayt|Maksimum|Sunucu Günlüğü depolama sınırı|None|
|active_connections|Etkin Bağlantılar|Sayı|Ortalama|Etkin Bağlantılar|None|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|None|
|backup_storage_used|Yedek Depolama kullanılan|Bayt|Ortalama|Yedek Depolama kullanılan|None|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında Ağ Dışında|None|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Ağ Etkin bağlantılar arasında|None|
|pg_replica_log_delay_in_seconds|Çoğaltma Gecikmesi|Saniye|Maksimum|Saniye cinsinden çoğaltma gecikmesi|None|
|pg_replica_log_delay_in_bytes|Çoğaltmalar Arasında Max Lag|Bayt|Maksimum|En geciken yinelemenin baytlarında gecikme|None|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|None|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|None|
|ıop|IOPS|Sayı|Ortalama|Saniyede IO İşlemleri|None|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|None|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|None|
|active_connections|Etkin Bağlantılar|Sayı|Ortalama|Etkin Bağlantılar|None|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında Ağ Dışında|None|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Ağ Etkin bağlantılar arasında|None|


## <a name="microsoftdbforpostgresqlsingleservers"></a>Microsoft.DBforPostgreSQL/singleservers

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|None|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|None|
|ıop|IOPS|Sayı|Ortalama|Saniyede IO İşlemleri|None|
|storage_percent|Depolama yüzdesi|Yüzde|Ortalama|Depolama yüzdesi|None|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|None|
|active_connections|Etkin Bağlantılar|Sayı|Ortalama|Etkin Bağlantılar|None|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında Ağ Dışında|None|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Ağ Etkin bağlantılar arasında|None|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|None|
|connections_succeeded|Başarılı Bağlantılar|Sayı|Toplam|Başarılı Bağlantılar|None|
|maximum_used_transactionIDs|Maksimum Kullanılan İşlem T.C.|Sayı|Ortalama|Maksimum Kullanılan İşlem T.C.|None|





## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetri.ingress.allProtocol|Telemetri iletisi gönderme girişimleri|Sayı|Toplam|IoT hub'ınıza gönderilmeye çalışılan aygıttan buluta telemetri iletilerinin sayısı|None|
|d2c.telemetri.ingress.success|Gönderilen Telemetri iletileri|Sayı|Toplam|IoT hub'ınıza başarıyla gönderilen aygıttan buluta telemetri iletilerinin sayısı|None|
|c2d.commands.egress.complete.success|C2D ileti teslimatları tamamlandı|Sayı|Toplam|Aygıt tarafından başarıyla tamamlanan buluttan cihaza ileti teslimlerinin sayısı|None|
|c2d.commands.egress.abandon.success|C2D iletileri terk edildi|Sayı|Toplam|Aygıt tarafından terk edilen buluttan aygıta ileti sayısı|None|
|c2d.commands.egress.reject.success|C2D iletileri reddedildi|Sayı|Toplam|Aygıt tarafından reddedilen buluttan aygıta ileti sayısı|None|
|C2DMesajlarSüresi Doldu|C2D İletilerinin Süresi Doldu (önizleme)|Sayı|Toplam|Süresi dolmuş buluttan aygıta ileti sayısı|None|
|devices.totalCihazlar|Toplam aygıtlar (amortismana uğradı)|Sayı|Toplam|IoT hub'ınıza kayıtlı aygıt sayısı|None|
|devices.connectedDevices.allProtocol|Bağlı aygıtlar (amortismana uğradı) |Sayı|Toplam|IoT hub'ınıza bağlı aygıt sayısı|None|
|d2c.telemetry.egress.success|Yönlendirme: telemetri iletileri teslim|Sayı|Toplam|IoT Hub yönlendirmesini kullanarak iletilerin tüm uç noktalara başarıyla teslim ediliş sayısı. İleti birden çok uç noktaya yönlendirilirse, bu değer her başarılı teslimat için bir artar. İleti aynı bitiş noktasına birden çok kez teslim edilirse, bu değer her başarılı teslimat için bir artar.|None|
|d2c.telemetry.egress.düştü|Yönlendirme: telemetri iletileri düştü |Sayı|Toplam|İletilerin, çıkmaz noktalar nedeniyle IoT Hub yönlendirmesi tarafından kaç kez bırakıldığı. Bırakılan iletiler burada teslim edilmedikçe, bu değer geri dönüş rotasına teslim edilen iletileri saymaz.|None|
|d2c.telemetry.egress.orphaned|Yönlendirme: telemetri mesajları öksüz |Sayı|Toplam|İletilerin ioT Hub yönlendirmesi tarafından kaç kez değiştirildikleri, çünkü yönlendirme kurallarına (geri dönüş kuralı dahil) eşleşmedikleri için. |None|
|d2c.telemetry.çıkış.geçersiz|Yönlendirme: telemetri iletileri uyumsuz|Sayı|Toplam|IoT Hub yönlendirmesi, bitiş noktasıyla uyumsuzluk nedeniyle iletileri kaç kez teslim edemedi. Bu değer yeniden denemeleri içermez.|None|
|d2c.telemetry.egress.fallback|Yönlendirme: geri dönüşe teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin geri dönüş rotasıyla ilişkili bitiş noktasına iletileri teslim etme sayısı.|None|
|d2c.endpoints.egress.eventHubs|Yönlendirme: Event Hub'a iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin Olay Hub uç noktalarına iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.eventHubs|Yönlendirme: Olay Hub'ı için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'ına ileti girişi ile Olay Hub bitiş noktasına ileti girişi arasındaki ortalama gecikme (milisaniye).|None|
|d2c.endpoints.egress.serviceBusQueues|Yönlendirme: Servis Veri Servisi Kuyruğuna gönderilen iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin Hizmet Veri Gönderi sıra uç noktalarına iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.serviceBusQueues|Yönlendirme: Servis Veri Servisi Sırası için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile Telemetri ileti girişi arasındaki ortalama gecikme (milisaniye) hizmet veri günü sıra bitiş noktasına girer.|None|
|d2c.endpoints.egress.serviceBusTopics|Yönlendirme: Servis Veri Servisi Konusuna iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin Hizmet Veri Gönderi konu uç noktalarına iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.serviceBusTopics|Yönlendirme: Servis Veri Servisi Konusu için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile Telemetri ileti girişi arasındaki ortalama gecikme (milisaniye) bir Hizmet Veri Merkezi konu bitiş noktasına girer.|None|
|d2c.endpoints.egress.builtIn.events|Yönlendirme: iletilere/etkinliklere iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin yerleşik bitiş noktasına (iletiler/olaylar) iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.builtIn.events|Yönlendirme: iletiler/olaylar için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile yerleşik bitiş noktasına (ileti/olaylar) telemetri ileti girişi arasındaki ortalama gecikme (milisaniye).|None|
|d2c.endpoints.egress.depolama|Yönlendirme: depolama alanına teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmesinin depolama bitiş noktalarına iletileri başarıyla teslim etme sayısı.|None|
|d2c.endpoints.latency.storage|Yönlendirme: depolama için ileti gecikmesi|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile depolama bitiş noktasına telemetri ileti girişi arasındaki ortalama gecikme (milisaniye).|None|
|d2c.endpoints.egress.storage.bytes|Yönlendirme: depolamaya teslim edilen veriler|Bayt|Toplam|Depolama uç noktalarına teslim edilen veri (bayt) IoT Hub yönlendirme miktarı.|None|
|d2c.endpoints.egress.storage.blobs|Yönlendirme: depoya teslim edilen lekeler|Sayı|Toplam|IoT Hub yönlendirmesinin depolama uç noktalarına blob'lar teslim etme sayısı.|None|
|EventGridDeliveries|Olay Izgara teslimatları (önizleme)|Sayı|Toplam|Olay Grid'de yayınlanan IoT Hub olaylarının sayısı. Başarılı ve başarısız isteklerin sayısı için Sonuç boyutunu kullanın. EventType boyutu olay türünühttps://aka.ms/ioteventgrid)gösterir ( .|ResourceId,Sonuç,EventType|
|EventGridLatency|Olay Izgara gecikmesi (önizleme)|Milisaniye|Ortalama|Iot Hub olayının oluşturulduğu andan Olay Grid'de yayımlandığı zamana kadar olan ortalama gecikme süresi (milisaniye). Bu sayı tüm olay türleri arasında bir ortalamadır. Belirli bir olay türünün gecikmesini görmek için EventType boyutunu kullanın.|ResourceId,EventType|
|RoutingDeliveries|Yönlendirme Teslimatları (önizleme)|Milisaniye|Toplam|IoT Hub yönlendirmekullanarak tüm uç noktalara iletileri teslim etmeye çalıştı sayısı. Başarılı veya başarısız deneme sayısını görmek için Sonuç boyutunu kullanın. Geçersiz, bırakılan veya yetim bırakılan hatanın nedenini görmek için FailureReasonCategory boyutunu kullanın. Farklı uç noktalarınıza kaç ileti teslim edildiğini anlamak için EndpointName ve EndpointType boyutlarını da kullanabilirsiniz. İletinin birden çok uç noktaya teslim edilip edilsin veya iletinin aynı bitiş noktasına birden çok kez teslim edilip edilse de, her teslim girişimi için metrik değer bir artar.|ResourceId,EndpointType,EndpointName,FailureReasonCategory,Sonuç,RoutingSource|
|YönlendirmeDeliveryLatency|Yönlendirme Teslimat Gecikmesi (önizleme)|Milisaniye|Ortalama|IoT Hub'a ileti girişi ile telemetri ileti girişi arasındaki ortalama gecikme (milisaniye). Farklı uç noktalarınızın gecikmesini anlamak için EndpointName ve EndpointType boyutlarını kullanabilirsiniz.|ResourceId,EndpointType,EndpointName,RoutingSource|
|d2c.twin.read.success|Cihazlardan başarılı ikiz okuma|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan ikiz okuma sayısı.|None|
|d2c.twin.read.failure|Cihazlardan başarısız ikiz okuma|Sayı|Toplam|Tüm başarısız cihaz tarafından başlatılan ikiz okuma sayısı.|None|
|d2c.twin.read.size|Cihazlardan gelen ikiz okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikiz okumaların ortalaması, min ve maksimumu.|None|
|d2c.twin.update.success|Cihazlardan başarılı ikiz güncellemeleri|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan ikiz güncelleştirmelerinin sayısı.|None|
|d2c.twin.update.failure|Aygıtlardan başarısız ikiz güncelleştirmeleri|Sayı|Toplam|Başarısız olan tüm aygıt tarafından başlatılan ikiz güncelleştirmelerinin sayısı.|None|
|d2c.twin.update.size|Cihazlardan gelen ikiz güncellemelerin boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikiz güncelleştirmelerinin ortalama, min ve maksimum boyutu.|None|
|c2d.methods.success|Başarılı doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarılı doğrudan yöntem çağrılarının sayısı.|None|
|c2d.methods.failure|Başarısız doğrudan yöntem çağrıları|Sayı|Toplam|Tüm başarısız doğrudan yöntem çağrılarının sayısı.|None|
|c2d.methods.requestSize|Doğrudan yöntem çağrılarının istek boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem isteklerinin ortalaması, min ve maksimumu.|None|
|c2d.methods.responseSize|Doğrudan yöntem çağrılarının yanıt boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem yanıtlarının ortalaması, min ve maksimumu.|None|
|c2d.twin.read.success|Başarılı ikiz arka uçtan okur|Sayı|Toplam|Tüm başarılı arka uç başlatılan ikiz sayısı okur.|None|
|c2d.twin.read.failure|Başarısız ikiz arka uçtan okur|Sayı|Toplam|Tüm başarısız arka uç başlatılan ikiz okuma sayısı.|None|
|c2d.twin.read.size|İkiz okumanın yanıt boyutu arka uçtan okur|Bayt|Ortalama|Tüm başarılı arka uç başlatılan ikiz okumaların ortalaması, min ve max'i.|None|
|c2d.twin.update.success|Arka uçtan başarılı ikiz güncellemeleri|Sayı|Toplam|Tüm başarılı arka uç başlatılan ikiz güncelleştirmelerinin sayısı.|None|
|c2d.twin.update.failure|Arka uçtan başarısız ikiz güncelleştirmeleri|Sayı|Toplam|Tüm başarısız geri uç başlatılan ikiz güncelleştirmelerinin sayısı.|None|
|c2d.twin.update.size|Arka uçtan ikiz güncellemeleriboyutu|Bayt|Ortalama|Tüm başarılı arka uç başlatılan ikiz güncelleştirmelerinin ortalama, min ve maksimum boyutu.|None|
|twinQueries.success|Başarılı ikiz sorgular|Sayı|Toplam|Tüm başarılı ikiz sorguların sayısı.|None|
|twinQueries.failure|Başarısız çift sorgular|Sayı|Toplam|Tüm başarısız ikiz sorguların sayısı.|None|
|twinQueries.resultSize|İkiz sorgular sonuç boyutu|Bayt|Ortalama|Tüm başarılı ikiz sorgularının sonuç boyutunun ortalaması, min ve maksimumu.|None|
|jobs.createTwinUpdateJob.success|İkiz güncelleştirme işlerinin başarılı oluşturmaları|Sayı|Toplam|İkiz güncelleştirme işlerinin tüm başarılı oluşturulmasının sayısı.|None|
|jobs.createTwinUpdateJob.failure|İkiz güncelleştirme işlerinin başarısız oluşturmaları|Sayı|Toplam|İkiz güncelleştirme işlerinin başarısız olan tüm sayısı.|None|
|jobs.createDirectMethodJob.success|Yöntem çağırma işlerinin başarılı oluşturulması|Sayı|Toplam|Doğrudan yöntem çağırma işlerinin tüm başarılı oluşturulmasının sayısı.|None|
|jobs.createDirectMethodJob.failure|Yöntem çağırma işlerinin başarısız oluşturmaları|Sayı|Toplam|Doğrudan yöntem çağırma işlerinin başarısız tüm oluşturma sayısı.|None|
|jobs.listJobs.success|İşleri listelemek için başarılı çağrılar|Sayı|Toplam|İşleri listelemek için yapılan tüm başarılı çağrıların sayısı.|None|
|jobs.listJobs.failure|İşleri listelemek için başarısız çağrılar|Sayı|Toplam|İşleri listelemek için yapılan tüm başarısız çağrıların sayısı.|None|
|jobs.cancelJob.success|Başarılı iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için yapılan tüm başarılı çağrıların sayısı.|None|
|jobs.cancelJob.failure|Başarısız iş iptalleri|Sayı|Toplam|Bir işi iptal etmek için yapılan tüm başarısız çağrıların sayısı.|None|
|jobs.queryJobs.success|Başarılı iş sorguları|Sayı|Toplam|Sorgu işleri için tüm başarılı çağrıların sayısı.|None|
|jobs.queryJobs.failure|Başarısız iş sorguları|Sayı|Toplam|İşleri sorgulamak için başarısız olan tüm çağrıların sayısı.|None|
|jobs.completed|Tamamlanan işler|Sayı|Toplam|Tamamlanan tüm işlerin sayısı.|None|
|jobs.failed|Başarısız işler|Sayı|Toplam|Tüm başarısız işlerin sayısı.|None|
|d2c.telemetri.ingress.sendThrottle|Azaltma hatalarının sayısı|Sayı|Toplam|Cihaz iş letme azaltmalarına bağlı azaltma hatalarının sayısı|None|
|dailyMessageQuotaUsed|Kullanılan toplam ileti sayısı|Sayı|Ortalama|Bugün kullanılan toplam ileti sayısı|None|
|deviceDataUsage|Toplam cihaz veri kullanımı|Bayt|Toplam|IotHub'a bağlı tüm cihazlara aktarılan baytlar|None|
|cihazDataUsageV2|Toplam cihaz veri kullanımı (önizleme)|Bayt|Toplam|IotHub'a bağlı tüm cihazlara aktarılan baytlar|None|
|totalDeviceCount|Toplam aygıtlar (önizleme)|Sayı|Ortalama|IoT hub'ınıza kayıtlı aygıt sayısı|None|
|bağlıCihaz Sayısı|Bağlı aygıtlar (önizleme)|Sayı|Ortalama|IoT hub'ınıza bağlı aygıt sayısı|None|
|Yapılandırma|Yapılandırma Ölçümleri|Sayı|Toplam|Yapılandırma İşlemleri için Ölçümler|None|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kayıt Denemeleri|Kayıt denemeleri|Sayı|Toplam|Denenen cihaz kaydı sayısı|ProvisioningServiceName,IotHubName,Durum|
|Aygıt Atamaları|Atanan aygıtlar|Sayı|Toplam|Bir IoT hub'ına atanan aygıt sayısı|ProvisioningServiceName,IotHubName|
|AttestationGirişimleri|Attestation girişimleri|Sayı|Toplam|Denenen cihaz attestation sayısı|ProvisioningServiceName,Durum, Protokol|




## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseHesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AddRegion|Eklenen Bölge|Sayı|Sayı|Eklenen Bölge|Bölge|
|Kullanılabilir Depolama|Kullanılabilir Depolama|Bayt|Toplam|5 dakikalık ayrıntılı olarak bildirilen toplam kullanılabilir depolama alanı|CollectionName,DatabaseName,Region|
|CassandraConnectionClosures|Cassandra Bağlantı Kapakları|Sayı|Toplam|Kapatılan Cassandra bağlantı sayısı, 1 dakikalık bir parçalılıkla raporlandı|APIType,Bölge,Kapanış Nedeni|
|CassandraKeyspaceSilin|Cassandra Keyspace Silindi|Sayı|Sayı|Cassandra Keyspace Silindi|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|CassandraKeyspaceThroughputUpdate|Cassandra Keyspace İş Yeri Güncellendi|Sayı|Sayı|Cassandra Keyspace İş Yeri Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|CassandraKeyspaceUpdate|Cassandra Keyspace Güncellendi|Sayı|Sayı|Cassandra Keyspace Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|CassandraRequestÜcretleri|Cassandra İstek Ücretleri|Sayı|Toplam|Cassandra istekleri için tüketilen RUs yapıldı|APiType,DatabaseName,CollectionName,Region,OperationType,ResourceType|
|Cassandraİstek'ler|Cassandra İstekleri|Sayı|Sayı|Cassandra isteklerinin sayısı|APiType,DatabaseName,CollectionName,Region,OperationType,ResourceType,ErrorCode|
|CassandraTableSilin|Cassandra Tablo Silindi|Sayı|Sayı|Cassandra Tablo Silindi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,OperationType|
|CassandraTableThroughputUpdate|Cassandra Tablo İşİ Güncellendi|Sayı|Sayı|Cassandra Tablo İşİ Güncellendi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|CassandraTableUpdate|Cassandra Tablo Güncellendi|Sayı|Sayı|Cassandra Tablo Güncellendi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|Create Account|Oluşturulan Hesap|Sayı|Sayı|Oluşturulan Hesap|None|
|VeriKullanımı|Veri Kullanımı|Bayt|Toplam|5 dakika ayrıntılı olarak bildirilen toplam veri kullanımı|CollectionName,DatabaseName,Region|
|Hesabı Silme|Silinen Hesap|Sayı|Sayı|Silinen Hesap|None|
|Belge Sayısı|Belge Sayısı|Sayı|Toplam|5 dakikalık ayrıntılı olarak bildirilen toplam belge sayısı|CollectionName,DatabaseName,Region|
|BelgeKota|Belge Kotası|Bayt|Toplam|Toplam depolama kotası 5 dakikalık parçalı olarak raporedildi|CollectionName,DatabaseName,Region|
|GremlinDatabaseDelete|Gremlin Veritabanı Silindi|Sayı|Sayı|Gremlin Veritabanı Silindi|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|GremlinDatabaseThroughputUpdate|Gremlin Veritabanı İş İlerletme Güncellendi|Sayı|Sayı|Gremlin Veritabanı İş İlerletme Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|GremlinDatabaseUpdate|Gremlin Veritabanı Güncellendi|Sayı|Sayı|Gremlin Veritabanı Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|GremlinGraphDelete|Gremlin Grafiği Silindi|Sayı|Sayı|Gremlin Grafiği Silindi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,OperationType|
|GremlinGraphThroughputUpdate|Gremlin Grafik İşİ Güncellendi|Sayı|Sayı|Gremlin Grafik İşİ Güncellendi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|GremlinGraphUpdate|Gremlin Grafiği Güncellendi|Sayı|Sayı|Gremlin Grafiği Güncellendi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|IndexKullanım|Dizin Kullanımı|Bayt|Toplam|Toplam indeks kullanımı 5 dakika parçalı olarak rapor|CollectionName,DatabaseName,Region|
|Meta dataRequests|Meta veri istekleri|Sayı|Sayı|Meta veri isteklerisayısı. Cosmos DB, her hesap için koleksiyon, veritabanları vb. ve bunların yapılandırmalarını ücretsiz olarak sayanıza olanak tanıyan sistem meta veri toplama yı sağlar.|DatabaseName,CollectionName,Region,StatusCode,Role|
|MongoCollectionDelete|Mongo Koleksiyonu Silindi|Sayı|Sayı|Mongo Koleksiyonu Silindi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,OperationType|
|MongoCollectionThroughputUpdate|Mongo Koleksiyonu İşlem Emzisi Güncellendi|Sayı|Sayı|Mongo Koleksiyonu İşlem Emzisi Güncellendi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|MongoCollectionUpdate|Mongo Koleksiyonu Güncellendi|Sayı|Sayı|Mongo Koleksiyonu Güncellendi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|MongoDBDatabaseUpdate|Mongo Veritabanı Güncellendi|Sayı|Sayı|Mongo Veritabanı Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|MongoDatabaseDelete|Mongo Veritabanı Silindi|Sayı|Sayı|Mongo Veritabanı Silindi|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|MongoDatabaseThroughputUpdate|Mongo Veritabanı İş İbzülLeri Güncellendi|Sayı|Sayı|Mongo Veritabanı İş İbzülLeri Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|MongoRequestCharge|Mongo İstek Ücreti|Sayı|Toplam|Mongo İstek Birimleri Tüketilen|DatabaseName,CollectionName,Region,CommandName,ErrorCode,Status|
|Mongoİstek'ler|Mongo İstekleri|Sayı|Sayı|Yapılan Mongo İstekLerinin Sayısı|DatabaseName,CollectionName,Region,CommandName,ErrorCode,Status|
|MongoRequestsCount|Mongo İstek Oranı|CountPerSecond|Ortalama|Mongo istek Saniyede Sayım|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|MongoRequestsDelete|Mongo Silme İstek Oranı|CountPerSecond|Ortalama|Mongo Saniyede silme isteği|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|MongoRequestsInsert|Mongo Ekle İstek Oranı|CountPerSecond|Ortalama|Saniyede Mongo Ekle sayısı|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|MongoRequestsQuery|Mongo Sorgu İstek Oranı|CountPerSecond|Ortalama|Saniyede Mongo Sorgu isteği|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|MongoRequestsUpdate|Mongo Güncelleme İstek Oranı|CountPerSecond|Ortalama|Saniyede Mongo Güncelleştirme isteği|DatabaseName,CollectionName,Region,CommandName,ErrorCode|
|Normalleştirilmiş RUTüketim|Normalleştirilmiş RU Tüketimi|Yüzde|Maksimum|Dakikada maksimum RU tüketim yüzdesi|CollectionName,DatabaseName,Region|
|ProvisionedThroughput|Sağlanan İşleme Hızı|Sayı|Maksimum|Sağlanan İşleme Hızı|DatabaseName,CollectionName|
|BölgeFailover|Bölge Üzerinde Başarısız Oldu|Sayı|Sayı|Bölge Üzerinde Başarısız Oldu|None|
|RemoveRegion|Kaldırılan Bölge|Sayı|Sayı|Kaldırılan Bölge|Bölge|
|ReplicationLatency|P99 Çoğaltma Gecikmesi|Milisaniye|Ortalama|Coğrafi etkin hesap için kaynak ve hedef bölgeler arasında P99 Çoğaltma Gecikmesi|SourceRegion,TargetRegion|
|ServerSideLatency|Sunucu Tarafı GecikmeSi|Milisaniye|Ortalama|Sunucu Tarafı GecikmeSi|DatabaseName,CollectionName,Region,ConnectionMode,OperationType,PublicAPIType|
|Servis Kullanılabilirliği|Hizmet Kullanılabilirliği|Yüzde|Ortalama|Hesap, bir saat, gün veya ay parçalı olarak kullanılabilirlik isteklerini|None|
|SqlContainerDelete|Sql Konteyner Silindi|Sayı|Sayı|Sql Konteyner Silindi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,OperationType|
|SqlContainerThroughputUpdate|Sql Konteyner İşİ Güncellendi|Sayı|Sayı|Sql Konteyner İşİ Güncellendi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|SqlContainerUpdate|Sql Konteyner Güncellendi|Sayı|Sayı|Sql Konteyner Güncellendi|ResourceName,ChildResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|SqlDatabaseDelete|Sql Veritabanı Silindi|Sayı|Sayı|Sql Veritabanı Silindi|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|SqlDatabaseThroughputUpdate|Sql Veritabanı İşİ Güncellendi|Sayı|Sayı|Sql Veritabanı İşİ Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|SqlDatabaseUpdate|Sql Veritabanı Güncellendi|Sayı|Sayı|Sql Veritabanı Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|TableTableDelete|AzureTable Tablosu Silindi|Sayı|Sayı|AzureTable Tablosu Silindi|ResourceName,ApiKind,ApiKindResourceType,OperationType|
|TableTableThroughputUpdate|AzureTable Tablo İşİ Güncellendi|Sayı|Sayı|AzureTable Tablo İşİ Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|TableTableUpdate|AzureTable Tablosu Güncellendi|Sayı|Sayı|AzureTable Tablosu Güncellendi|ResourceName,ApiKind,ApiKindResourceType,IsThroughputRequest|
|Toplam İstek Birimleri|Toplam İstek Birimleri|Sayı|Toplam|Tüketilen İstek Birimleri|DatabaseName,CollectionName,Region,StatusCode,OperationType,Status|
|TotalRequests|Toplam İstek Sayısı|Sayı|Sayı|Yapılan istek sayısı|DatabaseName,CollectionName,Region,StatusCode,OperationType,Status|
|Hesap Anahtarlarını Güncelle|Hesap Anahtarları Güncellendi|Sayı|Sayı|Hesap Anahtarları Güncellendi|Keytype|
|GüncelleHesapAğ Ayarları|Hesap Ağı Ayarları Güncellendi|Sayı|Sayı|Hesap Ağı Ayarları Güncellendi|None|
|UpdateAccountReplicationSettings|Hesap Çoğaltma Ayarları Güncellendi|Sayı|Sayı|Hesap Çoğaltma Ayarları Güncellendi|None|
|GüncellemeDiagnosticsAyarlar|Hesap Tanı ayarları güncellendi|Sayı|Sayı|Hesap Tanı ayarları güncellendi|DiagnosticSettingsName,KaynakGrubuAdı|



## <a name="microsoftenterpriseknowledgegraphservices"></a>Microsoft.EnterpriseKnowledgeGraph/hizmetleri

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İşlem Sayısı|İşlem Sayısı|Sayı|Sayı|Toplam İşlem Sayısı|İşlem Sayısı|
|Başarı Sayısı|Başarı Sayısı|Sayı|Sayı|Başarılı İşlem Sayısı|Başarı Sayısı|
|Hata Sayısı|Hata Sayısı|Sayı|Sayı|Başarısız Hareket Sayısı|Hata Sayısı|
|SuccessLatency|Başarı Gecikmesi|Milisaniye|Ortalama|Başarılı İşlemlerin GecikmeSi|Başarı Sayısı|

## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/etki alanları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|YayınBaşarı Sayısı|Yayınlanan Etkinlikler|Sayı|Toplam|Bu konuda yayınlanan toplam olaylar|Konu başlığı|
|YayınFailCount|Başarısız Olayları Yayımla|Sayı|Toplam|Toplam olaylar bu konuya yayımlamak için başarısız|Konu,Hata Türü,Hata|
|YayınBaşarıLatencyInMs|Başarı Gecikmesi Yayımla|Milisaniye|Toplam|Başarı gecikmesi milisaniye cinsinden yayımlama|None|
|EşleşenEventSayısı|Eşleşen Etkinlikler|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay|Konu,EventSubscriptionName,DomainEventSubscriptionName|
|TeslimatGirişimiFailCount|Teslim Başarısız Olaylar|Sayı|Toplam|Toplam olaylar bu olay aboneliğine teslim edilemese|Konu,EventSubscriptionName,DomainEventSubscriptionName,Hata,Hata Türü|
|TeslimatBaşarı Sayısı|Teslim Etkinlikleri|Sayı|Toplam|Bu etkinlik aboneliğine teslim edilen toplam olay|Konu,EventSubscriptionName,DomainEventSubscriptionName|
|Hedef İşlemlerDurationinms|Hedef İşlem Süresi|Milisaniye|Ortalama|Milisaniye cinsinden hedef işleme süresi|Konu,EventSubscriptionName,DomainEventSubscriptionName|
|BırakılanEventCount|Bırakılan Olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olaylar|Konu,EventSubscriptionName,DomainEventSubscriptionName,DropReason|
|DeadLetteredSayısı|Ölü Harfli Etkinlikler|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam ölü harfli olaylar|Konu,EventSubscriptionName,DomainEventSubscriptionName,DeadLetterReason|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/konular

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|YayınBaşarı Sayısı|Yayınlanan Etkinlikler|Sayı|Toplam|Bu konuda yayınlanan toplam olaylar|None|
|YayınFailCount|Başarısız Olayları Yayımla|Sayı|Toplam|Toplam olaylar bu konuya yayımlamak için başarısız|ErrorType,Hata|
|EşleşmemişEventSayısı|Eşsiz Olaylar|Sayı|Toplam|Bu konu yla ilgili etkinlik aboneliklerinin hiçbirinde eşleşmeyen toplam olaylar|None|
|YayınBaşarıLatencyInMs|Başarı Gecikmesi Yayımla|Milisaniye|Toplam|Başarı gecikmesi milisaniye cinsinden yayımlama|None|
|EşleşenEventSayısı|Eşleşen Etkinlikler|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay|EventSubscriptionName|
|TeslimatGirişimiFailCount|Teslim Başarısız Olaylar|Sayı|Toplam|Toplam olaylar bu olay aboneliğine teslim edilemese|Hata,Hata Türü,EventSubscriptionName|
|TeslimatBaşarı Sayısı|Teslim Etkinlikleri|Sayı|Toplam|Bu etkinlik aboneliğine teslim edilen toplam olay|EventSubscriptionName|
|Hedef İşlemlerDurationinms|Hedef İşlem Süresi|Milisaniye|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|BırakılanEventCount|Bırakılan Olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olaylar|DropReason,EventSubscriptionNameName|
|DeadLetteredSayısı|Ölü Harfli Etkinlikler|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam ölü harfli olaylar|DeadLetterReason,EventSubscriptionName|

## <a name="microsofteventgridsystemtopics"></a>Microsoft.EventGrid/systemTopics

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|YayınBaşarı Sayısı|Yayınlanan Etkinlikler|Sayı|Toplam|Bu konuda yayınlanan toplam olaylar|None|
|YayınFailCount|Başarısız Olayları Yayımla|Sayı|Toplam|Toplam olaylar bu konuya yayımlamak için başarısız|ErrorType,Hata|
|EşleşmemişEventSayısı|Eşsiz Olaylar|Sayı|Toplam|Bu konu yla ilgili etkinlik aboneliklerinin hiçbirinde eşleşmeyen toplam olaylar|None|
|YayınBaşarıLatencyInMs|Başarı Gecikmesi Yayımla|Milisaniye|Toplam|Başarı gecikmesi milisaniye cinsinden yayımlama|None|
|EşleşenEventSayısı|Eşleşen Etkinlikler|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay|EventSubscriptionName|
|TeslimatGirişimiFailCount|Teslim Başarısız Olaylar|Sayı|Toplam|Toplam olaylar bu olay aboneliğine teslim edilemese|Hata,Hata Türü,EventSubscriptionName|
|TeslimatBaşarı Sayısı|Teslim Etkinlikleri|Sayı|Toplam|Bu etkinlik aboneliğine teslim edilen toplam olay|EventSubscriptionName|
|Hedef İşlemlerDurationinms|Hedef İşlem Süresi|Milisaniye|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|BırakılanEventCount|Bırakılan Olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olaylar|DropReason,EventSubscriptionNameName|
|DeadLetteredSayısı|Ölü Harfli Etkinlikler|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam ölü harfli olaylar|DeadLetterReason,EventSubscriptionName|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventAbonelikler

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|EşleşenEventSayısı|Eşleşen Etkinlikler|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay|None|
|TeslimatGirişimiFailCount|Teslim Başarısız Olaylar|Sayı|Toplam|Toplam olaylar bu olay aboneliğine teslim edilemese|Hata,Hata Türü|
|TeslimatBaşarı Sayısı|Teslim Etkinlikleri|Sayı|Toplam|Bu etkinlik aboneliğine teslim edilen toplam olay|None|
|Hedef İşlemlerDurationinms|Hedef İşlem Süresi|Milisaniye|Ortalama|Milisaniye cinsinden hedef işleme süresi|None|
|BırakılanEventCount|Bırakılan Olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olaylar|Damla Neden|
|DeadLetteredSayısı|Ölü Harfli Etkinlikler|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam ölü harfli olaylar|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|YayınBaşarı Sayısı|Yayınlanan Etkinlikler|Sayı|Toplam|Bu konuda yayınlanan toplam olaylar|None|
|YayınFailCount|Başarısız Olayları Yayımla|Sayı|Toplam|Toplam olaylar bu konuya yayımlamak için başarısız|ErrorType,Hata|
|EşleşmemişEventSayısı|Eşsiz Olaylar|Sayı|Toplam|Bu konu yla ilgili etkinlik aboneliklerinin hiçbirinde eşleşmeyen toplam olaylar|None|
|YayınBaşarıLatencyInMs|Başarı Gecikmesi Yayımla|Milisaniye|Toplam|Başarı gecikmesi milisaniye cinsinden yayımlama|None|




## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Başarılı İstekler|Başarılı İstekler|Sayı|Toplam|Microsoft.EventHub için Başarılı İstekler.|EntityName,İşlem Sonucu|
|Sunucu Hataları|Sunucu Hataları.|Sayı|Toplam|Microsoft.EventHub için Sunucu Hataları.|EntityName,İşlem Sonucu|
|Kullanıcı Hataları|Kullanıcı Hataları.|Sayı|Toplam|Microsoft.EventHub için Kullanıcı Hataları.|EntityName,İşlem Sonucu|
|KotaAşıldı Hatalar|Kota Hataları Aştı.|Sayı|Toplam|Microsoft.EventHub için Kota Hataları Aşıldı.|EntityName,İşlem Sonucu|
|OperationName|Daraltılmış İstekler.|Sayı|Toplam|Microsoft.EventHub için daraltılmış istekler.|EntityName,İşlem Sonucu|
|Gelen İstekler|Gelen İstekler|Sayı|Toplam|Microsoft.EventHub için gelen istekler.|Varlıkadı|
|Gelen Mesajlar|Gelen Mesajlar|Sayı|Toplam|Microsoft.EventHub için gelen iletiler.|Varlıkadı|
|Giden Mesajlar|Giden Mesajlar|Sayı|Toplam|Microsoft.EventHub için giden iletiler.|Varlıkadı|
|Gelen Baytlar|Gelen Baytlar.|Bayt|Toplam|Microsoft.EventHub için gelen Baytlar.|Varlıkadı|
|Giden Baytlar|Giden Baytlar.|Bayt|Toplam|Microsoft.EventHub için giden Baytlar.|Varlıkadı|
|Aktif Bağlantılar|Aktif Bağlantılar|Sayı|Ortalama|Microsoft.EventHub için Toplam Etkin Bağlantı.|None|
|BağlantılarAçıldı|Bağlantılar Açıldı.|Sayı|Ortalama|Microsoft.EventHub için Açılan Bağlantılar.|Varlıkadı|
|BağlantılarKapalı|Bağlantılar Kapalı.|Sayı|Ortalama|Microsoft.EventHub için Kapalı Bağlantılar.|Varlıkadı|
|CaptureBacklog|Biriktirme Listesini Yakalayın.|Sayı|Toplam|Microsoft.EventHub için Biriktirme Listesi'ni yakalayın.|Varlıkadı|
|Yakalanan Mesajlar|Yakalanan Mesajlar.|Sayı|Toplam|Microsoft.EventHub için yakalanan iletiler.|Varlıkadı|
|Yakalanan Baytlar|Ele geçirilen Baytlar.|Bayt|Toplam|Microsoft.EventHub için yakalanan Baytlar.|Varlıkadı|
|Boyut|Boyut|Bayt|Ortalama|Bayt'ta bir EventHub boyutu.|Varlıkadı|
|INREQS|Gelen İstekler (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için toplam gelen gönderme istekleri (Amortismana uğradı)|None|
|SUCCREQ|Başarılı İstekler (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için toplam başarılı istek (Deprecated)|None|
|FAILREQ|Başarısız İstekler (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için toplam başarısız istek (Deprecated)|None|
|SVRBSY|Sunucu Meşgul Hataları (Deprecated)|Sayı|Toplam|Ad alanı için toplam sunucu meşgul hataları (Deprecated)|None|
|INTERR|Dahili Sunucu Hataları (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için toplam iç sunucu hataları (Deprecated)|None|
|MISCERR|Diğer Hatalar (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için toplam başarısız istek (Deprecated)|None|
|INMSGS|Gelen İletiler (eski) (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için toplam gelen iletiler. Bu metrik amortismana alındı. Lütfen bunun yerine Gelen İletiler metrik (Deprecated) kullanın|None|
|EHINMSGS|Gelen İletiler (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için gelen toplam ileti (Amortismana Uğradı)|None|
|OUTMSGS|Giden İletiler (eski) (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için toplam giden iletiler. Bu metrik amortismana alındı. Lütfen Bunun yerine Giden İletiler metrik (Deprecated) kullanın|None|
|EHOUTMSGS|Giden İletiler (Amortismana Uğradı)|Sayı|Toplam|Ad alanı için toplam giden iletiler (Amortismana uğradı)|None|
|EHINMBS|Gelen baytlar (eski) (Amortismana ermiş)|Bayt|Toplam|Ad alanı için Olay Hub gelen ileti girişi. Bu metrik amortismana alındı. Lütfen bunun yerine Gelen bayt metrik kullanın (Deprecated)|None|
|EHINBYTES|Gelen baytlar (Amortismana uğradı)|Bayt|Toplam|Ad alanı için Olay Hub'ı gelen ileti girişi (Amortismana Uğradı)|None|
|EHOUTMBS|Giden baytlar (eski) (Amortismana ermiş)|Bayt|Toplam|Ad alanı için Olay Hub giden ileti girişi. Bu metrik amortismana alındı. Lütfen bunun yerine Giden bayt metrik kullanın (Amortismana Uğradı)|None|
|EHOUTBYTES|Giden baytlar (Deprecated)|Bayt|Toplam|Ad alanı için Olay Hub giden ileti girişi (Deprecated)|None|
|EHABL|Biriktirme listesi iletilerini arşivle (Amortismana Uğradı)|Sayı|Toplam|Ad alanı (Deprecated) için biriktirme listesindeki Olay Hub arşivi iletileri|None|
|EHAMSGS|Arşiv iletileri (Amortismana uğradı)|Sayı|Toplam|Olay Hub'ı iletileri ad alanında arşivledi (Amortismana Uğradı)|None|
|EHAMBS|Arşiv iletisi girişi (Amortismana uğradı)|Bayt|Toplam|Olay Hub'ı ileti girişini ad alanında arşivledi (Amortismana Uğradı)|None|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/kümeleri

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Başarılı İstekler|Başarılı İstekler|Sayı|Toplam|Microsoft.EventHub için Başarılı İstekler.|İşlem Sonucu|
|Sunucu Hataları|Sunucu Hataları.|Sayı|Toplam|Microsoft.EventHub için Sunucu Hataları.|İşlem Sonucu|
|Kullanıcı Hataları|Kullanıcı Hataları.|Sayı|Toplam|Microsoft.EventHub için Kullanıcı Hataları.|İşlem Sonucu|
|KotaAşıldı Hatalar|Kota Hataları Aştı.|Sayı|Toplam|Microsoft.EventHub için Kota Hataları Aşıldı.|İşlem Sonucu|
|OperationName|Daraltılmış İstekler.|Sayı|Toplam|Microsoft.EventHub için daraltılmış istekler.|İşlem Sonucu|
|Gelen İstekler|Gelen İstekler|Sayı|Toplam|Microsoft.EventHub için gelen istekler.|None|
|Gelen Mesajlar|Gelen Mesajlar|Sayı|Toplam|Microsoft.EventHub için gelen iletiler.|None|
|Giden Mesajlar|Giden Mesajlar|Sayı|Toplam|Microsoft.EventHub için giden iletiler.|None|
|Gelen Baytlar|Gelen Baytlar.|Bayt|Toplam|Microsoft.EventHub için gelen Baytlar.|None|
|Giden Baytlar|Giden Baytlar.|Bayt|Toplam|Microsoft.EventHub için giden Baytlar.|None|
|Aktif Bağlantılar|Aktif Bağlantılar|Sayı|Ortalama|Microsoft.EventHub için Toplam Etkin Bağlantı.|None|
|BağlantılarAçıldı|Bağlantılar Açıldı.|Sayı|Ortalama|Microsoft.EventHub için Açılan Bağlantılar.|None|
|BağlantılarKapalı|Bağlantılar Kapalı.|Sayı|Ortalama|Microsoft.EventHub için Kapalı Bağlantılar.|None|
|CaptureBacklog|Biriktirme Listesini Yakalayın.|Sayı|Toplam|Microsoft.EventHub için Biriktirme Listesi'ni yakalayın.|None|
|Yakalanan Mesajlar|Yakalanan Mesajlar.|Sayı|Toplam|Microsoft.EventHub için yakalanan iletiler.|None|
|Yakalanan Baytlar|Ele geçirilen Baytlar.|Bayt|Toplam|Microsoft.EventHub için yakalanan Baytlar.|None|
|CPU|CPU|Yüzde|Maksimum|Olay Hub Kümesi için yüzde olarak CPU kullanımı|Rol|
|Kullanılabilir Bellek|Kullanılabilir Bellek|Yüzde|Maksimum|Toplam belleğin yüzdesi olarak Olay Hub Kümesi için kullanılabilir bellek.|Rol|
|Boyut|Bayt'ta bir EventHub boyutu.|Bayt|Ortalama|Bayt'ta bir EventHub boyutu.|Rol|


## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/kümeler

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Ağ Geçidi İstekleri|Ağ Geçidi İstekleri|Sayı|Toplam|Ağ geçidi isteklerinin sayısı|httpstatus|
|KategorizeAğ İstekleri|Kategorize Ağ Geçidi İstekleri|Sayı|Toplam|Kategorilere göre ağ geçidi istekleri sayısı (1xx/2xx/3xx/4xx/5xx)|httpstatus|
|NumActiveWorkers|Aktif Çalışan Sayısı|Sayı|Maksimum|Aktif Çalışan Sayısı|MetricName|


## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/Otomatik Ölçeklendirme Ayarları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Gözlenen Metrik Değer|Gözlenen Metrik Değer|Sayı|Ortalama|Yürütüldüğünde otomatik ölçekle hesaplanan değer|MetricTriggerSource|
|Metrik Eşik|Metrik Eşik|Sayı|Ortalama|Otomatik ölçek çalıştırıldığında yapılandırılan otomatik ölçeklendirme eşiği.|MetricTriggerRule|
|Gözlemlenen Kapasite|Gözlenen Kapasite|Sayı|Ortalama|Yürütüldüğünde otomatik ölçeklendirmeye bildirilen kapasite.|None|
|ScaleActionsBaşlatılan|Başlatılan Ölçeklendirme Eylemleri|Sayı|Toplam|Ölçek işleminin yönü.|Ölçekyönü|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|availabilitySonuçlar/kullanılabilirlikPercentage|Kullanılabilirlik|Yüzde|Ortalama|Başarıyla tamamlanan kullanılabilirlik testlerinin yüzdesi|kullanılabilirlikSonuç/ad,availabilitySonuç/konum|
|availabilitySonuçlar/sayım|Kullanılabilirlik testleri|Sayı|Sayı|Kullanılabilirlik testleri sayısı|kullanılabilirlikSonuç/ad,availabilitySonuç/konum,kullanılabilirlikSonuç/başarı|
|availabilitySonuçlar/süre|Kullanılabilirlik test süresi|Milisaniye|Ortalama|Kullanılabilirlik test süresi|kullanılabilirlikSonuç/ad,availabilitySonuç/konum,kullanılabilirlikSonuç/başarı|
|browserTimings/networkDuration|Sayfa yükleme ağı bağlama süresi|Milisaniye|Ortalama|Kullanıcı isteği ve ağ bağlantısı arasındaki süre. DNS arama ve aktarım bağlantısını içerir.|None|
|browserTimings/processingDuration|İstemci işleme süresi|Milisaniye|Ortalama|DOM yüklenene kadar belgenin son baytını alma arasındaki süre. Async istekleri hala işliyor olabilir.|None|
|browserTimings/receiveDuration|Yanıt süresi alma|Milisaniye|Ortalama|İlk ve son baytlar arasındaki süre, ya da kopukluk kadar.|None|
|browserTimings/sendDuration|İstek süresi gönder|Milisaniye|Ortalama|Ağ bağlantısı ile ilk bayt alma arasındaki süre.|None|
|browserZamanlamas/totalDuration|Tarayıcı sayfası yükleme süresi|Milisaniye|Ortalama|DOM, stylesheets, scripts ve görüntüler yüklenene kadar kullanıcı isteği nden zaman.|None|
|bağımlılıklar/sayım|Bağımlılık çağrıları|Sayı|Sayı|Dış kaynaklara uygulama tarafından yapılan aramaların sayısı.|bağımlılık/tür,bağımlılık/performansKova,bağımlılık/başarı,bağımlılık/hedef,bağımlılık/sonuçKodu,işlem/sentetik,bulut/rolInstance,bulut/roleName|
|bağımlılıklar/süre|Bağımlılık süresi|Milisaniye|Ortalama|Dış kaynaklara uygulama tarafından yapılan aramaların süresi.|bağımlılık/tür,bağımlılık/performansKova,bağımlılık/başarı,bağımlılık/hedef,bağımlılık/sonuçKodu,işlem/sentetik,bulut/rolInstance,bulut/roleName|
|bağımlılıklar/başarısız|Bağımlılık çağrı hataları|Sayı|Sayı|Uygulama tarafından dış kaynaklara yapılan başarısız bağımlılık çağrıları sayısı.|bağımlılık/tür,bağımlılık/performansKova,bağımlılık/başarı,bağımlılık/hedef,bağımlılık/sonuçKodu,işlem/sentetik,bulut/rolInstance,bulut/roleName|
|sayfaGörünümler/sayım|Sayfa görünümleri|Sayı|Sayı|Sayfa görüntüleme sayısı.|operasyon/sentetik,bulut/roleName|
|sayfaGörünümler/süre|Sayfa görünümü yükleme süresi|Milisaniye|Ortalama|Sayfa görünümü yükleme süresi|operasyon/sentetik,bulut/roleName|
|performanceCounters/requestExecutionTime|HTTP yürütme süresi isteği|Milisaniye|Ortalama|En son isteğin yürütme zamanı.|bulut/roleInstance|
|performanceCounters/requestsInQueue|Uygulama kuyruğundaki HTTP istekleri|Sayı|Ortalama|Uygulama istek sırasının uzunluğu.|bulut/roleInstance|
|performanceCounters/requestsPerSecond|HTTP istek oranı|CountPerSecond|Ortalama|ASP.NET itibaren uygulama için tüm isteklerin oranı.|bulut/roleInstance|
|performanceCounters/exceptionsPerSecond|Özel durum oranı|CountPerSecond|Ortalama|.NET özel durumları ve .NET özel durumlara dönüştürülen yönetilmeyen özel durumlar da dahil olmak üzere pencerelere bildirilen işlenmemiş ve işlenmemiş özel durumlar sayısı.|bulut/roleInstance|
|performansSayaçlar/processIOBytesPerSecond|Proses IO oranı|BaytPerİkinci|Ortalama|Saniyedeki toplam bayt lar dosya, ağ ve aygıtlara okunur ve yazılır.|bulut/roleInstance|
|performanceSayaçlar/prosesCpuPercentage|İşlem CPU'su|Yüzde|Ortalama|Tüm işlem iş parçacıkları talimatları yürütmek için işlemci kullanılan geçen süre yüzdesi. Bu 0 ile 100 arasında değişebilir. Bu metrik, w3wp işleminin performansını tek başına gösterir.|bulut/roleInstance|
|performanceSayaçlar/işlemciCpuPercentage|İşlemci süresi|Yüzde|Ortalama|İşlemcinin boşta olmayan iş parçacıklarında harcadığı zaman yüzdesi.|bulut/roleInstance|
|performanceSayaçlar/memoryAvailableBytes|Kullanılabilir bellek|Bayt|Ortalama|Fiziksel bellek hemen bir işleme veya sistem kullanımı için tahsis için kullanılabilir.|bulut/roleInstance|
|performanceSayaçlar/prosesPrivateBytes|Özel baytları işleme|Bayt|Ortalama|Yalnızca izlenen uygulamanın işlemlerine atanan bellek.|bulut/roleInstance|
|istekler/süre|Sunucu yanıt süresi|Milisaniye|Ortalama|BIR HTTP isteği alma ve yanıt gönderme bitirme arasındaki süre.|istek/performansKova,istek/sonuçKodu,işlem/sentetik,bulut/roleInstance,request/success,cloud/roleName|
|istekleri/sayısı|Sunucu istekleri|Sayı|Sayı|HTTP isteklerinin sayısı tamamlandı.|istek/performansKova,istek/sonuçKodu,işlem/sentetik,bulut/roleInstance,request/success,cloud/roleName|
|istekleri/başarısız|Başarısız istekler|Sayı|Sayı|Başarısız olarak işaretlenmiş HTTP isteklerisayısı. Çoğu durumda bu bir yanıt kodu >= 400 ve 401 eşit olmayan isteklerdir.|istek/performansKova,istek/sonuçKodu,istek/başarı,işlem/sentetik,bulut/rolInstance,cloud/roleName|
|istekler/oran|Sunucu istek oranı|CountPerSecond|Ortalama|Sunucu isteklerinin saniyede oranı|istek/performansKova,istek/sonuçKodu,işlem/sentetik,bulut/roleInstance,request/success,cloud/roleName|
|özel durumlar/sayım|Özel durumlar|Sayı|Sayı|Tüm yakalanmamış özel durumlar için birleştirilmiş sayı.|bulut/roleName,bulut/roleInstance,istemci/tür|
|özel durumlar/tarayıcı|Tarayıcı özel durumları|Sayı|Sayı|Tarayıcıya atılan yakalanmamış özel durumlar sayısı.|istemci/isServer,bulut/roleName|
|özel durumlar/sunucu|Sunucu özel durumları|Sayı|Sayı|Sunucu uygulamasına atılan yakalanmamış özel durumlar sayısı.|istemci/isServer,cloud/roleName,cloud/roleInstance|
|izler/sayım|İzlemeler|Sayı|Sayı|Belge sayısını izleme|trace/önemDüzeyi,çalışma/sentetik,bulut/roleName,bulut/roleInstance|


## <a name="microsoftiotcentraliotapps"></a>Microsoft.IoTCentral/IoTApps

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|bağlıCihaz Sayısı|Toplam Bağlı Aygıtlar|Sayı|Ortalama|IoT Central'a bağlı cihaz sayısı|None|
|c2d.property.read.success|Başarılı Cihaz Özelliği IoT Central'dan Okur|Sayı|Toplam|IoT Central'dan başlatılan tüm başarılı mülklerin sayısı|None|
|c2d.property.read.failure|Başarısız Aygıt Özelliği IoT Merkezi'nden Okur|Sayı|Toplam|IoT Central'dan başlatılan tüm başarısız malların sayısı|None|
|d2c.property.read.success|Başarılı Aygıt Özelliği Cihazlardan Okur|Sayı|Toplam|Cihazlardan başlatılan tüm başarılı özellik okumalarının sayısı|None|
|d2c.property.read.failure|Arızalı Aygıt Özelliği Cihazlardan Okur|Sayı|Toplam|Aygıtlardan başlatılan tüm başarısız özelliğin sayısı|None|
|c2d.property.update.success|IoT Central'dan Başarılı Aygıt Özellik Güncellemeleri|Sayı|Toplam|IoT Central'dan başlatılan tüm başarılı özellik güncelleştirmelerinin sayısı|None|
|c2d.property.update.failure|IoT Central'dan Başarısız Aygıt Özellik Güncellemeleri|Sayı|Toplam|IoT Central'dan başlatılan tüm başarısız özellik güncelleştirmelerinin sayısı|None|
|d2c.property.update.success|Cihazlardan Başarılı Aygıt Özelliği Güncellemeleri|Sayı|Toplam|Aygıtlardan başlatılan tüm başarılı özellik güncelleştirmelerinin sayısı|None|
|d2c.property.update.failure|Cihazlardan Başarısız Aygıt Özellik Güncellemeleri|Sayı|Toplam|Aygıtlardan başlatılan tüm başarısız özellik güncelleştirmelerinin sayısı|None|


## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServiceApiHit|Toplam Servis Api Hits|Sayı|Sayı|Toplam servis api isabet sayısı|Etkinlik Türü,Etkinlik Adı|
|ServiceApiLatency|Genel Hizmet Api Gecikme|Milisaniye|Ortalama|Hizmet api isteklerinin genel gecikmesi|ActivityType,ActivityName,StatusCode,StatusCodeClass|
|ServiceApiResult|Toplam Hizmet Api Sonuçları|Sayı|Sayı|Toplam hizmet api sonuçlarının sayısı|ActivityType,ActivityName,StatusCode,StatusCodeClass|
|DoygunlukShoebox|Genel Kasa Doygunluğu|Yüzde|Ortalama|Kullanılan kasa kapasitesi|Etkinlik Türü,Etkinlik Adı,İşlem Türü|
|Kullanılabilirlik|Genel Kasa Durumu|Yüzde|Ortalama|Vault, kullanılabilirlik isteklerini talep etti|ActivityType,ActivityName,StatusCode,StatusCodeClass|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Kümeler

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ÖnbellekKullanımı|Önbellek kullanımı|Yüzde|Ortalama|Küme kapsamındaki kullanım düzeyi|None|
|Sorgu Süresi|Sorgu süresi|Milisaniye|Ortalama|Sorguların süresi saniye cinsinden|Sorgu Durumu|
|YutmaKullanımı|Yutma kullanımı|Yüzde|Ortalama|Kümede kullanılan yutma yuvalarının oranı|None|
|Keepalive|Canlı tutun|Sayı|Ortalama|Akıl sağlığı denetimi kümenin sorgulara yanıt verebisini gösterir|None|
|Yutma HacimInMB|Yutma hacmi (MB'de)|Sayı|Toplam|Kümeye yutulan verilerin toplam hacmi (MB'de)|Veritabanı|
|IngestionLatencyInSeconds|Yutma gecikmesi (saniye cinsinden)|Saniye|Ortalama|Kaynaktan (örn. ileti EventHub'da) saniyeler içinde kümeye yutma süresi|None|
|EtkinliklerProcessedForEventHubs|İşlenen olaylar (Olay/IoT Hub'ları için)|Sayı|Toplam|Event/IoT Hub'ından sindirilirken küme tarafından işlenen olay sayısı|EventStatus|
|Yutma Sonucu|Yutma sonucu|Sayı|Sayı|Yutma işlemleri sayısı|Yutma Sonucu Ayrıntıları|
|CPU|CPU|Yüzde|Ortalama|CPU kullanım düzeyi|None|
|SürekliExportNumOfRecordsExported|Sürekli ihracat – ihraç edilen kayıtların num'u|Sayı|Toplam|İhraç işlemi sırasında yazılan her depolama eseri için ihraç edilen kayıt sayısı|SürekliExportName,Veritabanı|
|İhracatKullanım|İhracat Kullanımı|Yüzde|Maksimum|İhracat kullanımı|None|
|SürekliExportPendingCount|Sürekli İhracat Bekleyen Sayısı|Sayı|Maksimum|Yürütmeye hazır bekleyen sürekli ihracat işlerinin sayısı|None|
|ContinuousExportMaxLatenessMinutes|Sürekli İhracat Max Lateness|Sayı|Maksimum|Kümedeki sürekli ihracat işleri tarafından bildirilen gecikme (dakika içinde)|None|
|Sürekli İhracat Sonucu|Sürekli İhracat Sonucu|Sayı|Sayı|Sürekli Dışa Aktarma'nın başarılı olup olmadığını veya başarısız olup olmadığını gösterir|SürekliExportName,Sonuç,Veritabanı|
|StreamingingestDuration|Akış Alma Süresi|Milisaniye|Ortalama|Milisaniye cinsinden en yüksek süreyi akış|None|
|StreamingingestDataRate|Veri Hızı Akış|Sayı|Ortalama|Veri alma hızı (saniyede MB)|None|
|SteamingIngestRequestRate|Akış Alma İstek Oranı|Sayı|RateRequestsPerSecond|Akış alma istek oranı (istekler/ saniye)|None|
|StreamingingestSonuçlar|Akış Ingest Sonucu|Sayı|Ortalama|Akış sonucu|Sonuç|
|ToplamNumberOfConcurrentQueries|Toplam eşzamanlı sorgu sayısı|Sayı|Toplam|Toplam eşzamanlı sorgu sayısı|None|
|ToplamNumberOfThrottledSorgular|Toplam daraltılmış sorgu sayısı|Sayı|Toplam|Toplam daraltılmış sorgu sayısı|None|
|ToplamNumberOfThrottledKomutları|Toplam daraltılmış komut sayısı|Sayı|Toplam|Toplam daraltılmış komut sayısı|Commandtype|
|ToplamNumberOfExtents|Toplam kapsam sayısı|Sayı|Toplam|Toplam veri kapsamı sayısı|None|
|InstanceCount|Örnek Sayısı|Sayı|Ortalama|Toplam örnek sayısı|None|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/iş akışları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Çalıştırmalar Başladı|Çalıştırmalar Başladı|Sayı|Toplam|İş akışı çalıştırma sayısı başladı.|None|
|Tamamlanan Çalıştırmalar|Tamamlanan Çalıştırmalar|Sayı|Toplam|Tamamlanan iş akışı sayısı.|None|
|RunsBaşarılı|Başarılı Çalışır|Sayı|Toplam|İş akışı çalıştırma sayısı başarılı oldu.|None|
|RunsFailed|Çalıştırmalar Başarısız Oldu|Sayı|Toplam|İş akışı çalıştırma sayısı başarısız oldu.|None|
|Runsİptal edildi|İptal Edilen Çalıştırmalar|Sayı|Toplam|İş akışı çalıştırma sayısı iptal edildi.|None|
|RunLatency|Run Gecikme|Saniye|Ortalama|Tamamlanan iş akışının gecikmesi çalışır.|None|
|RunSuccessLatency|Run Başarı Gecikmesi|Saniye|Ortalama|Başarılı iş akışının gecikmesi çalışır.|None|
|RunThrottledEtkinlikler|Daraltılmış Olayları Çalıştır|Sayı|Toplam|İş akışı eylemi veya tetikleme olayları sayısı.|None|
|RunStartThrottledEtkinlikler|Çalıştır Başlangıç Daraltılmış Olaylar|Sayı|Toplam|İş akışı çalıştırma sayısı daraltılmış olayları başlatın.|None|
|RunFailure Yüzdesi|Çalıştır Hatası Yüzdesi|Yüzde|Toplam|İş akışı çalıştırma yüzdesi başarısız oldu.|None|
|Eylemler Başlatıldı|Eylemler Başladı |Sayı|Toplam|İş akışı eylem sayısı başladı.|None|
|Tamamlanan İşlemler|Tamamlanan İşlemler |Sayı|Toplam|Tamamlanan iş akışı eylem sayısı.|None|
|İşlemlerBaşarılı|Başarılı Olan Eylemler |Sayı|Toplam|İş akışı eylem sayısı başarılı oldu.|None|
|EylemlerBaşarısız|Başarısız Olan Eylemler |Sayı|Toplam|İş akışı eylem sayısı başarısız oldu.|None|
|EylemlerAtlatı|Atlanan Eylemler |Sayı|Toplam|Atlanan iş akışı eylem sayısı.|None|
|ActionLatency|Eylem Gecikme |Saniye|Ortalama|Tamamlanan iş akışı eylemlerinin gecikmesi.|None|
|ActionSuccessLatency|Eylem Başarı Gecikme |Saniye|Ortalama|Başarılı iş akışı eylemlerinin gecikmesi.|None|
|ActionThrottledEtkinlikler|Eylem Daraltılmış Olaylar|Sayı|Toplam|İş akışı eylem sayısı olayları daralttı...|None|
|TetikleyicilerBaşlatıldı|Tetikleyiciler Başladı |Sayı|Toplam|İş akışı tetikleyicilerinin sayısı başladı.|None|
|TetikleyicilerTamamlandı|Tamamlanan Tetikleyiciler |Sayı|Toplam|Tamamlanan iş akışı tetikleyicilerinin sayısı.|None|
|TetikleyicilerBaşarılı|Başarılı Tetikleyiciler |Sayı|Toplam|İş akışı tetikleyicilerinin sayısı başarılı oldu.|None|
|TetikleyicilerBaşarısız|Tetikleyiciler Başarısız Oldu |Sayı|Toplam|İş akışı tetikleyicilerinin sayısı başarısız oldu.|None|
|TetikleyicilerAtlanır|Atlanan Tetikleyiciler|Sayı|Toplam|Atlanan iş akışı tetikleyicilerinin sayısı.|None|
|TetikleyicilerAteşlendi|Ateşlenen Tetikleyiciler |Sayı|Toplam|Ateşlenen iş akışı tetikleyicilerinin sayısı.|None|
|TriggerLatency|Tetik Gecikme |Saniye|Ortalama|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|None|
|TriggerFireLatency|Tetik Yangın Gecikmesi |Saniye|Ortalama|Ateşlenen iş akışının gecikmesi tetikleyiciler.|None|
|TriggerSuccessLatency|Tetik Başarı Gecikmesi |Saniye|Ortalama|Başarılı iş akışının gecikmesi tetikleyiciler.|None|
|TriggerThrottledOlaylar|Tetikleyici Daraltılmış Olaylar|Sayı|Toplam|İş akışı sayısı daraltılmış olayları tetikler.|None|
|FaturalanabilirEylemYürütmeler|Faturalandırılabilir Eylem Yürütmeleri|Sayı|Toplam|Faturalanan iş akışı eylem yürütmelerinin sayısı.|None|
|FaturalandırılabilirTetikleyiciLer|Faturalandırılabilir Tetikleyici Yürütmeler|Sayı|Toplam|İş akışı tetikleyici yürütmelerin sayısı faturalandırılır.|None|
|Toplam Faturalı Yürütmeler|Toplam Faturalandırılabilir Yürütmeler|Sayı|Toplam|Faturalanan iş akışı yürütmelerinin sayısı.|None|
|FaturalamaKullanımıNativeOperation|Yerel İşlem Yürütmeleri için FaturaLandırma Kullanımı|Sayı|Toplam|Faturalanan yerel işlem yürütmelerinin sayısı.|None|
|FaturalamaKullanımStandartBağlantı|Standart Bağlayıcı Yürütmeler için Faturalandırma Kullanımı|Sayı|Toplam|Faturalanan standart bağlayıcı yürütme sayısı.|None|
|FaturalamaKullanımStorageConsumption|Depolama Tüketim Uygulamaları için Faturalandırma Kullanımı|Sayı|Toplam|Faturalanan depolama tüketim yürütmelerinin sayısı.|None|

## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/integrationServiceEnvironments

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Çalıştırmalar Başladı|Çalıştırmalar Başladı|Sayı|Toplam|İş akışı çalıştırma sayısı başladı.|None|
|Tamamlanan Çalıştırmalar|Tamamlanan Çalıştırmalar|Sayı|Toplam|Tamamlanan iş akışı sayısı.|None|
|RunsBaşarılı|Başarılı Çalışır|Sayı|Toplam|İş akışı çalıştırma sayısı başarılı oldu.|None|
|RunsFailed|Çalıştırmalar Başarısız Oldu|Sayı|Toplam|İş akışı çalıştırma sayısı başarısız oldu.|None|
|Runsİptal edildi|İptal Edilen Çalıştırmalar|Sayı|Toplam|İş akışı çalıştırma sayısı iptal edildi.|None|
|RunLatency|Run Gecikme|Saniye|Ortalama|Tamamlanan iş akışının gecikmesi çalışır.|None|
|RunSuccessLatency|Run Başarı Gecikmesi|Saniye|Ortalama|Başarılı iş akışının gecikmesi çalışır.|None|
|RunThrottledEtkinlikler|Daraltılmış Olayları Çalıştır|Sayı|Toplam|İş akışı eylemi veya tetikleme olayları sayısı.|None|
|RunStartThrottledEtkinlikler|Çalıştır Başlangıç Daraltılmış Olaylar|Sayı|Toplam|İş akışı çalıştırma sayısı daraltılmış olayları başlatın.|None|
|RunFailure Yüzdesi|Çalıştır Hatası Yüzdesi|Yüzde|Toplam|İş akışı çalıştırma yüzdesi başarısız oldu.|None|
|Eylemler Başlatıldı|Eylemler Başladı |Sayı|Toplam|İş akışı eylem sayısı başladı.|None|
|Tamamlanan İşlemler|Tamamlanan İşlemler |Sayı|Toplam|Tamamlanan iş akışı eylem sayısı.|None|
|İşlemlerBaşarılı|Başarılı Olan Eylemler |Sayı|Toplam|İş akışı eylem sayısı başarılı oldu.|None|
|EylemlerBaşarısız|Başarısız Olan Eylemler |Sayı|Toplam|İş akışı eylem sayısı başarısız oldu.|None|
|EylemlerAtlatı|Atlanan Eylemler |Sayı|Toplam|Atlanan iş akışı eylem sayısı.|None|
|ActionLatency|Eylem Gecikme |Saniye|Ortalama|Tamamlanan iş akışı eylemlerinin gecikmesi.|None|
|ActionSuccessLatency|Eylem Başarı Gecikme |Saniye|Ortalama|Başarılı iş akışı eylemlerinin gecikmesi.|None|
|ActionThrottledEtkinlikler|Eylem Daraltılmış Olaylar|Sayı|Toplam|İş akışı eylem sayısı olayları daralttı...|None|
|TetikleyicilerBaşlatıldı|Tetikleyiciler Başladı |Sayı|Toplam|İş akışı tetikleyicilerinin sayısı başladı.|None|
|TetikleyicilerTamamlandı|Tamamlanan Tetikleyiciler |Sayı|Toplam|Tamamlanan iş akışı tetikleyicilerinin sayısı.|None|
|TetikleyicilerBaşarılı|Başarılı Tetikleyiciler |Sayı|Toplam|İş akışı tetikleyicilerinin sayısı başarılı oldu.|None|
|TetikleyicilerBaşarısız|Tetikleyiciler Başarısız Oldu |Sayı|Toplam|İş akışı tetikleyicilerinin sayısı başarısız oldu.|None|
|TetikleyicilerAtlanır|Atlanan Tetikleyiciler|Sayı|Toplam|Atlanan iş akışı tetikleyicilerinin sayısı.|None|
|TetikleyicilerAteşlendi|Ateşlenen Tetikleyiciler |Sayı|Toplam|Ateşlenen iş akışı tetikleyicilerinin sayısı.|None|
|TriggerLatency|Tetik Gecikme |Saniye|Ortalama|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|None|
|TriggerFireLatency|Tetik Yangın Gecikmesi |Saniye|Ortalama|Ateşlenen iş akışının gecikmesi tetikleyiciler.|None|
|TriggerSuccessLatency|Tetik Başarı Gecikmesi |Saniye|Ortalama|Başarılı iş akışının gecikmesi tetikleyiciler.|None|
|TriggerThrottledOlaylar|Tetikleyici Daraltılmış Olaylar|Sayı|Toplam|İş akışı sayısı daraltılmış olayları tetikler.|None|
|EntegrasyonServiceEnvironmentWorkflowProcessorUsage|Entegrasyon Hizmet Ortamı için İş Akışı İşlemci Kullanımı|Yüzde|Ortalama|Entegrasyon hizmeti ortamı için iş akışı işlemci kullanımı.|None|
|EntegrasyonServiceEnvironmentWorkflowMemoryUsage|Entegrasyon Hizmet Ortamı için İş Akışı Bellek Kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için iş akışı bellek kullanımı.|None|
|EntegrasyonServiceEnvironmentConnectorProcessorUsage|Entegrasyon Hizmet Ortamı için Bağlayıcı İşlemci Kullanımı|Yüzde|Ortalama|Entegrasyon servis ortamı için bağlayıcı işlemci kullanımı.|None|
|EntegrasyonServiceEnvironmentConnectorMemoryUsage|Entegrasyon Hizmet Ortamı için Bağlayıcı Bellek Kullanımı|Yüzde|Ortalama|Entegrasyon hizmeti ortamı için bağlayıcı bellek kullanımı.|None|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/çalışma alanları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İptal Edilen Çalıştırmalar|İptal Edilen Çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için iptal edilen çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|İstenen Çalıştırmaları İptal Et|İstenen Çalıştırmaları İptal Et|Sayı|Toplam|Bu çalışma alanı için iptal in istendiği çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Tamamlanan Çalıştırmalar|Tamamlanan Çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için başarıyla tamamlanan çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Başarısız Çalıştırmalar|Başarısız Çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için başarısız olan çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Çalıştırmaları Sonuçlandırma|Çalıştırmaları Sonuçlandırma|Sayı|Toplam|Bu çalışma alanı için sonlandırıcı duruma girilen çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Çalıştırmalara Yanıt Vermiyor|Çalıştırmalara Yanıt Vermiyor|Sayı|Toplam|Bu çalışma alanı için yanıt vermeyen çalışan sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Başlatılan Çalıştırmalar|Başlatılan Çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için başlatılmayan durumdaki çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Koşuları Hazırlama|Koşuları Hazırlama|Sayı|Toplam|Bu çalışma alanına hazırlanan çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Provizyon Çalıştırmaları|Provizyon Çalıştırmaları|Sayı|Toplam|Bu çalışma alanı için sağlanan çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Sıralı Çalıştırmalar|Sıralı Çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için sıraya gelen çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Başlatılan Çalıştırmalar|Başlatılan Çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için başlatılan çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Başlangıç Çalıştırmaları|Başlangıç Çalıştırmaları|Sayı|Toplam|Bu çalışma alanı için başlatılan çalıştırma sayısı|Senaryo,RunType,PublishedPipelineId,ComputeType,PipelineStepType|
|Hatalar|Hatalar|Sayı|Toplam|Bu çalışma alanındaki çalışma hatalarının sayısı|Senaryo|
|Uyarılar|Uyarılar|Sayı|Toplam|Bu çalışma alanındaki çalışma uyarılarının sayısı|Senaryo|
|Model Kaydı Başarılı|Model Kaydı Başarılı|Sayı|Toplam|Bu çalışma alanında başarılı olan model kayıtlarının sayısı|Senaryo|
|Model Kaydı Başarısız Oldu|Model Kaydı Başarısız Oldu|Sayı|Toplam|Bu çalışma alanında başarısız olan model kayıtlarının sayısı|Senaryo,StatusCode|
|Model Dağıtımı Başladı|Model Dağıtımı Başladı|Sayı|Toplam|Bu çalışma alanında başlatılan model dağıtımlarının sayısı|Senaryo|
|Model Dağıtma Başarılı|Model Dağıtma Başarılı|Sayı|Toplam|Bu çalışma alanında başarılı olan model dağıtımlarının sayısı|Senaryo|
|Model Dağıtma Başarısız Oldu|Model Dağıtma Başarısız Oldu|Sayı|Toplam|Bu çalışma alanında başarısız olan model dağıtımlarının sayısı|Senaryo,StatusCode|
|Toplam Düğümler|Toplam Düğümler|Sayı|Ortalama|Toplam düğüm sayısı. Bu toplam, Bazı Etkin Düğümler, Boşta Düğümler, Kullanılamaz Düğümler, Premepted Düğümler, Ayrılan Düğümler içerir|Senaryo,ClusterName|
|Etkin Düğümler|Etkin Düğümler|Sayı|Ortalama|Acitve düğümlerinin sayısı. Bunlar, bir işi etkin olarak çalıştıran düğümlerdir.|Senaryo,ClusterName|
|Boşta Düğümler|Boşta Düğümler|Sayı|Ortalama|Boşta düğüm sayısı. Boşta düğümler, herhangi bir işi çalıştırmayan, ancak varsa yeni işi kabul edebilen düğümlerdir.|Senaryo,ClusterName|
|Kullanılamaz Düğümler|Kullanılamaz Düğümler|Sayı|Ortalama|Kullanılamaz düğüm sayısı. Kullanılamaz düğümleri çözülemeyen bazı sorun nedeniyle işlevsel değildir. Azure bu düğümleri geri dönüştürür.|Senaryo,ClusterName|
|Preempted Düğümler|Preempted Düğümler|Sayı|Ortalama|Preempted düğüm sayısı. Bu düğümler, kullanılabilir düğüm havuzundan alınan düşük öncelikli düğümlerdir.|Senaryo,ClusterName|
|Düğümleri Bırakma|Düğümleri Bırakma|Sayı|Ortalama|Ayrılan düğüm sayısı. Düğümleri bırakarak, bir işi işlemeyi yeni bitiren ve Boşta duruma geçecek düğümlerdir.|Senaryo,ClusterName|
|Toplam Çekirdek|Toplam Çekirdek|Sayı|Ortalama|Toplam çekirdek sayısı|Senaryo,ClusterName|
|Aktif Çekirdekler|Aktif Çekirdekler|Sayı|Ortalama|Aktif çekirdek sayısı|Senaryo,ClusterName|
|BoşTa Çekirdekler|BoşTa Çekirdekler|Sayı|Ortalama|Boşta kalan çekirdek sayısı|Senaryo,ClusterName|
|Kullanılmaz Çekirdekler|Kullanılmaz Çekirdekler|Sayı|Ortalama|Kullanışsız çekirdek sayısı|Senaryo,ClusterName|
|Preempted Çekirdekler|Preempted Çekirdekler|Sayı|Ortalama|Preempted çekirdek sayısı|Senaryo,ClusterName|
|Çekirdekleri Bırakma|Çekirdekleri Bırakma|Sayı|Ortalama|Ayrılan çekirdek sayısı|Senaryo,ClusterName|
|Kota Kullanım Yüzdesi|Kota Kullanım Yüzdesi|Sayı|Ortalama|Kullanılan kota yüzdesi|Senaryo,ClusterName,VmFamilyName,VmPriority|
|CpuKullanımı|CpuKullanımı|Sayı|Ortalama|Cpu (Önizleme)|Senaryo,runId,Düğüm,CreatedTime|
|GpuUtilization|GpuUtilization|Sayı|Ortalama|GPU (Önizleme)|Senaryo,runId,Düğüm,CreatedTime,DeviceId|


## <a name="microsoftmapsaccounts"></a>Microsoft.Haritalar/hesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Sayı|Sayı|API çağrılarının sayısı|ApiCategory,ApiName,ResultType,ResponseCode|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|API'lerin kullanılabilirliği|ApiCategory,ApiName|

## <a name="microsoftmediamediaservicesstreamingendpoints"></a>Microsoft.Media/mediaservices/streamingEndpoints

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Çıkış|Çıkış|Bayt|Toplam|Baytlar halindeki Çıkış verisi miktarı.|Çıktı Biçimi|
|SuccessE2ELatency|Gecikme sonu için başarı sona erer|Milisaniye|Ortalama|Milisaniyecinsinden başarılı isteklerin ortalama gecikmesi.|Çıktı Biçimi|
|İstekler|İstekler|Sayı|Toplam|Akış Bitiş Noktasına istekler.|OutputFormat,httpstatuscode,errorcode|


## <a name="microsoftmediamediaservices"></a>Microsoft.Media/mediaservices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Varlık Kotası|Varlık kotası|Sayı|Ortalama|Geçerli medya hizmeti hesabı için kaç varlık izin verilir|None|
|Varlık Sayısı|Varlık sayısı|Sayı|Ortalama|Geçerli medya hizmeti hesabında zaten kaç varlık oluşturuldu|None|
|Varlık KotasıKullanılma Yüzdesi|Varlık kotası kullanılan yüzde|Yüzde|Ortalama|Geçerli ortam hizmeti hesabında kullanılan varlık yüzdesi|None|
|İçerikKeyPolicyKota|İçerik Anahtar İlkesi kotası|Sayı|Ortalama|Geçerli medya hizmeti hesabı için kaç içerik anahtarı polisine izin verilir|None|
|İçerikKeyPolicyCount|İçerik Anahtar İlkesi sayısı|Sayı|Ortalama|Geçerli medya hizmeti hesabında zaten kaç içerik temel ilke oluşturuldu|None|
|İçerikKeyPolicyQuotaUsedPercentage|İçerik Anahtar İlkesi kotası kullanılan yüzde|Yüzde|Ortalama|Geçerli medya hizmeti hesabında kullanılan İçerik Anahtarı Politikası|None|
|StreamingPolicyQuota|Akış İlkesi kotası|Sayı|Ortalama|Geçerli medya hizmeti hesabı için kaç akış politikasına izin verilir|None|
|Akış Politikası Sayısı|Akış İlkesi sayısı|Sayı|Ortalama|Geçerli medya hizmeti hesabında zaten kaç akış ilkeleri oluşturuldu|None|
|StreamingPolicyQuotaUsedPercentage|Akış İlkesi kotası kullanılan yüzde|Yüzde|Ortalama|Akış İlkesi geçerli medya hizmeti hesabında kullanılan yüzde|None|


## <a name="microsoftmixedrealityremoterenderingaccounts"></a>Microsoft.MixedReality/remoteRenderingHesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VarlıklarDönüştürüldü|Dönüştürülen Varlıklar|Sayı|Toplam|Dönüştürülen toplam varlık sayısı|Appid,Resourceid,SDKVersion|
|ActiveRenderingOturumları|Etkin İşleme Oturumları|Sayı|Toplam|Toplam etkin işleme oturumu sayısı|Appid,Resourceid,SessionType,SDKVersion|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/volumes

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageReadLatency|Ortalama okuma gecikmesi|Milisaniye|Ortalama|İşlem başına milisaniye cinsinden ortalama okuma gecikmesi|None|
|AverageWriteLatency|Ortalama yazma gecikmesi|Milisaniye|Ortalama|İşlem başına milisaniye cinsinden ortalama yazma gecikmesi|None|
|Ciltleme Boyutu|Hacim Tüketilen Boyut|Bayt|Ortalama|Birimin mantıksal boyutu (kullanılan baytlar)|None|
|VolumeSnapshotSize|Birim anlık görüntü boyutu|Bayt|Ortalama|Birimdeki tüm anlık görüntülerin boyutu|None|
|ReadIops|Iops'u okuyun|CountPerSecond|Ortalama|Saniyede Katılım/çıkış işlemlerini okuma|None|
|Yazma|iops yaz|CountPerSecond|Ortalama|Saniyede Çıkarma/Çıkış işlemleri yazma|None|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VolumePoolAllocatedUsed|Hacim Boyutuna Ayrılan Havuz|Bayt|Ortalama|Havuzun ayrılan kullanılan boyutu|None|
|VolumePoolTotalLogicalSize|Havuz Tüketilen Boyut|Bayt|Ortalama|Havuza ait tüm birimlerin mantıksal boyutunun toplamı|None|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BaytSentRate|Gönderilen Baytlar|Bayt|Toplam|Ağ Arabiriminin gönderdiği bayt sayısı|None|
|BaytReceivedRate|Alınan Baytlar|Bayt|Toplam|Ağ Arabiriminin aldığı bayt sayısı|None|
|PaketlerSentRate|Gönderilen Paketler|Sayı|Toplam|Ağ Arabiriminin gönderdiği paket sayısı|None|
|PaketlerReceivedRate|Alınan Paketler|Sayı|Toplam|Ağ Arabiriminin aldığı paket sayısı|None|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VipAvailability|Veri Yolu Kullanılabilirliği|Sayı|Ortalama|Zaman süresi başına ortalama Yük Dengeleyici veri yolu kullanılabilirliği|FrontendIPAddress,FrontendPort|
|DipAvailability|Sağlık Sondası Durumu|Sayı|Ortalama|Zaman süresi başına ortalama Yük Dengeleyici sağlık sondası durumu|ProtocolType,BackendPort,FrontendIPAddress,FrontendPort,BackendIPAddress|
|Bytecount|Bayt Sayısı|Sayı|Toplam|Zaman dilimi içinde iletilen toplam Bayt sayısı|FrontendIPAddress,FrontendPort,Yön|
|Paket Sayısı|Paket Sayısı|Sayı|Toplam|Zaman dilimi içinde iletilen toplam Paket sayısı|FrontendIPAddress,FrontendPort,Yön|
|SYNCount|SYN Sayısı|Sayı|Toplam|Zaman dilimi içinde iletilen TOPLAM SYN Paketi sayısı|FrontendIPAddress,FrontendPort,Yön|
|SnatConnectionCount|SNAT Bağlantı Sayısı|Sayı|Toplam|Zaman dilimi içinde oluşturulan toplam yeni SNAT bağlantısı sayısı|FrontendIPAddress,BackendIPAddress,ConnectionState|
|Tahsis edilenSnatPort'lar|Tahsis edilmiş SNAT Bağlantı Noktaları (Önizleme)|Sayı|Toplam|Zaman dilimi içinde ayrılan Toplam SNAT bağlantı noktası sayısı|FrontendIPAddress,BackendIPAddress,ProtocolType,IsAwaitingRemoval|
|UsedSnatPorts|İkinci El SNAT Bağlantı Noktaları (Önizleme)|Sayı|Toplam|Zaman dilimi içinde kullanılan Toplam SNAT bağlantı noktası sayısı|FrontendIPAddress,BackendIPAddress,ProtocolType,IsAwaitingRemoval|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SorguHacmi|Sorgu Hacmi|Sayı|Toplam|DNS bölgesi için hizmet verilen sorgu sayısı|None|
|Kayıt Seti Sayısı|Kayıt Kümesi Sayısı|Sayı|Maksimum|DNS bölgesindeki Kayıt Kümesi Sayısı|None|
|RecordSetCapacityKullanımation|Kayıt Seti Kapasite Kullanımı|Yüzde|Maksimum|Bir DNS bölgesi tarafından kullanılan Kayıt Kümesi kapasitesinin yüzdesi|None|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAdresleri

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PaketlerInDDoS|Gelen paketler DDoS|CountPerSecond|Maksimum|Gelen paketler DDoS|None|
|PaketlerDroppedDDoS|Gelen paketler DDoS düştü|CountPerSecond|Maksimum|Gelen paketler DDoS düştü|None|
|PaketlerForwardedDDoS|Gelen paketler iletilen DDoS|CountPerSecond|Maksimum|Gelen paketler iletilen DDoS|None|
|TCPPacketsInDDoS|Gelen TCP paketleri DDoS|CountPerSecond|Maksimum|Gelen TCP paketleri DDoS|None|
|TCPPacketsDroppedDDoS|Gelen TCP paketleri DDoS düştü|CountPerSecond|Maksimum|Gelen TCP paketleri DDoS düştü|None|
|TCPPacketsForwardedDDoS|Gelen TCP paketleri iletilmiş DDoS|CountPerSecond|Maksimum|Gelen TCP paketleri iletilmiş DDoS|None|
|UDPPacketsInDDoS|Gelen UDP paketleri DDoS|CountPerSecond|Maksimum|Gelen UDP paketleri DDoS|None|
|UDPPacketsDroppedDDoS|Gelen UDP paketleri DDoS düştü|CountPerSecond|Maksimum|Gelen UDP paketleri DDoS düştü|None|
|UDPPacketsForwardedDDoS|Gelen UDP paketleri iletilen DDoS|CountPerSecond|Maksimum|Gelen UDP paketleri iletilen DDoS|None|
|BaytInDDoS|Gelen baytlar DDoS|BaytPerİkinci|Maksimum|Gelen baytlar DDoS|None|
|BytesDroppedDDoS|Gelen baytd DDoS düştü|BaytPerİkinci|Maksimum|Gelen baytd DDoS düştü|None|
|BaytForwardedDDoS|Gelen baytlar iletilen DDoS|BaytPerİkinci|Maksimum|Gelen baytlar iletilen DDoS|None|
|TCPBytesInDDoS|Gelen TCP baytd DDoS|BaytPerİkinci|Maksimum|Gelen TCP baytd DDoS|None|
|TCPBytesDroppedDDoS|Gelen TCP baytDDoS düştü|BaytPerİkinci|Maksimum|Gelen TCP baytDDoS düştü|None|
|TCPBytesForwardedDDoS|Gelen TCP baytları iletilen DDoS|BaytPerİkinci|Maksimum|Gelen TCP baytları iletilen DDoS|None|
|UDPBytesInDDoS|Gelen UDP bayt DDoS|BaytPerİkinci|Maksimum|Gelen UDP bayt DDoS|None|
|UDPBytesDroppedDDoS|Gelen UDP bayt DDoS düştü|BaytPerİkinci|Maksimum|Gelen UDP bayt DDoS düştü|None|
|UDPBytesForwardedDDoS|Gelen UDP baytları iletilen DDoS|BaytPerİkinci|Maksimum|Gelen UDP baytları iletilen DDoS|None|
|IfunderDdosAttack|DDoS saldırısı altında veya değil|Sayı|Maksimum|DDoS saldırısı altında veya değil|None|
|DDoSTriggerTCPPaketleri|DDoS azaltmayı tetiklemek için gelen TCP paketleri|CountPerSecond|Maksimum|DDoS azaltmayı tetiklemek için gelen TCP paketleri|None|
|DDoSTriggerUDPPackets|DDoS azaltmayı tetiklemek için gelen UDP paketleri|CountPerSecond|Maksimum|DDoS azaltmayı tetiklemek için gelen UDP paketleri|None|
|DDoSTriggerSYNPaketleri|DDoS azaltmayı tetiklemek için gelen SYN paketleri|CountPerSecond|Maksimum|DDoS azaltmayı tetiklemek için gelen SYN paketleri|None|
|VipAvailability|Veri Yolu Kullanılabilirliği|Sayı|Ortalama|Zaman süresi başına ortalama IP Adresi kullanılabilirliği|Bağlantı noktası|
|Bytecount|Bayt Sayısı|Sayı|Toplam|Zaman dilimi içinde iletilen toplam Bayt sayısı|Bağlantı Noktası, Yön|
|Paket Sayısı|Paket Sayısı|Sayı|Toplam|Zaman dilimi içinde iletilen toplam Paket sayısı|Bağlantı Noktası, Yön|
|SynCount|SYN Sayısı|Sayı|Toplam|Zaman dilimi içinde iletilen TOPLAM SYN Paketi sayısı|Bağlantı Noktası, Yön|



## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PingMeshAverageRoundtripMs|Ping'ler için VM'ye gidiş dönüş süresi|Milisaniye|Ortalama|Hedef VM'ye gönderilen Ping'ler için gidiş-dönüş süresi|KaynakMüşteri Adresi,HedefMüşteri Adresi|
|PingMeshProbesFailedPercent|VM'de Başarısız Ping|Yüzde|Ortalama|Bir hedef VM'nin gönderilen Ping'lerini toplama kadar başarısız Ping sayısının yüzdesi|KaynakMüşteri Adresi,HedefMüşteri Adresi|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|UygulamaRuleHit|Uygulama kuralları sayısına isabet|Sayı|Toplam|Uygulama kurallarının vurulma sayısı|Durum, Neden, Protokol|
|NetworkRuleHit|Ağ kuralları isabet sayısı|Sayı|Toplam|Ağ kurallarının kaç kez vurulduğu|Durum, Neden, Protokol|
|Güvenlik DuvarıSağlık|Güvenlik duvarı sistem durumu|Yüzde|Ortalama|Güvenlik duvarı sistem durumu|Durum, Neden|
|Veriİşlendi|İşlenen veriler|Bayt|Toplam|Güvenlik Duvarı tarafından işlenen toplam veri miktarı|None|
|SNATPortKullanım|SNAT bağlantı noktası kullanımı|Yüzde|Ortalama|SNAT bağlantı noktası kullanımı|None|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Aktarım hızı|Aktarım hızı|BaytPerİkinci|Ortalama|Uygulama Ağ Geçidi'nin hizmet verdiği saniyede bayt sayısı|None|
|SağlıksızHostCount|Sağlıksız Ev Sahibi Sayısı|Sayı|Ortalama|Sağlıksız arka uç ana bilgisayar sayısı|BackendSettingsPool|
|Sağlıklı HostCount|Sağlıklı Ev Sahibi Sayısı|Sayı|Ortalama|Sağlıklı arka uç ana bilgisayar sayısı|BackendSettingsPool|
|TotalRequests|Toplam İstek Sayısı|Sayı|Toplam|Uygulama Ağ Geçidi'nin hizmet verdiği başarılı isteklerin sayısı|BackendSettingsPool|
|AvgRequestCountPerHealthyHost|Sağlıklı Ana Bilgisayar Başına dakika başına istekler|Sayı|Ortalama|Havuzdaki sağlıklı arka uç ana bilgisayar başına dakika başına ortalama istek sayısı|BackendSettingsPool|
|Başarısız İstekler|Başarısız İstekler|Sayı|Toplam|Uygulama Ağ Geçidi'nin hizmet verdiği başarısız isteklerin sayısı|BackendSettingsPool|
|Yanıt Durumu|Yanıt Durumu|Sayı|Toplam|Uygulama Ağ Geçidi tarafından döndürülen http yanıt durumu|httpstatusgroup|
|Güncel Bağlantılar|Geçerli Bağlantılar|Sayı|Toplam|Uygulama Ağ Geçidi ile kurulan geçerli bağlantıların sayısı|None|
|NewConnectionsPerSecond|Saniyede yeni bağlantılar|CountPerSecond|Ortalama|Uygulama Ağ Geçidi ile kurulan saniyede yeni bağlantılar|None|
|CpuKullanımı|CPU Kullanımı|Yüzde|Ortalama|Uygulama Ağ Geçidinin geçerli CPU kullanımı|None|
|Kapasite Birimleri|Mevcut Kapasite Birimleri|Sayı|Ortalama|Tüketilen Kapasite Birimleri|None|
|Sabit Faturalandırılabilir Kapasite Birimleri|Sabit Faturalandırılabilir Kapasite Birimleri|Sayı|Ortalama|Ücretlendirilecek minimum kapasite birimleri|None|
|Tahmini Faturalı Kapasite Birimleri|Tahmini Faturalı Kapasite Birimleri|Sayı|Ortalama|Ücretlendirilecek tahmini kapasite birimleri|None|
|İşlem Birimleri|Geçerli İşlem Birimleri|Sayı|Ortalama|Tüketilen İşlem Birimleri|None|
|BackendResponseStatus|Arka Uç Yanıt Durumu|Sayı|Toplam|Arka uç üyeleri tarafından oluşturulan HTTP yanıt kodlarının sayısı. Bu, Uygulama Ağ Geçidi tarafından oluşturulan yanıt kodlarını içermez.|BackendServer,BackendPool,BackendHttpSetting,HttpStatusGroup|
|TlsProtokol|İstemci TLS Protokolü|Sayı|Toplam|Uygulama Ağ Geçidi ile bağlantı kuran istemci tarafından başlatılan TLS ve TLS dışı istek sayısı. TLS protokol dağılımını görüntülemek için TLS Protokolü boyutuna göre filtre uygulayın.|Dinleyici,TlsProtocol|
|Baytgönderildi|Gönderilen Baytlar|Bayt|Toplam|Uygulama Ağ Geçidi tarafından istemcilere gönderilen toplam bayt sayısı|Dinleyici|
|BaytAlınan|Alınan Baytlar|Bayt|Toplam|Uygulama Ağ Geçidi tarafından istemcilerden alınan toplam bayt sayısı|Dinleyici|
|MüşteriRtt|İstemci RTT|Milisaniye|Ortalama|İstemciler ve Uygulama Ağ Geçidi arasındaki ortalama gidiş-dönüş süresi. Bu metrik, bağlantıların kurulmasının ve bildirimlerin iade edilmesinin ne kadar sürdüğünü gösterir|Dinleyici|
|UygulamaGatewayTotalTime|Uygulama Ağ Geçidi Toplam Süresi|Milisaniye|Ortalama|Bir isteğin işlenmesi ve yanıtın gönderilmesi için gereken ortalama süre. Bu, Uygulama Ağ Geçidi'nin bir HTTP isteğinin ilk baytını aldığı andan yanıt gönderme işleminin bittiği zamana kadar olan aralığın ortalaması olarak hesaplanır. Bunun genellikle Uygulama Ağ Geçidi işleme süresini, istek ve yanıt paketlerinin ağ üzerinde seyahat ettiği zamanı ve arka uç sunucusunun yanıt verme süresini içerdiğini unutmayın.|Dinleyici|
|BackendConnectTime|Arka Uç Bağlantı Süresi|Milisaniye|Ortalama|Arka uç sunucusuyla bağlantı kurmak için harcanan zaman|Dinleyici,BackendServer,BackendPool,BackendHttpSetting|
|BackendFirstByteResponseTime|Backend İlk Bayt Yanıt Süresi|Milisaniye|Ortalama|Arka uç sunucusuna bağlantı kurmanın başlangıcı ile yanıt üstbilgisinin ilk baytını alma arasındaki zaman aralığı, arka uç sunucusunun işlem süresini yaklaşık olarak|Dinleyici,BackendServer,BackendPool,BackendHttpSetting|
|BackendLastByteResponseTime|Backend Son Bayt Yanıt Süresi|Milisaniye|Ortalama|Arka uç sunucusuna bağlantı kurma nın başlangıcı ile yanıt gövdesinin son baytını alma arasındaki zaman aralığı|Dinleyici,BackendServer,BackendPool,BackendHttpSetting|
|Eşleşen Sayı|Web Uygulama Firewall v1 Toplam Kural Dağılımı|Sayı|Toplam|Web Uygulama Güvenlik Duvarı v1 Gelen trafik için Toplam Kural Dağılımı|RuleGroup,RuleId|
|Engellenen Sayım|Web Uygulama Güvenlik Duvarı v1 Engellenen İstekler Kural Dağılımı|Sayı|Toplam|Web Application Firewall v1 engellenen istekler kural dağıtımı|RuleGroup,RuleId|
|BlokeReqCount|Web Uygulama Güvenlik Duvarı v1 Engellenen İstek Sayısı|Sayı|Toplam|Web Uygulama Güvenlik Duvarı v1 engellenen istekleri sayısı|None|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkAğ Ağ Geçidi

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Ortalama Bant Genişliği|Ağ Geçidi S2S Bant Genişliği|BaytPerİkinci|Ortalama|Saniyede bayt lar halinde bir ağ geçidinin siteden siteye ortalama bant genişliği|None|
|P2SBant Genişliği|Ağ Geçidi P2S Bant Genişliği|BaytPerİkinci|Ortalama|Saniyede bayt bir ağ geçidinin ortalama noktadan siteye bant genişliği|None|
|P2SConnectionCount|P2S Bağlantı Sayısı|Sayı|Maksimum|Ağ geçidinin noktadan siteye bağlantı sayısı|Protokol|
|TünelOrtalamaBant Genişliği|Tünel Bant Genişliği|BaytPerİkinci|Ortalama|Bir tünelin saniyede baytiçinde ortalama bant genişliği|Bağlantı Adı,RemoteIP|
|TunnelEgressBayt|Tünel Çıkış Bayt|Bayt|Toplam|Bir tünelin giden baytları|Bağlantı Adı,RemoteIP|
|TunnelIngressBayt|Tünel Girişi Baytları|Bayt|Toplam|Tünelin gelen baytları|Bağlantı Adı,RemoteIP|
|TunnelEgressPackets|Tünel Çıkış Paketleri|Sayı|Toplam|Bir tünelin giden paket sayısı|Bağlantı Adı,RemoteIP|
|TunnelIngressPaketleri|Tünel Giriş Paketleri|Sayı|Toplam|Tünelin gelen paket sayısı|Bağlantı Adı,RemoteIP|
|TunnelEgressPacketDropTSMismatch|Tünel Çıkış TS Uyumsuzluk Paket Bırak|Sayı|Toplam|Bir tünelin trafik seçici uyumsuzluktan giden paket bırakma sayısı|Bağlantı Adı,RemoteIP|
|Tünel IngressPacketDropTSMismatch|Tünel Girişi TS Uyuşmazlık Paket Bırakma|Sayı|Toplam|Tünelin trafik seçici uyumsuzluktan gelen paket bırakma sayısı|Bağlantı Adı,RemoteIP|


## <a name="microsoftnetworkexpressrouteports"></a>Microsoft.Network/expressRoutePorts

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RxLightLevel|RxLightLevel|Sayı|Ortalama|dBm'de Rx Işık seviyesi|Bağlantı,Şerit|
|TxLightLevel|TxLightLevel|Sayı|Ortalama|dBm'de Tx ışık seviyesi|Bağlantı,Şerit|
|Yönetici Durumu|Yönetici Durumu|Sayı|Ortalama|Bağlantı noktasının yönetici durumu|Bağlantı|
|LineProtokolü|LineProtokolü|Sayı|Ortalama|Bağlantı noktasının satır protokolü durumu|Bağlantı|
|PortBitsInPerİkinci|BitsInPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'u giriken bitler|Bağlantı|
|PortBitsOutPerİkinci|BitsOutPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'dan çıkan bitler|Bağlantı|



## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerİkinci|BitsInPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'u giriken bitler|PeeringType|
|BitsOutPerİkinci|BitsOutPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'dan çıkan bitler|PeeringType|
|GlobalReachBitsInPerSecond|GlobalReachBitsInPerSecond|CountPerSecond|Ortalama|Saniyede Azure'u giriken bitler|PeeredCircuitSKey|
|GlobalReachBitsOutPerSecond|GlobalReachBitsOutPerSecond|CountPerSecond|Ortalama|Saniyede Azure'dan çıkan bitler|PeeredCircuitSKey|
|BgpKullanılabilirlik|Bgp Kullanılabilirlik|Yüzde|Ortalama|MSEE'den tüm meslektaşlarına yönelik BGP Kullanılabilirliği.|PeeringType,Peer|
|ArpAvailability|Arp Kullanılabilirlik|Yüzde|Ortalama|MSEE'den tüm meslektaşlarına arp kullanılabilirliği.|PeeringType,Peer|
|QosDropBitsInPerİkinci|DüştüInBitsPerSecond|CountPerSecond|Ortalama|Saniyeye bırakılan veri giriş bitleri|None|
|QosDropBitsOutPerSecond|BırakılanOutBitsPerSecond|CountPerSecond|Ortalama|Saniyede bırakılan veri çıkış bitleri|None|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerİkinci|BitsInPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'u giriken bitler|None|
|BitsOutPerİkinci|BitsOutPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'dan çıkan bitler|None|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/bağlantılar

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerİkinci|BitsInPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'u giriken bitler|None|
|BitsOutPerİkinci|BitsOutPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'dan çıkan bitler|None|

## <a name="microsoftnetworkexpressroutegateways"></a>Microsoft.Network/expressRouteGateways

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ErGatewayConnectionBitsInPerİkinci|BitsInPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'u giriken bitler|Connectionname|
|ErGatewayConnectionBitsOutPerSecond|BitsOutPerİkinci|CountPerSecond|Ortalama|Saniyede Azure'dan çıkan bitler|Connectionname|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QpsByEndpoint|Bitiş Noktası İadesi'ne göre sorgular|Sayı|Toplam|Verilen zaman diliminde trafik yöneticisi bitiş noktasının döndürülme sayısı|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Bitiş Noktasına Göre Uç Noktası Durumu|Sayı|Maksimum|Bir bitiş noktasının sonda durumu "Etkinleştirilmişse" 1, aksi takdirde 0.|EndpointName|



## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Sondalar Başarısız Oldu|Yüzde|Ortalama|Bağlantı izleme problarının %'si başarısız oldu|None|
|AverageRoundtripMs|Avg. Gidiş-dönüş Saati (ms)|Milisaniye|Ortalama|Kaynak ve hedef arasında gönderilen bağlantı izleme probları için ortalama ağ gidiş-dönüş süresi (ms)|None|
|ChecksFailedPercent|Denetler Başarısız Yüzde (Önizleme)|Yüzde|Ortalama|Bağlantı izleme denetimlerinin %'si başarısız oldu|SourceAddress,Sourcename,SourceResourceid,SourceType,Protocol,DestinationAddress,DestinationName,DestinationResourceid,DestinationType,DestinationPort,TestGroupName,TestConfigurationname|
|Gidiş-Dönüş Zamanları|Gidiş-Dönüş Süresi (ms) (Önizleme)|Milisaniye|Ortalama|Bağlantı izleme kontrolleri için milisaniye cinsinden gidiş-dönüş süresi|SourceAddress,Sourcename,SourceResourceid,SourceType,Protocol,DestinationAddress,DestinationName,DestinationResourceid,DestinationType,DestinationPort,TestGroupName,TestConfigurationname|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/ön kapılar

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstek Sayısı|İstek Sayısı|Sayı|Toplam|HTTP/S proxy tarafından sunulan istemci isteklerinin sayısı|httpstatus,httpstatusgroup,clientregion,clientcountry|
|İstek Boyutu|İstek Boyutu|Bayt|Toplam|İstemlerden HTTP/S proxy'sine istek olarak gönderilen bayt sayısı|httpstatus,httpstatusgroup,clientregion,clientcountry|
|YanıtBoyutu|Yanıt Boyutu|Bayt|Toplam|HTTP/S proxy'den istemcilere yanıt olarak gönderilen bayt sayısı|httpstatus,httpstatusgroup,clientregion,clientcountry|
|FaturalanabilirYanıtBoyutu|Faturalandırılabilir Yanıt Boyutu|Bayt|Toplam|Http/S proxy'den istemcilere yanıt olarak gönderilen faturalandırılabilir bayt sayısı (istek başına en az 2KB).|httpstatus,httpstatusgroup,clientregion,clientcountry|
|BackendRequestCount|Arka Uç İstek Sayısı|Sayı|Toplam|HTTP/S proxy'sinden arka uçlara gönderilen istek sayısı|httpStatus,HttpStatusGroup,Backend|
|BackendRequestLatency|Arka Uç İstek GecikmeSi|Milisaniye|Ortalama|İsteğin HTTP/S proxy'si tarafından arka uça gönderildiği tarihten, HTTP/S proxy'si arka uçtan son yanıt baytını alana kadar hesaplanan süre|Arka uç|
|TotalLatency|Toplam Gecikme|Milisaniye|Ortalama|İstemci isteğinin HTTP/S proxy'si tarafından alındığı tarihten, istemci NIN HTTP/S proxy'sinden son yanıt baytını kabul edene kadar hesaplanan süre|httpstatus,httpstatusgroup,clientregion,clientcountry|
|BackendHealthPercentage|Arka Uç Sağlık Yüzdesi|Yüzde|Ortalama|HTTP/S proxy'den arka uçlara başarılı sağlık sondalarının yüzdesi|Arka uç,BackendPool|
|WebApplicationFirewallRequestCount|Web Uygulaması Güvenlik Duvarı İstek Sayısı|Sayı|Toplam|Web Uygulaması Güvenlik Duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName,RuleName,Eylem|


## <a name="microsoftnetworkprivatednszones"></a>Microsoft.Network/privateDnsZones

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SorguHacmi|Sorgu Hacmi|Sayı|Toplam|Özel DNS bölgesi için hizmet verilen sorgu sayısı|None|
|Kayıt Seti Sayısı|Kayıt Kümesi Sayısı|Sayı|Maksimum|Özel DNS bölgesindeki Kayıt Kümesi Sayısı|None|
|RecordSetCapacityKullanımation|Kayıt Seti Kapasite Kullanımı|Yüzde|Maksimum|Özel Bir DNS bölgesi tarafından kullanılan Kayıt Kümesi kapasitesinin yüzdesi|None|
|VirtualNetworkLinkCount|Sanal Ağ Bağlantı Sayısı|Sayı|Maksimum|Özel Bir DNS bölgesine bağlı Sanal Ağ Sayısı|None|
|VirtualNetworkLinkCapacityKullanım|Sanal Ağ Bağlantısı Kapasite Kullanımı|Yüzde|Maksimum|Özel Bir DNS bölgesi tarafından kullanılan Sanal Ağ Bağlantısı kapasitesinin yüzdesi|None|
|VirtualNetworkWithRegistrationLinkCount|Sanal Ağ Kayıt Bağlantı Sayısı|Sayı|Maksimum|Otomatik kayıt etkinleştirilmiş özel bir DNS bölgesine bağlı Sanal Ağ Sayısı|None|
|VirtualNetworkWithRegistrationCapacityKullanım|Sanal Ağ Kayıt Bağlantısı Kapasite Kullanımı|Yüzde|Maksimum|Özel Bir DNS bölgesi tarafından kullanılan otomatik kayıt kapasitesine sahip Sanal Ağ Bağlantısı yüzdesi|None|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kayıt.all|Kayıt İşlemleri|Sayı|Toplam|Tüm başarılı kayıt işlemlerinin sayısı (kreasyonlar sorguları ve silmeleri güncelleştirir). |None|
|kayıt.create|Kayıt Oluşturma İşlemleri|Sayı|Toplam|Tüm başarılı kayıt kreasyonlarının sayısı.|None|
|kayıt.update|Kayıt Güncelleme İşlemleri|Sayı|Toplam|Tüm başarılı kayıt güncelleştirmelerinin sayısı.|None|
|kayıt.get|Kayıt Okuma İşlemleri|Sayı|Toplam|Tüm başarılı kayıt sorgularının sayısı.|None|
|kayıt.delete|Kayıt Silme İşlemleri|Sayı|Toplam|Tüm başarılı kayıt silme sayısı.|None|
|Gelen|Gelen Mesajlar|Sayı|Toplam|Tüm başarılı gönderme API çağrılarının sayısı. |None|
|gelen.zamanlanmış|Gönderilen Zamanlanmış Anında Iletme Bildirimleri|Sayı|Toplam|Zamanlanan Anında Iletme Bildirimleri İptal Edildi|None|
|gelen.scheduled.cancel|Zamanlanan Anında Iletme Bildirimleri İptal Edildi|Sayı|Toplam|Zamanlanan Anında Iletme Bildirimleri İptal Edildi|None|
|zamanlandı.beklemede|Bekleyen Zamanlanan Bildirimler|Sayı|Toplam|Bekleyen Zamanlanan Bildirimler|None|
|yükleme.all|Kurulum Yönetimi İşlemleri|Sayı|Toplam|Kurulum Yönetimi İşlemleri|None|
|yükleme.get|Kurulum İşlemleri alın|Sayı|Toplam|Kurulum İşlemleri alın|None|
|installation.upsert|Kurulum İşlemleri Oluşturma veya Güncelleştirme|Sayı|Toplam|Kurulum İşlemleri Oluşturma veya Güncelleştirme|None|
|installation.patch|Yama Kurulum İşlemleri|Sayı|Toplam|Yama Kurulum İşlemleri|None|
|installation.delete|Yükleme Işlemlerini Sil|Sayı|Toplam|Yükleme Işlemlerini Sil|None|
|giden.allpns.başarı|Başarılı bildirimler|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|None|
|giden.allpns.geçersizpayload|Yük Hataları|Sayı|Toplam|PNS kötü bir yük hatası döndürür, çünkü başarısız itme sayısı.|None|
|giden.allpns.pnserror|Dış Bildirim Sistemi Hataları|Sayı|Toplam|PNS ile iletişim kurmada bir sorun olduğundan başarısız olan itme sayısı (kimlik doğrulama sorunlarını hariç tutar).|None|
|giden.allpns.channelerror|Kanal Hataları|Sayı|Toplam|Kanal geçersiz olduğu için başarısız olan itme sayısı, doğru uygulamayla ilişkili değil, daraltıldı veya süresi doldu.|None|
|giden.allpns.badorexpiredchannel|Hatalı veya Süresi Dolmuş Kanal Hataları|Sayı|Toplam|Kayıttaki kanal/belirteç/kayıtId süresi dolmuş veya geçersiz olduğu için başarısız olan itme sayısı.|None|
|giden.wns.success|WNS Başarılı Bildirimler|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|None|
|giden.wns.geçersiz kimlik bilgileri|WNS Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)|Sayı|Toplam|PNS sağlanan kimlik bilgilerini kabul etmediği nden veya kimlik bilgileri engellenmediği için başarısız olan itme sayısı. (Windows Live kimlik bilgilerini tanımıyor).|None|
|giden.wns.badchannel|WNS Kötü Kanal Hatası|Sayı|Toplam|Kayıttaki ChannelURI tanınmadığı için başarısız olan itme sayısı (WNS durumu: 404 bulunamadı).|None|
|giden.wns.expiredchannel|WNS Süresi Dolmuş Kanal Hatası|Sayı|Toplam|ChannelURI süresi dolduğu için başarısız olan itme sayısı (WNS durumu: 410 Gitti).|None|
|giden.wns.throttled|WNS Daraltılmış Bildirimler|Sayı|Toplam|WNS bu uygulamayı daralttığı için başarısız olan itme sayısı (WNS durumu: 406 Kabul Edilemez).|None|
|giden.wns.tokenproviderunreachable|WNS Yetkilendirme Hataları (Erişilemez)|Sayı|Toplam|Windows Live'a erişilemez.|None|
|giden.wns.invalidtoken|WNS Yetkilendirme Hataları (Geçersiz Belirteç)|Sayı|Toplam|WNS'ye sağlanan belirteç geçerli değildir (WNS durumu: 401 Yetkisiz).|None|
|giden.wns.wrongtoken|WNS Yetkilendirme Hataları (Yanlış Belirteç)|Sayı|Toplam|WNS'ye sağlanan belirteç geçerlidir ancak başka bir uygulama için (WNS durumu: 403 Yasak). Bu, kayıttaki ChannelURI başka bir uygulamayla ilişkiliyse gerçekleşebilir. İstemci uygulamasının, kimlik bilgileri bildirim merkezinde bulunan aynı uygulamayla ilişkili olup olmadığını kontrol edin.|None|
|giden.wns.geçersizbildirim formatı|WNS Geçersiz Bildirim Biçimi|Sayı|Toplam|Bildirimbiçimi geçersizdir (WNS durumu: 400). WNS'nin tüm geçersiz yükleri reddetmediğini unutmayın.|None|
|giden.wns.geçersizbildirimboyutu|WNS Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Bildirim yükü çok büyüktür (WNS durumu: 413).|None|
|giden.wns.channelthrottled|WNS Kanal Daraltılmış|Sayı|Toplam|Kayıttaki ChannelURI daraltıldığı için bildirim kaldırıldı (WNS yanıt üstbilgisi: X-WNS-NotificationStatus:channelThrottled).|None|
|giden.wns.channeldisconnected|WNS Kanalı Kesildi|Sayı|Toplam|Kayıttaki ChannelURI daraltıldığı için bildirim kaldırıldı (WNS yanıt üstbilgisi: X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|None|
|giden.wns.düştü|WNS Bırakılan Bildirimler|Sayı|Toplam|Kayıttaki ChannelURI daraldığı için bildirim kaldırıldı (X-WNS-BildirimDurumu: düştü, ancak X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|None|
|giden.wns.pnserror|WNS Hataları|Sayı|Toplam|WNS ile iletişim kuran hatalar nedeniyle bildirim teslim edilmedi.|None|
|giden.wns.authenticationerror|WNS Kimlik Doğrulama Hataları|Sayı|Toplam|Windows Live geçersiz kimlik bilgileri veya yanlış belirteç ile iletişim hataları nedeniyle teslim edilmedi.|None|
|giden.apns.success|APNS Başarılı Bildirimler|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|None|
|giden.apns.geçersiz kimlik bilgileri|APNS Yetkilendirme Hataları|Sayı|Toplam|PNS sağlanan kimlik bilgilerini kabul etmediği nden veya kimlik bilgileri engellenmediği için başarısız olan itme sayısı.|None|
|giden.apns.badchannel|APNS Kötü Kanal Hatası|Sayı|Toplam|Belirteç geçersiz olduğu için başarısız olan itme sayısı (APNS durum kodu: 8).|None|
|giden.apns.expiredchannel|APNS Süresi Dolmuş Kanal Hatası|Sayı|Toplam|APNS geri bildirim kanalı tarafından geçersiz kılınan belirteç sayısı.|None|
|giden.apns.geçersizbildirimboyutu|APNS Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Yük çok büyük olduğu için başarısız olan itme sayısı (APNS durum kodu: 7).|None|
|giden.apns.pnserror|APNS Hataları|Sayı|Toplam|APNS ile iletişim hataları nedeniyle başarısız olan itme sayısı.|None|
|giden.gcm.başarı|GCM Başarılı Bildirimler|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|None|
|giden.gcm.geçersiz kimlik bilgileri|GCM Yetkilendirme Hataları (Geçersiz Kimlik Bilgileri)|Sayı|Toplam|PNS sağlanan kimlik bilgilerini kabul etmediği nden veya kimlik bilgileri engellenmediği için başarısız olan itme sayısı.|None|
|giden.gcm.badchannel|GCM Kötü Kanal Hatası|Sayı|Toplam|KayıtKimliği tanınmadığı için başarısız olan itme sayısı (GCM sonucu: Geçersiz Kayıt).|None|
|giden.gcm.expiredchannel|GCM Süresi Dolmuş Kanal Hatası|Sayı|Toplam|KayıtTaki kayıt Kimliği süresi dolduğu için başarısız olan itme sayısı (GCM sonucu: Kayıtlı değil).|None|
|giden.gcm.throttled|GCM Daraltılmış Bildirimler|Sayı|Toplam|GCM bu uygulamayı daralttığı için başarısız olan itme sayısı (GCM durum kodu: 501-599 veya sonuç:Kullanılamıyor).|None|
|giden.gcm.geçersizbildirim formatı|GCM Geçersiz Bildirim Biçimi|Sayı|Toplam|Yük doğru biçimlendirilmemiş olduğundan başarısız olan itme sayısı (GCM sonucu: InvalidDataKey veya InvalidTtl).|None|
|giden.gcm.geçersizbildirimboyutu|GCM Geçersiz Bildirim Boyutu Hatası|Sayı|Toplam|Yük çok büyük olduğu için başarısız olan itme sayısı (GCM sonucu: MessageTooBig).|None|
|giden.gcm.wrongchannel|GCM Yanlış Kanal Hatası|Sayı|Toplam|Kayıttaki kayıt Kimliği geçerli uygulamayla ilişkili olmadığı için başarısız olan itme sayısı (GCM sonucu: GeçersizPackageName).|None|
|giden.gcm.pnserror|GCM Hataları|Sayı|Toplam|GCM ile iletişim hataları nedeniyle başarısız olan itme sayısı.|None|
|giden.gcm.authenticationerror|GCM Kimlik Doğrulama Hataları|Sayı|Toplam|PNS sağlanan kimlik bilgilerini kabul etmediği için başarısız olan itme sayısı kimlik bilgileri engellenir veya SenderId uygulamada doğru şekilde yapılandırılmamıştır (GCM sonucu: UyumsuzSenderId).|None|
|giden.mpns.success|MPNS Başarılı Bildirimler|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|None|
|giden.mpns.geçersiz kimlik bilgileri|MPNS Geçersiz Kimlik Bilgileri|Sayı|Toplam|PNS sağlanan kimlik bilgilerini kabul etmediği nden veya kimlik bilgileri engellenmediği için başarısız olan itme sayısı.|None|
|giden.mpns.badchannel|MPNS Kötü Kanal Hatası|Sayı|Toplam|Kayıttaki ChannelURI tanınmadığı için başarısız olan itme sayısı (MPNS durumu: 404 bulunamadı).|None|
|giden.mpns.throttled|MPNS Daraltılmış Bildirimler|Sayı|Toplam|MPNS bu uygulamayı daralttığı için başarısız olan itme sayısı (WNS MPNS: 406 Kabul Edilemez).|None|
|giden.mpns.geçersizbildirim formatı|MPNS Geçersiz Bildirim Biçimi|Sayı|Toplam|Bildirimin yükü çok büyük olduğu için başarısız olan itme sayısı.|None|
|giden.mpns.channeldisconnected|MPNS Kanalı Kesildi|Sayı|Toplam|Kayıttaki ChannelURI bağlantısı kesildiği için başarısız olan itme sayısı (MPNS durumu: 412 bulunamadı).|None|
|giden.mpns.düştü|MPNS Bırakılan Bildirimler|Sayı|Toplam|MPNS (MPNS yanıt üstbilgisi: X-NotificationStatus: QueueFull veya Suppressed) tarafından bırakılan itme sayısı.|None|
|giden.mpns.pnserror|MPNS Hataları|Sayı|Toplam|MPNS ile iletişim hataları nedeniyle başarısız olan itme sayısı.|None|
|giden.mpns.kimlik doğrulama hatası|MPNS Kimlik Doğrulama Hataları|Sayı|Toplam|PNS sağlanan kimlik bilgilerini kabul etmediği nden veya kimlik bilgileri engellenmediği için başarısız olan itme sayısı.|None|
|notificationhub.pushs|Tüm Giden Bildirimler|Sayı|Toplam|Bildirim merkezinin tüm giden bildirimleri|None|
|gelen.all.requests|Tüm Gelen İstekler|Sayı|Toplam|Bildirim merkezi için toplam gelen istek|None|
|gelen.all.failedrequests|Tüm Gelen Başarısız İstekler|Sayı|Toplam|Bildirim hub'ı için gelen toplam başarısız istek|None|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/çalışma alanları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|%Average_ Ücretsiz Inodes|% Ücretsiz Inodes|Sayı|Ortalama|%Average_ Ücretsiz Inodes|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Boş Alan|% Boş Alan|Sayı|Ortalama|%Average_ Boş Alan|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kullanılmış Inodes|% Kullanılmış Inodes|Sayı|Ortalama|%Average_ Kullanılmış Inodes|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kullanılmış Alan|% Kullanılan Alan|Sayı|Ortalama|%Average_ Kullanılmış Alan|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Disk Bayt/sn oku|Disk Okuma Bayt/sn|Sayı|Ortalama|Average_Disk Bayt/sn oku|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Disk Okur/sn|Disk Okuma/sn|Sayı|Ortalama|Average_Disk Okur/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Disk Transferleri/sn|Disk Transferleri/sn|Sayı|Ortalama|Average_Disk Transferleri/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Disk Bayt/sn Yaz|Disk Bayt/sn Yazma|Sayı|Ortalama|Average_Disk Bayt/sn Yaz|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Disk Yazıyor/sn|Disk Yazma/sn|Sayı|Ortalama|Average_Disk Yazıyor/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Free Megabayt|Ücretsiz Megabayt|Sayı|Ortalama|Average_Free Megabayt|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Logical Disk Bayt/sn|Mantıksal Disk Baytları/sn|Sayı|Ortalama|Average_Logical Disk Bayt/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kullanılabilir Bellek|% Kullanılabilir Bellek|Sayı|Ortalama|%Average_ Kullanılabilir Bellek|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kullanılabilir Takas Alanı|% Kullanılabilir Takas Alanı|Sayı|Ortalama|%Average_ Kullanılabilir Takas Alanı|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kullanılmış Bellek|% Kullanılan Bellek|Sayı|Ortalama|%Average_ Kullanılmış Bellek|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kullanılmış Takas Alanı|% Kullanılan Takas Alanı|Sayı|Ortalama|%Average_ Kullanılmış Takas Alanı|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Available MBytes Bellek|Kullanılabilir MBytes Bellek|Sayı|Ortalama|Average_Available MBytes Bellek|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Available MBytes Takas|Mevcut MBytes Swap|Sayı|Ortalama|Average_Available MBytes Takas|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Page Okuma/sn|Sayfa Okuma/sn|Sayı|Ortalama|Average_Page Okuma/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Page Yazıyor/sn|Sayfa Yazma/sn|Sayı|Ortalama|Average_Page Yazıyor/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Pages/sn|Sayfalar/sn|Sayı|Ortalama|Average_Pages/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Used MByteS Takas Alanı|İkinci El MByteS Takas Alanı|Sayı|Ortalama|Average_Used MByteS Takas Alanı|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Used Bellek MBytes|İkinci El Bellek MByte'ları|Sayı|Ortalama|Average_Used Bellek MBytes|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Total Bayt Lar Aktarıldı|Aktarılan Toplam Bayt|Sayı|Ortalama|Average_Total Bayt Lar Aktarıldı|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|alınan Average_Total Bayt|Alınan Toplam Bayt Sayısı|Sayı|Ortalama|alınan Average_Total Bayt|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Total Bayt|Toplam Bayt|Sayı|Ortalama|Average_Total Bayt|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Total Paketler İletilen|Aktarılan Toplam Paketler|Sayı|Ortalama|Average_Total Paketler İletilen|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|alınan Average_Total Paket|Alınan Toplam Paket Sayısı|Sayı|Ortalama|alınan Average_Total Paket|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Total Rx Hataları|Toplam Rx Hataları|Sayı|Ortalama|Average_Total Rx Hataları|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Total Tx Hataları|Toplam Tx Hataları|Sayı|Ortalama|Average_Total Tx Hataları|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Total Çarpışmaları|Toplam Çarpışmalar|Sayı|Ortalama|Average_Total Çarpışmaları|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Avg. Disk sn/Okuma|Avg. Disk sn/Oku|Sayı|Ortalama|Average_Avg. Disk sn/Okuma|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Avg. Disk sn/Transfer|Avg. Disk sn/Transfer|Sayı|Ortalama|Average_Avg. Disk sn/Transfer|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Avg. Disk sn/Yazma|Avg. Disk sn/Yazma|Sayı|Ortalama|Average_Avg. Disk sn/Yazma|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Physical Disk Bayt/sn|Fiziksel Disk Bayt/sn|Sayı|Ortalama|Average_Physical Disk Bayt/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Pct Ayrıcalıklı Zaman|Pct Ayrıcalıklı Zaman|Sayı|Ortalama|Average_Pct Ayrıcalıklı Zaman|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Pct Kullanıcı Süresi|Pct Kullanıcı Saati|Sayı|Ortalama|Average_Pct Kullanıcı Süresi|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Used Bellek kByte|İkinci El Bellek kByte'ları|Sayı|Ortalama|Average_Used Bellek kByte|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Virtual Paylaşılan Bellek|Sanal Paylaşılan Bellek|Sayı|Ortalama|Average_Virtual Paylaşılan Bellek|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ DPC Süresi|% DPC Süresi|Sayı|Ortalama|%Average_ DPC Süresi|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Boşta Kalma Süresi|% Boşta Kalma Süresi|Sayı|Ortalama|%Average_ Boşta Kalma Süresi|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kesme Süresi|% Kesme Süresi|Sayı|Ortalama|%Average_ Kesme Süresi|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ IO Bekleme Süresi|% IO Bekleme Süresi|Sayı|Ortalama|%Average_ IO Bekleme Süresi|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Güzel Zaman|% Güzel Zaman|Sayı|Ortalama|%Average_ Güzel Zaman|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Ayrıcalıklı Zaman|% Ayrıcalıklı Zaman|Sayı|Ortalama|%Average_ Ayrıcalıklı Zaman|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ İşlemci Süresi|% İşlemci zamanı|Sayı|Ortalama|%Average_ İşlemci Süresi|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kullanıcı Süresi|% Kullanıcı Süresi|Sayı|Ortalama|%Average_ Kullanıcı Süresi|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Free Fiziksel Bellek|Ücretsiz Fiziksel Bellek|Sayı|Ortalama|Average_Free Fiziksel Bellek|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Sayfalama Dosyalarında Average_Free Alanı|Sayfalama Dosyalarında Boş Alan|Sayı|Ortalama|Sayfalama Dosyalarında Average_Free Alanı|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Free Sanal Bellek|Ücretsiz Sanal Bellek|Sayı|Ortalama|Average_Free Sanal Bellek|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Processes|İşlemler|Sayı|Ortalama|Average_Processes|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Size Sayfalama Dosyalarında Depolanır|Sayfalama Dosyalarında Depolanan Boyut|Sayı|Ortalama|Average_Size Sayfalama Dosyalarında Depolanır|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Uptime|Uptime|Sayı|Ortalama|Average_Uptime|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Users|Kullanıcılar|Sayı|Ortalama|Average_Users|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Current Disk Sıra Uzunluğu|Geçerli Disk Sıra Uzunluğu|Sayı|Ortalama|Average_Current Disk Sıra Uzunluğu|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Available MBytes|Kullanılabilir MBytes|Sayı|Ortalama|Average_Available MBytes|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|%Average_ Kullanımda Taahhüt Edilen Baytlar|% Taahhüt Bayt Lar Kullanımda|Sayı|Ortalama|%Average_ Kullanımda Taahhüt Edilen Baytlar|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|alınan/sn Average_Bytes|Alınan baytlar/sn|Sayı|Ortalama|alınan/sn Average_Bytes|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Bytes Gönderildi/sn|Gönderilen bayt/sn|Sayı|Ortalama|Average_Bytes Gönderildi/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Bytes Toplam/sn|Bayt Toplam/sn|Sayı|Ortalama|Average_Bytes Toplam/sn|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Average_Processor Sıra Uzunluğu|İşlemci Sıra Uzunluğu|Sayı|Ortalama|Average_Processor Sıra Uzunluğu|Bilgisayar,ObjectName,InstanceName,Counterpath,SourceSystem|
|Sinyal|Sinyal|Sayı|Toplam|Sinyal|Bilgisayar, OSType,Sürüm, SourceComputerid|
|Güncelleştirme|Güncelleştirme|Sayı|Ortalama|Güncelleştirme|Bilgisayar, Ürün, Sınıflandırma, Güncelleme Durumu, İsteğe Bağlı, Onaylı|
|Olay|Olay|Sayı|Ortalama|Olay|Kaynak,EventLog,Bilgisayar,EventKategori,EventLevel,EventLevelName,EventID|

## <a name="microsoftpeeringpeeringservices"></a>Microsoft.Peering/peeringServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ÖnekAt|Önek Gecikmesi|Milisaniye|Ortalama|Ortanca önek gecikmesi|ÖnekAdı|

## <a name="microsoftpeeringpeerings"></a>Microsoft.Peering/peerings

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|OturumKullanılabilirlikV4|Oturum Kullanılabilirliği V4|Yüzde|Ortalama|V4 oturumunun kullanılabilirliği|Connectionıd|
|OturumKullanılabilirlikV6|Oturum Kullanılabilirliği V6|Yüzde|Ortalama|V6 oturumunun kullanılabilirliği|Connectionıd|
|IngressTrafficRate|Giriş Trafik Oranı|BitSPerİkinci|Ortalama|Saniyede bit olarak giriş trafiği hızı|Connectionıd|
|ÇıkışTrafikRate|Çıkış Trafik Oranı|BitSPerİkinci|Ortalama|Saniyede bit olarak çıkış trafik hızı|Connectionıd|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/kapasiteleri

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Sorgu Süresi|Sorgu Süresi|Milisaniye|Ortalama|Son aralıktaki DAX Sorgu süresi|Boyut Yok|
|QueryPoolJobQueueLength|İş parçacıkları: Sorgu havuzu iş sırası uzunluğu|Sayı|Ortalama|Sorgu iş parçacığı havuzunun kuyruğundaki iş sayısı.|Boyut Yok|
|qpu_high_utilization_metric|QPU Yüksek Kullanımı|Sayı|Toplam|Son Dakikada QPU Yüksek Kullanımı, 1 Yüksek QPU Kullanımı için, Aksi takdirde 0|Boyut Yok|
|memory_metric|Bellek|Bayt|Ortalama|Bellek. A1 için 0-3 GB, A2 için 0-5 GB, A3 için 0-10 GB, A4 için 0-25 GB, A5 için 0-50 GB ve A6 için 0-100 GB aralığı|Boyut Yok|
|memory_thrashing_metric|Bellek Temizleme|Yüzde|Ortalama|Ortalama hafıza dayak.|Boyut Yok|


## <a name="microsoftprojectbabylonaccounts"></a>Microsoft.ProjectBabylon/hesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Varlık Dağılımı ByClassification|Sınıflandırmaya göre varlık dağılımı|Sayı|Toplam|Belirli bir sınıflandırma atanmış varlıkların sayısını gösterir, yani bu etiketle sınıflandırılır.|Sınıflandırma, Kaynak, ResourceId|
|Varlık DağılımıByStorageType|Depolama türüne göre varlık dağılımı|Sayı|Toplam|Belirli bir depolama türüne sahip varlıkların sayısını gösterir.|Depolama Türü,ResourceId|
|CatalogActiveUsers|Günlük Aktif Kullanıcılar|Sayı|Toplam|Günlük aktif kullanıcı sayısı|ResourceId|
|KatalogKullanımı|Operasyona Göre Kullanım Dağılımı|Sayı|Toplam|Kullanıcının katalogda yaptığı işlem sayısını, yani Erişim, Arama, Sözlük'ü belirtin.|Operasyon,ResourceId|
|NumberOfAssetsWithClassifications|En az bir sınıflandırmaya sahip varlıkların sayısı|Sayı|Ortalama|En az bir etiket sınıflandırması olan varlıkların sayısını gösterir.|ResourceId|
|Scanİptal edildi|Tarama İptal Edildi|Sayı|Toplam|İptal edilen tarama sayısını gösterir.|ResourceId|
|ScanTamamlandı|Tsama Tamamlandı|Sayı|Toplam|Başarıyla tamamlanan tarama sayısını gösterir.|ResourceId|
|ScanFailed|Tolan Başarısız|Sayı|Toplam|Başarısız olan tarama sayısını gösterir.|ResourceId|
|ScanTimeTaken|Alınan süreyi tazyikle|Saniye|Toplam|Saniyecinsinden toplam tbmksüresini gösterir.|ResourceId|




## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DinleyiciBağlantıları-Başarı|DinleyiciBağlantıları-Başarı|Sayı|Toplam|Microsoft.Relay için Başarılı Dinleyici Bağlantıları.|EntityName,İşlem Sonucu|
|DinleyiciConnections-ClientError|DinleyiciConnections-ClientError|Sayı|Toplam|Microsoft.Relay için Dinleyici Bağlantılarında ClientError.|EntityName,İşlem Sonucu|
|DinleyiciConnections-ServerError|DinleyiciConnections-ServerError|Sayı|Toplam|Microsoft.Relay için ListenerConnections servererror.|EntityName,İşlem Sonucu|
|GönderenConnections-Başarı|GönderenConnections-Başarı|Sayı|Toplam|Microsoft.Relay için Başarılı Gönderen Bağlantıları.|EntityName,İşlem Sonucu|
|GönderenBağlantılar-İstemci Hatası|GönderenBağlantılar-İstemci Hatası|Sayı|Toplam|Microsoft.Relay için Gönderen Bağlantılarda İstemci Hatası.|EntityName,İşlem Sonucu|
|GönderenConnections-ServerError|GönderenConnections-ServerError|Sayı|Toplam|Microsoft.Relay için GönderenConnections servererror.|EntityName,İşlem Sonucu|
|DinleyiciBağlantıları-TotalRequests|DinleyiciBağlantıları-TotalRequests|Sayı|Toplam|Microsoft.Relay için Toplam Dinleyici Bağlantıları.|Varlıkadı|
|GönderenBağlantılar-Toplam İstekler|GönderenBağlantılar-Toplam İstekler|Sayı|Toplam|Microsoft.Relay için Toplam GönderenBağlantıları istekleri.|Varlıkadı|
|Aktif Bağlantılar|Aktif Bağlantılar|Sayı|Toplam|Microsoft.Relay için Toplam ActiveConnections.|Varlıkadı|
|ActiveListeners|ActiveListeners|Sayı|Toplam|Microsoft.Relay için Toplam ActiveListener.|Varlıkadı|
|BaytTransfer|BaytTransfer|Sayı|Toplam|Microsoft.Relay için toplam Bytetransfer.|Varlıkadı|
|Dinleyici Keser|Dinleyici Keser|Sayı|Toplam|Microsoft.Relay için Toplam Dinleyici Keser.|Varlıkadı|
|Gönderen Keser|Gönderen Keser|Sayı|Toplam|Microsoft.Relay için Toplam Gönderen Keser.|Varlıkadı|


## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Arama Latency|Arama Gecikmesi|Saniye|Ortalama|Arama hizmeti için ortalama arama gecikmesi|None|
|AramaSorgularPerSecond|Saniyede arama sorguları|CountPerSecond|Ortalama|Arama hizmeti için saniyede arama sorguları|None|
|DaraltılmışAramaSorgularıYüzdesi|Daraltılmış arama sorguları yüzdesi|Yüzde|Ortalama|Arama hizmeti için daraltılmış arama sorgularının yüzdesi|None|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Başarılı İstekler|Başarılı İstekler|Sayı|Toplam|Ad alanı için toplam başarılı istek|EntityName,İşlem Sonucu|
|Sunucu Hataları|Sunucu Hataları.|Sayı|Toplam|Microsoft.ServiceBus için Sunucu Hataları.|EntityName,İşlem Sonucu|
|Kullanıcı Hataları|Kullanıcı Hataları.|Sayı|Toplam|Microsoft.ServiceBus için Kullanıcı Hataları.|EntityName,İşlem Sonucu|
|OperationName|Daraltılmış İstekler.|Sayı|Toplam|Microsoft.ServiceBus için Daraltılmış İstekler.|EntityName,İşlem Sonucu|
|Gelen İstekler|Gelen İstekler|Sayı|Toplam|Microsoft.ServiceBus için gelen istekler.|Varlıkadı|
|Gelen Mesajlar|Gelen Mesajlar|Sayı|Toplam|Microsoft.ServiceBus için gelen iletiler.|Varlıkadı|
|Giden Mesajlar|Giden Mesajlar|Sayı|Toplam|Microsoft.ServiceBus için Giden İletiler.|Varlıkadı|
|Aktif Bağlantılar|Aktif Bağlantılar|Sayı|Toplam|Microsoft.ServiceBus için Toplam Etkin Bağlantı.|None|
|BağlantılarAçıldı|Bağlantılar Açıldı.|Sayı|Ortalama|Microsoft.ServiceBus için Açılan Bağlantılar.|Varlıkadı|
|BağlantılarKapalı|Bağlantılar Kapalı.|Sayı|Ortalama|Microsoft.ServiceBus için Kapalı Bağlantılar.|Varlıkadı|
|Boyut|Boyut|Bayt|Ortalama|Baytlarda Bir Sıra/Konu Boyutu.|Varlıkadı|
|İletiler|Sıra/Konu'daki ileti sayısı.|Sayı|Ortalama|Sıra/Konu'daki ileti sayısı.|Varlıkadı|
|ActiveMessages|Bir Sıra/Konu'daki etkin iletisayısı.|Sayı|Ortalama|Bir Sıra/Konu'daki etkin iletisayısı.|Varlıkadı|
|DeadletteredMesajlar|Sıra/Konu'daki ölü harflerle yazılmış iletilerin sayısı.|Sayı|Ortalama|Sıra/Konu'daki ölü harflerle yazılmış iletilerin sayısı.|Varlıkadı|
|Zamanlanmış Mesajlar|Bir Sıra/Konu'daki zamanlanmış iletisayısı.|Sayı|Ortalama|Bir Sıra/Konu'daki zamanlanmış iletisayısı.|Varlıkadı|
|NamespaceCpuKullanım|CPU|Yüzde|Maksimum|Servis veri günü premium namespace CPU kullanım ölçümü.|Çoğaltma|
|NamespaceMemoryUsage|Bellek Kullanımı|Yüzde|Maksimum|Servis veri günü premium namespace bellek kullanım ölçümü.|Çoğaltma|
|CPUXNS|Cpu (Amortismana Uğradı)|Yüzde|Maksimum|Servis veri günü premium namespace CPU kullanım ölçümü. Bu metrik depricated olduğunu. Bunun yerine lütfen CPU metrik (NamespaceCpuUsage) kullanın.|Çoğaltma|
|WSXNS|Bellek Kullanımı (Amortismana Uğradı)|Yüzde|Maksimum|Servis veri günü premium namespace bellek kullanım ölçümü. Bu metrik amortismana alındı. Bunun yerine Bellek Kullanımı (NamespaceMemoryUsage) metrik kullanın.|Çoğaltma|


## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/uygulamaları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TahsisCpu|TahsisCpu|Sayı|Ortalama|Bu kapsayıcıya milli çekirdek cinsinden ayrılan cpu|Uygulama Adı,Servis Adı,CodePackageName,ServiceReplicaname|
|Ayrılmış Bellek|Ayrılmış Bellek|Bayt|Ortalama|MB'de bu kapsayıcıya ayrılan bellek|Uygulama Adı,Servis Adı,CodePackageName,ServiceReplicaname|
|ActualCpu|ActualCpu|Sayı|Ortalama|Milli çekirdeklerde gerçek CPU kullanımı|Uygulama Adı,Servis Adı,CodePackageName,ServiceReplicaname|
|Fiili Bellek|Fiili Bellek|Bayt|Ortalama|MB'de gerçek bellek kullanımı|Uygulama Adı,Servis Adı,CodePackageName,ServiceReplicaname|
|CpuKullanımı|CpuKullanımı|Yüzde|Ortalama|Bu kapsayıcı için CPU kullanımı AllocatedCPU yüzdesi olarak|Uygulama Adı,Servis Adı,CodePackageName,ServiceReplicaname|
|MemoryKullanımation|MemoryKullanımation|Yüzde|Ortalama|Bu kapsayıcı için CPU kullanımı AllocatedCPU yüzdesi olarak|Uygulama Adı,Servis Adı,CodePackageName,ServiceReplicaname|
|Başvuru Durumu|Başvuru Durumu|Sayı|Ortalama|Hizmet In Durumu Kumaş Örgü uygulaması|Başvuru Adı,Durum|
|Hizmet Durumu|Hizmet Durumu|Sayı|Ortalama|Hizmet Kumaş Örgü uygulamasında bir hizmetin Sağlık Durumu|Başvuru Adı,Durum,Hizmet Adı|
|ServiceReplicaStatus|ServiceReplicaStatus|Sayı|Ortalama|Hizmet Kumaş Örgü uygulamasında bir hizmet çoğaltma Sağlık Durumu|Uygulama Adı,Durum,Hizmet Adı,ServiceReplicaName|
|Konteyner Durumu|Konteyner Durumu|Sayı|Ortalama|Servis Kumaş Örgü uygulamasında konteynerin durumu|Uygulama Adı,Servis Adı,CodePackageName,ServiceReplicaName,Durum|
|Yeniden BaşlatmaSayısı|Yeniden BaşlatmaSayısı|Sayı|Ortalama|Service Fabric Mesh uygulamasında bir konteynerin sayısını yeniden başlatma|Uygulama Adı,Durum,Hizmet Adı,ServiceReplicaName,CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Bağlantı Sayısı|Bağlantı Sayısı|Sayı|Maksimum|Kullanıcı bağlantısı miktarı.|Uç Nokta|
|İleti Sayısı|İleti Sayısı|Sayı|Toplam|İletilerin toplam miktarı.|None|
|Gelen Trafik|Gelen Trafik|Bayt|Toplam|Gelen hizmet trafiği|None|
|GidenTrafik|Giden Trafik|Bayt|Toplam|Giden hizmet trafiği|None|
|Kullanıcı Hataları|Kullanıcı Hataları|Yüzde|Maksimum|Kullanıcı hatalarının yüzdesi|None|
|Sistem Hataları|Sistem Hataları|Yüzde|Maksimum|Sistem hatalarının yüzdesi|None|



## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/veritabanları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|None|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|None|
|log_write_percent|Günlük IO yüzdesi|Yüzde|Ortalama|Günlük IO yüzdesi. Veri ambarları için geçerli değildir.|None|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU Yüzdesi. DTU tabanlı veritabanları için geçerlidir.|None|
|depolama|Kullanılan veri alanı|Bayt|Maksimum|Kullanılan veri alanı. Veri ambarları için geçerli değildir.|None|
|connection_successful|Başarılı Bağlantılar|Sayı|Toplam|Başarılı Bağlantılar|None|
|connection_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|None|
|blocked_by_firewall|Güvenlik Duvarı tarafından engellendi|Sayı|Toplam|Güvenlik Duvarı tarafından engellendi|None|
|Çıkmaz|Kilitlenmeleri|Sayı|Toplam|Kilitlenmeleri. Veri ambarları için geçerli değildir.|None|
|storage_percent|Yüzde kullanılan veri alanı|Yüzde|Maksimum|Veri alanı yüzde kullanılır. Veri ambarları veya hiper ölçekli veritabanları için geçerli değildir.|None|
|xtp_storage_percent|Bellek IÇI OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek IÇI OLTP depolama yüzdesi. Veri ambarları için geçerli değildir.|None|
|workers_percent|İşçi yüzdesi|Yüzde|Ortalama|İşçi yüzdesi. Veri ambarları için geçerli değildir.|None|
|sessions_percent|Oturumyüzdesi|Yüzde|Ortalama|Oturum yüzdesi. Veri ambarları için geçerli değildir.|None|
|dtu_limit|DTU Limiti|Sayı|Ortalama|DTU Limit. DTU tabanlı veritabanları için geçerlidir.|None|
|dtu_used|Kullanılan DTU|Sayı|Ortalama|DTU kullanılır. DTU tabanlı veritabanları için geçerlidir.|None|
|cpu_limit|CPU sınırı|Sayı|Ortalama|CPU sınırı. vCore tabanlı veritabanları için geçerlidir.|None|
|cpu_used|Kullanılan CPU|Sayı|Ortalama|CPU kullanılır. vCore tabanlı veritabanları için geçerlidir.|None|
|dwu_limit|DWU sınırı|Sayı|Maksimum|DWU sınırı. Yalnızca veri ambarları için geçerlidir.|None|
|dwu_consumption_percent|DWU yüzdesi|Yüzde|Maksimum|DWU yüzdesi. Yalnızca veri ambarları için geçerlidir.|None|
|dwu_used|Kullanılan DWU|Sayı|Maksimum|DWU kullanılır. Yalnızca veri ambarları için geçerlidir.|None|
|cache_hit_percent|Önbellek isabet yüzdesi|Yüzde|Maksimum|Önbellek isabet yüzdesi. Yalnızca veri ambarları için geçerlidir.|None|
|cache_used_percent|Önbellek kullanılan yüzde|Yüzde|Maksimum|Önbellek kullanılan yüzde. Yalnızca veri ambarları için geçerlidir.|None|
|sqlserver_process_core_percent<sup>1</sup> |SQL Server işlem çekirdek yüzdesi|Yüzde|Maksimum|İşletim sistemi tarafından ölçülen SQL Server işlemi için CPU kullanım yüzdesi.|None|
|sqlserver_process_memory_percent<sup>1</sup> |SQL Server işleme bellek yüzdesi|Yüzde|Maksimum|İşletim sistemi tarafından ölçülen SQL Server işlemi için bellek kullanım yüzdesi.|None|
|tempdb_data_size<sup>2</sup> |Tempdb Veri Dosya Boyutu Kilobayt|Sayı|Maksimum|Tempdb Veri Dosya Boyutu Kilobayt.|None|
|tempdb_log_size<sup>2</sup> |Tempdb Log Dosya Boyutu Kilobayt|Sayı|Maksimum|Tempdb Log Dosya Boyutu Kilobayt.|None|
|tempdb_log_used_percent<sup>2</sup> |Tempdb Yüzde Günlük Kullanılan|Yüzde|Maksimum|Tempdb Yüzde Günlüğü Kullanılır.|None|
|local_tempdb_usage_percent|Yerel tempdb yüzdesi|Yüzde|Ortalama|Yerel tempdb yüzdesi. Yalnızca veri ambarları için geçerlidir.|None|
|app_cpu_billed|Uygulama CPU faturalı|Sayı|Toplam|Uygulama CPU faturalandırılır. Sunucusuz veritabanları için geçerlidir.|None|
|app_cpu_percent|Uygulama CPU yüzdesi|Yüzde|Ortalama|Uygulama CPU yüzdesi. Sunucusuz veritabanları için geçerlidir.|None|
|app_memory_percent|Uygulama bellek yüzdesi|Yüzde|Ortalama|Uygulama bellek yüzdesi. Sunucusuz veritabanları için geçerlidir.|None|
|allocated_data_storage|Ayrılan veri alanı|Bayt|Ortalama|Tahsis edilen veri depolama. Veri ambarları için geçerli değildir.|None|
|memory_usage_percent|Bellek yüzdesi|Yüzde|Maksimum|Bellek yüzdesi. Yalnızca veri ambarları için geçerlidir.|None|
|dw_backup_size_gb|Veri Depolama Boyutu|Sayı|Toplam|Veri Depolama Boyutu, verilerinizin boyutundan ve işlem günlüğünden oluşur. Metrik, faturanızın 'Depolama' bölümüne doğru sayılır. Yalnızca veri ambarları için geçerlidir.|None|
|dw_snapshot_size_gb|Anlık Görüntü Depolama Boyutu|Sayı|Toplam|Anlık Görüntü Depolama Boyutu, kullanıcı tanımlı ve otomatik geri yükleme noktaları oluşturmak için anlık görüntüler tarafından yakalanan artımlı değişikliklerin boyutudur. Metrik, faturanızın 'Depolama' bölümüne doğru sayılır. Yalnızca veri ambarları için geçerlidir.|None|
|dw_geosnapshot_size_gb|Olağanüstü Durum Kurtarma Depolama Boyutu|Sayı|Toplam|Olağanüstü Durum Kurtarma Depolama Boyutu faturanızda 'Olağanüstü Durum Kurtarma Depolama' olarak yansıtılır. Yalnızca veri ambarları için geçerlidir.|None|
|wlg_allocation_relative_to_system_percent|Sistem yüzdeye göre iş yükü grup tahsisi|Yüzde|Maksimum|İş yükü grubu başına tüm sisteme göre ayrılan kaynak yüzdesi. Yalnızca veri ambarları için geçerlidir.|İş YüküGroupName,IsuserDefined|
|wlg_allocation_relative_to_wlg_effective_cap_percent|Üst kaynak yüzdeye göre iş yükü grubu tahsisi|Yüzde|Maksimum|İş yükü grubu başına belirtilen kap kaynaklarına göre ayrılan kaynakların yüzdesi. Yalnızca veri ambarları için geçerlidir.|İş YüküGroupName,IsuserDefined|
|wlg_active_queries|İş yükü grubu etkin sorguları|Sayı|Toplam|İş yükü grubunda etkin sorgular. Yalnızca veri ambarları için geçerlidir.|İş YüküGroupName,IsuserDefined|
|wlg_queued_queries|İş yükü grubu sıralı sorgular|Sayı|Toplam|İş yükü grubunda sıralanmış sorgular. Yalnızca veri ambarları için geçerlidir.|İş YüküGroupName,IsuserDefined|
|active_queries|Etkin sorgular|Sayı|Toplam|Tüm iş yükü gruplarında etkin sorgular. Yalnızca veri ambarları için geçerlidir.|None|
|queued_queries|Sıralı sorgular|Sayı|Toplam|Tüm iş yükü gruplarında sıralanmış sorgular. Yalnızca veri ambarları için geçerlidir.|None|
|wlg_active_queries_timeouts|İş yükü grubu sorgu zaman ları|Sayı|Toplam|İş yükü grubu için zaman lanmış sorgular. Yalnızca veri ambarları için geçerlidir.|İş YüküGroupName,IsuserDefined|
|wlg_effective_min_resource_percent|Etkili min kaynak yüzdesi|Yüzde|Maksimum|Hizmet düzeyi minimum dikkate alınarak, iş yükü grubu için ayrılmış ve yalıtılmış kaynakların minimum yüzdesi. Yalnızca veri ambarları için geçerlidir.|İş YüküGroupName,IsuserDefined|
|wlg_effective_cap_resource_percent|Etkili kap kaynak yüzdesi|Yüzde|Maksimum|Diğer iş yükü grupları için ayrılan Etkili Min Kaynak Yüzdesini hesaba katarak, iş yükü grubu için izin verilen kaynakların yüzdesine ilişkin sabit bir sınır. Yalnızca veri ambarları için geçerlidir.|İş YüküGroupName,IsuserDefined|
|full_backup_size_bytes|Tam yedekleme depolama boyutu|Bayt|Maksimum|Birikmeli tam yedekleme depolama boyutu. vCore tabanlı veritabanları için geçerlidir. Hyperscale veritabanları için geçerli değildir.|None|
|diff_backup_size_bytes|Diferansiyel yedekleme depolama boyutu|Bayt|Maksimum|Birikmeli diferansiyel yedekleme depolama boyutu. vCore tabanlı veritabanları için geçerlidir. Hyperscale veritabanları için geçerli değildir.|None|
|log_backup_size_bytes|Günlük yedekleme depolama boyutu|Bayt|Maksimum|Kümülatif günlük yedekleme depolama boyutu. vCore tabanlı ve Hyperscale veritabanları için geçerlidir.|None|
|snapshot_backup_size_bytes|Anlık görüntü yedekleme depolama boyutu|Bayt|Maksimum|Birikmeli anlık görüntü yedekleme depolama boyutu. Hyperscale veritabanları için geçerlidir.|None|
|base_blob_size_bytes|Taban blob depolama boyutu|Bayt|Maksimum|Taban blob depolama boyutu. Hyperscale veritabanları için geçerlidir.|None|

<sup>1</sup> Bu metrik, 2 vCore ve daha yüksek vCore veya DTU tabanlı satın alma modelleri için 200 DTU ve daha yüksek vCore satın alma modelini kullanan veritabanları için kullanılabilir. 

<sup>2</sup> Bu metrik, 2 vCore ve daha yüksek vCore satÝn alýnan vCore satýnýnatýs modelini kullanan veritabanları veya DTU tabanlı satın alma modelleri için 200 DTU ve daha yüksek veritabanýr. Bu metrik şu anda Hiper ölçekli veritabanları veya veri ambarları için kullanılamaz.

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|None|
|database_cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|VeritabanıResourceId|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|None|
|database_physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|VeritabanıResourceId|
|log_write_percent|Günlük IO yüzdesi|Yüzde|Ortalama|Günlük IO yüzdesi|None|
|database_log_write_percent|Günlük IO yüzdesi|Yüzde|Ortalama|Günlük IO yüzdesi|VeritabanıResourceId|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU Yüzdesi. DTU tabanlı elastik havuzlar için geçerlidir.|None|
|database_dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|VeritabanıResourceId|
|storage_percent|Yüzde kullanılan veri alanı|Yüzde|Ortalama|Yüzde kullanılan veri alanı|None|
|workers_percent|İşçi yüzdesi|Yüzde|Ortalama|İşçi yüzdesi|None|
|database_workers_percent|İşçi yüzdesi|Yüzde|Ortalama|İşçi yüzdesi|VeritabanıResourceId|
|sessions_percent|Oturumyüzdesi|Yüzde|Ortalama|Oturumyüzdesi|None|
|database_sessions_percent|Oturumyüzdesi|Yüzde|Ortalama|Oturumyüzdesi|VeritabanıResourceId|
|eDTU_limit|eDTU sınırı|Sayı|Ortalama|eDTU sınırı. DTU tabanlı elastik havuzlar için geçerlidir.|None|
|storage_limit|Veri max boyutu|Bayt|Ortalama|Veri max boyutu|None|
|eDTU_used|eDTU kullanılan|Sayı|Ortalama|eDTU kullanılır. DTU tabanlı elastik havuzlar için geçerlidir.|None|
|database_eDTU_used|eDTU kullanılan|Sayı|Ortalama|eDTU kullanılan|VeritabanıResourceId|
|storage_used|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|None|
|database_storage_used|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|VeritabanıResourceId|
|xtp_storage_percent|Bellek IÇI OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek IÇI OLTP depolama yüzdesi|None|
|cpu_limit|CPU sınırı|Sayı|Ortalama|CPU sınırı. vCore tabanlı elastik havuzlar için geçerlidir.|None|
|database_cpu_limit|CPU sınırı|Sayı|Ortalama|CPU sınırı|VeritabanıResourceId|
|cpu_used|Kullanılan CPU|Sayı|Ortalama|CPU kullanılır. vCore tabanlı elastik havuzlar için geçerlidir.|None|
|database_cpu_used|Kullanılan CPU|Sayı|Ortalama|Kullanılan CPU|VeritabanıResourceId|
|sqlserver_process_core_percent<sup>1</sup>|SQL Server işlem çekirdek yüzdesi|Yüzde|Maksimum|İşletim sistemi tarafından ölçülen SQL Server işlemi için CPU kullanım yüzdesi. Elastik havuzlar için geçerlidir. |None|
|sqlserver_process_memory_percent<sup>1</sup>|SQL Server işleme bellek yüzdesi|Yüzde|Maksimum|İşletim sistemi tarafından ölçülen SQL Server işlemi için bellek kullanım yüzdesi. Elastik havuzlar için geçerlidir. |None|
|tempdb_data_size<sup>2</sup>|Tempdb Veri Dosya Boyutu Kilobayt|Sayı|Maksimum|Tempdb Veri Dosya Boyutu Kilobayt.|None|
|tempdb_log_size<sup>2</sup>|Tempdb Log Dosya Boyutu Kilobayt|Sayı|Maksimum|Tempdb Log Dosya Boyutu Kilobayt. |None|
|tempdb_log_used_percent<sup>2</sup>|Tempdb Yüzde Günlük Kullanılan|Yüzde|Maksimum|Tempdb Yüzde Günlüğü Kullanılır.|None|
|allocated_data_storage|Ayrılan veri alanı|Bayt|Ortalama|Ayrılan veri alanı|None|
|database_allocated_data_storage|Ayrılan veri alanı|Bayt|Ortalama|Ayrılan veri alanı|VeritabanıResourceId|
|allocated_data_storage_percent|Yüzde ayrılan veri alanı|Yüzde|Maksimum|Yüzde ayrılan veri alanı|None|

<sup>1</sup> Bu metrik, 2 vCore ve daha yüksek vCore veya DTU tabanlı satın alma modelleri için 200 DTU ve daha yüksek vCore satın alma modelini kullanan veritabanları için kullanılabilir. 

<sup>2</sup> Bu metrik, 2 vCore ve daha yüksek vCore satÝn alýnan vCore satýnýnatýs modelini kullanan veritabanları veya DTU tabanlı satın alma modelleri için 200 DTU ve daha yüksek veritabanýr. Bu metrik şu anda Hiper ölçekli veritabanları için kullanılamıyor.


## <a name="microsoftsqlservers"></a>Microsoft.Sql/sunucular

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|ElasticPoolResourceId|
|database_dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|DatabaseResourceId,ElasticPoolResourceId|
|storage_used|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|ElasticPoolResourceId|
|database_storage_used|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|DatabaseResourceId,ElasticPoolResourceId|
|dtu_used|Kullanılan DTU|Sayı|Ortalama|Kullanılan DTU|VeritabanıResourceId|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|virtual_core_count|Sanal çekirdek sayısı|Sayı|Ortalama|Sanal çekirdek sayısı|None|
|avg_cpu_percent|Ortalama CPU yüzdesi|Yüzde|Ortalama|Ortalama CPU yüzdesi|None|
|reserved_storage_mb|Depolama alanı ayrılmıştır|Sayı|Ortalama|Depolama alanı ayrılmıştır|None|
|storage_space_used_mb|Kullanılan depolama alanı|Sayı|Ortalama|Kullanılan depolama alanı|None|
|io_requests|IO istekleri sayısı|Sayı|Ortalama|IO istekleri sayısı|None|
|io_bytes_read|IO bayt okuyun|Bayt|Ortalama|IO bayt okuyun|None|
|io_bytes_written|IO bayt lar yazılı|Bayt|Ortalama|IO bayt lar yazılı|None|



## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageHesapları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanılmış Kapasite|Kullanılmış kapasite|Bayt|Ortalama|Hesap kullanılan kapasite|None|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Kimlik Doğrulama|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Başarılı bir isteği milisaniye cinsinden işlemek için Azure Depolama tarafından kullanılan ortalama gecikme sonu. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin ortalama uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobKapasite|Blob Kapasitesi|Bayt|Ortalama|Depolama hesabının Blob hizmeti tarafından baytlarda kullanılan depolama miktarı.|BlobType,Seviye|
|BlobCount|Blob Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType,Seviye|
|Konteyner Sayısı|Blob Konteyner Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki kapsayıcı sayısı.|None|
|Dizin Kapasitesi|Dizin Kapasitesi|Bayt|Ortalama|ADLS Gen2 (Hiyerarşik) Endeksi tarafından bayt cinsinden kullanılan depolama miktarı.|None|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Kimlik Doğrulama|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Başarılı bir isteği milisaniye cinsinden işlemek için Azure Depolama tarafından kullanılan ortalama gecikme sonu. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin ortalama uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Tablo Kapasitesi|Tablo Kapasitesi|Bayt|Ortalama|Depolama hesabının Tablo hizmeti tarafından baytlarda kullanılan depolama miktarı.|None|
|Tablo Sayısı|Tablo Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo sayısı.|None|
|TabloEntityCount|Tablo Varlık Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo varlıkların sayısı.|None|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Kimlik Doğrulama|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Başarılı bir isteği milisaniye cinsinden işlemek için Azure Depolama tarafından kullanılan ortalama gecikme sonu. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin ortalama uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Dosya Kapasitesi|Dosya Kapasitesi|Bayt|Ortalama|Depolama hesabının Dosya hizmeti tarafından baytlarda kullanılan depolama alanı miktarı.|Fileshare|
|Dosya Sayısı|Dosya Sayısı|Sayı|Ortalama|Depolama hesabının Dosya hizmetindeki dosya sayısı.|Fileshare|
|FileShareCount|Dosya Paylaşım Sayısı|Sayı|Ortalama|Depolama hesabının Dosya hizmetindeki dosya paylaşımlarının sayısı.|None|
|FileShareSnapshotCount|Dosya paylaşımı anlık görüntü sayısı|Sayı|Ortalama|Depolama hesabının Dosyalar Hizmeti'ndeki paylaşımda bulunan anlık görüntü sayısı.|Fileshare|
|FileShareSnapshotSize|Dosya paylaşımı anlık görüntü boyutu|Bayt|Ortalama|Depolama hesabının Dosya hizmetindeki anlık görüntüler tarafından baytlarda kullanılan depolama alanı miktarı.|Fileshare|
|FileShareQuota|Dosya paylaşımı kota boyutu|Bayt|Ortalama|Azure Dosyalar Hizmeti tarafından baytolarak kullanılabilecek depolama alanı miktarının üst sınırı.|Fileshare|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Authentication,FileShare|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama,FileShare|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama,FileShare|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Başarılı bir isteği milisaniye cinsinden işlemek için Azure Depolama tarafından kullanılan ortalama gecikme sonu. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama,FileShare|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin ortalama uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama,FileShare|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama,FileShare|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Sıra Kapasitesi|Sıra Kapasitesi|Bayt|Ortalama|Depolama hesabının Sıra hizmeti tarafından baytlarda kullanılan depolama alanı miktarı.|None|
|Sıra Sayısı|Sıra Sayısı|Sayı|Ortalama|Depolama hesabının Sıra hizmetindeki sıra sayısı.|None|
|QueueMessageCount|Sıra İleti Sayısı|Sayı|Ortalama|Depolama hesabının Sıra hizmetindeki yaklaşık sıra ileti sayısı.|None|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType,GeoType,ApiName,Kimlik Doğrulama|
|Giriş|Giriş|Bayt|Toplam|Giriş verisi miktarı, baytlar. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Çıkış|Çıkış|Bayt|Toplam|Çıkış verisi miktarı, baytlar halinde. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Başarılı bir isteği milisaniye cinsinden işlemek için Azure Depolama tarafından kullanılan ortalama gecikme sonu. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|GeoType,ApiName,Kimlik Doğrulama|
|SuccessE2ELatency|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine milisaniye cinsinden yapılan başarılı isteklerin ortalama uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|GeoType,ApiName,Kimlik Doğrulama|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|GeoType,ApiName,Kimlik Doğrulama|





## <a name="microsoftstoragecachecaches"></a>Microsoft.StorageÖnbellek/önbellekler

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Müşteri Likleri|Toplam İstemci IOPS|Sayı|Ortalama|Önbellek tarafından işlenen istemci dosyaları işlemlerinin oranı.|None|
|Müşteri Gecikmesi|Ortalama İstemci Gecikmesi|Milisaniye|Ortalama|İstemci dosya işlemlerinin Depolama Önbelleği için ortalama gecikmesi.|None|
|MüşteriReadIOPS|İstemci IOPS Okuyun|CountPerSecond|Ortalama|İstemci işlemleri saniyede okur.|None|
|MüşteriReadThroughput|Ortalama Önbellek Okuma Throughput|BaytPerİkinci|Ortalama|İstemci veri aktarım hızını okudu.|None|
|Müşteri Yazma|İstemci IOPS Yaz|CountPerSecond|Ortalama|İstemci saniyede işlem yazma.|None|
|ClientWriteThroughput|Ortalama Önbellek Yazma Throughput|BaytPerİkinci|Ortalama|İstemci veri aktarım hızı yazın.|None|
|MüşteriMetadataReadIOPS|İstemci Meta veri IOPS okuyun|CountPerSecond|Ortalama|Kalıcı durumu değiştirmeyen veri okumaları hariç, Önbellek'e gönderilen istemci dosya işlemlerinin oranı.|None|
|MüşteriMetadataWriteIOPS|İstemci Meta veri Yazma IOPS|CountPerSecond|Ortalama|Kalıcı durumu değiştiren veri yazmaları hariç, Önbellek'e gönderilen istemci dosya işlemlerinin oranı.|None|
|MüşteriLockiOPS|İstemci Kilidi IOPS|CountPerSecond|Ortalama|İstemci dosya kilitleme işlemleri saniye.|None|
|DepolamaHedefSağlık|Depolama Hedef Sağlık|Sayı|Ortalama|Önbellek ve Depolama Hedefleri arasındaki bağlantı testinin Boolean sonuçları.|None|
|Uptime|Uptime|Sayı|Ortalama|Önbellek ve izleme sistemi arasındaki bağlantı testinin Boolean sonuçları.|None|
|DepolamaTargetIOPS|Toplam DepolamaHedef IOPS|Sayı|Ortalama|Önbelleğin belirli bir StorageTarget'a gönderdiği tüm dosya işlemlerinin hızı.|DepolamaHedefi|
|DepolamaHedefWriteIOPS|StorageTarget Yazma IOPS|Sayı|Ortalama|Önbellek belirli bir StorageTarget'a gönderdiği dosya yazma işlemlerinin hızı.|DepolamaHedefi|
|DepolamaTargetAsyncYazma|StorageTarget Asynchronous Yazma Throughput|BaytPerİkinci|Ortalama|Önbellek belirli bir StorageTarget'a eşzamanlı olarak veri yazma oranı. Bunlar, istemcilerin engellemesine neden olmayan fırsatçı yazılardır.|DepolamaHedefi|
|DepolamaTargetSyncWriteThroughput|StorageTarget Synchronous Yazma Throughput|BaytPerİkinci|Ortalama|Önbellek belirli bir StorageTarget'a eşzamanlı olarak veri yazma oranı. Bunlar istemcilerin engellemesine neden olan yazılardır.|DepolamaHedefi|
|DepolamaTargetTotalWriteThroughput|StorageTarget Toplam Yazma İş Ilik|BaytPerİkinci|Ortalama|Önbellek'in belirli bir StorageTarget'a veri yazma oranı.|DepolamaHedefi|
|DepolamaTargetLatency|DepolamaHedef Gecikme|Milisaniye|Ortalama|Önbellek'in partriküler StorageTarget'e gönderdiği tüm dosya işlemlerinin ortalama gidiş-dönüş gecikmesi.|DepolamaHedefi|
|DepolamaTargetMetadataReadIOPS|StorageTarget Meta data Oku IOPS|CountPerSecond|Ortalama|Kalıcı durumu değiştirmeyen ve Önbellek'in belirli bir Depolama Hedefine gönderdiği okuma işlemini hariç tartışıyorum dosya işlemlerinin oranı.|DepolamaHedefi|
|DepolamaTargetMetadataWriteIOPS|StorageTarget Meta data Write IOPS|CountPerSecond|Ortalama|Önbellek belirli bir StorageTarget gönderir kalıcı durumu değiştirmek ve yazma işlemi hariç dosya işlemleri oranı.|DepolamaHedefi|
|DepolamaHedefReadIOPS|StorageTarget IOPS Okuyun|CountPerSecond|Ortalama|Önbellek'in belirli bir Depolama Hedefine gönderdiği dosya okuma işlem hızı.|DepolamaHedefi|
|DepolamaTargetReadAheadThroughput|StorageTarget İleriyi Oku Bilgi|BaytPerİkinci|Ortalama|Önbellek fırsatçı bir şekilde StorageTarget'ten gelen verileri okuma hızı.|DepolamaHedefi|
|DepolamaHedefDoldurmaThroughput|DepolamaHedef Dolgu Throughput|BaytPerİkinci|Ortalama|Önbellek, önbellek kaçırmayı işlemek için StorageTarget'tan gelen verileri okuma hızıdır.|DepolamaHedefi|
|DepolamaTargetTotalReadThroughput|DepolamaHedef Toplam Okuma İş İlerletme|BaytPerİkinci|Ortalama|Önbellek belirli bir StorageTarget verileri okur toplam oranı.|DepolamaHedefi|

## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Eşitleme Oturum Sonucu|Sayı|Ortalama|Sunucu Bitiş Noktası, Bulut Bitiş Noktası ile eşitleme oturumunu başarıyla tamamlayınca 1 değerini günlüğe kaydeden metrik|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Eşitlenen Dosyalar|Sayı|Toplam|Eşitlenen Dosya Sayısı|SyncGroupName,ServerEndpointName,SyncDirection|
|StorageSyncSyncSessionPeritemErrorsCount|Eşitleme olmayan dosyalar|Sayı|Toplam|Dosya sayısı eşitleme debaşarısız|SyncGroupName,ServerEndpointName,SyncDirection|
|DepolamaSyncBatchTransferredFileBytes|Bayt senkronize|Bayt|Toplam|Eşitleme Oturumları için aktarılan toplam dosya boyutu|SyncGroupName,ServerEndpointName,SyncDirection|
|DepolamaSyncServerKalp Atışı|Sunucu Çevrimiçi Durumu|Sayı|Maksimum|Resigtered sunucu Bulut Bitiş Noktası ile bir sinyal ilerleme başarıyla kaydeder her zaman 1 değeri günlük metrik|aboneliğinde ve|
|DepolamaSyncRecallIOTotalSizeBytes|Bulut katmanlama geri çağırma|Bayt|Toplam|Sunucu tarafından geri çağrılan verilerin toplam boyutu|aboneliğinde ve|
|DepolamaSyncRecalledTotalNetworkBytes|Bulut katmanlama geri çağırma boyutu|Bayt|Toplam|Geri çağrılan verilerin boyutu|SyncGroupName,Sunucu Adı|
|DepolamaSyncRecallThroughputBytesPerSecond|Bulut katmanlama geri çağırma iş|BaytPerİkinci|Ortalama|Veri hatırlama veri sinin boyutu|SyncGroupName,Sunucu Adı|
|StorageSyncRecalledNetworkBytesByApplication|Uygulamaya göre bulut katmanlama geri çağırma boyutu|Bayt|Toplam|Uygulama tarafından geri çağrılan verilerin boyutu|SyncGroupName,ServerName,ApplicationName|

## <a name="microsoftstoragesyncstoragesyncservicessyncgroups"></a>microsoft.storagesync/storageSyncServices/syncGroups

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SyncGroupSyncSessionAppliedFilesCount|Eşitlenen Dosyalar|Sayı|Toplam|Eşitlenen Dosya Sayısı|SyncGroupName,ServerEndpointName,SyncDirection|
|SyncGroupSyncSessionPeritemErrorsCount|Eşitleme olmayan dosyalar|Sayı|Toplam|Dosya sayısı eşitleme debaşarısız|SyncGroupName,ServerEndpointName,SyncDirection|
|SyncGroupBatchTransferredFileBytes|Bayt senkronize|Bayt|Toplam|Eşitleme Oturumları için aktarılan toplam dosya boyutu|SyncGroupName,ServerEndpointName,SyncDirection|

## <a name="microsoftstoragesyncstoragesyncservicessyncgroupsserverendpoints"></a>microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServerEndpointSyncSessionAppliedFilesCount|Eşitlenen Dosyalar|Sayı|Toplam|Eşitlenen Dosya Sayısı|ServerEndpointName,SyncDirection|
|ServerEndpointSyncSessionPerItemErrorsCount|Eşitleme olmayan dosyalar|Sayı|Toplam|Dosya sayısı eşitleme debaşarısız|ServerEndpointName,SyncDirection|
|ServerEndpointBatchTransferredFileBytes|Bayt senkronize|Bayt|Toplam|Eşitleme Oturumları için aktarılan toplam dosya boyutu|ServerEndpointName,SyncDirection|

## <a name="microsoftstoragesyncstoragesyncservicesregisteredservers"></a>microsoft.storagesync/storageSyncServices/registeredServers

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SunucuKalp Atışı|Sunucu Çevrimiçi Durumu|Sayı|Maksimum|Resigtered sunucu Bulut Bitiş Noktası ile bir sinyal ilerleme başarıyla kaydeder her zaman 1 değeri günlük metrik|ServerResourceId,ServerName|
|ServerRecallIOTotalSizeBytes|Bulut katmanlama geri çağırma|Bayt|Toplam|Sunucu tarafından geri çağrılan verilerin toplam boyutu|ServerResourceId,ServerName|



## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kaynak Kullanımı|SU % Kullanım|Yüzde|Maksimum|SU % Kullanım|MantıksalName,PartitionId|
|GirişEtkinlikleri|Giriş Olayları|Sayı|Toplam|Giriş Olayları|MantıksalName,PartitionId|
|InputEventBytes|Giriş Olay Bayt|Bayt|Toplam|Giriş Olay Bayt|MantıksalName,PartitionId|
|LateInputEtkinlikler|Geç Giriş Olayları|Sayı|Toplam|Geç Giriş Olayları|MantıksalName,PartitionId|
|OutputEvents|Çıktı Olayları|Sayı|Toplam|Çıktı Olayları|MantıksalName,PartitionId|
|Dönüşüm Hataları|Veri Dönüştürme Hataları|Sayı|Toplam|Veri Dönüştürme Hataları|MantıksalName,PartitionId|
|Hatalar|Çalışma Zamanı Hataları|Sayı|Toplam|Çalışma Zamanı Hataları|MantıksalName,PartitionId|
|BırakılanOrAdjustedEvents|Sıra dışı Etkinlikler|Sayı|Toplam|Sıra dışı Etkinlikler|MantıksalName,PartitionId|
|AMLCalloutRequests|Fonksiyon İstekleri|Sayı|Toplam|Fonksiyon İstekleri|MantıksalName,PartitionId|
|AMLCalloutFailedRequests|Başarısız Fonksiyon İstekleri|Sayı|Toplam|Başarısız Fonksiyon İstekleri|MantıksalName,PartitionId|
|AMLCalloutInputEtkinlikler|Fonksiyon Etkinlikleri|Sayı|Toplam|Fonksiyon Etkinlikleri|MantıksalName,PartitionId|
|DeserializationHatası|Giriş Deserialization Hataları|Sayı|Toplam|Giriş Deserialization Hataları|MantıksalName,PartitionId|
|EarlyInputOlaylar|Erken Giriş Olayları|Sayı|Toplam|Erken Giriş Olayları|MantıksalName,PartitionId|
|OutputWatermarkDelaySeconds|Filigran Gecikmesi|Saniye|Maksimum|Filigran Gecikmesi|MantıksalName,PartitionId|
|InputEventsSourcesBacklogged|Backlogged Giriş Olayları|Sayı|Maksimum|Backlogged Giriş Olayları|MantıksalName,PartitionId|
|InputEventsSourcesPerSecond|Alınan Giriş Kaynakları|Sayı|Toplam|Alınan Giriş Kaynakları|MantıksalName,PartitionId|

## <a name="microsoftsynapseworkspaces"></a>Microsoft.Synapse/çalışma alanları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|OrkestrasyonPipelineRunsEnded|Boru hattı seferleri sona erdi|Sayı|Toplam|Başarılı olan, başarısız olan veya iptal edilen orkestrasyon ardışık ardışık çalıştırma sayısı|Sonuç,FailureType,Pipeline|
|OrkestrasyonAktiviteRunsEnded|Etkinlik çalıştırmaları sona erdi|Sayı|Toplam|Başarılı, başarısız veya iptal edilen orkestrasyon etkinliklerinin sayısı|Sonuç,Hata Türü,Etkinlik,Etkinlik Türü,Ardışık Hatlar|
|OrkestrasyonTriggersEnded|Tetikleyiciler sona erdi|Sayı|Toplam|Başarılı olan, başarısız olan veya iptal edilen düzenleme tetikleyicilerinin sayısı|Sonuç,FailureType,Tetikleyici|
|SQLOnDemandLoginDenemeler|Giriş denemeleri|Sayı|Toplam|Başarılı olan veya başarısız olan oturum açma denemelerinin sayısı|Sonuç|
|SQLOnDemandQueriesSona erdi|Sorgular sona erdi|Sayı|Toplam|Başarılı olan, başarısız olan veya iptal edilen sorgusayısı|Sonuç|
|SQLOnDemandQueryİşbayt|İşlenen veriler|Bayt|Toplam|Sorgular tarafından işlenen veri miktarı|None|

## <a name="microsoftsynapseworkspacesbigdatapools"></a>Microsoft.Synapse/çalışma alanları/bigDataPools

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SparkJobsEnded|Sona eren uygulamalar|Sayı|Toplam|Sona eren başvuruların sayısı|JobType,JobResult|
|Çekirdek Kapasitesi|Çekirdek kapasitesi|Sayı|Maksimum|Çekirdek kapasitesi|None|
|BellekKapasitesiGB|Bellek kapasitesi (GB)|Sayı|Maksimum|Bellek kapasitesi (GB)|None|

## <a name="microsoftsynapseworkspacessqlpools"></a>Microsoft.Synapse/çalışma alanları/sqlPools

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DWULimit|DWU sınırı|Sayı|Maksimum|SQL havuzunun hizmet düzeyi hedefi|None|
|DWUUsed|Kullanılan DWU|Sayı|Maksimum|SQL havuzunda kullanımın üst düzey bir temsilini temsil eder. DWU limiti * DWU yüzdesi ile ölçülür|None|
|DWUUsedPercent|DWU kullanılan yüzdesi|Yüzde|Maksimum|SQL havuzunda kullanımın üst düzey bir temsilini temsil eder. CPU yüzdesi ile Veri IO yüzdesi arasında maksimum alarak ölçülen|None|
|BağlantılarBlockedByFirewall|Güvenlik duvarı tarafından engellenen bağlantılar|Sayı|Toplam|Güvenlik duvarı kuralları tarafından engellenen bağlantı sayısı. SQL havuzunuz için erişim denetim ilkelerini yeniden ziyaret edin ve sayım yüksekse bu bağlantıları izleyin|None|
|AdaptiveCacheHitPercent|Uyarlanabilir önbellek isabet yüzdesi|Yüzde|Maksimum|İş yüklerinin uyarlanabilir önbelleği ne kadar iyi kullandığını ölçer. Önbelleğe gerek getirmek için ek kapasite yi ölçeklendirmek mi yoksa iş yüklerini yeniden mi çalıştıracağınıbelirlemek için önbellek isabet yüzdesi ölçümü yle bu ölçütü kullanın|None|
|AdaptiveCacheUsedPercent|Adaptif önbellek kullanılan yüzdesi|Yüzde|Maksimum|İş yüklerinin uyarlanabilir önbelleği ne kadar iyi kullandığını ölçer. Önbelleğe almak için ek kapasite yi ölçeklendirmek mi yoksa iş yüklerini yeniden mi çalıştıracağını belirlemek için kullanılan önbellek yüzde ölçümü yle bu ölçütü kullanın|None|
|yereltempdbusedpercent|Yerel tempdb kullanılan yüzdesi|Yüzde|Maksimum|Tüm işlem düğümleri arasında yerel tempdb kullanımı - değerler her beş dakikada bir yayılır|None|
|MemoryUsedPercent|Bellek kullanılan yüzdesi|Yüzde|Maksimum|SQL havuzundaki tüm düğümlerde bellek kullanımı|None|
|Bağlantılar|Bağlantılar|Sayı|Toplam|SQL havuzuna toplam giriş sayısı|Sonuç|
|WLGActiveSorgular|İş yükü grubu etkin sorguları|Sayı|Toplam|İş yükü grubundaki etkin sorgular. Bu metrik filtrelenmemiş ve bölünmemiş kullanarak sistemde çalışan tüm etkin sorguları görüntüler|isuserdefined,iş yükü grubu|
|WLGActiveQueriesTimeouts|İş yükü grubu sorgu zaman ları|Sayı|Toplam|Zaman dolan iş yükü grubu için sorgular. Bu ölçüm tarafından bildirilen sorgu zaman zaman zaman ları yalnızca sorgu yürütmeye başladıktan sonra olur (kilitleme veya kaynak bekleme nedeniyle bekleme süresini içermez)|isuserdefined,iş yükü grubu|
|WLGAllocationBySystemPercent|Sistem yüzdeye göre iş yükü grup tahsisi|Yüzde|Maksimum|Kaynakların tüm sisteme göre yüzde dağılımı|isuserdefined,iş yükü grubu|
|WLGAllocationbyMaxResourcePercent|Maksimum kaynak yüzdeye göre iş yükü grubu tahsisi|Yüzde|Maksimum|İş yükü grubu başına Etkili kap kaynak yüzdesine göre kaynakların yüzde dağılımını görüntüler. Bu metrik, iş yükü grubunun etkin kullanımını sağlar|isuserdefined,iş yükü grubu|
|WLGEffectiveCapResourcePercent|Etkili kap kaynak yüzdesi|Yüzde|Maksimum|İş yükü grubu için etkin kap kaynağı yüzdesi. min_percentage_resource > 0'a sahip başka iş yükü grupları varsa, effective_cap_percentage_resource orantılı olarak düşürülrılır|isuserdefined,iş yükü grubu|
|wlg_effective_min_resource_percent|Etkili min kaynak yüzdesi|Yüzde|Minimum|Etkili min kaynak yüzdesi ayarı, hizmet düzeyi ve iş yükü grubu ayarları göz önünde bulundurularak izin verilir. Etkin min_percentage_resource daha düşük hizmet seviyelerinde daha yüksek ayarlanabilir|isuserdefined,iş yükü grubu|
|WLGQueuedSorgular|İş yükü grubu sıralı sorgular|Sayı|Toplam|Maksimum eşzamanlılık sınırına ulaşıldıktan sonra sıraya alınan isteklerin kümülatif sayısı|isuserdefined,iş yükü grubu|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/ortamlar

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|IngressReceivedMessages|Alınan İletileri Gir|Sayı|Toplam|Tüm Olay hub'ından veya IoT hub olay kaynaklarından okunan ileti sayısı|None|
|IngressReceivedGeçersiz Mesajlar|Giriş Alınan Geçersiz İletiler|Sayı|Toplam|Tüm Olay hub'ından veya IoT hub olay kaynaklarından okunan geçersiz iletisayısı|None|
|IngressReceivedBaytlar|Giriş Alınan Baytlar|Bayt|Toplam|Tüm olay kaynaklarından okunan bayt sayısı|None|
|IngressStoredBayt|Giriş Depolanan Baytlar|Bayt|Toplam|Başarıyla işlenen ve sorgu için kullanılabilir olayların toplam boyutu|None|
|IngressStoredEtkinlikler|Giriş Saklı Etkinlikler|Sayı|Toplam|Başarılı bir şekilde işlenen ve sorgu için kullanılabilir düzleştirilmiş olayların sayısı|None|
|IngressReceivedMessagesTimeLag|Giriş Alınan İletiler Zaman Gecikmesi|Saniye|Maksimum|İletinin olay kaynağında sıralı olduğu saat ile Giriş'te işlendiği saat arasındaki fark|None|
|IngressReceivedMessagesCountLag|Giriş Alınan İletisayısı Gecikmesi|Sayı|Ortalama|Olay kaynağı bölümündeki son sıralanan iletinin sıra numarası ile Giriş'te işlenen iletilerin sıra numarası arasındaki fark|None|
|WarmStorageMaxProperties|Sıcak Depolama Max Özellikleri|Sayı|Maksimum|S1/S2 SKU için çevre tarafından izin verilen maksimum özellik sayısı ve PAYG SKU için Warm Store tarafından izin verilen maksimum özellik sayısı|None|
|WarmStorageUsedÖzellikleri|Sıcak Depolama Kullanılan Özellikler |Sayı|Maksimum|Çevrenin S1/S2 SKU için kullandığı özellik sayısı ve Warm Store tarafından PAYG SKU için kullanılan özellik sayısı|None|



## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/ortamlar/olay kaynakları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|IngressReceivedMessages|Alınan İletileri Gir|Sayı|Toplam|Olay kaynağından okunan ileti sayısı|None|
|IngressReceivedGeçersiz Mesajlar|Giriş Alınan Geçersiz İletiler|Sayı|Toplam|Olay kaynağından okunan geçersiz iletisayısı|None|
|IngressReceivedBaytlar|Giriş Alınan Baytlar|Bayt|Toplam|Olay kaynağından okunan bayt sayısı|None|
|IngressStoredBayt|Giriş Depolanan Baytlar|Bayt|Toplam|Başarıyla işlenen ve sorgu için kullanılabilir olayların toplam boyutu|None|
|IngressStoredEtkinlikler|Giriş Saklı Etkinlikler|Sayı|Toplam|Başarılı bir şekilde işlenen ve sorgu için kullanılabilir düzleştirilmiş olayların sayısı|None|
|IngressReceivedMessagesTimeLag|Giriş Alınan İletiler Zaman Gecikmesi|Saniye|Maksimum|İletinin olay kaynağında sıralı olduğu saat ile Giriş'te işlendiği saat arasındaki fark|None|
|IngressReceivedMessagesCountLag|Giriş Alınan İletisayısı Gecikmesi|Sayı|Ortalama|Olay kaynağı bölümündeki son sıralanan iletinin sıra numarası ile Giriş'te işlenen iletilerin sıra numarası arasındaki fark|None|
|WarmStorageMaxProperties|Sıcak Depolama Max Özellikleri|Sayı|Maksimum|S1/S2 SKU için çevre tarafından izin verilen maksimum özellik sayısı ve PAYG SKU için Warm Store tarafından izin verilen maksimum özellik sayısı|None|
|WarmStorageUsedÖzellikleri|Sıcak Depolama Kullanılan Özellikler |Sayı|Maksimum|Çevrenin S1/S2 SKU için kullandığı özellik sayısı ve Warm Store tarafından PAYG SKU için kullanılan özellik sayısı|None|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudBasit/virtualMachines

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk Okuma Bayt/Sn|BaytPerİkinci|Ortalama|Örnek dönem boyunca okuma işlemleri nedeniyle ortalama disk iş ortası.|None|
|DiskWriteBytesPerSecond|Disk Bayt/Sn Yazma|BaytPerİkinci|Ortalama|Örnek dönem boyunca yazma işlemleri nedeniyle ortalama disk iş ortası.|None|
|Disk Okuma Baytları|Disk Okuma Baytları|Bayt|Toplam|Örnek dönem boyunca okuma işlemleri nedeniyle toplam disk elde etme.|None|
|Disk Yazma Baytları|Disk Yazma Baytları|Bayt|Toplam|Örnek dönem boyunca yazma işlemleri nedeniyle toplam disk işliği.|None|
|DiskReadİşlemleri|Disk Okuma İşlemleri|Sayı|Toplam|Önceki örnek dönemdeki IO okuma işlemleri sayısı. Bu işlemlerin değişken boyutlu olabileceğini unutmayın.|None|
|DiskYazma İşlemleri|Disk Yazma İşlemleri|Sayı|Toplam|Önceki örnek dönemdeki IO yazma işlemlerinin sayısı. Bu işlemlerin değişken boyutlu olabileceğini unutmayın.|None|
|Disk Okuma İşlemleri/Sn|Disk Okuma İşlemleri/Sn|CountPerSecond|Ortalama|Önceki örnek dönemde ki ortalama IO okuma işlemleri sayısı. Bu işlemlerin değişken boyutlu olabileceğini unutmayın.|None|
|Disk Yazma İşlemleri/Sn|Disk Yazma İşlemleri/Sn|CountPerSecond|Ortalama|Önceki örnek dönemdeki ortalama IO yazma işlemi sayısı. Bu işlemlerin değişken boyutlu olabileceğini unutmayın.|None|
|DiskReadLatency|Disk Okuma Gecikmesi|Milisaniye|Ortalama|Toplam okuma gecikmesi. Cihaz ve çekirdek toplamı gecikme leri okuyun.|None|
|DiskWriteLatency|Disk Yazma Gecikme|Milisaniye|Ortalama|Toplam yazma gecikmesi. Aygıt ve çekirdek toplamı gecikme leri yazar.|None|
|NetworkInBytesPerSecond|Ağ Bayt/Sn|BaytPerİkinci|Ortalama|Alınan trafik için ortalama ağ iş ortası.|None|
|AğOutBytesPerSecond|Ağ Çıkış Baytları/Sn|BaytPerİkinci|Ortalama|Aktarılan trafik için ortalama ağ iş ortası.|None|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Alınan trafik için toplam ağ girişi.|None|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Aktarılan trafik için toplam ağ girişi.|None|
|BellekLer Kullanıldı|Kullanılan Bellek|Bayt|Ortalama|VM tarafından kullanılan makine belleği miktarı.|None|
|MemoryVerilen|Bellek Verilen|Bayt|Ortalama|Ev sahibi tarafından VM'ye verilen bellek miktarı. Bir kez dokunulana kadar bellek ana bilgisayara verilmez ve VMkernel'in belleğe ihtiyacı olduğunda bellek değiştirilebilir veya balonla satılabilir.|None|
|MemoryActive|Bellek Etkin|Bayt|Ortalama|VM tarafından geçmiş küçük zaman penceresinde kullanılan bellek miktarı. Bu, VM'nin şu anda ne kadar belleğe ihtiyacı olduğunun "gerçek" sayısıdır. Ek olarak, kullanılmayan bellek değiştirilebilir veya konuğun performansını etkilemeden balonla şişirilebilir.|None|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|CPU kullanımı. Bu değer, sistemdeki tüm işlemci çekirdeklerini temsil eden %100 ile bildirilir. Örnek olarak, dört çekirdekli sistemin %50'sini kullanan 2 yönlü bir VM tamamen iki çekirdek kullanıyor.|None|
|YüzdeCpuReady|Yüzde CPU Hazır|Milisaniye|Toplam|Hazır zaman, CPU'nun(lar) geçmiş güncelleştirme aralığında kullanılabilir olmasını bekleyerek geçen süredir.|None|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuYüzdesi|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|Bellek Yüzdesi|Bellek Yüzdesi|Yüzde|Ortalama|Bellek Yüzdesi|Örnek|
|DiskQueueLength|Disk Sıra Uzunluğu|Sayı|Ortalama|Disk Sıra Uzunluğu|Örnek|
|httpQueueLength|Http Sıra Uzunluğu|Sayı|Ortalama|Http Sıra Uzunluğu|Örnek|
|BaytAlınan|Veri In|Bayt|Toplam|Veri In|Örnek|
|Baytgönderildi|Veri Çıkış|Bayt|Toplam|Veri Çıkış|Örnek|
|TcpSynSent|TCP Syn Gönderildi|Sayı|Ortalama|TCP Syn Gönderildi|Örnek|
|TcpSynReceived|TCP Syn Alındı|Sayı|Ortalama|TCP Syn Alındı|Örnek|
|TcpKuruldu|TCP Kuruldu|Sayı|Ortalama|TCP Kuruldu|Örnek|
|TcpFinWait1|TCP Fin Bekle 1|Sayı|Ortalama|TCP Fin Bekle 1|Örnek|
|TcpFinWait2|TCP Fin Bekle 2|Sayı|Ortalama|TCP Fin Bekle 2|Örnek|
|TcpKapanış|TCP Kapanış|Sayı|Ortalama|TCP Kapanış|Örnek|
|TcpCloseWait|TCP Kapanış Bekle|Sayı|Ortalama|TCP Kapanış Bekle|Örnek|
|TcpLastAck|TCP Son Ack|Sayı|Ortalama|TCP Son Ack|Örnek|
|TcpTimeWait|TCP Zaman Bekle|Sayı|Ortalama|TCP Zaman Bekle|Örnek|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/siteler (işlevler hariç) 

> [!NOTE]
> **Dosya Sistemi Kullanımı,** genel olarak kullanıma çıkarılan yeni bir metriktir, özel önizleme için beyaz listeye alınmadıkça veri beklenmiyor.

> [!IMPORTANT]
> **Ortalama Yanıt Süresi,** metrik toplamalarla karışıklığı önlemek için amortismana kesilir. **Yanıt Süresini** yedek olarak kullanın.

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuTime|CPU Süresi|Saniye|Toplam|CPU Süresi|Örnek|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BaytAlınan|Veri In|Bayt|Toplam|Veri In|Örnek|
|Baytgönderildi|Veri Çıkış|Bayt|Toplam|Veri Çıkış|Örnek|
|Http101 ile|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401 sayılı|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403 sayılı|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406 sayılı|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|OrtalamaMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|Yanıt Süresi|Yanıt Süresi|Saniye|Toplam|Yanıt Süresi|Örnek|
|Ortalama Yanıt Süresi|Ortalama Yanıt Süresi (amortismana uğradı)|Saniye|Ortalama|Ortalama Yanıt Süresi|Örnek|
|AppConnections|Bağlantılar|Sayı|Ortalama|Bağlantılar|Örnek|
|Handles|Tanıtıcı Sayısı|Sayı|Ortalama|Tanıtıcı Sayısı|Örnek|
|İş Parçacıkları|Ilmekli|Sayı|Ortalama|Ilmekli|Örnek|
|Özel Baytlar|Özel Baytlar|Bayt|Ortalama|Özel Baytlar|Örnek|
|IoReadBytesPerSecond|IO Saniyede Bayt Oku|BaytPerİkinci|Toplam|IO Saniyede Bayt Oku|Örnek|
|IoWriteBytesPerSecond|IO Saniyede Bayt Yaz|BaytPerİkinci|Toplam|IO Saniyede Bayt Yaz|Örnek|
|IoOtherBytesPerSecond|IO Saniyede Diğer Baytlar|BaytPerİkinci|Toplam|IO Saniyede Diğer Baytlar|Örnek|
|IoReadOperationsPerSecond|IO Saniyede Okuma İşlemleri|BaytPerİkinci|Toplam|IO Saniyede Okuma İşlemleri|Örnek|
|IoWriteOperationsPerSecond|IO Saniyede Yazma İşlemleri|BaytPerİkinci|Toplam|IO Saniyede Yazma İşlemleri|Örnek|
|IoOtherOperationsPerSecond|IO Saniyede Diğer İşlemler|BaytPerİkinci|Toplam|IO Saniyede Diğer İşlemler|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Sayı|Ortalama|Uygulama Kuyruğundaki İstekler|Örnek|
|Güncel Montajlar|Geçerli Meclisler|Sayı|Ortalama|Geçerli Meclisler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanı|Sayı|Ortalama|Toplam Uygulama Etki Alanı|Örnek|
|TotalAppDomainsUnloaded|Toplam Uygulama Etki Alanı Boşaltıldı|Sayı|Ortalama|Toplam Uygulama Etki Alanı Boşaltıldı|Örnek|
|Gen0Koleksiyonlar|Gen 0 Çöp Koleksiyonları|Sayı|Toplam|Gen 0 Çöp Koleksiyonları|Örnek|
|Gen1Koleksiyonlar|Gen 1 Çöp Koleksiyonları|Sayı|Toplam|Gen 1 Çöp Koleksiyonları|Örnek|
|Gen2Koleksiyonlar|Gen 2 Çöp Koleksiyonları|Sayı|Toplam|Gen 2 Çöp Koleksiyonları|Örnek|
|HealthCheckDurumu|Sağlık kontrolü durumu|Sayı|Ortalama|Sağlık kontrolü durumu|Örnek|
|FileSystemUsage|Dosya Sistemi Kullanımı|Bayt|Ortalama|Dosya Sistemi Kullanımı|None|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/siteler (işlevler)

> [!NOTE]
> **Dosya Sistemi Kullanımı,** genel olarak kullanıma çıkarılan yeni bir metriktir, özel önizleme için beyaz listeye alınmadıkça veri beklenmiyor.

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BaytAlınan|Veri In|Bayt|Toplam|Veri In|Örnek|
|Baytgönderildi|Veri Çıkış|Bayt|Toplam|Veri Çıkış|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|OrtalamaMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|Fonksiyonyürütme Birimleri|Fonksiyon Yürütme Birimleri|MB / Milisaniye|Toplam|[Fonksiyon Yürütme Birimleri](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Örnek|
|FonksiyonYürütme Sayısı|İşlev Yürütme Sayısı|Sayı|Toplam|İşlev Yürütme Sayısı|Örnek|
|Özel Baytlar|Özel Baytlar|Bayt|Ortalama|Özel Baytlar|Örnek|
|IoReadBytesPerSecond|IO Saniyede Bayt Oku|BaytPerİkinci|Toplam|IO Saniyede Bayt Oku|Örnek|
|IoWriteBytesPerSecond|IO Saniyede Bayt Yaz|BaytPerİkinci|Toplam|IO Saniyede Bayt Yaz|Örnek|
|IoOtherBytesPerSecond|IO Saniyede Diğer Baytlar|BaytPerİkinci|Toplam|IO Saniyede Diğer Baytlar|Örnek|
|IoReadOperationsPerSecond|IO Saniyede Okuma İşlemleri|BaytPerİkinci|Toplam|IO Saniyede Okuma İşlemleri|Örnek|
|IoWriteOperationsPerSecond|IO Saniyede Yazma İşlemleri|BaytPerİkinci|Toplam|IO Saniyede Yazma İşlemleri|Örnek|
|IoOtherOperationsPerSecond|IO Saniyede Diğer İşlemler|BaytPerİkinci|Toplam|IO Saniyede Diğer İşlemler|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Sayı|Ortalama|Uygulama Kuyruğundaki İstekler|Örnek|
|Güncel Montajlar|Geçerli Meclisler|Sayı|Ortalama|Geçerli Meclisler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanı|Sayı|Ortalama|Toplam Uygulama Etki Alanı|Örnek|
|TotalAppDomainsUnloaded|Toplam Uygulama Etki Alanı Boşaltıldı|Sayı|Ortalama|Toplam Uygulama Etki Alanı Boşaltıldı|Örnek|
|Gen0Koleksiyonlar|Gen 0 Çöp Koleksiyonları|Sayı|Toplam|Gen 0 Çöp Koleksiyonları|Örnek|
|Gen1Koleksiyonlar|Gen 1 Çöp Koleksiyonları|Sayı|Toplam|Gen 1 Çöp Koleksiyonları|Örnek|
|Gen2Koleksiyonlar|Gen 2 Çöp Koleksiyonları|Sayı|Toplam|Gen 2 Çöp Koleksiyonları|Örnek|
|HealthCheckDurumu|Sağlık kontrolü durumu|Sayı|Ortalama|Sağlık kontrolü durumu|Örnek|
|FileSystemUsage|Dosya Sistemi Kullanımı|Bayt|Ortalama|Dosya Sistemi Kullanımı|None|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/siteler/yuvalar

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuTime|CPU Süresi|Saniye|Toplam|CPU Süresi|Örnek|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BaytAlınan|Veri In|Bayt|Toplam|Veri In|Örnek|
|Baytgönderildi|Veri Çıkış|Bayt|Toplam|Veri Çıkış|Örnek|
|Http101 ile|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401 sayılı|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403 sayılı|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406 sayılı|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|OrtalamaMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|Ortalama Yanıt Süresi|Ortalama Yanıt Süresi|Saniye|Ortalama|Ortalama Yanıt Süresi|Örnek|
|httpResponseTime|Yanıt Süresi|Saniye|Ortalama|Yanıt Süresi|Örnek|
|Fonksiyonyürütme Birimleri|Fonksiyon Yürütme Birimleri|Sayı|Toplam|Fonksiyon Yürütme Birimleri|Örnek|
|FonksiyonYürütme Sayısı|İşlev Yürütme Sayısı|Sayı|Toplam|İşlev Yürütme Sayısı|Örnek|
|AppConnections|Bağlantılar|Sayı|Ortalama|Bağlantılar|Örnek|
|Handles|Tanıtıcı Sayısı|Sayı|Ortalama|Tanıtıcı Sayısı|Örnek|
|İş Parçacıkları|Ilmekli|Sayı|Ortalama|Ilmekli|Örnek|
|Özel Baytlar|Özel Baytlar|Bayt|Ortalama|Özel Baytlar|Örnek|
|IoReadBytesPerSecond|IO Saniyede Bayt Oku|BaytPerİkinci|Toplam|IO Saniyede Bayt Oku|Örnek|
|IoWriteBytesPerSecond|IO Saniyede Bayt Yaz|BaytPerİkinci|Toplam|IO Saniyede Bayt Yaz|Örnek|
|IoOtherBytesPerSecond|IO Saniyede Diğer Baytlar|BaytPerİkinci|Toplam|IO Saniyede Diğer Baytlar|Örnek|
|IoReadOperationsPerSecond|IO Saniyede Okuma İşlemleri|BaytPerİkinci|Toplam|IO Saniyede Okuma İşlemleri|Örnek|
|IoWriteOperationsPerSecond|IO Saniyede Yazma İşlemleri|BaytPerİkinci|Toplam|IO Saniyede Yazma İşlemleri|Örnek|
|IoOtherOperationsPerSecond|IO Saniyede Diğer İşlemler|BaytPerİkinci|Toplam|IO Saniyede Diğer İşlemler|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Sayı|Ortalama|Uygulama Kuyruğundaki İstekler|Örnek|
|Güncel Montajlar|Geçerli Meclisler|Sayı|Ortalama|Geçerli Meclisler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanı|Sayı|Ortalama|Toplam Uygulama Etki Alanı|Örnek|
|TotalAppDomainsUnloaded|Toplam Uygulama Etki Alanı Boşaltıldı|Sayı|Ortalama|Toplam Uygulama Etki Alanı Boşaltıldı|Örnek|
|Gen0Koleksiyonlar|Gen 0 Çöp Koleksiyonları|Sayı|Toplam|Gen 0 Çöp Koleksiyonları|Örnek|
|Gen1Koleksiyonlar|Gen 1 Çöp Koleksiyonları|Sayı|Toplam|Gen 1 Çöp Koleksiyonları|Örnek|
|Gen2Koleksiyonlar|Gen 2 Çöp Koleksiyonları|Sayı|Toplam|Gen 2 Çöp Koleksiyonları|Örnek|
|HealthCheckDurumu|Sağlık kontrolü durumu|Sayı|Ortalama|Sağlık kontrolü durumu|Örnek|
|FileSystemUsage|Dosya Sistemi Kullanımı|Bayt|Ortalama|Dosya Sistemi Kullanımı|None|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingOrtamlar/multiRolePools

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BaytAlınan|Veri In|Bayt|Toplam|Veri In|Örnek|
|Baytgönderildi|Veri Çıkış|Bayt|Toplam|Veri Çıkış|Örnek|
|Http101 ile|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401 sayılı|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403 sayılı|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|Http 404|Sayı|Toplam|Http 404|Örnek|
|Http406 sayılı|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Sayı|Toplam|Http Sunucu Hataları|Örnek|
|Ortalama Yanıt Süresi|Ortalama Yanıt Süresi|Saniye|Ortalama|Ortalama Yanıt Süresi|Örnek|
|CpuYüzdesi|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|Bellek Yüzdesi|Bellek Yüzdesi|Yüzde|Ortalama|Bellek Yüzdesi|Örnek|
|DiskQueueLength|Disk Sıra Uzunluğu|Sayı|Ortalama|Disk Sıra Uzunluğu|Örnek|
|httpQueueLength|Http Sıra Uzunluğu|Sayı|Ortalama|Http Sıra Uzunluğu|Örnek|
|Aktif İstekler|Etkin İstekler|Sayı|Toplam|Etkin İstekler|Örnek|
|Toplam FrontEnds|Toplam Ön Uç|Sayı|Ortalama|Toplam Ön Uç|None|
|SmallAppServicePlanInstances|Küçük Uygulama Hizmet Planı Çalışanları|Sayı|Ortalama|Küçük Uygulama Hizmet Planı Çalışanları|None|
|MediumAppServicePlanInstances|Orta Uygulama Hizmet Planı İşçileri|Sayı|Ortalama|Orta Uygulama Hizmet Planı İşçileri|None|
|LargeAppServicePlanInstances|Büyük Uygulama Hizmet Planı Çalışanları|Sayı|Ortalama|Büyük Uygulama Hizmet Planı Çalışanları|None|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingOrtamlar/işçiHavuzları

|Ölçüm|Metrik Görüntü Adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İşçiToplam|Toplam İşçi|Sayı|Ortalama|Toplam İşçi|None|
|İşçiler Kullanılabilir|Mevcut İşçiler|Sayı|Ortalama|Mevcut İşçiler|None|
|İşçiler Kullanılmış|İkinci El İşçiler|Sayı|Ortalama|İkinci El İşçiler|None|
|CpuYüzdesi|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|Bellek Yüzdesi|Bellek Yüzdesi|Yüzde|Ortalama|Bellek Yüzdesi|Örnek|
## <a name="next-steps"></a>Sonraki adımlar
* [Azure Monitor'da ölçümler hakkında bilgi edinin](data-platform.md)
* [Ölçümler üzerinde uyarı oluşturma](alerts-overview.md)
* [Ölçümleri depolama, Olay Merkezi veya Günlük Analizine aktarma](platform-logs-overview.md)

