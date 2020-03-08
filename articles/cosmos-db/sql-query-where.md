---
title: Azure Cosmos DB WHERE yan tümcesi
description: Azure Cosmos DB için SQL WHERE yan tümcesi hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: tisande
ms.openlocfilehash: 483a0533eafc81ef8698d260a753062ae074f6d4
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78898769"
---
# <a name="where-clause-in-azure-cosmos-db"></a>Azure Cosmos DB WHERE yan tümcesi

İsteğe bağlı WHERE yan tümcesi (`WHERE <filter_condition>`), kaynak JSON öğelerinin sorgunun sonuçlara dahil etmek için karşılaması gereken koşulları belirtir. Bir JSON öğesi, sonuç için kabul edilecek `true` için belirtilen koşulları değerlendirmelidir. Dizin Katmanı WHERE yan tümcesini kullanarak sonucun parçası olabilecek en küçük kaynak öğeleri alt kümesini tespit edebilir.
  
## <a name="syntax"></a>Sözdizimi
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler

- `<filter_condition>`  
  
   Döndürülecek belgeler için karşılanması gereken bir koşulu belirtir.  
  
- `<scalar_expression>`  
  
   Hesaplanmasını değeri gösteren ifade. Ayrıntılar için bkz. [skalar ifadeler](sql-query-scalar-expressions.md) .  
  
## <a name="remarks"></a>Açıklamalar
  
  Filtre olarak belirtilen bir ifade döndürülecek belge sırada koşul true olarak değerlendirilmelidir. Yalnızca Boolean değeri `true` koşulu karşılar, diğer tüm değerler: tanımsız, null, yanlış, sayı, dizi veya nesne koşulu karşılamıyor.

  Bölüm anahtarınızı bir eşitlik filtresinin parçası olarak `WHERE` yan tümcesine eklerseniz, sorgunuz otomatik olarak yalnızca ilgili bölümlere filtre uygulanır.

## <a name="examples"></a>Örnekler

Aşağıdaki sorgu, değeri `AndersenFamily`olan `id` bir özelliği içeren öğeleri ister. `id` özelliğine sahip olmayan ya da değeri `AndersenFamily`eşleşmeyen herhangi bir öğeyi dışlar.

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şunlardır:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>WHERE yan tümcesindeki skaler ifadeler

Önceki örnekte, bir basit eşitlik sorgu gösterdi. SQL API ayrıca çeşitli [Skalar ifadeleri](sql-query-scalar-expressions.md)destekler. En sık kullanılan ikili ve birli ifadelerdir. Kaynak JSON nesne özelliği başvurularından da geçerli ifadelerdir.

Aşağıdaki desteklenen ikili işleçleri kullanabilirsiniz:  

|**İşleç türü**  | **Değerler** |
|---------|---------|
|Aritmetik | +,-,*,/,% |
|bit düzeyinde    | \|, &, ^, < <, > >, > > > (sıfır dolgulu sağa kaydırma) |
|Mantıksal    | VE, VEYA DEĞİL      |
|Karşılaştırma | =,! =, &lt;, &gt;, &lt;=, &gt;=, < > |
|Dize     |  \|\| (Birleştir) |

Aşağıdaki sorgular ikili işleçler kullanır:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

Ayrıca, aşağıdaki örneklerde gösterildiği gibi, sorgularda DEĞIL Birli İşleçler +,-, ~, kullanabilirsiniz:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

Sorgularda Özellik başvurularını da kullanabilirsiniz. Örneğin `SELECT * FROM Families f WHERE f.isRegistered`, `true`değerine eşit değere sahip `isRegistered` özelliğini içeren JSON öğesini döndürür. `false`, `null`, `Undefined`, `<number>`, `<string>`, `<object>`veya `<array>`gibi başka herhangi bir değer, öğeyi sonuçtan dışlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [IN anahtar sözcüğü](sql-query-keywords.md#in)
- [FROM yan tümcesi](sql-query-from.md)