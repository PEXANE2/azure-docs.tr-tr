---
title: Azure Izleyici kaynak türüne göre desteklenen ölçümler
description: Azure Izleyici ile her kaynak türü için kullanılabilen ölçümlerin listesi.
author: rboucher
services: azure-monitor
ms.topic: reference
ms.date: 07/16/2020
ms.author: robb
ms.subservice: metrics
ms.openlocfilehash: 59df49d320b23686a3d053335ea2b95e98125b28
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88135564"
---
# <a name="supported-metrics-with-azure-monitor"></a>Azure Izleyici ile desteklenen ölçümler

> [!NOTE]
> Bu liste, büyük ölçüde Azure Izleyici ölçümlerinden REST API otomatik olarak oluşturulur. Bu listede GitHub aracılığıyla yapılan herhangi bir değişiklik, uyarı olmadan üzerine yazılabilir. Kalıcı güncelleştirmeler yapma hakkında daha fazla bilgi için bu makalenin yazarına başvurun.

Azure Izleyici, ölçümlerle etkileşimde bulunmak için, portalda grafik oluşturma, REST API aracılığıyla erişme veya PowerShell veya CLı kullanarak sorgulama gibi çeşitli yollar sağlar. 

Bu makale, Azure Izleyicisinin birleştirilmiş ölçüm işlem hattı ile Şu anda kullanılabilir olan tüm platform (yani otomatik olarak toplanan) ölçümlerinin tümüyle bir listesidir. Listenin en son 6 Mart, 2020. Bu tarihten sonra değiştirilen veya eklenen ölçümler aşağıda görünmeyebilir. Program aracılığıyla ölçüm listesini sorgulamak ve erişmek için lütfen [2018-01-01 api sürümünü](/rest/api/monitor/metricdefinitions)kullanın. Bu listede bulunmayan diğer ölçümler portalda veya eski API 'Ler kullanılarak kullanılabilir olabilir.

Ölçümler, kaynak sağlayıcılarına ve kaynak türüne göre düzenlenir. Hizmetlerin ve bunlara ait kaynak sağlayıcılarının listesi için bkz. [Azure hizmetleri Için kaynak sağlayıcıları](../../azure-resource-manager/management/azure-services-resource-providers.md). 

## <a name="exporting-platform-metrics-to-other-locations"></a>Platform ölçümlerini diğer konumlara dışarı aktarma

Azure izleyici ardışık düzeninde bulunan Platform ölçümlerini iki şekilde başka konumlara dışarı aktarabilirsiniz.
1. Ölçümleri kullanın [REST API](/rest/api/monitor/metrics/list)
2. Platform ölçümlerini yönlendirmek için [tanılama ayarlarını](diagnostic-settings.md) kullanın 
    - Azure Storage
    - Azure Izleyici günlükleri (ve bu nedenle Log Analytics)
    - Onları Microsoft dışı sistemlere alma yöntemi olan olay hub 'ları 

Tanılama ayarlarını kullanmak ölçümleri yönlendirmenin en kolay yoludur, ancak bazı sınırlamalar vardır: 

- **Bazı dışarı aktarılabilir** olmayan-tüm ölçümler REST API kullanılarak dışarı aktarılabilir, ancak bazıları Azure izleyici arka ucunun içindeki yanlışlıklar nedeniyle Tanılama ayarları kullanılarak dışarı aktarılamaz. Aşağıdaki tablolarda bulunan *Tanılama ayarları aracılığıyla dışarı aktarılabilir* sütun, hangi ölçümlerin bu şekilde dışarı aktarılabileceği listelenmiştir.  

- **Çok boyutlu ölçümler** -çok boyutlu ölçümleri Tanılama ayarları aracılığıyla diğer konumlara gönderme Şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır. *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.

## <a name="guest-os-and-host-os-metrics"></a>Konuk işletim sistemi ve konak işletim sistemi ölçümleri

> [!WARNING]
> Azure sanal makineler, Service Fabric ve Cloud Services üzerinde çalışan konuk işletim sistemi (konuk OS) ölçümleri **burada listelenmez.** Konuk işletim sistemi ölçümleri, Konuk işletim sisteminin bir parçası olarak veya üzerinde çalışan bir veya daha fazla aracı aracılığıyla toplanmalıdır.  Konuk işletim sistemi ölçümleri, her ikisi de otomatik ölçekleme veya uyarı için sık kullanılan Konuk CPU yüzdesini veya bellek kullanımını izleyen performans sayaçlarını içerir. 
>
> **Konak işletim sistemi ölçümleri kullanılabilir ve aşağıda listelenmiştir.** Bunlar aynı değildir. Konak işletim sistemi ölçümleri, Konuk işletim sistemi oturumunuzu barındıran Hyper-V oturumu ile ilgilidir. 

> [!TIP]
> En iyi yöntem, Konuk işletim sistemi performans ölçümlerini platform ölçümlerinin depolandığı Azure Izleyici ölçüm veritabanına göndermek için [Azure tanılama uzantısını](diagnostics-extension-overview.md) kullanmak ve yapılandırmaktır. Uzantı, Konuk işletim sistemi ölçümlerini [özel ölçüm](metrics-custom-overview.md) API 'si aracılığıyla yönlendirir. Daha sonra, bir grafik oluşturabilir, uyarır ve başka bir deyişle platform ölçümleri gibi Konuk işletim sistemi ölçümlerini kullanabilirsiniz. Alternatif olarak veya Ayrıca, Azure Izleyici günlüklerine/Log Analytics Konuk işletim sistemi ölçümleri göndermek için Log Analytics aracısını kullanabilirsiniz. Ölçüm olmayan verilerle birlikte bu ölçümler üzerinde sorgulama yapabilirsiniz. 

Önemli ek bilgiler için bkz. [Izleme aracılarına genel bakış](agents-overview.md).    

## <a name="table-formatting"></a>Tablo biçimlendirme

> [!IMPORTANT] 
> Bu en son güncelleştirme yeni bir sütun ekler ve ölçümleri alfabetik olarak yeniden sıralanabilir. Ek bilgiler, tarayıcı pencerenizin genişliğine bağlı olarak, aşağıdaki tabloda alt kısımdaki bir yatay kaydırma çubuğuna sahip olabileceği anlamına gelir. Bilgilerin eksik olduğunu düşünüyorsanız, tablonun tamamını görmek için kaydırma çubuğunu kullanın.


## <a name="microsoftanalysisservicesservers"></a>Microsoft. AnalysisServices/sunucuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|CleanerCurrentPrice|Evet|Bellek: temizleyici geçerli fiyatı|Count|Ortalama|Geçerli bellek fiyatı, $/Byte/Time, 1000 olarak normalleştirilemez.|Sunucuresourcetype|
|Cleanermemorynonınkable|Evet|Bellek: temizleyici bellek daraltılamaz|Bayt|Ortalama|Arka plan temizleyici tarafından temizlenme konusu değil, bayt cinsinden bellek miktarı.|Sunucuresourcetype|
|Cleanermemoryshrınkable|Evet|Bellek: temizleyici bellek shrınılabilir|Bayt|Ortalama|Arka plan temizleyici tarafından temizlenme konusu bayt cinsinden bellek miktarı.|Sunucuresourcetype|
|Commandpoolbusyıthreads|Evet|İş parçacıkları: komut havuzu meşgul iş parçacıkları|Count|Ortalama|Komut iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|Sunucuresourcetype|
|Commandpokaydlithreads|Evet|İş parçacıkları: komut havuzu boşta iş parçacıkları|Count|Ortalama|Komut iş parçacığı havuzundaki boşta iş parçacığı sayısı.|Sunucuresourcetype|
|CommandPoolJobQueueLength|Evet|Komut havuzu Iş kuyruğu uzunluğu|Count|Ortalama|Komut iş parçacığı havuzu sırasındaki iş sayısı.|Sunucuresourcetype|
|CurrentConnections|Evet|Bağlantı: geçerli bağlantılar|Count|Ortalama|Kurulan istemci bağlantılarının geçerli sayısı.|Sunucuresourcetype|
|CurrentUserSessions|Evet|Geçerli Kullanıcı oturumları|Count|Ortalama|Geçerli kullanıcı oturumlarının sayısı.|Sunucuresourcetype|
|Longparsingbusyıthreads|Evet|İş parçacıkları: uzun ayrıştırma meşgul iş parçacıkları|Count|Ortalama|Uzun ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|Sunucuresourcetype|
|LongParsingIdleThreads|Evet|İş parçacıkları: uzun ayrıştırma boşta iş parçacıkları|Count|Ortalama|Uzun ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|Sunucuresourcetype|
|LongParsingJobQueueLength|Evet|İş parçacıkları: uzun ayrıştırma işi sıra uzunluğu|Count|Ortalama|Uzun ayrıştırma iş parçacığı havuzu kuyruğundaki iş sayısı.|Sunucuresourcetype|
|mashup_engine_memory_metric|Evet|D motoru belleği|Bayt|Ortalama|Karma altyapı işlemlerine göre bellek kullanımı|Sunucuresourcetype|
|mashup_engine_private_bytes_metric|Evet|D motoru özel baytlar|Bayt|Ortalama|Karma altyapı işlemlerine göre özel bayt kullanımı.|Sunucuresourcetype|
|mashup_engine_qpu_metric|Evet|A motoru QPU|Count|Ortalama|Karma altyapı işlemlerine göre QPU kullanımı|Sunucuresourcetype|
|mashup_engine_virtual_bytes_metric|Evet|D motoru sanal bayt sayısı|Bayt|Ortalama|Karma altyapı işlemlerine göre sanal bayt kullanımı.|Sunucuresourcetype|
|memory_metric|Evet|Bellek|Bayt|Ortalama|Bellek. S1 için 0-25 GB, S2 için 0-50 GB ve S4 için 0-100 GB|Sunucuresourcetype|
|memory_thrashing_metric|Evet|Bellek Temizleme|Yüzde|Ortalama|Ortalama bellek miktarı.|Sunucuresourcetype|
|MemoryLimitHard|Evet|Bellek: bellek sınırı sabit|Bayt|Ortalama|Yapılandırma dosyasından sabit bellek sınırı.|Sunucuresourcetype|
|Memoryhighlimit|Evet|Bellek: bellek sınırı yüksek|Bayt|Ortalama|Yapılandırma dosyasından yüksek bellek sınırı.|Sunucuresourcetype|
|MemoryLimitLow|Evet|Bellek: bellek sınırı düşük|Bayt|Ortalama|Yapılandırma dosyasından düşük bellek sınırı.|Sunucuresourcetype|
|MemoryLimitVertiPaq|Evet|Bellek: bellek sınırı VertiPaq|Bayt|Ortalama|Yapılandırma dosyasından bellek içi sınır.|Sunucuresourcetype|
|MemoryUsage|Evet|Bellek: bellek kullanımı|Bayt|Ortalama|Sunucu işleminin temizleyici bellek fiyatını hesaplamada kullanılan bellek kullanımı. XVelocity altyapısının bellek sınırının fazla olması halinde, xVelocity bellek içi analiz altyapısı (VertiPaq) tarafından eşlenen veya ayrılan belleği yoksayarak, işlem \ bayt ve bellek eşlemeli verilerin boyutuna eşit.|Sunucuresourcetype|
|private_bytes_metric|Evet|Özel baytlar|Bayt|Ortalama|Özel baytlar.|Sunucuresourcetype|
|Processingpoolbusyıiojobthreads|Evet|İş parçacıkları: Işleme havuzu meşgul g/ç işi iş parçacıkları|Count|Ortalama|İşleme iş parçacığı havuzunda g/ç işleri çalıştıran iş parçacığı sayısı.|Sunucuresourcetype|
|Processingpoolbusınonıothreads|Evet|İş parçacıkları: Işleme havuzu meşgul olmayan g/ç iş parçacıkları|Count|Ortalama|İşleme iş parçacığı havuzunda g/ç olmayan işler çalıştıran iş parçacığı sayısı.|Sunucuresourcetype|
|Processingpokaydliiojobthreads|Evet|İş parçacıkları: Işleme havuzu boşta g/ç işi iş parçacıkları|Count|Ortalama|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|Sunucuresourcetype|
|Processingpokaydlenoniothreads|Evet|İş parçacıkları: Işleme havuzu boşta g/ç olmayan iş parçacıkları|Count|Ortalama|İşleme iş parçacığı havuzunda g/ç dışı işlere adanmış boşta iş parçacığı sayısı.|Sunucuresourcetype|
|Processingpokayojobqueuelength|Evet|İş parçacıkları: Işleme havuzu g/ç iş kuyruğu uzunluğu|Count|Ortalama|İşleme iş parçacığı havuzu kuyruğundaki g/ç işlerinin sayısı.|Sunucuresourcetype|
|ProcessingPoolJobQueueLength|Evet|İşleme havuzu Iş kuyruğu uzunluğu|Count|Ortalama|İşleme iş parçacığı havuzu kuyruğundaki g/ç dışı iş sayısı.|Sunucuresourcetype|
|qpu_metric|Evet|QPU|Count|Ortalama|QPU. S1 için 0-100 aralığı, S2 için 0-200 ve S4 için 0-400|Sunucuresourcetype|
|Querypoolbusyıthreads|Evet|Sorgu havuzu meşgul Iş parçacıkları|Count|Ortalama|Sorgu iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|Sunucuresourcetype|
|Querypokaydlithreads|Evet|İş parçacıkları: sorgu havuzu boşta iş parçacıkları|Count|Ortalama|İşleme iş parçacığı havuzundaki g/ç işleri için boşta iş parçacığı sayısı.|Sunucuresourcetype|
|QueryPoolJobQueueLength|Evet|İş parçacıkları: sorgu havuzu iş kuyruğu uzunluğu|Count|Ortalama|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|Sunucuresourcetype|
|Kota|Evet|Bellek: kota|Bayt|Ortalama|Bayt cinsinden geçerli bellek kotası. Bellek kotası, bellek verme veya bellek ayırma olarak da bilinir.|Sunucuresourcetype|
|Quotabkilitli|Evet|Bellek: kota engellendi|Count|Ortalama|Diğer bellek kotaları boşaltılana kadar engellenen kota isteklerinin geçerli sayısı.|Sunucuresourcetype|
|RowsConvertedPerSec|Evet|İşleme: dönüştürülen satır sayısı/saniye|Sayaçpersaniye|Ortalama|İşlem sırasında dönüştürülen satır oranı.|Sunucuresourcetype|
|RowsReadPerSec|Evet|İşleme: okunan satır sayısı/saniye|Sayaçpersaniye|Ortalama|Tüm ilişkisel veritabanlarından okunan satır oranı.|Sunucuresourcetype|
|RowsWrittenPerSec|Evet|İşleme: yazılan satır sayısı/saniye|Sayaçpersaniye|Ortalama|İşlem sırasında yazılan satır oranı.|Sunucuresourcetype|
|Shortparsingbusyıthreads|Evet|İş parçacıkları: kısa ayrıştırma meşgul iş parçacıkları|Count|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki meşgul iş parçacığı sayısı.|Sunucuresourcetype|
|ShortParsingIdleThreads|Evet|İş parçacıkları: kısa ayrıştırma boşta iş parçacıkları|Count|Ortalama|Kısa ayrıştırma iş parçacığı havuzundaki boşta iş parçacığı sayısı.|Sunucuresourcetype|
|ShortParsingJobQueueLength|Evet|İş parçacıkları: kısa ayrıştırma iş kuyruğu uzunluğu|Count|Ortalama|Kısa ayrıştırma iş parçacığı havuzu sırasındaki iş sayısı.|Sunucuresourcetype|
|Başarılı bir bağlantı Spersec|Evet|Saniyedeki başarılı bağlantı sayısı|Sayaçpersaniye|Ortalama|Başarılı bağlantı tamamlama oranı.|Sunucuresourcetype|
|Totalconnectionarızaları|Evet|Toplam bağlantı başarısızlığı sayısı|Count|Ortalama|Toplam başarısız bağlantı denemesi.|Sunucuresourcetype|
|TotalConnectionRequests|Evet|Toplam bağlantı Isteği sayısı|Count|Ortalama|Toplam bağlantı isteği sayısı. Bunlar, varışlardır.|Sunucuresourcetype|
|Vertipaqdisk belleksiz|Evet|Bellek: VertiPaq disk belleksiz|Bayt|Ortalama|Bellek içi altyapı tarafından kullanılmak üzere çalışma kümesinde kilitlenen bellek baytları.|Sunucuresourcetype|
|Vertipaqdisk|Evet|Bellek: VertiPaq disk belleğine|Bayt|Ortalama|Bellek içi veriler için kullanılan disk belleğine alınan bellek miktarı.|Sunucuresourcetype|
|virtual_bytes_metric|Evet|Sanal Bayt Sayısı|Bayt|Ortalama|Sanal bayt sayısı.|Sunucuresourcetype|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|BackendDuration|Evet|Arka uç Isteklerinin süresi|Mayacak|Ortalama|Milisaniye cinsinden arka uç Isteklerinin süresi|Konum, ana bilgisayar adı|
|Kapasite|Evet|Kapasite|Yüzde|Ortalama|Apimanayönetimi hizmeti için kullanım ölçümü|Konum|
|Süre|Evet|Ağ Geçidi Isteklerinin genel süresi|Mayacak|Ortalama|Milisaniye olarak ağ geçidi Isteklerinin genel süresi|Konum, ana bilgisayar adı|
|EventHubDroppedEvents|Evet|Bırakılan EventHub olayları|Count|Toplam|Sıra boyutu sınırına ulaşıldığından atlanan olay sayısı|Konum|
|EventHubRejectedEvents|Evet|Reddedilen EventHub olayları|Count|Toplam|Reddedilen EventHub olaylarının sayısı (yanlış yapılandırma veya yetkisiz)|Konum|
|Eventhubbaşarılı Futavents|Evet|Başarılı EventHub olayları|Count|Toplam|Başarılı EventHub olaylarının sayısı|Konum|
|EventHubThrottledEvents|Evet|Kısıtlanmış EventHub olayları|Count|Toplam|Kısıtlanmış EventHub olay sayısı|Konum|
|EventHubTimedoutEvents|Evet|Zaman aşımına uğrayan EventHub olayları|Count|Toplam|Zaman aşımına uğrayan EventHub olaylarının sayısı|Konum|
|EventHubTotalBytesSent|Evet|EventHub olaylarının boyutu|Bayt|Toplam|Toplam EventHub olay boyutu (bayt)|Konum|
|EventHubTotalEvents|Evet|Toplam EventHub olayları|Count|Toplam|EventHub 'e gönderilen olay sayısı|Konum|
|EventHubTotalFailedEvents|Evet|Başarısız EventHub olayları|Count|Toplam|Başarısız EventHub olaylarının sayısı|Konum|
|FailedRequests|Evet|Başarısız ağ geçidi Istekleri (kullanım dışı)|Count|Toplam|Ağ Geçidi isteklerindeki başarısızlık sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|NetworkConnectivity|Evet|Kaynakların ağ bağlantısı durumu (Önizleme)|Count|Ortalama|API Management hizmetinden bağımlı kaynak türlerinin ağ bağlantısı durumu|Konum, ResourceType|
|Diğer Istekler|Evet|Diğer ağ geçidi Istekleri (kullanım dışı)|Count|Toplam|Diğer ağ geçidi isteklerinin sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|İstekler|Evet|İstekler|Count|Toplam|Birden çok boyut içeren ağ geçidi isteği ölçümleri|Konum, ana bilgisayar adı, LastErrorReason, BackendResponseCode, GatewayResponseCode, BackendResponseCodeCategory, GatewayResponseCodeCategory|
|SuccessfulRequests|Evet|Başarılı ağ geçidi Istekleri (kullanım dışı)|Count|Toplam|Başarılı ağ geçidi isteklerinin sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|TotalRequests|Evet|Toplam ağ geçidi Isteği sayısı (kullanım dışı)|Count|Toplam|Ağ Geçidi isteklerinin sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|
|UnauthorizedRequests|Evet|Yetkisiz ağ geçidi Istekleri (kullanım dışı)|Count|Toplam|Yetkisiz ağ geçidi isteklerinin sayısı-çok boyutlu istek ölçüsünü GatewayResponseCodeCategory boyutuyla kullanın|Konum, ana bilgisayar adı|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft. AppConfiguration/Configurationmağazaları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Httpıncomingrequestcount|Evet|Httpıncomingrequestcount|Count|Count|Gelen http isteklerinin toplam sayısı.|StatusCode, kimlik doğrulaması|
|Httpıncomingrequestduration|Evet|Httpıncomingrequestduration|Count|Ortalama|Http isteğinde gecikme süresi.|StatusCode, kimlik doğrulaması|
|Çalışan Httprequestcount|Evet|Çalışan Httprequestcount|Count|Count|Kısıtlanmış http istekleri.|Boyut yok|


## <a name="microsoftappplatformspring"></a>Microsoft. AppPlatform/yay

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|JVM. GC. Live. Data. size|Evet|JVM. GC. Live. Data. size|Bayt|Ortalama|Tam GC sonrasında eski nesil bellek havuzunun boyutu|Dağıtım, AppName, Pod|
|JVM. GC. Max. Data. size|Evet|JVM. GC. Max. Data. size|Bayt|Ortalama|Eski nesil bellek havuzunun en büyük boyutu|Dağıtım, AppName, Pod|
|JVM. GC. Memory. ayrılmış|Evet|JVM. GC. Memory. ayrılmış|Bayt|Maksimum|Bir GC 'nin bir sonraki öncesine ait olması durumunda küçük kuşak bellek havuzunun boyutunun artması için artırılır|Dağıtım, AppName, Pod|
|JVM. GC. Memory. yükseltilen|Evet|JVM. GC. Memory. yükseltilen|Bayt|Maksimum|GC 'den sonra GC 'den önce eski nesil bellek havuzunun boyutundaki pozitif artış sayısı|Dağıtım, AppName, Pod|
|JVM. GC. Pause. Total. Count|Evet|JVM. GC. Pause. Total. Count|Count|Toplam|GC duraklatma sayısı|Dağıtım, AppName, Pod|
|JVM. GC. Pause. Total. Time|Evet|JVM. GC. Pause. Total. Time|Mayacak|Toplam|GC duraklatma toplam süre|Dağıtım, AppName, Pod|
|JVM. Memory. taahhüt|Evet|JVM. Memory. taahhüt|Bayt|Ortalama|JVM 'ye bayt olarak atanan bellek|Dağıtım, AppName, Pod|
|JVM. Memory. Max|Evet|JVM. Memory. Max|Bayt|Maksimum|Bellek yönetimi için kullanılabilecek bayt cinsinden maksimum bellek miktarı|Dağıtım, AppName, Pod|
|JVM. Memory. kullanıldı|Evet|JVM. Memory. kullanıldı|Bayt|Ortalama|Bayt cinsinden kullanılan uygulama belleği|Dağıtım, AppName, Pod|
|Process. CPU. Usage|Evet|Process. CPU. Usage|Yüzde|Ortalama|Uygulama JVM CPU kullanım yüzdesi|Dağıtım, AppName, Pod|
|System. CPU. Usage|Evet|System. CPU. Usage|Yüzde|Ortalama|Tüm sistem için en son CPU kullanımı|Dağıtım, AppName, Pod|
|Tomcat. Global. Error|Evet|Tomcat. Global. Error|Count|Toplam|Tomcat genel hatası|Dağıtım, AppName, Pod|
|Tomcat. Global. alındı|Evet|Tomcat. Global. alındı|Bayt|Toplam|Tomcat alınan toplam bayt sayısı|Dağıtım, AppName, Pod|
|Tomcat. Global. Request. ort. saat|Evet|Tomcat. Global. Request. ort. saat|Mayacak|Ortalama|Tomcat Isteği ortalama zamanı|Dağıtım, AppName, Pod|
|Tomcat. Global. Request. Max|Evet|Tomcat. Global. Request. Max|Mayacak|Maksimum|Tomcat Isteği maksimum zamanı|Dağıtım, AppName, Pod|
|Tomcat. Global. Request. Total. Count|Evet|Tomcat. Global. Request. Total. Count|Count|Toplam|Tomcat Isteği toplam sayısı|Dağıtım, AppName, Pod|
|Tomcat. Global. Request. Total. Time|Evet|Tomcat. Global. Request. Total. Time|Mayacak|Toplam|Tomcat Isteği toplam süre|Dağıtım, AppName, Pod|
|Tomcat. Global. gönderildi|Evet|Tomcat. Global. gönderildi|Bayt|Toplam|Tomcat gönderilen toplam bayt sayısı|Dağıtım, AppName, Pod|
|Tomcat. Sessions. ACTIVE. Current|Evet|Tomcat. Sessions. ACTIVE. Current|Count|Toplam|Tomcat oturumu etkin sayısı|Dağıtım, AppName, Pod|
|Tomcat. Sessions. Active. Max|Evet|Tomcat. Sessions. Active. Max|Count|Toplam|Tomcat oturumu en fazla etkin sayısı|Dağıtım, AppName, Pod|
|Tomcat. Sessions. canlı. Max|Evet|Tomcat. Sessions. canlı. Max|Mayacak|Maksimum|Tomcat oturumu maksimum etkin süresi|Dağıtım, AppName, Pod|
|Tomcat. Sessions. oluşturuldu|Evet|Tomcat. Sessions. oluşturuldu|Count|Toplam|Tomcat oturum oluşturma sayısı|Dağıtım, AppName, Pod|
|Tomcat. Sessions. süre geçildi|Evet|Tomcat. Sessions. süre geçildi|Count|Toplam|Tomcat oturumunun süre dolma sayısı|Dağıtım, AppName, Pod|
|Tomcat. Sessions. reddedildi|Evet|Tomcat. Sessions. reddedildi|Count|Toplam|Tomcat oturumu reddedildi sayısı|Dağıtım, AppName, Pod|
|tomcat.threads.config. Max|Evet|tomcat.threads.config. Max|Count|Toplam|Tomcat yapılandırması en fazla Iş parçacığı sayısı|Dağıtım, AppName, Pod|
|Tomcat. Threads. Current|Evet|Tomcat. Threads. Current|Count|Toplam|Tomcat geçerli Iş parçacığı sayısı|Dağıtım, AppName, Pod|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft. Automation/automationAccounts

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|TotalJob|Evet|Toplam Iş sayısı|Count|Toplam|Toplam iş sayısı|Runbook, durum|
|Totalupdatedeploymentmachinerçalıştırır|Evet|Toplam güncelleştirme dağıtım makinesi çalıştırması|Count|Toplam|Yazılım güncelleştirme dağıtımı çalıştırmasında toplam yazılım güncelleştirme dağıtımı makinesi|SoftwareUpdateConfigurationName, Status, TargetComputer, Softwareupdateconfigurationrunıd|
|TotalUpdateDeploymentRuns|Evet|Toplam güncelleştirme dağıtımı çalıştırmaları|Count|Toplam|Toplam yazılım güncelleştirme dağıtımı çalıştırmaları|SoftwareUpdateConfigurationName, durum|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|CoreCount|Hayır|Adanmış çekirdek sayısı|Count|Toplam|Batch hesabındaki toplam adanmış çekirdek sayısı|Boyut yok|
|CreatingNodeCount|Hayır|Düğüm sayısı oluşturuluyor|Count|Toplam|Oluşturulmakta olan düğüm sayısı|Boyut yok|
|Idıdnodecount|Hayır|Boştaki düğüm sayısı|Count|Toplam|Boştaki düğüm sayısı|Boyut yok|
|JobDeleteCompleteEvent|Evet|İş silme Tamam olayları|Count|Toplam|Başarıyla silinen işlerin toplam sayısı.|No|
|JobDeleteStartEvent|Evet|İş silme başlangıç olayları|Count|Toplam|Silinmesi istenen toplam iş sayısı.|No|
|JobDisableCompleteEvent|Evet|İş tüm olayları devre dışı bırak|Count|Toplam|Başarıyla devre dışı bırakılmış toplam iş sayısı.|No|
|JobDisableStartEvent|Evet|İş devre dışı başlatma olayları|Count|Toplam|Devre dışı bırakılması istenen toplam iş sayısı.|No|
|JobStartEvent|Evet|İş başlatma olayları|Count|Toplam|Başarıyla başlatılmış toplam iş sayısı.|No|
|JobTerminateCompleteEvent|Evet|İş sonlandırma tamamlanma olayları|Count|Toplam|Başarıyla sonlandırılan toplam iş sayısı.|No|
|JobTerminateStartEvent|Evet|İş sonlandırma başlangıç olayları|Count|Toplam|Sonlandırılması istenen toplam iş sayısı.|No|
|LeavingPoolNodeCount|Hayır|Havuz düğüm sayısından çıkılıyor|Count|Toplam|Havuzdan ayrılma düğüm sayısı|Boyut yok|
|LowPriorityCoreCount|Hayır|LowPriority çekirdek sayısı|Count|Toplam|Batch hesabındaki toplam düşük öncelikli çekirdek sayısı|Boyut yok|
|OfflineNodeCount|Hayır|Çevrimdışı düğüm sayısı|Count|Toplam|Çevrimdışı düğüm sayısı|Boyut yok|
|PoolCreateEvent|Evet|Havuz oluşturma olayları|Count|Toplam|Oluşturulan Toplam Havuz sayısı|poolId|
|PoolDeleteCompleteEvent|Evet|Havuz silme Tamam olayları|Count|Toplam|Tamamlanan toplam havuz silme sayısı|poolId|
|PoolDeleteStartEvent|Evet|Havuz silme başlangıç olayları|Count|Toplam|Başlatılmış olan toplam havuz silme sayısı|poolId|
|PoolResizeCompleteEvent|Evet|Havuz yeniden boyutlandırma Tamam olayları|Count|Toplam|Tamamlanan toplam havuz yeniden boyutlandırmaları sayısı|poolId|
|PoolResizeStartEvent|Evet|Havuz yeniden boyutlandırma başlangıç olayları|Count|Toplam|Başlatılan toplam havuz yeniden boyutlandırmaları sayısı|poolId|
|PreemptedNodeCount|Hayır|Önden düğüm sayısı|Count|Toplam|Yok eden düğüm sayısı|Boyut yok|
|RebootingNodeCount|Hayır|Düğüm sayısı yeniden başlatılıyor|Count|Toplam|Yeniden başlatma düğümlerinin sayısı|Boyut yok|
|ReimagingNodeCount|Hayır|Yeniden Imaging düğüm sayısı|Count|Toplam|Yeniden görüntüleme düğümlerinin sayısı|Boyut yok|
|RunningNodeCount|Hayır|Çalışan düğüm sayısı|Count|Toplam|Çalışan düğümlerin sayısı|Boyut yok|
|StartingNodeCount|Hayır|Başlangıç düğüm sayısı|Count|Toplam|Başlayan düğüm sayısı|Boyut yok|
|StartTaskFailedNodeCount|Hayır|Başlangıç görevi başarısız düğüm sayısı|Count|Toplam|Başlangıç görevinin başarısız olduğu düğüm sayısı|Boyut yok|
|TaskCompleteEvent|Evet|Görev tamamlanma olayları|Count|Toplam|Tamamlanan toplam görev sayısı|poolId, iş kimliği|
|TaskFailEvent|Evet|Görev başarısız olayları|Count|Toplam|Hatalı durumda tamamlanan toplam görev sayısı|poolId, iş kimliği|
|TaskStartEvent|Evet|Görev başlatma olayları|Count|Toplam|Başlatılan toplam görev sayısı|poolId, iş kimliği|
|TotalLowPriorityNodeCount|Hayır|Düşük öncelikli düğüm sayısı|Count|Toplam|Batch hesabındaki toplam düşük öncelikli düğüm sayısı|Boyut yok|
|TotalNodeCount|Hayır|Adanmış düğüm sayısı|Count|Toplam|Batch hesabındaki toplam ayrılmış düğüm sayısı|Boyut yok|
|UnusableNodeCount|Hayır|Kullanılamayan düğüm sayısı|Count|Toplam|Kullanılamayan düğüm sayısı|Boyut yok|
|Waitingforstarttasnot Decount|Hayır|Başlangıç görevi düğüm sayısı bekleniyor|Count|Toplam|Başlangıç görevinin tamamlanması için bekleyen düğüm sayısı|Boyut yok|


## <a name="microsoftbatchaiworkspaces"></a>ChAI/çalışma alanlarını Microsoft.Bat

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Etkin çekirdekler|Evet|Etkin çekirdekler|Count|Ortalama|Etkin çekirdek sayısı|Senaryo, ClusterName|
|Etkin düğümler|Evet|Etkin düğümler|Count|Ortalama|Çalışan düğümlerin sayısı|Senaryo, ClusterName|
|Boştaki çekirdekler|Evet|Boştaki çekirdekler|Count|Ortalama|Boşta çekirdek sayısı|Senaryo, ClusterName|
|Boştaki düğümler|Evet|Boştaki düğümler|Count|Ortalama|Boştaki düğüm sayısı|Senaryo, ClusterName|
|İş tamamlandı|Evet|İş tamamlandı|Count|Toplam|Tamamlanan iş sayısı|Senaryo, ClusterName, ResultType|
|İş gönderildi|Evet|İş gönderildi|Count|Toplam|Gönderilen iş sayısı|Senaryo, ClusterName|
|Çekirdekleri bırakma|Evet|Çekirdekleri bırakma|Count|Ortalama|Bırakma çekirdekleri sayısı|Senaryo, ClusterName|
|Düğümlerden çıkılıyor|Evet|Düğümlerden çıkılıyor|Count|Ortalama|Bırakma düğümü sayısı|Senaryo, ClusterName|
|Önden çekirdekler|Evet|Önden çekirdekler|Count|Ortalama|Yok edilecek çekirdek sayısı|Senaryo, ClusterName|
|Önden düğümler|Evet|Önden düğümler|Count|Ortalama|Yok eden düğüm sayısı|Senaryo, ClusterName|
|Kota kullanım yüzdesi|Evet|Kota kullanım yüzdesi|Count|Ortalama|Kullanılan Kota yüzdesi|Senaryo, ClusterName, VmFamilyName, VmPriority|
|Toplam çekirdek sayısı|Evet|Toplam çekirdek sayısı|Count|Ortalama|Toplam çekirdek sayısı|Senaryo, ClusterName|
|Toplam düğüm sayısı|Evet|Toplam düğüm sayısı|Count|Ortalama|Toplam düğüm sayısı|Senaryo, ClusterName|
|Kullanılamayan çekirdekler|Evet|Kullanılamayan çekirdekler|Count|Ortalama|Kullanılamayan çekirdek sayısı|Senaryo, ClusterName|
|Kullanılamayan düğümler|Evet|Kullanılamayan düğümler|Count|Ortalama|Kullanılamayan düğüm sayısı|Senaryo, ClusterName|


## <a name="microsoftblockchainblockchainmembers"></a>Microsoft. Blockzincirine/blockchainMembers

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Yayınsaysedcount|Evet|Yayın Işlenen sayısı|Count|Ortalama|İşlenen işlem sayısı|Düğüm, kanal, tür, durum|
|Connectionkabul edildi|Evet|Kabul edilen bağlantılar|Count|Toplam|Kabul edilen bağlantılar|Node|
|ConnectionActive|Evet|Etkin Bağlantılar|Count|Ortalama|Etkin Bağlantılar|Node|
|Connectionişlenmiş|Evet|İşlenmiş bağlantılar|Count|Toplam|İşlenmiş bağlantılar|Node|
|ConsensusEtcdraftCommittedBlockNumber|Evet|Consensus Etcdraft kaydedilmiş blok numarası|Count|Ortalama|Yürütülen en son bloğun blok numarası|Düğüm, kanal|
|CpuUsagePercentageInDouble|Evet|CPU kullanım yüzdesi|Yüzde|Maksimum|CPU kullanım yüzdesi|Node|
|Onaylama başarısız|Evet|Doğrulayıcı onay başarısızlığı|Count|Ortalama|Başarısız olan onaylama sayısı.|Düğüm, kanal, chaincode, chaincodeerror|
|Gossıpleaderelectionöncü|Evet|GOSSIP öncü seçim lideri|Count|Toplam|Eş, öncü (1) veya İzleyici (0)|Düğüm, kanal|
|Gossıpmembershiptotalpeersbilinen|Evet|GOSSIP üyeliği bilinen toplam eş sayısı|Count|Ortalama|Bilinen toplam eş|Düğüm, kanal|
|Gossıpstateheight|Evet|GOSSIP durumu yüksekliği|Count|Ortalama|Geçerli defter yüksekliği|Düğüm, kanal|
|Ioreadbytes|Evet|GÇ okuma baytları|Bayt|Toplam|GÇ okuma baytları|Node|
|IOWriteBytes|Evet|GÇ Yazma baytları|Bayt|Toplam|GÇ Yazma baytları|Node|
|LedgerTransactionCount|Evet|Muhasebe Işlem sayısı|Count|Ortalama|İşlenen işlem sayısı|Düğüm, kanal, transaction_type, chaincode, validation_code|
|MemoryLimit|Evet|Bellek sınırı|Bayt|Ortalama|Bellek sınırı|Node|
|MemoryUsage|Evet|Bellek Kullanımı|Bayt|Ortalama|Bellek Kullanımı|Node|
|MemoryUsagePercentageInDouble|Evet|Bellek kullanım yüzdesi|Yüzde|Ortalama|Bellek kullanım yüzdesi|Node|
|PendingTransactions|Evet|Bekleyen Işlemler|Count|Ortalama|Bekleyen Işlemler|Node|
|Processedbkilitler|Evet|İşlenen bloklar|Count|Toplam|İşlenen bloklar|Node|
|ProcessedTransactions|Evet|İşlenen Işlemler|Count|Toplam|İşlenen Işlemler|Node|
|QueuedTransactions|Evet|Kuyruğa alınmış Işlemler|Count|Ortalama|Kuyruğa alınmış Işlemler|Node|
|RequestHandled|Evet|İşlenmiş Istekler|Count|Toplam|İşlenmiş Istekler|Node|
|StorageUsage|Evet|Depolama alanı kullanımı|Bayt|Ortalama|Depolama alanı kullanımı|Node|


## <a name="microsoftcacheredis"></a>Microsoft. Cache/redsıs

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|cachehits|Evet|İsabetli Önbellek Okuma Sayısı|Count|Toplam||Parça|
|cachehits0|Evet|Önbellek ısabetleri (parça 0)|Count|Toplam||Boyut yok|
|cachehits1|Evet|Önbellek ısabetleri (parça 1)|Count|Toplam||Boyut yok|
|cachehits2|Evet|Önbellek ısabetleri (parça 2)|Count|Toplam||Boyut yok|
|cachehits3|Evet|Önbellek ısabetleri (parça 3)|Count|Toplam||Boyut yok|
|cachehits4|Evet|Önbellek ısabetleri (parça 4)|Count|Toplam||Boyut yok|
|cachehits5|Evet|Önbellek ısabetleri (parça 5)|Count|Toplam||Boyut yok|
|cachehits6|Evet|Önbellek ısabetleri (parça 6)|Count|Toplam||Boyut yok|
|cachehits7|Evet|Önbellek ısabetleri (parça 7)|Count|Toplam||Boyut yok|
|cachehits8|Evet|Önbellek ısabetleri (parça 8)|Count|Toplam||Boyut yok|
|cachehits9|Evet|Önbellek ısabetleri (parça 9)|Count|Toplam||Boyut yok|
|cacheLatency|Evet|Önbellek gecikmesi mikrosaniye (Önizleme)|Count|Ortalama||Parça|
|cacheisabetsizlik|Evet|İsabetsiz Önbellek Okuma Sayısı|Count|Toplam||Parça|
|cachemisses0|Evet|Önbellekte bulunamayanlar (parça 0)|Count|Toplam||Boyut yok|
|cachemisses1|Evet|Önbellekte bulunamayanlar (parça 1)|Count|Toplam||Boyut yok|
|cachemisses2|Evet|Önbellekte bulunamayanlar (parça 2)|Count|Toplam||Boyut yok|
|cachemisses3|Evet|Önbellekte bulunamayanlar (parça 3)|Count|Toplam||Boyut yok|
|cachemisses4|Evet|Önbellekte bulunamayanlar (parça 4)|Count|Toplam||Boyut yok|
|cachemisses5|Evet|Önbellekte bulunamayanlar (parça 5)|Count|Toplam||Boyut yok|
|cachemisses6|Evet|Önbellekte bulunamayanlar (parça 6)|Count|Toplam||Boyut yok|
|cachemisses7|Evet|Önbellekte bulunamayanlar (parça 7)|Count|Toplam||Boyut yok|
|cachemisses8|Evet|Önbellekte bulunamayanlar (parça 8)|Count|Toplam||Boyut yok|
|cachemisses9|Evet|Önbellekte bulunamayanlar (parça 9)|Count|Toplam||Boyut yok|
|cachemissrate|Evet|Önbellek Isabetsizlik oranı|Yüzde|cachemissrate||Parça|
|cacheRead|Evet|Önbellek Okuması|BytesPerSecond|Maksimum||Parça|
|cacheRead0|Evet|Önbellek Okuma (parça 0)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead1|Evet|Önbellek Okuma (parça 1)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead2|Evet|Önbellek Okuma (parça 2)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead3|Evet|Önbellek Okuma (parça 3)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead4|Evet|Önbellek Okuma (parça 4)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead5|Evet|Önbellek Okuma (parça 5)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead6|Evet|Önbellek Okuma (parça 6)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead7|Evet|Önbellek Okuma (parça 7)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead8|Evet|Önbellek Okuma (parça 8)|BytesPerSecond|Maksimum||Boyut yok|
|cacheRead9|Evet|Önbellek Okuma (parça 9)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite|Evet|Önbellek Yazması|BytesPerSecond|Maksimum||Parça|
|cacheWrite0|Evet|Önbellek yazma (parça 0)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite1|Evet|Önbellek yazma (parça 1)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite2|Evet|Önbellek yazma (parça 2)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite3|Evet|Önbellek yazma (parça 3)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite4|Evet|Önbellek yazma (parça 4)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite5|Evet|Önbellek yazma (parça 5)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite6|Evet|Önbellek yazma (parça 6)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite7|Evet|Önbellek yazma (parça 7)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite8|Evet|Önbellek yazma (parça 8)|BytesPerSecond|Maksimum||Boyut yok|
|cacheWrite9|Evet|Önbellek yazma (parça 9)|BytesPerSecond|Maksimum||Boyut yok|
|connectedistemcileri|Evet|Bağlı İstemciler|Count|Maksimum||Parça|
|connectedclients0|Evet|Bağlı Istemciler (parça 0)|Count|Maksimum||Boyut yok|
|connectedclients1|Evet|Bağlı Istemciler (parça 1)|Count|Maksimum||Boyut yok|
|connectedclients2|Evet|Bağlı Istemciler (parça 2)|Count|Maksimum||Boyut yok|
|connectedclients3|Evet|Bağlı Istemciler (parça 3)|Count|Maksimum||Boyut yok|
|connectedclients4|Evet|Bağlı Istemciler (parça 4)|Count|Maksimum||Boyut yok|
|connectedclients5|Evet|Bağlı Istemciler (parça 5)|Count|Maksimum||Boyut yok|
|connectedclients6|Evet|Bağlı Istemciler (parça 6)|Count|Maksimum||Boyut yok|
|connectedclients7|Evet|Bağlı Istemciler (parça 7)|Count|Maksimum||Boyut yok|
|connectedclients8|Evet|Bağlı Istemciler (parça 8)|Count|Maksimum||Boyut yok|
|connectedclients9|Evet|Bağlı Istemciler (parça 9)|Count|Maksimum||Boyut yok|
|hatalar|Evet|Hatalar|Count|Maksimum||Shardıd, ErrorType|
|çıkarılan anahtarlar|Evet|Çıkarılan Anahtarlar|Count|Toplam||Parça|
|evictedkeys0|Evet|Çıkarılan anahtarlar (parça 0)|Count|Toplam||Boyut yok|
|evictedkeys1|Evet|Çıkarılan anahtarlar (parça 1)|Count|Toplam||Boyut yok|
|evictedkeys2|Evet|Çıkarılan anahtarlar (parça 2)|Count|Toplam||Boyut yok|
|evictedkeys3|Evet|Çıkarılan anahtarlar (parça 3)|Count|Toplam||Boyut yok|
|evictedkeys4|Evet|Çıkarılan anahtarlar (parça 4)|Count|Toplam||Boyut yok|
|evictedkeys5|Evet|Çıkarılan anahtarlar (parça 5)|Count|Toplam||Boyut yok|
|evictedkeys6|Evet|Çıkarılan anahtarlar (parça 6)|Count|Toplam||Boyut yok|
|evictedkeys7|Evet|Çıkarılan anahtarlar (parça 7)|Count|Toplam||Boyut yok|
|evictedkeys8|Evet|Çıkarılan anahtarlar (parça 8)|Count|Toplam||Boyut yok|
|evictedkeys9|Evet|Çıkarılan anahtarlar (parça 9)|Count|Toplam||Boyut yok|
|expiredkeys|Evet|Süresi Dolan Anahtarlar|Count|Toplam||Parça|
|expiredkeys0|Evet|Süre dolma anahtarları (parça 0)|Count|Toplam||Boyut yok|
|expiredkeys1|Evet|Süre dolma anahtarları (parça 1)|Count|Toplam||Boyut yok|
|expiredkeys2|Evet|Süre dolma anahtarları (parça 2)|Count|Toplam||Boyut yok|
|expiredkeys3|Evet|Süre dolma anahtarları (parça 3)|Count|Toplam||Boyut yok|
|expiredkeys4|Evet|Süre dolma anahtarları (parça 4)|Count|Toplam||Boyut yok|
|expiredkeys5|Evet|Süre dolma anahtarları (parça 5)|Count|Toplam||Boyut yok|
|expiredkeys6|Evet|Süre dolma anahtarları (parça 6)|Count|Toplam||Boyut yok|
|expiredkeys7|Evet|Süre dolma anahtarları (parça 7)|Count|Toplam||Boyut yok|
|expiredkeys8|Evet|Süre dolma anahtarları (parça 8)|Count|Toplam||Boyut yok|
|expiredkeys9|Evet|Süre dolma anahtarları (parça 9)|Count|Toplam||Boyut yok|
|GetCommands|Evet|Alınanlar|Count|Toplam||Parça|
|getcommands0|Evet|Alır (parça 0)|Count|Toplam||Boyut yok|
|getcommands1|Evet|Alır (parça 1)|Count|Toplam||Boyut yok|
|getcommands2|Evet|Alır (parça 2)|Count|Toplam||Boyut yok|
|getcommands3|Evet|Alır (parça 3)|Count|Toplam||Boyut yok|
|getcommands4|Evet|Alır (parça 4)|Count|Toplam||Boyut yok|
|getcommands5|Evet|Alır (parça 5)|Count|Toplam||Boyut yok|
|getcommands6|Evet|Alır (parça 6)|Count|Toplam||Boyut yok|
|getcommands7|Evet|Alır (parça 7)|Count|Toplam||Boyut yok|
|getcommands8|Evet|Al (Shard 8)|Count|Toplam||Boyut yok|
|getcommands9|Evet|Alır (parça 9)|Count|Toplam||Boyut yok|
|operationsPerSecond|Evet|Saniye Başına İşlem|Count|Maksimum||Parça|
|operationsPerSecond0|Evet|Saniyedeki işlem (parça 0)|Count|Maksimum||Boyut yok|
|operationsPerSecond1|Evet|Saniyedeki işlem (parça 1)|Count|Maksimum||Boyut yok|
|operationsPerSecond2|Evet|Saniyedeki işlem (parça 2)|Count|Maksimum||Boyut yok|
|operationsPerSecond3|Evet|Saniyedeki işlem (parça 3)|Count|Maksimum||Boyut yok|
|operationsPerSecond4|Evet|Saniyedeki işlem (parça 4)|Count|Maksimum||Boyut yok|
|operationsPerSecond5|Evet|Saniyedeki işlem (parça 5)|Count|Maksimum||Boyut yok|
|operationsPerSecond6|Evet|Saniyedeki işlem (parça 6)|Count|Maksimum||Boyut yok|
|operationsPerSecond7|Evet|Saniyedeki işlem (parça 7)|Count|Maksimum||Boyut yok|
|operationsPerSecond8|Evet|Saniyedeki işlem (parça 8)|Count|Maksimum||Boyut yok|
|operationsPerSecond9|Evet|Saniyedeki işlem (parça 9)|Count|Maksimum||Boyut yok|
|percentProcessorTime|Evet|CPU|Yüzde|Maksimum||Parça|
|percentProcessorTime0|Evet|CPU (parça 0)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime1|Evet|CPU (parça 1)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime2|Evet|CPU (parça 2)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime3|Evet|CPU (parça 3)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime4|Evet|CPU (parça 4)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime5|Evet|CPU (parça 5)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime6|Evet|CPU (parça 6)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime7|Evet|CPU (parça 7)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime8|Evet|CPU (parça 8)|Yüzde|Maksimum||Boyut yok|
|percentProcessorTime9|Evet|CPU (parça 9)|Yüzde|Maksimum||Boyut yok|
|Sunucuyükü|Evet|Sunucu Yükü|Yüzde|Maksimum||Parça|
|serverLoad0|Evet|Sunucu yükü (parça 0)|Yüzde|Maksimum||Boyut yok|
|serverLoad1|Evet|Sunucu yükü (parça 1)|Yüzde|Maksimum||Boyut yok|
|serverLoad2|Evet|Sunucu yükü (parça 2)|Yüzde|Maksimum||Boyut yok|
|serverLoad3|Evet|Sunucu yükü (parça 3)|Yüzde|Maksimum||Boyut yok|
|serverLoad4|Evet|Sunucu yükü (parça 4)|Yüzde|Maksimum||Boyut yok|
|serverLoad5|Evet|Sunucu yükü (parça 5)|Yüzde|Maksimum||Boyut yok|
|serverLoad6|Evet|Sunucu yükü (parça 6)|Yüzde|Maksimum||Boyut yok|
|serverLoad7|Evet|Sunucu yükü (parça 7)|Yüzde|Maksimum||Boyut yok|
|serverLoad8|Evet|Sunucu yükü (parça 8)|Yüzde|Maksimum||Boyut yok|
|serverLoad9|Evet|Sunucu yükü (parça 9)|Yüzde|Maksimum||Boyut yok|
|SetCommands|Evet|Kümeler|Count|Toplam||Parça|
|setcommands0|Evet|Kümeler (parça 0)|Count|Toplam||Boyut yok|
|setcommands1|Evet|Kümeler (parça 1)|Count|Toplam||Boyut yok|
|setcommands2|Evet|Kümeler (parça 2)|Count|Toplam||Boyut yok|
|setcommands3|Evet|Kümeler (parça 3)|Count|Toplam||Boyut yok|
|setcommands4|Evet|Kümeler (parça 4)|Count|Toplam||Boyut yok|
|setcommands5|Evet|Kümeler (parça 5)|Count|Toplam||Boyut yok|
|setcommands6|Evet|Kümeler (parça 6)|Count|Toplam||Boyut yok|
|setcommands7|Evet|Kümeler (parça 7)|Count|Toplam||Boyut yok|
|setcommands8|Evet|Kümeler (Shard 8)|Count|Toplam||Boyut yok|
|setcommands9|Evet|Kümeler (parça 9)|Count|Toplam||Boyut yok|
|totalcommandsişlendi|Evet|Toplam İşlem Sayısı|Count|Toplam||Parça|
|totalcommandsprocessed0|Evet|Toplam Işlem (parça 0)|Count|Toplam||Boyut yok|
|totalcommandsprocessed1|Evet|Toplam Işlem (parça 1)|Count|Toplam||Boyut yok|
|totalcommandsprocessed2|Evet|Toplam Işlem (parça 2)|Count|Toplam||Boyut yok|
|totalcommandsprocessed3|Evet|Toplam Işlem (parça 3)|Count|Toplam||Boyut yok|
|totalcommandsprocessed4|Evet|Toplam Işlem (parça 4)|Count|Toplam||Boyut yok|
|totalcommandsprocessed5|Evet|Toplam Işlem (parça 5)|Count|Toplam||Boyut yok|
|totalcommandsprocessed6|Evet|Toplam Işlem (parça 6)|Count|Toplam||Boyut yok|
|totalcommandsprocessed7|Evet|Toplam Işlem (parça 7)|Count|Toplam||Boyut yok|
|totalcommandsprocessed8|Evet|Toplam Işlem (parça 8)|Count|Toplam||Boyut yok|
|totalcommandsprocessed9|Evet|Toplam Işlem (parça 9)|Count|Toplam||Boyut yok|
|totalkeys|Evet|Toplam anahtar sayısı|Count|Maksimum||Parça|
|totalkeys0|Evet|Toplam anahtar (parça 0)|Count|Maksimum||Boyut yok|
|totalkeys1|Evet|Toplam anahtar (parça 1)|Count|Maksimum||Boyut yok|
|totalkeys2|Evet|Toplam anahtar (parça 2)|Count|Maksimum||Boyut yok|
|totalkeys3|Evet|Toplam anahtar (parça 3)|Count|Maksimum||Boyut yok|
|totalkeys4|Evet|Toplam anahtar (parça 4)|Count|Maksimum||Boyut yok|
|totalkeys5|Evet|Toplam anahtar (parça 5)|Count|Maksimum||Boyut yok|
|totalkeys6|Evet|Toplam anahtar (parça 6)|Count|Maksimum||Boyut yok|
|totalkeys7|Evet|Toplam anahtar (parça 7)|Count|Maksimum||Boyut yok|
|totalkeys8|Evet|Toplam anahtar (Shard 8)|Count|Maksimum||Boyut yok|
|totalkeys9|Evet|Toplam anahtar (parça 9)|Count|Maksimum||Boyut yok|
|usedmemory|Evet|Kullanılan Bellek|Bayt|Maksimum||Parça|
|usedmemory0|Evet|Kullanılan bellek (parça 0)|Bayt|Maksimum||Boyut yok|
|usedmemory1|Evet|Kullanılan bellek (parça 1)|Bayt|Maksimum||Boyut yok|
|usedmemory2|Evet|Kullanılan bellek (parça 2)|Bayt|Maksimum||Boyut yok|
|usedmemory3|Evet|Kullanılan bellek (parça 3)|Bayt|Maksimum||Boyut yok|
|usedmemory4|Evet|Kullanılan bellek (parça 4)|Bayt|Maksimum||Boyut yok|
|usedmemory5|Evet|Kullanılan bellek (parça 5)|Bayt|Maksimum||Boyut yok|
|usedmemory6|Evet|Kullanılan bellek (parça 6)|Bayt|Maksimum||Boyut yok|
|usedmemory7|Evet|Kullanılan bellek (parça 7)|Bayt|Maksimum||Boyut yok|
|usedmemory8|Evet|Kullanılan bellek (parça 8)|Bayt|Maksimum||Boyut yok|
|usedmemory9|Evet|Kullanılan bellek (parça 9)|Bayt|Maksimum||Boyut yok|
|usedmemorypercentage|Evet|Kullanılan Bellek Yüzdesi|Yüzde|Maksimum||Parça|
|usedmemoryRss|Evet|Kullanılan bellek RSS|Bayt|Maksimum||Parça|
|usedmemoryRss0|Evet|Kullanılan bellek RSS (parça 0)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss1|Evet|Kullanılan bellek RSS (parça 1)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss2|Evet|Kullanılan bellek RSS (parça 2)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss3|Evet|Kullanılan bellek RSS (parça 3)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss4|Evet|Kullanılan bellek RSS (parça 4)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss5|Evet|Kullanılan bellek RSS (parça 5)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss6|Evet|Kullanılan bellek RSS (parça 6)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss7|Evet|Kullanılan bellek RSS (parça 7)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss8|Evet|Kullanılan bellek RSS (parça 8)|Bayt|Maksimum||Boyut yok|
|usedmemoryRss9|Evet|Kullanılan bellek RSS (parça 9)|Bayt|Maksimum||Boyut yok|


## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft. CDN/cdnwebapplicationfirewallpolicies

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|WebApplicationFirewallRequestCount|Evet|Web uygulaması güvenlik duvarı Istek sayısı|Count|Toplam|Web uygulaması güvenlik duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName, RuleName, eylem|


## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft. ClassicCompute/domainNames/yuvalar/roller

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Disk okuma bayt/sn|Hayır|Disk okuma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diskten okunan ortalama bayt.|Roleınstanceıd|
|Disk okuma Işlemi/sn|Evet|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si.|Roleınstanceıd|
|Disk yazma bayt/sn|Hayır|Disk yazma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diske yazılan ortalama bayt.|Roleınstanceıd|
|Disk yazma Işlemi/sn|Evet|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si.|Roleınstanceıd|
|Ağ Girişi|Evet|Ağ Girişi|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik).|Roleınstanceıd|
|Ağ Çıkışı|Evet|Ağ Çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik).|Roleınstanceıd|
|CPU yüzdesi|Evet|CPU yüzdesi|Yüzde|Ortalama|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi.|Roleınstanceıd|


## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft. ClassicCompute/virtualMachines

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Disk okuma bayt/sn|Hayır|Disk okuma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diskten okunan ortalama bayt.|Boyut yok|
|Disk okuma Işlemi/sn|Evet|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si.|Boyut yok|
|Disk yazma bayt/sn|Hayır|Disk yazma|BytesPerSecond|Ortalama|İzleme dönemi boyunca diske yazılan ortalama bayt.|Boyut yok|
|Disk yazma Işlemi/sn|Evet|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si.|Boyut yok|
|Ağ Girişi|Evet|Ağ Girişi|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik).|Boyut yok|
|Ağ Çıkışı|Evet|Ağ Çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik).|Boyut yok|
|CPU yüzdesi|Evet|CPU yüzdesi|Yüzde|Ortalama|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi.|Boyut yok|


## <a name="microsoftclassicstoragestorageaccounts"></a>Microsoft. ClassicStorage/storageAccounts

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|UsedCapacity|Hayır|Kullanılan kapasite|Bayt|Ortalama|Hesabın kullanılan kapasitesi|Boyut yok|


## <a name="microsoftclassicstoragestorageaccountsblobservices"></a>Microsoft. ClassicStorage/storageAccounts/blobServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|
|BlobCapacity|Hayır|Blob kapasitesi|Bayt|Ortalama|Depolama hesabının blob hizmeti tarafından bayt olarak kullanılan depolama miktarı.|BlobType, katman|
|BlobCount|Hayır|BLOB sayısı|Count|Ortalama|Depolama hesabının blob hizmetindeki BLOB sayısı.|BlobType, katman|
|ContainerCount|Evet|Blob kapsayıcı sayısı|Count|Ortalama|Depolama hesabının blob hizmetindeki kapsayıcı sayısı.|Boyut yok|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Dizin kapasitesi|Hayır|Dizin kapasitesi|Bayt|Ortalama|Bayt cinsinden ADLS 2. (hiyerarşik) dizin tarafından kullanılan depolama miktarı.|Boyut yok|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|


## <a name="microsoftclassicstoragestorageaccountsfileservices"></a>Microsoft. ClassicStorage/storageAccounts/fileServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Dosya kapasitesi|Hayır|Dosya kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının dosya hizmeti tarafından kullanılan depolama miktarı.|Dosya Paylaşımı|
|FileCount|Hayır|Dosya sayısı|Count|Ortalama|Depolama hesabının dosya hizmetindeki dosya sayısı.|Dosya Paylaşımı|
|Dosya ShareCount|Hayır|Dosya paylaşma sayısı|Count|Ortalama|Depolama hesabının dosya hizmetindeki dosya paylaşımlarının sayısı.|Boyut yok|
|Dosya Sharequota|Hayır|Dosya paylaşımının kota boyutu|Bayt|Ortalama|Azure dosyaları hizmeti tarafından bayt olarak kullanılabilecek depolama miktarının üst sınırı.|Dosya Paylaşımı|
|FileShareSnapshotCount|Hayır|Dosya paylaşımının anlık görüntü sayısı|Count|Ortalama|Depolama hesabının dosyalar hizmetindeki paylaşımda bulunan anlık görüntü sayısı.|Dosya Paylaşımı|
|FileShareSnapshotSize|Hayır|Dosya paylaşımının anlık görüntü boyutu|Bayt|Ortalama|Bayt olarak depolama hesabının dosya hizmetindeki anlık görüntüler tarafından kullanılan depolama miktarı.|Dosya Paylaşımı|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, Authentication, FileShare|


## <a name="microsoftclassicstoragestorageaccountsqueueservices"></a>Microsoft. ClassicStorage/storageAccounts/queueServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|QueueCapacity|Evet|Kuyruk kapasitesi|Bayt|Ortalama|Depolama hesabının Kuyruk hizmeti bayt cinsinden kullandığı depolama miktarı.|Boyut yok|
|QueueCount|Evet|Sıra sayısı|Count|Ortalama|Depolama hesabının Kuyruk hizmeti sıra sayısı.|Boyut yok|
|QueueMessageCount|Evet|Kuyruk Iletisi sayısı|Count|Ortalama|Depolama hesabının Kuyruk hizmeti sıra iletilerinin yaklaşık sayısı.|Boyut yok|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|


## <a name="microsoftclassicstoragestorageaccountstableservices"></a>Microsoft. ClassicStorage/storageAccounts/tableServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verisi miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden uçtan uca gecikmesi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure depolama tarafından başarılı bir isteği işlemek için kullanılan gecikme süresi (milisaniye cinsinden). Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Tablokapasitesi|Evet|Tablo kapasitesi|Bayt|Ortalama|Bayt olarak depolama hesabının tablo hizmeti tarafından kullanılan depolama miktarı.|Boyut yok|
|Tablosayısı|Evet|Tablo sayısı|Count|Ortalama|Depolama hesabının tablo hizmetindeki tablo sayısı.|Boyut yok|
|TableEntityCount|Evet|Tablo varlık sayısı|Count|Ortalama|Depolama hesabının tablo hizmetindeki tablo varlıklarının sayısı.|Boyut yok|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft. Biliveservices/hesapları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Blockedçağrılarında|Evet|Engellenen çağrılar|Count|Toplam|Oran veya kota sınırını aşan çağrı sayısı.|ApiName, OperationName, bölge|
|Karakterçeli|Evet|Eğitilen karakterler|Count|Toplam|Eğitilen toplam karakter sayısı.|ApiName, OperationName, bölge|
|Karakter Yabanslamuş|Evet|Çevrilen karakterler|Count|Toplam|Gelen metin isteğindeki toplam karakter sayısı.|ApiName, OperationName, bölge|
|ClientErrors|Evet|İstemci hataları|Count|Toplam|İstemci tarafı hatası olan çağrı sayısı (HTTP yanıt kodu 4xx).|ApiName, OperationName, bölge|
|Dataın|Evet|Içindeki veriler|Bayt|Toplam|Gelen verilerin bayt cinsinden boyutu.|ApiName, OperationName, bölge|
|Veri çıkışı|Evet|Giden veriler|Bayt|Toplam|Giden verilerin bayt cinsinden boyutu.|ApiName, OperationName, bölge|
|Gecikme süresi|Evet|Gecikme süresi|Mayacak|Ortalama|Milisaniye cinsinden gecikme süresi.|ApiName, OperationName, bölge|
|ProcessedImages|Evet|İşlenen görüntüler|Count|Toplam| Görüntü işleme için Işlem sayısı.|ApiName, FeatureName, UsageChannel, bölge|
|ServerErrors|Evet|Sunucu hataları|Count|Toplam|Hizmet iç hatası olan çağrı sayısı (HTTP yanıt kodu 5xx).|ApiName, OperationName, bölge|
|SpeechSessionDuration|Evet|Konuşma oturumu süresi|Saniye|Toplam|Konuşma oturumunun saniye cinsinden toplam süresi.|ApiName, OperationName, bölge|
|Başarılı çağrılar|Evet|Başarılı çağrılar|Count|Toplam|Başarılı çağrı sayısı.|ApiName, OperationName, bölge|
|Toplam çağrılar|Evet|Toplam çağrı sayısı|Count|Toplam|Toplam çağrı sayısı.|ApiName, OperationName, bölge|
|Toplam hata sayısı|Evet|Toplam Hata Sayısı|Count|Toplam|Hata yanıtıyla Toplam çağrı sayısı (HTTP yanıt kodu 4xx veya 5xx).|ApiName, OperationName, bölge|
|TotalTokenCalls|Evet|Toplam belirteç çağrısı sayısı|Count|Toplam|Toplam belirteç çağrısı sayısı.|ApiName, OperationName, bölge|
|TotalTransactions|Evet|Toplam Işlem sayısı|Count|Toplam|Toplam işlem sayısı.|Boyut yok|


## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Tüketilen CPU kredileri|Evet|Tüketilen CPU kredileri|Count|Ortalama|Sanal makine tarafından tüketilen toplam kredi sayısı|Boyut yok|
|Kalan CPU kredileri|Evet|Kalan CPU kredileri|Count|Ortalama|Patlama için kullanılabilen toplam kredi sayısı|Boyut yok|
|Veri Diski Kuyruk Derinliği|Evet|Veri diski sıra derinliği (Önizleme)|Count|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|'YI|
|Veri diski okuma bayt/sn|Evet|Veri diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|'YI|
|Veri diski okuma Işlemi/sn|Evet|Veri diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|'YI|
|Veri diski yazma bayt/sn|Evet|Veri diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|'YI|
|Veri diski yazma Işlemi/sn|Evet|Veri diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|'YI|
|Disk okuma bayt sayısı|Evet|Disk okuma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt sayısı|Boyut yok|
|Disk okuma Işlemi/sn|Evet|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si|Boyut yok|
|Disk yazma baytları|Evet|Disk yazma baytları|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt sayısı|Boyut yok|
|Disk yazma Işlemi/sn|Evet|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si|Boyut yok|
|Gelen Akışlar|Evet|Gelen Akışlar|Count|Ortalama|Gelen akışlar, gelen yöndeki geçerli akışların sayısıdır (VM 'ye giden trafik)|Boyut yok|
|Gelen akışlar en yüksek oluşturma oranı|Evet|Gelen akışlar en yüksek oluşturma oranı|Sayaçpersaniye|Ortalama|Gelen akışların en yüksek oluşturma oranı (VM 'ye giden trafik)|Boyut yok|
|Ağ Girişi|Evet|Faturalanabilir (kullanım dışı) ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan faturalandırılabilir bayt sayısı (gelen trafik) (kullanım dışı)|Boyut yok|
|Toplam ağ|Evet|Toplam ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik)|Boyut yok|
|Ağ Çıkışı|Evet|Ağdan çıkış faturalandırılabilir (kullanım dışı)|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı (giden trafik) (kullanım dışı)|Boyut yok|
|Toplam ağ çıkışı|Evet|Toplam ağ çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik)|Boyut yok|
|İşletim Sistemi Diski Kuyruk Derinliği|Evet|İşletim sistemi diski sıra derinliği (Önizleme)|Count|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|İşletim sistemi diski okuma bayt/sn|Evet|İşletim sistemi diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Boyut yok|
|İşletim sistemi diski okuma Işlemi/sn|Evet|İşletim sistemi diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Boyut yok|
|İşletim sistemi diski yazma bayt/sn|Evet|İşletim sistemi diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski yazma Işlemi/sn|Evet|İşletim sistemi diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Boyut yok|
|Disk başına işletim sistemi QD|Evet|İşletim sistemi diski QD (kullanım dışı)|Count|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|İşletim sistemi disk başına okuma bayt/sn|Evet|İşletim sistemi diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Boyut yok|
|İşletim sistemi diski başına okuma Işlemi/sn|Evet|İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Boyut yok|
|İşletim sistemi disk başına yazma bayt/sn|Evet|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski başına yazma Işlemi/sn|Evet|İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Boyut yok|
|Giden akışlar|Evet|Giden akışlar|Count|Ortalama|Giden akışlar giden yöndeki geçerli akış sayısıdır (VM 'den giden trafik)|Boyut yok|
|Giden akış maksimum oluşturma oranı|Evet|Giden akış maksimum oluşturma oranı|Sayaçpersaniye|Ortalama|Giden akışların en yüksek oluşturma oranı (VM 'den giden trafik)|Boyut yok|
|Disk başına QD|Evet|Veri diski QD (kullanım dışı)|Count|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotID|
|Disk başına okunan bayt/sn|Evet|Veri diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|SlotID|
|Disk başına okuma Işlemi/sn|Evet|Veri diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|SlotID|
|Disk başına yazma bayt/sn|Evet|Veri diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|SlotID|
|Disk başına yazma Işlemi/sn|Evet|Veri diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|SlotID|
|CPU yüzdesi|Evet|CPU yüzdesi|Yüzde|Ortalama|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi|Boyut yok|
|Premium veri diski önbelleği okuma Isabeti|Evet|Premium veri diski önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabeti|'YI|
|Premium veri diski önbelleği okuma Isabetsizliği|Evet|Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabetsizliği|'YI|
|Premium işletim sistemi disk önbelleği okuma Isabeti|Evet|Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabeti|Boyut yok|
|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Evet|Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Boyut yok|


## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Tüketilen CPU kredileri|Evet|Tüketilen CPU kredileri|Count|Ortalama|Sanal makine tarafından tüketilen toplam kredi sayısı|Boyut yok|
|Kalan CPU kredileri|Evet|Kalan CPU kredileri|Count|Ortalama|Patlama için kullanılabilen toplam kredi sayısı|Boyut yok|
|Veri Diski Kuyruk Derinliği|Evet|Veri diski sıra derinliği (Önizleme)|Count|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|LUN, VMName|
|Veri diski okuma bayt/sn|Evet|Veri diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|LUN, VMName|
|Veri diski okuma Işlemi/sn|Evet|Veri diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|LUN, VMName|
|Veri diski yazma bayt/sn|Evet|Veri diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|LUN, VMName|
|Veri diski yazma Işlemi/sn|Evet|Veri diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|LUN, VMName|
|Disk okuma bayt sayısı|Evet|Disk okuma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt sayısı|VMName|
|Disk okuma Işlemi/sn|Evet|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si|VMName|
|Disk yazma baytları|Evet|Disk yazma baytları|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt sayısı|VMName|
|Disk yazma Işlemi/sn|Evet|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si|VMName|
|Gelen Akışlar|Evet|Gelen Akışlar|Count|Ortalama|Gelen akışlar, gelen yöndeki geçerli akışların sayısıdır (VM 'ye giden trafik)|VMName|
|Gelen akışlar en yüksek oluşturma oranı|Evet|Gelen akışlar en yüksek oluşturma oranı|Sayaçpersaniye|Ortalama|Gelen akışların en yüksek oluşturma oranı (VM 'ye giden trafik)|VMName|
|Ağ Girişi|Evet|Faturalanabilir (kullanım dışı) ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan faturalandırılabilir bayt sayısı (gelen trafik) (kullanım dışı)|VMName|
|Toplam ağ|Evet|Toplam ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik)|VMName|
|Ağ Çıkışı|Evet|Ağdan çıkış faturalandırılabilir (kullanım dışı)|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı (giden trafik) (kullanım dışı)|VMName|
|Toplam ağ çıkışı|Evet|Toplam ağ çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik)|VMName|
|İşletim Sistemi Diski Kuyruk Derinliği|Evet|İşletim sistemi diski sıra derinliği (Önizleme)|Count|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|VMName|
|İşletim sistemi diski okuma bayt/sn|Evet|İşletim sistemi diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|VMName|
|İşletim sistemi diski okuma Işlemi/sn|Evet|İşletim sistemi diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|VMName|
|İşletim sistemi diski yazma bayt/sn|Evet|İşletim sistemi diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|VMName|
|İşletim sistemi diski yazma Işlemi/sn|Evet|İşletim sistemi diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|VMName|
|Disk başına işletim sistemi QD|Evet|İşletim sistemi diski QD (kullanım dışı)|Count|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|İşletim sistemi disk başına okuma bayt/sn|Evet|İşletim sistemi diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Boyut yok|
|İşletim sistemi diski başına okuma Işlemi/sn|Evet|İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Boyut yok|
|İşletim sistemi disk başına yazma bayt/sn|Evet|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski başına yazma Işlemi/sn|Evet|İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Boyut yok|
|Giden akışlar|Evet|Giden akışlar|Count|Ortalama|Giden akışlar giden yöndeki geçerli akış sayısıdır (VM 'den giden trafik)|VMName|
|Giden akış maksimum oluşturma oranı|Evet|Giden akış maksimum oluşturma oranı|Sayaçpersaniye|Ortalama|Giden akışların en yüksek oluşturma oranı (VM 'den giden trafik)|VMName|
|Disk başına QD|Evet|Veri diski QD (kullanım dışı)|Count|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotID|
|Disk başına okunan bayt/sn|Evet|Veri diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|SlotID|
|Disk başına okuma Işlemi/sn|Evet|Veri diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|SlotID|
|Disk başına yazma bayt/sn|Evet|Veri diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|SlotID|
|Disk başına yazma Işlemi/sn|Evet|Veri diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|SlotID|
|CPU yüzdesi|Evet|CPU yüzdesi|Yüzde|Ortalama|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi|VMName|
|Premium veri diski önbelleği okuma Isabeti|Evet|Premium veri diski önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabeti|LUN, VMName|
|Premium veri diski önbelleği okuma Isabetsizliği|Evet|Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabetsizliği|LUN, VMName|
|Premium işletim sistemi disk önbelleği okuma Isabeti|Evet|Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabeti|VMName|
|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Evet|Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|VMName|


## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Tüketilen CPU kredileri|Evet|Tüketilen CPU kredileri|Count|Ortalama|Sanal makine tarafından tüketilen toplam kredi sayısı|Boyut yok|
|Kalan CPU kredileri|Evet|Kalan CPU kredileri|Count|Ortalama|Patlama için kullanılabilen toplam kredi sayısı|Boyut yok|
|Veri Diski Kuyruk Derinliği|Evet|Veri diski sıra derinliği (Önizleme)|Count|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|'YI|
|Veri diski okuma bayt/sn|Evet|Veri diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|'YI|
|Veri diski okuma Işlemi/sn|Evet|Veri diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|'YI|
|Veri diski yazma bayt/sn|Evet|Veri diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|'YI|
|Veri diski yazma Işlemi/sn|Evet|Veri diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|'YI|
|Disk okuma bayt sayısı|Evet|Disk okuma bayt sayısı|Bayt|Toplam|İzleme dönemi boyunca diskten okunan bayt sayısı|Boyut yok|
|Disk okuma Işlemi/sn|Evet|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk okuma ıOPS 'si|Boyut yok|
|Disk yazma baytları|Evet|Disk yazma baytları|Bayt|Toplam|İzleme dönemi boyunca diske yazılan bayt sayısı|Boyut yok|
|Disk yazma Işlemi/sn|Evet|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Disk yazma ıOPS 'si|Boyut yok|
|Gelen Akışlar|Evet|Gelen Akışlar|Count|Ortalama|Gelen akışlar, gelen yöndeki geçerli akışların sayısıdır (VM 'ye giden trafik)|Boyut yok|
|Gelen akışlar en yüksek oluşturma oranı|Evet|Gelen akışlar en yüksek oluşturma oranı|Sayaçpersaniye|Ortalama|Gelen akışların en yüksek oluşturma oranı (VM 'ye giden trafik)|Boyut yok|
|Ağ Girişi|Evet|Faturalanabilir (kullanım dışı) ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan faturalandırılabilir bayt sayısı (gelen trafik) (kullanım dışı)|Boyut yok|
|Toplam ağ|Evet|Toplam ağ|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde alınan bayt sayısı (gelen trafik)|Boyut yok|
|Ağ Çıkışı|Evet|Ağdan çıkış faturalandırılabilir (kullanım dışı)|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde faturalandırılabilir bayt sayısı (giden trafik) (kullanım dışı)|Boyut yok|
|Toplam ağ çıkışı|Evet|Toplam ağ çıkışı|Bayt|Toplam|Sanal makineler tarafından tüm ağ arabirimlerinde giden bayt sayısı (giden trafik)|Boyut yok|
|İşletim Sistemi Diski Kuyruk Derinliği|Evet|İşletim sistemi diski sıra derinliği (Önizleme)|Count|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|İşletim sistemi diski okuma bayt/sn|Evet|İşletim sistemi diski okuma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Boyut yok|
|İşletim sistemi diski okuma Işlemi/sn|Evet|İşletim sistemi diski okuma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Boyut yok|
|İşletim sistemi diski yazma bayt/sn|Evet|İşletim sistemi diski yazma bayt/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski yazma Işlemi/sn|Evet|İşletim sistemi diski yazma Işlemi/sn (Önizleme)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Boyut yok|
|Disk başına işletim sistemi QD|Evet|İşletim sistemi diski QD (kullanım dışı)|Count|Ortalama|İşletim sistemi diski sıra derinliği (veya sıra uzunluğu)|Boyut yok|
|İşletim sistemi disk başına okuma bayt/sn|Evet|İşletim sistemi diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten okunan bayt/sn|Boyut yok|
|İşletim sistemi diski başına okuma Işlemi/sn|Evet|İşletim sistemi diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS 'yi okuyun|Boyut yok|
|İşletim sistemi disk başına yazma bayt/sn|Evet|İşletim sistemi diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diske yazılan bayt/sn|Boyut yok|
|İşletim sistemi diski başına yazma Işlemi/sn|Evet|İşletim sistemi diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İşletim sistemi diski için izleme döneminde tek bir diskten ıOPS yaz|Boyut yok|
|Giden akışlar|Evet|Giden akışlar|Count|Ortalama|Giden akışlar giden yöndeki geçerli akış sayısıdır (VM 'den giden trafik)|Boyut yok|
|Giden akış maksimum oluşturma oranı|Evet|Giden akış maksimum oluşturma oranı|Sayaçpersaniye|Ortalama|Giden akışların en yüksek oluşturma oranı (VM 'den giden trafik)|Boyut yok|
|Disk başına QD|Evet|Veri diski QD (kullanım dışı)|Count|Ortalama|Veri diski sıra derinliği (veya sıra uzunluğu)|SlotID|
|Disk başına okunan bayt/sn|Evet|Veri diski okuma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten okunan bayt/sn|SlotID|
|Disk başına okuma Işlemi/sn|Evet|Veri diski okuma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diskten ıOPS 'yi okuma|SlotID|
|Disk başına yazma bayt/sn|Evet|Veri diski yazma bayt/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme döneminde tek bir diske yazılan bayt/sn|SlotID|
|Disk başına yazma Işlemi/sn|Evet|Veri diski yazma Işlemi/sn (kullanım dışı)|Sayaçpersaniye|Ortalama|İzleme dönemi boyunca tek bir diskten ıOPS yazma|SlotID|
|CPU yüzdesi|Evet|CPU yüzdesi|Yüzde|Ortalama|Şu anda sanal makineler tarafından kullanılan ayrılmış işlem birimlerinin yüzdesi|Boyut yok|
|Premium veri diski önbelleği okuma Isabeti|Evet|Premium veri diski önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabeti|'YI|
|Premium veri diski önbelleği okuma Isabetsizliği|Evet|Premium veri diski önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium veri diski önbelleği okuma Isabetsizliği|'YI|
|Premium işletim sistemi disk önbelleği okuma Isabeti|Evet|Premium işletim sistemi disk önbelleği okuma Isabeti (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabeti|Boyut yok|
|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Evet|Premium işletim sistemi disk önbelleği okuma Isabetsizlik (Önizleme)|Yüzde|Ortalama|Premium işletim sistemi disk önbelleği okuma Isabetsizlik|Boyut yok|


## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft. Containerınstance/containerGroups

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|CpuUsage|Evet|CPU Kullanımı|Count|Ortalama|Miliçekirdekte tüm çekirdekler üzerinde CPU kullanımı.|Adı|
|MemoryUsage|Evet|Bellek Kullanımı|Bayt|Ortalama|Bayt cinsinden toplam bellek kullanımı.|Adı|
|NetworkBytesReceivedPerSecond|Evet|Saniye başına alınan ağ bayt sayısı|Bayt|Ortalama|Saniye başına alınan ağ bayt sayısı.|Boyut yok|
|NetworkBytesTransmittedPerSecond|Evet|Saniye başına aktarılan ağ baytları|Bayt|Ortalama|Saniye başına aktarılan ağ baytları.|Boyut yok|


## <a name="microsoftcontainerregistryregistries"></a>Microsoft. ContainerRegistry/kayıt defterleri

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AgentPoolCPUTime|Evet|AgentPool CPU süresi|Saniye|Toplam|Saniye cinsinden AgentPool CPU süresi|Boyut yok|
|RunDuration|Evet|Çalışma süresi|Mayacak|Toplam|Çalışma süresi (milisaniye)|Boyut yok|
|Başarılı sayı|Evet|Başarılı çekme sayısı|Count|Ortalama|Başarılı görüntü sayısı|Boyut yok|
|Başarılı Pushcount|Evet|Başarılı gönderme sayısı|Count|Ortalama|Başarılı görüntü gönderme sayısı|Boyut yok|
|TotalPullCount|Evet|Toplam çekme sayısı|Count|Ortalama|Toplam görüntü sayısı|Boyut yok|
|Toplam Pushcount|Evet|Toplam gönderme sayısı|Count|Ortalama|Toplam resim gönderim sayısı|Boyut yok|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft. ContainerService/Managedkümeler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Hayır|Yönetilen kümede toplam kullanılabilir CPU çekirdeği sayısı|Count|Ortalama|Yönetilen kümede toplam kullanılabilir CPU çekirdeği sayısı|Boyut yok|
|kube_node_status_allocatable_memory_bytes|Hayır|Yönetilen kümede toplam kullanılabilir bellek miktarı|Bayt|Ortalama|Yönetilen kümede toplam kullanılabilir bellek miktarı|Boyut yok|
|kube_node_status_condition|Hayır|Çeşitli düğüm koşullarına yönelik durumlar|Count|Ortalama|Çeşitli düğüm koşullarına yönelik durumlar|koşul, durum, status2, düğüm|
|kube_pod_status_phase|Hayır|Aşamasına göre Pod sayısı|Count|Ortalama|Aşamasına göre Pod sayısı|aşama, ad alanı, Pod|
|kube_pod_status_ready|Hayır|Hazırlık durumundaki Pod sayısı|Count|Ortalama|Hazırlık durumundaki Pod sayısı|ad alanı, Pod|


## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft. CustomProviders/resourceproviders

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|FailedRequests|Evet|Başarısız İstekler|Count|Toplam|Özel kaynak sağlayıcıları için kullanılabilir günlükleri alır|HttpMethod, CallPath, StatusCode|
|Başarılı Istek sayısı|Evet|Başarılı İstekler|Count|Toplam|Özel sağlayıcı tarafından yapılan başarılı istekler|HttpMethod, CallPath, StatusCode|


## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft. DataBoxEdge/dataBoxEdgeDevices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AvailableCapacity|Evet|Kullanılabilir kapasite|Bayt|Ortalama|Raporlama dönemi boyunca kullanılabilir kapasite (bayt cinsinden).|Boyut yok|
|BytesUploadedToCloud|Evet|Karşıya yüklenen bulut baytları (cihaz)|Bayt|Ortalama|Raporlama dönemi boyunca bir cihazdan Azure 'a yüklenen toplam bayt sayısı.|Boyut yok|
|BytesUploadedToCloudPerShare|Evet|Karşıya yüklenen bulut baytları (paylaşma)|Bayt|Ortalama|Raporlama dönemi boyunca bir paylaşımdan Azure 'a yüklenen toplam bayt sayısı.|Paylaş|
|Cloudreadüretilen Iş|Evet|Bulut Indirme performansı|BytesPerSecond|Ortalama|Raporlama döneminde Azure 'a bulut indirme performansı.|Boyut yok|
|CloudReadThroughputPerShare|Evet|Bulut Indirme verimlilik (paylaşma)|BytesPerSecond|Ortalama|Raporlama dönemi boyunca bir paylaşımdan Azure 'a aktarım hızını indirin.|Paylaş|
|Clouduploadüretilen Iş|Evet|Bulut karşıya yükleme performansı|BytesPerSecond|Ortalama|Raporlama dönemi boyunca Azure 'a bulut yükleme performansı.|Boyut yok|
|CloudUploadThroughputPerShare|Evet|Bulut karşıya yükleme üretilen Işi (paylaşma)|BytesPerSecond|Ortalama|Raporlama dönemi boyunca bir paylaşımdan Azure 'a yük aktarımını yükleme.|Paylaş|
|Hipervmemoryutilileştirme|Evet|Edge Işlem-bellek kullanımı|Yüzde|Ortalama|Kullanımdaki RAM miktarı|InstanceName|
|Hypervvirtualprocessorkullanım|Evet|Edge hesaplama-CPU yüzdesi|Yüzde|Ortalama|CPU kullanımı yüzdesi|InstanceName|
|Nicreadüretilen Iş|Evet|Okuma performansı (ağ)|BytesPerSecond|Ortalama|Cihazdaki ağ arabiriminin, ağ geçidinde bulunan tüm birimler için raporlama döneminde okuma performansı.|InstanceName|
|Nicwriteüretilen Iş|Evet|Yazma Işleme (ağ)|BytesPerSecond|Ortalama|Cihazdaki ağ arabiriminin, ağ geçidinde bulunan tüm birimler için raporlama döneminde yazma aktarım hızı.|InstanceName|
|TotalCapacity|Evet|Toplam kapasite|Bayt|Ortalama|Toplam kapasite|Boyut yok|


## <a name="microsoftdatafactorydatafactories"></a>Microsoft. DataFactory/DataFactory

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Failedçalıştırmaları|Evet|Başarısız çalıştırmalar|Count|Toplam||Ardışık düzen ınename, activityName|
|Başarılı çalıştırmalar|Evet|Başarılı çalıştırmalar|Count|Toplam||Ardışık düzen ınename, activityName|


## <a name="microsoftdatafactoryfactories"></a>Microsoft. DataFactory/Factory

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Activitycancelledçalıştırmaları|Evet|İptal edilen etkinlik ölçümleri çalıştırıyor|Count|Toplam||ActivityType, ardışık düzen adı, Failuyeniden yazma, ad|
|Activityfailedçalıştırmaları|Evet|Başarısız etkinlik çalıştırma ölçümleri|Count|Toplam||ActivityType, ardışık düzen adı, Failuyeniden yazma, ad|
|ActivitySucceededRuns|Evet|Başarılı etkinlik ölçümleri çalıştırıyor|Count|Toplam||ActivityType, ardışık düzen adı, Failuyeniden yazma, ad|
|Factorysizeingbits|Evet|Toplam fabrika boyutu (GB birimi)|Count|Maksimum||Boyut yok|
|Integrationruntimekullanılabilirliği Blememory|Evet|Tümleştirme çalışma zamanı kullanılabilir belleği|Bayt|Ortalama||Integrationruntimename, düğ,|
|IntegrationRuntimeAvailableNodeNumber|Evet|Tümleştirme çalışma zamanı kullanılabilir düğüm sayısı|Count|Ortalama||Integrationruntimename|
|Integrationruntimeaveragetaskpickupdelay|Evet|Tümleştirme çalışma zamanı sıra süresi|Saniye|Ortalama||Integrationruntimename|
|Integrationruntimecpuyüzdesi|Evet|Tümleştirme çalışma zamanı CPU kullanımı|Yüzde|Ortalama||Integrationruntimename, düğ,|
|Integrationruntimequeuelength|Evet|Tümleştirme çalışma zamanı sıra uzunluğu|Count|Ortalama||Integrationruntimename|
|Maxallowedfactorysizeingbits|Evet|İzin verilen en yüksek fabrika boyutu (GB birimi)|Count|Maksimum||Boyut yok|
|MaxAllowedResourceCount|Evet|İzin verilen en fazla varlık sayısı|Count|Maksimum||Boyut yok|
|Ardışık düzen ınecancelledçalıştırmaları|Evet|İptal edilen işlem hattı çalışma ölçümleri|Count|Toplam||Failuıyeniden yazma, ad|
|Pipelinefailedçalıştırmaları|Evet|Başarısız işlem hattı çalıştırmaları ölçümleri|Count|Toplam||Failuıyeniden yazma, ad|
|Ardışık düzen Inesucceededçalıştırmaları|Evet|Başarılı işlem hattı çalışma ölçümleri|Count|Toplam||Failuıyeniden yazma, ad|
|ResourceCount|Evet|Toplam varlık sayısı|Count|Maksimum||Boyut yok|
|Triggercancelledçalıştırmaları|Evet|İptal edilen tetikleyici çalışan ölçümleri|Count|Toplam||Ad, Failuyeniden yazma|
|Triggerfailedçalıştırmaları|Evet|Başarısız tetikleyici çalıştırma ölçümleri|Count|Toplam||Ad, Failuyeniden yazma|
|TriggerSucceededRuns|Evet|Başarılı tetikleyici çalışan ölçümleri|Count|Toplam||Ad, Failuyeniden yazma|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft. DataLakeStore/accounts

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DataRead|Evet|Okunan veriler|Bayt|Toplam|Hesaptan okunan toplam veri miktarı.|Boyut yok|
|Veri yazıldı|Evet|Yazılan veriler|Bayt|Toplam|Hesaba yazılan toplam veri miktarı.|Boyut yok|
|ReadRequests|Evet|Okuma Istekleri|Count|Toplam|Hesaba yönelik veri okuma isteklerinin sayısı.|Boyut yok|
|TotalStorage|Evet|Toplam Depolama Alanı|Bayt|Maksimum|Hesapta depolanan toplam veri miktarı.|Boyut yok|
|WriteRequests|Evet|Yazma Istekleri|Count|Toplam|Hesaba veri yazma isteği sayısı.|Boyut yok|


## <a name="microsoftdbformariadbservers"></a>Microsoft. Dbformarıdb/sunucular

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|active_connections|Evet|Etkin Bağlantılar|Count|Ortalama|Etkin Bağlantılar|Boyut yok|
|backup_storage_used|Evet|Kullanılan yedekleme depolama alanı|Bayt|Ortalama|Kullanılan yedekleme depolama alanı|Boyut yok|
|connections_failed|Evet|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|cpu_percent|Evet|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|io_consumption_percent|Evet|GÇ yüzdesi|Yüzde|Ortalama|GÇ yüzdesi|Boyut yok|
|memory_percent|Evet|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Boyut yok|
|network_bytes_egress|Evet|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Evet|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|
|seconds_behind_master|Evet|Saniye cinsinden çoğaltma gecikmesi|Count|Maksimum|Saniye cinsinden çoğaltma gecikmesi|Boyut yok|
|serverlog_storage_limit|Evet|Sunucu günlüğü depolama sınırı|Bayt|Ortalama|Sunucu günlüğü depolama sınırı|Boyut yok|
|serverlog_storage_percent|Evet|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Evet|Kullanılan sunucu günlüğü depolaması|Bayt|Ortalama|Kullanılan sunucu günlüğü depolaması|Boyut yok|
|storage_limit|Evet|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|Boyut yok|
|storage_percent|Evet|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Evet|Kullanılan depolama alanı|Bayt|Ortalama|Kullanılan depolama alanı|Boyut yok|


## <a name="microsoftdbformysqlservers"></a>Microsoft. Dbformyısql/sunucuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|active_connections|Evet|Etkin Bağlantılar|Count|Ortalama|Etkin Bağlantılar|Boyut yok|
|backup_storage_used|Evet|Kullanılan yedekleme depolama alanı|Bayt|Ortalama|Kullanılan yedekleme depolama alanı|Boyut yok|
|connections_failed|Evet|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|cpu_percent|Evet|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|io_consumption_percent|Evet|GÇ yüzdesi|Yüzde|Ortalama|GÇ yüzdesi|Boyut yok|
|memory_percent|Evet|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Boyut yok|
|network_bytes_egress|Evet|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Evet|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|
|seconds_behind_master|Evet|Saniye cinsinden çoğaltma gecikmesi|Count|Maksimum|Saniye cinsinden çoğaltma gecikmesi|Boyut yok|
|serverlog_storage_limit|Evet|Sunucu günlüğü depolama sınırı|Bayt|Maksimum|Sunucu günlüğü depolama sınırı|Boyut yok|
|serverlog_storage_percent|Evet|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Evet|Kullanılan sunucu günlüğü depolaması|Bayt|Ortalama|Kullanılan sunucu günlüğü depolaması|Boyut yok|
|storage_limit|Evet|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|Boyut yok|
|storage_percent|Evet|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Evet|Kullanılan depolama alanı|Bayt|Ortalama|Kullanılan depolama alanı|Boyut yok|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft. DBforPostgreSQL/sunucuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|active_connections|Evet|Etkin Bağlantılar|Count|Ortalama|Etkin Bağlantılar|Boyut yok|
|backup_storage_used|Evet|Kullanılan yedekleme depolama alanı|Bayt|Ortalama|Kullanılan yedekleme depolama alanı|Boyut yok|
|connections_failed|Evet|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|cpu_percent|Evet|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|io_consumption_percent|Evet|GÇ yüzdesi|Yüzde|Ortalama|GÇ yüzdesi|Boyut yok|
|memory_percent|Evet|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Boyut yok|
|network_bytes_egress|Evet|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Evet|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|
|pg_replica_log_delay_in_bytes|Evet|Çoğaltmalar genelinde en fazla gecikme|Bayt|Maksimum|En fazla çoğaltma çoğaltmasının bayt cinsinden gecikmesi|Boyut yok|
|pg_replica_log_delay_in_seconds|Evet|Çoğaltma gecikmesi|Saniye|Maksimum|Saniye cinsinden çoğaltma gecikmesi|Boyut yok|
|serverlog_storage_limit|Evet|Sunucu günlüğü depolama sınırı|Bayt|Maksimum|Sunucu günlüğü depolama sınırı|Boyut yok|
|serverlog_storage_percent|Evet|Sunucu günlüğü depolama yüzdesi|Yüzde|Ortalama|Sunucu günlüğü depolama yüzdesi|Boyut yok|
|serverlog_storage_usage|Evet|Kullanılan sunucu günlüğü depolaması|Bayt|Ortalama|Kullanılan sunucu günlüğü depolaması|Boyut yok|
|storage_limit|Evet|Depolama sınırı|Bayt|Maksimum|Depolama sınırı|Boyut yok|
|storage_percent|Evet|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Evet|Kullanılan depolama alanı|Bayt|Ortalama|Kullanılan depolama alanı|Boyut yok|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft. DBforPostgreSQL/serversv2

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|active_connections|Evet|Etkin Bağlantılar|Count|Ortalama|Etkin Bağlantılar|Boyut yok|
|cpu_percent|Evet|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|'ye|Evet|IOPS|Count|Ortalama|GÇ Işlemi/saniye|Boyut yok|
|memory_percent|Evet|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Boyut yok|
|network_bytes_egress|Evet|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Evet|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|
|storage_percent|Evet|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Evet|Kullanılan depolama alanı|Bayt|Ortalama|Kullanılan depolama alanı|Boyut yok|


## <a name="microsoftdbforpostgresqlsingleservers"></a>Microsoft. DBforPostgreSQL/singleservers

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|active_connections|Evet|Etkin Bağlantılar|Count|Ortalama|Etkin Bağlantılar|Boyut yok|
|connections_failed|Evet|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|connections_succeeded|Evet|Başarılı bağlantılar|Count|Toplam|Başarılı bağlantılar|Boyut yok|
|cpu_percent|Evet|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|'ye|Evet|IOPS|Count|Ortalama|GÇ Işlemi/saniye|Boyut yok|
|maximum_used_transactionIDs|Evet|Kullanılan en fazla Işlem kimliği|Count|Ortalama|Kullanılan en fazla Işlem kimliği|Boyut yok|
|memory_percent|Evet|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Boyut yok|
|network_bytes_egress|Evet|Ağ Çıkışı|Bayt|Toplam|Etkin bağlantılar arasında ağ çıkışı|Boyut yok|
|network_bytes_ingress|Evet|Ağ Girişi|Bayt|Toplam|Etkin bağlantılar genelinde ağ|Boyut yok|
|storage_percent|Evet|Depolama alanı yüzdesi|Yüzde|Ortalama|Depolama alanı yüzdesi|Boyut yok|
|storage_used|Evet|Kullanılan depolama alanı|Bayt|Ortalama|Kullanılan depolama alanı|Boyut yok|


## <a name="microsoftdevicesiothubs"></a>Microsoft. Devices/IotHubs

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|C2D. Commands. çıkış. Abandon. Success|Evet|C2D iletileri bırakıldı|Count|Toplam|Cihaz tarafından bırakılan buluttan cihaza yönelik ileti sayısı|Boyut yok|
|C2D. Commands. çıkış. tamamlandı. başarılı|Evet|C2D ileti teslimleri tamamlandı|Count|Toplam|Cihaz tarafından başarıyla tamamlanan buluttan cihaza ileti teslimleri sayısı|Boyut yok|
|C2D. Commands. çıkış. Red. Success|Evet|C2D iletileri reddedildi|Count|Toplam|Cihaz tarafından reddedilen buluttan cihaza yönelik ileti sayısı|Boyut yok|
|C2D. Methods. Failure|Evet|Başarısız doğrudan yöntem etkinleştirmeleri|Count|Toplam|Tüm başarısız doğrudan yöntem çağrılarının sayısı.|Boyut yok|
|C2D. Methods. requestSize|Evet|Doğrudan yöntem etkinleştirmeleri istek boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem isteklerinin ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|C2D. Methods. responseSize|Evet|Doğrudan yöntem etkinleştirmeleri yanıt boyutu|Bayt|Ortalama|Tüm başarılı doğrudan yöntem yanıtlarının ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|C2D. Methods. Success|Evet|Başarılı doğrudan yöntem etkinleştirmeleri|Count|Toplam|Tüm başarılı doğrudan yöntem çağrılarının sayısı.|Boyut yok|
|C2D. ikizi. Read. Failure|Evet|Arka uçtan başarısız ikizi okumaları|Count|Toplam|Tüm başarısız arka uç tarafından başlatılan ikizi okumaların sayısı.|Boyut yok|
|C2D. ikizi. Read. size|Evet|Arka uçtan gelen ikizi okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı arka uç başlatılan ikizi okumalarının ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|C2D. ikizi. Read. Success|Evet|Arka uçtan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı arka uç tarafından başlatılan ikizi okumaların sayısı.|Boyut yok|
|C2D. ikizi. Update. Failure|Evet|Arka uçtan başarısız ikizi güncelleştirmeleri|Count|Toplam|Tüm başarısız arka uç başlatılan ikizi güncelleştirmelerinin sayısı.|Boyut yok|
|C2D. ikizi. Update. size|Evet|Arka uçtan ikizi güncelleştirmelerinin boyutu|Bayt|Ortalama|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin ortalama, en düşük ve en büyük boyutu.|Boyut yok|
|C2D. ikizi. Update. Success|Evet|Arka uçtan başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı arka uç ile başlatılan ikizi güncelleştirmelerinin sayısı.|Boyut yok|
|C2DMessagesExpired|Evet|C2D Iletilerinin süre dolma (Önizleme)|Count|Toplam|Zaman aşımına uğradı buluttan cihaza ileti sayısı|Boyut yok|
|konfigürasyonları|Evet|Yapılandırma ölçümleri|Count|Toplam|Yapılandırma Işlemleri için ölçümler|Boyut yok|
|connectedDeviceCount|Hayır|Bağlı cihazlar (Önizleme)|Count|Ortalama|IoT Hub 'ınıza bağlı cihazların sayısı|Boyut yok|
|D2C. endpoints. çıkış. builtIn. Events|Evet|Yönlendirme: iletilere/olaylara teslim edilen iletiler|Count|Toplam|Yönlendirmenin, yerleşik uç noktaya (iletiler/olaylar) başarıyla ileti teslim IoT Hub sayısı.|Boyut yok|
|D2C. endpoints. çıkış. eventHubs|Evet|Yönlendirme: Olay Hub 'ına teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin Olay Hub 'ı uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Boyut yok|
|D2C. endpoints. çıkış. serviceBusQueues|Evet|Yönlendirme: Service Bus kuyruğuna teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin iletileri Service Bus sıra uç noktalarına başarıyla teslim dağıttığı zaman sayısı.|Boyut yok|
|D2C. endpoints. çıkış. Servicebuskonular|Evet|Yönlendirme: Service Bus konuya teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin Service Bus konu uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Boyut yok|
|D2C. endpoints. çıkış. Storage|Evet|Yönlendirme: depolamaya teslim edilen iletiler|Count|Toplam|IoT Hub yönlendirmenin depolama uç noktalarına başarıyla ileti teslim eden zaman sayısı.|Boyut yok|
|D2C. endpoints. çıkış. Storage. blob 'ları|Evet|Yönlendirme: depolamaya teslim edilen Bloblar|Count|Toplam|IoT Hub blob 'ları depolama uç noktalarına yönlendirmenin kaç kez yönlendirtığı sayısıdır.|Boyut yok|
|D2C. endpoints. çıkış. Storage. Bytes|Evet|Yönlendirme: depolamaya teslim edilen veriler|Bayt|Toplam|Depolama uç noktalarına teslim edilen veri miktarı (bayt) IoT Hub.|Boyut yok|
|D2C. endpoints. Latency. yerleik. Events|Evet|Yönlendirme: iletiler/olaylar için ileti gecikmesi|Mayacak|Ortalama|Yerleşik uç noktaya (iletiler/olaylar) gelen ileti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye).|Boyut yok|
|D2C. endpoints. Latency. eventHubs|Evet|Yönlendirme: Olay Hub 'ı için ileti gecikmesi|Mayacak|Ortalama|İleti girişi ile bir olay hub 'ı uç noktasına giriş IoT Hub ve ileti girişi arasındaki ortalama gecikme (milisaniye).|Boyut yok|
|D2C. endpoints. Latency. serviceBusQueues|Evet|Yönlendirme: Service Bus kuyruğu için ileti gecikmesi|Mayacak|Ortalama|İleti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye) Service Bus kuyruğu uç noktasına giriş.|Boyut yok|
|D2C. endpoints. Latency. Servicebuskonular|Evet|Yönlendirme: Service Bus konusu için ileti gecikmesi|Mayacak|Ortalama|İleti girişi IoT Hub ve telemetri iletisi arasındaki ortalama gecikme süresi (milisaniye) Service Bus konu uç noktasına giriş.|Boyut yok|
|D2C. endpoints. Latency. Storage|Evet|Yönlendirme: depolama için ileti gecikmesi|Mayacak|Ortalama|İleti girişi IoT Hub ve telemetri iletisine bir depolama uç noktasına giriş arasındaki ortalama gecikme süresi (milisaniye).|Boyut yok|
|D2C. telemetri. çıkış. bırakıldı|Evet|Yönlendirme: telemetri iletileri bırakıldı |Count|Toplam|İleti, ölü uç noktalar nedeniyle IoT Hub yönlendirmenin kaç kez bırakılmakta olduğunu. Bu değer, bırakılan iletiler buraya teslim edimediğinden geri dönüş rotasına teslim edilen iletileri saymaz.|Boyut yok|
|D2C. telemetri. çıkış. geri dönüş|Evet|Yönlendirme: geri dönüşe teslim edilen iletiler|Count|Toplam|Geri dönüş rotası ile ilişkili uç noktaya gönderilen iletileri yönlendirmenin IoT Hub sayısı.|Boyut yok|
|D2C. telemetri. çıkış. geçersiz|Evet|Yönlendirme: telemetri iletileri uyumsuz|Count|Toplam|Uç nokta ile uyumsuzluk nedeniyle IoT Hub yönlendirmenin iletileri teslim etme başarısız olduğu zaman sayısı. Bu değer yeniden denemeler içermez.|Boyut yok|
|D2C. telemetri. çıkış. yalnız bırakılmış|Evet|Yönlendirme: telemetri iletileri yalnız bırakılmış |Count|Toplam|Herhangi bir yönlendirme kuralıyla eşleşmediğinden (geri dönüş kuralı dahil) iletilerin IoT Hub yönlendirme tarafından yalnız bırakılmış olduğu zaman sayısı. |Boyut yok|
|D2C. telemetri. çıkış. başarılı|Evet|Yönlendirme: teslim edilen telemetri iletileri|Count|Toplam|IoT Hub yönlendirme kullanılarak iletilerin tüm uç noktalara başarıyla ulaştırılan ileti sayısı. Bir ileti birden çok uç noktaya yönlendirilmemişse, her başarılı teslimat için bu değer bir artar. Bir ileti aynı uç noktaya birden çok kez teslim edildiğinde, her başarılı teslimat için bu değer bir artar.|Boyut yok|
|D2C. telemetri. ınress. allProtocol|Evet|Telemetri iletisi gönderme denemeleri|Count|Toplam|IoT Hub 'ınıza gönderilmeye çalışılan cihazdan buluta telemetri iletisi sayısı|Boyut yok|
|D2C. telemetri. ınress. Sendvaliz|Evet|Daraltma hatası sayısı|Count|Toplam|Cihaz performansı azaltmasından kaynaklanan azaltma hatası sayısı|Boyut yok|
|D2C. telemetri. giriş. başarılı|Evet|Gönderilen telemetri iletileri|Count|Toplam|IoT Hub 'ınıza başarıyla gönderilen cihazdan buluta telemetri iletisi sayısı|Boyut yok|
|D2C. ikizi. Read. Failure|Evet|Cihazlardan başarısız ikizi okumaları|Count|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi okumaların sayısı.|Boyut yok|
|D2C. ikizi. Read. size|Evet|Cihazlardan gelen ikizi okumaların yanıt boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikizi okumalarının ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|D2C. ikizi. Read. Success|Evet|Cihazlardan başarılı ikizi okumaları|Count|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi okuma sayısı.|Boyut yok|
|D2C. ikizi. Update. Failure|Evet|Cihazlardan ikizi güncelleştirmeleri başarısız oldu|Count|Toplam|Tüm başarısız cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Boyut yok|
|D2C. ikizi. Update. size|Evet|Cihazlardan ikizi güncelleştirmelerinin boyutu|Bayt|Ortalama|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin ortalama, en düşük ve en büyük boyutu.|Boyut yok|
|D2C. ikizi. Update. Success|Evet|Cihazlardan başarılı ikizi güncelleştirmeleri|Count|Toplam|Tüm başarılı cihaz tarafından başlatılan ikizi güncelleştirmelerinin sayısı.|Boyut yok|
|Günlükmessagequotakullanıldı|Evet|Kullanılan toplam ileti sayısı|Count|Maksimum|Bugün kullanılan toplam ileti sayısı|Boyut yok|
|deviceDataUsage|Evet|Toplam cihaz verisi kullanımı|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Boyut yok|
|deviceDataUsageV2|Evet|Toplam cihaz verisi kullanımı (Önizleme)|Bayt|Toplam|Iothub 'e bağlı tüm cihazlardan ve bunlardan aktarılan baytlar|Boyut yok|
|Devices. connectedDevices. allProtocol|Evet|Bağlı cihazlar (kullanım dışı) |Count|Toplam|IoT Hub 'ınıza bağlı cihazların sayısı|Boyut yok|
|Devices. totalDevices|Evet|Toplam cihaz (kullanım dışı)|Count|Toplam|IoT Hub 'ınıza kayıtlı cihazların sayısı|Boyut yok|
|Eventgridteslimler|Evet|Event Grid teslimler (Önizleme)|Count|Toplam|Event Grid yayımlanan IoT Hub olaylarının sayısı. Başarılı ve başarısız isteklerin sayısı için sonuç boyutunu kullanın. EventType boyutu olay türünü gösterir ( https://aka.ms/ioteventgrid) .|Sonuç, EventType|
|EventGridLatency|Evet|Event Grid gecikme süresi (Önizleme)|Mayacak|Ortalama|Olay Event Grid yayımlandığında IoT Hub olayının oluşturulduğu sürenin ortalama gecikme süresi (milisaniye). Bu sayı, tüm olay türleri arasındaki bir ortalama değer. Belirli bir olay türünün gecikmesini görmek için EventType boyutunu kullanın.|Olay türü|
|Jobs. cancelJob. Failure|Evet|Başarısız iş iptalleri|Count|Toplam|İşi iptal etmek için başarısız olan tüm çağrıların sayısı.|Boyut yok|
|Jobs. cancelJob. Success|Evet|Başarılı iş iptalleri|Count|Toplam|İşi iptal etmek için tüm başarılı çağrı sayısı.|Boyut yok|
|işler. tamamlandı|Evet|Tamamlanan İşler|Count|Toplam|Tamamlanan tüm işlerin sayısı.|Boyut yok|
|Jobs. createDirectMethodJob. Failure|Evet|Yöntem çağırma işlerinin oluşturma işlemi başarısız oldu|Count|Toplam|Doğrudan yöntem çağırma işlerinin tüm başarısız oluşturma sayısı.|Boyut yok|
|Jobs. createDirectMethodJob. Success|Evet|Yöntem çağırma işlerinin başarılı oluşturmaları|Count|Toplam|Tüm başarılı doğrudan yöntem çağırma işlerinin oluşturulma sayısı.|Boyut yok|
|Jobs. createTwinUpdateJob. Failure|Evet|İkizi Update işlerinin başarısız oluşturmaları|Count|Toplam|İkizi Update işlerinin başarısız olan tüm oluşturma sayısı.|Boyut yok|
|Jobs. createTwinUpdateJob. Success|Evet|İkizi Update işlerinin başarılı oluşturmaları|Count|Toplam|Tüm başarılı ikizi Update işlerinin oluşturma sayısı.|Boyut yok|
|işler. başarısız|Evet|Başarısız işler|Count|Toplam|Başarısız olan tüm işlerin sayısı.|Boyut yok|
|Jobs. listJobs. hata|Evet|İşleri listelemek için başarısız çağrılar|Count|Toplam|Listeleme işleri için başarısız olan tüm çağrıların sayısı.|Boyut yok|
|işler. listJobs. başarılı|Evet|İşleri listelemek için başarılı çağrılar|Count|Toplam|İşleri listelemek için başarılı olan tüm çağrıların sayısı.|Boyut yok|
|Jobs. queryJobs. hata|Evet|Başarısız iş sorguları|Count|Toplam|Sorgu işlerine yapılan tüm başarısız çağrıların sayısı.|Boyut yok|
|işler. queryJobs. başarılı|Evet|Başarılı iş sorguları|Count|Toplam|Sorgu işlerine yapılan tüm başarılı çağrıların sayısı.|Boyut yok|
|RoutingDataSizeInBytesDelivered|Evet|Yönlendirme teslim Iletisi boyutunu bayt cinsinden (Önizleme)|Bayt|Toplam|IoT Hub tarafından bir uç noktaya gönderilen iletilerin bayt cinsinden toplam boyutu. Mesajların farklı uç noktalarınıza teslim edilen bayt boyutunu görüntülemek için EndpointName ve EndpointType boyutlarını kullanabilirsiniz. Ölçüm değeri, ileti birden çok uç noktaya teslim edildiğinde veya ileti aynı uç noktaya birden çok kez teslim edildiğinde de dahil olmak üzere, teslim edilen her ileti için artar.|EndpointType, EndpointName, RoutingSource|
|Routingteslimler|Evet|Yönlendirme teslimleri (Önizleme)|Count|Toplam|IoT Hub yönlendirme kullanan tüm uç noktalara ileti sunmaya kaç kez girişimde denendiğinde. Başarılı veya başarısız denemelerin sayısını görmek için sonuç boyutunu kullanın. Hatalı, bırakılmış veya yalnız bırakılmış gibi başarısızlık nedenini görmek için FailureReasonCategory boyutunu kullanın. Ayrıca, farklı uç noktalarınıza kaç tane ileti teslim edildiğini anlamak için EndpointName ve EndpointType boyutlarını da kullanabilirsiniz. Bir ileti birden çok uç noktaya teslim edildiğinde veya ileti aynı uç noktaya birden çok kez teslim edildiğinde, ölçüm değeri her bir teslim girişimi için bir artırılır.|EndpointType, EndpointName, FailureReasonCategory, result, RoutingSource|
|Routingdeliverygecikme süresi|Evet|Yönlendirme teslimi gecikmesi (Önizleme)|Mayacak|Ortalama|İleti girişi IoT Hub ve telemetri iletisi ile bir uç noktaya giriş arasındaki ortalama gecikme (milisaniye). Farklı uç noktalarınıza gecikme süresini anlamak için EndpointName ve EndpointType boyutlarını kullanabilirsiniz.|EndpointType, EndpointName, RoutingSource|
|totalDeviceCount|Hayır|Toplam cihaz (Önizleme)|Count|Ortalama|IoT Hub 'ınıza kayıtlı cihazların sayısı|Boyut yok|
|twinQueries. Failure|Evet|Başarısız ikizi sorguları|Count|Toplam|Tüm başarısız ikizi sorgularının sayısı.|Boyut yok|
|twinQueries. resultSize|Evet|İkizi sorguları sonuç boyutu|Bayt|Ortalama|Tüm başarılı ikizi sorgularının sonuç boyutunun ortalama, en düşük ve en yüksek değeri.|Boyut yok|
|twinQueries. Success|Evet|Başarılı ikizi sorguları|Count|Toplam|Tüm başarılı ikizi sorgularının sayısı.|Boyut yok|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft. Devices/provisioningServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AttestationAttempts|Evet|Kanıtlama denemeleri|Count|Toplam|Denenen cihaz belirlediğimizi karşıladığımızı sayısı|ProvisioningServiceName, durum, protokol|
|Deviceasyleri|Evet|Atanan cihazlar|Count|Toplam|IoT Hub 'ına atanan cihazların sayısı|ProvisioningServiceName, IotHubName|
|Registrationdenemeleri|Evet|Kayıt denemeleri|Count|Toplam|Denenen cihaz kaydı sayısı|ProvisioningServiceName, IotHubName, durum|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>UmentDB/databaseAccounts Microsoft.Doc

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AddRegion|Evet|Eklenen bölge|Count|Count|Eklenen bölge|Bölge|
|Oto Scalemaxüretilen Iş|Hayır|Otomatik ölçeklendirme en fazla aktarım hızı|Count|Maksimum|Otomatik ölçeklendirme en fazla aktarım hızı|DatabaseName, CollectionName|
|AvailableStorage|Hayır|kullanım dışı Kullanılabilir depolama alanı|Bayt|Toplam|"Kullanılabilir depolama alanı", Eylül 2020 ' nin sonunda Azure Izleyici 'den kaldırılacak. Cosmos DB koleksiyon depolama boyutu artık sınırsız. Tek kısıtlama, her mantıksal bölüm anahtarı için depolama boyutunun 20GB olduğu bir kısıtlamadır. En üst bölüm anahtarlarına yönelik depolama tüketimini bilmeniz için tanılama günlüğünde PartitionKeyStatistics 'i etkinleştirebilirsiniz. Cosmos DB depolama kotası hakkında daha fazla bilgi için lütfen bu belgeyi kontrol edin https://docs.microsoft.com/azure/cosmos-db/concepts-limits . Kullanımdan kaldırıldıktan sonra, kullanımdan kaldırılan ölçüm üzerinde hala tanımlanmış olan kalan uyarı kuralları otomatik olarak devre dışı bırakılır.|CollectionName, DatabaseName, bölgesi|
|Cassandraconnectionkapanışları|Hayır|Cassandra bağlantı kapanışları|Count|Toplam|1 dakikalık ayrıntı düzeyinde raporlanan, kapatılan Cassandra bağlantısı sayısı|Bölge, ClosureReason|
|Cassandraconnectosgreplicationlatency|Hayır|Cassandra Bağlayıcısı ortalama ReplicationLatency|Mayacak|Ortalama|Cassandra Bağlayıcısı ortalama ReplicationLatency|Boyut yok|
|CassandraConnectorReplicationHealthStatus|Hayır|Cassandra bağlayıcısı çoğaltma sistem durumu|Count|Count|Cassandra bağlayıcısı çoğaltma sistem durumu|NotStarted, Replicationınprogress, hata|
|Cassandrakeyspacecoluştur|Hayır|Cassandra keyspace oluşturuldu|Count|Count|Cassandra keyspace oluşturuldu|Kaynak |
|CassandraKeyspaceDelete|Hayır|Cassandra keyspace silindi|Count|Count|Cassandra keyspace silindi|Kaynak |
|CassandraKeyspaceThroughputUpdate|Hayır|Cassandra keyspace üretilen Işi güncelleştirildi|Count|Count|Cassandra keyspace üretilen Işi güncelleştirildi|Kaynak |
|CassandraKeyspaceUpdate|Hayır|Cassandra keyspace güncelleştirildi|Count|Count|Cassandra keyspace güncelleştirildi|Kaynak |
|Cassandrarequestücretleri|Hayır|Cassandra Istek ücretleri|Count|Toplam|Cassandra istekleri için tüketilen RUs|DatabaseName, CollectionName, bölge, OperationType, ResourceType|
|CassandraRequests|Hayır|Cassandra Istekleri|Count|Count|Yapılan Cassandra isteği sayısı|DatabaseName, CollectionName, bölge, OperationType, ResourceType, ErrorCode|
|CassandraTableCreate|Hayır|Cassandra tablosu oluşturuldu|Count|Count|Cassandra tablosu oluşturuldu|ResourceName, ChildResourceName, |
|CassandraTableDelete|Hayır|Cassandra tablosu silindi|Count|Count|Cassandra tablosu silindi|ResourceName, ChildResourceName, |
|CassandraTableThroughputUpdate|Hayır|Cassandra tablosu üretilen Işi güncelleştirildi|Count|Count|Cassandra tablosu üretilen Işi güncelleştirildi|ResourceName, ChildResourceName, |
|CassandraTableUpdate|Hayır|Cassandra tablosu güncelleştirildi|Count|Count|Cassandra tablosu güncelleştirildi|ResourceName, ChildResourceName, |
|CreateAccount|Evet|Hesap oluşturuldu|Count|Count|Hesap oluşturuldu|Boyut yok|
|Veri kullanımı|Hayır|Veri Kullanımı|Bayt|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam veri kullanımı|CollectionName, DatabaseName, bölgesi|
|DeleteAccount|Evet|Hesap silindi|Count|Count|Hesap silindi|Boyut yok|
|DocumentCount|Hayır|Belge sayısı|Count|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam belge sayısı|CollectionName, DatabaseName, bölgesi|
|DocumentQuota|Hayır|Belge kotası|Bayt|Toplam|5 dakika ayrıntı düzeyinde raporlanan toplam depolama kotası|CollectionName, DatabaseName, bölgesi|
|GremlinDatabaseCreate|Hayır|Gremlin veritabanı oluşturuldu|Count|Count|Gremlin veritabanı oluşturuldu|Kaynak |
|GremlinDatabaseDelete|Hayır|Gremlin veritabanı silindi|Count|Count|Gremlin veritabanı silindi|Kaynak |
|GremlinDatabaseThroughputUpdate|Hayır|Gremlin veritabanı performansı güncelleştirildi|Count|Count|Gremlin veritabanı performansı güncelleştirildi|Kaynak |
|GremlinDatabaseUpdate|Hayır|Gremlin veritabanı güncelleştirildi|Count|Count|Gremlin veritabanı güncelleştirildi|Kaynak |
|GremlinGraphCreate|Hayır|Gremlin grafiği oluşturuldu|Count|Count|Gremlin grafiği oluşturuldu|ResourceName, ChildResourceName, |
|GremlinGraphDelete|Hayır|Gremlin grafiği silindi|Count|Count|Gremlin grafiği silindi|ResourceName, ChildResourceName, |
|GremlinGraphThroughputUpdate|Hayır|Gremlin Graf üretilen Işi güncelleştirildi|Count|Count|Gremlin Graf üretilen Işi güncelleştirildi|ResourceName, ChildResourceName, |
|GremlinGraphUpdate|Hayır|Gremlin grafiği güncelleştirildi|Count|Count|Gremlin grafiği güncelleştirildi|ResourceName, ChildResourceName, |
|Indexusage|Hayır|Dizin Kullanımı|Bayt|Toplam|5 dakika ayrıntı düzeyi olarak raporlanan toplam dizin kullanımı|CollectionName, DatabaseName, bölgesi|
|Meta veri datarequests|Hayır|Meta veri Istekleri|Count|Count|Meta veri isteklerinin sayısı. Cosmos DB, her bir hesap için sistem meta veri toplamayı korur, bu da koleksiyonları, veritabanlarını, vb. ve bunların yapılandırmasını, ücretsiz olarak listeletmenize olanak tanır.|DatabaseName, CollectionName, bölge, StatusCode, |
|MongoCollectionCreate|Hayır|Mongo koleksiyonu oluşturuldu|Count|Count|Mongo koleksiyonu oluşturuldu|ResourceName, ChildResourceName, |
|MongoCollectionDelete|Hayır|Mongo koleksiyonu silindi|Count|Count|Mongo koleksiyonu silindi|ResourceName, ChildResourceName, |
|MongoCollectionThroughputUpdate|Hayır|Mongo koleksiyon üretilen Işi güncelleştirildi|Count|Count|Mongo koleksiyon üretilen Işi güncelleştirildi|ResourceName, ChildResourceName, |
|MongoCollectionUpdate|Hayır|Mongo koleksiyonu güncelleştirildi|Count|Count|Mongo koleksiyonu güncelleştirildi|ResourceName, ChildResourceName, |
|MongoDatabaseDelete|Hayır|Mongo veritabanı silindi|Count|Count|Mongo veritabanı silindi|Kaynak |
|MongoDatabaseThroughputUpdate|Hayır|Mongo veritabanı üretilen Işi güncelleştirildi|Count|Count|Mongo veritabanı üretilen Işi güncelleştirildi|Kaynak |
|MongoDBDatabaseCreate|Hayır|Mongo veritabanı oluşturuldu|Count|Count|Mongo veritabanı oluşturuldu|Kaynak |
|MongoDBDatabaseUpdate|Hayır|Mongo veritabanı güncelleştirildi|Count|Count|Mongo veritabanı güncelleştirildi|Kaynak |
|Mongorequestşarj|Evet|Mongo Istek ücreti|Count|Toplam|Tüketilen Mongo Istek birimleri|DatabaseName, CollectionName, Region, CommandName, ErrorCode, durum|
|MongoRequests|Evet|Mongo Istekleri|Count|Count|Yapılan Mongo Isteği sayısı|DatabaseName, CollectionName, Region, CommandName, ErrorCode, durum|
|MongoRequestsCount|Hayır|Mongo Istek hızı|Sayaçpersaniye|Ortalama|Mongo istek sayısı/saniye|DatabaseName, CollectionName, bölge, hata kodu|
|MongoRequestsDelete|Hayır|Mongo silme Isteği hızı|Sayaçpersaniye|Ortalama|Mongo silme isteği/saniye|DatabaseName, CollectionName, bölge, hata kodu|
|Mongorequestsınsert|Hayır|Mongo ekleme Isteği oranı|Sayaçpersaniye|Ortalama|Mongo ekleme sayısı/saniye|DatabaseName, CollectionName, bölge, hata kodu|
|MongoRequestsQuery|Hayır|Mongo sorgu Isteği hızı|Sayaçpersaniye|Ortalama|Mongo sorgu isteği/saniye|DatabaseName, CollectionName, bölge, hata kodu|
|MongoRequestsUpdate|Hayır|Mongo güncelleştirme Isteği hızı|Sayaçpersaniye|Ortalama|Saniye başına Mongo güncelleştirme isteği|DatabaseName, CollectionName, bölge, hata kodu|
|Normalizedrutüketim|Hayır|Normalleştirilmiş RU tüketimi|Yüzde|Maksimum|Dakikada maksimum RU tüketim yüzdesi|CollectionName, DatabaseName, Region, Partitionkeyrangeıd|
|Provisionedüretilen Iş|Hayır|Sağlanan İşleme Hızı|Count|Maksimum|Sağlanan İşleme Hızı|DatabaseName, CollectionName|
|RegionFailover|Evet|Yük devredilen bölge|Count|Count|Yük devredilen bölge|Boyut yok|
|RemoveRegion|Evet|Bölge kaldırıldı|Count|Count|Bölge kaldırıldı|Bölge|
|ReplicationLatency|Evet|P99 çoğaltma gecikmesi|Mayacak|Ortalama|Coğrafi olarak etkinleştirilen hesap için kaynak ve hedef bölgelerde P99 çoğaltma gecikme süresi|SourceRegion, TargetRegion|
|ServerSideLatency|Hayır|Sunucu tarafı gecikme süresi|Mayacak|Ortalama|Sunucu tarafı gecikme süresi|DatabaseName, CollectionName, bölge, ConnectionMode, OperationType, Publicapitürü|
|ServiceAvailability|Hayır|Hizmet kullanılabilirliği|Yüzde|Ortalama|Hesap istekleri bir saat, gün veya ay ayrıntı düzeyinde kullanılabilirlik|Boyut yok|
|SqlContainerCreate|Hayır|Oluşturulan SQL kapsayıcısı|Count|Count|Oluşturulan SQL kapsayıcısı|ResourceName, ChildResourceName, |
|SqlContainerDelete|Hayır|SQL kapsayıcısı silindi|Count|Count|SQL kapsayıcısı silindi|ResourceName, ChildResourceName, |
|SqlContainerThroughputUpdate|Hayır|SQL kapsayıcısı üretilen Işi güncelleştirildi|Count|Count|SQL kapsayıcısı üretilen Işi güncelleştirildi|ResourceName, ChildResourceName, |
|SqlContainerUpdate|Hayır|SQL kapsayıcısı güncelleştirildi|Count|Count|SQL kapsayıcısı güncelleştirildi|ResourceName, ChildResourceName, |
|SqlDatabaseCreate|Hayır|SQL veritabanı oluşturuldu|Count|Count|SQL veritabanı oluşturuldu|Kaynak |
|SqlDatabaseDelete|Hayır|SQL veritabanı silindi|Count|Count|SQL veritabanı silindi|Kaynak |
|SqlDatabaseThroughputUpdate|Hayır|SQL veritabanı verimlilik güncelleştirildi|Count|Count|SQL veritabanı verimlilik güncelleştirildi|Kaynak |
|SqlDatabaseUpdate|Hayır|SQL veritabanı güncelleştirildi|Count|Count|SQL veritabanı güncelleştirildi|Kaynak |
|Tablotablooluştur|Hayır|AzureTable tablo oluşturuldu|Count|Count|AzureTable tablo oluşturuldu|Kaynak |
|TableTableDelete|Hayır|AzureTable tablo silindi|Count|Count|AzureTable tablo silindi|Kaynak |
|TableTableThroughputUpdate|Hayır|AzureTable tablo üretilen Işi güncelleştirildi|Count|Count|AzureTable tablo üretilen Işi güncelleştirildi|Kaynak |
|TableTableUpdate|Hayır|AzureTable tablo güncelleştirildi|Count|Count|AzureTable tablo güncelleştirildi|Kaynak |
|TotalRequests|Evet|Toplam İstek Sayısı|Count|Count|Yapılan istek sayısı|DatabaseName, CollectionName, bölge, StatusCode, OperationType, durum|
|TotalRequestUnits|Evet|Toplam Istek birimleri|Count|Toplam|Tüketilen istek birimleri|DatabaseName, CollectionName, bölge, StatusCode, OperationType, durum|
|UpdateAccountKeys|Evet|Hesap anahtarları güncelleştirildi|Count|Count|Hesap anahtarları güncelleştirildi|Anahtar|
|UpdateAccountNetworkSettings|Evet|Hesap Ağ ayarları güncelleştirildi|Count|Count|Hesap Ağ ayarları güncelleştirildi|Boyut yok|
|UpdateAccountReplicationSettings|Evet|Hesap çoğaltma ayarları güncelleştirildi|Count|Count|Hesap çoğaltma ayarları güncelleştirildi|Boyut yok|
|UpdateDiagnosticsSettings|Hayır|Hesap Tanılama ayarları güncelleştirildi|Count|Count|Hesap Tanılama ayarları güncelleştirildi|DiagnosticSettingsName, ResourceGroupName|


## <a name="microsofteventgriddomains"></a>Microsoft. EventGrid/Domains

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Evet|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|
|Ityattemptfailcount|Hayır|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, hata, ErrorType|
|Deliverybaşarılı sayısı|Evet|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Hayır|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Evet|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|MatchedEventCount|Evet|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Konu başlığı, EventSubscriptionName, DomainEventSubscriptionName|
|PublishFailCount|Evet|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|Konu, ErrorType, hata|
|Publishbaşarılı sayısı|Evet|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Konu|
|Publishbaşarıyla Latencınms|Evet|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|


## <a name="microsofteventgrideventsubscriptions"></a>Microsoft. EventGrid/Eventabonelikleri

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Evet|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason|
|Ityattemptfailcount|Hayır|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType|
|Deliverybaşarılı sayısı|Evet|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|Boyut yok|
|DestinationProcessingDurationInMs|Hayır|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|Boyut yok|
|DroppedEventCount|Evet|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason|
|MatchedEventCount|Evet|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|Boyut yok|


## <a name="microsofteventgridextensiontopics"></a>Microsoft. EventGrid/Extensionkonuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|PublishFailCount|Evet|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Evet|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Evet|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Evet|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|


## <a name="microsofteventgridsystemtopics"></a>Microsoft. EventGrid/Systemkonuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Evet|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|
|Ityattemptfailcount|Hayır|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Evet|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|Hayır|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Evet|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|MatchedEventCount|Evet|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|PublishFailCount|Evet|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Evet|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Evet|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Evet|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|


## <a name="microsofteventgridtopics"></a>Microsoft. EventGrid/konuları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Evet|Kullanılmayan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam atılacak olay sayısı|DeadLetterReason, EventSubscriptionName|
|Ityattemptfailcount|Hayır|Teslim başarısız olaylar|Count|Toplam|Bu olay aboneliğine teslim edilemedi toplam olay sayısı|Hata, ErrorType, EventSubscriptionName|
|Deliverybaşarılı sayısı|Evet|Teslim edilen olaylar|Count|Toplam|Bu olay aboneliğine teslim edilen toplam olay sayısı|EventSubscriptionName|
|DestinationProcessingDurationInMs|Hayır|Hedef Işleme süresi|Mayacak|Ortalama|Milisaniye cinsinden hedef işleme süresi|EventSubscriptionName|
|DroppedEventCount|Evet|Bırakılan olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam bırakılan olay sayısı|DropReason, EventSubscriptionName|
|MatchedEventCount|Evet|Eşleşen olaylar|Count|Toplam|Bu olay aboneliğiyle eşleşen toplam olay sayısı|EventSubscriptionName|
|PublishFailCount|Evet|Başarısız olayları Yayımla|Count|Toplam|Bu konuda yayımlanamadı toplam olay sayısı|ErrorType, hata|
|Publishbaşarılı sayısı|Evet|Yayımlanan olaylar|Count|Toplam|Bu konuda yayınlanan toplam olay sayısı|Boyut yok|
|Publishbaşarıyla Latencınms|Evet|Yayımlama başarı gecikmesi|Mayacak|Toplam|Milisaniye cinsinden yayımlama başarısı gecikmesi|Boyut yok|
|UnmatchedEventCount|Evet|Eşleşmeyen olaylar|Count|Toplam|Bu konu için olay abonelikleriyle eşleşmeyen toplam olay sayısı|Boyut yok|


## <a name="microsofteventhubclusters"></a>Microsoft. EventHub/kümeler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ActiveConnections|Hayır|ActiveConnections|Count|Ortalama|Microsoft. EventHub için toplam etkin bağlantı sayısı.|Boyut yok|
|Kullanılabilirlik Blememory|Hayır|Kullanılabilir Bellek|Yüzde|Maksimum|Toplam belleğin yüzdesi olarak olay hub 'ı kümesi için kullanılabilir bellek.|Rol|
|CaptureBacklog|Hayır|Biriktirme listesini yakala.|Count|Toplam|Microsoft. EventHub için yakalama biriktirme listesi.|Boyut yok|
|CapturedBytes|Hayır|Yakalanan baytlar.|Bayt|Toplam|Microsoft. EventHub için yakalanan baytlar.|Boyut yok|
|CapturedMessages|Hayır|Yakalanan Iletiler.|Count|Toplam|Microsoft. EventHub için yakalanan Iletiler.|Boyut yok|
|ConnectionsClosed|Hayır|Kapatılan Bağlantılar.|Count|Ortalama|Microsoft. EventHub için kapatılan bağlantılar.|Boyut yok|
|ConnectionsOpened|Hayır|Açılan Bağlantılar.|Count|Ortalama|Microsoft. EventHub için açılan bağlantılar.|Boyut yok|
|CPU|Hayır|CPU|Yüzde|Maksimum|Bir yüzde olarak olay hub 'ı kümesine yönelik CPU kullanımı|Rol|
|Incomingbytes|Evet|Gelen Bayt Miktarı|Bayt|Toplam|Microsoft. EventHub için gelen baytlar.|Boyut yok|
|Incomingmessages|Evet|Gelen İletiler|Count|Toplam|Microsoft. EventHub için gelen Iletiler.|Boyut yok|
|Incomingrequests|Evet|Gelen İstekler|Count|Toplam|Microsoft. EventHub için gelen Istekler.|Boyut yok|
|OutgoingBytes|Evet|Giden bayt sayısı.|Bayt|Toplam|Microsoft. EventHub için giden baytlar.|Boyut yok|
|OutgoingMessages|Evet|Giden İletiler|Count|Toplam|Microsoft. EventHub için giden Iletiler.|Boyut yok|
|QuotaExceededErrors|Hayır|Kota Aşıldı Hataları.|Count|Toplam|Microsoft. EventHub için Kota aşıldı.|Boyut yok|
|ServerErrors|Hayır|Sunucu Hataları.|Count|Toplam|Microsoft. EventHub için sunucu hataları.|Boyut yok|
|Boyut|Hayır|Boyut|Bayt|Ortalama|Bir EventHub 'ın bayt cinsinden boyutu.|Rol|
|SuccessfulRequests|Hayır|Başarılı İstekler|Count|Toplam|Microsoft. EventHub için başarılı Istekler.|Boyut yok|
|OperationName|Hayır|Kısıtlanan İstekler.|Count|Toplam|Microsoft. EventHub için kısıtlanan Istekler.|Boyut yok|
|UserErrors|Hayır|Kullanıcı Hataları.|Count|Toplam|Microsoft. EventHub için Kullanıcı hataları.|Boyut yok|


## <a name="microsofteventhubnamespaces"></a>Microsoft. EventHub/ad alanları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ActiveConnections|Hayır|ActiveConnections|Count|Ortalama|Microsoft. EventHub için toplam etkin bağlantı sayısı.|Boyut yok|
|CaptureBacklog|Hayır|Biriktirme listesini yakala.|Count|Toplam|Microsoft. EventHub için yakalama biriktirme listesi.|EntityName|
|CapturedBytes|Hayır|Yakalanan baytlar.|Bayt|Toplam|Microsoft. EventHub için yakalanan baytlar.|EntityName|
|CapturedMessages|Hayır|Yakalanan Iletiler.|Count|Toplam|Microsoft. EventHub için yakalanan Iletiler.|EntityName|
|ConnectionsClosed|Hayır|Kapatılan Bağlantılar.|Count|Ortalama|Microsoft. EventHub için kapatılan bağlantılar.|EntityName|
|ConnectionsOpened|Hayır|Açılan Bağlantılar.|Count|Ortalama|Microsoft. EventHub için açılan bağlantılar.|EntityName|
|EHABL|Evet|Biriktirme listesi iletilerini Arşivle (kullanım dışı)|Count|Toplam|Ad alanı için biriktirme listesindeki Olay Hub 'ı arşiv iletileri (kullanım dışı)|Boyut yok|
|EHAMB|Evet|Arşiv iletisi verimlilik (kullanım dışı)|Bayt|Toplam|Bir ad alanında olay hub 'ı arşivlenmiş ileti işleme (kullanım dışı)|Boyut yok|
|EHAMESAJ|Evet|İletileri Arşivle (kullanım dışı)|Count|Toplam|Bir ad alanında olay hub 'ı arşivlenmiş iletiler (kullanım dışı)|Boyut yok|
|EHINBYTES|Evet|Gelen baytlar (kullanım dışı)|Bayt|Toplam|Ad alanı için Olay Hub 'ı gelen ileti işleme (kullanım dışı)|Boyut yok|
|EHINMB|Evet|Gelen bayt (geçersiz) (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı gelen ileti işleme. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine gelen bayt ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHINILETISI|Evet|Gelen Iletiler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam gelen ileti (kullanım dışı)|Boyut yok|
|EHOUTBYTES|Evet|Giden baytlar (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı giden ileti işleme (kullanım dışı)|Boyut yok|
|EHOUTMB|Evet|Giden baytlar (eski) (kullanım dışı)|Bayt|Toplam|Bir ad alanı için Olay Hub 'ı giden ileti işleme. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine giden bayt ölçümünü kullanın (kullanım dışı)|Boyut yok|
|EHODıKILETISI|Evet|Giden Iletiler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam giden ileti (kullanım dışı)|Boyut yok|
|FAILREQ|Evet|Başarısız Istekler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam başarısız istek (kullanım dışı)|Boyut yok|
|Incomingbytes|Evet|Gelen Bayt Miktarı|Bayt|Toplam|Microsoft. EventHub için gelen baytlar.|EntityName|
|Incomingmessages|Evet|Gelen İletiler|Count|Toplam|Microsoft. EventHub için gelen Iletiler.|EntityName|
|Incomingrequests|Evet|Gelen İstekler|Count|Toplam|Microsoft. EventHub için gelen Istekler.|EntityName|
|Giriş iletileri|Evet|Gelen Iletiler (eski) (kullanım dışı)|Count|Toplam|Bir ad alanı için gelen toplam ileti sayısı. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine gelen Iletiler ölçümünü kullanın (kullanım dışı)|Boyut yok|
|INREQS|Evet|Gelen Istekler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam gelen gönderme isteği sayısı (kullanım dışı)|Boyut yok|
|INTERR|Evet|İç sunucu hataları (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam iç sunucu hatası (kullanım dışı)|Boyut yok|
|Hatalı cerr|Evet|Diğer hatalar (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam başarısız istek (kullanım dışı)|Boyut yok|
|OutgoingBytes|Evet|Giden bayt sayısı.|Bayt|Toplam|Microsoft. EventHub için giden baytlar.|EntityName|
|OutgoingMessages|Evet|Giden İletiler|Count|Toplam|Microsoft. EventHub için giden Iletiler.|EntityName|
|Dış mesaj|Evet|Giden Iletiler (eski) (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam giden ileti. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine giden Iletiler ölçümünü kullanın (kullanım dışı)|Boyut yok|
|QuotaExceededErrors|Hayır|Kota Aşıldı Hataları.|Count|Toplam|Microsoft. EventHub için Kota aşıldı.|EntityName |
|ServerErrors|Hayır|Sunucu Hataları.|Count|Toplam|Microsoft. EventHub için sunucu hataları.|EntityName |
|Boyut|Hayır|Boyut|Bayt|Ortalama|Bir EventHub 'ın bayt cinsinden boyutu.|EntityName|
|SuccessfulRequests|Hayır|Başarılı İstekler|Count|Toplam|Microsoft. EventHub için başarılı Istekler.|EntityName |
|SUCCREQ|Evet|Başarılı Istekler (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam başarılı istek (kullanım dışı)|Boyut yok|
|SVRBSY|Evet|Sunucu meşgul hataları (kullanım dışı)|Count|Toplam|Bir ad alanı için toplam sunucu meşgul hatası (kullanım dışı)|Boyut yok|
|OperationName|Hayır|Kısıtlanan İstekler.|Count|Toplam|Microsoft. EventHub için kısıtlanan Istekler.|EntityName |
|UserErrors|Hayır|Kullanıcı Hataları.|Count|Toplam|Microsoft. EventHub için Kullanıcı hataları.|EntityName |


## <a name="microsofthdinsightclusters"></a>Microsoft. HDInsight/kümeler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kategorizedgatewayrequests|Evet|Kategorilere ayrılmış ağ geçidi Istekleri|Count|Toplam|Kategorilere göre ağ geçidi isteklerinin sayısı (1xx/2xx/3xx/4xx/5xx)|Http durumu|
|GatewayRequests|Evet|Ağ Geçidi Istekleri|Count|Toplam|Ağ Geçidi isteklerinin sayısı|Http durumu|
|Numactiveçalışanları|Evet|Etkin çalışan sayısı|Count|Maksimum|Etkin çalışan sayısı|MetricName|


## <a name="microsoftinsightsautoscalesettings"></a>Microsoft. Insights/oto Scalesettings

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|MetricThreshold|Evet|Ölçüm eşiği|Count|Ortalama|Otomatik ölçeklendirme çalıştırıldığında yapılandırılan otomatik ölçeklendirme eşiği.|MetricTriggerRule|
|ObservedCapacity|Evet|Gözlenen kapasite|Count|Ortalama|Yürütüldüğünde otomatik ölçeklendirme olarak bildirilen kapasite.|Boyut yok|
|ObservedMetricValue|Evet|Gözlemlenen ölçüm değeri|Count|Ortalama|Yürütüldüğünde otomatik ölçeklendirme tarafından hesaplanan değer|MetricTriggerSource|
|Scaleactionsınıated|Evet|Ölçek eylemleri başlatıldı|Count|Toplam|Ölçek işleminin yönü.|ScaleDirection|


## <a name="microsoftinsightscomponents"></a>Microsoft. Insights/bileşenler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik sonuçları/kullanılabilirliği yüzdesi|Evet|Kullanılabilirlik|Yüzde|Ortalama|Başarıyla tamamlanan kullanılabilirlik testlerinin yüzdesi|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu|
|Kullanılabilirlik sonuçları/sayısı|Hayır|Kullanılabilirlik testleri|Count|Count|Kullanılabilirlik testlerinin sayısı|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu, kullanılabilirlik sonucu/başarısı|
|Kullanılabilirlik sonuçları/süresi|Evet|Kullanılabilirlik testi süresi|Mayacak|Ortalama|Kullanılabilirlik testi süresi|Kullanılabilirlik sonucu/adı, kullanılabilirlik sonucu/konumu, kullanılabilirlik sonucu/başarısı|
|Browserzamanlamalar/networkDuration|Evet|Sayfa yükleme ağ bağlantı süresi|Mayacak|Ortalama|Kullanıcı isteği ile ağ bağlantısı arasındaki süre. DNS arama ve aktarım bağlantısı içerir.|Boyut yok|
|Browserzamanlamalar/processingDuration|Evet|İstemci işlem süresi|Mayacak|Ortalama|DOM yükleninceye kadar belgenin son baytını alma arasındaki süre. Zaman uyumsuz istekler hala işlenmeye devam edebilir.|Boyut yok|
|Browserzamanlamalar/receiveDuration|Evet|Yanıt süresini alma|Mayacak|Ortalama|İlk ve son bayt veya bağlantının kesilmesi arasındaki süre.|Boyut yok|
|Browserzamanlamalar/sendDuration|Evet|İstek gönderme süresi|Mayacak|Ortalama|Ağ bağlantısı ve ilk baytın alınması arasındaki süre.|Boyut yok|
|Browserzamanlamalar/totalDuration|Evet|Tarayıcı sayfa yükleme süresi|Mayacak|Ortalama|DOM, stil sayfaları, betikler ve görüntüler yükleninceye kadar Kullanıcı isteğinden geçen süre.|Boyut yok|
|Bağımlılıklar/sayı|Hayır|Bağımlılık çağrıları|Count|Count|Uygulama tarafından dış kaynaklara yapılan çağrıların sayısı.|bağımlılık/tür, bağımlılık/performanceBucket, bağımlılık/başarı, bağımlılık/hedef, bağımlılık/resultCode, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|Bağımlılıklar/süre|Evet|Bağımlılık süresi|Mayacak|Ortalama|Uygulama tarafından dış kaynaklara yapılan çağrıların süresi.|bağımlılık/tür, bağımlılık/performanceBucket, bağımlılık/başarı, bağımlılık/hedef, bağımlılık/resultCode, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|Bağımlılıklar/başarısız|Hayır|Bağımlılık çağrısı sorunları|Count|Count|Uygulama tarafından dış kaynaklara yapılan başarısız bağımlılık çağrılarının sayısı.|bağımlılık/tür, bağımlılık/performanceBucket, bağımlılık/hedef, bağımlılık/resultCode, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|özel durumlar/tarayıcı|Hayır|Tarayıcı özel durumları|Count|Count|Tarayıcıda oluşturulan yakalanamayan özel durumların sayısı.|bulut/roleName|
|özel durumlar/say|Evet|Özel durumlar|Count|Count|Tüm yakalanamayan özel durumların Birleşik sayısı.|bulut/rol adı, bulut/Roleınstance, istemci/tür|
|özel durumlar/sunucu|Hayır|Sunucu özel durumları|Count|Count|Sunucu uygulamasında oluşturulan yakalanamayan özel durum sayısı.|Cloud/roleName, Cloud/Roleınstance|
|pageViews/Count|Evet|Sayfa görünümleri|Count|Count|Sayfa görüntüleme sayısı.|işlem/yapay, bulut/roleName|
|pageViews/Duration|Evet|Sayfa görünümü yükleme süresi|Mayacak|Ortalama|Sayfa görünümü yükleme süresi|işlem/yapay, bulut/roleName|
|performanceCounters/exceptionsPerSecond|Evet|Özel durum oranı|Sayaçpersaniye|Ortalama|.NET özel durumları ve .NET özel durumlarına dönüştürülmüş yönetilmeyen özel durumlar da dahil olmak üzere, Windows 'a bildirilen işlenmiş ve işlenmemiş özel durumların sayısı.|Cloud/Roleınstance|
|performanceCounters/memoryAvailableBytes|Evet|Kullanılabilir bellek|Bayt|Ortalama|Bir işleme veya sistem kullanımına ayırmak için hemen kullanılabilir fiziksel bellek.|Cloud/Roleınstance|
|performanceCounters/processCpuPercentage|Evet|İşlem CPU 'SU|Yüzde|Ortalama|Tüm işlem iş parçacıklarının yönergeleri yürütmek için işlemciyi kullandığı geçen sürenin yüzdesi. Bu, 0 ila 100 arasında farklılık gösterebilir. Bu ölçüm, W3wp sürecinin performansını tek başına gösterir.|Cloud/Roleınstance|
|performanceCounters/Processıobitespersecond|Evet|İşlem GÇ oranı|BytesPerSecond|Ortalama|Dakikada, ağda ve cihazlarda okunan ve yazılan toplam bayt/saniye.|Cloud/Roleınstance|
|performanceCounters/Processorcpuyüzdesi|Evet|İşlemci zamanı|Yüzde|Ortalama|İşlemcinin boş olmayan iş parçacıklarında harcadığı sürenin yüzdesi.|Cloud/Roleınstance|
|performanceCounters/processPrivateBytes|Evet|İşlem özel baytları|Bayt|Ortalama|İzlenen uygulamanın işlemlerine özel olarak atanan bellek.|Cloud/Roleınstance|
|performanceCounters/requestExecutionTime|Evet|HTTP isteği yürütme süresi|Mayacak|Ortalama|En son isteğin yürütme süresi.|Cloud/Roleınstance|
|performanceCounters/Requestsınqueue|Evet|Uygulama kuyruğundaki HTTP istekleri|Count|Ortalama|Uygulama isteği sırasının uzunluğu.|Cloud/Roleınstance|
|performanceCounters/requestsPerSecond|Evet|HTTP istek hızı|Sayaçpersaniye|Ortalama|ASP.NET ' den saniyede uygulamaya yapılan tüm isteklerin oranı.|Cloud/Roleınstance|
|istek/sayı|Hayır|Sunucu istekleri|Count|Count|Tamamlanan HTTP isteği sayısı.|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|istek/süre|Evet|Sunucu yanıt süresi|Mayacak|Ortalama|Bir HTTP isteği alma ve yanıtın gönderilmesini tamamlama arasındaki süre.|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|istek/başarısız|Hayır|Başarısız istekler|Count|Count|Başarısız olarak işaretlenen HTTP isteklerinin sayısı. Çoğu durumda bunlar >= 400 yanıt koduna sahip isteklerdir ve 401 ' e eşit değildir.|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, Cloud/roleName|
|istek/hız|Hayır|Sunucu isteği hızı|Sayaçpersaniye|Ortalama|Saniye başına sunucu isteği oranı|istek/performanceBucket, istek/resultCode, işlem/yapay, Cloud/Roleınstance, istek/başarı, bulut/roleName|
|izlemeler/say|Evet|İzlemeler|Count|Count|İzleme belgesi sayısı|izleme/severityLevel, işlem/yapay, Cloud/roleName, Cloud/Roleınstance|

## <a name="microsoftiotcentraliotapps"></a>Microsoft. ıotcentral/ıotapps

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|C2D. Property. Read. Failure|Evet|IoT Central 'den cihaz özelliği okumaları başarısız oldu|Count|Toplam|IoT Central başlatılan tüm başarısız Özellik okuma sayısı|Boyut yok|
|C2D. Property. Read. Success|Evet|IoT Central 'den başarılı cihaz özelliği okumaları|Count|Toplam|IoT Central başlatılan tüm başarılı Özellik okuma sayısı|Boyut yok|
|C2D. Property. Update. Failure|Evet|IoT Central cihaz özelliği güncelleştirmeleri başarısız oldu|Count|Toplam|IoT Central tarafından başlatılan tüm başarısız özellik güncelleştirmelerinin sayısı|Boyut yok|
|C2D. Property. Update. Success|Evet|IoT Central 'den başarılı cihaz özelliği güncelleştirmeleri|Count|Toplam|IoT Central başlatıldığı tüm başarılı özellik güncelleştirmeleri sayısı|Boyut yok|
|connectedDeviceCount|Hayır|Toplam bağlı cihaz sayısı|Count|Ortalama|IoT Central bağlı cihazların sayısı|Boyut yok|
|D2C. Property. Read. Failure|Evet|Cihazlardan cihaz özelliği okumaları başarısız oldu|Count|Toplam|Cihazlardan başlatılan tüm başarısız Özellik okuma sayısı|Boyut yok|
|D2C. Property. Read. Success|Evet|Cihazlardan başarılı cihaz özelliği okumaları|Count|Toplam|Cihazlardan başlatılan tüm başarılı Özellik okuma sayısı|Boyut yok|
|D2C. Property. Update. Failure|Evet|Cihazlardan cihaz özelliği güncelleştirmeleri başarısız oldu|Count|Toplam|Cihazlardan başlatılan tüm başarısız özellik güncelleştirmelerinin sayısı|Boyut yok|
|D2C. Property. Update. Success|Evet|Cihazlardan başarılı cihaz özelliği güncelleştirmeleri|Count|Toplam|Cihazlardan başlatılan tüm başarılı özellik güncelleştirmelerinin sayısı|Boyut yok|


## <a name="microsoftkeyvaultvaults"></a>Microsoft. Keykasası/kasa

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Genel kasa kullanılabilirliği|Yüzde|Ortalama|Kasa istekleri kullanılabilirliği|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|Cumartesi Ationshoe kutusu|Hayır|Genel kasa doygunluğu|Yüzde|Ortalama|Kullanılan kasa kapasitesi|ActivityType, ActivityName, TransactionType|
|ServiceApiHit|Evet|Toplam hizmet API 'Si Isabet sayısı|Count|Count|Toplam hizmet API 'si isabet sayısı|ActivityType, ActivityName|
|Hizmet Apilatesi|Evet|Genel hizmet API 'Si gecikmesi|Mayacak|Ortalama|Hizmet API 'si isteklerinin genel gecikmesi|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|ServiceApiResult|Evet|Toplam hizmet API 'Si sonuçları|Count|Count|Toplam hizmet API 'si sonuç sayısı|ActivityType, ActivityName, StatusCode, StatusCodeClass|


## <a name="microsoftkustoclusters"></a>Microsoft. kusto/kümeler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|BatchBlobCount|Evet|Toplu iş blobu sayısı|Count|Ortalama|Alma için toplanmış toplu işteki veri kaynağı sayısı.|Veritabanı|
|BatchDuration|Evet|Toplu iş süresi|Saniye|Ortalama|Alma akışındaki toplama aşamasının süresi.|Veritabanı|
|Batchesişlendi|Evet|Işlenen toplu işler|Count|Ortalama|Alma için toplanan toplu iş sayısı. Toplu işlem tamamlanma nedeni: toplu işlemin toplu işlem zamanına, veri boyutuna veya dosya sayısına göre ayarlanmış olup olmadığı, ilke toplu işleme tarafından ayarlanan|Veritabanı, SealReason|
|BatchSize|Evet|Toplu İş Boyutu|Bayt|Ortalama|Alma için toplanmış bir toplu işte beklenen veri boyutu sıkıştırılmamış.|Veritabanı|
|Cachekullanım|Evet|Önbellek kullanımı|Yüzde|Ortalama|Küme kapsamındaki kullanım düzeyi|Boyut yok|
|ContinuousExportMaxLatenessMinutes|Evet|Sürekli dışarı aktarma maksimum değeri|Count|Maksimum|Kümedeki sürekli dışarı aktarma işleri tarafından raporlanan süre (dakika cinsinden)|Boyut yok|
|Continuousexportnumofrecordsexports|Evet|Sürekli dışa aktarma – dışarı aktarılmış kayıtların sayısı|Count|Toplam|Dışarı aktarma işlemi sırasında yazılan her depolama yapıtı için tetiklenen, dışarı aktarılmış kayıt sayısı|Devamousexportname, veritabanı|
|ContinuousExportPendingCount|Evet|Sürekli dışarı aktarma bekleyen sayısı|Count|Maksimum|Yürütmeye hazırlamış bekleyen sürekli dışarı aktarma işlerinin sayısı|Boyut yok|
|ContinuousExportResult|Evet|Sürekli dışarı aktarma sonucu|Count|Count|Sürekli dışarı aktarmanın başarılı veya başarısız olup olmadığını belirtir|Devamousexportname, sonuç, veritabanı|
|CPU|Evet|CPU|Yüzde|Ortalama|CPU kullanım düzeyi|Boyut yok|
|Processedforeventhubs olayları|Evet|İşlenen Olaylar (olay/IoT Hub 'Ları için)|Count|Toplam|Olay/IoT Hub geri ödeme yaparken küme tarafından işlenen olay sayısı|EventStatus|
|Dışarı Aktaramstilileştirme|Evet|Dışarı Aktarma Kullanımı|Yüzde|Maksimum|Kullanım verme|Boyut yok|
|Inestionlatencınseconds|Evet|Alma gecikmesi (saniye)|Saniye|Ortalama|Kaynaktan (örn. ileti EventHub ' de bulunur) kümeye saniye cinsinden alma süresi|Boyut yok|
|Inestionresult|Evet|Alım sonucu|Count|Count|Alma işlemlerinin sayısı|Inestionresultdetails|
|Alım kullanımı|Evet|Alım kullanımı|Yüzde|Ortalama|Kümedeki kullanılan alma yuvaları oranı|Boyut yok|
|Inestionvolumeınmb|Evet|Alım birimi (MB)|Count|Toplam|Kümeye alınan verilerin toplam hacmi (MB)|Boyut yok|
|Yükteki InstanceCount|Evet|Örnek Sayısı|Count|Ortalama|Toplam örnek sayısı|Boyut yok|
|Alı|Evet|Canlı tut|Count|Ortalama|Sanity denetimi, kümenin sorgulara yanıt verdiğini belirtir|Boyut yok|
|QueryDuration|Evet|Sorgu süresi|Mayacak|Ortalama|Sorgu süresi (saniye)|QueryStatus|
|SteamingIngestRequestRate|Evet|Akış Alma İstek Oranı|Count|Kterequestspersecond|Akış alma isteği hızı (saniye başına istek)|Boyut yok|
|StreamingIngestDataRate|Evet|Akış Alma Veri Oranı|Count|Ortalama|Akış alma verileri oranı (MB/saniye)|Boyut yok|
|StreamingIngestDuration|Evet|Akış Alma Süresi|Mayacak|Ortalama|Milisaniye cinsinden akış alma süresi|Boyut yok|
|StreamingIngestResults|Evet|Akış Alma Sonucu|Count|Ortalama|Akış alma sonucu|Sonuç|
|TotalNumberOfConcurrentQueries|Evet|Toplam eşzamanlı sorgu sayısı|Count|Toplam|Toplam eşzamanlı sorgu sayısı|Boyut yok|
|Totalnumberofkapsamları|Evet|Toplam kapsam sayısı|Count|Toplam|Toplam veri uzantısı sayısı|Boyut yok|
|Totalnumberofkısıtlar Ledcommands|Evet|Kısıtlanmış komutların toplam sayısı|Count|Toplam|Kısıtlanmış komutların toplam sayısı|CommandType|
|Toplam sorgu sayısı|Evet|Toplam kısıtlanmış sorgu sayısı|Count|Toplam|Toplam kısıtlanmış sorgu sayısı|Boyut yok|


## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft. Logic/ıntegrationserviceortamortamları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ActionLatency|Evet|Eylem gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionsCompleted|Evet|Tamamlanan eylemler |Count|Toplam|Tamamlanan iş akışı eylemlerinin sayısı.|Boyut yok|
|ActionsFailed|Evet|Başarısız eylemler |Count|Toplam|Başarısız iş akışı eylemi sayısı.|Boyut yok|
|ActionsSkipped|Evet|Atlanan eylemler |Count|Toplam|Atlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionsStarted|Evet|Başlatılan eylemler |Count|Toplam|Başlatılan iş akışı eylemi sayısı.|Boyut yok|
|ActionsSucceeded|Evet|İşlemler başarılı oldu |Count|Toplam|Başarılı iş akışı eylemi sayısı.|Boyut yok|
|Actionbaşarılı gecikme süresi|Evet|Eylem başarı gecikmesi |Saniye|Ortalama|Başarılı iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionThrottledEvents|Evet|Eylem kısıtlanmış olaylar|Count|Toplam|İş akışı eylemi kısıtlanmış olay sayısı..|Boyut yok|
|Integrationserviceenvironmentconnectormemoryusage|Evet|Tümleştirme Hizmeti Ortamı için bağlayıcı bellek kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için bağlayıcı bellek kullanımı.|Boyut yok|
|Integrationserviceenvironmentconnectorprocessorusage|Evet|Tümleştirme Hizmeti Ortamı için bağlayıcı Işlemci kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için bağlayıcı işlemci kullanımı.|Boyut yok|
|Integrationserviceenvironmentworkflowmemoryusage|Evet|Tümleştirme Hizmeti Ortamı için iş akışı bellek kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için iş akışı bellek kullanımı.|Boyut yok|
|Integrationserviceenvironmentworkflowprocessorusage|Evet|Tümleştirme Hizmeti Ortamı için iş akışı Işlemcisi kullanımı|Yüzde|Ortalama|Tümleştirme hizmeti ortamı için iş akışı işlemcisi kullanımı.|Boyut yok|
|RunFailurePercentage yüzdesi|Evet|Çalıştırma hatası yüzdesi|Yüzde|Toplam|İş akışı çalıştırmalarının yüzdesi başarısız oldu.|Boyut yok|
|RunLatency|Evet|Çalıştırma gecikmesi|Saniye|Ortalama|Tamamlanan iş akışı çalıştırmalarının gecikmesi.|Boyut yok|
|RunsCancelled|Evet|Çalıştırma Iptal edildi|Count|Toplam|İptal edilen iş akışı çalıştırma sayısı.|Boyut yok|
|RunsCompleted|Evet|Tamamlanan çalıştırmalar|Count|Toplam|Tamamlanan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsFailed|Evet|Çalıştırma başarısız|Count|Toplam|Başarısız iş akışı çalıştırma sayısı.|Boyut yok|
|RunsStarted|Evet|Çalışma başlatıldı|Count|Toplam|Başlatılan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsSucceeded|Evet|Çalışma başarılı|Count|Toplam|İş akışı çalıştırma sayısı başarılı oldu.|Boyut yok|
|RunStartThrottledEvents|Evet|Kısıtlanmış başlangıç olaylarını Çalıştır|Count|Toplam|İş akışı çalıştırma, kısıtlanmış olay sayısı.|Boyut yok|
|Runbaşarılı gecikme süresi|Evet|Çalıştırma başarılı gecikme süresi|Saniye|Ortalama|Başarılı iş akışı çalıştırmalarının gecikmesi.|Boyut yok|
|RunThrottledEvents|Evet|Kısıtlanmış olayları Çalıştır|Count|Toplam|İş akışı eylemi veya tetikleyici kısıtlanan olay sayısı.|Boyut yok|
|Triggerfirelatlik|Evet|Tetikleyici Tetikleme gecikmesi |Saniye|Ortalama|Tetiklenen iş akışı Tetikleyicileri gecikmesi.|Boyut yok|
|TriggerLatency|Evet|Tetikleme gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggersCompleted|Evet|Tamamlanan Tetikleyiciler |Count|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersFailed|Evet|Başarısız Tetikleyiciler |Count|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Boyut yok|
|Triggersharekete geçirildi|Evet|Tetiklenen Tetikleyiciler |Count|Toplam|Tetiklenen iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSkipped|Evet|Atlanan Tetikleyiciler|Count|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersStarted|Evet|Başlatılan Tetikleyiciler |Count|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSucceeded|Evet|Başarılı Tetikleyiciler |Count|Toplam|Başarılı iş akışı tetikleyicisi sayısı.|Boyut yok|
|Triggerbaşarılı gecikme|Evet|Tetikleyici başarı gecikme süresi |Saniye|Ortalama|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerThrottledEvents|Evet|Kısıtlanmış olayları tetikleyin|Count|Toplam|İş akışı tetikleyicisi kısıtlanmış olay sayısı.|Boyut yok|


## <a name="microsoftlogicworkflows"></a>Microsoft. Logic/iş akışları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ActionLatency|Evet|Eylem gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionsCompleted|Evet|Tamamlanan eylemler |Count|Toplam|Tamamlanan iş akışı eylemlerinin sayısı.|Boyut yok|
|ActionsFailed|Evet|Başarısız eylemler |Count|Toplam|Başarısız iş akışı eylemi sayısı.|Boyut yok|
|ActionsSkipped|Evet|Atlanan eylemler |Count|Toplam|Atlanan iş akışı eylemi sayısı.|Boyut yok|
|ActionsStarted|Evet|Başlatılan eylemler |Count|Toplam|Başlatılan iş akışı eylemi sayısı.|Boyut yok|
|ActionsSucceeded|Evet|İşlemler başarılı oldu |Count|Toplam|Başarılı iş akışı eylemi sayısı.|Boyut yok|
|Actionbaşarılı gecikme süresi|Evet|Eylem başarı gecikmesi |Saniye|Ortalama|Başarılı iş akışı eylemlerinin gecikme süresi.|Boyut yok|
|ActionThrottledEvents|Evet|Eylem kısıtlanmış olaylar|Count|Toplam|İş akışı eylemi kısıtlanmış olay sayısı..|Boyut yok|
|Billableactionyürütmeler|Evet|Faturalanabilir eylem yürütmeleri|Count|Toplam|Faturalandırılan iş akışı eylemi yürütmelerinin sayısı.|Boyut yok|
|Billabletriggeryürütmeler|Evet|Faturalanabilir tetikleyici yürütmeleri|Count|Toplam|Faturalandırılan iş akışı tetikleyici yürütmelerinin sayısı.|Boyut yok|
|BillingUsageNativeOperation|Evet|Yerel Işlem yürütmeleri için faturalandırma kullanımı|Count|Toplam|Faturalandırılan yerel işlem yürütmelerinin sayısı.|Boyut yok|
|BillingUsageStandardConnector|Evet|Standart bağlayıcı yürütmeleri için faturalandırma kullanımı|Count|Toplam|Faturalandırılan standart bağlayıcı yürütmelerinin sayısı.|Boyut yok|
|Billingusagestorampatüketim|Evet|Depolama tüketimi yürütmeleri için faturalama kullanımı|Count|Toplam|Faturalandırılan depolama tüketimi yürütmelerinin sayısı.|Boyut yok|
|RunFailurePercentage yüzdesi|Evet|Çalıştırma hatası yüzdesi|Yüzde|Toplam|İş akışı çalıştırmalarının yüzdesi başarısız oldu.|Boyut yok|
|RunLatency|Evet|Çalıştırma gecikmesi|Saniye|Ortalama|Tamamlanan iş akışı çalıştırmalarının gecikmesi.|Boyut yok|
|RunsCancelled|Evet|Çalıştırma Iptal edildi|Count|Toplam|İptal edilen iş akışı çalıştırma sayısı.|Boyut yok|
|RunsCompleted|Evet|Tamamlanan çalıştırmalar|Count|Toplam|Tamamlanan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsFailed|Evet|Çalıştırma başarısız|Count|Toplam|Başarısız iş akışı çalıştırma sayısı.|Boyut yok|
|RunsStarted|Evet|Çalışma başlatıldı|Count|Toplam|Başlatılan iş akışı çalıştırma sayısı.|Boyut yok|
|RunsSucceeded|Evet|Çalışma başarılı|Count|Toplam|İş akışı çalıştırma sayısı başarılı oldu.|Boyut yok|
|RunStartThrottledEvents|Evet|Kısıtlanmış başlangıç olaylarını Çalıştır|Count|Toplam|İş akışı çalıştırma, kısıtlanmış olay sayısı.|Boyut yok|
|Runbaşarılı gecikme süresi|Evet|Çalıştırma başarılı gecikme süresi|Saniye|Ortalama|Başarılı iş akışı çalıştırmalarının gecikmesi.|Boyut yok|
|RunThrottledEvents|Evet|Kısıtlanmış olayları Çalıştır|Count|Toplam|İş akışı eylemi veya tetikleyici kısıtlanan olay sayısı.|Boyut yok|
|Totalbillableyürütmeleri|Evet|Toplam faturalandırılabilir yürütmeler|Count|Toplam|Faturalandırılan iş akışı yürütmelerinin sayısı.|Boyut yok|
|Triggerfirelatlik|Evet|Tetikleyici Tetikleme gecikmesi |Saniye|Ortalama|Tetiklenen iş akışı Tetikleyicileri gecikmesi.|Boyut yok|
|TriggerLatency|Evet|Tetikleme gecikmesi |Saniye|Ortalama|Tamamlanan iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggersCompleted|Evet|Tamamlanan Tetikleyiciler |Count|Toplam|Tamamlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersFailed|Evet|Başarısız Tetikleyiciler |Count|Toplam|Başarısız iş akışı tetikleyicisi sayısı.|Boyut yok|
|Triggersharekete geçirildi|Evet|Tetiklenen Tetikleyiciler |Count|Toplam|Tetiklenen iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSkipped|Evet|Atlanan Tetikleyiciler|Count|Toplam|Atlanan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersStarted|Evet|Başlatılan Tetikleyiciler |Count|Toplam|Başlatılan iş akışı tetikleyicisi sayısı.|Boyut yok|
|TriggersSucceeded|Evet|Başarılı Tetikleyiciler |Count|Toplam|Başarılı iş akışı tetikleyicisi sayısı.|Boyut yok|
|Triggerbaşarılı gecikme|Evet|Tetikleyici başarı gecikme süresi |Saniye|Ortalama|Başarılı iş akışı tetikleyicilerinin gecikmesi.|Boyut yok|
|TriggerThrottledEvents|Evet|Kısıtlanmış olayları tetikleyin|Count|Toplam|İş akışı tetikleyicisi kısıtlanmış olay sayısı.|Boyut yok|


## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft. MachineLearningServices/çalışma alanları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Etkin çekirdekler|Evet|Etkin çekirdekler|Count|Ortalama|Etkin çekirdek sayısı|Senaryo, ClusterName|
|Etkin düğümler|Evet|Etkin düğümler|Count|Ortalama|Acitve düğümlerin sayısı. Bunlar, etkin olarak bir işi çalıştıran düğümlerdir.|Senaryo, ClusterName|
|Istenen çalıştırmaları iptal et|Evet|Istenen çalıştırmaları iptal et|Count|Toplam|Bu çalışma alanı için iptal edilen çalıştırma sayısı. Bir çalıştırma için iptal isteği alındığında sayı güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|İptal edilen çalıştırmalar|Evet|İptal edilen çalıştırmalar|Count|Toplam|Bu çalışma alanı için iptal edilen çalıştırma sayısı. Bir çalıştırma başarıyla iptal edildiğinde sayı güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Tamamlanan çalıştırmalar|Evet|Tamamlanan çalıştırmalar|Count|Toplam|Bu çalışma alanı için çalıştırma sayısı başarıyla tamamlandı. Bir çalıştırma tamamlandığında ve çıkış toplandığında sayı güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Cpukullanımı|Evet|Cpukullanımı|Count|Ortalama|Bir CPU düğümündeki bellek kullanımının yüzdesi. Kullanım, bir dakikalık aralıklarla raporlanır.|Senaryo, RunId, NodeId, ClusterName|
|Hatalar|Evet|Hatalar|Count|Toplam|Bu çalışma alanındaki çalıştırma hatalarının sayısı. Çalışma bir hatayla karşılaştığında, sayı güncellenir.|Senaryo|
|Başarısız çalıştırmalar|Evet|Başarısız çalıştırmalar|Count|Toplam|Bu çalışma alanı için başarısız olan çalıştırma sayısı. Bir çalıştırma başarısız olduğunda sayı güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Çalıştırmalar sonlandırılıyor|Evet|Çalıştırmalar sonlandırılıyor|Count|Toplam|Bu çalışma alanı için sonlandırılıyor durum girilen çalıştırma sayısı. Bir çalıştırma tamamlandığında ancak çıkış koleksiyonu hala devam ederken sayı güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Gpukullanım|Evet|Gpukullanım|Count|Ortalama|Bir GPU düğümündeki bellek kullanımının yüzdesi. Kullanım, bir dakikalık aralıklarla raporlanır.|Senaryo, RunId, NodeId, DeviceID, ClusterName|
|Boştaki çekirdekler|Evet|Boştaki çekirdekler|Count|Ortalama|Boşta çekirdek sayısı|Senaryo, ClusterName|
|Boştaki düğümler|Evet|Boştaki düğümler|Count|Ortalama|Boştaki düğüm sayısı. Boştaki düğümler, herhangi bir işi çalıştırmayan ancak varsa yeni işi kabul edebilecek düğümlerdir.|Senaryo, ClusterName|
|Çekirdekleri bırakma|Evet|Çekirdekleri bırakma|Count|Ortalama|Bırakma çekirdekleri sayısı|Senaryo, ClusterName|
|Düğümlerden çıkılıyor|Evet|Düğümlerden çıkılıyor|Count|Ortalama|Düğüm sayısı. Düğüm ayrıldığınızda, bir işi işlemeyi tamamlamış olan düğümler ve boşta durumuna gidecektir.|Senaryo, ClusterName|
|Model Dağıtımı başarısız oldu|Evet|Model Dağıtımı başarısız oldu|Count|Toplam|Bu çalışma alanında başarısız olan model dağıtımı sayısı|Senaryo, StatusCode|
|Model Dağıtımı başladı|Evet|Model Dağıtımı başladı|Count|Toplam|Bu çalışma alanında başlatılan model dağıtımı sayısı|Senaryo|
|Model Dağıtımı başarılı oldu|Evet|Model Dağıtımı başarılı oldu|Count|Toplam|Bu çalışma alanındaki başarılı olan model dağıtımı sayısı|Senaryo|
|Model kaydı başarısız oldu|Evet|Model kaydı başarısız oldu|Count|Toplam|Bu çalışma alanında başarısız olan model kayıtlarının sayısı|Senaryo, StatusCode|
|Model kaydı başarılı oldu|Evet|Model kaydı başarılı oldu|Count|Toplam|Bu çalışma alanında başarılı olan model kaydı sayısı|Senaryo|
|Yanıt vermiyor çalıştırmaları|Evet|Yanıt vermiyor çalıştırmaları|Count|Toplam|Bu çalışma alanı için yanıt vermeyen çalıştırma sayısı. Bir çalıştırma yanıt vermiyorsa, sayı güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Başlatılmamış çalıştırmalar|Evet|Başlatılmamış çalıştırmalar|Count|Toplam|Bu çalışma alanı için başlatılmamış durumdaki çalıştırma sayısı. Sayı, çalıştırma oluşturmak için bir istek alındığında ancak çalıştırma bilgisi henüz doldurulmadığında güncelleştirilir. |Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Önden çekirdekler|Evet|Önden çekirdekler|Count|Ortalama|Yok edilecek çekirdek sayısı|Senaryo, ClusterName|
|Önden düğümler|Evet|Önden düğümler|Count|Ortalama|Yok edilecek düğüm sayısı. Bu düğümler, kullanılabilir düğüm havuzundan alınan düşük öncelikli düğümlerdir.|Senaryo, ClusterName|
|Çalıştırmalar hazırlanıyor|Evet|Çalıştırmalar hazırlanıyor|Count|Toplam|Bu çalışma alanı için hazırlanma çalıştırmaların sayısı. Çalışma ortamı hazırlanırken bir çalıştırma duruma hazırlık girdiğinde sayı güncellenir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Sağlama çalıştırmaları|Evet|Sağlama çalıştırmaları|Count|Toplam|Bu çalışma alanı için sağlama olan çalıştırma sayısı. Bir çalıştırma, işlem hedefi oluşturmayı veya sağlamayı beklerken, sayı güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Sıraya alınan çalıştırmalar|Evet|Sıraya alınan çalıştırmalar|Count|Toplam|Bu çalışma alanı için sıraya alınan çalıştırmaların sayısı. Bir çalıştırma işlem hedefinde sıraya alınmışsa, sayı güncelleştirilir. Gerekli işlem düğümlerinin hazırlanmasını beklerken ortaya kodan olabilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Kota kullanım yüzdesi|Evet|Kota kullanım yüzdesi|Count|Ortalama|Kullanılan Kota yüzdesi|Senaryo, ClusterName, VmFamilyName, VmPriority|
|Başlatılan çalıştırmalar|Evet|Başlatılan çalıştırmalar|Count|Toplam|Bu çalışma alanı için çalıştırılan çalıştırma sayısı. Çalıştırma, gerekli kaynaklar üzerinde çalışmaya başladığında, sayı güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Çalıştırmalar başlatılıyor|Evet|Çalıştırmalar başlatılıyor|Count|Toplam|Bu çalışma alanı için başlatılan çalıştırma sayısı. Sayı, çalıştırma kimliği oluşturma ve çalıştırma kimliği gibi çalışma bilgisi doldurulduktan sonra güncelleştirilir.|Senaryo, RunType, Publishedpipelineıd, ComputeType, PipelineStepType|
|Toplam çekirdek sayısı|Evet|Toplam çekirdek sayısı|Count|Ortalama|Toplam çekirdek sayısı|Senaryo, ClusterName|
|Toplam düğüm sayısı|Evet|Toplam düğüm sayısı|Count|Ortalama|Toplam düğüm sayısı. Bu toplam etkin düğümlerin, boştaki düğümlerin, kullanılamayan düğümlerin, önceden ayrılmış düğümlerin bazılarını içerir, düğümleri bırakır|Senaryo, ClusterName|
|Kullanılamayan çekirdekler|Evet|Kullanılamayan çekirdekler|Count|Ortalama|Kullanılamayan çekirdek sayısı|Senaryo, ClusterName|
|Kullanılamayan düğümler|Evet|Kullanılamayan düğümler|Count|Ortalama|Kullanılamayan düğüm sayısı. Çözümlenebilen bazı sorunlar nedeniyle kullanılamaz düğümler çalışmıyor. Azure bu düğümleri geri dönüştürecek.|Senaryo, ClusterName|
|Uyarılar|Evet|Uyarılar|Count|Toplam|Bu çalışma alanındaki çalışma uyarısı sayısı. Bir çalıştırma bir uyarıyla karşılaştığında, sayı güncellenir.|Senaryo|


## <a name="microsoftmapsaccounts"></a>Microsoft. Maps/hesaplar

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|API 'lerin kullanılabilirliği|ApiCategory, ApiName|
|Kullanım|Hayır|Kullanım|Count|Count|API çağrısı sayısı|ApiCategory, ApiName, ResultType, ResponseCode|


## <a name="microsoftmediamediaservices"></a>Microsoft. Media/mediaservices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AssetCount|Evet|Varlık sayısı|Count|Ortalama|Geçerli medya hizmeti hesabında kaç varlık zaten oluşturuldu|Boyut yok|
|AssetQuota|Evet|Varlık kotası|Count|Ortalama|Geçerli medya hizmeti hesabı için kaç kıymete izin veriliyor?|Boyut yok|
|AssetQuotaUsedPercentage|Evet|Kullanılan varlık kotası yüzdesi|Yüzde|Ortalama|Geçerli medya hizmeti hesabındaki varlık kullanım yüzdesi|Boyut yok|
|ContentKeyPolicyCount|Evet|İçerik anahtarı Ilke sayısı|Count|Ortalama|Geçerli medya hizmeti hesabında kaç tane içerik anahtar ilkesi oluşturuldu|Boyut yok|
|ContentKeyPolicyQuota|Evet|İçerik anahtarı Ilke kotası|Count|Ortalama|Geçerli medya hizmeti hesabı için kaç tane içerik anahtar ilkelerine izin verilir?|Boyut yok|
|ContentKeyPolicyQuotaUsedPercentage|Evet|İçerik anahtarı Ilke kotası kullanılan yüzde|Yüzde|Ortalama|Geçerli medya hizmeti hesabında kullanılan içerik anahtarı Ilkesi yüzdesi|Boyut yok|
|Streammingpolicycount|Evet|Akış Ilkesi sayısı|Count|Ortalama|Geçerli medya hizmeti hesabında kaç tane akış ilkesi zaten oluşturuldu|Boyut yok|
|StreamingPolicyQuota|Evet|Akış Ilkesi kotası|Count|Ortalama|Geçerli medya hizmeti hesabı için kaç akış ilkesine izin verilir?|Boyut yok|
|StreamingPolicyQuotaUsedPercentage|Evet|Akış Ilkesi kotası kullanılan yüzde|Yüzde|Ortalama|Geçerli medya hizmeti hesabındaki akış Ilkesi kullanım yüzdesi|Boyut yok|


## <a name="microsoftmediamediaservicesstreamingendpoints"></a>Microsoft. Media/mediaservices/streamingEndpoints

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Bayt cinsinden çıkış verisi miktarı.|OutputFormat|
|İstekler|Evet|İstekler|Count|Toplam|Akış uç noktası istekleri.|OutputFormat, HttpStatusCode, ErrorCode|
|SuccessE2ELatency|Evet|Başarılı uçtan uca gecikme süresi|Mayacak|Ortalama|Başarılı istekler için milisaniye cinsinden ortalama gecikme süresi.|OutputFormat|


## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft. NetApp/netAppAccounts/Capacityhavuzları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Evet|Havuza ayrılan boyut|Bayt|Ortalama|Bu havuzun boyutu sağlandı|Boyut yok|
|VolumePoolAllocatedUsed|Evet|Birim boyutuna ayrılan havuz|Bayt|Ortalama|Havuzun ayrılan boyutu ayrılmış|Boyut yok|
|VolumePoolTotalLogicalSize|Evet|Kullanılan havuz boyutu|Bayt|Ortalama|Havuza ait tüm birimlerin mantıksal boyutunun toplamı|Boyut yok|
|VolumePoolTotalSnapshotSize|Evet|Havuz için toplam anlık görüntü boyutu|Bayt|Ortalama|Bu havuzdaki tüm birimlerin anlık görüntü boyutu toplamı|Boyut yok|


## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft. NetApp/netAppAccounts/Capacityhavuzları/birimleri

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AverageReadLatency|Evet|Ortalama okuma gecikmesi|Mayacak|Ortalama|İşlem başına milisaniye cinsinden ortalama okuma gecikmesi|Boyut yok|
|AverageWriteLatency|Evet|Ortalama yazma gecikmesi|Mayacak|Ortalama|İşlem başına milisaniye cinsinden ortalama yazma gecikmesi|Boyut yok|
|Cbsvolumebackupactıve|Evet|Birim yedeklemesi etkin durumu|Count|Ortalama|Yedekleme, birim için şu anda askıya alınmış durumda.|Boyut yok|
|CbsVolumeLogicalBackupBytes|Evet|Yedeklenen mantıksal baytlar|Bayt|Ortalama|Toatl sıkıştırılmamış/şifrelenmemiş baytlar bu birim için yedeklendi.|Boyut yok|
|Cbsvolumeoperationtamamlanmıştır|Evet|İşlem durumu|Count|Ortalama|Son yedekleme/geri yükleme işlemi başarılı oldu.|Boyut yok|
|CbsVolumeOperationTransferredBytes|Evet|İşlem için aktarılan bayt sayısı|Bayt|Ortalama|Son yedekleme/geri yükleme işlemi için aktarılan toplam bayt sayısı.|Boyut yok|
|CbsVolumeProtected|Evet|Birim korumalı durum|Count|Ortalama|, Bulut yedekleme hizmeti tarafından korunan birimdir.|Boyut yok|
|ReadIops|Evet|IOPS 'yi oku|Sayaçpersaniye|Ortalama|Saniye başına okuma/kapatma işlemi|Boyut yok|
|Birimayrıtedboyut|Evet|Birim ayırma boyutu|Bayt|Ortalama|Bir birimin sağlanan boyutu|Boyut yok|
|Birimlogicalsize|Evet|Tüketilen birim boyutu|Bayt|Ortalama|Birimin mantıksal boyutu (kullanılan baytlar)|Boyut yok|
|VolumeSnapshotSize|Evet|Birim anlık görüntü boyutu|Bayt|Ortalama|Birimdeki tüm anlık görüntülerin boyutu|Boyut yok|
|Writeıops|Evet|IOPS yaz|Sayaçpersaniye|Ortalama|Saniyede yazma/çıkış işlemi sayısı|Boyut yok|
|Xregionreplicationsağlıklı|Evet|Birim çoğaltma durumu sağlıklı|Count|Ortalama|İlişki koşulu, 1 veya 0.|Boyut yok|
|XregionReplicationLagTime|Evet|Birim çoğaltma gecikme süresi|Saniye|Ortalama|Yansıtmadaki verilerin kaynağın arkasında olduğu sürenin saniye cinsinden miktarı.|Boyut yok|
|XregionReplicationLastTransferDuration|Evet|Birim çoğaltma son aktarım süresi|Saniye|Ortalama|Son aktarımın tamamlanması için geçen saniye cinsinden süre.|Boyut yok|
|XregionReplicationLastTransferSize|Evet|Birim çoğaltma son aktarım boyutu|Bayt|Ortalama|Son aktarımın bir parçası olarak aktarılan toplam bayt sayısı.|Boyut yok|
|XregionReplicationRelationshipProgress|Evet|Birim çoğaltma ilerleme durumu|Bayt|Ortalama|Geçerli aktarım işlemi için aktarılan toplam veri miktarı.|Boyut yok|
|Xregionreplicationrelationshiptransfer|Evet|Birim çoğaltma aktarılıyor|Count|Ortalama|Birim çoğaltmasının durumunun ' aktarma ' olup olmadığı.|Boyut yok|
|XregionReplicationTotalTransferBytes|Evet|Birim çoğaltma toplam aktarımı|Bayt|Ortalama|İlişki için aktarılan birikmeli baytlar.|Boyut yok|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft. Network/Applicationgateway 'ler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ApplicationGatewayTotalTime|Hayır|Application Gateway toplam süre|Mayacak|Ortalama|Bir isteğin işlenmesi için geçen ortalama süre ve gönderilecek yanıt. Bu, yanıt gönderme işlemi tamamlandığında Application Gateway bir HTTP isteğinin ilk baytını aldığında zaman aralığının ortalaması olarak hesaplanır. Bu, genellikle Application Gateway işleme süresini, istek ve Yanıt paketlerinin ağ üzerinden hareket etmekte olduğu süreyi ve arka uç sunucusunun yanıt vermesi gerektiğini unutmayın.|Dinleyici|
|Avgrequestcountperhealthyıhost|Hayır|Sağlıklı ana bilgisayar başına dakika başına istek|Count|Ortalama|Bir havuzda sağlıklı arka uç Konağı başına dakika başına ortalama istek sayısı|BackendSettingsPool|
|BackendConnectTime|Hayır|Arka uç bağlantı saati|Mayacak|Ortalama|Arka uç sunucusuyla bağlantı kurmaya harcanan süre|Dinleyici, BackendServer, BackendPool, BackendHttpSetting|
|BackendFirstByteResponseTime|Hayır|Arka uç Ilk bayt yanıt süresi|Mayacak|Ortalama|Arka uç sunucusuna bağlantı kurma ve yanıt üst bilgisinin ilk baytını alma işlemi arasındaki zaman aralığı, arka uç sunucusunun işlem süresini yaklaştırmaya başladı|Dinleyici, BackendServer, BackendPool, BackendHttpSetting|
|BackendLastByteResponseTime|Hayır|Arka uç son bayt yanıt süresi|Mayacak|Ortalama|Arka uç sunucusuna bağlantı kurma ve yanıt gövdesinin son baytını alma arasındaki zaman aralığı|Dinleyici, BackendServer, BackendPool, BackendHttpSetting|
|BackendResponseStatus|Evet|Arka uç yanıt durumu|Count|Toplam|Arka uç üyeleri tarafından oluşturulan HTTP yanıt kodlarının sayısı. Bu, Application Gateway tarafından oluşturulan yanıt kodlarını içermez.|BackendServer, BackendPool, BackendHttpSetting, HttpStatusGroup|
|BlockedCount|Evet|Web uygulaması güvenlik duvarı engellenen Istek kuralı dağıtımı|Count|Toplam|Web uygulaması güvenlik duvarı engellenen istek kuralı dağıtımı|RuleGroup, RuleId|
|BlockedReqCount|Evet|Web uygulaması güvenlik duvarı engellenen Istek sayısı|Count|Toplam|Web uygulaması güvenlik duvarı engellenen istek sayısı|Boyut yok|
|BytesReceived|Evet|Alınan bayt|Bayt|Toplam|İstemcilerden Application Gateway tarafından alınan toplam bayt sayısı|Dinleyici|
|BytesSent|Evet|Gönderilen bayt|Bayt|Toplam|Application Gateway tarafından istemcilere gönderilen toplam bayt sayısı|Dinleyici|
|CapacityUnits|Hayır|Geçerli kapasite birimleri|Count|Ortalama|Tüketilen Kapasite birimleri|Boyut yok|
|ClientRtt|Hayır|İstemci RTT|Mayacak|Ortalama|İstemcilerle Application Gateway arasında ortalama gidiş dönüş süresi. Bu ölçüm, bağlantı kurmak ve bildirimlerin döndürülmesi için ne kadar sürdüğünü gösterir|Dinleyici|
|ComputeUnits|Hayır|Geçerli Işlem birimleri|Count|Ortalama|Tüketilen işlem birimleri|Boyut yok|
|Cpukullanımı|Hayır|CPU Kullanımı|Yüzde|Ortalama|Application Gateway geçerli CPU kullanımı|Boyut yok|
|CurrentConnections|Evet|Geçerli bağlantılar|Count|Toplam|Application Gateway ile kurulan geçerli bağlantı sayısı|Boyut yok|
|Estimatedbilledcapacitybirimleri|Hayır|Tahmini faturalandırılan Kapasite birimleri|Count|Ortalama|Ücretlendirilebilecek tahmini Kapasite birimleri|Boyut yok|
|FailedRequests|Evet|Başarısız İstekler|Count|Toplam|Application Gateway tarafından sunulan başarısız istek sayısı|BackendSettingsPool|
|Fixedbillablecapacitybirimleri|Hayır|Sabit Faturalanabilir Kapasite Birimleri|Count|Ortalama|Ücretlendirilebilecek minimum kapasite birimleri|Boyut yok|
|Healthyıhostcount|Evet|Sağlıklı konak sayısı|Count|Ortalama|Sağlıklı arka uç Konakları sayısı|BackendSettingsPool|
|MatchedCount|Evet|Web uygulaması güvenlik duvarı toplam kural dağıtımı|Count|Toplam|Gelen trafik için Web uygulaması güvenlik duvarı toplam kural dağıtımı|RuleGroup, RuleId|
|NewConnectionsPerSecond|Hayır|Saniyedeki yeni bağlantı sayısı|Sayaçpersaniye|Ortalama|Saniye başına yeni bağlantı Application Gateway ile oluşturuldu|Boyut yok|
|ResponseStatus|Evet|Yanıt durumu|Count|Toplam|Application Gateway tarafından döndürülen http yanıt durumu|HttpStatusGroup|
|Aktarım hızı|Hayır|Aktarım hızı|BytesPerSecond|Ortalama|Application Gateway saniyede hizmet verilen bayt sayısı|Boyut yok|
|TlsProtocol|Evet|İstemci TLS protokolü|Count|Toplam|Application Gateway ile bağlantı kurulan istemci tarafından başlatılan TLS ve TLS olmayan isteklerin sayısı. TLS protokol dağıtımını görüntülemek için Dimension TLS protokolüne göre filtreleyin.|Dinleyici, TlsProtocol|
|TotalRequests|Evet|Toplam İstek Sayısı|Count|Toplam|Application Gateway tarafından sunulan başarılı istek sayısı|BackendSettingsPool|
|Unhealthyıhostcount|Evet|Sağlıksız konak sayısı|Count|Ortalama|Sağlıklı olmayan arka uç Konakları sayısı|BackendSettingsPool|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft. Network/azurefirewalls

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ApplicationRuleHit|Evet|Uygulama kuralları isabet sayısı|Count|Toplam|Uygulama kurallarının isabet sayısı|Durum, neden, protokol|
|Veri Işlendi|Evet|İşlenen veri|Bayt|Toplam|Bu güvenlik duvarı tarafından işlenen toplam veri miktarı|Boyut yok|
|FirewallHealth|Evet|Güvenlik duvarı sistem durumu|Yüzde|Ortalama|Bu güvenlik duvarının genel durumunu gösterir|Durum, neden|
|NetworkRuleHit|Evet|Ağ kuralları isabet sayısı|Count|Toplam|Ağ kurallarının isabet sayısı|Durum, neden, protokol|
|SNATPortUtilization|Evet|SNAT bağlantı noktası kullanımı|Yüzde|Ortalama|Şu anda kullanımda olan giden SNAT bağlantı noktalarının yüzdesi|Protokol|
|Aktarım hızı|Hayır|Aktarım hızı|BitsPerSecond|Ortalama|Bu güvenlik duvarı tarafından işlenen işleme|Boyut yok|


## <a name="microsoftnetworkconnections"></a>Microsoft. Network/Connections

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|BitsInPerSecond|Evet|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit girişi|Boyut yok|
|BitsOutPerSecond|Evet|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit çıkışı|Boyut yok|


## <a name="microsoftnetworkdnszones"></a>Microsoft. Network/dnszones

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|QueryVolume|Evet|Sorgu hacmi|Count|Toplam|Bir DNS bölgesi için sunulan sorgu sayısı|Boyut yok|
|Recordsetkapaıyutilileştirme|Hayır|Kayıt kümesi kapasite kullanımı|Yüzde|Maksimum|Bir DNS bölgesi tarafından kullanılan kayıt kümesi kapasitesinin yüzdesi|Boyut yok|
|Kayıt kümesi sayısı|Evet|Kayıt kümesi sayısı|Count|Maksimum|Bir DNS bölgesindeki kayıt kümesi sayısı|Boyut yok|


## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft. Network/Expressroutedevreleri

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ArpAvailability|Evet|ARP kullanılabilirliği|Yüzde|Ortalama|Tüm eşleri doğrultusunda MSEE 'tan ARP kullanılabilirliği.|PeeringType, eş|
|BgpAvailability|Evet|BGP kullanılabilirliği|Yüzde|Ortalama|Tüm Peers yönelik MSEE BGP kullanılabilirliği.|PeeringType, eş|
|BitsInPerSecond|Hayır|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit girişi|PeeringType|
|BitsOutPerSecond|Hayır|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit çıkışı|PeeringType|
|GlobalReachBitsInPerSecond|Hayır|GlobalReachBitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit girişi|Peereddevresi Skey|
|GlobalReachBitsOutPerSecond|Hayır|GlobalReachBitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit çıkışı|Peereddevresi Skey|
|QosDropBitsInPerSecond|Hayır|DroppedInBitsPerSecond|Sayaçpersaniye|Ortalama|Saniye başına bırakılan veri girişi bitleri|Boyut yok|
|QosDropBitsOutPerSecond|Hayır|DroppedOutBitsPerSecond|Sayaçpersaniye|Ortalama|Saniye başına bırakılan veri çıkış bitleri|Boyut yok|


## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft. Network/Expressroutedevreleri/eşayarları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|BitsInPerSecond|Evet|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit girişi|Boyut yok|
|BitsOutPerSecond|Evet|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit çıkışı|Boyut yok|


## <a name="microsoftnetworkexpressroutegateways"></a>Microsoft. Network/Expressroutegateway 'ler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ErGatewayConnectionBitsInPerSecond|Hayır|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit girişi|ConnectionName|
|ErGatewayConnectionBitsOutPerSecond|Hayır|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit çıkışı|ConnectionName|


## <a name="microsoftnetworkexpressrouteports"></a>Microsoft. Network/expressRoutePorts

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AdminState|Evet|AdminState|Count|Ortalama|Bağlantı noktasının yönetici durumu|Bağlantı|
|LineProtocol|Evet|LineProtocol|Count|Ortalama|Bağlantı noktasının hat protokol durumu|Bağlantı|
|PortBitsInPerSecond|Evet|BitsInPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit girişi|Bağlantı|
|PortBitsOutPerSecond|Evet|BitsOutPerSecond|Sayaçpersaniye|Ortalama|Saniye başına Azure’da bit çıkışı|Bağlantı|
|RxLightLevel|Evet|RxLightLevel|Count|Ortalama|DBm 'de RX hafif düzeyi|Bağlantı, kulvar|
|TxLightLevel|Evet|TxLightLevel|Count|Ortalama|DBm 'de TX hafif düzeyi|Bağlantı, kulvar|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft. Network/frontkapaklı

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|BackendHealthPercentage|Evet|Arka uç sistem durumu yüzdesi|Yüzde|Ortalama|HTTP/S proxy 'sinden arka uçlara başarılı sistem durumu araştırmalarının yüzdesi|Arka uç, BackendPool|
|BackendRequestCount|Evet|Arka uç Istek sayısı|Count|Toplam|HTTP/S proxy 'sinden arka uçlara gönderilen isteklerin sayısı|HttpStatus, HttpStatusGroup, arka uç|
|BackendRequestLatency|Evet|Arka uç Isteği gecikmesi|Mayacak|Ortalama|HTTP/s proxy, arka ucun son yanıt baytını almayana kadar isteğin HTTP/S proxy tarafından arka uca gönderildiği süre|Arka uç|
|Billilableresponsesize|Evet|Faturalanabilir yanıt boyutu|Bayt|Toplam|HTTP/S proxy 'sinden istemcilere yanıt olarak gönderilen faturalandırılabilir bayt sayısı (istek başına en az 2 KB).|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|Istek sayısı|Evet|İstek Sayısı|Count|Toplam|HTTP/S proxy tarafından hizmet verilen istemci isteklerinin sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Evet|İstek boyutu|Bayt|Toplam|İstemcilerden HTTP/S proxy 'sine istek olarak gönderilen bayt sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|Yanıt boyutu|Evet|Yanıt boyutu|Bayt|Toplam|HTTP/S proxy 'sinden istemcilere yanıt olarak gönderilen baytların sayısı|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|TotalLatency|Evet|Toplam gecikme süresi|Mayacak|Ortalama|İstemci, http/s proxy 'sinden gelen son yanıt baytını kabul edene kadar, istemci isteğinin HTTP/S proxy tarafından alındığı zamana göre hesaplanır|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|WebApplicationFirewallRequestCount|Evet|Web uygulaması güvenlik duvarı Istek sayısı|Count|Toplam|Web uygulaması güvenlik duvarı tarafından işlenen istemci isteklerinin sayısı|PolicyName, RuleName, eylem|


## <a name="microsoftnetworkloadbalancers"></a>Microsoft. Network/loadBalancers

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AllocatedSnatPorts|Hayır|Ayrılan SNAT bağlantı noktaları|Count|Ortalama|Zaman aralığı içinde ayrılan toplam SNAT bağlantı noktası sayısı|Frontendıbaddress, Backendıaddress, ProtocolType, |
|ByteCount|Evet|Bayt sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam bayt sayısı|Frontendıaaddress, FrontendPort, Yön|
|DipAvailability|Evet|Durum Yoklaması Durumu|Count|Ortalama|Süre başına ortalama Load Balancer sistem durumu araştırma durumu|ProtocolType, BackendPort, Frontendıbaddress, FrontendPort, Backendıdıaddress|
|PacketCount|Evet|Paket Sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam paket sayısı|Frontendıaaddress, FrontendPort, Yön|
|SnatConnectionCount|Evet|SNAT bağlantı sayısı|Count|Toplam|Zaman aralığı içinde oluşturulan toplam yeni SNAT bağlantısı sayısı|Frontendıbaddress, Backendıo adresi, ConnectionState|
|SYNCount|Evet|SYN sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam SYN paketi sayısı|Frontendıaaddress, FrontendPort, Yön|
|UsedSnatPorts|Hayır|Kullanılan SNAT bağlantı noktaları|Count|Ortalama|Zaman aralığı içinde kullanılan SNAT bağlantı noktalarının toplam sayısı|Frontendıbaddress, Backendıaddress, ProtocolType, |
|VipAvailability|Evet|Veri Yolu Kullanılabilirliği|Count|Ortalama|Süre başına ortalama Load Balancer veri yolu kullanılabilirliği|Frontendıaddress, FrontendPort|


## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft. Network/NetworkInterfaces

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|BytesReceivedRate|Evet|Alınan bayt|Bayt|Toplam|Ağ arabiriminin aldığı bayt sayısı|Boyut yok|
|BytesSentRate|Evet|Gönderilen bayt|Bayt|Toplam|Ağ arabiriminin gönderdiği bayt sayısı|Boyut yok|
|PacketsReceivedRate|Evet|Alınan paketler|Count|Toplam|Ağ arabiriminin aldığı paket sayısı|Boyut yok|
|PacketsSentRate|Evet|Gönderilen paketler|Count|Toplam|Ağ arabiriminin gönderdiği paket sayısı|Boyut yok|


## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft. Network/networkWatchers/Connectionmonitörleri

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AverageRoundtripMs|Evet|Ort. gidiş dönüş süresi (MS)|Mayacak|Ortalama|Kaynak ve hedef arasında gönderilen bağlantı izleme araştırmaları için Ortalama ağ gidiş dönüş süresi (MS)|Boyut yok|
|ChecksFailedPercent|Evet|Başarısız denetimler yüzdesi (Önizleme)|Yüzde|Ortalama|bağlantı izleme denetimlerinin yüzdesi başarısız oldu|SourceAddress, SourceName, Sourceresourceıd, SourceType, protokol, DestinationAddress, DestinationName, Destinationresourceıd, DestinationType, DestinationPort, TestGroupName, TestConfigurationName|
|ProbesFailedPercent|Evet|% Yoklama başarısız oldu|Yüzde|Ortalama|Bağlantı İzleme Araştırmaları yüzdesi başarısız oldu|Boyut yok|
|Roundroundtimems|Evet|Gidiş dönüş süresi (MS) (Önizleme)|Mayacak|Ortalama|Bağlantı izleme denetimleri için milisaniye cinsinden gidiş dönüş süresi|SourceAddress, SourceName, Sourceresourceıd, SourceType, protokol, DestinationAddress, DestinationName, Destinationresourceıd, DestinationType, DestinationPort, TestGroupName, TestConfigurationName|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft. Network/Publicıpaddresses

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ByteCount|Evet|Bayt sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam bayt sayısı|Bağlantı noktası, Yön|
|BytesDroppedDDoS|Evet|Gelen bayt bırakıldı DDoS|BytesPerSecond|Maksimum|Gelen bayt bırakıldı DDoS|Boyut yok|
|BytesForwardedDDoS|Evet|İletilen gelen bayt sayısı|BytesPerSecond|Maksimum|İletilen gelen bayt sayısı|Boyut yok|
|BytesInDDoS|Evet|Gelen bayt sayısı DDoS|BytesPerSecond|Maksimum|Gelen bayt sayısı DDoS|Boyut yok|
|Ddobir Ggersynpackets|Evet|DDoS azaltma tetiklenmesi için gelen SYN paketleri|Sayaçpersaniye|Maksimum|DDoS azaltma tetiklenmesi için gelen SYN paketleri|Boyut yok|
|DDO, Ggertcppackets|Evet|DDoS azaltma tetiklemeye yönelik gelen TCP paketleri|Sayaçpersaniye|Maksimum|DDoS azaltma tetiklemeye yönelik gelen TCP paketleri|Boyut yok|
|DDO, Ggerudppaketleri|Evet|DDoS risk azaltma tetiklenecek gelen UDP paketleri|Sayaçpersaniye|Maksimum|DDoS risk azaltma tetiklenecek gelen UDP paketleri|Boyut yok|
|Iunderddosattack|Evet|DDoS saldırısı altında|Count|Maksimum|DDoS saldırısı altında|Boyut yok|
|PacketCount|Evet|Paket Sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam paket sayısı|Bağlantı noktası, Yön|
|PacketsDroppedDDoS|Evet|DDoS bırakılan gelen paketler|Sayaçpersaniye|Maksimum|DDoS bırakılan gelen paketler|Boyut yok|
|PacketsForwardedDDoS|Evet|DDoS iletilen gelen paketler|Sayaçpersaniye|Maksimum|DDoS iletilen gelen paketler|Boyut yok|
|PacketsInDDoS|Evet|Gelen paketler DDoS|Sayaçpersaniye|Maksimum|Gelen paketler DDoS|Boyut yok|
|SynCount|Evet|SYN sayısı|Count|Toplam|Zaman aralığı içinde aktarılan toplam SYN paketi sayısı|Bağlantı noktası, Yön|
|TCPBytesDroppedDDoS|Evet|Gelen TCP bayt bırakılan DDoS|BytesPerSecond|Maksimum|Gelen TCP bayt bırakılan DDoS|Boyut yok|
|TCPBytesForwardedDDoS|Evet|İletilen gelen TCP baytları|BytesPerSecond|Maksimum|İletilen gelen TCP baytları|Boyut yok|
|TCPBytesInDDoS|Evet|Gelen TCP bayt DDoS|BytesPerSecond|Maksimum|Gelen TCP bayt DDoS|Boyut yok|
|TCPPacketsDroppedDDoS|Evet|Gelen TCP paketleri bırakıldı DDoS|Sayaçpersaniye|Maksimum|Gelen TCP paketleri bırakıldı DDoS|Boyut yok|
|TCPPacketsForwardedDDoS|Evet|DDoS iletilen gelen TCP paketleri|Sayaçpersaniye|Maksimum|DDoS iletilen gelen TCP paketleri|Boyut yok|
|TCPPacketsInDDoS|Evet|Gelen TCP paketleri DDoS|Sayaçpersaniye|Maksimum|Gelen TCP paketleri DDoS|Boyut yok|
|UDPBytesDroppedDDoS|Evet|Gelen UDP bayt bırakılan DDoS|BytesPerSecond|Maksimum|Gelen UDP bayt bırakılan DDoS|Boyut yok|
|UDPBytesForwardedDDoS|Evet|İletilen gelen UDP baytları|BytesPerSecond|Maksimum|İletilen gelen UDP baytları|Boyut yok|
|UDPBytesInDDoS|Evet|Gelen UDP bayt sayısı DDoS|BytesPerSecond|Maksimum|Gelen UDP bayt sayısı DDoS|Boyut yok|
|UDPPacketsDroppedDDoS|Evet|Gelen UDP paketleri bırakıldı|Sayaçpersaniye|Maksimum|Gelen UDP paketleri bırakıldı|Boyut yok|
|UDPPacketsForwardedDDoS|Evet|DDoS iletilen gelen UDP paketleri|Sayaçpersaniye|Maksimum|DDoS iletilen gelen UDP paketleri|Boyut yok|
|UDPPacketsInDDoS|Evet|Gelen UDP paketleri DDoS|Sayaçpersaniye|Maksimum|Gelen UDP paketleri DDoS|Boyut yok|
|VipAvailability|Evet|Veri Yolu Kullanılabilirliği|Count|Ortalama|Süre başına ortalama IP adresi kullanılabilirliği|Bağlantı noktası|


## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft. Network/trafficManagerProfiles

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Probeagentcurrentendpointstatebyprofileresourceıd|Evet|Uç nokta tarafından uç nokta durumu|Count|Maksimum|1 bir uç noktanın araştırma durumu "etkin" ise, 0 yoksa.|Uçnoktaadı|
|QpsByEndpoint|Evet|Bitiş noktasına göre sorgular döndürüldü|Count|Toplam|Verilen zaman çerçevesinde Traffic Manager uç noktasının döndürüldüğü sayı|Uçnoktaadı|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft. Network/Virtualnetworkgateway 'ler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AverageBandwidth|Evet|Ağ Geçidi S2S bant genişliği|BytesPerSecond|Ortalama|Bayt/saniye cinsinden bir ağ geçidinin ortalama siteden siteye bant genişliği|Boyut yok|
|P2SBandwidth|Evet|Ağ Geçidi P2S bant genişliği|BytesPerSecond|Ortalama|Bayt/saniye cinsinden bir ağ geçidinin ortalama Noktadan siteye bant genişliği|Boyut yok|
|P2SConnectionCount|Evet|P2S Bağlantı Sayısı|Count|Maksimum|Bir ağ geçidinin noktadan siteye bağlantı sayısı|Protokol|
|TunnelAverageBandwidth|Evet|Tünel Bant Genişliği|BytesPerSecond|Ortalama|Bir tünelin bayt/saniye cinsinden ortalama bant genişliği|ConnectionName, Remoteıp|
|TunnelEgressBytes|Evet|Tünel Çıkış Baytları|Bayt|Toplam|Bir tünelin giden bayt sayısı|ConnectionName, Remoteıp|
|TunnelEgressPacketDropTSMismatch|Evet|Tünel Çıkış TS Uyumsuzluğu Paket Bırakma|Count|Toplam|Bir tünelin trafik seçiciyle eşleşmeyen giden paket bırakma sayısı|ConnectionName, Remoteıp|
|TunnelEgressPackets|Evet|Tünel Çıkış Paketleri|Count|Toplam|Bir tünelin giden paket sayısı|ConnectionName, Remoteıp|
|TunnelIngressBytes|Evet|Tünel Giriş Baytları|Bayt|Toplam|Bir tünelin gelen bayt sayısı|ConnectionName, Remoteıp|
|TunnelIngressPacketDropTSMismatch|Evet|Tünel Giriş TS Uyumsuzluğu Paket Bırakma|Count|Toplam|Bir tünelin trafik seçiciyle eşleşmeyen gelen paket bırakma sayısı|ConnectionName, Remoteıp|
|TunnelIngressPackets|Evet|Tünel giriş paketleri|Count|Toplam|Bir tünelin gelen paket sayısı|ConnectionName, Remoteıp|


## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft. Network/virtualNetworks

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|PingMeshAverageRoundtripMs|Evet|Bir VM 'ye ping için gidiş dönüş süresi|Mayacak|Ortalama|Hedef VM 'ye gönderilen pingler için gidiş dönüş süresi|SourceCustomerAddress, DestinationCustomerAddress|
|PingMeshProbesFailedPercent|Evet|VM 'ye ping işlemi başarısız oldu|Yüzde|Ortalama|Hedef VM 'nin gönderilen toplam ping Pinglerine yönelik başarısız Ping sayısı yüzdesi|SourceCustomerAddress, DestinationCustomerAddress|


## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|gelen|Evet|Gelen İletiler|Count|Toplam|Başarılı bir gönderme API çağrısı sayısı. |Boyut yok|
|gelen. tüm. failedistekleri|Evet|Tüm gelen başarısız Istekler|Count|Toplam|Bir Bildirim Hub 'ı için toplam gelen başarısız istek sayısı|Boyut yok|
|gelen. ALL. Requests|Evet|Tüm gelen Istekler|Count|Toplam|Bir Bildirim Hub 'ı için toplam gelen istek sayısı|Boyut yok|
|gelen. zamanlandı|Evet|Zamanlanan anında Iletme bildirimleri gönderildi|Count|Toplam|Zamanlanan anında Iletme bildirimleri Iptal edildi|Boyut yok|
|gelen. zamanlandı. iptal|Evet|Zamanlanan anında Iletme bildirimleri Iptal edildi|Count|Toplam|Zamanlanan anında Iletme bildirimleri Iptal edildi|Boyut yok|
|yükleme. tümü|Evet|Yükleme yönetimi Işlemleri|Count|Toplam|Yükleme yönetimi Işlemleri|Boyut yok|
|yükleme. Delete|Evet|Yükleme Işlemlerini silme|Count|Toplam|Yükleme Işlemlerini silme|Boyut yok|
|yükleme. Get|Evet|Yükleme Işlemlerini al|Count|Toplam|Yükleme Işlemlerini al|Boyut yok|
|yükleme. Patch|Evet|Düzeltme eki yükleme Işlemleri|Count|Toplam|Düzeltme eki yükleme Işlemleri|Boyut yok|
|yükleme. upsert|Evet|Yükleme Işlemlerini oluşturma veya güncelleştirme|Count|Toplam|Yükleme Işlemlerini oluşturma veya güncelleştirme|Boyut yok|
|notificationhub. gönderimleri|Evet|Tüm giden bildirimler|Count|Toplam|Bildirim Hub 'ının tüm giden bildirimleri|Boyut yok|
|giden. allpns. badorexpiredchannel|Evet|Hatalı veya süre dolma kanalı hataları|Count|Toplam|Kayıttaki kanal/belirteç/RegistrationId 'nin geçerliliği aşıldığı veya geçersiz olduğu için başarısız olan gönderim sayısı.|Boyut yok|
|giden. allpns. channelerror|Evet|Kanal hataları|Count|Toplam|Kanal geçersiz olduğu için doğru uygulamayla ilişkili olmayan veya geçerliliği aşıldığı için başarısız olan gönderim sayısı.|Boyut yok|
|giden. allpns. ınvalidpayload|Evet|Yük hataları|Count|Toplam|PNS hatalı yük hatası döndürdüğünden başarısız olan gönderim sayısı.|Boyut yok|
|giden. allpns. pnserror|Evet|Dış bildirim sistemi hataları|Count|Toplam|PNS ile iletişim kurulurken bir sorun oluştuğundan başarısız olan gönderim sayısı (kimlik doğrulama sorunları hariç).|Boyut yok|
|giden. allpns. Success|Evet|Başarılı bildirimler|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. APNs. badchannel|Evet|APNS geçersiz kanal hatası|Count|Toplam|Belirteç geçersiz olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 8).|Boyut yok|
|giden. APNs. expiredchannel|Evet|APNS süre dolma kanalı hatası|Count|Toplam|APNS geri bildirim kanalı tarafından geçersiz kılınan belirteç sayısı.|Boyut yok|
|giden. APNs. ınvalidcredentials|Evet|APNS yetkilendirme hataları|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Boyut yok|
|giden. APNs. ınvalidnotificationsize|Evet|APNS geçersiz bildirim boyutu hatası|Count|Toplam|Yük çok büyük olduğu için başarısız olan gönderim sayısı (APNS durum kodu: 7).|Boyut yok|
|giden. APNs. pnserror|Evet|APNS hataları|Count|Toplam|APNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|giden. APNs. Success|Evet|APNS başarılı bildirimler|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. gcm. authenticationerror|Evet|GCM kimlik doğrulama hataları|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden başarısız olan gönderim sayısı, kimlik bilgilerinin engellenmesi veya SenderId uygulamada doğru yapılandırılmamış (GCM sonucu: Mismatchedsenderıd).|Boyut yok|
|giden. gcm. badchannel|Evet|GCM hatalı kanal hatası|Count|Toplam|Kayıttaki RegistrationId tanınmadığı için başarısız olan gönderim sayısı (GCM sonucu: geçersiz kayıt).|Boyut yok|
|giden. gcm. expiredchannel|Evet|GCM süre aşımına uğradı kanal hatası|Count|Toplam|Kayıttaki RegistrationId 'nin zaman aşımına uğradığından başarısız olan gönderim sayısı (GCM sonucu: NotRegistered).|Boyut yok|
|giden. gcm. ınvalidcredentials|Evet|GCM yetkilendirme hataları (geçersiz kimlik bilgileri)|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Boyut yok|
|giden. gcm. ınvalidnotificationformat|Evet|GCM geçersiz bildirim biçimi|Count|Toplam|Yük doğru biçimlendirilmediği için başarısız olan gönderim sayısı (GCM sonucu: ınvaliddatakey veya ınvalidttl).|Boyut yok|
|giden. gcm. ınvalidnotificationsize|Evet|GCM geçersiz bildirim boyutu hatası|Count|Toplam|Yük çok büyük olduğu için başarısız olan gönderim sayısı (GCM sonucu: Messagetoobıg).|Boyut yok|
|giden. gcm. pnserror|Evet|GCM hataları|Count|Toplam|GCM ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|giden. gcm. Success|Evet|GCM başarılı bildirimleri|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. gcm. kısıtlanıyor|Evet|GCM kısıtlanan bildirimler|Count|Toplam|GCM tarafından kısıtlanan bu uygulamayı (GCM durum kodu: 501-599 veya sonuç: kullanılamıyor) nedeniyle başarısız olan gönderim sayısı.|Boyut yok|
|giden. gcm. yanlışlıkla gchannel|Evet|GCM yanlış kanal hatası|Count|Toplam|Kayıttaki RegistrationId geçerli uygulamayla ilişkili olmadığından başarısız olan gönderim sayısı (GCM sonucu: ınvalidpackagename).|Boyut yok|
|giden. MPNS. authenticationerror|Evet|MPNS kimlik doğrulama hataları|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Boyut yok|
|giden. MPNS. badchannel|Evet|MPNS hatalı kanal hatası|Count|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (MPNS durumu: 404 bulunamadı).|Boyut yok|
|giden. MPNS. channelconnected bağlantısı kesildi|Evet|MPNS kanalının bağlantısı kesildi|Count|Toplam|Kayıttaki Channelurı bağlantısı kesildiğinden başarısız olan gönderim sayısı (MPNS durumu: 412 bulunamadı).|Boyut yok|
|giden. MPNS. bırakıldı|Evet|MPNS bırakılan bildirimler|Count|Toplam|MPNS tarafından bırakılan gönderim sayısı (MPNS yanıt üst bilgisi: X-NotificationStatus: QueueFull veya gizlendi).|Boyut yok|
|giden. MPNS. ınvalidcredentials|Evet|MPNS geçersiz kimlik bilgileri|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı.|Boyut yok|
|giden. MPNS. ınvalidnotificationformat|Evet|MPNS geçersiz bildirim biçimi|Count|Toplam|Bildirimin yükü çok büyük olduğu için başarısız olan gönderim sayısı.|Boyut yok|
|giden. MPNS. pnserror|Evet|MPNS hataları|Count|Toplam|MPNS ile iletişim kurulurken oluşan hatalardan dolayı başarısız olan gönderim sayısı.|Boyut yok|
|giden. MPNS. Success|Evet|MPNS başarılı bildirimleri|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. MPNS. kısıtlanıyor|Evet|MPNS kısıtlanan bildirimler|Count|Toplam|MPNS bu uygulamayı azaltdığı için başarısız olan gönderim sayısı (WNS MPNS: 406 kabul edilemez).|Boyut yok|
|giden. WNS. authenticationerror|Evet|WNS kimlik doğrulama hataları|Count|Toplam|Windows Live geçersiz kimlik bilgileri veya yanlış belirteçle iletişim kurarken oluşan hatalar nedeniyle bildirim teslim edilemiyor.|Boyut yok|
|giden. WNS. badchannel|Evet|WNS hatalı kanal hatası|Count|Toplam|Kayıttaki Channelurı tanınmadığı için başarısız olan gönderim sayısı (WNS durumu: 404 bulunamadı).|Boyut yok|
|giden. WNS. channelconnected bağlantısı kesildi|Evet|WNS kanal bağlantısı kesildi|Count|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (WNS yanıt üst bilgisi: X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Boyut yok|
|giden. WNS. channelkısıtlanıyor|Evet|WNS kanal kısıtlandı|Count|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (WNS yanıt üst bilgisi: X-WNS-NotificationStatus: Channelkısıtlanıyor).|Boyut yok|
|giden. WNS. bırakıldı|Evet|WNS bırakılan bildirimler|Count|Toplam|Kayıttaki Channelurı kısıtlandığı için bildirim bırakıldı (X-WNS-NotificationStatus: bırakıldı ancak X-WNS-DeviceConnectionStatus: bağlantısı kesildi).|Boyut yok|
|giden. WNS. expiredchannel|Evet|WNS süre dolduğunda kanal hatası|Count|Toplam|ChannelURI 'nin geçerliliği aşıldığı için başarısız olan gönderim sayısı (WNS durumu: 410 gitti).|Boyut yok|
|giden. WNS. ınvalidcredentials|Evet|WNS yetkilendirme hataları (geçersiz kimlik bilgileri)|Count|Toplam|PNS belirtilen kimlik bilgilerini kabul etmediğinden veya kimlik bilgileri engellendiğinden başarısız olan gönderim sayısı. (Windows Live kimlik bilgilerini tanımaz).|Boyut yok|
|giden. WNS. ınvalidnotificationformat|Evet|WNS geçersiz bildirim biçimi|Count|Toplam|Bildirimin biçimi geçersiz (WNS durumu: 400). WNS 'nin tüm geçersiz yükleri reddetmediğini unutmayın.|Boyut yok|
|giden. WNS. ınvalidnotificationsize|Evet|WNS geçersiz bildirim boyutu hatası|Count|Toplam|Bildirim yükü çok büyük (WNS durumu: 413).|Boyut yok|
|giden. WNS. ınvalidtoken|Evet|WNS yetkilendirme hataları (geçersiz belirteç)|Count|Toplam|WNS öğesine sunulan belirteç geçerli değil (WNS Status: 401 Yetkisiz).|Boyut yok|
|giden. WNS. pnserror|Evet|WNS hataları|Count|Toplam|Bildirim, WNS ile iletişim kuran hatalar nedeniyle teslim edilemiyor.|Boyut yok|
|giden. WNS. Success|Evet|WNS başarılı bildirimleri|Count|Toplam|Tüm başarılı bildirimlerin sayısı.|Boyut yok|
|giden. WNS. kısıtlanıyor|Evet|WNS kısıtlanıyor bildirimleri|Count|Toplam|WNS bu uygulamayı azaltdığı için başarısız olan gönderim sayısı (WNS durumu: 406 kabul edilemez).|Boyut yok|
|giden. WNS. tokenproviderunreachable|Evet|WNS yetkilendirme hataları (ulaşılamıyor)|Count|Toplam|Windows Live ulaşılamıyor.|Boyut yok|
|giden. WNS. yanlışlıkla gtoken|Evet|WNS yetkilendirme hataları (yanlış belirteç)|Count|Toplam|WNS öğesine sunulan belirteç geçerli, ancak başka bir uygulama için (WNS durumu: 403 Yasak). Kayıttaki Channelurı başka bir uygulamayla ilişkiliyse bu durum oluşabilir. İstemci uygulamasının kimlik bilgileri Bildirim Hub 'ında olan aynı uygulamayla ilişkilendirildiğinden emin olun.|Boyut yok|
|kayıt. tümü|Evet|Kayıt Işlemleri|Count|Toplam|Tüm başarılı kayıt işlemlerinin sayısı (güncelleştirme sorguları ve silmeleri oluşturma). |Boyut yok|
|kayıt. oluştur|Evet|Kayıt oluşturma Işlemleri|Count|Toplam|Tüm başarılı kayıt oluşturma işlemlerinin sayısı.|Boyut yok|
|kayıt. Sil|Evet|Kayıt silme Işlemleri|Count|Toplam|Tüm başarılı kayıt silme işlemlerinin sayısı.|Boyut yok|
|kayıt. Get|Evet|Kayıt okuma Işlemleri|Count|Toplam|Tüm başarılı kayıt sorgularının sayısı.|Boyut yok|
|kayıt. güncelleştirme|Evet|Kayıt güncelleştirme Işlemleri|Count|Toplam|Tüm başarılı kayıt güncelleştirmelerinin sayısı.|Boyut yok|
|zamanlandı. bekliyor|Evet|Bekleyen zamanlanmış bildirimler|Count|Toplam|Bekleyen zamanlanmış bildirimler|Boyut yok|


## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft. Operationalınsights/çalışma alanları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilir% Average_ bellek|Evet|% Kullanılabilir bellek|Count|Ortalama|Kullanılabilir% Average_ bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılabilir takas alanı|Evet|% Kullanılabilir takas alanı|Count|Ortalama|Average_% kullanılabilir takas alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|% Average_ kaydedilmiş bayt kullanımda|Evet|Kullanılan kaydedilmiş bayt yüzdesi|Count|Ortalama|% Average_ kaydedilmiş bayt kullanımda|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% DPC Zamanı|Evet|% DPC Zamanı|Count|Ortalama|Average_% DPC Zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% boş ınomdes|Evet|% Boş ınomdes|Count|Ortalama|Average_% boş ınomdes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|% Average_ boş alan|Evet|% Boş alan|Count|Ortalama|% Average_ boş alan|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% boşta kalma süresi|Evet|% Boşta kalma süresi|Count|Ortalama|Average_% boşta kalma süresi|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Kesme zamanı Average_%|Evet|Kesme zamanı yüzdesi|Count|Ortalama|Kesme zamanı Average_%|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% GÇ bekleme süresi|Evet|% GÇ bekleme süresi|Count|Ortalama|Average_% GÇ bekleme süresi|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Iyi zaman|Evet|% İyi zaman|Count|Ortalama|Average_% Iyi zaman|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% ayrıcalıklı zaman|Evet|% Ayrıcalıklı zaman|Count|Ortalama|Average_% ayrıcalıklı zaman|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Işlemci zamanı|Evet|% İşlemci zamanı|Count|Ortalama|Average_% Işlemci zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılan ınomdes|Evet|% Kullanılan ınomdes|Count|Ortalama|Average_% kullanılan ınomdes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Kullanılan% Average_ bellek|Evet|% Kullanılan bellek|Count|Ortalama|Kullanılan% Average_ bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Kullanılan Average_ alan yüzdesi|Evet|% Kullanılan alan|Count|Ortalama|Kullanılan Average_ alan yüzdesi|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% kullanılan takas alanı|Evet|% Kullanılan takas alanı|Count|Ortalama|Average_% kullanılan takas alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_% Kullanıcı Zamanı|Evet|% Kullanıcı Zamanı|Count|Ortalama|Average_% Kullanıcı Zamanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt|Evet|Kullanılabilir MBayt|Count|Ortalama|Average_Available MBayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt bellek|Evet|Kullanılabilir MBayt belleği|Count|Ortalama|Average_Available MBayt bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Available MBayt takas|Evet|Kullanılabilir MBayt takas|Count|Ortalama|Average_Available MBayt takas|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Okuma|Evet|Ortalama Disk sn/okuma|Count|Ortalama|Average_Avg. Disk sn/Okuma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Aktarım|Evet|Ortalama Disk sn/Aktarım|Count|Ortalama|Average_Avg. Disk sn/Aktarım|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Avg. Disk sn/Yazma|Evet|Ortalama Disk sn/yazma|Count|Ortalama|Average_Avg. Disk sn/Yazma|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Alınan Average_Bytes/sn|Evet|Alınan bayt/sn|Count|Ortalama|Alınan Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Gönderilen Average_Bytes/sn|Evet|Gönderilen bayt/sn|Count|Ortalama|Gönderilen Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Toplam Average_Bytes/sn|Evet|Toplam bayt/sn|Count|Ortalama|Toplam Average_Bytes/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Current disk kuyruğu uzunluğu|Evet|Geçerli disk sırası uzunluğu|Count|Ortalama|Average_Current disk kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk okunan bayt/sn|Evet|Disk okuma bayt/sn|Count|Ortalama|Average_Disk okunan bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Okuma/sn Average_Disk|Evet|Disk Okuma/sn|Count|Ortalama|Okuma/sn Average_Disk|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk aktarım sayısı/sn|Evet|Disk aktarımı/sn|Count|Ortalama|Average_Disk aktarım sayısı/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk yazma bayt/sn|Evet|Disk yazma bayt/sn|Count|Ortalama|Average_Disk yazma bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Disk yazma/sn|Evet|Disk yazma/sn|Count|Ortalama|Average_Disk yazma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Free megabayt|Evet|Boş megabayt|Count|Ortalama|Average_Free megabayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Fiziksel bellek Average_Free|Evet|Boş fiziksel bellek|Count|Ortalama|Fiziksel bellek Average_Free|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Disk belleği dosyalarında Average_Free alanı|Evet|Disk belleği dosyalarındaki boş alan|Count|Ortalama|Disk belleği dosyalarında Average_Free alanı|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Sanal bellek Average_Free|Evet|Boş sanal bellek|Count|Ortalama|Sanal bellek Average_Free|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Logical disk bayt/sn|Evet|Mantıksal disk bayt/sn|Count|Ortalama|Average_Logical disk bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Okuma/sn Average_Page|Evet|Sayfa Okuma/sn|Count|Ortalama|Okuma/sn Average_Page|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Page yazma/sn|Evet|Sayfa yazma/sn|Count|Ortalama|Average_Page yazma/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pages/sn|Evet|Sayfa/sn|Count|Ortalama|Average_Pages/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pct ayrıcalıklı saat|Evet|Ayrıcalıklı zaman yüzdesi|Count|Ortalama|Average_Pct ayrıcalıklı saat|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Pct Kullanıcı saati|Evet|Kullanım süresi yüzdesi|Count|Ortalama|Average_Pct Kullanıcı saati|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Physical disk bayt/sn|Evet|Fiziksel disk bayt/sn|Count|Ortalama|Average_Physical disk bayt/sn|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Processes|Evet|İşlemler|Count|Ortalama|Average_Processes|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Processor kuyruğu uzunluğu|Evet|İşlemci kuyruğu uzunluğu|Count|Ortalama|Average_Processor kuyruğu uzunluğu|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Disk belleği dosyalarında depolanan Average_Size|Evet|Disk belleği dosyalarında depolanan boyut|Count|Ortalama|Disk belleği dosyalarında depolanan Average_Size|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total bayt|Evet|Toplam bayt sayısı|Count|Ortalama|Average_Total bayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total alınan bayt|Evet|Alınan toplam bayt|Count|Ortalama|Average_Total alınan bayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total aktarılan baytlar|Evet|Aktarılan toplam bayt sayısı|Count|Ortalama|Average_Total aktarılan baytlar|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total çakışmaları|Evet|Toplam çakışma sayısı|Count|Ortalama|Average_Total çakışmaları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Alınan Average_Total paketleri|Evet|Alınan toplam paket sayısı|Count|Ortalama|Alınan Average_Total paketleri|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Iletilen Average_Total paketleri|Evet|Aktarılan toplam paket sayısı|Count|Ortalama|Iletilen Average_Total paketleri|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total RX hataları|Evet|Toplam RX hatası sayısı|Count|Ortalama|Average_Total RX hataları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Total TX hataları|Evet|Toplam TX hatası sayısı|Count|Ortalama|Average_Total TX hataları|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Uptime|Evet|Hizmet|Count|Ortalama|Average_Uptime|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used MBayt alanı değiştirme|Evet|Kullanılan MBayt değiştirme alanı|Count|Ortalama|Average_Used MBayt alanı değiştirme|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used belleği Kbayt|Evet|Kullanılan bellek Kbayt|Count|Ortalama|Average_Used belleği Kbayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Used belleği MBayt|Evet|Kullanılan bellek MBayt|Count|Ortalama|Average_Used belleği MBayt|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Users|Evet|Kullanıcılar|Count|Ortalama|Average_Users|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Average_Virtual paylaşılan bellek|Evet|Sanal paylaşılan bellek|Count|Ortalama|Average_Virtual paylaşılan bellek|Bilgisayar, ObjectName, InstanceName, CounterPath, dir|
|Olay|Evet|Olay|Count|Ortalama|Olay|Kaynak, olay günlüğü, bilgisayar, EventCategory, EventLevel, EventLevelName, EventID|
|Sinyal|Evet|Sinyal|Count|Toplam|Sinyal|Bilgisayar, OSType, sürüm, Sourcecomputerıd|
|Güncelleştir|Evet|Güncelleştir|Count|Ortalama|Güncelleştir|Bilgisayar, ürün, sınıflandırma, UpdateState, Isteğe bağlı, onaylanan|


## <a name="microsoftpeeringpeerings"></a>Microsoft. eşleme/eşlemeler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|EgressTrafficRate|Evet|Çıkış trafik oranı|BitsPerSecond|Ortalama|Bit/saniye cinsinden çıkış trafiği oranı|ConnectionID|
|IngressTrafficRate|Evet|Giriş trafiği oranı|BitsPerSecond|Ortalama|Bit/saniye cinsinden giriş trafiği oranı|ConnectionID|
|SessionAvailabilityV4|Evet|Oturum kullanılabilirliği v4|Yüzde|Ortalama|V4 oturumunun kullanılabilirliği|ConnectionID|
|SessionAvailabilityV6|Evet|Oturum kullanılabilirliği V6|Yüzde|Ortalama|V6 oturumunun kullanılabilirliği|ConnectionID|


## <a name="microsoftpeeringpeeringservices"></a>Microsoft. eşleme/peeringServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|PrefixLatency|Evet|Ön ek gecikmesi|Mayacak|Ortalama|Ortanca ön ek gecikmesi|PrefixName|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft. Powerbiadanmış/kapasiteler

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|memory_metric|Evet|Bellek|Bayt|Ortalama|Bellek. A1 için 0-3 GB, a2 için 0-5 GB, a3 için 0-10 GB, A4 için 0-25 GB, A6 için 0-50 GB ve A6 için 0-100 GB|Boyut yok|
|memory_thrashing_metric|Evet|Bellek kullanımı (veri kümeleri)|Yüzde|Ortalama|Ortalama bellek miktarı.|Boyut yok|
|qpu_high_utilization_metric|Evet|QPU Yüksek Kullanımı|Count|Toplam|Son dakikada QPU yüksek kullanımı, yüksek QPU kullanımı Için 1, aksi durumda 0|Boyut yok|
|QueryDuration|Evet|Sorgu süresi (veri kümeleri)|Mayacak|Ortalama|Son aralıktaki DAX sorgu süresi|Boyut yok|
|QueryPoolJobQueueLength|Evet|Sorgu havuzu Iş kuyruğu uzunluğu (veri kümeleri)|Count|Ortalama|Sorgu iş parçacığı havuzu kuyruğundaki iş sayısı.|Boyut yok|


## <a name="microsoftrelaynamespaces"></a>Microsoft. Relay/Namespace

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ActiveConnections|Hayır|ActiveConnections|Count|Toplam|Microsoft. Relay için toplam ActiveConnections.|EntityName|
|ActiveListeners|Hayır|ActiveListeners|Count|Toplam|Microsoft. Relay için toplam ActiveListeners.|EntityName|
|BytesTransferred|Evet|BytesTransferred|Count|Toplam|Microsoft. Relay için toplam BytesTransferred.|EntityName|
|ListenerConnections-ClientError|Hayır|ListenerConnections-ClientError|Count|Toplam|Microsoft. Relay için ListenerConnections 'da ClientError.|EntityName |
|ListenerConnections-Sunucuhatası|Hayır|ListenerConnections-Sunucuhatası|Count|Toplam|Microsoft. Relay için ListenerConnections 'da ServerError.|EntityName |
|ListenerConnections-başarılı|Hayır|ListenerConnections-başarılı|Count|Toplam|Microsoft. Relay için başarılı bir ListenerConnections.|EntityName |
|ListenerConnections-TotalRequests|Hayır|ListenerConnections-TotalRequests|Count|Toplam|Microsoft. Relay için toplam ListenerConnections.|EntityName|
|Listenerdisconnect|Hayır|Listenerdisconnect|Count|Toplam|Microsoft. Relay için toplam Listenerdisconnect.|EntityName|
|SenderConnections-ClientError|Hayır|SenderConnections-ClientError|Count|Toplam|Microsoft. Relay için SenderConnections 'ta ClientError.|EntityName |
|SenderConnections-Sunucuhatası|Hayır|SenderConnections-Sunucuhatası|Count|Toplam|Microsoft. Relay için SenderConnections 'ta ServerError.|EntityName |
|SenderConnections-başarılı|Hayır|SenderConnections-başarılı|Count|Toplam|Microsoft. Relay için başarılı SenderConnections.|EntityName |
|SenderConnections-TotalRequests|Hayır|SenderConnections-TotalRequests|Count|Toplam|Microsoft. Relay için toplam SenderConnections isteği.|EntityName|
|Gönderenin bağlantısı kesiliyor|Hayır|Gönderenin bağlantısı kesiliyor|Count|Toplam|Microsoft. Relay için toplam Senderdisconnect.|EntityName|


## <a name="microsoftsearchsearchservices"></a>Microsoft. Search/searchServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|SearchLatency|Evet|Arama gecikmesi|Saniye|Ortalama|Arama hizmeti için Ortalama arama gecikmesi|Boyut yok|
|SearchQueriesPerSecond|Evet|Sorgu arama/saniye|Sayaçpersaniye|Ortalama|Arama hizmeti için saniye başına arama sorgusu|Boyut yok|
|Tüm Searchqueriespercentage 'ı kısıtlar|Evet|Kısıtlanmış arama sorguları yüzdesi|Yüzde|Ortalama|Arama hizmeti için kısıtlanan arama sorgularının yüzdesi|Boyut yok|


## <a name="microsoftservicebusnamespaces"></a>Microsoft. ServiceBus/ad alanları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ActiveConnections|Hayır|ActiveConnections|Count|Toplam|Microsoft. ServiceBus için toplam etkin bağlantı sayısı.|Boyut yok|
|ActiveMessages|Hayır|Bir kuyruktaki/konudaki etkin ileti sayısı.|Count|Ortalama|Bir kuyruktaki/konudaki etkin ileti sayısı.|EntityName|
|ConnectionsClosed|Hayır|Kapatılan Bağlantılar.|Count|Ortalama|Microsoft. ServiceBus için kapatılan bağlantılar.|EntityName|
|ConnectionsOpened|Hayır|Açılan Bağlantılar.|Count|Ortalama|Microsoft. ServiceBus için açılan bağlantılar.|EntityName|
|CPUXNS|Hayır|CPU (kullanım dışı)|Yüzde|Maksimum|Service Bus Premium ad alanı CPU kullanımı ölçümü. Bu ölçüm eşit olarak dağıtılır. Lütfen bunun yerine CPU ölçümünü kullanın (NamespaceCpuUsage).|Boyut yok|
|DeadletteredMessages|Hayır|Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı.|Count|Ortalama|Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı.|EntityName|
|Incomingmessages|Evet|Gelen İletiler|Count|Toplam|Microsoft. ServiceBus için gelen Iletiler.|EntityName|
|Incomingrequests|Evet|Gelen İstekler|Count|Toplam|Microsoft. ServiceBus için gelen Istekler.|EntityName|
|İletiler|Hayır|Kuyruktaki/Konudaki iletilerin sayısı.|Count|Ortalama|Kuyruktaki/Konudaki iletilerin sayısı.|EntityName|
|NamespaceCpuUsage|Hayır|CPU|Yüzde|Maksimum|Service Bus Premium ad alanı CPU kullanımı ölçümü.|Boyut yok|
|NamespaceMemoryUsage|Hayır|Bellek Kullanımı|Yüzde|Maksimum|Service Bus Premium ad alanı bellek kullanımı ölçümü.|Boyut yok|
|OutgoingMessages|Evet|Giden İletiler|Count|Toplam|Microsoft. ServiceBus için giden Iletiler.|EntityName|
|ScheduledMessages|Hayır|Bir kuyruktaki/konudaki zamanlanan ileti sayısı.|Count|Ortalama|Bir kuyruktaki/konudaki zamanlanan ileti sayısı.|EntityName|
|ServerErrors|Hayır|Sunucu Hataları.|Count|Toplam|Microsoft. ServiceBus için sunucu hataları.|EntityName |
|Boyut|Hayır|Boyut|Bayt|Ortalama|Bir kuyruğun/konunun bayt cinsinden boyutu.|EntityName|
|SuccessfulRequests|Hayır|Başarılı İstekler|Count|Toplam|Bir ad alanı için toplam başarılı istek sayısı|EntityName |
|OperationName|Hayır|Kısıtlanan İstekler.|Count|Toplam|Microsoft. ServiceBus için kısıtlanan Istekler.|EntityName |
|UserErrors|Hayır|Kullanıcı Hataları.|Count|Toplam|Microsoft. ServiceBus için Kullanıcı hataları.|EntityName |
|WSXNS|Hayır|Bellek kullanımı (kullanım dışı)|Yüzde|Maksimum|Service Bus Premium ad alanı bellek kullanımı ölçümü. Bu ölçüm kullanım dışıdır. Lütfen bunun yerine bellek kullanımı (NamespaceMemoryUsage) ölçüsünü kullanın.|Boyut yok|


## <a name="microsoftservicefabricmeshapplications"></a>Microsoft. Servicefabrickafes/uygulamaları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ActualCpu|Hayır|ActualCpu|Count|Ortalama|Milli çekirdekte gerçek CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualMemory|Hayır|ActualMemory|Bayt|Ortalama|MB cinsinden gerçek bellek kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Ayrılatedcpu|Hayır|Ayrılatedcpu|Count|Ortalama|Milli çekirdekleri içinde bu kapsayıcıya ayrılan CPU|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Ayrılan bellek|Hayır|Ayrılan bellek|Bayt|Ortalama|Bu kapsayıcıya MB cinsinden ayrılan bellek|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ApplicationStatus|Hayır|ApplicationStatus|Count|Ortalama|Service Fabric kafes uygulamasının durumu|ApplicationName, durum|
|ContainerStatus|Hayır|ContainerStatus|Count|Ortalama|Service Fabric kafes uygulamasındaki kapsayıcının durumu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName, durum|
|Cpukullanımı|Hayır|Cpukullanımı|Yüzde|Ortalama|Ayrılan CPU yüzdesi olarak bu kapsayıcı için CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|Memoryutilileştirme|Hayır|Memoryutilileştirme|Yüzde|Ortalama|Ayrılan CPU yüzdesi olarak bu kapsayıcı için CPU kullanımı|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|RestartCount|Hayır|RestartCount|Count|Ortalama|Service Fabric kafes uygulamasındaki bir kapsayıcının yeniden başlatma sayısı|ApplicationName, Status, HizmetAdı, ServiceReplicaName, CodePackageName|
|Servicereperepstatus|Hayır|Servicereperepstatus|Count|Ortalama|Service Fabric kafes uygulamasındaki bir hizmet çoğaltmasının sistem durumu|ApplicationName, Status, HizmetAdı, ServiceReplicaName|
|ServiceStatus|Hayır|ServiceStatus|Count|Ortalama|Service Fabric kafes uygulamasındaki bir hizmetin sistem durumu|ApplicationName, durum, HizmetAdı|


## <a name="microsoftsignalrservicesignalr"></a>Microsoft. SignalRService/SignalR

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ConnectionCount|Evet|Bağlantı sayısı|Count|Maksimum|Kullanıcı bağlantısı miktarı.|Uç Noktası|
|Inboundtraffic|Evet|Gelen Trafik|Bayt|Toplam|Hizmetin gelen trafiği|Boyut yok|
|MessageCount|Evet|İleti Sayısı|Count|Toplam|Toplam ileti miktarı.|Boyut yok|
|OutboundTraffic|Evet|Giden Trafik|Bayt|Toplam|Hizmetin giden trafiği|Boyut yok|
|SystemErrors|Evet|Sistem hataları|Yüzde|Maksimum|Sistem hatalarının yüzdesi|Boyut yok|
|UserErrors|Evet|Kullanıcı hataları|Yüzde|Maksimum|Kullanıcı hatalarının yüzdesi|Boyut yok|


## <a name="microsoftsqlmanagedinstances"></a>Microsoft. SQL/ManagedInstances

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|avg_cpu_percent|Evet|Ortalama CPU yüzdesi|Yüzde|Ortalama|Ortalama CPU yüzdesi|Boyut yok|
|io_bytes_read|Evet|Okunan GÇ baytları|Bayt|Ortalama|Okunan GÇ baytları|Boyut yok|
|io_bytes_written|Evet|Yazılan GÇ baytları|Bayt|Ortalama|Yazılan GÇ baytları|Boyut yok|
|io_requests|Evet|GÇ istek sayısı|Count|Ortalama|GÇ istek sayısı|Boyut yok|
|reserved_storage_mb|Evet|Ayrılan depolama alanı|Count|Ortalama|Ayrılan depolama alanı|Boyut yok|
|storage_space_used_mb|Evet|Kullanılan depolama alanı|Count|Ortalama|Kullanılan depolama alanı|Boyut yok|
|virtual_core_count|Evet|Sanal çekirdek sayısı|Count|Ortalama|Sanal çekirdek sayısı|Boyut yok|


## <a name="microsoftsqlserversdatabases"></a>Microsoft. SQL/Servers/veritabanları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|active_queries|Evet|Etkin sorgular|Count|Toplam|Tüm iş yükü grupları genelinde etkin sorgular. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|allocated_data_storage|Evet|Ayrılan veri alanı|Bayt|Ortalama|Ayrılan veri depolama alanı. Veri ambarlarında geçerli değildir.|Boyut yok|
|app_cpu_billed|Evet|Faturalandırılan uygulama CPU 'SU|Count|Toplam|Uygulama CPU 'SU faturalandırılır. Sunucusuz veritabanları için geçerlidir.|Boyut yok|
|app_cpu_percent|Evet|Uygulama CPU yüzdesi|Yüzde|Ortalama|Uygulama CPU yüzdesi. Sunucusuz veritabanları için geçerlidir.|Boyut yok|
|app_memory_percent|Evet|Uygulama belleği yüzdesi|Yüzde|Ortalama|Uygulama belleği yüzdesi. Sunucusuz veritabanları için geçerlidir.|Boyut yok|
|base_blob_size_bytes|Evet|Temel BLOB depolama boyutu|Bayt|Maksimum|Temel BLOB depolama boyutu. Hiper ölçekli veritabanları için geçerlidir.|Boyut yok|
|blocked_by_firewall|Evet|Güvenlik duvarı tarafından engellendi|Count|Toplam|Güvenlik duvarı tarafından engellendi|Boyut yok|
|cache_hit_percent|Evet|İsabetli önbellek okuması yüzdesi|Yüzde|Maksimum|İsabetli önbellek okuması yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|cache_used_percent|Evet|Önbellek kullanılan yüzde|Yüzde|Maksimum|Önbellek kullanım yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|connection_failed|Evet|Başarısız Bağlantılar|Count|Toplam|Başarısız Bağlantılar|Boyut yok|
|connection_successful|Evet|Başarılı bağlantılar|Count|Toplam|Başarılı bağlantılar|Boyut yok|
|cpu_limit|Evet|CPU sınırı|Count|Ortalama|CPU sınırı. Sanal çekirdek tabanlı veritabanları için geçerlidir.|Boyut yok|
|cpu_percent|Evet|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|cpu_used|Evet|Kullanılan CPU|Count|Ortalama|Kullanılan CPU. Sanal çekirdek tabanlı veritabanları için geçerlidir.|Boyut yok|
|Çözül|Evet|Çık|Count|Toplam|Çık. Veri ambarlarında geçerli değildir.|Boyut yok|
|diff_backup_size_bytes|Evet|Değişiklik yedekleme depolama boyutu|Bayt|Maksimum|Toplu fark yedekleme depolama boyutu. Sanal çekirdek tabanlı veritabanları için geçerlidir. Hiper ölçekli veritabanları için geçerli değildir.|Boyut yok|
|dtu_consumption_percent|Evet|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi. DTU tabanlı veritabanları için geçerlidir.|Boyut yok|
|dtu_limit|Evet|DTU sınırı|Count|Ortalama|DTU sınırı. DTU tabanlı veritabanları için geçerlidir.|Boyut yok|
|dtu_used|Evet|Kullanılan DTU|Count|Ortalama|DTU kullanıldı. DTU tabanlı veritabanları için geçerlidir.|Boyut yok|
|dw_backup_size_gb|Evet|Veri depolama boyutu (GB)|Count|Toplam|Veri depolama boyutu, verilerinizin boyutundan ve işlem günlüğünden oluşur. Ölçüm faturanızda ' depolama ' bölümüne doğru sayılır. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|dw_geosnapshot_size_gb|Evet|Olağanüstü durum kurtarma depolama boyutu (GB)|Count|Toplam|Olağanüstü durum kurtarma depolama boyutu faturanızda ' olağanüstü durum kurtarma depolaması ' olarak yansıtılır. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|dw_snapshot_size_gb|Evet|Anlık görüntü depolama boyutu (GB)|Count|Toplam|Anlık görüntü depolama boyutu, Kullanıcı tanımlı ve otomatik geri yükleme noktaları oluşturmak için anlık görüntüler tarafından yakalanan artımlı değişikliklerin boyutudur. Ölçüm faturanızda ' depolama ' bölümüne doğru sayılır. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|dwu_consumption_percent|Evet|DWU yüzdesi|Yüzde|Maksimum|DWU yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|dwu_limit|Evet|DWU sınırı|Count|Maksimum|DWU sınırı. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|dwu_used|Evet|Kullanılan DWU|Count|Maksimum|DWU kullanıldı. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|full_backup_size_bytes|Evet|Tam yedekleme depolama boyutu|Bayt|Maksimum|Toplu tam yedekleme depolama boyutu. Sanal çekirdek tabanlı veritabanları için geçerlidir. Hiper ölçekli veritabanları için geçerli değildir.|Boyut yok|
|local_tempdb_usage_percent|Evet|Yerel tempdb yüzdesi|Yüzde|Ortalama|Yerel tempdb yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|log_backup_size_bytes|Evet|Günlük yedekleme depolama boyutu|Bayt|Maksimum|Toplu günlük yedekleme depolama boyutu. Sanal çekirdek tabanlı ve hiper ölçekli veritabanları için geçerlidir.|Boyut yok|
|log_write_percent|Evet|Günlük GÇ yüzdesi|Yüzde|Ortalama|Günlük GÇ yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|
|memory_usage_percent|Evet|Bellek yüzdesi|Yüzde|Maksimum|Bellek yüzdesi. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|physical_data_read_percent|Evet|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|Boyut yok|
|queued_queries|Evet|Kuyruğa alınmış sorgular|Count|Toplam|Tüm iş yükü gruplarında sıraya alınmış sorgular. Yalnızca veri ambarlarında geçerlidir.|Boyut yok|
|sessions_percent|Evet|Oturum yüzdesi|Yüzde|Ortalama|Oturum yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|
|snapshot_backup_size_bytes|Evet|Anlık görüntü yedekleme depolama boyutu|Bayt|Maksimum|Birikmeli anlık görüntü yedekleme depolama boyutu. Hiper ölçekli veritabanları için geçerlidir.|Boyut yok|
|sqlserver_process_core_percent|Evet|SQL Server işlem çekirdeği yüzdesi|Yüzde|Maksimum|SQL DB işleminin yüzdesi olarak CPU kullanımı. Veri ambarlarında geçerli değildir.|Boyut yok|
|sqlserver_process_memory_percent|Evet|SQL Server işlem belleği yüzdesi|Yüzde|Maksimum|SQL DB işleminin yüzdesi olarak bellek kullanımı. Veri ambarlarında geçerli değildir.|Boyut yok|
|depolama|Evet|Kullanılan veri alanı|Bayt|Maksimum|Kullanılan veri alanı. Veri ambarlarında geçerli değildir.|Boyut yok|
|storage_percent|Evet|Kullanılan veri alanı yüzdesi|Yüzde|Maksimum|Kullanılan veri alanı yüzdesi. Veri ambarları veya hiper ölçekli veritabanları için geçerli değildir.|Boyut yok|
|tempdb_data_size|Evet|Tempdb veri dosyası boyutu kilobayt|Count|Maksimum|Tempdb veri dosyası boyutu kilobayt. Veri ambarlarında geçerli değildir.|Boyut yok|
|tempdb_log_size|Evet|Tempdb günlük dosyası boyutu kilobayt|Count|Maksimum|Tempdb günlük dosyası boyutu kilobayt. Veri ambarlarında geçerli değildir.|Boyut yok|
|tempdb_log_used_percent|Evet|Kullanılan tempdb günlüğü yüzdesi|Yüzde|Maksimum|Kullanılan tempdb günlüğü yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|
|wlg_active_queries|Evet|İş yükü grubu etkin sorguları|Count|Toplam|İş yükü grubu içindeki etkin sorgular. Yalnızca veri ambarlarında geçerlidir.|WorkloadGroupName, IsUserDefined|
|wlg_active_queries_timeouts|Evet|İş yükü grubu sorgu zaman aşımları|Count|Toplam|İş yükü grubu için zaman aşımına uğramış sorgular. Yalnızca veri ambarlarında geçerlidir.|WorkloadGroupName, IsUserDefined|
|wlg_allocation_relative_to_system_percent|Evet|Sistem iş yükü grubuna göre ayırma yüzdesi|Yüzde|Maksimum|Tüm iş yükü grubu başına sistemin tamamına göre ayrılan kaynak yüzdesi. Yalnızca veri ambarlarında geçerlidir.|WorkloadGroupName, IsUserDefined|
|wlg_allocation_relative_to_wlg_effective_cap_percent|Evet|Sınır kaynağına göre iş yükü grubu ayırma yüzdesi|Yüzde|Maksimum|Her iş yükü grubu için belirtilen sınır kaynağına göre ayrılan kaynak yüzdesi. Yalnızca veri ambarlarında geçerlidir.|WorkloadGroupName, IsUserDefined|
|wlg_effective_cap_resource_percent|Evet|Etkin uç kaynak yüzdesi|Yüzde|Maksimum|Diğer iş yükü grupları için ayrılan en düşük kaynak yüzdesine sahip olan iş yükü grubu için izin verilen kaynakların yüzdesine yönelik sabit sınır. Yalnızca veri ambarlarında geçerlidir.|WorkloadGroupName, IsUserDefined|
|wlg_effective_min_resource_percent|Evet|Geçerli Min kaynak yüzdesi|Yüzde|Maksimum|Hizmet düzeyi en düşük hesaba katılarak iş yükü grubu için ayrılan ve yalıtılmış kaynakların minimum yüzdesi. Yalnızca veri ambarlarında geçerlidir.|WorkloadGroupName, IsUserDefined|
|wlg_queued_queries|Evet|İş yükü grubu sıraya alınmış sorgular|Count|Toplam|İş yükü grubu içindeki sıraya alınmış sorgular. Yalnızca veri ambarlarında geçerlidir.|WorkloadGroupName, IsUserDefined|
|workers_percent|Evet|Çalışan yüzdesi|Yüzde|Ortalama|Çalışan yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|
|xtp_storage_percent|Evet|Bellek içi OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek içi OLTP depolaması yüzdesi. Veri ambarlarında geçerli değildir.|Boyut yok|


## <a name="microsoftsqlserverselasticpools"></a>Microsoft. SQL/Servers/Elaun havuzları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|allocated_data_storage|Evet|Ayrılan veri alanı|Bayt|Ortalama|Ayrılan veri alanı|Boyut yok|
|allocated_data_storage_percent|Evet|Ayrılan veri alanı yüzdesi|Yüzde|Maksimum|Ayrılan veri alanı yüzdesi|Boyut yok|
|cpu_limit|Evet|CPU sınırı|Count|Ortalama|CPU sınırı. Sanal çekirdek tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|cpu_percent|Evet|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|Boyut yok|
|cpu_used|Evet|Kullanılan CPU|Count|Ortalama|Kullanılan CPU. Sanal çekirdek tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|database_allocated_data_storage|Hayır|Ayrılan veri alanı|Bayt|Ortalama|Ayrılan veri alanı|DatabaseResourceId|
|database_cpu_limit|Hayır|CPU sınırı|Count|Ortalama|CPU sınırı|DatabaseResourceId|
|database_cpu_percent|Hayır|CPU yüzdesi|Yüzde|Ortalama|CPU yüzdesi|DatabaseResourceId|
|database_cpu_used|Hayır|Kullanılan CPU|Count|Ortalama|Kullanılan CPU|DatabaseResourceId|
|database_dtu_consumption_percent|Hayır|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi|DatabaseResourceId|
|database_eDTU_used|Hayır|eDTU kullanıldı|Count|Ortalama|eDTU kullanıldı|DatabaseResourceId|
|database_log_write_percent|Hayır|Günlük GÇ yüzdesi|Yüzde|Ortalama|Günlük GÇ yüzdesi|DatabaseResourceId|
|database_physical_data_read_percent|Hayır|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|DatabaseResourceId|
|database_sessions_percent|Hayır|Oturum yüzdesi|Yüzde|Ortalama|Oturum yüzdesi|DatabaseResourceId|
|database_storage_used|Hayır|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|DatabaseResourceId|
|database_workers_percent|Hayır|Çalışan yüzdesi|Yüzde|Ortalama|Çalışan yüzdesi|DatabaseResourceId|
|dtu_consumption_percent|Evet|DTU yüzdesi|Yüzde|Ortalama|DTU yüzdesi. DTU tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|eDTU_limit|Evet|eDTU sınırı|Count|Ortalama|eDTU sınırı. DTU tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|eDTU_used|Evet|eDTU kullanıldı|Count|Ortalama|eDTU kullanıldı. DTU tabanlı elastik havuzlar için geçerlidir.|Boyut yok|
|log_write_percent|Evet|Günlük GÇ yüzdesi|Yüzde|Ortalama|Günlük GÇ yüzdesi|Boyut yok|
|physical_data_read_percent|Evet|Veri G/Ç yüzdesi|Yüzde|Ortalama|Veri G/Ç yüzdesi|Boyut yok|
|sessions_percent|Evet|Oturum yüzdesi|Yüzde|Ortalama|Oturum yüzdesi|Boyut yok|
|sqlserver_process_core_percent|Evet|SQL Server işlem çekirdeği yüzdesi|Yüzde|Maksimum|SQL DB işleminin yüzdesi olarak CPU kullanımı. Elastik havuzlar için geçerlidir.|Boyut yok|
|sqlserver_process_memory_percent|Evet|SQL Server işlem belleği yüzdesi|Yüzde|Maksimum|SQL DB işleminin yüzdesi olarak bellek kullanımı. Elastik havuzlar için geçerlidir.|Boyut yok|
|storage_limit|Evet|Veri boyutu üst sınırı|Bayt|Ortalama|Veri boyutu üst sınırı|Boyut yok|
|storage_percent|Evet|Kullanılan veri alanı yüzdesi|Yüzde|Ortalama|Kullanılan veri alanı yüzdesi|Boyut yok|
|storage_used|Evet|Kullanılan veri alanı|Bayt|Ortalama|Kullanılan veri alanı|Boyut yok|
|tempdb_data_size|Evet|Tempdb veri dosyası boyutu kilobayt|Count|Maksimum|Tempdb veri dosyası boyutu kilobayt|Boyut yok|
|tempdb_log_size|Evet|Tempdb günlük dosyası boyutu kilobayt|Count|Maksimum|Tempdb günlük dosyası boyutu kilobayt|Boyut yok|
|tempdb_log_used_percent|Evet|Kullanılan tempdb günlüğü yüzdesi|Yüzde|Maksimum|Kullanılan tempdb günlüğü yüzdesi|Boyut yok|
|workers_percent|Evet|Çalışan yüzdesi|Yüzde|Ortalama|Çalışan yüzdesi|Boyut yok|
|xtp_storage_percent|Evet|Bellek içi OLTP depolama yüzdesi|Yüzde|Ortalama|Bellek içi OLTP depolama yüzdesi|Boyut yok|


## <a name="microsoftstoragestorageaccounts"></a>Microsoft. Storage/storageAccounts

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Çıkış verileri miktarı. Bu sayı, Azure Storage 'dan dış istemciye çıkış ve Azure 'da çıkış içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|
|UsedCapacity|Hayır|Kullanılan kapasite|Bayt|Ortalama|Depolama hesabı tarafından kullanılan depolama miktarı. Standart depolama hesapları için blob, tablo, dosya ve kuyruk tarafından kullanılan kapasitenin toplamıdır. Premium Depolama hesapları ve BLOB depolama hesapları için BlobCapacity veya FileCapacity ile aynıdır.|Boyut yok|


## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft. Storage/storageAccounts/blobServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|
|BlobCapacity|Hayır|Blob kapasitesi|Bayt|Ortalama|Depolama hesabının blob hizmeti tarafından bayt olarak kullanılan depolama miktarı.|BlobType, katman|
|BlobCount|Hayır|BLOB sayısı|Count|Ortalama|Depolama hesabında depolanan BLOB nesnelerinin sayısı.|BlobType, katman|
|BlobProvisionedSize|Hayır|Blob sağlanan boyut|Bayt|Ortalama|Depolama hesabının blob hizmetinde bayt olarak sağlanan depolama miktarı.|BlobType, katman|
|ContainerCount|Evet|Blob kapsayıcı sayısı|Count|Ortalama|Depolama hesabındaki kapsayıcıların sayısı.|Boyut yok|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Çıkış verileri miktarı. Bu sayı, Azure Storage 'dan dış istemciye çıkış ve Azure 'da çıkış içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Dizin kapasitesi|Hayır|Dizin kapasitesi|Bayt|Ortalama|Hiyerarşik Dizin Azure Data Lake Storage 2. tarafından kullanılan depolama miktarı.|Boyut yok|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|


## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft. Storage/storageAccounts/fileServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Çıkış verileri miktarı. Bu sayı, Azure Storage 'dan dış istemciye çıkış ve Azure 'da çıkış içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Dosya kapasitesi|Hayır|Dosya kapasitesi|Bayt|Ortalama|Depolama hesabı tarafından kullanılan dosya depolama alanı miktarı.|Dosya Paylaşımı|
|FileCount|Hayır|Dosya sayısı|Count|Ortalama|Depolama hesabındaki dosyaların sayısı.|Dosya Paylaşımı|
|Filesharecapacitykotası|Hayır|Dosya paylaşma kapasitesi kotası|Bayt|Ortalama|Azure dosyaları hizmeti tarafından bayt olarak kullanılabilecek depolama miktarının üst sınırı.|Dosya Paylaşımı|
|Dosya ShareCount|Hayır|Dosya paylaşma sayısı|Count|Ortalama|Depolama hesabındaki dosya paylaşımlarının sayısı.|Boyut yok|
|FileShareProvisionedIOPS|Hayır|Dosya paylaşımının sağlandığı ıOPS|Bayt|Ortalama|Premium dosyalar depolama hesabındaki Premium dosya paylaşımında sağlanan ıOPS 'nin temel sayısı. Bu sayı, paylaşma kapasitesinin sağlanan boyutu (Kota) temel alınarak hesaplanır.|Dosya Paylaşımı|
|FileShareSnapshotCount|Hayır|Dosya paylaşımının anlık görüntü sayısı|Count|Ortalama|Depolama hesabının dosyalar hizmetindeki paylaşımda bulunan anlık görüntü sayısı.|Dosya Paylaşımı|
|FileShareSnapshotSize|Hayır|Dosya paylaşımının anlık görüntü boyutu|Bayt|Ortalama|Bayt olarak depolama hesabının dosya hizmetindeki anlık görüntüler tarafından kullanılan depolama miktarı.|Dosya Paylaşımı|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulama, FileShare|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|ResponseType, GeoType, ApiName, Authentication, FileShare|


## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft. Storage/storageAccounts/queueServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Çıkış verileri miktarı. Bu sayı, Azure Storage 'dan dış istemciye çıkış ve Azure 'da çıkış içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|QueueCapacity|Evet|Kuyruk kapasitesi|Bayt|Ortalama|Depolama hesabı tarafından kullanılan kuyruk depolama miktarı.|Boyut yok|
|QueueCount|Evet|Sıra sayısı|Count|Ortalama|Depolama hesabındaki sıraların sayısı.|Boyut yok|
|QueueMessageCount|Evet|Kuyruk Iletisi sayısı|Count|Ortalama|Depolama hesabındaki süre dolma edilmemiş sıra iletilerinin sayısı.|Boyut yok|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|


## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft. Storage/storageAccounts/tableServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Kullanılabilirlik|Evet|Kullanılabilirlik|Yüzde|Ortalama|Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, TotalBillableRequests değerinin beklenmeyen hata üretenler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Çıkış|Evet|Çıkış|Bayt|Toplam|Çıkış verileri miktarı. Bu sayı, Azure Storage 'dan dış istemciye çıkış ve Azure 'da çıkış içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Giriş|Evet|Giriş|Bayt|Toplam|Bayt cinsinden giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|SuccessE2ELatency|Evet|Başarı E2E Gecikme Süresi|Mayacak|Ortalama|Bir depolama hizmetine veya belirtilen API işlemine yapılan başarılı isteklerin milisaniye cinsinden ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Başarılı Sunucugecikmesi|Evet|Başarı sunucusu gecikmesi|Mayacak|Ortalama|Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez.|Coğrafi tür, ApiName, kimlik doğrulaması|
|Tablokapasitesi|Evet|Tablo kapasitesi|Bayt|Ortalama|Depolama hesabı tarafından kullanılan tablo depolaması miktarı.|Boyut yok|
|Tablosayısı|Evet|Tablo sayısı|Count|Ortalama|Depolama hesabındaki tablo sayısı.|Boyut yok|
|TableEntityCount|Evet|Tablo varlık sayısı|Count|Ortalama|Depolama hesabındaki tablo varlıklarının sayısı.|Boyut yok|
|İşlemler|Evet|İşlemler|Count|Toplam|Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız isteklerin yanı sıra hata üreten istekleri içerir. Farklı yanıt türü sayısı için ResponseType boyutunu kullanın.|Yanıt türü, coğrafi tür, ApiName, kimlik doğrulama|


## <a name="microsoftstoragesyncstoragesyncservices"></a>Microsoft. storagessync/storageSyncServices

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ServerSyncSessionResult|Evet|Eşitleme oturumu sonucu|Count|Ortalama|Sunucu uç noktası, bulut uç noktası ile eşitleme oturumunu başarıyla tamamladığında 1 değerini günlüğe kaydeden ölçüm|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Evet|Eşitlenen baytlar|Bayt|Toplam|Eşitleme oturumları için aktarılan toplam dosya boyutu|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncRecalledNetworkBytesByApplication|Evet|Uygulamaya göre bulut katmanlama geri çağırma boyutu|Bayt|Toplam|Uygulama tarafından geri çekilen verilerin boyutu|SyncGroupName, ServerName, ApplicationName|
|Storagesyncyeniden hesaplama Ledtotalnetworkbytes|Evet|Bulut katmanlama geri çağırma boyutu|Bayt|Toplam|Geri çekilen verilerin boyutu|SyncGroupName, ServerName|
|Storagesyncyeniden hesaplama Liototalsizebytes|Evet|Bulut katmanlama geri çağırma|Bayt|Toplam|Sunucu tarafından geri çekilen toplam veri boyutu|aboneliğinde ve|
|StorageSyncRecallThroughputBytesPerSecond|Evet|Bulut katmanlama geri çekme performansı|BytesPerSecond|Ortalama|Veri geri çağırma işleme boyutu|SyncGroupName, ServerName|
|Storagesyncserversinyal|Evet|Sunucu çevrimiçi durumu|Count|Maksimum|Yeniden sigvbulunan sunucu, bulut uç noktasıyla bir sinyal başarıyla kaydetilişinde 1 değerini günlüğe kaydeden ölçüm|aboneliğinde ve|
|StorageSyncSyncSessionAppliedFilesCount|Evet|Eşitlenen dosyalar|Count|Toplam|Eşitlenen dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Evet|Dosyalar eşitlenmiyor|Count|Toplam|Eşitleme başarısız olan dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|


## <a name="microsoftstoragesyncstoragesyncservicesregisteredservers"></a>Microsoft. storagessync/storageSyncServices/registeredServers

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Sunucusinyal|Evet|Sunucu çevrimiçi durumu|Count|Maksimum|Yeniden sigvbulunan sunucu, bulut uç noktasıyla bir sinyal başarıyla kaydetilişinde 1 değerini günlüğe kaydeden ölçüm|Sunucuresourceıd, ServerName|
|Sunucu yeniden hesaplama Liototalsizebytes|Evet|Bulut katmanlama geri çağırma|Bayt|Toplam|Sunucu tarafından geri çekilen toplam veri boyutu|Sunucuresourceıd, ServerName|


## <a name="microsoftstoragesyncstoragesyncservicessyncgroups"></a>Microsoft. storagessync/storageSyncServices/syncGroups

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|SyncGroupBatchTransferredFileBytes|Evet|Eşitlenen baytlar|Bayt|Toplam|Eşitleme oturumları için aktarılan toplam dosya boyutu|SyncGroupName, ServerEndpointName, SyncDirection|
|SyncGroupSyncSessionAppliedFilesCount|Evet|Eşitlenen dosyalar|Count|Toplam|Eşitlenen dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|
|SyncGroupSyncSessionPerItemErrorsCount|Evet|Dosyalar eşitlenmiyor|Count|Toplam|Eşitleme başarısız olan dosya sayısı|SyncGroupName, ServerEndpointName, SyncDirection|


## <a name="microsoftstoragesyncstoragesyncservicessyncgroupsserverendpoints"></a>Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ServerEndpointBatchTransferredFileBytes|Evet|Eşitlenen baytlar|Bayt|Toplam|Eşitleme oturumları için aktarılan toplam dosya boyutu|ServerEndpointName, SyncDirection|
|ServerEndpointSyncSessionAppliedFilesCount|Evet|Eşitlenen dosyalar|Count|Toplam|Eşitlenen dosya sayısı|ServerEndpointName, SyncDirection|
|ServerEndpointSyncSessionPerItemErrorsCount|Evet|Dosyalar eşitlenmiyor|Count|Toplam|Eşitleme başarısız olan dosya sayısı|ServerEndpointName, SyncDirection|


## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft. StreamAnalytics/streamingjobs

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AMLCalloutFailedRequests|Evet|Başarısız Işlev Istekleri|Count|Toplam|Başarısız Işlev Istekleri|LogicalName, PartitionID|
|AMLCalloutInputEvents|Evet|İşlev olayları|Count|Toplam|İşlev olayları|LogicalName, PartitionID|
|AMLCalloutRequests|Evet|İşlev Istekleri|Count|Toplam|İşlev Istekleri|LogicalName, PartitionID|
|Dönüştürme hataları|Evet|Veri dönüştürme hataları|Count|Toplam|Veri dönüştürme hataları|LogicalName, PartitionID|
|DeserializationError|Evet|Giriş seri kaldırma hataları|Count|Toplam|Giriş seri kaldırma hataları|LogicalName, PartitionID|
|Droppedorayarlantedevents|Evet|Sıra dışı olaylar|Count|Toplam|Sıra dışı olaylar|LogicalName, PartitionID|
|EarlyInputEvents|Evet|Erken giriş olayları|Count|Toplam|Erken giriş olayları|LogicalName, PartitionID|
|Hatalar|Evet|Çalışma zamanı hataları|Count|Toplam|Çalışma zamanı hataları|LogicalName, PartitionID|
|Inputeventbytes|Evet|Olay baytlarını gir|Bayt|Toplam|Olay baytlarını gir|LogicalName, PartitionID|
|Inputevents|Evet|Giriş olayları|Count|Toplam|Giriş olayları|LogicalName, PartitionID|
|Inputeventssourcesbackgünlüğe kaydediliyor|Evet|Biriktirme listesindeki giriş olayları|Count|Maksimum|Biriktirme listesindeki giriş olayları|LogicalName, PartitionID|
|Inputeventssourcespersecond|Evet|Alınan giriş kaynakları|Count|Toplam|Alınan giriş kaynakları|LogicalName, PartitionID|
|LateInputEvents|Evet|Geç giriş olayları|Count|Toplam|Geç giriş olayları|LogicalName, PartitionID|
|OutputEvents|Evet|Çıkış olayları|Count|Toplam|Çıkış olayları|LogicalName, PartitionID|
|Outputsulu Markdelayseconds|Evet|Filigran gecikmesi|Saniye|Maksimum|Filigran gecikmesi|LogicalName, PartitionID|
|Resourcekullanım|Evet|SU kullanımı yüzdesi|Yüzde|Maksimum|SU kullanımı yüzdesi|LogicalName, PartitionID|


## <a name="microsoftsynapseworkspaces"></a>Microsoft. SYNAPSE/çalışma alanları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|OrchestrationActivityRunsEnded|Hayır|Etkinlik çalıştırmaları sona erdi|Count|Toplam|Başarılı, başarısız veya iptal edilen düzenleme etkinliği sayısı|Sonuç, Failuyeniden yazma, etkinlik, ActivityType, işlem hattı|
|Orchestrationardışık düzen eylemsizlik|Hayır|İşlem hattı çalıştırmaları sona erdi|Count|Toplam|Başarılı, başarısız veya iptal edilen düzenleme işlem hattı çalıştırmaları sayısı|Sonuç, Failuyeniden yazma, işlem hattı|
|OrchestrationTriggersEnded|Hayır|Sonlanmış Tetikleyiciler|Count|Toplam|Başarılı, başarısız veya iptal edilen düzenleme Tetikleyicileri sayısı|Sonuç, Failuyeniden yazma, tetikleyici|
|Sqlonon Logindenemeleri|Hayır|Oturum açma girişimleri|Count|Toplam|Başarılı veya başarısız olan oturum açma denemesi sayısı|Sonuç|
|Sqlonon Queriesbitirildi|Hayır|Sorgular sona erdi|Count|Toplam|Başarılı, başarısız veya iptal edilen sorgu sayısı|Sonuç|
|Sqlonon Queryprocessedbytes|Hayır|İşlenen veri|Bayt|Toplam|Sorgular tarafından işlenen veri miktarı|Boyut yok|


## <a name="microsoftsynapseworkspacesbigdatapools"></a>Microsoft. SYNAPSE/Workspaces/bigDataPools

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|CoresCapacity|Hayır|Çekirdek kapasitesi|Count|Maksimum|Çekirdek kapasitesi|Boyut yok|
|MemoryCapacityGB|Hayır|Bellek kapasitesi (GB)|Count|Maksimum|Bellek kapasitesi (GB)|Boyut yok|
|Parlak iş|Evet|Sonlandırılan uygulamalar|Count|Toplam|Sonlandırılan uygulamaların sayısı|JobType, JobResult|


## <a name="microsoftsynapseworkspacessqlpools"></a>Microsoft. SYNAPSE/Workspaces/Sqlhavuzlar

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Uyartivecachehitpercent|Hayır|Uyarlamalı önbellek isabet yüzdesi|Yüzde|Maksimum|İş yüklerinin Uyarlamalı önbelleğin kullanılmasıyla ne kadar iyi olduğunu ölçer. Ek kapasite için ölçekleneceğini veya önbelleği doldurma için iş yüklerini yeniden çalıştırmayı öğrenmek üzere bu ölçümü önbellek isabet yüzdesi ölçümüyle birlikte kullanın|Boyut yok|
|Uyartivecacheusedpercent|Hayır|Uyarlamalı önbellek kullanılan yüzde|Yüzde|Maksimum|İş yüklerinin Uyarlamalı önbelleğin kullanılmasıyla ne kadar iyi olduğunu ölçer. Bu ölçümü, ek kapasite için ölçekleneceğini veya önbelleğe almak üzere iş yüklerini yeniden çalıştırmayı belirtmek için kullanılan önbellek yüzdesi ölçümüyle kullanın|Boyut yok|
|Bağlantılar|Evet|Bağlantılar|Count|Toplam|SQL havuzunda Toplam oturum açma sayısı|Sonuç|
|ConnectionsBlockedByFirewall|Hayır|Güvenlik duvarı tarafından engellenen bağlantılar|Count|Toplam|Güvenlik duvarı kuralları tarafından engellenen bağlantı sayısı. SQL havuzunuz için erişim denetim ilkelerini yeniden ziyaret edin ve sayı yüksekse bu bağlantıları izleyin|Boyut yok|
|DWULimit|Hayır|DWU sınırı|Count|Maksimum|SQL havuzunun hizmet düzeyi hedefi|Boyut yok|
|Kullanılan DWU|Hayır|Kullanılan DWU|Count|Maksimum|SQL havuzu genelinde kullanım üst düzey gösterimini temsil eder. DWU sınırına göre ölçülür * DWU yüzdesi|Boyut yok|
|DWUUsedPercent|Hayır|Kullanılan DWU yüzdesi|Yüzde|Maksimum|SQL havuzu genelinde kullanım üst düzey gösterimini temsil eder. CPU yüzdesi ile veri GÇ yüzdesi arasındaki en yüksek değer alınarak ölçülür|Boyut yok|
|LocalTempDBUsedPercent|Hayır|Yerel tempdb kullanılan yüzde|Yüzde|Maksimum|Tüm işlem düğümlerinde yerel tempdb kullanımı-değerler her beş dakikada bir dağıtılır|Boyut yok|
|MemoryUsedPercent|Hayır|Kullanılan bellek yüzdesi|Yüzde|Maksimum|SQL havuzundaki tüm düğümlerde bellek kullanımı|Boyut yok|
|wlg_effective_min_resource_percent|Evet|Geçerli Min kaynak yüzdesi|Yüzde|Minimum|Etkin Min kaynak yüzdesi ayarı, hizmet düzeyini ve iş yükü grubu ayarlarını göz önünde bulundur, izin verildi. Etkin min_percentage_resource daha düşük hizmet düzeylerinde ayarlanabilir|IsUserDefined, WorkloadGroup|
|WLGActiveQueries|Hayır|İş yükü grubu etkin sorguları|Count|Toplam|İş yükü grubu içindeki etkin sorgular. Bu ölçüyü filtrelenmemiş ve Böl, sistemde çalışan tüm etkin sorguları görüntüler|IsUserDefined, WorkloadGroup|
|Wlgactivequerieszamanaşımları|Hayır|İş yükü grubu sorgu zaman aşımları|Count|Toplam|Zaman aşımına uğramış iş yükü grubu için sorgular. Bu ölçüm tarafından bildirilen sorgu zaman aşımları yalnızca sorgu yürütülmeye başladıktan sonra (kilitleme veya kaynak bekleme nedeniyle bekleme zamanı içermez)|IsUserDefined, WorkloadGroup|
|WLGAllocationByMaxResourcePercent|Hayır|Maksimum kaynak yüzdesi bazında iş yükü grubu ayırması|Yüzde|Maksimum|Toplam iş yükü grubu başına düşen etkin sınır kaynağına göre kaynakların yüzde ayırmasını görüntüler. Bu ölçüm, iş yükü grubunun etkin kullanımını sağlar|IsUserDefined, WorkloadGroup|
|Wlgallocationbysystempersent|Hayır|Sistem iş yükü grubuna göre ayırma yüzdesi|Yüzde|Maksimum|Tüm sisteme göre kaynakların yüzde tahsisi|IsUserDefined, WorkloadGroup|
|WLGEffectiveCapResourcePercent|Evet|Etkin uç kaynak yüzdesi|Yüzde|Maksimum|İş yükü grubu için geçerli uç kaynak yüzdesi. Min_percentage_resource > 0 olan başka iş yükü grupları varsa effective_cap_percentage_resource orantılı bir şekilde düşürülemez|IsUserDefined, WorkloadGroup|
|WLGQueuedQueries|Hayır|İş yükü grubu sıraya alınmış sorgular|Count|Toplam|En fazla eşzamanlılık sınırına ulaşıldıktan sonra sıraya alınan toplam istek sayısı|IsUserDefined, WorkloadGroup|


## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft. Timeseriesınsights/ortamlar

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Alınan baytlar|Evet|Alınan bayt sayısı|Bayt|Toplam|Tüm olay kaynaklarından okunan bayt sayısı|Boyut yok|
|Inressreceiveınvalidmessages|Evet|Giriş geçersiz Iletiler aldı|Count|Toplam|Tüm Olay Hub 'ından veya IoT Hub olay kaynaklarından okunan geçersiz ileti sayısı|Boyut yok|
|Inressreceived Iletileri|Evet|Giriş alınan Iletiler|Count|Toplam|Tüm Olay Hub 'ından veya IoT Hub olay kaynaklarından okunan ileti sayısı|Boyut yok|
|Inressreceived Iletisayaçgecikmesi|Evet|Alınan Ileti sayısı gecikmesi|Count|Ortalama|Olay kaynak bölümünde en son sıraya alınmış iletinin sıra numarası ve giriş bölümünde işlenmekte olan iletilerin sıra numarası arasındaki fark|Boyut yok|
|Inressreceived Iletizamangecikmesi|Evet|Alınan Ileti zaman gecikmesi|Saniye|Maksimum|İleti olay kaynağında sıraya alındığı zaman ve giriş sırasında işlendiği zaman arasındaki fark|Boyut yok|
|IngressStoredBytes|Evet|Giriş depolanan baytlar|Bayt|Toplam|Başarılı bir şekilde işlenen ve sorgu için kullanılabilir olayların toplam boyutu|Boyut yok|
|Inressstoo Vents|Evet|Giriş saklı olayları|Count|Toplam|Sorgu için başarıyla işlenen ve kullanılabilir düzleştirilmiş olay sayısı|Boyut yok|
|WarmStorageMaxProperties|Evet|Isınma depolama en yüksek özellikleri|Count|Maksimum|S1/S2 SKU 'SU için ortam tarafından izin verilen en fazla özellik sayısı ve PAYG SKU 'SU için ısınma tarafından izin verilen en fazla özellik sayısı|Boyut yok|
|WarmStorageUsedProperties|Evet|Sıcak depolama kullanılan özellikler |Count|Maksimum|S1/S2 SKU 'SU için ortam tarafından kullanılan özellik sayısı ve PAYG SKU 'SU için sıcak mağaza tarafından kullanılan özellik sayısı|Boyut yok|


## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft. Timeseriesınsights/ortamlar/EventSources

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Alınan baytlar|Evet|Alınan bayt sayısı|Bayt|Toplam|Olay kaynağından okunan bayt sayısı|Boyut yok|
|Inressreceiveınvalidmessages|Evet|Giriş geçersiz Iletiler aldı|Count|Toplam|Olay kaynağından okunan geçersiz ileti sayısı|Boyut yok|
|Inressreceived Iletileri|Evet|Giriş alınan Iletiler|Count|Toplam|Olay kaynağından okunan ileti sayısı|Boyut yok|
|Inressreceived Iletisayaçgecikmesi|Evet|Alınan Ileti sayısı gecikmesi|Count|Ortalama|Olay kaynak bölümünde en son sıraya alınmış iletinin sıra numarası ve giriş bölümünde işlenmekte olan iletilerin sıra numarası arasındaki fark|Boyut yok|
|Inressreceived Iletizamangecikmesi|Evet|Alınan Ileti zaman gecikmesi|Saniye|Maksimum|İleti olay kaynağında sıraya alındığı zaman ve giriş sırasında işlendiği zaman arasındaki fark|Boyut yok|
|IngressStoredBytes|Evet|Giriş depolanan baytlar|Bayt|Toplam|Başarılı bir şekilde işlenen ve sorgu için kullanılabilir olayların toplam boyutu|Boyut yok|
|Inressstoo Vents|Evet|Giriş saklı olayları|Count|Toplam|Sorgu için başarıyla işlenen ve kullanılabilir düzleştirilmiş olay sayısı|Boyut yok|
|WarmStorageMaxProperties|Evet|Isınma depolama en yüksek özellikleri|Count|Maksimum|S1/S2 SKU 'SU için ortam tarafından izin verilen en fazla özellik sayısı ve PAYG SKU 'SU için ısınma tarafından izin verilen en fazla özellik sayısı|Boyut yok|
|WarmStorageUsedProperties|Evet|Sıcak depolama kullanılan özellikler |Count|Maksimum|S1/S2 SKU 'SU için ortam tarafından kullanılan özellik sayısı ve PAYG SKU 'SU için sıcak mağaza tarafından kullanılan özellik sayısı|Boyut yok|


## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft. Vmwarechoparlör basit/virtualMachines

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Disk okuma bayt sayısı|Evet|Disk okuma bayt sayısı|Bayt|Toplam|Örnek süre boyunca okuma işlemleri nedeniyle toplam disk aktarım hızı.|Boyut yok|
|Disk okuma Işlemi/sn|Evet|Disk okuma Işlemi/sn|Sayaçpersaniye|Ortalama|Önceki örnek dönemdeki ortalama GÇ okuma işlemi sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Boyut yok|
|Disk yazma baytları|Evet|Disk yazma baytları|Bayt|Toplam|Örnek süre içinde yazma işlemleri nedeniyle toplam disk aktarım hızı.|Boyut yok|
|Disk yazma Işlemi/sn|Evet|Disk yazma Işlemi/sn|Sayaçpersaniye|Ortalama|Önceki örnek dönemdeki ortalama GÇ yazma işlemi sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Boyut yok|
|DiskReadBytesPerSecond|Evet|Disk okuma bayt/sn|BytesPerSecond|Ortalama|Örnek süre içinde okuma işlemleri nedeniyle ortalama disk aktarım hızı.|Boyut yok|
|DiskReadLatency|Evet|Disk okuma gecikmesi|Mayacak|Ortalama|Toplam okuma gecikmesi. Cihaz ve çekirdek okuma gecikme sürelerinin toplamı.|Boyut yok|
|DiskReadOperations|Evet|Disk okuma Işlemleri|Count|Toplam|Önceki örnek dönemdeki GÇ okuma işlemlerinin sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Boyut yok|
|DiskWriteBytesPerSecond|Evet|Disk yazma bayt/sn|BytesPerSecond|Ortalama|Örnek süre içinde yazma işlemleri nedeniyle ortalama disk aktarım hızı.|Boyut yok|
|DiskWriteLatency|Evet|Disk yazma gecikmesi|Mayacak|Ortalama|Toplam yazma gecikmesi. Cihaz ve çekirdek yazma gecikme sürelerinin toplamı.|Boyut yok|
|DiskWriteOperations|Evet|Disk yazma Işlemleri|Count|Toplam|Önceki örnek dönemdeki GÇ Yazma işlemlerinin sayısı. Bu işlemlerin değişken boyutlandırılabilir olabileceğini unutmayın.|Boyut yok|
|MemoryActive|Evet|Bellek etkin|Bayt|Ortalama|Son küçük zaman penceresinde VM tarafından kullanılan bellek miktarı. Bu, VM 'nin şu anda ihtiyacı olan bellek miktarını "doğru" sayısıdır. Ek olarak kullanılmayan bellek, konuğun performansına hiçbir etkilenmeden dışarı veya ballooned olabilir.|Boyut yok|
|Memoryverildi|Evet|Verilen bellek|Bayt|Ortalama|VM 'ye konak tarafından verilen bellek miktarı. Bir kez dokunulmadan ve VMkernel belleğe ihtiyaç duyuncaya kadar bellek verilebilene kadar bu konağa bellek verilmez.|Boyut yok|
|MemoryUsed|Evet|Kullanılan bellek|Bayt|Ortalama|VM tarafından kullanılan makine belleği miktarı.|Boyut yok|
|Ağ Girişi|Evet|Ağ Girişi|Bayt|Toplam|Alınan trafik için toplam ağ aktarım hızı.|Boyut yok|
|Ağ Çıkışı|Evet|Ağ Çıkışı|Bayt|Toplam|Aktarılan trafik için toplam ağ aktarım hızı.|Boyut yok|
|Networkınbytespersecond|Evet|Bayt/sn cinsinden ağ|BytesPerSecond|Ortalama|Alınan trafik için Ortalama ağ performansı.|Boyut yok|
|NetworkOutBytesPerSecond|Evet|Ağ çıkış bayt/sn|BytesPerSecond|Ortalama|Aktarılan trafik için Ortalama ağ aktarım hızı.|Boyut yok|
|CPU yüzdesi|Evet|CPU yüzdesi|Yüzde|Ortalama|CPU kullanımı. Bu değer, sistemdeki tüm işlemci çekirdeğini temsil eden %100 ile raporlanır. Örnek olarak, dört çekirdekli bir sistemin %50 ' ünü kullanan 2 yönlü bir VM tamamen iki çekirdek kullanmaktır.|Boyut yok|
|Yüztagecpuready|Evet|CPU için hazırlanma yüzdesi|Mayacak|Toplam|Hazır süre, CPU (ler) in geçmiş güncelleştirme aralığında kullanılabilir olmasını bekleyen zamanın harcadığı süredir.|Boyut yok|


## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft. Web/hostingEnvironments/multiRolePools

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|ActiveRequests|Evet|Etkin Istekler|Count|Toplam|Etkin Istekler|Örnek|
|AverageResponseTime|Evet|Ortalama yanıt süresi|Saniye|Ortalama|Ortalama yanıt süresi|Örnek|
|BytesReceived|Evet|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Evet|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|Cpuyüzdesi|Evet|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|DiskQueueLength|Evet|Disk kuyruğu uzunluğu|Count|Ortalama|Disk kuyruğu uzunluğu|Örnek|
|Http101|Evet|Http 101|Count|Toplam|Http 101|Örnek|
|Http2xx|Evet|Http 2xx|Count|Toplam|Http 2xx|Örnek|
|Http3xx|Evet|Http 3xx|Count|Toplam|Http 3xx|Örnek|
|Http401|Evet|Http 401|Count|Toplam|Http 401|Örnek|
|Http403|Evet|Http 403|Count|Toplam|Http 403|Örnek|
|Http404|Evet|HTTP 404|Count|Toplam|HTTP 404|Örnek|
|Http406|Evet|Http 406|Count|Toplam|Http 406|Örnek|
|Http4xx|Evet|Http 4xx|Count|Toplam|Http 4xx|Örnek|
|Http5xx|Evet|Http sunucu hataları|Count|Toplam|Http sunucu hataları|Örnek|
|HttpQueueLength|Evet|Http kuyruğu uzunluğu|Count|Ortalama|Http kuyruğu uzunluğu|Örnek|
|Largeappserviceplanınstances|Evet|Büyük App Service çalışanları planlıyor|Count|Ortalama|Büyük App Service çalışanları planlıyor|Boyut yok|
|Mediumappserviceplanınstances|Evet|Orta App Service çalışanları planlıyor|Count|Ortalama|Orta App Service çalışanları planlıyor|Boyut yok|
|MemoryPercentage|Evet|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Örnek|
|İstekler|Evet|İstekler|Count|Toplam|İstekler|Örnek|
|Smallappserviceplanınstances|Evet|Küçük App Service çalışanları planlıyor|Count|Ortalama|Küçük App Service çalışanları planlıyor|Boyut yok|
|Toplam ön uçlar|Evet|Toplam ön uçlar|Count|Ortalama|Toplam ön uçlar|Boyut yok|


## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft. Web/hostingEnvironments/workerPools

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|Cpuyüzdesi|Evet|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|MemoryPercentage|Evet|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Örnek|
|WorkersAvailable|Evet|Kullanılabilir çalışanlar|Count|Ortalama|Kullanılabilir çalışanlar|Boyut yok|
|WorkersTotal|Evet|Toplam çalışan sayısı|Count|Ortalama|Toplam çalışan sayısı|Boyut yok|
|WorkersUsed|Evet|Kullanılan çalışanlar|Count|Ortalama|Kullanılan çalışanlar|Boyut yok|


## <a name="microsoftwebserverfarms"></a>Microsoft. Web/sunucugrupları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|BytesReceived|Evet|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Evet|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|Cpuyüzdesi|Evet|CPU Yüzdesi|Yüzde|Ortalama|CPU Yüzdesi|Örnek|
|DiskQueueLength|Evet|Disk kuyruğu uzunluğu|Count|Ortalama|Disk kuyruğu uzunluğu|Örnek|
|HttpQueueLength|Evet|Http kuyruğu uzunluğu|Count|Ortalama|Http kuyruğu uzunluğu|Örnek|
|MemoryPercentage|Evet|Bellek yüzdesi|Yüzde|Ortalama|Bellek yüzdesi|Örnek|
|Socketınboundall|Evet|Socketınboundall|Count|Ortalama|Socketınboundall|Örnek|
|SocketLoopback|Evet|SocketLoopback|Count|Ortalama|SocketLoopback|Örnek|
|SocketOutboundAll|Evet|SocketOutboundAll|Count|Ortalama|SocketOutboundAll|Örnek|
|Socketoutsıçramaed|Evet|Socketoutsıçramaed|Count|Ortalama|Socketoutsıçramaed|Örnek|
|SocketOutboundTimeWait|Evet|SocketOutboundTimeWait|Count|Ortalama|SocketOutboundTimeWait|Örnek|
|TcpCloseWait|Evet|TCP kapatma bekleme|Count|Ortalama|TCP kapatma bekleme|Örnek|
|TcpClosing|Evet|TCP kapatma|Count|Ortalama|TCP kapatma|Örnek|
|Tcpkurdu|Evet|TCP oluşturuldu|Count|Ortalama|TCP oluşturuldu|Örnek|
|TcpFinWait1|Evet|TCP Fin bekleme 1|Count|Ortalama|TCP Fin bekleme 1|Örnek|
|TcpFinWait2|Evet|TCP Fin bekleme 2|Count|Ortalama|TCP Fin bekleme 2|Örnek|
|TcpLastAck|Evet|TCP son ACK|Count|Ortalama|TCP son ACK|Örnek|
|Tcpsynalındı|Evet|TCP SYN alındı|Count|Ortalama|TCP SYN alındı|Örnek|
|TcpSynSent|Evet|TCP SYN gönderilen|Count|Ortalama|TCP SYN gönderilen|Örnek|
|TcpTimeWait|Evet|TCP zaman bekleme|Count|Ortalama|TCP zaman bekleme|Örnek|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft. Web/Sites (işlevler hariç) 

> [!NOTE]
> **Dosya sistemi kullanımı** , genel olarak alınan yeni bir ölçümdür, özel önizleme için beyaz listeye alınmadığınız takdirde hiçbir veri beklenmez.

> [!IMPORTANT]
> Ölçüm toplamalarının karışmasını önlemek için **Ortalama yanıt süresi** kullanım dışı olacaktır. **Yanıt süresini** değiştirme olarak kullanın.

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AppConnections|Evet|Bağlantılar|Count|Ortalama|Bağlantılar|Örnek|
|AverageMemoryWorkingSet|Evet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Evet|Ortalama yanıt süresi **(kullanım dışı)**|Saniye|Ortalama|Ortalama yanıt süresi|Örnek|
|BytesReceived|Evet|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Evet|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|CpuTime|Evet|CPU süresi|Saniye|Toplam|CPU süresi|Örnek|
|CurrentAssemblies|Evet|Geçerli derlemeler|Count|Ortalama|Geçerli derlemeler|Örnek|
|Filesystemage|Evet|Dosya sistemi kullanımı|Bayt|Ortalama|Dosya sistemi kullanımı|Boyut yok|
|Gen0Collections|Evet|Gen 0 çöp koleksiyonları|Count|Toplam|Gen 0 çöp koleksiyonları|Örnek|
|Gen1Collections|Evet|Gen 1 çöp koleksiyonları|Count|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Evet|Gen 2 çöp koleksiyonları|Count|Toplam|Gen 2 çöp koleksiyonları|Örnek|
|Handles|Evet|Tanıtıcı Sayısı|Count|Ortalama|Tanıtıcı Sayısı|Örnek|
|HealthCheckStatus|Evet|Durum denetimi durumu|Count|Ortalama|Durum denetimi durumu|Örnek|
|Http101|Evet|Http 101|Count|Toplam|Http 101|Örnek|
|Http2xx|Evet|Http 2xx|Count|Toplam|Http 2xx|Örnek|
|Http3xx|Http 3xx|Count|Toplam|Http 3xx|Örnek|
|Http401|Evet|Http 401|Count|Toplam|Http 401|Örnek|
|Http403|Evet|Http 403|Count|Toplam|Http 403|Örnek|
|Http404|Evet|HTTP 404|Count|Toplam|HTTP 404|Örnek|
|Http406|Evet|Http 406|Count|Toplam|Http 406|Örnek|
|Http4xx|Evet|Http 4xx|Count|Toplam|Http 4xx|Örnek|
|Http5xx|Evet|Http sunucu hataları|Count|Toplam|Http sunucu hataları|Örnek|
|HttpResponseTime|Evet|Yanıt süresi|Saniye|Ortalama|Yanıt süresi|Örnek|
|Iootherbytespersecond|Evet|GÇ diğer bayt/saniye|BytesPerSecond|Toplam|GÇ diğer bayt/saniye|Örnek|
|Iootheroperationspersecond|Evet|GÇ diğer Işlem/saniye|BytesPerSecond|Toplam|GÇ diğer Işlem/saniye|Örnek|
|Ioreadbytespersecond|Evet|GÇ okuma bayt/saniye|BytesPerSecond|Toplam|GÇ okuma bayt/saniye|Örnek|
|IoReadOperationsPerSecond|Evet|GÇ okuma Işlemi/saniye|BytesPerSecond|Toplam|GÇ okuma Işlemi/saniye|Örnek|
|Iowritebytespersecond|Evet|GÇ yazma bayt/saniye|BytesPerSecond|Toplam|GÇ yazma bayt/saniye|Örnek|
|Iowriteoperationspersecond|Evet|GÇ Yazma Işlemi/saniye|BytesPerSecond|Toplam|GÇ Yazma Işlemi/saniye|Örnek|
|MemoryWorkingSet|Evet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|PrivateBytes|Evet|Özel baytlar|Bayt|Ortalama|Özel baytlar|Örnek|
|İstekler|Evet|İstekler|Count|Toplam|İstekler|Örnek|
|RequestsInApplicationQueue|Evet|Uygulama kuyruğundaki istekler|Count|Ortalama|Uygulama kuyruğundaki istekler|Örnek|
|İş Parçacıkları|Evet|İş parçacığı sayısı|Count|Ortalama|İş parçacığı sayısı|Örnek|
|TotalAppDomains|Evet|Toplam uygulama etki alanları|Count|Ortalama|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Evet|Toplam yüklenmeyen uygulama etki alanları|Count|Ortalama|Toplam yüklenmeyen uygulama etki alanları|Örnek|

## <a name="microsoftwebsites-functions"></a>Microsoft. Web/Sites (işlevler)

> [!NOTE]
> **Dosya sistemi kullanımı** , genel olarak alınan yeni bir ölçümdür, özel önizleme için beyaz listeye alınmadığınız takdirde hiçbir veri beklenmez.

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AverageMemoryWorkingSet|Evet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|BytesReceived|Evet|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Evet|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|CurrentAssemblies|Evet|Geçerli derlemeler|Count|Ortalama|Geçerli derlemeler|Örnek|
|Filesystemage|Evet|Dosya sistemi kullanımı|Bayt|Ortalama|Dosya sistemi kullanımı|Boyut yok|
|Işlev yürütme sayısı|Evet|İşlev yürütme sayısı|Count|Toplam|İşlev yürütme sayısı|Örnek|
|FunctionExecutionUnits|Evet|İşlev yürütme birimleri|Count|Toplam|[İşlev yürütme birimleri](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Örnek|
|Gen0Collections|Evet|Gen 0 çöp koleksiyonları|Count|Toplam|Gen 0 çöp koleksiyonları|Örnek|
|Gen1Collections|Evet|Gen 1 çöp koleksiyonları|Count|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Evet|Gen 2 çöp koleksiyonları|Count|Toplam|Gen 2 çöp koleksiyonları|Örnek|
|HealthCheckStatus|Evet|Durum denetimi durumu|Count|Ortalama|Durum denetimi durumu|Örnek|
|Http5xx|Evet|Http sunucu hataları|Count|Toplam|Http sunucu hataları|Örnek|
|Iootherbytespersecond|Evet|GÇ diğer bayt/saniye|BytesPerSecond|Toplam|GÇ diğer bayt/saniye|Örnek|
|Iootheroperationspersecond|Evet|GÇ diğer Işlem/saniye|BytesPerSecond|Toplam|GÇ diğer Işlem/saniye|Örnek|
|Ioreadbytespersecond|Evet|GÇ okuma bayt/saniye|BytesPerSecond|Toplam|GÇ okuma bayt/saniye|Örnek|
|IoReadOperationsPerSecond|Evet|GÇ okuma Işlemi/saniye|BytesPerSecond|Toplam|GÇ okuma Işlemi/saniye|Örnek|
|Iowritebytespersecond|Evet|GÇ yazma bayt/saniye|BytesPerSecond|Toplam|GÇ yazma bayt/saniye|Örnek|
|Iowriteoperationspersecond|Evet|GÇ Yazma Işlemi/saniye|BytesPerSecond|Toplam|GÇ Yazma Işlemi/saniye|Örnek|
|MemoryWorkingSet|Evet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|PrivateBytes|Evet|Özel baytlar|Bayt|Ortalama|Özel baytlar|Örnek|
|RequestsInApplicationQueue|Evet|Uygulama kuyruğundaki istekler|Count|Ortalama|Uygulama kuyruğundaki istekler|Örnek|
|TotalAppDomains|Evet|Toplam uygulama etki alanları|Count|Ortalama|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Evet|Toplam yüklenmeyen uygulama etki alanları|Count|Ortalama|Toplam yüklenmeyen uygulama etki alanları|Örnek|

## <a name="microsoftwebsitesslots"></a>Microsoft. Web/Sites/Yuvaları

|Ölçüm|Tanılama ayarları aracılığıyla dışarı aktarılabilir mi?|Ölçüm görünen adı|Birim|Toplama Türü|Açıklama|Boyutlar|
|---|---|---|---|---|---|---|
|AppConnections|Evet|Bağlantılar|Count|Ortalama|Bağlantılar|Örnek|
|AverageMemoryWorkingSet|Evet|Ortalama bellek çalışma kümesi|Bayt|Ortalama|Ortalama bellek çalışma kümesi|Örnek|
|AverageResponseTime|Evet|Ortalama yanıt süresi|Saniye|Ortalama|Ortalama yanıt süresi|Örnek|
|BytesReceived|Evet|Içindeki veriler|Bayt|Toplam|Içindeki veriler|Örnek|
|BytesSent|Evet|Giden veriler|Bayt|Toplam|Giden veriler|Örnek|
|CpuTime|Evet|CPU süresi|Saniye|Toplam|CPU süresi|Örnek|
|CurrentAssemblies|Evet|Geçerli derlemeler|Count|Ortalama|Geçerli derlemeler|Örnek|
|Filesystemage|Evet|Dosya sistemi kullanımı|Bayt|Ortalama|Dosya sistemi kullanımı|Boyut yok|
|Işlev yürütme sayısı|Evet|İşlev yürütme sayısı|Count|Toplam|İşlev yürütme sayısı|Örnek|
|FunctionExecutionUnits|Evet|İşlev yürütme birimleri|Count|Toplam|İşlev yürütme birimleri|Örnek|
|Gen0Collections|Evet|Gen 0 çöp koleksiyonları|Count|Toplam|Gen 0 çöp koleksiyonları|Örnek|
|Gen1Collections|Evet|Gen 1 çöp koleksiyonları|Count|Toplam|Gen 1 çöp koleksiyonları|Örnek|
|Gen2Collections|Evet|Gen 2 çöp koleksiyonları|Count|Toplam|Gen 2 çöp koleksiyonları|Örnek|
|Handles|Evet|Tanıtıcı Sayısı|Count|Ortalama|Tanıtıcı Sayısı|Örnek|
|HealthCheckStatus|Evet|Durum denetimi durumu|Count|Ortalama|Durum denetimi durumu|Örnek|
|Http101|Evet|Http 101|Count|Toplam|Http 101|Örnek|
|Http2xx|Evet|Http 2xx|Count|Toplam|Http 2xx|Örnek|
|Http3xx|Evet|Http 3xx|Count|Toplam|Http 3xx|Örnek|
|Http401|Evet|Http 401|Count|Toplam|Http 401|Örnek|
|Http403|Evet|Http 403|Count|Toplam|Http 403|Örnek|
|Http404|Evet|HTTP 404|Count|Toplam|HTTP 404|Örnek|
|Http406|Evet|Http 406|Count|Toplam|Http 406|Örnek|
|Http4xx|Evet|Http 4xx|Count|Toplam|Http 4xx|Örnek|
|Http5xx|Evet|Http sunucu hataları|Count|Toplam|Http sunucu hataları|Örnek|
|HttpResponseTime|Evet|Yanıt süresi|Saniye|Ortalama|Yanıt süresi|Örnek|
|Iootherbytespersecond|Evet|GÇ diğer bayt/saniye|BytesPerSecond|Toplam|GÇ diğer bayt/saniye|Örnek|
|Iootheroperationspersecond|Evet|GÇ diğer Işlem/saniye|BytesPerSecond|Toplam|GÇ diğer Işlem/saniye|Örnek|
|Ioreadbytespersecond|Evet|GÇ okuma bayt/saniye|BytesPerSecond|Toplam|GÇ okuma bayt/saniye|Örnek|
|IoReadOperationsPerSecond|Evet|GÇ okuma Işlemi/saniye|BytesPerSecond|Toplam|GÇ okuma Işlemi/saniye|Örnek|
|Iowritebytespersecond|Evet|GÇ yazma bayt/saniye|BytesPerSecond|Toplam|GÇ yazma bayt/saniye|Örnek|
|Iowriteoperationspersecond|Evet|GÇ Yazma Işlemi/saniye|BytesPerSecond|Toplam|GÇ Yazma Işlemi/saniye|Örnek|
|MemoryWorkingSet|Evet|Bellek çalışma kümesi|Bayt|Ortalama|Bellek çalışma kümesi|Örnek|
|PrivateBytes|Evet|Özel baytlar|Bayt|Ortalama|Özel baytlar|Örnek|
|İstekler|Evet|İstekler|Count|Toplam|İstekler|Örnek|
|RequestsInApplicationQueue|Evet|Uygulama kuyruğundaki istekler|Count|Ortalama|Uygulama kuyruğundaki istekler|Örnek|
|İş Parçacıkları|Evet|İş parçacığı sayısı|Count|Ortalama|İş parçacığı sayısı|Örnek|
|TotalAppDomains|Evet|Toplam uygulama etki alanları|Count|Ortalama|Toplam uygulama etki alanları|Örnek|
|TotalAppDomainsUnloaded|Evet|Toplam yüklenmeyen uygulama etki alanları|Count|Ortalama|Toplam yüklenmeyen uygulama etki alanları|Örnek|


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Izleyici 'de ölçümler hakkında bilgi edinin](data-platform.md)
* [Ölçümler üzerinde uyarı oluşturma](alerts-overview.md)
* [Ölçümleri depolama, Olay Hub 'ına veya Log Analytics Dışa aktarma](platform-logs-overview.md)
