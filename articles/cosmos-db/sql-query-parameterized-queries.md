---
title: Azure Cosmos DB parametreli sorgular
description: SQL parametreli sorguların, Kullanıcı girişi için güçlü işleme ve kaçışı nasıl sağladığını ve SQL ekleme aracılığıyla verilerin yanlışlıkla açıklanmasını nasıl önleyeceğinizi öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/29/2020
ms.author: tisande
ms.openlocfilehash: f66bc89ef56dd0c2291903d531a4637210abd8df
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496993"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Azure Cosmos DB parametreli sorgular

Azure Cosmos DB tanıdık @ gösterimi tarafından ifade edilen parametrelere sahip sorguları destekler. Parametreli SQL, Kullanıcı girişi için güçlü işleme ve kaçış sağlar ve SQL ekleme aracılığıyla verilerin yanlışlıkla açıklanmasını önler.

## <a name="examples"></a>Örnekler

Örneğin, parametreleri alan ve olarak bir sorgu yazabilir `lastName` `address.state` ve `lastName` Kullanıcı girişine göre çeşitli değerleri ve bunları çalıştırabilirsiniz `address.state` .

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Daha sonra bu isteği aşağıdaki gibi parametreli bir JSON sorgusu olarak Azure Cosmos DB gönderebilirsiniz:

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

Aşağıdaki örnek, en üstteki bağımsız değişkeni parametreli bir sorgu ile ayarlar:

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Parametre değerleri geçerli bir JSON olabilir: dizeler, sayılar, Boole değerleri, null, hatta diziler veya iç içe geçmiş JSON. Azure Cosmos DB şemaya daha az olduğundan, parametreler hiçbir türe göre doğrulanmaz.

Her Azure Cosmos DB SDK 'sında parametreli sorgular için örnekler aşağıda verilmiştir:

- [.NET SDK](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L195)
- [Java](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/src/main/java/com/azure/cosmos/examples/queries/sync/QueriesQuickstart.java#L392-L421)
- [Node.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L58-L79)
- [Python](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/document_management.py#L66-L78)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Belge verilerini modelleme](modeling-data.md)
