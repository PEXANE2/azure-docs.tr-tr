---
title: Azure Monitör'de gelişmiş sorgular | Microsoft Dokümanlar
description: Bu makalede, Azure Monitor'da sorgu yazmak için Analytics portalını kullanmak için bir öğretici verilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/15/2018
ms.openlocfilehash: 3d228c62cd2d1bcb7f4515cd698186e2ebcbe929
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670296"
---
# <a name="writing-advanced-queries-in-azure-monitor"></a>Azure Monitör'de gelişmiş sorgular yazma

> [!NOTE]
> Bu dersi tamamlamadan önce [Azure Monitor Log Analytics](get-started-portal.md) ve [Getting ile devam](get-started-queries.md) edin'i tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

## <a name="reusing-code-with-let"></a>Let ile kodu yeniden kullanma
Sonuçları `let` bir değişkene atamak ve sorguda daha sonra başvurmak için kullanın:

```Kusto
// get all events that have level 2 (indicates warning level)
let warning_events=
Event
| where EventLevel == 2;
// count the number of warning events per computer
warning_events
| summarize count() by Computer 
```

Değişkenlere sabit değerler de atayabilirsiniz. Bu, sorguyu her çalıştırdığınızda değiştirmeniz gereken alanlar için parametreleri ayarlamak için bir yöntemi destekler. Gerektiğinde bu parametreleri değiştirin. Örneğin, belirli bir zaman penceresinde boş disk alanını ve boş belleği (yüzdelik olarak) hesaplamak için:

```Kusto
let startDate = datetime(2018-08-01T12:55:02);
let endDate = datetime(2018-08-02T13:21:35);
let FreeDiskSpace =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Logical Disk" and CounterName=="Free Megabytes"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
let FreeMemory =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Memory" and CounterName=="Available MBytes Memory"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
union FreeDiskSpace, FreeMemory
```

Bu, sorguyu bir sonraki çalıştırdığınızda bitiş saatinin başlangıcını değiştirmeyi kolaylaştırır.

### <a name="local-functions-and-parameters"></a>Yerel fonksiyonlar ve parametreler
Aynı `let` sorguda kullanılabilecek işlevler oluşturmak için deyimleri kullanın. Örneğin, bir tarih alanı (UTC biçiminde) alan ve standart BIR ABD biçimine dönüştüren bir işlev tanımlayın. 

```Kusto
let utc_to_us_date_format = (t:datetime)
{
  strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
  bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
};
Event 
| where TimeGenerated > ago(1h) 
| extend USTimeGenerated = utc_to_us_date_format(TimeGenerated)
| project TimeGenerated, USTimeGenerated, Source, Computer, EventLevel, EventData 
```

## <a name="print"></a>Yazdır
`print`hesaplama sonucunu gösteren tek bir sütun ve tek satırlı bir tablo döndürecektir. Bu genellikle basit bir hesaplama ya da hesaplama ya da hesaplama gereken durumlarda kullanılır. Örneğin, PST'de geçerli saati bulmak ve EST içeren bir sütun eklemek için:

```Kusto
print nowPst = now()-8h
| extend nowEst = nowPst+3h
```

## <a name="datatable"></a>Datatable
`datatable`bir veri kümesi tanımlamanıza olanak tanır. Bir şema ve bir değer kümesi sağlarsınız ve sonra tabloyu başka bir sorgu öğesine dönüştürürsunuz. Örneğin, RAM kullanım tablosu oluşturmak ve saat başına ortalama değerlerini hesaplamak için:

```Kusto
datatable (TimeGenerated: datetime, usage_percent: double)
[
  "2018-06-02T15:15:46.3418323Z", 15.5,
  "2018-06-02T15:45:43.1561235Z", 20.2,
  "2018-06-02T16:16:49.2354895Z", 17.3,
  "2018-06-02T16:46:44.9813459Z", 45.7,
  "2018-06-02T17:15:41.7895423Z", 10.9,
  "2018-06-02T17:44:23.9813459Z", 24.7,
  "2018-06-02T18:14:59.7283023Z", 22.3,
  "2018-06-02T18:45:12.1895483Z", 25.4
]
| summarize avg(usage_percent) by bin(TimeGenerated, 1h)
```

Veri tablosu yapıları, bir arama tablosu oluştururken de çok yararlıdır. Örneğin, _SecurityEvent_ tablosundan olay lı ibareler gibi tablo verilerini, başka bir yerde listelenen olay türlerine eşlemek için, olay türlerini kullanarak `datatable` bir arama tablosu oluşturun ve bu veri tablosunu _SecurityEvent_ verileriyle birleştirin:

```Kusto
let eventCodes = datatable (EventID: int, EventType:string)
[
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4672, "Access",
    4624, "Account activity",
    4799, "Access management",
    4798, "Access management",
    5059, "Key operation",
    4648, "A logon was attempted using explicit credentials",
    4768, "Access management",
    4662, "Other",
    8002, "Process action",
    4904, "Security event management",
    4905, "Security event management",
];
SecurityEvent
| where TimeGenerated > ago(1h) 
| join kind=leftouter (
  eventCodes
) on EventID
| project TimeGenerated, Account, AccountType, Computer, EventType
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Monitor günlük verileriyle [Kusto sorgu dilini](/azure/kusto/query/) kullanmak için diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Birleştirme](joins.md)
- [Grafikler](charts.md)
