---
title: Azure Cosmos DB'de diziler ve nesnelerle çalışma
description: Azure Cosmos DB'de diziler ve nesneler oluşturmak için SQL sözdizimini öğrenin. Bu makalede, dizi nesneleri üzerinde işlemleri gerçekleştirmek için bazı örnekler de sağlar
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 5b2801b0a71f04803955e9d8bc18a97133019996
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246558"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Azure Cosmos DB'de diziler ve nesnelerle çalışma

Azure Cosmos DB SQL API'nin önemli bir özelliği dizi ve nesne oluşturmadır.

## <a name="arrays"></a>Diziler

Aşağıdaki örnekte gösterildiği gibi diziler oluşturabilirsiniz:

```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

Sonuçlar:

```json
    [
      {
        "CityState": [
          "Seattle",
          "WA"
        ]
      },
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]
```

[Ayrıca,](sql-query-subquery.md#array-expression) [alt sorgunun](sql-query-subquery.md) sonuçlarından bir dizi oluşturmak için ARRAY ifadesini de kullanabilirsiniz. Bu sorgu, bir dizideki tüm farklı verilen çocukların adlarını alır.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

## <a name="iteration"></a><a id="Iteration"></a>Yineleme

SQL API, FROM kaynağındaki [IN anahtar kelimesi](sql-query-keywords.md#in) aracılığıyla eklenen yeni bir yapıyla JSON dizileri üzerinde yeniden derecelendirme desteği sağlar. Aşağıdaki örnekte:

```sql
    SELECT *
    FROM Families.children
```

Sonuçlar:

```json
    [
      [
        {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        }, 
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

Sonraki sorgu `children` `Families` kapsayıcıüzerinde yineleme gerçekleştirir. Çıktı dizisi önceki sorgudan farklıdır. Bu örnek, `children`sonuçları böler ve tek bir diziye böler:  

```sql
    SELECT *
    FROM c IN Families.children
```

Sonuçlar:

```json
    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]
```

Aşağıdaki örnekte gösterildiği gibi, dizinin her bir girişine daha fazla filtre uygulayabilirsiniz:

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

Sonuçlar:

```json
    [{
      "givenName": "Lisa"
    }]
```

Ayrıca, bir dizi yinelemesonucu üzerinde toplayabilir. Örneğin, aşağıdaki sorgu tüm aileler arasında çocuk sayısını sayar:

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

Sonuçlar:

```json
    [
      {
        "$1": 3
      }
    ]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Birleştirme](sql-query-join.md)