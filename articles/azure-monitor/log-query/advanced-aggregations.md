---
title: Azure Izleyici günlük sorgularıyla gelişmiş toplamalar | Microsoft Docs
description: Azure Izleyici günlük sorgularının kullanabildiği daha gelişmiş toplama seçeneklerinden bazılarını açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: dba058dce09e958a2ae769d927a5569fb3e42113
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87324565"
---
# <a name="advanced-aggregations-in-azure-monitor-log-queries"></a>Azure Izleyici günlük sorgularında gelişmiş toplamalar

> [!NOTE]
> Bu dersi tamamlamadan önce [Azure izleyici sorgularında toplamaları](./aggregations.md) tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, Azure Izleyici sorguları için kullanabileceğiniz daha gelişmiş toplama seçeneklerinden bazıları açıklanmaktadır.

## <a name="generating-lists-and-sets"></a>Listeler ve kümeler oluşturma
`makelist`Verileri, belirli bir sütundaki değerlerin sırasına göre özet yapmak için kullanabilirsiniz. Örneğin, makinelerinizde en sık kullanılan sıra olaylarını incelemek isteyebilirsiniz. Temel olarak verileri her bir makinedeki EventID 'Ler sırasına göre özetleyebilirsiniz. 

```Kusto
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makelist(EventID) by Computer
```

|Bilgisayar|list_EventID|
|---|---|
| Bilgisayar1 | [704, 701, 1501, 1500, 1085, 704, 704, 701] |
| BİLGİSAYAR2 | [326.105.302.301.300.102] |
| ... | ... |

`makelist`verilerin kendisine geçirildiği sırada bir liste oluşturur. Olayları en eskiden en eskiye sıralamak için `asc` yerine Order ifadesinde kullanın `desc` . 

Yalnızca benzersiz değerlerin bir listesini oluşturmak da yararlıdır. Bu bir _küme_ olarak adlandırılır ve şu şekilde oluşturulabilir `makeset` :

```Kusto
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makeset(EventID) by Computer
```

|Bilgisayar|list_EventID|
|---|---|
| Bilgisayar1 | [704, 701, 1501, 1500, 1085] |
| BİLGİSAYAR2 | [326.105.302.301.300.102] |
| ... | ... |

Benzer şekilde `makelist` , `makeset` sıralı verilerle de çalışır ve buna geçirilen satırların sırasına göre dizileri oluşturur.

## <a name="expanding-lists"></a>Listeleri genişletme
`makelist`Ya da `makeset` `mvexpand` bir değer listesini ayrı satırlara genişleten ters işlemi. Bu, hem JSON hem de dizi olmak üzere herhangi bir sayıda dinamik sütunda genişleyebilir. Örneğin, son bir saat içinde sinyal gönderen bilgisayarlardan veri gönderen çözümlerin *sinyal* tablosunu kontrol edebilirsiniz:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, Solutions
```

| Bilgisayar | Çözümler | 
|--------------|----------------------|
| Bilgisayar1 | "güvenlik", "Güncelleştirmeler", "changeTracking" |
| BİLGİSAYAR2 | "güvenlik", "Güncelleştirmeler" |
| computer3 | "kötü amaçlı yazılımdan koruma", "changeTracking" |
| ... | ... |

`mvexpand`Her değeri, virgülle ayrılmış bir liste yerine ayrı bir satırda göstermek için kullanın:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
```

| Bilgisayar | Çözümler | 
|--------------|----------------------|
| Bilgisayar1 | güven |
| Bilgisayar1 | güncelleştirmeleriyle |
| Bilgisayar1 | Değişiklik izleme dosyanız |
| BİLGİSAYAR2 | güven |
| BİLGİSAYAR2 | güncelleştirmeleriyle |
| computer3 | Korunma |
| computer3 | Değişiklik izleme dosyanız |
| ... | ... |


Daha sonra `makelist` öğeleri birlikte gruplandırmak için yeniden kullanabilirsiniz ve bu kez çözüm başına bilgisayar listesini görebilirsiniz:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
| summarize makelist(Computer) by tostring(Solutions) 
```

|Çözümler | list_Computer |
|--------------|----------------------|
| güven | ["Bilgisayar1", "bilgisayar2"] |
| güncelleştirmeleriyle | ["Bilgisayar1", "bilgisayar2"] |
| Değişiklik izleme dosyanız | ["Bilgisayar1", "computer3"] |
| Korunma | ["computer3"] |
| ... | ... |

## <a name="handling-missing-bins"></a>Eksik depo gözlerini işleme
Yararlı bir uygulaması, `mvexpand` eksik depo gözleri için içindeki varsayılan değerleri doldurmaya gereksinimdir. Örneğin, sinyali inceleyerek belirli bir makinenin çalışma süresini aradığınızı varsayalım. Ayrıca _Kategori_ sütununda olan sinyalin kaynağını görmek istersiniz. Normal olarak, aşağıdaki gibi basit bir özetleme ekstresi kullanacağız:

```Kusto
Heartbeat
| where TimeGenerated > ago(12h)
| summarize count() by Category, bin(TimeGenerated, 1h)
```

| Kategori | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Doğrudan aracı | 2017-06-06T17:00:00Z | 15 |
| Doğrudan aracı | 2017-06-06T18:00:00Z | 60 |
| Doğrudan aracı | 2017-06-06T20:00:00Z | 55 |
| Doğrudan aracı | 2017-06-06T21:00:00Z | 57 |
| Doğrudan aracı | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |

Bu sonuçlarda, bu saat için herhangi bir sinyal verisi olmadığından "2017-06-06T19:00:00Z" ile ilişkili demet yok. `make-series`Boş demetlere varsayılan değer atamak için işlevini kullanın. Bu, biri değerler için, diğeri de eşleşen zaman demetleri için olmak üzere iki ek dizi sütunu olan her bir kategori için bir satır oluşturur:

```Kusto
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
```

| Kategori | count_ | TimeGenerated |
|---|---|---|
| Doğrudan aracı | [15, 60, 0, 55, 60, 57, 60,...] | ["2017-06-06T17:00:00.0000000 Z", "2017-06-06T18:00:00.0000000 Z", "2017-06-06T19:00:00.0000000 Z", "2017-06-06T20:00:00.0000000 Z", "2017-06-06T21:00:00.0000000 Z",...] |
| ... | ... | ... |

*Count_* dizisinin üçüncü öğesi beklendiği gibi 0 ' dır ve _TimeGenerated_ dizisinde "2017-06-06T19:00:00.0000000 z" ile eşleşen bir zaman damgası vardır. Bu dizi biçimi de okunması zordur. `mvexpand`Dizileri genişletmek ve tarafından oluşturulan biçim çıkışını oluşturmak için kullanın `summarize` :

```Kusto
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
| mvexpand TimeGenerated, count_
| project Category, TimeGenerated, count_
```

| Kategori | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Doğrudan aracı | 2017-06-06T17:00:00Z | 15 |
| Doğrudan aracı | 2017-06-06T18:00:00Z | 60 |
| Doğrudan aracı | 2017-06-06T19:00:00Z | 0 |
| Doğrudan aracı | 2017-06-06T20:00:00Z | 55 |
| Doğrudan aracı | 2017-06-06T21:00:00Z | 57 |
| Doğrudan aracı | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |



## <a name="narrowing-results-to-a-set-of-elements-let-makeset-toscalar-in"></a>Sonuçları bir dizi öğe için daraltma: `let` , `makeset` , `toscalar` ,`in`
Yaygın bir senaryo, bazı belirli varlıkların adlarını bir dizi ölçüte göre seçmek ve sonra farklı bir veri kümesini bu varlık kümesine göre filtrelemenize olanak sağlar. Örneğin, eksik güncelleştirmeleri olduğunu bildiğiniz ve bu bilgisayarların şu şekilde çağırdığı IP 'Leri tanımlayan bilgisayarları bulabilirsiniz:


```Kusto
let ComputersNeedingUpdate = toscalar(
    Update
    | summarize makeset(Computer)
    | project set_Computer
);
WindowsFirewall
| where Computer in (ComputersNeedingUpdate)
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Izleyici günlük verileriyle [kusto sorgu dilini](/azure/kusto/query/) kullanmaya yönelik diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar]()
- [JSON ve veri yapıları](json-data-structures.md)
- [Birleştirme](joins.md)
- [Grafikler](charts.md)

