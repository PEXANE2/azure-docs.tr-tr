---
title: Azure Izleyici uyarıları için geçişi anlama
description: Uyarı geçişinin nasıl çalıştığını anlayın ve sorunları giderin.
ms.topic: conceptual
ms.date: 02/14/2021
ms.author: yalavi
author: yalavi
ms.openlocfilehash: 88b75c46898e1a74b33051d8996af96201f639c7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102037719"
---
# <a name="understand-migration-options-to-newer-alerts"></a>Geçiş seçeneklerini daha yeni uyarılara anlayın

Klasik uyarılar, genel bulut kullanıcıları için [devre](./monitoring-classic-retirement.md) dışı bırakılsa da hala **31 Mayıs 2021** tarihine kadar sınırlı kullanımda. Azure Kamu bulutu ve Azure Çin 21Vianet için klasik uyarılar, **29 şubat 2024** tarihinde devre dışı bırakılacak.

Bu makalede, el ile geçiş ve gönüllü geçiş aracının nasıl çalıştığı ve kalan uyarı kurallarının geçirilmesi için kullanılacağı açıklanmaktadır. Ayrıca bazı yaygın sorunlara yönelik çözümleri açıklar.

> [!IMPORTANT]
> Etkinlik günlüğü uyarıları (hizmet durumu uyarıları dahil) ve günlük uyarıları geçişten etkilenmez. Geçiş yalnızca [burada](./monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform)açıklanan klasik uyarı kuralları için geçerlidir.

> [!NOTE]
> Klasik uyarı kurallarınız geçersiz ise, bunlar [kullanım dışı ölçümlerde](#classic-alert-rules-on-deprecated-metrics) veya silinmiş kaynaklardır ve hizmet devre dışı bırakıldıktan sonra kullanılamaz olur.

## <a name="manually-migrating-classic-alerts-to-newer-alerts"></a>Klasik uyarıları daha yeni uyarılara el ile geçirme

Kalan uyarılarını el ile geçirmeye ilgilenen müşteriler, aşağıdaki bölümleri kullanarak zaten bu işlemleri yapabilir. Ayrıca, kullanımdan kaldırılan ölçümleri de içerir, bu nedenle doğrudan geçirilemez.

### <a name="guest-metrics-on-virtual-machines"></a>Sanal makinelerde Konuk ölçümleri

Konuk ölçümlerinde yeni ölçüm uyarıları oluşturabilmeniz için, Konuk ölçümlerinin Azure Izleyici günlükleri deposuna gönderilmesi gerekir. Uyarı oluşturmak için aşağıdaki yönergeleri izleyin:

- [Günlük Analizi için konuk ölçümleri toplamayı etkinleştirme](../agents/agent-data-sources.md)
- [Azure Izleyici 'de günlük uyarıları oluşturma](./alerts-log.md)

Konuk ölçümleri toplamak ve üzerinde uyarı almak için daha fazla seçenek mevcuttur, [daha fazla bilgi edinin](../agents/agents-overview.md).

### <a name="storage-and-classic-storage-account-metrics"></a>Depolama ve klasik depolama hesabı ölçümleri

Depolama hesaplarında bulunan tüm klasik uyarılar, bu ölçümlerde uyarılar dışında geçirilebilir:

- Yüztauthorizationerror
- Percentclienentothererror
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- Anonymouskısıtlar Lingerror
- Saskısıtlar Lingerror
- ThrottlingError

Yüzde ölçümlerinde klasik uyarı kurallarının [, eski ve yeni depolama ölçümleri arasındaki eşlemeye](../../storage/common/storage-metrics-migration.md#metrics-mapping-between-old-metrics-and-new-metrics)göre geçirilmesi gerekir. Kullanılabilir yeni ölçüm mutlak bir değer olduğundan eşiklerin uygun şekilde değiştirilmesi gerekir.

Aynı işlevselliği sağlayan bir birleştirilmiş ölçüm olmadığından, Anonymouskısıtlar Lingerror, Saskısıtlar Lingerror ve kısıtlar Lingerror üzerinde klasik uyarı kuralları iki yeni uyarıya bölünmelidir. Eşiklerin uygun şekilde uyarlanması gerekir.

### <a name="cosmos-db-metrics"></a>Cosmos DB ölçümleri

Cosmos DB ölçümlerinde bulunan tüm klasik uyarılar, bu ölçümlerde uyarılar dışında geçirilebilir:

- Saniye başına ortalama Istek
- Tutarlılık Düzeyi
- Http 2xx
- Http 3xx
- Dakikada tüketilen maksimum RUPD
- Saniyede en fazla ru
- Mongo diğer Istek ücreti
- Mongo diğer Istek hızı
- Gözlemlenen okuma gecikmesi
- Gözlemlenen yazma gecikmesi
- Hizmet kullanılabilirliği
- Depolama Kapasitesi

Saniyede ortalama Istek sayısı, tutarlılık düzeyi, dakikada tüketilen maksimum RUPISI, saniyede maksimum ru, gözlemlenen okuma gecikmesi, gözlemlenen yazma gecikmesi ve depolama kapasitesi Şu anda [Yeni sistemde](../essentials/metrics-supported.md#microsoftdocumentdbdatabaseaccounts)kullanılamıyor.

Http 2xx, http 3xx ve hizmet kullanılabilirliği gibi istek ölçümlerinde uyarılar, klasik ölçümler ve yeni ölçümler arasında farklı olduğu için isteklerin sayılmadığı için geçirilmez. Bu ölçümlerde uyarıların ayarlandığı eşiklerle el ile yeniden oluşturulması gerekir.

### <a name="classic-alert-rules-on-deprecated-metrics"></a>Kullanım dışı ölçümler üzerinde klasik uyarı kuralları

Aşağıda, daha önce desteklenen ancak sonunda kullanım dışı olan ölçümler üzerinde klasik uyarı kuralları verilmiştir. Müşterilerin küçük bir yüzdesi, bu tür ölçümler üzerinde geçersiz klasik uyarı kurallarına sahip olabilir. Bu uyarı kuralları geçersiz olduğundan, geçirilmez.

| Kaynak türü| Kullanım dışı olan metriler |
|-------------|----------------- |
| Microsoft. Dbformyısql/sunucuları | compute_consumption_percent, compute_limit |
| Microsoft. DBforPostgreSQL/sunucuları | compute_consumption_percent, compute_limit |
| Microsoft. Network/Publicıpaddresses | defaultddoçabaggerrate |
| Microsoft. SQL/Servers/veritabanları | service_level_objective, storage_limit, storage_used, azaltma, dtu_consumption_percent, storage_used |
| Microsoft. Web/hostingEnvironments/multirolepools | averagememoryworkingset |
| Microsoft. Web/hostingEnvironments/workerpools | BytesReceived, httpqueuelength |

## <a name="how-equivalent-new-alert-rules-and-action-groups-are-created"></a>Eşit yeni uyarı kuralları ve eylem grupları oluşturma

Geçiş Aracı, klasik uyarı kurallarınızı eşdeğer yeni uyarı kurallarına ve eylem gruplarına dönüştürür. Çoğu klasik uyarı kuralları için, eşdeğer yeni uyarı kuralları, ve gibi aynı özelliklerle aynı ölçümdedir `windowSize` `aggregationType` . Ancak bazı klasik uyarı kuralları, yeni sistemde farklı, eşdeğer bir ölçüme sahip olan ölçümlerde bulunur. Aşağıdaki ilkeler aşağıdaki bölümde belirtilmedikçe klasik uyarıların geçirilmesi için geçerlidir:

- **Sıklık**: klasik veya yeni bir uyarı kuralının koşulu ne sıklıkta denetleyeceğini tanımlar. `frequency`Klasik uyarı kuralları Kullanıcı tarafından yapılandırılamaz ve tüm kaynak türleri için her zaman 5 dakikadır. Denk kuralların sıklığı da 5 dk olarak ayarlanır.
- **Toplama türü**: ölçümün ilgilendiğiniz pencere üzerinden nasıl toplandığını tanımlar. `aggregationType`Ayrıca, klasik uyarılar ve çoğu ölçüm için yeni uyarılar arasında de aynıdır. Bazı durumlarda, ölçüm klasik uyarılar ve yeni uyarılar arasında farklılık gösterdiğinden, eşdeğer `aggregationType` veya `primary Aggregation Type` ölçüm için tanımlanan değer kullanılır.
- **Units**: uyarının oluşturulduğu ölçümün özelliği. Bazı eşdeğer ölçümler farklı birimlere sahiptir. Eşik gerektiği şekilde uygun şekilde ayarlanır. Örneğin, özgün ölçüm, birim olarak saniyeler içeriyorsa, ancak eşdeğer yeni ölçüm, birim olarak milisaniyeye sahipse, aynı davranışı sağlamak için özgün eşik 1000 ile çarpılır.
- **Pencere boyutu**: eşiğe göre Karşılaştırılacak ölçüm verilerinin üzerine toplanan pencereyi tanımlar. `windowSize`5 dakika, 15 dakika, 30 dakika, 1 saat, 3 saat, 6 saat, 12 saat, 1 gün gibi standart değerler için, eşdeğer yeni uyarı kuralı için değişiklik yapılmaz. Diğer değerler için en yakın `windowSize` kullanılır. Çoğu müşteri için, bu değişikliğe göre hiçbir etkisi yoktur. Müşterilerin küçük bir yüzdesi için, tam olarak aynı davranışı almak üzere eşiğin ince ayar bir olması gerekebilir.

Aşağıdaki bölümlerde, yeni sistemde farklı, eşdeğer bir ölçüme sahip olan ölçümleri ayrıntılandırıyoruz. Klasik ve yeni uyarı kuralları için aynı kalan her türlü ölçüm listelenmez. [Burada](../essentials/metrics-supported.md)yeni sistemde desteklenen ölçümlerin listesini bulabilirsiniz.

### <a name="microsoftstoragestorageaccounts-and-microsoftclassicstoragestorageaccounts"></a>Microsoft. Storage/storageAccounts ve Microsoft. ClassicStorage/storageAccounts

Blob, tablo, dosya ve kuyruk gibi depolama hesabı Hizmetleri için aşağıdaki ölçümler aşağıda gösterildiği gibi eşdeğer ölçümlere eşlenir:

| Klasik uyarılarda ölçüm | Yeni uyarılardaki eşdeğer ölçüm | Yorumlar|
|--------------------------|---------------------------------|---------|
| AnonymousAuthorizationError| "ResponseType" = "AuthorizationError" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü| |
| Anonymousclienentothererror | "ResponseType" = "Clienentothererror" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousClientTimeOutError| "ResponseType" = "ClientTimeOutError" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousNetworkError | "ResponseType" = "NetworkError" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousServerOtherError | "ResponseType" = "Sunucudiğerhatası" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousServerTimeOutError | "ResponseType" = "ServerTimeOutError" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AnonymousSuccess | "ResponseType" = "Success" ve "Authentication" = "Anonymous" boyutlarıyla işlem ölçümü | |
| AuthorizationError | "ResponseType" = "AuthorizationError" boyutlarıyla işlem ölçümü | |
| AverageE2ELatency | SuccessE2ELatency | |
| AverageServerLatency | Başarılı Sunucugecikmesi | |
| Kapasite | BlobCapacity | `aggregationType`' Last ' yerine ' Average ' kullanın. Ölçüm yalnızca blob Hizmetleri için geçerlidir |
| Clienentothererror | "ResponseType" = "Clienentothererror" boyutlarıyla işlem ölçümü  | |
| ClientTimeoutError | "ResponseType" = "ClientTimeOutError" boyutlarıyla işlem ölçümü | |
| ContainerCount | ContainerCount | `aggregationType`' Last ' yerine ' Average ' kullanın. Ölçüm yalnızca blob Hizmetleri için geçerlidir |
| NetworkError | "ResponseType" = "NetworkError" boyutlarıyla işlem ölçümü | |
| ObjectCount | BlobCount| `aggregationType`' Last ' yerine ' Average ' kullanın. Ölçüm yalnızca blob Hizmetleri için geçerlidir |
| SASAuthorizationError | "ResponseType" = "AuthorizationError" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASClientOtherError | "ResponseType" = "Clienentothererror" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASClientTimeOutError | "ResponseType" = "ClientTimeOutError" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASNetworkError | "ResponseType" = "NetworkError" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASServerOtherError | "ResponseType" = "Sunucudiğerhatası" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASServerTimeOutError | "ResponseType" = "ServerTimeOutError" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| SASSuccess | "ResponseType" = "Success" ve "Authentication" = "SAS" boyutlarıyla işlem ölçümü | |
| ServerOtherError | Boyutlarla birlikte işlem ölçümü "ResponseType" = "Sunucudiğerhatası" | |
| ServerTimeOutError | "ResponseType" = "ServerTimeOutError" boyutlarıyla işlem ölçümü  | |
| Başarılı | Boyutlarla birlikte işlem ölçümü "ResponseType" = "Success" | |
| TotalBillableRequests| İşlemler | |
| TotalEgress | Çıkış | |
| TotalIngress | Giriş | |
| TotalRequests | İşlemler | |

### <a name="microsoftdocumentdbdatabaseaccounts"></a>UmentDB/databaseAccounts Microsoft.Doc

Cosmos DB için, eşdeğer ölçümler aşağıda gösterildiği gibidir:

| Klasik uyarılarda ölçüm | Yeni uyarılardaki eşdeğer ölçüm | Yorumlar|
|--------------------------|---------------------------------|---------|
| AvailableStorage | AvailableStorage||
| Veri Boyutu | Veri kullanımı| |
| Belge sayısı | DocumentCount||
| Dizin boyutu | Indexusage||
| Hizmet kullanılamıyor | ServiceAvailability||
| TotalRequestUnits | TotalRequestUnits||
| Kısıtlanmış Istekler | "StatusCode" boyutundaki TotalRequests = "429"| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| İç Sunucu Hataları | "StatusCode" ve "500"} boyutundaki TotalRequests| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| Http 401 | "StatusCode" boyutundaki TotalRequests = "401"| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| Http 400 | "StatusCode" boyutundaki TotalRequests = "400"| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| Toplam İstek Sayısı | TotalRequests| ' Max ' toplama türü ' Count ' olarak düzeltildi|
| Mongo sayımı Istek ücreti| "CommandName" = "Count" boyutuyla Mongorequestücret||
| Mongo sayımı Istek hızı | Dimension "CommandName" = "Count" olan MongoRequestsCount||
| Mongo silme Isteği ücreti | Dimension "CommandName" = "Sil" olacak mongorequestücret||
| Mongo silme Isteği hızı | Dimension ile MongoRequestsCount "CommandName" = "Delete"||
| Mongo ekleme Isteği ücreti | "CommandName" = "INSERT" boyutuyla Mongorequestücretle||
| Mongo ekleme Isteği oranı | Dimension ile MongoRequestsCount "CommandName" = "INSERT"||
| Mongo sorgu Isteği ücreti | "CommandName" = "Find" boyutuyla Mongorequestücretle||
| Mongo sorgu Isteği hızı | Dimension ile MongoRequestsCount "CommandName" = "Find"||
| Mongo güncelleştirme Isteği ücreti | Dimension "CommandName" = "Update" ile mongorequestücret||
| Mongo ekleme başarısız Istekleri | "CommandName" = "INSERT" ve "Status" = "Failed" boyutlarıyla MongoRequestCount| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| Mongo sorgu başarısız Istekleri | "CommandName" = "Query" ve "Status" = "Failed" boyutlarıyla MongoRequestCount| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| Mongo sayısı başarısız Istekler | "CommandName" = "Count" ve "Status" = "Failed" boyutlarıyla MongoRequestCount| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| Mongo güncelleştirme başarısız Istekleri | "CommandName" = "Update" ve "Status" = "Failed" boyutlarıyla MongoRequestCount| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| Diğer başarısız Istekleri Mongo | "CommandName" = "Other" ve "Status" = "Failed" boyutlarıyla MongoRequestCount| ' Average ' toplama türü ' Count ' olarak düzeltildi|
| Mongo silme başarısız Istekleri | "CommandName" = "Delete" ve "Status" = "Failed" boyutlarıyla MongoRequestCount| ' Average ' toplama türü ' Count ' olarak düzeltildi|

### <a name="how-equivalent-action-groups-are-created"></a>Denk eylem grupları oluşturma

Klasik uyarı kurallarında e-posta, Web kancası, mantıksal uygulama ve uyarı kuralına bağlı runbook eylemleri vardı. Yeni uyarı kuralları, birden fazla uyarı kuralı arasında yeniden kullanılabilen eylem gruplarını kullanır. Geçiş Aracı, eylemi kaç tane uyarı kuralı kullandıklarından bağımsız olarak aynı eylemler için tek bir eylem grubu oluşturur. Geçiş Aracı tarafından oluşturulan eylem grupları ' Migrated_AG * ' adlandırma biçimini kullanır.

> [!NOTE]
> Klasik uyarılar, klasik yönetici rollerini bilgilendirmek için kullanıldığında klasik yöneticinin yerel ayarlarına bağlı olarak yerelleştirilmiş e-postalar gönderdi. Yeni uyarı e-postaları eylem grupları aracılığıyla gönderilir ve yalnızca Ingilizce olarak gösterilir.

## <a name="rollout-phases"></a>Dağıtım aşamaları

Geçiş Aracı, klasik uyarı kuralları kullanan müşterilere yönelik aşamalar halinde kullanıma alınıyor. Abonelik, Aracı kullanılarak geçirilmesi için hazırsa, abonelik sahipleri bir e-posta alır.

> [!NOTE]
> Araç aşamalarda kullanıma alındığından, aboneliklerinizin bazılarının erken aşamalar sırasında henüz geçirilmeye hazır olmadığını görebilirsiniz.

Aboneliklerin çoğu şu anda geçiş için hazırlanın olarak işaretlendi. Yalnızca aşağıdaki kaynak türlerinde klasik uyarıları olan abonelikler hala geçişe hazırlanmaz.

- Microsoft. classicCompute/domainNames/yuvalar/roller
- Microsoft. Insights/bileşenler

## <a name="who-can-trigger-the-migration"></a>Geçişi kimlerin tetikleyebilen?

Abonelik düzeyinde katkıda bulunan Izleme katılımcısı rolü olan herhangi bir Kullanıcı, geçişi tetikleyebilir. Aşağıdaki izinlerle özel bir rolü olan kullanıcılar geçişi de tetikleyebilir:

- */Read
- Microsoft. Insights/actiongroups/*
- Microsoft. Insights/AlertRules/*
- Microsoft. Insights/metricAlerts/*
- Microsoft. AlertsManagement/smartDetectorAlertRules/*

> [!NOTE]
> İzinlerinizin yukarısında olmasının yanı sıra, aboneliğinizin da Microsoft. AlertsManagement kaynak sağlayıcısına kayıtlı olması gerekir. Application Insights hata anomali uyarılarını başarıyla geçirmek için bu gereklidir. 

## <a name="common-problems-and-remedies"></a>Yaygın sorunlar ve düzeltmeler

[Geçişi tetikledikten](alerts-using-migration-tool.md)sonra, geçiş işleminin tamamlandığını veya sizin için herhangi bir eylemin gerekli olduğunu bildirmek için verdiğiniz adreste e-posta alacaksınız. Bu bölümde bazı yaygın sorunlar ve bunlarla ilgilenme işlemleri açıklanmaktadır.

### <a name="validation-failed"></a>Doğrulama başarısız oldu

Aboneliğinizdeki klasik uyarı kurallarında yapılan bazı son değişiklikler nedeniyle abonelik geçirilemez. Bu sorun geçicidir. Geçiş durumu, geçiş **için** birkaç gün geri alındıktan sonra geçişi yeniden başlatabilirsiniz.

### <a name="scope-lock-preventing-us-from-migrating-your-rules"></a>Kapsam kilidi kurallarınızı geçirmemizi önler

Geçişin bir parçası olarak yeni ölçüm uyarıları ve yeni eylem grupları oluşturulur ve ardından klasik uyarı kuralları silinir. Ancak, bir kapsam kilidi, kaynak oluşturmamızı veya silmenizi önleyebilir. Kapsam kilidine bağlı olarak, bazı veya tüm kurallar geçirilemez. [Geçiş aracında](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)listelenen abonelik, kaynak grubu veya kaynak için kapsam kilidini kaldırarak ve geçişi yeniden tetikleyerek bu sorunu çözebilirsiniz. Kapsam kilidi devre dışı bırakılamaz ve geçiş işlemi sırasında kaldırılması gerekir. [Kapsam kilitlerini yönetme hakkında daha fazla bilgi edinin](../../azure-resource-manager/management/lock-resources.md#portal).

### <a name="policy-with-deny-effect-preventing-us-from-migrating-your-rules"></a>Kurallarınızı geçirmemizi engelleyen ' Reddet ' efektli ilke

Geçişin bir parçası olarak yeni ölçüm uyarıları ve yeni eylem grupları oluşturulur ve ardından klasik uyarı kuralları silinir. Ancak, bir [Azure ilke](../../governance/policy/index.yml) ataması, kaynak oluşturmamızı önleyebilir. İlke atamaya bağlı olarak, bazı veya tüm kurallar geçirilemez. İşlemi engelleyen ilke atamaları [geçiş aracında](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)listelenir. Bu sorunu aşağıdakilerden birini yaparak çözün:

- İlke atamasından geçiş işlemi sırasında abonelikleri, kaynak gruplarını veya tek tek kaynakları hariç tutma. [İlke dışlama kapsamlarını yönetme hakkında daha fazla bilgi edinin](../../governance/policy/tutorials/create-and-manage.md#remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion).
- ' Zorlama modu ' nu ilke atamasında **devre dışı** olarak ayarlayın. [İlke atamasının enforcementMode özelliği hakkında daha fazla bilgi edinin](../../governance/policy/concepts/assignment-structure.md#enforcement-mode).
- Abonelikler, kaynak grupları veya tek kaynaklarda ilke atamasına bir Azure Ilke muafiyeti (Önizleme) ayarlayın. [Azure ilke muafiyeti yapısı hakkında daha fazla bilgi edinin](../../governance/policy/concepts/exemption-structure.md).
- ' DISABLED ', ' Audit ', ' append ' veya ' Modify ' gibi etkileri kaldırma veya değiştirme (örneğin, eksik etiketlerle ilgili sorunları çözebilen). [İlke efektlerini yönetme hakkında daha fazla bilgi edinin](../../governance/policy/concepts/definition-structure.md#policy-rule).

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş için hazırlama](alerts-prepare-migration.md)