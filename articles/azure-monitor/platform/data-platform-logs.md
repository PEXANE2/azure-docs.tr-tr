---
title: Azure Izleyici 'de Günlükler | Microsoft Docs
description: İzleme verilerinin gelişmiş analizine yönelik olarak kullanılan Azure Izleyici 'de günlükleri açıklar.
documentationcenter: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 09/09/2020
ms.author: bwren
ms.openlocfilehash: e08c649b9a1d7e8b909a413ee435fce30a8d7e48
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90032799"
---
# <a name="azure-monitor-logs-overview"></a>Azure Izleyici günlüklerine genel bakış
Azure Izleyici günlükleri, çeşitli kaynaklardan günlük verilerini toplayıp düzenleyen ve gelişmiş bir sorgu dili kullanarak analiz için kullanılabilir hale getiren bir Azure Izleyici özelliğidir. Farklı kaynaklardaki veriler tek bir çalışma alanında birleştirilebilir ve bu tür görevler ve eğilim, uyarı ve görselleştirmeyi gerçekleştirmek için birlikte analiz edilebilir.

## <a name="relationship-to-azure-monitor-metrics"></a>Azure Izleyici ölçümleriyle ilişki
Azure Izleyici ölçümleri sayısal verileri bir zaman serisi veritabanında depolar, bu da bu verilerin Azure Izleyici günlüklerinden daha hafif olmasını sağlar ve sorunların uyarı ve hızlı bir şekilde algılanması için özellikle yararlı hale gelir. Yalnızca belirli bir yapıda sayısal veriler depolayabilse de, Günlükler her biri kendi yapısıyla çeşitli farklı veri türlerini depolayabilse de ölçümler. Ayrıca, ölçüm verilerinin çözümlenmesi için kullanılamayan günlük sorgularını kullanarak Günlükler verilerinde karmaşık analizler gerçekleştirebilirsiniz.

Sayısal veriler genellikle ölçümlere ek olarak veri kaynaklarından günlüklere gönderilir. Günlüklerde bu verilerin toplanması ve bu verileri saklamak için ek bir ücret söz konusu olduğunda, ölçüm verilerini günlük sorgularına dahil etmenize ve diğer izleme verileriyle çözümlemenize olanak tanır.

## <a name="relationship-to-azure-data-explorer"></a>Azure Veri Gezgini ile ilişki
Azure Izleyici günlükleri Azure Veri Gezgini tabanlıdır. Log Analytics çalışma alanı kabaca Azure Veri Gezgini 'deki bir veritabanının eşdeğeridir, tablolar aynı şekilde yapılandırılmıştır ve her ikisi de aynı kusto sorgu dilini (KQL) kullanır. Azure portal Azure Izleyici sorgularıyla çalışmak için Log Analytics kullanma deneyimi, Azure Veri Gezgini Web Kullanıcı arabirimini kullanarak deneyimle benzerdir. [Azure Veri Gezgini sorgusunda bir Log Analytics çalışma alanından da veri](/azure/data-explorer/query-monitor-data)ekleyebilirsiniz. 


## <a name="structure-of-data"></a>Veri yapısı
Azure Izleyici günlükleri tarafından toplanan veriler, her biri belirli bir kaynaktaki verilerin depolandığı birden çok tablo içeren bir [Log Analytics çalışma alanında](./design-logs-deployment.md) depolanır. Çalışma alanı verilerin coğrafi konumunu, hangi kullanıcıların verilere erişebileceğini tanımlayan erişim haklarını ve fiyatlandırma katmanı ve veri saklama gibi yapılandırma ayarlarını tanımlar. Gereksinimlerinize bağlı olarak, tüm izleme verileriniz için tek bir çalışma alanı kullanabilir veya birden çok çalışma alanı oluşturabilirsiniz. Bkz. birden çok çalışma alanı oluşturma konusunda dikkat edilmesi gereken [Azure Izleyici günlükleri dağıtımınızı tasarlama](design-logs-deployment.md) .

Her çalışma alanı birden fazla veri satırı içeren ayrı sütunlarda düzenlenmiş birden çok tablo içerir. Her tablo, veri kaynağı tarafından belirtilen veri satırları tarafından paylaşılan benzersiz bir sütun kümesi tarafından tanımlanır. 

[![Azure Izleyici günlükleri yapısı](media/data-platform-logs/logs-structure.png)](media/data-platform-logs/logs-structure.png#lightbox)


Application Insights günlük verileri Azure Izleyici günlüklerinde de depolanır, ancak uygulamanızın nasıl yapılandırıldığına bağlı olarak farklı bir şekilde depolanır. Çalışma alanı tabanlı bir uygulama için veriler, uygulama istekleri, özel durumlar ve sayfa görünümleri gibi verileri tutmak üzere standart bir tablo kümesindeki Log Analytics çalışma alanında depolanır. Birden çok uygulama aynı çalışma alanını kullanabilir. Klasik bir uygulama için, veriler Log Analytics çalışma alanında depolanmaz. Aynı sorgu dilini kullanır ve Azure portal aynı Log Analytics aracını kullanarak sorgular oluşturup çalıştırırsınız. Klasik uygulamalar için veriler birbirinden ayrı olarak depolanır. Tablo ve sütun adları farklı olsa da, genel yapısı çalışma alanı tabanlı uygulamalarla aynıdır. Bu ikisinin ayrıntılı karşılaştırması için [çalışma alanı tabanlı kaynak değişikliklerine](../app/apm-tables.md) bakın.


> [!NOTE]
> Application Insights deneyiminde Application Insights klasik kaynak sorguları, çalışma kitapları ve günlük tabanlı uyarılarınız için de tam geriye doğru uyumluluk sağlıyoruz. [Yeni çalışma alanı tabanlı tablo yapısına/şemasına](../app/apm-tables.md) karşı sorgu/görünüm için öncelikle Log Analytics çalışma alanınıza gitmeniz gerekir. Önizleme sırasında, Application Insights bölmeleri içinden **günlüklerin** seçilmesi, klasik Application Insights sorgu deneyimine erişmenizi sağlayacaktır. Daha fazla ayrıntı için bkz. [sorgu kapsamı](../log-query/scope.md) .


[![Application Insights için Azure Izleyici günlük yapısı](media/data-platform-logs/logs-structure-ai.png)](media/data-platform-logs/logs-structure-ai.png#lightbox)

## <a name="log-queries"></a>Günlük sorguları
Veriler, verileri işlemek ve sonuçları döndürmek için salt okunurdur bir istek olan [günlük sorgusu](../log-query/log-query-overview.md) kullanılarak bir Log Analytics çalışma alanından alınır. Günlük sorguları, Azure Veri Gezgini tarafından kullanılan sorgu dili olan [kusto sorgu dili 'nde (KQL)](/azure/data-explorer/kusto/query/)yazılır. Günlük sorgularını düzenlemek ve çalıştırmak ve sonuçları etkileşimli olarak çözümlemek için Azure portal bir araç olan Log Analytics kullanın. Daha sonra, Azure Izleyici 'de günlük sorgu uyarıları ve çalışma kitapları gibi diğer özellikleri desteklemek için oluşturduğunuz sorguları kullanabilirsiniz.


## <a name="sources-of-data-for-azure-monitor-logs"></a>Azure Izleyici günlükleri için veri kaynakları
Azure Izleyici, Azure Izleyici ve sanal makinelerde çalışan aracılarda bulunan kaynaklar da dahil olmak üzere çeşitli kaynaklardan günlük verilerini toplar. Log Analytics çalışma alanına veri gönderen veri kaynaklarının tamamen listesi için bkz. [Azure izleyici tarafından Izlenen nedir?](../monitor-reference.md) .



## <a name="next-steps"></a>Sonraki adımlar

- Log Analytics çalışma alanından verileri almak ve analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.
- [Azure izleyici 'de ölçümler](data-platform-metrics.md)hakkında bilgi edinin.
- Azure 'daki farklı kaynaklar için [kullanılabilen izleme verileri](data-sources.md) hakkında bilgi edinin.

