---
title: Azure Monitör'de günlük sorgularına genel bakış | Microsoft Dokümanlar
description: Günlük sorguları ile ilgili sık sorulan soruları yanıtlar ve bunları kullanmaya başlamanızı alır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/19/2019
ms.openlocfilehash: 54a6f875bc33d24d412d2424c634d1019b4af399
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670126"
---
# <a name="overview-of-log-queries-in-azure-monitor"></a>Azure Monitör'de günlük sorgularına genel bakış
Günlük sorguları, [Azure Monitör Günlükleri'nde](../platform/data-platform-logs.md)toplanan verilerin değerinden tam olarak yararlanmanıza yardımcı olur. Güçlü bir sorgu dili, birden çok tablodaki verileri birleştirmenize, büyük veri kümelerini toplamanıza ve en az kodla karmaşık işlemler gerçekleştirmenize olanak tanır. Hemen hemen her soru, destekleyen veriler toplandığı sürece yanıtlanabilir ve analiz yapılabilir ve doğru sorguyu nasıl oluşturabileceğinizi bilirsiniz.

Azure Monitor'daki [öngörüler](../insights/insights-overview.md) ve [çözümler](../insights/solutions-inventory.md) gibi bazı özellikler, günlük verilerini sizi temel sorgulara maruz bırakmadan işlemeye zorlar. Azure Monitor'un diğer özelliklerinden tam olarak yararlanmak için, sorguların nasıl oluşturulduğunu ve Azure Monitör Günlükleri'ndeki verileri etkileşimli olarak analiz etmek için bunları nasıl kullanabileceğinizi anlamanız gerekir.

Bu makaleyi Azure Monitor'da günlük sorguları hakkında bilgi edinmek için bir başlangıç noktası olarak kullanın. Sık sorulan soruları yanıtlar ve daha fazla ayrıntı ve ders sağlayan diğer belgelere bağlantılar sağlar.

## <a name="how-can-i-learn-how-to-write-queries"></a>Sorgu yazmayı nasıl öğrenebilirim?
Bir şeylerin içine atlamak istiyorsanız, aşağıdaki öğreticiler ile başlayabilirsiniz:

- [Azure Monitor'da Log Analytics ile başlayın.](get-started-portal.md)
- [Azure Monitor'da günlük sorgularıyla başlayın.](get-started-queries.md)

Temel bilgileri indirdikten sonra, aşağıdakilerden başlayarak kendi verilerinizi veya demo ortamımızdan verilerinizi kullanarak birden fazla dersten yürüyün: 

- [Azure Monitor günlük sorgularında dizeleri ile çalışma](string-operations.md)
 
## <a name="what-language-do-log-queries-use"></a>Günlük sorguları hangi dili kullanır?
Azure Monitor Günlükleri [Azure Veri Gezgini'ni](/azure/data-explorer)temel alır ve günlük sorguları aynı Kusto sorgu dili (KQL) kullanılarak yazılır. Bu, okunması ve yazılması kolay olması için tasarlanmış zengin bir dildir ve en az yol gösterici olarak kullanmaya başlayabilmelisiniz.

KQL ile ilgili tam belgeler ve kullanılabilen farklı işlevler hakkında başvuru için [Azure Veri Gezgini KQL belgelerine](/azure/kusto/query) bakın.<br>
Bkz. Azure Monitör Günlükleri'ndeki verileri kullanarak dilin hızlı bir şekilde gözden geçirilen kısmı için [Azure Monitor'daki günlük sorgularına başlayın.](get-started-queries.md)
Azure Monitor tarafından kullanılan KQL sürümündeki küçük farklılıklar için [Azure Monitor günlük sorgu dil farklılıklarına](data-explorer-difference.md) bakın.

## <a name="what-data-is-available-to-log-queries"></a>Sorguları günlüğe kaydetmek için hangi veriler kullanılabilir?
Azure Monitör Günlükleri'nde toplanan tüm veriler günlük sorgularını almak ve analiz etmek için kullanılabilir. Farklı veri kaynakları verilerini farklı tablolara yazar, ancak birden çok kaynak taki verileri çözümlemek için tek bir sorguya birden çok tablo ekleyebilirsiniz. Bir sorgu oluşturduğunuzda, hangi tabloların aradığınız verilere sahip olduğunu belirleyerek başlarsınız, bu nedenle Azure Monitör Günlükleri'ndeki verilerin nasıl yapılandırıldığına dair en azından temel bir anlayışa sahip olmalısınız.

Azure Monitör Günlükleri'ni dolduran farklı veri kaynaklarının listesi için [Azure Monitör Günlüklerinin Kaynakları'na](../platform/data-platform-logs.md#sources-of-azure-monitor-logs)bakın.<br>
Verilerin nasıl yapılandırıldığına ilişkin bir açıklama için [Azure Monitör Günlüklerinin Yapısı'na](logs-structure.md) bakın.

## <a name="what-does-a-log-query-look-like"></a>Günlük sorgusu nasıl görünür?
Bir sorgu, bu tablodaki tüm kayıtları almak için tek bir tablo adı kadar basit olabilir:

```Kusto
Syslog
```

Veya belirli kayıtları filtreleyebilir, özetleyebilir ve sonuçları bir grafikte görselleştirebilir:

```
SecurityEvent
| where TimeGenerated > ago(7d)
| where EventID == 4625
| summarize count() by Computer, bin(TimeGenerated, 1h)
| render timechart 
```

Daha karmaşık çözümleme için, sonuçları birlikte çözümlemek için birbirleştirme kullanarak birden çok tablodan veri alabilirsiniz.

```Kusto
app("ContosoRetailWeb").requests
| summarize count() by bin(timestamp,1hr)
| join kind= inner (Perf
    | summarize avg(CounterValue) 
      by bin(TimeGenerated,1hr))
on $left.timestamp == $right.TimeGenerated
```
KQL aşina olmasa bile, en azından bu sorgular tarafından kullanılan temel mantık anlamaya gerekir. Bir tablonun adıyla başlarlar ve sonra bu verileri filtrelemek ve işlemek için birden çok komut eklerler. Bir sorgu herhangi bir sayıda komut kullanabilir ve mevcut farklı KQL komutlarına aşina olduğunuzda daha karmaşık sorgular yazabilirsiniz.

Bkz. Dili ve ortak işlevleri tanıtan günlük sorguları hakkında bir öğretici için [Azure Monitor'daki günlük sorgularıyla başlayın.](get-started-queries.md)<br>


## <a name="what-is-log-analytics"></a>Log Analytics nedir?
Log Analytics, azure portalında günlük sorguları yazmak ve sonuçlarını etkileşimli olarak analiz etmek için birincil araçtır. Azure Monitor'un başka bir yerinde günlük sorgusu kullanılsa bile, genellikle Log Analytics'i kullanarak sorguyu ilk olarak yazar ve sınarsınız.

Azure portalında Çeşitli yerlerden Log Analytics'i başlatabilirsiniz. Log Analytics tarafından kullanılabilen verilerin kapsamı, nasıl başlattığınıza göre belirlenir. Daha fazla ayrıntı için [Sorgu Kapsamı'na](scope.md) bakın.

- **Azure Monitörü** menüsünden günlükler veya **Log Analytics çalışma alanları** menüsünden **Günlükler'i** seçin.
- Uygulama Öngörüleri uygulamasının **Genel Bakış** sayfasından **Analytics'i** seçin.
- Azure kaynağının menüsünden **Günlükler'i** seçin.

![Log Analytics](media/log-query-overview/log-analytics.png)

Log Analytics'in bazı özelliklerini tanıtan öğretici bir walkthrough için [Azure Monitor'da Log Analytics ile başlayın.](get-started-portal.md)

## <a name="where-else-are-log-queries-used"></a>Günlük sorguları başka nerede kullanılır?
Günlük sorguları ve bunların Log Analytics'teki sonuçlarıyla etkileşimli olarak çalışmanın yanı sıra, Azure Monitor'da sorguları kullanacağınız alanlar şunlardır:

- **Uyarı kuralları.** [Uyarı kuralları,](../platform/alerts-overview.md) çalışma alanınızdaki verilerden gelen sorunları proaktif olarak tanımlar.  Her uyarı kuralı, düzenli aralıklarla otomatik olarak çalıştırılabilen bir günlük araması temel alınarak uygulanır.  Sonuçlar, bir uyarı oluşturulması gerekip gerekmeden belirlenip oluşturulmaması gerektiğini belirlemek için denetlenir.
- **Pano.** Herhangi bir sorgunun sonuçlarını, günlük ve metrik verileri birlikte görselleştirmenize ve isteğe bağlı olarak diğer Azure kullanıcılarıyla paylaşmanıza olanak tanıyan bir [Azure panosuna](../learn/tutorial-logs-dashboards.md) sabitleyebilirsiniz.
- **Görünümler.**  [View Designer](../platform/view-designer.md)ile kullanıcı panolarına eklenecek verilerin görselleştirmelerini oluşturabilirsiniz.  Günlük sorguları, her görünümde [kutucuklar](../platform/view-designer-tiles.md) ve [görselleştirme bölümleri](../platform/view-designer-parts.md) tarafından kullanılan verileri sağlar.  
- **Ihracat.**  Azure Monitor'dan Excel veya [Power BI'ye](../platform/powerbi.md)günlük verileri aktardığınızda, dışa aktaracak verileri tanımlamak için bir günlük sorgusu oluşturursunuz.
- **Powershell.** Azure Monitor'dan günlük verilerini almak için [Get-AzOperationalInsightsSearchResults](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult) kullanan bir komut satırından veya Azure Otomasyon çalışma kitabından bir PowerShell komut dosyası çalıştırabilirsiniz.  Bu cmdlet almak için verileri belirlemek için bir sorgu gerektirir.
- **Azure Monitor, API'yi kaydeder.**  [Azure Monitör Günlükleri API'si,](https://dev.loganalytics.io) herhangi bir REST API istemcisinin çalışma alanından günlük verilerini almasına olanak tanır.  API isteği, alınacak verileri belirlemek için Azure Monitor'a karşı çalıştırılabilen bir sorgu içerir.


## <a name="next-steps"></a>Sonraki adımlar
- [Azure portalında Log Analytics'i kullanma yla ilgili bir öğreticiye yürüyün.](get-started-portal.md)
- [Sorgu yazma konusunda](get-started-queries.md)bir öğretici ile yürüyün.
