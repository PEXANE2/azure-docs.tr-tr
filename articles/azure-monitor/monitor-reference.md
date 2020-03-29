---
title: Azure Monitor tarafından neler izlenir?
description: Azure Monitor tarafından izlenen tüm hizmetlerin ve diğer kaynakların başvurusu.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/17/2020
ms.openlocfilehash: 3cd330e9c4ceba2feeb7a74cafe9f094fd03d690
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249171"
---
# <a name="what-is-monitored-by-azure-monitor"></a>Azure Monitor tarafından neler izlenir?
Bu makalede, Azure Monitor tarafından izlenen farklı uygulamalar ve hizmetler açıklanmaktadır. 

## <a name="insights-and-core-solutions"></a>Öngörüler ve temel çözümler
Temel öngörüler ve çözümler Azure Monitor'un bir parçası olarak kabul edilir ve Azure için destek ve hizmet düzeyi anlaşmalarını izler. Azure Monitor'un kullanılabildiği tüm Azure bölgelerinde desteklenirler.

### <a name="insights"></a>Insights

Öngörüler, belirli uygulamalar ve hizmetler için özelleştirilmiş bir izleme deneyimi sağlar. Hem günlükleri hem de ölçümleri toplar ve analiz ederler.

| İçgörü | Açıklama |
|:---|:---|
| [Uygulama Bilgileri](app/app-insights-overview.md) | Herhangi bir platformda canlı web uygulama izlemek için Genişletilebilir Uygulama Performans Yönetimi (APM) hizmeti. |
| [Kapsayıcılar için Azure Monitörü](insights/container-insights-overview.md) | Azure Kapsayıcı Örnekleri'ne veya Azure Kubernetes Hizmeti'nde (AKS) barındırılan yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinin performansını izler. |
| [Cosmos DB için Azure Monitör (önizleme)](insights/cosmosdb-insights-overview.md) | Tüm Azure Cosmos DB kaynaklarınızın genel performansını, hatalarını, kapasitesini ve operasyonel durumunu birleşik etkileşimli bir deneyimle görüntüleyin. |
| [Ağlar için Azure Monitörü (önizleme)](insights/network-insights-overview.md) | Tüm ağ kaynağınız için sistem durumu ve ölçümlerin kapsamlı bir görünümünü sağlar. Gelişmiş arama özelliği, yalnızca web sitenizin adını arayarak web sitenizi barındıran kaynağı tanımlama gibi senaryoları etkinleştirerek kaynak bağımlılıklarını belirlemenize yardımcı olur. |
[Kaynak Grupları için Azure Monitörü (önizleme)](insights/resource-group-insights.md) |  Tek tek kaynaklarınızın karşılaştığı sorunları üçleme ve tanılama, bir bütün olarak kaynak grubunun sağlık ve performansı yla ilgili bağlam sunar. |
| [Depolama için Azure Monitörü (önizleme)](insights/storage-insights-overview.md) | Azure Depolama hizmetleri performansınızın, kapasitenizin ve kullanılabilirliğinin birleşik bir görünümünü sunarak Azure Depolama hesaplarınızın kapsamlı bir şekilde izlenmesini sağlar. |
| [VM'ler için Azure Monitör (önizleme)](insights/container-insights-overview.md) | Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümelerinizi ölçekte izler. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder. |

### <a name="core-solutions"></a>Çekirdek çözümleri

Çözümler, belirli bir uygulama veya hizmet için özelleştirilmiş günlük sorgularını ve görünümlerini temel adatır. Sadece günlükleri toplar ve analiz ederler ve zaman içinde içgörüler lehine amortismana uğruyor.

| Çözüm | Açıklama |
|:---|:---|
| [Aracı durumu](insights/solution-agenthealth.md) | Log Analytics aracılarının sistem durumunu ve yapılandırmasını analiz edin. |
| [Uyarı yönetimi](platform/alert-management-solution.md) | System Center Operations Manager, Nagios veya Zabbix'ten toplanan uyarıları analiz edin. |
| [Hizmet Eşlemesi](insights/service-map.md) | Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak keşfeder ve hizmetler arasındaki iletişimi eşler. |



## <a name="azure-services"></a>Azure hizmetleri
Aşağıdaki tabloda Azure hizmetleri ve topladıkları veriler Azure Monitor'da listelenmektedir. 

- Ölçümler - Hizmet, ölçümleri otomatik olarak Azure Monitör Ölçümleri'nde toplar. 
- Günlükler - Hizmet, Azure Monitör Günlükleri için platform günlükleri ve ölçümleri toplayabilen tanılama ayarlarını destekler.
- Insight - Hizmet için özelleştirilmiş bir izleme deneyimi sağlayan hizmet için bir içgörü vardır.

| Hizmet | Ölçümler | Günlükler | İçgörü | Notlar |
|:---|:---|:---|:---|:---|
|Active Directory | Hayır | Evet | [Evet](../active-directory/reports-monitoring/howto-use-azure-monitor-workbooks.md) |  |
|Active Directory B2C | Hayır | Hayır | Hayır |  |
|Active Directory Domain Services | Hayır | Evet | Hayır |  |
|Etkinlik günlüğü | Hayır | Evet | Hayır | |
|Gelişmiş Tehdit Koruması | Hayır | Hayır | Hayır |  |
|Advisor | Hayır | Hayır | Hayır |  |
|Yapay Zeka Oluşturucusu | Hayır | Hayır | Hayır |  |
|Analysis Services | Evet | Evet | Hayır |  |
|FHIR için API | Hayır | Hayır | Hayır |  |
|API Management | Evet | Evet | Hayır |  |
|App Service | Evet | Evet | Hayır |  |
|AppConfig | Hayır | Hayır | Hayır |  |
|Application Gateway | Evet | Evet | Hayır |  |
|Attestation Servisi | Hayır | Hayır | Hayır |  |
|Automation | Evet | Evet | Hayır |  |
|Azure Servis Yöneticisi (RDFE) | Hayır | Hayır | Hayır |  |
|Backup | Hayır | Evet | Hayır |  |
|Bastion | Hayır | Hayır | Hayır |  |
|Batch | Evet | Evet | Hayır |  |
|Batch AI | Hayır | Hayır | Hayır |  |
|Blok Zinciri Hizmeti | Hayır | Evet | Hayır |  |
|Şemalar | Hayır | Hayır | Hayır |  |
|Bot Hizmeti | Hayır | Hayır | Hayır |  |
|Cloud Services | Evet | Evet | Hayır | Konuk işletim sistemini ve iş akışlarını izlemek için gereken aracı.  |
|Cloud Shell | Hayır | Hayır | Hayır |  |
|Bilişsel hizmetler | Evet | Evet | Hayır |  |
|Container Instances | Evet | Hayır | Hayır |  |
|Container Kayıt Defteri | Evet | Evet | Hayır |  |
|İçerik Dağıtım Ağı (CDN) | Hayır | Evet | Hayır |  |
|Cosmos DB | Evet | Evet | [Evet](insights/cosmosdb-insights-overview.md) |  |
|Maliyet Yönetimi | Hayır | Hayır | Hayır |  |
|Data Box | Hayır | Hayır | Hayır |  |
|Veri Kataloğu Gen2 | Hayır | Hayır | Hayır |  |
|Veri Gezgini | Evet | Evet | Hayır |  |
|Data Factory | Evet | Evet | Hayır |  |
|Veri Fabrikası v2 | Hayır | Evet | Hayır |  |
|Veri Paylaşımı | Hayır | Hayır | Hayır |  |
|MariaDB için Veritabanı | Evet | Evet | Hayır |  |
|MySQL için Veritabanı | Evet | Evet | Hayır |  |
|PostgreSQL için Veritabanı | Evet | Evet | Hayır |  |
|Veritabanı Geçiş Hizmeti | Hayır | Hayır | Hayır |  |
|Databricks | Hayır | Evet | Hayır |  |
|DDOS Koruması | Evet | Evet | Hayır |  |
|DevOps | Hayır | Hayır | Hayır |  |
|DNS | Evet | Hayır | Hayır |  |
|Etki alanı adları | Hayır | Hayır | Hayır |  |
|Dps | Hayır | Hayır | Hayır |  |
|Dynamics 365 Müşteri Katılımı | Hayır | Hayır | Hayır |  |
|Dynamics 365 Finans ve Operasyon | Hayır | Hayır | Hayır |  |
|Event Grid | Evet | Hayır | Hayır |  |
|Event Hubs | Evet | Evet | Hayır |  |
|ExpressRoute | Evet | Evet | Hayır |  |
|Güvenlik duvarı | Evet | Evet | Hayır |  |
|Front Door | Evet | Evet | Hayır |  |
|İşlevler | Evet | Evet | Hayır |  |
|HDInsight | Hayır | Evet | Hayır |  |
|HPC Önbelleği | Hayır | Hayır | Hayır |  |
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
|Microsoft Yönetilen Masaüstü | Hayır | Hayır | Hayır |  |
|Microsoft PowerApps | Hayır | Hayır | Hayır |  |
|Microsoft Social Engagement | Hayır | Hayır | Hayır |  |
|Microsoft Stream | Evet | Evet | Hayır |  |
|Geçiş | Hayır | Hayır | Hayır |  |
|Multi-Factor Authentication | Hayır | Evet | Hayır |  |
|Ağ İzleyicisi | Evet | Evet | Hayır |  |
|Notification Hubs | Evet | Hayır | Hayır |  |
|Açık Veri Kümeleri | Hayır | Hayır | Hayır |  |
|İlke | Hayır | Hayır | Hayır |  |
|Power BI | Evet | Evet | Hayır |  |
|Power BI Embedded | Hayır | Hayır | Hayır |  |
|Özel Bağlantı | Hayır | Hayır | Hayır |  |
|Proje Spool İletişim Platformu | Hayır | Hayır | Hayır |  |
|Red Hat OpenShift | Hayır | Hayır | Hayır |  |
|Redis Cache | Evet | Evet | Hayır |  |
|Kaynak Grafiği | Hayır | Hayır | Hayır |  |
|Resource Manager | Hayır | Hayır | Hayır |  |
|Perakende Arama - Bing tarafından | Hayır | Hayır | Hayır |  |
|Search | Evet | Evet | Hayır |  |
|Service Bus | Evet | Evet | Hayır |  |
|Service Fabric | Hayır | Evet | Hayır | Konuk işletim sistemini ve iş akışlarını izlemek için gereken aracı.  |
|Kayıt Portalı | Hayır | Hayır | Hayır |  |
|Site Recovery | Hayır | Evet | Hayır |  |
|Bahar Bulut Hizmeti | Hayır | Hayır | Hayır |  |
|SQL Veri Ambarı | Evet | Evet | Hayır |  |
|SQL Database | Evet | Evet | Hayır |  |
|SQL Server Stretch Database | Evet | Evet | Hayır |  |
|Yığın | Hayır | Hayır | Hayır |  |
|Depolama | Evet | Hayır | [Evet](insights/storage-insights-overview.md) |  |
|Depolama Önbelleği | Hayır | Hayır | Hayır |  |
|Depolama Eşitleme Hizmetleri | Hayır | Hayır | Hayır |  |
|Stream Analytics | Evet | Evet | Hayır |  |
|Time Series Insights | Evet | Evet | Hayır |  |
|TINA | Hayır | Hayır | Hayır |  |
|Traffic Manager | Evet | Evet | Hayır |  |
|Evrensel Baskı | Hayır | Hayır | Hayır |  |
|Sanal Makine Ölçek Kümeleri | Hayır | Evet | [Evet](insights/vminsights-overview.md) | Konuk işletim sistemini ve iş akışlarını izlemek için gereken aracı. |
|Virtual Machines | Evet | Evet | [Evet](insights/vminsights-overview.md) | Konuk işletim sistemini ve iş akışlarını izlemek için gereken aracı. |
|Sanal Ağ | Evet | Evet | [Evet](insights/network-insights-overview.md) |  |
|Sanal Ağ - NSG Akış Günlükleri | Hayır | Evet | Hayır |  |
|VPN Gateway | Evet | Evet | Hayır |  |
|Windows Sanal Masaüstü | Hayır | Hayır | Hayır |  |


## <a name="product-integrations"></a>Ürün entegrasyonları
Aşağıdaki tablodaki hizmetler ve çözümler, Azure Monitor tarafından toplanan diğer günlük verileriyle analiz edilebilmek için verilerini Log Analytics çalışma alanında saklar.

| Ürün / Hizmet | Açıklama |
|:---|:---|
| [Azure Otomasyonu](/azure/automation/) | İşletim sistemi güncelleştirmelerini yönetin ve Windows ve Linux bilgisayarlardaki değişiklikleri izleyin. Bkz. [Değişiklik İzleme](../automation/change-tracking.md) ve [Güncelleme Yönetimi.](../automation/automation-update-management.md) |
| [Azure Bilgi Koruması](https://docs.microsoft.com/azure/information-protection/) | Belgeleri ve e-postaları sınıflandırın ve isteğe bağlı olarak koruyun. [Azure Bilgi Koruması için Merkezi raporlamaya](https://docs.microsoft.com/azure/information-protection/reports-aip#configure-a-log-analytics-workspace-for-the-reports)bakın. |
| [Azure Güvenlik Merkezi](/azure/security-center/) | Güvenlik olaylarını toplayın ve analiz edin ve tehdit analizi yapın. [Azure Güvenlik Merkezi'nde Veri toplama](/azure/security-center/security-center-enable-data-collection) |
| [Azure Sentinel](/azure/sentinel/) | Office 365 ve Amazon Web Services Cloud Trail gibi farklı kaynaklara bağlanır. Bkz. [Veri kaynaklarını bağla.](/azure/sentinel/connect-data-sources) |
| [Anahtar Kasası Analizi](insights/azure-key-vault.md) | Azure Key Vault AuditEvent günlüklerini analiz edin. |
| [Microsoft Intune](https://docs.microsoft.com/intune/) | Günlükleri Azure Monitor'a göndermek için tanılama ayarı oluşturun. Bkz. [Günlük verilerini Intune'da depolamaya, olay merkezlerine veya günlük analitiğine gönder (önizleme)](https://docs.microsoft.com/intune/fundamentals/review-logs-using-azure-monitor).  |
| Ağ  | [Ağ Performans Monitörü](insights/network-performance-monitor.md) - Ağ bağlantısını ve performansını hizmet ve uygulama uç noktalarına kadar izleyin.<br>[Azure Uygulama Ağ Geçidi](insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor) - Azure Application Gateway'deki günlükleri ve ölçümleri analiz edin.<br>[Trafik Analizi](/azure/network-watcher/traffic-analytics) - Azure bulutunuzdaki trafik akışı hakkında öngörüler sağlamak için Ağ İzleyiciağ güvenlik grubunu (NSG) akış günlüğelerini analiz eder. |
| [Ofis 365](insights/solution-office-365.md) | Office 365 ortamınızı izleyin. Azure Sentinel aracılığıyla geliştirilmiş onboarding sürümü ile güncelleştirilmiş sürüm. |
| [SQL Analytics](insights/azure-sql.md) | Azure SQL veritabanlarının, esnek havuzların ve yönetilen örneklerin performansını ölçekte ve birden çok abonelikte izleyin. |
| [Surface Hub](insights/surface-hubs.md) | Surface Hub aygıtlarının sistem durumunu ve kullanımını izleyin. |
| [System Center Operations Manager](https://docs.microsoft.com/system-center/scom) | Yönetim gruplarını Azure Monitor'a bağlayarak Operasyon Yöneticisi aracılarından veri toplayın. [Bkz. İşlem Yöneticisini Azure Monitörüne Bağla](platform/om-agents.md)<br> [Operasyon Yöneticisi Değerlendirmesi](insights/scom-assessment.md) çözümü ile Sistem Merkezi Operasyon Yöneticisi yönetim grubunuzun risk ve sağlığını değerlendirin. |
| [Microsoft Teams Odaları](https://docs.microsoft.com/microsoftteams/room-systems/azure-monitor-deploy) | Microsoft Teams Rooms aygıtlarının tümleşik, uçtan uca yönetimi. |
| [Visual Studio App Center](https://docs.microsoft.com/appcenter/) | Uygulamaları oluşturun, sınayın ve dağıtın ve ardından durumlarını ve kullanımlarını izleyin. Bkz. [Uygulama Merkezi ve Uygulama Öngörüleri ile mobil uygulamanızı analiz etmeye başlayın.](learn/mobile-center-quickstart.md) |
| Windows | [Windows Update Uyumluluğu](https://docs.microsoft.com/windows/deployment/update/update-compliance-get-started) - Windows masaüstü yükseltmelerinizi değerlendirin.<br>[Masaüstü Analitiği](https://docs.microsoft.com/configmgr/desktop-analytics/overview) - Windows istemcilerinizin güncellemeye hazır olup olma durumu hakkında daha bilinçli kararlar almak için fikir ve istihbarat sağlamak için Configuration Manager ile tümleştirir. |



## <a name="other-solutions"></a>Diğer çözümler
Farklı uygulama ve hizmetlerin izlenmesi için başka çözümler mevcuttur, ancak etkin geliştirme durduruldu ve tüm bölgelerde kullanılamayabilir. Azure Log Analytics veri alma hizmeti düzeyi sözleşmesi kapsamındadır.

| Çözüm | Açıklama |
|:---|:---|
| [Aktif Dizin sağlık kontrolü](insights/ad-assessment.md) | Aktif Dizin ortamlarınızın risk ve sağlığını değerlendirin. |
| [Etkin Dizin çoğaltma durumu](insights/ad-replication-status.md) | Etkin Dizin ortamınızı çoğaltma hataları için düzenli olarak izler. |
| [Etkinlik günlüğü analitiği](platform/activity-log-view.md#activity-logs-analytics-monitoring-solution) | Önceden tanımlanmış günlük sorgularını ve görünümlerini kullanarak Etkinlik günlüğü girişlerini analiz edin. |
| [DNS Analytics (önizleme)](insights/dns-analytics.md) | DNS sunucularınızdan Windows DNS analitik ve denetim günlüklerini ve diğer ilgili verileri toplar, analiz eder ve ilişkilendirir. |
| [Cloud Foundry](../cloudfoundry/cloudfoundry-oms-nozzle.md) | Birden fazla dağıtımda Bulut Döküm sistemi sistem sistem sistem ve performans ölçümlerinizi toplayın, görüntüleyin ve analiz edin. |
| [Kapsayıcılar](insights/containers.md) | Docker ve Windows kapsayıcı ana bilgisayarlarını görüntüleyin ve yönetin. |
| [İsteğe Bağlı Değerlendirmeler](https://docs.microsoft.com/services-hub/health/getting_started_with_on_demand_assessments) | Şirket içi, karma ve bulut Microsoft teknoloji ortamlarınızın kullanılabilirliğini, güvenliğini ve performansını değerlendirin ve optimize edin. |
| [SQL sistem durumu denetimi](insights/sql-assessment.md) | SQL Server ortamlarınızın risk ve sağlık durumunu değerlendirin.  |
| [Kablo Verileri](insights/wire-data.md) | Log Analytics aracısı ile Windows'a bağlı ve Linux'a bağlı bilgisayarlardan toplanan konsolide ağ ve performans verileri. |


## <a name="third-party-integration"></a>Üçüncü taraf entegrasyonu

| Çözüm | Açıklama |
|:---|:---|
| [ITSM](platform/itsmc-overview.md) | BT Hizmet Yönetimi Bağlayıcısı (ITSMC), Azure'u ve desteklenen bir BT Hizmet Yönetimi (ITSM) ürün/hizmetini bağlamanızı sağlar.  |


## <a name="resources-outside-of-azure"></a>Azure dışındaki kaynaklar
Azure Monitor, aşağıdaki tabloda listelenen yöntemleri kullanarak Azure dışındaki kaynaklardan veri toplayabilir.

| Kaynak | Yöntem |
|:---|:---|
| Uygulamalar | Uygulama Öngörüleri'ni kullanarak Azure dışındaki web uygulamalarını izleyin. Bkz. [Uygulama Öngörüleri Nedir?](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). |
| Sanal makineler | Diğer bulut ortamlarında veya şirket içinde sanal makinelerin konuk işletim sisteminden veri toplamak için Log Analytics aracısını kullanın. Bkz. [Log Analytics aracısıyla günlük verilerini topla.](platform/log-analytics-agent.md) |
| REST API İstemci | Azure Monitör Günlükleri ve Ölçümleri'ne herhangi bir REST API istemcisinden veri yazmak için ayrı API'ler kullanılabilir. Günlük verilerini Günlükler için [HTTP Veri Toplayıcı API'si ile Azure Monitor'a gönderin](platform/data-collector-api.md) ve Bir Azure kaynağı için özel ölçümleri Ölçümler için REST [API kullanarak Azure Monitor metrik deposuna gönderin.](platform/metrics-store-custom-rest-api.md) |



## <a name="next-steps"></a>Sonraki adımlar

- Öngörüler ve çözümler tarafından [toplanan günlükleri ve ölçümleri depolayan Azure Monitor veri platformu](platform/data-platform.md)hakkında daha fazla bilgi edinin.
- Azure [kaynağını izleme yle ilgili bir öğreticiyi](learn/tutorial-resource-logs.md)tamamlayın.
- Azure [Monitör Günlükleri'ndeki verileri çözümlemek için günlük sorgusu yazma yla ilgili bir öğreticiyi tamamlayın.](learn/tutorial-resource-logs.md)
- Azure [Monitör Ölçümleri'ndeki verileri çözümlemek için bir ölçüm grafiği oluşturma yla ilgili bir öğreticiyi](learn/tutorial-metrics-explorer.md)tamamlayın.

 
