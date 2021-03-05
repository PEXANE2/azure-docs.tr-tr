---
title: Azure Cosmos DB için SQL anahtar sözcükleri
description: Azure Cosmos DB için SQL anahtar kelimeleri hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 01/20/2021
ms.author: tisande
ms.openlocfilehash: 1f3c4ef56feb77e9b01375b8b5dbdb567f5bfadb
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102179978"
---
# <a name="keywords-in-azure-cosmos-db"></a>Azure Cosmos DB anahtar sözcükler
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

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

`DISTINCT` , bir alt sorgu içindeki projeksiyde kullanılabilir:

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

## <a name="like"></a>LIKE

Belirli bir karakter dizesinin belirtilen bir Düzenle eşleşip eşleşmediğini bağlı olarak bir Boole değeri döndürür. Bir desenler, düzenli karakter ve joker karakterler içerebilir. `LIKE`Anahtar sözcüğü ya da [Regexmatch](sql-query-regexmatch.md) sistem işlevini kullanarak mantıksal olarak eşdeğer sorgular yazabilirsiniz. Seçtiğiniz dizinden bağımsız olarak aynı dizin kullanımını gözlemleyeceksiniz. Bu nedenle, `LIKE` söz dizimini normal ifadelerden daha fazla tercih ediyorsanız kullanmanız gerekir.

> [!NOTE]
> `LIKE`Bir dizini kullanabileceğinden, karşılaştırmak istediğiniz özellikler için [bir Aralık dizini oluşturmanız](./index-policy.md) gerekir `LIKE` .

Aşağıdaki joker karakterleri şu şekılde kullanabilirsiniz:

| Joker karakter | Açıklama                                                  | Örnek                                     |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------- |
| %                    | Herhangi bir sıfır veya daha fazla karakter dizesi                      | BURADA c. Description, "% SO% PS%" gıbı      |
| _ (alt çizgi)     | Herhangi bir tek karakter                                       | BURADA c. Description "% SO_PS%" gıbı      |
| [ ]                  | Belirtilen Aralık ([a-f]) veya set ([abcdef]) içinde herhangi bir tek karakter. | BURADA c. Description, "% SO [t-z] PS%" gıbı  |
| [^]                  | Belirtilen Aralık ([^ a-f]) veya set ([^ abcdef]) içinde olmayan tek bir karakter. | BURADA c. Description, "% SO [^ abc] PS%" gıbı |


### <a name="using-like-with-the--wildcard-character"></a>% Joker karakterle BEĞENME

`%`Karakter, sıfır veya daha fazla karakterden oluşan herhangi bir dizeyle eşleşir. Örneğin, `%` deseninin başlangıcına ve sonuna bir koyarak aşağıdaki sorgu, içeren bir açıklama içeren tüm öğeleri döndürür `fruit` :

```sql
SELECT *
FROM c
WHERE c.description LIKE "%fruit%"
```

`%`Düzenin sonunda yalnızca bir karakter kullandıysanız, yalnızca ile başlayan bir açıklama içeren öğeleri geri döndürdük `fruit` :

```sql
SELECT *
FROM c
WHERE c.description LIKE "fruit%"
```


### <a name="using-not-like"></a>BEĞENMIYOR

Aşağıdaki örnek, bir açıklama içeren tüm öğeleri döndürür `fruit` :

```sql
SELECT *
FROM c
WHERE c.description NOT LIKE "%fruit%"
```

### <a name="using-the-escape-clause"></a>Kaçış yan tümcesini kullanma

KAÇıŞ yan tümcesini kullanarak bir veya daha fazla joker karakter içeren desenler için arama yapabilirsiniz. Örneğin, dizeyi içeren açıklamaları aramak isterseniz, `20-30%` bunu `%` joker karakter olarak yorumlamak istemezsiniz.

```sql
SELECT *
FROM c
WHERE c.description LIKE '%20-30!%%' ESCAPE '!'
```

### <a name="using-wildcard-characters-as-literals"></a>Joker karakterleri değişmez değer olarak kullanma

Joker karakterleri, değişmez karakterler olarak değerlendirmek için köşeli ayraç içine alabilirsiniz. Bir joker karakteri köşeli ayraç içine aldığınızda, tüm özel öznitelikleri kaldırırsınız. İşte bazı örnekler:

| Desen           | Anlamı |
| ----------------- | ------- |
| "20-30 [%]" GIBI | % 20-30  |
| "[_] N" gıbı     | _n      |
| "[[]" Gıbı    | [       |
| "]" Gıbı        | ]       |

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
