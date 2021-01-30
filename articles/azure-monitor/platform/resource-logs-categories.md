---
title: Azure Izleyici kaynak günlüğü desteklenen Hizmetleri ve kategorileri
description: Azure Izleyici başvurusu, Azure Kaynak günlükleri için desteklenen Hizmetleri ve olay şemasını anlayın.
ms.subservice: logs
ms.topic: reference
ms.date: 01/29/2021
ms.openlocfilehash: 02488e1a3ff26acf9ff318a2d5c09115aaba8df9
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99070755"
---
# <a name="supported-categories-for-azure-resource-logs"></a>Azure Kaynak günlükleri için desteklenen Kategoriler

> [!NOTE]
> Kaynak günlükleri daha önce tanılama günlükleri olarak bilinirdi. Azure Izleyici tarafından toplanan günlüklerin türleri yalnızca Azure kaynağına eşit olacak şekilde kaydırılacağı için ad Ekim 2019 ' de değiştirilmiştir.

Azure [izleyici kaynak günlükleri](./platform-logs-overview.md) , bu hizmet veya kaynakların çalışmasını tanımlayan Azure hizmetleri tarafından oluşturulan günlüklerdir. Azure Izleyici aracılığıyla kullanılabilen tüm kaynak günlükleri, her bir hizmetin kendi olayları için benzersiz özellikler yaymasını sağlayan ortak bir üst düzey şemayı paylaşır.

Kaynak türünün bir birleşimi ( `resourceId` özellikte mevcuttur) ve `category` bir şemayı benzersiz bir şekilde tanımlar. Hizmete özgü alanlarla, daha sonra farklı günlük kategorileri için eklenen tüm kaynak günlüklerine ortak bir şema vardır. Daha fazla bilgi için bkz. [Azure Kaynak günlükleri Için ortak ve hizmete özgü şema]()


## <a name="costs"></a>Maliyetler

Verileri Log Analytics, Azure depolama ve/veya Olay Hub 'ına gönderme ve depolama ile ilişkili maliyetler vardır. Bu konumlara verileri almak ve bu konumlara ulaşmak için ücret ödeyebilirsiniz.  Kaynak günlükleri, bu konumlara gönderebilmeniz için bir veri türüdür. 

Bazı kaynak günlüğü kategorilerini bu konumlara dışarı aktarmak için ek bir maliyet vardır. Dışarı aktarma maliyetlerine sahip bu Günlükler aşağıdaki tabloda listelenmiştir. Bu fiyatlandırma hakkında daha fazla bilgi için [Azure izleyici fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/monitor/)platform günlükleri bölümüne bakın.

## <a name="supported-log-categories-per-resource-type"></a>Kaynak türü başına desteklenen günlük kategorileri

Aşağıda her bir kaynak türü için kullanılabilen günlük türlerinin bir listesi verilmiştir. 

Bazı kategoriler yalnızca belirli kaynak türleri için desteklenir. Bir kaynağınız eksik olduğunu düşünüyorsanız kaynağa özgü belgelere bakın. Örneğin, Microsoft. SQL/Servers/veritabanları kategorileri tüm veritabanı türleri için kullanılamaz. Daha fazla bilgi için bkz. [SQL veritabanı tanılama günlüğü bilgileri](../../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md). 

Eksik bir sorun olduğunu düşünüyorsanız, bu makalenin alt kısmında bir GitHub açıklaması açabilirsiniz.
## <a name="microsoftaaddomainservices"></a>Microsoft. AAD/domainServices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AccountLogon|AccountLogon|Hayır|
|Hesap yönetimi|Hesap yönetimi|Hayır|
|Ayrıntı Izleme|Ayrıntı Izleme|Hayır|
|DirectoryServiceAccess|DirectoryServiceAccess|Hayır|
|LogonLogoff|LogonLogoff|Hayır|
|ObjectAccess|ObjectAccess|Hayır|
|PolicyChange|PolicyChange|Hayır|
|Ayrıcalıklı Geuse|Ayrıcalıklı Geuse|Hayır|
|SystemSecurity|SystemSecurity|Hayır|


## <a name="microsoftanalysisservicesservers"></a>Microsoft. AnalysisServices/sunucuları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Altyapı|Altyapı|Hayır|
|Hizmet|Hizmet|Hayır|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|GatewayLogs|Imanagementpackgateway ile ilgili Günlükler|Hayır|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft. AppConfiguration/Configurationmağazaları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|HttpRequest|HTTP Istekleri|Yes|


## <a name="microsoftappplatformspring"></a>Microsoft. AppPlatform/yay

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ApplicationConsole|Uygulama konsolu|Hayır|
|Sistem günlükleri|Sistem günlükleri|Hayır|


## <a name="microsoftattestationattestationproviders"></a>Microsoft. kanıtlama/attestationProviders

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AuditEvent|AuditEvent ileti günlüğü kategorisi.|Hayır|
|HATA|Hata iletisi günlüğü kategorisi.|Hayır|
|INF|Bilgilendirici ileti günlüğü kategorisi.|Hayır|
|UYR|Uyarı iletisi günlüğü kategorisi.|Hayır|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft. Automation/automationAccounts

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|DscNodeStatus|DSC düğüm durumu|Hayır|
|JobLogs|İş günlükleri|Hayır|
|JobStreams|İş akışları|Hayır|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ServiceLog|Hizmet günlükleri|Hayır|


## <a name="microsoftbatchaiworkspaces"></a>ChAI/çalışma alanlarını Microsoft.Bat

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|BaiClusterEvent|BaiClusterEvent|Hayır|
|BaiClusterNodeEvent|BaiClusterNodeEvent|Hayır|
|BaiJobEvent|BaiJobEvent|Hayır|


## <a name="microsoftblockchainblockchainmembers"></a>Microsoft. Blockzincirine/blockchainMembers

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|BlockchainApplication|Blok zinciri uygulaması|Hayır|
|FabricOrderer|Doku sırası|Hayır|
|FabricPeer|Doku eşi|Hayır|
|Ara sunucu|Ara sunucu|Hayır|


## <a name="microsoftblockchaincordamembers"></a>Microsoft. Blockzincirine/Cordadmembers

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|BlockchainApplication|Blok zinciri uygulaması|Hayır|


## <a name="microsoftbotservicebotservices"></a>Microsoft. botservice/botservices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|BotRequest|Kanallardan bot 'a istek|Hayır|
|DependencyRequest|Bağımlılıklara yönelik istekler|Hayır|


## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft. CDN/cdnwebapplicationfirewallpolicies

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|WebApplicationFirewallLogs|Web çıkışının başlatılıp başlatılmayacağını güvenlik duvarı günlükleri|Hayır|


## <a name="microsoftcdnprofiles"></a>Microsoft. CDN/profiller

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AzureCdnAccessLog|Azure CDN erişim günlüğü|Hayır|
|FrontDoorAccessLog|Frontkapısı erişim günlüğü|Yes|
|FrontDoorHealthProbeLog|Frontkapılı sistem durumu araştırma günlüğü|Yes|
|FrontDoorWebApplicationFirewallLog|Frontkapısı WebApplicationFirewall günlüğü|Yes|


## <a name="microsoftcdnprofilesendpoints"></a>Microsoft. CDN/profiller/uç noktaları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|CoreAnalytics|Uç noktanın ölçümlerini (örn. bant genişliği, çıkış vb.) alır.|Hayır|


## <a name="microsoftclassicnetworknetworksecuritygroups"></a>Microsoft. ClassicNetwork/networksecuritygroups

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Ağ güvenlik grubu kural akışı olayı|Ağ güvenlik grubu kural akışı olayı|Hayır|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft. Biliveservices/hesapları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Denetim|Denetim Günlükleri|Hayır|
|RequestResponse|İstek ve yanıt günlükleri|Hayır|
|İzleme|İzleme günlükleri|Hayır|


## <a name="microsoftcommunicationcommunicationservices"></a>Microsoft. Communication/CommunicationServices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ChatOperational|İşletimsel sohbet günlükleri|Hayır|
|SMSOperational|İşletimsel SMS günlükleri|Hayır|
|Kullanım|Kullanım kayıtları|Hayır|


## <a name="microsoftcontainerregistryregistries"></a>Microsoft. ContainerRegistry/kayıt defterleri

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ContainerRegistryLoginEvents|Oturum açma olayları|Hayır|
|Containerregistrydepotoryevents|Olay günlüklerini günlüğe kaydeder|Hayır|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft. ContainerService/Managedkümeler

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|küme-otomatik Scaler|Kubernetes kümesi otomatik Scaler|Hayır|
|Guard|Guard|Hayır|
|kuin-apiserver|Kubernetes API sunucusu|Hayır|
|kuin-denetim|Kubernetes denetimi|Hayır|
|Kume-denetim-yönetici|Kubernetes denetimi yönetici günlükleri|Hayır|
|kuin-Controller-Manager|Kubernetes Denetleyici Yöneticisi|Hayır|
|kuin-Scheduler|Kubernetes Zamanlayıcı|Hayır|


## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft. CustomProviders/resourceproviders

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AuditLogs|Minırp çağrıları için denetim günlükleri|Hayır|


## <a name="microsoftd365customerinsightsinstances"></a>Microsoft. D365CustomerInsights/örnekleri

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Denetim|Denetim olayları|Hayır|
|Operasyonel|İşletimsel olaylar|Hayır|


## <a name="microsoftdatabricksworkspaces"></a>Microsoft. Databricks/çalışma alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|accounts|Databricks hesapları|Hayır|
|leriniz|Databricks kümeleri|Hayır|
|dBFS|Databricks Dosya Sistemi|Hayır|
|ınstancepools|Örnek havuzları|Hayır|
|Çizelge|Databricks Işleri|Hayır|
|not defteri|Databricks Not Defteri|Hayır|
|kaynaklanır|Databricks gizli dizileri|Hayır|
|sqlPermissions|Databricks SQLPermissions|Hayır|
|SSH|Databricks SSH|Hayır|
|çalışma alanı|Databricks çalışma alanı|Hayır|


## <a name="microsoftdatacollaborationworkspaces"></a>Microsoft. Dataişbirliği/çalışma alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Ortak çalışan Ationaudit|İşbirliği denetimi|Yes|
|Veri varlıkları|Veri varlıkları|Hayır|
|Pipelines|Pipelines|Hayır|
|Tekliflerden|Tekliflerden|Hayır|
|Betikler|Betikler|Hayır|


## <a name="microsoftdatafactoryfactories"></a>Microsoft. DataFactory/Factory

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Etkinlik çalıştırmaları|İşlem hattı etkinlik günlüğü çalıştırmaları|Hayır|
|Ardışık düzen eylemsizlik|İşlem hattı çalıştırmaları günlüğü|Hayır|
|Ssisıntegrationruntimelogs 'lar|SSIS tümleştirme çalışma zamanı günlükleri|Hayır|
|SSISPackageEventMessageContext|SSIS paketi olay iletisi bağlamı|Hayır|
|SSISPackageEventMessages|SSIS paketi olay iletileri|Hayır|
|Ssispackageyürütülebilirstatıstıcs|SSIS paketi yürütülebilir istatistikleri|Hayır|
|Ssispackageexecutioncomponentaşamalar|SSIS paketi yürütme bileşeni aşamaları|Hayır|
|Ssispackageexecutiondatastatıstıcs|SSIS paketi veri istatistikleri istatistikleri|Hayır|
|Triggerçalıştırmaları|Tetikleyici çalıştırma günlüğü|Hayır|


## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft. DataLakeAnalytics/hesapları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Denetim|Denetim Günlükleri|Hayır|
|İstekler|İstek günlükleri|Hayır|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft. DataLakeStore/accounts

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Denetim|Denetim Günlükleri|Hayır|
|İstekler|İstek günlükleri|Hayır|


## <a name="microsoftdatashareaccounts"></a>Microsoft. DataShare/hesapları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ReceivedShareSnapshots|Alınan paylaşma anlık görüntüleri|Hayır|
|SentShareSnapshots|Gönderilen paylaşılan anlık görüntüler|Hayır|
|Paylaşımlar|Paylaşımlar|Hayır|
|Parça sayısı|Abonelikleri paylaşma|Hayır|


## <a name="microsoftdbformariadbservers"></a>Microsoft. Dbformarıdb/sunucular

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|MySqlAuditLogs|MariaDB denetim günlükleri|Hayır|
|Mysqlyavaşlatma günlükleri|MariaDB sunucu günlükleri|Hayır|


## <a name="microsoftdbformysqlflexibleservers"></a>Microsoft. Dbformyısql/Flexibelservers

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|MySqlAuditLogs|MySQL denetim günlükleri|Hayır|
|Mysqlyavaşlatma günlükleri|MySQL yavaş Günlükler|Hayır|


## <a name="microsoftdbformysqlservers"></a>Microsoft. Dbformyısql/sunucuları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|MySqlAuditLogs|MySQL denetim günlükleri|Hayır|
|Mysqlyavaşlatma günlükleri|MySQL Server günlükleri|Hayır|


## <a name="microsoftdbforpostgresqlflexibleservers"></a>Microsoft. DBforPostgreSQL/Flexibtaservers

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Postgressqllogs|PostgreSQL sunucu günlükleri|Hayır|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft. DBforPostgreSQL/sunucuları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Postgressqllogs|PostgreSQL sunucu günlükleri|Hayır|
|QueryStoreRuntimeStatistics|PostgreSQL sorgu deposu çalışma zamanı Istatistikleri|Hayır|
|QueryStoreWaitStatistics|PostgreSQL sorgu deposu bekleme Istatistikleri|Hayır|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft. DBforPostgreSQL/serversv2

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Postgressqllogs|PostgreSQL sunucu günlükleri|Hayır|


## <a name="microsoftdesktopvirtualizationapplicationgroups"></a>Microsoft. DesktopVirtualization/applicationgroups

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Checkpoint|Checkpoint|Hayır|
|Hata|Hata|Hayır|
|Yönetim|Yönetim|Hayır|


## <a name="microsoftdesktopvirtualizationhostpools"></a>Microsoft. DesktopVirtualization/hostpools

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Yazılım durumu|Yazılım durumu|Hayır|
|Checkpoint|Checkpoint|Hayır|
|Bağlantı|Bağlantı|Hayır|
|Hata|Hata|Hayır|
|HostRegistration|HostRegistration|Hayır|
|Yönetim|Yönetim|Hayır|


## <a name="microsoftdesktopvirtualizationworkspaces"></a>Microsoft. DesktopVirtualization/çalışma alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Checkpoint|Checkpoint|Hayır|
|Hata|Hata|Hayır|
|Akış|Akış|Hayır|
|Yönetim|Yönetim|Hayır|


## <a name="microsoftdeviceselasticpoolsiothubtenants"></a>Microsoft. Devices/Elaun havuzları/Iothubkiracılar

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|C2DCommands|C2D komutları|Hayır|
|C2DTwinOperations|C2D Ikizi Işlemleri|Hayır|
|Yapılandırmalar|Yapılandırmalar|Hayır|
|Bağlantılar|Bağlantılar|Hayır|
|D2CTwinOperations|D2CTwinOperations|Hayır|
|Deviceıdentityoperations|Cihaz kimliği Işlemleri|Hayır|
|DeviceStreams|Cihaz akışları (Önizleme)|Hayır|
|Devicetelemetri|Cihaz Telemetrisi|Hayır|
|DirectMethods|Doğrudan Yöntemler|Hayır|
|Distributedizleme|Dağıtılmış Izleme (Önizleme)|Hayır|
|FileUploadOperations|Karşıya dosya yükleme Işlemleri|Hayır|
|JobsOperations|İş Işlemleri|Hayır|
|Yollar|Yollar|Hayır|
|TwinQueries|İkizi sorguları|Hayır|


## <a name="microsoftdevicesiothubs"></a>Microsoft. Devices/IotHubs

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|C2DCommands|C2D komutları|Hayır|
|C2DTwinOperations|C2D Ikizi Işlemleri|Hayır|
|Yapılandırmalar|Yapılandırmalar|Hayır|
|Bağlantılar|Bağlantılar|Hayır|
|D2CTwinOperations|D2CTwinOperations|Hayır|
|Deviceıdentityoperations|Cihaz kimliği Işlemleri|Hayır|
|DeviceStreams|Cihaz akışları (Önizleme)|Hayır|
|Devicetelemetri|Cihaz Telemetrisi|Hayır|
|DirectMethods|Doğrudan Yöntemler|Hayır|
|Distributedizleme|Dağıtılmış Izleme (Önizleme)|Hayır|
|FileUploadOperations|Karşıya dosya yükleme Işlemleri|Hayır|
|JobsOperations|İş Işlemleri|Hayır|
|Yollar|Yollar|Hayır|
|TwinQueries|İkizi sorguları|Hayır|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft. Devices/provisioningServices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|DeviceOperations Işlemleri|Cihaz Işlemleri|Hayır|
|ServiceOperations|Hizmet İşlemleri|Hayır|


## <a name="microsoftdigitaltwinsdigitaltwinsinstances"></a>Microsoft. DigitalTwins/Digitaltwınsınstances

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|DigitalTwinsOperation|DigitalTwinsOperation|Hayır|
|EventRoutesOperation|EventRoutesOperation|Hayır|
|ModelsOperation|ModelsOperation|Hayır|
|Queryoperation başvurusu destekleniyor|Queryoperation başvurusu destekleniyor|Hayır|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>UmentDB/databaseAccounts Microsoft.Doc

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|CassandraRequests|CassandraRequests|Hayır|
|ControlPlaneRequests|ControlPlaneRequests|Hayır|
|DataPlaneRequests|DataPlaneRequests|Hayır|
|GremlinRequests|GremlinRequests|Hayır|
|MongoRequests|MongoRequests|Hayır|
|PartitionKeyRUConsumption|PartitionKeyRUConsumption|Hayır|
|PartitionKeyStatistics|PartitionKeyStatistics|Hayır|
|QueryRuntimeStatistics|QueryRuntimeStatistics|Hayır|


## <a name="microsofteventgriddomains"></a>Microsoft. EventGrid/Domains

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Deliveryarızaları|Teslim hatası günlükleri|Hayır|
|Publisharızaları|Hata günlüklerini Yayımla|Hayır|


## <a name="microsofteventgridpartnernamespaces"></a>Microsoft. EventGrid/partnerNamespaces

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Deliveryarızaları|Teslim hatası günlükleri|Hayır|
|Publisharızaları|Hata günlüklerini Yayımla|Hayır|


## <a name="microsofteventgridpartnertopics"></a>Microsoft. EventGrid/Partnerkonuları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Deliveryarızaları|Teslim hatası günlükleri|Hayır|


## <a name="microsofteventgridsystemtopics"></a>Microsoft. EventGrid/Systemkonuları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Deliveryarızaları|Teslim hatası günlükleri|Hayır|


## <a name="microsofteventgridtopics"></a>Microsoft. EventGrid/konuları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Deliveryarızaları|Teslim hatası günlükleri|Hayır|
|Publisharızaları|Hata günlüklerini Yayımla|Hayır|


## <a name="microsofteventhubnamespaces"></a>Microsoft. EventHub/ad alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ArchiveLogs|Arşiv günlükleri|Hayır|
|Oto Scalelogs|Günlükleri otomatik ölçeklendir|Hayır|
|CustomerManagedKeyUserLogs|Müşteri tarafından yönetilen anahtar günlükleri|Hayır|
|EventHubVNetConnectionEvent|VNet/IP filtreleme bağlantı günlükleri|Hayır|
|KafkaCoordinatorLogs|Kafka Coordinator günlükleri|Hayır|
|KafkaUserErrorLogs|Kafka Kullanıcı hatası günlükleri|Hayır|
|OperationalLogs|İşletimsel Günlükler|Hayır|


## <a name="microsoftexperimentationexperimentworkspaces"></a>Microsoft. deneme/experimentWorkspaces

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|İstek|İstek|Hayır|


## <a name="microsofthealthcareapisservices"></a>Microsoft. Healthgelişme API 'leri/Hizmetleri

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AuditLogs|Denetim günlükleri|Hayır|


## <a name="microsoftinsightsautoscalesettings"></a>Microsoft. Insights/oto scalesettings

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Oto Scaledeğerlendirmeleri|Otomatik ölçeklendirme değerlendirmeleri|Hayır|
|Oto Scalescaleactions|Otomatik ölçeklendirme ölçeklendirme eylemleri|Hayır|


## <a name="microsoftinsightscomponents"></a>Microsoft. Insights/bileşenler

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AppAvailabilityResults|Kullanılabilirlik sonuçları|Hayır|
|Appbrowserzamanlamalar|Tarayıcı zamanlamaları|Hayır|
|AppDependencies|Bağımlılıklar|Hayır|
|AppEvents|Ekinlikler|Hayır|
|AppExceptions|Özel durumlar|Hayır|
|Appölçümler|Ölçümler|Hayır|
|AppPageViews|Sayfa görünümleri|Hayır|
|AppPerformanceCounters|Performans sayaçları|Hayır|
|AppRequests|İstekler|Hayır|
|AppSystemEvents|Sistem olayları|Hayır|
|Appizlemeler|İzlemeler|Hayır|


## <a name="microsoftiotspacesgraph"></a>Microsoft. ıotspaces/Graph

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Denetim|Denetim|Hayır|
|Çıkış|Çıkış|Hayır|
|Giriş|Giriş|Hayır|
|Operasyonel|Operasyonel|Hayır|
|İzleme|İzleme|Hayır|
|UserDefinedFunction|UserDefinedFunction|Hayır|


## <a name="microsoftkeyvaultmanagedhsms"></a>Microsoft. keykasası/managedhsms

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AuditEvent|Denetim Olayı|Hayır|


## <a name="microsoftkeyvaultvaults"></a>Microsoft. Keykasası/kasa

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AuditEvent|Denetim Günlükleri|Hayır|


## <a name="microsoftkustoclusters"></a>Microsoft. kusto/kümeler

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Komut|Komut|Hayır|
|FailedIngestion|Başarısız alma işlemleri|Hayır|
|İçeri toplu işleme|Alma toplu işlemi|Hayır|
|Sorgu|Sorgu|Hayır|
|SucceededIngestion|Başarılı alma işlemleri|Hayır|
|Tabloayrıntıları|Tablo ayrıntıları|Hayır|
|Tablousagestatistika|Tablo kullanım istatistikleri|Hayır|


## <a name="microsoftlogicintegrationaccounts"></a>Microsoft. Logic/ıntegrationaccounts

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Integrationaccounttrackingevents|Tümleştirme hesabı izleme olayları|Hayır|


## <a name="microsoftlogicworkflows"></a>Microsoft. Logic/iş akışları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Başlatıldıktan|İş akışı çalışma zamanı tanılama olayları|Hayır|


## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft. MachineLearningServices/çalışma alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AmlComputeClusterEvent|AmlComputeClusterEvent|Hayır|
|AmlComputeClusterNodeEvent|AmlComputeClusterNodeEvent|Hayır|
|AmlComputeCpuGpuUtilization|AmlComputeCpuGpuUtilization|Hayır|
|AmlComputeJobEvent|AmlComputeJobEvent|Hayır|
|AmlRunStatusChangedEvent|AmlRunStatusChangedEvent|Hayır|


## <a name="microsoftmediamediaservices"></a>Microsoft. Media/mediaservices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|KeyDeliveryRequests|Anahtar teslim Istekleri|Hayır|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft. Network/Applicationgateway 'ler

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ApplicationGatewayAccessLog|Application Gateway erişim günlüğü|Hayır|
|ApplicationGatewayFirewallLog|Application Gateway Güvenlik duvarı günlüğü|Hayır|
|ApplicationGatewayPerformanceLog|Application Gateway performans günlüğü|Hayır|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft. Network/azurefirewalls

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AzureFirewallApplicationRule|Azure Güvenlik Duvarı uygulama kuralı|Hayır|
|AzureFirewallDnsProxy|Azure Güvenlik duvarı DNS proxy 'Si|Hayır|
|AzureFirewallNetworkRule|Azure Güvenlik Duvarı ağ kuralı|Hayır|


## <a name="microsoftnetworkbastionhosts"></a>Microsoft. Network/bastionHosts

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Savunma günlükleri|Savunma denetim günlükleri|Hayır|


## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft. Network/Expressroutedevreleri

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|PeeringRouteLog|Yol tablosu günlüklerini eşleme|Hayır|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft. Network/frontkapaklı

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|FrontdoorAccessLog|Frontkapısı erişim günlüğü|Hayır|
|FrontdoorWebApplicationFirewallLog|Frontkapılı Web uygulaması güvenlik duvarı günlüğü|Hayır|


## <a name="microsoftnetworkloadbalancers"></a>Microsoft. Network/loadBalancers

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|LoadBalancerAlertEvent|Uyarı olaylarını Load Balancer|Hayır|
|LoadBalancerProbeHealthStatus|Load Balancer araştırma sistem durumu|Hayır|


## <a name="microsoftnetworknetworksecuritygroups"></a>Microsoft. Network/networksecuritygroups

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|NetworkSecurityGroupEvent|Ağ güvenlik grubu olayı|Hayır|
|NetworkSecurityGroupFlowEvent|Ağ güvenlik grubu kural akışı olayı|Hayır|
|NetworkSecurityGroupRuleCounter|Ağ güvenlik grubu kuralı sayacı|Hayır|


## <a name="microsoftnetworkp2svpngateways"></a>Microsoft. Network/p2sVpnGateways

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|GatewayDiagnosticLog|Ağ geçidi tanılama günlükleri|Hayır|
|Ikediagnosticlog|IKE tanılama günlükleri|Hayır|
|P2SDiagnosticLog|P2S tanılama günlükleri|Hayır|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft. Network/Publicıpaddresses

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|DDoSMitigationFlowLogs|DDoS azaltma kararlarının akış günlükleri|Hayır|
|DDoSMitigationReports|DDoS azaltmaları raporları|Hayır|
|DDoSProtectionNotifications|DDoS koruması bildirimleri|Hayır|


## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft. Network/trafficManagerProfiles

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Probehealthkara Sevents|Araştırma durumu sonuçları olayını Traffic Manager|Hayır|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft. Network/Virtualnetworkgateway 'ler

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|GatewayDiagnosticLog|Ağ geçidi tanılama günlükleri|Hayır|
|Ikediagnosticlog|IKE tanılama günlükleri|Hayır|
|P2SDiagnosticLog|P2S tanılama günlükleri|Hayır|
|RouteDiagnosticLog|Tanılama günlüklerini yönlendirme|Hayır|
|Tüneldiagnosticlog|Tünel tanılama günlükleri|Hayır|


## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft. Network/virtualNetworks

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|VMProtectionAlerts|VM koruma uyarıları|Hayır|


## <a name="microsoftnetworkvpngateways"></a>Microsoft. Network/Vpngateway 'ler

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|GatewayDiagnosticLog|Ağ geçidi tanılama günlükleri|Hayır|
|Ikediagnosticlog|IKE tanılama günlükleri|Hayır|
|RouteDiagnosticLog|Tanılama günlüklerini yönlendirme|Hayır|
|Tüneldiagnosticlog|Tünel tanılama günlükleri|Hayır|


## <a name="microsoftnotificationhubsnamespaces"></a>Microsoft. Notificationhub 'Lar/ad alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|OperationalLogs|İşletimsel Günlükler|Hayır|


## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft. Operationalınsights/çalışma alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Denetim|Denetim Günlükleri|Hayır|


## <a name="microsoftpowerbitenants"></a>Microsoft. PowerBI/kiracılar

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Altyapı|Altyapı|Hayır|


## <a name="microsoftpowerbitenantsworkspaces"></a>Microsoft. PowerBI/kiracılar/çalışma alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Altyapı|Altyapı|Hayır|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft. Powerbiadanmış/kapasiteler

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Altyapı|Altyapı|Hayır|


## <a name="microsoftprojectbabylonaccounts"></a>Microsoft. ProjectBabylon/hesaplar

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ScanStatusLogEvent|ScanStatus|Hayır|


## <a name="microsoftpurviewaccounts"></a>Microsoft. purview/hesapları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|ScanStatusLogEvent|ScanStatus|Hayır|


## <a name="microsoftrecoveryservicesvaults"></a>Microsoft. RecoveryServices/kasa

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AddonAzureBackupAlerts|Eklenti Azure Backup uyarı verisi|Hayır|
|AddonAzureBackupJobs|Eklenti Azure Backup Iş verileri|Hayır|
|AddonAzureBackupPolicy|Eklenti Azure Backup Ilkesi verileri|Hayır|
|AddonAzureBackupProtectedInstance|Eklenti Azure Backup korumalı örnek verileri|Hayır|
|AddonAzureBackupStorage|Eklenti Azure Backup depolama verileri|Hayır|
|AzureBackupReport|Raporlama verilerini Azure Backup|Hayır|
|Azuresterekapak Yevents|Azure Site Recovery olaylar|Hayır|
|AzureSiteRecoveryJobs|Azure Site Recovery Işleri|Hayır|
|AzureSiteRecoveryProtectedDiskDataChurn|Korunan disk verileri dalgalanmasını Azure Site Recovery|Hayır|
|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery kurtarma noktaları|Hayır|
|AzureSiteRecoveryReplicatedItems|Çoğaltılan öğeleri Azure Site Recovery|Hayır|
|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery çoğaltma verilerini karşıya yükleme hızı|Hayır|
|AzureSiteRecoveryReplicationStats|Azure Site Recovery çoğaltma Istatistikleri|Hayır|
|CoreAzureBackup|Çekirdek Azure Backup verileri|Hayır|


## <a name="microsoftrelaynamespaces"></a>Microsoft. Relay/Namespace

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|HybridConnectionsEvent|HybridConnections olayları|Hayır|
|HybridConnectionsLogs|HybridConnectionsLogs|Hayır|


## <a name="microsoftsearchsearchservices"></a>Microsoft. Search/searchServices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|OperationLogs|İşlem günlükleri|Hayır|


## <a name="microsoftservicebusnamespaces"></a>Microsoft. ServiceBus/ad alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|OperationalLogs|İşletimsel Günlükler|Hayır|


## <a name="microsoftsignalrservicesignalr"></a>Microsoft. SignalRService/SignalR

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AllLogs|Azure SignalR hizmeti günlükleri.|Hayır|


## <a name="microsoftsqlmanagedinstances"></a>Microsoft. SQL/ManagedInstances

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Devopsoperationsaudıt|DevOps işlemleri denetim günlükleri|Hayır|
|ResourceUsageStats|Kaynak kullanım Istatistikleri|Hayır|
|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|Hayır|


## <a name="microsoftsqlmanagedinstancesdatabases"></a>Microsoft. SQL/ManagedInstances/veritabanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Hatalar|Hatalar|Hayır|
|QueryStoreRuntimeStatistics|Sorgu deposu çalışma zamanı Istatistikleri|Hayır|
|QueryStoreWaitStatistics|Sorgu deposu bekleme Istatistikleri|Hayır|
|Sqlinsıghts|SQL öngörüleri|Hayır|


## <a name="microsoftsqlserversdatabases"></a>Microsoft. SQL/Servers/veritabanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Otomatik olarak ayarlama|Otomatik ayarlama|Hayır|
|Bloklar|Bloklar|Hayır|
|DatabaseWaitStatistics|Veritabanı bekleme Istatistikleri|Hayır|
|Kilitlenmeler|Kilitlenmeler|Hayır|
|Devopsoperationsaudıt|DevOps işlemleri denetim günlükleri|Hayır|
|Dmsçalışanları|DMS çalışanları|Hayır|
|Hatalar|Hatalar|Hayır|
|ExecRequests|Yürütme Istekleri|Hayır|
|QueryStoreRuntimeStatistics|Sorgu deposu çalışma zamanı Istatistikleri|Hayır|
|QueryStoreWaitStatistics|Sorgu deposu bekleme Istatistikleri|Hayır|
|RequestSteps|İstek adımları|Hayır|
|Sqlinsıghts|SQL öngörüleri|Hayır|
|SqlRequests|SQL Istekleri|Hayır|
|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|Hayır|
|Zaman aşımları|Zaman aşımları|Hayır|
|Bekler|Bekler|Hayır|


## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft. Storage/storageAccounts/blobServices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|StorageDelete|StorageDelete|Yes|
|StorageRead|StorageRead|Yes|
|StorageWrite|StorageWrite|Yes|


## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft. Storage/storageAccounts/fileServices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|StorageDelete|StorageDelete|Yes|
|StorageRead|StorageRead|Yes|
|StorageWrite|StorageWrite|Yes|


## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft. Storage/storageAccounts/queueServices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|StorageDelete|StorageDelete|Yes|
|StorageRead|StorageRead|Yes|
|StorageWrite|StorageWrite|Yes|


## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft. Storage/storageAccounts/tableServices

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|StorageDelete|StorageDelete|Yes|
|StorageRead|StorageRead|Yes|
|StorageWrite|StorageWrite|Yes|


## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft. StreamAnalytics/streamingjobs

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Yazma|Yazma|Hayır|
|Yürütme|Yürütme|Hayır|


## <a name="microsoftsynapseworkspaces"></a>Microsoft. SYNAPSE/çalışma alanları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|BuiltinSqlReqsEnded|Yerleşik SQL havuzu Istekleri sona erdi|Hayır|
|GatewayApiRequests|SYNAPSE ağ geçidi API 'Si Istekleri|Hayır|
|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|Hayır|
|SynapseRbacOperations|SYNAPSE RBAC Işlemleri|Hayır|


## <a name="microsoftsynapseworkspacesbigdatapools"></a>Microsoft. SYNAPSE/Workspaces/bigDataPools

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|BigDataPoolAppsEnded|Büyük veri havuzu uygulamaları sona erdi|Hayır|


## <a name="microsoftsynapseworkspacessqlpools"></a>Microsoft. SYNAPSE/Workspaces/Sqlhavuzlar

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Dmsçalışanları|DMS çalışanları|Hayır|
|ExecRequests|Yürütme Istekleri|Hayır|
|RequestSteps|İstek adımları|Hayır|
|SqlRequests|SQL Istekleri|Hayır|
|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|Hayır|
|Bekler|Bekler|Hayır|


## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft. Timeseriesınsights/ortamlar

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Giriş|Giriş|Hayır|
|Yönetim|Yönetim|Hayır|


## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft. Timeseriesınsights/ortamlar/EventSources

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|Giriş|Giriş|Hayır|
|Yönetim|Yönetim|Hayır|


## <a name="microsoftwebhostingenvironments"></a>Microsoft. Web/hostingenvironments

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AppServiceEnvironmentPlatformLogs|App Service Ortamı platform günlükleri|Hayır|


## <a name="microsoftwebsites"></a>Microsoft. Web/siteler

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AppServiceAntivirusScanAuditLogs|Virüsten koruma denetim günlüklerini raporla|Hayır|
|AppServiceAppLogs|Uygulama günlüklerini App Service|Hayır|
|AppServiceAuditLogs|Denetim günlüklerine erişim|Hayır|
|AppServiceConsoleLogs|Konsol günlüklerini App Service|Hayır|
|AppServiceFileAuditLogs|Site Içeriği değişikliği denetim günlükleri|Hayır|
|AppServiceHTTPLogs|HTTP günlükleri|Hayır|
|AppServiceIPSecAuditLogs|IPSecurity denetim günlükleri|Hayır|
|AppServicePlatformLogs|App Service platform günlükleri|Hayır|
|FunctionAppLogs|İşlev uygulama günlükleri|Hayır|


## <a name="microsoftwebsitesslots"></a>Microsoft. Web/Sites/Yuvaları

|Kategori|Kategori görünen adı|Dışarı aktarılacak maliyetler|
|---|---|---|
|AppServiceAntivirusScanAuditLogs|Virüsten koruma denetim günlüklerini raporla|Hayır|
|AppServiceAppLogs|Uygulama günlüklerini App Service|Hayır|
|AppServiceAuditLogs|Denetim günlüklerine erişim|Hayır|
|AppServiceConsoleLogs|Konsol günlüklerini App Service|Hayır|
|AppServiceFileAuditLogs|Site Içeriği değişikliği denetim günlükleri|Hayır|
|AppServiceHTTPLogs|HTTP günlükleri|Hayır|
|AppServiceIPSecAuditLogs|IPSecurity denetim günlükleri|Hayır|
|AppServicePlatformLogs|App Service platform günlükleri|Hayır|
|FunctionAppLogs|İşlev uygulama günlükleri|Hayır|



## <a name="next-steps"></a>Sonraki Adımlar

* [Kaynak günlükleri hakkında daha fazla bilgi edinin](./platform-logs-overview.md)
* [**Event Hubs** için kaynak kaynağı günlüklerini akışla](./resource-logs.md#send-to-azure-event-hubs)
* [Azure Izleyici REST API kullanarak kaynak günlüğü tanılama ayarlarını değiştirme](/rest/api/monitor/diagnosticsettings)
* [Azure depolama 'daki günlükleri Log Analytics ile analiz etme](./resource-logs.md#send-to-log-analytics-workspace)

