---
title: Azure Cosmos DB Graph üzerinden sistem belge özelliklerine erişin
description: Gremlin API aracılığıyla Cosmos DB sistem belge özelliklerini nasıl okuyup yazdığını öğrenin
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 09/10/2019
author: luisbosquez
ms.author: lbosq
ms.openlocfilehash: 4ed7e67ae0ef027b260d0e0f0407e4e05ed5a8f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898298"
---
# <a name="system-document-properties"></a>Sistem belge özellikleri

Azure Cosmos DB, , ```_ts``` ```_self```, ```_attachments``` ```_rid```, ```_etag``` , ve her belgede gibi [sistem özelliklerine](https://docs.microsoft.com/rest/api/cosmos-db/databases) sahiptir. Bunun yanı sıra Gremlin altyapısı kenarlara ```inVPartition``` ve ```outVPartition``` özelliklerini ekler. Varsayılan olarak, bu özellikler geçiş için kullanılabilir. Ancak, Gremlin traversal belirli özellikleri veya hepsini dahil etmek mümkündür.

```
g.withStrategies(ProjectionStrategy.build().IncludeSystemProperties('_ts').create())
```

## <a name="e-tag"></a>E-Tag

Bu özellik iyimser eşzamanlılık denetimi için kullanılır. Uygulamanın işlemi birkaç ayrı geçişe ayırması gerekiyorsa, eşzamanlı yazmalarda veri kaybını önlemek için eTag özelliğini kullanabilir.

```
g.withStrategies(ProjectionStrategy.build().IncludeSystemProperties('_etag').create()).V('1').has('_etag', '"00000100-0000-0800-0000-5d03edac0000"').property('test', '1')
```

## <a name="time-to-live-ttl"></a>Etkin kalma süresi (TTL)

Koleksiyonun belge sona erme ```ttl``` süresi etkinleştirilmişse ve belgelerin üzerinde özellik ayarlanmışsa, bu özellik gremlin traversal'de normal bir tepe noktası veya kenar özelliği olarak kullanılabilir. ```ProjectionStrategy```zaman-to-live özellik pozlama etkinleştirmek için gerekli değildir.

Aşağıdaki dolaşma ile oluşturulan köşe **123 saniye** içinde otomatik olarak silinecektir.

```
g.addV('vertex-one').property('ttl', 123)
```

## <a name="next-steps"></a>Sonraki adımlar
* [Cosmos DB İyimser Eşzamanlılık](faq.md#how-does-the-sql-api-provide-concurrency)
* Azure Cosmos DB'de [Yaşama Zamanı (TTL)](time-to-live.md)
