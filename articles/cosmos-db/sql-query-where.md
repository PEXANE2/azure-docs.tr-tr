---
title: Azure Cosmos DB WHERE yan tümcesi
description: Azure Cosmos DB için SQL WHERE yan tümcesi hakkında bilgi edinin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 362024868de269ed64a440a25e8c19c5b68bef80
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003462"
---
# <a name="where-clause"></a>WHERE yan tümcesi

İsteğe bağlı where yan tümcesi`WHERE <filter_condition>`(), sorgu için kaynak JSON öğelerinin sonuçlara dahil etmek için karşılaması gereken koşulları belirtir. Bir JSON öğesi, sonuç için göz önünde bulundurulması `true` için belirtilen koşulları değerlendirmelidir. Dizin Katmanı WHERE yan tümcesini kullanarak sonucun parçası olabilecek en küçük kaynak öğeleri alt kümesini tespit edebilir.
  
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

Aşağıdaki sorgu, `id` `AndersenFamily`değeri olan bir özelliği içeren öğeleri ister. Özelliği olmayan veya değeri eşleşmeyen `id` `AndersenFamily`herhangi bir öğeyi dışlar.

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

Sorgularda Özellik başvurularını da kullanabilirsiniz. Örneğin, `SELECT * FROM Families f WHERE f.isRegistered` değerine eşit `isRegistered` olan özelliği içeren JSON öğesini döndürür. `true` , `false`,, `<number>`, `null` `Undefined` ,`<object>`,Veya gibiherhangibirdeğer,`<array>`sonucu sonuçtan dışlar. `<string>` 

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [FROM yan tümcesi](sql-query-from.md)