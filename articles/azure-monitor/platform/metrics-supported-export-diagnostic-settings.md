---
title: NULL ve sıfır değerleriyle ölçümler dışarı aktarma davranışı
description: Ölçüm dışarı aktarılırken NULL ve sıfır değerlerinin tartışılması ve ölçüm dışa aktarılabilir olmayan bir liste işaretçisi.
services: azure-monitor
ms.topic: reference
ms.date: 07/22/2020
ms.subservice: metrics
ms.openlocfilehash: ca6acb97e52123a0663d988b3f217d305bce2c4b
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87131693"
---
# <a name="azure-monitor-platform-metrics-exportable-via-diagnostic-settings"></a>Azure Izleyici platformu ölçümleri, Tanılama ayarları aracılığıyla dışarı aktarılabilir

Azure Izleyici, varsayılan olarak hiçbir yapılandırma olmadan [Platform ölçümleri](data-platform-metrics.md) sağlar. Bu, portalda grafik oluşturma, REST API aracılığıyla erişme veya PowerShell veya CLı kullanarak sorgulama dahil olmak üzere platform ölçümleriyle etkileşimde bulunmak için çeşitli yollar sağlar. Bkz. [ölçümler-](metrics-supported.md) Azure izleyicisinin birleştirilmiş ölçüm işlem hattı ile Şu anda kullanılabilir olan platform ölçümlerinin tam listesi için desteklenir. Bu ölçümleri sorgulamak ve bunlara erişmek için lütfen [2018-01-01 api sürümünü](/rest/api/monitor/metricdefinitions)kullanın. Diğer ölçümler portalda veya eski API 'Ler kullanılarak kullanılabilir.

## <a name="metrics-not-exportable-via-diagnostic-settings"></a>Ölçümler, Tanılama ayarları aracılığıyla dışarı aktarılabilir değil

Bu konumda olması için kullanılan içerik [Azure Izleyici ölçümlerinin desteklenen listesine](metrics-supported.md#exporting-platform-metrics-to-other-locations)taşındı.

Tanılama ayarları aracılığıyla ölçümleri dışarı aktarırken sınırlamalar vardır. Tüm ölçümler REST API kullanılarak dışarı aktarılabilir. 

## <a name="exported-zero-vs-null-values"></a>Sıfır ve NULL değerleri içe aktarılmış 

Ölçümler 0 veya NULL değerlerle ilgilenirken farklı davranışa sahiptir.  Bir veri alınamıyorsa bazı ölçümler 0 rapor, örneğin, http hatalarıyla ilgili ölçümler. Başka ölçümler, kaynağın çevrimdışı olduğunu belirtebileceğinden, hiçbir veri alınmadığı zaman null olur. Bu ölçümleri, [kesikli çizgiler](metrics-troubleshoot.md#chart-shows-dashed-line)olarak gösterilen null değerlerle birlikte çizerek farkı görebilirsiniz. 

Platform ölçümleri, Tanılama ayarları aracılığıyla verilebileceği zaman, ölçümün davranışıyla eşleşir. Diğer bir deyişle, kaynak veri gönderdiğinde null değerleri dışarı aktarırlar.  Yalnızca temel alınan kaynak tarafından gerçekten yayıldıklarında, ' 0 ' dışarı aktarır. 

Bir kaynak grubunu veya belirli bir kaynağı silerseniz, etkilenen kaynaklardaki ölçüm verileri artık tanılama ayarı dışarı aktarma hedeflerine gönderilmez. Diğer bir deyişle, artık Event Hubs, Azure depolama hesapları ve Log Analytics çalışma alanlarında görünmez.

### <a name="metrics-that-used-to-export-zero-for-null"></a>NULL için sıfır dışarı aktarmak için kullanılan ölçümler

1 Haziran 2020 ' den önce, aşağıdaki ölçümler, veri olmadığında ' 0 ' öğesini göstermek **için kullanılır** . Bu sıfırlar daha sonra Log Analytics ve Azure depolama gibi aşağı akış sistemlerine aktarılabilir.  Bu davranış, gerçek ' 0s ' (kaynak tarafından yayılan) ve yorumlanan ' 0s ' (null değerler) arasında bir karışıklıklara neden oldu ve bu nedenle, önceki bölümde belirtildiği gibi, davranışın temel alınan ölçümle eşleşecek şekilde değiştirilmiştir. 

Değişiklik tüm genel ve özel bulutlarda oluştu.

Değişiklik aşağıdaki deneyimlerden herhangi birinin davranışını etkilemeyecek: 
   - Tanılama ayarları aracılığıyla içe aktarılmış platform kaynak günlükleri
   - Ölçüm grafik Ölçüm Gezgini
   - Platform ölçümleri üzerinde uyarı verme
 
Davranışı değişmiş olan ölçümlerin bir listesi aşağıda verilmiştir. 

| ResourceType                    | Ölçüm               |  MetricDisplayName  | 
|---------------------------------|----------------------|---------------------|
| Microsoft.ApiManagement/service | UnauthorizedRequests |  Yetkisiz ağ geçidi Istekleri (kullanım dışı)  | 
| Microsoft.ApiManagement/service | TotalRequests |  Toplam ağ geçidi Isteği sayısı (kullanım dışı)  | 
| Microsoft.ApiManagement/service | SuccessfulRequests |  Başarılı ağ geçidi Istekleri (kullanım dışı)  | 
| Microsoft.ApiManagement/service | İstekler |  İstekler  | 
| Microsoft.ApiManagement/service | Diğer Istekler |  Diğer ağ geçidi Istekleri (kullanım dışı)  | 
| Microsoft.ApiManagement/service | FailedRequests |  Başarısız ağ geçidi Istekleri (kullanım dışı)  | 
| Microsoft.ApiManagement/service | EventHubTotalFailedEvents |  Başarısız EventHub olayları  | 
| Microsoft.ApiManagement/service | EventHubTotalEvents |  Toplam EventHub olayları  | 
| Microsoft.ApiManagement/service | EventHubTotalBytesSent |  EventHub olaylarının boyutu  | 
| Microsoft.ApiManagement/service | EventHubTimedoutEvents |  Zaman aşımına uğrayan EventHub olayları  | 
| Microsoft.ApiManagement/service | EventHubThrottledEvents |  Kısıtlanmış EventHub olayları  | 
| Microsoft.ApiManagement/service | Eventhubbaşarılı Futavents |  Başarılı EventHub olayları  | 
| Microsoft.ApiManagement/service | EventHubRejectedEvents |  Reddedilen EventHub olayları  | 
| Microsoft.ApiManagement/service | EventHubDroppedEvents |  Bırakılan EventHub olayları  | 
| Microsoft.ApiManagement/service | Süre |  Ağ Geçidi Isteklerinin genel süresi  | 
| Microsoft.ApiManagement/service | BackendDuration |  Arka uç Isteklerinin süresi  | 
| Microsoft. Dbformarıdb/sunucular | storage_used |  Kullanılan depolama alanı  | 
| Microsoft. Dbformarıdb/sunucular | storage_percent |  Depolama alanı yüzdesi  | 
| Microsoft. Dbformarıdb/sunucular | storage_limit |  Depolama sınırı  | 
| Microsoft. Dbformarıdb/sunucular | serverlog_storage_usage |  Kullanılan sunucu günlüğü depolaması  | 
| Microsoft. Dbformarıdb/sunucular | serverlog_storage_percent |  Sunucu günlüğü depolama yüzdesi  | 
| Microsoft. Dbformarıdb/sunucular | serverlog_storage_limit |  Sunucu günlüğü depolama sınırı  | 
| Microsoft. Dbformarıdb/sunucular | seconds_behind_master |  Saniye cinsinden çoğaltma gecikmesi  | 
| Microsoft. Dbformarıdb/sunucular | network_bytes_ingress |  Ağ Girişi  | 
| Microsoft. Dbformarıdb/sunucular | network_bytes_egress |  Ağ Çıkışı  | 
| Microsoft. Dbformarıdb/sunucular | memory_percent |  Bellek yüzdesi  | 
| Microsoft. Dbformarıdb/sunucular | io_consumption_percent |  GÇ yüzdesi  | 
| Microsoft. Dbformarıdb/sunucular | cpu_percent |  CPU yüzdesi  | 
| Microsoft. Dbformarıdb/sunucular | connections_failed |  Başarısız Bağlantılar  | 
| Microsoft. Dbformarıdb/sunucular | backup_storage_used |  Kullanılan yedekleme depolama alanı  | 
| Microsoft. Dbformarıdb/sunucular | active_connections |  Etkin Bağlantılar  | 
| Microsoft. Dbformyısql/sunucuları | storage_used |  Kullanılan depolama alanı  | 
| Microsoft. Dbformyısql/sunucuları | storage_percent |  Depolama alanı yüzdesi  | 
| Microsoft. Dbformyısql/sunucuları | storage_limit |  Depolama sınırı  | 
| Microsoft. Dbformyısql/sunucuları | serverlog_storage_usage |  Kullanılan sunucu günlüğü depolaması  | 
| Microsoft. Dbformyısql/sunucuları | serverlog_storage_percent |  Sunucu günlüğü depolama yüzdesi  | 
| Microsoft. Dbformyısql/sunucuları | serverlog_storage_limit |  Sunucu günlüğü depolama sınırı  | 
| Microsoft. Dbformyısql/sunucuları | seconds_behind_master |  Saniye cinsinden çoğaltma gecikmesi  | 
| Microsoft. Dbformyısql/sunucuları | network_bytes_ingress |  Ağ Girişi  | 
| Microsoft. Dbformyısql/sunucuları | network_bytes_egress |  Ağ Çıkışı  | 
| Microsoft. Dbformyısql/sunucuları | memory_percent |  Bellek yüzdesi  | 
| Microsoft. Dbformyısql/sunucuları | io_consumption_percent |  GÇ yüzdesi  | 
| Microsoft. Dbformyısql/sunucuları | cpu_percent |  CPU yüzdesi  | 
| Microsoft. Dbformyısql/sunucuları | connections_failed |  Başarısız Bağlantılar  | 
| Microsoft. Dbformyısql/sunucuları | backup_storage_used |  Kullanılan yedekleme depolama alanı  | 
| Microsoft. Dbformyısql/sunucuları | active_connections |  Etkin Bağlantılar  | 
| Microsoft. Devices/Account | digitaltwins. telemetri. düğümleri |  Dijital TWINS düğümü telemetri yer tutucusu  | 
| Microsoft. Devices/IotHubs | twinQueries. Success |  Başarılı ikizi sorguları  | 
| Microsoft. Devices/IotHubs | twinQueries. resultSize |  İkizi sorguları sonuç boyutu  | 
| Microsoft. Devices/IotHubs | twinQueries. Failure |  Başarısız ikizi sorguları  | 
| Microsoft. Devices/IotHubs | işler. queryJobs. başarılı |  Başarılı iş sorguları  | 
| Microsoft. Devices/IotHubs | Jobs. queryJobs. hata |  Başarısız iş sorguları  | 
| Microsoft. Devices/IotHubs | işler. listJobs. başarılı |  İşleri listelemek için başarılı çağrılar  | 
| Microsoft. Devices/IotHubs | Jobs. listJobs. hata |  İşleri listelemek için başarısız çağrılar  | 
| Microsoft. Devices/IotHubs | işler. başarısız |  Başarısız işler  | 
| Microsoft. Devices/IotHubs | Jobs. createTwinUpdateJob. Success |  İkizi Update işlerinin başarılı oluşturmaları  | 
| Microsoft. Devices/IotHubs | Jobs. createTwinUpdateJob. Failure |  İkizi Update işlerinin başarısız oluşturmaları  | 
| Microsoft. Devices/IotHubs | Jobs. createDirectMethodJob. Success |  Yöntem çağırma işlerinin başarılı oluşturmaları  | 
| Microsoft. Devices/IotHubs | Jobs. createDirectMethodJob. Failure |  Yöntem çağırma işlerinin oluşturma işlemi başarısız oldu  | 
| Microsoft. Devices/IotHubs | işler. tamamlandı |  Tamamlanan İşler  | 
| Microsoft. Devices/IotHubs | Jobs. cancelJob. Success |  Başarılı iş iptalleri  | 
| Microsoft. Devices/IotHubs | Jobs. cancelJob. Failure |  Başarısız iş iptalleri  | 
| Microsoft. Devices/IotHubs | EventGridLatency |  Event Grid gecikme süresi (Önizleme)  | 
| Microsoft. Devices/IotHubs | Eventgridteslimler |  Event Grid teslimler (Önizleme)  | 
| Microsoft. Devices/IotHubs | Devices. totalDevices |  Toplam cihaz (kullanım dışı)  | 
| Microsoft. Devices/IotHubs | Devices. connectedDevices. allProtocol |  Bağlı cihazlar (kullanım dışı)   | 
| Microsoft. Devices/IotHubs | deviceDataUsageV2 |  Toplam cihaz verisi kullanımı (Önizleme)  | 
| Microsoft. Devices/IotHubs | deviceDataUsage |  Toplam cihaz verisi kullanımı  | 
| Microsoft. Devices/IotHubs | Günlükmessagequotakullanıldı |  Kullanılan toplam ileti sayısı  | 
| Microsoft. Devices/IotHubs | D2C. ikizi. Update. Success |  Cihazlardan başarılı ikizi güncelleştirmeleri  | 
| Microsoft. Devices/IotHubs | D2C. ikizi. Update. size |  Cihazlardan ikizi güncelleştirmelerinin boyutu  | 
| Microsoft. Devices/IotHubs | D2C. ikizi. Update. Failure |  Cihazlardan ikizi güncelleştirmeleri başarısız oldu  | 
| Microsoft. Devices/IotHubs | D2C. ikizi. Read. Success |  Cihazlardan başarılı ikizi okumaları  | 
| Microsoft. Devices/IotHubs | D2C. ikizi. Read. size |  Cihazlardan gelen ikizi okumaların yanıt boyutu  | 
| Microsoft. Devices/IotHubs | D2C. ikizi. Read. Failure |  Cihazlardan başarısız ikizi okumaları  | 
| Microsoft. Devices/IotHubs | D2C. telemetri. giriş. başarılı |  Gönderilen telemetri iletileri  | 
| Microsoft. Devices/IotHubs | D2C. telemetri. ınress. Sendvaliz |  Daraltma hatası sayısı  | 
| Microsoft. Devices/IotHubs | D2C. telemetri. ınress. allProtocol |  Telemetri iletisi gönderme denemeleri  | 
| Microsoft. Devices/IotHubs | D2C. telemetri. çıkış. başarılı |  Yönlendirme: teslim edilen telemetri iletileri  | 
| Microsoft. Devices/IotHubs | D2C. telemetri. çıkış. yalnız bırakılmış |  Yönlendirme: telemetri iletileri yalnız bırakılmış   | 
| Microsoft. Devices/IotHubs | D2C. telemetri. çıkış. geçersiz |  Yönlendirme: telemetri iletileri uyumsuz  | 
| Microsoft. Devices/IotHubs | D2C. telemetri. çıkış. geri dönüş |  Yönlendirme: geri dönüşe teslim edilen iletiler  | 
| Microsoft. Devices/IotHubs | D2C. telemetri. çıkış. bırakıldı |  Yönlendirme: telemetri iletileri bırakıldı   | 
| Microsoft. Devices/IotHubs | D2C. endpoints. Latency. Storage |  Yönlendirme: depolama için ileti gecikmesi  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. Latency. Servicebuskonular |  Yönlendirme: Service Bus konusu için ileti gecikmesi  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. Latency. serviceBusQueues |  Yönlendirme: Service Bus kuyruğu için ileti gecikmesi  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. Latency. eventHubs |  Yönlendirme: Olay Hub 'ı için ileti gecikmesi  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. Latency. yerleik. Events |  Yönlendirme: iletiler/olaylar için ileti gecikmesi  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. çıkış. Storage. Bytes |  Yönlendirme: depolamaya teslim edilen veriler  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. çıkış. Storage. blob 'ları |  Yönlendirme: depolamaya teslim edilen Bloblar  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. çıkış. Storage |  Yönlendirme: depolamaya teslim edilen iletiler  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. çıkış. Servicebuskonular |  Yönlendirme: Service Bus konuya teslim edilen iletiler  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. çıkış. serviceBusQueues |  Yönlendirme: Service Bus kuyruğuna teslim edilen iletiler  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. çıkış. eventHubs |  Yönlendirme: Olay Hub 'ına teslim edilen iletiler  | 
| Microsoft. Devices/IotHubs | D2C. endpoints. çıkış. builtIn. Events |  Yönlendirme: iletilere/olaylara teslim edilen iletiler  | 
| Microsoft. Devices/IotHubs | konfigürasyonları |  Yapılandırma ölçümleri  | 
| Microsoft. Devices/IotHubs | C2DMessagesExpired |  C2D Iletilerinin süre dolma (Önizleme)  | 
| Microsoft. Devices/IotHubs | C2D. ikizi. Update. Success |  Arka uçtan başarılı ikizi güncelleştirmeleri  | 
| Microsoft. Devices/IotHubs | C2D. ikizi. Update. size |  Arka uçtan ikizi güncelleştirmelerinin boyutu  | 
| Microsoft. Devices/IotHubs | C2D. ikizi. Update. Failure |  Arka uçtan başarısız ikizi güncelleştirmeleri  | 
| Microsoft. Devices/IotHubs | C2D. ikizi. Read. Success |  Arka uçtan başarılı ikizi okumaları  | 
| Microsoft. Devices/IotHubs | C2D. ikizi. Read. size |  Arka uçtan gelen ikizi okumaların yanıt boyutu  | 
| Microsoft. Devices/IotHubs | C2D. ikizi. Read. Failure |  Arka uçtan başarısız ikizi okumaları  | 
| Microsoft. Devices/IotHubs | C2D. Methods. Success |  Başarılı doğrudan yöntem etkinleştirmeleri  | 
| Microsoft. Devices/IotHubs | C2D. Methods. responseSize |  Doğrudan yöntem etkinleştirmeleri yanıt boyutu  | 
| Microsoft. Devices/IotHubs | C2D. Methods. requestSize |  Doğrudan yöntem etkinleştirmeleri istek boyutu  | 
| Microsoft. Devices/IotHubs | C2D. Methods. Failure |  Başarısız doğrudan yöntem etkinleştirmeleri  | 
| Microsoft. Devices/IotHubs | C2D. Commands. çıkış. Red. Success |  C2D iletileri reddedildi  | 
| Microsoft. Devices/IotHubs | C2D. Commands. çıkış. tamamlandı. başarılı |  C2D ileti teslimleri tamamlandı  | 
| Microsoft. Devices/IotHubs | C2D. Commands. çıkış. Abandon. Success |  C2D iletileri bırakıldı  | 
| Microsoft. Devices/provisioningServices | Registrationdenemeleri |  Kayıt denemeleri  | 
| Microsoft. Devices/provisioningServices | Deviceasyleri |  Atanan cihazlar  | 
| Microsoft. Devices/provisioningServices | AttestationAttempts |  Kanıtlama denemeleri  | 
| UmentDB/databaseAccounts Microsoft.Doc | TotalRequestUnits |  Toplam Istek birimleri  | 
| UmentDB/databaseAccounts Microsoft.Doc | TotalRequests |  Toplam İstek Sayısı  | 
| UmentDB/databaseAccounts Microsoft.Doc | MongoRequests |  Mongo Istekleri  | 
| UmentDB/databaseAccounts Microsoft.Doc | Mongorequestşarj |  Mongo Istek ücreti  | 
| Microsoft. EventGrid/Domains | Publishbaşarıyla Latencınms |  Yayımlama başarı gecikmesi  | 
| Microsoft. EventGrid/Domains | Publishbaşarılı sayısı |  Yayımlanan olaylar  | 
| Microsoft. EventGrid/Domains | PublishFailCount |  Başarısız olayları Yayımla  | 
| Microsoft. EventGrid/Domains | MatchedEventCount |  Eşleşen olaylar  | 
| Microsoft. EventGrid/Domains | DroppedEventCount |  Bırakılan olaylar  | 
| Microsoft. EventGrid/Domains | Deliverybaşarılı sayısı |  Teslim edilen olaylar  | 
| Microsoft. EventGrid/Domains | DeadLetteredCount |  Kullanılmayan olaylar  | 
| Microsoft. EventGrid/Eventabonelikleri | MatchedEventCount |  Eşleşen olaylar  | 
| Microsoft. EventGrid/Eventabonelikleri | DroppedEventCount |  Bırakılan olaylar  | 
| Microsoft. EventGrid/Eventabonelikleri | Deliverybaşarılı sayısı |  Teslim edilen olaylar  | 
| Microsoft. EventGrid/Eventabonelikleri | DeadLetteredCount |  Kullanılmayan olaylar  | 
| Microsoft. EventGrid/Extensionkonuları | UnmatchedEventCount |  Eşleşmeyen olaylar  | 
| Microsoft. EventGrid/Extensionkonuları | Publishbaşarıyla Latencınms |  Yayımlama başarı gecikmesi  | 
| Microsoft. EventGrid/Extensionkonuları | Publishbaşarılı sayısı |  Yayımlanan olaylar  | 
| Microsoft. EventGrid/Extensionkonuları | PublishFailCount |  Başarısız olayları Yayımla  | 
| Microsoft. EventGrid/konuları | UnmatchedEventCount |  Eşleşmeyen olaylar  | 
| Microsoft. EventGrid/konuları | Publishbaşarıyla Latencınms |  Yayımlama başarı gecikmesi  | 
| Microsoft. EventGrid/konuları | Publishbaşarılı sayısı |  Yayımlanan olaylar  | 
| Microsoft. EventGrid/konuları | PublishFailCount |  Başarısız olayları Yayımla  | 
| Microsoft. EventHub/kümeler | OutgoingMessages |  Giden İletiler  | 
| Microsoft. EventHub/kümeler | OutgoingBytes |  Giden bayt sayısı.  | 
| Microsoft. EventHub/kümeler | Incomingrequests |  Gelen İstekler  | 
| Microsoft. EventHub/kümeler | Incomingmessages |  Gelen İletiler  | 
| Microsoft. EventHub/kümeler | Incomingbytes |  Gelen Bayt Miktarı  | 
| Microsoft. EventHub/ad alanları | SVRBSY |  Sunucu meşgul hataları (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | SUCCREQ |  Başarılı Istekler (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | Dış mesaj |  Giden Iletiler (eski) (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | OutgoingMessages |  Giden İletiler  | 
| Microsoft. EventHub/ad alanları | OutgoingBytes |  Giden bayt sayısı.  | 
| Microsoft. EventHub/ad alanları | Hatalı cerr |  Diğer hatalar (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | INTERR |  İç sunucu hataları (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | INREQS |  Gelen Istekler (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | Giriş iletileri |  Gelen Iletiler (eski) (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | Incomingrequests |  Gelen İstekler  | 
| Microsoft. EventHub/ad alanları | Incomingmessages |  Gelen İletiler  | 
| Microsoft. EventHub/ad alanları | Incomingbytes |  Gelen Bayt Miktarı  | 
| Microsoft. EventHub/ad alanları | FAILREQ |  Başarısız Istekler (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHODıKILETISI |  Giden Iletiler (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHOUTMB |  Giden baytlar (eski) (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHOUTBYTES |  Giden baytlar (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHINILETISI |  Gelen Iletiler (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHINMB |  Gelen bayt (geçersiz) (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHINBYTES |  Gelen baytlar (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHAMESAJ |  İletileri Arşivle (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHAMB |  Arşiv iletisi verimlilik (kullanım dışı)  | 
| Microsoft. EventHub/ad alanları | EHABL |  Biriktirme listesi iletilerini Arşivle (kullanım dışı)  | 
| Microsoft. HDInsight/kümeler | Numactiveçalışanları |  Etkin çalışan sayısı  | 
| Microsoft. HDInsight/kümeler | GatewayRequests |  Ağ Geçidi Istekleri  | 
| Microsoft. HDInsight/kümeler | Kategorizedgatewayrequests |  Kategorilere ayrılmış ağ geçidi Istekleri  | 
| Microsoft. Insights/oto Scalesettings | Scaleactionsınıated |  Ölçek eylemleri başlatıldı  | 
| Microsoft. Insights/bileşenler | izlemeler/say |  İzlemeler  | 
| Microsoft. Insights/bileşenler | performanceCounters/requestsPerSecond |  HTTP istek hızı  | 
| Microsoft. Insights/bileşenler | performanceCounters/Requestsınqueue |  Uygulama kuyruğundaki HTTP istekleri  | 
| Microsoft. Insights/bileşenler | performanceCounters/exceptionsPerSecond |  Özel durum oranı  | 
| Microsoft. Insights/bileşenler | pageViews/Count |  Sayfa görünümleri  | 
| Microsoft. Insights/bileşenler | özel durumlar/say |  Özel durumlar  | 
| Microsoft. kusto/kümeler | StreamingIngestResults |  Akış alma sonucu  | 
| Microsoft. kusto/kümeler | StreamingIngestDuration |  Akış alma süresi  | 
| Microsoft. kusto/kümeler | StreamingIngestDataRate |  Akış alma verileri oranı  | 
| Microsoft. kusto/kümeler | SteamingIngestRequestRate |  Akış alma Isteği oranı  | 
| Microsoft. kusto/kümeler | QueryDuration |  Sorgu süresi  | 
| Microsoft. kusto/kümeler | Alı |  Canlı tut  | 
| Microsoft. kusto/kümeler | Inestionvolumeınmb |  Alım birimi (MB)  | 
| Microsoft. kusto/kümeler | Alım kullanımı |  Alım kullanımı  | 
| Microsoft. kusto/kümeler | Inestionresult |  Alım sonucu  | 
| Microsoft. kusto/kümeler | Inestionlatencınseconds |  Alma gecikmesi (saniye)  | 
| Microsoft. kusto/kümeler | Dışarı Aktaramstilileştirme |  Dışarı Aktarma Kullanımı  | 
| Microsoft. kusto/kümeler | Processedforeventhubs olayları |  İşlenen Olaylar (olay/IoT Hub 'Ları için)  | 
| Microsoft. kusto/kümeler | CPU |  CPU  | 
| Microsoft. kusto/kümeler | ContinuousExportResult |  Sürekli dışarı aktarma sonucu  | 
| Microsoft. kusto/kümeler | ContinuousExportPendingCount |  Sürekli dışarı aktarma bekleyen sayısı  | 
| Microsoft. kusto/kümeler | Continuousexportnumofrecordsexports |  Sürekli dışa aktarma-dışarı aktarılmış kayıtların sayısı  | 
| Microsoft. kusto/kümeler | ContinuousExportMaxLatenessMinutes |  Sürekli dışarı aktarma en fazla dakika  | 
| Microsoft. kusto/kümeler | Cachekullanım |  Önbellek kullanımı  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | TriggerThrottledEvents |  Kısıtlanmış olayları tetikleyin  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | TriggersSucceeded |  Başarılı Tetikleyiciler   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | TriggersStarted |  Başlatılan Tetikleyiciler   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | TriggersSkipped |  Atlanan Tetikleyiciler  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | Triggersharekete geçirildi |  Tetiklenen Tetikleyiciler   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | TriggersFailed |  Başarısız Tetikleyiciler   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | TriggersCompleted |  Tamamlanan Tetikleyiciler   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | RunThrottledEvents |  Kısıtlanmış olayları Çalıştır  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | RunStartThrottledEvents |  Kısıtlanmış başlangıç olaylarını Çalıştır  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | RunsSucceeded |  Çalışma başarılı  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | RunsStarted |  Çalışma başlatıldı  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | RunsFailed |  Çalıştırma başarısız  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | RunsCompleted |  Tamamlanan çalıştırmalar  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | RunsCancelled |  Çalıştırma Iptal edildi  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | RunFailurePercentage yüzdesi |  Çalıştırma hatası yüzdesi  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | ActionThrottledEvents |  Eylem kısıtlanmış olaylar  | 
| Microsoft. Logic/ıntegrationserviceortamortamları | ActionsSucceeded |  İşlemler başarılı oldu   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | ActionsStarted |  Başlatılan eylemler   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | ActionsSkipped |  Atlanan eylemler   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | ActionsFailed |  Başarısız eylemler   | 
| Microsoft. Logic/ıntegrationserviceortamortamları | ActionsCompleted |  Tamamlanan eylemler   | 
| Microsoft. Logic/iş akışları | TriggerThrottledEvents |  Kısıtlanmış olayları tetikleyin  | 
| Microsoft. Logic/iş akışları | TriggersSucceeded |  Başarılı Tetikleyiciler   | 
| Microsoft. Logic/iş akışları | TriggersStarted |  Başlatılan Tetikleyiciler   | 
| Microsoft. Logic/iş akışları | TriggersSkipped |  Atlanan Tetikleyiciler  | 
| Microsoft. Logic/iş akışları | Triggersharekete geçirildi |  Tetiklenen Tetikleyiciler   | 
| Microsoft. Logic/iş akışları | TriggersFailed |  Başarısız Tetikleyiciler   | 
| Microsoft. Logic/iş akışları | TriggersCompleted |  Tamamlanan Tetikleyiciler   | 
| Microsoft. Logic/iş akışları | Totalbillableyürütmeleri |  Toplam faturalandırılabilir yürütmeler  | 
| Microsoft. Logic/iş akışları | RunThrottledEvents |  Kısıtlanmış olayları Çalıştır  | 
| Microsoft. Logic/iş akışları | RunStartThrottledEvents |  Kısıtlanmış başlangıç olaylarını Çalıştır  | 
| Microsoft. Logic/iş akışları | RunsSucceeded |  Çalışma başarılı  | 
| Microsoft. Logic/iş akışları | RunsStarted |  Çalışma başlatıldı  | 
| Microsoft. Logic/iş akışları | RunsFailed |  Çalıştırma başarısız  | 
| Microsoft. Logic/iş akışları | RunsCompleted |  Tamamlanan çalıştırmalar  | 
| Microsoft. Logic/iş akışları | RunsCancelled |  Çalıştırma Iptal edildi  | 
| Microsoft. Logic/iş akışları | RunFailurePercentage yüzdesi |  Çalıştırma hatası yüzdesi  | 
| Microsoft. Logic/iş akışları | Billingusagestorampatüketim |  Depolama tüketimi yürütmeleri için faturalama kullanımı  | 
| Microsoft. Logic/iş akışları | Billingusagestorampatüketim |  Depolama tüketimi yürütmeleri için faturalama kullanımı  | 
| Microsoft. Logic/iş akışları | BillingUsageStandardConnector |  Standart bağlayıcı yürütmeleri için faturalandırma kullanımı  | 
| Microsoft. Logic/iş akışları | BillingUsageStandardConnector |  Standart bağlayıcı yürütmeleri için faturalandırma kullanımı  | 
| Microsoft. Logic/iş akışları | BillingUsageNativeOperation |  Yerel Işlem yürütmeleri için faturalandırma kullanımı  | 
| Microsoft. Logic/iş akışları | BillingUsageNativeOperation |  Yerel Işlem yürütmeleri için faturalandırma kullanımı  | 
| Microsoft. Logic/iş akışları | Billabletriggeryürütmeler |  Faturalanabilir tetikleyici yürütmeleri  | 
| Microsoft. Logic/iş akışları | Billableactionyürütmeler |  Faturalanabilir eylem yürütmeleri  | 
| Microsoft. Logic/iş akışları | ActionThrottledEvents |  Eylem kısıtlanmış olaylar  | 
| Microsoft. Logic/iş akışları | ActionsSucceeded |  İşlemler başarılı oldu   | 
| Microsoft. Logic/iş akışları | ActionsStarted |  Başlatılan eylemler   | 
| Microsoft. Logic/iş akışları | ActionsSkipped |  Atlanan eylemler   | 
| Microsoft. Logic/iş akışları | ActionsFailed |  Başarısız eylemler   | 
| Microsoft. Logic/iş akışları | ActionsCompleted |  Tamamlanan eylemler   | 
| Microsoft. Network/frontkapaklı | WebApplicationFirewallRequestCount |  Web uygulaması güvenlik duvarı Istek sayısı  | 
| Microsoft. Network/frontkapaklı | TotalLatency |  Toplam gecikme süresi  | 
| Microsoft. Network/frontkapaklı | Yanıt boyutu |  Yanıt boyutu  | 
| Microsoft. Network/frontkapaklı | RequestSize |  İstek boyutu  | 
| Microsoft. Network/frontkapaklı | Istek sayısı |  İstek Sayısı  | 
| Microsoft. Network/frontkapaklı | Billilableresponsesize |  Faturalanabilir yanıt boyutu  | 
| Microsoft. Network/frontkapaklı | BackendRequestLatency |  Arka uç Isteği gecikmesi  | 
| Microsoft. Network/frontkapaklı | BackendRequestCount |  Arka uç Istek sayısı  | 
| Microsoft. Network/frontkapaklı | BackendHealthPercentage |  Arka uç sistem durumu yüzdesi  | 
| Microsoft. Network/trafficManagerProfiles | QpsByEndpoint |  Bitiş noktasına göre sorgular döndürüldü  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | zamanlandı. bekliyor |  Bekleyen zamanlanmış bildirimler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | kayıt. güncelleştirme |  Kayıt güncelleştirme Işlemleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | kayıt. Get |  Kayıt okuma Işlemleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | kayıt. Sil |  Kayıt silme Işlemleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | kayıt. oluştur |  Kayıt oluşturma Işlemleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | kayıt. tümü |  Kayıt Işlemleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. yanlışlıkla gtoken |  WNS yetkilendirme hataları (yanlış belirteç)  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. tokenproviderunreachable |  WNS yetkilendirme hataları (ulaşılamıyor)  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. kısıtlanıyor |  WNS kısıtlanıyor bildirimleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. Success |  WNS başarılı bildirimleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. pnserror |  WNS hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. ınvalidtoken |  WNS yetkilendirme hataları (geçersiz belirteç)  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. ınvalidnotificationsize |  WNS geçersiz bildirim boyutu hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. ınvalidnotificationformat |  WNS geçersiz bildirim biçimi  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. ınvalidcredentials |  WNS yetkilendirme hataları (geçersiz kimlik bilgileri)  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. expiredchannel |  WNS süre dolduğunda kanal hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. bırakıldı |  WNS bırakılan bildirimler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. channelkısıtlanıyor |  WNS kanal kısıtlandı  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. channelconnected bağlantısı kesildi |  WNS kanal bağlantısı kesildi  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. badchannel |  WNS hatalı kanal hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. WNS. authenticationerror |  WNS kimlik doğrulama hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. kısıtlanıyor |  MPNS kısıtlanan bildirimler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. Success |  MPNS başarılı bildirimleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. pnserror |  MPNS hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. ınvalidnotificationformat |  MPNS geçersiz bildirim biçimi  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. ınvalidcredentials |  MPNS geçersiz kimlik bilgileri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. bırakıldı |  MPNS bırakılan bildirimler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. channelconnected bağlantısı kesildi |  MPNS kanalının bağlantısı kesildi  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. badchannel |  MPNS hatalı kanal hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. MPNS. authenticationerror |  MPNS kimlik doğrulama hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. yanlışlıkla gchannel |  GCM yanlış kanal hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. kısıtlanıyor |  GCM kısıtlanan bildirimler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. Success |  GCM başarılı bildirimleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. pnserror |  GCM hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. ınvalidnotificationsize |  GCM geçersiz bildirim boyutu hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. ınvalidnotificationformat |  GCM geçersiz bildirim biçimi  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. ınvalidcredentials |  GCM yetkilendirme hataları (geçersiz kimlik bilgileri)  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. expiredchannel |  GCM süre aşımına uğradı kanal hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. badchannel |  GCM hatalı kanal hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. gcm. authenticationerror |  GCM kimlik doğrulama hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. APNs. Success |  APNS başarılı bildirimler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. APNs. pnserror |  APNS hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. APNs. ınvalidnotificationsize |  APNS geçersiz bildirim boyutu hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. APNs. ınvalidcredentials |  APNS yetkilendirme hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. APNs. expiredchannel |  APNS süre dolma kanalı hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. APNs. badchannel |  APNS geçersiz kanal hatası  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. allpns. Success |  Başarılı bildirimler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. allpns. pnserror |  Dış bildirim sistemi hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. allpns. ınvalidpayload |  Yük hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | giden. allpns. channelerror |  Kanal hataları  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | notificationhub. gönderimleri |  Tüm giden bildirimler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | yükleme. upsert |  Yükleme Işlemlerini oluşturma veya güncelleştirme  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | yükleme. Patch |  Düzeltme eki yükleme Işlemleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | yükleme. Get |  Yükleme Işlemlerini al  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | yükleme. Delete |  Yükleme Işlemlerini silme  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | yükleme. tümü |  Yükleme yönetimi Işlemleri  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | gelen. zamanlandı. iptal |  Zamanlanan anında Iletme bildirimleri Iptal edildi  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | gelen. zamanlandı |  Zamanlanan anında Iletme bildirimleri gönderildi  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | gelen. ALL. Requests |  Tüm gelen Istekler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | gelen. tüm. failedistekleri |  Tüm gelen başarısız Istekler  | 
| Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar | gelen |  Gelen İletiler  | 
| Microsoft. Operationalınsights/çalışma alanları | Sinyal |  Sinyal  | 
| Microsoft. Relay/Namespace | BytesTransferred |  BytesTransferred  | 
| Microsoft. ServiceBus/ad alanları | OutgoingMessages |  Giden İletiler  | 
| Microsoft. ServiceBus/ad alanları | Incomingrequests |  Gelen İstekler  | 
| Microsoft. ServiceBus/ad alanları | Incomingmessages |  Gelen İletiler  | 
| Microsoft. SignalRService/SignalR | UserErrors |  Kullanıcı hataları  | 
| Microsoft. SignalRService/SignalR | SystemErrors |  Sistem hataları  | 
| Microsoft. SignalRService/SignalR | OutboundTraffic |  Giden Trafik  | 
| Microsoft. SignalRService/SignalR | MessageCount |  İleti Sayısı  | 
| Microsoft. SignalRService/SignalR | Inboundtraffic |  Gelen Trafik  | 
| Microsoft. SignalRService/SignalR | ConnectionCount |  Bağlantı sayısı  | 
| Microsoft. SQL/ManagedInstances | avg_cpu_percent |  Ortalama CPU yüzdesi  | 
| Microsoft. SQL/sunucuları | dtu_used |  Kullanılan DTU  | 
| Microsoft. SQL/sunucuları | dtu_consumption_percent |  DTU yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | xtp_storage_percent |  Bellek içi OLTP depolama yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | workers_percent |  Çalışan yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | sessions_percent |  Oturum yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | physical_data_read_percent |  Veri G/Ç yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | log_write_percent |  Günlük GÇ yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | dwu_used |  Kullanılan DWU  | 
| Microsoft. SQL/Servers/veritabanları | dwu_consumption_percent |  DWU yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | dtu_used |  Kullanılan DTU  | 
| Microsoft. SQL/Servers/veritabanları | dtu_consumption_percent |  DTU yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | Çözül |  Çık  | 
| Microsoft. SQL/Servers/veritabanları | cpu_used |  Kullanılan CPU  | 
| Microsoft. SQL/Servers/veritabanları | cpu_percent |  CPU yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | connection_successful |  Başarılı bağlantılar  | 
| Microsoft. SQL/Servers/veritabanları | connection_failed |  Başarısız Bağlantılar  | 
| Microsoft. SQL/Servers/veritabanları | cache_hit_percent |  İsabetli önbellek okuması yüzdesi  | 
| Microsoft. SQL/Servers/veritabanları | blocked_by_firewall |  Güvenlik duvarı tarafından engellendi  | 
| Microsoft. SQL/Servers/Elaun havuzları | xtp_storage_percent |  Bellek içi OLTP depolama yüzdesi  | 
| Microsoft. SQL/Servers/Elaun havuzları | workers_percent |  Çalışan yüzdesi  | 
| Microsoft. SQL/Servers/Elaun havuzları | sessions_percent |  Oturum yüzdesi  | 
| Microsoft. SQL/Servers/Elaun havuzları | physical_data_read_percent |  Veri G/Ç yüzdesi  | 
| Microsoft. SQL/Servers/Elaun havuzları | log_write_percent |  Günlük GÇ yüzdesi  | 
| Microsoft. SQL/Servers/Elaun havuzları | eDTU_used |  eDTU kullanıldı  | 
| Microsoft. SQL/Servers/Elaun havuzları | dtu_consumption_percent |  DTU yüzdesi  | 
| Microsoft. SQL/Servers/Elaun havuzları | cpu_percent |  CPU yüzdesi  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Toplam ön uçlar |  Toplam ön uçlar  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Smallappserviceplanınstances |  Küçük App Service çalışanları planlıyor  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | İstekler |  İstekler  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | MemoryPercentage |  Bellek yüzdesi  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Mediumappserviceplanınstances |  Orta App Service çalışanları planlıyor  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Largeappserviceplanınstances |  Büyük App Service çalışanları planlıyor  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | HttpQueueLength |  Http kuyruğu uzunluğu  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http5xx |  Http sunucu hataları  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http4xx |  Http 4xx  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http406 |  Http 406  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http404 |  HTTP 404  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http403 |  Http 403  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http401 |  Http 401  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http3xx |  Http 3xx  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http2xx |  Http 2xx  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Http101 |  Http 101  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | DiskQueueLength |  Disk kuyruğu uzunluğu  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | Cpuyüzdesi |  CPU Yüzdesi  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | BytesSent |  Giden veriler  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | BytesReceived |  Içindeki veriler  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | AverageResponseTime |  Ortalama yanıt süresi  | 
| Microsoft. Web/hostingEnvironments/multiRolePools | ActiveRequests |  Etkin Istekler  | 
| Microsoft. Web/hostingEnvironments/workerPools | WorkersUsed |  Kullanılan çalışanlar  | 
| Microsoft. Web/hostingEnvironments/workerPools | WorkersTotal |  Toplam çalışan sayısı  | 
| Microsoft. Web/hostingEnvironments/workerPools | WorkersAvailable |  Kullanılabilir çalışanlar  | 
| Microsoft. Web/hostingEnvironments/workerPools | MemoryPercentage |  Bellek yüzdesi  | 
| Microsoft. Web/hostingEnvironments/workerPools | Cpuyüzdesi |  CPU Yüzdesi  | 
| Microsoft. Web/sunucugrupları | TcpTimeWait |  TCP zaman bekleme  | 
| Microsoft. Web/sunucugrupları | TcpSynSent |  TCP SYN gönderilen  | 
| Microsoft. Web/sunucugrupları | Tcpsynalındı |  TCP SYN alındı  | 
| Microsoft. Web/sunucugrupları | TcpLastAck |  TCP son ACK  | 
| Microsoft. Web/sunucugrupları | TcpFinWait2 |  TCP Fin bekleme 2  | 
| Microsoft. Web/sunucugrupları | TcpFinWait1 |  TCP Fin bekleme 1  | 
| Microsoft. Web/sunucugrupları | Tcpkurdu |  TCP oluşturuldu  | 
| Microsoft. Web/sunucugrupları | TcpClosing |  TCP kapatma  | 
| Microsoft. Web/sunucugrupları | TcpCloseWait |  TCP kapatma bekleme  | 
| Microsoft. Web/sunucugrupları | MemoryPercentage |  Bellek yüzdesi  | 
| Microsoft. Web/sunucugrupları | HttpQueueLength |  Http kuyruğu uzunluğu  | 
| Microsoft. Web/sunucugrupları | DiskQueueLength |  Disk kuyruğu uzunluğu  | 
| Microsoft. Web/sunucugrupları | Cpuyüzdesi |  CPU Yüzdesi  | 
| Microsoft. Web/sunucugrupları | BytesSent |  Giden veriler  | 
| Microsoft. Web/sunucugrupları | BytesReceived |  Içindeki veriler  | 
| Microsoft. Web/siteler | TotalAppDomainsUnloaded |  Toplam yüklenmeyen uygulama etki alanları  | 
| Microsoft. Web/siteler | TotalAppDomains |  Toplam uygulama etki alanları  | 
| Microsoft. Web/siteler | İş Parçacıkları |  İş parçacığı sayısı  | 
| Microsoft. Web/siteler | RequestsInApplicationQueue |  Uygulama kuyruğundaki istekler  | 
| Microsoft. Web/siteler | İstekler |  İstekler  | 
| Microsoft. Web/siteler | PrivateBytes |  Özel baytlar  | 
| Microsoft. Web/siteler | MemoryWorkingSet |  Bellek çalışma kümesi  | 
| Microsoft. Web/siteler | Iowriteoperationspersecond |  GÇ Yazma Işlemi/saniye  | 
| Microsoft. Web/siteler | Iowritebytespersecond |  GÇ yazma bayt/saniye  | 
| Microsoft. Web/siteler | IoReadOperationsPerSecond |  GÇ okuma Işlemi/saniye  | 
| Microsoft. Web/siteler | Ioreadbytespersecond |  GÇ okuma bayt/saniye  | 
| Microsoft. Web/siteler | Iootheroperationspersecond |  GÇ diğer Işlem/saniye  | 
| Microsoft. Web/siteler | Iootherbytespersecond |  GÇ diğer bayt/saniye  | 
| Microsoft. Web/siteler | HttpResponseTime |  Yanıt süresi  | 
| Microsoft. Web/siteler | Http5xx |  Http sunucu hataları  | 
| Microsoft. Web/siteler | Http4xx |  Http 4xx  | 
| Microsoft. Web/siteler | Http406 |  Http 406  | 
| Microsoft. Web/siteler | Http404 |  HTTP 404  | 
| Microsoft. Web/siteler | Http403 |  Http 403  | 
| Microsoft. Web/siteler | Http401 |  Http 401  | 
| Microsoft. Web/siteler | Http3xx |  Http 3xx  | 
| Microsoft. Web/siteler | Http2xx |  Http 2xx  | 
| Microsoft. Web/siteler | Http101 |  Http 101  | 
| Microsoft. Web/siteler | HealthCheckStatus |  Durum denetimi durumu  | 
| Microsoft. Web/siteler | Handles |  Tanıtıcı Sayısı  | 
| Microsoft. Web/siteler | Gen2Collections |  Gen 2 çöp koleksiyonları  | 
| Microsoft. Web/siteler | Gen1Collections |  Gen 1 çöp koleksiyonları  | 
| Microsoft. Web/siteler | Gen0Collections |  Gen 0 çöp koleksiyonları  | 
| Microsoft. Web/siteler | FunctionExecutionUnits |  İşlev yürütme birimleri  | 
| Microsoft. Web/siteler | Işlev yürütme sayısı |  İşlev yürütme sayısı  | 
| Microsoft. Web/siteler | CurrentAssemblies |  Geçerli derlemeler  | 
| Microsoft. Web/siteler | CpuTime |  CPU süresi  | 
| Microsoft. Web/siteler | BytesSent |  Giden veriler  | 
| Microsoft. Web/siteler | BytesReceived |  Içindeki veriler  | 
| Microsoft. Web/siteler | AverageResponseTime |  Ortalama yanıt süresi  | 
| Microsoft. Web/siteler | AverageMemoryWorkingSet |  Ortalama bellek çalışma kümesi  | 
| Microsoft. Web/siteler | AppConnections |  Bağlantılar  | 
| Microsoft. Web/Sites/Yuvaları | TotalAppDomainsUnloaded |  Toplam yüklenmeyen uygulama etki alanları  | 
| Microsoft. Web/Sites/Yuvaları | TotalAppDomains |  Toplam uygulama etki alanları  | 
| Microsoft. Web/Sites/Yuvaları | İş Parçacıkları |  İş parçacığı sayısı  | 
| Microsoft. Web/Sites/Yuvaları | RequestsInApplicationQueue |  Uygulama kuyruğundaki istekler  | 
| Microsoft. Web/Sites/Yuvaları | İstekler |  İstekler  | 
| Microsoft. Web/Sites/Yuvaları | PrivateBytes |  Özel baytlar  | 
| Microsoft. Web/Sites/Yuvaları | MemoryWorkingSet |  Bellek çalışma kümesi  | 
| Microsoft. Web/Sites/Yuvaları | Iowriteoperationspersecond |  GÇ Yazma Işlemi/saniye  | 
| Microsoft. Web/Sites/Yuvaları | Iowritebytespersecond |  GÇ yazma bayt/saniye  | 
| Microsoft. Web/Sites/Yuvaları | IoReadOperationsPerSecond |  GÇ okuma Işlemi/saniye  | 
| Microsoft. Web/Sites/Yuvaları | Ioreadbytespersecond |  GÇ okuma bayt/saniye  | 
| Microsoft. Web/Sites/Yuvaları | Iootheroperationspersecond |  GÇ diğer Işlem/saniye  | 
| Microsoft. Web/Sites/Yuvaları | Iootherbytespersecond |  GÇ diğer bayt/saniye  | 
| Microsoft. Web/Sites/Yuvaları | HttpResponseTime |  Yanıt süresi  | 
| Microsoft. Web/Sites/Yuvaları | Http5xx |  Http sunucu hataları  | 
| Microsoft. Web/Sites/Yuvaları | Http4xx |  Http 4xx  | 
| Microsoft. Web/Sites/Yuvaları | Http406 |  Http 406  | 
| Microsoft. Web/Sites/Yuvaları | Http404 |  HTTP 404  | 
| Microsoft. Web/Sites/Yuvaları | Http403 |  Http 403  | 
| Microsoft. Web/Sites/Yuvaları | Http401 |  Http 401  | 
| Microsoft. Web/Sites/Yuvaları | Http3xx |  Http 3xx  | 
| Microsoft. Web/Sites/Yuvaları | Http2xx |  Http 2xx  | 
| Microsoft. Web/Sites/Yuvaları | Http101 |  Http 101  | 
| Microsoft. Web/Sites/Yuvaları | HealthCheckStatus |  Durum denetimi durumu  | 
| Microsoft. Web/Sites/Yuvaları | Handles |  Tanıtıcı Sayısı  | 
| Microsoft. Web/Sites/Yuvaları | Gen2Collections |  Gen 2 çöp koleksiyonları  | 
| Microsoft. Web/Sites/Yuvaları | Gen1Collections |  Gen 1 çöp koleksiyonları  | 
| Microsoft. Web/Sites/Yuvaları | Gen0Collections |  Gen 0 çöp koleksiyonları  | 
| Microsoft. Web/Sites/Yuvaları | FunctionExecutionUnits |  İşlev yürütme birimleri  | 
| Microsoft. Web/Sites/Yuvaları | Işlev yürütme sayısı |  İşlev yürütme sayısı  | 
| Microsoft. Web/Sites/Yuvaları | CurrentAssemblies |  Geçerli derlemeler  | 
| Microsoft. Web/Sites/Yuvaları | CpuTime |  CPU süresi  | 
| Microsoft. Web/Sites/Yuvaları | BytesSent |  Giden veriler  | 
| Microsoft. Web/Sites/Yuvaları | BytesReceived |  Içindeki veriler  | 
| Microsoft. Web/Sites/Yuvaları | AverageResponseTime |  Ortalama yanıt süresi  | 
| Microsoft. Web/Sites/Yuvaları | AverageMemoryWorkingSet |  Ortalama bellek çalışma kümesi  | 
| Microsoft. Web/Sites/Yuvaları | AppConnections |  Bağlantılar  | 
| Microsoft. SQL/Servers/veritabanları | cpu_percent | CPU yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | physical_data_read_percent | Veri G/Ç yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | log_write_percent | Günlük GÇ yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | dtu_consumption_percent | DTU yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | connection_successful | Başarılı bağlantılar | 
| Microsoft. SQL/Servers/veritabanları | connection_failed | Başarısız Bağlantılar | 
| Microsoft. SQL/Servers/veritabanları | blocked_by_firewall | Güvenlik duvarı tarafından engellendi | 
| Microsoft. SQL/Servers/veritabanları | Çözül | Çık | 
| Microsoft. SQL/Servers/veritabanları | xtp_storage_percent | Bellek içi OLTP depolama yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | workers_percent | Çalışan yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | sessions_percent | Oturum yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | dtu_used | Kullanılan DTU | 
| Microsoft. SQL/Servers/veritabanları | cpu_used | Kullanılan CPU | 
| Microsoft. SQL/Servers/veritabanları | dwu_consumption_percent | DWU yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | dwu_used | Kullanılan DWU | 
| Microsoft. SQL/Servers/veritabanları | cache_hit_percent | İsabetli önbellek okuması yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | wlg_allocation_relative_to_system_percent | Sistem iş yükü grubuna göre ayırma yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | wlg_allocation_relative_to_wlg_effective_cap_percent | Maksimum kaynak yüzdesi bazında iş yükü grubu ayırması | 
| Microsoft. SQL/Servers/veritabanları | wlg_active_queries | İş yükü grubu etkin sorguları | 
| Microsoft. SQL/Servers/veritabanları | wlg_queued_queries | İş yükü grubu sıraya alınmış sorgular | 
| Microsoft. SQL/Servers/veritabanları | active_queries | Etkin sorgular | 
| Microsoft. SQL/Servers/veritabanları | queued_queries | Kuyruğa alınmış sorgular | 
| Microsoft. SQL/Servers/veritabanları | wlg_active_queries_timeouts | İş yükü grubu sorgu zaman aşımları | 
| Microsoft. SQL/Servers/veritabanları | wlg_queued_queries_timeouts | İş yükü grubu sıraya alınan sorgu zaman aşımları | 
| Microsoft. SQL/Servers/veritabanları | wlg_effective_min_resource_percent | Geçerli Min kaynak yüzdesi | 
| Microsoft. SQL/Servers/veritabanları | wlg_effective_cap_resource_percent | Etkin uç kaynak yüzdesi | 
| Microsoft. SQL/Servers/Elaun havuzları | cpu_percent | CPU yüzdesi | 
| Microsoft. SQL/Servers/Elaun havuzları | physical_data_read_percent | Veri G/Ç yüzdesi | 
| Microsoft. SQL/Servers/Elaun havuzları | log_write_percent | Günlük GÇ yüzdesi | 
| Microsoft. SQL/Servers/Elaun havuzları | dtu_consumption_percent | DTU yüzdesi | 
| Microsoft. SQL/Servers/Elaun havuzları | workers_percent | Çalışan yüzdesi | 
| Microsoft. SQL/Servers/Elaun havuzları | sessions_percent | Oturum yüzdesi | 
| Microsoft. SQL/Servers/Elaun havuzları | eDTU_used | eDTU kullanıldı | 
| Microsoft. SQL/Servers/Elaun havuzları | xtp_storage_percent | Bellek içi OLTP depolama yüzdesi | 
| Microsoft. SQL/sunucuları | dtu_consumption_percent | DTU yüzdesi | 
| Microsoft. SQL/sunucuları | dtu_used | Kullanılan DTU | 
| Microsoft. SQL/ManagedInstances | avg_cpu_percent | Ortalama CPU yüzdesi | 

