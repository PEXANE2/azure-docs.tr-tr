---
title: Azure Monitor günlük sorgularında tarih saati değerleriyle çalışma| Microsoft Dokümanlar
description: Azure Monitor günlük sorgularında tarih ve saat verileriyle nasıl çalışılabildiğini açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: ea7c98a1b5b4059c5fea0cf1e8ea2ff5ef08d9d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655387"
---
# <a name="working-with-date-time-values-in-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularında tarih saati değerleriyle çalışma

> [!NOTE]
> [Analytics portalı ile başlayın](get-started-portal.md) ve bu dersi tamamlamadan önce [sorgularla başlayın'ı](get-started-queries.md) tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, Azure Monitor günlük sorgularında tarih ve saat verileriyle nasıl çalışılalı şiş.


## <a name="date-time-basics"></a>Tarih saati temelleri
Kusto sorgu dilitarih ve saatlerle ilişkili iki ana veri türüvardır: tarih ve zaman dilimi. Tüm tarihler UTC cinsinden ifade edilir. Birden çok datetime biçimi desteklenirken, ISO8601 biçimi tercih edilir. 

Zaman dilimleri ondalık olarak ifade edilir ve ardından bir zaman birimi:

|Steno   | zaman birimi    |
|:---|:---|
|d           | gün          |
|h           | saat         |
|m           | dakika       |
|s           | saniye       |
|Bayan          | milisaniye  |
|Microsecond | Microsecond  |
|Kene        | Içerir   |

Datetimes `todatetime` işleci kullanarak bir dize döküm tarafından oluşturulabilir. Örneğin, belirli bir zaman diliminde gönderilen VM sinyallerini `between` gözden geçirmek için, bir zaman aralığı belirtmek için işleci kullanın.

```Kusto
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Başka bir yaygın senaryo, bir datetime'ı şimdiki zamanile karşılaştırmaktır. Örneğin, son iki dakikadaki tüm sinyallerini görmek için `now` işleci iki dakikayı temsil eden bir zaman açıklığıyla birlikte kullanabilirsiniz:

```Kusto
Heartbeat
| where TimeGenerated > now() - 2m
```

Bu işlev için bir kısayol da kullanılabilir:
```Kusto
Heartbeat
| where TimeGenerated > now(-2m)
```

En kısa ve en okunabilir yöntem `ago` olsa operatör kullanıyor:
```Kusto
Heartbeat
| where TimeGenerated > ago(2m)
```

Başlangıç ve bitiş saatini bilmek yerine, başlangıç saatini ve süresini bildiğinizi varsayalım. Sorguyu aşağıdaki gibi yeniden yazabilirsiniz:

```Kusto
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Zaman birimlerini dönüştürme
Varsayılan değer dışında bir zaman biriminde bir datetime veya timepan ifade etmek isteyebilirsiniz. Örneğin, son 30 dakikadaki hata olaylarını gözden geçiriyorsanız ve olayın ne kadar zaman önce gerçekleştiğini gösteren hesaplanmış bir sütuna ihtiyacınız varsa:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

Sütun `timeAgo` da şu değerlere sahiptir: "00:09:31.5118992", yani hh:mm:ss.fffffff olarak biçimlendirilirler. Bu değerleri başlangıç saatinden `numver` beri dakikalara biçimlendirmek istiyorsanız, bu değeri "1 dakika" olarak bölün:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Zaman aralıklarına göre toplama ve kovalama
Başka bir yaygın senaryo belirli bir zaman diliminde belirli bir zaman dilimi içinde istatistik elde etmek için ihtiyaçtır. Bu senaryo için, bir `bin` işleç özet yan tümcesinin bir parçası olarak kullanılabilir.

Son yarım saat içinde her 5 dakikada bir meydana gelen olay sayısını almak için aşağıdaki sorguyu kullanın:

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Bu sorgu aşağıdaki tabloyu üretir:  

|Zaman Oluşturuldu(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

Sonuç kovaları oluşturmak için başka bir yolu `startofday`gibi işlevleri kullanmaktır:

```Kusto
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

Bu sorgu aşağıdaki sonuçları üretir:

|timestamp|count_|
|--|--|
|2018-07-28T00:00:00.000|7,136|
|2018-07-29T00:00:00.000|12,315|
|2018-07-30T00:00:00.000|16,847|
|2018-07-31T00:00:00.000|12,616|
|2018-08-01T00:00:00.000|5,416|


## <a name="time-zones"></a>Saat dilimleri
Tüm tarih zamanı değerleri UTC'de ifade edildiklerinden, bu değerleri yerel saat dilimine dönüştürmek genellikle yararlıdır. Örneğin, UTC'yi PST kez'e dönüştürmek için bu hesaplamayı kullanın:

```Kusto
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>İlgili işlevler

| Kategori | İşlev |
|:---|:---|
| Veri türlerini dönüştürme | [todatetime](/azure/kusto/query/todatetimefunction)  [totimepan](/azure/kusto/query/totimespanfunction)  |
| Çöp kutusu boyutuna yuvarlatma değeri | [bin](/azure/kusto/query/binfunction) |
| Belirli bir tarih veya saat alma | [önce](/azure/kusto/query/agofunction) [şimdi](/azure/kusto/query/nowfunction)   |
| Değerin bir parçasını alma | [datetime_part](/azure/kusto/query/datetime-partfunction) [dayofyear](/azure/kusto/query/dayofyearfunction) [amonth](/azure/kusto/query/getmonthfunction) [monthofyear](/azure/kusto/query/monthofyearfunction) [alayimsene](/azure/kusto/query/getyearfunction) [gün güngünhafta](/azure/kusto/query/dayofmonthfunction) [haftanın](/azure/kusto/query/dayofweekfunction) [haftası](/azure/kusto/query/weekofyearfunction) |
| Göreli tarih değeri alma  | [gün](/azure/kusto/query/endofdayfunction) [sonu hafta sonu](/azure/kusto/query/endofweekfunction) ay [sonu](/azure/kusto/query/endofmonthfunction) [yıl sonu](/azure/kusto/query/endofyearfunction) [başlangıçgünü](/azure/kusto/query/startofdayfunction) [başlangıçhafta](/azure/kusto/query/startofweekfunction) [başlangıcıay](/azure/kusto/query/startofmonthfunction) [başlangıcı](/azure/kusto/query/startofyearfunction) |

## <a name="next-steps"></a>Sonraki adımlar
Azure Monitor günlük verileriyle [Kusto sorgu dilini](/azure/kusto/query/) kullanmak için diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleştirme](joins.md)
- [Grafikler](charts.md)
