---
title: Azure Kaynak günlüklerine genel bakış | Microsoft Docs
description: Azure Kaynak günlükleri için desteklenen Hizmetleri ve olay şemasını anlayın.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: reference
author: rboucher
ms.author: robb
ms.date: 09/20/2019
ms.openlocfilehash: a418e3d1a59379284422d1d24c1457ab61d84a4c
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72551757"
---
# <a name="azure-resource-logs-overview"></a>Azure Kaynak günlüklerine genel bakış
Azure Kaynak günlükleri, iç işlemlerini tanımlayan Azure kaynakları tarafından yayılan [Platform günlüklerdir](platform-logs-overview.md) . Tüm kaynak günlükleri, ortak bir üst düzey şemayı, her hizmet için her bir hizmetin esnekliğiyle paylaşarak, kendi olayları için benzersiz özellikler sunar.

> [!NOTE]
> Kaynak günlükleri daha önce tanılama günlükleri olarak bilinirdi.

## <a name="collecting-resource-logs"></a>Kaynak günlüklerini toplama
Kaynak günlükleri desteklenen Azure kaynakları tarafından otomatik olarak oluşturulur, ancak bunları bir [Tanılama ayarı](diagnostic-settings.md)kullanarak yapılandırmadığınız müddetçe toplanmaz. Günlükleri aşağıdaki hedeflere iletmek için her bir Azure kaynağı için bir tanılama ayarı oluşturun:

| Hedef | Senaryo |
|:---|:---|:---|
| [Log Analytics çalışma alanı](resource-logs-collect-storage.md) | Günlükleri diğer izleme verileriyle çözümleyin ve günlük sorguları ve günlük uyarıları gibi Azure Izleyici özelliklerinden yararlanın. |
| [Azure depolama alanı](archive-diagnostic-logs.md) | Denetim veya yedekleme için günlükleri arşivle. |
| [Olay Hub 'ı](resource-logs-stream-event-hubs.md) | Günlükleri üçüncü taraf günlüğe kaydetme ve telemetri sistemlerine akışa alma.  |

## <a name="compute-resources"></a>İşlem kaynakları
Kaynak günlükleri, Azure işlem kaynaklarındaki Konuk işletim sistemi düzeyi günlüklerinden farklıdır. İşlem kaynakları, olay günlükleri, syslog ve performans sayaçları gibi veriler de dahil olmak üzere Konuk IŞLETIM sisteminden günlükleri ve ölçümleri toplamak için bir aracı gerektirir. Azure sanal makineler ve [Log Analytics aracılarından](agents-overview.md#log-analytics-agent) günlük verilerinin yönlendirilmesini sağlamak Için [Tanılama uzantısını](agents-overview.md#azure-diagnostic-extension) kullanın ve Azure 'daki sanal makinelerden, diğer bulutlarda ve şirket içinde bir Log Analytics çalışma alanına Günlükler ve ölçümler toplayın. Ayrıntılar için bkz. [Azure izleyici için izleme verileri kaynakları](data-sources.md) .

## <a name="resource-logs-schema"></a>Kaynak günlükleri şeması
Kaynak günlükleri hedeflerinden birine yazıldığında her bir Azure hizmeti kendi şemasına sahiptir, ancak hepsi aşağıdaki tabloda yer aldığı en üst düzey şemayı paylaşır. Her hizmet için şemanın bağlantıları için aşağıdaki [hizmete özgü şemalara](#service-specific-schemas) bakın. 

| Adı | Gerekli/Isteğe bağlı | Açıklama |
|---|---|---|
| time | Gereklidir | Olayın zaman damgası (UTC). |
| resourceId | Gereklidir | Olayı veren kaynağın kaynak KIMLIĞI. Kiracı Hizmetleri için bu,/Tenants/Tenant-id/Providers/Provider-nameformundadır. |
| Değerine | Kiracı günlükleri için gereklidir | Bu olayın bağlı olduğu Active Directory kiracının kiracı KIMLIĞI. Bu özellik yalnızca kiracı düzeyindeki Günlükler için kullanılır, kaynak düzeyinde günlüklerde görünmez. |
| operationName | Gereklidir | Bu olayla temsil edilen işlemin adı. Olay bir RBAC işlemini gösteriyorsa, bu RBAC işlem adıdır (örn. Microsoft. Storage/storageAccounts/blobServices/blob/Read). Genellikle Kaynak Yöneticisi bir işlem biçiminde modellenmiştir, bunlar gerçek belgelenmiş Kaynak Yöneticisi işlemleri olmasa bile (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | İsteğe Bağlı | OperationName bir API (örn.) kullanılarak gerçekleştirildiyse, işlemle ilişkili api sürümü (ör. `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Bu işleme karşılık gelen bir API yoksa, sürüm, gelecekte işlemle ilişkili özellikler, bu işlemin sürümünü temsil eder. |
| category | Gereklidir | Etkinliğin günlük kategorisi. Kategori, belirli bir kaynaktaki günlükleri etkinleştirebilmeniz veya devre dışı bırakabilmeniz için gereken ayrıntı düzeyi. Bir olayın Özellikler blobu içinde görünen özellikler, belirli bir günlük kategorisi ve kaynak türü içinde aynıdır. Tipik günlük kategorileri "Audit" "Işletimsel" "yürütme" ve "Istek" dir. |
| resultType | İsteğe Bağlı | Etkinliğin durumu. Tipik değerler başlangıç, devam etme, başarılı, başarısız, etkin ve Çözümlenmiş. |
| resultSignature | İsteğe Bağlı | Etkinliğin alt durumu. Bu işlem bir REST API çağrısına karşılık geliyorsa, karşılık gelen REST çağrısının HTTP durum kodudur. |
| resultDescription | İsteğe Bağlı | Bu işlemin statik metin açıklaması, örn. "Depolama dosyası Al". |
| durationMs | İsteğe Bağlı | İşlem süresi (milisaniye cinsinden). |
| callerIpAddress | İsteğe Bağlı | İşlem, genel kullanıma açık bir IP adresine sahip bir varlıktan gelen bir API çağrısına karşılık geliyorsa, çağıran IP adresi. |
| correlationId | İsteğe Bağlı | Bir dizi ilgili olayı gruplamak için kullanılan GUID. Genellikle, iki olay aynı operationName öğesine ancak iki farklı durum (örn. "Başlatıldı" ve "başarılı"), aynı bağıntı KIMLIĞINI paylaşır. Bu, olaylar arasındaki diğer ilişkileri de temsil edebilir. |
| kimlik | İsteğe Bağlı | İşlemi gerçekleştiren kullanıcının veya uygulamanın kimliğini açıklayan bir JSON blobu. Genellikle bu, Active Directory 'den yetkilendirme ve talepler/JWT belirtecini içerir. |
| Düzey | İsteğe Bağlı | Etkinliğin önem düzeyi. Bilgilendirici, uyarı, hata veya kritik bir olmalıdır. |
| location | İsteğe Bağlı | Olayı yayan kaynağın bölgesi, örn. "Doğu ABD" veya "Fransa Güney" |
| properties | İsteğe Bağlı | Bu belirli olay kategorisiyle ilgili genişletilmiş özellikler. Tüm özel/benzersiz özellikler şemanın "Bölüm B" içinde yerleştirilmelidir. |

## <a name="service-specific-schemas"></a>Hizmete özgü şemalar
Kaynak tanılama günlükleri şeması, `resourceId` özelliği tarafından tanımlanan kaynak türüne ve `category` özelliklerine göre farklılık gösterir. Aşağıdaki listede, kullanılabilir olduğunda, hizmete ve kategoriye özgü şemaya yönelik bağlantılarla kaynak günlüklerini destekleyen tüm Azure hizmetleri gösterilmektedir.

| Hizmet | Şema & belgeleri |
| --- | --- |
| Azure Active Directory | [Genel bakış](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Denetim günlüğü şeması](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) ve [oturum açma şeması](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [Tanılama günlüklerini API Management](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Application Gatewayler |[Application Gateway için tanılama günlüğü](../../application-gateway/application-gateway-diagnostics.md) |
| Azure Otomasyonu |[Azure Otomasyonu için Log Analytics](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Tanılama günlük Azure Batch](../../batch/batch-diagnostics.md) |
| MySQL için Azure Veritabanı | [MySQL için Azure veritabanı tanılama günlükleri](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| PostgreSQL için Azure Veritabanı | [PostgreSQL için Azure veritabanı tanılama günlükleri](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Cognitive Services | [Azure bilişsel hizmetler için tanılama günlüğü](../../cognitive-services/diagnostic-logging.md) |
| İçerik Teslim Ağı | [CDN için Azure tanılama günlükleri](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Günlüğe kaydetme Azure Cosmos DB](../../cosmos-db/logging.md) |
| Data Factory | [Azure Izleyici 'yi kullanarak veri fabrikalarını izleme](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Azure Data Lake Analytics’te tanılama günlüklerine erişim](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Azure Data Lake Store için tanılama günlüklerine erişme](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Event Hubs tanılama günlükleri](../../event-hubs/event-hubs-diagnostic-logs.md) |
| Express Route | Şema kullanılamıyor. |
| Azure Güvenlik Duvarı | Şema kullanılamıyor. |
| IoT Hub | [IoT Hub Işlemler](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Azure Anahtar Kasası Günlüğü](../../key-vault/key-vault-logging.md) |
| Yük Dengeleyici |[Azure Load Balancer için Log Analytics](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B özel izleme şeması](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Ağ Güvenlik Grupları |[Ağ güvenlik grupları (NSG’ler) için Log Analytics](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS Koruması | [Azure DDoS koruması standardını yönetme](../../virtual-network/manage-ddos-protection.md) |
| Power BI Ayrılmış | [Azure 'da Power BI Embedded için tanılama günlüğü](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Kurtarma Hizmetleri | [Azure Backup için veri modeli](../../backup/backup-azure-reports-data-model.md)|
| Arama |[Arama Trafik Analizi etkinleştirme ve kullanma](../../search/search-traffic-analytics.md) |
| Service Bus |[Tanılama günlüklerini Azure Service Bus](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Azure SQL veritabanı tanılama günlüğü](../../sql-database/sql-database-metrics-diag-logging.md) |
| Akış Analizi |[İş tanılama günlükleri](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | [Traffic Manager log şeması](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Sanal Ağlar | Şema kullanılamıyor. |
| Sanal Ağ Geçitleri | Şema kullanılamıyor. |

## <a name="next-steps"></a>Sonraki Adımlar

* Azure 'ı izlemek için kullanabileceğiniz [diğer Azure platformu günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md) .
