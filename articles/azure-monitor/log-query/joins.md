---
title: Azure Monitör günlük sorgularına katılır | Microsoft Dokümanlar
description: Bu makalede, Azure Monitor günlük sorgularında birleştirmeleri kullanma yla ilgili bir ders bulunmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: 2dace6968fbbe69f806c27fb7a46e60c63f78b4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670211"
---
# <a name="joins-in-azure-monitor-log-queries"></a>Azure Monitör günlük sorgularına katılır

> [!NOTE]
> Bu dersi tamamlamadan önce [Azure Monitor Log Analytics](get-started-portal.md) ve [Azure Monitor günlük sorgularını](get-started-queries.md) başlatın'ı tamamlamanız gerekir.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Birleştirmeler, aynı sorguda birden çok tablodaki verileri çözümlemenize olanak sağlar. İki veri kümesinin satırlarını, belirtilen sütunların değerlerini eşleştirerek birleştirirler.


```Kusto
SecurityEvent 
| where EventID == 4624     // sign-in events
| project Computer, Account, TargetLogonId, LogonTime=TimeGenerated
| join kind= inner (
    SecurityEvent 
    | where EventID == 4634     // sign-out events
    | project TargetLogonId, LogoffTime=TimeGenerated
) on TargetLogonId
| extend Duration = LogoffTime-LogonTime
| project-away TargetLogonId1 
| top 10 by Duration desc
```

Bu örnekte, ilk veri kümesi tüm oturum açma olayları için filtreler. Bu, tüm oturum açma olaylarını filtreleyen ikinci bir veri kümesiyle birleştirilir. Öngörülen sütunlar _Bilgisayar_, _Hesap_, _TargetLogonId_ve _TimeGenerated_vardır. Veri kümeleri paylaşılan bir sütun, _TargetLogonId_ile ilişkilidir. Çıktı, hem oturum açma hem de oturum açma süresine sahip olan korelasyon başına tek bir kayıttır.

Her iki veri kümesinde de aynı adlara sahip sütunlar varsa, sağ taraftaki veri kümesinin sütunlarında bir dizin numarası verilir, bu nedenle bu örnekte sonuçlar _TargetLogonId'i_ sol taraftaki tablodaki değerlerle ve _TargetLogonId1'i_ sağ taraftaki tablodaki değerlerle gösterir. Bu durumda, ikinci _TargetLogonId1_ sütunu `project-away` işleç kullanılarak kaldırıldı.

> [!NOTE]
> Performansı artırmak `project` için, işleci kullanarak yalnızca birleştirilmiş veri kümelerinin ilgili sütunlarını saklayın.


İki veri kümesini birleştirmek için aşağıdaki sözdizimini kullanın ve birleştirilmiş anahtarın iki tablo arasında farklı bir adı vardır:
```
Table1
| join ( Table2 ) 
on $left.key1 == $right.key2
```

## <a name="lookup-tables"></a>Arama Tabloları
Birleştirmelerin yaygın kullanımı, sonuçların daha uygun `datatable` bir şekilde dönüştürülmesine yardımcı olabilecek değerlerin statik eşlemi kullanmaktır. Örneğin, güvenlik olay verilerini her olay kimliği için olay adı ile zenginleştirmek için.

```Kusto
let DimTable = datatable(EventID:int, eventName:string)
  [
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4658, "The handle to an object was closed",
    4656, "A handle to an object was requested",
    4690, "An attempt was made to duplicate a handle to an object",
    4663, "An attempt was made to access an object",
    5061, "Cryptographic operation",
    5058, "Key file operation"
  ];
SecurityEvent
| join kind = inner
 DimTable on EventID
| summarize count() by eventName
```

![Veri tablosuyla katılma](media/joins/dim-table.png)

## <a name="join-kinds"></a>Birleştirme türleri
_Tür_ bağımsız değişkeniyle birleştirme türünü belirtin. Her tür, aşağıdaki tabloda açıklandığı gibi verilen tabloların kayıtları arasında farklı bir eşleşme gerçekleştirir.

| Katılım türü | Açıklama |
|:---|:---|
| innerunique | Bu varsayılan birleştirme modudur. Önce sol tablodaki eşleşen sütunun değerleri bulunur ve yinelenen değerler kaldırılır.  Daha sonra benzersiz değerler kümesi doğru tabloyla eşleşir. |
| Iç | Sonuçlara yalnızca her iki tablodaki eşleşen kayıtlar dahildir. |
| leftouter | Sol tablodaki tüm kayıtlar ve sağ tablodaki eşleşen kayıtlar sonuçlara dahil edilir. Eşleşmemiş çıkış özellikleri nulls içerir.  |
| leftanti | Sol taraftan sağdan eşleşme olmayan kayıtlar sonuçlara dahil edilir. Sonuç tablosunda yalnızca sol tablodan sütunlar bulunur. |
| solsemi | Sonuçlara sağdan eşleşen sol taraftan gelen kayıtlar dahildir. Sonuç tablosunda yalnızca sol tablodan sütunlar bulunur. |


## <a name="best-practices"></a>En iyi uygulamalar

En iyi performans için aşağıdaki noktaları göz önünde bulundurun:

- Birleştirme için değerlendirilmesi gereken kayıtları azaltmak için her tabloda bir zaman filtresi kullanın.
- Birleştirmeden önce giriş tablolarındaki satır ve sütun sayısını kullanın `where` ve `project` azaltmak için kullanın.
- Bir tablo her zaman diğerinden daha küçükse, birleştirmenin sol tarafı olarak kullanın.


## <a name="next-steps"></a>Sonraki adımlar
Azure Monitor günlük sorgularını kullanmak için diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Grafikler](charts.md)
