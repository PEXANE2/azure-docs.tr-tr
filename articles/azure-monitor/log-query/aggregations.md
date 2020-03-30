---
title: Azure Monitor günlük sorgularında toplamalar| Microsoft Dokümanlar
description: Verilerinizi çözümlemek için yararlı yollar sunan Azure Monitor günlük sorgularında toplama işlevlerini açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: d164c53e7e2be55f3cede389901a256ba388808d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670313"
---
# <a name="aggregations-in-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularında toplamalar

> [!NOTE]
> [Analytics portalı ile başlayın](get-started-portal.md) ve bu dersi tamamlamadan önce [sorgularla başlayın'ı](get-started-queries.md) tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, verilerinizi çözümlemek için yararlı yollar sunan Azure Monitor günlük sorgularında toplama işlevleri açıklanmaktadır. Bu işlevlerin tümü, giriş tablosunun `summarize` toplu sonuçlarıyla bir tablo üreten işleçle çalışır.

## <a name="counts"></a>Sayar

### <a name="count"></a>count
Herhangi bir filtre uygulandıktan sonra sonuç kümesindeki satır sayısını sayın. Aşağıdaki örnek, Son 30 dakikadaki _Perf_ tablosundaki toplam satır sayısını döndürür. Sonuç, belirli bir ad atadığınız sürece *count_* adlı bir sütunda döndürülür:


```Kusto
Perf
| where TimeGenerated > ago(30m) 
| summarize count()
```

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| summarize num_of_records=count() 
```

Zaman çizelgesi görselleştirmesi zaman içinde bir eğilim görmek için yararlı olabilir:

```Kusto
Perf 
| where TimeGenerated > ago(30m) 
| summarize count() by bin(TimeGenerated, 5m)
| render timechart
```

Bu örnekten elde edilen çıktı, perf kayıt sayısı eğilim çizgisini 5 dakikalık aralıklarla gösterir:

![Sayma eğilimi](media/aggregations/count-trend.png)


### <a name="dcount-dcountif"></a>dcount, dcountif
Belirli `dcount` `dcountif` bir sütundaki farklı değerleri kullanın ve sayın. Aşağıdaki sorgu, son bir saat içinde kaç farklı bilgisayarın sinyal atışını gönderdiğini değerlendirir:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcount(Computer)
```

Yalnızca kalp atışlarını gönderen Linux bilgisayarlarını saymak için şunları kullanın: `dcountif`

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize dcountif(Computer, OSType=="Linux")
```

### <a name="evaluating-subgroups"></a>Alt grupların değerlendirilmesi
Verilerinizdeki alt gruplarda sayım veya diğer toplamaları gerçekleştirmek `by` için anahtar kelimeyi kullanın. Örneğin, her ülkede/bölgede kalp atışları gönderen farklı Linux bilgisayarlarının sayısını saymak için:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry
```

|RemoteIPCountry  | distinct_computers  |
------------------|---------------------|
|Amerika Birleşik Devletleri    | 19                  |
|Kanada           | 3                   |
|İrlanda          | 0                   |
|Birleşik Krallık   | 0                   |
|Hollanda      | 2                   |


Verilerinizin daha küçük alt gruplarını analiz etmek `by` için bölüme ek sütun adları ekleyin. Örneğin, OSType'a göre her ülkeden/bölgeden farklı bilgisayarları saymak isteyebilirsiniz:

```Kusto
Heartbeat 
| where TimeGenerated > ago(1h) 
| summarize distinct_computers=dcountif(Computer, OSType=="Linux") by RemoteIPCountry, OSType
```

## <a name="percentiles-and-variance"></a>Yüzdelik dilimleri ve varyans
Sayısal değerleri değerlendirirken, yaygın bir uygulama `summarize avg(expression)`kullanarak ortalama. Ortalamalar, yalnızca birkaç vakayı karakterize eden aşırı değerlerden etkilenir. Bu sorunu gidermek için, daha az `median` hassas `variance`işlevleri kullanabilirsiniz veya .

### <a name="percentile"></a>Yüzdebirlik
Ortanca değeri bulmak için, `percentile` yüzdelik belirterek değeri olan işlevi kullanın:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 50) by Computer
```

Ayrıca, her biri için toplu bir sonuç elde etmek için farklı yüzdeler belirtebilirsiniz:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize percentiles(CounterValue, 25, 50, 75, 90) by Computer
```

Bu, bazı bilgisayar CPU'larının benzer ortanca değerlere sahip olduğunu gösterebilir, ancak bazıları ortanca etrafında sabit olsa da, diğer bilgisayarlar çok daha düşük ve daha yüksek CPU değerleri bildirmiş, yani ani artışlar yaşamıştır.

### <a name="variance"></a>Varyans
Bir değerin varyansını doğrudan değerlendirmek için standart sapma ve varyans yöntemlerini kullanın:

```Kusto
Perf
| where TimeGenerated > ago(30m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), variance(CounterValue) by Computer
```

CPU kullanımının kararlılığını analiz etmenin iyi bir yolu, stdev'i ortanca hesaplamayla birleştirmektir:

```Kusto
Perf
| where TimeGenerated > ago(130m) 
| where CounterName == "% Processor Time" and InstanceName == "_Total" 
| summarize stdev(CounterValue), percentiles(CounterValue, 50) by Computer
```

Azure Monitor günlük verileriyle [Kusto sorgu dilini](/azure/kusto/query/) kullanmak için diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleştirme](joins.md)
- [Grafikler](charts.md)
