---
title: Azure Cosmos DB için SQL anahtar sözcükleri
description: Azure Cosmos DB için SQL anahtar kelimeleri hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: tisande
ms.openlocfilehash: 069548b9b69ef6f7f6bde85ede830d97f3d312db
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81261576"
---
# <a name="keywords-in-azure-cosmos-db"></a>Azure Cosmos DB anahtar sözcükler

Bu makalede, Azure Cosmos DB SQL sorgularında kullanılabilecek anahtar sözcükler ayrıntılı olarak açıklanır.

## <a name="between"></a>BETWEEN

`BETWEEN`Sorguları dize veya sayısal değer aralıklarına göre ifade etmek için anahtar sözcüğünü kullanabilirsiniz. Örneğin, aşağıdaki sorgu ilk alt öğenin 1-5, dahil olduğu tüm öğeleri döndürür.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

`BETWEEN` `SELECT` Aşağıdaki örnekte olduğu gibi yan tümcesindeki anahtar sözcüğünü de kullanabilirsiniz.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

SQL API 'de, ANSI SQL 'den farklı olarak, Aralık sorguları karışık türlerin özelliklerine göre ifade edebilirsiniz. Örneğin, `grade` `5` bazı öğeler ve diğerleri gibi bir dize gibi bir sayı olabilir `grade4` . Bu durumlarda, JavaScript 'de olduğu gibi, iki farklı tür arasındaki karşılaştırma ile sonuçlanır, bu `Undefined` nedenle öğe atlanır.

> [!TIP]
> Daha hızlı sorgu yürütme süreleri için, bir Aralık dizin türünü kullanan bir dizin oluşturma ilkesi oluşturun ve bu yan tümce filtre uygulayan herhangi bir sayısal özellik veya yol `BETWEEN` .

## <a name="distinct"></a>DISTINCT

`DISTINCT`Anahtar sözcüğü sorgunun projeksiyonundaki yinelemeleri ortadan kaldırır.

Bu örnekte, her bir soyadı için sorgu proje değerleri:

```sql
SELECT DISTINCT VALUE f.lastName
FROM Families f
```

Sonuçlar:

```json
[
    "Andersen"
]
```

Ayrıca, benzersiz nesneleri de proje yapabilirsiniz. Bu durumda, lastName alanı iki belgeden birinde yok, bu nedenle sorgu boş bir nesne döndürüyor.

```sql
SELECT DISTINCT f.lastName
FROM Families f
```

Sonuçlar:

```json
[
    {
        "lastName": "Andersen"
    },
    {}
]
```

DISTINCT, bir alt sorgu içindeki projeksiyde kullanılabilir:

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

Bu sorgu, her bir çocuğun yinelenenleri kaldırılmış olan 1 ' i içeren bir diziyi projeler. Bu dizi, dış sorguda ChildNames ve yansıtılan olarak diğer ad.

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

Toplu sistem işlevine ve alt sorgusuna sahip sorgular `DISTINCT` desteklenmez. Örneğin, aşağıdaki sorgu desteklenmez:

```sql
SELECT COUNT(1) FROM (SELECT DISTINCT f.lastName FROM f)
```

## <a name="in"></a>IN

Belirtilen değerin listedeki herhangi bir değerle eşleşip eşleşmediğini denetlemek için ın anahtar sözcüğünü kullanın. Örneğin, aşağıdaki sorgu, veya olduğu tüm aile öğelerini döndürür `id` `WakefieldFamily` `AndersenFamily` .

```sql
    SELECT *
    FROM Families
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')
```

Aşağıdaki örnek, durumun belirtilen değerlerden herhangi biri olduğu tüm öğeleri döndürür:

```sql
    SELECT *
    FROM Families
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")
```

SQL API 'si, FROM kaynağında ın anahtar sözcüğüyle eklenen yeni bir yapı ile [JSON dizileri üzerinde yineleme](sql-query-object-array.md#Iteration)desteği sağlar.

Bölüm anahtarınızı `IN` filtreye eklerseniz, sorgunuz otomatik olarak yalnızca ilgili bölümlere filtre uygulanır.

## <a name="top"></a>TOP

TOP anahtar sözcüğü, ilk `N` sorgu sonucu sayısını tanımsız bir sırada döndürür. En iyi uygulama olarak, `ORDER BY` sonuçları sıralanmış değerlerin ilk sayısıyla sınırlamak için yan tümcesiyle birlikte en üstteki öğesini kullanın `N` . Bu iki yan tümceyi birleştirmek, en üst düzey etkileri tahmin edilebilir olarak göstermek için tek yoldur.

Aşağıdaki örnekte olduğu gibi, ya da parametreli sorguları kullanarak bir değişken değeri ile en üstteki bir sabit değerle kullanabilirsiniz.

```sql
    SELECT TOP 1 *
    FROM Families f
```

Sonuçlar:

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Birleştirme](sql-query-join.md)
- [Alt Sorgular](sql-query-subquery.md)