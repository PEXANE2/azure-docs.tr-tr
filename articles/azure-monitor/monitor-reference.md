---
title: Azure Izleyici tarafından izlenen
description: Azure Izleyici tarafından izlenen tüm hizmetlerin ve diğer kaynakların başvurusu.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/15/2020
ms.openlocfilehash: 1f76966b0ed82ae3d4b32e54627a28e99b0dba2f
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86505729"
---
# <a name="what-is-monitored-by-azure-monitor"></a>Azure Izleyici ile neler izlenir?
Bu makalede, Azure Izleyici tarafından izlenen farklı uygulama ve hizmetler açıklanır. 

## <a name="insights-and-core-solutions"></a>Öngörüler ve temel çözümler
Temel Öngörüler ve çözümler Azure Izleyici 'nin bir parçası olarak kabul edilir ve Azure için destek ve hizmet düzeyi sözleşmelerini izler. Azure Izleyici 'nin kullanılabildiği tüm Azure bölgelerinde desteklenir.

### <a name="insights"></a>Insights

Öngörüler, belirli uygulamalar ve hizmetler için özelleştirilmiş bir izleme deneyimi sağlar. Hem günlükleri hem de ölçümleri toplar ve analiz eder.

| İçgörü | Description |
|:---|:---|
| [Uygulama Bilgileri](app/app-insights-overview.md) | Canlı Web uygulamanızı herhangi bir platformda izlemek için Genişletilebilir uygulama performans yönetimi (APM) hizmeti. |
| [Kapsayıcılar için Azure Izleyici](insights/container-insights-overview.md) | Azure Kubernetes Service (AKS) üzerinde barındırılan Azure Container Instances ya da yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinin performansını izler. |
| [Cosmos DB için Azure Izleyici](insights/cosmosdb-insights-overview.md) | Birleşik etkileşimli bir deneyime sahip tüm Azure Cosmos DB kaynaklarınızın genel performans, başarısızlık, kapasite ve işlemsel sistem durumunun bir görünümünü sağlar. |
| [Ağlar için Azure Izleyici (Önizleme)](insights/network-insights-overview.md) | Tüm ağ kaynağınız için sistem durumu ve ölçümlerinin kapsamlı bir görünümünü sağlar. Gelişmiş arama özelliği, yalnızca Web sitenizin adını arayarak Web sitenizi barındıran kaynağı tanımlama gibi senaryoları etkinleştirerek Kaynak bağımlılıklarını belirlemenize yardımcı olur. |
[Kaynak grupları için Azure Izleyici (Önizleme)](insights/resource-group-insights.md) |  Tek tek kaynaklarınızın karşılaştığı sorunları önceliklendirin ve tanılayın ve kaynak grubunun bir bütün olarak sistem durumu ve performansına göre bağlamı sunar. |
| [Depolama için Azure Izleyici](insights/storage-insights-overview.md) | Azure depolama hizmetleri Performanslarınızın, kapasitesinin ve kullanılabilirliğinden oluşan Birleşik bir görünüm sunarak Azure depolama hesaplarınızın kapsamlı bir şekilde izlenmesini sağlar. |
| [VM'ler için Azure İzleyici](insights/vminsights-overview.md) | Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümelerinizi ölçeklendirerek izler. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder. |
| [Key Vault için Azure Izleyici (Önizleme)](./insights/key-vault-insights-overview.md) | Key Vault isteklerinizin, performanlarınızın ve gecikmelerinden oluşan Birleşik bir görünüm sunarak anahtar kasalarınızın kapsamlı bir şekilde izlenmesini sağlar. |
| [Redin için Azure önbelleği için Azure Izleyici (Önizleme)](insights/redis-cache-insights-overview.md) |  Genel performans, başarısızlık, kapasite ve işlemsel sistem durumunun Birleşik, etkileşimli bir görünümünü sağlar. |


### <a name="core-solutions"></a>Temel çözümler

Çözümler, belirli bir uygulama veya hizmet için özelleştirilmiş günlük sorgularını ve görünümleri temel alır. Bunlar yalnızca günlükleri toplayıp analiz eder ve Öngörüler için zaman içinde kullanım dışıdır.

| Çözüm | Description |
|:---|:---|
| [Aracı sistem durumu](insights/solution-agenthealth.md) | Log Analytics aracılarının sistem durumunu ve yapılandırmasını çözümleyin. |
| [Uyarı yönetimi](platform/alert-management-solution.md) | System Center Operations Manager, Nagios veya Zabbix ' ten toplanan uyarıları analiz edin. |
| [Hizmet Eşlemesi](insights/service-map.md) | Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. |



## <a name="azure-services"></a>Azure hizmetleri
Aşağıdaki tabloda Azure hizmetleri ve Azure Izleyici 'de topladıkları veriler listelenmektedir. 

- Ölçümler-hizmet ölçümleri Azure Izleyici ölçümlerine otomatik olarak toplar. 
- Günlükler-hizmet, Azure Izleyici günlüklerinde platform günlüklerini ve ölçümlerini toplayabilen tanılama ayarlarını destekler.
- Öngörüler-hizmet için özelleştirilmiş bir izleme deneyimi sağlayan, hizmet için sunulan bir öngörü vardır.

| Hizmet | Ölçümler | Günlükler | İçgörü | Notlar |
|:---|:---|:---|:---|:---|
|Active Directory | No | Evet | [Evet](../active-directory/reports-monitoring/howto-use-azure-monitor-workbooks.md) |  |
|Active Directory B2C | Hayır | Hayır | Hayır |  |
|Active Directory Domain Services | No | Evet | No |  |
|Etkinlik günlüğü | No | Evet | No | |
|Gelişmiş Tehdit Koruması | Hayır | Hayır | Hayır |  |
|Advisor | Hayır | Hayır | Hayır |  |
|AI Builder | Hayır | Hayır | Hayır |  |
|Analysis Services | Yes | Yes | No |  |
|FHIR için API | Hayır | Hayır | Hayır |  |
|API Management | Yes | Yes | No |  |
|App Service | Yes | Yes | No |  |
|AppConfig | Hayır | Hayır | Hayır |  |
|Application Gateway | Yes | Yes | No |  |
|Kanıtlama hizmeti | Hayır | Hayır | Hayır |  |
|Otomasyon | Yes | Yes | No |  |
|Azure Service Manager (RDFE) | Hayır | Hayır | Hayır |  |
|Backup | No | Evet | No |  |
|Bastion | Hayır | Hayır | Hayır |  |
|Batch | Yes | Yes | No |  |
|Batch AI | Hayır | Hayır | Hayır |  |
|Blok Zinciri Hizmeti | No | Evet | No |  |
|Blueprints | Hayır | Hayır | Hayır |  |
|Bot Hizmeti | Hayır | Hayır | Hayır |  |
|Cloud Services | Yes | Yes | No | Konuk işletim sistemini ve iş akışlarını izlemek için aracı gereklidir.  |
|Cloud Shell | Hayır | Hayır | Hayır |  |
|Bilişsel Hizmetler | Yes | Yes | No |  |
|Container Instances | Evet | Hayır | Hayır |  |
|Container Registry | Yes | Yes | No |  |
|Content Delivery Network (CDN) | No | Evet | No |  |
|Cosmos DB | Yes | Yes | [Evet](insights/cosmosdb-insights-overview.md) |  |
|Maliyet Yönetimi | Hayır | Hayır | Hayır |  |
|Data Box | Hayır | Hayır | Hayır |  |
|Veri Kataloğu Gen2 | Hayır | Hayır | Hayır |  |
|Veri Gezgini | Yes | Yes | No |  |
|Data Factory | Yes | Yes | No |  |
|Data Factory v2 | No | Evet | No |  |
|Veri Paylaşımı | Hayır | Hayır | Hayır |  |
|MariaDB için Veritabanı | Yes | Yes | No |  |
|MySQL için Veritabanı | Yes | Yes | No |  |
|PostgreSQL için Veritabanı | Yes | Yes | No |  |
|Veritabanı geçiş hizmeti | Hayır | Hayır | Hayır |  |
|Databricks | No | Evet | No |  |
|DDOS Koruması | Yes | Yes | No |  |
|DevOps | Hayır | Hayır | Hayır |  |
|DNS | Evet | Hayır | Hayır |  |
|Etki alanı adları | Hayır | Hayır | Hayır |  |
|DPS | Hayır | Hayır | Hayır |  |
|Dynamics 365 müşteri katılımı | Hayır | Hayır | Hayır |  |
|Dynamics 365 finans ve Işlemler | Hayır | Hayır | Hayır |  |
|Event Grid | Evet | Hayır | Hayır |  |
|Event Hubs | Yes | Yes | No |  |
|ExpressRoute | Yes | Yes | No |  |
|Güvenlik duvarı | Yes | Yes | No |  |
|Front Door | Yes | Yes | No |  |
|İşlevler | Yes | Yes | No |  |
|HDInsight | No | Evet | No |  |
|HPC Önbelleği | Hayır | Hayır | Hayır |  |
|Information Protection | No | Evet | No |  |
|Intune | No | Evet | No |  |
|IoT Central | Hayır | Hayır | Hayır |  |
|IoT Hub | Yes | Yes | No |  |
|Key Vault | Yes | Yes | [Evet](./insights/key-vault-insights-overview.md) |  |
|Kubernetes Hizmeti (AKS) | No | Hayır | [Evet](insights/container-insights-overview.md)  |  |
|Load Balancer | Yes | Yes | No |  |
|Logic Apps | Yes | Yes | No |  |
|Machine Learning Hizmeti | No | Hayır | Hayır |  |
|Yönetilen Uygulamalar  | No | Hayır | Hayır |  |
|Haritalar  | No | Hayır | Hayır |  |
|Media Services | Yes | Yes | No |  |
|Microsoft Flow | No | Hayır | Hayır |  |
|Microsoft Yönetilen Masaüstü | No | Hayır | Hayır |  |
|Microsoft PowerApps | No | Hayır | Hayır |  |
|Microsoft Social Engagement | No | Hayır | Hayır |  |
|Microsoft Stream | Yes | Yes | No |  |
|Geçiş | No | Hayır | Hayır |  |
|Multi-Factor Authentication | No | Evet | No |  |
|Ağ İzleyicisi | Yes | Yes | No |  |
|Notification Hubs | Evet | No | Hayır |  |
|Açık Veri Kümeleri | No | Hayır | Hayır |  |
|İlke | No | Hayır | Hayır |  |
|Power BI Embedded | Yes | Yes | No |  |
|Özel Bağlantı | No | Hayır | Hayır |  |
|Proje biriktirme Iletişim platformu | No | Hayır | Hayır |  |
|Red Hat OpenShift | No | Hayır | Hayır |  |
|Redis Cache | Yes | Yes | [Evet](insights/redis-cache-insights-overview.md) | |
|Kaynak Grafı | No | Hayır | Hayır |  |
|Resource Manager | No | Hayır | Hayır |  |
|Perakende arama-Bing tarafından | Hayır | Hayır | Hayır |  |
|Arayın | Yes | Yes | No |  |
|Service Bus | Yes | Yes | No |  |
|Service Fabric | No | Evet | No | Konuk işletim sistemini ve iş akışlarını izlemek için aracı gereklidir.  |
|Kaydolma portalı | Hayır | Hayır | Hayır |  |
|Site Recovery | No | Evet | No |  |
|Yay bulut hizmeti | Hayır | Hayır | Hayır |  |
|SQL Veri Ambarı | Yes | Yes | No |  |
|SQL Veritabanı | Yes | Yes | No |  |
|SQL Server Stretch Database | Yes | Yes | No |  |
|Yığın | Hayır | Hayır | Hayır |  |
|Depolama | Evet | No | [Evet](insights/storage-insights-overview.md) |  |
|Depolama önbelleği | Hayır | Hayır | Hayır |  |
|Depolama Eşitleme Hizmetleri | Hayır | Hayır | Hayır |  |
|Stream Analytics | Yes | Yes | No |  |
|Time Series Insights | Yes | Yes | No |  |
|TINA | Hayır | Hayır | Hayır |  |
|Traffic Manager | Yes | Yes | No |  |
|Evrensel Yazdırma | Hayır | Hayır | Hayır |  |
|Sanal Makine Ölçek Kümeleri | No | Evet | [Evet](insights/vminsights-overview.md) | Konuk işletim sistemini ve iş akışlarını izlemek için aracı gereklidir. |
|Sanal Makineler | Yes | Yes | [Evet](insights/vminsights-overview.md) | Konuk işletim sistemini ve iş akışlarını izlemek için aracı gereklidir. |
|Sanal Ağ | Yes | Yes | [Evet](insights/network-insights-overview.md) |  |
|Sanal ağ-NSG akış günlükleri | No | Evet | No |  |
|VPN Gateway | Yes | Yes | No |  |
|Windows Sanal Masaüstü | Hayır | Hayır | Hayır |  |


## <a name="product-integrations"></a>Ürün tümleştirmeleri
Aşağıdaki tabloda yer alan hizmetler ve çözümler, Azure Izleyici tarafından toplanan diğer günlük verileriyle çözümlenebilmesi için verilerini bir Log Analytics çalışma alanında depolar.

| Ürün/hizmet | Description |
|:---|:---|
| [Azure Otomasyonu](../automation/index.yml) | Windows ve Linux bilgisayarlarda işletim sistemi güncelleştirmelerini yönetin ve değişiklikleri izleyin. Bkz. [değişiklik izleme](../automation/change-tracking.md) ve [güncelleştirme yönetimi](../automation/automation-update-management.md). |
| [Azure Information Protection](/azure/information-protection/) | Belge ve e-postaları sınıflandırma ve isteğe bağlı olarak koruma. Bkz. [Azure Information Protection Için merkezi raporlama](/azure/information-protection/reports-aip#configure-a-log-analytics-workspace-for-the-reports). |
| [Azure Güvenlik Merkezi](../security-center/index.yml) | Güvenlik olaylarını toplayın ve çözümleyin ve tehdit analizi gerçekleştirin. Bkz. [Azure Güvenlik Merkezi 'Nde veri toplamayı](../security-center/security-center-enable-data-collection.md) |
| [Azure Sentinel](../sentinel/index.yml) | Office 365 ve Amazon Web Services bulut Izi gibi farklı kaynaklara bağlanır. Bkz. [veri kaynaklarını bağlama](../sentinel/connect-data-sources.md). |
| [Microsoft Intune](/intune/) | Günlükleri Azure Izleyici 'ye göndermek için bir tanılama ayarı oluşturun. Bkz. [Intune 'da günlük verilerini depolama, Olay Hub 'ları veya Log Analytics 'e gönderme (Önizleme)](/intune/fundamentals/review-logs-using-azure-monitor).  |
| Ağ  | [Ağ performansı İzleyicisi](insights/network-performance-monitor.md) -hizmet ve uygulama uç noktalarına ağ bağlantısını ve performansını izleyin.<br>[Azure Application Gateway](insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor) -Azure Application Gateway günlükleri ve ölçümleri analiz edin.<br>[Trafik Analizi](../network-watcher/traffic-analytics.md) -Azure bulutunuzda trafik akışına ilişkin Öngörüler sağlamak Için Ağ İzleyicisi ağ güvenlik grubu (NSG) akış günlüklerini analiz eder. |
| [Office 365](insights/solution-office-365.md) | Office 365 ortamınızı izleyin. Azure Sentinel ile sunulan geliştirilmiş ekleme ile güncelleştirilmiş sürüm. |
| [SQL Analytics](insights/azure-sql.md) | Azure SQL veritabanlarının ve SQL yönetilen örneklerinin performansını ölçeklendirerek ve birden çok aboneliğe göre izleyin. |
| [Surface Hub](insights/surface-hubs.md) | Surface Hub cihazların sistem durumunu ve kullanımını izleyin. |
| [System Center Operations Manager](/system-center/scom) | Yönetim gruplarını Azure Izleyici 'ye bağlayarak Operations Manager aracılardan veri toplayın. Bkz. [Azure izleyici 'ye bağlanma Operations Manager](platform/om-agents.md)<br> [Operations Manager değerlendirmesi](insights/scom-assessment.md) çözümüyle System Center Operations Manager yönetim grubunuzun riskini ve sistem durumunu değerlendirin. |
| [Microsoft ekipleri Odalar](/microsoftteams/room-systems/azure-monitor-deploy) | Microsoft ekipleri Oda cihazlarının tümleşik, uçtan uca yönetimi. |
| [Visual Studio App Center](/appcenter/) | Uygulamalar oluşturun, test edin ve dağıtın ve ardından durumlarını ve kullanımını izleyin. Bkz. [App Center ve Application Insights mobil uygulamanızı çözümlemeyi başlatma](learn/mobile-center-quickstart.md). |
| Windows | [Uyumluluk Windows Update](/windows/deployment/update/update-compliance-get-started) -Windows Masaüstü yükseltmelerinizi değerlendirin.<br>[Masaüstü Analizi](/configmgr/desktop-analytics/overview) -Windows istemcilerinizin güncelleştirme hazırlığı hakkında daha bilinçli kararlar almak için öngörüleri ve zekası sağlamak üzere Configuration Manager ile tümleşir. |



## <a name="other-solutions"></a>Diğer çözümler
Farklı uygulama ve hizmetleri izlemek için diğer çözümler mevcuttur, ancak etkin geliştirme durdurulur ve tüm bölgelerde kullanılamayabilir. Bunlar Azure Log Analytics veri alma hizmeti düzeyi sözleşmesi kapsamında ele alınmıştır.

| Çözüm | Description |
|:---|:---|
| [Active Directory sistem durumu denetimi](insights/ad-assessment.md) | Active Directory ortamlarınızın riskini ve sistem durumunu değerlendirin. |
| [Active Directory çoğaltma durumu](insights/ad-replication-status.md) | Active Directory ortamınızı tüm çoğaltma hatalarıyla düzenli olarak izler. |
| [Etkinlik günlüğü Analizi](platform/activity-log.md#activity-log-analytics-monitoring-solution) | Etkinlik günlüğü girdilerini görüntüleyin. |
| [DNS Analizi (Önizleme)](insights/dns-analytics.md) | Windows DNS analitik ve denetim günlüklerini ve DNS sunucularınızdaki diğer ilgili verileri toplar, analiz eder ve ilişkilendirir. |
| [Cloud Foundry](../cloudfoundry/cloudfoundry-oms-nozzle.md) | Cloud Foundry sistem durumu ve performans ölçümlerinizi birden çok dağıtımda toplayın, görüntüleyin ve çözümleyin. |
| [Kapsayıcılar](insights/containers.md) | Docker ve Windows kapsayıcı ana bilgisayarlarını görüntüleyin ve yönetin. |
| [Isteğe bağlı değerlendirmeler](/services-hub/health/getting_started_with_on_demand_assessments) | Şirket içi, karma ve bulut Microsoft teknoloji ortamlarınızın kullanılabilirliğini, güvenliğini ve performansını değerlendirin ve iyileştirin. |
| [SQL sistem durumu denetimi](insights/sql-assessment.md) | SQL Server ortamlarınızın riskini ve sistem durumunu değerlendirin.  |
| [Kablo Verileri](insights/wire-data.md) | Windows ile bağlı ve Linux bağlantılı bilgisayarlardan Log Analytics aracısıyla toplanan birleştirilmiş ağ ve performans verileri. |

## <a name="third-party-integration"></a>Üçüncü taraf tümleştirmesi

| Çözüm | Description |
|:---|:---|
| [ITSM](platform/itsmc-overview.md) | BT Hizmet Yönetimi Bağlayıcısı (ITSMC), Azure ile desteklenen BT Hizmet Yönetimi (ITSM) ürünü/hizmeti arasında bağlantı kurmanızı sağlar.  |


## <a name="resources-outside-of-azure"></a>Azure dışındaki kaynaklar
Azure Izleyici, aşağıdaki tabloda listelenen yöntemleri kullanarak Azure dışındaki kaynaklardan veri toplayabilir.

| Kaynak | Yöntem |
|:---|:---|
| Uygulamalar | Application Insights kullanarak Azure dışındaki Web uygulamalarını izleyin. Bkz. [Application Insights nedir?](./app/app-insights-overview.md). |
| Sanal makineler | Diğer bulut ortamlarında veya Şirket içindeki sanal makinelerin Konuk işletim sisteminden veri toplamak için Log Analytics aracısını kullanın. Bkz. [Log Analytics aracısıyla günlük verilerini toplama](platform/log-analytics-agent.md). |
| REST API Istemcisi | Her bir REST API istemcisinden Azure Izleyici günlüklerine ve ölçümlerine veri yazmak için ayrı API 'Ler kullanılabilir. Bkz. Azure izleyici 'de Günlükler için [http veri toplayıcı API 'si ile günlük verileri gönderme](platform/data-collector-api.md) ve bir [Azure kaynağı için azure izleyici ölçüm REST API deposuna özel ölçümler gönderme](platform/metrics-store-custom-rest-api.md) . |



## <a name="next-steps"></a>Sonraki adımlar

- [Öngörüler ve çözümler tarafından toplanan günlükleri ve ölçümleri depolayan Azure izleyici veri platformu](platform/data-platform.md)hakkında daha fazla bilgi edinin.
- Bir [Azure kaynağını izlemeye yönelik bir öğreticiyi](learn/tutorial-resource-logs.md)doldurun.
- [Azure Izleyici günlüklerinde verileri çözümlemek için günlük sorgusu yazma öğreticisini](learn/tutorial-resource-logs.md)doldurun.
- [Azure Izleyici ölçümlerinde verileri çözümlemek için ölçüm grafiği oluşturma öğreticisini](learn/tutorial-metrics-explorer.md)doldurun.

 
