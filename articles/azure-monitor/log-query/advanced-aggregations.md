---
title: Azure Monitor günlük sorgularında gelişmiş toplamalar| Microsoft Dokümanlar
description: Azure Monitor günlük sorgularında kullanılabilen daha gelişmiş toplama seçeneklerinden bazılarını açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: e5dc290a40342e0797001dde6cab90e12dd5cf39
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77662187"
---
# <a name="advanced-aggregations-in-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularında gelişmiş toplamalar

> [!NOTE]
> Bu dersi tamamlamadan önce [Azure Monitor sorgularında Toplama'yı](./aggregations.md) tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Bu makalede, Azure Monitor sorgularında kullanılabilen daha gelişmiş toplama seçeneklerinden bazıları açıklanmaktadır.

## <a name="generating-lists-and-sets"></a>Liste ve küme oluşturma
Belirli bir `makelist` sütundaki değerlerin sırasına göre verileri döndürmek için kullanabilirsiniz. Örneğin, makinelerinizde gerçekleşen en yaygın sipariş olaylarını keşfetmek isteyebilirsiniz. Verileri her makinedeki EventI'lerin sırasına göre döndürebilirsiniz. 

```Kusto
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makelist(EventID) by Computer
```

|Bilgisayar|list_EventID|
|---|---|
| bilgisayar1 | [704,701,1501,1500,1085,704,704,701] |
| bilgisayar2 | [326,105,302,301,300,102] |
| ... | ... |

`makelist`verilerin içine aktarılması sırasına göre bir liste oluşturur. Olayları en eskiden en `asc` yeniye sıralamak `desc`için, sipariş deyimiyerine . 

Ayrıca, sadece farklı değerlerin bir listesini oluşturmak için yararlıdır. Buna _Set_ denir ve `makeset`şu şekilde oluşturulabilir:

```Kusto
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makeset(EventID) by Computer
```

|Bilgisayar|list_EventID|
|---|---|
| bilgisayar1 | [704,701,1501,1500,1085] |
| bilgisayar2 | [326,105,302,301,300,102] |
| ... | ... |

Gibi `makelist` `makeset` , aynı zamanda sıralı verilerle çalışır ve içine geçirilen satırların sırasına göre diziler oluşturur.

## <a name="expanding-lists"></a>Listeleri genişletme
Ters işlem `makelist` veya `makeset` , `mvexpand`ayrı satırdeğerler listesini genişletir. Hem JSON hem de dizi olmak üzere herhangi bir sayıda dinamik sütun arasında genişletilebilir. Örneğin, son bir saat içinde kalp atışı gönderen bilgisayarlardan veri gönderen çözümler için *Sinyal* tablosunu denetleyebilirsiniz:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, Solutions
```

| Bilgisayar | Çözümler | 
|--------------|----------------------|
| bilgisayar1 | "güvenlik", "güncellemeler", "changeTracking" |
| bilgisayar2 | "güvenlik", "güncellemeler" |
| bilgisayar3 | "antiMalware", "changeTracking" |
| ... | ... |

Virgülle ayrılmış bir liste yerine her değeri ayrı bir satırda göstermek için kullanın: `mvexpand`

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
```

| Bilgisayar | Çözümler | 
|--------------|----------------------|
| bilgisayar1 | "güvenlik" |
| bilgisayar1 | "güncellemeler" |
| bilgisayar1 | "changeTracking" |
| bilgisayar2 | "güvenlik" |
| bilgisayar2 | "güncellemeler" |
| bilgisayar3 | "antiMalware" |
| bilgisayar3 | "changeTracking" |
| ... | ... |


Daha sonra `makelist` öğeleri birlikte gruplandırmak için yeniden kullanabilirsiniz ve bu kez çözüm başına bilgisayarların listesini görebilirsiniz:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
| summarize makelist(Computer) by tostring(Solutions) 
```

|Çözümler | list_Computer |
|--------------|----------------------|
| "güvenlik" | ["bilgisayar1", "bilgisayar2"] |
| "güncellemeler" | ["bilgisayar1", "bilgisayar2"] |
| "changeTracking" | ["bilgisayar1", "bilgisayar3"] |
| "antiMalware" | ["bilgisayar3"] |
| ... | ... |

## <a name="handling-missing-bins"></a>Eksik kutuları işleme
Yararlı bir `mvexpand` uygulama eksik kutular için varsayılan değerleri doldurmak için ihtiyaçtır. Örneğin, belirli bir makinenin kalp atışlarını keşfederek çalışma süresini aradığınızı varsayalım. Ayrıca, _kategori_ sütununda bulunan sinyalin kaynağını da görmek istersiniz. Normalde, aşağıdaki gibi basit bir özet ifade kullanırız:

```Kusto
Heartbeat
| where TimeGenerated > ago(12h)
| summarize count() by Category, bin(TimeGenerated, 1h)
```

| Kategori | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Doğrudan Temsilci | 2017-06-06T17:00:00Z | 15 |
| Doğrudan Temsilci | 2017-06-06T18:00:00Z | 60 |
| Doğrudan Temsilci | 2017-06-06T20:00:00Z | 55 |
| Doğrudan Temsilci | 2017-06-06T21:00:00Z | 57 |
| Doğrudan Temsilci | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |

Bu sonuçlarda "2017-06-06T19:00:00Z" ile ilişkili kova eksik olsa da, o saat için herhangi bir kalp atışı verisi yok. Boş `make-series` kovalara varsayılan değer atamak için işlevi kullanın. Bu, her kategori için iki ekstra dizi sütunu, biri değerler için ve diğeri eşleşen zaman kovaları için bir satır oluşturur:

```Kusto
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
```

| Kategori | count_ | TimeGenerated |
|---|---|---|
| Doğrudan Temsilci | [15,60,0,55,60,57,60,...] | ["2017-06-06T17:00.000000Z","2017-06-06T18:00:00.0000000Z","2017-06-06T19:0 00:00.000000Z","2017-06-06T20:00:000000Z","2017-06-06-06T21:00:000000Z",...] |
| ... | ... | ... |

*count_* dizisinin üçüncü öğesi beklendiği gibi 0'dır ve _TimeGenerated_ dizisinde "2017-06-06T19:00:00.0000000Z" eşleşen bir zaman damgası vardır. Bu dizi biçimini okumak zor olsa da. Dizileri genişletmek ve aşağıdakiler tarafından oluşturulan aynı `summarize`biçim çıktısını üretmek için kullanın: `mvexpand`

```Kusto
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
| mvexpand TimeGenerated, count_
| project Category, TimeGenerated, count_
```

| Kategori | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Doğrudan Temsilci | 2017-06-06T17:00:00Z | 15 |
| Doğrudan Temsilci | 2017-06-06T18:00:00Z | 60 |
| Doğrudan Temsilci | 2017-06-06T19:00:00Z | 0 |
| Doğrudan Temsilci | 2017-06-06T20:00:00Z | 55 |
| Doğrudan Temsilci | 2017-06-06T21:00:00Z | 57 |
| Doğrudan Temsilci | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |



## <a name="narrowing-results-to-a-set-of-elements-let-makeset-toscalar-in"></a>Sonuçları bir dizi öğeye `let`daraltma: , , `makeset` `toscalar`,`in`
Yaygın bir senaryo, bazı belirli varlıkların adlarını bir ölçüt kümesine göre seçmek ve ardından bu varlık kümesine göre farklı bir veri kümesine filtre uygulayın. Örneğin, eksik güncelleştirmeleri olduğu bilinen bilgisayarları bulabilir ve bu bilgisayarların çağırdığı IP'leri tanımlayabilirsiniz:


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

Azure Monitor günlük verileriyle [Kusto sorgu dilini](/azure/kusto/query/) kullanmak için diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Birleştirme](joins.md)
- [Grafikler](charts.md)
