---
title: Azure Izleyici günlük sorgusundaki Resource () ifadesi | Microsoft Docs
description: Kaynak ifadesi, birden çok kaynaktan veri almak için kaynak merkezli bir Azure Izleyici günlük sorgusunda kullanılır.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 9a5e5c7959a243d6c9d243b706524f624ffa3cdb
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102047222"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>Azure Izleyici günlük sorgusunda Resource () ifadesi

`resource`İfade, diğer kaynaklardan verileri almak için [kaynak kapsamındaki](scope.md#query-scope) bir Azure izleyici sorgusunda kullanılır. 


## <a name="syntax"></a>Syntax

`resource(`*Tanımlayıcısını*`)`

## <a name="arguments"></a>Bağımsız değişkenler

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
