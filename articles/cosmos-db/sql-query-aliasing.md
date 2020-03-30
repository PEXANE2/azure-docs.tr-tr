---
title: Azure Cosmos DB'de Diğer Adlama
description: Aynı ada sahip iki özelliği ayırt etmek için Azure Cosmos DB SQL sorgularında takma adı nasıl kullanacağınızı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 74849eec4c5808a584894321269c49c41f0b8a5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873480"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Azure Cosmos DB'de Diğer Adlama

Sorgularda açıkça diğer ad değerleri yapabilirsiniz. Bir sorgunun aynı ada sahip iki özelliği varsa, öngörülen sonuçta ayrıştırılmak için özelliklerden birini veya her ikisini yeniden adlandırmak için diğer adı kullanın.

## <a name="examples"></a>Örnekler

Diğer ad için kullanılan AS anahtar kelimesi, aşağıdaki örnekte gösterildiği gibi `NameInfo`isteğe bağlıdır:

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
