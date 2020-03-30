---
title: Azure Monitor günlük sorgularından grafik ve diyagram oluşturma | Microsoft Dokümanlar
description: Günlük verilerinizi farklı şekillerde görüntülemek için Azure Monitor'daki çeşitli görselleştirmeleri açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: 8a515f01bfa9f8ec579c51b806c997d79b629250
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670330"
---
# <a name="creating-charts-and-diagrams-from-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularından grafikler ve diyagramlar oluşturma

> [!NOTE]
> Bu dersi tamamlamadan önce [Azure Monitor günlük sorgularında Gelişmiş toplamaları](advanced-aggregations.md) tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, günlük verilerinizi farklı şekillerde görüntülemek için Azure Monitor'daki çeşitli görselleştirmeler açıklanmaktadır.

## <a name="charting-the-results"></a>Sonuçları grafikleme
Son bir saat içinde işletim sistemi başına kaç bilgisayar olduğunu gözden geçirerek başlayın:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count(Computer) by OSType  
```

Varsayılan olarak, sonuçlar tablo olarak görüntülenir:

![Tablo](media/charts/table-display.png)

Daha iyi bir görünüm elde etmek için **Grafik'i**seçin ve sonuçları görselleştirmek için **Pasta** seçeneğini seçin:

![Pasta grafiği](media/charts/charts-and-diagrams-pie.png)


## <a name="timecharts"></a>Zaman çizelgeleri
İşlemci saatinin ortalama, 50 ve 95'inci yüzdelik dilimini 1 saatlik kutularda gösterin. Sorgu birden çok seri oluşturur ve daha sonra zaman grafiğinde hangi diziyi gösterebileceğinizi seçebilirsiniz:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
```

**Çizgi** grafik görüntüleme seçeneğini seçin:

![Çizgi grafik](media/charts/charts-and-diagrams-multiSeries.png)

### <a name="reference-line"></a>Referans satırı

Referans satırı, metrik belirli bir eşiği aşarsa kolayca belirlemenize yardımcı olabilir. Grafiğe satır eklemek için, veri kümesini sabit bir sütunla genişletin:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
| extend Threshold = 20
```

![Referans satırı](media/charts/charts-and-diagrams-multiSeriesThreshold.png)

## <a name="multiple-dimensions"></a>Birden çok boyut
Sonuçlarda birden `by` çok `summarize` satır oluşturma yan tümcesindeki birden çok ifade, her değer birleşimi için bir tane.

```Kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize count() by tostring(EventID), AccountType, bin(TimeGenerated, 1h)
```

Sonuçları bir grafik olarak gördüğünüzde, yan tümcedeki ilk sütunu `by` kullanır. Aşağıdaki _örnekte, EventID'yi_ kullanarak yığılmış bir sütun grafiği gösterilmektedir. Boyutlar türde `string` olmalıdır, bu nedenle bu örnekte _EventID_ dize için döküm ediliyor. 

![Çubuk grafik EventID](media/charts/charts-and-diagrams-multiDimension1.png)

Sütun adı ile açılır bırakma seçerek arasında geçiş yapabilirsiniz. 

![Çubuk grafik AccountType](media/charts/charts-and-diagrams-multiDimension2.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Monitor günlük verileriyle [Kusto sorgu dilini](/azure/kusto/query/) kullanmak için diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleştirme](joins.md)
