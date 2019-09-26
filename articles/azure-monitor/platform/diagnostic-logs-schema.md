---
title: Azure tanılama, desteklenen Hizmetleri ve şemaları günlüğe kaydeder
description: Azure tanılama günlükleri için desteklenen Hizmetleri ve olay şemasını anlayın.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 10/11/2018
ms.author: robb
ms.subservice: logs
ms.openlocfilehash: fdcfcbaf99d48a345d2be4da297be1c9139da15c
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71308121"
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Azure tanılama günlükleri için desteklenen hizmetler, şemalar ve Kategoriler

Azure [izleyici tanılama günlükleri](../../azure-monitor/platform/resource-logs-overview.md) , bu hizmet veya kaynakların çalışmasını tanımlayan Azure hizmetleri tarafından oluşturulan günlüklerdir. Azure Izleyici aracılığıyla kullanılabilen tüm tanılama günlükleri, her bir hizmetin kendi olayları için benzersiz özellikler yaymasını sağlayan ortak bir üst düzey şemayı paylaşır.

Kaynak türünün bir birleşimi ( `resourceId` özellikte mevcuttur) `category` ve bir şemayı benzersiz bir şekilde tanımlar. Bu makalede tanılama günlükleri için en üst düzey şema ve her hizmet için şemaların serileştirilmesi bağlantıları açıklanmaktadır.

## <a name="top-level-diagnostic-logs-schema"></a>Üst düzey tanılama günlükleri şeması

| Name | Gerekli/Isteğe bağlı | Açıklama |
|---|---|---|
| time | Gerekli | Olayın zaman damgası (UTC). |
| resourceId | Gerekli | Olayı veren kaynağın kaynak KIMLIĞI. Kiracı Hizmetleri için bu,/Tenants/Tenant-id/Providers/Provider-nameformundadır. |
| tenantId | Kiracı günlükleri için gereklidir | Bu olayın bağlı olduğu Active Directory kiracının kiracı KIMLIĞI. Bu özellik yalnızca kiracı düzeyindeki Günlükler için kullanılır, kaynak düzeyinde günlüklerde görünmez. |
| operationName | Gerekli | Bu olayla temsil edilen işlemin adı. Olay bir RBAC işlemini gösteriyorsa, bu RBAC işlem adıdır (örn. Microsoft. Storage/storageAccounts/blobServices/blob/Read). Genellikle Kaynak Yöneticisi bir işlem biçiminde modellenmiştir, bunlar gerçek belgelenmiş Kaynak Yöneticisi işlemleri (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) olmasalar bile |
| operationVersion | İsteğe Bağlı | OperationName bir API (örn.) kullanılarak gerçekleştirildiyse, işlemle ilişkili api sürümü (ör. `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Bu işleme karşılık gelen bir API yoksa, sürüm, gelecekte işlemle ilişkili özellikler, bu işlemin sürümünü temsil eder. |
| category | Gerekli | Etkinliğin günlük kategorisi. Kategori, belirli bir kaynaktaki günlükleri etkinleştirebilmeniz veya devre dışı bırakabilmeniz için gereken ayrıntı düzeyi. Bir olayın Özellikler blobu içinde görünen özellikler, belirli bir günlük kategorisi ve kaynak türü içinde aynıdır. Tipik günlük kategorileri "Audit" "Işletimsel" "yürütme" ve "Istek" dir. |
| resultType | İsteğe Bağlı | Etkinliğin durumu. Tipik değerler başlangıç, devam etme, başarılı, başarısız, etkin ve Çözümlenmiş. |
| resultSignature | İsteğe Bağlı | Etkinliğin alt durumu. Bu işlem bir REST API çağrısına karşılık geliyorsa, karşılık gelen REST çağrısının HTTP durum kodudur. |
| resultDescription | İsteğe Bağlı | Bu işlemin statik metin açıklaması, örn. "Depolama dosyası Al". |
| durationMs | İsteğe Bağlı | İşlem süresi (milisaniye cinsinden). |
| callerIpAddress | İsteğe Bağlı | İşlem, genel kullanıma açık bir IP adresine sahip bir varlıktan gelen bir API çağrısına karşılık geliyorsa, çağıran IP adresi. |
| correlationId | İsteğe Bağlı | Bir dizi ilgili olayı gruplamak için kullanılan GUID. Genellikle, iki olay aynı operationName öğesine ancak iki farklı durum (örn. "Başlatıldı" ve "başarılı"), aynı bağıntı KIMLIĞINI paylaşır. Bu, olaylar arasındaki diğer ilişkileri de temsil edebilir. |
| identity | İsteğe Bağlı | İşlemi gerçekleştiren kullanıcının veya uygulamanın kimliğini açıklayan bir JSON blobu. Genellikle bu, Active Directory 'den yetkilendirme ve talepler/JWT belirtecini içerir. |
| Düzey | İsteğe Bağlı | Etkinliğin önem düzeyi. Bilgilendirici, uyarı, hata veya kritik bir olmalıdır. |
| location | İsteğe Bağlı | Olayı yayan kaynağın bölgesi, örn. "Doğu ABD" veya "Fransa Güney" |
| properties | İsteğe Bağlı | Bu belirli olay kategorisiyle ilgili genişletilmiş özellikler. Tüm özel/benzersiz özellikler şemanın "Bölüm B" içinde yerleştirilmelidir. |

## <a name="service-specific-schemas-for-resource-diagnostic-logs"></a>Kaynak tanılama günlükleri için hizmete özgü şemalar
Kaynak tanılama günlükleri şeması, kaynak ve günlük kategorisine göre değişir. Bu liste, kullanılabilir olduğunda, hizmete ve kategoriye özgü şemaya kullanılabilir tanılama günlükleri ve bağlantıları oluşturan tüm hizmetleri gösterir.

| Hizmet | Şema & belgeleri |
| --- | --- |
| Azure Active Directory | [Genel bakış](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Denetim günlüğü şeması](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) ve [oturum açma şeması](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [Tanılama günlüklerini API Management](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Uygulama Ağ Geçitleri |[Application Gateway için tanılama günlüğü](../../application-gateway/application-gateway-diagnostics.md) |
| Azure Otomasyonu |[Azure Otomasyonu için Log Analytics](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Tanılama günlük Azure Batch](../../batch/batch-diagnostics.md) |
| MySQL için Azure Veritabanı | [MySQL için Azure veritabanı tanılama günlükleri](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| PostgreSQL için Azure Veritabanı | [PostgreSQL için Azure veritabanı tanılama günlükleri](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Bilişsel Hizmetler | [Azure bilişsel hizmetler için tanılama günlüğü](../../cognitive-services/diagnostic-logging.md) |
| Content Delivery Network | [CDN için Azure tanılama günlükleri](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Günlüğe kaydetme Azure Cosmos DB](../../cosmos-db/logging.md) |
| Data Factory | [Azure Izleyici 'yi kullanarak veri fabrikalarını izleme](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Azure Data Lake Analytics’te tanılama günlüklerine erişim](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Azure Data Lake Store için tanılama günlüklerine erişme](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Event Hubs tanılama günlükleri](../../event-hubs/event-hubs-diagnostic-logs.md) |
| Express Route | Şema kullanılamıyor. |
| Azure Güvenlik Duvarı | Şema kullanılamıyor. |
| IoT Hub | [IoT Hub Işlemler](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Azure Anahtar Kasası Günlüğü](../../key-vault/key-vault-logging.md) |
| Kubernetes Service |[Azure Kubernetes günlüğü](../../aks/view-master-logs.md#log-event-schema) |
| Yük Dengeleyici |[Azure Load Balancer için Log Analytics](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B özel izleme şeması](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Ağ Güvenlik Grupları |[Ağ güvenlik grupları (NSG’ler) için Log Analytics](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS Koruması | [Azure DDoS koruması standardını yönetme](../../virtual-network/manage-ddos-protection.md) |
| Power BI Ayrılmış | [Azure 'da Power BI Embedded için tanılama günlüğü](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Recovery Services | [Azure Backup için veri modeli](../../backup/backup-azure-reports-data-model.md)|
| Ara |[Arama Trafik Analizi etkinleştirme ve kullanma](../../search/search-traffic-analytics.md) |
| Service Bus |[Tanılama günlüklerini Azure Service Bus](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Veritabanı | [Azure SQL veritabanı tanılama günlüğü](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[İş tanılama günlükleri](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | [Traffic Manager log şeması](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Sanal Ağlar | Şema kullanılamıyor. |
| Sanal Ağ Geçitleri | Şema kullanılamıyor. |

## <a name="supported-log-categories-per-resource-type"></a>Kaynak türü başına desteklenen günlük kategorileri
|Kaynak Türü|Category|Kategori görünen adı|
|---|---|---|
|Microsoft. AnalysisServices/sunucuları|Altyapısı|Altyapısı|
|Microsoft. AnalysisServices/sunucuları|Hizmet|Hizmet|
|Microsoft.ApiManagement/service|GatewayLogs|Imanagementpackgateway ile ilgili Günlükler|
|Microsoft. Automation/automationAccounts|JobLogs|İş günlükleri|
|Microsoft. Automation/automationAccounts|JobStreams|İş akışları|
|Microsoft. Automation/automationAccounts|DscNodeStatus|DSC düğüm durumu|
|Microsoft.Batch/batchAccounts|ServiceLog|Hizmet günlükleri|
|Microsoft. CDN/profiller/uç noktaları|CoreAnalytics|Uç noktanın ölçümlerini (örn. bant genişliği, çıkış vb.) alır.|
|Microsoft. ClassicNetwork/networksecuritygroups|Ağ güvenlik grubu kural akışı olayı|Ağ güvenlik grubu kural akışı olayı|
|Microsoft.CognitiveServices/accounts|Denetim|Denetim Günlükleri|
|Microsoft.CognitiveServices/accounts|RequestResponse|İstek ve yanıt günlükleri|
|Microsoft. ContainerService/Managedkümeler|kuin-apiserver|Kubernetes API Sunucusu|
|Microsoft. ContainerService/Managedkümeler|kuin-Controller-Manager|Kubernetes Denetleyici Yöneticisi|
|Microsoft. ContainerService/Managedkümeler|küme-otomatik Scaler|Kubernetes Kümesi Otomatik Ölçeklendiricisi|
|Microsoft. ContainerService/Managedkümeler|kuin-Scheduler|Kubernetes Zamanlayıcısı|
|Microsoft. ContainerService/Managedkümeler|Guard|Kimlik Doğrulama Web Kancası|
|Microsoft. Customerınsights/hub 'ları|AuditEvents|AuditEvents|
|Microsoft. DataFactory/Factory|ActivityRuns|İşlem hattı etkinlik günlüğü çalıştırmaları|
|Microsoft. DataFactory/Factory|PipelineRuns|İşlem hattı çalıştırmaları günlüğü|
|Microsoft. DataFactory/Factory|TriggerRuns|Tetikleyici çalıştırma günlüğü|
|Microsoft.DataLakeAnalytics/accounts|Denetim|Denetim Günlükleri|
|Microsoft.DataLakeAnalytics/accounts|İstekler|İstek günlükleri|
|Microsoft.DataLakeStore/accounts|Denetim|Denetim Günlükleri|
|Microsoft.DataLakeStore/accounts|İstekler|İstek günlükleri|
|Microsoft. Dbformyısql/sunucuları|Mysqlyavaşlatma günlükleri|MySQL Server günlükleri|
|Microsoft. DBforPostgreSQL/sunucuları|Postgressqllogs|PostgreSQL sunucu günlükleri|
|Microsoft.Devices/ıothubs|Bağlantılar|Bağlantılar|
|Microsoft.Devices/ıothubs|Devicetelemetri|Cihaz Telemetrisi|
|Microsoft.Devices/ıothubs|C2DCommands|C2D komutları|
|Microsoft.Devices/ıothubs|DeviceIdentityOperations|Cihaz kimliği Işlemleri|
|Microsoft.Devices/ıothubs|FileUploadOperations|Karşıya dosya yükleme Işlemleri|
|Microsoft.Devices/ıothubs|Yol|Yol|
|Microsoft.Devices/ıothubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/ıothubs|C2DTwinOperations|C2D Ikizi Işlemleri|
|Microsoft.Devices/ıothubs|twinQueries|İkizi sorguları|
|Microsoft.Devices/ıothubs|JobsOperations|İş Işlemleri|
|Microsoft.Devices/ıothubs|DirectMethods|Doğrudan Yöntemler|
|Microsoft.Devices/ıothubs|E2EDiagnostics|E2E tanılama (Önizleme)|
|Microsoft.Devices/ıothubs|Yapılandırmaları|Yapılandırmaları|
|Microsoft. Devices/provisioningServices|DeviceOperations Işlemleri|Cihaz İşlemleri|
|Microsoft. Devices/provisioningServices|ServiceOperations|Hizmet Işlemleri|
|Microsoft. DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft. DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft. DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.EventHub/namespaces|ArchiveLogs|Arşiv günlükleri|
|Microsoft.EventHub/namespaces|OperationalLogs|İşletimsel Günlükler|
|Microsoft.EventHub/namespaces|Oto Scalelogs|Günlükleri otomatik ölçeklendir|
|Microsoft. Insights/oto Scalesettings|Oto Scaledeğerlendirmeleri|Değerlendirmeleri Otomatik Ölçeklendirme|
|Microsoft. Insights/oto Scalesettings|Oto Scalescaleactions|Ölçek Eylemlerini Otomatik Ölçeklendirme|
|Microsoft. ıotspaces/Graph|İzleme|İzleme|
|Microsoft. ıotspaces/Graph|İşletimsel|İşletimsel|
|Microsoft. ıotspaces/Graph|Denetim|Denetim|
|Microsoft. ıotspaces/Graph|UserDefinedFunction|UserDefinedFunction|
|Microsoft. ıotspaces/Graph|Giriş|Giriş|
|Microsoft. ıotspaces/Graph|Çıkış|Çıkış|
|Microsoft.KeyVault/vaults|AuditEvent|Denetim Günlükleri|
|Microsoft.Logic/workflows|Başlatıldıktan|İş akışı çalışma zamanı tanılama olayları|
|Microsoft.Logic/integrationAccounts|Integrationaccounttrackingevents|Tümleştirme hesabı izleme olayları|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Ağ güvenlik grubu olayı|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Ağ güvenlik grubu kuralı sayacı|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Uyarı olaylarını Load Balancer|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Load Balancer araştırma sistem durumu|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS koruması bildirimleri|
|Microsoft.Network/publicIPAddresses|DDoSMitigationFlowLogs|DDoS azaltma kararlarının akış günlükleri|
|Microsoft.Network/publicIPAddresses|DDoSMitigationReports|DDoS azaltmaları raporları|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM koruma uyarıları|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Application Gateway erişim günlüğü|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Application Gateway performans günlüğü|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Application Gateway Güvenlik duvarı günlüğü|
|Microsoft. Network/securegateyollar|AzureFirewallApplicationRule|Azure Güvenlik Duvarı uygulama kuralı|
|Microsoft. Network/securegateyollar|AzureFirewallNetworkRule|Azure Güvenlik Duvarı ağ kuralı|
|Microsoft. Network/azurefirewalls|AzureFirewallApplicationRule|Azure Güvenlik Duvarı uygulama kuralı|
|Microsoft. Network/azurefirewalls|AzureFirewallNetworkRule|Azure Güvenlik Duvarı ağ kuralı|
|Microsoft. Network/Virtualnetworkgateway 'ler|GatewayDiagnosticLog|Ağ geçidi tanılama günlükleri|
|Microsoft. Network/Virtualnetworkgateway 'ler|Tüneldiagnosticlog|Tünel tanılama günlükleri|
|Microsoft. Network/Virtualnetworkgateway 'ler|RouteDiagnosticLog|Tanılama günlüklerini yönlendirme|
|Microsoft. Network/Virtualnetworkgateway 'ler|Ikediagnosticlog|IKE tanılama günlükleri|
|Microsoft. Network/Virtualnetworkgateway 'ler|P2SDiagnosticLog|P2S tanılama günlükleri|
|Microsoft.Network/trafficManagerProfiles|Probehealthkara Sevents|Araştırma durumu sonuçları olayını Traffic Manager|
|Microsoft. Network/Expressroutedevreleri|PeeringRouteLog|Yol tablosu günlüklerini eşleme|
|Microsoft. Network/frontkapaklı|FrontdoorAccessLog|Frontkapısı erişim günlüğü|
|Microsoft. Network/frontkapaklı|FrontdoorWebApplicationFirewallLog|Frontkapılı Web uygulaması güvenlik duvarı günlüğü|
|Microsoft. Powerbiadanmış/kapasiteler|Altyapısı|Altyapısı|
|Microsoft. RecoveryServices/kasa|AzureBackupReport|Azure Backup Raporlama Verileri|
|Microsoft. RecoveryServices/kasa|AzureSiteRecoveryJobs|Azure Site Recovery İşleri|
|Microsoft. RecoveryServices/kasa|AzureSiteRecoveryEvents|Azure Site Recovery Etkinlikleri|
|Microsoft. RecoveryServices/kasa|AzureSiteRecoveryReplicatedItems|Azure Site Recovery Çoğaltılan Öğeler|
|Microsoft. RecoveryServices/kasa|AzureSiteRecoveryReplicationStats|Azure Site Recovery Çoğaltma İstatistikleri|
|Microsoft. RecoveryServices/kasa|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery Kurtarma Noktaları|
|Microsoft. RecoveryServices/kasa|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery çoğaltma verilerini karşıya yükleme hızı|
|Microsoft. RecoveryServices/kasa|AzureSiteRecoveryProtectedDiskDataChurn|Korunan disk verileri dalgalanmasını Azure Site Recovery|
|Microsoft.Search/searchServices|OperationLogs|İşlem günlükleri|
|Microsoft.ServiceBus/namespaces|OperationalLogs|İşletimsel Günlükler|
|Microsoft.Sql/servers/databases|Sqlinsıghts|SQL İçgörüleri|
|Microsoft.Sql/servers/databases|Otomatik olarak ayarlama|Otomatik ayarlama|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Sorgu deposu çalışma zamanı Istatistikleri|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Sorgu deposu bekleme Istatistikleri|
|Microsoft.Sql/servers/databases|Hatalar|Hatalar|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Veritabanı bekleme Istatistikleri|
|Microsoft.Sql/servers/databases|Zaman Aşımları|Zaman Aşımları|
|Microsoft.Sql/servers/databases|Öbekleri|Öbekleri|
|Microsoft.Sql/servers/databases|Kilitlenmeler|Kilitlenmeler|
|Microsoft.Sql/servers/databases|Denetim|Denetim Günlükleri|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|
|Microsoft.Sql/servers/databases|Dmsçalışanları|DMS çalışanları|
|Microsoft.Sql/servers/databases|ExecRequests|Yürütme Istekleri|
|Microsoft.Sql/servers/databases|RequestSteps|İstek adımları|
|Microsoft.Sql/servers/databases|SqlRequests|SQL Istekleri|
|Microsoft.Sql/servers/databases|Bekler|Bekler|
|Microsoft. SQL/ManagedInstances|ResourceUsageStats|Kaynak kullanım Istatistikleri|
|Microsoft. SQL/ManagedInstances|SQLSecurityAuditEvents|SQL güvenlik denetim olayı|
|Microsoft. SQL/ManagedInstances/veritabanları|Sqlinsıghts|SQL İçgörüleri|
|Microsoft. SQL/ManagedInstances/veritabanları|QueryStoreRuntimeStatistics|Sorgu deposu çalışma zamanı Istatistikleri|
|Microsoft. SQL/ManagedInstances/veritabanları|QueryStoreWaitStatistics|Sorgu deposu bekleme Istatistikleri|
|Microsoft. SQL/ManagedInstances/veritabanları|Hatalar|Hatalar|
|Microsoft. StreamAnalytics/streamingjobs|Yürütme|Yürütme|
|Microsoft. StreamAnalytics/streamingjobs|Yazma|Yazma|
|Microsoft. Web/siteler|FunctionExecutionLogs|İşlev yürütme günlükleri|
|Microsoft. Web/Sites/Yuvaları|FunctionExecutionLogs|İşlev yürütme günlükleri|

## <a name="next-steps"></a>Sonraki Adımlar

* [Tanılama günlükleri hakkında daha fazla bilgi edinin](../../azure-monitor/platform/resource-logs-overview.md)
* [Kaynak tanılama günlükleri için Stream **olay hub'ları**](../../azure-monitor/platform/resource-logs-stream-event-hubs.md)
* [Azure İzleyici REST API'sini kullanarak kaynak tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Log Analytics ile Azure depolama biriminden günlüklerini çözümleme](../../azure-monitor/platform/collect-azure-metrics-logs.md)
