---
title: Azure Monitor günlük sorgularında işlevler | Microsoft Dokümanlar
description: Bu makalede, Azure Monitor'daki başka bir günlük sorgusundan sorgu çağırmak için işlevlerin nasıl kullanılacağı açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/15/2018
ms.openlocfilehash: 7d94e53abbe8f4d2953729aa2363c3906ce94f74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670228"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularında işlevleri kullanma

Başka bir sorguile bir günlük sorgusu kullanmak için bir işlev olarak kaydedebilirsiniz. Bu, karmaşık sorguları parçalara ayırarak basitleştirmenize ve ortak kodu birden çok sorguyla yeniden kullanmanıza olanak tanır.

## <a name="create-a-function"></a>İşlev oluşturma

**Kaydet'i** tıklatarak ve ardından aşağıdaki tablodaki bilgileri sağlayarak Azure portalında Log Analytics ile bir işlev oluşturun.

| Ayar | Açıklama |
|:---|:---|
| Adı           | **Sorgu gezgininde**sorgunun görüntü adı. |
| Farklı kaydet        | İşlev |
| İşlev Diğer Adları | Diğer sorgularda işlevi kullanmak için kısa ad. Boşluk içermeyebilir ve benzersiz olmalıdır. |
| Kategori       | **Sorgu gezgininde**kaydedilen sorguları ve işlevleri düzenlemek için bir kategori. |

> [!NOTE]
> Azure Monitor'daki bir işlev başka bir işlev içeremez.




## <a name="use-a-function"></a>Bir işlev kullanma
Başka bir sorguya takma adını ekleyerek bir işlev kullanın. Diğer masalar gibi kullanılabilir.

## <a name="example"></a>Örnek
Aşağıdaki örnek sorgu, son gün içinde bildirilen tüm eksik güvenlik güncelleştirmelerini döndürür. Bu sorguyu _diğer_ad security_updates_last_day olan bir işlev olarak kaydedin. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

BAŞKA bir sorgu oluşturun ve SQL ile ilgili gerekli güvenlik güncelleştirmelerini aramak için _security_updates_last_day_ işlevine başvurun.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Monitor günlük sorgularını yazmak için diğer derslere bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamalar](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Birleştirme](joins.md)
- [Grafikler](charts.md)
