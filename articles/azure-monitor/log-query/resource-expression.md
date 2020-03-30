---
title: azure monitor günlük sorgusunda kaynak() ifadesi | Microsoft Dokümanlar
description: Kaynak ifadesi, birden çok kaynaktan veri almak için kaynak merkezli Azure Monitor günlük sorgusunda kullanılır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 2a729caefe698b13833098ba48df9d4bfbd97356
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665708"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>Azure Monitor günlük sorgusunda kaynak() ifadesi

İfade, `resource` diğer kaynaklardan veri almak [için kaynağa kapsamlı bir](scope.md#query-scope) Azure Monitor sorgusunda kullanılır. 


## <a name="syntax"></a>Sözdizimi

`resource(`*Tanımlayıcı*`)`

## <a name="arguments"></a>Bağımsız Değişkenler

- *Tanımlayıcı*: Kaynağın kaynak kimliği.

| Tanımlayıcı | Açıklama | Örnek
|:---|:---|:---|
| Kaynak | Kaynak için veri içerir. | kaynak("/abonelikler/xxxxxxx-xxxx-xxxx-xxxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm") |
| Kaynak Grubu veya Abonelik | Kaynak ve içerdiği tüm kaynaklar için veri içerir.  | kaynak("/abonelikler/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup) |


## <a name="notes"></a>Notlar

* Kaynağa erişimi okumuş olmalısınız.


## <a name="examples"></a>Örnekler

```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm").Heartbeat) | summarize count() by _ResourceId, TenantId
```
```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup).Heartbeat) | summarize count() by _ResourceId, TenantId
```


## <a name="next-steps"></a>Sonraki adımlar

- Sorgu kapsamıyla ilgili ayrıntılar için [Azure Monitor Log Analytics'teki Günlük sorgusu kapsamı ve zaman aralığına](scope.md) bakın.
- [Kusto sorgu dili](/azure/kusto/query/)için tam belgelere erişin.
