---
title: Azure Cosmos DB WHERE yan tümcesi
description: Azure Cosmos DB için SQL WHERE yan tümcesi hakkında bilgi edinin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: cd5643d8be06afcd43c5bfe38d6f5e9caa6f906e
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72326633"
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
  
  Filtre olarak belirtilen bir ifade döndürülecek belge sırada koşul true olarak değerlendirilmelidir. Başka bir değer koşulu, Boole değeri true yerine getirecek yalnızca: tanımsız, null, false, sayı, dizi veya nesne karşılamaz koşul. 

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
|bit düzeyinde    | \|, &, ^, <<>>,, >>> (sıfır dolgu sağa kaydırma) |
|Mantıksal    | VE, VEYA DEĞİL      |
|Karşılaştırma | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|Dize     |  \|\| (birleştirme) |

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

- [Başlangıç](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [FROM yan tümcesi](sql-query-from.md)