---
title: Azure Cosmos DB diziler ve nesnelerle çalışma
description: Azure Cosmos DB diziler ve nesneler oluşturmak için SQL sözdizimini öğrenin. Bu makalede, dizi nesnelerinde işlemler gerçekleştirmek için bazı örnekler de sağlanmaktadır
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 01/07/2021
ms.author: tisande
ms.openlocfilehash: f959e4e230c1d9f89ad5141713b6a17a8cbb17a2
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98018930"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Azure Cosmos DB diziler ve nesnelerle çalışma
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Azure Cosmos DB SQL API 'sinin temel bir özelliği dizi ve nesne oluşturma 'dır.

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

[Alt sorgu](sql-query-subquery.md) sonuçlarından bir dizi oluşturmak için [dizi ifadesini](sql-query-subquery.md#array-expression) de kullanabilirsiniz. Bu sorgu, bir dizideki tüm alt öğelerin verilen benzersiz adlarını alır.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

Sonuçlar:

```json
[
    {
        "id": "AndersenFamily",
        "ChildNames": []
    },
    {
        "id": "WakefieldFamily",
        "ChildNames": [
            "Jesse",
            "Lisa"
        ]
    }
]
```

## <a name="iteration"></a><a id="Iteration"></a>Mesinde

SQL API 'si, FROM kaynağında [ın anahtar sözcüğüyle](sql-query-keywords.md#in) birlikte JSON dizileri üzerinde yineleme desteği sağlar. Aşağıdaki örnekte:

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

Sonraki sorgu, kapsayıcıda yineleme gerçekleştirir `children` `Families` . Çıkış dizisi, önceki sorgudan farklı. Bu örnek `children` , sonuçları böler ve sonuçları tek bir diziye düzleştirir:  

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

Aşağıdaki örnekte gösterildiği gibi, her bir dizi girişi üzerinde daha fazla filtre uygulayabilirsiniz:

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

Ayrıca, bir dizi yinelemesinin sonucunu toplayabilirsiniz. Örneğin, aşağıdaki sorgu tüm aileler arasındaki alt öğe sayısını sayar:

```sql
SELECT COUNT(1) AS Count
FROM child IN Families.children
```

Sonuçlar:

```json
[
  {
    "Count": 3
  }
]
```

> [!NOTE]
> Yineleme için ın anahtar sözcüğünü kullanırken, dizi dışında herhangi bir özelliği filtrelemez veya projenizle. Bunun yerine [birleştirmeleri](sql-query-join.md)kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Birleştirme](sql-query-join.md)
