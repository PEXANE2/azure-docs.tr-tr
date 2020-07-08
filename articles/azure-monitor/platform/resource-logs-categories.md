---
title: Azure Izleyici kaynak günlüğü desteklenen Hizmetleri ve kategorileri
description: Azure Izleyici başvurusu, Azure Kaynak günlükleri için desteklenen Hizmetleri ve olay şemasını anlayın.
ms.subservice: logs
ms.topic: reference
ms.date: 06/03/2020
ms.openlocfilehash: 6156b9d311859534160258256ff6315b037f3a14
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84418767"
---
# <a name="supported-categories-for-azure-resource-logs"></a>Azure Kaynak günlükleri için desteklenen Kategoriler

> [!NOTE]
> Kaynak günlükleri daha önce tanılama günlükleri olarak bilinirdi. Azure Izleyici tarafından toplanan günlüklerin türleri yalnızca Azure kaynağına eşit olacak şekilde kaydırılacağı için ad Ekim 2019 ' de değiştirilmiştir.

Azure [izleyici kaynak günlükleri](../../azure-monitor/platform/platform-logs-overview.md) , bu hizmet veya kaynakların çalışmasını tanımlayan Azure hizmetleri tarafından oluşturulan günlüklerdir. Azure Izleyici aracılığıyla kullanılabilen tüm kaynak günlükleri, her bir hizmetin kendi olayları için benzersiz özellikler yaymasını sağlayan ortak bir üst düzey şemayı paylaşır.

Kaynak türünün bir birleşimi ( `resourceId` özellikte mevcuttur) ve `category` bir şemayı benzersiz bir şekilde tanımlar. Daha sonra farklı günlük kategorileri için eklenen hizmete özgü alanları olan tüm kaynak günlüklerine ortak bir şema vardır. Daha fazla bilgi için bkz. [Azure Kaynak günlükleri Için ortak ve hizmete özel şema](resource-logs-categories.md)

## <a name="supported-log-categories-per-resource-type"></a>Kaynak türü başına desteklenen günlük kategorileri

Aşağıda her bir kaynak türü için kullanılabilen günlük türlerinin bir listesi verilmiştir. 

Bazı kategoriler yalnızca belirli kaynak türleri için desteklenir. Bir kaynağınız eksik olduğunu düşünüyorsanız kaynağa özgü belgelere bakın. Örneğin, Microsoft. SQL/Servers/veritabanları kategorileri tüm veritabanı türleri için kullanılamaz. Daha fazla bilgi için bkz. [SQL veritabanı tanılama günlüğü bilgileri](../../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md). 

Hala bir sorun yoksa, bu makalenin alt kısmında bir GitHub açıklaması açabilirsiniz.

## <a name="microsoftaadiamtenants"></a>Microsoft. aadihar/kiracılar

|Kategori|Kategori görünen adı|
|---|---|
|Giriş|Giriş|


## <a name="microsoftanalysisservicesservers"></a>Microsoft. AnalysisServices/sunucuları

|Kategori|Kategori görünen adı|
|---|---|
|Altyapı|Altyapı|
|Hizmet|Hizmet|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Kategori|Kategori görünen adı|
|---|---|
|GatewayLogs|Imanagementpackgateway ile ilgili Günlükler|


## <a name="microsoftappplatformspring"></a>Microsoft. AppPlatform/yay

|Kategori|Kategori görünen adı|
|---|---|
|ApplicationConsole|Uygulama konsolu|
|Sistem günlükleri|Sistem günlükleri|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft. Automation/automationAccounts

|Kategori|Kategori görünen adı|
|---|---|
|JobLogs|İş günlükleri|
|JobStreams|İş akışları|
|DscNodeStatus|DSC düğüm durumu|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Kategori|Kategori görünen adı|
|---|---|
|ServiceLog|Hizmet günlükleri|


## <a name="microsoftbatchaiworkspaces"></a>ChAI/çalışma alanlarını Microsoft.Bat

|Kategori|Kategori görünen adı|
|---|---|
|BaiClusterEvent|BaiClusterEvent|
|BaiClusterNodeEvent|BaiClusterNodeEvent|
|BaiJobEvent|BaiJobEvent|


## <a name="microsoftblockchainblockchainmembers"></a>Microsoft. Blockzincirine/blockchainMembers

|Kategori|Kategori görünen adı|
|---|---|
|BlockchainApplication|Blok zinciri uygulaması|
|Ara sunucu|Ara sunucu|


## <a name="microsoftblockchaincordamembers"></a>Microsoft. Blockzincirine/Cordadmembers

|Kategori|Kategori görünen adı|
|---|---|
|BlockchainApplication|Blok zinciri uygulaması|


## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft. CDN/cdnwebapplicationfirewallpolicies

|Kategori|Kategori görünen adı|
|---|---|
|WebApplicationFirewallLogs|Web uygulaması güvenlik duvarı günlükleri|


## <a name="microsoftcdnprofiles"></a>Microsoft. CDN/profiller

|Kategori|Kategori görünen adı|
|---|---|
|AzureCdnAccessLog|Azure CDN erişim günlüğü|


## <a name="microsoftcdnprofilesendpoints"></a>Microsoft. CDN/profiller/uç noktaları

|Kategori|Kategori görünen adı|
|---|---|
|CoreAnalytics|Uç noktanın ölçümlerini alır (örneğin, bant genişliği, çıkış vb.)|


## <a name="microsoftclassicnetworknetworksecuritygroups"></a>Microsoft. ClassicNetwork/networksecuritygroups

|Kategori|Kategori görünen adı|
|---|---|
|Ağ güvenlik grubu kural akışı olayı|Ağ güvenlik grubu kural akışı olayı|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft. Biliveservices/hesapları

|Kategori|Kategori görünen adı|
|---|---|
|Denetim|Denetim Günlükleri|
|RequestResponse|İstek ve yanıt günlükleri|


## <a name="microsoftcontainerregistryregistries"></a>Microsoft. ContainerRegistry/kayıt defterleri

|Kategori|Kategori görünen adı|
|---|---|
|ContainerRegistryLoginEvents|Oturum açma olayları|
|Containerregistrydepotoryevents|Olay günlüklerini günlüğe kaydeder|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft. ContainerService/Managedkümeler

|Kategori|Kategori görünen adı|
|---|---|
|küme-otomatik Scaler|Kubernetes kümesi otomatik Scaler|
|kuin-apiserver|Kubernetes API sunucusu|
|kuin-denetim|Kubernetes denetimi|
|kuin-Controller-Manager|Kubernetes Denetleyici Yöneticisi|
|kuin-Scheduler|Kubernetes Zamanlayıcı|


## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft. CustomProviders/resourceproviders

|Kategori|Kategori görünen adı|
|---|---|
|AuditLogs|Minırp çağrıları için denetim günlükleri|


## <a name="microsoftdatabricksworkspaces"></a>Microsoft. Databricks/çalışma alanları

|Kategori|Kategori görünen adı|
|---|---|
|accounts|Databricks hesapları|
|leriniz|Databricks kümeleri|
|dBFS|Databricks Dosya Sistemi|
|ınstancepools|Örnek havuzları|
|Çizelge|Databricks Işleri|
|Mini|Databricks Not Defteri|
|kaynaklanır|Databricks gizli dizileri|
|sqlPermissions|Databricks SQLPermissions|
|SSH|Databricks SSH|
|çalışma alanı|Databricks çalışma alanı|


## <a name="microsoftdatacatalogdatacatalogs"></a>Microsoft. DataCatalog/datacatalogs

|Kategori|Kategori görünen adı|
|---|---|
|ScanStatusLogEvent|ScanStatus|


## <a name="microsoftdatafactoryfactories"></a>Microsoft. DataFactory/Factory

|Kategori|Kategori görünen adı|
|---|---|
|Etkinlik çalıştırmaları|İşlem hattı etkinlik günlüğü çalıştırmaları|
|Ardışık düzen eylemsizlik|İşlem hattı çalıştırmaları günlüğü|
|Triggerçalıştırmaları|Tetikleyici çalıştırma günlüğü|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft. DataLakeStore/accounts

|Kategori|Kategori görünen adı|
|---|---|
|Denetim|Denetim Günlükleri|
|İstekler|İstek günlükleri|


## <a name="microsoftdbformariadbservers"></a>Microsoft. Dbformarıdb/sunucular

|Kategori|Kategori görünen adı|
|---|---|
|MySqlAuditLogs|MariaDB denetim günlükleri|
|Mysqlyavaşlatma günlükleri|MariaDB sunucu günlükleri|


## <a name="microsoftdbformysqlservers"></a>Microsoft. Dbformyısql/sunucuları

|Kategori|Kategori görünen adı|
|---|---|
|MySqlAuditLogs|MySQL denetim günlükleri|
|Mysqlyavaşlatma günlükleri|MySQL Server günlükleri|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft. DBforPostgreSQL/sunucuları

|Kategori|Kategori görünen adı|
|---|---|
|Postgressqllogs|PostgreSQL sunucu günlükleri|
|QueryStoreRuntimeStatistics|PostgreSQL sorgu deposu çalışma zamanı Istatistikleri|
|QueryStoreWaitStatistics|PostgreSQL sorgu deposu bekleme Istatistikleri|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft. DBforPostgreSQL/serversv2

|Kategori|Kategori görünen adı|
|---|---|
|Postgressqllogs|PostgreSQL sunucu günlükleri|


## <a name="microsoftdbforpostgresqlsingleservers"></a>Microsoft. DBforPostgreSQL/singleservers

|Kategori|Kategori görünen adı|
|---|---|
|Postgressqllogs|PostgreSQL sunucu günlükleri|


## <a name="microsoftdesktopvirtualizationapplicationgroups"></a>Microsoft. DesktopVirtualization/applicationgroups

|Kategori|Kategori görünen adı|
|---|---|
|Checkpoint|Checkpoint|
|Hata|Hata|
|Yönetim|Yönetim|


## <a name="microsoftdesktopvirtualizationhostpools"></a>Microsoft. DesktopVirtualization/hostpools

|Kategori|Kategori görünen adı|
|---|---|
|Checkpoint|Checkpoint|
|Bağlantı|Bağlantı|
|Hata|Hata|
|HostRegistration|HostRegistration|
|Yönetim|Yönetim|


## <a name="microsoftdesktopvirtualizationworkspaces"></a>Microsoft. DesktopVirtualization/çalışma alanları

|Kategori|Kategori görünen adı|
|---|---|
|Checkpoint|Checkpoint|
|Hata|Hata|
|Akış|Akış|
|Yönetim|Yönetim|


## <a name="microsoftdevicesiothubs"></a>Microsoft. Devices/IotHubs

|Kategori|Kategori görünen adı|
|---|---|
|C2DCommands|C2D komutları|
|C2DTwinOperations|C2D Ikizi Işlemleri|
|Yapılandırmalar|Yapılandırmalar|
|Bağlantılar|Bağlantılar|
|D2CTwinOperations|D2CTwinOperations|
|Deviceıdentityoperations|Cihaz kimliği Işlemleri|
|DeviceStreams|Cihaz akışları (Önizleme)|
|Devicetelemetri|Cihaz Telemetrisi|
|DirectMethods|Doğrudan Yöntemler|
|Distributedizleme|Dağıtılmış Izleme (Önizleme)|
|FileUploadOperations|Karşıya dosya yükleme Işlemleri|
|JobsOperations|İş Işlemleri|
|Yollar|Yollar|
|TwinQueries|İkizi sorguları|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft. Devices/provisioningServices

|Kategori|Kategori görünen adı|
|---|---|
|DeviceOperations Işlemleri|Cihaz Işlemleri|
|ServiceOperations|Hizmet İşlemleri|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>UmentDB/databaseAccounts Microsoft.Doc

|Kategori|Kategori görünen adı|
|---|---|
|CassandraRequests|CassandraRequests|
|ControlPlaneRequests|ControlPlaneRequests|
|DataPlaneRequests|DataPlaneRequests|
|MongoRequests|MongoRequests|
|Partitionkeyrutüketim|Partitionkeyrutüketim|
|PartitionKeyStatistics|PartitionKeyStatistics|
|QueryRuntimeStatistics|QueryRuntimeStatistics|


## <a name="microsoftenterpriseknowledgegraphservices"></a>Microsoft. EnterpriseKnowledgeGraph/hizmetler

|Kategori|Kategori görünen adı|
|---|---|
|AuditEvent|AuditEvent günlüğü|
|Veri sorunu|Datasorun günlüğü|
|İstekler|Yapılandırma günlüğü|

## <a name="microsofteventgriddomains"></a>Microsoft. EventGrid/Domains

|Kategori|Kategori görünen adı|
|---|---|
|Deliveryarızaları|Teslim hatası günlükleri|
|Publisharızaları|Hata günlüklerini Yayımla|


## <a name="microsofteventgridsystemtopics"></a>Microsoft. EventGrid/Systemkonuları

|Kategori|Kategori görünen adı|
|---|---|
|Deliveryarızaları|Teslim hatası günlükleri|


## <a name="microsofteventgridtopics"></a>Microsoft. EventGrid/konuları

|Kategori|Kategori görünen adı|
|---|---|
|Deliveryarızaları|Teslim hatası günlükleri|
|Publisharızaları|Hata günlüklerini Yayımla|


## <a name="microsofteventhubnamespaces"></a>Microsoft. EventHub/ad alanları

|Kategori|Kategori görünen adı|
|---|---|
|ArchiveLogs|Arşiv günlükleri|
|Oto Scalelogs|Günlükleri otomatik ölçeklendir|
|CustomerManagedKeyUserLogs|Müşteri tarafından yönetilen anahtar günlükleri|
|EventHubVNetConnectionEvent|VNet/IP filtreleme bağlantı günlükleri|
|KafkaCoordinatorLogs|Kafka Coordinator günlükleri|
|KafkaUserErrorLogs|Kafka Kullanıcı hatası günlükleri|
|OperationalLogs|İşletimsel Günlükler|


## <a name="microsofthealthcareapisservices"></a>Microsoft. Healthgelişme API 'leri/Hizmetleri

|Kategori|Kategori görünen adı|
|---|---|
|AuditLogs|Denetim günlükleri|


## <a name="microsoftinsightsautoscalesettings"></a>Microsoft. Insights/oto Scalesettings

|Kategori|Kategori görünen adı|
|---|---|
|Oto Scaledeğerlendirmeleri|Otomatik ölçeklendirme değerlendirmeleri|
|Oto Scalescaleactions|Otomatik ölçeklendirme ölçeklendirme eylemleri|


## <a name="microsoftinsightscomponents"></a>Microsoft. Insights/bileşenler

|Kategori|Kategori görünen adı|
|---|---|
|AppAvailabilityResults|Kullanılabilirlik sonuçları|
|Appbrowserzamanlamalar|Tarayıcı zamanlamaları|
|AppDependencies|Bağımlılıklar|
|AppEvents|Olaylar|
|AppExceptions|Özel durumlar|
|Appölçümler|Ölçümler|
|AppPageViews|Sayfa görünümleri|
|AppPerformanceCounters|Performans sayaçları|
|AppRequests|İstekler|
|AppSystemEvents|Sistem olayları|
|Appizlemeler|İzlemeler|


## <a name="microsoftiotspacesgraph"></a>Microsoft. ıotspaces/Graph

|Kategori|Kategori görünen adı|
|---|---|
|Denetim|Denetim|
|Çıkış|Çıkış|
|Giriş|Giriş|
|İşlemdeki|İşlemdeki|
|İzleme|İzleme|
|UserDefinedFunction|UserDefinedFunction|


## <a name="microsoftkeyvaultvaults"></a>Microsoft. Keykasası/kasa

|Kategori|Kategori görünen adı|
|---|---|
|AuditEvent|Denetim Günlükleri|


## <a name="microsoftkustoclusters"></a>Microsoft. kusto/kümeler

|Kategori|Kategori görünen adı|
|---|---|
|FailedIngestion|Başarısız alma işlemleri|
|SucceededIngestion|Başarılı alma işlemleri|


## <a name="microsoftlogicintegrationaccounts"></a>Microsoft. Logic/ıntegrationaccounts

|Kategori|Kategori görünen adı|
|---|---|
|Integrationaccounttrackingevents|Tümleştirme hesabı izleme olayları|


## <a name="microsoftlogicworkflows"></a>Microsoft. Logic/iş akışları

|Kategori|Kategori görünen adı|
|---|---|
|Başlatıldıktan|İş akışı çalışma zamanı tanılama olayları|


## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft. MachineLearningServices/çalışma alanları

|Kategori|Kategori görünen adı|
|---|---|
|AmlComputeClusterEvent|AmlComputeClusterEvent|
|AmlComputeClusterNodeEvent|AmlComputeClusterNodeEvent|
|AmlComputeCpuGpuUtilization|AmlComputeCpuGpuUtilization|
|AmlComputeJobEvent|AmlComputeJobEvent|
|AmlRunStatusChangedEvent|AmlRunStatusChangedEvent|


## <a name="microsoftmediamediaservices"></a>Microsoft. Media/mediaservices

|Kategori|Kategori görünen adı|
|---|---|
|KeyDeliveryRequests|Anahtar teslim Istekleri|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft. Network/Applicationgateway 'ler

|Kategori|Kategori görünen adı|
|---|---|
|ApplicationGatewayAccessLog|Application Gateway erişim günlüğü|
|ApplicationGatewayFirewallLog|Application Gateway Güvenlik duvarı günlüğü|
|ApplicationGatewayPerformanceLog|Application Gateway performans günlüğü|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft. Network/azurefirewalls

|Kategori|Kategori görünen adı|
|---|---|
|AzureFirewallApplicationRule|Azure Güvenlik Duvarı uygulama kuralı|
|AzureFirewallNetworkRule|Azure Güvenlik Duvarı ağ kuralı|


## <a name="microsoftnetworkbastionhosts"></a>Microsoft. Network/bastionHosts

|Kategori|Kategori görünen adı|
|---|---|
|Savunma günlükleri|Savunma denetim günlükleri|


## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft. Network/Expressroutedevreleri

|Kategori|Kategori görünen adı|
|---|---|
|PeeringRouteLog|Yol tablosu günlüklerini eşleme|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft. Network/frontkapaklı

|Kategori|Kategori görünen adı|
|---|---|
|FrontdoorAccessLog|Frontkapısı erişim günlüğü|
|FrontdoorWebApplicationFirewallLog|Frontkapılı Web uygulaması güvenlik duvarı günlüğü|


## <a name="microsoftnetworkloadbalancers"></a>Microsoft. Network/loadBalancers

|Kategori|Kategori görünen adı|
|---|---|
|LoadBalancerAlertEvent|Uyarı olaylarını Load Balancer|
|LoadBalancerProbeHealthStatus|Load Balancer araştırma sistem durumu|


## <a name="microsoftnetworknetworksecuritygroups"></a>Microsoft. Network/networksecuritygroups

|Kategori|Kategori görünen adı|
|---|---|
|NetworkSecurityGroupEvent|Ağ güvenlik grubu olayı|
|NetworkSecurityGroupFlowEvent|Ağ güvenlik grubu kural akışı olayı|
|NetworkSecurityGroupRuleCounter|Ağ güvenlik grubu kuralı sayacı|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft. Network/Publicıpaddresses

|Kategori|Kategori görünen adı|
|---|---|
|DDoSMitigationFlowLogs|DDoS azaltma kararlarının akış günlükleri|
|DDoSMitigationReports|DDoS azaltmaları raporları|
|DDoSProtectionNotifications|DDoS koruması bildirimleri|


## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft. Network/trafficManagerProfiles

|Kategori|Kategori görünen adı|
|---|---|
|Probehealthkara Sevents|Araştırma durumu sonuçları olayını Traffic Manager|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft. Network/Virtualnetworkgateway 'ler

|Kategori|Kategori görünen adı|
|---|---|
|GatewayDiagnosticLog|Ağ geçidi tanılama günlükleri|
|Ikediagnosticlog|IKE tanılama günlükleri|
|P2SDiagnosticLog|P2S tanılama günlükleri|
|RouteDiagnosticLog|Tanılama günlüklerini yönlendirme|
|Tüneldiagnosticlog|Tünel tanılama günlükleri|


## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft. Network/virtualNetworks

|Kategori|Kategori görünen adı|
|---|---|
|VMProtectionAlerts|VM koruma uyarıları|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft. Powerbiadanmış/kapasiteler

|Kategori|Kategori görünen adı|
|---|---|
|Altyapı|Altyapı|


## <a name="microsoftrecoveryservicesvaults"></a>Microsoft. RecoveryServices/kasa

|Kategori|Kategori görünen adı|
|---|---|
|AddonAzureBackupAlerts|Eklenti Azure Backup uyarı verisi|
|AddonAzureBackupJobs|Eklenti Azure Backup Iş verileri|
|AddonAzureBackupPolicy|Eklenti Azure Backup Ilkesi verileri|
|AddonAzureBackupProtectedInstance|Eklenti Azure Backup korumalı örnek verileri|
|AddonAzureBackupStorage|Eklenti Azure Backup depolama verileri|
|AzureBackupReport|Raporlama verilerini Azure Backup|
|Azuresterekapak Yevents|Azure Site Recovery olaylar|
|AzureSiteRecoveryJobs|Azure Site Recovery Işleri|
|AzureSiteRecoveryProtectedDiskDataChurn|Korunan disk verileri dalgalanmasını Azure Site Recovery|
|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery kurtarma noktaları|
|AzureSiteRecoveryReplicatedItems|Çoğaltılan öğeleri Azure Site Recovery|
|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery çoğaltma verilerini karşıya yükleme hızı|
|AzureSiteRecoveryReplicationStats|Azure Site Recovery çoğaltma Istatistikleri|
|CoreAzureBackup|Çekirdek Azure Backup verileri|


## <a name="microsoftrelaynamespaces"></a>Microsoft. Relay/Namespace

|Kategori|Kategori görünen adı|
|---|---|
|HybridConnectionsEvent|HybridConnections olayları|


## <a name="microsoftsearchsearchservices"></a>Microsoft. Search/searchServices

|Kategori|Kategori görünen adı|
|---|---|
|OperationLogs|İşlem günlükleri|


## <a name="microsoftservicebusnamespaces"></a>Microsoft. ServiceBus/ad alanları

|Kategori|Kategori görünen adı|
|---|---|
|OperationalLogs|İşletimsel Günlükler|


## <a name="microsoftsignalrservicesignalr"></a>Microsoft. SignalRService/SignalR

|Kategori|Kategori görünen adı|
|---|---|
|AllLogs|Azure SignalR hizmeti günlükleri.|


## <a name="microsoftsqlmanagedinstances"></a>Microsoft. SQL/ManagedInstances

|Kategori|Kategori görünen adı|
|---|---|
|Devopsoperationsaudıt|DevOps işlemleri denetim günlükleri|
|ResourceUsageStats|Kaynak kullanım Istatistikleri|
|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|


## <a name="microsoftsqlmanagedinstancesdatabases"></a>Microsoft. SQL/ManagedInstances/veritabanları

|Kategori|Kategori görünen adı|
|---|---|
|Hatalar|Hatalar|
|QueryStoreRuntimeStatistics|Sorgu deposu çalışma zamanı Istatistikleri|
|QueryStoreWaitStatistics|Sorgu deposu bekleme Istatistikleri|
|Sqlinsıghts|SQL öngörüleri|


## <a name="microsoftsqlserversdatabases"></a>Microsoft. SQL/Servers/veritabanları

|Kategori|Kategori görünen adı|
|---|---|
|Otomatik olarak ayarlama|Otomatik ayarlama|
|Bloklar|Bloklar|
|DatabaseWaitStatistics|Veritabanı bekleme Istatistikleri|
|Çık|Çık|
|Devopsoperationsaudıt|DevOps işlemleri denetim günlükleri|
|Dmsçalışanları|DMS çalışanları|
|Hatalar|Hatalar|
|ExecRequests|Yürütme Istekleri|
|QueryStoreRuntimeStatistics|Sorgu deposu çalışma zamanı Istatistikleri|
|QueryStoreWaitStatistics|Sorgu deposu bekleme Istatistikleri|
|RequestSteps|İstek adımları|
|Sqlinsıghts|SQL öngörüleri|
|SqlRequests|SQL Istekleri|
|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|
|Zaman aşımları|Zaman aşımları|
|Bekler|Bekler|


## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft. Storage/storageAccounts/blobServices

|Kategori|Kategori görünen adı|
|---|---|
|StorageDelete|StorageDelete|
|StorageRead|StorageRead|
|StorageWrite|StorageWrite|


## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft. Storage/storageAccounts/fileServices

|Kategori|Kategori görünen adı|
|---|---|
|StorageDelete|StorageDelete|
|StorageRead|StorageRead|
|StorageWrite|StorageWrite|


## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft. Storage/storageAccounts/queueServices

|Kategori|Kategori görünen adı|
|---|---|
|StorageDelete|StorageDelete|
|StorageRead|StorageRead|
|StorageWrite|StorageWrite|


## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft. Storage/storageAccounts/tableServices

|Kategori|Kategori görünen adı|
|---|---|
|StorageDelete|StorageDelete|
|StorageRead|StorageRead|
|StorageWrite|StorageWrite|


## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft. StreamAnalytics/streamingjobs

|Kategori|Kategori görünen adı|
|---|---|
|Yazma|Yazma|
|Yürütme|Yürütme|


## <a name="microsoftwebhostingenvironments"></a>Microsoft. Web/hostingenvironments

|Kategori|Kategori görünen adı|
|---|---|
|AppServiceEnvironmentPlatformLogs|App Service Ortamı platform günlükleri|


## <a name="microsoftwebsites"></a>Microsoft. Web/siteler

|Kategori|Kategori görünen adı|
|---|---|
|AppServiceAppLogs|Uygulama günlüklerini App Service|
|AppServiceAuditLogs|Denetim günlüklerine erişim|
|AppServiceConsoleLogs|Konsol günlüklerini App Service|
|AppServiceFileAuditLogs|Site Içeriği değişikliği denetim günlükleri|
|AppServiceHTTPLogs|HTTP günlükleri|
|FunctionAppLogs|İşlev uygulama günlükleri|
|ScanLogs|Virüsten koruma taraması günlükleri|


## <a name="microsoftwebsitesslots"></a>Microsoft. Web/Sites/Yuvaları


|Kategori|Kategori görünen adı|
|---|---|
|AppServiceAppLogs|Uygulama günlüklerini App Service|
|AppServiceAuditLogs|Denetim günlüklerine erişim|
|AppServiceConsoleLogs|Konsol günlüklerini App Service|
|AppServiceFileAuditLogs|Site Içeriği değişikliği denetim günlükleri|
|AppServiceHTTPLogs|HTTP günlükleri|
|FunctionAppLogs|İşlev uygulama günlükleri|
|ScanLogs|Virüsten koruma taraması günlükleri|


## <a name="next-steps"></a>Sonraki Adımlar

* [Kaynak günlükleri hakkında daha fazla bilgi edinin](../../azure-monitor/platform/platform-logs-overview.md)
* [**Event Hubs** için kaynak kaynağı günlüklerini akışla](../../azure-monitor/platform/resource-logs-stream-event-hubs.md)
* [Azure Izleyici REST API kullanarak kaynak günlüğü tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Azure depolama 'daki günlükleri Log Analytics ile analiz etme](../../azure-monitor/platform/collect-azure-metrics-logs.md)
