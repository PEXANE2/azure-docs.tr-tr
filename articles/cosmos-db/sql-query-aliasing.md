---
title: Azure Cosmos DB diğer ad
description: Aynı ada sahip iki özelliği ayırt etmek için Azure Cosmos DB SQL sorgularında diğer ad kullanmayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 74849eec4c5808a584894321269c49c41f0b8a5c
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873480"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Azure Cosmos DB diğer ad

Sorgularda açıkça diğer ad değerleri ekleyebilirsiniz. Sorgunun aynı ada sahip iki özelliği varsa, bir veya her ikisini de yeniden adlandırmak için diğer adları kullanın.

## <a name="examples"></a>Örnekler

Diğer ad için kullanılan AS anahtar sözcüğü, aşağıdaki örnekte gösterildiği gibi isteğe bağlıdır. Örneğin, ikinci değer `NameInfo`olarak yansıtıyorsunuz:

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [SELECT yan tümcesi](sql-query-select.md)
- [FROM yan tümcesi](sql-query-from.md)
