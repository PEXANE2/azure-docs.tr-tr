---
title: Azure Cosmos DB Graph aracılığıyla sistem belgesi özelliklerine erişin
description: Gremlin API aracılığıyla Cosmos DB sistem belge özelliklerini okuma ve yazma hakkında bilgi edinin
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: how-to
ms.date: 09/10/2019
author: luisbosquez
ms.author: lbosq
ms.openlocfilehash: a0feac9bbd98dc4c67464e84e9a3204bd9730355
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85390355"
---
# <a name="system-document-properties"></a>Sistem belgesi özellikleri

Azure Cosmos DB her belgede,,, ve gibi [sistem özelliklerine](/rest/api/cosmos-db/databases) sahiptir ```_ts``` ```_self``` ```_attachments``` ```_rid``` ```_etag``` . Bunun yanı sıra Gremlin altyapısı kenarlara ```inVPartition``` ve ```outVPartition``` özelliklerini ekler. Varsayılan olarak, bu özellikler çapraz geçiş için kullanılabilir. Ancak, Gremlin geçişi içinde belirli özellikleri veya bunların tümünü dahil etmek mümkündür.

```
g.withStrategies(ProjectionStrategy.build().IncludeSystemProperties('_ts').create())
```

## <a name="e-tag"></a>E-Tag

Bu özellik iyimser eşzamanlılık denetimi için kullanılır. Uygulamanın işlemi birkaç ayrı traversals bir kez kesmesi gerekiyorsa, eş zamanlı yazma işlemlerinde veri kaybını önlemek için eTag özelliğini kullanabilir.

```
g.withStrategies(ProjectionStrategy.build().IncludeSystemProperties('_etag').create()).V('1').has('_etag', '"00000100-0000-0800-0000-5d03edac0000"').property('test', '1')
```

## <a name="time-to-live-ttl"></a>Etkin kalma süresi (TTL)

Koleksiyonda belge kullanım süresi etkinse ve belgeler ```ttl``` üzerinde özellik ayarlandıysa, bu özellik Gremlin geçiş sırasında normal bir köşe veya kenar özelliği olarak kullanılabilir. ```ProjectionStrategy```yaşam süresi özelliği pozlaması sağlamak için gerekli değildir.

Aşağıdaki dolaşma ile oluşturulan köşe **123 saniye** içinde otomatik olarak silinecektir.

```
g.addV('vertex-one').property('ttl', 123)
```

## <a name="next-steps"></a>Sonraki adımlar
* [Cosmos DB İyimser Eşzamanlılık](faq.md#how-does-the-sql-api-provide-concurrency)
* Azure Cosmos DB [yaşam süresi (TTL)](time-to-live.md)
