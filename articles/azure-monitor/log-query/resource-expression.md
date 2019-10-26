---
title: Azure Izleyici günlük sorgusundaki Resource () ifadesi | Microsoft Docs
description: Kaynak ifadesi, birden çok kaynaktan veri almak için kaynak merkezli bir Azure Izleyici günlük sorgusunda kullanılır.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: article
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 4787dc8a8cc06e0ad0be88597dab3481284fb58b
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900224"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>Azure Izleyici günlük sorgusunda Resource () ifadesi

`resource` ifadesi, diğer kaynaklardan verileri almak için [kaynak kapsamındaki](scope.md#query-scope) bir Azure izleyici sorgusunda kullanılır. 


## <a name="syntax"></a>Sözdizimi

`resource(`*tanımlayıcı*`)`

## <a name="arguments"></a>Bağımsız Değişkenler

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
