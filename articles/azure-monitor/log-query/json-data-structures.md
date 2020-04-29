---
title: Azure Izleyici günlük sorgularıyla dizelerle çalışma | Microsoft Docs
description: Bu makalede, Azure izleyici 'de günlük verilerini sorgulamak ve çözümlemek için Azure portal Azure Izleyici Log Analytics kullanmaya yönelik bir öğretici sunulmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: f792820b7b0dff20e647031410ba87ac26c2495a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80672967"
---
# <a name="working-with-json-and-data-structures-in-azure-monitor-log-queries"></a>Azure Izleyici günlük sorgularındaki JSON ve veri yapıları ile çalışma

> [!NOTE]
> Bu dersi tamamlamadan önce [Azure izleyici Log Analytics kullanmaya başlama](get-started-portal.md) ve [Azure izleyici günlük sorgularını](get-started-queries.md) kullanmaya başlama işlemini tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

İç içe geçmiş nesneler, bir dizideki diğer nesneleri veya anahtar-değer çiftlerinin haritasını içeren nesnelerdir. Bu nesneler JSON dizeleri olarak temsil edilir. Bu makalede, JSON 'ın verileri almak ve iç içe nesneleri çözümlemek için nasıl kullanıldığı açıklanmaktadır.

## <a name="working-with-json-strings"></a>JSON dizeleriyle çalışma
Bilinen `extractjson` bir yoldaki belırlı bir JSON öğesine erişmek için kullanın. Bu işlev, aşağıdaki kuralları kullanan bir yol ifadesi gerektirir.

- _$_ kök klasöre başvurmak için
- Aşağıdaki örneklerde gösterildiği gibi dizin ve öğelere başvurmak için köşeli ayracı veya nokta gösterimini kullanın.


Dizinler ve noktalar için köşeli ayraçları kullanın öğeleri ayırın:

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report
| extend status = extractjson("$.hosts[0].status", hosts_report)
```

Bu, yalnızca köşeli ayraç gösterimi ile aynı sonuçdır:

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report 
| extend status = extractjson("$['hosts'][0]['status']", hosts_report)
```

Yalnızca bir öğe varsa yalnızca nokta gösterimini kullanabilirsiniz:

```Kusto
let hosts_report=dynamic({"location":"North_DC", "status":"running", "rate":5});
print hosts_report 
| extend status = hosts_report.status
```


## <a name="working-with-objects"></a>Nesneler ile çalışma

### <a name="parsejson"></a>parseJSON
JSON yapınız içindeki birden çok öğeye erişmek için, dinamik bir nesne olarak erişmek daha kolay olur. Metin `parsejson` verilerini dinamik bir nesneye dönüştürmek için kullanın. Dinamik bir türe dönüştürüldükten sonra, verileri çözümlemek için ek işlevler kullanılabilir.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend status0=hosts_object.hosts[0].status, rate1=hosts_object.hosts[1].rate
```



### <a name="arraylength"></a>arraylength
Dizideki `arraylength` öğelerin sayısını saymak için kullanın:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend hosts_num=arraylength(hosts_object.hosts)
```

### <a name="mvexpand"></a>mvexpand
Bir `mvexpand` nesnenin özelliklerini ayrı satırlara bölmek için kullanın.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| mvexpand hosts_object.hosts[0]
```

![mvexpand](media/json-data-structures/mvexpand.png)

### <a name="buildschema"></a>buildschema
Bir `buildschema` nesnenin tüm değerlerini admits şemayı almak için kullanın:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```

Çıktı JSON biçimindeki bir şemadır:
```json
{
    "hosts":
    {
        "indexer":
        {
            "location": "string",
            "rate": "int",
            "status": "string"
        }
    }
}
```
Bu çıktı, nesne alanlarının adlarını ve bunların eşleşen veri türlerini açıklar. 

İç içe geçmiş nesneler aşağıdaki örnekte olduğu gibi farklı şemalara sahip olabilir:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"status":"stopped", "rate":"3", "range":100}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```


![Derleme şeması](media/json-data-structures/buildschema.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Izleyici 'de günlük sorgularını kullanmaya yönelik diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleştirme](joins.md)
- [Grafikler](charts.md)
