---
title: Azure Izleyici tarafından izlenen
description: Azure Izleyici tarafından izlenen tüm hizmetlerin ve diğer kaynakların başvurusu.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/17/2020
ms.openlocfilehash: d605b852d37af3b1e2ef0868084f3c0b99c24784
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76765731"
---
# <a name="what-is-monitored-by-azure-monitor"></a>Azure Izleyici ile neler izlenir?
Bu makalede, Azure Izleyici tarafından izlenen farklı uygulama ve hizmetler açıklanır. 

## <a name="insights-and-core-solutions"></a>Öngörüler ve temel çözümler
Temel Öngörüler ve çözümler Azure Izleyici 'nin bir parçası olarak kabul edilir ve Azure için destek ve hizmet düzeyi sözleşmelerini izler. Azure Izleyici 'nin kullanılabildiği tüm Azure bölgelerinde desteklenir.

### <a name="insights"></a>Bilgiler

Öngörüler, belirli uygulamalar ve hizmetler için özelleştirilmiş bir izleme deneyimi sağlar. Hem günlükleri hem de ölçümleri toplar ve analiz eder.

| İçgörü | Açıklama |
|:---|:---|
| [Application Insights](app/app-insights-overview.md) | Canlı Web uygulamanızı herhangi bir platformda izlemek için Genişletilebilir uygulama performans yönetimi (APM) hizmeti. |
| [Kapsayıcılar için Azure Izleyici](insights/container-insights-overview.md) | Azure Kubernetes Service (AKS) üzerinde barındırılan Azure Container Instances ya da yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinin performansını izler. |
| [Cosmos DB için Azure Izleyici (Önizleme)](insights/cosmosdb-insights-overview.md) | Birleşik etkileşimli bir deneyime sahip tüm Azure Cosmos DB kaynaklarınızın genel performans, başarısızlık, kapasite ve işlemsel sistem durumunun bir görünümünü sağlar. |
| [Ağlar için Azure Izleyici (Önizleme)](insights/network-insights-overview.md) | Tüm ağ kaynağınız için sistem durumu ve ölçümlerinin kapsamlı bir görünümünü sağlar. Gelişmiş arama özelliği, yalnızca Web sitenizin adını arayarak Web sitenizi barındıran kaynağı tanımlama gibi senaryoları etkinleştirerek Kaynak bağımlılıklarını belirlemenize yardımcı olur. |
[Kaynak grupları için Azure Izleyici (Önizleme)](insights/resource-group-insights.md) |  Tek tek kaynaklarınızın karşılaştığı sorunları önceliklendirin ve tanılayın ve kaynak grubunun bir bütün olarak sistem durumu ve performansına göre bağlamı sunar. |
| [Depolama için Azure Izleyici (Önizleme)](insights/storage-insights-overview.md) | Azure depolama hizmetleri Performanslarınızın, kapasitesinin ve kullanılabilirliğinden oluşan Birleşik bir görünüm sunarak Azure depolama hesaplarınızın kapsamlı bir şekilde izlenmesini sağlar. |
| [VM'ler için Azure İzleyici (Önizleme)](insights/container-insights-overview.md) | Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümelerinizi ölçeklendirerek izler. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder. |

### <a name="core-solutions"></a>Temel çözümler

Çözümler, belirli bir uygulama veya hizmet için özelleştirilmiş günlük sorgularını ve görünümleri temel alır. Bunlar yalnızca günlükleri toplayıp analiz eder ve Öngörüler için zaman içinde kullanım dışıdır.

| Çözüm | Açıklama |
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
|Active Directory | Hayır | Evet | [Evet](../active-directory/reports-monitoring/howto-use-azure-monitor-workbooks.md) |  |
|Active Directory B2C | Hayır | Hayır | Hayır |  |
|Active Directory Domain Services | Hayır | Evet | Hayır |  |
|Etkinlik günlüğü | Hayır | Evet | Hayır | |
|Gelişmiş Tehdit Koruması | Hayır | Hayır | Hayır |  |
|Advisor | Hayır | Hayır | Hayır |  |
|CnE UE A.com Mktg TM [RESX] | Hayır | Hayır | Hayır |  |
|Analysis Services | Evet | Evet | Hayır |  |
|FHıR için API | Hayır | Hayır | Hayır |  |
|API Management | Evet | Evet | Hayır |  |
|App Service | Evet | Evet | Hayır |  |
|AppConfig | Hayır | Hayır | Hayır |  |
|Application Gateway | Evet | Evet | Hayır |  |
|Kanıtlama hizmeti | Hayır | Hayır | Hayır |  |
|Automation | Evet | Evet | Hayır |  |
|Azure Service Manager (RDFE) | Hayır | Hayır | Hayır |  |
|Backup | Hayır | Evet | Hayır |  |
|Bastion | Hayır | Hayır | Hayır |  |
|Batch | Evet | Evet | Hayır |  |
|Batch AI | Hayır | Hayır | Hayır |  |
|Blok Zinciri Hizmeti | Hayır | Evet | Hayır |  |
|Şemalar | Hayır | Hayır | Hayır |  |
|Bot Hizmeti | Hayır | Hayır | Hayır |  |
|Bulut Hizmetleri | Evet | Evet | Hayır | Konuk işletim sistemini ve iş akışlarını izlemek için aracı gereklidir.  |
|Cloud Shell | Hayır | Hayır | Hayır |  |
|Cognitive Services | Evet | Evet | Hayır |  |
|Container Instances | Evet | Hayır | Hayır |  |
|Container Registry | Evet | Evet | Hayır |  |
|Content Delivery Network (CDN) | Hayır | Evet | Hayır |  |
|Cosmos DB | Evet | Evet | [Evet](insights/cosmosdb-insights-overview.md) |  |
|Cost Management (Maliyet Yönetimi) | Hayır | Hayır | Hayır |  |
|Data Box | Hayır | Hayır | Hayır |  |
|Veri Kataloğu Gen2 | Hayır | Hayır | Hayır |  |
|Veri Gezgini | Evet | Evet | Hayır |  |
|Data Factory | Evet | Evet | Hayır |  |
|Data Factory v2 | Hayır | Evet | Hayır |  |
|Veri paylaşma | Hayır | Hayır | Hayır |  |
|MariaDB için veritabanı | Evet | Evet | Hayır |  |
|MySQL için veritabanı | Evet | Evet | Hayır |  |
|PostgreSQL için veritabanı | Evet | Evet | Hayır |  |
|Veritabanı Geçiş Hizmeti | Hayır | Hayır | Hayır |  |
|Databricks | Hayır | Evet | Hayır |  |
|DDoS Koruması | Evet | Evet | Hayır |  |
|DevOps | Hayır | Hayır | Hayır |  |
|DNS | Evet | Hayır | Hayır |  |
|Etki alanı adları | Hayır | Hayır | Hayır |  |
|DPS | Hayır | Hayır | Hayır |  |
|Dynamics 365 müşteri katılımı | Hayır | Hayır | Hayır |  |
|Dynamics 365 finans ve Işlemler | Hayır | Hayır | Hayır |  |
|Event Grid | Evet | Hayır | Hayır |  |
|Event Hubs | Evet | Evet | Hayır |  |
|ExpressRoute | Evet | Evet | Hayır |  |
|Güvenlik Duvarı | Evet | Evet | Hayır |  |
|Front Door | Evet | Evet | Hayır |  |
|İşlevler | Evet | Evet | Hayır |  |
|HDInsight | Hayır | Evet | Hayır |  |
|HPC önbelleği | Hayır | Hayır | Hayır |  |
|Bilgi Koruma | Hayır | Evet | Hayır |  |
|Intune | Hayır | Evet | Hayır |  |
|IoT Central | Hayır | Hayır | Hayır |  |
|IoT Hub | Evet | Evet | Hayır |  |
|Key Vault | Evet | Evet | Hayır |  |
|Kubernetes Hizmeti (AKS) | Hayır | Hayır | [Evet](insights/container-insights-overview.md)  |  |
|Load Balancer | Evet | Evet | Hayır |  |
|Logic Apps | Evet | Evet | Hayır |  |
|Machine Learning Hizmeti | Hayır | Hayır | Hayır |  |
|Yönetilen Uygulamalar  | Hayır | Hayır | Hayır |  |
|Haritalar  | Hayır | Hayır | Hayır |  |
|Media Services | Evet | Evet | Hayır |  |
|Microsoft Flow | Hayır | Hayır | Hayır |  |
|Microsoft tarafından yönetilen Masaüstü | Hayır | Hayır | Hayır |  |
|Microsoft PowerApps | Hayır | Hayır | Hayır |  |
|Microsoft sosyal katılım | Hayır | Hayır | Hayır |  |
|Microsoft Stream | Evet | Evet | Hayır |  |
|Geçiş yapın | Hayır | Hayır | Hayır |  |
|Multi-Factor Authentication | Hayır | Evet | Hayır |  |
|Ağ İzleyicisi | Evet | Evet | Hayır |  |
|Notification Hubs | Evet | Hayır | Hayır |  |
|Açık Veri Kümeleri | Hayır | Hayır | Hayır |  |
|İlke | Hayır | Hayır | Hayır |  |
|Power BI | Evet | Evet | Hayır |  |
|Power BI Embedded | Hayır | Hayır | Hayır |  |
|Özel Bağlantı | Hayır | Hayır | Hayır |  |
|Proje biriktirme Iletişim platformu | Hayır | Hayır | Hayır |  |
|Red Hat OpenShift | Hayır | Hayır | Hayır |  |
|Redis Önbelleği | Evet | Evet | Hayır |  |
|Kaynak Grafiği | Hayır | Hayır | Hayır |  |
|Kaynak Yöneticisi | Hayır | Hayır | Hayır |  |
|Perakende arama-Bing tarafından | Hayır | Hayır | Hayır |  |
|Arama | Evet | Evet | Hayır |  |
|Service Bus | Evet | Evet | Hayır |  |
|Service Fabric | Hayır | Evet | Hayır | Konuk işletim sistemini ve iş akışlarını izlemek için aracı gereklidir.  |
|Kaydolma portalı | Hayır | Hayır | Hayır |  |
|Site Kurtarma | Hayır | Evet | Hayır |  |
|Yay bulut hizmeti | Hayır | Hayır | Hayır |  |
|SQL Veri Ambarı | Evet | Evet | Hayır |  |
|SQL Veritabanı | Evet | Evet | Hayır |  |
|SQL Server Stretch Database | Evet | Evet | Hayır |  |
|Yığın | Hayır | Hayır | Hayır |  |
|Depolama | Evet | Hayır | [Evet](insights/storage-insights-overview.md) |  |
|Depolama önbelleği | Hayır | Hayır | Hayır |  |
|Depolama Eşitleme Hizmetleri | Hayır | Hayır | Hayır |  |
|Stream Analytics | Evet | Evet | Hayır |  |
|Zaman Serisi İçgörüleri | Evet | Evet | Hayır |  |
|TINA | Hayır | Hayır | Hayır |  |
|Traffic Manager | Evet | Evet | Hayır |  |
|Evrensel Yazdırma | Hayır | Hayır | Hayır |  |
|Sanal Makine Ölçek Kümeleri | Hayır | Evet | [Evet](insights/vminsights-overview.md) | Konuk işletim sistemini ve iş akışlarını izlemek için aracı gereklidir. |
|Virtual Machines (Sanal Makineler) | Evet | Evet | [Evet](insights/vminsights-overview.md) | Konuk işletim sistemini ve iş akışlarını izlemek için aracı gereklidir. |
|Sanal Ağ | Evet | Evet | [Evet](insights/network-insights-overview.md) |  |
|Sanal ağ-NSG akış günlükleri | Hayır | Evet | Hayır |  |
|VPN Gateway | Evet | Evet | Hayır |  |
|Windows Sanal Masaüstü | Hayır | Hayır | Hayır |  |


## <a name="product-integrations"></a>Ürün tümleştirmeleri
Aşağıdaki tabloda yer alan hizmetler ve çözümler, Azure Izleyici tarafından toplanan diğer günlük verileriyle çözümlenebilmesi için verilerini bir Log Analytics çalışma alanında depolar.

| Ürün/hizmet | Açıklama |
|:---|:---|
| [Azure Otomasyonu](/azure/automation/) | Windows ve Linux bilgisayarlarda işletim sistemi güncelleştirmelerini yönetin ve değişiklikleri izleyin. Bkz. [değişiklik izleme](../automation/change-tracking.md) ve [güncelleştirme yönetimi](../automation/automation-update-management.md). |
| [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/) | Belge ve e-postaları sınıflandırma ve isteğe bağlı olarak koruma. Bkz. [Azure Information Protection Için merkezi raporlama](https://docs.microsoft.com/azure/information-protection/reports-aip#configure-a-log-analytics-workspace-for-the-reports). |
| [Azure Güvenlik Merkezi](/azure/security-center/) | Güvenlik olaylarını toplayın ve çözümleyin ve tehdit analizi gerçekleştirin. Bkz. [Azure Güvenlik Merkezi 'Nde veri toplamayı](/azure/security-center/security-center-enable-data-collection) |
| [Azure Sentinel](/azure/sentinel/) | Office 365 ve Amazon Web Services bulut Izi gibi farklı kaynaklara bağlanır. Bkz. [veri kaynaklarını bağlama](/azure/sentinel/connect-data-sources). |
| [Anahtar Kasası Analizi](insights/azure-key-vault.md) | Azure Key Vault AuditEvent günlüklerini çözümleyin. |
| [Microsoft Intune](https://docs.microsoft.com/intune/) | Günlükleri Azure Izleyici 'ye göndermek için bir tanılama ayarı oluşturun. Bkz. [Intune 'da günlük verilerini depolama, Olay Hub 'ları veya Log Analytics 'e gönderme (Önizleme)](https://docs.microsoft.com/intune/fundamentals/review-logs-using-azure-monitor).  |
| Ağ  | [Ağ performansı İzleyicisi](insights/network-performance-monitor.md) -hizmet ve uygulama uç noktalarına ağ bağlantısını ve performansını izleyin.<br>[Azure Application Gateway](insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor) -Azure Application Gateway günlükleri ve ölçümleri analiz edin.<br>[Trafik Analizi](/azure/network-watcher/traffic-analytics) -Azure bulutunuzda trafik akışına ilişkin Öngörüler sağlamak Için Ağ İzleyicisi ağ güvenlik grubu (NSG) akış günlüklerini analiz eder. |
| [Office 365](insights/solution-office-365.md) | Office 365 ortamınızı izleyin. Azure Sentinel ile sunulan geliştirilmiş ekleme ile güncelleştirilmiş sürüm. |
| [SQL Analytics](insights/azure-sql.md) | Azure SQL veritabanlarının, elastik havuzların ve yönetilen örneklerin performansını ölçeklendirerek ve birden çok aboneliğe göre izleyin. |
| [Surface Hub](insights/surface-hubs.md) | Surface Hub cihazların sistem durumunu ve kullanımını izleyin. |
| [System Center Operations Manager](https://docs.microsoft.com/system-center/scom) | Yönetim gruplarını Azure Izleyici 'ye bağlayarak Operations Manager aracılardan veri toplayın. Bkz. [Azure izleyici 'ye bağlanma Operations Manager](platform/om-agents.md)<br> [Operations Manager değerlendirmesi](insights/scom-assessment.md) çözümüyle System Center Operations Manager yönetim grubunuzun riskini ve sistem durumunu değerlendirin. |
| [Microsoft ekipleri Odalar](https://docs.microsoft.com/microsoftteams/room-systems/azure-monitor-deploy) | Microsoft ekipleri Oda cihazlarının tümleşik, uçtan uca yönetimi. |
| [Visual Studio App Center](https://docs.microsoft.com/appcenter/) | Uygulamalar oluşturun, test edin ve dağıtın ve ardından durumlarını ve kullanımını izleyin. Bkz. [App Center ve Application Insights mobil uygulamanızı çözümlemeyi başlatma](learn/mobile-center-quickstart.md). |
| Windows | [Uyumluluk Windows Update](https://docs.microsoft.com/windows/deployment/update/update-compliance-get-started) -Windows Masaüstü yükseltmelerinizi değerlendirin.<br>[Masaüstü Analizi](https://docs.microsoft.com/configmgr/desktop-analytics/overview) -Windows istemcilerinizin güncelleştirme hazırlığı hakkında daha bilinçli kararlar almak için öngörüleri ve zekası sağlamak üzere Configuration Manager ile tümleşir. |



## <a name="other-solutions"></a>Diğer çözümler
Farklı uygulama ve hizmetleri izlemek için diğer çözümler mevcuttur, ancak etkin geliştirme durdurulur ve tüm bölgelerde kullanılamayabilir. Bunlar Azure Log Analytics veri alma hizmeti düzeyi sözleşmesi kapsamında ele alınmıştır.

| Çözüm | Açıklama |
|:---|:---|
| [Active Directory değerlendirmesi](insights/ad-assessment.md) | Active Directory ortamlarınızın riskini ve sistem durumunu değerlendirin. |
| [Active Directory çoğaltma durumu](insights/ad-replication-status.md) | Active Directory ortamınızı tüm çoğaltma hatalarıyla düzenli olarak izler. |
| [Etkinlik günlüğü Analizi](platform/activity-log-view.md#activity-logs-analytics-monitoring-solution) | Önceden tanımlanmış günlük sorgularını ve görünümlerini kullanarak etkinlik günlüğü girdilerini çözümleyin. |
| [DNS Analizi (Önizleme)](insights/dns-analytics.md) | Windows DNS analitik ve denetim günlüklerini ve DNS sunucularınızdaki diğer ilgili verileri toplar, analiz eder ve ilişkilendirir. |
| [Cloud Foundry](../cloudfoundry/cloudfoundry-oms-nozzle.md) | Cloud Foundry sistem durumu ve performans ölçümlerinizi birden çok dağıtımda toplayın, görüntüleyin ve çözümleyin. |
| [Kapsayıcılar](insights/containers.md) | Docker ve Windows kapsayıcı ana bilgisayarlarını görüntüleyin ve yönetin. |
| [Isteğe bağlı değerlendirmeler](https://docs.microsoft.com/services-hub/health/getting_started_with_on_demand_assessments) | Şirket içi, karma ve bulut Microsoft teknoloji ortamlarınızın kullanılabilirliğini, güvenliğini ve performansını değerlendirin ve iyileştirin. |
| [SQL değerlendirmesi](insights/sql-assessment.md) | SQL Server ortamlarınızın riskini ve sistem durumunu değerlendirin.  |
| [Kablo Verileri](insights/wire-data.md) | Windows ile bağlı ve Linux bağlantılı bilgisayarlardan Log Analytics aracısıyla toplanan birleştirilmiş ağ ve performans verileri. |


## <a name="third-party-integration"></a>Üçüncü taraf tümleştirmesi

| Çözüm | Açıklama |
|:---|:---|
| [ITSM](platform/itsmc-overview.md) | BT Hizmet Yönetimi Bağlayıcısı (ıSMC), Azure ve desteklenen bir BT hizmet yönetimi (ıTSM) ürünü/hizmeti 'ne bağlanmanızı sağlar.  |


## <a name="resources-outside-of-azure"></a>Azure dışındaki kaynaklar
Azure Izleyici, aşağıdaki tabloda listelenen yöntemleri kullanarak Azure dışındaki kaynaklardan veri toplayabilir.

| Kaynak | Yöntem |
|:---|:---|
| Uygulamalar | Application Insights kullanarak Azure dışındaki Web uygulamalarını izleyin. Bkz. [Application Insights nedir?](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). |
| Sanal makineler | Diğer bulut ortamlarında veya Şirket içindeki sanal makinelerin Konuk işletim sisteminden veri toplamak için Log Analytics aracısını kullanın. Bkz. [Log Analytics aracısıyla günlük verilerini toplama](platform/log-analytics-agent.md). |
| REST API Istemcisi | Her bir REST API istemcisinden Azure Izleyici günlüklerine ve ölçümlerine veri yazmak için ayrı API 'Ler kullanılabilir. Bkz. Azure izleyici 'de Günlükler için [http veri toplayıcı API 'si ile günlük verileri gönderme](platform/data-collector-api.md) ve bir [Azure kaynağı için azure izleyici ölçüm REST API deposuna özel ölçümler gönderme](platform/metrics-store-custom-rest-api.md) . |



## <a name="next-steps"></a>Sonraki adımlar

- [Öngörüler ve çözümler tarafından toplanan günlükleri ve ölçümleri depolayan Azure izleyici veri platformu](platform/data-platform.md)hakkında daha fazla bilgi edinin.
- Bir [Azure kaynağını izlemeye yönelik bir öğreticiyi](learn/tutorial-resource-logs.md)doldurun.
- [Azure Izleyici günlüklerinde verileri çözümlemek için günlük sorgusu yazma öğreticisini](learn/tutorial-resource-logs.md)doldurun.
- [Azure Izleyici ölçümlerinde verileri çözümlemek için ölçüm grafiği oluşturma öğreticisini](learn/tutorial-metrics-explorer.md)doldurun.

 
