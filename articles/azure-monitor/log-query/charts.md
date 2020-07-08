---
title: Azure Izleyici günlük sorgularından grafikler ve diyagramlar oluşturma | Microsoft Docs
description: Günlük verilerinizi farklı yollarla görüntülemek için Azure Izleyici 'de çeşitli görselleştirmeler açıklanmıştır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: 8a515f01bfa9f8ec579c51b806c997d79b629250
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77670330"
---
# <a name="creating-charts-and-diagrams-from-azure-monitor-log-queries"></a>Azure Izleyici günlük sorgularından grafikler ve diyagramlar oluşturma

> [!NOTE]
> Bu dersi tamamlamadan önce [Azure izleyici günlük sorgularında gelişmiş toplamaları](advanced-aggregations.md) tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, Azure Izleyici 'de günlük verilerinizi farklı yollarla görüntülemek için çeşitli görselleştirmeler açıklanmaktadır.

## <a name="charting-the-results"></a>Sonuçları grafikleme
Son bir saat içinde işletim sistemi başına kaç bilgisayar olduğunu inceleyerek başlayın:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count(Computer) by OSType  
```

Varsayılan olarak, sonuçlar tablo olarak görüntülenir:

![Tablo](media/charts/table-display.png)

Daha iyi bir görünüm elde etmek için **grafik**' i seçin ve sonuçları görselleştirmek için **pasta** seçeneğini belirleyin:

![Pasta grafiği](media/charts/charts-and-diagrams-pie.png)


## <a name="timecharts"></a>Timegrafiklerim
1 saatlik depo gözlerinde ortalama, 50. ve 6. yüzdebirlik değeri için işlemci zamanı gösterir. Sorgu birden çok seri oluşturur ve ardından zaman grafiğinde hangi serinin gösterileceğini seçebilirsiniz:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
```

**Çizgi** grafik görüntüleme seçeneğini belirleyin:

![Çizgi grafik](media/charts/charts-and-diagrams-multiSeries.png)

### <a name="reference-line"></a>Başvuru çizgisi

Başvuru satırı, ölçümün belirli bir eşiği aşmadığını kolayca belirlemenize yardımcı olabilir. Grafiğe satır eklemek için, veri kümesini sabit bir sütun ile genişletin:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
| extend Threshold = 20
```

![Başvuru çizgisi](media/charts/charts-and-diagrams-multiSeriesThreshold.png)

## <a name="multiple-dimensions"></a>Birden çok boyut
`by` `summarize` Her değer birleşimi için bir tane olmak üzere sonuçlarda birden çok satır oluşturma yan tümcesindeki birden çok ifade.

```Kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize count() by tostring(EventID), AccountType, bin(TimeGenerated, 1h)
```

Sonuçları bir grafik olarak görüntülediğinizde, yan tümcesindeki ilk sütunu kullanır `by` . Aşağıdaki örnek, _EventID_ kullanılarak yığılmış bir sütun grafiği gösterir. Boyutların türünde olması gerekir `string` , bu nedenle bu örnekte _EventID_ dizeye yayınlanmakta. 

![Çubuk grafik olay kimliği](media/charts/charts-and-diagrams-multiDimension1.png)

Sütun adıyla açılan menüyü seçerek arasında geçiş yapabilirsiniz. 

![Çubuk grafik AccountType](media/charts/charts-and-diagrams-multiDimension2.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Izleyici günlük verileriyle [kusto sorgu dilini](/azure/kusto/query/) kullanmaya yönelik diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleştirme](joins.md)
