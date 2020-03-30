---
title: Azure Cosmos DB için SQL anahtar kelimeleri
description: Azure Cosmos DB için SQL anahtar kelimeleri hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: mjbrown
ms.openlocfilehash: f2da2695ec20eac9dd2636104d3314427e60d541
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79498546"
---
# <a name="keywords-in-azure-cosmos-db"></a>Azure Cosmos DB'deki anahtar kelimeler

Bu makalede, Azure Cosmos DB SQL sorgularında kullanılabilecek anahtar kelimeler ayrıntılı olarak belirtilmiştir.

## <a name="between"></a>BETWEEN

Sözcük tabirini `BETWEEN` dize veya sayısal değerler aralıklarına karşı ifade etmek için anahtar sözcüğü kullanabilirsiniz. Örneğin, aşağıdaki sorgu, ilk çocuğun notunun 1-5 olduğu tüm öğeleri, dahil olarak döndürür.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

Aşağıdaki örnekte `BETWEEN` olduğu `SELECT` gibi, yan tümcedeki anahtar kelimeyi de kullanabilirsiniz.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

SQL API'de, ANSI SQL'in aksine, aralık sorgularını karışık türlerdeki özelliklere karşı ifade edebilirsiniz. Örneğin, `grade` bazı öğelerdeki `5` gibi bir sayı ve `grade4` diğeröğelerdeki gibi bir dize olabilir. Bu gibi durumlarda, JavaScript'te olduğu gibi, iki `Undefined`farklı tür arasındaki karşılaştırma, öğenin atlanabilmesi için sonuç verir.

> [!TIP]
> Daha hızlı sorgu yürütme süreleri için, `BETWEEN` yan tümcenin filtre uyguladığı sayısal özelliklere veya yollara karşı aralık dizin türü kullanan bir dizin oluşturma ilkesi oluşturun.

## <a name="distinct"></a>DISTINCT

Anahtar `DISTINCT` kelime, sorgunun projeksiyonundaki yinelenenleri ortadan kaldırır.

Bu örnekte, sorgu her soyadı için değerleri projeleri:

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

Benzersiz nesneleri de yansıtabilirsiniz. Bu durumda, soyad alanı iki belgeden birinde bulunmaz, bu nedenle sorgu boş bir nesneyi döndürür.

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

DISTINCT, bir alt sorgu içindeki projeksiyonda da kullanılabilir:

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

Bu sorgu, yinelenenleri kaldırılmış olan her çocuğun givenName içeren bir dizi yi yitir. Bu dizi, Alt Adlar olarak adlanır ve dış sorguda yansıtılır.

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

Toplu sistem işlevi ve alt sorgusu `DISTINCT` olan sorgular desteklenmez. Örneğin, aşağıdaki sorgu desteklenmez:

```sql
SELECT COUNT(1) FROM (SELECT DISTINCT f.lastName FROM f)
```

## <a name="in"></a>IN

Belirtilen bir değerin listedeki herhangi bir değerle eşleşip eşleşmediğini kontrol etmek için IN anahtar sözcüğini kullanın. Örneğin, aşağıdaki sorgu tüm aile öğelerini `WakefieldFamily` `AndersenFamily`döndürür. `id`

```sql
    SELECT *
    FROM Families
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')
```

Aşağıdaki örnek, durum belirtilen değerlerden herhangi biri olduğu tüm öğeleri döndürür:

```sql
    SELECT *
    FROM Families
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")
```

SQL API, FROM kaynağındaki anahtar kelime aracılığıyla eklenen yeni bir yapıyla [JSON dizileri üzerinde yeniden derecelendirme](sql-query-object-array.md#Iteration)desteği sağlar.

Bölüm anahtarınızı filtreye `IN` eklerseniz, sorgunuz otomatik olarak yalnızca ilgili bölümlere filtre olur.

## <a name="top"></a>TOP

TOP anahtar sözcüğü, `N` ilk sorgu sonuçlarını tanımlanmamış bir sırada döndürür. En iyi uygulama olarak, `ORDER BY` sonuçları ilk sıralı `N` değer sayısıyla sınırlamak için yan tümceyle BIRLIKTE TOP'u kullanın. Bu iki yan tümceyi birleştirmek, TOP'un hangi satırları etkilediğini tahmin edilebilir bir şekilde göstermenin tek yoludur.

Top'u aşağıdaki örnekte olduğu gibi sabit bir değerle veya parametreli sorguları kullanarak değişken değeriyle kullanabilirsiniz.

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
- [Alt sorgular](sql-query-subquery.md)