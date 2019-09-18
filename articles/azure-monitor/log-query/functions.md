---
title: Azure Izleyici günlük sorgularının işlevleri | Microsoft Docs
description: Bu makalede, Azure Izleyici 'de başka bir günlük sorgusundan bir sorgu çağırmak için işlevlerinin nasıl kullanılacağı açıklanır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: bwren
ms.openlocfilehash: 75beb7b66863efd2fb3679f034a3663dca4a6d2f
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71076711"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Azure Izleyici günlük sorgularındaki işlevleri kullanma

Başka bir sorguyla bir günlük sorgusu kullanmak için, bunu bir işlev olarak kaydedebilirsiniz. Bu, karmaşık sorguları bölümlere ayırarak basitleştirmeye ve birden çok sorguyla ortak kod kullanmanıza olanak sağlar.

## <a name="create-a-function"></a>İşlev oluşturma

**Kaydet** ' e tıklayarak ve ardından aşağıdaki tablodaki bilgileri sağlayarak Azure Portal Log Analytics bir işlev oluşturun.

| Ayar | Açıklama |
|:---|:---|
| Name           | Sorgu **Gezgini**'nde sorgu için görünen ad. |
| Farklı Kaydet        | İşlev |
| İşlev Diğer Adı | Diğer sorgularda işlevi kullanmak için kısa ad. Boşluk içeremez ve benzersiz olmalıdır. |
| Category       | **Sorgu Gezgini**'nde Kaydedilmiş sorguları ve işlevleri düzenlemek için bir kategori. |

> [!NOTE]
> Azure Izleyici 'deki bir işlev başka bir işlev içeremez.




## <a name="use-a-function"></a>Bir işlev kullanın
Diğer bir sorguya diğer adını ekleyerek bir işlev kullanın. Diğer tablolar gibi kullanılabilir.

## <a name="example"></a>Örnek
Aşağıdaki örnek sorgu, son gün içinde bildirilen tüm eksik güvenlik güncelleştirmelerini döndürür. Bu sorguyu, _security_updates_last_day_diğer adına sahip bir işlev olarak kaydedin. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

SQL ile ilgili gerekli güvenlik güncelleştirmelerini aramak için başka bir sorgu oluşturun ve _security_updates_last_day_ işlevine başvurun.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Sonraki adımlar
Bkz. Azure Izleyici günlük sorgularını yazmak için diğer dersler:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Birleştirmeler](joins.md)
- [Grafik](charts.md)
