---
title: Azure Monitör'e genel bakış | Microsoft Dokümanlar
description: Azure hizmetleriniz ve uygulamalarınız için eksiksiz bir izleme stratejisine katkıda bulunan Microsoft hizmetleri ve işlevlerine genel bakış.
ms.subservice: ''
ms.topic: overview
author: bwren
ms.author: bwren
ms.date: 10/07/2019
ms.openlocfilehash: cfdd0beac7d257a424d327df71602b4612c3da3b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79536862"
---
# <a name="azure-monitor-overview"></a>Azure İzleyici'ye genel bakış

Azure Monitor, bulut ve şirket içi ortamlarınızdan telemetri toplamak, analiz etmek ve hareket etmek için kapsamlı bir çözüm sunarak uygulamalarınızın ve hizmetlerinizin kullanılabilirliğini ve performansını en üst düzeye çıkarır. Uygulamalarınızın performansını anlamanıza ve uygulamalarla bağlı oldukları kaynakları etkileyen sorunları önceden tespit etmenize yardımcı olur.

Azure Monitor ile yapabileceklere birkaç örnek:

- [Uygulama Öngörüleri](app/app-insights-overview.md)ile uygulamalar ve bağımlılıklar arasında sorunları algılama ve tanı.
- Altyapı sorunlarını [VM'ler için Azure Monitörü](insights/vminsights-overview.md) ve [Kapsayıcılar için Azure Monitörü](insights/container-insights-overview.md)ile ilişkilendirin.
- Sorun giderme ve derin tanılama için [Log Analytics](log-query/log-query-overview.md) ile izleme verilerinizi ayrıntılı olarak analiz edin.
- [Akıllı uyarılar](platform/alerts-smartgroups-overview.md) ve [otomatik eylemlerle](platform/alerts-action-rules.md)ölçekte operasyonları destekleyin.
- Azure panoları ve [çalışma kitaplarıyla](app/usage-workbooks.md)görselleştirmeler oluşturun. [dashboards](learn/tutorial-logs-dashboards.md)

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4qXeL]


[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagramda Azure İzleyici'nin üst düzey bir görünümü sunulmaktadır. Diyagramın merkezinde, Azure Monitor'un iki temel veri kullanımı türü olan ölçümler ve günlükler için veri depoları yer almaktadır. Solda bu [veri depolarını](platform/data-platform.md)dolduran [izleme verileri kaynakları](platform/data-sources.md) vardır. Sağda, Azure Monitor'un bu toplanan verilerle gerçekleştirdiği analiz, uyarı ve harici sistemlere akış gibi farklı işlevler bulunmaktadır.

![Azure İzleyici'ye genel bakış](media/overview/overview.png)

## <a name="monitoring-data-platform"></a>İzleme veri platformu

Azure Monitor tarafından toplanan tüm veriler iki temel türden birine, [ölçümlere ve günlüklere](platform/data-platform.md)uyar. [Ölçümler,](platform/data-platform-metrics.md) bir sistemin belirli bir zaman dilimindeki bazı yönünü açıklayan sayısal değerlerdir. Onlar hafif ve yakın gerçek zamanlı senaryoları destekleme yeteneğine sahiptir. [Günlükler,](platform/data-platform-logs.md) her tür için farklı özellik kümelerine sahip kayıtlara düzenlenmiş farklı türde veri içerir. Olaylar ve izlemeler gibi telemetri, performans verilerine ek olarak günlük olarak depolanır, böylece hepsi analiz için birleştirilebilir.

Birçok Azure kaynağında, Azure Monitor tarafından toplanan verileri Azure portalındaki Genel Bakış sayfalarında görürsünüz. Örneğin herhangi bir sanal makineye göz atın ve performans ölçümlerini görüntüleyen birkaç grafik görürsünüz. Azure portalındaki [ölçümler gezginindeki](platform/metrics-charts.md) verileri açmak için grafiklerden herhangi birini tıklatın ve bu da zaman içinde birden çok ölçümün değerlerini grafikle çizmenize olanak tanır.  Grafikleri etkileşimli olarak görüntüleyebilir veya diğer görsellerle görüntülemek için bir panoya sabitleyebilirsiniz.

![Ölçümler](media/overview/metrics.png)

Azure Monitor tarafından toplanan günlük verileri, toplanan verileri hızla almak, birleştirmek ve analiz etmek için [sorgularla](log-query/log-query-overview.md) analiz edilebilir.  Azure portalında [Log Analytics'i](log-query/portals.md) kullanarak sorgular oluşturabilir ve sınayabilir ve ardından bu araçları kullanarak verileri doğrudan analiz edebilir veya [görselleştirmeler](visualizations.md) veya [uyarı kurallarıyla](platform/alerts-overview.md)kullanılmak üzere sorguları kaydedebilirsiniz.

Azure Monitor, Azure Veri Gezgini tarafından kullanılan ve basit günlük sorguları için uygun olan ancak toplamalar, birleştirmeler ve akıllı analizler gibi gelişmiş işlevleri de içeren [Kusto sorgu dilinin](/azure/kusto/query/) bir sürümünü kullanır. [Birden fazla dersi](log-query/get-started-queries.md)kullanarak sorgu dilini hızlı bir şekilde öğrenebilirsiniz.  [SQL](log-query/sql-cheatsheet.md) ve [Splunk](log-query/splunk-cheatsheet.md)’u önceden bilen kullanıcılara belirli yönergeler sağlanır.

![Günlükler](media/overview/logs.png)

## <a name="what-data-does-azure-monitor-collect"></a>Azure İzleyici hangi verileri toplar?

Azure İzleyici, farklı kaynaklardan veri toplayabilir. Uygulamalarınızla ilgili verileri izleme sürecini uygulamanızdan, bağlı olduğu işletim sisteminden ve hizmetlerinden platformun kendisine kadar olan katmanlar halinde değerlendirebilirsiniz. Azure İzleyici aşağıdaki katmanların her birinden veri toplar:

- **Uygulama izleme verileri**: Platformundan bağımsız olarak yazdığınız kodun performansı ve işlevselliği ile ilgili veriler.
- **Konuk İşletim Sistemi izleme verileri**: Uygulamanızın çalıştığı işletim sistemi yle ilgili veriler. Bu bileşen Azure'da, başka bir bulutta veya şirket içi ortamda çalışıyor olabilir. 
- **Azure kaynak izleme verileri**: Bir Azure kaynağının çalışmasıyla ilgili veriler.
- **Azure abonelik izleme verileri**: Azure aboneliğinin işleyişi ve yönetimi yle ilgili verilerin yanı sıra Azure'un kendisi yle ilgili sistem durumu ve işleyişiyle ilgili veriler. 
- **Azure kiracı izleme verileri**: Azure Etkin Dizini gibi kiracı düzeyindeki Azure hizmetlerinin çalışmasıyla ilgili veriler.

Bir Azure aboneliği oluşturup sanal makineler ve web uygulamaları gibi kaynaklar eklemeye başladığınızda Azure İzleyici veri toplamaya başlar.  Kaynaklar oluşturulduğunda veya değiştirildiğinde [etkinlik günlükleri](platform/platform-logs-overview.md) kaydedilir. [Ölçümler,](platform/data-platform.md) kaynağın nasıl performans gösterdiğini ve tüketen kaynakları anlatır. 

[Tanılamayı etkinleştirerek](platform/platform-logs-overview.md) ve kaynakları hesaplamak için bir aracı [ekleyerek,](platform/agent-windows.md) topladığınız verileri kaynakların gerçek çalışmasına genişletin. Bu, kaynağın dahili çalışması için telemetri toplar ve Windows ve Linux konuk işletim sisteminden günlükleri ve ölçümleri toplamak için farklı [veri kaynaklarını](platform/agent-data-sources.md) yapılandırmanızı sağlar. 

Uygulama Öngörüleri'nin sayfa görünümleri, uygulama istekleri ve özel durumlar da dahil olmak üzere uygulamanız hakkında ayrıntılı bilgi toplamasını sağlamak için [Uygulama Hizmetleri uygulamanız](app/azure-web-apps.md) veya [VM ve sanal makine ölçeği set uygulaması](app/azure-vm-vmss-apps.md)için izleme olanağı sağlar. Kullanıcı trafiğini simüle etmek için [kullanılabilirlik testini](app/monitor-web-app-availability.md) yapılandırarak uygulamanızın kullanılabilirliğini daha da doğrulayın.

### <a name="custom-sources"></a>Özel kaynaklar

Azure Monitor, [Veri Toplayıcı API'sini](platform/data-collector-api.md)kullanarak herhangi bir REST istemcisinden günlük verileri toplayabilir. Bu, özel izleme senaryoları oluşturmanıza ve izlemeyi diğer kaynaklar aracılığıyla telemetriyi açığa çıkarmayan kaynaklara genişletmenize olanak tanır.

## <a name="insights"></a>Insights
Verileri izleme, yalnızca bilgi işlem ortamınızın çalışmasına görünürlüğünüzü artırabilirse yararlıdır. Azure Monitor, uygulamalarınız ve bağlı oldukları diğer kaynaklar hakkında değerli bilgiler sağlayan çeşitli özellikler ve araçlar içerir. Kapsayıcılar için Uygulama [Öngörüleri](app/app-insights-overview.md) ve [Azure Monitörü](insights/container-insights-overview.md) gibi çözümleri ve özellikleri [izlemek,](insights/solutions.md) uygulamanızın ve belirli Azure hizmetlerinin farklı yönleri hakkında derin bilgiler sağlar. 

### <a name="application-insights"></a>Application Insights
[Application Insights,](app/app-insights-overview.md) bulutta veya şirket içinde barındırılan web uygulamalarınızın kullanılabilirliğini, performansını ve kullanımını izler. Azure Monitor'daki güçlü veri analizi platformundan yararlanarak uygulamanızın işlemleri hakkında derin bilgiler sağlar ve bir kullanıcının bunları bildirmesini beklemeden hataları tanılar. Application Insights, çeşitli geliştirme araçlarına bağlantı noktaları içerir ve DevOps süreçlerinizi desteklemek için Visual Studio ile tümleşir.

![Uygulama Öngörüleri](media/overview/app-insights.png)

### <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
[Kapsayıcılar için Azure Monitörü,](insights/container-insights-overview.md) Azure Kubernetes Hizmeti'nde (AKS) barındırılan yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinin performansını izlemek üzere tasarlanmış bir özelliktir. Ölçümler API'si aracılığıyla Kubernetes'te bulunan denetleyicilerden, düğümlerden ve kapsayıcılardan bellek ve işlemci ölçümlerini toplayarak performans görünürlüğü sağlar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerinden izlemeyi etkinleştirdikten sonra, bu ölçümler ve günlükler Linux için Log Analytics aracısının konteynerleştirilmiş bir sürümü aracılığıyla sizin için otomatik olarak toplanır.

![Konteyner Sağlık](media/overview/container-insights.png)

### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
[Sanal Makineler için Azure Monitor,](insights/vminsights-overview.md) Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz ederek azure sanal makinelerinizi (VM) farklı süreçleri ve diğer kaynaklara ve dış işlemlere olan birbirine bağlı bağımlılıkları da dahil olmak üzere ölçekte izler. Çözüm, şirket içinde barındırılan VM'ler veya başka bir bulut sağlayıcısı için performans ve uygulama bağımlılıklarını izleme desteğini içerir.  


![VM Öngörüleri](media/overview/vm-insights.png)

### <a name="monitoring-solutions"></a>İzleme çözümleri
Azure Monitor'daki [izleme çözümleri,](insights/solutions.md) belirli bir uygulama veya hizmet için öngörüler sağlayan paketlenmiş mantık kümeleridir. Bunlar uygulama veya hizmet için izleme verileri toplamak için mantık, bu verileri çözümlemek için [sorgular](log-query/log-query-overview.md) ve görselleştirme için [görünümler](../log-analytics/log-analytics-view-designer.md) içerir. İzleme çözümleri, çeşitli Azure hizmetleri ve diğer uygulamalar için izleme sağlamak için Microsoft ve iş ortakları [tarafından kullanılabilir.](insights/solutions-inventory.md)

![İzleme çözümleri](media/overview/solutions-overview.png)

## <a name="responding-to-critical-situations"></a>Kritik durumlara yanıt verme
İzleme verilerini etkileşimli olarak analiz etmenize izin vermenin yanı sıra, etkili bir izleme çözümü topladığı verilerde tanımlanan kritik koşullara proaktif bir şekilde yanıt verebilmelidir. Bu, bir sorunu araştırmadan sorumlu yöneticiye bir metin veya posta göndermek olabilir. Veya bir hata koşulunu düzeltmeye çalışan otomatik bir işlem başlatabilirsiniz.


### <a name="alerts"></a>Uyarılar
[Azure Monitor'daki uyarılar,](platform/alerts-overview.md) kritik koşulları proaktif bir şekilde bildirir ve düzeltici eylemde bulunma potansiyeline bağlıdır. Ölçümlere dayalı uyarı kuralları sayısal değerlere dayalı neredeyse gerçek zamanlı uyarı sağlarken, günlüklere dayalı kurallar birden çok kaynaktan gelen veriler arasında karmaşık mantık sağlar.

Azure Monitor'daki uyarı kuralları, benzersiz alıcı kümeleri ve birden çok kural arasında paylaşılabilen eylemler içeren [eylem gruplarını](platform/action-groups.md)kullanır. İhtiyaçlarınıza bağlı olarak, eylem grupları uyarıların dış eylemleri başlatması veya ITSM araçlarınızla tümleştirmesi için web hooks kullanma gibi eylemleri gerçekleştirebilir.

![Uyarılar](media/overview/alerts.png)

### <a name="autoscale"></a>Otomatik Ölçeklendirme
Otomatik ölçeklendirme, uygulamanızdaki yükü işlemek için doğru miktarda kaynağa sahip olmasını sağlar. Yük artışlarını işlemek için otomatik olarak ne zaman kaynak ekleyeceğinibelirlemek ve boşta oturan kaynakları kaldırarak paradan tasarruf etmek için Azure Monitor tarafından toplanan ölçümleri kullanan kurallar oluşturmanıza olanak tanır. En az ve en fazla sayıda örnek ve kaynakları ne zaman artıracağınız veya azaltacağınız mantığını belirtirsiniz.

![Otomatik Ölçeklendirme](media/overview/autoscale.png)

## <a name="visualizing-monitoring-data"></a>İzleme verilerini görselleştirme
Grafikler ve tablolar gibi [görselleştirmeler,](visualizations.md) verileri özetleme ve farklı kitlelere sunma için etkili araçlardır. Azure Monitor, izleme verilerini görselleştirmek için kendi özelliklere sahiptir ve verileri farklı kitlelere yayınlamak için diğer Azure hizmetlerinden yararlanır.

### <a name="dashboards"></a>Panolar
[Azure panoları,](../azure-portal/azure-portal-dashboards.md) hem ölçümler hem de günlükler de dahil olmak üzere farklı veri türlerini [Azure portalında](https://portal.azure.com)tek bir bölmede birleştirmenize olanak tanır. Panoyu isteğe bağlı olarak diğer Azure kullanıcılarıyla paylaşabilirsiniz. Azure Monitor'daki öğeler, herhangi bir günlük sorgusunun veya ölçüm grafiğinin çıktısına ek olarak Azure panosuna eklenebilir. Örneğin, ölçümlerin grafiğini, etkinlik günlükleri tablosunu, Application Insights'tan bir kullanım grafiğini ve günlük sorgusunun çıktısını gösteren kutucukları birleştiren bir pano oluşturabilirsiniz.

![Pano](media/overview/dashboard.png)

### <a name="views"></a>Görünümler
Azure Monitor'da [günlük](../log-analytics/log-analytics-view-designer.md) verilerini görsel olarak görüntüler.  Her görünüm, kritik verileri özetleyen listelere ek olarak çubuk ve çizgi grafikleri gibi görselleştirmelerin bir birleşimini deşifre eden tek bir döşeme içerir.  İzleme çözümleri, belirli bir uygulama için verileri özetleyen görünümleri içerir ve herhangi bir günlük sorgusundan veri sunmak için kendi görünümlerinizi oluşturabilirsiniz. Azure Monitor'daki diğer öğeler gibi görünümler de Azure panolarına eklenebilir.

![Görünüm](media/overview/view.png)

### <a name="power-bi"></a>Power BI
[Power BI,](https://powerbi.microsoft.com) çeşitli veri kaynaklarında etkileşimli görselleştirmeler sağlayan ve verilerin kuruluşunuz içinde ve dışında başkaları tarafından kullanılabilir hale getirilmesi için etkili bir araç olan bir iş analitiği hizmetidir. Power BI'yi, bu ek görselleştirmelerden yararlanmak için [Azure Monitor'dan günlük verilerini otomatik olarak almak](../log-analytics/log-analytics-powerbi.md) üzere yapılandırabilirsiniz.


![Power BI](media/overview/power-bi.png)


## <a name="integrate-and-export-data"></a>Verileri tümleştirme ve dışa aktarma
Azure Monitor'u diğer sistemlerle tümleştirme ve izleme verilerinizi kullanan özel çözümler oluşturma gereksiniminiz genellikle vardır. Diğer Azure hizmetleri, bu tümleştirmeyi sağlamak için Azure Monitor ile birlikte çalışır.

### <a name="event-hub"></a>Olay Hub'ı
[Azure Etkinlik Hub'ları,](https://docs.microsoft.com/azure/event-hubs) gerçek zamanlı analiz sağlayıcısı veya toplu/depolama bağdaştırıcıları kullanarak verileri dönüştürebilen ve depolayabilen bir akış platformu ve etkinlik oluşturma hizmetidir. İş ortağı SIEM ve izleme araçlarına [Azure Monitor verilerini aktarmak](platform/stream-monitoring-data-event-hubs.md) için Olay Hub'larını kullanın.


### <a name="logic-apps"></a>Logic Apps
[Logic Apps,](https://azure.microsoft.com/services/logic-apps) farklı sistem ve hizmetlerle tümleşen iş akışlarını kullanarak görevleri ve iş süreçlerini otomatikleştirmenize olanak tanıyan bir hizmettir. Azure Monitor'da ölçümleri ve günlükleri okuyan ve yazan etkinlikler mevcuttur ve bu da çeşitli diğer sistemlerle entegre iş akışları oluşturmanıza olanak tanır.


### <a name="api"></a>API
Oluşturulan uyarılara erişmeye ek olarak Azure Monitor'a ve Azure Monitor'dan ölçümleri ve günlükleri okumak ve yazmak için birden fazla API kullanılabilir. Uyarıları yapılandırabilir ve alabilirsiniz. Bu, Azure Monitor ile entegre olan özel çözümler oluşturmanız için size sınırsız olanaklar sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdakiler hakkında daha fazla bilgi edinin:

* Azure Monitor tarafından toplanan verilerin [ölçümleri ve günlükleri.](platform/data-platform.md)
* Uygulamanızın farklı bileşenlerinin telemetriyi nasıl gönderdiğine yönelik [veri kaynakları.](platform/data-sources.md)
* Toplanan verileri çözümleme için [günlük sorguları.](log-query/log-query-overview.md)
* Bulut uygulamalarını ve hizmetlerini izlemek için [en iyi uygulamalar.](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)
