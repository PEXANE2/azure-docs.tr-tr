---
title: Azure Monitor günlük sorgularında dizeleri ile çalışma | Microsoft Dokümanlar
description: Bu makalede, Azure Monitör'deki günlük verilerini sorgulamak ve analiz etmek için Azure portalında Azure Monitör Günlük Analitiği'ni kullanmak için bir öğretici verilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: 8be4f318149590ff08b73fda719e99a17220ec2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670160"
---
# <a name="working-with-json-and-data-structures-in-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularında JSON ve veri Yapıları ile çalışma

> [!NOTE]
> Bu dersi tamamlamadan önce [Azure Monitor Log Analytics](get-started-portal.md) ve [Almaya Azure Monitör günlük sorgularıyla başlamayı](get-started-queries.md) tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

İç içe nesneler, bir dizideki diğer nesneleri veya anahtar değer çiftleri haritasını içeren nesnelerdir. Bu nesneler JSON dizeleri olarak temsil edilir. Bu makalede, JSON'Un veri almak ve iç içe geçmiş nesneleri çözümlemek için nasıl kullanıldığı açıklanmaktadır.

## <a name="working-with-json-strings"></a>JSON dizeleri ile çalışma
Bilinen `extractjson` bir yolda belirli bir JSON öğesine erişmek için kullanın. Bu işlev, aşağıdaki kuralları kullanan bir yol ifadesi gerektirir.

- _$_ kök klasörüne başvurmak için
- Aşağıdaki örneklerde gösterildiği gibi dizinlere ve öğelere başvurmak için parantez veya nokta gösterimini kullanın.


Öğeleri ayırmak için dizinler ve noktalar için parantezler kullanın:

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report
| extend status = extractjson("$.hosts[0].status", hosts_report)
```

Bu, yalnızca parantez gösterimini kullanarak aynı sonuçtur:

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report 
| extend status = extractjson("$['hosts'][0]['status']", hosts_report)
```

Yalnızca bir öğe varsa, yalnızca nokta gösterimini kullanabilirsiniz:

```Kusto
let hosts_report='{"location":"North_DC", "status":"running", "rate":5}';
print hosts_report 
| extend status = hosts_report.status
```


## <a name="working-with-objects"></a>Nesneler ile çalışma

### <a name="parsejson"></a>parsejson
Json yapınızdaki birden çok öğeye erişmek için dinamik bir nesne olarak erişmek daha kolaydır. Metin `parsejson` verilerini dinamik bir nesneye dökmek için kullanın. Dinamik bir türe dönüştürüldükten sonra, verileri çözümlemek için ek işlevler kullanılabilir.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend status0=hosts_object.hosts[0].status, rate1=hosts_object.hosts[1].rate
```



### <a name="arraylength"></a>dizi uzunluğu
Bir `arraylength` dizideki öğe sayısını saymak için kullanın:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend hosts_num=arraylength(hosts_object.hosts)
```

### <a name="mvexpand"></a>mvexpand
Nesnenin özelliklerini ayrı satırlara ayırmak için kullanın. `mvexpand`

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| mvexpand hosts_object.hosts[0]
```

![mvexpand](media/json-data-structures/mvexpand.png)

### <a name="buildschema"></a>buildschema
Bir `buildschema` nesnenin tüm değerlerini kabul eden şemayı almak için kullanın:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```

Çıktı JSON formatında bir şema:
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
Bu çıktı, nesne alanlarının adlarını ve eşleşen veri türlerini açıklar. 

İç içe nesneler aşağıdaki örnekte olduğu gibi farklı şemalara sahip olabilir:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"status":"stopped", "rate":"3", "range":100}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```


![Şema oluşturma](media/json-data-structures/buildschema.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Monitor'da günlük sorgularını kullanmak için diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleştirme](joins.md)
- [Grafikler](charts.md)
