---
title: Azure Izleyici ile Azure kaynaklarını izleme | Microsoft Docs
description: Azure Izleyici kullanarak Azure 'daki kaynaklardan izleme verilerinin nasıl toplanacağını ve analiz edileceğini açıklar.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/08/2019
ms.openlocfilehash: 8ba50e5b55e31c6542e81879068bf231918ce1b0
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244895"
---
# <a name="monitoring-azure-resources-with-azure-monitor"></a>Azure Izleyici ile Azure kaynaklarını izleme
Azure kaynaklarına bağlı kritik Uygulamalarınız ve iş süreçleriniz olduğunda, bu kaynakları kullanılabilirlik, performans ve işlem için izlemek istersiniz. Bu makalede, Azure kaynakları tarafından oluşturulan izleme verileri ve bu verileri çözümlemek ve uyarmak için Azure Izleyici 'nin özelliklerini nasıl kullanabileceğiniz açıklanır.

> [!IMPORTANT]
> Bu makale Azure Izleyici kullanan tüm Azure hizmetleri için geçerlidir. VM 'Ler ve App Service dahil işlem kaynakları, burada açıklanan izleme verilerinin aynısını oluşturur ve ayrıca Günlükler ve ölçümler oluşturabilen bir konuk işletim sistemine sahiptir. Bu verilerin toplanması ve çözümlenmesi hakkında ayrıntılı bilgi edinmek için bu hizmetlere ilişkin izleme belgelerine bakın.

## <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?
Azure Izleyici, Azure 'da, diğer bulutlardaki ve Şirket içindeki kaynaklara ek olarak Azure kaynaklarınızı izlemeye yönelik eksiksiz bir özellik kümesi sunan tam bir yığın izleme hizmetidir. [Azure izleyici veri platformu](../platform/data-platform.md) , aşağıdaki bölümlerde açıklandığı gibi, bir dizi izleme aracı kullanılarak birlikte çözümlenebilecekleri [günlüklere](../platform/data-platform-logs.md) ve [ölçümlere](../platform/data-platform-metrics.md) veri toplar.

- [Azure Izleyici ölçümleriyle ne yapabilirsiniz?](../platform/data-platform-metrics.md#what-can-you-do-with-azure-monitor-metrics)
- [Azure Izleyici günlükleri ile neler yapabilirsiniz?](../platform/data-platform-logs.md#what-can-you-do-with-azure-monitor-logs)

Bir Azure kaynağı oluşturandan itibaren Azure Izleyici etkinleştirilir ve [Azure Portal görüntüleyip çözümleyebilmeniz](#monitoring-in-the-azure-portal)gereken ölçümleri ve etkinlik günlüklerini toplamaya başlar. Bazı yapılandırmalar ile ek izleme verileri toplayabilir ve ek özellikleri etkinleştirebilirsiniz. Yapılandırma gereksinimleriyle ilgili ayrıntılı bilgi için aşağıdaki [Izleme verilerini](#monitoring-data) inceleyin.


## <a name="costs-associated-with-monitoring"></a>İzleme ile ilişkili maliyetler
Varsayılan olarak toplanan izleme verilerinin çözümlenmesi için bir maliyet yoktur. Buna aşağıdakiler dahildir:

- Platform ölçümleri toplanıyor ve bunları Ölçüm Gezgini ile analiz etme.
- Etkinlik günlüğü toplanıyor ve Azure portal analiz ediliyor.
- Etkinlik günlüğü uyarı kuralı oluşturuluyor.

Günlükleri ve ölçümleri toplamak ve dışarı aktarmak için Azure Izleyici maliyeti yoktur, ancak hedefle ilişkili ilişkili maliyetler olabilir:

- Log Analytics çalışma alanında Günlükler ve ölçümler toplanırken veri alımı ve bekletme ile ilişkili maliyetler. [Log Analytics için bkz. Azure izleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/).
- Azure depolama hesabına Günlükler ve ölçümler toplanırken veri depolama ile ilişkili maliyetler. Bkz. [BLOB depolama Için Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).
- Günlükler ve ölçümler Azure Event Hubs iletilirken, Olay Hub 'ı akışıyla ilişkili maliyetler. Bkz. [Azure Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

Aşağıdakiler ile ilişkili Azure Izleyici maliyeti olabilir. Bkz. [Azure izleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/):

- Günlük sorgusu çalıştırılıyor.
- Ölçüm veya günlük sorgusu uyarı kuralı oluşturma.
- Herhangi bir uyarı kuralından bildirim gönderiliyor.
- API aracılığıyla ölçümlere erişme.

## <a name="monitoring-data"></a>Verileri izleme
Azure 'daki kaynaklar, aşağıdaki diyagramı gösterilen şekilde [Günlükler](../platform/data-platform-logs.md) ve [ölçümler](../platform/data-platform-metrics.md) oluşturur. Oluşturdukları belirli veriler ve sağladıkları tüm ek çözümler veya içgörüler için her bir Azure hizmeti için belgelere bakın.

![Genel Bakış](media/monitor-azure-resource/logs-metrics.png)



- [Platform ölçümleri](../platform/data-platform-metrics.md) -düzenli aralıklarla otomatik olarak toplanan ve belirli bir zamanda kaynağın bazı yönlerini tanımlayan sayısal değerlerdir. 
- [Kaynak günlükleri](../platform/resource-logs-overview.md) -bir Azure kaynağı içinde (veri düzlemi) gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Örneğin, bir Key Vault gizli anahtar alma veya bir veritabanına istek yapma. Kaynak günlüklerinin içeriği ve yapısı, Azure hizmeti ve kaynak türüne göre farklılık gösterir.
- [Etkinlik günlüğü](../platform/activity-logs-overview.md) -abonelik içindeki her bir Azure kaynağında (Yönetim düzlemi) gerçekleştirilen işlemlere ilişkin öngörüler sağlar (örneğin, yeni bir kaynak oluşturma veya bir sanal makine başlatma). Bu, aboneliğinizdeki kaynaklar üzerinde herhangi bir yazma işlemi (PUT, POST, SILME) için ne zaman, kim ve ne zaman alındığını öğrenin.


## <a name="configuration-requirements"></a>Yapılandırma gereksinimleri
Bazı izleme verileri otomatik olarak toplanır, ancak gereksinimlerinize bağlı olarak bazı yapılandırmalar yapmanız gerekebilir. Her bir izleme verileri türü için belirli bilgiler için aşağıdaki bilgilere bakın.

- [Platform ölçümleri](../platform/data-platform-metrics.md) -platform ölçümleri, hiçbir yapılandırma gerekmeden [Azure izleyici ölçümlerine](../platform/data-platform-metrics.md) otomatik olarak toplanır. Azure Izleyici günlüklerine giriş göndermek veya Azure 'un dışına iletmek için bir tanılama ayarı oluşturun.
- [Kaynak günlükleri](../platform/resource-logs-overview.md) -kaynak günlükleri Azure kaynakları tarafından otomatik olarak oluşturulur ancak bir tanılama ayarı olmadan toplanmaz.  Azure Izleyici günlüklerine giriş göndermek veya Azure 'un dışına iletmek için bir tanılama ayarı oluşturun.
- [Etkinlik günlüğü](../platform/activity-logs-overview.md) -etkinlik günlüğü, yapılandırma gerekmeden otomatik olarak toplanır ve Azure Portal görüntülenebilir. Azure Izleyici günlüklerine kopyalamak veya bunları Azure 'un dışına iletmek için bir tanılama ayarı oluşturun.


## <a name="diagnostic-settings"></a>Tanılama ayarları
Tanılama ayarları, belirli bir kaynağın kaynak günlüklerinin ve ölçümlerinin nereye gönderileceğini tanımlar. Olası hedefler şunlardır:

- Güçlü günlük sorguları ve ayrıca günlük uyarıları ve görselleştirmeler gibi diğer Azure Izleyici özelliklerinden yararlanmak için, Azure Izleyici tarafından toplanan diğer izleme verileriyle verileri analiz etmenizi sağlayan [Log Analytics çalışma alanı](../platform/resource-logs-collect-workspace.md) . 
- Üçüncü taraf Sıems ve diğer Log Analytics çözümleri gibi dış sistemlere veri akışı için [Olay Hub 'ları](../platform/resource-logs-stream-event-hubs.md) . 
- Denetim, statik analiz veya yedekleme için yararlı olan [Azure depolama hesabı](../platform/resource-logs-collect-storage.md) .

Azure portal aracılığıyla tanılama ayarlarını oluşturmak ve yönetmek için [Azure 'da platform günlükleri ve ölçümleri toplamak üzere tanılama oluştur](../platform/diagnostic-settings.md) bölümündeki yordamı izleyin. Bir şablonda tanımlanmaları için bir [Kaynak Yöneticisi şablonu kullanarak Azure 'da tanılama ayarı oluşturma](../platform/diagnostic-settings-template.md) konusuna bakın ve bir kaynağın oluşturulduğu sırada tamamını izlemeyi etkinleştirin.


## <a name="monitoring-in-the-azure-portal"></a>Azure portal izleme
 Çoğu Azure kaynağının izleme verilerine, Azure portal kaynağın menüsünden erişebilirsiniz. Bu, standart Azure Izleyici araçlarını kullanarak tek bir kaynağın verilerine erişmenizi sağlar. Bazı Azure Hizmetleri farklı seçenekler sağlayacak, bu nedenle ek bilgi için bu hizmetin belgelerine başvurmanız gerekir. İzlenen tüm kaynaklardaki verileri çözümlemek için **Azure izleyici** menüsünü kullanın. 

Birçok hizmet, **genel bakış** sayfasına, işlem için hızlı bir bakış olarak izleme verileri içerir. Bu, genellikle Azure Izleyici ölçümlerinde depolanan platform ölçümlerinin bir alt kümesini temel alır. Diğer izleme seçenekleri, genellikle hizmetlerin **izleme** bölümünde kullanılabilir. Menü.

![Genel Bakış sayfası](media/monitor-azure-resource/overview-page.png)

## <a name="analyzing-metrics"></a>Ölçümler çözümleniyor
[Ölçüm Gezgini](../platform/metrics-getting-started.md)'ni kullanarak bağıntılar ve eğilimleri belirlemek için tek tek ölçümleri çözümleyin ve birden çok ölçümü ilişkilendirin. Bazı hizmetler, kaynak menüsündeki **ölçümleri** açtığınızda ölçümleriyle çalışmaya yönelik özelleştirilmiş bir deneyim sağlar.

- Ölçüm Gezgini 'ni kullanmanın temelleri için bkz. [Azure Ölçüm Gezgini](../platform/metrics-getting-started.md) kullanmaya başlama.
- Birden çok ölçüm kullanma ve filtre uygulama ve bölme gibi Ölçüm Gezgini 'nin gelişmiş özellikleri için [Azure Ölçüm Gezgini gelişmiş özelliklerine](../platform/metrics-charts.md) bakın.

![Ölçümler](media/monitor-azure-resource/metrics.png)


## <a name="analyzing-logs"></a>Günlükler çözümleniyor

### <a name="activity-log"></a>Etkinlik günlüğü 
Geçerli kaynağa ilk filtresi ayarlanmış Azure portal etkinlik günlüğündeki girdileri görüntüleyin. Etkinlik günlüğünü, oturum sorguları ve çalışma kitaplarında kullanmak üzere bir Log Analytics çalışma alanına kopyalayın. 

- Etkinlik günlüğünü görüntüleme ve çeşitli yöntemler kullanılarak girişleri alma hakkında ayrıntılı bilgi için bkz. [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../platform/activity-log-view.md) .
- Günlüğe kaydedilen belirli olaylar için Azure hizmetinize yönelik belgelere bakın.

![Etkinlik Günlüğü](media/monitor-azure-resource/activity-log.png)

### <a name="azure-monitor-logs"></a>Azure İzleyici Günlükleri
Azure Izleyici günlükleri, güçlü bir sorgu aracıyla analizler için birden çok hizmetten ve diğer veri kaynaklarından günlükleri ve ölçümleri birleştirir. Yukarıda açıklandığı gibi, Azure Izleyici 'deki bir Log Analytics çalışma alanında platform ölçümleri, etkinlik günlüğü ve kaynak günlüklerini toplamak için bir tanılama ayarı oluşturun.

[Log Analytics](../log-query/get-started-portal.md) , tam özellikli bir sorgu dili kullanarak günlük verilerinin gelişmiş analizini gerçekleştirmenize olanak tanıyan, Azure izleyici 'nin güçlü bir özelliği olan [günlük sorgularıyla](../log-query/log-query-overview.md)çalışmanıza olanak sağlar. [Sorgu kapsamı](../log-query/scope.md#query-scope)olarak kaynağı kullanarak günlük sorgularıyla çalışmak Için bir Azure kaynağının **izleme** menüsündeki **günlüklerden** Log Analytics açın. Bu, yalnızca söz konusu kaynak için birden çok tablo genelinde verileri analiz etmenizi sağlar. Tüm kaynaklar için günlüklere erişmek üzere Azure Izleyici menüsünden **günlükleri** kullanın. 

- Bir sorgu yazmak ve sonuçlarla çalışmak için Log Analytics kullanma hakkında yönergeler için bkz. [Azure izleyici 'de Log Analytics kullanmaya başlama](../log-query/get-started-portal.md) .
- Günlük sorgularını yazmak için kullanılan sorgu dilini kullanma hakkında bir öğretici için bkz. [Azure izleyici 'de günlük sorgularıyla çalışmaya başlama](../log-query/get-started-queries.md) .
- Azure Izleyici günlüklerinde kaynak günlüklerinin nasıl toplandığını ve bir sorgudaki bunlara nasıl erişecekleri hakkında bilgi için bkz. Azure [izleyici 'de Log Analytics çalışma alanında Azure Kaynak günlüklerini toplama](../platform/resource-logs-collect-workspace.md) .
- Kaynak günlük verilerinin Azure Izleyici günlüklerinde nasıl yapılandırıldığı hakkında bir açıklama için bkz. [koleksiyon modu](../platform/resource-logs-collect-workspace.md#collection-mode) .
- Azure Izleyici günlüklerinde tablosu hakkındaki ayrıntılar için her bir Azure hizmetine yönelik belgelere bakın.

![Günlükler](media/monitor-azure-resource/logs.png)


## <a name="alerts"></a>Uyarılar
[Uyarılar](../platform/alerts-overview.md) , izleme verilerinizde önemli koşullar bulunduğunda sizi etkili bir şekilde bilgilendirir ve işlem yapabilirsiniz. Uyarı için bir hedef tanımlayan bir uyarı kuralı, bir uyarı oluşturulup oluşturulmayacağını belirten koşullar ve yanıt almak için herhangi bir eylemin oluşturulması gerekir.

Farklı türlerde izleme verileri farklı tür uyarı kuralları için kullanılır.

- [Etkinlik günlüğü uyarısı](../platform/alerts-activity-log.md) -etkinlik günlüğünde belirli ölçütlerle eşleşen bir girdi oluşturulduğunda bir uyarı oluşturun. Bu, örneğin belirli bir kaynak türü oluşturulduğunda veya bir yapılandırma değişikliği başarısız olduğunda bildirim almanıza olanak sağlar.
- [Ölçüm uyarısı](../platform/alerts-metric.md) -ölçüm değeri belirli bir eşiği aştığında uyarı oluştur. Ölçüm uyarıları diğer uyarılardan daha fazla yanıt verebilir ve sorun düzeltildiğinde otomatik olarak çözülebilir.
- [Sorgu uyarısını günlüğe](../platform/alerts-log.md) kaydet-belirli bir koşul bulunursa, düzenli aralıklarla bir günlük sorgusu çalıştırın ve uyarı oluşturun. Bu, birden çok veri kümesi ve üzerinde karmaşık analiz gerçekleştirmenize olanak sağlar.

Uyarıları görüntülemek ve bu kaynakla ilgili uyarı kurallarını yönetmek için bir kaynağın menüsündeki **uyarıları** kullanın. Yalnızca etkinlik günlüğü uyarıları ve ölçüm uyarıları tek tek Azure kaynaklarını hedef olarak kullanır. Günlük sorgusu uyarıları bir Log Analytics çalışma alanını hedef olarak kullanır ve bu çalışma alanında depolanan tüm günlüklere erişebilen bir sorguyu temel alır. Tüm kaynaklarla ilgili uyarıları görüntülemek ve yönetmek için Azure Izleyici menüsünü ve günlük sorgusu uyarı kurallarını yönet ' i kullanın.

- Uyarı kuralları oluşturma hakkında ayrıntılı bilgi için yukarıdaki farklı Uyarı türleri makalelerine bakın.
- Uyarılara yanıtları yönetmenizi sağlayan bir eylem grubu oluşturma hakkında ayrıntılı bilgi için [Azure Portal eylem grupları oluşturma ve yönetme](../platform/action-groups.md) bölümüne bakın.

## <a name="insights-and-solutions"></a>Öngörüler ve çözümler 
Bazı hizmetler, Azure Izleyici 'nin standart özelliklerinden daha fazla araç sağlar. [Çözümler](../insights/solutions.md) , standart Azure izleyici özellikleri üzerinde oluşturulmuş, önceden tanımlanmış izleme mantığı sağlar. [Öngörüler](../insights/insights-overview.md) , Azure izleyici veri platformu ve standart özellikleri üzerinde oluşturulmuş özelleştirilmiş bir izleme deneyimi sağlar.

Bir kaynakta öngörü varsa, kaynak menüsündeki **öngörülere** erişebilirsiniz. Azure Izleyici menüsünden tüm Öngörüler ve çözümlere erişin.

- Öngörüler veya çözümlerin mevcut olup olmadığını öğrenmek için her hizmet için izleme belgelerine bakın.

![Bilgiler](media/monitor-azure-resource/insights.png)

## <a name="next-steps"></a>Sonraki adımlar

* Farklı Azure hizmetlerine yönelik kaynak günlüklerinin ayrıntıları için bkz. [Azure tanılama günlükleri Için desteklenen hizmetler, şemalar ve Kategoriler](../platform/diagnostic-logs-schema.md) .  
