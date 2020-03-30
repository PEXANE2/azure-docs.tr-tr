---
title: Azure Monitör'deki veri kaynakları | Microsoft Dokümanlar
description: Azure kaynaklarınızın ve bunların üzerinde çalışan uygulamaların sistem durumunu ve performansını izlemek için kullanılabilir verileri açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/19/2019
ms.openlocfilehash: d1c0652844556b545cf0617032d21b80dd67d198
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79479850"
---
# <a name="sources-of-monitoring-data-for-azure-monitor"></a>Azure Monitor için izleme verisi kaynakları
Azure Monitor, [Günlükler](data-platform-logs.md) ve [Ölçümler](data-platform-metrics.md)içeren ortak bir [izleme veri platformuna](data-platform.md) dayanır. Bu platformda veri toplamak, birden çok kaynaktan gelen verilerin Azure Monitor'da ortak bir araç kümesi kullanılarak birlikte analiz edilmesine olanak tanır. İzleme verileri, belirli senaryoları desteklemek üzere başka konumlara da gönderilebilir ve bazı kaynaklar Günlükler veya Ölçümler'de toplanmadan önce diğer konumlara yazılabilir.

Bu makalede, Azure kaynakları tarafından oluşturulan izleme verilerine ek olarak Azure Monitor tarafından toplanan farklı izleme verileri kaynakları açıklanmaktadır. Bağlantılar, bu verileri farklı konumlara toplamak için gereken yapılandırma hakkında ayrıntılı bilgilere verilir.

## <a name="application-tiers"></a>Uygulama katmanları

Azure uygulamalarından gelen izleme verileri, uygulamanızın kendisi ve alt katmanlar Azure platformunun bileşenleri olmak üzere en yüksek katmanlar olmak üzere katmanlar halinde düzenlenebilir. Her katmandan verilere erişim yöntemi değişir. Uygulama katmanları aşağıdaki tabloda özetlenir ve her katmandaki izleme verisi kaynakları aşağıdaki bölümlerde sunulmuştur. Her veri konumunun açıklaması ve verilerine nasıl erişebileceğiniz için [Azure'daki veri konumlarını izleme](data-locations.md) bölümüne bakın.


![İzleme katmanları](../media/overview/overview.png)


### <a name="azure"></a>Azure
Aşağıdaki tabloda Azure'a özgü uygulama katmanları kısaca açıklanmaktadır. Aşağıdaki bölümlerde her biri hakkında daha fazla bilgi için bağlantıyı takip edin.

| Katman | Açıklama | Toplama yöntemi |
|:---|:---|:---|
| [Azure Kiracı](#azure-tenant) | Azure Active Directory gibi kiracı düzeyindeki Azure hizmetlerinin çalışmasıyla ilgili veriler. | Portaldaki AAD verilerini görüntüleyin veya kiracı tanılama ayarını kullanarak koleksiyonu Azure Monitor'a yapılandırın. |
| [Azure aboneliği](#azure-subscription) | Kaynak Yöneticisi ve Hizmet Durumu gibi Azure aboneliğinizdeki kaynaklar arası hizmetlerin sistem durumu ve yönetimiyle ilgili veriler. | Portalda görüntüleyin veya günlük profilini kullanarak koleksiyonu Azure Monitor'a yapılandırın. |
| [Azure kaynakları](#azure-resources) |  Her Azure kaynağının çalışması ve performansı yla ilgili veriler. | Otomatik olarak toplanan ölçümler, Metrics Explorer'da görüntülenebilir.<br>Azure Monitor'da günlükleri toplamak için tanılama ayarlarını yapılandırın.<br>Belirli kaynak türleri için daha ayrıntılı izleme için çözümlerin ve Öngörülerin izlenmesi. |

### <a name="azure-other-cloud-or-on-premises"></a>Azure, diğer bulut veya şirket içi 
Aşağıdaki tabloda, Azure'da, başka bir bulutta veya şirket içinde olabilecek uygulama katmanları kısaca açıklanmaktadır. Aşağıdaki bölümlerde her biri hakkında daha fazla bilgi için bağlantıyı takip edin.

| Katman | Açıklama | Toplama yöntemi |
|:---|:---|:---|
| [İşletim sistemi (misafir)](#operating-system-guest) | Bilgi işlem kaynakları üzerindeki işletim sistemi hakkındaki veriler. | Müşteri veri kaynaklarını Azure Monitor ve GEksenler için Azure Monitor'u destekleyen bağımlılıkları toplamak için Azure Monitor ve Bağımlılık aracısına toplamak için Log Analytics aracısını yükleyin.<br>Azure sanal makineleri için, günlükleri ve ölçümleri Azure Monitor'da toplamak için Azure Tanı Uzantısı'nı yükleyin. |
| [Uygulama Kodu](#application-code) | Performans izlemeleri, uygulama günlükleri ve kullanıcı telemetrisi de dahil olmak üzere gerçek uygulama ve kodun performansı ve işlevselliği hakkında veriler. | Uygulama Öngörüleri'ne veri toplamak için kodunuzu aygıt. |
| [Özel kaynaklar](#custom-sources) | Harici hizmetlerden veya diğer bileşenlerden veya aygıtlardan gelen veriler. | Herhangi bir REST istemcisinden Azure Monitor'da günlük veya metrik verileri toplayın. |

## <a name="azure-tenant"></a>Azure kiracısı
Azure kiracınızla ilgili telemetri, Azure Etkin Dizini gibi kiracı çapındaki hizmetlerden toplanır.

![Azure kiracı koleksiyonu](media/data-sources/tenant.png)

### <a name="azure-active-directory-audit-logs"></a>Azure Active Directory Denetim Günlükleri
[Azure Etkin Dizin raporlaması,](../../active-directory/reports-monitoring/overview-reports.md) oturum açma etkinliğinin geçmişini ve belirli bir kiracı içinde yapılan değişikliklerin denetim izini içerir. 

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici Günlükleri | Azure AD günlüklerini Azure Monitor'da toplanacak şekilde yapılandırarak diğer izleme verileriyle analiz edin. | [Azure AD günlüklerini Azure Monitor günlükleriyle tümleştirme (önizleme)](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure Storage | Arşivleme için Azure AD günlüklerini Azure Depolama'ya dışa aktarın. | [Öğretici: Azure AD günlüklerini bir Azure depolama hesabında arşivleme (önizleme)](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Olay Hub'ı | Azure AD günlüklerini Olay Hub'larını kullanarak diğer konumlara aktarın. | [Öğretici: Azure Etkin Dizin günlüğünü bir Azure etkinlik merkezine (önizleme) aktarın.](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |



## <a name="azure-subscription"></a>Azure aboneliği
Azure aboneliğinizin sağlığı ve işletimi ile ilgili telemetri.

![Azure aboneliği](media/data-sources/azure-subscription.png)

### <a name="azure-activity-log"></a>Azure Etkinliği günlüğü 
[Azure Etkinliği günlüğü,](platform-logs-overview.md) Azure aboneliğinizdeki kaynaklarda yapılan yapılandırma değişikliklerinin kayıtlarıyla birlikte hizmet durumu kayıtlarını içerir. Etkinlik günlüğü tüm Azure kaynakları tarafından kullanılabilir ve _dış_ görünümlerini temsil eder.

| Hedef | Açıklama | Başvuru |
|:---|:---|
| Etkinlik günlüğü | Etkinlik günlüğü, Azure Monitörü menüsünden görüntüleyebileceğiniz veya Etkinlik günlüğü uyarıları oluşturmak için kullanabileceğiniz kendi veri deposunda toplanır. | [Azure portalında Etkinlik günlüğünü sorgula](activity-log-view.md#azure-portal) |
| Azure İzleyici Günlükleri | Azure Monitör Günlüklerini, etkinlik günlüğünü toplamak ve diğer izleme verileriyle analiz etmek için yapılandırma. | [Azure Monitor'da Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve analiz](activity-log-collect.md) |
| Azure Storage | Arşivleme için Etkinlik günlüğünü Azure Depolama'ya dışa aktarın. | [Arşiv Etkinliği günlüğü](resource-logs-collect-storage.md)  |
| Event Hubs | Etkinlik hub'larını kullanarak Etkinlik günlüğünü diğer konumlara aktarın | [Etkinlik Günlüğü'ne Akış Etkinliği günlüğü.](resource-logs-stream-event-hubs.md) |

### <a name="azure-service-health"></a>Azure Hizmet Durumu
[Azure Hizmet Durumu,](../../service-health/service-health-overview.md) aboneliğinizde uygulamanızın ve kaynaklarınızın güvendiği Azure hizmetlerinin durumu hakkında bilgi sağlar.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Etkinlik günlüğü<br>Azure İzleyici Günlükleri | Hizmet Durumu kayıtları Azure Etkinliği günlüğünde depolanır, böylece azure portalında görüntüleyebilir veya Etkinlik günlüğüyle gerçekleştirebileceğiniz diğer etkinlikleri gerçekleştirebilirsiniz. | [Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme](service-notifications.md) |


## <a name="azure-resources"></a>Azure kaynakları
Ölçümler ve kaynak günlükleri, Azure kaynaklarının _dahili_ çalışması hakkında bilgi sağlar. Bunlar çoğu Azure hizmeti için kullanılabilir ve izleme çözümleri ve öngörüler belirli hizmetler için ek veri toplar.

![Azure kaynak koleksiyonu](media/data-sources/azure-resources.png)


### <a name="platform-metrics"></a>Platform ölçümleri 
Azure hizmetlerinin çoğu, performanslarını ve işlerini doğrudan ölçümler veritabanına yansıtan [platform ölçümleri](data-platform-metrics.md) gönderir. Belirli [ölçümler her kaynak türü için değişir.](metrics-supported.md) 

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure Monitör Ölçümleri | Platform ölçümleri, yapılandırma olmadan Azure Monitor ölçümleri veritabanına yazar. Metrics Explorer'dan platform ölçümlerine erişin.  | [Azure Ölçüm Gezgini'ni kullanmaya başlama](metrics-getting-started.md)<br>[Azure Monitor ile desteklenen ölçümler](metrics-supported.md) |
| Azure İzleyici Günlükleri | Log Analytics'i kullanarak eğilim ve diğer analizler için platform ölçümlerini Günlükler'e kopyalayın. | [Azure tanılama doğrudan Log Analytics'e](resource-logs-collect-workspace.md) |
| Event Hubs | Olay Hub'larını kullanarak ölçümleri diğer konumlara aktarın. |[Azure izleme verilerini harici bir araç tarafından tüketim için bir olay hub'ına aktarın](stream-monitoring-data-event-hubs.md) |

### <a name="resource-logs"></a>Kaynak günlükleri
[Kaynak günlükleri,](platform-logs-overview.md) bir Azure kaynağının _dahili_ çalışması hakkında öngörüler sağlar.  Kaynak günlükleri otomatik olarak oluşturulur, ancak her kaynak için toplanacak bir hedef belirtmek için bir tanılama ayarı oluşturmanız gerekir.

Kaynak günlüklerinin yapılandırma gereksinimleri ve içeriği kaynak türüne göre değişir ve tüm hizmetler bunları oluşturmaz. Her hizmetle ilgili ayrıntılar ve ayrıntılı yapılandırma yordamlarına bağlantılar [için Azure kaynak günlükleri için Desteklenen hizmetler, şemalar ve kategorilere](diagnostic-logs-schema.md) bakın. Hizmet bu makalede listelenmemişse, bu hizmet şu anda kaynak günlükleri oluşturmaz.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici Günlükleri | Toplanan diğer günlük verileriyle analiz için kaynak günlüklerini Azure Monitor Günlükleri'ne gönderin. | [Azure Monitor'da Günlük Analizi çalışma alanında Azure kaynak günlüklerini toplama](resource-logs-collect-storage.md) |
| Depolama | Arşivleme için kaynak günlüklerini Azure Depolama'ya gönderin. | [Azure kaynak günlüklerini arşivle](resource-logs-collect-workspace.md) |
| Event Hubs | Olay Hub'larını kullanarak kaynak günlüğünü diğer konumlara aktarın. |[Azure kaynak günlüğünü bir etkinlik merkezine aktarın](resource-logs-stream-event-hubs.md) |

## <a name="operating-system-guest"></a>İşletim sistemi (misafir)
Azure'daki, diğer bulutlardaki ve şirket içi hesaplama kaynaklarının izlenecek bir konuk işletim sistemi vardır. Bir veya daha fazla aracının yüklenmesiyle, azure hizmetleriyle aynı izleme araçlarıyla analiz etmek için konuktan Azure Monitor'a telemetri toplayabilirsiniz.

![Azure bilgi işlem kaynak koleksiyonu](media/data-sources/compute-resources.png)

### <a name="azure-diagnostic-extension"></a>Azure Tanılama uzantısı
Azure Sanal makineler için Azure Tanılama uzantısını etkinleştirmek, Azure Bulut Hizmeti (klasik) Web ve Çalışan Rolleri, Sanal Makineler, sanal makine dahil olmak üzere Azure bilgi işlem kaynaklarının konuk işletim sisteminden günlükleri ve ölçümleri toplamanızı sağlar ölçek setleri ve Servis Kumaşı.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Depolama | Azure tanılama uzantısı her zaman bir Azure Depolama hesabına yazar. | [Windows Azure tanılama uzantısını yükleme ve yapılandırma (WAD)](diagnostics-extension-windows-install.md)<br>[Ölçümleri ve günlükleri izlemek için Linux Tanılama Uzantısı’nı kullanma](../../virtual-machines/extensions/diagnostics-linux.md) |
| Azure Monitör Ölçümleri | Performans sayaçları toplamak için Tanılama Uzantısı'nı yapılandırdığınızda, bunlar Azure Monitor ölçümleri veritabanına yazılır. | [Windows sanal makinesi için Kaynak Yöneticisi şablonu kullanarak Konuk İşletim Sistemi ölçümlerini Azure Monitor metrik deposuna gönderme](collect-custom-metrics-guestos-resource-manager-vm.md) |
| Event Hubs | Olay Hub'larını kullanarak verileri diğer konumlara aktarmak için Tanılama Uzantısını yapılandırın.  | [Olay Hub'larını kullanarak Azure Tanılama verilerini akışla aktarın](diagnostics-extension-stream-event-hubs.md)<br>[Ölçümleri ve günlükleri izlemek için Linux Tanılama Uzantısı’nı kullanma](../../virtual-machines/extensions/diagnostics-linux.md) |
| Uygulama Öngörüleri Günlükleri | Diğer uygulama verileriyle analiz edilecek uygulamanızı destekleyen bilgi işlem kaynağından günlükleri ve performans sayaçlarını toplayın. | [Bulut Hizmeti, Sanal Makine veya Hizmet Kumaşı tanı verilerini Uygulama Öngörülerine Gönderin](diagnostics-extension-to-application-insights.md) |


### <a name="log-analytics-agent"></a>Log Analytics aracısı 
Windows veya Linux sanal makinelerinizin kapsamlı izlenmesi ve yönetimi için Log Analytics aracısını yükleyin. Sanal makine Azure'da, başka bir bulutta veya şirket içinde çalışıyor olabilir.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici Günlükleri | Log Analytics aracısı Azure Monitor'a doğrudan veya Sistem Merkezi Operasyon Yöneticisi aracılığıyla bağlanır ve yapılandırma yaptığınız veri kaynaklarından veya uygulamalar hakkında ek bilgiler sağlayan izleme çözümlerinden veri toplamanızı sağlar sanal makineüzerinde çalışıyor. | [Azure Monitor'da veri kaynağı aracısı](agent-data-sources.md)<br>[İşlem Yöneticisini Azure Monitörüne Bağlayın](om-agents.md) |
| VM Depolama | VM'ler için Azure Monitor, sağlık durumu bilgilerini özel bir konumda depolamak için Log Analytics aracısını kullanır. Daha fazla bilgi için sonraki bölüme bakın.  |


### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici 
[Sanal Makineler için Azure Monitör,](../insights/vminsights-overview.md) temel Azure Monitor işlevinin ötesinde özellikler sağlayan sanal makineler için özelleştirilmiş bir izleme deneyimi sağlar. Sanal makinede çalışan işlemler ve dış işlem bağımlılıkları hakkında keşfedilen verileri toplamak için Log Analytics aracısıyla entegre olan Windows ve Linux sanal makinelerde bir Bağımlılık Aracısı gerektirir.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici Günlükleri | İşlemler ve aracıüzerindeki bağımlılıklar hakkında veri depolar. | [Uygulama bileşenlerini anlamak için Sanal Ayarlar için Azure Monitörünü (önizleme) Harita'yı kullanma](../insights/vminsights-maps.md) |



## <a name="application-code"></a>Uygulama Kodu
Azure Monitor'da ayrıntılı uygulama izleme, çeşitli platformlarda çalışan uygulamalardan veri toplayan [Application Insights](https://docs.microsoft.com/azure/application-insights/) ile yapılır. Uygulama Azure'da, başka bir bulutta veya şirket içinde çalışıyor olabilir.

![Uygulama veri toplama](media/data-sources/applications.png)


### <a name="application-data"></a>Uygulama verileri
Bir enstrümantasyon paketi yükleyerek bir uygulama için Uygulama Öngörüleri etkinleştirdiğinizde, uygulamanın performansı ve çalışmasıyla ilgili ölçümleri ve günlükleri toplar. Application Insights topladığı verileri diğer veri kaynakları tarafından kullanılan aynı Azure Monitor veri platformunda depolar. Bu verileri analiz etmek için kapsamlı araçlar içerir, ancak Metrics Explorer ve Log Analytics gibi araçları kullanarak diğer kaynaklardan gelen verilerle de analiz edebilirsiniz.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici Günlükleri | Sayfa görünümleri, uygulama istekleri, özel durumlar ve izlemeler de dahil olmak üzere uygulamanızla ilgili operasyonel veriler. | [Azure Monitor'da günlük verilerini analiz edin](../log-query/log-query-overview.md) |
|                    | Uygulama Haritası ve telemetri korelasyondesteklemek için uygulama bileşenleri arasında bağımlılık bilgileri. | [Uygulama Öngörülerinde Telemetri korelasyon](../app/correlation.md) <br> [Uygulama Eşlemesi](../app/app-map.md) |
|            | Genel Internet'teki farklı konumlardan uygulamanızın kullanılabilirliğini ve yanıt verme durumunu test eden kullanılabilirlik testlerinin sonuçları. | [Web sitelerinin kullanılabilirlik ve yanıt hızını izleme](../app/monitor-web-app-availability.md) |
| Azure Monitör Ölçümleri | Application Insights, uygulamanızda tanımladığınız özel ölçümlere ek olarak Azure Monitor ölçümleri veritabanına uygulamanın performansını ve çalışmasını açıklayan ölçümler toplar. | [Application Insights’daki günlük tabanlı ve önceden toplanmış ölçümler](../app/pre-aggregated-metrics-log-metrics.md)<br>[Özel olaylar ve ölçümler için Application Insights API](../app/api-custom-events-metrics.md) |
| Azure Storage | Arşivleme için uygulama verilerini Azure Depolama'ya gönderin. | [Application Insights’tan telemetriyi dışarı aktarma](../app/export-telemetry.md) |
|            | Kullanılabilirlik testlerinin ayrıntıları Azure Depolama'da depolanır. Yerel analiz için indirmek için Azure portalında Uygulama Öngörüleri'ni kullanın. Kullanılabilirlik testlerinin sonuçları Azure Monitör Günlükleri'nde depolanır. | [Web sitelerinin kullanılabilirlik ve yanıt hızını izleme](../app/monitor-web-app-availability.md) |
|            | Profiler izleme verileri Azure Depolama'da depolanır. Yerel analiz için indirmek için Azure portalında Uygulama Öngörüleri'ni kullanın.  | [Application Insights ile Azure'da profil üretim uygulamaları](../app/profiler-overview.md) 
|            | Özel durumların bir alt kümesi için yakalanan hata ayıklama anlık görüntü verileri Azure Depolama'da depolanır. Yerel analiz için indirmek için Azure portalında Uygulama Öngörüleri'ni kullanın.  | [Anlık görüntüler nasıl çalışır?](../app/snapshot-debugger.md#how-snapshots-work) |

## <a name="monitoring-solutions-and-insights"></a>İzleme Çözümleri ve Öngörüler
[İzleme çözümleri](../insights/solutions.md) ve [Öngörüler,](../insights/insights-overview.md) belirli bir hizmetin veya uygulamanın işleyişihakkında ek bilgiler sağlamak için veri toplar. Farklı uygulama katmanlarındaki ve hatta birden çok katmandaki kaynaklara hitap edebilirler.

### <a name="monitoring-solutions"></a>İzleme çözümleri

| Hedef | Açıklama | Başvuru
|:---|:---|:---|
| Azure İzleyici Günlükleri | İzleme çözümleri, sorgu dilini veya genellikle çözüme dahil olan [görünümleri](view-designer.md) kullanarak analiz edilebildiği Azure Monitor günlüklerinde veri toplar. | [Azure'da çözümleri izlemek için veri toplama ayrıntıları](../insights/solutions-inventory.md) |


### <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
[Kapsayıcılar için Azure Monitör,](../insights/container-insights-overview.md) Azure [Kubernetes Hizmeti (AKS)](/azure/aks/)için özelleştirilmiş bir izleme deneyimi sağlar. Aşağıdaki tabloda açıklanan bu kaynaklar hakkında ek veri toplar.

| Hedef | Açıklama | Başvuru |
|:---|:---|:---|
| Azure İzleyici Günlükleri | Stok, günlükler ve olaylar da dahil olmak üzere AKS izleme verilerini depolar. Ölçüm verileri, portaldaki analiz işlevselliğinden yararlanmak için Günlükler'de de depolanır. | [Kapsayıcılar için Azure İzleyici ile AKS kümesi performansını anlama](../insights/container-insights-analyze.md) |
| Azure Monitör Ölçümleri | Görselleştirme ve uyarıları yönlendirmek için metrik veriler metrik veritabanında depolanır. | [Ölçüler gezgininde kapsayıcı ölçümlerini görüntüleme](../insights/container-insights-analyze.md#view-container-metrics-in-metrics-explorer) |
| Azure Kubernetes Service | Portaldaki Azure Kubernetes Hizmeti (AKS) konteyner günlüklerinize (stdout/stderror), olaylara ve pod ölçümlerine doğrudan erişim sağlar. | [Kubernetes günlüklerini, olaylarını ve pod ölçümlerini gerçek zamanlı olarak görüntüleme](../insights/container-insights-livedata-overview.md) |

### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
[Sanal Makineler için Azure Monitör,](../insights/vminsights-overview.md) sanal makineleri izlemek için özelleştirilmiş bir deneyim sağlar. Azure Monitor tarafından VM'ler için toplanan verilerin açıklaması, yukarıdaki [İşletim Sistemi (misafir)](#operating-system-guest) bölümüne dahildir.

## <a name="custom-sources"></a>Özel kaynaklar
Bir uygulamanın standart katmanlarına ek olarak, diğer veri kaynaklarıyla birlikte toplanamaz telemetrisi olan diğer kaynakları izlemeniz gerekebilir. Bu kaynaklar için, azure monitörü API'sini kullanarak bu verileri Ölçümlere veya Günlüklere yazın.

![Özel koleksiyon](media/data-sources/custom.png)

| Hedef | Yöntem | Açıklama | Başvuru |
|:---|:---|:---|:---|
| Azure İzleyici Günlükleri | Veri Toplayıcı API’si | Log Analytics çalışma alanında herhangi bir REST istemcisinden günlük verileri toplayın ve depolayın. | [GÜNLÜK verilerini HTTP Veri Toplayıcı API 'si (genel önizleme) ile Azure Monitörü'ne gönderme](data-collector-api.md) |
| Azure Monitör Ölçümleri | Özel Ölçümler API | Herhangi bir REST istemcisinden metrik veri toplayın ve Azure Monitor ölçümleri veritabanında depolayın. | [BIR REST API kullanarak Azure kaynağı için özel ölçümleri Azure Monitor metrik deposuna gönderme](metrics-store-custom-rest-api.md) |


## <a name="other-services"></a>Diğer hizmetler
Azure'daki diğer hizmetler verileri Azure Monitor veri platformuna yazar. Bu, Azure Monitor tarafından toplanan verilerle bu hizmetler tarafından toplanan verileri analiz etmenizi ve aynı analiz ve görselleştirme araçlarından yararlanmanızı sağlar.

| Hizmet | Hedef | Açıklama | Başvuru |
|:---|:---|:---|:---|
| [Azure Güvenlik Merkezi](/azure/security-center/) | Azure İzleyici Günlükleri | Azure Güvenlik Merkezi, topladığı güvenlik verilerini Azure Monitor tarafından toplanan diğer günlük verileriyle analiz edilmesine olanak tanıyan bir Log Analytics çalışma alanında depolar.  | [Azure Güvenlik Merkezinde veri toplama](../../security-center/security-center-enable-data-collection.md) |
| [Azure Sentinel](/azure/sentinel/) | Azure İzleyici Günlükleri | Azure Sentinel, farklı veri kaynaklarından topladığı verileri, Azure Monitor tarafından toplanan diğer günlük verileriyle analiz edilmesine olanak tanıyan bir Log Analytics çalışma alanında saklar.  | [Veri kaynaklarını bağlama](/azure/sentinel/quickstart-onboard) |


## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitor [tarafından toplanan izleme veritürleri](data-platform.md) ve bu verileri görüntüleme ve analiz etme hakkında daha fazla bilgi edinin.
- Azure [kaynaklarının verileri depoladığı farklı konumları](data-locations.md) ve verilere nasıl erişebileceğinizi listeleyin. 
