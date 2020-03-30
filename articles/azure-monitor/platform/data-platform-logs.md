---
title: Azure Monitör'de Oturum Açma | Microsoft Dokümanlar
description: Azure Monitor'da, izleme verilerinin gelişmiş analizi için kullanılan günlükleri açıklar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: dd499a82ba1011d96772d6722e25a434d43a6bf0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480207"
---
# <a name="logs-in-azure-monitor"></a>Azure İzleyici'deki günlükler

> [!NOTE]
> Azure Monitor tarafından toplanan tüm veriler, Ölçümler ve Günlükler olmak üzere iki temel türden birine uyar. Bu makalede Günlükler açıklanmaktadır. Ölçümlerin ayrıntılı bir açıklaması için [Azure Monitor'daki Ölçümlere](data-platform-metrics.md) ve azure [monitor tarafından toplanan verilerin](data-platform.md) izlenmesine bakın.

Azure Monitor'daki günlükler, özellikle çeşitli kaynaklardan gelen veriler arasında karmaşık analizler gerçekleştirmek için yararlıdır. Bu makalede, Günlüklerin Azure Monitor'da nasıl yapılandırıldığı, verilerle neler yapabileceğiniz açıklanmaktadır ve Günlüklerde veri depolayan farklı veri kaynaklarını tanımlar.

> [!NOTE]
> Azure Monitör Günlükleri ile Azure'daki günlük veri kaynakları arasında ayrım yapmak önemlidir. Örneğin, Azure'daki abonelik düzeyi olayları, Azure Monitörü menüsünden görüntülediğiniz bir [etkinlik günlüğüne](platform-logs-overview.md) yazılır. Kaynakların çoğu, farklı konumlara iletebileceğiniz bir [kaynak günlüğüne](platform-logs-overview.md) operasyonel bilgiler yazar. Azure Monitor Günlükleri, tüm kaynak kümenizde derin analiz sağlamak için etkinlik günlüklerini ve kaynak günlüklerini diğer izleme verileriyle birlikte toplayan bir günlük veri platformudur.

## <a name="what-are-azure-monitor-logs"></a>Azure Monitör Günlükleri nedir?

Azure Monitor'daki günlükler, her tür için farklı özellik kümelerine sahip kayıtlara düzenlenmiş farklı türde veriler içerir. Günlükler Azure Monitör Ölçümleri gibi sayısal değerler içerebilir, ancak genellikle ayrıntılı açıklamalariçeren metin verileri içerir. Ayrıca, yapılarına göre farklılık gösterirler ve genellikle düzenli aralıklarla toplanmazlardı. Olaylar ve izlemeler gibi telemetri, performans verilerine ek olarak Azure Monitör Günlükleri depolanır, böylece tümü analiz için birleştirilebilir.

Günlük girişi nin yaygın bir türü, ara sıra toplanan bir olaydır. Olaylar bir uygulama veya hizmet tarafından oluşturulur ve genellikle kendi tam bağlam sağlamak için yeterli bilgi içerir. Örneğin, bir olay belirli bir kaynağın oluşturulduğunu veya değiştirildiğini, artan trafiğe yanıt olarak yeni bir ana bilgisayar başlatıldığını veya bir uygulamada bir hata algılandığını gösterebilir.

 Verilerin biçimi değişebildiği için, uygulamalar gereksinim duydukları yapıyı kullanarak özel günlükler oluşturabilir. Metrik veriler, eğilim ve diğer veri analizi için diğer izleme verileriyle birleştirmek için Günlükler'de bile depolanabilir.


## <a name="what-can-you-do-with-azure-monitor-logs"></a>Azure Monitör Günlükleri ile ne yapabilirsiniz?
Aşağıdaki tabloda, Azure Monitor'da Günlükler'i kullanabileceğiniz farklı yollar listelenmektedir.


|  |  |
|:---|:---|
| Çözümleme | [Günlük sorguları](../log-query/log-query-overview.md) yazmak ve güçlü Data Explorer çözümleme altyapısını kullanarak günlük verilerini etkileşimli olarak analiz etmek için Azure portalındaki [Günlük Analitiği'ni](../log-query/get-started-portal.md) kullanın.<br>Günlük sorguları yazmak ve Application Insights'tan gelen günlük verilerini etkileşimli olarak analiz etmek için Azure portalındaki [Application Insights analiz konsoluna](../app/analytics.md) bakın. |
| Görselleştirme | Tablo veya grafik olarak işlenen sorgu sonuçlarını [Azure panosuna](../../azure-portal/azure-portal-dashboards.md)sabitleyin.<br>Etkileşimli bir raporda birden çok veri kümesiyle birleştirmek için bir [çalışma kitabı](../app/usage-workbooks.md) oluşturun. <br>Farklı görselleştirmeler kullanmak ve Azure dışındaki kullanıcılarla paylaşmak için bir sorgunun sonuçlarını [Power BI'ye](powerbi.md) dışa aktarın.<br>Bir sorgunun sonuçlarını [grafana'ya](grafana-plugin.md) aktararak panolarından yararlanın ve diğer veri kaynaklarıyla birleştirin.|
| Uyarı | Sorgusonuçları belirli bir sonuçla eşleştiğinde bildirim gönderen veya [otomatik eylemde](action-groups.md) bulunulan bir [günlük uyarı kuralını](alerts-log.md) yapılandırın.<br>Metrik olarak ayıklanan belirli günlük veri günlüklerinde [bir metrik uyarı kuralını](alerts-metric-logs.md) yapılandırın. |
| Almak | [Azure CLI'yi](/cli/azure/ext/log-analytics/monitor/log-analytics)kullanan bir komut satırından giriş günlüğü sorgusu sonuçlarına erişin.<br>[PowerShell cmdlets](https://docs.microsoft.com/powershell/module/az.operationalinsights)kullanarak bir komut satırından erişim günlüğü sorgu sonuçları .<br>REST API'yi kullanarak özel bir uygulamadan gelen günlük sorgusu [sonuçlarına erişin.](https://dev.loganalytics.io/) |
| Dışarı Aktarma | Günlük verilerini almak ve [Logic Apps'ı](~/articles/logic-apps/index.yml)kullanarak harici bir konuma kopyalamak için bir iş akışı oluşturun. |


## <a name="how-is-data-in-azure-monitor-logs-structured"></a>Azure Monitör Günlükleri'ndeki veriler nasıl yapılandırılır?
Azure Monitor Günlükleri tarafından toplanan veriler bir [Log Analytics çalışma alanında](../platform/design-logs-deployment.md)depolanır. Her çalışma alanı, her bir kaynağın verilerini depolayan birden çok tablo içerir. Tüm tablolar [bazı ortak özellikleri](log-standard-properties.md)paylaşırken, her biri depolanan veri türüne bağlı olarak benzersiz bir özellik kümesine sahiptir. Yeni bir çalışma alanı standart tablo kümesine sahip olacak ve farklı izleme çözümleri ve çalışma alanına yazan diğer hizmetler tarafından daha fazla tablo eklenecektir.

Application Insights'taki günlük verileri çalışma alanlarıyla aynı Log Analytics altyapısını kullanır, ancak izlenen her uygulama için ayrı olarak depolanır. Her uygulama, uygulama istekleri, özel durumlar ve sayfa görünümleri gibi verileri tutmak için standart bir tablo kümesine sahiptir.

Günlük sorguları, Log Analytics çalışma alanından veya Application Insights uygulamasından gelen verileri kullanır. Uygulama verilerini diğer günlük verileriyle birlikte çözümlemek veya birden çok çalışma alanı veya uygulama içeren sorgular oluşturmak için bir [çapraz kaynak sorgusu](../log-query/cross-workspace-query.md) kullanabilirsiniz.

![Çalışma Alanları](media/data-platform-logs/workspaces.png)

## <a name="log-queries"></a>Günlük sorguları
Azure Monitor Günlükleri'ndeki veriler, toplanan verileri hızla almanıza, birleştirmenize ve çözümlemenize olanak tanıyan [Kusto sorgu diliyle](../log-query/get-started-queries.md)yazılmış bir [günlük sorgusu](../log-query/log-query-overview.md) kullanılarak alınır. Azure portalında günlük sorgularını yazmak ve test etmek için [Günlük Analizi'ni](../log-query/portals.md) kullanın. Sonuçlarla etkileşimli olarak çalışmanızı veya diğer görselleştirmelerle görüntülemek için bir panoya sabitlemenizi sağlar.

![Log Analytics](media/data-platform-logs/log-analytics.png)

Application Insights verilerini analiz etmek için [Application Insights'tan Günlük Analizi'ni](../app/analytics.md) açın.

![Uygulama Öngörüleri Analizi](media/data-platform-logs/app-insights-analytics.png)

[Ayrıca Log Analytics API](https://dev.loganalytics.io/documentation/overview) ve Application [Insights REST API'yi](https://dev.applicationinsights.io/documentation/overview)kullanarak günlük verilerini de alabilirsiniz.


## <a name="sources-of-azure-monitor-logs"></a>Azure Monitör Günlüklerinin Kaynakları
Azure Monitor, hem Azure içindeki çeşitli kaynaklardan hem de şirket içi kaynaklardan günlük verileri toplayabilir. Aşağıdaki tablolar, Azure Monitor Günlükleri'ne veri yazan farklı kaynaklardan elde edilen farklı veri kaynaklarını listelemaktadır. Her gerekli yapılandırma ayrıntıları için bir bağlantı vardır.

### <a name="azure-tenant-and-subscription"></a>Azure kiracı ve abonelik

| Veri | Açıklama |
|:---|:---|
| Azure Active Directory denetim günlükleri | Her dizin için Tanılama ayarları ile yapılandırıldı. Bkz. [Azure REKLAM günlüklerini Azure Monitor günlükleriyle tümleştirin.](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)  |
| Etkinlik günlükleri | Varsayılan olarak ayrı olarak depolanır ve neredeyse gerçek zamanlı uyarılar için kullanılabilir. Log Analytics çalışma alanına yazmak için Etkinlik günlüğü Analytics çözümlerini yükleyin. [Bkz. Log Analytics'te Azure etkinlik günlüklerini topla ve analiz edin.](activity-log-collect.md) |

### <a name="azure-resources"></a>Azure kaynakları

| Veri | Açıklama |
|:---|:---|
| Kaynak tanılama | Tanılama ayarlarını, günlük analizi çalışma alanına ölçümler de dahil olmak üzere tanılama verilerine yazacak şekilde yapılandırın. [Bkz. Azure kaynak günlüklerini Günlük Analitiği'ne](resource-logs-collect-workspace.md)aktarın. |
| İzleme çözümleri | İzleme çözümleri, topladıkları verileri Log Analytics çalışma alanına yazar. Bir çözüm listesi [için Azure'daki yönetim çözümleri için Veri toplama ayrıntılarına](../insights/solutions-inventory.md) bakın. Çözümleri yükleme ve kullanma yla ilgili ayrıntılar için [Azure Monitor'daki İzleme çözümlerine](../insights/solutions.md) bakın. |
| Ölçümler | Günlük verilerini daha uzun süre tutmak ve [Kusto sorgu dilini](/azure/kusto/query/)kullanarak diğer veri türleri ile karmaşık analizler gerçekleştirmek için Azure Monitor kaynakları için platform ölçümlerini Günlük Analizi çalışma alanına gönderin. [Bkz. Azure kaynak günlüklerini Günlük Analitiği'ne](resource-logs-collect-storage.md)aktarın. |
| Azure tablo depolama | Bazı Azure kaynaklarının izleme verileri yazdığı Azure depolamasından veri toplayın. Bkz. [Log Analytics'e sahip etkinlikler için IIS ve Azure tablo depolama alanı için Azure blob depolama](diagnostics-extension-logs.md)kullanın. |

### <a name="virtual-machines"></a>Virtual Machines

| Veri | Açıklama |
|:---|:---|
|  Aracı veri kaynakları | [Windows](agent-windows.md) ve [Linux](../learn/quick-collect-linux-computer.md) aracılarından toplanan veri kaynakları arasında olaylar, performans verileri ve özel günlükler yer almaktadır. Veri kaynakları nın ve yapılandırmayla ilgili ayrıntıların listesi için [Azure Monitor'daki Aracı veri kaynaklarına](data-sources.md) bakın. |
| İzleme çözümleri | İzleme çözümleri, aracılardan topladıkları verileri Log Analytics çalışma alanına yazar. Bir çözüm listesi [için Azure'daki yönetim çözümleri için Veri toplama ayrıntılarına](../insights/solutions-inventory.md) bakın. Çözümleri yükleme ve kullanma yla ilgili ayrıntılar için [Azure Monitor'daki İzleme çözümlerine](../insights/solutions.md) bakın. |
| System Center Operations Manager | Şirket içi aracılardan etkinlik ve performans verilerini günlüklere toplamak için Operasyon Yöneticisi yönetim grubunu Azure Monitor'a bağlayın. Bu yapılandırmayla ilgili ayrıntılar için [İşlem Yöneticisi'ni Log Analytics'e bağlayın.](om-agents.md) |


### <a name="applications"></a>Uygulamalar

| Veri | Açıklama |
|:---|:---|
| İstekler ve özel durumlar | Uygulama istekleri ve özel durumlar hakkında ayrıntılı veriler _isteklerde,_ _sayfa Görünümlerinde_ve _özel durumlar_ tablolarında dır. Dış [bileşenlere](../app/asp-net-dependencies.md) yapılan çağrılar _bağımlılıklar_ tablosundadır. |
| Kullanım ve performans | Uygulama için performans _istekleri_mevcuttur , _browserTimings_ ve _performanceCounters_ tablolar. Özel [ölçümlere](../app/api-custom-events-metrics.md#trackevent) ait veriler _özel Ölçümler_ tablosundadır.|
| Verileri izleme | [Dağıtılmış izleme](../app/distributed-tracing.md) sonuçları _izleme_ tablosunda depolanır. |
| Kullanılabilirlik testleri | [Kullanılabilirlik testlerinden](../app/monitor-web-app-availability.md) elde edilen özet veriler _kullanılabilirlik Sonuçları_ tablosunda depolanır. Bu testlerin ayrıntılı verileri ayrı depolama alanındadır ve Azure portalındaki Uygulama Öngörüleri'nden erişilir. |

### <a name="insights"></a>Insights

| Veri | Açıklama |
|:---|:---|
| Kapsayıcılar için Azure İzleyici | [Kapsayıcılar için Azure Monitor](../insights/container-insights-overview.md)tarafından toplanan envanter ve performans verileri. Tabloların listesi için [Kapsayıcı veri toplama ayrıntılarına](../insights/container-insights-log-search.md#container-records) bakın. |
| VM'ler için Azure İzleyici | [Sanal Veriler için Azure Monitor](../insights/vminsights-overview.md)tarafından toplanan harita ve performans verileri. Bu verileri sorgulamayla ilgili ayrıntılar [için Sanal Taşıtlar için Azure Monitor'dan günlükleri nasıl sorgularınız.](../insights/vminsights-log-search.md) |

### <a name="custom"></a>Özel 

| Veri | Açıklama |
|:---|:---|
| REST API | Herhangi bir REST istemcisinden bir Log Analytics çalışma alanına veri yazın. Ayrıntılar için [http veri toplayıcı api'si ile azure monitörüne günlük verilerini gönder'e](data-collector-api.md) bakın.
| Logic App | Azure Log **Analytics Veri Toplayıcı** eylemiyle Bir Logic App iş akışından günlük analizi çalışma alanına herhangi bir veri yazın. |

### <a name="security"></a>Güvenlik

| Veri | Açıklama |
|:---|:---|
| Azure Güvenlik Merkezi | [Azure Güvenlik Merkezi,](/azure/security-center/) topladığı verileri diğer günlük verileriyle analiz edilebilen bir Log Analytics çalışma alanında depolar. Çalışma alanı yapılandırması hakkında ayrıntılar için [Azure Güvenlik Merkezi'ndeki Veri koleksiyonuna](../../security-center/security-center-enable-data-collection.md) bakın. |
| Azure Sentinel | [Azure Sentinel,](/azure/sentinel/) veri kaynaklarından elde edilen verileri bir Log Analytics çalışma alanına kaydeder. Bkz. [Veri kaynaklarını bağla.](/azure/sentinel/connect-data-sources)  |


## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitör [veri platformu](data-platform.md)hakkında daha fazla bilgi edinin.
- Azure [Monitor'da ölçümler](data-platform-metrics.md)hakkında bilgi edinin.
- Azure'daki farklı kaynaklar için [kullanılabilen izleme verileri](data-sources.md) hakkında bilgi edinin.
