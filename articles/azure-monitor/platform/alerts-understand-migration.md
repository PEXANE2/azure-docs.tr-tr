---
title: Azure Monitör uyarıları için geçiş aracını anlama
description: Uyarılar geçiş aracının nasıl çalıştığını ve sorunları giderme sorunlarını anlayın.
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: yalavi
author: yalavi
ms.subservice: alerts
ms.openlocfilehash: d31c856e17348c23ad61130869af6ae440d3050d
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81114316"
---
# <a name="understand-how-the-migration-tool-works"></a>Geçiş aracının nasıl çalıştığını anlama

[Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure Monitor'daki klasik uyarılar 31 Ağustos 2019'a kadar kullanımdan kaldırılmaktadır (başlangıçta 30 Haziran 2019'du). Azure portalında, klasik uyarı kuralları kullanan ve geçişi kendileri tetiklemek isteyen müşteriler için bir geçiş aracı kullanılabilir.

Bu makalede, gönüllü geçiş aracının nasıl çalıştığı açıklanmaktadır. Ayrıca bazı ortak sorunlar için çareler açıklar.

> [!NOTE]
> Geçiş aracının kullanıma sunulmasındaki gecikme nedeniyle, klasik uyarı geçişi için emeklilik tarihi 30 Haziran 2019 tarihinden itibaren [31 Ağustos 2019'a uzatıldı.](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/)

## <a name="classic-alert-rules-that-will-not-be-migrated"></a>Geçirilmeyecek olan klasik uyarı kuralları

> [!IMPORTANT]
> Etkinlik günlüğü uyarıları (Hizmet sistem durumu uyarıları dahil) ve Günlük uyarıları geçişten etkilenmez. Geçiş yalnızca [burada](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform)açıklanan klasik uyarı kuralları için geçerlidir.

Araç hemen hemen tüm [klasik uyarı kuralları](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform)geçirebilirsiniz rağmen, bazı istisnalar vardır. Aşağıdaki uyarı kuralları aracı kullanarak (veya Eylül 2019'dan itibaren otomatik geçiş sırasında) geçirilmez:

- Sanal makine konuk ölçümleri (hem Windows hem de Linux) klasik uyarı kuralları. Bu makalenin ilerleyen saatlerinde [yeni metrik uyarılarda bu tür uyarı kurallarını yeniden oluşturma kılavuzuna](#guest-metrics-on-virtual-machines) bakın.
- Klasik depolama ölçümlerinde klasik uyarı kuralları. Klasik [depolama hesaplarınızı izlemek için kılavuza](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/)bakın.
- Bazı depolama hesabı ölçümlerinde klasik uyarı kuralları. Bu makalenin [ilerleyen](#storage-account-metrics) bilgilerine bakın.
- Bazı Cosmos DB ölçümlerinde klasik uyarı kuralları. Bu makalenin [ilerleyen](#cosmos-db-metrics) bilgilerine bakın.
- Tüm klasik sanal makineler ve bulut hizmetleri ölçümlerinde (Microsoft.ClassicCompute/virtualMachines ve Microsoft.ClassicCompute/domainNames/slots/roles) klasik uyarı kuralları. Bu makalenin [ilerleyen](#classic-compute-metrics) bilgilerine bakın.

Aboneliğinizde bu tür klasik kurallar varsa, bunları el ile geçirmeniz gerekir. Otomatik geçiş sağlayamadığımız için, bu tür klasik metrik uyarıları Haziran 2020'ye kadar çalışmaya devam edecektir. Bu uzantı, yeni uyarılara geçmeniz için zaman sağlar. Ayrıca, Haziran 2020'ye kadar yukarıda listelenen özel durumlar hakkında yeni klasik uyarılar oluşturmaya devam edebilirsiniz. Ancak her şey için, Ağustos 2019'dan sonra yeni klasik uyarılar oluşturulamaz.

> [!NOTE]
> Yukarıda listelenen özel durumların yanı sıra, klasik uyarı kurallarınız geçersizse, yani [bunlar amortismana alınmış ölçümlerde](#classic-alert-rules-on-deprecated-metrics) veya silinmiş kaynaklardaysa, bunlar geçiş olmaz ve hizmet kullanımdan kaldırıldıktan sonra kullanılamaz.

### <a name="guest-metrics-on-virtual-machines"></a>Sanal makinelerde konuk ölçümleri

Konuk ölçümleri üzerinde yeni metrik uyarıları oluşturabilmek için, konuk ölçümlerin Azure Monitörü özel ölçümleri deposuna gönderilmesi gerekir. Azure Monitörü'nün tanı ayarlarında batmasını etkinleştirmek için aşağıdaki yönergeleri izleyin:

- [Windows VM'leri için konuk ölçümlerini etkinleştirme](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Linux VM'leri için konuk ölçümlerini etkinleştirme](collect-custom-metrics-linux-telegraf.md)

Bu adımlar yapıldıktan sonra, konuk ölçümleri üzerinde yeni metrik uyarılar oluşturabilirsiniz. Yeni metrik uyarıları oluşturduktan sonra, klasik uyarıları silebilirsiniz.

### <a name="storage-account-metrics"></a>Depolama hesabı ölçümleri

Depolama hesaplarındaki tüm klasik uyarılar, bu ölçümlerdeki uyarılar dışında geçirilebilir:

- Yüzde Yetkilendirme Hatası
- YüzdeistemciOtherError
- YüzdeAğ Hatası
- YüzdeServerOtherError
- Yüzde Başarı
- YüzdeThrottlingHatası
- YüzdeTimeoutHatası
- AnonimThrottlingHatası
- SASThrottlingHatası
- ThrottlingError

Yüzde ölçümleri üzerindeki klasik uyarı [kuralları, eski ve yeni depolama ölçümleri arasındaki eşleme temel](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics)alınarak geçirilmelidir. Kullanılabilir yeni metrik mutlak bir ölçüm olduğundan, eşiklerin uygun şekilde değiştirilmesi gerekir.

AnonymousThrottlingError, SASThrottlingError ve ThrottlingError'ın klasik uyarı kuralları, aynı işlevselliği sağlayan birleşik bir metrik olmadığından iki yeni uyarıya bölünmelidir. Eşiklerin uygun şekilde uyarlanmış olması gerekir.

### <a name="cosmos-db-metrics"></a>Cosmos DB ölçümleri

Cosmos DB ölçümlerinde bu ölçümlerdeki uyarılar dışında tüm klasik uyarılar geçirilebilir:

- Saniye Başına Ortalama İstek
- Tutarlılık Düzeyi
- Http 2xx
- Http 3xx
- Http 400
- Http 401
- Dahili Sunucu Hatası
- Max RUPM Dakika Başına Tüketilen
- Max Rus Saniyede
- Mongo Sayısı Başarısız İstekler
- Mongo Başarısız İstekleri Sil
- Mongo Ekle Başarısız İstekler
- Mongo Diğer Başarısız İstekler
- Mongo Diğer İstek Ücreti
- Mongo Diğer İstek Oranı
- Mongo Sorgusu Başarısız İstekler
- Mongo Güncelleştirme Başarısız İstekleri
- Gözlenen Okuma Gecikmesi
- Gözlenen Yazma Gecikme
- Hizmet Kullanılabilirliği
- Depolama Kapasitesi
- Daraltılmış İstekler
- Toplam İstek Sayısı

Saniye Başına Ortalama İstek, Tutarlılık Düzeyi, Max RUPM Dakika Başına Tüketilen, Saniyede Max RUs, Gözlenen Okuma Gecikmesi, Gözlenen Yazma Gecikmesi, Depolama Kapasitesi şu anda [yeni sistemde](metrics-supported.md#microsoftdocumentdbdatabaseaccounts)mevcut değildir.

Http 2xx, Http 3xx, Http 400, Http 401, Internal Server Error, Service Availability, Throttled Requests ve Total Requests gibi istek ölçümlerine ilişkin uyarılar, isteklerin sayılma şekli klasik ölçümler ve yeni ölçümler arasında farklı olduğundan geçirilir. Bu uyarılar, eşikler ayarlanmış el ile yeniden oluşturulması gerekir.

Aynı işlevselliği sağlayan birleşik bir metrik olmadığından, Mongo Başarısız İstekler ölçümlerinde uyarılar birden çok uyarıya bölünmelidir. Eşiklerin uygun şekilde uyarlanmış olması gerekir.

### <a name="classic-compute-metrics"></a>Klasik işlem ölçümleri

Klasik işlem kaynakları henüz yeni uyarılarla desteklenmedikçe, klasik işlem ölçümlerinde yapılan tüm uyarılar geçiş aracı kullanılarak geçirilmez. Gelecekte bu kaynak türleri ile ilgili yeni uyarılar desteği eklenecektir. Bu kullanıma sunulduktan sonra, müşterilerin Haziran 2020'den önce klasik uyarı kurallarına göre yeni eşdeğer uyarı kurallarını yeniden oluşturmaları gerekir.

### <a name="classic-alert-rules-on-deprecated-metrics"></a>Amortismana dayalı ölçümlerde klasik uyarı kuralları

Bunlar, daha önce desteklenen ancak sonunda küçümsenen ölçümlerle ilgili klasik uyarı kurallarıdır. Müşterinin küçük bir yüzdesi bu tür ölçümlerde geçersiz klasik uyarı kurallarına sahip olabilir. Bu uyarı kuralları geçersiz olduğundan, geçirilmezler.

| Kaynak türü| Amortismana karşı metrik(ler) |
|-------------|----------------- |
| Microsoft.DBforMySQL/sunucular | compute_consumption_percent, compute_limit |
| Microsoft.DBforPostgreSQL/sunucular | compute_consumption_percent, compute_limit |
| Microsoft.Network/publicIPAdresleri | defaultddostriggerrate |
| Microsoft.SQL/servers/veritabanları | service_level_objective, storage_limit, storage_used, azaltma, dtu_consumption_percent, storage_used |
| Microsoft.Web/hostingOrtamlar/çok amaçlı havuzlar | ortalama memoryworkingset |
| Microsoft.Web/hostingOrtamlar/işçi havuzları | bayt, httpqueuelength |

## <a name="how-equivalent-new-alert-rules-and-action-groups-are-created"></a>Eşdeğer yeni uyarı kuralları ve eylem grupları nasıl oluşturulur?

Geçiş aracı, klasik uyarı kurallarınızı eşdeğer yeni uyarı kurallarına ve eylem gruplarına dönüştürür. Çoğu klasik uyarı kuralı için eşdeğer yeni uyarı kuralları aynı metriktedir `windowSize` ve `aggregationType`. Ancak, bazı klasik uyarı kuralları yeni sistemde farklı, eşdeğer metrik olan ölçümler vardır. Aşağıdaki bölümde belirtilmedikçe klasik uyarıların geçişi için aşağıdaki ilkeler geçerlidir:

- **Sıklık**: Klasik veya yeni bir uyarı kuralının durumu ne sıklıkta denetleyenleri tanımlar. Klasik `frequency` uyarı kuralları kullanıcı tarafından yapılandırılamaz ve 1 dakika olduğu Application Insights bileşenleri hariç tüm kaynak türleri için her zaman 5 dakika idi. Yani eşdeğer kuralların sıklığı da sırasıyla 5 dk ve 1 dk olarak ayarlanır.
- **Toplama Türü**: Ölçümün ilgi alanı üzerinde nasıl toplanmış olduğunu tanımlar. Aynı `aggregationType` zamanda klasik uyarılar ve çoğu ölçümler için yeni uyarılar arasında aynıdır. Bazı durumlarda, metrik klasik uyarılar ve yeni uyarılar arasında `aggregationType` farklı `primary Aggregation Type` olduğundan, eşdeğer veya metrik için tanımlanan kullanılır.
- **Birimler**: Uyarının oluşturulduğu metrin özelliği. Bazı eşdeğer ölçümlerfarklı birimlere sahiptir. Eşik gerektiği gibi uygun şekilde ayarlanır. Örneğin, özgün metriğin birim olarak saniyeleri varsa, ancak eşdeğer yeni metrik birim olarak miliSaniyeye sahipse, aynı davranışı sağlamak için özgün eşik 1000 ile çarpılır.
- **Pencere Boyutu**: Metrik verilerin eşiğe göre karşılaştırılmak üzere toplandığı pencereyi tanımlar. 5mins, 15mins, 30dakika, 1saat, 3saat, 6 saat, 12 saat, 1 gün gibi standart `windowSize` değerler için, eşdeğer yeni uyarı kuralı için yapılan hiçbir değişiklik yoktur. Diğer değerler için `windowSize` en yakın ı kullanılacak şekilde seçilir. Çoğu müşteri için, bu değişikliğin hiçbir etkisi yoktur. Müşterilerin küçük bir yüzdesi için, tam olarak aynı davranışı almak için eşik tweak gerekebilir.

Aşağıdaki bölümlerde, yeni sistemde farklı, eşdeğer bir metriğe sahip ölçümleri ayrıntılı olarak inceleriz. Klasik ve yeni uyarı kuralları için aynı kalan herhangi bir metrik listelenmez. Yeni sistemde desteklenen ölçümlerin listesini [burada](metrics-supported.md)bulabilirsiniz.

### <a name="microsoftstorageaccountsservices"></a>Microsoft.StorageHesapları/hizmetleri

Blob, tablo, dosya ve sıra gibi Depolama hesabı hizmetleri için aşağıdaki ölçümler aşağıdaki gibi eşdeğer ölçümlerle eşlenir:

| Klasik uyarılarda metrik | Yeni uyarılarda eşdeğer metrik | Yorumlar|
|--------------------------|---------------------------------|---------|
| Anonim Yetkilendirme Hatası| "ResponseType"="AuthorizationError" ve "Authentication" boyutlarıile hareketler metrik = "Anonim"| |
| AnonimClientOtherError | "ResponseType"="ClientOtherError" ve "Authentication" = "Anonim" boyutlarıile hareketler metrik | |
| Anonim Istemci TimeOutHatası| "ResponseType"="ClientTimeOutError" ve "Authentication" = "Anonim" boyutlarıile hareketler metrik | |
| AnonimAğ Hatası | "ResponseType"="NetworkError" ve "Authentication" boyutlarıile hareketler metrik = "Anonim" | |
| AnonimServerOtherError | "ResponseType"="ServerOtherError" ve "Authentication" = "Anonim" boyutlarıyla yapılan işlemler metrik | |
| AnonimServerTimeOutHatası | "ResponseType"="ServerTimeOutError" ve "Authentication" = "Anonymous" boyutlarıyla hareketler metrik | |
| Anonim Başarı | "ResponseType"="Başarı" ve "Kimlik Doğrulama" boyutlarıile hareketler metrik = "Anonim" | |
| Yetkilendirme Hatası | "ResponseType"="AuthorizationError" boyutlarıyla hareketler metrik | |
| AverageE2ELatency | SuccessE2ELatency | |
| AverageServerLatency | SuccessServerLatency | |
| Kapasite | BlobKapasite | 'Son' yerine 'ortalama' kullanın. `aggregationType` Metrik yalnızca Blob hizmetleri için geçerlidir |
| IstemciOtherError | "ResponseType"="ClientOtherError" boyutlarıyla hareketler metrik  | |
| İstemciTimeoutHatası | "ResponseType"="ClientTimeOutError" boyutlarıyla hareketler metrik | |
| Konteyner Sayısı | Konteyner Sayısı | 'Son' yerine 'ortalama' kullanın. `aggregationType` Metrik yalnızca Blob hizmetleri için geçerlidir |
| NetworkError | "ResponseType"="NetworkError" boyutlarıyla hareketler metrik | |
| Objectcount | BlobCount| 'Son' yerine 'ortalama' kullanın. `aggregationType` Metrik yalnızca Blob hizmetleri için geçerlidir |
| SASAuthorizationError | "ResponseType"="AuthorizationError" ve "Authentication" = "SAS" boyutlarıyla yapılan işlemler metrik | |
| SASClientOtherError | "ResponseType"="ClientOtherError" ve "Authentication" = "SAS" boyutlarıyla hareketler metrik | |
| SASClientTimeOutHatası | "ResponseType"="ClientTimeOutError" ve "Authentication" = "SAS" boyutlarıyla hareketler metrik | |
| SASNetworkHatası | "ResponseType"="NetworkError" ve "Authentication" = "SAS" boyutlarıyla hareketler metrik | |
| SASServerOtherError | "ResponseType"="ServerOtherError" ve "Authentication" = "SAS" boyutlarıyla yapılan işlemler metrik | |
| SASServerTimeOutHatası | "ResponseType"="ServerTimeOutError" ve "Authentication" = "SAS" boyutlarıyla yapılan işlemler metrik | |
| SASSuccess | "ResponseType"="Başarı" ve "Kimlik Doğrulama" boyutlarıile hareketler metrik = "SAS" | |
| ServerOtherError | "ResponseType"="ServerOtherError" boyutlarıyla hareketler metrik | |
| ServerTimeOutHatası | "ResponseType"="ServerTimeOutError" boyutlarıyla hareketler metrik  | |
| Başarılı | "ResponseType"="Başarı" boyutlarıyla hareketler metrik | |
| Toplam Faturalı İstekler| İşlemler | |
| TotalEgress | Çıkış | |
| TotalIngress | Giriş | |
| TotalRequests | İşlemler | |

### <a name="microsoftinsightscomponents"></a>Microsoft.insights/components

Uygulama Öngörüleri için eşdeğer ölçümler aşağıda gösterildiği gibi:

| Klasik uyarılarda metrik | Yeni uyarılarda eşdeğer metrik | Yorumlar|
|--------------------------|---------------------------------|---------|
| availability.availabilityMetric.value | availabilitySonuçlar/kullanılabilirlikPercentage|   |
| availability.durationMetric.value | availabilitySonuçlar/süre| Klasik metrik için birimler saniye cinsinden ve yenisi miliSaniye cinsinden olduğu için orijinal eşiği 1000 ile çarpın.  |
| basicExceptionBrowser.count | özel durumlar/tarayıcı|  'Toplam' yerine 'count' kullanın. `aggregationType` |
| basicExceptionServer.count | özel durumlar/sunucu| 'Toplam' yerine 'count' kullanın. `aggregationType`  |
| clientPerformance.clientProcess.value | browserTimings/processingDuration| Klasik metrik için birimler saniye cinsinden ve yenisi miliSaniye cinsinden olduğu için orijinal eşiği 1000 ile çarpın.  |
| istemciPerformance.networkConnection.value | browserTimings/networkDuration|  Klasik metrik için birimler saniye cinsinden ve yenisi miliSaniye cinsinden olduğu için orijinal eşiği 1000 ile çarpın. |
| clientPerformance.receiveRequest.value | browserTimings/receiveDuration| Klasik metrik için birimler saniye cinsinden ve yenisi miliSaniye cinsinden olduğu için orijinal eşiği 1000 ile çarpın.  |
| clientPerformance.sendRequest.value | browserTimings/sendDuration| Klasik metrik için birimler saniye cinsinden ve yenisi miliSaniye cinsinden olduğu için orijinal eşiği 1000 ile çarpın.  |
| clientPerformance.total.value | browserZamanlamas/totalDuration| Klasik metrik için birimler saniye cinsinden ve yenisi miliSaniye cinsinden olduğu için orijinal eşiği 1000 ile çarpın.  |
| performanceCounter.available_bytes.value | performanceSayaçlar/memoryAvailableBytes|   |
| performanceCounter.io_data_bytes_per_sec.value | performansSayaçlar/processIOBytesPerSecond|   |
| performanceCounter.number_of_exceps_thrown_per_sec.value | performanceCounters/exceptionsPerSecond|   |
| performanceCounter.percentage_processor_time_normalized.value | performanceSayaçlar/prosesCpuPercentage|   |
| performanceCounter.percentage_processor_time.value | performanceSayaçlar/prosesCpuPercentage| Özgün metrik tüm çekirdekler arasında olduğu ve yeni metrik tek bir çekirdekte normalleştirildiğinden, eşik uygun şekilde değiştirilmesi gerekir. Geçiş aracı eşikleri değiştirmez.  |
| performanceCounter.percentage_processor_total.value | performanceSayaçlar/işlemciCpuPercentage|   |
| performanceCounter.process_private_bytes.value | performanceSayaçlar/prosesPrivateBytes|   |
| performanceCounter.request_execution_time.value | performanceCounters/requestExecutionTime|   |
| performanceCounter.requests_in_application_queue.value | performanceCounters/requestsInQueue|   |
| performanceCounter.requests_per_sec.value | performanceCounters/requestsPerSecond|   |
| request.duration | istekler/süre| Klasik metrik için birimler saniye cinsinden ve yenisi miliSaniye cinsinden olduğu için orijinal eşiği 1000 ile çarpın.  |
| request.rate | istekler/oran|   |
| requestFailed.count | istekleri/başarısız| 'Toplam' yerine 'count' kullanın. `aggregationType`   |
| view.count | sayfaGörünümler/sayım| 'Toplam' yerine 'count' kullanın. `aggregationType`   |

### <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseHesapları

Cosmos DB için eşdeğer ölçümler aşağıda gösterildiği gibi:

| Klasik uyarılarda metrik | Yeni uyarılarda eşdeğer metrik | Yorumlar|
|--------------------------|---------------------------------|---------|
| Kullanılabilir Depolama     |Kullanılabilir Depolama|   |
| Veri Boyutu | VeriKullanımı| |
| Belge Sayısı | Belge Sayısı||
| Dizin Boyutu | IndexKullanım||
| Mongo Sayma İstek Ücreti| Boyut "CommandName" = "count" ile MongoRequestCharge||
| Mongo Sayısı İstek Oranı | Boyut "CommandName" = "count" ile MongoRequestsCount||
| Mongo Silme İstek Ücreti | Boyut "CommandName" = "silme" ile MongoRequestCharge||
| Mongo Silme İstek Oranı | Boyut "CommandName" = "silme" ile MongoRequestsCount||
| Mongo Ekle İstek Ücreti | Boyut "CommandName" = "insert" ile MongoRequestCharge||
| Mongo Ekle İstek Oranı | Boyut "CommandName" = "insert" ile MongoRequestsCount||
| Mongo Sorgu Talep Ücreti | Boyutu "CommandName" = "bul" ile MongoRequestCharge||
| Mongo Sorgu İstek Oranı | Boyut "CommandName" = "bul" ile MongoRequestsCount||
| Mongo Güncelleme İstek Ücreti | Boyut "CommandName" = "güncelleme" ile MongoRequestCharge||
| Hizmet Kullanılamıyor| Servis Kullanılabilirliği||
| Toplam İstek Birimleri | Toplam İstek Birimleri||

### <a name="how-equivalent-action-groups-are-created"></a>Eşdeğer eylem grupları nasıl oluşturulur?

Klasik uyarı kuralları, uyarı kuralının kendisine bağlı e-posta, webhook, mantık uygulaması ve runbook eylemleri vardı. Yeni uyarı kuralları, birden çok uyarı kuralı arasında yeniden kullanılabilen eylem gruplarını kullanır. Geçiş aracı, eylemi kaç uyarı kuralının kullandığına bakılmaksızın aynı eylemler için tek bir eylem grubu oluşturur. Geçiş aracı tarafından oluşturulan eylem grupları 'Migrated_AG*' adlandırma biçimini kullanır.

> [!NOTE]
> Klasik uyarılar, klasik yönetici rollerini bildirmek için kullanıldığında, klasik yöneticinin yerel durumuna göre yerelleştirilmiş e-postalar gönderir. Yeni uyarı e-postaları Eylem Grupları aracılığıyla gönderilir ve yalnızca İngilizcedir.

## <a name="rollout-phases"></a>Dağıtım aşamaları

Geçiş aracı, klasik uyarı kuralları kullanan müşterilere aşamalı olarak kullanıma uyruyor. Abonelik sahipleri, abonelik aracını kullanarak geçiş eve hazır olduğunda bir e-posta alır.

> [!NOTE]
> Araç aşamalı olarak kullanıma sunulduğundan, aboneliklerinizden bazılarının erken aşamalarda geçirilmeye henüz hazır olmadığını görebilirsiniz.

Aboneliklerin çoğu şu anda geçiş için hazır olarak işaretlenmiştir. Yalnızca aşağıdaki kaynak türleri hakkında klasik uyarıları olan abonelikler geçiş için hala hazır değil.

- Microsoft.classicCompute/domainNames/slots/roles
- Microsoft.insights/components

## <a name="who-can-trigger-the-migration"></a>Göçü kim tetikleyebilir?

Abonelik düzeyinde Katılımcıyı İzleme dahili rolüne sahip olan herhangi bir kullanıcı geçişi tetikleyebilir. Aşağıdaki izinlerle özel bir rolü olan kullanıcılar da geçişi tetikleyebilir:

- */okumak
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*
- Microsoft.AlertsManagement/smartDetectorAlertRules/*

> [!NOTE]
> Yukarıdaki izinlere sahip olmanın yanı sıra, aboneliğinizin ayrıca Microsoft.AlertsManagement kaynak sağlayıcısına da kaydedilmesi gerekir. Bu, Uygulama Öngörüleri'ndeki Hata Anomali uyarılarını başarıyla geçirmek için gereklidir. 

## <a name="common-problems-and-remedies"></a>Sık karşılaşılan sorunlar ve çareler

Geçişi [tetikledikten](alerts-using-migration-tool.md)sonra, geçişin tamamlandığını veya sizden herhangi bir eylem gerekip gerekdiğini bildirmek için sağladığınız adreslerden e-posta alırsınız. Bu bölümde bazı yaygın sorunlar ve bunlarla nasıl başa çıkılabilmek açıklanmaktadır.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Aboneliğinizdeki klasik uyarı kurallarında yapılan bazı son değişiklikler nedeniyle abonelik geçirilemez. Bu sorun geçicidir. Geçiş durumu birkaç gün içinde geçiş **için hazır** olarak geri döndükten sonra geçişi yeniden başlatabilirsiniz.

### <a name="scope-lock-preventing-us-from-migrating-your-rules"></a>Kurallarınızı geçirmemizi engelleyen kapsam kilidi

Geçişin bir parçası olarak, yeni metrik uyarılar ve yeni eylem grupları oluşturulur ve ardından klasik uyarı kuralları silinir. Ancak, kapsam kilidi kaynak oluşturmamızı veya silmemizi engelleyebilir. Kapsam kilidine bağlı olarak, bazı veya tüm kurallar geçirilemedi. Geçiş [aracında](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)listelenen abonelik, kaynak grubu veya kaynak için kapsam kilidini kaldırarak ve geçişi yeniden tetikleyerek bu sorunu çözebilirsiniz. Kapsam kilidi devre dışı tutulamaz ve geçiş işlemi süresince kaldırılması gerekir. [Kapsam kilitlerini yönetme hakkında daha fazla bilgi edinin.](../../azure-resource-manager/management/lock-resources.md#portal)

### <a name="policy-with-deny-effect-preventing-us-from-migrating-your-rules"></a>Kurallarınızı geçirmemizi engelleyen 'Reddet' etkisi olan politika

Geçişin bir parçası olarak, yeni metrik uyarılar ve yeni eylem grupları oluşturulur ve ardından klasik uyarı kuralları silinir. Ancak, bir ilke kaynak oluşturmamızı engelleyebilir. İlke bağlı olarak, bazı veya tüm kurallar geçirilemedi. İşlemi engelleyen ilkeler [geçiş aracında](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)listelenir. Bu sorunu aşağıdakileri yaparak çözüme kavuşturun:

- Geçiş işlemi süresince abonelikleri veya kaynak gruplarını ilke atamasından hariç tetkik eder. [İlkeleri dışlama kapsamını yönetme hakkında daha fazla bilgi edinin.](../../governance/policy/tutorials/create-and-manage.md#exempt-a-non-compliant-or-denied-resource-using-exclusion)
- Efekti 'denetim' veya 'ek' olarak kaldırma veya değiştirme (örneğin, eksik etiketlerle ilgili sorunları çözebilir). [İlkeleri yönetme etkisi hakkında daha fazla bilgi edinin.](../../governance/policy/concepts/definition-structure.md#policy-rule)

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş için hazırlama](alerts-prepare-migration.md)
