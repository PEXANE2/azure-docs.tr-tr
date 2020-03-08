---
title: Azure Cosmos DB için SQL anahtar sözcükleri
description: Azure Cosmos DB için SQL anahtar kelimeleri hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: 711e961bd5eb1607e2e6f11b0b5762423d78c0e7
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78898784"
---
# <a name="keywords-in-azure-cosmos-db"></a>Azure Cosmos DB anahtar sözcükler
Bu makalede, Azure Cosmos DB SQL sorgularında kullanılabilecek anahtar sözcükler ayrıntılı olarak açıklanır.

## <a name="between"></a>ARALARıNDA

ANSI SQL 'de olduğu gibi, sorguları dize veya sayısal değer aralıklarına göre ifade etmek için BETWEEN anahtar sözcüğünü kullanabilirsiniz. Örneğin, aşağıdaki sorgu ilk alt öğenin 1-5, dahil olduğu tüm öğeleri döndürür.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

ANSI SQL 'den farklı olarak, aşağıdaki örnekte olduğu gibi FROM yan tümcesindeki BETWEEN yan tümcesini de kullanabilirsiniz.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

SQL API 'de, ANSI SQL 'den farklı olarak, Aralık sorguları karışık türlerin özelliklerine göre ifade edebilirsiniz. Örneğin, `grade` bazı öğelerde `5` gibi bir sayı ve diğer `grade4` gibi bir dize olabilir. Bu durumlarda, JavaScript 'de olduğu gibi, iki farklı tür arasındaki karşılaştırma `Undefined`sonuçlanır, bu nedenle öğe atlanır.

> [!TIP]
> Daha hızlı sorgu yürütme süreleri için, bir Aralık dizin türünü kullanan bir dizin oluşturma ilkesi oluşturun herhangi bir sayısal özellik veya BETWEEN yan tümcesi filtreleyen yollar.

## <a name="distinct"></a>AYRı

DISTINCT anahtar sözcüğü sorgunun projeksiyonundaki yinelemeleri ortadan kaldırır.

Bu örnekte, her bir soyadı için sorgu proje değerleri:

```sql
SELECT DISTINCT VALUE f.lastName
FROM Families f
```

Sonuçlar şunlardır:

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

Sonuçlar şunlardır:

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

Sonuçlar şunlardır:

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

Toplam sistem işlevi olan sorgular ve DISTINCT içeren bir alt sorgu desteklenmez. Örneğin, aşağıdaki sorgu desteklenmez:

```sql
SELECT COUNT(1) FROM (SELECT DISTINCT f.lastName FROM f)
```

## <a name="in"></a>IN

Belirtilen değerin listedeki herhangi bir değerle eşleşip eşleşmediğini denetlemek için ın anahtar sözcüğünü kullanın. Örneğin, aşağıdaki sorgu `id` `WakefieldFamily` veya `AndersenFamily`tüm aile öğelerini döndürür.

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

Bölüm anahtarınızı `IN` filtresine eklerseniz, sorgunuz otomatik olarak yalnızca ilgili bölümlere filtre uygulanır.

## <a name="top"></a>Sayfanın Üstü

ÜSTTEKI anahtar sözcüğü, sorgu sonuçlarının ilk `N` sayısını tanımsız bir sırada döndürür. En iyi uygulama olarak, sonuçları sıralı değerlerin ilk `N` sayısıyla sınırlamak için ORDER BY yan tümcesiyle birlikte en üstteki ' ı kullanın. Bu iki yan tümceyi birleştirmek, en üst düzey etkileri tahmin edilebilir olarak göstermek için tek yoldur.

Aşağıdaki örnekte olduğu gibi, ya da parametreli sorguları kullanarak bir değişken değeri ile en üstteki bir sabit değerle kullanabilirsiniz.

```sql
    SELECT TOP 1 *
    FROM Families f
```

Sonuçlar şunlardır:

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
- [Birleştirmeler](sql-query-join.md)
- [Alt](sql-query-subquery.md)