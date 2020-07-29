---
title: Azure Cosmos DB parametreli sorgular
description: SQL parametreli sorguların, Kullanıcı girişi için güçlü işleme ve kaçışı nasıl sağladığını ve SQL ekleme aracılığıyla verilerin yanlışlıkla açıklanmasını nasıl önleyeceğinizi öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: e15a8236723c1efd80f27f2d253e9bbc44af4b0b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74870828"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Azure Cosmos DB parametreli sorgular

Cosmos DB tanıdık @ gösterimi tarafından ifade edilen parametrelere sahip sorguları destekler. Parametreli SQL, Kullanıcı girişi için güçlü işleme ve kaçış sağlar ve SQL ekleme aracılığıyla verilerin yanlışlıkla açıklanmasını önler.

## <a name="examples"></a>Örnekler

Örneğin, parametreleri alan ve olarak bir sorgu yazabilir `lastName` `address.state` ve `lastName` Kullanıcı girişine göre çeşitli değerleri ve bunları çalıştırabilirsiniz `address.state` .

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Daha sonra bu isteği aşağıdaki gibi parametreli bir JSON sorgusu olarak Cosmos DB gönderebilirsiniz:

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

Parametre değerleri geçerli bir JSON olabilir: dizeler, sayılar, Boole değerleri, null, hatta diziler veya iç içe geçmiş JSON. Cosmos DB şemaya daha az olduğundan, parametreler hiçbir türe göre doğrulanmaz.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Belge verilerini modelleme](modeling-data.md)
