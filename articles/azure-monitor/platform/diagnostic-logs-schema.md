---
title: Azure Kaynak Günlükleri desteklenen hizmetler ve şemalar
description: Azure kaynak günlükleri için desteklenen hizmetleri ve olay şemasını anlayın.
ms.subservice: logs
ms.topic: reference
ms.date: 10/22/2019
ms.openlocfilehash: 8abd8767d9bb7e3c4336f6600b94f6b3f4ea48f1
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81380514"
---
# <a name="supported-services-schemas-and-categories-for-azure-resource-logs"></a>Azure Kaynak Günlükleri için desteklenen hizmetler, şemalar ve kategoriler

> [!NOTE]
> Kaynak günlükleri daha önce tanılama günlükleri olarak biliniyordu.

[Azure Monitor kaynak günlükleri,](../../azure-monitor/platform/platform-logs-overview.md) Azure hizmetleri tarafından bu hizmetlerin veya kaynakların çalışmasını açıklayan günlüklerdir. Azure Monitor aracılığıyla kullanılabilen tüm kaynak günlükleri, her hizmetin kendi etkinlikleri için benzersiz özellikler yaçması için esneklik içeren ortak bir üst düzey şemayı paylaşır.

Kaynak türü `resourceId` (özellik mevcuttur) ve `category` benzersiz bir şema tanımlamak bir arada. Bu makalede, kaynak günlükleri ve her hizmet için şema bağlantıları için üst düzey şema açıklanır.

## <a name="top-level-resource-logs-schema"></a>Üst düzey kaynak şema günlükleri

| Adı | Gerekli/İsteğe Bağlı | Açıklama |
|---|---|---|
| time | Gerekli | Olayın zaman damgası (UTC). |
| resourceId | Gerekli | Olayı yayan kaynağın kaynak kimliği. Kiracı hizmetleri için bu form /kiracı/kiracı-id/sağlayıcı/sağlayıcı adıdır. |
| tenantId | Kiracı günlükleri için gerekli | Bu olayın bağlı olduğu Etkin Dizin kiracısının kiracı kimliği. Bu özellik yalnızca kiracı düzeyindegünlükler için kullanılır, kaynak düzeyinde günlükleri görünmüyor. |
| operationName | Gerekli | Bu olaytarafından temsil edilen işlemin adı. Olay bir RBAC işlemini temsil ediyorsa, bu RBAC işlem adıdır (örn. Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Gerçek belgelenmiş Kaynak Yöneticisi işlemleri olmasalar bile, genellikle Kaynak Yöneticisi işlemi`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`biçiminde modellenmiştir ( ) |
| operationVersion | İsteğe bağlı | OperasyonAdı bir API kullanılarak gerçekleştirildiyse, işlemle ilişkili api sürümü (örn. `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Bu işlemle karşılık gelen API yoksa, sürüm, işlemle ilişkili özelliklerin gelecekte değişmesi durumunda bu işlemin sürümünü temsil eder. |
| category | Gerekli | Olayın günlük kategorisi. Kategori, belirli bir kaynaktaki günlükleri etkinleştirebileceğiniz veya devre dışı kyapabileceğiniz parçalılıktır. Bir olayın özellikleri blob içinde görünen özellikleri belirli bir günlük kategorisi ve kaynak türü içinde aynıdır. Tipik günlük kategorileri "Denetim" "Operasyonel" "Yürütme" ve "İstek" dir. |
| resultType | İsteğe bağlı | Olayın durumu. Tipik değerler, Başlatılmış, Devam Eden, Başarılı, Başarısız, Etkin ve Çözümlenmiş değerleri içerir. |
| resultSignature | İsteğe bağlı | Olayın alt durumu. Bu işlem bir REST API çağrısına karşılık geliyorsa, bu ilgili REST çağrısının HTTP durum kodudur. |
| resultDescription | İsteğe bağlı | Bu işlemin statik metin açıklaması, örneğin. "Depolama dosyalarını alın." |
| durationMs | İsteğe bağlı | Milisaniye cinsinden işlemin süresi. |
| callerIpAddress | İsteğe bağlı | Arayanın IP adresi, işlem, herkese açık bir IP adresine sahip bir varlıktan gelecek bir API çağrısına karşılık geliyorsa. |
| correlationId | İsteğe bağlı | Bir guid, ilgili olaylar kümesini gruplandırmak için kullanılır. Genellikle, iki olay aynı operationName ama iki farklı durumları (örneğin. "Başladı" ve "Başarılı"), aynı korelasyon kimliğini paylaşırlar. Bu, olaylar arasındaki diğer ilişkileri de temsil edebilir. |
| identity | İsteğe bağlı | İşlemi gerçekleştiren kullanıcının veya uygulamanın kimliğini açıklayan bir JSON blob'u. Genellikle bu, etkin dizinden yetkilendirme ve talepleri / JWT belirteci içerir. |
| Düzey | İsteğe bağlı | Olayın önem düzeyi. Bilgilendirme, Uyarı, Hata veya Kritik lerden biri olmalıdır. |
| location | İsteğe bağlı | Olayı yayan kaynağın bölgesi, örneğin. "Doğu ABD" veya "Fransa Güney" |
| properties | İsteğe bağlı | Bu özel etkinlik kategorisiyle ilgili genişletilmiş özellikler. Tüm özel/benzersiz özellikler şema bu "Bölüm B" içine konulmalıdır. |

## <a name="service-specific-schemas-for-resource-logs"></a>Kaynak günlükleri için hizmete özel şemalar
Kaynak tanılama günlükleri için şema kaynak ve günlük kategorisine bağlı olarak değişir. Bu liste, kullanılabilir olan kaynak günlüklerini ve hizmete ve kategoriye özgü şemaya bağlantılar sağlayan tüm hizmetleri gösterir.

| Hizmet | Şema & Dokümanları |
| --- | --- |
| Azure Active Directory | [Genel Bakış](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Denetim günlüğü şeması](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) ve Oturum Açma [şeması](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [API Yönetimi Kaynak Günlükleri](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Uygulama Ağ Geçitleri |[Uygulama Ağ Geçidi için Günlük](../../application-gateway/application-gateway-diagnostics.md) |
| Azure Otomasyonu |[Azure Otomasyonu için günlük analitiği](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Toplu Günlüğe Kaydetme](../../batch/batch-diagnostics.md) |
| MySQL için Azure Veritabanı | [MySQL tanı günlükleri için Azure Veritabanı](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| PostgreSQL için Azure Veritabanı | [PostgreSQL günlükleri için Azure Veritabanı](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Azure Veri Gezgini | [Azure Veri Gezgini günlükleri](/azure/data-explorer/using-diagnostic-logs) |
| Bilişsel Hizmetler | [Azure Bilişsel Hizmetler için Günlük](../../cognitive-services/diagnostic-logging.md) |
| Container Kayıt Defteri | [Azure Kapsayıcı Kayıt Defteri için Günlüğe Kaydetme](../../container-registry/container-registry-diagnostics-audit-logs.md) |
| Content Delivery Network | [CDN için Azure Günlükleri](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Azure Cosmos DB Günlüğü](../../cosmos-db/logging.md) |
| Data Factory | [Azure Monitör'ü kullanarak Veri Fabrikalarını İzle](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Azure Veri Gölü Analitiği için günlüklere erişim](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Azure Veri Gölü Deposu günlüklerine erişim](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Etkinlik Hub'ları günlükleri](../../event-hubs/event-hubs-diagnostic-logs.md) |
| Express Route | Şema mevcut değil. |
| Azure Güvenlik Duvarı | Şema mevcut değil. |
| IoT Hub | [IoT Hub İşlemleri](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Azure Anahtar Kasası Günlüğü](../../key-vault/key-vault-logging.md) |
| Kubernetes Service |[Azure Kubernetes Günlük](../../aks/view-master-logs.md#log-event-schema) |
| Load Balancer |[Azure Load Balancer için Log Analytics](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B özel izleme şeması](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Ağ Güvenlik Grupları |[Ağ güvenlik grupları (NSG’ler) için Log Analytics](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS Koruması | [Azure DDoS Koruma Standardına Yönetme](../../virtual-network/manage-ddos-protection.md) |
| Power BI Ayrılmış | [Azure'da Gömülü Power BI için Günlüğe Kaydetme](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Kurtarma Hizmetleri | [Azure Yedekleme için Veri Modeli](../../backup/backup-azure-reports-data-model.md)|
| Arama |[Arama Trafiği Analitiğini etkinleştirme ve kullanma](../../search/search-traffic-analytics.md) |
| Service Bus |[Azure Servis Veri Servisi günlükleri](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Veritabanı | [Azure SQL Veritabanı günlüğü](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[İş günlükleri](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | [Trafik Yöneticisi Günlük şema](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Sanal Ağlar | Şema mevcut değil. |
| Sanal Ağ Geçitleri | Şema mevcut değil. |

## <a name="supported-log-categories-per-resource-type"></a>Kaynak türü başına desteklenen günlük kategorileri

Bazı kategoriler yalnızca belirli kaynak türleri için desteklenebilir. Bu, bir biçimde kullanılabilen tüm bunların listesidir.  Örneğin, Microsoft.Sql/servers/databases kategorileri tüm veritabanları için kullanılamaz. Daha fazla bilgi için [SQL Veritabanı tanılama günlüğü hakkında bilgi](../../sql-database/sql-database-metrics-diag-logging.md)edinin. 

|Kaynak Türü|Kategori|Kategori Görüntü Adı|
|---|---|---|
|Microsoft.AAD/domainServices|SystemSecurity|SystemSecurity|
|Microsoft.AAD/domainServices|Hesap Yönetimi|Hesap Yönetimi|
|Microsoft.AAD/domainServices|LogonLogoff|LogonLogoff|
|Microsoft.AAD/domainServices|ObjectAccess|ObjectAccess|
|Microsoft.AAD/domainServices|İlke Değişimi|İlke Değişimi|
|Microsoft.AAD/domainServices|AyrıcalıkKullanımı|AyrıcalıkKullanımı|
|Microsoft.AAD/domainServices|DetayTakibi|DetayTakibi|
|Microsoft.AAD/domainServices|DizinServiceAccess|DizinServiceAccess|
|Microsoft.AAD/domainServices|Hesap Logon|Hesap Logon|
|microsoft.aadiam/kiracı|Imzalıyorum|Imzalıyorum|
|Microsoft.AnalysisServices/sunucular|Altyapı|Altyapı|
|Microsoft.AnalysisServices/sunucular|Hizmet|Hizmet|
|Microsoft.ApiManagement/service|Ağ Geçidi Günlükleri|ApiManagement Ağ Geçidi ile ilgili günlükler|
|Microsoft.AppPlatform/Bahar|Uygulama Konsolu|Uygulama Konsolu|
|Microsoft.Automation/automationHesapları|JobLogs|İş Günlükleri|
|Microsoft.Automation/automationHesapları|İş Akışları|İş Akışları|
|Microsoft.Automation/automationHesapları|DscNodeDurum|Dsc Düğüm Durumu|
|Microsoft.Batch/batchHesapları|ServiceLog|Servis Günlükleri|
|Microsoft.BatchAI/çalışma alanları|BaiClusterOlay|BaiClusterOlay|
|Microsoft.BatchAI/çalışma alanları|BaiClusterNodeOlay|BaiClusterNodeOlay|
|Microsoft.BatchAI/çalışma alanları|BaiJobOlay|BaiJobOlay|
|Microsoft.Blockchain/blockchainÜyeler|BlockchainApplication|Blockchain Uygulaması|
|Microsoft.Blockchain/blockchainÜyeler|Ara sunucu|Ara sunucu|
|Microsoft.Cdn/profiller/uç noktalar|CoreAnalytics|Uç noktanın ölçümlerini alır, örneğin bant genişliği, çıkış, vb.|
|Microsoft.ClassicNetwork/ağ güvenliği grupları|Ağ Güvenliği Grubu Kural Akışı Olayı|Ağ Güvenliği Grubu Kural Akışı Olayı|
|Microsoft.CognitiveServices/hesapları|Denetim|Denetim Günlükleri|
|Microsoft.CognitiveServices/hesapları|İstek Yanıtı|İstek ve Yanıt Günlükleri|
|Microsoft.ContainerRegistry/registries|Konteyner Kayıt DefteriRepositoryEtkinlikler|RepositoryEvent günlükleri (Önizleme)|
|Microsoft.ContainerRegistry/registries|Konteyner Kayıt LoginEtkinlikler|Giriş Etkinlikleri (Önizleme)|
|Microsoft.ContainerService/managedClusters|kube-apiserver|Kubernetes API Sunucusu|
|Microsoft.ContainerService/managedClusters|kube-denetleyici-yönetici|Kubernetes Kontrolör Yöneticisi|
|Microsoft.ContainerService/managedClusters|kube-programcı|Kubernetes Zamanlayıcı|
|Microsoft.ContainerService/managedClusters|kube-denetim|Kubernetes Denetim|
|Microsoft.ContainerService/managedClusters|küme-otomatik ölçeklendirici|Kubernetes Küme Otomatik Ölçeklendir|
|Microsoft.Databricks/çalışma alanları|dbfs|Databricks Dosya Sistemi|
|Microsoft.Databricks/çalışma alanları|Küme|Databricks Kümeleri|
|Microsoft.Databricks/çalışma alanları|accounts|Databricks Hesapları|
|Microsoft.Databricks/çalışma alanları|Işleri|Databricks İş İlanları|
|Microsoft.Databricks/çalışma alanları|Dizüstü|Databricks Not Defteri|
|Microsoft.Databricks/çalışma alanları|Ssh|Databricks SSH|
|Microsoft.Databricks/çalışma alanları|çalışma alanı|Databricks Çalışma Alanı|
|Microsoft.Databricks/çalışma alanları|Sır -larını|Databricks Sırları|
|Microsoft.Databricks/çalışma alanları|sqlİzinler|Databricks SQLPermissions|
|Microsoft.Databricks/çalışma alanları|instancePools|Örnek Havuzları|
|Microsoft.DataCatalog/datacatalogs|ScanStatusLogOlay|ScanStatus|
|Microsoft.DataFactory/fabrikalar|Etkinlik Çalışır|Boru hattı etkinliği günlüğü çalıştırın|
|Microsoft.DataFactory/fabrikalar|Boru Hattı Seferleri|Ardışık hatlar çalışır günlüğü|
|Microsoft.DataFactory/fabrikalar|TriggerRuns|Tetikleyici günlük çalışır|
|Microsoft.DataLakeAnalytics/hesapları|Denetim|Denetim Günlükleri|
|Microsoft.DataLakeAnalytics/hesapları|İstekler|İstek Günlükleri|
|Microsoft.DataLakeStore/hesapları|Denetim|Denetim Günlükleri|
|Microsoft.DataLakeStore/hesapları|İstekler|İstek Günlükleri|
|Microsoft.DataShare/hesapları|Paylaşımlar|Paylaşımlar|
|Microsoft.DataShare/hesapları|Paylaşımabonelikleri|Abonelikleri Paylaş|
|Microsoft.DataShare/hesapları|SentShareSnapshots|Gönderilen Paylaşım Anlık Görüntüleri|
|Microsoft.DataShare/hesapları|AlınanShareSnapshots|Alınan Paylaşım Anlık Görüntüleri|
|Microsoft.DBforMySQL/sunucular|MySqlSlowLogs|MySQL Sunucu Günlükleri|
|Microsoft.DBforMySQL/sunucular|MySqlAuditLogs|MySQL Denetim Günlükleri|
|Microsoft.DBforPostgreSQL/sunucular|PostgreSQLLogs|PostgreSQL Sunucu Günlükleri|
|Microsoft.DBforPostgreSQL/sunucular|QueryStoreRuntimeİstatistikler|PostgreSQL Sorgu Deposu Çalışma Zamanı İstatistikleri|
|Microsoft.DBforPostgreSQL/sunucular|QueryStoreWaitStatistics|PostgreSQL Sorgu Mağaza Bekleme İstatistikleri|
|Microsoft.DBforPostgreSQL/serversv2|PostgreSQLLogs|PostgreSQL Sunucu Günlükleri|
|Microsoft.DBforPostgreSQL/serversv2|QueryStoreRuntimeİstatistikler|PostgreSQL Sorgu Deposu Çalışma Zamanı İstatistikleri|
|Microsoft.DBforPostgreSQL/serversv2|QueryStoreWaitStatistics|PostgreSQL Sorgu Mağaza Bekleme İstatistikleri|
|Microsoft.DesktopVirtualization/çalışma alanları|Checkpoint|Checkpoint|
|Microsoft.DesktopVirtualization/çalışma alanları|Hata|Hata|
|Microsoft.DesktopVirtualization/çalışma alanları|Yönetim|Yönetim|
|Microsoft.DesktopVirtualization/çalışma alanları|Akış|Akış|
|Microsoft.DesktopVirtualization/applicationGroups|Checkpoint|Checkpoint|
|Microsoft.DesktopVirtualization/applicationGroups|Hata|Hata|
|Microsoft.DesktopVirtualization/applicationGroups|Yönetim|Yönetim|
|Microsoft.DesktopVirtualization/hostPools|Checkpoint|Checkpoint|
|Microsoft.DesktopVirtualization/hostPools|Hata|Hata|
|Microsoft.DesktopVirtualization/hostPools|Yönetim|Yönetim|
|Microsoft.DesktopVirtualization/hostPools|Bağlantı|Bağlantı|
|Microsoft.DesktopVirtualization/hostPools|HostRegistration|HostRegistration|
|Microsoft.Devices/IotHubs|Bağlantılar|Bağlantılar|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Cihaz Telemetrisi|
|Microsoft.Devices/IotHubs|C2DKomutları|C2D Komutları|
|Microsoft.Devices/IotHubs|CihazKimlik İşlemleri|Cihaz Kimlik İşlemleri|
|Microsoft.Devices/IotHubs|FileUpload İşlemleri|Dosya Yükleme İşlemleri|
|Microsoft.Devices/IotHubs|Yollar|Yollar|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|C2D İkiz İşlemler|
|Microsoft.Devices/IotHubs|TwinQueries|İkiz Sorgular|
|Microsoft.Devices/IotHubs|JobsOperations|İşler İşlemleri|
|Microsoft.Devices/IotHubs|Yönlendirme Yöntemleri|Doğrudan Yöntemler|
|Microsoft.Devices/IotHubs|DistributedTracing|Dağıtılmış İzleme (Önizleme)|
|Microsoft.Devices/IotHubs|Yapılandırmalar|Yapılandırmalar|
|Microsoft.Devices/IotHubs|Aygıt Akışları|Aygıt Akışları (Önizleme)|
|Microsoft.Devices/provisioningServices|Cihaz İşlemleri|Cihaz İşlemleri|
|Microsoft.Devices/provisioningServices|Hizmet İşlemleri|Hizmet İşlemleri|
|Microsoft.DocumentDB/databaseHesapları|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseHesapları|Mongoİstek'ler|Mongoİstek'ler|
|Microsoft.DocumentDB/databaseHesapları|QueryRuntimeİstatistikler|QueryRuntimeİstatistikler|
|Microsoft.DocumentDB/databaseHesapları|PartitionKeyStatistics|PartitionKeyStatistics|
|Microsoft.DocumentDB/databaseHesapları|KontrolUçak İstekleri|KontrolUçak İstekleri|
|Microsoft.EnterpriseKnowledgeGraph/hizmetleri|Denetim Etkinliği|AuditEvent günlüğü|
|Microsoft.EnterpriseKnowledgeGraph/hizmetleri|Veri Sorunu|DataIssue günlüğü|
|Microsoft.EnterpriseKnowledgeGraph/hizmetleri|İstekler|Yapılandırma günlüğü|
|Microsoft.EventHub/namespaces|ArşivGünlükleri|Arşiv Günlükleri|
|Microsoft.EventHub/namespaces|OperationalLogs|Operasyonel Günlükler|
|Microsoft.EventHub/namespaces|Otomatik ÖlçekLendirme Günlükleri|Otomatik Ölçek Günlükleri|
|Microsoft.EventHub/namespaces|KafkaCoordinatorLoglar|Kafka Koordinatör Günlükleri|
|Microsoft.EventHub/namespaces|KafkaUserErrorLogs|Kafka Kullanıcı Hata Günlükleri|
|Microsoft.EventHub/namespaces|EventHubvNetConnectionOlay|VNet/IP Filtreleme Bağlantı Günlükleri|
|Microsoft.EventHub/namespaces|MüşteriManagedKeyUserLogs|Müşteri Yönetilen Anahtar Günlükleri|
|Microsoft.HealthcareApis/hizmetler|Denetim Kayıtları|Denetim günlükleri|
|Microsoft.Insights/Otomatik Ölçeklendirme Ayarları|Otomatik ÖlçeklendirmeDeğerlendirmeleri|Otomatik Ölçeklendirme Değerlendirmeleri|
|Microsoft.Insights/Otomatik Ölçeklendirme Ayarları|Otomatik ÖlçeklendirmeHareketleri|Otomatik Ölçeklendirme Eylemleri|
|Microsoft.IoTSpaces/Grafik|İzleme|İzleme|
|Microsoft.IoTSpaces/Grafik|Operasyonel|Operasyonel|
|Microsoft.IoTSpaces/Grafik|Denetim|Denetim|
|Microsoft.IoTSpaces/Grafik|UserDefinedFunction|UserDefinedFunction|
|Microsoft.IoTSpaces/Grafik|Giriş|Giriş|
|Microsoft.IoTSpaces/Grafik|Çıkış|Çıkış|
|Microsoft.KeyVault/vaults|Denetim Etkinliği|Denetim Günlükleri|
|Microsoft.Kusto/Kümeler|Başarılı Yutma|Başarılı yutma işlemleri|
|Microsoft.Kusto/Kümeler|FailedIngestion|Başarısız yutma işlemleri|
|Microsoft.Logic/iş akışları|İş AkışıRuntime|İş akışı çalışma zamanı tanı olayları|
|Microsoft.Logic/integrationAccounts|EntegrasyonHesap Takibi Etkinlikleri|Entegrasyon Hesabı olayları izleme|
|Microsoft.MachineLearningServices/çalışma alanları|AmlComputeClusterOlay|AmlComputeClusterOlay|
|Microsoft.MachineLearningServices/çalışma alanları|AmlComputeClusterNodeEvent|AmlComputeClusterNodeEvent|
|Microsoft.MachineLearningServices/çalışma alanları|AmlComputeJobOlay|AmlComputeJobOlay|
|Microsoft.Media/mediaservices|KeyDeliveryRequests|Anahtar Teslim Talepleri|
|Microsoft.Network/ağ güvenliği grupları|AğGüvenliğiGrubuEtkinlik|Ağ Güvenliği Grubu Etkinliği|
|Microsoft.Network/ağ güvenliği grupları|AğSecurityGroupRuleCounter|Ağ Güvenliği Grubu Kural Sayacı|
|Microsoft.Network/ağ güvenliği grupları|AğGüvenliğiGroupFlowOlay|Ağ Güvenliği Grubu Kural Akışı Olayı|
|Microsoft.Network/publicIPAdresleri|DDoSProtection Bildirimler|DDoS koruma bildirimleri|
|Microsoft.Network/publicIPAdresleri|DDoSMitigationFlowLogs|DDoS azaltma kararlarının akış günlükleri|
|Microsoft.Network/publicIPAdresleri|DDoSMitigationReports|DDoS azaltım raporları|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM koruma uyarıları|
|Microsoft.Network/applicationGateways|UygulamaGatewayAccessLog|Uygulama Ağ Geçidi Erişim Günlüğü|
|Microsoft.Network/applicationGateways|UygulamaGatewayPerformanceLog|Uygulama Ağ Geçidi Performans Günlüğü|
|Microsoft.Network/applicationGateways|UygulamaGatewayFirewallLog|Uygulama Ağ Geçidi Güvenlik Duvarı Günlüğü|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Azure Güvenlik Duvarı Uygulama Kuralı|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Azure Güvenlik Duvarı Ağ Kuralı|
|Microsoft.Network/virtualNetworkAğ Ağ Geçidi|Ağ GeçidiDiagnosticLog|Ağ Geçidi Tanı Günlükleri|
|Microsoft.Network/virtualNetworkAğ Ağ Geçidi|TünelDiagnosticLog|Tünel Tanı Lama Günlükleri|
|Microsoft.Network/virtualNetworkAğ Ağ Geçidi|RouteDiagnosticLog|Rota Tanı lama Günlükleri|
|Microsoft.Network/virtualNetworkAğ Ağ Geçidi|IKEDiagnosticLog|IKE Tanı Günlükleri|
|Microsoft.Network/virtualNetworkAğ Ağ Geçidi|P2SDiagnosticlog|P2S Tanı Günlükleri|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Trafik Yöneticisi Probe Sağlık Sonuçları Olay|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|Peering Route Tablo Günlükleri|
|Microsoft.Network/vpnAğ GeçitLeri|Ağ GeçidiDiagnosticLog|Ağ Geçidi Tanı Günlükleri|
|Microsoft.Network/vpnAğ GeçitLeri|TünelDiagnosticLog|Tünel Tanı Lama Günlükleri|
|Microsoft.Network/vpnAğ GeçitLeri|RouteDiagnosticLog|Rota Tanı lama Günlükleri|
|Microsoft.Network/vpnAğ GeçitLeri|IKEDiagnosticLog|IKE Tanı Günlükleri|
|Microsoft.Network/ön kapılar|FrontdoorAccessLog|Frontdoor Erişim Günlüğü|
|Microsoft.Network/ön kapılar|FrontdoorWebApplicationFirewallLog|Frontdoor Web Uygulaması Güvenlik Duvarı Günlüğü|
|Microsoft.Network/p2sVpnAğ Geçit|Ağ GeçidiDiagnosticLog|Ağ Geçidi Tanı Günlükleri|
|Microsoft.Network/p2sVpnAğ Geçit|IKEDiagnosticLog|IKE Tanı Günlükleri|
|Microsoft.Network/p2sVpnAğ Geçit|P2SDiagnosticlog|P2S Tanı Günlükleri|
|Microsoft.Network/bastionHosts|BurçAuditLoglar|Burç Denetim Günlükleri|
|Microsoft.Network/loadBalancers|LoadBalancerAlertOlay|Yük Dengeleyici Uyarı Etkinlikleri|
|Microsoft.Network/loadBalancers|LoadBalancerProbeSağlıkDurumu|Yük Dengeleyici Probu Sağlık Durumu|
|Microsoft.PowerBIDedicated/kapasiteleri|Altyapı|Altyapı|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure Yedekleme Raporlama Verileri|
|Microsoft.RecoveryServices/Vaults|CoreAzureYedekleme|Çekirdek Azure Yedekleme Verileri|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupJobs|Addon Azure Yedekleme İş Verileri|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupAlerts|Addon Azure Yedekleme Uyarı Verileri|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupPolicy|Addon Azure Yedekleme İlkesi Verileri|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupDepolama|Addon Azure Yedekleme Depolama Verileri|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupProtectedInstance|Addon Azure Yedekleme Korumalı Örnek Verileri|
|Microsoft.RecoveryServices/Vaults|AzureSiteKurtarma İşleri|Azure Site Kurtarma İşleri|
|Microsoft.RecoveryServices/Vaults|AzureSiteKurtarma Etkinlikleri|Azure Site Kurtarma Etkinlikleri|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Kurtarma Çoğaltılan Öğeler|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationİstatistiks|Azure Site Kurtarma Çoğaltma İstatistikleri|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Kurtarma Kurtarma Noktaları|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Kurtarma Çoğaltma Veri Yükleme Hızı|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Azure Site Kurtarma Korumalı Disk Veri Karmaşası|
|Microsoft.Search/searchServices|OperationLogs|İşlem Günlükleri|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Operasyonel Günlükler|
|Microsoft.Sql/servers/veritabanları|SQLInsights|SQL Öngörüleri|
|Microsoft.Sql/servers/veritabanları|Otomatik Tuning|Otomatik ayarlama|
|Microsoft.Sql/servers/veritabanları|QueryStoreRuntimeİstatistikler|Depo Çalışma Zamanı İstatistiklerini Sorgula|
|Microsoft.Sql/servers/veritabanları|QueryStoreWaitStatistics|Mağaza Bekleme İstatistiklerini Sorgula|
|Microsoft.Sql/servers/veritabanları|Hatalar|Hatalar|
|Microsoft.Sql/servers/veritabanları|VeritabanıBekleme İstatistikleri|Veritabanı Bekleme İstatistikleri|
|Microsoft.Sql/servers/veritabanları|Zaman aşımları|Zaman aşımları|
|Microsoft.Sql/servers/veritabanları|Bloklar|Bloklar|
|Microsoft.Sql/servers/veritabanları|Kilitlenmeleri|Kilitlenmeleri|
|Microsoft.Sql/servers/veritabanları|Denetim|Denetim Günlükleri|
|Microsoft.Sql/servers/veritabanları|SQLSecurityAuditEvents|SQL Güvenlik Denetimi Etkinliği|
|Microsoft.Sql/servers/veritabanları|Dmsİşçi|Dms İşçileri|
|Microsoft.Sql/servers/veritabanları|ExecRequests|Exec İstekleri|
|Microsoft.Sql/servers/veritabanları|İstek Adımları|İstek Adımları|
|Microsoft.Sql/servers/veritabanları|SqlRequests|Sql İstekleri|
|Microsoft.Sql/servers/veritabanları|Bekler|Bekler|
|Microsoft.Sql/managedInstances|KaynakKullanımıİstatistiks|Kaynak Kullanım İstatistikleri|
|Microsoft.Sql/managedInstances|SQLSecurityAuditEvents|SQL Güvenlik Denetimi Etkinliği|
|Microsoft.Sql/managedInstances/veritabanları|SQLInsights|SQL Öngörüleri|
|Microsoft.Sql/managedInstances/veritabanları|QueryStoreRuntimeİstatistikler|Depo Çalışma Zamanı İstatistiklerini Sorgula|
|Microsoft.Sql/managedInstances/veritabanları|QueryStoreWaitStatistics|Mağaza Bekleme İstatistiklerini Sorgula|
|Microsoft.Sql/managedInstances/veritabanları|Hatalar|Hatalar|
|Microsoft.Storage/storageAccounts/tableServices|Depolama Oku|Depolama Oku|
|Microsoft.Storage/storageAccounts/tableServices|Depolama Yazma|Depolama Yazma|
|Microsoft.Storage/storageAccounts/tableServices|DepolamaDelete|DepolamaDelete|
|Microsoft.Storage/storageAccounts/blobServices|Depolama Oku|Depolama Oku|
|Microsoft.Storage/storageAccounts/blobServices|Depolama Yazma|Depolama Yazma|
|Microsoft.Storage/storageAccounts/blobServices|DepolamaDelete|DepolamaDelete|
|Microsoft.Storage/storageAccounts/fileServices|Depolama Oku|Depolama Oku|
|Microsoft.Storage/storageAccounts/fileServices|Depolama Yazma|Depolama Yazma|
|Microsoft.Storage/storageAccounts/fileServices|DepolamaDelete|DepolamaDelete|
|Microsoft.Storage/storageAccounts/queueServices|Depolama Oku|Depolama Oku|
|Microsoft.Storage/storageAccounts/queueServices|Depolama Yazma|Depolama Yazma|
|Microsoft.Storage/storageAccounts/queueServices|DepolamaDelete|DepolamaDelete|
|Microsoft.StreamAnalytics/streamingjobs|Yürütme|Yürütme|
|Microsoft.StreamAnalytics/streamingjobs|Yazma|Yazma|
|microsoft.web/hosting ortamları|AppServiceEnvironmentPlatformLogs|Uygulama Hizmet Ortamı Platform Günlükleri|
|microsoft.web/siteler|FunctionAppLogs|Fonksiyon Uygulama Günlükleri|
|microsoft.web/siteler|AppServiceHTTPLogs|HTTP günlükleri|
|microsoft.web/siteler|AppServiceConsoleGünlükler|Uygulama Servis KonsolGünlükleri|
|microsoft.web/siteler|AppServiceAppLogs|Uygulama Hizmeti Uygulama Günlükleri|
|microsoft.web/siteler|AppServiceFileAuditLogs|Site İçeriği Değişikliği Denetim Günlükleri|
|microsoft.web/siteler|AppServiceAuditLogs|Denetim Günlüklerine Erişin|
|microsoft.web/sites/yuvaları|FunctionAppLogs|Fonksiyon Uygulama Günlükleri|
|microsoft.web/sites/yuvaları|AppServiceHTTPLogs|HTTP günlükleri|
|microsoft.web/sites/yuvaları|AppServiceConsoleGünlükler|Konsol Günlükleri|
|microsoft.web/sites/yuvaları|AppServiceAppLogs|Uygulama Günlükleri|
|microsoft.web/sites/yuvaları|AppServiceFileAuditLogs|Site İçeriği Değişikliği Denetim Günlükleri|
|microsoft.web/sites/yuvaları|AppServiceAuditLogs|Denetim Günlüklerine Erişin|

## <a name="next-steps"></a>Sonraki Adımlar

* [Kaynak günlükleri hakkında daha fazla bilgi edinin](../../azure-monitor/platform/platform-logs-overview.md)
* [Kaynak kaynağını Olay **Hub'larına** aktarın](../../azure-monitor/platform/resource-logs-stream-event-hubs.md)
* [Azure Monitor REST API'sini kullanarak kaynak günlüğü tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Log Analytics ile Azure depolamadaki günlükleri analiz edin](../../azure-monitor/platform/collect-azure-metrics-logs.md)
