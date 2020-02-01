---
title: Azure Izleyici kaynak türüne göre desteklenen ölçümler
description: Azure Izleyici ile her kaynak türü için kullanılabilen ölçümlerin listesi.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 12/18/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 0210317ef74433b740feb043a1cc4f1f9bc2ef57
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76901160"
---
# <a name="supported-metrics-with-azure-monitor"></a>Azure Izleyici ile desteklenen ölçümler

Azure Izleyici, ölçümlerle etkileşimde bulunmak için, portalda grafik oluşturma, REST API aracılığıyla erişme veya PowerShell veya CLı kullanarak sorgulama gibi çeşitli yollar sağlar. Aşağıda, Azure Izleyici 'nin ölçüm işlem hattı ile Şu anda kullanılabilir olan tüm ölçümlerin tamamı listelenmiştir. Diğer ölçümler portalda veya eski API 'Ler kullanılarak kullanılabilir. Aşağıdaki listede yalnızca birleştirilmiş Azure Izleyici ölçüm işlem hattı kullanılarak kullanılabilen ölçümler bulunur. Bu ölçümleri sorgulamak ve bunlara erişmek için lütfen [2018-01-01 API-sürümünü](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) kullanın

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
> Tanılama ayarları aracılığıyla dışarı aktarılabilir platform ölçümleri listesi için, [Bu makaleye](metrics-supported-export-diagnostic-settings.md)bakın.




## <a name="microsoftanalysisservicesservers"></a>Microsoft. AnalysisServices/sunucuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|qpu_metric|QPU|Sayı|Ortalama|QPU. S1 için 0-100 aralığı, S2 için 0-200 ve S4 için 0-400|ServerResourceType|
|memory_metric|Hafıza|Bayt|Ortalama|Bellek. S1 için 0-25 GB, S2 için 0-50 GB ve S4 için 0-100 GB|ServerResourceType|
|private_bytes_metric|Özel baytlar|Bayt|Ortalama|Özel baytlar.|ServerResourceType|
|virtual_bytes_metric|Sanal baytlar|Bayt|Ortalama|Sanal bayt sayısı.|ServerResourceType|
|TotalConnectionRequests|Toplam bağlantı Isteği sayısı|Sayı|Ortalama|Toplam bağlantı isteği sayısı. Bunlar, varışlardır.|ServerResourceType|
|Başarılı bir bağlantı Spersec|Saniyedeki başarılı bağlantı sayısı|Sayaçpersaniye|Ortalama|Başarılı bağlantı tamamlama oranı.|ServerResourceType|
|Totalconnectionarızaları|Toplam bağlantı başarısızlığı sayısı|Sayı|Ortalama|Toplam başarısız bağlantı denemesi.|ServerResourceType|
|CurrentUserSessions|Geçerli Kullanıcı oturumları|Sayı|Ortalama|Geçerli kullanıcı oturumlarının sayısı.|ServerResourceType|
|Querypoolbusyıthreads|Sorgu havuzu meşgul Iş parçacıkları|Sayı|Ortalama|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolJobQueueLength|Komut havuzu Iş kuyruğu uzunluğu|Sayı|Ortalama|Komut iş parçacığı havuzu sırasındaki iş sayısı.|ServerResourceType|
|ProcessingPoolJobQueueLength|İşleme havuzu Iş kuyruğu uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzu kuyruğundaki g/ç dışı iş sayısı.|ServerResourceType|
|CurrentConnections|Bağlantı: geçerli bağlantılar|Sayı|Ortalama|Kurulan istemci bağlantılarının geçerli sayısı.|ServerResourceType|
|CleanerCurrentPrice|Bellek: temizleyici geçerli fiyatı|Sayı|Ortalama|Geçerli bellek fiyatı, $/Byte/Time, 1000 olarak normalleştirilemez.|ServerResourceType|
|Cleanermemoryshrınkable|Bellek: temizleyici bellek shrınılabilir|Bayt|Ortalama|Arka plan temizleyici tarafından temizlenme konusu bayt cinsinden bellek miktarı.|ServerResourceType|
|Cleanermemorynonınkable|Bellek: temizleyici bellek daraltılamaz|Bayt|Ortalama|Arka plan temizleyici tarafından temizlenme konusu değil, bayt cinsinden bellek miktarı.|ServerResourceType|
|MemoryUsage|Bellek: bellek kullanımı|Bayt|Ortalama|Sunucu işleminin temizleyici bellek fiyatını hesaplamada kullanılan bellek kullanımı. XVelocity altyapısının bellek sınırının fazla olması halinde, xVelocity bellek içi analiz altyapısı (VertiPaq) tarafından eşlenen veya ayrılan belleği yoksayarak, işlem \ bayt ve bellek eşlemeli verilerin boyutuna eşit.|ServerResourceType|
|MemoryLimitHard|Bellek: bellek sınırı sabit|Bayt|Ortalama|Yapılandırma dosyasından sabit bellek sınırı.|ServerResourceType|
|Memoryhighlimit|Bellek: bellek sınırı yüksek|Bayt|Ortalama|Yapılandırma dosyasından yüksek bellek sınırı.|ServerResourceType|
|MemoryLimitLow|Bellek: bellek sınırı düşük|Bayt|Ortalama|Yapılandırma dosyasından düşük bellek sınırı.|ServerResourceType|
|MemoryLimitVertiPaq|Bellek: bellek sınırı VertiPaq|Bayt|Ortalama|Yapılandırma dosyasından bellek içi sınır.|ServerResourceType|
|Kota|Bellek: kota|Bayt|Ortalama|Bayt cinsinden geçerli bellek kotası. Bellek kotası, bellek verme veya bellek ayırma olarak da bilinir.|ServerResourceType|
|Quotabkilitli|Bellek: kota engellendi|Sayı|Ortalama|Diğer bellek kotaları boşaltılana kadar engellenen kota isteklerinin geçerli sayısı.|ServerResourceType|
|Vertipaqdisk belleksiz|Bellek: VertiPaq disk belleksiz|Bayt|Ortalama|Bellek içi altyapı tarafından kullanılmak üzere çalışma kümesinde kilitlenen bellek baytları.|ServerResourceType|
|Vertipaqdisk|Bellek: VertiPaq disk belleğine|Bayt|Ortalama|Bellek içi veriler için kullanılan disk belleğine alınan bellek miktarı.|ServerResourceType|
|RowsReadPerSec|İşleme: okunan satır sayısı/saniye|Sayaçpersaniye|Ortalama|Tüm ilişkisel veritabanlarından okunan satır oranı.|ServerResourceType|
|RowsConvertedPerSec|İşleme: dönüştürülen satır sayısı/saniye|Sayaçpersaniye|Ortalama|İşlem sırasında dönüştürülen satır oranı.|ServerResourceType|
|RowsWrittenPerSec|İşleme: yazılan satır sayısı/saniye|Sayaçpersaniye|Ortalama|İşlem sırasında yazılan satır oranı.|ServerResourceType|
|Commandpoolbusyıthreads|İş parçacıkları: komut havuzu meşgul iş parçacıkları|Sayı|Ortalama|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|CommandPoolIdleThreads|İş parçacıkları: komut havuzu boşta iş parçacıkları|Sayı|Ortalama|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|Longparsingbusyıthreads|İş parçacıkları: uzun ayrıştırma meşgul iş parçacıkları|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|LongParsingIdleThreads|İş parçacıkları: uzun ayrıştırma boşta iş parçacıkları|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|LongParsingJobQueueLength|İş parçacıkları: uzun ayrıştırma işi sıra uzunluğu|Sayı|Ortalama|Uzun ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|Processingpoolbusyıiojobthreads|İş parçacıkları: Işleme havuzu meşgul g/ç işi iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda g/ç işleri çalıştıran iş parçacığı sayısı.|ServerResourceType|
|Processingpoolbusınonıothreads|İş parçacıkları: Işleme havuzu meşgul olmayan g/ç iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda g/ç olmayan işler çalıştıran iş parçacığı sayısı.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|İş parçacıkları: Işleme havuzu g/ç iş kuyruğu uzunluğu|Sayı|Ortalama|İşleme iş parçacığı havuzu kuyruğundaki g/ç işlerinin sayısı.|ServerResourceType|
|Processingpokaydliiojobthreads|İş parçacıkları: Işleme havuzu boşta g/ç işi iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|Processingpokaydlenoniothreads|İş parçacıkları: Işleme havuzu boşta g/ç olmayan iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzunda g/ç dışı işlere adanmış boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolIdleThreads|İş parçacıkları: sorgu havuzu boşta iş parçacıkları|Sayı|Ortalama|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|ServerResourceType|
|QueryPoolJobQueueLength|İş parçacıkları: sorgu havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|ServerResourceType|
|Shortparsingbusyıthreads|İş parçacıkları: kısa ayrıştırma meşgul iş parçacıkları|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|ServerResourceType|
|ShortParsingIdleThreads|İş parçacıkları: kısa ayrıştırma boşta iş parçacıkları|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|ServerResourceType|
|ShortParsingJobQueueLength|İş parçacıkları: kısa ayrıştırma iş kuyruğu uzunluğu|Sayı|Ortalama|Kısa ayrıştırma iş parçacığı havuzu sırasındaki iş sayısı.|ServerResourceType|
|memory_thrashing_metric|Bellek dağılması|Yüzde|Ortalama|Ortalama bellek miktarı.|ServerResourceType|
|mashup_engine_qpu_metric|A motoru QPU|Sayı|Ortalama|Karma altyapı işlemlerine göre QPU kullanımı|ServerResourceType|
|mashup_engine_memory_metric|D motoru belleği|Bayt|Ortalama|Karma altyapı işlemlerine göre bellek kullanımı|ServerResourceType|
|mashup_engine_private_bytes_metric|D motoru özel baytlar|Bayt|Ortalama|Karma altyapı işlemlerine göre özel bayt kullanımı.|ServerResourceType|
|mashup_engine_virtual_bytes_metric|D motoru sanal bayt sayısı|Bayt|Ortalama|Karma altyapı işlemlerine göre sanal bayt kullanımı.|ServerResourceType|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Toplam Istek sayısı|Toplam ağ geçidi Isteği sayısı (kullanım dışı)|Sayı|Toplam|Ağ Geçidi isteklerinin sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|SuccessfulRequests|Başarılı ağ geçidi Istekleri (kullanım dışı)|Sayı|Toplam|Başarılı ağ geçidi isteklerinin sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|UnauthorizedRequests|Yetkisiz ağ geçidi Istekleri (kullanım dışı)|Sayı|Toplam|Yetkisiz ağ geçidi isteklerinin sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|FailedRequests|Başarısız ağ geçidi Istekleri (kullanım dışı)|Sayı|Toplam|Ağ Geçidi isteklerindeki başarısızlık sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|Diğer Istekler|Diğer ağ geçidi Istekleri (kullanım dışı)|Sayı|Toplam|Diğer ağ geçidi isteklerinin sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|Süre|Ağ Geçidi Isteklerinin genel süresi|Milisaniye|Ortalama|Milisaniye olarak ağ geçidi Isteklerinin genel süresi|Konum, ana bilgisayar adı|
|BackendDuration|Arka uç Isteklerinin süresi|Milisaniye|Ortalama|Milisaniye cinsinden arka uç Isteklerinin süresi|Konum, ana bilgisayar adı|
|Kapasite|Kapasite|Yüzde|Ortalama|Apimanayönetimi hizmeti için kullanım ölçümü|Konum|
|EventHubTotalEvents|Toplam EventHub olayları|Sayı|Toplam|EventHub 'e gönderilen olay sayısı|Konum|
|Eventhubbaşarılı Futavents|Başarılı EventHub olayları|Sayı|Toplam|Başarılı EventHub olaylarının sayısı|Konum|
|EventHubTotalFailedEvents|Başarısız EventHub olayları|Sayı|Toplam|Başarısız EventHub olaylarının sayısı|Konum|
|EventHubRejectedEvents|Reddedilen EventHub olayları|Sayı|Toplam|Reddedilen EventHub olaylarının sayısı (yanlış yapılandırma veya yetkisiz)|Konum|
|EventHubThrottledEvents|Kısıtlanmış EventHub olayları|Sayı|Toplam|Kısıtlanmış EventHub olay sayısı|Konum|
|EventHubTimedoutEvents|Zaman aşımına uğrayan EventHub olayları|Sayı|Toplam|Zaman aşımına uğrayan EventHub olaylarının sayısı|Konum|
|EventHubDroppedEvents|Bırakılan EventHub olayları|Sayı|Toplam|Sıra boyutu sınırına ulaşıldığından atlanan olay sayısı|Konum|
|EventHubTotalBytesSent|EventHub olaylarının boyutu|Bayt|Toplam|Toplam EventHub olay boyutu (bayt)|Konum|
|İstekler|İstekler|Sayı|Toplam|Birden çok boyut içeren ağ geçidi isteği ölçümleri|Konum, ana bilgisayar adı, LastErrorReason, BackendResponseCode, GatewayResponseCode, BackendResponseCodeCategory, GatewayResponseCodeCategory|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft. AppConfiguration/Configurationmağazaları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Httpıncomingrequestcount|Httpıncomingrequestcount|Sayı|Sayı|Gelen http isteklerinin toplam sayısı.|Hiçbiri|
|FailedHttpRequestCount|FailedHttpRequestCount|Sayı|Sayı|Başarısız http istekleri.|Hiçbiri|
|Httpıncomingrequestduration|Httpıncomingrequestduration|Sayı|Ortalama|Http isteğinde gecikme süresi.|Hiçbiri|


## <a name="microsoftappplatformspring"></a>Microsoft. AppPlatform/yay

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SystemCpuUsagePercentage|Sistem CPU kullanım yüzdesi|Yüzde|Ortalama|Tüm sistem için en son CPU kullanımı|AppName, Pod|
|AppCpuUsagePercentage|Uygulama CPU kullanım yüzdesi|Yüzde|Ortalama|Uygulama JVM CPU kullanım yüzdesi|AppName, Pod|
|AppMemoryCommitted|Atanan uygulama belleği|Bayt|Ortalama|JVM 'ye bayt olarak atanan bellek|AppName, Pod|
|Appmemorykullanıldı|Kullanılan uygulama belleği|Bayt|Ortalama|Bayt cinsinden kullanılan uygulama belleği|AppName, Pod|
|AppMemoryMax|En fazla uygulama belleği|Bayt|Maksimum|Bellek yönetimi için kullanılabilecek bayt cinsinden maksimum bellek miktarı|AppName, Pod|
|MaxOldGenMemoryPoolBytes|En fazla kullanılabilir eski nesil veri boyutu|Bayt|Ortalama|Eski nesil bellek havuzunun en büyük boyutu|AppName, Pod|
|OldGenMemoryPoolBytes|Eski nesil veri boyutu|Bayt|Ortalama|Tam GC sonrasında eski nesil bellek havuzunun boyutu|AppName, Pod|
|OldGenPromotedBytes|Eski nesil veri boyutuna yükselt|Bayt|Maksimum|GC 'den sonra GC 'den önce eski nesil bellek havuzunun boyutundaki pozitif artış sayısı|AppName, Pod|
|YoungGenPromotedBytes|Küçük ölçekli oluşturma veri boyutuna yükselt|Bayt|Maksimum|Bir GC 'nin bir sonraki öncesine ait olması durumunda küçük kuşak bellek havuzunun boyutunun artması için artırılır|AppName, Pod|
|GCPauseTotalCount|GC duraklatma sayısı|Sayı|Toplam|GC duraklatma sayısı|AppName, Pod|
|GCPauseTotalTime|GC duraklatma toplam süre|Milisaniye|Toplam|GC duraklatma toplam süre|AppName, Pod|
|TomcatSentBytes|Tomcat gönderilen toplam bayt sayısı|Bayt|Toplam|Tomcat gönderilen toplam bayt sayısı|AppName, Pod|
|Tomcatreceived baytları|Tomcat alınan toplam bayt sayısı|Bayt|Toplam|Tomcat alınan toplam bayt sayısı|AppName, Pod|
|TomcatRequestTotalTime|Tomcat Isteği toplam kez|Milisaniye|Toplam|Tomcat Isteği toplam kez|AppName, Pod|
|TomcatRequestTotalCount|Tomcat Isteği toplam sayısı|Sayı|Toplam|Tomcat Isteği toplam sayısı|AppName, Pod|
|TomcatResponseAvgTime|Tomcat Isteği ortalama zamanı|Milisaniye|Ortalama|Tomcat Isteği ortalama zamanı|AppName, Pod|
|TomcatRequestMaxTime|Tomcat Isteği maksimum zamanı|Milisaniye|Maksimum|Tomcat Isteği maksimum zamanı|AppName, Pod|
|TomcatErrorCount|Tomcat genel hatası|Sayı|Toplam|Tomcat genel hatası|AppName, Pod|
|TomcatSessionActiveMaxCount|Tomcat oturumu en fazla etkin sayısı|Sayı|Toplam|Tomcat oturumu en fazla etkin sayısı|AppName, Pod|
|TomcatSessionAliveMaxTime|Tomcat oturumu maksimum etkin süresi|Milisaniye|Maksimum|Tomcat oturumu maksimum etkin süresi|AppName, Pod|
|TomcatSessionActiveCurrentCount|Tomcat oturum etkin sayısı|Sayı|Toplam|Tomcat oturum etkin sayısı|AppName, Pod|
|TomcatSessionCreatedCount|Tomcat oturum oluşturma sayısı|Sayı|Toplam|Tomcat oturum oluşturma sayısı|AppName, Pod|
|TomcatSessionExpiredCount|Tomcat oturumunun süre dolma sayısı|Sayı|Toplam|Tomcat oturumunun süre dolma sayısı|AppName, Pod|
|TomcatSessionRejectedCount|Tomcat oturumu reddedildi sayısı|Sayı|Toplam|Tomcat oturumu reddedildi sayısı|AppName, Pod|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft. Automation/automationAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalJob|Toplam Iş sayısı|Sayı|Toplam|Toplam iş sayısı|Runbook, durum|
|TotalUpdateDeploymentRuns|Toplam güncelleştirme dağıtımı çalıştırmaları|Sayı|Toplam|Toplam yazılım güncelleştirme dağıtımı çalıştırmaları|SoftwareUpdateConfigurationName, durum|
|Totalupdatedeploymentmachinerçalıştırır|Toplam güncelleştirme dağıtım makinesi çalıştırması|Sayı|Toplam|Yazılım güncelleştirme dağıtımı çalıştırmasında toplam yazılım güncelleştirme dağıtımı makinesi|SoftwareUpdateConfigurationName, Status, TargetComputer, Softwareupdateconfigurationrunıd|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CoreCount|Adanmış çekirdek sayısı|Sayı|Toplam|Batch hesabındaki toplam adanmış çekirdek sayısı|Hiçbiri|
|TotalNodeCount|Adanmış düğüm sayısı|Sayı|Toplam|Batch hesabındaki toplam ayrılmış düğüm sayısı|Hiçbiri|
|LowPriorityCoreCount|LowPriority çekirdek sayısı|Sayı|Toplam|Batch hesabındaki toplam düşük öncelikli çekirdek sayısı|Hiçbiri|
|TotalLowPriorityNodeCount|Düşük öncelikli düğüm sayısı|Sayı|Toplam|Batch hesabındaki toplam düşük öncelikli düğüm sayısı|Hiçbiri|
|CreatingNodeCount|Düğüm sayısı oluşturuluyor|Sayı|Toplam|Oluşturulmakta olan düğüm sayısı|Hiçbiri|
|StartingNodeCount|Başlangıç düğüm sayısı|Sayı|Toplam|Başlayan düğüm sayısı|Hiçbiri|
|WaitingForStartTaskNodeCount|Başlangıç görevi düğüm sayısı bekleniyor|Sayı|Toplam|Başlangıç görevinin tamamlanması için bekleyen düğüm sayısı|Hiçbiri|
|StartTaskFailedNodeCount|Başlangıç görevi başarısız düğüm sayısı|Sayı|Toplam|Başlangıç görevinin başarısız olduğu düğüm sayısı|Hiçbiri|
|IdleNodeCount|Boştaki düğüm sayısı|Sayı|Toplam|Boştaki düğüm sayısı|Hiçbiri|
|OfflineNodeCount|Çevrimdışı düğüm sayısı|Sayı|Toplam|Çevrimdışı düğüm sayısı|Hiçbiri|
|RebootingNodeCount|Düğüm sayısı yeniden başlatılıyor|Sayı|Toplam|Yeniden başlatma düğümlerinin sayısı|Hiçbiri|
|ReimagingNodeCount|Yeniden Imaging düğüm sayısı|Sayı|Toplam|Yeniden görüntüleme düğümlerinin sayısı|Hiçbiri|
|RunningNodeCount|Çalışan düğüm sayısı|Sayı|Toplam|Çalışan düğümlerin sayısı|Hiçbiri|
|LeavingPoolNodeCount|Havuz düğüm sayısından çıkılıyor|Sayı|Toplam|Havuzdan ayrılma düğüm sayısı|Hiçbiri|
|UnusableNodeCount|Kullanılamayan düğüm sayısı|Sayı|Toplam|Kullanılamayan düğüm sayısı|Hiçbiri|
|PreemptedNodeCount|Önden düğüm sayısı|Sayı|Toplam|Yok eden düğüm sayısı|Hiçbiri|
|TaskStartEvent|Görev başlatma olayları|Sayı|Toplam|Başlatılan toplam görev sayısı|Hiçbiri|
|TaskCompleteEvent|Görev tamamlanma olayları|Sayı|Toplam|Tamamlanan toplam görev sayısı|Hiçbiri|
|TaskFailEvent|Görev başarısız olayları|Sayı|Toplam|Hatalı durumda tamamlanan toplam görev sayısı|Hiçbiri|
|PoolCreateEvent|Havuz oluşturma olayları|Sayı|Toplam|Oluşturulan Toplam Havuz sayısı|Hiçbiri|
|PoolResizeStartEvent|Havuz yeniden boyutlandırma başlangıç olayları|Sayı|Toplam|Başlatılan toplam havuz yeniden boyutlandırmaları sayısı|Hiçbiri|
|PoolResizeCompleteEvent|Havuz yeniden boyutlandırma Tamam olayları|Sayı|Toplam|Tamamlanan toplam havuz yeniden boyutlandırmaları sayısı|Hiçbiri|
|PoolDeleteStartEvent|Havuz silme başlangıç olayları|Sayı|Toplam|Başlatılmış olan toplam havuz silme sayısı|Hiçbiri|
|PoolDeleteCompleteEvent|Havuz silme Tamam olayları|Sayı|Toplam|Tamamlanan toplam havuz silme sayısı|Hiçbiri|
|JobDeleteCompleteEvent|İş silme Tamam olayları|Sayı|Toplam|Başarıyla silinen işlerin toplam sayısı.|Hiçbiri|
|JobDeleteStartEvent|İş silme başlangıç olayları|Sayı|Toplam|Silinmesi istenen toplam iş sayısı.|Hiçbiri|
|JobDisableCompleteEvent|İş tüm olayları devre dışı bırak|Sayı|Toplam|Başarıyla devre dışı bırakılmış toplam iş sayısı.|Hiçbiri|
|JobDisableStartEvent|İş devre dışı başlatma olayları|Sayı|Toplam|Devre dışı bırakılması istenen toplam iş sayısı.|Hiçbiri|
|JobStartEvent|İş başlatma olayları|Sayı|Toplam|Başarıyla başlatılmış toplam iş sayısı.|Hiçbiri|
|JobTerminateCompleteEvent|İş sonlandırma tamamlanma olayları|Sayı|Toplam|Başarıyla sonlandırılan toplam iş sayısı.|Hiçbiri|
|JobTerminateStartEvent|İş sonlandırma başlangıç olayları|Sayı|Toplam|Sonlandırılması istenen toplam iş sayısı.|Hiçbiri|


## <a name="microsoftbatchaiworkspaces"></a>Microsoft. Batchaı/çalışma alanları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İş gönderildi|İş gönderildi|Sayı|Toplam|Gönderilen iş sayısı|Senaryo, ClusterName|
|İş tamamlandı|İş tamamlandı|Sayı|Toplam|Tamamlanan iş sayısı|Senaryo, ClusterName, ResultType|
|Toplam düğüm sayısı|Toplam düğüm sayısı|Sayı|Ortalama|Toplam düğüm sayısı|Senaryo, ClusterName|
|Etkin düğümler|Etkin düğümler|Sayı|Ortalama|Çalışan düğümlerin sayısı|Senaryo, ClusterName|
|Boştaki düğümler|Boştaki düğümler|Sayı|Ortalama|Boştaki düğüm sayısı|Senaryo, ClusterName|
|Kullanılamayan düğümler|Kullanılamayan düğümler|Sayı|Ortalama|Kullanılamayan düğüm sayısı|Senaryo, ClusterName|
|Önden düğümler|Önden düğümler|Sayı|Ortalama|Yok eden düğüm sayısı|Senaryo, ClusterName|
|Düğümlerden çıkılıyor|Düğümlerden çıkılıyor|Sayı|Ortalama|Bırakma düğümü sayısı|Senaryo, ClusterName|
|Toplam çekirdek sayısı|Toplam çekirdek sayısı|Sayı|Ortalama|Toplam çekirdek sayısı|Senaryo, ClusterName|
|Etkin çekirdekler|Etkin çekirdekler|Sayı|Ortalama|Etkin çekirdek sayısı|Senaryo, ClusterName|
|Boştaki çekirdekler|Boştaki çekirdekler|Sayı|Ortalama|Boşta çekirdek sayısı|Senaryo, ClusterName|
|Kullanılamayan çekirdekler|Kullanılamayan çekirdekler|Sayı|Ortalama|Kullanılamayan çekirdek sayısı|Senaryo, ClusterName|
|Önden çekirdekler|Önden çekirdekler|Sayı|Ortalama|Yok edilecek çekirdek sayısı|Senaryo, ClusterName|
|Çekirdekleri bırakma|Çekirdekleri bırakma|Sayı|Ortalama|Bırakma çekirdekleri sayısı|Senaryo, ClusterName|
|Kota kullanım yüzdesi|Kota kullanım yüzdesi|Sayı|Ortalama|Kullanılan Kota yüzdesi|Senaryo, ClusterName, VmFamilyName, VmPriority|

## <a name="microsoftblockchainblockchainmembers"></a>Microsoft. Blockzincirine/blockchainMembers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuUsagePercentageInDouble|CPU kullanım yüzdesi|Yüzde|Maksimum|CPU kullanım yüzdesi|Düğüm|
|MemoryUsage|Bellek kullanımı|Bayt|Ortalama|Bellek kullanımı|Düğüm|
|MemoryLimit|Bellek sınırı|Bayt|Ortalama|Bellek sınırı|Düğüm|
|MemoryUsagePercentageInDouble|Bellek kullanım yüzdesi|Yüzde|Ortalama|Bellek kullanım yüzdesi|Düğüm|
|StorageUsage|Depolama alanı kullanımı|Bayt|Ortalama|Depolama alanı kullanımı|Düğüm|
|Ioreadbytes|GÇ okuma baytları|Bayt|Toplam|GÇ okuma baytları|Düğüm|
|IOWriteBytes|GÇ Yazma baytları|Bayt|Toplam|GÇ Yazma baytları|Düğüm|
|Connectionkabul edildi|Kabul edilen bağlantılar|Sayı|Toplam|Kabul edilen bağlantılar|Düğüm|
|Connectionişlenmiş|İşlenmiş bağlantılar|Sayı|Toplam|İşlenmiş bağlantılar|Düğüm|
|ConnectionActive|Etkin bağlantılar|Sayı|Ortalama|Etkin bağlantılar|Düğüm|
|RequestHandled|İşlenmiş Istekler|Sayı|Toplam|İşlenmiş Istekler|Düğüm|
|Processedbkilitler|İşlenen bloklar|Sayı|Toplam|İşlenen bloklar|Düğüm|
|ProcessedTransactions|İşlenen Işlemler|Sayı|Toplam|İşlenen Işlemler|Düğüm|
|PendingTransactions|Bekleyen Işlemler|Sayı|Ortalama|Bekleyen Işlemler|Düğüm|
|QueuedTransactions|Kuyruğa alınmış Işlemler|Sayı|Ortalama|Kuyruğa alınmış Işlemler|Düğüm|



## <a name="microsoftcacheredis"></a>Microsoft. Cache/redsıs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|connectedistemcileri|Bağlı Istemciler|Sayı|Maksimum||Parça|
|totalcommandsişlendi|Toplam Işlem sayısı|Sayı|Toplam||Parça|
|cachehits|Önbellek ısabetleri|Sayı|Toplam||Parça|
|cacheisabetsizlik|Önbellek Isabetsizliği|Sayı|Toplam||Parça|
|cachemissrate|Önbellek Isabetsizlik oranı|Yüzde|cachemissrate||Parça|
|GetCommands|İyorsa|Sayı|Toplam||Parça|
|SetCommands|Yapar|Sayı|Toplam||Parça|
|operationsPerSecond|İşlem/saniye|Sayı|Maksimum||Parça|
|çıkarılan anahtarlar|Çıkarılan anahtarlar|Sayı|Toplam||Parça|
|totalkeys|Toplam anahtar sayısı|Sayı|Maksimum||Parça|
|expiredkeys|Süre dolma anahtarları|Sayı|Toplam||Parça|
|usedmemory|Kullanılan bellek|Bayt|Maksimum||Parça|
|usedmemorypercentage|Kullanılan bellek yüzdesi|Yüzde|Maksimum||Parça|
|usedmemoryRss|Kullanılan bellek RSS|Bayt|Maksimum||Parça|
|Sunucuyükü|Sunucu yükü|Yüzde|Maksimum||Parça|
|cacheWrite|Önbellek yazma|BytesPerSecond|Maksimum||Parça|
|cacheRead|Önbellek Okuma|BytesPerSecond|Maksimum||Parça|
|PercentProcessorTime|CPU|Yüzde|Maksimum||Parça|
|cacheLatency|Önbellek gecikmesi mikrosaniye (Önizleme)|Sayı|Ortalama||Parça|
|hatalar|Hatalar|Sayı|Maksimum||Shardıd, ErrorType|
|connectedclients0|Bağlı Istemciler (parça 0)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed0|Toplam Işlem (parça 0)|Sayı|Toplam||Hiçbiri|
|cachehits0|Önbellek ısabetleri (parça 0)|Sayı|Toplam||Hiçbiri|
|cachemisses0|Önbellekte bulunamayanlar (parça 0)|Sayı|Toplam||Hiçbiri|
|getcommands0|Alır (parça 0)|Sayı|Toplam||Hiçbiri|
|setcommands0|Kümeler (parça 0)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond0|Saniyedeki işlem (parça 0)|Sayı|Maksimum||Hiçbiri|
|evictedkeys0|Çıkarılan anahtarlar (parça 0)|Sayı|Toplam||Hiçbiri|
|totalkeys0|Toplam anahtar (parça 0)|Sayı|Maksimum||Hiçbiri|
|expiredkeys0|Süre dolma anahtarları (parça 0)|Sayı|Toplam||Hiçbiri|
|usedmemory0|Kullanılan bellek (parça 0)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss0|Kullanılan bellek RSS (parça 0)|Bayt|Maksimum||Hiçbiri|
|serverLoad0|Sunucu yükü (parça 0)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite0|Önbellek yazma (parça 0)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead0|Önbellek Okuma (parça 0)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime0|CPU (parça 0)|Yüzde|Maksimum||Hiçbiri|
|connectedclients1|Bağlı Istemciler (parça 1)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed1|Toplam Işlem (parça 1)|Sayı|Toplam||Hiçbiri|
|cachehits1|Önbellek ısabetleri (parça 1)|Sayı|Toplam||Hiçbiri|
|cachemisses1|Önbellekte bulunamayanlar (parça 1)|Sayı|Toplam||Hiçbiri|
|getcommands1|Alır (parça 1)|Sayı|Toplam||Hiçbiri|
|setcommands1|Kümeler (parça 1)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond1|Saniyedeki işlem (parça 1)|Sayı|Maksimum||Hiçbiri|
|evictedkeys1|Çıkarılan anahtarlar (parça 1)|Sayı|Toplam||Hiçbiri|
|totalkeys1|Toplam anahtar (parça 1)|Sayı|Maksimum||Hiçbiri|
|expiredkeys1|Süre dolma anahtarları (parça 1)|Sayı|Toplam||Hiçbiri|
|usedmemory1|Kullanılan bellek (parça 1)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss1|Kullanılan bellek RSS (parça 1)|Bayt|Maksimum||Hiçbiri|
|serverLoad1|Sunucu yükü (parça 1)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite1|Önbellek yazma (parça 1)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead1|Önbellek Okuma (parça 1)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime1|CPU (parça 1)|Yüzde|Maksimum||Hiçbiri|
|connectedclients2|Bağlı Istemciler (parça 2)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed2|Toplam Işlem (parça 2)|Sayı|Toplam||Hiçbiri|
|cachehits2|Önbellek ısabetleri (parça 2)|Sayı|Toplam||Hiçbiri|
|cachemisses2|Önbellekte bulunamayanlar (parça 2)|Sayı|Toplam||Hiçbiri|
|getcommands2|Alır (parça 2)|Sayı|Toplam||Hiçbiri|
|setcommands2|Kümeler (parça 2)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond2|Saniyedeki işlem (parça 2)|Sayı|Maksimum||Hiçbiri|
|evictedkeys2|Çıkarılan anahtarlar (parça 2)|Sayı|Toplam||Hiçbiri|
|totalkeys2|Toplam anahtar (parça 2)|Sayı|Maksimum||Hiçbiri|
|expiredkeys2|Süre dolma anahtarları (parça 2)|Sayı|Toplam||Hiçbiri|
|usedmemory2|Kullanılan bellek (parça 2)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss2|Kullanılan bellek RSS (parça 2)|Bayt|Maksimum||Hiçbiri|
|serverLoad2|Sunucu yükü (parça 2)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite2|Önbellek yazma (parça 2)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead2|Önbellek Okuma (parça 2)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime2|CPU (parça 2)|Yüzde|Maksimum||Hiçbiri|
|connectedclients3|Bağlı Istemciler (parça 3)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed3|Toplam Işlem (parça 3)|Sayı|Toplam||Hiçbiri|
|cachehits3|Önbellek ısabetleri (parça 3)|Sayı|Toplam||Hiçbiri|
|cachemisses3|Önbellekte bulunamayanlar (parça 3)|Sayı|Toplam||Hiçbiri|
|getcommands3|Alır (parça 3)|Sayı|Toplam||Hiçbiri|
|setcommands3|Kümeler (parça 3)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond3|Saniyedeki işlem (parça 3)|Sayı|Maksimum||Hiçbiri|
|evictedkeys3|Çıkarılan anahtarlar (parça 3)|Sayı|Toplam||Hiçbiri|
|totalkeys3|Toplam anahtar (parça 3)|Sayı|Maksimum||Hiçbiri|
|expiredkeys3|Süre dolma anahtarları (parça 3)|Sayı|Toplam||Hiçbiri|
|usedmemory3|Kullanılan bellek (parça 3)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss3|Kullanılan bellek RSS (parça 3)|Bayt|Maksimum||Hiçbiri|
|serverLoad3|Sunucu yükü (parça 3)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite3|Önbellek yazma (parça 3)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead3|Önbellek Okuma (parça 3)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime3|CPU (parça 3)|Yüzde|Maksimum||Hiçbiri|
|connectedclients4|Bağlı Istemciler (parça 4)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed4|Toplam Işlem (parça 4)|Sayı|Toplam||Hiçbiri|
|cachehits4|Önbellek ısabetleri (parça 4)|Sayı|Toplam||Hiçbiri|
|cachemisses4|Önbellekte bulunamayanlar (parça 4)|Sayı|Toplam||Hiçbiri|
|getcommands4|Alır (parça 4)|Sayı|Toplam||Hiçbiri|
|setcommands4|Kümeler (parça 4)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond4|Saniyedeki işlem (parça 4)|Sayı|Maksimum||Hiçbiri|
|evictedkeys4|Çıkarılan anahtarlar (parça 4)|Sayı|Toplam||Hiçbiri|
|totalkeys4|Toplam anahtar (parça 4)|Sayı|Maksimum||Hiçbiri|
|expiredkeys4|Süre dolma anahtarları (parça 4)|Sayı|Toplam||Hiçbiri|
|usedmemory4|Kullanılan bellek (parça 4)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss4|Kullanılan bellek RSS (parça 4)|Bayt|Maksimum||Hiçbiri|
|serverLoad4|Sunucu yükü (parça 4)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite4|Önbellek yazma (parça 4)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead4|Önbellek Okuma (parça 4)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime4|CPU (parça 4)|Yüzde|Maksimum||Hiçbiri|
|connectedclients5|Bağlı Istemciler (parça 5)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed5|Toplam Işlem (parça 5)|Sayı|Toplam||Hiçbiri|
|cachehits5|Önbellek ısabetleri (parça 5)|Sayı|Toplam||Hiçbiri|
|cachemisses5|Önbellekte bulunamayanlar (parça 5)|Sayı|Toplam||Hiçbiri|
|getcommands5|Alır (parça 5)|Sayı|Toplam||Hiçbiri|
|setcommands5|Kümeler (parça 5)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond5|Saniyedeki işlem (parça 5)|Sayı|Maksimum||Hiçbiri|
|evictedkeys5|Çıkarılan anahtarlar (parça 5)|Sayı|Toplam||Hiçbiri|
|totalkeys5|Toplam anahtar (parça 5)|Sayı|Maksimum||Hiçbiri|
|expiredkeys5|Süre dolma anahtarları (parça 5)|Sayı|Toplam||Hiçbiri|
|usedmemory5|Kullanılan bellek (parça 5)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss5|Kullanılan bellek RSS (parça 5)|Bayt|Maksimum||Hiçbiri|
|serverLoad5|Sunucu yükü (parça 5)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite5|Önbellek yazma (parça 5)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead5|Önbellek Okuma (parça 5)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime5|CPU (parça 5)|Yüzde|Maksimum||Hiçbiri|
|connectedclients6|Bağlı Istemciler (parça 6)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed6|Toplam Işlem (parça 6)|Sayı|Toplam||Hiçbiri|
|cachehits6|Önbellek ısabetleri (parça 6)|Sayı|Toplam||Hiçbiri|
|cachemisses6|Önbellekte bulunamayanlar (parça 6)|Sayı|Toplam||Hiçbiri|
|getcommands6|Alır (parça 6)|Sayı|Toplam||Hiçbiri|
|setcommands6|Kümeler (parça 6)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond6|Saniyedeki işlem (parça 6)|Sayı|Maksimum||Hiçbiri|
|evictedkeys6|Çıkarılan anahtarlar (parça 6)|Sayı|Toplam||Hiçbiri|
|totalkeys6|Toplam anahtar (parça 6)|Sayı|Maksimum||Hiçbiri|
|expiredkeys6|Süre dolma anahtarları (parça 6)|Sayı|Toplam||Hiçbiri|
|usedmemory6|Kullanılan bellek (parça 6)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss6|Kullanılan bellek RSS (parça 6)|Bayt|Maksimum||Hiçbiri|
|serverLoad6|Sunucu yükü (parça 6)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite6|Önbellek yazma (parça 6)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead6|Önbellek Okuma (parça 6)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime6|CPU (parça 6)|Yüzde|Maksimum||Hiçbiri|
|connectedclients7|Bağlı Istemciler (parça 7)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed7|Toplam Işlem (parça 7)|Sayı|Toplam||Hiçbiri|
|cachehits7|Önbellek ısabetleri (parça 7)|Sayı|Toplam||Hiçbiri|
|cachemisses7|Önbellekte bulunamayanlar (parça 7)|Sayı|Toplam||Hiçbiri|
|getcommands7|Alır (parça 7)|Sayı|Toplam||Hiçbiri|
|setcommands7|Kümeler (parça 7)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond7|Saniyedeki işlem (parça 7)|Sayı|Maksimum||Hiçbiri|
|evictedkeys7|Çıkarılan anahtarlar (parça 7)|Sayı|Toplam||Hiçbiri|
|totalkeys7|Toplam anahtar (parça 7)|Sayı|Maksimum||Hiçbiri|
|expiredkeys7|Süre dolma anahtarları (parça 7)|Sayı|Toplam||Hiçbiri|
|usedmemory7|Kullanılan bellek (parça 7)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss7|Kullanılan bellek RSS (parça 7)|Bayt|Maksimum||Hiçbiri|
|serverLoad7|Sunucu yükü (parça 7)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite7|Önbellek yazma (parça 7)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead7|Önbellek Okuma (parça 7)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime7|CPU (parça 7)|Yüzde|Maksimum||Hiçbiri|
|connectedclients8|Bağlı Istemciler (parça 8)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed8|Toplam Işlem (parça 8)|Sayı|Toplam||Hiçbiri|
|cachehits8|Önbellek ısabetleri (parça 8)|Sayı|Toplam||Hiçbiri|
|cachemisses8|Önbellekte bulunamayanlar (parça 8)|Sayı|Toplam||Hiçbiri|
|getcommands8|Al (Shard 8)|Sayı|Toplam||Hiçbiri|
|setcommands8|Kümeler (Shard 8)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond8|Saniyedeki işlem (parça 8)|Sayı|Maksimum||Hiçbiri|
|evictedkeys8|Çıkarılan anahtarlar (parça 8)|Sayı|Toplam||Hiçbiri|
|totalkeys8|Toplam anahtar (Shard 8)|Sayı|Maksimum||Hiçbiri|
|expiredkeys8|Süre dolma anahtarları (parça 8)|Sayı|Toplam||Hiçbiri|
|usedmemory8|Kullanılan bellek (parça 8)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss8|Kullanılan bellek RSS (parça 8)|Bayt|Maksimum||Hiçbiri|
|serverLoad8|Sunucu yükü (parça 8)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite8|Önbellek yazma (parça 8)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead8|Önbellek Okuma (parça 8)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime8|CPU (parça 8)|Yüzde|Maksimum||Hiçbiri|
|connectedclients9|Bağlı Istemciler (parça 9)|Sayı|Maksimum||Hiçbiri|
|totalcommandsprocessed9|Toplam Işlem (parça 9)|Sayı|Toplam||Hiçbiri|
|cachehits9|Önbellek ısabetleri (parça 9)|Sayı|Toplam||Hiçbiri|
|cachemisses9|Önbellekte bulunamayanlar (parça 9)|Sayı|Toplam||Hiçbiri|
|getcommands9|Alır (parça 9)|Sayı|Toplam||Hiçbiri|
|setcommands9|Kümeler (parça 9)|Sayı|Toplam||Hiçbiri|
|operationsPerSecond9|Saniyedeki işlem (parça 9)|Sayı|Maksimum||Hiçbiri|
|evictedkeys9|Çıkarılan anahtarlar (parça 9)|Sayı|Toplam||Hiçbiri|
|totalkeys9|Toplam anahtar (parça 9)|Sayı|Maksimum||Hiçbiri|
|expiredkeys9|Süre dolma anahtarları (parça 9)|Sayı|Toplam||Hiçbiri|
|usedmemory9|Kullanılan bellek (parça 9)|Bayt|Maksimum||Hiçbiri|
|usedmemoryRss9|Kullanılan bellek RSS (parça 9)|Bayt|Maksimum||Hiçbiri|
|serverLoad9|Sunucu yükü (parça 9)|Yüzde|Maksimum||Hiçbiri|
|cacheWrite9|Önbellek yazma (parça 9)|BytesPerSecond|Maksimum||Hiçbiri|
|cacheRead9|Önbellek Okuma (parça 9)|BytesPerSecond|Maksimum||Hiçbiri|
|percentProcessorTime9|CPU (parça 9)|Yüzde|Maksimum||Hiçbiri|



## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft. CDN/cdnwebapplicationfirewallpolicies

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|WebApplicationFirewallRequestCount|Web uygulaması güvenlik duvarı Istek sayısı|Sayı|Toplam|Web uygulaması güvenlik duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName, RuleName, eylem|


## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft. ClassicCompute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi.|Hiçbiri|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik).|Hiçbiri|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik).|Hiçbiri|
|Disk okuma bayt/sn|Disk okuma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diskten okunan ortalama bayt.|Hiçbiri|
|Disk yazma bayt/sn|Disk yazma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diske yazılan ortalama bayt.|Hiçbiri|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si.|Hiçbiri|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si.|Hiçbiri|


## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft. ClassicCompute/domainNames/yuvalar/roller

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi.|Roleınstanceıd|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik).|Roleınstanceıd|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik).|Roleınstanceıd|
|Disk okuma bayt/sn|Disk okuma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diskten okunan ortalama bayt.|Roleınstanceıd|
|Disk yazma bayt/sn|Disk yazma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diske yazılan ortalama bayt.|Roleınstanceıd|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si.|Roleınstanceıd|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si.|Roleınstanceıd|



## <a name="microsoftclassicstoragestorageaccounts"></a>Microsoft. ClassicStorage/storageAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|UsedCapacity|Kullanılan kapasite|Bayt|Ortalama|Hesabın kullanılan kapasitesi|Hiçbiri|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftclassicstoragestorageaccountsblobservices"></a>Microsoft. ClassicStorage/storageAccounts/blobServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobCapacity|Blob Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı.|BlobType, katman|
|BLOB sayısı|Blob Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType, katman|
|ContainerCount|Blob Kapsayıcı Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki kapsayıcı sayısı.|Hiçbiri|
|Dizin kapasitesi|Dizin kapasitesi|Bayt|Ortalama|Bayt cinsinden ADLS 2. (hiyerarşik) dizin tarafından kullanılan depolama miktarı.|Hiçbiri|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftclassicstoragestorageaccountstableservices"></a>Microsoft. ClassicStorage/storageAccounts/tableServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TableCapacity|Tablo Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Tablo hizmeti tarafından kullanılan depolama miktarı.|Hiçbiri|
|TableCount|Tablo Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo sayısı.|Hiçbiri|
|TableEntityCount|Tablo Varlık Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo varlıklarının sayısı.|Hiçbiri|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftclassicstoragestorageaccountsfileservices"></a>Microsoft. ClassicStorage/storageAccounts/fileServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Dosya kapasitesi|Dosya kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının dosya hizmeti tarafından kullanılan depolama miktarı.|FileShare|
|FileCount|Dosya sayısı|Sayı|Ortalama|Depolama hesabının dosya hizmetindeki dosya sayısı.|FileShare|
|Dosya ShareCount|Dosya paylaşma sayısı|Sayı|Ortalama|Depolama hesabının dosya hizmetindeki dosya paylaşımlarının sayısı.|Hiçbiri|
|FileShareSnapshotCount|Dosya paylaşımının anlık görüntü sayısı|Sayı|Ortalama|Depolama hesabının dosyalar hizmetindeki paylaşımda bulunan anlık görüntü sayısı.|FileShare|
|FileShareSnapshotSize|Dosya paylaşımının anlık görüntü boyutu|Bayt|Ortalama|Bayt olarak depolama hesabının dosya hizmetindeki anlık görüntüler tarafından kullanılan depolama miktarı.|FileShare|
|Dosya Sharequota|Dosya paylaşımının kota boyutu|Bayt|Ortalama|Azure dosyaları hizmeti tarafından bayt olarak kullanılabilecek depolama miktarının üst sınırı.|FileShare|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, Authentication, FileShare|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|

## <a name="microsoftclassicstoragestorageaccountsqueueservices"></a>Microsoft. ClassicStorage/storageAccounts/queueServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueueCapacity|Kuyruk Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Kuyruk hizmeti tarafından kullanılan depolama miktarı.|Hiçbiri|
|QueueCount|Kuyruk Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra sayısı.|Hiçbiri|
|QueueMessageCount|Kuyruk İleti Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra iletilerinin yaklaşık sayısı.|Hiçbiri|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Toplam çağrılar|Toplam çağrı sayısı|Sayı|Toplam|Toplam çağrı sayısı.|ApiName, OperationName, bölge|
|Başarılı çağrılar|Başarılı çağrılar|Sayı|Toplam|Başarılı çağrı sayısı.|ApiName, OperationName, bölge|
|Toplam hata sayısı|Toplam hata sayısı|Sayı|Toplam|Hata yanıtıyla Toplam çağrı sayısı (HTTP yanıt kodu 4xx veya 5xx).|ApiName, OperationName, bölge|
|Blockedçağrılarında|Engellenen çağrılar|Sayı|Toplam|Oran veya kota sınırını aşan çağrı sayısı.|ApiName, OperationName, bölge|
|ServerErrors|Sunucu hataları|Sayı|Toplam|Hizmet iç hatası olan çağrı sayısı (HTTP yanıt kodu 5xx).|ApiName, OperationName, bölge|
|ClientErrors|İstemci hataları|Sayı|Toplam|İstemci tarafı hatası olan çağrı sayısı (HTTP yanıt kodu 4xx).|ApiName, OperationName, bölge|
|Dataın|Içindeki veriler|Bayt|Toplam|Gelen verilerin bayt cinsinden boyutu.|ApiName, OperationName, bölge|
|Veri çıkışı|Giden veriler|Bayt|Toplam|Giden verilerin bayt cinsinden boyutu.|ApiName, OperationName, bölge|
|Gecikme süresi|Gecikme süresi|Mayacak|Ortalama|Milisaniye cinsinden gecikme süresi.|ApiName, OperationName, bölge|
|Karakter Yabanslamuş|Çevrilen karakterler|Sayı|Toplam|Gelen metin isteğindeki toplam karakter sayısı.|ApiName, OperationName, bölge|
|Karakterçeli|Eğitilen karakterler|Sayı|Toplam|Eğitilen toplam karakter sayısı.|ApiName, OperationName, bölge|
|SpeechSessionDuration|Konuşma oturumu süresi|Saniye|Toplam|Konuşma oturumunun saniye cinsinden toplam süresi.|ApiName, OperationName, bölge|
|TotalTransactions|Toplam Işlem sayısı|Sayı|Toplam|Toplam işlem sayısı.|Hiçbiri|
|TotalTokenCalls|Toplam belirteç çağrısı sayısı|Sayı|Toplam|Toplam belirteç çağrısı sayısı.|ApiName, OperationName, bölge|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Hiçbiri|
|Ağ Girişi|Faturalanabilir (kullanım dışı) ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan faturalandırılabilir bayt sayısı (gelen trafik) (kullanım dışı)|Hiçbiri|
|Ağ Çıkışı|Ağdan çıkış faturalandırılabilir (kullanım dışı)|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı (giden trafik) (kullanım dışı)|Hiçbiri|
|Disk okuma bayt sayısı|Disk okuma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt sayısı|Hiçbiri|
|Disk yazma baytları|Disk yazma baytları|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt sayısı|Hiçbiri|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si|Hiçbiri|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si|Hiçbiri|
|Kalan CPU kredileri|Kalan CPU kredileri|Sayı|Ortalama|Patlama için kullanılabilen toplam kredi sayısı|Hiçbiri|
|Tüketilen CPU kredileri|Tüketilen CPU kredileri|Sayı|Ortalama|Sanal makine tarafından tüketilen toplam kredi sayısı|Hiçbiri|
|Disk başına okunan bayt/sn|Veri diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|SlotID|
|Disk başına yazma bayt/sn|Veri diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|SlotID|
|Disk başına okuma Işlemi/sn|Veri diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|SlotID|
|Disk başına yazma Işlemi/sn|Veri diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|SlotID|
|Disk başına QD|Veri diski QD (kullanım dışı)|Sayı|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotID|
|İşletim sistemi disk başına okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Hiçbiri|
|İşletim sistemi disk başına yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Hiçbiri|
|İşletim sistemi diski başına okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Hiçbiri|
|İşletim sistemi diski başına yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Hiçbiri|
|Disk başına işletim sistemi QD|İşletim sistemi diski QD (kullanım dışı)|Sayı|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Hiçbiri|
|Veri diski okuma bayt/sn|Veri diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|LUN|
|Veri diski yazma bayt/sn|Veri diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|LUN|
|Veri diski okuma Işlemi/sn|Veri diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|LUN|
|Veri diski yazma Işlemi/sn|Veri diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|LUN|
|Veri diski sıra derinliği|Veri diski sıra derinliği (Önizleme)|Sayı|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|LUN|
|İşletim sistemi diski okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Hiçbiri|
|İşletim sistemi diski yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Hiçbiri|
|İşletim sistemi diski okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Hiçbiri|
|İşletim sistemi diski yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Hiçbiri|
|İşletim sistemi diski sıra derinliği|İşletim sistemi diski sıra derinliği (Önizleme)|Sayı|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Hiçbiri|
|Gelen akışlar|Gelen akışlar|Sayı|Ortalama|Gelen akışlar, gelen yöndeki geçerli akışların sayısıdır (VM 'ye giden trafik)|Hiçbiri|
|Giden akışlar|Giden akışlar|Sayı|Ortalama|Giden akışlar giden yöndeki geçerli akış sayısıdır (VM 'den giden trafik)|Hiçbiri|
|Gelen akışlar en yüksek oluşturma oranı|Gelen akışlar en yüksek oluşturma oranı (Önizleme)|Sayaçpersaniye|Ortalama|Gelen akışların en yüksek oluşturma oranı (VM 'ye giden trafik)|Hiçbiri|
|Giden akış maksimum oluşturma oranı|Giden akışlar maksimum oluşturma oranı (Önizleme)|Sayaçpersaniye|Ortalama|Giden akışların en yüksek oluşturma oranı (VM 'den giden trafik)|Hiçbiri|
|Premium veri diski önbelleği okuma Isabeti|Premium veri diski önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabeti|LUN|
|Premium veri diski önbelleği okuma Isabetsizliği|Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabetsizliği|LUN|
|Premium işletim sistemi disk önbelleği okuma Isabeti|Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabeti|Hiçbiri|
|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Hiçbiri|
|Toplam ağ|Toplam ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik)|Hiçbiri|
|Toplam ağ çıkışı|Toplam ağ çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik)|Hiçbiri|


## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|VMName|
|Ağ Girişi|Faturalanabilir (kullanım dışı) ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan faturalandırılabilir bayt sayısı (gelen trafik) (kullanım dışı)|VMName|
|Ağ Çıkışı|Ağdan çıkış faturalandırılabilir (kullanım dışı)|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı (giden trafik) (kullanım dışı)|VMName|
|Disk okuma bayt sayısı|Disk okuma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt sayısı|VMName|
|Disk yazma baytları|Disk yazma baytları|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt sayısı|VMName|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si|VMName|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si|VMName|
|Kalan CPU kredileri|Kalan CPU kredileri|Sayı|Ortalama|Patlama için kullanılabilen toplam kredi sayısı|Hiçbiri|
|Tüketilen CPU kredileri|Tüketilen CPU kredileri|Sayı|Ortalama|Sanal makine tarafından tüketilen toplam kredi sayısı|Hiçbiri|
|Disk başına okunan bayt/sn|Veri diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|SlotID|
|Disk başına yazma bayt/sn|Veri diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|SlotID|
|Disk başına okuma Işlemi/sn|Veri diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|SlotID|
|Disk başına yazma Işlemi/sn|Veri diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|SlotID|
|Disk başına QD|Veri diski QD (kullanım dışı)|Sayı|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotID|
|İşletim sistemi disk başına okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Hiçbiri|
|İşletim sistemi disk başına yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Hiçbiri|
|İşletim sistemi diski başına okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Hiçbiri|
|İşletim sistemi diski başına yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Hiçbiri|
|Disk başına işletim sistemi QD|İşletim sistemi diski QD (kullanım dışı)|Sayı|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Hiçbiri|
|Veri diski okuma bayt/sn|Veri diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|LUN, VMName|
|Veri diski yazma bayt/sn|Veri diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|LUN, VMName|
|Veri diski okuma Işlemi/sn|Veri diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|LUN, VMName|
|Veri diski yazma Işlemi/sn|Veri diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|LUN, VMName|
|Veri diski sıra derinliği|Veri diski sıra derinliği (Önizleme)|Sayı|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|LUN, VMName|
|İşletim sistemi diski okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|VMName|
|İşletim sistemi diski yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|VMName|
|İşletim sistemi diski okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|VMName|
|İşletim sistemi diski yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|VMName|
|İşletim sistemi diski sıra derinliği|İşletim sistemi diski sıra derinliği (Önizleme)|Sayı|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|VMName|
|Gelen akışlar|Gelen akışlar|Sayı|Ortalama|Gelen akışlar, gelen yöndeki geçerli akışların sayısıdır (VM 'ye giden trafik)|VMName|
|Giden akışlar|Giden akışlar|Sayı|Ortalama|Giden akışlar giden yöndeki geçerli akış sayısıdır (VM 'den giden trafik)|VMName|
|Gelen akışlar en yüksek oluşturma oranı|Gelen akışlar en yüksek oluşturma oranı (Önizleme)|Sayaçpersaniye|Ortalama|Gelen akışların en yüksek oluşturma oranı (VM 'ye giden trafik)|VMName|
|Giden akış maksimum oluşturma oranı|Giden akışlar maksimum oluşturma oranı (Önizleme)|Sayaçpersaniye|Ortalama|Giden akışların en yüksek oluşturma oranı (VM 'den giden trafik)|VMName|
|Premium veri diski önbelleği okuma Isabeti|Premium veri diski önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabeti|LUN, VMName|
|Premium veri diski önbelleği okuma Isabetsizliği|Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabetsizliği|LUN, VMName|
|Premium işletim sistemi disk önbelleği okuma Isabeti|Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabeti|VMName|
|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|VMName|
|Toplam ağ|Toplam ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik)|VMName|
|Toplam ağ çıkışı|Toplam ağ çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik)|VMName|


## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|Sanal Makineler tarafından kullanılan ayrılmış işlem birimlerinin geçerli yüzdesi|Hiçbiri|
|Ağ Girişi|Faturalanabilir (kullanım dışı) ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan faturalandırılabilir bayt sayısı (gelen trafik) (kullanım dışı)|Hiçbiri|
|Ağ Çıkışı|Ağdan çıkış faturalandırılabilir (kullanım dışı)|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı (giden trafik) (kullanım dışı)|Hiçbiri|
|Disk okuma bayt sayısı|Disk okuma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt sayısı|Hiçbiri|
|Disk yazma baytları|Disk yazma baytları|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt sayısı|Hiçbiri|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si|Hiçbiri|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si|Hiçbiri|
|Kalan CPU kredileri|Kalan CPU kredileri|Sayı|Ortalama|Patlama için kullanılabilen toplam kredi sayısı|Hiçbiri|
|Tüketilen CPU kredileri|Tüketilen CPU kredileri|Sayı|Ortalama|Sanal makine tarafından tüketilen toplam kredi sayısı|Hiçbiri|
|Disk başına okunan bayt/sn|Veri diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|SlotID|
|Disk başına yazma bayt/sn|Veri diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|SlotID|
|Disk başına okuma Işlemi/sn|Veri diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|SlotID|
|Disk başına yazma Işlemi/sn|Veri diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|SlotID|
|Disk başına QD|Veri diski QD (kullanım dışı)|Sayı|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotID|
|İşletim sistemi disk başına okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Hiçbiri|
|İşletim sistemi disk başına yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Hiçbiri|
|İşletim sistemi diski başına okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Hiçbiri|
|İşletim sistemi diski başına yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Hiçbiri|
|Disk başına işletim sistemi QD|İşletim sistemi diski QD (kullanım dışı)|Sayı|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Hiçbiri|
|Veri diski okuma bayt/sn|Veri diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|LUN|
|Veri diski yazma bayt/sn|Veri diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|LUN|
|Veri diski okuma Işlemi/sn|Veri diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|LUN|
|Veri diski yazma Işlemi/sn|Veri diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|LUN|
|Veri diski sıra derinliği|Veri diski sıra derinliği (Önizleme)|Sayı|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|LUN|
|İşletim sistemi diski okuma bayt/sn|İşletim sistemi diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Hiçbiri|
|İşletim sistemi diski yazma bayt/sn|İşletim sistemi diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Hiçbiri|
|İşletim sistemi diski okuma Işlemi/sn|İşletim sistemi diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Hiçbiri|
|İşletim sistemi diski yazma Işlemi/sn|İşletim sistemi diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Hiçbiri|
|İşletim sistemi diski sıra derinliği|İşletim sistemi diski sıra derinliği (Önizleme)|Sayı|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Hiçbiri|
|Gelen akışlar|Gelen akışlar|Sayı|Ortalama|Gelen akışlar, gelen yöndeki geçerli akışların sayısıdır (VM 'ye giden trafik)|Hiçbiri|
|Giden akışlar|Giden akışlar|Sayı|Ortalama|Giden akışlar giden yöndeki geçerli akış sayısıdır (VM 'den giden trafik)|Hiçbiri|
|Gelen akışlar en yüksek oluşturma oranı|Gelen akışlar en yüksek oluşturma oranı (Önizleme)|Sayaçpersaniye|Ortalama|Gelen akışların en yüksek oluşturma oranı (VM 'ye giden trafik)|Hiçbiri|
|Giden akış maksimum oluşturma oranı|Giden akışlar maksimum oluşturma oranı (Önizleme)|Sayaçpersaniye|Ortalama|Giden akışların en yüksek oluşturma oranı (VM 'den giden trafik)|Hiçbiri|
|Premium veri diski önbelleği okuma Isabeti|Premium veri diski önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabeti|LUN|
|Premium veri diski önbelleği okuma Isabetsizliği|Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabetsizliği|LUN|
|Premium işletim sistemi disk önbelleği okuma Isabeti|Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabeti|Hiçbiri|
|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Hiçbiri|
|Toplam ağ|Toplam ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik)|Hiçbiri|
|Toplam ağ çıkışı|Toplam ağ çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik)|Hiçbiri|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft. Containerınstance/containerGroups

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuUsage|CPU kullanımı|Sayı|Ortalama|Miliçekirdekte tüm çekirdekler üzerinde CPU kullanımı.|Adı|
|MemoryUsage|Bellek kullanımı|Bayt|Ortalama|Bayt cinsinden toplam bellek kullanımı.|Adı|
|NetworkBytesReceivedPerSecond|Saniye başına alınan ağ bayt sayısı|Bayt|Ortalama|Saniye başına alınan ağ bayt sayısı.|Hiçbiri|
|NetworkBytesTransmittedPerSecond|Saniye başına aktarılan ağ baytları|Bayt|Ortalama|Saniye başına aktarılan ağ baytları.|Hiçbiri|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft. ContainerRegistry/kayıt defterleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalPullCount|Toplam çekme sayısı|Sayı|Ortalama|Toplam görüntü sayısı|Hiçbiri|
|Başarılı sayı|Başarılı çekme sayısı|Sayı|Ortalama|Başarılı görüntü sayısı|Hiçbiri|
|Toplam Pushcount|Toplam gönderme sayısı|Sayı|Ortalama|Toplam resim gönderim sayısı|Hiçbiri|
|Başarılı Pushcount|Başarılı gönderme sayısı|Sayı|Ortalama|Başarılı görüntü gönderme sayısı|Hiçbiri|
|RunDuration|Çalışma süresi|Milisaniye|Toplam|Çalışma süresi (milisaniye)|Hiçbiri|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft. ContainerService/Managedkümeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Yönetilen kümede toplam kullanılabilir CPU çekirdeği sayısı|Sayı|Ortalama|Yönetilen kümede toplam kullanılabilir CPU çekirdeği sayısı|Hiçbiri|
|kube_node_status_allocatable_memory_bytes|Yönetilen kümede toplam kullanılabilir bellek miktarı|Bayt|Ortalama|Yönetilen kümede toplam kullanılabilir bellek miktarı|Hiçbiri|
|kube_pod_status_ready|Hazırlık durumundaki Pod sayısı|Sayı|Ortalama|Hazırlık durumundaki Pod sayısı|ad alanı, Pod|
|kube_node_status_condition|Çeşitli düğüm koşullarına yönelik durumlar|Sayı|Ortalama|Çeşitli düğüm koşullarına yönelik durumlar|koşul, durum, status2, düğüm|
|kube_pod_status_phase|Aşamasına göre Pod sayısı|Sayı|Ortalama|Aşamasına göre Pod sayısı|aşama, ad alanı, Pod|



## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft. CustomProviders/resourceproviders

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Başarılı Istek sayısı|Başarılı Istekler|Sayı|Toplam|Özel sağlayıcı tarafından yapılan başarılı istekler|HttpMethod, CallPath, StatusCode|
|FailedRequests|Başarısız İstekler|Sayı|Toplam|Özel kaynak sağlayıcıları için kullanılabilir günlükleri alır|HttpMethod, CallPath, StatusCode|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft. DataBoxEdge/dataBoxEdgeDevices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Nicreadüretilen Iş|Okuma performansı (ağ)|BytesPerSecond|Ortalama|Cihazdaki ağ arabiriminin, ağ geçidinde bulunan tüm birimler için raporlama döneminde okuma performansı.|InstanceName|
|Nicwriteüretilen Iş|Yazma Işleme (ağ)|BytesPerSecond|Ortalama|Cihazdaki ağ arabiriminin, ağ geçidinde bulunan tüm birimler için raporlama döneminde yazma aktarım hızı.|InstanceName|
|CloudReadThroughputPerShare|Bulut Indirme verimlilik (paylaşma)|BytesPerSecond|Ortalama|Raporlama dönemi boyunca bir paylaşımdan Azure 'a aktarım hızını indirin.|Paylaşın|
|CloudUploadThroughputPerShare|Bulut karşıya yükleme üretilen Işi (paylaşma)|BytesPerSecond|Ortalama|Raporlama dönemi boyunca bir paylaşımdan Azure 'a yük aktarımını yükleme.|Paylaşın|
|BytesUploadedToCloudPerShare|Karşıya yüklenen bulut baytları (paylaşma)|Bayt|Ortalama|Raporlama dönemi boyunca bir paylaşımdan Azure 'a yüklenen toplam bayt sayısı.|Paylaşın|
|TotalCapacity|Toplam kapasite|Bayt|Ortalama|Toplam kapasite|Hiçbiri|
|AvailableCapacity|Kullanılabilir kapasite|Bayt|Ortalama|Raporlama dönemi boyunca kullanılabilir kapasite (bayt cinsinden).|Hiçbiri|
|Clouduploadüretilen Iş|Bulut karşıya yükleme performansı|BytesPerSecond|Ortalama|Raporlama dönemi boyunca Azure 'a bulut yükleme performansı.|Hiçbiri|
|Cloudreadüretilen Iş|Bulut Indirme performansı|BytesPerSecond|Ortalama|Raporlama döneminde Azure 'a bulut indirme performansı.|Hiçbiri|
|BytesUploadedToCloud|Karşıya yüklenen bulut baytları (cihaz)|Bayt|Ortalama|Raporlama dönemi boyunca bir cihazdan Azure 'a yüklenen toplam bayt sayısı.|Hiçbiri|
|Hypervvirtualprocessorkullanım|Edge hesaplama-CPU yüzdesi|Yüzde|Ortalama|CPU kullanımı yüzdesi|InstanceName|
|Hipervmemoryutilileştirme|Edge Işlem-bellek kullanımı|Yüzde|Ortalama|Kullanımdaki RAM miktarı|InstanceName|


## <a name="microsoftdatafactorydatafactories"></a>Microsoft. DataFactory/DataFactory

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Failedçalıştırmaları|Başarısız çalıştırmalar|Sayı|Toplam||Ardışık düzen ınename, activityName|
|Başarılı çalıştırmalar|Başarılı çalıştırmalar|Sayı|Toplam||Ardışık düzen ınename, activityName|


## <a name="microsoftdatafactoryfactories"></a>Microsoft. DataFactory/Factory

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Pipelinefailedçalıştırmaları|Başarısız işlem hattı çalıştırmaları ölçümleri|Sayı|Toplam||Failuıyeniden yazma, ad|
|Ardışık düzen Inesucceededçalıştırmaları|Başarılı işlem hattı çalışma ölçümleri|Sayı|Toplam||Failuıyeniden yazma, ad|
|Ardışık düzen ınecancelledçalıştırmaları|İptal edilen işlem hattı çalışma ölçümleri|Sayı|Toplam||Failuıyeniden yazma, ad|
|Activityfailedçalıştırmaları|Başarısız etkinlik çalıştırma ölçümleri|Sayı|Toplam||ActivityType, ardışık düzen adı, Failuyeniden yazma, ad|
|ActivitySucceededRuns|Başarılı etkinlik ölçümleri çalıştırıyor|Sayı|Toplam||ActivityType, ardışık düzen adı, Failuyeniden yazma, ad|
|Activitycancelledçalıştırmaları|İptal edilen etkinlik ölçümleri çalıştırıyor|Sayı|Toplam||ActivityType, ardışık düzen adı, Failuyeniden yazma, ad|
|Triggerfailedçalıştırmaları|Başarısız tetikleyici çalıştırma ölçümleri|Sayı|Toplam||Ad, Failuyeniden yazma|
|TriggerSucceededRuns|Başarılı tetikleyici çalışan ölçümleri|Sayı|Toplam||Ad, Failuyeniden yazma|
|Triggercancelledçalıştırmaları|İptal edilen tetikleyici çalışan ölçümleri|Sayı|Toplam||Ad, Failuyeniden yazma|
|Integrationruntimecpuyüzdesi|Tümleştirme çalışma zamanı CPU kullanımı|Yüzde|Ortalama||Integrationruntimename, düğ,|
|Integrationruntimekullanılabilirliği Blememory|Tümleştirme çalışma zamanı kullanılabilir belleği|Bayt|Ortalama||Integrationruntimename, düğ,|
|Integrationruntimeaveragetaskpickupdelay|Tümleştirme çalışma zamanı sıra süresi|Saniye|Ortalama||Integrationruntimename|
|Integrationruntimequeuelength|Tümleştirme çalışma zamanı sıra uzunluğu|Sayı|Ortalama||Integrationruntimename|
|IntegrationRuntimeAvailableNodeNumber|Tümleştirme çalışma zamanı kullanılabilir düğüm sayısı|Sayı|Ortalama||Integrationruntimename|
|MaxAllowedResourceCount|İzin verilen en fazla varlık sayısı|Sayı|Maksimum||Hiçbiri|
|Maxallowedfactorysizeingbits|İzin verilen en yüksek fabrika boyutu (GB birimi)|Sayı|Maksimum||Hiçbiri|
|ResourceCount|Toplam varlık sayısı|Sayı|Maksimum||Hiçbiri|
|Factorysizeingbits|Toplam fabrika boyutu (GB birimi)|Sayı|Maksimum||Hiçbiri|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|JobEndedSuccess|Başarılı Işler|Sayı|Toplam|Başarılı iş sayısı.|Hiçbiri|
|JobEndedFailure|Başarısız Işler|Sayı|Toplam|Başarısız iş sayısı.|Hiçbiri|
|Jobendediptal edildi|İptal edilen Işler|Sayı|Toplam|İptal edilen işlerin sayısı.|Hiçbiri|
|JobAUEndedSuccess|Başarılı AU Saati|Saniye|Toplam|Başarılı işler için toplam AU süresi.|Hiçbiri|
|JobAUEndedFailure|Başarısız AU Saati|Saniye|Toplam|Başarısız işler için toplam AU süresi.|Hiçbiri|
|Jobauendediptal edildi|İptal edilen AU Saati|Saniye|Toplam|İptal edilen işler için toplam AU süresi.|Hiçbiri|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TotalStorage|Toplam Depolama Alanı|Bayt|Maksimum|Hesapta depolanan toplam veri miktarı.|Hiçbiri|
|Veri yazıldı|Yazılan veriler|Bayt|Toplam|Hesaba yazılan toplam veri miktarı.|Hiçbiri|
|DataRead|Okunan veriler|Bayt|Toplam|Hesaptan okunan toplam veri miktarı.|Hiçbiri|
|WriteRequests|Yazma Istekleri|Sayı|Toplam|Hesaba veri yazma isteği sayısı.|Hiçbiri|
|ReadRequests|Okuma Istekleri|Sayı|Toplam|Hesaba yönelik veri okuma isteklerinin sayısı.|Hiçbiri|


## <a name="microsoftdbformariadbservers"></a>Microsoft. Dbformarıdb/sunucular

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiçbiri|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Hiçbiri|
|io_consumption_percent|GÇ yüzdesi|Yüzde|Ortalama|GÇ yüzdesi|Hiçbiri|
|storage_percent|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Hiçbiri|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Hiçbiri|
|storage_limit|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|Hiçbiri|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Hiçbiri|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Bayt|Ortalama|Kullanılan sunucu günlüğü depolaması|Hiçbiri|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Bayt|Ortalama|Sunucu günlüğü depolama sınırı|Hiçbiri|
|active_connections|Etkin bağlantılar|Sayı|Ortalama|Etkin bağlantılar|Hiçbiri|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|Hiçbiri|
|seconds_behind_master|Saniye cinsinden çoğaltma gecikmesi|Sayı|Maksimum|Saniye cinsinden çoğaltma gecikmesi|Hiçbiri|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Ortalama|Kullanılan yedekleme depolama alanı|Hiçbiri|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Hiçbiri|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Hiçbiri|


## <a name="microsoftdbformysqlservers"></a>Microsoft. Dbformyısql/sunucuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiçbiri|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Hiçbiri|
|io_consumption_percent|GÇ yüzdesi|Yüzde|Ortalama|GÇ yüzdesi|Hiçbiri|
|storage_percent|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Hiçbiri|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Hiçbiri|
|storage_limit|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|Hiçbiri|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Hiçbiri|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Bayt|Ortalama|Kullanılan sunucu günlüğü depolaması|Hiçbiri|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Bayt|Maksimum|Sunucu günlüğü depolama sınırı|Hiçbiri|
|active_connections|Etkin bağlantılar|Sayı|Ortalama|Etkin bağlantılar|Hiçbiri|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|Hiçbiri|
|seconds_behind_master|Saniye cinsinden çoğaltma gecikmesi|Sayı|Maksimum|Saniye cinsinden çoğaltma gecikmesi|Hiçbiri|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Ortalama|Kullanılan yedekleme depolama alanı|Hiçbiri|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Hiçbiri|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Hiçbiri|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft. DBforPostgreSQL/sunucuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiçbiri|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Hiçbiri|
|io_consumption_percent|GÇ yüzdesi|Yüzde|Ortalama|GÇ yüzdesi|Hiçbiri|
|storage_percent|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Hiçbiri|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Hiçbiri|
|storage_limit|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|Hiçbiri|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Hiçbiri|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Bayt|Ortalama|Kullanılan sunucu günlüğü depolaması|Hiçbiri|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Bayt|Maksimum|Sunucu günlüğü depolama sınırı|Hiçbiri|
|active_connections|Etkin bağlantılar|Sayı|Ortalama|Etkin bağlantılar|Hiçbiri|
|connections_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|Hiçbiri|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Ortalama|Kullanılan yedekleme depolama alanı|Hiçbiri|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Hiçbiri|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Hiçbiri|
|pg_replica_log_delay_in_seconds|Çoğaltma gecikmesi|Saniye|Maksimum|Saniye cinsinden çoğaltma gecikmesi|Hiçbiri|
|pg_replica_log_delay_in_bytes|Çoğaltmalar genelinde en fazla gecikme|Bayt|Maksimum|En fazla çoğaltma çoğaltmasının bayt cinsinden gecikmesi|Hiçbiri|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft. DBforPostgreSQL/serversv2

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiçbiri|
|memory_percent|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Hiçbiri|
|'ye|IOPS|Sayı|Ortalama|GÇ Işlemi/saniye|Hiçbiri|
|storage_percent|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Hiçbiri|
|storage_used|Kullanılan depolama|Bayt|Ortalama|Kullanılan depolama|Hiçbiri|
|active_connections|Etkin bağlantılar|Sayı|Ortalama|Etkin bağlantılar|Hiçbiri|
|network_bytes_egress|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Hiçbiri|
|network_bytes_ingress|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Hiçbiri|





## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/ıothubs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetri iletisi gönderme denemeleri|Sayı|Toplam|IoT Hub 'ınıza gönderilmeye çalışılan cihazdan buluta telemetri iletisi sayısı|Hiçbiri|
|D2C. telemetri. giriş. başarılı|Gönderilen telemetri iletileri|Sayı|Toplam|IoT Hub 'ınıza başarıyla gönderilen cihazdan buluta telemetri iletisi sayısı|Hiçbiri|
|c2d.commands.egress.complete.success|C2D ileti teslimleri tamamlandı|Sayı|Toplam|Cihaz tarafından başarıyla tamamlanan buluttan cihaza ileti teslimleri sayısı|Hiçbiri|
|c2d.commands.egress.abandon.success|C2D iletileri bırakıldı|Sayı|Toplam|Cihaz tarafından bırakılan buluttan cihaza yönelik ileti sayısı|Hiçbiri|
|c2d.commands.egress.reject.success|C2D iletileri reddedildi|Sayı|Toplam|Cihaz tarafından reddedilen buluttan cihaza yönelik ileti sayısı|Hiçbiri|
|C2DMessagesExpired|C2D Iletilerinin süre dolma (Önizleme)|Sayı|Toplam|Zaman aşımına uğradı buluttan cihaza ileti sayısı|Hiçbiri|
|Devices. totalDevices|Toplam cihaz (kullanım dışı)|Sayı|Toplam|IoT Hub 'ınıza kayıtlı cihazların sayısı|Hiçbiri|
|devices.connectedDevices.allProtocol|Bağlı cihazlar (kullanım dışı) |Sayı|Toplam|IoT Hub 'ınıza bağlı cihazların sayısı|Hiçbiri|
|D2C. telemetri. çıkış. başarılı|Yönlendirme: teslim edilen telemetri iletileri|Sayı|Toplam|IoT Hub yönlendirme kullanılarak iletilerin tüm uç noktalara başarıyla ulaştırılan ileti sayısı. Bir ileti birden çok uç noktaya yönlendirilmemişse, her başarılı teslimat için bu değer bir artar. Bir ileti aynı uç noktaya birden çok kez teslim edildiğinde, her başarılı teslimat için bu değer bir artar.|Hiçbiri|
|D2C. telemetri. çıkış. bırakıldı|Yönlendirme: telemetri iletileri bırakıldı |Sayı|Toplam|İleti, ölü uç noktalar nedeniyle IoT Hub yönlendirmenin kaç kez bırakılmakta olduğunu. Bu değer, bırakılan iletiler buraya teslim edimediğinden geri dönüş rotasına teslim edilen iletileri saymaz.|Hiçbiri|
|d2c.telemetry.egress.orphaned|Yönlendirme: telemetri iletileri yalnız bırakılmış |Sayı|Toplam|Herhangi bir yönlendirme kuralıyla eşleşmediğinden (geri dönüş kuralı dahil) iletilerin IoT Hub yönlendirme tarafından yalnız bırakılmış olduğu zaman sayısı. |Hiçbiri|
|D2C. telemetri. çıkış. geçersiz|Yönlendirme: telemetri iletileri uyumsuz|Sayı|Toplam|Uç nokta ile uyumsuzluk nedeniyle IoT Hub yönlendirmenin iletileri teslim etme başarısız olduğu zaman sayısı. Bu değer yeniden denemeler içermez.|Hiçbiri|
|d2c.telemetry.egress.fallback|Yönlendirme: geri dönüşe teslim edilen iletiler|Sayı|Toplam|Geri dönüş rotası ile ilişkili uç noktaya gönderilen iletileri yönlendirmenin IoT Hub sayısı.|Hiçbiri|
|D2C. endpoints. çıkış. eventHubs|Yönlendirme: Olay Hub 'ına teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmenin Olay Hub 'ı uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Hiçbiri|
|D2C. endpoints. Latency. eventHubs|Yönlendirme: Olay Hub 'ı için ileti gecikmesi|Milisaniye|Ortalama|İleti girişi ile bir olay hub 'ı uç noktasına giriş IoT Hub ve ileti girişi arasındaki ortalama gecikme (milisaniye).|Hiçbiri|
|d2c.endpoints.egress.serviceBusQueues|Yönlendirme: Service Bus kuyruğuna teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmenin iletileri Service Bus sıra uç noktalarına başarıyla teslim dağıttığı zaman sayısı.|Hiçbiri|
|d2c.endpoints.latency.serviceBusQueues|Yönlendirme: Service Bus kuyruğu için ileti gecikmesi|Milisaniye|Ortalama|İleti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye) Service Bus kuyruğu uç noktasına giriş.|Hiçbiri|
|d2c.endpoints.egress.serviceBusTopics|Yönlendirme: Service Bus konuya teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmenin Service Bus konu uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Hiçbiri|
|d2c.endpoints.latency.serviceBusTopics|Yönlendirme: Service Bus konusu için ileti gecikmesi|Milisaniye|Ortalama|İleti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye) Service Bus konu uç noktasına giriş.|Hiçbiri|
|D2C. endpoints. çıkış. builtIn. Events|Yönlendirme: iletilere/olaylara teslim edilen iletiler|Sayı|Toplam|Yönlendirmenin, yerleşik uç noktaya (iletiler/olaylar) başarıyla ileti teslim IoT Hub sayısı.|Hiçbiri|
|D2C. endpoints. Latency. yerleik. Events|Yönlendirme: iletiler/olaylar için ileti gecikmesi|Milisaniye|Ortalama|Yerleşik uç noktaya (iletiler/olaylar) gelen ileti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye).|Hiçbiri|
|D2C. endpoints. çıkış. Storage|Yönlendirme: depolamaya teslim edilen iletiler|Sayı|Toplam|IoT Hub yönlendirmenin depolama uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Hiçbiri|
|D2C. endpoints. Latency. Storage|Yönlendirme: depolama için ileti gecikmesi|Milisaniye|Ortalama|İleti girişi IoT Hub ve telemetri iletisine bir depolama uç noktasına giriş arasındaki ortalama gecikme süresi (milisaniye).|Hiçbiri|
|D2C. endpoints. çıkış. Storage. Bytes|Yönlendirme: depolamaya teslim edilen veriler|Bayt|Toplam|Depolama uç noktalarına teslim edilen veri miktarı (bayt) IoT Hub.|Hiçbiri|
|D2C. endpoints. çıkış. Storage. blob 'ları|Yönlendirme: depolamaya teslim edilen Bloblar|Sayı|Toplam|IoT Hub blob 'ları depolama uç noktalarına yönlendirmenin kaç kez yönlendirtığı sayısıdır.|Hiçbiri|
|Eventgridteslimler|Event Grid teslimler (Önizleme)|Sayı|Toplam|Event Grid yayımlanan IoT Hub olaylarının sayısı. Başarılı ve başarısız isteklerin sayısı için sonuç boyutunu kullanın. EventType boyutu olay türünü gösterir (https://aka.ms/ioteventgrid).|RESOURCEID, sonuç, EventType|
|EventGridLatency|Event Grid gecikme süresi (Önizleme)|Milisaniye|Ortalama|Olay Event Grid yayımlandığında IoT Hub olayının oluşturulduğu sürenin ortalama gecikme süresi (milisaniye). Bu sayı, tüm olay türleri arasındaki bir ortalama değer. Belirli bir olay türünün gecikmesini görmek için EventType boyutunu kullanın.|RESOURCEID, EventType|
|d2c.twin.read.success|Cihazlardan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi okuma sayısı.|Hiçbiri|
|d2c.twin.read.failure|Cihazlardan başarısız ikizi okumaları|Sayı|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|D2C. ikizi. Read. size|Cihazlardan gelen ikizi okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikizi okumalarının ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|D2C. ikizi. Update. Success|Cihazlardan başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|d2c.twin.update.failure|Cihazlardan ikizi güncelleştirmeleri başarısız oldu|Sayı|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|d2c.twin.update.size|Cihazlardan ikizi güncelleştirmelerinin boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin ortalama, en düşük ve en büyük boyutu.|Hiçbiri|
|c2d.methods.success|Başarılı doğrudan yöntem etkinleştirmeleri|Sayı|Toplam|Tüm başarılı doğrudan yöntem çağrılarının sayısı.|Hiçbiri|
|C2D. Methods. Failure|Başarısız doğrudan yöntem etkinleştirmeleri|Sayı|Toplam|Tüm başarısız doğrudan yöntem çağrılarının sayısı.|Hiçbiri|
|c2d.methods.requestSize|Doğrudan yöntem etkinleştirmeleri istek boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem isteklerinin ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|c2d.methods.responseSize|Doğrudan yöntem etkinleştirmeleri yanıt boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem yanıtlarının ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|c2d.twin.read.success|Arka uçtan başarılı ikizi okumaları|Sayı|Toplam|Tüm başarılı arka uç tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|C2D. ikizi. Read. Failure|Arka uçtan başarısız ikizi okumaları|Sayı|Toplam|Tüm başarısız arka uç tarafından başlatılan ikizi okumaların sayısı.|Hiçbiri|
|C2D. ikizi. Read. size|Arka uçtan gelen ikizi okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı arka uç başlatılan ikizi okumalarının ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|C2D. ikizi. Update. Success|Arka uçtan başarılı ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|C2D. ikizi. Update. Failure|Arka uçtan başarısız ikizi güncelleştirmeleri|Sayı|Toplam|Tüm başarısız arka uç başlatılan ikizi güncelleştirmelerinin sayısı.|Hiçbiri|
|c2d.twin.update.size|Arka uçtan ikizi güncelleştirmelerinin boyutu|Bayt|Ortalama|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin ortalama, en düşük ve en büyük boyutu.|Hiçbiri|
|twinQueries. Success|Başarılı ikizi sorguları|Sayı|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Hiçbiri|
|twinQueries. Failure|Başarısız ikizi sorguları|Sayı|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Hiçbiri|
|twinQueries.resultSize|İkizi sorguları sonuç boyutu|Bayt|Ortalama|Tüm başarılı ikizi sorgularının sonuç boyutunun ortalama, en düşük ve en yüksek değeri.|Hiçbiri|
|jobs.createTwinUpdateJob.success|İkizi Update işlerinin başarılı oluşturmaları|Sayı|Toplam|Tüm başarılı ikizi Update işlerinin oluşturma sayısı.|Hiçbiri|
|jobs.createTwinUpdateJob.failure|İkizi Update işlerinin başarısız oluşturmaları|Sayı|Toplam|İkizi Update işlerinin başarısız olan tüm oluşturma sayısı.|Hiçbiri|
|Jobs. createDirectMethodJob. Success|Yöntem çağırma işlerinin başarılı oluşturmaları|Sayı|Toplam|Tüm başarılı doğrudan yöntem çağırma işlerinin oluşturulma sayısı.|Hiçbiri|
|Jobs. createDirectMethodJob. Failure|Yöntem çağırma işlerinin oluşturma işlemi başarısız oldu|Sayı|Toplam|Doğrudan yöntem çağırma işlerinin tüm başarısız oluşturma sayısı.|Hiçbiri|
|işler. listJobs. başarılı|İşleri listelemek için başarılı çağrılar|Sayı|Toplam|İşleri listelemek için başarılı olan tüm çağrıların sayısı.|Hiçbiri|
|Jobs. listJobs. hata|İşleri listelemek için başarısız çağrılar|Sayı|Toplam|Listeleme işleri için başarısız olan tüm çağrıların sayısı.|Hiçbiri|
|Jobs. cancelJob. Success|Başarılı iş iptalleri|Sayı|Toplam|İşi iptal etmek için tüm başarılı çağrı sayısı.|Hiçbiri|
|Jobs. cancelJob. Failure|Başarısız iş iptalleri|Sayı|Toplam|İşi iptal etmek için başarısız olan tüm çağrıların sayısı.|Hiçbiri|
|işler. queryJobs. başarılı|Başarılı iş sorguları|Sayı|Toplam|Sorgu işlerine yapılan tüm başarılı çağrıların sayısı.|Hiçbiri|
|Jobs. queryJobs. hata|Başarısız iş sorguları|Sayı|Toplam|Sorgu işlerine yapılan tüm başarısız çağrıların sayısı.|Hiçbiri|
|işler. tamamlandı|Tamamlanan İşler|Sayı|Toplam|Tamamlanan tüm işlerin sayısı.|Hiçbiri|
|işler. başarısız|Başarısız işler|Sayı|Toplam|Başarısız olan tüm işlerin sayısı.|Hiçbiri|
|d2c.telemetry.ingress.sendThrottle|Daraltma hatası sayısı|Sayı|Toplam|Cihaz performansı azaltmasından kaynaklanan azaltma hatası sayısı|Hiçbiri|
|Günlükmessagequotakullanıldı|Kullanılan toplam ileti sayısı|Sayı|Ortalama|Bugün kullanılan toplam ileti sayısı|Hiçbiri|
|deviceDataUsage|Toplam cihaz verisi kullanımı|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Hiçbiri|
|deviceDataUsageV2|Toplam cihaz verisi kullanımı (Önizleme)|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Hiçbiri|
|totalDeviceCount|Toplam cihaz (Önizleme)|Sayı|Ortalama|IoT Hub 'ınıza kayıtlı cihazların sayısı|Hiçbiri|
|connectedDeviceCount|Bağlı cihazlar (Önizleme)|Sayı|Ortalama|IoT Hub 'ınıza bağlı cihazların sayısı|Hiçbiri|
|konfigürasyonları|Yapılandırma ölçümleri|Sayı|Toplam|Yapılandırma Işlemleri için ölçümler|Hiçbiri|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft. Devices/provisioningServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Registrationdenemeleri|Kayıt denemeleri|Sayı|Toplam|Denenen cihaz kaydı sayısı|ProvisioningServiceName, IotHubName, durum|
|Deviceasyleri|Atanan cihazlar|Sayı|Toplam|IoT Hub 'ına atanan cihazların sayısı|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Kanıtlama denemeleri|Sayı|Toplam|Denenen cihaz belirlediğimizi karşıladığımızı sayısı|ProvisioningServiceName, durum, protokol|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft. DocumentDB/databaseAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AddRegion|Eklenen bölge|Sayı|Sayı|Eklenen bölge|Bölge|
|AvailableStorage|Kullanılabilir depolama alanı|Bayt|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam kullanılabilir depolama alanı|CollectionName, DatabaseName, bölgesi|
|Cassandraconnectionkapanışları|Cassandra bağlantı kapanışları|Sayı|Toplam|1 dakikalık ayrıntı düzeyinde raporlanan, kapatılan Cassandra bağlantısı sayısı|Apitürü, Region, ClosureReason|
|Cassandrarequestücretleri|Cassandra Istek ücretleri|Sayı|Toplam|Cassandra istekleri için tüketilen RUs|Apitürü, DatabaseName, CollectionName, bölge, OperationType, ResourceType|
|CassandraRequests|Cassandra Istekleri|Sayı|Sayı|Yapılan Cassandra isteği sayısı|Apitürü, DatabaseName, CollectionName, bölge, OperationType, ResourceType, ErrorCode|
|CreateAccount|Hesap oluşturuldu|Sayı|Sayı|Hesap oluşturuldu|Hiçbiri|
|Veri kullanımı|Veri Kullanımı|Bayt|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam veri kullanımı|CollectionName, DatabaseName, bölgesi|
|DeleteAccount|Hesap silindi|Sayı|Sayı|Hesap silindi|Hiçbiri|
|DocumentCount|Belge sayısı|Sayı|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam belge sayısı|CollectionName, DatabaseName, bölgesi|
|DocumentQuota|Belge kotası|Bayt|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam depolama kotası|CollectionName, DatabaseName, bölgesi|
|Indexusage|Dizin kullanımı|Bayt|Toplam|5 dakika ayrıntı düzeyi olarak raporlanan toplam dizin kullanımı|CollectionName, DatabaseName, bölgesi|
|Meta veri datarequests|Meta veri Istekleri|Sayı|Sayı|Meta veri isteklerinin sayısı. Cosmos DB, her bir hesap için sistem meta veri toplamayı korur, bu da koleksiyonları, veritabanlarını, vb. ve bunların yapılandırmasını, ücretsiz olarak listeletmenize olanak tanır.|DatabaseName, CollectionName, bölge, StatusCode, rol|
|MongoRequestCharge|Mongo Istek ücreti|Sayı|Toplam|Tüketilen Mongo Istek birimleri|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequests|Mongo Istekleri|Sayı|Sayı|Yapılan Mongo Isteği sayısı|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsCount|Mongo Istek hızı|Sayaçpersaniye|Ortalama|Mongo istek sayısı/saniye|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsDelete|Mongo silme Isteği hızı|Sayaçpersaniye|Ortalama|Mongo silme isteği/saniye|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|Mongorequestsınsert|Mongo ekleme Isteği oranı|Sayaçpersaniye|Ortalama|Mongo ekleme sayısı/saniye|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsQuery|Mongo sorgu Isteği hızı|Sayaçpersaniye|Ortalama|Mongo sorgu isteği/saniye|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequestsUpdate|Mongo güncelleştirme Isteği hızı|Sayaçpersaniye|Ortalama|Saniye başına Mongo güncelleştirme isteği|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|Provisionedüretilen Iş|Sağlanan Aktarım Hızı|Sayı|Maksimum|Sağlanan Aktarım Hızı|DatabaseName, CollectionName|
|RegionFailover|Yük devredilen bölge|Sayı|Sayı|Yük devredilen bölge|Hiçbiri|
|RemoveRegion|Bölge kaldırıldı|Sayı|Sayı|Bölge kaldırıldı|Bölge|
|ReplicationLatency|P99 çoğaltma gecikmesi|Mayacak|Ortalama|Coğrafi olarak etkinleştirilen hesap için kaynak ve hedef bölgelerde P99 çoğaltma gecikme süresi|SourceRegion, TargetRegion|
|ServiceAvailability|Hizmet kullanılabilirliği|Yüzde|Ortalama|Hesap istekleri bir saat, gün veya ay ayrıntı düzeyinde kullanılabilirlik|Hiçbiri|
|TotalRequestUnits|Toplam Istek birimleri|Sayı|Toplam|Tüketilen istek birimleri|DatabaseName, CollectionName, bölge, StatusCode, OperationType|
|Toplam Istek sayısı|Toplam İstek Sayısı|Sayı|Sayı|Yapılan istek sayısı|DatabaseName, CollectionName, bölge, StatusCode, OperationType|
|UpdateAccountKeys|Hesap anahtarları güncelleştirildi|Sayı|Sayı|Hesap anahtarları güncelleştirildi|Anahtar|
|UpdateAccountNetworkSettings|Hesap Ağ ayarları güncelleştirildi|Sayı|Sayı|Hesap Ağ ayarları güncelleştirildi|Hiçbiri|
|UpdateAccountReplicationSettings|Hesap çoğaltma ayarları güncelleştirildi|Sayı|Sayı|Hesap çoğaltma ayarları güncelleştirildi|Hiçbiri|



## <a name="microsoftenterpriseknowledgegraphservices"></a>Microsoft. EnterpriseKnowledgeGraph/hizmetler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Işlem sayısı|İşlem sayısı|Sayı|Sayı|Toplam Işlem sayısı|Işlem sayısı|
|Başarılı sayısı|Başarı sayısı|Sayı|Sayı|Başarılı Işlem sayısı|Başarılı sayısı|
|FailureCount|Hata sayısı|Sayı|Sayı|Başarısız Işlem sayısı|FailureCount|
|Başarılı gecikme|Başarı gecikmesi|Mayacak|Ortalama|Başarılı Işlemlerin gecikmesi|Başarılı sayısı|

## <a name="microsofteventgriddomains"></a>Microsoft. EventGrid/Domains

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Sayı|Toplam|Bu konuda yayınlanan toplam olay sayısı|Konu başlığı|
|PublishFailCount|Başarısız olayları Yayımla|Sayı|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|Konu, ErrorType, hata|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Milisaniye|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Hiçbiri|
|MatchedEventCount|Eşleşen olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|Ityattemptfailcount|Teslim başarısız olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, hata, ErrorType|
|Deliverybaşarılı sayısı|Teslim edilen olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Hedef Işleme süresi|Milisaniye|Ortalama|Milisaniye cinsinden hedef işleme süresi|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Bırakılan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|DeadLetteredCount|Kullanılmayan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|

## <a name="microsofteventgridtopics"></a>Microsoft. EventGrid/konuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Sayı|Toplam|Bu konuda yayınlanan toplam olay sayısı|Hiçbiri|
|PublishFailCount|Başarısız olayları Yayımla|Sayı|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olaylar|Sayı|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Hiçbiri|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Milisaniye|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Hiçbiri|
|MatchedEventCount|Eşleşen olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|Ityattemptfailcount|Teslim başarısız olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Teslim edilen olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|Hedef Işleme süresi|Milisaniye|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Bırakılan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|DeadLetteredCount|Kullanılmayan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft. EventGrid/Eventabonelikleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|MatchedEventCount|Eşleşen olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Hiçbiri|
|Ityattemptfailcount|Teslim başarısız olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType|
|Deliverybaşarılı sayısı|Teslim edilen olaylar|Sayı|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Hiçbiri|
|DestinationProcessingDurationInMs|Hedef Işleme süresi|Milisaniye|Ortalama|Milisaniye cinsinden hedef işleme süresi|Hiçbiri|
|DroppedEventCount|Bırakılan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason|
|DeadLetteredCount|Kullanılmayan olaylar|Sayı|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft. EventGrid/Extensionkonuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Publishbaşarılı sayısı|Yayımlanan olaylar|Sayı|Toplam|Bu konuda yayınlanan toplam olay sayısı|Hiçbiri|
|PublishFailCount|Başarısız olayları Yayımla|Sayı|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|UnmatchedEventCount|Eşleşmeyen olaylar|Sayı|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Hiçbiri|
|Publishbaşarıyla Latencınms|Yayımlama başarı gecikmesi|Milisaniye|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Hiçbiri|



## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı Istekler|Sayı|Toplam|Microsoft. EventHub için başarılı Istekler.|EntityName, OperationResult|
|ServerErrors|Sunucu hataları.|Sayı|Toplam|Microsoft. EventHub için sunucu hataları.|EntityName, OperationResult|
|UserErrors|Kullanıcı hataları.|Sayı|Toplam|Microsoft. EventHub için Kullanıcı hataları.|EntityName, OperationResult|
|QuotaExceededErrors|Kota, hataları aştı.|Sayı|Toplam|Microsoft. EventHub için Kota aşıldı.|EntityName, OperationResult|
|OperationName|Kısıtlanmış Istekler.|Sayı|Toplam|Microsoft. EventHub için kısıtlanan Istekler.|EntityName, OperationResult|
|Incomingrequests|Gelen Istekler|Sayı|Toplam|Microsoft. EventHub için gelen Istekler.|EntityName|
|Incomingmessages|Gelen Iletiler|Sayı|Toplam|Microsoft. EventHub için gelen Iletiler.|EntityName|
|OutgoingMessages|Giden Iletiler|Sayı|Toplam|Microsoft. EventHub için giden Iletiler.|EntityName|
|Incomingbytes|Gelen bayt sayısı.|Bayt|Toplam|Microsoft. EventHub için gelen baytlar.|EntityName|
|OutgoingBytes|Giden bayt sayısı.|Bayt|Toplam|Microsoft. EventHub için giden baytlar.|EntityName|
|ActiveConnection sayısı|ActiveConnection sayısı|Sayı|Ortalama|Microsoft. EventHub için toplam etkin bağlantı sayısı.|Hiçbiri|
|ConnectionsOpened|Bağlantılar açıldı.|Sayı|Ortalama|Microsoft. EventHub için açılan bağlantılar.|EntityName|
|ConnectionsClosed|Bağlantı kapatıldı.|Sayı|Ortalama|Microsoft. EventHub için kapatılan bağlantılar.|EntityName|
|CaptureBacklog|Biriktirme listesini yakala.|Sayı|Toplam|Microsoft. EventHub için yakalama biriktirme listesi.|EntityName|
|CapturedMessages|Yakalanan Iletiler.|Sayı|Toplam|Microsoft. EventHub için yakalanan Iletiler.|EntityName|
|CapturedBytes|Yakalanan baytlar.|Bayt|Toplam|Microsoft. EventHub için yakalanan baytlar.|EntityName|
|Boyut|Boyut|Bayt|Ortalama|Bir EventHub 'ın bayt cinsinden boyutu.|EntityName|
|INREQS|Gelen Istekler (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam gelen gönderme isteği sayısı (kullanım dışı)|Hiçbiri|
|SUCCREQ|Başarılı Istekler (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam başarılı istek (kullanım dışı)|Hiçbiri|
|FAILREQ|Başarısız Istekler (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam başarısız istek (kullanım dışı)|Hiçbiri|
|SVRBSY|Sunucu meşgul hataları (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam sunucu meşgul hatası (kullanım dışı)|Hiçbiri|
|INTERR|İç sunucu hataları (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam iç sunucu hatası (kullanım dışı)|Hiçbiri|
|Hatalı cerr|Diğer hatalar (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam başarısız istek (kullanım dışı)|Hiçbiri|
|Giriş iletileri|Gelen Iletiler (eski) (kullanım dışı)|Sayı|Toplam|Bir ad alanı için gelen toplam ileti sayısı. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine gelen Iletiler ölçümünü kullanın (kullanım dışı)|Hiçbiri|
|EHINILETISI|Gelen Iletiler (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam gelen ileti (kullanım dışı)|Hiçbiri|
|Dış mesaj|Giden Iletiler (eski) (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam giden ileti. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine giden Iletiler ölçümünü kullanın (kullanım dışı)|Hiçbiri|
|EHODıKILETISI|Giden Iletiler (kullanım dışı)|Sayı|Toplam|Bir ad alanı için toplam giden ileti (kullanım dışı)|Hiçbiri|
|EHINMB|Gelen bayt (geçersiz) (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı gelen ileti işleme. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine gelen bayt ölçümünü kullanın (kullanım dışı)|Hiçbiri|
|EHINBYTES|Gelen baytlar (kullanım dışı)|Bayt|Toplam|Ad alanı için Olay Hub 'ı gelen ileti işleme (kullanım dışı)|Hiçbiri|
|EHOUTMB|Giden baytlar (eski) (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı giden ileti işleme. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine giden bayt ölçümünü kullanın (kullanım dışı)|Hiçbiri|
|EHOUTBYTES|Giden baytlar (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı giden ileti işleme (kullanım dışı)|Hiçbiri|
|EHABL|Biriktirme listesi iletilerini Arşivle (kullanım dışı)|Sayı|Toplam|Ad alanı için biriktirme listesindeki Olay Hub 'ı arşiv iletileri (kullanım dışı)|Hiçbiri|
|EHAMSGS|İletileri Arşivle (kullanım dışı)|Sayı|Toplam|Bir ad alanında olay hub 'ı arşivlenmiş iletiler (kullanım dışı)|Hiçbiri|
|EHAMB|Arşiv iletisi verimlilik (kullanım dışı)|Bayt|Toplam|Bir ad alanında olay hub 'ı arşivlenmiş ileti işleme (kullanım dışı)|Hiçbiri|

## <a name="microsofteventhubclusters"></a>Microsoft. EventHub/kümeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı Istekler|Sayı|Toplam|Microsoft. EventHub için başarılı Istekler.|OperationResult|
|ServerErrors|Sunucu hataları.|Sayı|Toplam|Microsoft. EventHub için sunucu hataları.|OperationResult|
|UserErrors|Kullanıcı hataları.|Sayı|Toplam|Microsoft. EventHub için Kullanıcı hataları.|OperationResult|
|QuotaExceededErrors|Kota, hataları aştı.|Sayı|Toplam|Microsoft. EventHub için Kota aşıldı.|OperationResult|
|OperationName|Kısıtlanmış Istekler.|Sayı|Toplam|Microsoft. EventHub için kısıtlanan Istekler.|OperationResult|
|Incomingrequests|Gelen Istekler|Sayı|Toplam|Microsoft. EventHub için gelen Istekler.|Hiçbiri|
|Incomingmessages|Gelen Iletiler|Sayı|Toplam|Microsoft. EventHub için gelen Iletiler.|Hiçbiri|
|OutgoingMessages|Giden Iletiler|Sayı|Toplam|Microsoft. EventHub için giden Iletiler.|Hiçbiri|
|Incomingbytes|Gelen bayt sayısı.|Bayt|Toplam|Microsoft. EventHub için gelen baytlar.|Hiçbiri|
|OutgoingBytes|Giden bayt sayısı.|Bayt|Toplam|Microsoft. EventHub için giden baytlar.|Hiçbiri|
|ActiveConnection sayısı|ActiveConnection sayısı|Sayı|Ortalama|Microsoft. EventHub için toplam etkin bağlantı sayısı.|Hiçbiri|
|ConnectionsOpened|Bağlantılar açıldı.|Sayı|Ortalama|Microsoft. EventHub için açılan bağlantılar.|Hiçbiri|
|ConnectionsClosed|Bağlantı kapatıldı.|Sayı|Ortalama|Microsoft. EventHub için kapatılan bağlantılar.|Hiçbiri|
|CaptureBacklog|Biriktirme listesini yakala.|Sayı|Toplam|Microsoft. EventHub için yakalama biriktirme listesi.|Hiçbiri|
|CapturedMessages|Yakalanan Iletiler.|Sayı|Toplam|Microsoft. EventHub için yakalanan Iletiler.|Hiçbiri|
|CapturedBytes|Yakalanan baytlar.|Bayt|Toplam|Microsoft. EventHub için yakalanan baytlar.|Hiçbiri|
|CPU|CPU|Yüzde|Maksimum|Bir yüzde olarak olay hub 'ı kümesine yönelik CPU kullanımı|Rol|
|Kullanılabilirlik Blememory|Kullanılabilir Bellek|Yüzde|Maksimum|Toplam belleğin yüzdesi olarak olay hub 'ı kümesi için kullanılabilir bellek.|Rol|


## <a name="microsofthdinsightclusters"></a>Microsoft. HDInsight/kümeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|GatewayRequests|Ağ Geçidi Istekleri|Sayı|Toplam|Ağ Geçidi isteklerinin sayısı|Http durumu|
|Kategorizedgatewayrequests|Kategorilere ayrılmış ağ geçidi Istekleri|Sayı|Toplam|Kategorilere göre ağ geçidi isteklerinin sayısı (1xx/2xx/3xx/4xx/5xx)|Http durumu|
|Numactiveçalışanları|Etkin çalışan sayısı|Sayı|Maksimum|Etkin çalışan sayısı|MetricName|


## <a name="microsoftinsightsautoscalesettings"></a>Microsoft. Insights/oto Scalesettings

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ObservedMetricValue|Gözlemlenen ölçüm değeri|Sayı|Ortalama|Yürütüldüğünde otomatik ölçeklendirme tarafından hesaplanan değer|MetricTriggerSource|
|MetricThreshold|Ölçüm eşiği|Sayı|Ortalama|Otomatik ölçeklendirme çalıştırıldığında yapılandırılan otomatik ölçeklendirme eşiği.|MetricTriggerRule|
|ObservedCapacity|Gözlenen kapasite|Sayı|Ortalama|Yürütüldüğünde otomatik ölçeklendirme olarak bildirilen kapasite.|Hiçbiri|
|Scaleactionsınıated|Ölçek eylemleri başlatıldı|Sayı|Toplam|Ölçek işleminin yönü.|ScaleDirection|




## <a name="microsoftinsightscomponents"></a>Microsoft. Insights/bileşenler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanılabilirlik sonuçları/kullanılabilirliği yüzdesi|Erişilebilirlik|Yüzde|Ortalama|Başarıyla tamamlanan kullanılabilirlik testlerinin yüzdesi|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu|
|Kullanılabilirlik sonuçları/sayısı|Kullanılabilirlik testleri|Sayı|Sayı|Kullanılabilirlik testlerinin sayısı|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu, kullanılabilirlik sonucu/başarısı|
|Kullanılabilirlik sonuçları/süresi|Kullanılabilirlik testi süresi|Mayacak|Ortalama|Kullanılabilirlik testi süresi|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu, kullanılabilirlik sonucu/başarısı|
|Browserzamanlamalar/networkDuration|Sayfa yükleme ağ bağlantı süresi|Mayacak|Ortalama|Kullanıcı isteği ile ağ bağlantısı arasındaki süre. DNS arama ve aktarım bağlantısı içerir.|Hiçbiri|
|Browserzamanlamalar/processingDuration|İstemci işlem süresi|Mayacak|Ortalama|DOM yükleninceye kadar belgenin son baytını alma arasındaki süre. Zaman uyumsuz istekler hala işlenmeye devam edebilir.|Hiçbiri|
|Browserzamanlamalar/receiveDuration|Yanıt süresini alma|Mayacak|Ortalama|İlk ve son bayt veya bağlantının kesilmesi arasındaki süre.|Hiçbiri|
|Browserzamanlamalar/sendDuration|İstek gönderme süresi|Mayacak|Ortalama|Ağ bağlantısı ve ilk baytın alınması arasındaki süre.|Hiçbiri|
|Browserzamanlamalar/totalDuration|Tarayıcı sayfa yükleme süresi|Mayacak|Ortalama|DOM, stil sayfaları, betikler ve görüntüler yükleninceye kadar Kullanıcı isteğinden geçen süre.|Hiçbiri|
|Bağımlılıklar/sayı|Bağımlılık çağrıları|Sayı|Sayı|Uygulama tarafından dış kaynaklara yapılan çağrıların sayısı.|bağımlılık/tür, bağımlılık/performanceBucket, bağımlılık/başarı, bağımlılık/hedef, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|Bağımlılıklar/süre|Bağımlılık süresi|Mayacak|Ortalama|Uygulama tarafından dış kaynaklara yapılan çağrıların süresi.|bağımlılık/tür, bağımlılık/performanceBucket, bağımlılık/başarı, bağımlılık/hedef, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|Bağımlılıklar/başarısız|Bağımlılık çağrısı sorunları|Sayı|Sayı|Uygulama tarafından dış kaynaklara yapılan başarısız bağımlılık çağrılarının sayısı.|bağımlılık/tür, bağımlılık/performanceBucket, bağımlılık/başarı, bağımlılık/hedef, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|pageViews/Count|Sayfa görünümleri|Sayı|Sayı|Sayfa görüntüleme sayısı.|işlem/yapay, bulut/roleName|
|pageViews/Duration|Sayfa görünümü yükleme süresi|Mayacak|Ortalama|Sayfa görünümü yükleme süresi|işlem/yapay, bulut/roleName|
|performanceCounters/requestExecutionTime|HTTP isteği yürütme süresi|Mayacak|Ortalama|En son isteğin yürütme süresi.|Cloud/Roleınstance|
|performanceCounters/Requestsınqueue|Uygulama kuyruğundaki HTTP istekleri|Sayı|Ortalama|Uygulama isteği sırasının uzunluğu.|Cloud/Roleınstance|
|performanceCounters/requestsPerSecond|HTTP istek hızı|Sayaçpersaniye|Ortalama|ASP.NET ' den saniyede uygulamaya yapılan tüm isteklerin oranı.|Cloud/Roleınstance|
|performanceCounters/exceptionsPerSecond|Özel durum oranı|Sayaçpersaniye|Ortalama|.NET özel durumları ve .NET özel durumlarına dönüştürülmüş yönetilmeyen özel durumlar da dahil olmak üzere, Windows 'a bildirilen işlenmiş ve işlenmemiş özel durumların sayısı.|Cloud/Roleınstance|
|performanceCounters/Processıobitespersecond|İşlem GÇ oranı|BytesPerSecond|Ortalama|Dakikada, ağda ve cihazlarda okunan ve yazılan toplam bayt/saniye.|Cloud/Roleınstance|
|performanceCounters/processCpuPercentage|İşlem CPU 'SU|Yüzde|Ortalama|Tüm işlem iş parçacıklarının yönergeleri yürütmek için işlemciyi kullandığı geçen sürenin yüzdesi. Bu, 0 ila 100 arasında farklılık gösterebilir. Bu ölçüm, W3wp sürecinin performansını tek başına gösterir.|Cloud/Roleınstance|
|performanceCounters/Processorcpuyüzdesi|İşlemci zamanı|Yüzde|Ortalama|İşlemcinin boş olmayan iş parçacıklarında harcadığı sürenin yüzdesi.|Cloud/Roleınstance|
|performanceCounters/memoryAvailableBytes|Available memory|Bayt|Ortalama|Bir işleme veya sistem kullanımına ayırmak için hemen kullanılabilir fiziksel bellek.|Cloud/Roleınstance|
|performanceCounters/processPrivateBytes|İşlem özel baytları|Bayt|Ortalama|İzlenen uygulamanın işlemlerine özel olarak atanan bellek.|Cloud/Roleınstance|
|istek/süre|Sunucu yanıt süresi|Mayacak|Ortalama|Bir HTTP isteği alma ve yanıtın gönderilmesini tamamlama arasındaki süre.|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|istek/sayı|Sunucu istekleri|Sayı|Sayı|Tamamlanan HTTP isteği sayısı.|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|istek/başarısız|Başarısız istekler|Sayı|Sayı|Başarısız olarak işaretlenen HTTP isteklerinin sayısı. Çoğu durumda bunlar > = 400 yanıt koduna sahip isteklerdir ve 401 ' e eşit değildir.|istek/performanceBucket, istek/resultCode, istek/başarı, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|istek/hız|Sunucu isteği hızı|Sayaçpersaniye|Ortalama|Saniye başına sunucu isteği oranı|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|özel durumlar/say|Özel durumlar|Sayı|Sayı|Tüm yakalanamayan özel durumların Birleşik sayısı.|bulut/rol adı, bulut/Roleınstance, istemci/tür|
|özel durumlar/tarayıcı|Tarayıcı özel durumları|Sayı|Sayı|Tarayıcıda oluşturulan yakalanamayan özel durumların sayısı.|istemci/isServer, bulut/roleName|
|özel durumlar/sunucu|Sunucu özel durumları|Sayı|Sayı|Sunucu uygulamasında oluşturulan yakalanamayan özel durum sayısı.|istemci/isServer, bulut/roleName, Cloud/Roleınstance|
|izlemeler/say|Lerin|Sayı|Sayı|İzleme belgesi sayısı|izleme/severityLevel, işlem/yapay, Cloud/roleName, Cloud/Roleınstance|



## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServiceApiHit|Toplam hizmet API 'Si Isabet sayısı|Sayı|Sayı|Toplam hizmet API 'si isabet sayısı|ActivityType, ActivityName|
|Hizmet Apilatesi|Genel hizmet API 'Si gecikmesi|Milisaniye|Ortalama|Hizmet API 'si isteklerinin genel gecikmesi|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|ServiceApiResult|Toplam hizmet API 'Si sonuçları|Sayı|Sayı|Toplam hizmet API 'si sonuç sayısı|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|Cumartesi Ationshoe kutusu|Genel kasa doygunluğu|Yüzde|Ortalama|Kullanılan kasa kapasitesi|ActivityType, ActivityName, TransactionType|
|Erişilebilirlik|Genel kasa kullanılabilirliği|Yüzde|Ortalama|Kasa istekleri kullanılabilirliği|ActivityType, ActivityName, StatusCode, StatusCodeClass|

## <a name="microsoftkustoclusters"></a>Microsoft. kusto/kümeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Cachekullanım|Önbellek kullanımı|Yüzde|Ortalama|Küme kapsamındaki kullanım düzeyi|Hiçbiri|
|QueryDuration|Sorgu süresi|Milisaniye|Ortalama|Sorgu süresi (saniye)|QueryStatus|
|Alım kullanımı|Alım kullanımı|Yüzde|Ortalama|Kümedeki kullanılan alma yuvaları oranı|Hiçbiri|
|Alı|Canlı tut|Sayı|Ortalama|Sanity denetimi, kümenin sorgulara yanıt verdiğini belirtir|Hiçbiri|
|Inestionvolumeınmb|Alım birimi (MB)|Sayı|Toplam|Kümeye alınan verilerin toplam hacmi (MB)|Veritabanı|
|Inestionlatencınseconds|Alma gecikmesi (saniye)|Saniye|Ortalama|Kaynaktan (örn. ileti EventHub ' de bulunur) kümeye saniye cinsinden alma süresi|Hiçbiri|
|Processedforeventhubs olayları|İşlenen Olaylar (olay/IoT Hub 'Ları için)|Sayı|Toplam|Olay/IoT Hub geri ödeme yaparken küme tarafından işlenen olay sayısı|EventStatus|
|Inestionresult|Alım sonucu|Sayı|Sayı|Alma işlemlerinin sayısı|Inestionresultdetails|
|CPU|CPU|Yüzde|Ortalama|CPU kullanım düzeyi|Hiçbiri|
|Continuousexportnumofrecordsexports|Sürekli dışa aktarma – dışarı aktarılmış kayıtların sayısı|Sayı|Toplam|Dışarı aktarma işlemi sırasında yazılan her depolama yapıtı için tetiklenen, dışarı aktarılmış kayıt sayısı|Hiçbiri|
|Dışarı Aktaramstilileştirme|Kullanım verme|Yüzde|Maksimum|Kullanım verme|Hiçbiri|
|ContinuousExportPendingCount|Sürekli dışarı aktarma bekleyen sayısı|Sayı|Maksimum|Yürütmeye hazırlamış bekleyen sürekli dışarı aktarma işlerinin sayısı|Hiçbiri|
|ContinuousExportMaxLatenessMinutes|Sürekli dışarı aktarma en fazla dakika|Sayı|Maksimum|Bekleyen ve yürütme için hazırlanma işlemleri için dakika cinsinden maksimum değer sayısı|Hiçbiri|
|ContinuousExportResult|Sürekli dışarı aktarma sonucu|Sayı|Sayı|Sürekli dışarı aktarmanın başarılı veya başarısız olup olmadığını belirtir|Devamousexportname, sonuç, veritabanı|
|StreamingIngestDuration|Akış alma süresi|Milisaniye|Ortalama|Milisaniye cinsinden akış alma süresi|Hiçbiri|
|StreamingIngestDataRate|Akış alma verileri oranı|Sayı|Ortalama|Akış alma verileri oranı (MB/saniye)|Hiçbiri|
|SteamingIngestRequestRate|Akış alma Isteği oranı|Sayı|Kterequestspersecond|Akış alma isteği hızı (saniye başına istek)|Hiçbiri|
|StreamingIngestResults|Akış alma sonucu|Sayı|Ortalama|Akış alma sonucu|Sonuç|
|TotalNumberOfConcurrentQueries|Toplam eşzamanlı sorgu sayısı|Sayı|Toplam|Toplam eşzamanlı sorgu sayısı|Hiçbiri|
|Toplam sorgu sayısı|Toplam kısıtlanmış sorgu sayısı|Sayı|Toplam|Toplam kısıtlanmış sorgu sayısı|Hiçbiri|
|Totalnumberofkısıtlar Ledcommands|Kısıtlanmış komutların toplam sayısı|Sayı|Toplam|Kısıtlanmış komutların toplam sayısı|CommandType|
|Totalnumberofkapsamları|Toplam kapsam sayısı|Sayı|Toplam|Toplam veri uzantısı sayısı|Hiçbiri|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Çalışma başlatıldı|Sayı|Toplam|Başlatılan iş akışı çalıştırma sayısı.|Hiçbiri|
|RunsCompleted|Tamamlanan çalıştırmalar|Sayı|Toplam|Tamamlanan iş akışı çalıştırma sayısı.|Hiçbiri|
|RunsSucceeded|Çalışma başarılı|Sayı|Toplam|İş akışı çalıştırma sayısı başarılı oldu.|Hiçbiri|
|RunsFailed|Çalıştırma başarısız|Sayı|Toplam|Başarısız iş akışı çalıştırma sayısı.|Hiçbiri|
|RunsCancelled|Çalıştırma Iptal edildi|Sayı|Toplam|İptal edilen iş akışı çalıştırma sayısı.|Hiçbiri|
|RunLatency|Çalıştırma gecikmesi|Saniye|Ortalama|Tamamlanan iş akışı çalıştırmalarının gecikmesi.|Hiçbiri|
|Runbaşarılı gecikme süresi|Çalıştırma başarılı gecikme süresi|Saniye|Ortalama|Başarılı iş akışı çalıştırmalarının gecikmesi.|Hiçbiri|
|RunThrottledEvents|Kısıtlanmış olayları Çalıştır|Sayı|Toplam|İş akışı eylemi veya tetikleyici kısıtlanan olay sayısı.|Hiçbiri|
|RunStartThrottledEvents|Kısıtlanmış başlangıç olaylarını Çalıştır|Sayı|Toplam|İş akışı çalıştırma, kısıtlanmış olay sayısı.|Hiçbiri|
|RunFailurePercentage yüzdesi|Çalıştırma hatası yüzdesi|Yüzde|Toplam|İş akışı çalıştırmalarının yüzdesi başarısız oldu.|Hiçbiri|
|ActionsStarted|Başlatılan eylemler |Sayı|Toplam|Başlatılan iş akışı eylemi sayısı.|Hiçbiri|
|ActionsCompleted|Tamamlanan eylemler |Sayı|Toplam|Tamamlanan iş akışı eylemlerinin sayısı.|Hiçbiri|
|ActionsSucceeded|İşlemler başarılı oldu |Sayı|Toplam|Başarılı iş akışı eylemi sayısı.|Hiçbiri|
|ActionsFailed|Başarısız eylemler |Sayı|Toplam|Başarısız iş akışı eylemi sayısı.|Hiçbiri|
|ActionsSkipped|Atlanan eylemler |Sayı|Toplam|Atlanan iş akışı eylemi sayısı.|Hiçbiri|
|ActionLatency|Eylem gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Hiçbiri|
|Actionbaşarılı gecikme süresi|Eylem başarı gecikmesi |Saniye|Ortalama|Başarılı iş akışı eylemlerinin gecikme süresi.|Hiçbiri|
|ActionThrottledEvents|Eylem kısıtlanmış olaylar|Sayı|Toplam|İş akışı eylemi kısıtlanmış olay sayısı..|Hiçbiri|
|TriggersStarted|Başlatılan Tetikleyiciler |Sayı|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggersCompleted|Tamamlanan Tetikleyiciler |Sayı|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggersSucceeded|Başarılı Tetikleyiciler |Sayı|Toplam|Başarılı iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggersFailed|Başarısız Tetikleyiciler |Sayı|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggersSkipped|Atlanan Tetikleyiciler|Sayı|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Hiçbiri|
|Triggersharekete geçirildi|Tetiklenen Tetikleyiciler |Sayı|Toplam|Tetiklenen iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggerLatency|Tetikleme gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Hiçbiri|
|Triggerfirelatlik|Tetikleyici Tetikleme gecikmesi |Saniye|Ortalama|Tetiklenen iş akışı Tetikleyicileri gecikmesi.|Hiçbiri|
|Triggerbaşarılı gecikme|Tetikleyici başarı gecikme süresi |Saniye|Ortalama|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Hiçbiri|
|TriggerThrottledEvents|Kısıtlanmış olayları tetikleyin|Sayı|Toplam|İş akışı tetikleyicisi kısıtlanmış olay sayısı.|Hiçbiri|
|Billableactionyürütmeler|Faturalanabilir eylem yürütmeleri|Sayı|Toplam|Faturalandırılan iş akışı eylemi yürütmelerinin sayısı.|Hiçbiri|
|Billabletriggeryürütmeler|Faturalanabilir tetikleyici yürütmeleri|Sayı|Toplam|Faturalandırılan iş akışı tetikleyici yürütmelerinin sayısı.|Hiçbiri|
|Totalbillableyürütmeleri|Toplam faturalandırılabilir yürütmeler|Sayı|Toplam|Faturalandırılan iş akışı yürütmelerinin sayısı.|Hiçbiri|
|BillingUsageNativeOperation|Yerel Işlem yürütmeleri için faturalandırma kullanımı|Sayı|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Hiçbiri|
|BillingUsageStandardConnector|Standart bağlayıcı yürütmeleri için faturalandırma kullanımı|Sayı|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Hiçbiri|
|Billingusagestorampatüketim|Depolama tüketimi yürütmeleri için faturalama kullanımı|Sayı|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Hiçbiri|
|BillingUsageNativeOperation|Yerel Işlem yürütmeleri için faturalandırma kullanımı|Sayı|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Hiçbiri|
|BillingUsageStandardConnector|Standart bağlayıcı yürütmeleri için faturalandırma kullanımı|Sayı|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Hiçbiri|
|Billingusagestorampatüketim|Depolama tüketimi yürütmeleri için faturalama kullanımı|Sayı|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Hiçbiri|



## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft. Logic/ıntegrationserviceortamortamları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RunsStarted|Çalışma başlatıldı|Sayı|Toplam|Başlatılan iş akışı çalıştırma sayısı.|Hiçbiri|
|RunsCompleted|Tamamlanan çalıştırmalar|Sayı|Toplam|Tamamlanan iş akışı çalıştırma sayısı.|Hiçbiri|
|RunsSucceeded|Çalışma başarılı|Sayı|Toplam|İş akışı çalıştırma sayısı başarılı oldu.|Hiçbiri|
|RunsFailed|Çalıştırma başarısız|Sayı|Toplam|Başarısız iş akışı çalıştırma sayısı.|Hiçbiri|
|RunsCancelled|Çalıştırma Iptal edildi|Sayı|Toplam|İptal edilen iş akışı çalıştırma sayısı.|Hiçbiri|
|RunLatency|Çalıştırma gecikmesi|Saniye|Ortalama|Tamamlanan iş akışı çalıştırmalarının gecikmesi.|Hiçbiri|
|Runbaşarılı gecikme süresi|Çalıştırma başarılı gecikme süresi|Saniye|Ortalama|Başarılı iş akışı çalıştırmalarının gecikmesi.|Hiçbiri|
|RunThrottledEvents|Kısıtlanmış olayları Çalıştır|Sayı|Toplam|İş akışı eylemi veya tetikleyici kısıtlanan olay sayısı.|Hiçbiri|
|RunStartThrottledEvents|Kısıtlanmış başlangıç olaylarını Çalıştır|Sayı|Toplam|İş akışı çalıştırma, kısıtlanmış olay sayısı.|Hiçbiri|
|RunFailurePercentage yüzdesi|Çalıştırma hatası yüzdesi|Yüzde|Toplam|İş akışı çalıştırmalarının yüzdesi başarısız oldu.|Hiçbiri|
|ActionsStarted|Başlatılan eylemler |Sayı|Toplam|Başlatılan iş akışı eylemi sayısı.|Hiçbiri|
|ActionsCompleted|Tamamlanan eylemler |Sayı|Toplam|Tamamlanan iş akışı eylemlerinin sayısı.|Hiçbiri|
|ActionsSucceeded|İşlemler başarılı oldu |Sayı|Toplam|Başarılı iş akışı eylemi sayısı.|Hiçbiri|
|ActionsFailed|Başarısız eylemler |Sayı|Toplam|Başarısız iş akışı eylemi sayısı.|Hiçbiri|
|ActionsSkipped|Atlanan eylemler |Sayı|Toplam|Atlanan iş akışı eylemi sayısı.|Hiçbiri|
|ActionLatency|Eylem gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Hiçbiri|
|Actionbaşarılı gecikme süresi|Eylem başarı gecikmesi |Saniye|Ortalama|Başarılı iş akışı eylemlerinin gecikme süresi.|Hiçbiri|
|ActionThrottledEvents|Eylem kısıtlanmış olaylar|Sayı|Toplam|İş akışı eylemi kısıtlanmış olay sayısı..|Hiçbiri|
|TriggersStarted|Başlatılan Tetikleyiciler |Sayı|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggersCompleted|Tamamlanan Tetikleyiciler |Sayı|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggersSucceeded|Başarılı Tetikleyiciler |Sayı|Toplam|Başarılı iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggersFailed|Başarısız Tetikleyiciler |Sayı|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggersSkipped|Atlanan Tetikleyiciler|Sayı|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Hiçbiri|
|Triggersharekete geçirildi|Tetiklenen Tetikleyiciler |Sayı|Toplam|Tetiklenen iş akışı tetikleyicisi sayısı.|Hiçbiri|
|TriggerLatency|Tetikleme gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Hiçbiri|
|Triggerfirelatlik|Tetikleyici Tetikleme gecikmesi |Saniye|Ortalama|Tetiklenen iş akışı Tetikleyicileri gecikmesi.|Hiçbiri|
|Triggerbaşarılı gecikme|Tetikleyici başarı gecikme süresi |Saniye|Ortalama|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Hiçbiri|
|TriggerThrottledEvents|Kısıtlanmış olayları tetikleyin|Sayı|Toplam|İş akışı tetikleyicisi kısıtlanmış olay sayısı.|Hiçbiri|
|Integrationserviceenvironmentworkflowprocessorusage|Tümleştirme Hizmeti Ortamı için iş akışı Işlemcisi kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için iş akışı işlemcisi kullanımı.|Hiçbiri|
|Integrationserviceenvironmentworkflowmemoryusage|Tümleştirme Hizmeti Ortamı için iş akışı bellek kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için iş akışı bellek kullanımı.|Hiçbiri|
|Integrationserviceenvironmentconnectorprocessorusage|Tümleştirme Hizmeti Ortamı için bağlayıcı Işlemci kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için bağlayıcı işlemci kullanımı.|Hiçbiri|
|Integrationserviceenvironmentconnectormemoryusage|Tümleştirme Hizmeti Ortamı için bağlayıcı bellek kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için bağlayıcı bellek kullanımı.|Hiçbiri|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft. MachineLearningServices/çalışma alanları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Tamamlanan çalıştırmalar|Tamamlanan çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için başarıyla tamamlanan çalıştırma sayısı|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Başlatılan çalıştırmalar|Başlatılan çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için başlatılan çalıştırma sayısı|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Başarısız çalıştırmalar|Başarısız çalıştırmalar|Sayı|Toplam|Bu çalışma alanı için başarısız olan çalıştırma sayısı|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Model kaydı başarılı oldu|Model kaydı başarılı oldu|Sayı|Toplam|Bu çalışma alanında başarılı olan model kaydı sayısı|Senaryo|
|Model kaydı başarısız oldu|Model kaydı başarısız oldu|Sayı|Toplam|Bu çalışma alanında başarısız olan model kayıtlarının sayısı|Senaryo, StatusCode|
|Model Dağıtımı başladı|Model Dağıtımı başladı|Sayı|Toplam|Bu çalışma alanında başlatılan model dağıtımı sayısı|Senaryo|
|Model Dağıtımı başarılı oldu|Model Dağıtımı başarılı oldu|Sayı|Toplam|Bu çalışma alanındaki başarılı olan model dağıtımı sayısı|Senaryo|
|Model Dağıtımı başarısız oldu|Model Dağıtımı başarısız oldu|Sayı|Toplam|Bu çalışma alanında başarısız olan model dağıtımı sayısı|Senaryo, StatusCode|
|Toplam düğüm sayısı|Toplam düğüm sayısı|Sayı|Ortalama|Toplam düğüm sayısı. Bu toplam etkin düğümlerin, boşta düğümlerin, kullanılamayan düğümlerin, Önkullanılmayan düğümlerin, düğüm dışı düğümlerin bazılarını içerir|Senaryo, ClusterName|
|Etkin düğümler|Etkin düğümler|Sayı|Ortalama|Acitve düğümlerin sayısı. Bunlar, etkin olarak bir işi çalıştıran düğümlerdir.|Senaryo, ClusterName|
|Boştaki düğümler|Boştaki düğümler|Sayı|Ortalama|Boştaki düğüm sayısı. Boştaki düğümler, herhangi bir işi çalıştırmayan ancak varsa yeni işi kabul edebilecek düğümlerdir.|Senaryo, ClusterName|
|Kullanılamayan düğümler|Kullanılamayan düğümler|Sayı|Ortalama|Kullanılamayan düğüm sayısı. Çözümlenebilen bazı sorunlar nedeniyle kullanılamaz düğümler çalışmıyor. Azure bu düğümleri geri dönüştürecek.|Senaryo, ClusterName|
|Önden düğümler|Önden düğümler|Sayı|Ortalama|Yok edilecek düğüm sayısı. Bu düğümler, kullanılabilir düğüm havuzundan alınan düşük öncelikli düğümlerdir.|Senaryo, ClusterName|
|Düğümlerden çıkılıyor|Düğümlerden çıkılıyor|Sayı|Ortalama|Düğüm sayısı. Düğüm ayrıldığınızda, bir işi işlemeyi tamamlamış olan düğümler ve boşta durumuna gidecektir.|Senaryo, ClusterName|
|Toplam çekirdek sayısı|Toplam çekirdek sayısı|Sayı|Ortalama|Toplam çekirdek sayısı|Senaryo, ClusterName|
|Etkin çekirdekler|Etkin çekirdekler|Sayı|Ortalama|Etkin çekirdek sayısı|Senaryo, ClusterName|
|Boştaki çekirdekler|Boştaki çekirdekler|Sayı|Ortalama|Boşta çekirdek sayısı|Senaryo, ClusterName|
|Kullanılamayan çekirdekler|Kullanılamayan çekirdekler|Sayı|Ortalama|Kullanılamayan çekirdek sayısı|Senaryo, ClusterName|
|Önden çekirdekler|Önden çekirdekler|Sayı|Ortalama|Yok edilecek çekirdek sayısı|Senaryo, ClusterName|
|Çekirdekleri bırakma|Çekirdekleri bırakma|Sayı|Ortalama|Bırakma çekirdekleri sayısı|Senaryo, ClusterName|
|Kota kullanım yüzdesi|Kota kullanım yüzdesi|Sayı|Ortalama|Kullanılan Kota yüzdesi|Senaryo, ClusterName, VmFamilyName, VmPriority|
|Cpukullanımı|Cpukullanımı|Sayı|Ortalama|CPU (Önizleme)|Senaryo, RunId, NodeId, CreatedTime|
|Gpukullanım|Gpukullanım|Sayı|Ortalama|GPU (Önizleme)|Senaryo, RunId, NodeId, CreatedTime, DeviceID|


## <a name="microsoftmapsaccounts"></a>Microsoft. Maps/hesaplar

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Kullanım|Kullanım|Sayı|Sayı|API çağrısı sayısı|ApiCategory, ApiName, ResultType, ResponseCode|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|API 'lerin kullanılabilirliği|ApiCategory, ApiName|

## <a name="microsoftmediamediaservicesstreamingendpoints"></a>Microsoft. Media/mediaservices/streamingEndpoints

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verisi miktarı.|OutputFormat|
|Başarı E2e|Başarılı uçtan uca gecikme süresi|Milisaniye|Ortalama|Başarılı istekler için milisaniye cinsinden ortalama gecikme süresi.|OutputFormat|
|İstekler|İstekler|Sayı|Toplam|Akış uç noktası istekleri.|OutputFormat, HttpStatusCode, ErrorCode|


## <a name="microsoftmediamediaservices"></a>Microsoft. Media/mediaservices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AssetQuota|Varlık kotası|Sayı|Ortalama|Geçerli medya hizmeti hesabı için kaç kıymete izin veriliyor?|Hiçbiri|
|AssetCount|Varlık sayısı|Sayı|Ortalama|Geçerli medya hizmeti hesabında kaç varlık zaten oluşturuldu|Hiçbiri|
|AssetQuotaUsedPercentage|Kullanılan varlık kotası yüzdesi|Yüzde|Ortalama|Geçerli medya hizmeti hesabındaki varlık kullanım yüzdesi|Hiçbiri|
|ContentKeyPolicyQuota|İçerik anahtarı Ilke kotası|Sayı|Ortalama|Geçerli medya hizmeti hesabı için kaç tane içerik anahtar ilkelerine izin verilir?|Hiçbiri|
|ContentKeyPolicyCount|İçerik anahtarı Ilke sayısı|Sayı|Ortalama|Geçerli medya hizmeti hesabında kaç tane içerik anahtar ilkesi oluşturuldu|Hiçbiri|
|ContentKeyPolicyQuotaUsedPercentage|İçerik anahtarı Ilke kotası kullanılan yüzde|Yüzde|Ortalama|Geçerli medya hizmeti hesabında kullanılan içerik anahtarı Ilkesi yüzdesi|Hiçbiri|
|StreamingPolicyQuota|Akış Ilkesi kotası|Sayı|Ortalama|Geçerli medya hizmeti hesabı için kaç akış ilkesine izin verilir?|Hiçbiri|
|Streammingpolicycount|Akış Ilkesi sayısı|Sayı|Ortalama|Geçerli medya hizmeti hesabında kaç tane akış ilkesi zaten oluşturuldu|Hiçbiri|
|StreamingPolicyQuotaUsedPercentage|Akış Ilkesi kotası kullanılan yüzde|Yüzde|Ortalama|Geçerli medya hizmeti hesabındaki akış Ilkesi kullanım yüzdesi|Hiçbiri|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageReadLatency|Ortalama okuma gecikmesi|Mayacak|Ortalama|İşlem başına milisaniye cinsinden ortalama okuma gecikmesi|Hiçbiri|
|AverageWriteLatency|Ortalama yazma gecikmesi|Mayacak|Ortalama|İşlem başına milisaniye cinsinden ortalama yazma gecikmesi|Hiçbiri|
|Birimlogicalsize|Birim mantıksal boyutu|Bayt|Ortalama|Birimin mantıksal boyutu (kullanılan baytlar)|Hiçbiri|
|VolumeSnapshotSize|Birim anlık görüntü boyutu|Bayt|Ortalama|Birimdeki tüm anlık görüntülerin boyutu|Hiçbiri|
|ReadIops|IOPS 'yi oku|Sayaçpersaniye|Ortalama|Saniye başına okuma/kapatma işlemi|Hiçbiri|
|Writeıops|IOPS yaz|Sayaçpersaniye|Ortalama|Saniyede yazma/çıkış işlemi sayısı|Hiçbiri|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft. NetApp/netAppAccounts/Capacityhavuzları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VolumePoolAllocatedUsed|Ayrılan birim havuzu|Bayt|Ortalama|Havuzun ayrılan boyutu ayrılmış|Hiçbiri|
|VolumePoolTotalLogicalSize|Birim havuzu toplam mantıksal boyut|Bayt|Ortalama|Havuza ait tüm birimlerin mantıksal boyutunun toplamı|Hiçbiri|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft. Network/NetworkInterfaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesSentRate|Gönderilen bayt|Bayt|Toplam|Ağ arabiriminin gönderdiği bayt sayısı|Hiçbiri|
|BytesReceivedRate|Alınan bayt|Bayt|Toplam|Ağ arabiriminin aldığı bayt sayısı|Hiçbiri|
|PacketsSentRate|Gönderilen paketler|Sayı|Toplam|Ağ arabiriminin gönderdiği paket sayısı|Hiçbiri|
|PacketsReceivedRate|Alınan paketler|Sayı|Toplam|Ağ arabiriminin aldığı paket sayısı|Hiçbiri|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|VipAvailability|Veri yolu kullanılabilirliği|Sayı|Ortalama|Süre başına ortalama Load Balancer veri yolu kullanılabilirliği|Frontendıaddress, FrontendPort|
|DipAvailability|Durum araştırma durumu|Sayı|Ortalama|Süre başına ortalama Load Balancer sistem durumu araştırma durumu|ProtocolType, BackendPort, Frontendıbaddress, FrontendPort, Backendıdıaddress|
|ByteCount|Bayt sayısı|Sayı|Toplam|Zaman aralığı içinde aktarılan toplam bayt sayısı|Frontendıaaddress, FrontendPort, Yön|
|PacketCount|Paket sayısı|Sayı|Toplam|Zaman aralığı içinde aktarılan toplam paket sayısı|Frontendıaaddress, FrontendPort, Yön|
|SYNCount|SYN sayısı|Sayı|Toplam|Zaman aralığı içinde aktarılan toplam SYN paketi sayısı|Frontendıaaddress, FrontendPort, Yön|
|SnatConnectionCount|SNAT bağlantı sayısı|Sayı|Toplam|Zaman aralığı içinde oluşturulan toplam yeni SNAT bağlantısı sayısı|Frontendıbaddress, Backendıo adresi, ConnectionState|
|AllocatedSnatPorts|Ayrılan SNAT bağlantı noktaları (Önizleme)|Sayı|Toplam|Zaman aralığı içinde ayrılan toplam SNAT bağlantı noktası sayısı|Frontendıbaddress, Backendıaddress, ProtocolType, ısawaitingkaldırması|
|UsedSnatPorts|Kullanılan SNAT bağlantı noktaları (Önizleme)|Sayı|Toplam|Zaman aralığı içinde kullanılan SNAT bağlantı noktalarının toplam sayısı|Frontendıbaddress, Backendıaddress, ProtocolType, ısawaitingkaldırması|


## <a name="microsoftnetworkdnszones"></a>Microsoft. Network/dnszones

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryVolume|Sorgu hacmi|Sayı|Toplam|Bir DNS bölgesi için sunulan sorgu sayısı|Hiçbiri|
|Kayıt kümesi sayısı|Kayıt kümesi sayısı|Sayı|Maksimum|Bir DNS bölgesindeki kayıt kümesi sayısı|Hiçbiri|
|Recordsetkapaıyutilileştirme|Kayıt kümesi kapasite kullanımı|Yüzde|Maksimum|Bir DNS bölgesi tarafından kullanılan kayıt kümesi kapasitesinin yüzdesi|Hiçbiri|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PacketsInDDoS|Gelen paketler DDoS|Sayaçpersaniye|Maksimum|Gelen paketler DDoS|Hiçbiri|
|PacketsDroppedDDoS|DDoS bırakılan gelen paketler|Sayaçpersaniye|Maksimum|DDoS bırakılan gelen paketler|Hiçbiri|
|PacketsForwardedDDoS|DDoS iletilen gelen paketler|Sayaçpersaniye|Maksimum|DDoS iletilen gelen paketler|Hiçbiri|
|TCPPacketsInDDoS|Gelen TCP paketleri DDoS|Sayaçpersaniye|Maksimum|Gelen TCP paketleri DDoS|Hiçbiri|
|TCPPacketsDroppedDDoS|Gelen TCP paketleri bırakıldı DDoS|Sayaçpersaniye|Maksimum|Gelen TCP paketleri bırakıldı DDoS|Hiçbiri|
|TCPPacketsForwardedDDoS|DDoS iletilen gelen TCP paketleri|Sayaçpersaniye|Maksimum|DDoS iletilen gelen TCP paketleri|Hiçbiri|
|UDPPacketsInDDoS|Gelen UDP paketleri DDoS|Sayaçpersaniye|Maksimum|Gelen UDP paketleri DDoS|Hiçbiri|
|UDPPacketsDroppedDDoS|Gelen UDP paketleri bırakıldı|Sayaçpersaniye|Maksimum|Gelen UDP paketleri bırakıldı|Hiçbiri|
|UDPPacketsForwardedDDoS|DDoS iletilen gelen UDP paketleri|Sayaçpersaniye|Maksimum|DDoS iletilen gelen UDP paketleri|Hiçbiri|
|BytesInDDoS|Gelen bayt sayısı DDoS|BytesPerSecond|Maksimum|Gelen bayt sayısı DDoS|Hiçbiri|
|BytesDroppedDDoS|Gelen bayt bırakıldı DDoS|BytesPerSecond|Maksimum|Gelen bayt bırakıldı DDoS|Hiçbiri|
|BytesForwardedDDoS|İletilen gelen bayt sayısı|BytesPerSecond|Maksimum|İletilen gelen bayt sayısı|Hiçbiri|
|TCPBytesInDDoS|Gelen TCP bayt DDoS|BytesPerSecond|Maksimum|Gelen TCP bayt DDoS|Hiçbiri|
|TCPBytesDroppedDDoS|Gelen TCP bayt bırakılan DDoS|BytesPerSecond|Maksimum|Gelen TCP bayt bırakılan DDoS|Hiçbiri|
|TCPBytesForwardedDDoS|İletilen gelen TCP baytları|BytesPerSecond|Maksimum|İletilen gelen TCP baytları|Hiçbiri|
|UDPBytesInDDoS|Gelen UDP bayt sayısı DDoS|BytesPerSecond|Maksimum|Gelen UDP bayt sayısı DDoS|Hiçbiri|
|UDPBytesDroppedDDoS|Gelen UDP bayt bırakılan DDoS|BytesPerSecond|Maksimum|Gelen UDP bayt bırakılan DDoS|Hiçbiri|
|UDPBytesForwardedDDoS|İletilen gelen UDP baytları|BytesPerSecond|Maksimum|İletilen gelen UDP baytları|Hiçbiri|
|IfUnderDDoSAttack|DDoS saldırısı altında|Sayı|Maksimum|DDoS saldırısı altında|Hiçbiri|
|DDO, Ggertcppackets|DDoS azaltma tetiklemeye yönelik gelen TCP paketleri|Sayaçpersaniye|Maksimum|DDoS azaltma tetiklemeye yönelik gelen TCP paketleri|Hiçbiri|
|DDO, Ggerudppaketleri|DDoS risk azaltma tetiklenecek gelen UDP paketleri|Sayaçpersaniye|Maksimum|DDoS risk azaltma tetiklenecek gelen UDP paketleri|Hiçbiri|
|Ddobir Ggersynpackets|DDoS azaltma tetiklenmesi için gelen SYN paketleri|Sayaçpersaniye|Maksimum|DDoS azaltma tetiklenmesi için gelen SYN paketleri|Hiçbiri|
|VipAvailability|Veri yolu kullanılabilirliği|Sayı|Ortalama|Süre başına ortalama IP adresi kullanılabilirliği|Bağlantı noktası|
|ByteCount|Bayt sayısı|Sayı|Toplam|Zaman aralığı içinde aktarılan toplam bayt sayısı|Bağlantı noktası, Yön|
|PacketCount|Paket sayısı|Sayı|Toplam|Zaman aralığı içinde aktarılan toplam paket sayısı|Bağlantı noktası, Yön|
|SynCount|SYN sayısı|Sayı|Toplam|Zaman aralığı içinde aktarılan toplam SYN paketi sayısı|Bağlantı noktası, Yön|



## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PingMeshAverageRoundtripMs|Bir VM 'ye ping için gidiş dönüş süresi|Mayacak|Ortalama|Hedef VM 'ye gönderilen pingler için gidiş dönüş süresi|SourceCustomerAddress, DestinationCustomerAddress|
|PingMeshProbesFailedPercent|VM 'ye ping işlemi başarısız oldu|Yüzde|Ortalama|Hedef VM 'nin gönderilen toplam ping Pinglerine yönelik başarısız Ping sayısı yüzdesi|SourceCustomerAddress, DestinationCustomerAddress|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft. Network/azurefirewalls

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ApplicationRuleHit|Uygulama kuralları isabet sayısı|Sayı|Toplam|Uygulama kurallarının isabet sayısı|Durum, neden, protokol|
|NetworkRuleHit|Ağ kuralları isabet sayısı|Sayı|Toplam|Ağ kurallarının isabet sayısı|Durum, neden, protokol|
|FirewallHealth|Güvenlik duvarı sistem durumu|Yüzde|Ortalama|Güvenlik duvarı sistem durumu|Durum, neden|
|Veri Işlendi|İşlenen veriler|Bayt|Toplam|Güvenlik duvarı tarafından işlenen toplam veri miktarı|Hiçbiri|
|SNATPortUtilization|SNAT bağlantı noktası kullanımı|Yüzde|Ortalama|SNAT bağlantı noktası kullanımı|Hiçbiri|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İşleme|İşleme|BytesPerSecond|Ortalama|Application Gateway saniyede hizmet verilen bayt sayısı|Hiçbiri|
|Unhealthyıhostcount|Sağlıksız konak sayısı|Sayı|Ortalama|Sağlıklı olmayan arka uç Konakları sayısı|BackendSettingsPool|
|Healthyıhostcount|Sağlıklı konak sayısı|Sayı|Ortalama|Sağlıklı arka uç Konakları sayısı|BackendSettingsPool|
|Toplam Istek sayısı|Toplam İstek Sayısı|Sayı|Toplam|Application Gateway tarafından sunulan başarılı istek sayısı|BackendSettingsPool|
|Avgrequestcountperhealthyıhost|Sağlıklı ana bilgisayar başına dakika başına istek|Sayı|Ortalama|Bir havuzda sağlıklı arka uç Konağı başına dakika başına ortalama istek sayısı|BackendSettingsPool|
|FailedRequests|Başarısız İstekler|Sayı|Toplam|Application Gateway tarafından sunulan başarısız istek sayısı|BackendSettingsPool|
|ResponseStatus|Yanıt durumu|Sayı|Toplam|Application Gateway tarafından döndürülen http yanıt durumu|HttpStatusGroup|
|CurrentConnections|Geçerli bağlantılar|Sayı|Toplam|Application Gateway ile kurulan geçerli bağlantı sayısı|Hiçbiri|
|Cpukullanımı|CPU kullanımı|Yüzde|Ortalama|Application Gateway geçerli CPU kullanımı|Hiçbiri|
|CapacityUnits|Geçerli kapasite birimleri|Sayı|Ortalama|Tüketilen Kapasite birimleri|Hiçbiri|
|ComputeUnits|Geçerli Işlem birimleri|Sayı|Ortalama|Tüketilen işlem birimleri|Hiçbiri|
|BackendResponseStatus|Arka uç yanıt durumu|Sayı|Toplam|Arka uç üyeleri tarafından oluşturulan HTTP yanıt kodlarının sayısı. Bu, Application Gateway tarafından oluşturulan yanıt kodlarını içermez.|BackendServer, BackendPool, BackendHttpSetting, HttpStatusGroup|
|TlsProtocol|İstemci TLS protokolü|Sayı|Toplam|Application Gateway ile bağlantı kurulan istemci tarafından başlatılan TLS ve TLS olmayan isteklerin sayısı. TLS protokol dağıtımını görüntülemek için Dimension TLS protokolüne göre filtreleyin.|Dinleyici, TlsProtocol|
|BytesSent|Gönderilen bayt|Bayt|Toplam|Application Gateway tarafından istemcilere gönderilen toplam bayt sayısı|Oluşturulurken|
|BytesReceived|Alınan bayt|Bayt|Toplam|İstemcilerden Application Gateway tarafından alınan toplam bayt sayısı|Oluşturulurken|
|ClientRtt|İstemci RTT|Mayacak|Ortalama|İstemcilerle Application Gateway arasında ortalama gidiş dönüş süresi. Bu ölçüm, bağlantı kurmak ve bildirimlerin döndürülmesi için ne kadar sürdüğünü gösterir|Oluşturulurken|
|ApplicationGatewayTotalTime|Application Gateway toplam süre|Mayacak|Ortalama|Bir isteğin işlenmesi için geçen ortalama süre ve gönderilecek yanıt. Bu, yanıt gönderme işlemi tamamlandığında Application Gateway bir HTTP isteğinin ilk baytını aldığında zaman aralığının ortalaması olarak hesaplanır. Bu, genellikle Application Gateway işleme süresini, istek ve Yanıt paketlerinin ağ üzerinden hareket etmekte olduğu süreyi ve arka uç sunucusunun yanıt vermesi gerektiğini unutmayın.|Oluşturulurken|
|BackendConnectTime|Arka uç bağlantı saati|Mayacak|Ortalama|Arka uç sunucusuyla bağlantı kurmaya harcanan süre|Dinleyici, BackendServer, BackendPool, BackendHttpSetting|
|BackendFirstByteResponseTime|Arka uç Ilk bayt yanıt süresi|Mayacak|Ortalama|Arka uç sunucusuna bağlantı kurma ve yanıt üst bilgisinin ilk baytını alma işlemi arasındaki zaman aralığı, arka uç sunucusunun işlem süresini yaklaştırmaya başladı|Dinleyici, BackendServer, BackendPool, BackendHttpSetting|
|BackendLastByteResponseTime|Arka uç son bayt yanıt süresi|Mayacak|Ortalama|Arka uç sunucusuna bağlantı kurma ve yanıt gövdesinin son baytını alma arasındaki zaman aralığı|Dinleyici, BackendServer, BackendPool, BackendHttpSetting|
|MatchedCount|Web uygulaması güvenlik duvarı toplam kural dağıtımı|Sayı|Toplam|Gelen trafik için Web uygulaması güvenlik duvarı toplam kural dağıtımı|RuleGroup, RuleId|
|BlockedCount|Web uygulaması güvenlik duvarı engellenen Istek kuralı dağıtımı|Sayı|Toplam|Web uygulaması güvenlik duvarı engellenen istek kuralı dağıtımı|RuleGroup, RuleId|
|BlockedReqCount|Web uygulaması güvenlik duvarı engellenen Istek sayısı|Sayı|Toplam|Web uygulaması güvenlik duvarı engellenen istek sayısı|Hiçbiri|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft. Network/Virtualnetworkgateway 'ler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|AverageBandwidth|Ağ Geçidi S2S bant genişliği|BytesPerSecond|Ortalama|Bayt/saniye cinsinden bir ağ geçidinin ortalama siteden siteye bant genişliği|Hiçbiri|
|P2SBandwidth|Ağ Geçidi P2S bant genişliği|BytesPerSecond|Ortalama|Bayt/saniye cinsinden bir ağ geçidinin ortalama Noktadan siteye bant genişliği|Hiçbiri|
|P2SConnectionCount|P2S bağlantı sayısı|Sayı|Maksimum|Bir ağ geçidinin Noktadan siteye bağlantı sayısı|Protokol|
|TunnelAverageBandwidth|Tünel bant genişliği|BytesPerSecond|Ortalama|Bir tünelin bayt/saniye cinsinden ortalama bant genişliği|ConnectionName, Remoteıp|
|TunnelEgressBytes|Tünel çıkış baytları|Bayt|Toplam|Bir tünelin giden baytları|ConnectionName, Remoteıp|
|TunnelIngressBytes|Tünel giriş baytları|Bayt|Toplam|Bir tünelin gelen baytları|ConnectionName, Remoteıp|
|TunnelEgressPackets|Tünel çıkış paketleri|Sayı|Toplam|Bir tünelin giden paket sayısı|ConnectionName, Remoteıp|
|TunnelIngressPackets|Tünel giriş paketleri|Sayı|Toplam|Bir tünelin gelen paket sayısı|ConnectionName, Remoteıp|
|TunnelEgressPacketDropTSMismatch|Tünel çıkış uyumsuzluğu paket bırakma|Sayı|Toplam|Bir tünelin trafik seçiciyle eşleşmeyen giden paket bırakma sayısı|ConnectionName, Remoteıp|
|TunnelIngressPacketDropTSMismatch|Tünel girişi uyumsuz paket bırakma|Sayı|Toplam|Trafik seçicideki gelen paket bırakma sayısı bir tünelin eşleşmemesi|ConnectionName, Remoteıp|


## <a name="microsoftnetworkexpressrouteports"></a>Microsoft. Network/expressRoutePorts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RxLightLevel|RxLightLevel|Sayı|Ortalama|DBm 'de RX hafif düzeyi|Bağlantı, kulvar|
|TxLightLevel|TxLightLevel|Sayı|Ortalama|DBm 'de TX hafif düzeyi|Bağlantı, kulvar|
|AdminState|AdminState|Sayı|Ortalama|Bağlantı noktasının yönetici durumu|Bağlantı|
|LineProtocol|LineProtocol|Sayı|Ortalama|Bağlantı noktasının hat protokol durumu|Bağlantı|
|PortBitsInPerSecond|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS giriş|Bağlantı|
|PortBitsOutPerSecond|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS yumursan|Bağlantı|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft. Network/Expressroutedevreleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS giriş|PeeringType|
|BitsOutPerSecond|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS yumursan|PeeringType|
|GlobalReachBitsInPerSecond|GlobalReachBitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS giriş|Peereddevresi Skey|
|GlobalReachBitsOutPerSecond|GlobalReachBitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS yumursan|Peereddevresi Skey|
|BgpAvailability|BGP kullanılabilirliği|Yüzde|Ortalama|Tüm Peers yönelik MSEE BGP kullanılabilirliği.|PeeringType, eş|
|ArpAvailability|ARP kullanılabilirliği|Yüzde|Ortalama|Tüm eşleri doğrultusunda MSEE 'tan ARP kullanılabilirliği.|PeeringType, eş|
|QosDropBitsInPerSecond|DroppedInBitsPerSecond|Sayaçpersaniye|Ortalama|Saniye başına bırakılan veri girişi bitleri|Hiçbiri|
|QosDropBitsOutPerSecond|DroppedOutBitsPerSecond|Sayaçpersaniye|Ortalama|Saniye başına bırakılan veri çıkış bitleri|Hiçbiri|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft. Network/Expressroutedevreleri/eşayarları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS giriş|Hiçbiri|
|BitsOutPerSecond|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS yumursan|Hiçbiri|

## <a name="microsoftnetworkconnections"></a>Microsoft. Network/Connections

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS giriş|Hiçbiri|
|BitsOutPerSecond|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS yumursan|Hiçbiri|

## <a name="microsoftnetworkexpressroutegateways"></a>Microsoft. Network/Expressroutegateway 'ler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ErGatewayConnectionBitsInPerSecond|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS giriş|ConnectionName|
|ErGatewayConnectionBitsOutPerSecond|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure 'da BITS yumursan|ConnectionName|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QpsByEndpoint|Bitiş noktasına göre sorgular döndürüldü|Sayı|Toplam|Verilen zaman çerçevesinde Traffic Manager uç noktasının döndürüldüğü sayı|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Uç nokta tarafından uç nokta durumu|Sayı|Maksimum|1 bir uç noktanın araştırma durumu "etkin" ise, 0 yoksa.|EndpointName|



## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft. Network/networkWatchers/Connectionmonitörleri

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Yoklama başarısız oldu|Yüzde|Ortalama|Bağlantı İzleme Araştırmaları yüzdesi başarısız oldu|Hiçbiri|
|AverageRoundtripMs|Ort. gidiş dönüş süresi (MS)|Mayacak|Ortalama|Kaynak ve hedef arasında gönderilen bağlantı izleme araştırmaları için Ortalama ağ gidiş dönüş süresi (MS)|Hiçbiri|
|ChecksFailedPercent|Başarısız denetimler yüzdesi (Önizleme)|Yüzde|Ortalama|bağlantı izleme denetimlerinin yüzdesi başarısız oldu|SourceAddress, SourceName, Sourceresourceıd, SourceType, protokol, DestinationAddress, DestinationName, Destinationresourceıd, DestinationType, DestinationPort, TestGroupName, TestConfigurationName|
|Roundroundtimems|Gidiş dönüş süresi (MS) (Önizleme)|Mayacak|Ortalama|Bağlantı izleme denetimleri için milisaniye cinsinden gidiş dönüş süresi|SourceAddress, SourceName, Sourceresourceıd, SourceType, protokol, DestinationAddress, DestinationName, Destinationresourceıd, DestinationType, DestinationPort, TestGroupName, TestConfigurationName|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft. Network/frontkapaklı

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|RequestCount|İstek Sayısı|Sayı|Toplam|HTTP/S proxy tarafından hizmet verilen istemci isteklerinin sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|İstek boyutu|Bayt|Toplam|İstemcilerden HTTP/S proxy 'sine istek olarak gönderilen bayt sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|Yanıt boyutu|Yanıt boyutu|Bayt|Toplam|HTTP/S proxy 'sinden istemcilere yanıt olarak gönderilen baytların sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|Billilableresponsesize|Faturalanabilir yanıt boyutu|Bayt|Toplam|HTTP/S proxy 'sinden istemcilere yanıt olarak gönderilen faturalandırılabilir bayt sayısı (istek başına en az 2 KB).|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Arka uç Istek sayısı|Sayı|Toplam|HTTP/S proxy 'sinden arka uçlara gönderilen isteklerin sayısı|HttpStatus, HttpStatusGroup, arka uç|
|BackendRequestLatency|Arka uç Isteği gecikmesi|Mayacak|Ortalama|HTTP/s proxy, arka ucun son yanıt baytını almayana kadar isteğin HTTP/S proxy tarafından arka uca gönderildiği süre|Sunucusundan|
|TotalLatency|Toplam gecikme süresi|Mayacak|Ortalama|İstemci, http/s proxy 'sinden gelen son yanıt baytını kabul edene kadar, istemci isteğinin HTTP/S proxy tarafından alındığı zamana göre hesaplanır|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Arka uç sistem durumu yüzdesi|Yüzde|Ortalama|HTTP/S proxy 'sinden arka uçlara başarılı sistem durumu araştırmalarının yüzdesi|Arka uç, BackendPool|
|WebApplicationFirewallRequestCount|Web uygulaması güvenlik duvarı Istek sayısı|Sayı|Toplam|Web uygulaması güvenlik duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName, RuleName, eylem|


## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|kayıt. tümü|Kayıt Işlemleri|Sayı|Toplam|Tüm başarılı kayıt işlemlerinin sayısı (güncelleştirme sorguları ve silmeleri oluşturma). |Hiçbiri|
|kayıt. oluştur|Kayıt oluşturma Işlemleri|Sayı|Toplam|Tüm başarılı kayıt oluşturma işlemlerinin sayısı.|Hiçbiri|
|kayıt. güncelleştirme|Kayıt güncelleştirme Işlemleri|Sayı|Toplam|Tüm başarılı kayıt güncelleştirmelerinin sayısı.|Hiçbiri|
|kayıt. Get|Kayıt okuma Işlemleri|Sayı|Toplam|Tüm başarılı kayıt sorgularının sayısı.|Hiçbiri|
|kayıt. Sil|Kayıt silme Işlemleri|Sayı|Toplam|Tüm başarılı kayıt silme işlemlerinin sayısı.|Hiçbiri|
|gelen|Gelen Iletiler|Sayı|Toplam|Başarılı bir gönderme API çağrısı sayısı. |Hiçbiri|
|gelen. zamanlandı|Zamanlanan anında Iletme bildirimleri gönderildi|Sayı|Toplam|Zamanlanan anında Iletme bildirimleri Iptal edildi|Hiçbiri|
|gelen. zamanlandı. iptal|Zamanlanan anında Iletme bildirimleri Iptal edildi|Sayı|Toplam|Zamanlanan anında Iletme bildirimleri Iptal edildi|Hiçbiri|
|scheduled.pending|Bekleyen zamanlanmış bildirimler|Sayı|Toplam|Bekleyen zamanlanmış bildirimler|Hiçbiri|
|yükleme. tümü|Yükleme yönetimi Işlemleri|Sayı|Toplam|Yükleme yönetimi Işlemleri|Hiçbiri|
|yükleme. Get|Yükleme Işlemlerini al|Sayı|Toplam|Yükleme Işlemlerini al|Hiçbiri|
|yükleme. upsert|Yükleme Işlemlerini oluşturma veya güncelleştirme|Sayı|Toplam|Yükleme Işlemlerini oluşturma veya güncelleştirme|Hiçbiri|
|yükleme. Patch|Düzeltme eki yükleme Işlemleri|Sayı|Toplam|Düzeltme eki yükleme Işlemleri|Hiçbiri|
|yükleme. Delete|Yükleme Işlemlerini silme|Sayı|Toplam|Yükleme Işlemlerini silme|Hiçbiri|
|giden. allpns. Success|Başarılı bildirimler|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|Hiçbiri|
|giden. allpns. ınvalidpayload|Yük hataları|Sayı|Toplam|PNS hatalı yük hatası döndürdüğünden başarısız olan gönderim sayısı.|Hiçbiri|
|outgoing.allpns.pnserror|Dış bildirim sistemi hataları|Sayı|Toplam|PNS ile iletişim kurulurken bir sorun oluştuğundan başarısız olan gönderim sayısı (kimlik doğrulama sorunları hariç).|Hiçbiri|
|outgoing.allpns.channelerror|Kanal hataları|Sayı|Toplam|Kanal geçersiz olduğu için doğru uygulamayla ilişkili olmayan veya geçerliliği aşıldığı için başarısız olan gönderim sayısı.|Hiçbiri|
|outgoing.allpns.badorexpiredchannel|Hatalı veya süre dolma kanalı hataları|Sayı|Toplam|Kayıttaki kanal/belirteç/RegistrationId 'nin geçerliliği aşıldığı veya geçersiz olduğu için başarısız olan gönderim sayısı.|Hiçbiri|
|giden. WNS. Success|WNS başarılı bildirimleri|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|Hiçbiri|
|giden. WNS. ınvalidcredentials|WNS yetkilendirme hataları (geçersiz kimlik bilgileri)|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı. (Windows Live kimlik bilgilerini tanımaz).|Hiçbiri|
|giden. WNS. badchannel|WNS hatalı kanal hatası|Sayı|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (WNS durumu: 404 bulunamadı).|Hiçbiri|
|outgoing.wns.expiredchannel|WNS süre dolduğunda kanal hatası|Sayı|Toplam|ChannelURI 'nin geçerliliği aşıldığı için başarısız olan gönderim sayısı (WNS durumu: 410 gitti).|Hiçbiri|
|giden. WNS. kısıtlanıyor|WNS kısıtlanıyor bildirimleri|Sayı|Toplam|WNS bu uygulamayı azaltdığı için başarısız olan gönderim sayısı (WNS durumu: 406 kabul edilemez).|Hiçbiri|
|giden. WNS. tokenproviderunreachable|WNS yetkilendirme hataları (ulaşılamıyor)|Sayı|Toplam|Windows Live ulaşılamıyor.|Hiçbiri|
|giden. WNS. ınvalidtoken|WNS yetkilendirme hataları (geçersiz belirteç)|Sayı|Toplam|WNS öğesine sunulan belirteç geçerli değil (WNS Status: 401 Yetkisiz).|Hiçbiri|
|giden. WNS. yanlışlıkla gtoken|WNS yetkilendirme hataları (yanlış belirteç)|Sayı|Toplam|WNS öğesine sunulan belirteç geçerli, ancak başka bir uygulama için (WNS durumu: 403 Yasak). Kayıttaki Channelurı başka bir uygulamayla ilişkiliyse bu durum oluşabilir. İstemci uygulamasının kimlik bilgileri Bildirim Hub 'ında olan aynı uygulamayla ilişkilendirildiğinden emin olun.|Hiçbiri|
|giden. WNS. ınvalidnotificationformat|WNS geçersiz bildirim biçimi|Sayı|Toplam|Bildirimin biçimi geçersiz (WNS durumu: 400). WNS 'nin tüm geçersiz yükleri reddetmediğini unutmayın.|Hiçbiri|
|giden. WNS. ınvalidnotificationsize|WNS geçersiz bildirim boyutu hatası|Sayı|Toplam|Bildirim yükü çok büyük (WNS durumu: 413).|Hiçbiri|
|giden. WNS. channelkısıtlanıyor|WNS kanal kısıtlandı|Sayı|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (WNS yanıt üst bilgisi: X-WNS-NotificationStatus: Channelkısıtlanıyor).|Hiçbiri|
|outgoing.wns.channeldisconnected|WNS kanal bağlantısı kesildi|Sayı|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (WNS yanıt üst bilgisi: X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Hiçbiri|
|giden. WNS. bırakıldı|WNS bırakılan bildirimler|Sayı|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (X-WNS-NotificationStatus: bırakıldı ancak X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Hiçbiri|
|outgoing.wns.pnserror|WNS hataları|Sayı|Toplam|Bildirim, WNS ile iletişim kuran hatalar nedeniyle teslim edilemiyor.|Hiçbiri|
|outgoing.wns.authenticationerror|WNS kimlik doğrulama hataları|Sayı|Toplam|Windows Live geçersiz kimlik bilgileri veya yanlış belirteçle iletişim kurarken oluşan hatalar nedeniyle bildirim teslim edilemiyor.|Hiçbiri|
|giden. APNs. Success|APNS başarılı bildirimler|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|Hiçbiri|
|giden. APNs. ınvalidcredentials|APNS yetkilendirme hataları|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Hiçbiri|
|giden. APNs. badchannel|APNS geçersiz kanal hatası|Sayı|Toplam|Belirteç geçersiz olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 8).|Hiçbiri|
|outgoing.apns.expiredchannel|APNS süre dolma kanalı hatası|Sayı|Toplam|APNS geri bildirim kanalı tarafından geçersiz kılınan belirteç sayısı.|Hiçbiri|
|giden. APNs. ınvalidnotificationsize|APNS geçersiz bildirim boyutu hatası|Sayı|Toplam|Yük çok büyük olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 7).|Hiçbiri|
|giden. APNs. pnserror|APNS hataları|Sayı|Toplam|APNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Hiçbiri|
|giden. gcm. Success|GCM başarılı bildirimleri|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|Hiçbiri|
|giden. gcm. ınvalidcredentials|GCM yetkilendirme hataları (geçersiz kimlik bilgileri)|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Hiçbiri|
|giden. gcm. badchannel|GCM hatalı kanal hatası|Sayı|Toplam|Kayıttaki RegistrationId tanınmadığı için başarısız olan gönderim sayısı (GCM sonucu: geçersiz kayıt).|Hiçbiri|
|giden. gcm. expiredchannel|GCM süre aşımına uğradı kanal hatası|Sayı|Toplam|Kayıttaki RegistrationId 'nin zaman aşımına uğradığından başarısız olan gönderim sayısı (GCM sonucu: NotRegistered).|Hiçbiri|
|giden. gcm. kısıtlanıyor|GCM kısıtlanan bildirimler|Sayı|Toplam|GCM tarafından kısıtlanan bu uygulamayı (GCM durum kodu: 501-599 veya sonuç: kullanılamıyor) nedeniyle başarısız olan gönderim sayısı.|Hiçbiri|
|giden. gcm. ınvalidnotificationformat|GCM geçersiz bildirim biçimi|Sayı|Toplam|Yük doğru biçimlendirilmediği için başarısız olan gönderim sayısı (GCM sonucu: ınvaliddatakey veya ınvalidttl).|Hiçbiri|
|giden. gcm. ınvalidnotificationsize|GCM geçersiz bildirim boyutu hatası|Sayı|Toplam|Yük çok büyük olduğu için başarısız olan gönderim sayısı (GCM sonucu: Messagetoobıg).|Hiçbiri|
|outgoing.gcm.wrongchannel|GCM yanlış kanal hatası|Sayı|Toplam|Kayıttaki RegistrationId geçerli uygulamayla ilişkili olmadığından başarısız olan gönderim sayısı (GCM sonucu: ınvalidpackagename).|Hiçbiri|
|outgoing.gcm.pnserror|GCM hataları|Sayı|Toplam|GCM ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Hiçbiri|
|giden. gcm. authenticationerror|GCM kimlik doğrulama hataları|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden başarısız olan gönderim sayısı, kimlik bilgilerinin engellenmesi veya SenderId uygulamada doğru yapılandırılmamış (GCM sonucu: Mismatchedsenderıd).|Hiçbiri|
|giden. MPNS. Success|MPNS başarılı bildirimleri|Sayı|Toplam|Tüm başarılı bildirimlerin sayısı.|Hiçbiri|
|giden. MPNS. ınvalidcredentials|MPNS geçersiz kimlik bilgileri|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Hiçbiri|
|giden. MPNS. badchannel|MPNS hatalı kanal hatası|Sayı|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (MPNS durumu: 404 bulunamadı).|Hiçbiri|
|giden. MPNS. kısıtlanıyor|MPNS kısıtlanan bildirimler|Sayı|Toplam|MPNS bu uygulamayı azaltdığı için başarısız olan gönderim sayısı (WNS MPNS: 406 kabul edilemez).|Hiçbiri|
|giden. MPNS. ınvalidnotificationformat|MPNS geçersiz bildirim biçimi|Sayı|Toplam|Bildirimin yükü çok büyük olduğu için başarısız olan gönderim sayısı.|Hiçbiri|
|outgoing.mpns.channeldisconnected|MPNS kanalının bağlantısı kesildi|Sayı|Toplam|Kayıttaki Channelurı bağlantısı kesildiğinden başarısız olan gönderim sayısı (MPNS durumu: 412 bulunamadı).|Hiçbiri|
|giden. MPNS. bırakıldı|MPNS bırakılan bildirimler|Sayı|Toplam|MPNS tarafından bırakılan gönderim sayısı (MPNS yanıt üst bilgisi: X-NotificationStatus: QueueFull veya gizlendi).|Hiçbiri|
|outgoing.mpns.pnserror|MPNS hataları|Sayı|Toplam|MPNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Hiçbiri|
|outgoing.mpns.authenticationerror|MPNS kimlik doğrulama hataları|Sayı|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Hiçbiri|
|notificationhub. gönderimleri|Tüm giden bildirimler|Sayı|Toplam|Bildirim Hub 'ının tüm giden bildirimleri|Hiçbiri|
|gelen. ALL. Requests|Tüm gelen Istekler|Sayı|Toplam|Bir Bildirim Hub 'ı için toplam gelen istek sayısı|Hiçbiri|
|gelen. tüm. failedistekleri|Tüm gelen başarısız Istekler|Sayı|Toplam|Bir Bildirim Hub 'ı için toplam gelen başarısız istek sayısı|Hiçbiri|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft. Operationalınsights/çalışma alanları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Average_% boş ınomdes|% Boş ınomdes|Sayı|Ortalama|Average_% boş ınomdes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|% Average_ boş alan|% Boş alan|Sayı|Ortalama|% Average_ boş alan|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılan ınomdes|% Kullanılan ınomdes|Sayı|Ortalama|Average_% kullanılan ınomdes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Kullanılan Average_ alan yüzdesi|% Kullanılan alan|Sayı|Ortalama|Kullanılan Average_ alan yüzdesi|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk okunan bayt/sn|Disk Okuma Bayt/sn|Sayı|Ortalama|Average_Disk okunan bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Okuma/sn Average_Disk|Disk Okuma/sn|Sayı|Ortalama|Okuma/sn Average_Disk|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk aktarım sayısı/sn|Disk aktarımı/sn|Sayı|Ortalama|Average_Disk aktarım sayısı/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk yazma bayt/sn|Disk Yazma Bayt/sn|Sayı|Ortalama|Average_Disk yazma bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk yazma/sn|Disk Yazma/sn|Sayı|Ortalama|Average_Disk yazma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Free megabayt|Boş megabayt|Sayı|Ortalama|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Logical disk bayt/sn|Mantıksal Disk Bayt/sn|Sayı|Ortalama|Average_Logical disk bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Kullanılabilir% Average_ bellek|% Kullanılabilir bellek|Sayı|Ortalama|Kullanılabilir% Average_ bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılabilir takas alanı|% Kullanılabilir takas alanı|Sayı|Ortalama|Average_% kullanılabilir takas alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Kullanılan% Average_ bellek|% Kullanılan bellek|Sayı|Ortalama|Kullanılan% Average_ bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılan takas alanı|% Kullanılan takas alanı|Sayı|Ortalama|Average_% kullanılan takas alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt bellek|Kullanılabilir MBayt belleği|Sayı|Ortalama|Average_Available MBayt bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt takas|Kullanılabilir MBayt takas|Sayı|Ortalama|Average_Available MBayt takas|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Okuma/sn Average_Page|Sayfa Okuma/sn|Sayı|Ortalama|Okuma/sn Average_Page|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Page yazma/sn|Sayfa yazma/sn|Sayı|Ortalama|Average_Page yazma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pages/sn|Sayfa/sn|Sayı|Ortalama|Average_Pages/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used MBayt alanı değiştirme|Kullanılan MBayt değiştirme alanı|Sayı|Ortalama|Average_Used MBayt alanı değiştirme|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used belleği MBayt|Kullanılan bellek MBayt|Sayı|Ortalama|Average_Used belleği MBayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total aktarılan baytlar|Aktarılan toplam bayt|Sayı|Ortalama|Average_Total aktarılan baytlar|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total alınan bayt|Alınan toplam bayt sayısı|Sayı|Ortalama|Average_Total alınan bayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total bayt|Toplam bayt sayısı|Sayı|Ortalama|Average_Total bayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Iletilen Average_Total paketleri|Aktarılan toplam paket sayısı|Sayı|Ortalama|Iletilen Average_Total paketleri|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Alınan Average_Total paketleri|Alınan toplam paket sayısı|Sayı|Ortalama|Alınan Average_Total paketleri|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total RX hataları|Toplam RX hatası sayısı|Sayı|Ortalama|Average_Total RX hataları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total TX hataları|Toplam TX hatası sayısı|Sayı|Ortalama|Average_Total TX hataları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total çakışmaları|Toplam çakışma sayısı|Sayı|Ortalama|Average_Total çakışmaları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Okuma|Ortalama Disk sn/okuma|Sayı|Ortalama|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Aktarım|Ortalama Disk sn/Aktarım|Sayı|Ortalama|Average_Avg. Disk sn/Aktarım|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/yazma|Ortalama Disk sn/yazma|Sayı|Ortalama|Average_Avg. Disk sn/yazma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Physical disk bayt/sn|Fiziksel disk bayt/sn|Sayı|Ortalama|Average_Physical disk bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pct ayrıcalıklı saat|Ayrıcalıklı zaman yüzdesi|Sayı|Ortalama|Average_Pct ayrıcalıklı saat|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pct Kullanıcı saati|Kullanım süresi yüzdesi|Sayı|Ortalama|Average_Pct Kullanıcı saati|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used belleği Kbayt|Kullanılan bellek Kbayt|Sayı|Ortalama|Average_Used belleği Kbayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Virtual paylaşılan bellek|Sanal paylaşılan bellek|Sayı|Ortalama|Average_Virtual paylaşılan bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% DPC Zamanı|% DPC Zamanı|Sayı|Ortalama|Average_% DPC Zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% boşta kalma süresi|% Boşta kalma süresi|Sayı|Ortalama|Average_% boşta kalma süresi|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Kesme zamanı Average_%|Kesme zamanı yüzdesi|Sayı|Ortalama|Kesme zamanı Average_%|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% GÇ bekleme süresi|% GÇ bekleme süresi|Sayı|Ortalama|Average_% GÇ bekleme süresi|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Iyi zaman|% İyi zaman|Sayı|Ortalama|Average_% Iyi zaman|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% ayrıcalıklı zaman|% Ayrıcalıklı zaman|Sayı|Ortalama|Average_% ayrıcalıklı zaman|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Işlemci zamanı|% İşlemci zamanı|Sayı|Ortalama|Average_% Işlemci zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Kullanıcı Zamanı|% Kullanıcı Zamanı|Sayı|Ortalama|Average_% Kullanıcı Zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Fiziksel bellek Average_Free|Boş fiziksel bellek|Sayı|Ortalama|Fiziksel bellek Average_Free|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Disk belleği dosyalarında Average_Free alanı|Disk belleği dosyalarındaki boş alan|Sayı|Ortalama|Disk belleği dosyalarında Average_Free alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Sanal bellek Average_Free|Boş sanal bellek|Sayı|Ortalama|Sanal bellek Average_Free|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Processes|İşlemler|Sayı|Ortalama|Average_Processes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Disk belleği dosyalarında depolanan Average_Size|Disk belleği dosyalarında depolanan boyut|Sayı|Ortalama|Disk belleği dosyalarında depolanan Average_Size|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Uptime|Çalışma Süresi|Sayı|Ortalama|Average_Uptime|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Users|Kullanıcılar|Sayı|Ortalama|Average_Users|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Okuma|Ortalama Disk sn/okuma|Sayı|Ortalama|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/yazma|Ortalama Disk sn/yazma|Sayı|Ortalama|Average_Avg. Disk sn/yazma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Current disk kuyruğu uzunluğu|Geçerli disk sırası uzunluğu|Sayı|Ortalama|Average_Current disk kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Okuma/sn Average_Disk|Disk Okuma/sn|Sayı|Ortalama|Okuma/sn Average_Disk|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk aktarım sayısı/sn|Disk aktarımı/sn|Sayı|Ortalama|Average_Disk aktarım sayısı/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk yazma/sn|Disk Yazma/sn|Sayı|Ortalama|Average_Disk yazma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Free megabayt|Boş megabayt|Sayı|Ortalama|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|% Average_ boş alan|% Boş alan|Sayı|Ortalama|% Average_ boş alan|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt|Kullanılabilir MBayt|Sayı|Ortalama|Average_Available MBayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|% Average_ kaydedilmiş bayt kullanımda|Kullanılan kaydedilmiş bayt yüzdesi|Sayı|Ortalama|% Average_ kaydedilmiş bayt kullanımda|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Alınan Average_Bytes/sn|Alınan Bayt/sn|Sayı|Ortalama|Alınan Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Gönderilen Average_Bytes/sn|Gönderilen bayt/sn|Sayı|Ortalama|Gönderilen Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Toplam Average_Bytes/sn|Toplam bayt/sn|Sayı|Ortalama|Toplam Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Işlemci zamanı|% İşlemci zamanı|Sayı|Ortalama|Average_% Işlemci zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Processor kuyruğu uzunluğu|İşlemci kuyruğu uzunluğu|Sayı|Ortalama|Average_Processor kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Sinyal|Sinyal|Sayı|Toplam|Sinyal|Bilgisayar, OSType, sürüm, Sourcecomputerıd|
|Güncelleştirme|Güncelleştirme|Sayı|Ortalama|Güncelleştirme|Bilgisayar, ürün, sınıflandırma, UpdateState, Isteğe bağlı, onaylanan|
|Olay|Olay|Sayı|Ortalama|Olay|Kaynak, olay günlüğü, bilgisayar, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpeeringpeeringservices"></a>Microsoft. eşleme/peeringServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|PrefixLatency|Ön ek gecikmesi|Milisaniye|Ortalama|Ortanca ön ek gecikmesi|PrefixName|

## <a name="microsoftpeeringpeerings"></a>Microsoft. eşleme/eşlemeler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SessionAvailabilityV4|Oturum kullanılabilirliği v4|Yüzde|Ortalama|V4 oturumunun kullanılabilirliği|ConnectionID|
|SessionAvailabilityV6|Oturum kullanılabilirliği V6|Yüzde|Ortalama|V6 oturumunun kullanılabilirliği|ConnectionID|
|IngressTrafficRate|Giriş trafiği oranı|BitsPerSecond|Ortalama|Bit/saniye cinsinden giriş trafiği oranı|ConnectionID|
|EgressTrafficRate|Çıkış trafik oranı|BitsPerSecond|Ortalama|Bit/saniye cinsinden çıkış trafiği oranı|ConnectionID|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft. Powerbiadanmış/kapasiteler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueryDuration|Sorgu süresi|Milisaniye|Ortalama|Son aralıktaki DAX sorgu süresi|Boyut yok|
|QueryPoolJobQueueLength|İş parçacıkları: sorgu havuzu iş kuyruğu uzunluğu|Sayı|Ortalama|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|Boyut yok|
|qpu_high_utilization_metric|QPU yüksek kullanım|Sayı|Toplam|Son dakikada QPU yüksek kullanımı, yüksek QPU kullanımı Için 1, aksi durumda 0|Boyut yok|
|memory_metric|Hafıza|Bayt|Ortalama|Bellek. A1 için 0-3 GB, a2 için 0-5 GB, a3 için 0-10 GB, A4 için 0-25 GB, A6 için 0-50 GB ve A6 için 0-100 GB|Boyut yok|
|memory_thrashing_metric|Bellek dağılması|Yüzde|Ortalama|Ortalama bellek miktarı.|Boyut yok|



## <a name="microsoftrelaynamespaces"></a>Microsoft. Relay/Namespace

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ListenerConnections-başarılı|ListenerConnections-başarılı|Sayı|Toplam|Microsoft. Relay için başarılı bir ListenerConnections.|EntityName, OperationResult|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Sayı|Toplam|Microsoft. Relay için ListenerConnections 'da ClientError.|EntityName, OperationResult|
|ListenerConnections-Sunucuhatası|ListenerConnections-Sunucuhatası|Sayı|Toplam|Microsoft. Relay için ListenerConnections 'da ServerError.|EntityName, OperationResult|
|SenderConnections-başarılı|SenderConnections-başarılı|Sayı|Toplam|Microsoft. Relay için başarılı SenderConnections.|EntityName, OperationResult|
|SenderConnections-ClientError|SenderConnections-ClientError|Sayı|Toplam|Microsoft. Relay için SenderConnections 'ta ClientError.|EntityName, OperationResult|
|SenderConnections-Sunucuhatası|SenderConnections-Sunucuhatası|Sayı|Toplam|Microsoft. Relay için SenderConnections 'ta ServerError.|EntityName, OperationResult|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Sayı|Toplam|Microsoft. Relay için toplam ListenerConnections.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Sayı|Toplam|Microsoft. Relay için toplam SenderConnections isteği.|EntityName|
|ActiveConnection sayısı|ActiveConnection sayısı|Sayı|Toplam|Microsoft. Relay için toplam ActiveConnections.|EntityName|
|ActiveListeners|ActiveListeners|Sayı|Toplam|Microsoft. Relay için toplam ActiveListeners.|EntityName|
|BytesTransferred|BytesTransferred|Sayı|Toplam|Microsoft. Relay için toplam BytesTransferred.|EntityName|
|Listenerdisconnect|Listenerdisconnect|Sayı|Toplam|Microsoft. Relay için toplam Listenerdisconnect.|EntityName|
|Gönderenin bağlantısı kesiliyor|Gönderenin bağlantısı kesiliyor|Sayı|Toplam|Microsoft. Relay için toplam Senderdisconnect.|EntityName|


## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SearchLatency|Arama gecikmesi|Saniye|Ortalama|Arama hizmeti için Ortalama arama gecikmesi|Hiçbiri|
|SearchQueriesPerSecond|Sorgu arama/saniye|Sayaçpersaniye|Ortalama|Arama hizmeti için saniye başına arama sorgusu|Hiçbiri|
|Tüm Searchqueriespercentage 'ı kısıtlar|Kısıtlanmış arama sorguları yüzdesi|Yüzde|Ortalama|Arama hizmeti için kısıtlanan arama sorgularının yüzdesi|Hiçbiri|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SuccessfulRequests|Başarılı Istekler|Sayı|Toplam|Bir ad alanı için toplam başarılı istek sayısı|EntityName, OperationResult|
|ServerErrors|Sunucu hataları.|Sayı|Toplam|Microsoft. ServiceBus için sunucu hataları.|EntityName, OperationResult|
|UserErrors|Kullanıcı hataları.|Sayı|Toplam|Microsoft. ServiceBus için Kullanıcı hataları.|EntityName, OperationResult|
|OperationName|Kısıtlanmış Istekler.|Sayı|Toplam|Microsoft. ServiceBus için kısıtlanan Istekler.|EntityName, OperationResult|
|Incomingrequests|Gelen Istekler|Sayı|Toplam|Microsoft. ServiceBus için gelen Istekler.|EntityName|
|Incomingmessages|Gelen Iletiler|Sayı|Toplam|Microsoft. ServiceBus için gelen Iletiler.|EntityName|
|OutgoingMessages|Giden Iletiler|Sayı|Toplam|Microsoft. ServiceBus için giden Iletiler.|EntityName|
|ActiveConnection sayısı|ActiveConnection sayısı|Sayı|Toplam|Microsoft. ServiceBus için toplam etkin bağlantı sayısı.|Hiçbiri|
|ConnectionsOpened|Bağlantılar açıldı.|Sayı|Ortalama|Microsoft. ServiceBus için açılan bağlantılar.|EntityName|
|ConnectionsClosed|Bağlantı kapatıldı.|Sayı|Ortalama|Microsoft. ServiceBus için kapatılan bağlantılar.|EntityName|
|Boyut|Boyut|Bayt|Ortalama|Bir kuyruğun/konunun bayt cinsinden boyutu.|EntityName|
|Mesajlar|Kuyruktaki/konudaki iletilerin sayısı.|Sayı|Ortalama|Kuyruktaki/konudaki iletilerin sayısı.|EntityName|
|ActiveMessages|Bir kuyruktaki/konudaki etkin ileti sayısı.|Sayı|Ortalama|Bir kuyruktaki/konudaki etkin ileti sayısı.|EntityName|
|DeadletteredMessages|Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı.|Sayı|Ortalama|Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı.|EntityName|
|ScheduledMessages|Bir kuyruktaki/konudaki zamanlanan ileti sayısı.|Sayı|Ortalama|Bir kuyruktaki/konudaki zamanlanan ileti sayısı.|EntityName|
|NamespaceCpuUsage|CPU|Yüzde|Maksimum|Service Bus Premium ad alanı CPU kullanımı ölçümü.|Çoğaltma|
|NamespaceMemoryUsage|Bellek kullanımı|Yüzde|Maksimum|Service Bus Premium ad alanı bellek kullanımı ölçümü.|Çoğaltma|
|CPUXNS|CPU (kullanım dışı)|Yüzde|Maksimum|Service Bus Premium ad alanı CPU kullanımı ölçümü. Bu ölçüm eşit olarak dağıtılır. Lütfen bunun yerine CPU ölçümünü kullanın (NamespaceCpuUsage).|Çoğaltma|
|WSXNS|Bellek kullanımı (kullanım dışı)|Yüzde|Maksimum|Service Bus Premium ad alanı bellek kullanımı ölçümü. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine bellek kullanımı (NamespaceMemoryUsage) ölçüsünü kullanın.|Çoğaltma|


## <a name="microsoftservicefabricmeshapplications"></a>Microsoft. Servicefabrickafes/uygulamaları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Ayrılatedcpu|Ayrılatedcpu|Sayı|Ortalama|Milli çekirdekleri içinde bu kapsayıcıya ayrılan CPU|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Ayrılan bellek|Ayrılan bellek|Bayt|Ortalama|Bu kapsayıcıya MB cinsinden ayrılan bellek|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualCpu|ActualCpu|Sayı|Ortalama|Milli çekirdekte gerçek CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualMemory|ActualMemory|Bayt|Ortalama|MB cinsinden gerçek bellek kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Cpukullanımı|Cpukullanımı|Yüzde|Ortalama|Ayrılan CPU yüzdesi olarak bu kapsayıcı için CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Memoryutilileştirme|Memoryutilileştirme|Yüzde|Ortalama|Ayrılan CPU yüzdesi olarak bu kapsayıcı için CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ApplicationStatus|ApplicationStatus|Sayı|Ortalama|Service Fabric kafes uygulamasının durumu|ApplicationName, durum|
|ServiceStatus|ServiceStatus|Sayı|Ortalama|Service Fabric kafes uygulamasındaki bir hizmetin sistem durumu|ApplicationName, durum, HizmetAdı|
|Servicereperepstatus|Servicereperepstatus|Sayı|Ortalama|Service Fabric kafes uygulamasındaki bir hizmet çoğaltmasının sistem durumu|ApplicationName, Status, HizmetAdı, ServiceReplicaName|
|ContainerStatus|ContainerStatus|Sayı|Ortalama|Service Fabric kafes uygulamasındaki kapsayıcının durumu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName, durum|
|RestartCount|RestartCount|Sayı|Ortalama|Service Fabric kafes uygulamasındaki bir kapsayıcının yeniden başlatma sayısı|ApplicationName, Status, HizmetAdı, ServiceReplicaName, CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft. SignalRService/SignalR

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ConnectionCount|Bağlantı sayısı|Sayı|Maksimum|Kullanıcı bağlantısı miktarı.|Uç nokta|
|MessageCount|İleti sayısı|Sayı|Toplam|Toplam ileti miktarı.|Hiçbiri|
|Inboundtraffic|Gelen Trafik|Bayt|Toplam|Hizmetin gelen trafiği|Hiçbiri|
|OutboundTraffic|Giden Trafik|Bayt|Toplam|Hizmetin giden trafiği|Hiçbiri|
|UserErrors|Kullanıcı hataları|Yüzde|Maksimum|Kullanıcı hatalarının yüzdesi|Hiçbiri|
|SystemErrors|Sistem hataları|Yüzde|Maksimum|Sistem hatalarının yüzdesi|Hiçbiri|





## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiçbiri|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|Hiçbiri|
|log_write_percent|Günlük GÇ yüzdesi|Yüzde|Ortalama|Günlük GÇ yüzdesi. Veri ambarlarında geçerli değildir.|Hiçbiri|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi. DTU tabanlı veritabanları için geçerlidir.|Hiçbiri|
|depolama|Kullanılan veri alanı|Bayt|Maksimum|Kullanılan veri alanı. Veri ambarlarında geçerli değildir.|Hiçbiri|
|connection_successful|Başarılı bağlantılar|Sayı|Toplam|Başarılı bağlantılar|Hiçbiri|
|connection_failed|Başarısız Bağlantılar|Sayı|Toplam|Başarısız Bağlantılar|Hiçbiri|
|blocked_by_firewall|Güvenlik duvarı tarafından engellendi|Sayı|Toplam|Güvenlik duvarı tarafından engellendi|Hiçbiri|
|Çözül|Çık|Sayı|Toplam|Çık. Veri ambarlarında geçerli değildir.|Hiçbiri|
|storage_percent|Kullanılan veri alanı yüzdesi|Yüzde|Maksimum|Kullanılan veri alanı yüzdesi. Veri ambarları veya hiper ölçekli veritabanları için geçerli değildir.|Hiçbiri|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek içi OLTP depolaması yüzdesi. Veri ambarlarında geçerli değildir.|Hiçbiri|
|workers_percent|Çalışan yüzdesi|Yüzde|Ortalama|Çalışan yüzdesi. Veri ambarlarında geçerli değildir.|Hiçbiri|
|sessions_percent|Oturum yüzdesi|Yüzde|Ortalama|Oturum yüzdesi. Veri ambarlarında geçerli değildir.|Hiçbiri|
|dtu_limit|DTU sınırı|Sayı|Ortalama|DTU sınırı. DTU tabanlı veritabanları için geçerlidir.|Hiçbiri|
|dtu_used|Kullanılan DTU|Sayı|Ortalama|DTU kullanıldı. DTU tabanlı veritabanları için geçerlidir.|Hiçbiri|
|cpu_limit|CPU sınırı|Sayı|Ortalama|CPU sınırı. Sanal çekirdek tabanlı veritabanları için geçerlidir.|Hiçbiri|
|cpu_used|Kullanılan CPU|Sayı|Ortalama|Kullanılan CPU. Sanal çekirdek tabanlı veritabanları için geçerlidir.|Hiçbiri|
|dwu_limit|DWU sınırı|Sayı|Maksimum|DWU sınırı. Yalnızca veri ambarlarında geçerlidir.|Hiçbiri|
|dwu_consumption_percent|DWU yüzdesi|Yüzde|Maksimum|DWU yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Hiçbiri|
|dwu_used|Kullanılan DWU|Sayı|Maksimum|DWU kullanıldı. Yalnızca veri ambarlarında geçerlidir.|Hiçbiri|
|cache_hit_percent|İsabetli önbellek okuması yüzdesi|Yüzde|Maksimum|İsabetli önbellek okuması yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Hiçbiri|
|cache_used_percent|Önbellek kullanılan yüzde|Yüzde|Maksimum|Önbellek kullanım yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Hiçbiri|
|sqlserver_process_core_percent|SQL Server işlem çekirdeği yüzdesi|Yüzde|Maksimum|SQL Server işlemi için işletim sistemi tarafından ölçülen CPU kullanım yüzdesi. Şu anda yalnızca sunucusuz veritabanları için kullanılabilir.|Hiçbiri|
|sqlserver_process_memory_percent|SQL Server işlem belleği yüzdesi|Yüzde|Maksimum|SQL Server işlemi için işletim sistemi tarafından ölçülen bellek kullanım yüzdesi. Şu anda yalnızca sunucusuz veritabanları için kullanılabilir.|Hiçbiri|
|tempdb_data_size|Tempdb veri dosyası boyutu kilobayt|Sayı|Maksimum|Tempdb veri dosyası boyutu kilobayt. Veri ambarlarında geçerli değildir.|Hiçbiri|
|tempdb_log_size|Tempdb günlük dosyası boyutu kilobayt|Sayı|Maksimum|Tempdb günlük dosyası boyutu kilobayt. Veri ambarlarında geçerli değildir.|Hiçbiri|
|tempdb_log_used_percent|Kullanılan tempdb günlüğü yüzdesi|Yüzde|Maksimum|Kullanılan tempdb günlüğü yüzdesi. Veri ambarlarında geçerli değildir.|Hiçbiri|
|local_tempdb_usage_percent|Yerel tempdb yüzdesi|Yüzde|Ortalama|Yerel tempdb yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Hiçbiri|
|app_cpu_billed|Faturalandırılan uygulama CPU 'SU|Sayı|Toplam|Uygulama CPU 'SU faturalandırılır. Sunucusuz veritabanları için geçerlidir.|Hiçbiri|
|app_cpu_percent|Uygulama CPU yüzdesi|Yüzde|Ortalama|Uygulama CPU yüzdesi. Sunucusuz veritabanları için geçerlidir.|Hiçbiri|
|app_memory_percent|Uygulama belleği yüzdesi|Yüzde|Ortalama|Uygulama belleği yüzdesi. Sunucusuz veritabanları için geçerlidir.|Hiçbiri|
|allocated_data_storage|Ayrılan veri alanı|Bayt|Ortalama|Ayrılan veri depolama alanı. Veri ambarlarında geçerli değildir.|Hiçbiri|
|memory_usage_percent|Bellek yüzdesi|Yüzde|Maksimum|Bellek yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Hiçbiri|
|full_backup_size_bytes|Tam yedekleme depolama boyutu|Bayt|Maksimum|Toplu tam yedekleme depolama boyutu. Sanal çekirdek tabanlı veritabanları için geçerlidir. Hiper ölçekli veritabanları için geçerli değildir.|Hiçbiri|
|diff_backup_size_bytes|Değişiklik yedekleme depolama boyutu|Bayt|Maksimum|Toplu fark yedekleme depolama boyutu. Sanal çekirdek tabanlı veritabanları için geçerlidir. Hiper ölçekli veritabanları için geçerli değildir.|Hiçbiri|
|log_backup_size_bytes|Günlük yedekleme depolama boyutu|Bayt|Maksimum|Toplu günlük yedekleme depolama boyutu. Sanal çekirdek tabanlı veritabanları için geçerlidir. Hiper ölçekli veritabanları için geçerli değildir.|Hiçbiri|


## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Hiçbiri|
|database_cpu_percent|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|DatabaseResourceId|
|physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|Hiçbiri|
|database_physical_data_read_percent|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|DatabaseResourceId|
|log_write_percent|Günlük GÇ yüzdesi|Yüzde|Ortalama|Günlük GÇ yüzdesi|Hiçbiri|
|database_log_write_percent|Günlük GÇ yüzdesi|Yüzde|Ortalama|Günlük GÇ yüzdesi|DatabaseResourceId|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi. DTU tabanlı elastik havuzlar için geçerlidir.|Hiçbiri|
|database_dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|DatabaseResourceId|
|storage_percent|Kullanılan veri alanı yüzdesi|Yüzde|Ortalama|Kullanılan veri alanı yüzdesi|Hiçbiri|
|workers_percent|Çalışan yüzdesi|Yüzde|Ortalama|Çalışan yüzdesi|Hiçbiri|
|database_workers_percent|Çalışan yüzdesi|Yüzde|Ortalama|Çalışan yüzdesi|DatabaseResourceId|
|sessions_percent|Oturum yüzdesi|Yüzde|Ortalama|Oturum yüzdesi|Hiçbiri|
|database_sessions_percent|Oturum yüzdesi|Yüzde|Ortalama|Oturum yüzdesi|DatabaseResourceId|
|eDTU_limit|eDTU sınırı|Sayı|Ortalama|eDTU sınırı. DTU tabanlı elastik havuzlar için geçerlidir.|Hiçbiri|
|storage_limit|Veri boyutu üst sınırı|Bayt|Ortalama|Veri boyutu üst sınırı|Hiçbiri|
|eDTU_used|eDTU kullanıldı|Sayı|Ortalama|eDTU kullanıldı. DTU tabanlı elastik havuzlar için geçerlidir.|Hiçbiri|
|database_eDTU_used|eDTU kullanıldı|Sayı|Ortalama|eDTU kullanıldı|DatabaseResourceId|
|storage_used|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|Hiçbiri|
|database_storage_used|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|DatabaseResourceId|
|xtp_storage_percent|Bellek içi OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek içi OLTP depolama yüzdesi|Hiçbiri|
|cpu_limit|CPU sınırı|Sayı|Ortalama|CPU sınırı. Sanal çekirdek tabanlı elastik havuzlar için geçerlidir.|Hiçbiri|
|database_cpu_limit|CPU sınırı|Sayı|Ortalama|CPU sınırı|DatabaseResourceId|
|cpu_used|Kullanılan CPU|Sayı|Ortalama|Kullanılan CPU. Sanal çekirdek tabanlı elastik havuzlar için geçerlidir.|Hiçbiri|
|database_cpu_used|Kullanılan CPU|Sayı|Ortalama|Kullanılan CPU|DatabaseResourceId|
|sqlserver_process_core_percent|SQL Server işlem çekirdeği yüzdesi|Yüzde|Maksimum|SQL DB işleminin yüzdesi olarak CPU kullanımı. Elastik havuzlar için geçerlidir.|Hiçbiri|
|sqlserver_process_memory_percent|SQL Server işlem belleği yüzdesi|Yüzde|Maksimum|SQL DB işleminin yüzdesi olarak bellek kullanımı. Elastik havuzlar için geçerlidir.|Hiçbiri|
|tempdb_data_size|Tempdb veri dosyası boyutu kilobayt|Sayı|Maksimum|Tempdb veri dosyası boyutu kilobayt|Hiçbiri|
|tempdb_log_size|Tempdb günlük dosyası boyutu kilobayt|Sayı|Maksimum|Tempdb günlük dosyası boyutu kilobayt|Hiçbiri|
|tempdb_log_used_percent|Kullanılan tempdb günlüğü yüzdesi|Yüzde|Maksimum|Kullanılan tempdb günlüğü yüzdesi|Hiçbiri|
|allocated_data_storage|Ayrılan veri alanı|Bayt|Ortalama|Ayrılan veri alanı|Hiçbiri|
|database_allocated_data_storage|Ayrılan veri alanı|Bayt|Ortalama|Ayrılan veri alanı|DatabaseResourceId|
|allocated_data_storage_percent|Ayrılan veri alanı yüzdesi|Yüzde|Maksimum|Ayrılan veri alanı yüzdesi|Hiçbiri|

## <a name="microsoftsqlservers"></a>Microsoft. SQL/sunucuları

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|ElasticPoolResourceId|
|database_dtu_consumption_percent|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|DatabaseResourceId, Elaun Poolresourceıd|
|storage_used|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|ElasticPoolResourceId|
|database_storage_used|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|DatabaseResourceId, Elaun Poolresourceıd|
|dtu_used|Kullanılan DTU|Sayı|Ortalama|Kullanılan DTU|DatabaseResourceId|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft. SQL/ManagedInstances

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|virtual_core_count|Sanal çekirdek sayısı|Sayı|Ortalama|Sanal çekirdek sayısı|Hiçbiri|
|avg_cpu_percent|Ortalama CPU yüzdesi|Yüzde|Ortalama|Ortalama CPU yüzdesi|Hiçbiri|
|reserved_storage_mb|Ayrılan depolama alanı|Sayı|Ortalama|Ayrılan depolama alanı|Hiçbiri|
|storage_space_used_mb|Kullanılan depolama alanı|Sayı|Ortalama|Kullanılan depolama alanı|Hiçbiri|
|io_requests|GÇ istek sayısı|Sayı|Ortalama|GÇ istek sayısı|Hiçbiri|
|io_bytes_read|Okunan GÇ baytları|Bayt|Ortalama|Okunan GÇ baytları|Hiçbiri|
|io_bytes_written|Yazılan GÇ baytları|Bayt|Ortalama|Yazılan GÇ baytları|Hiçbiri|



## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|UsedCapacity|Kullanılan kapasite|Bayt|Ortalama|Hesabın kullanılan kapasitesi|Hiçbiri|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BlobCapacity|Blob Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Blob hizmeti tarafından kullanılan depolama miktarı.|BlobType, katman|
|BLOB sayısı|Blob Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki Blob sayısı.|BlobType, katman|
|ContainerCount|Blob Kapsayıcı Sayısı|Sayı|Ortalama|Depolama hesabının Blob hizmetindeki kapsayıcı sayısı.|Hiçbiri|
|Dizin kapasitesi|Dizin kapasitesi|Bayt|Ortalama|Bayt cinsinden ADLS 2. (hiyerarşik) dizin tarafından kullanılan depolama miktarı.|Hiçbiri|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|TableCapacity|Tablo Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Tablo hizmeti tarafından kullanılan depolama miktarı.|Hiçbiri|
|TableCount|Tablo Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo sayısı.|Hiçbiri|
|TableEntityCount|Tablo Varlık Sayısı|Sayı|Ortalama|Depolama hesabının Tablo hizmetindeki tablo varlıklarının sayısı.|Hiçbiri|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft. Storage/storageAccounts/fileServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Dosya kapasitesi|Dosya kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının dosya hizmeti tarafından kullanılan depolama miktarı.|FileShare|
|FileCount|Dosya sayısı|Sayı|Ortalama|Depolama hesabının dosya hizmetindeki dosya sayısı.|FileShare|
|Dosya ShareCount|Dosya paylaşma sayısı|Sayı|Ortalama|Depolama hesabının dosya hizmetindeki dosya paylaşımlarının sayısı.|Hiçbiri|
|FileShareSnapshotCount|Dosya paylaşımının anlık görüntü sayısı|Sayı|Ortalama|Depolama hesabının dosyalar hizmetindeki paylaşımda bulunan anlık görüntü sayısı.|FileShare|
|FileShareSnapshotSize|Dosya paylaşımının anlık görüntü boyutu|Bayt|Ortalama|Bayt olarak depolama hesabının dosya hizmetindeki anlık görüntüler tarafından kullanılan depolama miktarı.|FileShare|
|Dosya Sharequota|Dosya paylaşımının kota boyutu|Bayt|Ortalama|Azure dosyaları hizmeti tarafından bayt olarak kullanılabilecek depolama miktarının üst sınırı.|FileShare|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, Authentication, FileShare|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|QueueCapacity|Kuyruk Kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının Kuyruk hizmeti tarafından kullanılan depolama miktarı.|Hiçbiri|
|QueueCount|Kuyruk Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra sayısı.|Hiçbiri|
|QueueMessageCount|Kuyruk İleti Sayısı|Sayı|Ortalama|Depolama hesabının Kuyruk hizmetindeki sıra iletilerinin yaklaşık sayısı.|Hiçbiri|
|İşlemler|İşlemler|Sayı|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı tür yanıtların sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|Giriş|Giriş|Bayt|Toplam|Bayt olarak giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessServerLatency|Başarı Sunucu Gecikme Süresi|Milisaniye|Ortalama|Milisaniye cinsinden başarılı bir isteği işlemek için Azure Depolama tarafından kullanılan ortalama gecikme süresi. Bu değer, AverageE2ELatency öğesinde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarı E2e|Başarı E2E Gecikme Süresi|Milisaniye|Ortalama|Bir depolama hizmetine veya belirtilen API işlemi, milisaniye cinsinden gönderilen başarılı isteklerin ortalama uçtan uca gecikme. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Erişilebilirlik|Erişilebilirlik|Yüzde|Ortalama|Depolama hizmetinin veya belirtilen API işleminin kullanılabilirlik yüzdesi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|





## <a name="microsoftstoragecachecaches"></a>Microsoft. StorageCache/önbellekler

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Clienentiops|Toplam Istemci ıOPS 'si|Sayı|Ortalama|Önbellek tarafından işlenen istemci dosyası işlemlerinin oranı.|Hiçbiri|
|ClientLatency|Ortalama Istemci gecikmesi|Milisaniye|Ortalama|Depolama önbelleğine yönelik istemci dosyası işlemlerinin ortalama gecikme süresi.|Hiçbiri|
|ClientReadIOPS|İstemci okuma ıOPS 'si|Sayaçpersaniye|Ortalama|Saniye başına istemci okuma işlemi.|Hiçbiri|
|Clientreadüretilen Iş|Ortalama önbellek okuma performansı|BytesPerSecond|Ortalama|İstemci okuma veri aktarımı hızı.|Hiçbiri|
|ClientWriteIOPS|İstemci yazma ıOPS 'si|Sayaçpersaniye|Ortalama|Saniye başına istemci yazma işlemi.|Hiçbiri|
|ClientWriteThroughput|Ortalama önbellek yazma performansı|BytesPerSecond|Ortalama|İstemci yazma verisi aktarım hızı.|Hiçbiri|
|ClientMetadataReadIOPS|İstemci meta verileri okuma ıOPS 'si|Sayaçpersaniye|Ortalama|Kalıcı durumu değiştirmeyen veri okuma işlemleri hariç, önbelleğe gönderilen istemci dosyası işlemlerinin oranı.|Hiçbiri|
|Clientmetadatawriteıops|İstemci meta verileri yazma ıOPS 'si|Sayaçpersaniye|Ortalama|Kalıcı durumu değiştiren veri yazmaları hariç önbelleğe gönderilen istemci dosyası işlemleri hızıdır.|Hiçbiri|
|Clientlockıops|İstemci kilidi ıOP|Sayaçpersaniye|Ortalama|Saniye başına istemci dosya kilitleme işlemleri.|Hiçbiri|
|Storagetargethegizli|Depolama hedefi durumu|Sayı|Ortalama|Önbellek ve depolama hedefleri arasındaki bağlantı testinin Boole sonuçları.|Hiçbiri|
|Çalışma Süresi|Çalışma Süresi|Sayı|Ortalama|Önbellek ve izleme sistemi arasındaki bağlantı testinin Boole sonuçları.|Hiçbiri|
|Storagetargetıops|Toplam StorageTarget ıOPS|Sayı|Ortalama|Önbelleğin belirli bir StorageTarget 'e gönderdiği tüm dosya işlemlerinin oranı.|StorageTarget|
|Storagetargetwriteıops|StorageTarget yazma ıOPS|Sayı|Ortalama|Önbellek belirli bir StorageTarget 'e gönderdiği dosya yazma işlemlerinin oranı.|StorageTarget|
|Storagetargetasyncwriteüretilen Iş|StorageTarget zaman uyumsuz yazma üretilen Işi|BytesPerSecond|Ortalama|Önbelleğin verileri belirli bir StorageTarget 'e zaman uyumsuz olarak yazma oranı. Bunlar, istemcilerin engellenmesine neden olmayan fırsatçı yazmalar.|StorageTarget|
|Storagetargetsyncwriteüretilen Iş|StorageTarget zaman uyumlu yazma performansı|BytesPerSecond|Ortalama|Önbelleğin verileri belirli bir StorageTarget 'e zaman uyumlu olarak yazma hızı. Bunlar, istemcilerin engellenmesine neden olan yazlardır.|StorageTarget|
|Storagetargettotalwriteverimini|StorageTarget Toplam yazma performansı|BytesPerSecond|Ortalama|Önbelleğin verileri belirli bir StorageTarget 'e yazdığı toplam oran.|StorageTarget|
|StorageTargetLatency|StorageTarget gecikmesi|Milisaniye|Ortalama|Önbelleğin bir ayrıştırısal StorageTarget 'e gönderdiği tüm dosya işlemlerinin ortalama gidiş dönüş gecikmesi.|StorageTarget|
|Storagetargetmetadatareadıops|StorageTarget meta verileri okuma ıOPS 'si|Sayaçpersaniye|Ortalama|Kalıcı durumu değiştirmeyen ve okuma işleminin dışında, önbelleğin belirli bir StorageTarget 'e gönderdiği dosya işlemlerinin oranı.|StorageTarget|
|Storagetargetmetadatawriteıops|StorageTarget meta verileri yazma ıOPS|Sayaçpersaniye|Ortalama|Kalıcı durumu değiştirme ve yazma işlemini hariç tutma, önbelleğin belirli bir StorageTarget 'e gönderdiği dosya işlemleri oranı.|StorageTarget|
|Storagetargetreadıops|Storagetaral okuma ıOPS|Sayaçpersaniye|Ortalama|Önbelleğin belirli bir StorageTarget 'e gönderdiği dosya okuma işlemlerinin oranı.|StorageTarget|
|Storagetargetreadaheadüretilen Iş|StorageTarget Ileriye yönelik okuma performansı|BytesPerSecond|Ortalama|Opportunisticly, StorageTarget 'ten verileri okuduğu önbelleğin hızını derecelendirin.|StorageTarget|
|Storagetargetfillüretilen Iş|StorageTarget dolgusu Işleme|BytesPerSecond|Ortalama|Önbelleğin, bir önbellek isabetsizliği işlemek için StorageTarget 'ten verileri okuduğu hız.|StorageTarget|
|Storagetargettotalreadüretilen Iş|StorageTarget toplam okuma performansı|BytesPerSecond|Ortalama|Önbelleğin belirli bir StorageTarget 'ten verileri okuduğu toplam oran.|StorageTarget|

## <a name="microsoftstoragesyncstoragesyncservices"></a>Microsoft. storagessync/storageSyncServices

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Eşitleme oturumu sonucu|Sayı|Ortalama|Sunucu uç noktası, bulut uç noktası ile eşitleme oturumunu başarıyla tamamladığında 1 değerini günlüğe kaydeden ölçüm|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Eşitlenen dosyalar|Sayı|Toplam|Eşitlenen dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Dosyalar eşitlenmiyor|Sayı|Toplam|Eşitleme başarısız olan dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Eşitlenen baytlar|Bayt|Toplam|Eşitleme oturumları için aktarılan toplam dosya boyutu|SyncGroupName, ServerEndpointName, SyncDirection|
|Storagesyncserversinyal|Sunucu çevrimiçi durumu|Sayı|Maksimum|Yeniden sigvbulunan sunucu, bulut uç noktasıyla bir sinyal başarıyla kaydetilişinde 1 değerini günlüğe kaydeden ölçüm|ServerName|
|Storagesyncyeniden hesaplama Liototalsizebytes|Bulut katmanlama geri çağırma|Bayt|Toplam|Sunucu tarafından geri çekilen toplam veri boyutu|ServerName|
|Storagesyncyeniden hesaplama Ledtotalnetworkbytes|Bulut katmanlama geri çağırma boyutu|Bayt|Toplam|Geri çekilen verilerin boyutu|SyncGroupName, ServerName|
|StorageSyncRecallThroughputBytesPerSecond|Bulut katmanlama geri çekme performansı|BytesPerSecond|Ortalama|Veri geri çağırma işleme boyutu|SyncGroupName, ServerName|
|StorageSyncRecalledNetworkBytesByApplication|Uygulamaya göre bulut katmanlama geri çağırma boyutu|Bayt|Toplam|Uygulama tarafından geri çekilen verilerin boyutu|SyncGroupName, ServerName, ApplicationName|

## <a name="microsoftstoragesyncstoragesyncservicessyncgroups"></a>Microsoft. storagessync/storageSyncServices/syncGroups

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|SyncGroupSyncSessionAppliedFilesCount|Eşitlenen dosyalar|Sayı|Toplam|Eşitlenen dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|SyncGroupSyncSessionPerItemErrorsCount|Dosyalar eşitlenmiyor|Sayı|Toplam|Eşitleme başarısız olan dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|SyncGroupBatchTransferredFileBytes|Eşitlenen baytlar|Bayt|Toplam|Eşitleme oturumları için aktarılan toplam dosya boyutu|SyncGroupName, ServerEndpointName, SyncDirection|

## <a name="microsoftstoragesyncstoragesyncservicessyncgroupsserverendpoints"></a>Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|ServerEndpointSyncSessionAppliedFilesCount|Eşitlenen dosyalar|Sayı|Toplam|Eşitlenen dosya sayısı|ServerEndpointName, SyncDirection|
|ServerEndpointSyncSessionPerItemErrorsCount|Dosyalar eşitlenmiyor|Sayı|Toplam|Eşitleme başarısız olan dosya sayısı|ServerEndpointName, SyncDirection|
|ServerEndpointBatchTransferredFileBytes|Eşitlenen baytlar|Bayt|Toplam|Eşitleme oturumları için aktarılan toplam dosya boyutu|ServerEndpointName, SyncDirection|

## <a name="microsoftstoragesyncstoragesyncservicesregisteredservers"></a>Microsoft. storagessync/storageSyncServices/registeredServers

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Sunucusinyal|Sunucu çevrimiçi durumu|Sayı|Maksimum|Yeniden sigvbulunan sunucu, bulut uç noktasıyla bir sinyal başarıyla kaydetilişinde 1 değerini günlüğe kaydeden ölçüm|Sunucuresourceıd, ServerName|
|Sunucu yeniden hesaplama Liototalsizebytes|Bulut katmanlama geri çağırma|Bayt|Toplam|Sunucu tarafından geri çekilen toplam veri boyutu|Sunucuresourceıd, ServerName|



## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft. StreamAnalytics/streamingjobs

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Resourcekullanım|SU Kullanım Yüzdesi|Yüzde|Maksimum|SU Kullanım Yüzdesi|LogicalName, PartitionID|
|Inputevents|Giriş Olayları|Sayı|Toplam|Giriş Olayları|LogicalName, PartitionID|
|Inputeventbytes|Giriş Olayı Bayt Sayısı|Bayt|Toplam|Giriş Olayı Bayt Sayısı|LogicalName, PartitionID|
|LateInputEvents|Geç Giriş Olayları|Sayı|Toplam|Geç Giriş Olayları|LogicalName, PartitionID|
|OutputEvents|Çıkış Olayları|Sayı|Toplam|Çıkış Olayları|LogicalName, PartitionID|
|Dönüştürme hataları|Veri Dönüştürme Hataları|Sayı|Toplam|Veri Dönüştürme Hataları|LogicalName, PartitionID|
|Hatalar|Çalışma Zamanı Hataları|Sayı|Toplam|Çalışma Zamanı Hataları|LogicalName, PartitionID|
|Droppedorayarlantedevents|Sıra dışı olaylar|Sayı|Toplam|Sıra dışı olaylar|LogicalName, PartitionID|
|AMLCalloutRequests|İşlev İstekleri|Sayı|Toplam|İşlev İstekleri|LogicalName, PartitionID|
|AMLCalloutFailedRequests|Başarısız İşlev İstekleri|Sayı|Toplam|Başarısız İşlev İstekleri|LogicalName, PartitionID|
|AMLCalloutInputEvents|İşlev Olayları|Sayı|Toplam|İşlev Olayları|LogicalName, PartitionID|
|DeserializationError|Giriş Serileştirme Kaldırma Hataları|Sayı|Toplam|Giriş Serileştirme Kaldırma Hataları|LogicalName, PartitionID|
|EarlyInputEvents|Erken Giriş Olayları|Sayı|Toplam|Erken Giriş Olayları|LogicalName, PartitionID|
|Outputsulu Markdelayseconds|Eşik Gecikmesi|Saniye|Maksimum|Eşik Gecikmesi|LogicalName, PartitionID|
|Inputeventssourcesbackgünlüğe kaydediliyor|Biriktirme Listesindeki Giriş Olayları|Sayı|Maksimum|Biriktirme Listesindeki Giriş Olayları|LogicalName, PartitionID|
|Inputeventssourcespersecond|Alınan Giriş Kaynakları|Sayı|Toplam|Alınan Giriş Kaynakları|LogicalName, PartitionID|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft. Timeseriesınsights/ortamlar

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Inressreceived Iletileri|Giriş alınan Iletiler|Sayı|Toplam|Tüm Olay Hub 'ından veya IoT Hub olay kaynaklarından okunan ileti sayısı|Hiçbiri|
|Inressreceiveınvalidmessages|Giriş geçersiz Iletiler aldı|Sayı|Toplam|Tüm Olay Hub 'ından veya IoT Hub olay kaynaklarından okunan geçersiz ileti sayısı|Hiçbiri|
|Alınan baytlar|Alınan bayt sayısı|Bayt|Toplam|Tüm olay kaynaklarından okunan bayt sayısı|Hiçbiri|
|IngressStoredBytes|Giriş depolanan baytlar|Bayt|Toplam|Başarılı bir şekilde işlenen ve sorgu için kullanılabilir olayların toplam boyutu|Hiçbiri|
|Inressstoo Vents|Giriş saklı olayları|Sayı|Toplam|Sorgu için başarıyla işlenen ve kullanılabilir düzleştirilmiş olay sayısı|Hiçbiri|
|Inressreceived Iletizamangecikmesi|Alınan Ileti zaman gecikmesi|Saniye|Maksimum|İleti olay kaynağında sıraya alındığı zaman ve giriş sırasında işlendiği zaman arasındaki fark|Hiçbiri|
|Inressreceived Iletisayaçgecikmesi|Alınan Ileti sayısı gecikmesi|Sayı|Ortalama|Olay kaynak bölümünde en son sıraya alınmış iletinin sıra numarası ve giriş bölümünde işlenmekte olan iletilerin sıra numarası arasındaki fark|Hiçbiri|
|WarmStorageMaxProperties|Isınma depolama en yüksek özellikleri|Sayı|Maksimum|S1/S2 SKU 'SU için ortam tarafından izin verilen en fazla özellik sayısı ve PAYG SKU 'SU için ısınma tarafından izin verilen en fazla özellik sayısı|Hiçbiri|
|WarmStorageUsedProperties|Sıcak depolama kullanılan özellikler |Sayı|Maksimum|S1/S2 SKU 'SU için ortam tarafından kullanılan özellik sayısı ve PAYG SKU 'SU için sıcak mağaza tarafından kullanılan özellik sayısı|Hiçbiri|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft. Timeseriesınsights/ortamlar/EventSources

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Inressreceived Iletileri|Giriş alınan Iletiler|Sayı|Toplam|Olay kaynağından okunan ileti sayısı|Hiçbiri|
|Inressreceiveınvalidmessages|Giriş geçersiz Iletiler aldı|Sayı|Toplam|Olay kaynağından okunan geçersiz ileti sayısı|Hiçbiri|
|Alınan baytlar|Alınan bayt sayısı|Bayt|Toplam|Olay kaynağından okunan bayt sayısı|Hiçbiri|
|IngressStoredBytes|Giriş depolanan baytlar|Bayt|Toplam|Başarılı bir şekilde işlenen ve sorgu için kullanılabilir olayların toplam boyutu|Hiçbiri|
|Inressstoo Vents|Giriş saklı olayları|Sayı|Toplam|Sorgu için başarıyla işlenen ve kullanılabilir düzleştirilmiş olay sayısı|Hiçbiri|
|Inressreceived Iletizamangecikmesi|Alınan Ileti zaman gecikmesi|Saniye|Maksimum|İleti olay kaynağında sıraya alındığı zaman ve giriş sırasında işlendiği zaman arasındaki fark|Hiçbiri|
|Inressreceived Iletisayaçgecikmesi|Alınan Ileti sayısı gecikmesi|Sayı|Ortalama|Olay kaynak bölümünde en son sıraya alınmış iletinin sıra numarası ve giriş bölümünde işlenmekte olan iletilerin sıra numarası arasındaki fark|Hiçbiri|
|WarmStorageMaxProperties|Isınma depolama en yüksek özellikleri|Sayı|Maksimum|S1/S2 SKU 'SU için ortam tarafından izin verilen en fazla özellik sayısı ve PAYG SKU 'SU için ısınma tarafından izin verilen en fazla özellik sayısı|Hiçbiri|
|WarmStorageUsedProperties|Sıcak depolama kullanılan özellikler |Sayı|Maksimum|S1/S2 SKU 'SU için ortam tarafından kullanılan özellik sayısı ve PAYG SKU 'SU için sıcak mağaza tarafından kullanılan özellik sayısı|Hiçbiri|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft. Vmwarechoparlör basit/virtualMachines

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk okuma bayt/sn|BytesPerSecond|Ortalama|Örnek süre içinde okuma işlemleri nedeniyle ortalama disk aktarım hızı.|Hiçbiri|
|DiskWriteBytesPerSecond|Disk yazma bayt/sn|BytesPerSecond|Ortalama|Örnek süre içinde yazma işlemleri nedeniyle ortalama disk aktarım hızı.|Hiçbiri|
|Disk okuma bayt sayısı|Disk okuma bayt sayısı|Bayt|Toplam|Örnek süre boyunca okuma işlemleri nedeniyle toplam disk aktarım hızı.|Hiçbiri|
|Disk yazma baytları|Disk yazma baytları|Bayt|Toplam|Örnek süre içinde yazma işlemleri nedeniyle toplam disk aktarım hızı.|Hiçbiri|
|DiskReadOperations|Disk okuma Işlemleri|Sayı|Toplam|Önceki örnek dönemdeki GÇ okuma işlemlerinin sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Hiçbiri|
|DiskWriteOperations|Disk yazma Işlemleri|Sayı|Toplam|Önceki örnek dönemdeki GÇ Yazma işlemlerinin sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Hiçbiri|
|Disk okuma Işlemi/sn|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Önceki örnek dönemdeki ortalama GÇ okuma işlemi sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Hiçbiri|
|Disk yazma Işlemi/sn|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Önceki örnek dönemdeki ortalama GÇ yazma işlemi sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Hiçbiri|
|DiskReadLatency|Disk okuma gecikmesi|Milisaniye|Ortalama|Toplam okuma gecikmesi. Cihaz ve çekirdek okuma gecikme sürelerinin toplamı.|Hiçbiri|
|DiskWriteLatency|Disk yazma gecikmesi|Milisaniye|Ortalama|Toplam yazma gecikmesi. Cihaz ve çekirdek yazma gecikme sürelerinin toplamı.|Hiçbiri|
|Networkınbytespersecond|Bayt/sn cinsinden ağ|BytesPerSecond|Ortalama|Alınan trafik için Ortalama ağ performansı.|Hiçbiri|
|NetworkOutBytesPerSecond|Ağ çıkış bayt/sn|BytesPerSecond|Ortalama|Aktarılan trafik için Ortalama ağ aktarım hızı.|Hiçbiri|
|Ağ Girişi|Ağ Girişi|Bayt|Toplam|Alınan trafik için toplam ağ aktarım hızı.|Hiçbiri|
|Ağ Çıkışı|Ağ Çıkışı|Bayt|Toplam|Aktarılan trafik için toplam ağ aktarım hızı.|Hiçbiri|
|MemoryUsed|Kullanılan bellek|Bayt|Ortalama|VM tarafından kullanılan makine belleği miktarı.|Hiçbiri|
|Memoryverildi|Verilen bellek|Bayt|Ortalama|VM 'ye konak tarafından verilen bellek miktarı. Bir kez dokunulmadan ve VMkernel belleğe ihtiyaç duyuncaya kadar bellek verilebilene kadar bu konağa bellek verilmez.|Hiçbiri|
|MemoryActive|Bellek etkin|Bayt|Ortalama|Son küçük zaman penceresinde VM tarafından kullanılan bellek miktarı. Bu, VM 'nin şu anda ihtiyacı olan bellek miktarını "doğru" sayısıdır. Ek olarak kullanılmayan bellek, konuğun performansına hiçbir etkilenmeden dışarı veya ballooned olabilir.|Hiçbiri|
|CPU yüzdesi|CPU yüzdesi|Yüzde|Ortalama|CPU kullanımı. Bu değer, sistemdeki tüm işlemci çekirdeğini temsil eden %100 ile raporlanır. Örnek olarak, dört çekirdekli bir sistemin %50 ' ünü kullanan 2 yönlü bir VM tamamen iki çekirdek kullanmaktır.|Hiçbiri|
|Yüztagecpuready|CPU için hazırlanma yüzdesi|Milisaniye|Toplam|Hazır süre, CPU (ler) in geçmiş güncelleştirme aralığında kullanılabilir olmasını bekleyen zamanın harcadığı süredir.|Hiçbiri|










## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|Cpuyüzdesi|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Örnek|
|DiskQueueLength|Disk kuyruğu uzunluğu|Sayı|Ortalama|Disk kuyruğu uzunluğu|Örnek|
|HttpQueueLength|Http kuyruğu uzunluğu|Sayı|Ortalama|Http kuyruğu uzunluğu|Örnek|
|BytesReceived|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|TcpSynSent|TCP SYN gönderilen|Sayı|Ortalama|TCP SYN gönderilen|Örnek|
|Tcpsynalındı|TCP SYN alındı|Sayı|Ortalama|TCP SYN alındı|Örnek|
|Tcpkurdu|TCP oluşturuldu|Sayı|Ortalama|TCP oluşturuldu|Örnek|
|TcpFinWait1|TCP Fin bekleme 1|Sayı|Ortalama|TCP Fin bekleme 1|Örnek|
|TcpFinWait2|TCP Fin bekleme 2|Sayı|Ortalama|TCP Fin bekleme 2|Örnek|
|TcpClosing|TCP kapatma|Sayı|Ortalama|TCP kapatma|Örnek|
|TcpCloseWait|TCP kapatma bekleme|Sayı|Ortalama|TCP kapatma bekleme|Örnek|
|TcpLastAck|TCP son ACK|Sayı|Ortalama|TCP son ACK|Örnek|
|TcpTimeWait|TCP zaman bekleme|Sayı|Ortalama|TCP zaman bekleme|Örnek|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft. Web/Sites (işlevler hariç)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuTime|CPU süresi|Saniye|Toplam|CPU süresi|Örnek|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|HTTP 404|Sayı|Toplam|HTTP 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http sunucu hataları|Sayı|Toplam|Http sunucu hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Ortalama yanıt süresi|Saniye|Ortalama|Ortalama yanıt süresi|Örnek|
|AppConnections|Bağlantılar|Sayı|Ortalama|Bağlantılar|Örnek|
|İşlendiğini|Tanıtıcı sayısı|Sayı|Ortalama|Tanıtıcı sayısı|Örnek|
|İş Parçacıkları|İş parçacığı sayısı|Sayı|Ortalama|İş parçacığı sayısı|Örnek|
|PrivateBytes|Özel baytlar|Bayt|Ortalama|Özel baytlar|Örnek|
|Ioreadbytespersecond|GÇ okuma bayt/saniye|BytesPerSecond|Toplam|GÇ okuma bayt/saniye|Örnek|
|Iowritebytespersecond|GÇ yazma bayt/saniye|BytesPerSecond|Toplam|GÇ yazma bayt/saniye|Örnek|
|Iootherbytespersecond|GÇ diğer bayt/saniye|BytesPerSecond|Toplam|GÇ diğer bayt/saniye|Örnek|
|IoReadOperationsPerSecond|GÇ okuma Işlemi/saniye|BytesPerSecond|Toplam|GÇ okuma Işlemi/saniye|Örnek|
|Iowriteoperationspersecond|GÇ Yazma Işlemi/saniye|BytesPerSecond|Toplam|GÇ Yazma Işlemi/saniye|Örnek|
|Iootheroperationspersecond|GÇ diğer Işlem/saniye|BytesPerSecond|Toplam|GÇ diğer Işlem/saniye|Örnek|
|RequestsInApplicationQueue|Uygulama kuyruğundaki istekler|Sayı|Ortalama|Uygulama kuyruğundaki istekler|Örnek|
|CurrentAssemblies|Geçerli derlemeler|Sayı|Ortalama|Geçerli derlemeler|Örnek|
|TotalAppDomains|Toplam uygulama etki alanları|Sayı|Ortalama|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Toplam yüklenmeyen uygulama etki alanları|Sayı|Ortalama|Toplam yüklenmeyen uygulama etki alanları|Örnek|
|Gen0Collections|Gen 0 çöp koleksiyonları|Sayı|Toplam|Gen 0 çöp koleksiyonları|Örnek|
|Gen1Collections|Gen 1 çöp koleksiyonları|Sayı|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Gen 2 çöp koleksiyonları|Sayı|Toplam|Gen 2 çöp koleksiyonları|Örnek|
|HealthCheckStatus|Durum denetimi durumu|Sayı|Ortalama|Durum denetimi durumu|Örnek|
|Filesystemage|Dosya sistemi kullanımı|Bayt|Ortalama|Dosya sistemi kullanımı|Hiçbiri|

## <a name="microsoftwebsites-functions"></a>Microsoft. Web/Sites (işlevler)

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|BytesReceived|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|Http5xx|Http sunucu hataları|Sayı|Toplam|Http sunucu hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|FunctionExecutionUnits|İşlev yürütme birimleri|MB/milisaniye|Toplam|[İşlev yürütme birimleri](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Örnek|
|Işlev yürütme sayısı|İşlev yürütme sayısı|Sayı|Toplam|İşlev yürütme sayısı|Örnek|
|PrivateBytes|Özel baytlar|Bayt|Ortalama|Özel baytlar|Örnek|
|Ioreadbytespersecond|GÇ okuma bayt/saniye|BytesPerSecond|Toplam|GÇ okuma bayt/saniye|Örnek|
|Iowritebytespersecond|GÇ yazma bayt/saniye|BytesPerSecond|Toplam|GÇ yazma bayt/saniye|Örnek|
|Iootherbytespersecond|GÇ diğer bayt/saniye|BytesPerSecond|Toplam|GÇ diğer bayt/saniye|Örnek|
|IoReadOperationsPerSecond|GÇ okuma Işlemi/saniye|BytesPerSecond|Toplam|GÇ okuma Işlemi/saniye|Örnek|
|Iowriteoperationspersecond|GÇ Yazma Işlemi/saniye|BytesPerSecond|Toplam|GÇ Yazma Işlemi/saniye|Örnek|
|Iootheroperationspersecond|GÇ diğer Işlem/saniye|BytesPerSecond|Toplam|GÇ diğer Işlem/saniye|Örnek|
|RequestsInApplicationQueue|Uygulama kuyruğundaki istekler|Sayı|Ortalama|Uygulama kuyruğundaki istekler|Örnek|
|CurrentAssemblies|Geçerli derlemeler|Sayı|Ortalama|Geçerli derlemeler|Örnek|
|TotalAppDomains|Toplam uygulama etki alanları|Sayı|Ortalama|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Toplam yüklenmeyen uygulama etki alanları|Sayı|Ortalama|Toplam yüklenmeyen uygulama etki alanları|Örnek|
|Gen0Collections|Gen 0 çöp koleksiyonları|Sayı|Toplam|Gen 0 çöp koleksiyonları|Örnek|
|Gen1Collections|Gen 1 çöp koleksiyonları|Sayı|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Gen 2 çöp koleksiyonları|Sayı|Toplam|Gen 2 çöp koleksiyonları|Örnek|
|HealthCheckStatus|Durum denetimi durumu|Sayı|Ortalama|Durum denetimi durumu|Örnek|
|Filesystemage|Dosya sistemi kullanımı|Bayt|Ortalama|Dosya sistemi kullanımı|Hiçbiri|


## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|CpuTime|CPU süresi|Saniye|Toplam|CPU süresi|Örnek|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|HTTP 404|Sayı|Toplam|HTTP 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http sunucu hataları|Sayı|Toplam|Http sunucu hataları|Örnek|
|MemoryWorkingSet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|AverageMemoryWorkingSet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Ortalama yanıt süresi|Saniye|Ortalama|Ortalama yanıt süresi|Örnek|
|HttpResponseTime|Yanıt süresi|Saniye|Ortalama|Yanıt süresi|Örnek|
|FunctionExecutionUnits|İşlev yürütme birimleri|Sayı|Toplam|İşlev yürütme birimleri|Örnek|
|Işlev yürütme sayısı|İşlev yürütme sayısı|Sayı|Toplam|İşlev yürütme sayısı|Örnek|
|AppConnections|Bağlantılar|Sayı|Ortalama|Bağlantılar|Örnek|
|İşlendiğini|Tanıtıcı sayısı|Sayı|Ortalama|Tanıtıcı sayısı|Örnek|
|İş Parçacıkları|İş parçacığı sayısı|Sayı|Ortalama|İş parçacığı sayısı|Örnek|
|PrivateBytes|Özel baytlar|Bayt|Ortalama|Özel baytlar|Örnek|
|Ioreadbytespersecond|GÇ okuma bayt/saniye|BytesPerSecond|Toplam|GÇ okuma bayt/saniye|Örnek|
|Iowritebytespersecond|GÇ yazma bayt/saniye|BytesPerSecond|Toplam|GÇ yazma bayt/saniye|Örnek|
|Iootherbytespersecond|GÇ diğer bayt/saniye|BytesPerSecond|Toplam|GÇ diğer bayt/saniye|Örnek|
|IoReadOperationsPerSecond|GÇ okuma Işlemi/saniye|BytesPerSecond|Toplam|GÇ okuma Işlemi/saniye|Örnek|
|Iowriteoperationspersecond|GÇ Yazma Işlemi/saniye|BytesPerSecond|Toplam|GÇ Yazma Işlemi/saniye|Örnek|
|Iootheroperationspersecond|GÇ diğer Işlem/saniye|BytesPerSecond|Toplam|GÇ diğer Işlem/saniye|Örnek|
|RequestsInApplicationQueue|Uygulama kuyruğundaki istekler|Sayı|Ortalama|Uygulama kuyruğundaki istekler|Örnek|
|CurrentAssemblies|Geçerli derlemeler|Sayı|Ortalama|Geçerli derlemeler|Örnek|
|TotalAppDomains|Toplam uygulama etki alanları|Sayı|Ortalama|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Toplam yüklenmeyen uygulama etki alanları|Sayı|Ortalama|Toplam yüklenmeyen uygulama etki alanları|Örnek|
|Gen0Collections|Gen 0 çöp koleksiyonları|Sayı|Toplam|Gen 0 çöp koleksiyonları|Örnek|
|Gen1Collections|Gen 1 çöp koleksiyonları|Sayı|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Gen 2 çöp koleksiyonları|Sayı|Toplam|Gen 2 çöp koleksiyonları|Örnek|
|HealthCheckStatus|Durum denetimi durumu|Sayı|Ortalama|Durum denetimi durumu|Örnek|
|Filesystemage|Dosya sistemi kullanımı|Bayt|Ortalama|Dosya sistemi kullanımı|Hiçbiri|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft. Web/hostingEnvironments/multiRolePools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|İstekler|İstekler|Sayı|Toplam|İstekler|Örnek|
|BytesReceived|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|Http101|Http 101|Sayı|Toplam|Http 101|Örnek|
|Http2xx|Http 2xx|Sayı|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Sayı|Toplam|Http 3xx|Örnek|
|Http401|Http 401|Sayı|Toplam|Http 401|Örnek|
|Http403|Http 403|Sayı|Toplam|Http 403|Örnek|
|Http404|HTTP 404|Sayı|Toplam|HTTP 404|Örnek|
|Http406|Http 406|Sayı|Toplam|Http 406|Örnek|
|Http4xx|Http 4xx|Sayı|Toplam|Http 4xx|Örnek|
|Http5xx|Http sunucu hataları|Sayı|Toplam|Http sunucu hataları|Örnek|
|AverageResponseTime|Ortalama yanıt süresi|Saniye|Ortalama|Ortalama yanıt süresi|Örnek|
|Cpuyüzdesi|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Örnek|
|DiskQueueLength|Disk kuyruğu uzunluğu|Sayı|Ortalama|Disk kuyruğu uzunluğu|Örnek|
|HttpQueueLength|Http kuyruğu uzunluğu|Sayı|Ortalama|Http kuyruğu uzunluğu|Örnek|
|ActiveRequests|Etkin Istekler|Sayı|Toplam|Etkin Istekler|Örnek|
|Toplam ön uçlar|Toplam ön uçlar|Sayı|Ortalama|Toplam ön uçlar|Hiçbiri|
|Smallappserviceplanınstances|Küçük App Service çalışanları planlıyor|Sayı|Ortalama|Küçük App Service çalışanları planlıyor|Hiçbiri|
|Mediumappserviceplanınstances|Orta App Service çalışanları planlıyor|Sayı|Ortalama|Orta App Service çalışanları planlıyor|Hiçbiri|
|Largeappserviceplanınstances|Büyük App Service çalışanları planlıyor|Sayı|Ortalama|Büyük App Service çalışanları planlıyor|Hiçbiri|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft. Web/hostingEnvironments/workerPools

|Ölçüm|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|
|WorkersTotal|Toplam çalışan sayısı|Sayı|Ortalama|Toplam çalışan sayısı|Hiçbiri|
|WorkersAvailable|Kullanılabilir çalışanlar|Sayı|Ortalama|Kullanılabilir çalışanlar|Hiçbiri|
|WorkersUsed|Kullanılan çalışanlar|Sayı|Ortalama|Kullanılan çalışanlar|Hiçbiri|
|Cpuyüzdesi|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Örnek|
## <a name="next-steps"></a>Sonraki adımlar
* [Azure Izleyici 'de ölçümler hakkında bilgi edinin](data-platform.md)
* [Ölçümler üzerinde uyarı oluşturma](alerts-overview.md)
* [Ölçümleri depolama, Olay Hub 'ına veya Log Analytics Dışa aktarma](platform-logs-overview.md)
