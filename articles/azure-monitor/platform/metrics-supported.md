---
title: Azure Izleyici kaynak türüne göre desteklenen ölçümler
description: Azure Izleyici ile her kaynak türü için kullanılabilen ölçümlerin listesi.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/20/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: dbbe5a667b8d467b416e4a4a571d8d3599ec45b6
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051810"
---
# <a name="supported-metrics-with-azure-monitor"></a>Azure Izleyici ile desteklenen ölçümler

Azure Izleyici, ölçümlerle etkileşimde bulunmak için, portalda grafik oluşturma, REST API aracılığıyla erişme veya PowerShell veya CLı kullanarak sorgulama gibi çeşitli yollar sağlar. Aşağıda, Azure Izleyici 'nin ölçüm işlem hattı ile Şu anda kullanılabilir olan tüm ölçümlerin tamamı listelenmiştir. Diğer ölçümler portalda veya eski API 'Ler kullanılarak kullanılabilir. Aşağıdaki listede yalnızca birleştirilmiş Azure Izleyici ölçüm işlem hattı kullanılarak kullanılabilen ölçümler bulunur. Bu ölçümleri sorgulamak ve bunlara erişmek için lütfen [2018-01-01 API-sürümünü](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) kullanın

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir olay hub 'ındaki ' gelen Iletiler ' ölçümü, sıra düzeyinde araştırılabilir ve görüntülenebilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft. AnalysisServices/sunucuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|qpu_metric|QPU|Count|Average|QPU. S1 için 0-100 aralığı, S2 için 0-200 ve S4 için 0-400|ServerResourceType|
|memory_metric|Bellek|Bayt|Average|Bellek. S1 için 0-25 GB, S2 için 0-50 GB ve S4 için 0-100 GB|ServerResourceType|
|TotalConnectionRequests|Toplam bağlantı Isteği sayısı|Count|Average|Toplam bağlantı isteği sayısı. Bunlar, varışlardır.|ServerResourceType|
|Başarılı bir bağlantı Spersec|Saniyedeki başarılı bağlantı sayısı|CountPerSecond|Average|Başarılı bağlantı tamamlama oranı.|ServerResourceType|
|Totalconnectionarızaları|Toplam bağlantı başarısızlığı sayısı|Count|Average|Toplam başarısız bağlantı denemesi.|ServerResourceType|
|CurrentUserSessions|Geçerli Kullanıcı oturumları|Count|Average|Geçerli kullanıcı oturumlarının sayısı.|ServerResourceType|
|Querypoolbusyıthreads|Sorgu havuzu meşgul Iş parçacıkları|Count|Average|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolJobQueueLength|Komut havuzu Iş kuyruğu uzunluğu|Count|Average|Komut iş parçacığı havuzu sırasındaki iş sayısı.|ServerResourceType|
|ProcessingPoolJobQueueLength|İşleme havuzu Iş kuyruğu uzunluğu|Count|Average|İşleme iş parçacığı havuzu kuyruğundaki g/ç dışı iş sayısı.|ServerResourceType|
|CurrentConnections|Bağlantı: Geçerli bağlantılar|Count|Average|Kurulan istemci bağlantılarının geçerli sayısı.|ServerResourceType|
|CleanerCurrentPrice|Bellek: Temizleyici geçerli fiyat|Count|Average|Geçerli bellek fiyatı, $/Byte/Time, 1000 olarak normalleştirilemez.|ServerResourceType|
|Cleanermemoryshrınkable|Bellek: Temizleyici bellek shrınılabilir|Bayt|Average|Arka plan temizleyici tarafından temizlenme konusu bayt cinsinden bellek miktarı.|ServerResourceType|
|Cleanermemorynonınkable|Bellek: Temizleyici bellek daraltılamaz|Bayt|Average|Arka plan temizleyici tarafından temizlenme konusu değil, bayt cinsinden bellek miktarı.|ServerResourceType|
|MemoryUsage|Bellek: Bellek Kullanımı|Bayt|Average|Sunucu işleminin temizleyici bellek fiyatını hesaplamada kullanılan bellek kullanımı. XVelocity altyapısının bellek sınırının fazla olması halinde, xVelocity bellek içi analiz altyapısı (VertiPaq) tarafından eşlenen veya ayrılan belleği yoksayarak, işlem \ bayt ve bellek eşlemeli verilerin boyutuna eşit.|ServerResourceType|
|MemoryLimitHard|Bellek: Bellek sınırı sabit|Bayt|Average|Yapılandırma dosyasından sabit bellek sınırı.|ServerResourceType|
|Memoryhighlimit|Bellek: Bellek sınırı yüksek|Bayt|Average|Yapılandırma dosyasından yüksek bellek sınırı.|ServerResourceType|
|MemoryLimitLow|Bellek: Bellek sınırı düşük|Bayt|Average|Yapılandırma dosyasından düşük bellek sınırı.|ServerResourceType|
|MemoryLimitVertiPaq|Bellek: Bellek sınırı VertiPaq|Bayt|Average|Yapılandırma dosyasından bellek içi sınır.|ServerResourceType|
|Kota|Bellek: Kota|Bayt|Average|Bayt cinsinden geçerli bellek kotası. Bellek kotası, bellek verme veya bellek ayırma olarak da bilinir.|ServerResourceType|
|Quotabkilitli|Bellek: Kota engellendi|Count|Average|Diğer bellek kotaları boşaltılana kadar engellenen kota isteklerinin geçerli sayısı.|ServerResourceType|
|Vertipaqdisk belleksiz|Bellek: VertiPaq disk belleksiz|Bayt|Average|Bellek içi altyapı tarafından kullanılmak üzere çalışma kümesinde kilitlenen bellek baytları.|ServerResourceType|
|Vertipaqdisk|Bellek: VertiPaq disk belleğine|Bayt|Average|Bellek içi veriler için kullanılan disk belleğine alınan bellek miktarı.|ServerResourceType|
|RowsReadPerSec|İşlerken Okunan satır sayısı/saniye|CountPerSecond|Average|Tüm ilişkisel veritabanlarından okunan satır oranı.|ServerResourceType|
|RowsConvertedPerSec|İşlerken Dönüştürülen satır sayısı/saniye|CountPerSecond|Average|İşlem sırasında dönüştürülen satır oranı.|ServerResourceType|
|RowsWrittenPerSec|İşlerken Saniyede yazılan satır sayısı|CountPerSecond|Average|İşlem sırasında yazılan satır oranı.|ServerResourceType|
|Commandpoolbusyıthreads|Akışları Komut havuzu meşgul iş parçacıkları|Count|Average|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolIdleThreads|Akışları Komut havuzu boşta iş parçacıkları|Count|Average|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|Longparsingbusyıthreads|Akışları Uzun ayrıştırma meşgul iş parçacıkları|Count|Average|Uzun ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|LongParsingIdleThreads|Akışları Uzun ayrıştırma boşta iş parçacıkları|Count|Average|Uzun ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|LongParsingJobQueueLength|Akışları Uzun süre ayrıştırması iş kuyruğu uzunluğu|Count|Average|Uzun ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|Processingpoolbusyıiojobthreads|Akışları İşlem havuzu meşgul g/ç işi iş parçacıkları|Count|Average|İşleme iş parçacığı havuzunda g/ç işleri çalıştıran iş parçacığı sayısı.|ServerResourceType|
|Processingpoolbusınonıothreads|Akışları İşlem havuzu meşgul olmayan g/ç iş parçacıkları|Count|Average|İşleme iş parçacığı havuzunda g/ç olmayan işler çalıştıran iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Akışları İşlem havuzu g/ç iş kuyruğu uzunluğu|Count|Average|İşleme iş parçacığı havuzu kuyruğundaki g/ç işlerinin sayısı.|ServerResourceType|
|Processingpokaydliiojobthreads|Akışları İşlem havuzu boşta g/ç işi iş parçacıkları|Count|Average|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|Processingpokaydlenoniothreads|Akışları İşlem havuzu boşta g/ç olmayan iş parçacıkları|Count|Average|İşleme iş parçacığı havuzunda g/ç dışı işlere adanmış boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolIdleThreads|Akışları Sorgu havuzu boşta iş parçacıkları|Count|Average|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolJobQueueLength|Akışları Sorgu havuzu iş kuyruğu uzunluğu|Count|Average|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|Shortparsingbusyıthreads|Akışları Kısa ayrıştırma meşgul iş parçacıkları|Count|Average|Kısa ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|ShortParsingIdleThreads|Akışları Kısa ayrıştırma boşta iş parçacıkları|Count|Average|Kısa ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|ShortParsingJobQueueLength|Akışları Kısa ayrıştırma iş kuyruğu uzunluğu|Count|Average|Kısa ayrıştırma iş parçacığı havuzu sırasındaki iş sayısı.|ServerResourceType|
|memory_thrashing_metric|Bellek dağılması|Percent|Average|Ortalama bellek miktarı.|ServerResourceType|
|mashup_engine_qpu_metric|A motoru QPU|Count|Average|Karma altyapı işlemlerine göre QPU kullanımı|ServerResourceType|
|mashup_engine_memory_metric|D motoru belleği|Bayt|Average|Karma altyapı işlemlerine göre bellek kullanımı|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstekler|İstekler|Count|Toplam|Belirli bir dönemdeki ağ geçidi isteklerinin toplam sayısı. Sorunları tanılamanıza yardımcı olması için çeşitli boyutlara göre dilimlenebilir. |Konum, BackendResponseCode, LastErrorReason, GatewayResponseCode|
|Toplam Istek sayısı|Toplam ağ geçidi Isteği sayısı|Count|Toplam|Belirli bir dönemdeki ağ geçidi isteklerinin toplam sayısı. Bu ölçüm kullanım dışı bırakılmıştır, yeni `Requests` ölçümü kullanmanızı öneririz. |Konum, ana bilgisayar adı|
|SuccessfulRequests|Başarılı ağ geçidi Istekleri|Count|Toplam|Belirli bir dönemdeki başarılı ağ geçidi isteklerinin toplam sayısı. Bu ölçüm kullanım dışı bırakılmıştır, yeni `Requests` ölçümü kullanmanızı öneririz.|Konum, ana bilgisayar adı|
|UnauthorizedRequests|Yetkisiz ağ geçidi Istekleri|Count|Toplam| Belirli bir dönemdeki yetkisiz ağ geçidi isteklerinin toplam sayısı. Bu ölçüm kullanım dışı bırakılmıştır, yeni `Requests` ölçümü kullanmanızı öneririz.|Konum, ana bilgisayar adı|
|FailedRequests|Başarısız ağ geçidi Istekleri|Count|Toplam|Belirli bir dönemdeki başarısız ağ geçidi isteklerinin toplam sayısı. Bu ölçüm kullanım dışı bırakılmıştır, yeni `Requests` ölçümü kullanmanızı öneririz.|Konum, ana bilgisayar adı|
|Diğer Istekler|Diğer ağ geçidi Istekleri|Count|Toplam|Belirli bir dönemdeki, başarılı, yetkisiz veya başarısız kategorilere ayrılan ağ geçidi isteklerinin toplam sayısı. Bu ölçüm kullanım dışı bırakılmıştır, yeni `Requests` ölçümü kullanmanızı öneririz. |Konum, ana bilgisayar adı|
|Duration|Ağ Geçidi Isteklerinin genel süresi|Milisaniye|Average|API Management bir istemciden bir istek aldığında ve istemciye yanıt döndürdüğünde geçen süre.|Konum, ana bilgisayar adı|
|Kapasite|Kapasite|Percent|Average|Örneği daha fazla yüklemeye uyum sağlayacak şekilde ölçeklendirip ölçeklendirmeyeceğine dair bilinçli kararlar vermek için bir API Management örneğindeki yükün göstergesi.|Location|
|EventHubTotalEvents|Toplam EventHub olayları|Count|Toplam|Belirli bir dönemdeki API Management EventHub 'e gönderilen toplam olay sayısıdır.|Location|
|Eventhubbaşarılı Futavents|Başarılı EventHub olayları|Count|Toplam|Belirli bir dönemdeki başarılı EventHub olaylarının toplam sayısı.|Location|
|EventHubTotalFailedEvents|Başarısız EventHub olayları|Count|Toplam|Belirli bir dönemdeki başarısız olan EventHub olaylarının toplam sayısı.|Location|
|EventHubRejectedEvents|Reddedilen EventHub olayları|Count|Toplam|Belirli bir dönemde reddedilen EventHub olaylarının toplam sayısı (yanlış yapılandırma veya yetkisiz).|Location|
|EventHubThrottledEvents|Kısıtlanmış EventHub olayları|Count|Toplam|Belirli bir dönemdeki kısıtlanmış EventHub olaylarının toplam sayısı.|Location|
|EventHubTimedoutEvents|Zaman aşımına uğrayan EventHub olayları|Count|Toplam|Belirli bir dönemdeki zaman aşımına uğrayan toplam EventHub olay sayısı.|Location|
|EventHubDroppedEvents|Bırakılan EventHub olayları|Count|Toplam|Belirli bir dönemde sıra boyutu sınırına ulaşıldığından atlanan toplam olay sayısı.|Location|
|EventHubTotalBytesSent|EventHub olaylarının boyutu|Bayt|Toplam|Belirli bir dönemdeki toplam EventHub olay boyutu (bayt).|Location|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft. Automation/automationAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalJob|Toplam Iş sayısı|Count|Toplam|Toplam iş sayısı|Runbook, durum|
|TotalUpdateDeploymentRuns|Toplam güncelleştirme dağıtımı çalıştırmaları|Count|Toplam|Toplam yazılım güncelleştirme dağıtımı çalıştırmaları|SoftwareUpdateConfigurationName, durum|
|Totalupdatedeploymentmachinerçalıştırır|Toplam güncelleştirme dağıtım makinesi çalıştırması|Count|Toplam|Yazılım güncelleştirme dağıtımı çalıştırmasında toplam yazılım güncelleştirme dağıtımı makinesi|SoftwareUpdateConfigurationName, Status, TargetComputer, Softwareupdateconfigurationrunıd|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CoreCount|Adanmış çekirdek sayısı|Count|Toplam|Batch hesabındaki toplam adanmış çekirdek sayısı|Boyut yok|
|TotalNodeCount|Adanmış düğüm sayısı|Count|Toplam|Batch hesabındaki toplam ayrılmış düğüm sayısı|Boyut yok|
|LowPriorityCoreCount|LowPriority çekirdek sayısı|Count|Toplam|Batch hesabındaki toplam düşük öncelikli çekirdek sayısı|Boyut yok|
|TotalLowPriorityNodeCount|Düşük öncelikli düğüm sayısı|Count|Toplam|Batch hesabındaki toplam düşük öncelikli düğüm sayısı|Boyut yok|
|CreatingNodeCount|Düğüm sayısı oluşturuluyor|Count|Toplam|Oluşturulmakta olan düğüm sayısı|Boyut yok|
|StartingNodeCount|Başlangıç düğüm sayısı|Count|Toplam|Başlayan düğüm sayısı|Boyut yok|
|WaitingForStartTaskNodeCount|Başlangıç görevi düğüm sayısı bekleniyor|Count|Toplam|Başlangıç görevinin tamamlanması için bekleyen düğüm sayısı|Boyut yok|
|StartTaskFailedNodeCount|Başlangıç görevi başarısız düğüm sayısı|Count|Toplam|Başlangıç görevinin başarısız olduğu düğüm sayısı|Boyut yok|
|IdleNodeCount|Boştaki düğüm sayısı|Count|Toplam|Boştaki düğüm sayısı|Boyut yok|
|OfflineNodeCount|Çevrimdışı düğüm sayısı|Count|Toplam|Çevrimdışı düğüm sayısı|Boyut yok|
|RebootingNodeCount|Düğüm sayısı yeniden başlatılıyor|Count|Toplam|Yeniden başlatma düğümlerinin sayısı|Boyut yok|
|ReimagingNodeCount|Yeniden Imaging düğüm sayısı|Count|Toplam|Yeniden görüntüleme düğümlerinin sayısı|Boyut yok|
|RunningNodeCount|Çalışan düğüm sayısı|Count|Toplam|Çalışan düğümlerin sayısı|Boyut yok|
|LeavingPoolNodeCount|Havuz düğüm sayısından çıkılıyor|Count|Toplam|Havuzdan ayrılma düğüm sayısı|Boyut yok|
|UnusableNodeCount|Kullanılamayan düğüm sayısı|Count|Toplam|Kullanılamayan düğüm sayısı|Boyut yok|
|PreemptedNodeCount|Önden düğüm sayısı|Count|Toplam|Yok eden düğüm sayısı|Boyut yok|
|TaskStartEvent|Görev başlatma olayları|Count|Toplam|Başlatılan toplam görev sayısı|Boyut yok|
|TaskCompleteEvent|Görev tamamlanma olayları|Count|Toplam|Tamamlanan toplam görev sayısı|Boyut yok|
|TaskFailEvent|Görev başarısız olayları|Count|Toplam|Hatalı durumda tamamlanan toplam görev sayısı|Boyut yok|
|PoolCreateEvent|Havuz oluşturma olayları|Count|Toplam|Oluşturulan Toplam Havuz sayısı|Boyut yok|
|PoolResizeStartEvent|Havuz yeniden boyutlandırma başlangıç olayları|Count|Toplam|Başlatılan toplam havuz yeniden boyutlandırmaları sayısı|Boyut yok|
|PoolResizeCompleteEvent|Havuz yeniden boyutlandırma Tamam olayları|Count|Toplam|Tamamlanan toplam havuz yeniden boyutlandırmaları sayısı|Boyut yok|
|PoolDeleteStartEvent|Havuz silme başlangıç olayları|Count|Toplam|Başlatılmış olan toplam havuz silme sayısı|Boyut yok|
|PoolDeleteCompleteEvent|Havuz silme Tamam olayları|Count|Toplam|Tamamlanan toplam havuz silme sayısı|Boyut yok|
|JobDeleteCompleteEvent|İş silme Tamam olayları|Count|Toplam|Başarıyla silinen işlerin toplam sayısı.|Boyut yok|
|JobDeleteStartEvent|İş silme başlangıç olayları|Count|Toplam|Silinmesi istenen toplam iş sayısı.|Boyut yok|
|JobDisableCompleteEvent|İş tüm olayları devre dışı bırak|Count|Toplam|Başarıyla devre dışı bırakılmış toplam iş sayısı.|Boyut yok|
|JobDisableStartEvent|İş devre dışı başlatma olayları|Count|Toplam|Devre dışı bırakılması istenen toplam iş sayısı.|Boyut yok|
|JobStartEvent|İş başlatma olayları|Count|Toplam|Başarıyla başlatılmış toplam iş sayısı.|Boyut yok|
|JobTerminateCompleteEvent|İş sonlandırma tamamlanma olayları|Count|Toplam|Başarıyla sonlandırılan toplam iş sayısı.|Boyut yok|
|JobTerminateStartEvent|İş sonlandırma başlangıç olayları|Count|Toplam|Sonlandırılması istenen toplam iş sayısı.|Boyut yok|

## <a name="microsoftcacheredis"></a>Microsoft. Cache/redsıs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|connectedistemcileri|Bağlı İstemciler|Count|Maksimum||Parça|
|totalcommandsişlendi|İşlemler Toplamı|Count|Toplam||Parça|
|cachehits|İsabetli Önbellek Okuma Sayısı|Count|Toplam||Parça|
|cacheisabetsizlik|İsabetsiz Önbellek Okuma Sayısı|Count|Toplam||Parça|
|GetCommands|Alınanlar|Count|Toplam||Parça|
|SetCommands|Kümeler|Count|Toplam||Parça|
|operationsPerSecond|İşlem/saniye|Count|Maksimum||Parça|
|çıkarılan anahtarlar|Çıkarılan Anahtarlar|Count|Toplam||Parça|
|totalkeys|Toplam Anahtar Sayısı|Count|Maksimum||Parça|
|expiredkeys|Süresi Dolan Anahtarlar|Count|Toplam||Parça|
|usedmemory|Kullanılan Bellek|Bayt|Maksimum||Parça|
|usedmemorypercentage|Kullanılan bellek yüzdesi|Percent|Maksimum||Parça|
|usedmemoryRss|Kullanılan bellek RSS|Bayt|Maksimum||Parça|
|Sunucuyükü|Sunucu Yükü|Percent|Maksimum||Parça|
|cacheWrite|Önbellek Yazması|BytesPerSecond|Maksimum||Parça|
|cacheRead|Önbellek Okuması|BytesPerSecond|Maksimum||Parça|
|percentProcessorTime|CPU|Percent|Maksimum||Parça|
|cacheLatency|Önbellek gecikmesi mikrosaniye (Önizleme)|Count|Average||Shardıd, SampleType|
|hatalar|Hatalar|Count|Maksimum||Shardıd, ErrorType|
|connectedclients0|Bağlı Istemciler (parça 0)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed0|Toplam Işlem (parça 0)|Count|Toplam||Boyut yok|
|cachehits0|Önbellek ısabetleri (parça 0)|Count|Toplam||Boyut yok|
|cachemisses0|Önbellekte bulunamayanlar (parça 0)|Count|Toplam||Boyut yok|
|getcommands0|Alır (parça 0)|Count|Toplam||Boyut yok|
|setcommands0|Kümeler (parça 0)|Count|Toplam||Boyut yok|
|operationsPerSecond0|Saniyedeki işlem (parça 0)|Count|Maksimum||Boyut yok|
|evictedkeys0|Çıkarılan anahtarlar (parça 0)|Count|Toplam||Boyut yok|
|totalkeys0|Toplam anahtar (parça 0)|Count|Maksimum||Boyut yok|
|expiredkeys0|Süre dolma anahtarları (parça 0)|Count|Toplam||Boyut yok|
|usedmemory0|Kullanılan bellek (parça 0)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss0|Kullanılan bellek RSS (parça 0)|Bayt|Maksimum||Boyut yok|
|serverLoad0|Sunucu yükü (parça 0)|Percent|Maksimum||Boyut yok|
|cacheWrite0|Önbellek yazma (parça 0)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead0|Önbellek Okuma (parça 0)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime0|CPU (parça 0)|Percent|Maksimum||Boyut yok|
|connectedclients1|Bağlı Istemciler (parça 1)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed1|Toplam Işlem (parça 1)|Count|Toplam||Boyut yok|
|cachehits1|Önbellek ısabetleri (parça 1)|Count|Toplam||Boyut yok|
|cachemisses1|Önbellekte bulunamayanlar (parça 1)|Count|Toplam||Boyut yok|
|getcommands1|Alır (parça 1)|Count|Toplam||Boyut yok|
|setcommands1|Kümeler (parça 1)|Count|Toplam||Boyut yok|
|operationsPerSecond1|Saniyedeki işlem (parça 1)|Count|Maksimum||Boyut yok|
|evictedkeys1|Çıkarılan anahtarlar (parça 1)|Count|Toplam||Boyut yok|
|totalkeys1|Toplam anahtar (parça 1)|Count|Maksimum||Boyut yok|
|expiredkeys1|Süre dolma anahtarları (parça 1)|Count|Toplam||Boyut yok|
|usedmemory1|Kullanılan bellek (parça 1)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss1|Kullanılan bellek RSS (parça 1)|Bayt|Maksimum||Boyut yok|
|serverLoad1|Sunucu yükü (parça 1)|Percent|Maksimum||Boyut yok|
|cacheWrite1|Önbellek yazma (parça 1)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead1|Önbellek Okuma (parça 1)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime1|CPU (parça 1)|Percent|Maksimum||Boyut yok|
|connectedclients2|Bağlı Istemciler (parça 2)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed2|Toplam Işlem (parça 2)|Count|Toplam||Boyut yok|
|cachehits2|Önbellek ısabetleri (parça 2)|Count|Toplam||Boyut yok|
|cachemisses2|Önbellekte bulunamayanlar (parça 2)|Count|Toplam||Boyut yok|
|getcommands2|Alır (parça 2)|Count|Toplam||Boyut yok|
|setcommands2|Kümeler (parça 2)|Count|Toplam||Boyut yok|
|operationsPerSecond2|Saniyedeki işlem (parça 2)|Count|Maksimum||Boyut yok|
|evictedkeys2|Çıkarılan anahtarlar (parça 2)|Count|Toplam||Boyut yok|
|totalkeys2|Toplam anahtar (parça 2)|Count|Maksimum||Boyut yok|
|expiredkeys2|Süre dolma anahtarları (parça 2)|Count|Toplam||Boyut yok|
|usedmemory2|Kullanılan bellek (parça 2)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss2|Kullanılan bellek RSS (parça 2)|Bayt|Maksimum||Boyut yok|
|serverLoad2|Sunucu yükü (parça 2)|Percent|Maksimum||Boyut yok|
|cacheWrite2|Önbellek yazma (parça 2)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead2|Önbellek Okuma (parça 2)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime2|CPU (parça 2)|Percent|Maksimum||Boyut yok|
|connectedclients3|Bağlı Istemciler (parça 3)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed3|Toplam Işlem (parça 3)|Count|Toplam||Boyut yok|
|cachehits3|Önbellek ısabetleri (parça 3)|Count|Toplam||Boyut yok|
|cachemisses3|Önbellekte bulunamayanlar (parça 3)|Count|Toplam||Boyut yok|
|getcommands3|Alır (parça 3)|Count|Toplam||Boyut yok|
|setcommands3|Kümeler (parça 3)|Count|Toplam||Boyut yok|
|operationsPerSecond3|Saniyedeki işlem (parça 3)|Count|Maksimum||Boyut yok|
|evictedkeys3|Çıkarılan anahtarlar (parça 3)|Count|Toplam||Boyut yok|
|totalkeys3|Toplam anahtar (parça 3)|Count|Maksimum||Boyut yok|
|expiredkeys3|Süre dolma anahtarları (parça 3)|Count|Toplam||Boyut yok|
|usedmemory3|Kullanılan bellek (parça 3)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss3|Kullanılan bellek RSS (parça 3)|Bayt|Maksimum||Boyut yok|
|serverLoad3|Sunucu yükü (parça 3)|Percent|Maksimum||Boyut yok|
|cacheWrite3|Önbellek yazma (parça 3)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead3|Önbellek Okuma (parça 3)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime3|CPU (parça 3)|Percent|Maksimum||Boyut yok|
|connectedclients4|Bağlı Istemciler (parça 4)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed4|Toplam Işlem (parça 4)|Count|Toplam||Boyut yok|
|cachehits4|Önbellek ısabetleri (parça 4)|Count|Toplam||Boyut yok|
|cachemisses4|Önbellekte bulunamayanlar (parça 4)|Count|Toplam||Boyut yok|
|getcommands4|Alır (parça 4)|Count|Toplam||Boyut yok|
|setcommands4|Kümeler (parça 4)|Count|Toplam||Boyut yok|
|operationsPerSecond4|Saniyedeki işlem (parça 4)|Count|Maksimum||Boyut yok|
|evictedkeys4|Çıkarılan anahtarlar (parça 4)|Count|Toplam||Boyut yok|
|totalkeys4|Toplam anahtar (parça 4)|Count|Maksimum||Boyut yok|
|expiredkeys4|Süre dolma anahtarları (parça 4)|Count|Toplam||Boyut yok|
|usedmemory4|Kullanılan bellek (parça 4)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss4|Kullanılan bellek RSS (parça 4)|Bayt|Maksimum||Boyut yok|
|serverLoad4|Sunucu yükü (parça 4)|Percent|Maksimum||Boyut yok|
|cacheWrite4|Önbellek yazma (parça 4)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead4|Önbellek Okuma (parça 4)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime4|CPU (parça 4)|Percent|Maksimum||Boyut yok|
|connectedclients5|Bağlı Istemciler (parça 5)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed5|Toplam Işlem (parça 5)|Count|Toplam||Boyut yok|
|cachehits5|Önbellek ısabetleri (parça 5)|Count|Toplam||Boyut yok|
|cachemisses5|Önbellekte bulunamayanlar (parça 5)|Count|Toplam||Boyut yok|
|getcommands5|Alır (parça 5)|Count|Toplam||Boyut yok|
|setcommands5|Kümeler (parça 5)|Count|Toplam||Boyut yok|
|operationsPerSecond5|Saniyedeki işlem (parça 5)|Count|Maksimum||Boyut yok|
|evictedkeys5|Çıkarılan anahtarlar (parça 5)|Count|Toplam||Boyut yok|
|totalkeys5|Toplam anahtar (parça 5)|Count|Maksimum||Boyut yok|
|expiredkeys5|Süre dolma anahtarları (parça 5)|Count|Toplam||Boyut yok|
|usedmemory5|Kullanılan bellek (parça 5)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss5|Kullanılan bellek RSS (parça 5)|Bayt|Maksimum||Boyut yok|
|serverLoad5|Sunucu yükü (parça 5)|Percent|Maksimum||Boyut yok|
|cacheWrite5|Önbellek yazma (parça 5)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead5|Önbellek Okuma (parça 5)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime5|CPU (parça 5)|Percent|Maksimum||Boyut yok|
|connectedclients6|Bağlı Istemciler (parça 6)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed6|Toplam Işlem (parça 6)|Count|Toplam||Boyut yok|
|cachehits6|Önbellek ısabetleri (parça 6)|Count|Toplam||Boyut yok|
|cachemisses6|Önbellekte bulunamayanlar (parça 6)|Count|Toplam||Boyut yok|
|getcommands6|Alır (parça 6)|Count|Toplam||Boyut yok|
|setcommands6|Kümeler (parça 6)|Count|Toplam||Boyut yok|
|operationsPerSecond6|Saniyedeki işlem (parça 6)|Count|Maksimum||Boyut yok|
|evictedkeys6|Çıkarılan anahtarlar (parça 6)|Count|Toplam||Boyut yok|
|totalkeys6|Toplam anahtar (parça 6)|Count|Maksimum||Boyut yok|
|expiredkeys6|Süre dolma anahtarları (parça 6)|Count|Toplam||Boyut yok|
|usedmemory6|Kullanılan bellek (parça 6)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss6|Kullanılan bellek RSS (parça 6)|Bayt|Maksimum||Boyut yok|
|serverLoad6|Sunucu yükü (parça 6)|Percent|Maksimum||Boyut yok|
|cacheWrite6|Önbellek yazma (parça 6)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead6|Önbellek Okuma (parça 6)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime6|CPU (parça 6)|Percent|Maksimum||Boyut yok|
|connectedclients7|Bağlı Istemciler (parça 7)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed7|Toplam Işlem (parça 7)|Count|Toplam||Boyut yok|
|cachehits7|Önbellek ısabetleri (parça 7)|Count|Toplam||Boyut yok|
|cachemisses7|Önbellekte bulunamayanlar (parça 7)|Count|Toplam||Boyut yok|
|getcommands7|Alır (parça 7)|Count|Toplam||Boyut yok|
|setcommands7|Kümeler (parça 7)|Count|Toplam||Boyut yok|
|operationsPerSecond7|Saniyedeki işlem (parça 7)|Count|Maksimum||Boyut yok|
|evictedkeys7|Çıkarılan anahtarlar (parça 7)|Count|Toplam||Boyut yok|
|totalkeys7|Toplam anahtar (parça 7)|Count|Maksimum||Boyut yok|
|expiredkeys7|Süre dolma anahtarları (parça 7)|Count|Toplam||Boyut yok|
|usedmemory7|Kullanılan bellek (parça 7)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss7|Kullanılan bellek RSS (parça 7)|Bayt|Maksimum||Boyut yok|
|serverLoad7|Sunucu yükü (parça 7)|Percent|Maksimum||Boyut yok|
|cacheWrite7|Önbellek yazma (parça 7)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead7|Önbellek Okuma (parça 7)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime7|CPU (parça 7)|Percent|Maksimum||Boyut yok|
|connectedclients8|Bağlı Istemciler (parça 8)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed8|Toplam Işlem (parça 8)|Count|Toplam||Boyut yok|
|cachehits8|Önbellek ısabetleri (parça 8)|Count|Toplam||Boyut yok|
|cachemisses8|Önbellekte bulunamayanlar (parça 8)|Count|Toplam||Boyut yok|
|getcommands8|Al (Shard 8)|Count|Toplam||Boyut yok|
|setcommands8|Kümeler (Shard 8)|Count|Toplam||Boyut yok|
|operationsPerSecond8|Saniyedeki işlem (parça 8)|Count|Maksimum||Boyut yok|
|evictedkeys8|Çıkarılan anahtarlar (parça 8)|Count|Toplam||Boyut yok|
|totalkeys8|Toplam anahtar (Shard 8)|Count|Maksimum||Boyut yok|
|expiredkeys8|Süre dolma anahtarları (parça 8)|Count|Toplam||Boyut yok|
|usedmemory8|Kullanılan bellek (parça 8)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss8|Kullanılan bellek RSS (parça 8)|Bayt|Maksimum||Boyut yok|
|serverLoad8|Sunucu yükü (parça 8)|Percent|Maksimum||Boyut yok|
|cacheWrite8|Önbellek yazma (parça 8)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead8|Önbellek Okuma (parça 8)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime8|CPU (parça 8)|Percent|Maksimum||Boyut yok|
|connectedclients9|Bağlı Istemciler (parça 9)|Count|Maksimum||Boyut yok|
|totalcommandsprocessed9|Toplam Işlem (parça 9)|Count|Toplam||Boyut yok|
|cachehits9|Önbellek ısabetleri (parça 9)|Count|Toplam||Boyut yok|
|cachemisses9|Önbellekte bulunamayanlar (parça 9)|Count|Toplam||Boyut yok|
|getcommands9|Alır (parça 9)|Count|Toplam||Boyut yok|
|setcommands9|Kümeler (parça 9)|Count|Toplam||Boyut yok|
|operationsPerSecond9|Saniyedeki işlem (parça 9)|Count|Maksimum||Boyut yok|
|evictedkeys9|Çıkarılan anahtarlar (parça 9)|Count|Toplam||Boyut yok|
|totalkeys9|Toplam anahtar (parça 9)|Count|Maksimum||Boyut yok|
|expiredkeys9|Süre dolma anahtarları (parça 9)|Count|Toplam||Boyut yok|
|usedmemory9|Kullanılan bellek (parça 9)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss9|Kullanılan bellek RSS (parça 9)|Bayt|Maksimum||Boyut yok|
|serverLoad9|Sunucu yükü (parça 9)|Percent|Maksimum||Boyut yok|
|cacheWrite9|Önbellek yazma (parça 9)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead9|Önbellek Okuma (parça 9)|BytesPerSecond|Maksimum||Boyut yok|
|percentProcessorTime9|CPU (parça 9)|Percent|Maksimum||Boyut yok|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft. ClassicCompute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Average|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi.|Boyut yok|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik).|Boyut yok|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik).|Boyut yok|
|Disk okuma bayt/sn|Disk Okuma|BytesPerSecond|Average|İzleme dönemi boyunca diskten okunan ortalama bayt.|Boyut yok|
|Disk yazma bayt/sn|Disk Yazma|BytesPerSecond|Average|İzleme dönemi boyunca diske yazılan ortalama bayt.|Boyut yok|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|CountPerSecond|Average|Disk okuma ıOPS 'si.|Boyut yok|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|CountPerSecond|Average|Disk yazma ıOPS 'si.|Boyut yok|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft. ClassicCompute/domainNames/yuvalar/roller

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Average|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi.|RoleInstanceId|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik).|RoleInstanceId|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik).|RoleInstanceId|
|Disk okuma bayt/sn|Disk Okuma|BytesPerSecond|Average|İzleme dönemi boyunca diskten okunan ortalama bayt.|RoleInstanceId|
|Disk yazma bayt/sn|Disk Yazma|BytesPerSecond|Average|İzleme dönemi boyunca diske yazılan ortalama bayt.|RoleInstanceId|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|CountPerSecond|Average|Disk okuma ıOPS 'si.|RoleInstanceId|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|CountPerSecond|Average|Disk yazma ıOPS 'si.|RoleInstanceId|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Toplam çağrılar|Toplam Çağrı Sayısı|Count|Toplam|Toplam çağrı sayısı.|ApiName, OperationName, bölge|
|Başarılı çağrılar|Başarılı Çağrılar|Count|Toplam|Başarılı çağrı sayısı.|ApiName, OperationName, bölge|
|Toplam hata sayısı|Hatalar Toplamı|Count|Toplam|Hata yanıtıyla Toplam çağrı sayısı (HTTP yanıt kodu 4xx veya 5xx).|ApiName, OperationName, bölge|
|Blockedçağrılarında|Engellenen Çağrılar|Count|Toplam|Oran veya kota sınırını aşan çağrı sayısı.|ApiName, OperationName, bölge|
|ServerErrors|Sunucu Hataları|Count|Toplam|Hizmet iç hatası olan çağrı sayısı (HTTP yanıt kodu 5xx).|ApiName, OperationName, bölge|
|ClientErrors|İstemci Hataları|Count|Toplam|İstemci tarafı hatası olan çağrı sayısı (HTTP yanıt kodu 4xx).|ApiName, OperationName, bölge|
|Dataın|Data Girişi|Bayt|Toplam|Gelen verilerin bayt cinsinden boyutu.|ApiName, OperationName, bölge|
|Veri çıkışı|Giden Veriler|Bayt|Toplam|Giden verilerin bayt cinsinden boyutu.|ApiName, OperationName, bölge|
|Gecikme süresi|Gecikme süresi|Mayacak|Average|Milisaniye cinsinden gecikme süresi.|ApiName, OperationName, bölge|
|Karakter Yabanslamuş|Çevrilen Karakterler|Count|Toplam|Gelen metin isteğindeki toplam karakter sayısı.|ApiName, OperationName, bölge|
|Karakterçeli|Eğitilen karakterler|Count|Toplam|Eğitilen toplam karakter sayısı.|ApiName, OperationName, bölge|
|SpeechSessionDuration|Konuşma Oturumu Süresi|Saniye|Toplam|Konuşma oturumunun saniye cinsinden toplam süresi.|ApiName, OperationName, bölge|
|TotalTransactions|Toplam İşlemler|Count|Toplam|Toplam işlem sayısı.|Boyut yok|
|TotalTokenCalls|Toplam Belirteç Çağrısı|Count|Toplam|Toplam belirteç çağrısı sayısı.|ApiName, OperationName, bölge|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Boyut yok|
|Ağ Girişi|Alınan Faturalanabilir Ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan faturalandırılabilir bayt sayısı (gelen trafik)|Boyut yok|
|Ağ Çıkışı|Gönderilen Faturalanabilir Ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı (giden trafik)|Boyut yok|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt sayısı|Boyut yok|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt sayısı|Boyut yok|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|CountPerSecond|Average|Disk Okuma IOPS|Boyut yok|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|CountPerSecond|Average|Disk Yazma IOPS|Boyut yok|
|Kalan CPU Kredisi|Kalan CPU Kredisi|Count|Average|Patlama için kullanılabilen toplam kredi sayısı|Boyut yok|
|Tüketilen CPU Kredisi|Tüketilen CPU Kredisi|Count|Average|Sanal makine tarafından tüketilen toplam kredi sayısı|Boyut yok|
|Disk başına okunan bayt/sn|Veri diski okuma bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diskten okunan bayt/sn|SlotId|
|Disk başına yazma bayt/sn|Veri diski yazma bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diske yazılan bayt/sn|SlotId|
|Disk başına okuma Işlemi/sn|Veri diski okuma Işlemi/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diskten ıOPS 'yi okuma|SlotId|
|Disk başına yazma Işlemi/sn|Veri diski yazma Işlemi/sn (kullanım dışı)|CountPerSecond|Average|İzleme dönemi boyunca tek bir diskten ıOPS yazma|SlotId|
|Disk Başına QD|Veri diski QD (kullanım dışı)|Count|Average|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotId|
|İşletim sistemi disk başına okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Boyut yok|
|İşletim sistemi disk başına yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski başına okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Boyut yok|
|İşletim sistemi diski başına yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Boyut yok|
|İşletim Sistemi Disk Başına QD|İşletim sistemi diski QD (kullanım dışı)|Count|Average|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|Veri diski okuma bayt/sn|Veri diski okuma bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten okunan bayt/sn|LUN|
|Veri diski yazma bayt/sn|Veri diski yazma bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diske yazılan bayt/sn|LUN|
|Veri diski okuma Işlemi/sn|Veri diski okuma Işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten ıOPS 'yi okuma|LUN|
|Veri diski yazma Işlemi/sn|Veri diski yazma Işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme dönemi boyunca tek bir diskten ıOPS yazma|LUN|
|Veri Diski Kuyruk Derinliği|Veri diski sıra derinliği (Önizleme)|Count|Average|Veri diski sıra derinliği (veya sıra uzunluğu)|LUN|
|İşletim sistemi diski okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Boyut yok|
|İşletim sistemi diski yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Boyut yok|
|İşletim sistemi diski yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Boyut yok|
|İşletim Sistemi Diski Kuyruk Derinliği|İşletim sistemi diski sıra derinliği (Önizleme)|Count|Average|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|Gelen Akışlar|Gelen akışlar (Önizleme)|Count|Average|Gelen akışlar, gelen yöndeki geçerli akışların sayısıdır (VM 'ye giden trafik)|Boyut yok|
|Giden Akışlar|Giden akışlar (Önizleme)|Count|Average|Giden akışlar giden yöndeki geçerli akış sayısıdır (VM 'den giden trafik)|Boyut yok|
|En Yüksek Gelen Akış Oluşturma Oranı|Gelen akışlar en yüksek oluşturma oranı (Önizleme)|CountPerSecond|Average|Gelen akışların en yüksek oluşturma oranı (VM 'ye giden trafik)|Boyut yok|
|En Yüksek Giden Akış Oluşturma Oranı|Giden akışlar maksimum oluşturma oranı (Önizleme)|CountPerSecond|Average|Giden akışların en yüksek oluşturma oranı (VM 'den giden trafik)|Boyut yok|
|Premium veri diski önbelleği okuma Isabeti|Premium veri diski önbelleği okuma Isabeti (Önizleme)|Percent|Average|Premium veri diski önbelleği okuma Isabeti|LUN|
|Premium veri diski önbelleği okuma Isabetsizliği|Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)|Percent|Average|Premium veri diski önbelleği okuma Isabetsizliği|LUN|
|Premium işletim sistemi disk önbelleği okuma Isabeti|Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)|Percent|Average|Premium işletim sistemi disk önbelleği okuma Isabeti|Boyut yok|
|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)|Percent|Average|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Boyut yok|
|Toplam Alınan Ağ|Toplam Alınan Ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik)|Boyut yok|
|Toplam Gönderilen Ağ|Toplam Gönderilen Ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik)|Boyut yok|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|VMName|
|Ağ Girişi|Alınan Faturalanabilir Ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan faturalandırılabilir bayt sayısı (gelen trafik)|VMName|
|Ağ Çıkışı|Gönderilen Faturalanabilir Ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı (giden trafik)|VMName|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt sayısı|VMName|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt sayısı|VMName|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|CountPerSecond|Average|Disk Okuma IOPS|VMName|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|CountPerSecond|Average|Disk Yazma IOPS|VMName|
|Kalan CPU Kredisi|Kalan CPU Kredisi|Count|Average|Patlama için kullanılabilen toplam kredi sayısı|Boyut yok|
|Tüketilen CPU Kredisi|Tüketilen CPU Kredisi|Count|Average|Sanal makine tarafından tüketilen toplam kredi sayısı|Boyut yok|
|Disk başına okunan bayt/sn|Veri diski okuma bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diskten okunan bayt/sn|SlotId|
|Disk başına yazma bayt/sn|Veri diski yazma bayt/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diske yazılan bayt/sn|SlotId|
|Disk başına okuma Işlemi/sn|Veri diski okuma Işlemi/sn (kullanım dışı)|CountPerSecond|Average|İzleme döneminde tek bir diskten ıOPS 'yi okuma|SlotId|
|Disk başına yazma Işlemi/sn|Veri diski yazma Işlemi/sn (kullanım dışı)|CountPerSecond|Average|İzleme dönemi boyunca tek bir diskten ıOPS yazma|SlotId|
|Disk Başına QD|Veri diski QD (kullanım dışı)|Count|Average|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotId|
|İşletim sistemi disk başına okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Boyut yok|
|İşletim sistemi disk başına yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski başına okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Boyut yok|
|İşletim sistemi diski başına yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Boyut yok|
|İşletim Sistemi Disk Başına QD|İşletim sistemi diski QD (kullanım dışı)|Count|Average|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|Veri diski okuma bayt/sn|Veri diski okuma bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten okunan bayt/sn|LUN, VMName|
|Veri diski yazma bayt/sn|Veri diski yazma bayt/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diske yazılan bayt/sn|LUN, VMName|
|Veri diski okuma Işlemi/sn|Veri diski okuma Işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme döneminde tek bir diskten ıOPS 'yi okuma|LUN, VMName|
|Veri diski yazma Işlemi/sn|Veri diski yazma Işlemi/sn (Önizleme)|CountPerSecond|Average|İzleme dönemi boyunca tek bir diskten ıOPS yazma|LUN, VMName|
|Veri Diski Kuyruk Derinliği|Veri diski sıra derinliği (Önizleme)|Count|Average|Veri diski sıra derinliği (veya sıra uzunluğu)|LUN, VMName|
|İşletim sistemi diski okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|VMName|
|İşletim sistemi diski yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|VMName|
|İşletim sistemi diski okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|VMName|
|İşletim sistemi diski yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (Önizleme)|CountPerSecond|Average|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|VMName|
|İşletim Sistemi Diski Kuyruk Derinliği|İşletim sistemi diski sıra derinliği (Önizleme)|Count|Average|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|VMName|
|Gelen Akışlar|Gelen akışlar (Önizleme)|Count|Average|Gelen akışlar, gelen yöndeki geçerli akışların sayısıdır (VM 'ye giden trafik)|VMName|
|Giden Akışlar|Giden akışlar (Önizleme)|Count|Average|Giden akışlar giden yöndeki geçerli akış sayısıdır (VM 'den giden trafik)|VMName|
|En Yüksek Gelen Akış Oluşturma Oranı|Gelen akışlar en yüksek oluşturma oranı (Önizleme)|CountPerSecond|Average|Gelen akışların en yüksek oluşturma oranı (VM 'ye giden trafik)|VMName|
|En Yüksek Giden Akış Oluşturma Oranı|Giden akışlar maksimum oluşturma oranı (Önizleme)|CountPerSecond|Average|Giden akışların en yüksek oluşturma oranı (VM 'den giden trafik)|VMName|
|Premium veri diski önbelleği okuma Isabeti|Premium veri diski önbelleği okuma Isabeti (Önizleme)|Percent|Average|Premium veri diski önbelleği okuma Isabeti|LUN, VMName|
|Premium veri diski önbelleği okuma Isabetsizliği|Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)|Percent|Average|Premium veri diski önbelleği okuma Isabetsizliği|LUN, VMName|
|Premium işletim sistemi disk önbelleği okuma Isabeti|Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)|Percent|Average|Premium işletim sistemi disk önbelleği okuma Isabeti|VMName|
|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)|Percent|Average|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|VMName|
|Toplam Alınan Ağ|Toplam Alınan Ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik)|VMName|
|Toplam Gönderilen Ağ|Toplam Gönderilen Ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik)|VMName|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft. Containerınstance/containerGroups

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuUsage|CPU Kullanımı|Count|Average|Miliçekirdekte tüm çekirdekler üzerinde CPU kullanımı.|Adı|
|MemoryUsage|Bellek Kullanımı|Bayt|Average|Bayt cinsinden toplam bellek kullanımı.|Adı|
|NetworkBytesReceivedPerSecond|Saniyede Alınan Ağ Bant Sayısı|Bayt|Average|Saniye başına alınan ağ bayt sayısı.|Boyut yok|
|NetworkBytesTransmittedPerSecond|Saniyede Aktarılan Ağ Bant Sayısı|Bayt|Average|Saniye başına aktarılan ağ baytları.|Boyut yok|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft. ContainerRegistry/kayıt defterleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalPullCount|Toplam çekme sayısı|Count|Average|Toplam görüntü sayısı|Boyut yok|
|Başarılı sayı|Başarılı çekme sayısı|Count|Average|Başarılı görüntü sayısı|Boyut yok|
|Toplam Pushcount|Toplam gönderme sayısı|Count|Average|Toplam resim gönderim sayısı|Boyut yok|
|Başarılı Pushcount|Başarılı gönderme sayısı|Count|Average|Başarılı görüntü gönderme sayısı|Boyut yok|
|RunDuration|Çalışma süresi|Milisaniye|Toplam|Çalışma süresi (milisaniye)|Boyut yok|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft. ContainerService/Managedkümeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Yönetilen kümede toplam kullanılabilir CPU çekirdeği sayısı|Count|Toplam|Yönetilen kümede toplam kullanılabilir CPU çekirdeği sayısı|Boyut yok|
|kube_node_status_allocatable_memory_bytes|Yönetilen kümede toplam kullanılabilir bellek miktarı|Bayt|Toplam|Yönetilen kümede toplam kullanılabilir bellek miktarı|Boyut yok|
|kube_pod_status_ready|Hazırlık durumundaki Pod sayısı|Count|Toplam|Hazırlık durumundaki Pod sayısı|ad alanı, Pod|
|kube_node_status_condition|Çeşitli düğüm koşullarına yönelik durumlar|Count|Toplam|Çeşitli düğüm koşullarına yönelik durumlar|koşul, durum, status2, düğüm|
|kube_pod_status_phase|Aşamasına göre Pod sayısı|Count|Toplam|Aşamasına göre Pod sayısı|aşama, ad alanı, Pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft. Customerınsights/hub 'ları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Dcıapıalls|Customer Insights API çağrıları|Count|Toplam||Boyut yok|
|Dcimappıngimportoperationbaşarılı satırlar|Içeri aktarma Işlemi başarılı satırları eşleme|Count|Toplam||Boyut yok|
|Dcımappıngimportoperationfailedlines|Eşleme Içeri aktarma Işlemi başarısız satırlar|Count|Toplam||Boyut yok|
|Dcımappıngimportoperationtotallines|Eşleme Içeri aktarma Işlemi toplam satırlar|Count|Toplam||Boyut yok|
|Dcımappıngimportoperationruntimeınseconds|Içeri aktarma Işlemi çalışma zamanını saniyeler Içinde eşleme|Saniye|Toplam||Boyut yok|
|DCIOutboundProfileExportSucceeded|Giden profili dışarı aktarma başarılı|Count|Toplam||Boyut yok|
|DCIOutboundProfileExportFailed|Giden profili dışarı aktarma başarısız|Count|Toplam||Boyut yok|
|DCIOutboundProfileExportDuration|Giden profili dışarı aktarma süresi|Saniye|Toplam||Boyut yok|
|Dcıoutboundkpiexportsucceeded|Giden KPI dışarı aktarma başarılı|Count|Toplam||Boyut yok|
|Dcıoutboundkpiexportfailed|Giden KPI dışarı aktarılamadı|Count|Toplam||Boyut yok|
|Dcıoutboundkpiexportduration|Giden KPI dışarı aktarma süresi|Saniye|Toplam||Boyut yok|
|Dcıoutboundkpiexportstarted|Giden KPI dışarı aktarma başlatıldı|Saniye|Toplam||Boyut yok|
|DCIOutboundKpiRecordCount|Giden KPI kayıt sayısı|Saniye|Toplam||Boyut yok|
|DCIOutboundProfileExportCount|Giden profili dışarı aktarma sayısı|Saniye|Toplam||Boyut yok|
|DCIOutboundInitialProfileExportFailed|Giden Ilk profili dışarı aktarma başarısız|Saniye|Toplam||Boyut yok|
|DCIOutboundInitialProfileExportSucceeded|Giden Ilk profili dışarı aktarma başarılı|Saniye|Toplam||Boyut yok|
|Dcıoutbounınvoitialkpiexportbaşarısız oldu|Giden başlangıç KPI 'si dışarı aktarılamadı|Saniye|Toplam||Boyut yok|
|Dcıoutboundınitialkpiexportsucceeded|Giden Ilk KPI dışarı aktarma başarılı|Saniye|Toplam||Boyut yok|
|DCIOutboundInitialProfileExportDurationInSeconds|Giden Ilk profil dışa aktarma süresi (saniye)|Saniye|Toplam||Boyut yok|
|AdlaJobForStandardKpiFailed|Standart KPI Için adla Işi saniye cinsinden başarısız oldu|Saniye|Toplam||Boyut yok|
|AdlaJobForStandardKpiTimeOut|Standart KPI zaman aşımı Için saniye cinsinden adla Işi|Saniye|Toplam||Boyut yok|
|AdlaJobForStandardKpiCompleted|Standart KPI Için adla Işi saniye cinsinden tamamlandı|Saniye|Toplam||Boyut yok|
|Importasavaluesfailed|ASA değerlerini içeri aktarma başarısız sayısı|Count|Toplam||Boyut yok|
|Importasavaluessucceeded|ASA değerlerini içeri aktarma başarılı sayısı|Count|Toplam||Boyut yok|
|Dcıprofilscount|Profil örnek sayısı|Count|Son||Boyut yok|
|Dciınteractionspermonthcount|Aylık etkileşimler sayısı|Count|Son||Boyut yok|
|DCIKpisCount|KPI sayısı|Count|Son||Boyut yok|
|DCISegmentsCount|Segment Sayısı|Count|Son||Boyut yok|
|Dcıpredictivematchpoliciescount|Tahmine dayalı eşleşme sayısı|Count|Son||Boyut yok|
|DCIPredictionsCount|Tahmin sayısı|Count|Son||Boyut yok|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft. DataBoxEdge/dataBoxEdgeDevices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Nicreadüretilen Iş|Okuma performansı (ağ)|BytesPerSecond|Average|Cihazdaki ağ arabiriminin, ağ geçidinde bulunan tüm birimler için raporlama döneminde okuma performansı.|InstanceName|
|Nicwriteüretilen Iş|Yazma Işleme (ağ)|BytesPerSecond|Average|Cihazdaki ağ arabiriminin, ağ geçidinde bulunan tüm birimler için raporlama döneminde yazma aktarım hızı.|InstanceName|
|CloudReadThroughputPerShare|Bulut Indirme verimlilik (paylaşma)|BytesPerSecond|Average|Raporlama dönemi boyunca bir paylaşımdan Azure 'a aktarım hızını indirin.|Paylaş|
|CloudUploadThroughputPerShare|Bulut karşıya yükleme üretilen Işi (paylaşma)|BytesPerSecond|Average|Raporlama dönemi boyunca bir paylaşımdan Azure 'a yük aktarımını yükleme.|Paylaş|
|BytesUploadedToCloudPerShare|Karşıya yüklenen bulut baytları (paylaşma)|Bayt|Average|Raporlama dönemi boyunca bir paylaşımdan Azure 'a yüklenen toplam bayt sayısı.|Paylaş|
|TotalCapacity|Toplam Kapasite|Bayt|Average|Toplam Kapasite|Boyut yok|
|AvailableCapacity|Kullanılabilir Kapasite|Bayt|Average|Raporlama dönemi boyunca kullanılabilir kapasite (bayt cinsinden).|Boyut yok|
|Clouduploadüretilen Iş|Bulut Karşıya Yükleme Aktarım Hızı|BytesPerSecond|Average|Raporlama dönemi boyunca Azure 'a bulut yükleme performansı.|Boyut yok|
|Cloudreadüretilen Iş|Bulut Indirme performansı|BytesPerSecond|Average|Raporlama döneminde Azure 'a bulut indirme performansı.|Boyut yok|
|BytesUploadedToCloud|Karşıya yüklenen bulut baytları (cihaz)|Bayt|Average|Raporlama dönemi boyunca bir cihazdan Azure 'a yüklenen toplam bayt sayısı.|Boyut yok|
|Hypervvirtualprocessorkullanım|Edge hesaplama-CPU yüzdesi|Percent|Average|CPU Kullanımı Yüzdesi|InstanceName|
|Hipervmemoryutilileştirme|Edge Işlem-bellek kullanımı|Percent|Average|Kullanımdaki RAM miktarı|InstanceName|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft. DataFactory/DataFactory

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Failedçalıştırmaları|Başarısız Çalışma Sayısı|Count|Toplam||Ardışık düzen ınename, activityName|
|Başarılı çalıştırmalar|Başarılı Çalışma Sayısı|Count|Toplam||Ardışık düzen ınename, activityName|

## <a name="microsoftdatafactoryfactories"></a>Microsoft. DataFactory/Factory

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Pipelinefailedçalıştırmaları|Başarısız işlem hattı çalıştırmaları ölçümleri|Count|Toplam||Failuıyeniden yazma, ad|
|Ardışık düzen Inesucceededçalıştırmaları|Başarılı işlem hattı çalışma ölçümleri|Count|Toplam||Failuıyeniden yazma, ad|
|Activityfailedçalıştırmaları|Başarısız etkinlik çalıştırma ölçümleri|Count|Toplam||ActivityType, ardışık düzen adı, Failuyeniden yazma, ad|
|ActivitySucceededRuns|Başarılı etkinlik ölçümleri çalıştırıyor|Count|Toplam||ActivityType, ardışık düzen adı, Failuyeniden yazma, ad|
|Triggerfailedçalıştırmaları|Başarısız tetikleyici çalıştırma ölçümleri|Count|Toplam||Ad, Failuyeniden yazma|
|TriggerSucceededRuns|Başarılı tetikleyici çalışan ölçümleri|Count|Toplam||Ad, Failuyeniden yazma|
|Integrationruntimecpuyüzdesi|Tümleştirme çalışma zamanı CPU kullanımı|Percent|Average||Integrationruntimename, düğ,|
|Integrationruntimekullanılabilirliği Blememory|Tümleştirme çalışma zamanı kullanılabilir belleği|Bayt|Average||Integrationruntimename, düğ,|
|MaxAllowedResourceCount|İzin verilen en fazla varlık sayısı|Count|Maksimum||Boyut yok|
|Maxallowedfactorysizeingbits|İzin verilen en yüksek fabrika boyutu (GB birimi)|Count|Maksimum||Boyut yok|
|ResourceCount|Toplam varlık sayısı|Count|Maksimum||Boyut yok|
|Factorysizeingbits|Toplam fabrika boyutu (GB birimi)|Count|Maksimum||Boyut yok|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|JobEndedSuccess|Başarılı Işler|Count|Toplam|Başarılı iş sayısı.|Boyut yok|
|JobEndedFailure|Başarısız Işler|Count|Toplam|Başarısız iş sayısı.|Boyut yok|
|Jobendediptal edildi|İptal edilen Işler|Count|Toplam|İptal edilen işlerin sayısı.|Boyut yok|
|JobAUEndedSuccess|Başarılı AU Saati|Saniye|Toplam|Başarılı işler için toplam AU süresi.|Boyut yok|
|JobAUEndedFailure|Başarısız AU Saati|Saniye|Toplam|Başarısız işler için toplam AU süresi.|Boyut yok|
|Jobauendediptal edildi|İptal edilen AU Saati|Saniye|Toplam|İptal edilen işler için toplam AU süresi.|Boyut yok|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalStorage|Toplam Depolama Alanı|Bayt|Maksimum|Hesapta depolanan toplam veri miktarı.|Boyut yok|
|Veri yazıldı|Yazılan Veriler|Bayt|Toplam|Hesaba yazılan toplam veri miktarı.|Boyut yok|
|DataRead|Okunan Veriler|Bayt|Toplam|Hesaptan okunan toplam veri miktarı.|Boyut yok|
|WriteRequests|Yazma Istekleri|Count|Toplam|Hesaba veri yazma isteği sayısı.|Boyut yok|
|ReadRequests|Okuma Istekleri|Count|Toplam|Hesaba yönelik veri okuma isteklerinin sayısı.|Boyut yok|

## <a name="microsoftdbformariadbservers"></a>Microsoft. Dbformarıdb/sunucular

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|GÇ yüzdesi|Percent|Average|GÇ yüzdesi|Boyut yok|
|storage_percent|Depolama alanı yüzdesi|Percent|Average|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama alanı|Bayt|Average|Kullanılan depolama alanı|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Average|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Percent|Average|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Bayt|Average|Kullanılan sunucu günlüğü depolaması|Boyut yok|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Bayt|Average|Sunucu günlüğü depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Count|Average|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|seconds_behind_master|Saniye cinsinden çoğaltma gecikmesi|Count|Average|Saniye cinsinden çoğaltma gecikmesi|Boyut yok|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Average|Kullanılan yedekleme depolama alanı|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|

## <a name="microsoftdbformysqlservers"></a>Microsoft. Dbformyısql/sunucuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|GÇ yüzdesi|Percent|Average|GÇ yüzdesi|Boyut yok|
|storage_percent|Depolama alanı yüzdesi|Percent|Average|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama alanı|Bayt|Average|Kullanılan depolama alanı|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Average|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Percent|Average|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Bayt|Average|Kullanılan sunucu günlüğü depolaması|Boyut yok|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Bayt|Average|Sunucu günlüğü depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Count|Average|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|seconds_behind_master|Saniye cinsinden çoğaltma gecikmesi|Count|Average|Saniye cinsinden çoğaltma gecikmesi|Boyut yok|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Average|Kullanılan yedekleme depolama alanı|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft. DBforPostgreSQL/sunucuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Boyut yok|
|io_consumption_percent|GÇ yüzdesi|Percent|Average|GÇ yüzdesi|Boyut yok|
|storage_percent|Depolama alanı yüzdesi|Percent|Average|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama alanı|Bayt|Average|Kullanılan depolama alanı|Boyut yok|
|storage_limit|Depolama sınırı|Bayt|Average|Depolama sınırı|Boyut yok|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Percent|Average|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Bayt|Average|Kullanılan sunucu günlüğü depolaması|Boyut yok|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Bayt|Average|Sunucu günlüğü depolama sınırı|Boyut yok|
|active_connections|Etkin bağlantılar|Count|Average|Etkin bağlantılar|Boyut yok|
|connections_failed|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Average|Kullanılan yedekleme depolama alanı|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|
|pg_replica_log_delay_in_seconds|Çoğaltma gecikmesi|Saniye|Maksimum|Saniye cinsinden çoğaltma gecikmesi|Boyut yok|
|pg_replica_log_delay_in_bytes|Çoğaltmalar genelinde en fazla gecikme|Bayt|Maksimum|En fazla çoğaltma çoğaltmasının bayt cinsinden gecikmesi|Boyut yok|

## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft. DBforPostgreSQL/serversv2

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|memory_percent|Bellek yüzdesi|Percent|Average|Bellek yüzdesi|Boyut yok|
|'ye|IOPS|Count|Average|GÇ Işlemi/saniye|Boyut yok|
|storage_percent|Depolama alanı yüzdesi|Percent|Average|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Kullanılan depolama alanı|Bayt|Average|Kullanılan depolama alanı|Boyut yok|
|active_connections|Etkin bağlantılar|Count|Average|Etkin bağlantılar|Boyut yok|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/ıothubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri iletisi gönderme denemeleri|Count|Toplam|IoT Hub 'ınıza gönderilmeye çalışılan cihazdan buluta telemetri iletisi sayısı|Boyut yok|
|D2C. telemetri. giriş. başarılı|Gönderilen telemetri iletileri|Count|Toplam|IoT Hub 'ınıza başarıyla gönderilen cihazdan buluta telemetri iletisi sayısı|Boyut yok|
|c2d.commands.egress.complete.success|Komutlar tamamlandı|Count|Toplam|Cihaz tarafından başarıyla tamamlanan buluttan cihaza yönelik komutların sayısı|Boyut yok|
|c2d.commands.egress.abandon.success|Komutlar bırakıldı|Count|Toplam|Cihaz tarafından bırakılan buluttan cihaza yönelik komutların sayısı|Boyut yok|
|c2d.commands.egress.reject.success|Reddedilen komutlar|Count|Toplam|Cihaz tarafından reddedilen buluttan cihaza yönelik komutların sayısı|Boyut yok|
|Devices. totalDevices|Toplam cihaz (kullanım dışı)|Count|Toplam|IoT Hub 'ınıza kayıtlı cihazların sayısı|Boyut yok|
|devices.connectedDevices.allProtocol|Bağlı cihazlar (kullanım dışı) |Count|Toplam|IoT Hub 'ınıza bağlı cihazların sayısı|Boyut yok|
|D2C. telemetri. çıkış. başarılı|Yönlendirme: teslim edilen telemetri iletileri|Count|Toplam|IoT Hub yönlendirme kullanılarak iletilerin tüm uç noktalara başarıyla ulaştırılan ileti sayısı. Bir ileti birden çok uç noktaya yönlendirilmemişse, her başarılı teslimat için bu değer bir artar. Bir ileti aynı uç noktaya birden çok kez teslim edildiğinde, her başarılı teslimat için bu değer bir artar.|Boyut yok|
|D2C. telemetri. çıkış. bırakıldı|Yönlendirme: telemetri iletileri bırakıldı |Count|Toplam|İleti, ölü uç noktalar nedeniyle IoT Hub yönlendirmenin kaç kez bırakılmakta olduğunu. Bu değer, bırakılan iletiler buraya teslim edimediğinden geri dönüş rotasına teslim edilen iletileri saymaz.|Boyut yok|
|d2c.telemetry.egress.orphaned|Yönlendirme: telemetri iletileri yalnız bırakılmış |Count|Toplam|Herhangi bir yönlendirme kuralıyla eşleşmediğinden (geri dönüş kuralı dahil) iletilerin IoT Hub yönlendirme tarafından yalnız bırakılmış olduğu zaman sayısı. |Boyut yok|
|D2C. telemetri. çıkış. geçersiz|Yönlendirme: telemetri iletileri uyumsuz|Count|Toplam|Uç nokta ile uyumsuzluk nedeniyle IoT Hub yönlendirmenin iletileri teslim etme başarısız olduğu zaman sayısı. Bu değer yeniden denemeler içermez.|Boyut yok|
|d2c.telemetry.egress.fallback|Yönlendirme: geri dönüşe teslim edilen iletiler|Count|Toplam|Geri dönüş rotası ile ilişkili uç noktaya gönderilen iletileri yönlendirmenin IoT Hub sayısı.|Boyut yok|
|D2C. endpoints. çıkış. eventHubs|Yönlendirme: Olay Hub 'ına teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin Olay Hub 'ı uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Boyut yok|
|D2C. endpoints. Latency. eventHubs|Yönlendirme: Olay Hub 'ı için ileti gecikmesi|Milisaniye|Average|İleti girişi ile bir olay hub 'ı uç noktasına giriş IoT Hub ve ileti girişi arasındaki ortalama gecikme (milisaniye).|Boyut yok|
|d2c.endpoints.egress.serviceBusQueues|Yönlendirme: Service Bus kuyruğuna teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin iletileri Service Bus sıra uç noktalarına başarıyla teslim dağıttığı zaman sayısı.|Boyut yok|
|d2c.endpoints.latency.serviceBusQueues|Yönlendirme: Service Bus kuyruğu için ileti gecikmesi|Milisaniye|Average|İleti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye) Service Bus kuyruğu uç noktasına giriş.|Boyut yok|
|d2c.endpoints.egress.serviceBusTopics|Yönlendirme: Service Bus konuya teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin Service Bus konu uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Boyut yok|
|d2c.endpoints.latency.serviceBusTopics|Yönlendirme: Service Bus konusu için ileti gecikmesi|Milisaniye|Average|İleti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye) Service Bus konu uç noktasına giriş.|Boyut yok|
|D2C. endpoints. çıkış. builtIn. Events|Yönlendirme: iletilere/olaylara teslim edilen iletiler|Count|Toplam|Yönlendirmenin, yerleşik uç noktaya (iletiler/olaylar) başarıyla ileti teslim IoT Hub sayısı. Bu ölçüm yalnızca Yönlendirme etkinken çalışmaya başlar (https://aka.ms/iotrouting) IoT Hub 'ı için).|Boyut yok|
|D2C. endpoints. Latency. yerleik. Events|Yönlendirme: iletiler/olaylar için ileti gecikmesi|Milisaniye|Average|Yerleşik uç noktaya (iletiler/olaylar) gelen ileti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye). Bu ölçüm yalnızca Yönlendirme etkinken çalışmaya başlar (https://aka.ms/iotrouting) IoT Hub 'ı için).|Boyut yok|
|D2C. endpoints. çıkış. Storage|Yönlendirme: depolamaya teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin depolama uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Boyut yok|
|D2C. endpoints. Latency. Storage|Yönlendirme: depolama için ileti gecikmesi|Milisaniye|Average|İleti girişi IoT Hub ve telemetri iletisine bir depolama uç noktasına giriş arasındaki ortalama gecikme süresi (milisaniye).|Boyut yok|
|D2C. endpoints. çıkış. Storage. Bytes|Yönlendirme: depolamaya teslim edilen veriler|Bayt|Toplam|Depolama uç noktalarına teslim edilen veri miktarı (bayt) IoT Hub.|Boyut yok|
|D2C. endpoints. çıkış. Storage. blob 'ları|Yönlendirme: depolamaya teslim edilen Bloblar|Count|Toplam|IoT Hub blob 'ları depolama uç noktalarına yönlendirmenin kaç kez yönlendirtığı sayısıdır.|Boyut yok|
|Eventgridteslimler|Event Grid teslimler (Önizleme)|Count|Toplam|Event Grid yayımlanan IoT Hub olaylarının sayısı. Başarılı ve başarısız isteklerin sayısı için sonuç boyutunu kullanın. EventType boyutu olay türünü gösterir (https://aka.ms/ioteventgrid).|Sonuç, EventType|
|EventGridLatency|Olay Event Grid yayımlandığında IoT Hub olayının oluşturulduğu sürenin ortalama gecikme süresi (milisaniye). Bu sayı, tüm olay türleri arasındaki bir ortalama değer. Belirli bir olay türünün gecikmesini görmek için EventType boyutunu kullanın.|EventType|
|d2c.twin.read.success|Cihazlardan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi okuma sayısı.|Boyut yok|
|d2c.twin.read.failure|Cihazlardan başarısız ikizi okumaları|Count|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi okumaların sayısı.|Boyut yok|
|D2C. ikizi. Read. size|Cihazlardan gelen ikizi okumaların yanıt boyutu|Bayt|Average|Tüm başarılı cihaz tarafından başlatılan ikizi okumalarının ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|D2C. ikizi. Update. Success|Cihazlardan başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Boyut yok|
|d2c.twin.update.failure|Cihazlardan ikizi güncelleştirmeleri başarısız oldu|Count|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Boyut yok|
|d2c.twin.update.size|Cihazlardan ikizi güncelleştirmelerinin boyutu|Bayt|Average|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin ortalama, en düşük ve en büyük boyutu.|Boyut yok|
|c2d.methods.success|Başarılı doğrudan yöntem etkinleştirmeleri|Count|Toplam|Tüm başarılı doğrudan yöntem çağrılarının sayısı.|Boyut yok|
|C2D. Methods. Failure|Başarısız doğrudan yöntem etkinleştirmeleri|Count|Toplam|Tüm başarısız doğrudan yöntem çağrılarının sayısı.|Boyut yok|
|c2d.methods.requestSize|Doğrudan yöntem etkinleştirmeleri istek boyutu|Bayt|Average|Tüm başarılı doğrudan yöntem isteklerinin ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|c2d.methods.responseSize|Doğrudan yöntem etkinleştirmeleri yanıt boyutu|Bayt|Average|Tüm başarılı doğrudan yöntem yanıtlarının ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|c2d.twin.read.success|Arka uçtan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı arka uç tarafından başlatılan ikizi okumaların sayısı.|Boyut yok|
|C2D. ikizi. Read. Failure|Arka uçtan başarısız ikizi okumaları|Count|Toplam|Tüm başarısız arka uç tarafından başlatılan ikizi okumaların sayısı.|Boyut yok|
|C2D. ikizi. Read. size|Arka uçtan gelen ikizi okumaların yanıt boyutu|Bayt|Average|Tüm başarılı arka uç başlatılan ikizi okumalarının ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|C2D. ikizi. Update. Success|Arka uçtan başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin sayısı.|Boyut yok|
|C2D. ikizi. Update. Failure|Arka uçtan başarısız ikizi güncelleştirmeleri|Count|Toplam|Tüm başarısız arka uç başlatılan ikizi güncelleştirmelerinin sayısı.|Boyut yok|
|c2d.twin.update.size|Arka uçtan ikizi güncelleştirmelerinin boyutu|Bayt|Average|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin ortalama, en düşük ve en büyük boyutu.|Boyut yok|
|twinQueries. Success|Başarılı ikizi sorguları|Count|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Boyut yok|
|twinQueries. Failure|Başarısız ikizi sorguları|Count|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Boyut yok|
|twinQueries.resultSize|İkizi sorguları sonuç boyutu|Bayt|Average|Tüm başarılı ikizi sorgularının sonuç boyutunun ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|jobs.createTwinUpdateJob.success|İkizi Update işlerinin başarılı oluşturmaları|Count|Toplam|Tüm başarılı ikizi Update işlerinin oluşturma sayısı.|Boyut yok|
|jobs.createTwinUpdateJob.failure|İkizi Update işlerinin başarısız oluşturmaları|Count|Toplam|İkizi Update işlerinin başarısız olan tüm oluşturma sayısı.|Boyut yok|
|Jobs. createDirectMethodJob. Success|Yöntem çağırma işlerinin başarılı oluşturmaları|Count|Toplam|Tüm başarılı doğrudan yöntem çağırma işlerinin oluşturulma sayısı.|Boyut yok|
|Jobs. createDirectMethodJob. Failure|Yöntem çağırma işlerinin oluşturma işlemi başarısız oldu|Count|Toplam|Doğrudan yöntem çağırma işlerinin tüm başarısız oluşturma sayısı.|Boyut yok|
|işler. listJobs. başarılı|İşleri listelemek için başarılı çağrılar|Count|Toplam|İşleri listelemek için başarılı olan tüm çağrıların sayısı.|Boyut yok|
|Jobs. listJobs. hata|İşleri listelemek için başarısız çağrılar|Count|Toplam|Listeleme işleri için başarısız olan tüm çağrıların sayısı.|Boyut yok|
|Jobs. cancelJob. Success|Başarılı iş iptalleri|Count|Toplam|İşi iptal etmek için tüm başarılı çağrı sayısı.|Boyut yok|
|Jobs. cancelJob. Failure|Başarısız iş iptalleri|Count|Toplam|İşi iptal etmek için başarısız olan tüm çağrıların sayısı.|Boyut yok|
|işler. queryJobs. başarılı|Başarılı iş sorguları|Count|Toplam|Sorgu işlerine yapılan tüm başarılı çağrıların sayısı.|Boyut yok|
|Jobs. queryJobs. hata|Başarısız iş sorguları|Count|Toplam|Sorgu işlerine yapılan tüm başarısız çağrıların sayısı.|Boyut yok|
|işler. tamamlandı|Tamamlanan işler|Count|Toplam|Tamamlanan tüm işlerin sayısı.|Boyut yok|
|işler. başarısız|Başarısız işler|Count|Toplam|Başarısız olan tüm işlerin sayısı.|Boyut yok|
|d2c.telemetry.ingress.sendThrottle|Daraltma hatası sayısı|Count|Toplam|Cihaz performansı azaltmasından kaynaklanan azaltma hatası sayısı|Boyut yok|
|Günlükmessagequotakullanıldı|Kullanılan toplam ileti sayısı|Count|Average|Bugün kullanılan toplam ileti sayısı. Bu, her gün 00:00 UTC tarihinde sıfıra sıfırlanan toplu bir değerdir.|Boyut yok|
|deviceDataUsage|Toplam cihaz verisi kullanımı|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Boyut yok|
|totalDeviceCount|Toplam cihaz (Önizleme)|Count|Average|IoT Hub 'ınıza kayıtlı cihazların sayısı|Boyut yok|
|connectedDeviceCount|Bağlı cihazlar (Önizleme)|Count|Average|IoT Hub 'ınıza bağlı cihazların sayısı|Boyut yok|
|konfigürasyonları|Yapılandırma ölçümleri|Count|Toplam|Yapılandırma Işlemleri için ölçümler|Boyut yok|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft. Devices/provisioningServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Registrationdenemeleri|Kayıt denemeleri|Count|Toplam|Denenen cihaz kaydı sayısı|ProvisioningServiceName, IotHubName, durum|
|Deviceasyleri|Atanan cihazlar|Count|Toplam|IoT Hub 'ına atanan cihazların sayısı|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Kanıtlama denemeleri|Count|Toplam|Denenen cihaz belirlediğimizi karşıladığımızı sayısı|ProvisioningServiceName, durum, protokol|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft. DocumentDB/databaseAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AvailableStorage|Kullanılabilir depolama alanı|Bayt|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam kullanılabilir depolama alanı|CollectionName, DatabaseName, bölgesi|
|Cassandraconnectionkapanışları|Cassandra bağlantı kapanışları|Count|Toplam|1 dakikalık ayrıntı düzeyinde raporlanan, kapatılan Cassandra bağlantısı sayısı|Bölge, ClosureReason|
|Cassandrarequestücretleri|Cassandra Istek ücretleri|Count|Toplam|Cassandra istekleri için tüketilen RUs|DatabaseName, CollectionName, bölge, OperationType, ResourceType|
|CassandraRequests|Cassandra Istekleri|Count|Count|Yapılan Cassandra isteği sayısı|DatabaseName, CollectionName, bölge, OperationType, ResourceType, ErrorCode|
|Veri kullanımı|Veri Kullanımı|Bayt|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam veri kullanımı|CollectionName, DatabaseName, bölgesi|
|DocumentCount|Belge sayısı|Count|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam belge sayısı|CollectionName, DatabaseName, bölgesi|
|DocumentQuota|Belge kotası|Bayt|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam depolama kotası|CollectionName, DatabaseName, bölgesi|
|Indexusage|Dizin kullanımı|Bayt|Toplam|5 dakika ayrıntı düzeyi olarak raporlanan toplam dizin kullanımı|CollectionName, DatabaseName, bölgesi|
|Meta veri datarequests|Meta veri Istekleri|Count|Count|Meta veri isteklerinin sayısı. Cosmos DB, her bir hesap için sistem meta veri toplamayı korur, bu da koleksiyonları, veritabanlarını, vb. ve bunların yapılandırmasını, ücretsiz olarak listeletmenize olanak tanır.|DatabaseName, CollectionName, bölge, StatusCode, |
|MongoRequestCharge|Mongo Istek ücreti|Count|Toplam|Tüketilen Mongo Istek birimleri|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequests|Mongo Istekleri|Count|Count|Yapılan Mongo Isteği sayısı|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|Provisionedüretilen Iş|Sağlanan Aktarım Hızı|Count|Maksimum|Sağlanan Aktarım Hızı|DatabaseName, CollectionName|
|ReplicationLatency|P99 çoğaltma gecikmesi|Mayacak|Average|Coğrafi olarak etkinleştirilen hesap için kaynak ve hedef bölgelerde P99 çoğaltma gecikme süresi|SourceRegion, TargetRegion|
|ServiceAvailability|Hizmet kullanılabilirliği|Percent|Average|Hesap istekleri bir saat, gün veya ay ayrıntı düzeyinde kullanılabilirlik|Boyut yok|
|TotalRequestUnits|Toplam Istek birimleri|Count|Toplam|Tüketilen istek birimleri|DatabaseName, CollectionName, bölge, StatusCode, OperationType|
|Toplam Istek sayısı|Toplam İstek Sayısı|Count|Count|Yapılan istek sayısı|DatabaseName, CollectionName, bölge, StatusCode, OperationType|

## <a name="microsofteventgridtopics"></a>Microsoft. EventGrid/konuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|PublishFailCount|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Count|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft. EventGrid/Eventabonelikleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|MatchedEventCount|Eşleşen Olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Boyut yok|
|Ityattemptfailcount|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType|
|Deliverybaşarılı sayısı|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Boyut yok|
|DestinationProcessingDurationInMs|Hedef Işleme süresi|Milisaniye|Average|Milisaniye cinsinden hedef işleme süresi|Boyut yok|
|DroppedEventCount|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason|
|DeadLetteredCount|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft. EventGrid/Extensionkonuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|PublishFailCount|Başarısız olaylar|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Count|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı İstekler|Count|Toplam|Microsoft. EventHub için başarılı Istekler.|EntityName |
|ServerErrors|Sunucu hataları.|Count|Toplam|Microsoft. EventHub için sunucu hataları.|EntityName |
|UserErrors|Kullanıcı hataları.|Count|Toplam|Microsoft. EventHub için Kullanıcı hataları.|EntityName |
|QuotaExceededErrors|Kota, hataları aştı.|Count|Toplam|Microsoft. EventHub için Kota aşıldı.|EntityName |
|ThrottledRequests|Kısıtlanmış Istekler.|Count|Toplam|Microsoft. EventHub için kısıtlanan Istekler.|EntityName |
|Incomingrequests|Gelen İstekler|Count|Toplam|Microsoft. EventHub için gelen Istekler.|EntityName|
|Incomingmessages|Gelen İletiler|Count|Toplam|Microsoft. EventHub için gelen Iletiler.|EntityName|
|OutgoingMessages|Giden İletiler|Count|Toplam|Microsoft. EventHub için giden Iletiler.|EntityName|
|Incomingbytes|Gelen bayt sayısı.|Bayt|Toplam|Microsoft. EventHub için gelen baytlar.|EntityName|
|OutgoingBytes|Giden bayt sayısı.|Bayt|Toplam|Microsoft. EventHub için giden baytlar.|EntityName|
|ActiveConnections|ActiveConnections|Count|Average|Microsoft. EventHub için toplam etkin bağlantı sayısı.|Boyut yok|
|ConnectionsOpened|Bağlantılar açıldı.|Count|Average|Microsoft. EventHub için açılan bağlantılar.|EntityName|
|ConnectionsClosed|Bağlantı kapatıldı.|Count|Average|Microsoft. EventHub için kapatılan bağlantılar.|EntityName|
|CaptureBacklog|Biriktirme listesini yakala.|Count|Toplam|Microsoft. EventHub için yakalama biriktirme listesi.|EntityName|
|CapturedMessages|Yakalanan Iletiler.|Count|Toplam|Microsoft. EventHub için yakalanan Iletiler.|EntityName|
|CapturedBytes|Yakalanan baytlar.|Bayt|Toplam|Microsoft. EventHub için yakalanan baytlar.|EntityName|
|Size|Size|Bayt|Average|Bir EventHub 'ın bayt cinsinden boyutu.|EntityName|
|INREQS|Gelen Istekler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam gelen gönderme isteği sayısı (kullanım dışı)|Boyut yok|
|SUCCREQ|Başarılı Istekler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam başarılı istek (kullanım dışı)|Boyut yok|
|FAILREQ|Başarısız Istekler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam başarısız istek (kullanım dışı)|Boyut yok|
|SVRBSY|Sunucu meşgul hataları (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam sunucu meşgul hatası (kullanım dışı)|Boyut yok|
|INTERR|İç sunucu hataları (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam iç sunucu hatası (kullanım dışı)|Boyut yok|
|HATALI CERR|Diğer hatalar (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam başarısız istek (kullanım dışı)|Boyut yok|
|GİRİŞ İLETİLERİ|Gelen Iletiler (kullanım dışı) (kullanım dışı)|Count|Toplam|Bir ad alanı için gelen toplam ileti sayısı. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine gelen Iletiler ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHINILETISI|Gelen Iletiler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam gelen ileti (kullanım dışı)|Boyut yok|
|DIŞ MESAJ|Giden Iletiler (kullanım dışı) (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam giden ileti. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine giden Iletiler ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHODIKILETISI|Giden Iletiler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam giden ileti (kullanım dışı)|Boyut yok|
|EHINMB|Gelen bayt (kullanım dışı) (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı gelen ileti işleme. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine gelen bayt ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHINBYTES|Gelen baytlar (kullanım dışı)|Bayt|Toplam|Ad alanı için Olay Hub 'ı gelen ileti işleme (kullanım dışı)|Boyut yok|
|EHOUTMB|Giden baytlar (kullanım dışı) (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı giden ileti işleme. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine giden bayt ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHOUTBYTES|Giden baytlar (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı giden ileti işleme (kullanım dışı)|Boyut yok|
|EHABL|Biriktirme listesi iletilerini Arşivle (kullanım dışı)|Count|Toplam|Ad alanı için biriktirme listesindeki Olay Hub 'ı arşiv iletileri (kullanım dışı)|Boyut yok|
|EHAMSGS|İletileri Arşivle (kullanım dışı)|Count|Toplam|Bir ad alanında olay hub 'ı arşivlenmiş iletiler (kullanım dışı)|Boyut yok|
|EHAMB|Arşiv iletisi verimlilik (kullanım dışı)|Bayt|Toplam|Bir ad alanında olay hub 'ı arşivlenmiş ileti işleme (kullanım dışı)|Boyut yok|

## <a name="microsofteventhubclusters"></a>Microsoft. EventHub/kümeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı Istekler (Önizleme)|Count|Toplam|Microsoft. EventHub için başarılı Istekler. (Önizleme)|Boyut yok|
|ServerErrors|Sunucu hataları. (Önizleme)|Count|Toplam|Microsoft. EventHub için sunucu hataları. (Önizleme)|Boyut yok|
|UserErrors|Kullanıcı hataları. (Önizleme)|Count|Toplam|Microsoft. EventHub için Kullanıcı hataları. (Önizleme)|Boyut yok|
|QuotaExceededErrors|Kota, hataları aştı. (Önizleme)|Count|Toplam|Microsoft. EventHub için Kota aşıldı. (Önizleme)|Boyut yok|
|ThrottledRequests|Kısıtlanmış Istekler. (Önizleme)|Count|Toplam|Microsoft. EventHub için kısıtlanan Istekler. (Önizleme)|Boyut yok|
|Incomingrequests|Gelen Istekler (Önizleme)|Count|Toplam|Microsoft. EventHub için gelen Istekler. (Önizleme)|Boyut yok|
|Incomingmessages|Gelen Iletiler (Önizleme)|Count|Toplam|Microsoft. EventHub için gelen Iletiler. (Önizleme)|Boyut yok|
|OutgoingMessages|Giden Iletiler (Önizleme)|Count|Toplam|Microsoft. EventHub için giden Iletiler. (Önizleme)|Boyut yok|
|Incomingbytes|Gelen bayt sayısı. (Önizleme)|Bayt|Toplam|Microsoft. EventHub için gelen baytlar. (Önizleme)|Boyut yok|
|OutgoingBytes|Giden bayt sayısı. (Önizleme)|Bayt|Toplam|Microsoft. EventHub için giden baytlar. (Önizleme)|Boyut yok|
|ActiveConnections|ActiveConnections (Önizleme)|Count|Average|Microsoft. EventHub için toplam etkin bağlantı sayısı. (Önizleme)|Boyut yok|
|ConnectionsOpened|Bağlantılar açıldı. (Önizleme)|Count|Average|Microsoft. EventHub için açılan bağlantılar. (Önizleme)|Boyut yok|
|ConnectionsClosed|Bağlantı kapatıldı. (Önizleme)|Count|Average|Microsoft. EventHub için kapatılan bağlantılar. (Önizleme)|Boyut yok|
|CaptureBacklog|Biriktirme listesini yakala. (Önizleme)|Count|Toplam|Microsoft. EventHub için yakalama biriktirme listesi. (Önizleme)|Boyut yok|
|CapturedMessages|Yakalanan Iletiler. (Önizleme)|Count|Toplam|Microsoft. EventHub için yakalanan Iletiler. (Önizleme)|Boyut yok|
|CapturedBytes|Yakalanan baytlar. (Önizleme)|Bayt|Toplam|Microsoft. EventHub için yakalanan baytlar. (Önizleme)|Boyut yok|
|CPU|CPU (Önizleme)|Percent|Maksimum|Bir yüzde olarak olay hub 'ı kümesine yönelik CPU kullanımı|Role|
|Kullanılabilirlik Blememory|Kullanılabilir bellek (Önizleme)|Count|Maksimum|Event hub kümesi için bayt cinsinden kullanılabilir bellek|Role|

## <a name="microsofthdinsightclusters"></a>Microsoft. HDInsight/kümeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|GatewayRequests|Ağ Geçidi Istekleri|Count|Toplam|Ağ Geçidi isteklerinin sayısı|ClusterDnsName, HttpStatus|
|Kategorizedgatewayrequests|Kategorilere ayrılmış ağ geçidi Istekleri|Count|Toplam|Kategorilere göre ağ geçidi isteklerinin sayısı (1xx/2xx/3xx/4xx/5xx)|ClusterDnsName, HttpStatus|
|Numactiveçalışanları|Etkin çalışan sayısı|Count|Maksimum|Etkin çalışan sayısı|ClusterDnsName, MetricName|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft. Insights/oto Scalesettings

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ObservedMetricValue|Gözlenen Ölçüm Değeri|Count|Average|Yürütüldüğünde otomatik ölçeklendirme tarafından hesaplanan değer|MetricTriggerSource|
|MetricThreshold|Ölçüm Eşiği|Count|Average|Otomatik ölçeklendirme çalıştırıldığında yapılandırılan otomatik ölçeklendirme eşiği.|MetricTriggerRule|
|ObservedCapacity|Gözlenen Kapasite|Count|Average|Yürütüldüğünde otomatik ölçeklendirme olarak bildirilen kapasite.|Boyut yok|
|Scaleactionsınıated|Başlatılan Ölçeklendirme Eylemleri|Count|Toplam|Ölçek işleminin yönü.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft. Insights/bileşenler

(Genel Önizleme)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanılabilirlik sonuçları/kullanılabilirliği yüzdesi|Kullanılabilirlik|Yüzde|Average|Başarıyla tamamlanan kullanılabilirlik testlerinin yüzdesi|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu|
|Kullanılabilirlik sonuçları/sayısı|Kullanılabilirlik testleri|Count|Count|Kullanılabilirlik testi sayısı|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu, kullanılabilirlik sonucu/başarısı|
|Kullanılabilirlik sonuçları/süresi|Kullanılabilirlik testi süresi|Mayacak|Average|Kullanılabilirlik testi süresi|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu, kullanılabilirlik sonucu/başarısı|
|Browserzamanlamalar/networkDuration|Sayfa yükleme ağ bağlantı süresi|Mayacak|Average|Kullanıcı isteği ile ağ bağlantısı arasındaki süre. DNS arama ve aktarım bağlantısı içerir.|Boyut yok|
|Browserzamanlamalar/processingDuration|İstemci işlem süresi|Mayacak|Average|DOM yükleninceye kadar belgenin son baytını alma arasındaki süre. Zaman uyumsuz istekler hala işlenmeye devam edebilir.|Boyut yok|
|Browserzamanlamalar/receiveDuration|Yanıt süresi alınıyor|Mayacak|Average|İlk ve son bayt veya bağlantının kesilmesi arasındaki süre.|Boyut yok|
|Browserzamanlamalar/sendDuration|İstek gönderme süresi|Mayacak|Average|Ağ bağlantısı ve ilk baytın alınması arasındaki süre.|Boyut yok|
|Browserzamanlamalar/totalDuration|Tarayıcı sayfa yükleme süresi|Mayacak|Average|DOM, stil sayfaları, betikler ve görüntüler yükleninceye kadar Kullanıcı isteğinden geçen süre.|Boyut yok|
|Bağımlılıklar/sayı|Bağımlılık çağrıları|Count|Count|Uygulama tarafından dış kaynaklara yapılan çağrıların sayısı.|bağımlılık/tür, bağımlılık/performanceBucket, bağımlılık/başarı, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|Bağımlılıklar/süre|Bağımlılık süresi|Mayacak|Average|Uygulama tarafından dış kaynaklara yapılan çağrıların süresi.|bağımlılık/tür, bağımlılık/performanceBucket, bağımlılık/başarı, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|Bağımlılıklar/başarısız|Bağımlılık çağrısı sorunları|Count|Count|Uygulama tarafından dış kaynaklara yapılan başarısız bağımlılık çağrılarının sayısı.|bağımlılık/tür, bağımlılık/performanceBucket, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|pageViews/Count|Sayfa görünümleri|Count|Count|Sayfa görüntüleme sayısı.|işlem/yapay|
|pageViews/Duration|Sayfa görüntüleme yükleme süresi|Mayacak|Average|Sayfa görüntüleme yükleme süresi|işlem/yapay|
|performanceCounters/requestExecutionTime|HTTP isteği yürütme süresi|Mayacak|Average|En son isteğin yürütme süresi.|Cloud/Roleınstance|
|performanceCounters/Requestsınqueue|Uygulama kuyruğundaki HTTP istekleri|Count|Average|Uygulama isteği sırasının uzunluğu.|Cloud/Roleınstance|
|performanceCounters/requestsPerSecond|HTTP isteği oranı|CountPerSecond|Average|ASP.NET ' den saniyede uygulamaya yapılan tüm isteklerin oranı.|Cloud/Roleınstance|
|performanceCounters/exceptionsPerSecond|Özel durum oranı|CountPerSecond|Average|.NET özel durumları ve .NET özel durumlarına dönüştürülmüş yönetilmeyen özel durumlar da dahil olmak üzere, Windows 'a bildirilen işlenmiş ve işlenmemiş özel durumların sayısı.|Cloud/Roleınstance|
|performanceCounters/Processıobitespersecond|İşlem GÇ hızı|BytesPerSecond|Average|Dakikada, ağda ve cihazlarda okunan ve yazılan toplam bayt/saniye.|Cloud/Roleınstance|
|performanceCounters/processCpuPercentage|İşlem CPU'su|Percent|Average|Tüm işlem iş parçacıklarının yönergeleri yürütmek için işlemciyi kullandığı geçen sürenin yüzdesi. Bu, 0 ila 100 arasında farklılık gösterebilir. Bu ölçüm, W3wp sürecinin performansını tek başına gösterir.|Cloud/Roleınstance|
|performanceCounters/Processorcpuyüzdesi|İşlemci zamanı|Percent|Average|İşlemcinin boş olmayan iş parçacıklarında harcadığı sürenin yüzdesi.|Cloud/Roleınstance|
|performanceCounters/memoryAvailableBytes|Uygun bellek|Bayt|Average|Bir işleme veya sistem kullanımına ayırmak için hemen kullanılabilir fiziksel bellek.|Cloud/Roleınstance|
|performanceCounters/processPrivateBytes|İşleme özel bayt sayısı|Bayt|Average|İzlenen uygulamanın işlemlerine özel olarak atanan bellek.|Cloud/Roleınstance|
|istek/süre|Sunucu yanıt süresi|Mayacak|Average|Bir HTTP isteği alma ve yanıtın gönderilmesini tamamlama arasındaki süre.|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|istek/sayı|Sunucu istekleri|Count|Count|Tamamlanan HTTP isteği sayısı.|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|istek/başarısız|Başarısız istekler|Count|Count|Başarısız olarak işaretlenen HTTP isteklerinin sayısı. Çoğu durumda bunlar > = 400 yanıt koduna sahip isteklerdir ve 401 ' e eşit değildir.|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|istek/hız|Sunucu isteği hızı|CountPerSecond|Average|Saniye başına sunucu isteği oranı|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|özel durumlar/say|Özel durumlar|Count|Count|Tüm yakalanamayan özel durumların Birleşik sayısı.|bulut/rol adı, bulut/Roleınstance, istemci/tür|
|özel durumlar/tarayıcı|Tarayıcı özel durumları|Count|Count|Tarayıcıda oluşturulan yakalanamayan özel durumların sayısı.|Boyut yok|
|özel durumlar/sunucu|Sunucu özel durumları|Count|Count|Sunucu uygulamasında oluşturulan yakalanamayan özel durum sayısı.|Cloud/roleName, Cloud/Roleınstance|
|izlemeler/say|İzlemeler|Count|Count|İzleme belgesi sayısı|izleme/severityLevel, işlem/yapay, Cloud/roleName, Cloud/Roleınstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServiceApiHit|Hizmet API'si Toplam İsabet|Count|Count|Toplam hizmet API 'si isabet sayısı|ActivityType, ActivityName|
|Hizmet Apilatesi|Hizmet API'si Toplam Gecikme|Milisaniye|Average|Hizmet API 'si isteklerinin genel gecikmesi|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Hizmet API'si Toplam Sonuç|Count|Count|Toplam hizmet API 'si sonuç sayısı|ActivityType, ActivityName, StatusCode|

## <a name="microsoftkustoclusters"></a>Microsoft. kusto/kümeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Cachekullanım|Önbellek kullanımı|Percent|Average|Küme kapsamındaki kullanım düzeyi|Yok.|
|QueryDuration|Sorgu süresi|Milisaniye|Average|Sorgu süresi (saniye)|Sorgu durumu|
|Alım kullanımı|Alım kullanımı|Percent|Average|Kümedeki kullanılan alma yuvaları oranı|Yok.|
|Alı|Canlı tut|Count|Average|Sanity denetimi, kümenin sorgulara yanıt verdiğini belirtir|Yok.|
|Inestionvolumeınmb|Alım birimi (MB)|Count|Toplam|Kümeye alınan verilerin toplam hacmi (MB)|Database|
|Inestionlatencınseconds|Alma gecikmesi (saniye)|Saniye|Average|Kaynaktan (örn. ileti EventHub ' de bulunur) kümeye saniye cinsinden alma süresi|Yok.|
|EventProcessedForEventHubs|Işlenen Olaylar (Event Hubs için)|Count|Toplam|Olay Hub 'ından geri ödeme yaparken küme tarafından işlenen olay sayısı|Yok.|
|Inestionresult|Alım sonucu|Count|Count|Alma işlemlerinin sayısı|Durum|
|CPU|CPU|Percent|Average|CPU kullanım düzeyi|Yok.|
| Continuousexportnumofrecordsexports | Sürekli dışarı aktarmada dışarı aktarılmış kayıt sayısı | Count | Toplam | Dışarı aktarma işlemi sırasında yazılan her depolama yapıtı için dışarı aktarılmış kayıt sayısı  | Yok. |
| Dışarı Aktaramstilileştirme | Kullanım verme | Percent | Maksimum | Kullanım verme | Yok. |
| ContinuousExportPendingCount | Sürekli dışarı aktarma bekleyen sayısı | Count | Maksimum | Yürütmeye hazırlamış bekleyen sürekli dışarı aktarma işlerinin sayısı | Yok. |
| ContinuousExportMaxLatenessMinutes | Sürekli dışarı aktarma en fazla dakika | Count | Maksimum | Bekleyen ve yürütme için hazırlanma olan tüm sürekli dışarı aktarmalar için dakika cinsinden en uzun süre | Yok. |

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft. LocationBasedServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Count|Count|API çağrısı sayısı|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Başlatılan Çalıştırmalar|Count|Toplam|Başlatılan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsCompleted|Tamamlanan Çalıştırmalar|Count|Toplam|Tamamlanan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsSucceeded|Başarılı Çalıştırmalar|Count|Toplam|İş akışı çalıştırma sayısı başarılı oldu.|Boyut yok|
|RunsFailed|Başarısız Çalıştırmalar|Count|Toplam|Başarısız iş akışı çalıştırma sayısı.|Boyut yok|
|RunsCancelled|Çalıştırma Iptal edildi|Count|Toplam|İptal edilen iş akışı çalıştırma sayısı.|Boyut yok|
|RunLatency|Çalıştırma Gecikmesi|Saniye|Average|Tamamlanan iş akışı çalıştırmalarının gecikmesi.|Boyut yok|
|Runbaşarılı gecikme süresi|Başarılı Çalıştırma Gecikmesi|Saniye|Average|Başarılı iş akışı çalıştırmalarının gecikmesi.|Boyut yok|
|RunThrottledEvents|Çalıştırılması Kısıtlanan Olaylar|Count|Toplam|İş akışı eylemi veya tetikleyici kısıtlanan olay sayısı.|Boyut yok|
|RunFailurePercentage yüzdesi|Çalıştırma Hatası Yüzdesi|Percent|Toplam|İş akışı çalıştırmalarının yüzdesi başarısız oldu.|Boyut yok|
|ActionsStarted|Başlatılan Eylemler |Count|Toplam|Başlatılan iş akışı eylemi sayısı.|Boyut yok|
|ActionsCompleted|Tamamlanan Eylemler |Count|Toplam|Tamamlanan iş akışı eylemlerinin sayısı.|Boyut yok|
|ActionsSucceeded|Başarılı Eylemler |Count|Toplam|Başarılı iş akışı eylemi sayısı.|Boyut yok|
|ActionsFailed|Başarısız Eylemler|Count|Toplam|Başarısız iş akışı eylemi sayısı.|Boyut yok|
|ActionsSkipped|Atlanan Eylemler |Count|Toplam|Atlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionLatency|Eylem Gecikmesi |Saniye|Average|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|Actionbaşarılı gecikme süresi|Başarılı Eylem Gecikmesi |Saniye|Average|Başarılı iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionThrottledEvents|Eylemi Kısıtlanan Olaylar|Count|Toplam|İş akışı eylemi kısıtlanmış olay sayısı..|Boyut yok|
|TriggersStarted|Başlatılan Tetikleyiciler |Count|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersCompleted|Tamamlanan Tetikleyiciler |Count|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSucceeded|Başarılı Olan Tetikleyiciler |Count|Toplam|Başarılı iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersFailed|Başarısız Olan Tetikleyiciler |Count|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSkipped|Atlanan Tetikleyiciler|Count|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|Triggersharekete geçirildi|Tetiklenen Tetikleyiciler |Count|Toplam|Tetiklenen iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggerLatency|Tetikleyici Gecikme Süresi |Saniye|Average|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|Triggerfirelatlik|Tetikleyici Tetikleme Gecikme Süresi |Saniye|Average|Tetiklenen iş akışı Tetikleyicileri gecikmesi.|Boyut yok|
|Triggerbaşarılı gecikme|Tetikleyici Başarı Gecikme Süresi |Saniye|Average|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerThrottledEvents|Tetikleyici Kısıtlanan Olaylar|Count|Toplam|İş akışı tetikleyicisi kısıtlanmış olay sayısı.|Boyut yok|
|Billableactionyürütmeler|Faturalanabilir Eylem Yürütmeleri|Count|Toplam|Faturalandırılan iş akışı eylemi yürütmelerinin sayısı.|Boyut yok|
|Billabletriggeryürütmeler|Faturalanabilir Tetikleyici Yürütmeleri|Count|Toplam|Faturalandırılan iş akışı tetikleyici yürütmelerinin sayısı.|Boyut yok|
|Totalbillableyürütmeleri|Toplam Faturalanabilir Yürütme Sayısı|Count|Toplam|Faturalandırılan iş akışı yürütmelerinin sayısı.|Boyut yok|
|BillingUsageNativeOperation|Yerel Işlem yürütmeleri için faturalandırma kullanımı|Count|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStandardConnector|Standart bağlayıcı yürütmeleri için faturalandırma kullanımı|Count|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Boyut yok|
|Billingusagestorampatüketim|Depolama tüketimi yürütmeleri için faturalama kullanımı|Count|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Boyut yok|
|BillingUsageNativeOperation|Yerel Işlem yürütmeleri için faturalandırma kullanımı|Count|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStandardConnector|Standart bağlayıcı yürütmeleri için faturalandırma kullanımı|Count|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Boyut yok|
|Billingusagestorampatüketim|Depolama tüketimi yürütmeleri için faturalama kullanımı|Count|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Boyut yok|

## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft. Logic/ıntegrationserviceortamortamları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Başlatılan Çalıştırmalar|Count|Toplam|Başlatılan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsCompleted|Tamamlanan Çalıştırmalar|Count|Toplam|Tamamlanan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsSucceeded|Başarılı Çalıştırmalar|Count|Toplam|İş akışı çalıştırma sayısı başarılı oldu.|Boyut yok|
|RunsFailed|Başarısız Çalıştırmalar|Count|Toplam|Başarısız iş akışı çalıştırma sayısı.|Boyut yok|
|RunsCancelled|Çalıştırma Iptal edildi|Count|Toplam|İptal edilen iş akışı çalıştırma sayısı.|Boyut yok|
|RunLatency|Çalıştırma Gecikmesi|Saniye|Average|Tamamlanan iş akışı çalıştırmalarının gecikmesi.|Boyut yok|
|Runbaşarılı gecikme süresi|Başarılı Çalıştırma Gecikmesi|Saniye|Average|Başarılı iş akışı çalıştırmalarının gecikmesi.|Boyut yok|
|RunThrottledEvents|Çalıştırılması Kısıtlanan Olaylar|Count|Toplam|İş akışı eylemi veya tetikleyici kısıtlanan olay sayısı.|Boyut yok|
|RunStartThrottledEvents|Çalıştırma Başlangıcı Kısıtlanan Olaylar|Count|Toplam|İş akışı çalıştırma, kısıtlanmış olay sayısı.|Boyut yok|
|RunFailurePercentage yüzdesi|Çalıştırma Hatası Yüzdesi|Percent|Toplam|İş akışı çalıştırmalarının yüzdesi başarısız oldu.|Boyut yok|
|ActionsStarted|Başlatılan Eylemler |Count|Toplam|Başlatılan iş akışı eylemi sayısı.|Boyut yok|
|ActionsCompleted|Tamamlanan Eylemler |Count|Toplam|Tamamlanan iş akışı eylemlerinin sayısı.|Boyut yok|
|ActionsSucceeded|Başarılı Eylemler |Count|Toplam|Başarılı iş akışı eylemi sayısı.|Boyut yok|
|ActionsFailed|Başarısız Eylemler |Count|Toplam|Başarısız iş akışı eylemi sayısı.|Boyut yok|
|ActionsSkipped|Atlanan Eylemler |Count|Toplam|Atlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionLatency|Eylem Gecikmesi |Saniye|Average|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|Actionbaşarılı gecikme süresi|Başarılı Eylem Gecikmesi |Saniye|Average|Başarılı iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionThrottledEvents|Eylemi Kısıtlanan Olaylar|Count|Toplam|İş akışı eylemi kısıtlanmış olay sayısı..|Boyut yok|
|TriggersStarted|Başlatılan Tetikleyiciler |Count|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersCompleted|Tamamlanan Tetikleyiciler |Count|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSucceeded|Başarılı Olan Tetikleyiciler |Count|Toplam|Başarılı iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersFailed|Başarısız Olan Tetikleyiciler |Count|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSkipped|Atlanan Tetikleyiciler|Count|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|Triggersharekete geçirildi|Tetiklenen Tetikleyiciler |Count|Toplam|Tetiklenen iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggerLatency|Tetikleyici Gecikme Süresi |Saniye|Average|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|Triggerfirelatlik|Tetikleyici Tetikleme Gecikme Süresi |Saniye|Average|Tetiklenen iş akışı Tetikleyicileri gecikmesi.|Boyut yok|
|Triggerbaşarılı gecikme|Tetikleyici Başarı Gecikme Süresi |Saniye|Average|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerThrottledEvents|Tetikleyici Kısıtlanan Olaylar|Count|Toplam|İş akışı tetikleyicisi kısıtlanmış olay sayısı.|Boyut yok|
|Integrationserviceenvironmentworkflowprocessorusage|Tümleştirme Hizmeti Ortamı için iş akışı Işlemcisi kullanımı|Percent|Average|Tümleştirme hizmeti ortamı için iş akışı işlemcisi kullanımı.|Boyut yok|
|Integrationserviceenvironmentworkflowmemoryusage|Tümleştirme Hizmeti Ortamı için iş akışı bellek kullanımı|Percent|Average|Tümleştirme hizmeti ortamı için iş akışı bellek kullanımı.|Boyut yok|
|Integrationserviceenvironmentconnectorprocessorusage|Tümleştirme Hizmeti Ortamı için bağlayıcı Işlemci kullanımı|Percent|Average|Tümleştirme hizmeti ortamı için bağlayıcı işlemci kullanımı.|Boyut yok|
|Integrationserviceenvironmentconnectormemoryusage|Tümleştirme Hizmeti Ortamı için bağlayıcı bellek kullanımı|Percent|Average|Tümleştirme hizmeti ortamı için bağlayıcı bellek kullanımı.|Boyut yok|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft. MachineLearningServices/çalışma alanları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Tamamlanan çalıştırmalar|Tamamlanan çalıştırmalar|Count|Toplam|Bu çalışma alanı için başarıyla tamamlanan çalıştırma sayısı|Senaryo|
|Başlatılan çalıştırmalar|Başlatılan çalıştırmalar|Count|Toplam|Bu çalışma alanı için başlatılan çalıştırma sayısı|Senaryo|
|Başarısız Çalışma Sayısı|Başarısız Çalışma Sayısı|Count|Toplam|Bu çalışma alanı için başarısız olan çalıştırma sayısı|Senaryo|

## <a name="microsoftmapsaccounts"></a>Microsoft. Maps/hesaplar

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Count|Count|API çağrısı sayısı|ApiCategory, ApiName, ResultType, ResponseCode|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Average|API 'lerin kullanılabilirliği|ApiCategory, ApiName|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageOtherLatency|Diğer ortalama gecikme süresi|MS/op|Average|İşlem başına milisaniye cinsinden ortalama diğer gecikme süresi (okuma veya yazma)|Boyut yok|
|AverageReadLatency|Ortalama okuma gecikmesi|MS/op|Average|İşlem başına milisaniye cinsinden ortalama okuma gecikmesi|Boyut yok|
|AverageTotalLatency|Ortalama toplam gecikme|MS/op|Average|İşlem başına milisaniye cinsinden ortalama toplam gecikme|Boyut yok|
|AverageWriteLatency|Ortalama yazma gecikmesi|MS/op|Average|İşlem başına milisaniye cinsinden ortalama yazma gecikmesi|Boyut yok|
|FilesystemOtherOps|Dosya sistemi diğer Ops|işlemleri|Average|Dosya sistemi diğer işlem sayısı (okuma veya yazma olmayan)|Boyut yok|
|FilesystemReadOps|Dosya sistemi okuma Ops|işlemleri|Average|Dosya sistemi okuma işlemi sayısı|Boyut yok|
|FilesystemTotalOps|Dosya sistemi toplam Ops|işlemleri|Average|Tüm dosya sistemi işlemlerinin toplamı|Boyut yok|
|FilesystemWriteOps|Dosya sistemi yazma Ops|işlemleri|Average|Dosya sistemi yazma işlemi sayısı|Boyut yok|
|Iobi Tesperotherops|Diğer Ops başına GÇ bayt sayısı|bayt/op|Average|Diğer işlemler başına gelen/giden bayt sayısı (okuma veya yazma olmayan)|Boyut yok|
|Iobi Tesperreadops|Okuma Ops başına GÇ bayt sayısı|bayt/op|Average|Okuma işlemi başına gelen/giden bayt sayısı|Boyut yok|
|Iobi Tespertotalops|Tüm işlemler genelinde OP başına GÇ bayt sayısı|bayt/op|Average|Tüm gelen/giden bayt işlemleri toplamı|Boyut yok|
|Iobi Tesperwriteops|Yazma Ops başına GÇ bayt sayısı|bayt/op|Average|Yazma işlemi başına gelen/giden bayt sayısı|Boyut yok|
|Diğer IOPS|Diğer IOPS|işlem/saniye|Average|Saniye başına diğer gelen/giden işlem|Boyut yok|
|Diğer aktarım hızı|Diğer aktarım hızı|Kadar|Average|Saniye başına megabayt cinsinden diğer üretilen iş (okuma veya yazma)|Boyut yok|
|ReadIops|IOPS 'yi oku|işlem/saniye|Average|Saniye başına okuma/kapatma işlemi|Boyut yok|
|Readüretilen Iş|Aktarım hızını oku|Kadar|Average|Saniye başına megabayt cinsinden okuma performansı|Boyut yok|
|Toplam IOPS|Toplam IOPS|işlem/saniye|Average|Saniye başına tüm gelen/giden işlemlerin toplamı|Boyut yok|
|Toplam aktarım hızı|Toplam işleme|Kadar|Average|Saniye başına megabayt cinsinden tüm aktarım hızı toplamı|Boyut yok|
|Birimayrıtedboyut|Birim ayırma boyutu|bayt|Average|Birimin ayrılan boyutu (gerçek kullanılan baytlar değil)|Boyut yok|
|Birimlogicalsize|Birim mantıksal boyutu|bayt|Average|Birimin mantıksal boyutu (kullanılan baytlar)|Boyut yok|
|VolumeSnapshotSize|Birim anlık görüntü boyutu|bayt|Average|Birimdeki tüm anlık görüntülerin boyutu|Boyut yok|
|Writeıops|IOPS yaz|işlem/saniye|Average|Saniyede yazma/çıkış işlemi sayısı|Boyut yok|
|Writeüretilen Iş|Yazma performansı|Kadar|Average|Saniye başına megabayt cinsinden yazma hızı|Boyut yok|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft. NetApp/netAppAccounts/Capacityhavuzları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Birim havuzu ayrılan boyut|bayt|Average|Havuzun ayrılan boyutu (gerçek kullanılan baytlar değil)|Boyut yok|
|VolumePoolAllocatedUsed|Ayrılan birim havuzu|bayt|Average|Havuzun ayrılan boyutu ayrılmış|Boyut yok|
|VolumePoolTotalLogicalSize|Birim havuzu toplam mantıksal boyut|bayt|Average|Havuza ait tüm birimlerin mantıksal boyutunun toplamı|Boyut yok|
|VolumePoolTotalSnapshotSize|Birim havuzu toplam anlık görüntü boyutu|bayt|Average|Havuzdaki tüm anlık görüntülerin toplamı|Boyut yok|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft. Network/NetworkInterfaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesSentRate|Gönderilen bayt|Count|Toplam|Ağ arabiriminin gönderdiği bayt sayısı|Boyut yok|
|BytesReceivedRate|Alınan bayt|Count|Toplam|Ağ arabiriminin aldığı bayt sayısı|Boyut yok|
|PacketsSentRate|Gönderilen paketler|Count|Toplam|Ağ arabiriminin gönderdiği paket sayısı|Boyut yok|
|PacketsReceivedRate|Alınan paketler|Count|Toplam|Ağ arabiriminin aldığı paket sayısı|Boyut yok|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VipAvailability|Veri yolu kullanılabilirliği|Count|Average|Süre başına ortalama Load Balancer veri yolu kullanılabilirliği|Frontendıaddress, FrontendPort|
|DipAvailability|Durum araştırma durumu|Count|Average|Süre başına ortalama Load Balancer sistem durumu araştırma durumu|ProtocolType, BackendPort, Frontendıbaddress, FrontendPort, Backendıdıaddress|
|ByteCount|Bayt sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam bayt sayısı|Frontendıaaddress, FrontendPort, Yön|
|PacketCount|Paket sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam paket sayısı|Frontendıaaddress, FrontendPort, Yön|
|SYNCount|SYN sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam SYN paketi sayısı|Frontendıaaddress, FrontendPort, Yön|
|SnatConnectionCount|SNAT bağlantı sayısı|Count|Toplam|Zaman aralığı içinde oluşturulan toplam yeni SNAT bağlantısı sayısı|Frontendıbaddress, Backendıo adresi, ConnectionState|
|AllocatedSnatPorts|Ayrılan SNAT bağlantı noktaları (Önizleme)|Count|Toplam|Zaman aralığı içinde ayrılan toplam SNAT bağlantı noktası sayısı|Frontendıbaddress, Backendıaddress, ProtocolType|
|UsedSnatPorts|Kullanılan SNAT bağlantı noktaları (Önizleme)|Count|Toplam|Zaman aralığı içinde kullanılan SNAT bağlantı noktalarının toplam sayısı|Frontendıbaddress, Backendıaddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft. Network/dnszones

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryVolume|Sorgu hacmi|Count|Toplam|Bir DNS bölgesi için sunulan sorgu sayısı|Boyut yok|
|Kayıt kümesi sayısı|Kayıt kümesi sayısı|Count|Maksimum|Bir DNS bölgesindeki kayıt kümesi sayısı|Boyut yok|
|Recordsetkapaıyutilileştirme|Kayıt kümesi kapasite kullanımı|Percent|Maksimum|Bir DNS bölgesi tarafından kullanılan kayıt kümesi kapasitesinin yüzdesi|Boyut yok|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PacketsInDDoS|Gelen paketler DDoS|CountPerSecond|Maksimum|Gelen paketler DDoS|Boyut yok|
|PacketsDroppedDDoS|DDoS bırakılan gelen paketler|CountPerSecond|Maksimum|DDoS bırakılan gelen paketler|Boyut yok|
|PacketsForwardedDDoS|DDoS iletilen gelen paketler|CountPerSecond|Maksimum|DDoS iletilen gelen paketler|Boyut yok|
|TCPPacketsInDDoS|Gelen TCP paketleri DDoS|CountPerSecond|Maksimum|Gelen TCP paketleri DDoS|Boyut yok|
|TCPPacketsDroppedDDoS|Gelen TCP paketleri bırakıldı DDoS|CountPerSecond|Maksimum|Gelen TCP paketleri bırakıldı DDoS|Boyut yok|
|TCPPacketsForwardedDDoS|DDoS iletilen gelen TCP paketleri|CountPerSecond|Maksimum|DDoS iletilen gelen TCP paketleri|Boyut yok|
|UDPPacketsInDDoS|Gelen UDP paketleri DDoS|CountPerSecond|Maksimum|Gelen UDP paketleri DDoS|Boyut yok|
|UDPPacketsDroppedDDoS|Gelen UDP paketleri bırakıldı|CountPerSecond|Maksimum|Gelen UDP paketleri bırakıldı|Boyut yok|
|UDPPacketsForwardedDDoS|DDoS iletilen gelen UDP paketleri|CountPerSecond|Maksimum|DDoS iletilen gelen UDP paketleri|Boyut yok|
|BytesInDDoS|Gelen bayt sayısı DDoS|BytesPerSecond|Maksimum|Gelen bayt sayısı DDoS|Boyut yok|
|BytesDroppedDDoS|Gelen bayt bırakıldı DDoS|BytesPerSecond|Maksimum|Gelen bayt bırakıldı DDoS|Boyut yok|
|BytesForwardedDDoS|İletilen gelen bayt sayısı|BytesPerSecond|Maksimum|İletilen gelen bayt sayısı|Boyut yok|
|TCPBytesInDDoS|Gelen TCP bayt DDoS|BytesPerSecond|Maksimum|Gelen TCP bayt DDoS|Boyut yok|
|TCPBytesDroppedDDoS|Gelen TCP bayt bırakılan DDoS|BytesPerSecond|Maksimum|Gelen TCP bayt bırakılan DDoS|Boyut yok|
|TCPBytesForwardedDDoS|İletilen gelen TCP baytları|BytesPerSecond|Maksimum|İletilen gelen TCP baytları|Boyut yok|
|UDPBytesInDDoS|Gelen UDP bayt sayısı DDoS|BytesPerSecond|Maksimum|Gelen UDP bayt sayısı DDoS|Boyut yok|
|UDPBytesDroppedDDoS|Gelen UDP bayt bırakılan DDoS|BytesPerSecond|Maksimum|Gelen UDP bayt bırakılan DDoS|Boyut yok|
|UDPBytesForwardedDDoS|İletilen gelen UDP baytları|BytesPerSecond|Maksimum|İletilen gelen UDP baytları|Boyut yok|
|IfUnderDDoSAttack|DDoS saldırısı altında|Count|Maksimum|DDoS saldırısı altında|Boyut yok|
|DDO, Ggertcppackets|DDoS azaltma tetiklemeye yönelik gelen TCP paketleri|CountPerSecond|Maksimum|DDoS azaltma tetiklemeye yönelik gelen TCP paketleri|Boyut yok|
|DDO, Ggerudppaketleri|DDoS risk azaltma tetiklenecek gelen UDP paketleri|CountPerSecond|Maksimum|DDoS risk azaltma tetiklenecek gelen UDP paketleri|Boyut yok|
|Ddobir Ggersynpackets|DDoS azaltma tetiklenmesi için gelen SYN paketleri|CountPerSecond|Maksimum|DDoS azaltma tetiklenmesi için gelen SYN paketleri|Boyut yok|
|VipAvailability|Veri yolu kullanılabilirliği|Count|Average|Süre başına ortalama IP adresi kullanılabilirliği|Port|
|ByteCount|Bayt sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam bayt sayısı|Bağlantı noktası, Yön|
|PacketCount|Paket sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam paket sayısı|Bağlantı noktası, Yön|
|SynCount|SYN sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam SYN paketi sayısı|Bağlantı noktası, Yön|

## <a name="microsoftnetworkazurefirewalls"></a>Microsoft. Network/azurefirewalls

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ApplicationRuleHit|Uygulama kuralları isabet sayısı|Count|Toplam|Uygulama kurallarının isabet sayısı|Durum, neden, protokol|
|NetworkRuleHit|Ağ kuralları isabet sayısı|Count|Toplam|Ağ kurallarının isabet sayısı|Durum, neden, protokol|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Aktarım hızı|Aktarım hızı|BytesPerSecond|Toplam|Application Gateway saniyede hizmet verilen bayt sayısı|Boyut yok|
|Unhealthyıhostcount|Sağlıksız konak sayısı|Count|Average|Sağlıklı olmayan arka uç Konakları sayısı|BackendSettingsPool|
|Healthyıhostcount|Sağlıklı konak sayısı|Count|Average|Sağlıklı arka uç Konakları sayısı|BackendSettingsPool|
|Toplam Istek sayısı|Toplam İstek Sayısı|Count|Toplam|Application Gateway tarafından sunulan başarılı istek sayısı|BackendSettingsPool|
|FailedRequests|Başarısız İstekler|Count|Toplam|Application Gateway tarafından sunulan başarısız istek sayısı|BackendSettingsPool|
|ResponseStatus|Yanıt durumu|Count|Toplam|Application Gateway tarafından döndürülen http yanıt durumu|HttpStatusGroup|
|CurrentConnections|Geçerli bağlantılar|Count|Toplam|Application Gateway ile kurulan geçerli bağlantı sayısı|Boyut yok|
|CapacityUnits|Geçerli kapasite birimleri|Count|Average|Tüketilen Kapasite birimleri|Boyut yok|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft. Network/Virtualnetworkgateway 'ler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageBandwidth|Ağ Geçidi S2S bant genişliği|BytesPerSecond|Average|Bayt/saniye cinsinden bir ağ geçidinin ortalama siteden siteye bant genişliği|Boyut yok|
|P2SBandwidth|Ağ Geçidi P2S bant genişliği|BytesPerSecond|Average|Bayt/saniye cinsinden bir ağ geçidinin ortalama Noktadan siteye bant genişliği|Boyut yok|
|P2SConnectionCount|P2S bağlantı sayısı|Count|Maksimum|Bir ağ geçidinin Noktadan siteye bağlantı sayısı|Protocol|
|TunnelAverageBandwidth|Tünel bant genişliği|BytesPerSecond|Average|Bir tünelin bayt/saniye cinsinden ortalama bant genişliği|ConnectionName, Remoteıp|
|TunnelEgressBytes|Tünel çıkış baytları|Bayt|Toplam|Bir tünelin giden baytları|ConnectionName, Remoteıp|
|TunnelIngressBytes|Tünel giriş baytları|Bayt|Toplam|Bir tünelin gelen baytları|ConnectionName, Remoteıp|
|TunnelEgressPackets|Tünel çıkış paketleri|Count|Toplam|Bir tünelin giden paket sayısı|ConnectionName, Remoteıp|
|TunnelIngressPackets|Tünel giriş paketleri|Count|Toplam|Bir tünelin gelen paket sayısı|ConnectionName, Remoteıp|
|TunnelEgressPacketDropTSMismatch|Tünel çıkış uyumsuzluğu paket bırakma|Count|Toplam|Bir tünelin trafik seçiciyle eşleşmeyen giden paket bırakma sayısı|ConnectionName, Remoteıp|
|TunnelIngressPacketDropTSMismatch|Tünel girişi uyumsuz paket bırakma|Count|Toplam|Trafik seçicideki gelen paket bırakma sayısı bir tünelin eşleşmemesi|ConnectionName, Remoteıp|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft. Network/Expressroutedevreleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Saniye başına Azure 'da BITS giriş|PeeringType|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Saniye başına Azure 'da BITS yumursan|PeeringType|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft. Network/Expressroutedevreleri/eşayarları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Saniye başına Azure 'da BITS giriş|Boyut yok|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Saniye başına Azure 'da BITS yumursan|Boyut yok|

## <a name="microsoftnetworkconnections"></a>Microsoft. Network/Connections

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Saniye başına Azure 'da BITS giriş|Boyut yok|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Saniye başına Azure 'da BITS yumursan|Boyut yok|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QpsByEndpoint|Bitiş noktasına göre sorgular döndürüldü|Count|Toplam|Verilen zaman çerçevesinde Traffic Manager uç noktasının döndürüldüğü sayı|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Uç nokta tarafından uç nokta durumu|Count|Maksimum|1 bir uç noktanın araştırma durumu "etkin" ise, 0 yoksa.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft. Network/networkWatchers/Connectionmonitörleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Yoklama başarısız oldu|Percent|Average|Bağlantı İzleme Araştırmaları yüzdesi başarısız oldu|Boyut yok|
|AverageRoundtripMs|Ort. Gidiş dönüş süresi (MS)|Mayacak|Average|Kaynak ve hedef arasında gönderilen bağlantı izleme araştırmaları için Ortalama ağ gidiş dönüş süresi (MS)|Boyut yok|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft. Network/frontkapaklı

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RequestCount|İstek Sayısı|Count|Toplam|HTTP/S proxy tarafından hizmet verilen istemci isteklerinin sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|İstek boyutu|Bayt|Toplam|İstemcilerden HTTP/S proxy 'sine istek olarak gönderilen bayt sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|Yanıt boyutu|Yanıt boyutu|Bayt|Toplam|HTTP/S proxy 'sinden istemcilere yanıt olarak gönderilen baytların sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Arka uç Istek sayısı|Count|Toplam|HTTP/S proxy 'sinden arka uçlara gönderilen isteklerin sayısı|HttpStatus, HttpStatusGroup, arka uç|
|BackendRequestLatency|Arka uç Isteği gecikmesi|Mayacak|Average|HTTP/s proxy, arka ucun son yanıt baytını almayana kadar isteğin HTTP/S proxy tarafından arka uca gönderildiği süre|Arka uç|
|TotalLatency|Toplam gecikme süresi|Mayacak|Average|İstemci, http/s proxy 'sinden gelen son yanıt baytını kabul edene kadar, istemci isteğinin HTTP/S proxy tarafından alındığı zamana göre hesaplanır|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Arka uç sistem durumu yüzdesi|Percent|Average|HTTP/S proxy 'sinden arka uçlara başarılı sistem durumu araştırmalarının yüzdesi|Arka uç, BackendPool|
|WebApplicationFirewallRequestCount|Web uygulaması güvenlik duvarı Istek sayısı|Count|Toplam|Web uygulaması güvenlik duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName, RuleName, eylem|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kayıt. tümü|Kayıt İşlemleri|Count|Toplam|Tüm başarılı kayıt işlemlerinin sayısı (güncelleştirme sorguları ve silmeleri oluşturma). |Boyut yok|
|kayıt. oluştur|Kayıt Oluşturma İşlemleri|Count|Toplam|Tüm başarılı kayıt oluşturma işlemlerinin sayısı.|Boyut yok|
|kayıt. güncelleştirme|Kayıt Güncelleme İşlemleri|Count|Toplam|Tüm başarılı kayıt güncelleştirmelerinin sayısı.|Boyut yok|
|kayıt. Get|Kayıt Okuma İşlemleri|Count|Toplam|Tüm başarılı kayıt sorgularının sayısı.|Boyut yok|
|kayıt. Sil|Kayıt Silme İşlemleri|Count|Toplam|Tüm başarılı kayıt silme işlemlerinin sayısı.|Boyut yok|
|gelen|Gelen İletiler|Count|Toplam|Başarılı bir gönderme API çağrısı sayısı. |Boyut yok|
|gelen. zamanlandı|Zamanlanan Anında İletme Bildirimleri Gönderildi|Count|Toplam|Zamanlanan anında Iletme bildirimleri Iptal edildi|Boyut yok|
|gelen. zamanlandı. iptal|Zamanlanan anında Iletme bildirimleri Iptal edildi|Count|Toplam|Zamanlanan anında Iletme bildirimleri Iptal edildi|Boyut yok|
|scheduled.pending|Bekleyen Zamanlanmış Bildirimler|Count|Toplam|Bekleyen Zamanlanmış Bildirimler|Boyut yok|
|yükleme. tümü|Yükleme Yönetimi İşlemleri|Count|Toplam|Yükleme Yönetimi İşlemleri|Boyut yok|
|yükleme. Get|Yükleme İşlemlerini Al|Count|Toplam|Yükleme İşlemlerini Al|Boyut yok|
|yükleme. upsert|Yükleme İşlemleri Oluştur veya Güncelleştir|Count|Toplam|Yükleme İşlemleri Oluştur veya Güncelleştir|Boyut yok|
|yükleme. Patch|Yükleme İşlemlerine Düzeltme Eki Uygula|Count|Toplam|Yükleme İşlemlerine Düzeltme Eki Uygula|Boyut yok|
|yükleme. Delete|Yükleme İşlemlerini Sil|Count|Toplam|Yükleme İşlemlerini Sil|Boyut yok|
|giden. allpns. Success|Başarılı bildirimler|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. allpns. ınvalidpayload|Yük Hataları|Count|Toplam|PNS hatalı yük hatası döndürdüğünden başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.allpns.pnserror|Harici Bildirim Sistemi Hataları|Count|Toplam|PNS ile iletişim kurulurken bir sorun oluştuğundan başarısız olan gönderim sayısı (kimlik doğrulama sorunları hariç).|Boyut yok|
|outgoing.allpns.channelerror|Kanal Hataları|Count|Toplam|Kanal geçersiz olduğu için doğru uygulamayla ilişkili olmayan veya geçerliliği aşıldığı için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.allpns.badorexpiredchannel|Hatalı veya Süresi Dolmuş Kanal Hataları|Count|Toplam|Kayıttaki kanal/belirteç/RegistrationId 'nin geçerliliği aşıldığı veya geçersiz olduğu için başarısız olan gönderim sayısı.|Boyut yok|
|giden. WNS. Success|WNS Başarılı Bildirimler|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. WNS. ınvalidcredentials|WNS yetkilendirme hataları (geçersiz kimlik bilgileri)|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı. (Windows Live kimlik bilgilerini tanımaz).|Boyut yok|
|giden. WNS. badchannel|WNS Geçersiz Kanal Hatası|Count|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (WNS durumu: 404 bulunamadı).|Boyut yok|
|outgoing.wns.expiredchannel|WNS Süresi Dolan Kanal Hatası|Count|Toplam|ChannelURI 'nin geçerliliği aşıldığı için başarısız olan gönderim sayısı (WNS durumu: 410 gitti).|Boyut yok|
|giden. WNS. kısıtlanıyor|WNS Kısıtlanan Bildirimler|Count|Toplam|WNS bu uygulamayı azaltdığı için başarısız olan gönderim sayısı (WNS durumu: 406 kabul edilemez).|Boyut yok|
|giden. WNS. tokenproviderunreachable|WNS yetkilendirme hataları (ulaşılamıyor)|Count|Toplam|Windows Live ulaşılamıyor.|Boyut yok|
|giden. WNS. ınvalidtoken|WNS yetkilendirme hataları (geçersiz belirteç)|Count|Toplam|WNS için belirtilen belirteç geçerli değil (WNS durumu: 401 Yetkisiz).|Boyut yok|
|giden. WNS. yanlışlıkla gtoken|WNS yetkilendirme hataları (yanlış belirteç)|Count|Toplam|WNS öğesine sunulan belirteç geçerli, ancak başka bir uygulama için (WNS durumu: 403 yasaklanmış). Kayıttaki Channelurı başka bir uygulamayla ilişkiliyse bu durum oluşabilir. İstemci uygulamasının kimlik bilgileri Bildirim Hub 'ında olan aynı uygulamayla ilişkilendirildiğinden emin olun.|Boyut yok|
|giden. WNS. ınvalidnotificationformat|WNS Geçersiz Bildirim Biçimi|Count|Toplam|Bildirimin biçimi geçersiz (WNS durumu: 400). WNS 'nin tüm geçersiz yükleri reddetmediğini unutmayın.|Boyut yok|
|giden. WNS. ınvalidnotificationsize|WNS Geçersiz Bildirim Boyutu Hatası|Count|Toplam|Bildirim yükü çok büyük (WNS durumu: 413).|Boyut yok|
|giden. WNS. channelkısıtlanıyor|WNS Kanal Kısıtlandı|Count|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (WNS yanıt üst bilgisi: X-WNS-NotificationStatus: Channelkısıtlanıyor).|Boyut yok|
|outgoing.wns.channeldisconnected|WNS Kanal Bağlantısı Kesildi|Count|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (WNS yanıt üst bilgisi: X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Boyut yok|
|giden. WNS. bırakıldı|WNS Bırakılan Bildirimler|Count|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (X-WNS-NotificationStatus: bırakıldı ancak X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Boyut yok|
|outgoing.wns.pnserror|WNS Hataları|Count|Toplam|Bildirim, WNS ile iletişim kuran hatalar nedeniyle teslim edilemiyor.|Boyut yok|
|outgoing.wns.authenticationerror|WNS Kimlik Doğrulaması Hataları|Count|Toplam|Windows Live geçersiz kimlik bilgileri veya yanlış belirteçle iletişim kurarken oluşan hatalar nedeniyle bildirim teslim edilemiyor.|Boyut yok|
|giden. APNs. Success|APNS Başarılı Bildirimler|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. APNs. ınvalidcredentials|APNS Yetkilendirme Hataları|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Boyut yok|
|giden. APNs. badchannel|APNS Geçersiz Kanal Hatası|Count|Toplam|Belirteç geçersiz olduğu için başarısız olan gönderim sayısı (APNS ikili protokol durum kodu: 8. APNS HTTP protokolü durum kodu: "BadDeviceToken" ile 400).|Boyut yok|
|outgoing.apns.expiredchannel|APNS Süresi Dolan Kanal Hatası|Count|Toplam|APNS geri bildirim kanalı tarafından geçersiz kılınan belirteç sayısı.|Boyut yok|
|giden. APNs. ınvalidnotificationsize|APNS Geçersiz Bildirim Boyutu Hatası|Count|Toplam|Yük çok büyük olduğu için başarısız olan gönderim sayısı (APNS ikili protokol durum kodu: 7).|Boyut yok|
|giden. APNs. pnserror|APNS Hataları|Count|Toplam|APNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|giden. gcm. Success|GCM Başarılı Bildirimler|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. gcm. ınvalidcredentials|GCM yetkilendirme hataları (geçersiz kimlik bilgileri)|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Boyut yok|
|giden. gcm. badchannel|GCM Geçersiz Kanal Hatası|Count|Toplam|Kayıttaki RegistrationId tanınmadığı için başarısız olan gönderim sayısı (GCM sonucu: Geçersiz kayıt).|Boyut yok|
|giden. gcm. expiredchannel|GCM Süresi Dolan Kanal Hatası|Count|Toplam|Kayıttaki RegistrationId 'nin zaman aşımına uğradığından başarısız olan gönderim sayısı (GCM sonucu: NotRegistered).|Boyut yok|
|giden. gcm. kısıtlanıyor|GCM Kısıtlanan Bildirimler|Count|Toplam|GCM 'nin bu uygulamayı kısıtazaltdığı için başarısız olan gönderim sayısı (GCM durum kodu: 501-599 veya sonuç: kullanılamıyor).|Boyut yok|
|giden. gcm. ınvalidnotificationformat|GCM Geçersiz Bildirim Biçimi|Count|Toplam|Yük doğru biçimlendirilmediği için başarısız olan gönderim sayısı (GCM sonucu: Invaliddatakey veya ınvalidttl).|Boyut yok|
|giden. gcm. ınvalidnotificationsize|GCM Geçersiz Bildirim Boyutu Hatası|Count|Toplam|Yük çok büyük olduğu için başarısız olan gönderim sayısı (GCM sonucu: Messagetoobıg).|Boyut yok|
|outgoing.gcm.wrongchannel|GCM Yanlış Kanal Hatası|Count|Toplam|Kayıttaki RegistrationId geçerli uygulamayla ilişkili olmadığından başarısız olan gönderim sayısı (GCM sonucu: Invalidpackagename).|Boyut yok|
|outgoing.gcm.pnserror|GCM Hataları|Count|Toplam|GCM ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|giden. gcm. authenticationerror|GCM Kimlik Doğrulaması Hataları|Count|Toplam|PNS 'nin girilen kimlik bilgilerini kabul etmediğinden başarısız olan gönderim sayısı, kimlik bilgilerinin engellenmesi veya SenderId uygulamada doğru yapılandırılmamış (GCM sonucu: Hatalı Matchedsenderıd).|Boyut yok|
|giden. MPNS. Success|MPNS Başarılı Bildirimler|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. MPNS. ınvalidcredentials|MPNS Geçersiz Kimlik Bilgileri|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Boyut yok|
|giden. MPNS. badchannel|MPNS Geçersiz Kanal Hatası|Count|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (MPNS durumu: 404 bulunamadı).|Boyut yok|
|giden. MPNS. kısıtlanıyor|MPNS Kısıtlanan Bildirimler|Count|Toplam|MPNS bu uygulamayı azaltdığı için başarısız olan gönderim sayısı (WNS MPNS: 406 kabul edilemez).|Boyut yok|
|giden. MPNS. ınvalidnotificationformat|MPNS Geçersiz Bildirim Biçimi|Count|Toplam|Bildirimin yükü çok büyük olduğu için başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.mpns.channeldisconnected|MPNS Kanalın Bağlantısı Kesildi|Count|Toplam|Kayıttaki ChannelURI bağlantısı kesildiğinden başarısız olan gönderim sayısı (MPNS durumu: 412 bulunamadı).|Boyut yok|
|giden. MPNS. bırakıldı|MPNS Bırakılan Bildirimler|Count|Toplam|MPNS tarafından bırakılan gönderim sayısı (MPNS yanıt üst bilgisi: X-NotificationStatus: QueueFull veya gizlendi).|Boyut yok|
|outgoing.mpns.pnserror|MPNS Hataları|Count|Toplam|MPNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|outgoing.mpns.authenticationerror|MPNS Kimlik Doğrulaması Hataları|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Boyut yok|
|notificationhub. gönderimleri|Tüm Giden Bildirimler|Count|Toplam|Bildirim Hub 'ının tüm giden bildirimleri|Boyut yok|
|gelen. ALL. Requests|Tüm Gelen İstekler|Count|Toplam|Bir Bildirim Hub 'ı için toplam gelen istek sayısı|Boyut yok|
|gelen. tüm. failedistekleri|Tüm Gelen Başarısız İstekler|Count|Toplam|Bir Bildirim Hub 'ı için toplam gelen başarısız istek sayısı|Boyut yok|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft. Operationalınsights/çalışma alanları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Average_% boş ınomdes|% Boş ınomdes|Count|Average|Average_% boş ınomdes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% boş alan|% Boş alan|Count|Average|Average_% boş alan|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılan ınomdes|% Kullanılan ınomdes|Count|Average|Average_% kullanılan ınomdes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|% Average_ kullanılan alan|% Kullanılan alan|Count|Average|% Average_ kullanılan alan|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk okunan bayt/sn|Disk Okuma Bayt/sn|Count|Average|Average_Disk okunan bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk Okuma/sn|Disk Okuma/sn|Count|Average|Average_Disk Okuma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk aktarımı/sn|Disk aktarımı/sn|Count|Average|Average_Disk aktarımı/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk yazma bayt/sn|Disk Yazma Bayt/sn|Count|Average|Average_Disk yazma bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk yazma/sn|Disk Yazma/sn|Count|Average|Average_Disk yazma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Free megabayt|Boş megabayt|Count|Average|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Logical disk bayt/sn|Mantıksal Disk Bayt/sn|Count|Average|Average_Logical disk bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılabilir bellek|% Kullanılabilir bellek|Count|Average|Average_% kullanılabilir bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılabilir takas alanı|% Kullanılabilir takas alanı|Count|Average|Average_% kullanılabilir takas alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılan bellek|% Kullanılan bellek|Count|Average|Average_% kullanılan bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılan değiştirme alanı|% Kullanılan takas alanı|Count|Average|Average_% kullanılan değiştirme alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt belleği|Kullanılabilir MBayt belleği|Count|Average|Average_Available MBayt belleği|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt takas|Kullanılabilir MBayt takas|Count|Average|Average_Available MBayt takas|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Page Okuma/sn|Sayfa Okuma/sn|Count|Average|Average_Page Okuma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Page yazma/sn|Sayfa yazma/sn|Count|Average|Average_Page yazma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pages/sn|Sayfa/sn|Count|Average|Average_Pages/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used MBayt değiştirme alanı|Kullanılan MBayt değiştirme alanı|Count|Average|Average_Used MBayt değiştirme alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used belleği MBayt|Kullanılan bellek MBayt|Count|Average|Average_Used belleği MBayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total aktarılan baytlar|Aktarılan toplam bayt|Count|Average|Average_Total aktarılan baytlar|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total bayt alındı|Alınan toplam bayt sayısı|Count|Average|Average_Total bayt alındı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total bayt|Toplam Bayt|Count|Average|Average_Total bayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total gönderilen paketler|Aktarılan toplam paket sayısı|Count|Average|Average_Total gönderilen paketler|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Alınan Average_Total paketleri|Alınan toplam paket sayısı|Count|Average|Alınan Average_Total paketleri|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total RX hataları|Toplam RX hatası sayısı|Count|Average|Average_Total RX hataları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total TX hataları|Toplam TX hatası sayısı|Count|Average|Average_Total TX hataları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total çakışmaları|Toplam çakışma sayısı|Count|Average|Average_Total çakışmaları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Okuma|Ort. Disk sn/Okuma|Count|Average|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Aktarım|Ort. Disk sn/Aktarım|Count|Average|Average_Avg. Disk sn/Aktarım|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/yazma|Ort. Disk sn/yazma|Count|Average|Average_Avg. Disk sn/yazma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Physical disk bayt/sn|Fiziksel disk bayt/sn|Count|Average|Average_Physical disk bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pct ayrıcalıklı süre|Ayrıcalıklı zaman yüzdesi|Count|Average|Average_Pct ayrıcalıklı süre|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pct Kullanıcı saati|Kullanım süresi yüzdesi|Count|Average|Average_Pct Kullanıcı saati|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used bellek Kbayt|Kullanılan bellek Kbayt|Count|Average|Average_Used bellek Kbayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Virtual paylaşılan bellek|Sanal paylaşılan bellek|Count|Average|Average_Virtual paylaşılan bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% DPC Zamanı|% DPC Zamanı|Count|Average|Average_% DPC Zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% boşta kalma süresi|% Boşta kalma süresi|Count|Average|Average_% boşta kalma süresi|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kesme zamanı|Kesme zamanı yüzdesi|Count|Average|Average_% kesme zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% GÇ bekleme süresi|% GÇ bekleme süresi|Count|Average|Average_% GÇ bekleme süresi|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Iyi zaman|% İyi zaman|Count|Average|Average_% Iyi zaman|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% ayrıcalıklı zaman|% Ayrıcalıklı zaman|Count|Average|Average_% ayrıcalıklı zaman|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Işlemci zamanı|% İşlemci zamanı|Count|Average|Average_% Işlemci zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Kullanıcı Zamanı|% Kullanıcı Zamanı|Count|Average|Average_% Kullanıcı Zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Free fiziksel belleği|Boş fiziksel bellek|Count|Average|Average_Free fiziksel belleği|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Disk belleği dosyalarında Average_Free alanı|Disk belleği dosyalarındaki boş alan|Count|Average|Disk belleği dosyalarında Average_Free alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Free sanal belleği|Boş sanal bellek|Count|Average|Average_Free sanal belleği|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Processes|İşlemler|Count|Average|Average_Processes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Disk belleği dosyalarında depolanan Average_Size|Disk belleği dosyalarında depolanan boyut|Count|Average|Disk belleği dosyalarında depolanan Average_Size|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Uptime|Çalışma Süresi|Count|Average|Average_Uptime|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Users|Kullanıcılar|Count|Average|Average_Users|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Okuma|Ort. Disk sn/Okuma|Count|Average|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/yazma|Ort. Disk sn/yazma|Count|Average|Average_Avg. Disk sn/yazma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Current disk kuyruğu uzunluğu|Geçerli disk sırası uzunluğu|Count|Average|Average_Current disk kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk Okuma/sn|Disk Okuma/sn|Count|Average|Average_Disk Okuma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk aktarımı/sn|Disk aktarımı/sn|Count|Average|Average_Disk aktarımı/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk yazma/sn|Disk Yazma/sn|Count|Average|Average_Disk yazma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Free megabayt|Boş megabayt|Count|Average|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% boş alan|% Boş alan|Count|Average|Average_% boş alan|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt|Kullanılabilir MBayt|Count|Average|Average_Available MBayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|% Average_ kaydedilmiş bayt kullanımda|Kullanılan kaydedilmiş bayt yüzdesi|Count|Average|% Average_ kaydedilmiş bayt kullanımda|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Alınan Average_Bytes/sn|Alınan Bayt/sn|Count|Average|Alınan Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Gönderilen Average_Bytes/sn|Gönderilen bayt/sn|Count|Average|Gönderilen Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Toplam Average_Bytes/sn|Toplam bayt/sn|Count|Average|Toplam Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Işlemci zamanı|% İşlemci zamanı|Count|Average|Average_% Işlemci zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Processor kuyruğu uzunluğu|İşlemci kuyruğu uzunluğu|Count|Average|Average_Processor kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Sinyal|Sinyal|Count|Toplam|Sinyal|Computer, OSType, Version, SourceComputerId|
|Güncelleştirme|Güncelleştirme|Count|Average|Güncelleştirme|Bilgisayar, ürün, sınıflandırma, UpdateState, Isteğe bağlı, onaylanan|
|Olay|Olay|Count|Average|Olay|Kaynak, olay günlüğü, bilgisayar, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft. Powerbiadanmış/kapasiteler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryDuration|Sorgu süresi|Milisaniye|Average|Son aralıktaki DAX sorgu süresi|Boyut yok|
|QueryPoolJobQueueLength|Akışları Sorgu havuzu iş kuyruğu uzunluğu|Count|Average|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|Boyut yok|
|qpu_high_utilization_metric|QPU yüksek kullanım|Count|Toplam|Son dakikada QPU yüksek kullanımı, yüksek QPU kullanımı Için 1, aksi durumda 0|Boyut yok|
|memory_metric|Bellek|Bayt|Average|Bellek. A1 için 0-3 GB, a2 için 0-5 GB, a3 için 0-10 GB, A4 için 0-25 GB, A6 için 0-50 GB ve A6 için 0-100 GB|Boyut yok|
|memory_thrashing_metric|Bellek dağılması|Percent|Average|Ortalama bellek miktarı.|Boyut yok|

## <a name="microsoftrelaynamespaces"></a>Microsoft. Relay/Namespace

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ListenerConnections-başarılı|ListenerConnections-başarılı|Count|Toplam|Microsoft. Relay için başarılı bir ListenerConnections.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Count|Toplam|Microsoft. Relay için ListenerConnections 'da ClientError.|EntityName|
|ListenerConnections-Sunucuhatası|ListenerConnections-Sunucuhatası|Count|Toplam|Microsoft. Relay için ListenerConnections 'da ServerError.|EntityName|
|SenderConnections-başarılı|SenderConnections-başarılı|Count|Toplam|Microsoft. Relay için başarılı SenderConnections.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Count|Toplam|Microsoft. Relay için SenderConnections 'ta ClientError.|EntityName|
|SenderConnections-Sunucuhatası|SenderConnections-Sunucuhatası|Count|Toplam|Microsoft. Relay için SenderConnections 'ta ServerError.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Count|Toplam|Microsoft. Relay için toplam ListenerConnections.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Count|Toplam|Microsoft. Relay için toplam SenderConnections isteği.|EntityName|
|ActiveConnections|ActiveConnections|Count|Toplam|Microsoft. Relay için toplam ActiveConnections.|EntityName|
|ActiveListeners|ActiveListeners|Count|Toplam|Microsoft. Relay için toplam ActiveListeners.|EntityName|
|BytesTransferred|BytesTransferred|Count|Toplam|Microsoft. Relay için toplam BytesTransferred.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Count|Toplam|Microsoft. Relay için toplam Listenerdisconnect.|EntityName|
|SenderDisconnects|SenderDisconnects|Count|Toplam|Microsoft. Relay için toplam Senderdisconnect.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SearchLatency|Arama gecikmesi|Saniye|Average|Arama hizmeti için Ortalama arama gecikmesi|Boyut yok|
|SearchQueriesPerSecond|Saniye başına arama sorguları|CountPerSecond|Average|Arama hizmeti için saniye başına arama sorgusu|Boyut yok|
|Tüm Searchqueriespercentage 'ı kısıtlar|Kısıtlanmış arama sorguları yüzdesi|Percent|Average|Arama hizmeti için kısıtlanan arama sorgularının yüzdesi|Boyut yok|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı Istekler (Önizleme)|Count|Toplam|Bir ad alanı için toplam başarılı istek (Önizleme)|EntityName|
|ServerErrors|Sunucu hataları. (Önizleme)|Count|Toplam|Microsoft. ServiceBus için sunucu hataları. (Önizleme)|EntityName|
|UserErrors|Kullanıcı hataları. (Önizleme)|Count|Toplam|Microsoft. ServiceBus için Kullanıcı hataları. (Önizleme)|EntityName|
|ThrottledRequests|Kısıtlanmış Istekler. (Önizleme)|Count|Toplam|Microsoft. ServiceBus için kısıtlanan Istekler. (Önizleme)|EntityName|
|Incomingrequests|Gelen Istekler (Önizleme)|Count|Toplam|Microsoft. ServiceBus için gelen Istekler. (Önizleme)|EntityName|
|Incomingmessages|Gelen Iletiler (Önizleme)|Count|Toplam|Microsoft. ServiceBus için gelen Iletiler. (Önizleme)|EntityName|
|OutgoingMessages|Giden Iletiler (Önizleme)|Count|Toplam|Microsoft. ServiceBus için giden Iletiler. (Önizleme)|EntityName|
|ActiveConnections|ActiveConnections (Önizleme)|Count|Toplam|Microsoft. ServiceBus için toplam etkin bağlantı sayısı. (Önizleme)|Boyut yok|
|Size|Boyut (Önizleme)|Bayt|Average|Bir kuyruğun/konunun bayt cinsinden boyutu. (Önizleme)|EntityName|
|İletiler|Kuyruktaki/konudaki iletilerin sayısı. (Önizleme)|Count|Average|Kuyruktaki/konudaki iletilerin sayısı. (Önizleme)|EntityName|
|ActiveMessages|Bir kuyruktaki/konudaki etkin ileti sayısı. (Önizleme)|Count|Average|Bir kuyruktaki/konudaki etkin ileti sayısı. (Önizleme)|EntityName|
|DeadletteredMessages|Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı. (Önizleme)|Count|Average|Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı. (Önizleme)|EntityName|
|ScheduledMessages|Bir kuyruktaki/konudaki zamanlanan ileti sayısı. (Önizleme)|Count|Average|Bir kuyruktaki/konudaki zamanlanan ileti sayısı. (Önizleme)|EntityName|
|CPUXNS|Ad alanı başına CPU kullanımı|Percent|Maksimum|Service Bus Premium ad alanı CPU kullanım ölçümü|Boyut yok|
|WSXNS|Ad alanı başına bellek boyutu kullanımı|Percent|Maksimum|Service Bus Premium ad alanı bellek kullanım ölçümü|Boyut yok|

## <a name="microsoftservicefabricmeshapplications"></a>Microsoft. Servicefabrickafes/uygulamaları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Ayrılatedcpu|Ayrılatedcpu|Count|Average|Miliçekirdekte bu kapsayıcıya ayrılan CPU|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Ayrılan bellek|Ayrılan bellek|Bayt|Average|Bu kapsayıcıya MB cinsinden ayrılan bellek|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualCpu|ActualCpu|Count|Average|Miliçekirdekte gerçek CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualMemory|ActualMemory|Bayt|Average|MB cinsinden gerçek bellek kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Cpukullanımı|Cpukullanımı|Percent|Average|Ayrılan CPU yüzdesi olarak bu kapsayıcı için CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Memoryutilileştirme|Memoryutilileştirme|Percent|Average|Ayrılan CPU yüzdesi olarak bu kapsayıcı için CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ApplicationStatus|ApplicationStatus|Count|Average|Service Fabric kafes uygulamasının durumu|ApplicationName, durum|
|ServiceStatus|ServiceStatus|Count|Average|Service Fabric kafes uygulamasındaki bir hizmetin sistem durumu|ApplicationName, durum, HizmetAdı|
|Servicereperepstatus|Servicereperepstatus|Count|Average|Service Fabric kafes uygulamasındaki bir hizmet çoğaltmasının sistem durumu|ApplicationName, Status, HizmetAdı, ServiceReplicaName|
|ContainerStatus|ContainerStatus|Count|Average|Service Fabric kafes uygulamasındaki kapsayıcının durumu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName, durum|
|RestartCount|RestartCount|Count|Average|Service Fabric kafes uygulamasındaki bir kapsayıcının yeniden başlatma sayısı|ApplicationName, Status, HizmetAdı, ServiceReplicaName, CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft. SignalRService/SignalR

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ConnectionCount|Bağlantı sayısı|Count|Maksimum|Kullanıcı bağlantısı miktarı.|Uç Nokta|
|MessageCount|İleti sayısı|Count|Toplam|Toplam ileti miktarı.|Boyut yok|
|Inboundtraffic|Gelen Trafik|Bayt|Toplam|Hizmetin gelen trafiği|Boyut yok|
|OutboundTraffic|Giden Trafik|Bayt|Toplam|Hizmetin giden trafiği|Boyut yok|
|UserErrors|Kullanıcı hataları|Percent|Maksimum|Kullanıcı hatalarının yüzdesi|Boyut yok|
|SystemErrors|Sistem hataları|Percent|Maksimum|Sistem hatalarının yüzdesi|Boyut yok|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|physical_data_read_percent|Veri G/Ç yüzdesi|Percent|Average|Veri G/Ç yüzdesi|Boyut yok|
|log_write_percent|Günlük G/Ç yüzdesi|Percent|Average|Günlük GÇ yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|
|dtu_consumption_percent|DTU yüzdesi|Percent|Average|DTU yüzdesi. DTU tabanlı veritabanları için geçerlidir.|Boyut yok|
|depolama|Kullanılan veri alanı|Bayt|Maksimum|Toplam veritabanı boyutu. Veri ambarlarında geçerli değildir.|Boyut yok|
|connection_successful|Başarılı bağlantılar|Count|Toplam|Başarılı bağlantılar|Boyut yok|
|connection_failed|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|blocked_by_firewall|Güvenlik duvarı tarafından engellendi|Count|Toplam|Güvenlik duvarı tarafından engellendi|Boyut yok|
|Çözül|Kilitlenmeler|Count|Toplam|Çık. Veri ambarlarında geçerli değildir.|Boyut yok|
|storage_percent|Kullanılan veri alanı yüzdesi|Percent|Maksimum|Veritabanı boyutu yüzdesi. Veri ambarları veya hiper ölçekli veritabanları için geçerli değildir.|Boyut yok|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Percent|Average|Bellek içi OLTP depolaması yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|
|workers_percent|Çalışan yüzdesi|Percent|Average|Çalışan yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|
|sessions_percent|Oturum yüzdesi|Percent|Average|Oturum yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|
|dtu_limit|DTU sınırı|Count|Average|DTU sınırı. DTU tabanlı veritabanları için geçerlidir.|Boyut yok|
|dtu_used|Kullanılan DTU|Count|Average|DTU kullanıldı. DTU tabanlı veritabanları için geçerlidir.|Boyut yok|
|cpu_limit|CPU sınırı|Count|Average|CPU sınırı. Sanal çekirdek tabanlı veritabanları için geçerlidir.|Boyut yok|
|cpu_used|Kullanılan CPU|Count|Average|Kullanılan CPU. Sanal çekirdek tabanlı veritabanları için geçerlidir.|Boyut yok|
|dwu_limit|DWU sınırı|Count|Maksimum|DWU sınırı. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|dwu_consumption_percent|DWU yüzdesi|Percent|Maksimum|DWU yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|dwu_used|Kullanılan DWU|Count|Maksimum|DWU kullanıldı. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|dw_cpu_percent|DW düğüm düzeyi CPU yüzdesi|Percent|Average|DW düğüm düzeyi CPU yüzdesi|DwLogicalNodeId|
|dw_physical_data_read_percent|DW düğüm düzeyi veri GÇ yüzdesi|Percent|Average|DW düğüm düzeyi veri GÇ yüzdesi|DwLogicalNodeId|
|cache_hit_percent|İsabetli önbellek okuması yüzdesi|Percent|Maksimum|İsabetli önbellek okuması yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|cache_used_percent|Önbellek kullanılan yüzde|Percent|Maksimum|Önbellek kullanım yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|local_tempdb_usage_percent|Yerel tempdb yüzdesi|Percent|Average|Yerel tempdb yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|app_cpu_billed|Fiyatlandırılan uygulama CPU'su|Count|Toplam|Uygulama CPU 'SU faturalandırılır. Sunucusuz veritabanları için geçerlidir.|Boyut yok|
|app_cpu_percent|Uygulama CPU yüzdesi|Percent|Average|Uygulama CPU yüzdesi. Sunucusuz veritabanları için geçerlidir.|Boyut yok|
|app_memory_percent|Kullanılan uygulama belleği yüzdesi|Percent|Average|Kullanılan uygulama belleği yüzdesi. Sunucusuz veritabanları için geçerlidir.|Boyut yok|
|allocated_data_storage|Ayrılmış veri alanı|Bayt|Average|Ayrılan veri alanı. Veri ambarlarında geçerli değildir.|Boyut yok|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Percent|Average|CPU yüzdesi|Boyut yok|
|physical_data_read_percent|Veri G/Ç yüzdesi|Percent|Average|Veri G/Ç yüzdesi|Boyut yok|
|log_write_percent|Günlük G/Ç yüzdesi|Percent|Average|Günlük G/Ç yüzdesi|Boyut yok|
|dtu_consumption_percent|DTU yüzdesi|Percent|Average|DTU yüzdesi. DTU tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|storage_percent|Kullanılan veri alanı yüzdesi||Percent|Average|Depolama yüzdesi|Boyut yok|
|workers_percent|Çalışan yüzdesi|Percent|Average|Çalışan yüzdesi|Boyut yok|
|sessions_percent|Oturum yüzdesi|Percent|Average|Oturum yüzdesi|Boyut yok|
|eDTU_limit|eDTU sınırı|Count|Average|eDTU sınırı. DTU tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|storage_limit|Maksimum veri boyutu|Bayt|Average|Depolama sınırı|Boyut yok|
|eDTU_used|eDTU kullanıldı|Count|Average|eDTU kullanıldı. DTU tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|storage_used|Kullanılan veri alanı|Bayt|Average|Kullanılan depolama alanı|Boyut yok|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Percent|Average|Bellek içi OLTP depolama yüzdesi|Boyut yok|
|cpu_limit|CPU sınırı|Count|Average|CPU sınırı. Sanal çekirdek tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|cpu_used|Kullanılan CPU|Count|Average|Kullanılan CPU. Sanal çekirdek tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|allocated_data_storage|Ayrılmış veri alanı|Bayt|Average|Ayrılmış veri alanı|Boyut yok|
|allocated_data_storage_percent|Ayrılan veri alanı yüzdesi|Percent|Maksimum|Ayrılan veri alanı yüzdesi|Boyut yok|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft. SQL/ManagedInstances

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|virtual_core_count|Sanal çekirdek sayısı|Count|Average|Sanal çekirdek sayısı|Boyut yok|
|avg_cpu_percent|Ortalama CPU yüzdesi|Percent|Average|Ortalama CPU yüzdesi|Boyut yok|
|reserved_storage_mb|Ayrılan depolama alanı|Count|Average|Ayrılan depolama alanı|Boyut yok|
|storage_space_used_mb|Kullanılan depolama alanı|Count|Average|Kullanılan depolama alanı|Boyut yok|
|io_requests|GÇ istek sayısı|Count|Average|GÇ istek sayısı|Boyut yok|
|io_bytes_read|Okunan GÇ baytları|Bayt|Average|Okunan GÇ baytları|Boyut yok|
|io_bytes_written|Yazılan GÇ baytları|Bayt|Average|Yazılan GÇ baytları|Boyut yok|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|UsedCapacity|Kullanılan kapasite|Bayt|Ortalama|Hesabın kullanılan kapasitesi|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobCapacity|Blob Kapasitesi|Bayt|Average|Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı.|BlobType, katman|
|BLOB sayısı|Blob Sayısı|Sayı|Toplam|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType|       |BLOB sayısı|Blob Sayısı|Count|Average|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType, katman|
|ContainerCount|Blob Kapsayıcı Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki kapsayıcı sayısı.|Boyut yok|
|Dizin kapasitesi|Dizin Kapasitesi|Bayt|Average|Bayt cinsinden ADLS 2. (hiyerarşik) dizin tarafından kullanılan depolama miktarı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft. Storage/storageAccounts/fileServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Dosya kapasitesi|Dosya Kapasitesi|Bayt|Average|Bayt olarak depolama hesabının dosya hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|FileCount|Dosya Sayısı|Count|Average|Depolama hesabının dosya hizmetindeki dosya sayısı.|Boyut yok|
|Dosya ShareCount|Dosya Paylaşım Sayısı|Count|Average|Depolama hesabının dosya hizmetindeki dosya paylaşımlarının sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueueCapacity|Kuyruk Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Kuyruk hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|QueueCount|Kuyruk Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra sayısı.|Boyut yok|
|QueueMessageCount|Kuyruk İleti Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra iletilerinin yaklaşık sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TableCapacity|Tablo Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Tablo hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|TableCount|Tablo Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo sayısı.|Boyut yok|
|TableEntityCount|Tablo Varlık Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo varlıklarının sayısı.|Boyut yok|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı başarılı ve başarısız isteklerin yanı sıra hataları üretilen istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer AverageE2ELatency'de belirtilen ağ gecikmesini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Kullanılabilirlik|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik TotalBillableRequests değeri ve beklenmeyen hata üreten dahil ilgili istek sayısına göre bölme göre hesaplanır. Azaltılmış kullanılabilirlik ve depolama hizmetine veya belirtilen API işlemi için beklenmeyen tüm hatalar sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragesyncstoragesyncservices"></a>Microsoft. storagessync/storageSyncServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Eşitleme oturumu sonucu|Count|Average|Sunucu uç noktası, bulut uç noktası ile eşitleme oturumunu başarıyla tamamladığında 1 değerini günlüğe kaydeden ölçüm|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Dosyalar Eşitlendi|Count|Toplam|Eşitlenen dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Dosyalar eşitlenmiyor|Count|Toplam|Eşitleme başarısız olan dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Eşitlenen baytlar|Bayt|Toplam|Eşitleme oturumları için aktarılan toplam dosya boyutu|SyncGroupName, ServerEndpointName, SyncDirection|
|Storagesyncserversinyal|Sunucu Çevrimiçi Durumu|Count|Maksimum|Kayıtlı sunucu, bulut uç noktasıyla bir sinyal başarıyla kaydetilişinde 1 değerini günlüğe kaydeden ölçüm|ServerName|
|Storagesyncyeniden hesaplama Liototalsizebytes|Bulut katmanlama geri çağırma|Bayt|Toplam|Sunucu tarafından geri çekilen toplam veri boyutu|ServerName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft. StreamAnalytics/streamingjobs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Resourcekullanım|SU Kullanım Yüzdesi|Percent|Maksimum|SU Kullanım Yüzdesi|LogicalName, PartitionID|
|Inputevents|Giriş Olayları|Count|Toplam|Giriş Olayları|LogicalName, PartitionID|
|Inputeventbytes|Giriş Olayı Bayt Sayısı|Bayt|Toplam|Giriş Olayı Bayt Sayısı|LogicalName, PartitionID|
|LateInputEvents|Geç Giriş Olayları|Count|Toplam|Geç Giriş Olayları|LogicalName, PartitionID|
|OutputEvents|Çıkış Olayları|Count|Toplam|Çıkış Olayları|LogicalName, PartitionID|
|Dönüştürme hataları|Veri Dönüştürme Hataları|Count|Toplam|Veri Dönüştürme Hataları|LogicalName, PartitionID|
|Hatalar|Çalışma Zamanı Hataları|Count|Toplam|Çalışma Zamanı Hataları|LogicalName, PartitionID|
|Droppedorayarlantedevents|Sıralama dışındaki Olaylar|Count|Toplam|Sıralama dışındaki Olaylar|LogicalName, PartitionID|
|AMLCalloutRequests|İşlev İstekleri|Count|Toplam|İşlev İstekleri|LogicalName, PartitionID|
|AMLCalloutFailedRequests|Başarısız İşlev İstekleri|Count|Toplam|Başarısız İşlev İstekleri|LogicalName, PartitionID|
|AMLCalloutInputEvents|İşlev Olayları|Count|Toplam|İşlev Olayları|LogicalName, PartitionID|
|DeserializationError|Giriş Serileştirme Kaldırma Hataları|Count|Toplam|Giriş Serileştirme Kaldırma Hataları|LogicalName, PartitionID|
|EarlyInputEvents|Erken Giriş Olayları|Count|Toplam|Erken Giriş Olayları|LogicalName, PartitionID|
|Outputsulu Markdelayseconds|Eşik Gecikmesi|Saniye|Maksimum|Eşik Gecikmesi|LogicalName, PartitionID|
|Inputeventssourcesbackgünlüğe kaydediliyor|Biriktirme Listesindeki Giriş Olayları|Count|Maksimum|Biriktirme Listesindeki Giriş Olayları|LogicalName, PartitionID|
|Inputeventssourcespersecond|Alınan Giriş Kaynakları|Count|Toplam|Alınan Giriş Kaynakları|LogicalName, PartitionID|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft. Timeseriesınsights/ortamlar

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Inressreceived Iletileri|Giriş alınan Iletiler|Count|Toplam|Tüm Olay Hub 'ından veya IoT Hub olay kaynaklarından okunan ileti sayısı|Boyut yok|
|Inressreceiveınvalidmessages|Giriş geçersiz Iletiler aldı|Count|Toplam|Tüm Olay Hub 'ından veya IoT Hub olay kaynaklarından okunan geçersiz ileti sayısı|Boyut yok|
|Alınan baytlar|Alınan bayt sayısı|Bayt|Toplam|Tüm olay kaynaklarından okunan bayt sayısı|Boyut yok|
|IngressStoredBytes|Giriş depolanan baytlar|Bayt|Toplam|Başarılı bir şekilde işlenen ve sorgu için kullanılabilir olayların toplam boyutu|Boyut yok|
|Inressstoo Vents|Giriş saklı olayları|Count|Toplam|Sorgu için başarıyla işlenen ve kullanılabilir düzleştirilmiş olay sayısı|Boyut yok|
|Inressreceived Iletizamangecikmesi|Alınan Ileti zaman gecikmesi|Saniye|Maksimum|İleti olay kaynağında sıraya alındığı zaman ve giriş sırasında işlendiği zaman arasındaki fark|Boyut yok|
|Inressreceived Iletisayaçgecikmesi|Alınan Ileti sayısı gecikmesi|Count|Average|Olay kaynak bölümünde en son sıraya alınan iletinin sıra numarası ve giriş dosyasında işlenen ileti sırası arasındaki fark|Boyut yok|
|WarmStorageMaxProperties|Isınma depolama en yüksek özellikleri|Count|Maksimum|S1/S2 SKU 'SU için ortam tarafından izin verilen en fazla özellik sayısı ve PAYG SKU 'SU için ısınma tarafından izin verilen en fazla özellik sayısı|Boyut yok|
|WarmStorageUsedProperties|Sıcak depolama kullanılan özellikler |Count|Maksimum|S1/S2 SKU 'SU için ortam tarafından kullanılan özellik sayısı ve PAYG SKU 'SU için sıcak mağaza tarafından kullanılan özellik sayısı|Boyut yok|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft. Timeseriesınsights/ortamlar/EventSources

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Inressreceived Iletileri|Giriş alınan Iletiler|Count|Toplam|Olay kaynağından okunan ileti sayısı|Boyut yok|
|Inressreceiveınvalidmessages|Giriş geçersiz Iletiler aldı|Count|Toplam|Olay kaynağından okunan geçersiz ileti sayısı|Boyut yok|
|Alınan baytlar|Alınan bayt sayısı|Bayt|Toplam|Olay kaynağından okunan bayt sayısı|Boyut yok|
|IngressStoredBytes|Giriş depolanan baytlar|Bayt|Toplam|Başarılı bir şekilde işlenen ve sorgu için kullanılabilir olayların toplam boyutu|Boyut yok|
|Inressstoo Vents|Giriş saklı olayları|Count|Toplam|Sorgu için başarıyla işlenen ve kullanılabilir düzleştirilmiş olay sayısı|Boyut yok|
|Inressreceived Iletizamangecikmesi|Alınan Ileti zaman gecikmesi|Saniye|Maksimum|İleti olay kaynağında sıraya alındığı zaman ve giriş sırasında işlendiği zaman arasındaki fark|Boyut yok|
|Inressreceived Iletisayaçgecikmesi|Alınan Ileti sayısı gecikmesi|Count|Average|Olay kaynak bölümünde en son sıraya alınan iletinin sıra numarası ve giriş dosyasında işlenen ileti sırası arasındaki fark|Boyut yok|
|WarmStorageMaxProperties|Isınma depolama en yüksek özellikleri|Count|Maksimum|S1/S2 SKU 'SU için ortam tarafından izin verilen en fazla özellik sayısı ve PAYG SKU 'SU için ısınma tarafından izin verilen en fazla özellik sayısı|Boyut yok|
|WarmStorageUsedProperties|Sıcak depolama kullanılan özellikler |Count|Maksimum|S1/S2 SKU 'SU için ortam tarafından kullanılan özellik sayısı ve PAYG SKU 'SU için sıcak mağaza tarafından kullanılan özellik sayısı|Boyut yok|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft. Vmwarechoparlör basit/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk okuma bayt/sn|BytesPerSecond|Average|Örnek süre içinde okuma işlemleri nedeniyle ortalama disk aktarım hızı.|Boyut yok|
|DiskWriteBytesPerSecond|Disk yazma bayt/sn|BytesPerSecond|Average|Örnek süre içinde yazma işlemleri nedeniyle ortalama disk aktarım hızı.|Boyut yok|
|Diskten Okunan Bayt|Diskten Okunan Bayt|Bayt|Toplam|Örnek süre boyunca okuma işlemleri nedeniyle toplam disk aktarım hızı.|Boyut yok|
|Diske Yazılan Bayt|Diske Yazılan Bayt|Bayt|Toplam|Örnek süre içinde yazma işlemleri nedeniyle toplam disk aktarım hızı.|Boyut yok|
|DiskReadOperations|Disk okuma Işlemleri|Count|Toplam|Önceki örnek dönemdeki GÇ okuma işlemlerinin sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Boyut yok|
|DiskWriteOperations|Disk yazma Işlemleri|Count|Toplam|Önceki örnek dönemdeki GÇ Yazma işlemlerinin sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Boyut yok|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|CountPerSecond|Average|Önceki örnek dönemdeki ortalama GÇ okuma işlemi sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Boyut yok|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|CountPerSecond|Average|Önceki örnek dönemdeki ortalama GÇ yazma işlemi sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Boyut yok|
|DiskReadLatency|Disk okuma gecikmesi|Milisaniye|Average|Toplam okuma gecikmesi. Cihaz ve çekirdek okuma gecikme sürelerinin toplamı.|Boyut yok|
|DiskWriteLatency|Disk yazma gecikmesi|Milisaniye|Average|Toplam yazma gecikmesi. Cihaz ve çekirdek yazma gecikme sürelerinin toplamı.|Boyut yok|
|Networkınbytespersecond|Bayt/sn cinsinden ağ|BytesPerSecond|Average|Alınan trafik için Ortalama ağ performansı.|Boyut yok|
|NetworkOutBytesPerSecond|Ağ çıkış bayt/sn|BytesPerSecond|Average|Aktarılan trafik için Ortalama ağ aktarım hızı.|Boyut yok|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Alınan trafik için toplam ağ aktarım hızı.|Boyut yok|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Aktarılan trafik için toplam ağ aktarım hızı.|Boyut yok|
|MemoryUsed|Kullanılan bellek|Bayt|Average|VM tarafından kullanılan makine belleği miktarı.|Boyut yok|
|Memoryverildi|Verilen bellek|Bayt|Average|VM 'ye konak tarafından verilen bellek miktarı. Bir kez dokunulmadan ve VMkernel belleğe ihtiyaç duyuncaya kadar bellek verilebilene kadar bu konağa bellek verilmez.|Boyut yok|
|MemoryActive|Bellek etkin|Bayt|Average|Son küçük zaman penceresinde VM tarafından kullanılan bellek miktarı. Bu, VM 'nin şu anda ihtiyacı olan bellek miktarını "doğru" sayısıdır. Ek olarak kullanılmayan bellek, konuğun performansına hiçbir etkilenmeden dışarı veya ballooned olabilir.|Boyut yok|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Average|CPU kullanımı. Bu değer, sistemdeki tüm işlemci çekirdeğini temsil eden% 100 ile raporlanır. Örnek olarak, dört çekirdekli bir sistemin% 50 ' ünü kullanan 2 yönlü bir VM tamamen iki çekirdek kullanmaktır.|Boyut yok|
|Yüztagecpuready|CPU için hazırlanma yüzdesi|Milisaniye|Toplam|Hazır süre, CPU (ler) in geçmiş güncelleştirme aralığında kullanılabilir olmasını bekleyen zamanın harcadığı süredir.|Boyut yok|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Cpuyüzdesi|CPU Yüzdesi|Percent|Average|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek Yüzdesi|Percent|Average|Bellek Yüzdesi|Örnek|
|DiskQueueLength|Disk Kuyruğu Uzunluğu|Count|Average|Disk Kuyruğu Uzunluğu|Örnek|
|HttpQueueLength|Http Kuyruk Uzunluğu|Count|Average|Http Kuyruk Uzunluğu|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft. Web/Sites (işlevler hariç)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuTime|CPU Süresi|Saniye|Toplam|CPU Süresi|Örnek|
|İstekler|İstekler|Count|Toplam|İstekler|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http101|Http 101|Count|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Count|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Count|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Count|Toplam|Http 401|Örnek|
|Http403|Http 403|Count|Toplam|Http 403|Örnek|
|Http404|Http 404|Count|Toplam|Http 404|Örnek|
|Http406|Http 406|Count|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Count|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Count|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Average|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Average|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Ortalama Yanıt Süresi|Saniye|Average|Ortalama Yanıt Süresi|Örnek|
|AppConnections|Bağlantılar|Count|Average|Bağlantılar|Örnek|
|Tanıtıcılar|Tanıtıcı Sayısı|Count|Average|Tanıtıcı Sayısı|Örnek|
|İş Parçacıkları|İş Parçacığı Sayısı|Count|Average|İş Parçacığı Sayısı|Örnek|
|PrivateBytes|Özel Baytlar|Bayt|Average|Özel Baytlar|Örnek|
|Ioreadbytespersecond|GÇ Okunan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / Saniye|Örnek|
|Iowritebytespersecond|GÇ Yazılan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Yazılan Bayt / Saniye|Örnek|
|Iootherbytespersecond|GÇ Diğer Bayt / Saniye|BytesPerSecond|Toplam|GÇ Diğer Bayt / Saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Okuma İşlemi / Saniye|Örnek|
|Iowriteoperationspersecond|GÇ Yazma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Yazma İşlemi / Saniye|Örnek|
|Iootheroperationspersecond|GÇ Diğer İşlemler / Saniye|BytesPerSecond|Toplam|GÇ Diğer İşlemler / Saniye|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Count|Average|Uygulama Kuyruğundaki İstekler|Örnek|
|CurrentAssemblies|Geçerli Derlemeler|Count|Average|Geçerli Derlemeler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanları|Count|Average|Toplam Uygulama Etki Alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş Toplam Uygulama Etki Alanları|Count|Average|Yüklenmemiş Toplam Uygulama Etki Alanları|Örnek|
|Gen0Collections|Gen 0 Atık Toplamaları|Count|Toplam|Gen 0 Atık Toplamaları|Örnek|
|Gen1Collections|Gen 1 Atık Toplamaları|Count|Toplam|Gen 1 Atık Toplamaları|Örnek|
|Gen2Collections|Gen 2 Atık Toplamaları|Count|Toplam|Gen 2 Atık Toplamaları|Örnek|

## <a name="microsoftwebsites-functions"></a>Microsoft. Web/Sites (işlevler)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http5xx|Http Sunucu Hataları|Count|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Average|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Average|Ortalama bellek çalışma kümesi|Örnek|
|FunctionExecutionUnits|İşlev Yürütme Birimleri|MB/milisaniye|Toplam|[İşlev yürütme birimleri](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Örnek|
|Işlev yürütme sayısı|İşlev Yürütme Sayısı|Count|Toplam|İşlev Yürütme Sayısı|Örnek|
|PrivateBytes|Özel Baytlar|Bayt|Average|Özel Baytlar|Örnek|
|Ioreadbytespersecond|GÇ Okunan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / Saniye|Örnek|
|Iowritebytespersecond|GÇ Yazılan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Yazılan Bayt / Saniye|Örnek|
|Iootherbytespersecond|GÇ Diğer Bayt / Saniye|BytesPerSecond|Toplam|GÇ Diğer Bayt / Saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Okuma İşlemi / Saniye|Örnek|
|Iowriteoperationspersecond|GÇ Yazma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Yazma İşlemi / Saniye|Örnek|
|Iootheroperationspersecond|GÇ Diğer İşlemler / Saniye|BytesPerSecond|Toplam|GÇ Diğer İşlemler / Saniye|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Count|Average|Uygulama Kuyruğundaki İstekler|Örnek|
|CurrentAssemblies|Geçerli Derlemeler|Count|Average|Geçerli Derlemeler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanları|Count|Average|Toplam Uygulama Etki Alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş Toplam Uygulama Etki Alanları|Count|Average|Yüklenmemiş Toplam Uygulama Etki Alanları|Örnek|
|Gen0Collections|Gen 0 Atık Toplamaları|Count|Toplam|Gen 0 Atık Toplamaları|Örnek|
|Gen1Collections|Gen 1 Atık Toplamaları|Count|Toplam|Gen 1 Atık Toplamaları|Örnek|
|Gen2Collections|Gen 2 Atık Toplamaları|Count|Toplam|Gen 2 Atık Toplamaları|Örnek|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuTime|CPU Süresi|Saniye|Toplam|CPU Süresi|Örnek|
|İstekler|İstekler|Count|Toplam|İstekler|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http101|Http 101|Count|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Count|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Count|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Count|Toplam|Http 401|Örnek|
|Http403|Http 403|Count|Toplam|Http 403|Örnek|
|Http404|Http 404|Count|Toplam|Http 404|Örnek|
|Http406|Http 406|Count|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Count|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Count|Toplam|Http Sunucu Hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Average|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Average|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Ortalama Yanıt Süresi|Saniye|Average|Ortalama Yanıt Süresi|Örnek|
|FunctionExecutionUnits|İşlev Yürütme Birimleri|Count|Toplam|İşlev Yürütme Birimleri|Örnek|
|Işlev yürütme sayısı|İşlev Yürütme Sayısı|Count|Toplam|İşlev Yürütme Sayısı|Örnek|
|AppConnections|Bağlantılar|Count|Average|Bağlantılar|Örnek|
|Tanıtıcılar|Tanıtıcı Sayısı|Count|Average|Tanıtıcı Sayısı|Örnek|
|İş Parçacıkları|İş Parçacığı Sayısı|Count|Average|İş Parçacığı Sayısı|Örnek|
|PrivateBytes|Özel Baytlar|Bayt|Average|Özel Baytlar|Örnek|
|Ioreadbytespersecond|GÇ Okunan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Okunan Bayt / Saniye|Örnek|
|Iowritebytespersecond|GÇ Yazılan Bayt / Saniye|BytesPerSecond|Toplam|GÇ Yazılan Bayt / Saniye|Örnek|
|Iootherbytespersecond|GÇ Diğer Bayt / Saniye|BytesPerSecond|Toplam|GÇ Diğer Bayt / Saniye|Örnek|
|IoReadOperationsPerSecond|GÇ Okuma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Okuma İşlemi / Saniye|Örnek|
|Iowriteoperationspersecond|GÇ Yazma İşlemi / Saniye|BytesPerSecond|Toplam|GÇ Yazma İşlemi / Saniye|Örnek|
|Iootheroperationspersecond|GÇ Diğer İşlemler / Saniye|BytesPerSecond|Toplam|GÇ Diğer İşlemler / Saniye|Örnek|
|RequestsInApplicationQueue|Uygulama Kuyruğundaki İstekler|Count|Average|Uygulama Kuyruğundaki İstekler|Örnek|
|CurrentAssemblies|Geçerli Derlemeler|Count|Average|Geçerli Derlemeler|Örnek|
|TotalAppDomains|Toplam Uygulama Etki Alanları|Count|Average|Toplam Uygulama Etki Alanları|Örnek|
|TotalAppDomainsUnloaded|Yüklenmemiş Toplam Uygulama Etki Alanları|Count|Average|Yüklenmemiş Toplam Uygulama Etki Alanları|Örnek|
|Gen0Collections|Gen 0 Atık Toplamaları|Count|Toplam|Gen 0 Atık Toplamaları|Örnek|
|Gen1Collections|Gen 1 Atık Toplamaları|Count|Toplam|Gen 1 Atık Toplamaları|Örnek|
|Gen2Collections|Gen 2 Atık Toplamaları|Count|Toplam|Gen 2 Atık Toplamaları|Örnek|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft. Web/hostingEnvironments/multiRolePools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstekler|İstekler|Count|Toplam|İstekler|Örnek|
|BytesReceived|Data Girişi|Bayt|Toplam|Data Girişi|Örnek|
|BytesSent|Giden Veriler|Bayt|Toplam|Giden Veriler|Örnek|
|Http101|Http 101|Count|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Count|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Count|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Count|Toplam|Http 401|Örnek|
|Http403|Http 403|Count|Toplam|Http 403|Örnek|
|Http404|Http 404|Count|Toplam|Http 404|Örnek|
|Http406|Http 406|Count|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Count|Toplam|Http 4xx|Örnek|
|Http5xx|Http Sunucu Hataları|Count|Toplam|Http Sunucu Hataları|Örnek|
|AverageResponseTime|Ortalama Yanıt Süresi|Saniye|Average|Ortalama Yanıt Süresi|Örnek|
|Cpuyüzdesi|CPU Yüzdesi|Percent|Average|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek Yüzdesi|Percent|Average|Bellek Yüzdesi|Örnek|
|DiskQueueLength|Disk Kuyruğu Uzunluğu|Count|Average|Disk Kuyruğu Uzunluğu|Örnek|
|HttpQueueLength|Http Kuyruk Uzunluğu|Count|Average|Http Kuyruk Uzunluğu|Örnek|
|ActiveRequests|Etkin İstekler|Count|Toplam|Etkin İstekler|Örnek|
|Toplam ön uçlar|Toplam Ön Uç Sayısı|Count|Average|Toplam Ön Uç Sayısı|Boyut yok|
|Smallappserviceplanınstances|Kısa App Service Planı Çalışanları|Count|Average|Kısa App Service Planı Çalışanları|Boyut yok|
|Mediumappserviceplanınstances|Orta App Service Planı Çalışanları|Count|Average|Orta App Service Planı Çalışanları|Boyut yok|
|Largeappserviceplanınstances|Uzun App Service Planı Çalışanları|Count|Average|Uzun App Service Planı Çalışanları|Boyut yok|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft. Web/hostingEnvironments/workerPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|WorkersTotal|Toplam Çalışanlar|Count|Average|Toplam Çalışanlar|Boyut yok|
|WorkersAvailable|Kullanılabilir Çalışanlar|Count|Average|Kullanılabilir Çalışanlar|Boyut yok|
|WorkersUsed|Kullanılan Çalışanlar|Count|Average|Kullanılan Çalışanlar|Boyut yok|
|Cpuyüzdesi|CPU Yüzdesi|Percent|Average|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek Yüzdesi|Percent|Average|Bellek Yüzdesi|Örnek|

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Izleyici 'de ölçümler hakkında bilgi edinin](data-platform.md)
* [Ölçümler üzerinde uyarı oluşturma](alerts-overview.md)
* [Ölçümleri depolama, Olay Hub 'ına veya Log Analytics Dışa aktarma](diagnostic-logs-overview.md)
