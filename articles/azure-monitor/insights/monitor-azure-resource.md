---
title: Azure Monitörü ile Azure kaynaklarını izleme | Microsoft Dokümanlar
description: Azure Monitor'u kullanarak Azure'daki kaynaklardan izleme verilerinin nasıl toplandığını ve analiz edilebildiğini açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/08/2019
ms.openlocfilehash: 01d188e0e39888297ff8d6a57129a3a17e1654fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249275"
---
# <a name="monitoring-azure-resources-with-azure-monitor"></a>Azure Monitörü ile Azure kaynaklarını izleme
Azure kaynaklarına dayanan kritik uygulamalarınız ve iş süreçleriniz olduğunda, bu kaynakların kullanılabilirliği, performansı ve işleyişi için izlemek istersiniz. Bu makalede, Azure kaynakları tarafından oluşturulan izleme verileri ve bu verileri analiz etmek ve uyarmak için Azure Monitor'un özelliklerini nasıl kullanabileceğiniz açıklanmaktadır.

> [!IMPORTANT]
> Bu makale, Azure Monitörü kullanan Tüm Azure hizmetleri için geçerlidir. VM'ler ve Uygulama Hizmeti de dahil olmak üzere bilgi işlem kaynakları, burada açıklanan aynı izleme verilerini oluşturur, ancak aynı zamanda günlükleri ve ölçümleri oluşturabilecek bir konuk işletim sistemine de sahiptir. Bu verilerin nasıl toplanıp analiz edilene ilişkin ayrıntılar için bu hizmetlerin izleme belgelerine bakın.

## <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?
Azure Monitor, Azure'daki tam bir yığın izleme hizmetidir ve diğer bulutlardaki ve şirket içi kaynaklara ek olarak Azure kaynaklarınızı izlemek için eksiksiz bir özellik kümesi sağlar. [Azure Monitor veri platformu,](../platform/data-platform.md) aşağıdaki bölümlerde açıklandığı gibi tam bir izleme araçları kümesi kullanılarak birlikte analiz edilebildikleri [günlükler](../platform/data-platform-logs.md) ve [ölçümler](../platform/data-platform-metrics.md) halinde veri toplar.

- [Azure Monitör Ölçümleri ile ne yapabilirsiniz?](../platform/data-platform-metrics.md#what-can-you-do-with-azure-monitor-metrics)
- [Azure Monitör Günlükleri ile ne yapabilirsiniz?](../platform/data-platform-logs.md#what-can-you-do-with-azure-monitor-logs)

Azure kaynağı oluşturduğunuz anda Azure Monitor etkinleştirilir ve [Azure portalında görüntüleyip analiz](#monitoring-in-the-azure-portal)edebileceğiniz ölçümler ve etkinlik günlükleri toplamaya başlar. Bazı yapılandırmaile ek izleme verileri toplayabilir ve ek özellikler etkinleştirebilirsiniz. Herhangi bir yapılandırma gereksinimi hakkında ayrıntılı bilgi için aşağıdaki [İzleme Verilerini](#monitoring-data) görün.


## <a name="costs-associated-with-monitoring"></a>İzleme ile ilgili maliyetler
Varsayılan olarak toplanan izleme verilerini çözümlemenin bir maliyeti yoktur. Bu, aşağıdakileri içerir:

- Platform ölçümlerini toplama ve ölçümler explorer ile analiz.
- Etkinlik günlüğünü toplama ve Azure portalında analiz etme.
- Etkinlik günlüğü uyarı kuralı oluşturma.

Günlükleri ve ölçümleri toplamak ve dışa aktarmak için Azure Monitor maliyeti yoktur, ancak hedefle ilişkili maliyetler olabilir:

- Log Analytics çalışma alanında günlükleri ve ölçümleri toplarken veri alımı ve bekletme yle ilişkili maliyetler. [Log Analytics için Azure Monitör fiyatlandırması bölümüne](https://azure.microsoft.com/pricing/details/monitor/)bakın.
- Bir Azure depolama hesabında günlükleri ve ölçümleri toplarken veri depolamayla ilişkili maliyetler. [Blob depolama için Azure Depolama fiyatlandırmasına](https://azure.microsoft.com/pricing/details/storage/blobs/)bakın.
- Günlükleri ve ölçümleri Azure Etkinlik Hub'larına ileterken olay merkezi akışıyla ilişkili maliyetler. Bkz. [Azure Etkinlik Hub'ları fiyatlandırması.](https://azure.microsoft.com/pricing/details/event-hubs/)

Aşağıdakilerle ilişkili Azure Monitor maliyetleri olabilir. [Bkz. Azure Monitör fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/):

- Günlük sorgusu çalıştırılır.
- Metrik veya günlük sorgusu uyarı kuralı oluşturma.
- Herhangi bir uyarı kuralından bildirim gönderme.
- ÖLÇÜMLERE API üzerinden erişme.

## <a name="monitoring-data"></a>Verileri izleme
Azure'daki kaynaklar aşağıdaki diyagramda gösterilen [günlükler](../platform/data-platform-logs.md) ve [ölçümler](../platform/data-platform-metrics.md) oluşturur. Oluşturdukları belirli veriler ve sağladıkları ek çözümler veya öngörüler için her Azure hizmetiiçin belgelere bakın.

![Genel Bakış](media/monitor-azure-resource/logs-metrics.png)



- [Platform ölçümleri](../platform/data-platform-metrics.md) - Düzenli aralıklarla otomatik olarak toplanan ve belirli bir zamanda kaynağın bazı yönünü açıklayan sayısal değerler. 
- [Kaynak günlükleri](../platform/platform-logs-overview.md) - Örneğin Bir Kilit Kasa'dan bir sır almak veya veritabanına istekte bulunmak gibi bir Azure kaynağı (veri düzlemi) içinde gerçekleştirilen işlemlerhakkında bilgi sağlayın. Kaynak günlüklerinin içeriği ve yapısı Azure hizmetine ve kaynak türüne göre değişir.
- [Etkinlik günlüğü](../platform/platform-logs-overview.md) - Abonelikteki her Azure kaynağındaki işlemlere dışarıdan (yönetim düzlemi) ilişkin işlemlerhakkında bilgi sağlar, örneğin yeni bir kaynak oluşturma veya sanal bir makine başlatma. Bu, aboneliğinizdeki kaynaklarda alınan herhangi bir yazma işleminin (PUT, POST, DELETE) ne, kim ve ne zaman alındığı hakkında bilgidir.


## <a name="configuration-requirements"></a>Yapılandırma gereksinimleri

### <a name="configure-monitoring"></a>İzlemeyi yapılandırma
Bazı izleme verileri otomatik olarak toplanır, ancak gereksinimlerinize bağlı olarak bazı yapılandırma gerçekleştirmeniz gerekebilir. Her izleme verisi türü için belirli bilgiler için aşağıdaki bilgilere bakın.

- [Platform ölçümleri](../platform/data-platform-metrics.md) - Platform ölçümleri yapılandırma gerektirmeden [Azure Monitör Ölçümleri'nde](../platform/data-platform-metrics.md) otomatik olarak toplanır. Girişleri Azure Monitör Günlükleri'ne göndermek veya Azure dışına iletmek için bir tanılama ayarı oluşturun.
- [Kaynak günlükleri](../platform/platform-logs-overview.md) - Kaynak günlükleri Azure kaynakları tarafından otomatik olarak oluşturulur, ancak tanılama ayarı olmadan toplanmaz.  Girişleri Azure Monitör Günlükleri'ne göndermek veya Azure dışına iletmek için bir tanılama ayarı oluşturun.
- [Etkinlik günlüğü](../platform/platform-logs-overview.md) - Etkinlik günlüğü yapılandırma gerektirmeden otomatik olarak toplanır ve Azure portalında görüntülenebilir. Bunları Azure Monitör Günlükleri'ne kopyalamak veya Azure dışına iletmek için bir tanılama ayarı oluşturun.

### <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı
Azure Monitör Günlükleri'nde veri toplamak için Bir Günlük Analizi çalışma alanı gerekir. Yeni bir çalışma alanı oluşturarak hizmetinizi hızlı bir şekilde izlemeye başlayabilirsiniz, ancak diğer hizmetlerden veri toplayan bir çalışma alanı kullanmanın bir değeri olabilir. Gereksinimleriniz için en iyi çalışma alanı tasarımını belirlemeye yardımcı olmak için çalışma alanı oluşturma ve [Azure Monitör Günlükleri dağıtımınızı tasarlama](../platform/design-logs-deployment.md) hakkında ayrıntılar için Azure [portalında Bir Günlük Analizi çalışma alanı oluşturun'](../learn/quick-create-workspace.md) a bakın. Kuruluşunuzdaki varolan bir çalışma alanı kullanıyorsanız, [Azure Monitor'da veri ve çalışma alanlarına erişimi yönet'te](../platform/manage-access.md)açıklandığı gibi uygun izinlere ihtiyacınız olacaktır. 





## <a name="diagnostic-settings"></a>Tanılama ayarları
Tanılama ayarları, belirli bir kaynağın kaynak günlüklerinin ve ölçümlerinin nereye gönderilmesi gerektiğini tanımlar. Olası varış noktaları şunlardır:

- Güçlü günlük sorguları kullanarak Azure Monitor tarafından toplanan diğer izleme verileriyle verileri analiz etmenizi ve ayrıca günlük uyarıları ve görselleştirmeler gibi diğer Azure Monitor özelliklerinden yararlanmanızı sağlayan [Log Analytics çalışma alanı.](../platform/resource-logs-collect-workspace.md) 
- Üçüncü taraf SIEM'ler ve diğer günlük analiz çözümleri gibi harici sistemlere veri akışı sağlamak için [etkinlik hub'ları.](../platform/resource-logs-stream-event-hubs.md) 
- Denetim, statik çözümleme veya yedekleme için yararlı olan [Azure depolama hesabı.](../platform/resource-logs-collect-storage.md)

Azure portalı üzerinden tanı lama ayarlarını oluşturmak ve yönetmek [için Azure'da platform günlüklerini ve ölçümlerini toplamak için tanı lama ayarını oluştur'daki](../platform/diagnostic-settings.md) yordamı izleyin. Bkz. Bir şablonda tanımlamak ve oluşturulduğunda bir kaynak için tam izleme sağlamak için [Kaynak Yöneticisi şablonu kullanarak Azure'da tanı lama ayarı oluştur'](../platform/diagnostic-settings-template.md) a bakın.


## <a name="monitoring-in-the-azure-portal"></a>Azure portalında izleme
 Çoğu Azure kaynağının izleme verilerine Azure portalındaki kaynak menüsünden erişebilirsiniz. Bu, standart Azure Monitor araçlarını kullanarak tek bir kaynağın verilerine erişmenizi sağlar. Bazı Azure hizmetleri farklı seçenekler sağlar, bu nedenle ek bilgi için söz söz le ilgili belgelere başvurmanız gerekir. Tüm izlenen kaynaklardan gelen verileri analiz etmek için **Azure Monitor** menüsünü kullanın. 

### <a name="overview"></a>Genel Bakış
Birçok hizmet, işlemlerine hızlı bir bakış olarak **Genel Bakış** sayfalarındaki verileri izlemeyi içerir. Bu genellikle Azure Monitör Ölçümleri'nde depolanan platform ölçümlerinin bir alt kümesini temel alacaktır. Diğer izleme seçenekleri genellikle hizmetlerin **Bir İzleme** bölümünde kullanılabilir. menüsünü seçerek erişebilirsiniz.

![Genel bakış sayfası](media/monitor-azure-resource/overview-page.png)


### <a name="insights-and-solutions"></a>Öngörüler ve Çözümler 
Bazı hizmetler Azure Monitor'un standart özelliklerinin ötesinde araçlar sağlar. [Öngörüler,](../insights/insights-overview.md) Azure Monitor veri platformu ve standart özellikler üzerine oluşturulmuş özelleştirilmiş bir izleme deneyimi sağlar. [Çözümler,](../insights/solutions.md) Azure Monitör Günlükleri'nde yerleşik olarak önceden tanımlanmış izleme mantığı sağlar. 

Bir hizmette Azure Monitörü öngörüsü varsa, her kaynağın menüsündeki **İzleme'den** erişebilirsiniz. **Azure Monitor** menüsünden tüm öngörülere ve çözümlere erişin.

![Insights](media/monitor-azure-resource/insights.png)

### <a name="metrics"></a>Ölçümler
Çoğu hizmet için **Ölçümler** menü öğesinden kullanılabilen [ölçümler gezginini](../platform/metrics-getting-started.md) kullanarak Azure portalındaki ölçümleri analiz edin. Bu araç, tek tek ölçümlerle çalışmanıza veya korelasyonları ve eğilimleri belirlemek için birden çok kez biraraya getirmenize olanak tanır. 

- Bkz. Metrikler gezginini kullanmanın temelleri için [Azure Ölçümleri Gezgini ile başlarken.](../platform/metrics-getting-started.md)
- Birden çok ölçüm kullanma ve filtre uygulama ve bölme gibi ölçümler in gelişmiş özellikleri için [Azure Ölçümleri](../platform/metrics-charts.md) Gezgini'nin Gelişmiş özelliklerine bakın.

![Ölçümler](media/monitor-azure-resource/metrics.png)


### <a name="activity-log"></a>Etkinlik günlüğü 
Azure portalındaki etkinlik günlüğündeki girişleri, geçerli kaynağa ayarlı ilk filtreyle görüntüleyin. Günlük sorgularında ve çalışma kitaplarında kullanmak için erişmek için etkinlik günlüğünü bir Log Analytics çalışma alanına kopyalayın. 

- Etkinlik günlüğünü görüntüleme ve çeşitli yöntemlerkullanarak girişleri alma hakkında ayrıntılar için [Azure Etkinliği günlük olaylarını görüntüle ve alın.](../platform/activity-log-view.md)
- Günlüğe kaydedilen belirli etkinlikler için Azure hizmetinizin belgelerine bakın.

![Etkinlik Günlüğü](media/monitor-azure-resource/activity-log.png)

### <a name="azure-monitor-logs"></a>Azure İzleyici Günlükleri
Azure Monitor Günlükleri, güçlü bir sorgu aracıyla analiz için birden çok hizmet ve diğer veri kaynaklarından gelen günlükleri ve ölçümleri birleştirir. Yukarıda açıklandığı gibi, Azure Monitor'da bir Günlük Analizi çalışma alanında platform ölçümleri, etkinlik günlüğü ve kaynak günlükleri toplamak için bir tanılama ayarı oluşturun.

[Log Analytics,](../log-query/get-started-portal.md) Azure Monitor'un tam özellikli bir sorgu dili kullanarak günlük verilerinin gelişmiş analizini gerçekleştirmenize olanak tanıyan güçlü bir özelliği olan [günlük sorgularıyla](../log-query/log-query-overview.md)çalışmanıza olanak tanır. Bir Azure kaynağının [sorgu kapsamı](../log-query/scope.md#query-scope)olarak kaynağı kullanarak günlük sorgularıyla çalışması için **İzleme** menüsündeki **Günlükler'den** Günlük Analitiği'ni açın. Bu, yalnızca bu kaynak için birden çok tablodaki verileri çözümlemenize olanak tanır. Tüm kaynakların günlüklerine erişmek için Azure Monitor menüsünden **Günlükler'i** kullanın. 

- Bkz. Günlük sorguları yazmak için kullanılan sorgu dilini kullanma yla ilgili bir öğretici için [Azure Monitor'daki günlük sorgularına başlayın.](../log-query/get-started-queries.md)
- Azure [Monitörü'nde](../platform/resource-logs-collect-workspace.md) kaynak günlüklerinin nasıl toplandığı ve bir sorguda nasıl erişileceksiniz hakkında ayrıntılar için Azure'daki Günlük Analizi çalışma alanında Azure kaynak günlüklerini topla'ya bakın.
- Kaynak günlüğü verilerinin Azure Monitör Günlükleri'nde nasıl yapılandırıldığına ilişkin açıklama için [Koleksiyon moduna](../platform/resource-logs-collect-workspace.md#resource-log-collection-mode) bakın.
- Azure Monitör Günlükleri'ndeki tablosundaki ayrıntılar için her Azure hizmetiiçin belgelere bakın.

![Günlükler](media/monitor-azure-resource/logs.png)

## <a name="monitoring-from-command-line"></a>Komut satırından izleme
Kaynağınızdan toplanan izleme verilerine bir komut satırından erişebilir veya [Azure PowerShell](/powershell/azure/) veya [Azure Komut Satırı Arabirimi'ni](/cli/azure/)kullanarak bir komut dosyasına ekleyebilirsiniz. 

- CLI'den metrik verilere erişmek için [CLI ölçümleri başvurusuna](/cli/azure/monitor/metrics) bakın.
- CLI'den bir günlük sorgusu kullanarak Azure Monitor Günlükleri verilerine erişmek için [CLI Log Analytics referansına](/cli/azure/ext/log-analytics/monitor/log-analytics) bakın.
- Azure PowerShell'den metrik verilere erişmek için [Azure PowerShell ölçümlerine](/powershell/module/azurerm.insights/get-azurermmetric) bakın.
- Azure PowerShell'den bir günlük sorgusu kullanarak Azure Monitor Günlükleri verilerine erişmek için [Azure PowerShell günlük sorgusu başvurusuna](/powershell/module/az.operationalinsights/Invoke-AzOperationalInsightsQuery) bakın.

## <a name="monitoring-from-rest-api"></a>REST API'den izleme
REST API kullanarak özel bir uygulamada kaynağınızdan toplanan izleme verilerini ekleyin.

- Azure Monitor REST API'sinden ölçümlere erişimle ilgili ayrıntılar için [Azure İzleme REST API walkthrough'una](../platform/rest-api-walkthrough.md) bakın.
- Azure PowerShell'den bir günlük sorgusu kullanarak Azure Monitör Günlükleri verilerine erişim hakkında bilgi için [Azure Log Analytics REST API'ye](https://dev.loganalytics.io/) bakın.

## <a name="alerts"></a>Uyarılar
[Uyarılar](../platform/alerts-overview.md) sizi proaktif olarak bilgilendirir ve izleme verilerinizde önemli koşullar bulunduğunda harekete geçme potansiyeline bağlıdır. Uyarı için bir hedef tanımlayan bir uyarı kuralı, uyarı oluşturup oluşturmamak için gerekli koşulları ve yanıt olarak yapılacak eylemleri oluşturursunuz.

Farklı türde izleme verileri, farklı türde uyarı kuralları için kullanılır.

- [Etkinlik günlüğü uyarısı](../platform/alerts-activity-log.md) - Etkinlik günlüğünde belirli ölçütlerile eşleşen bir giriş oluşturulduğunda bir uyarı oluşturun. Bu, örneğin belirli bir kaynak türü oluşturulduğunda veya yapılandırma değişikliği başarısız olduğunda bilgilendirilmenizi sağlar.
- [Metrik uyarı](../platform/alerts-metric.md) - Metrik değer belirli bir eşiği aştığında bir uyarı oluşturun. Metrik uyarılar diğer uyarılara göre daha duyarlıdır ve sorun düzeltildiğinde otomatik olarak çözülebilir.
- [Günlük sorgu uyarısı](../platform/alerts-log.md) - Günlük sorgusunu düzenli aralıklarla çalıştırın ve belirli bir koşul bulunursa bir uyarı oluşturun. Bu, birden çok veri kümesi arasında karmaşık çözümleme gerçekleştirmenize olanak tanır ve.

Uyarıları görüntülemek ve söz söz le ilgili kaynak için uyarı kurallarını yönetmek için kaynağın menüsünden **Uyarılar'ı** kullanın. Yalnızca Etkinlik günlüğü uyarıları ve Metrik uyarıları tek tek Azure kaynaklarını hedef olarak kullanır. Günlük sorgu uyarıları hedef olarak Log Analytics çalışma alanı kullanır ve bu çalışma alanında depolanan günlüklere erişebilen bir sorguya dayanır. Tüm kaynaklar ve yönetim günlüğü sorgusu uyarı kuralları için uyarıları görüntülemek ve yönetmek için Azure Monitor menüsünü kullanın.

- Uyarı kuralları oluşturma yla ilgili ayrıntılar için yukarıdaki farklı türde uyarılar için makalelere bakın.
- Uyarılara verilen yanıtları yönetmenize olanak tanıyan bir eylem grubu oluşturma yla ilgili ayrıntılar için [Azure portalında eylem grupları oluşturma ve yönetme](../platform/action-groups.md) bilgisine bakın.



## <a name="next-steps"></a>Sonraki adımlar

* Farklı Azure hizmetleri için kaynak günlüklerinin ayrıntıları [için Azure Kaynak Günlükleri için Desteklenen hizmetler, şemalar ve kategorilere](../platform/diagnostic-logs-schema.md) bakın.  
