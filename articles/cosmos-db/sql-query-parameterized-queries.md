---
title: Azure Cosmos DB'de parametreli sorgular
description: SQL parametreli sorguların kullanıcı girişinin sağlam bir şekilde işlemesini ve kaçmasını nasıl sağladığını öğrenin ve SQL enjeksiyonu yoluyla verilerin yanlışlıkla açığa alınmasını önleyin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: e15a8236723c1efd80f27f2d253e9bbc44af4b0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74870828"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Azure Cosmos DB'de parametreli sorgular

Cosmos DB, tanıdık @ gösterimi ile ifade edilen parametrelerle sorguları destekler. Parametreli SQL, kullanıcı girişinin sağlam bir şekilde işlemesini ve kaçmasını sağlar ve SQL enjeksiyonu yoluyla verilerin kazara maruz kalmasını önler.

## <a name="examples"></a>Örnekler

Örneğin, `lastName` parametre `address.state` olarak alan bir sorgu yazabilir ve kullanıcı `lastName` girişine `address.state` dayalı olarak çeşitli değerler için çalıştırabilirsiniz.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Daha sonra bu isteği aşağıdaki gibi parametrelendirilmiş JSON sorgusu olarak Cosmos DB'ye gönderebilirsiniz:

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

Aşağıdaki örnek, parametreli bir sorguyla TOP bağımsız değişkenini ayarlar: 

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Parametre değerleri geçerli bir JSON olabilir: dizeleri, sayılar, Booleans, null, çift diziler veya iç içe JSON. Cosmos DB şemasız olduğundan, parametreler herhangi bir türe göre doğrulanmaz.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Belge verilerini modelleme](modeling-data.md)
