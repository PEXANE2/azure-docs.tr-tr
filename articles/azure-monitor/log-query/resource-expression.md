---
title: Azure Izleyici günlük sorgusundaki Resource () ifadesi | Microsoft Docs
description: Kaynak ifadesi, birden çok kaynaktan veri almak için kaynak merkezli bir Azure Izleyici günlük sorgusunda kullanılır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 2a729caefe698b13833098ba48df9d4bfbd97356
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77665708"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>Azure Izleyici günlük sorgusunda Resource () ifadesi

`resource`İfade, diğer kaynaklardan verileri almak için [kaynak kapsamındaki](scope.md#query-scope) bir Azure izleyici sorgusunda kullanılır. 


## <a name="syntax"></a>Syntax

`resource(`*Tanımlayıcısını*`)`

## <a name="arguments"></a>Arguments

- *Tanımlayıcı*: bir KAYNAĞıN kaynak kimliği.

| Tanımlayıcı | Açıklama | Örnek
|:---|:---|:---|
| Kaynak | Kaynakla ilgili verileri içerir. | Kaynak ("/Subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/Providers/Microsoft.COMPUTE/virtualmachines/myvm") |
| Kaynak grubu veya abonelik | Kaynak ve içerdiği tüm kaynaklar için verileri içerir.  | Kaynak ("/Subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup) |


## <a name="notes"></a>Notlar

* Kaynağa okuma erişiminizin olması gerekir.


## <a name="examples"></a>Örnekler

```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm").Heartbeat) | summarize count() by _ResourceId, TenantId
```
```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup).Heartbeat) | summarize count() by _ResourceId, TenantId
```


## <a name="next-steps"></a>Sonraki adımlar

- Sorgu kapsamı hakkındaki ayrıntılar için bkz. [Azure izleyici 'de günlük sorgusu kapsamı ve zaman aralığı Log Analytics](scope.md) .
- [Kusto sorgu dili](/azure/kusto/query/)için tam belgelere erişin.
