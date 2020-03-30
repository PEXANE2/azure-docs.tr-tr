---
title: Azure Cosmos DB'deki WHERE yan tümcesi
description: Azure Cosmos DB için SQL WHERE yan tümcesi hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: tisande
ms.openlocfilehash: 483a0533eafc81ef8698d260a753062ae074f6d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898769"
---
# <a name="where-clause-in-azure-cosmos-db"></a>Azure Cosmos DB'deki WHERE yan tümcesi

İsteğe bağlı`WHERE <filter_condition>`WHERE yan tümcesi ( ) koşul(lar) kaynak JSON öğelerinin sorgunun bunları sonuçlara dahil edilebis olması için karşılaması gerektiğini belirtir. Bir JSON öğesi, sonuç `true` için dikkate alınması gereken koşulları değerlendirmelidir. Dizin katmanı, sonucun parçası olabilecek kaynak öğelerin en küçük alt kümesini belirlemek için WHERE yan tümcesini kullanır.
  
## <a name="syntax"></a>Sözdizimi
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler

- `<filter_condition>`  
  
   Belgelerin iade edilmesi için karşılanacak koşulu belirtir.  
  
- `<scalar_expression>`  
  
   Hesaplanacak değeri temsil eden ifade. Ayrıntılar için [Skaler ifadelerine](sql-query-scalar-expressions.md) bakın.  
  
## <a name="remarks"></a>Açıklamalar
  
  Belgenin döndürülebilmesi için filtre koşulu olarak belirtilen bir ifadenin doğru olarak değerlendirilmesi gerekir. Yalnızca Boolean `true` değeri durumu, başka bir değeri karşılar: tanımsız, null, false, Number, Array veya Object koşulu karşılamaz.

  Eşitlik filtresinin `WHERE` bir parçası olarak yan tümceye bölüm anahtarınızı eklerseniz, sorgunuz otomatik olarak yalnızca ilgili bölümlere filtre olur.

## <a name="examples"></a>Örnekler

Aşağıdaki sorgu, `id` değeri `AndersenFamily`. `id` Özelliği olmayan veya değeri eşleşmeyen `AndersenFamily`tüm öğeyi hariç tutar.

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar:

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

Önceki örnekte basit bir eşitlik sorgusu gösterildi. SQL API ayrıca çeşitli [skaler ifadeleri](sql-query-scalar-expressions.md)destekler. En sık kullanılan ikili ve unary ifadelerdir. Kaynak JSON nesnesinden özellik başvuruları da geçerli ifadelerdir.

Desteklenen aşağıdaki ikili işleçleri kullanabilirsiniz:  

|**Operatör türü**  | **Değer** |
|---------|---------|
|Aritmetik | +,-,*,/,% |
|Bitsel    | \|, &, ^, <<, >>, >>> (sıfır dolgu sağ kaydırma) |
|Mantıksal    | VE, YA DA, DEĞIL      |
|Karşılaştırma | =, &lt;!=, &gt; &lt;, &gt;, =, =, <> |
|Dize     |  \|\|(concatenate) |

Aşağıdaki sorgular ikili işleçleri kullanır:

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

Ayrıca aşağıdaki örneklerde gösterildiği gibi unary işleçleri +,-, ~ve NOT sorgularında kullanabilirsiniz:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

Sorgularda özellik başvurularını da kullanabilirsiniz. Örneğin, `SELECT * FROM Families f WHERE f.isRegistered` özelliği içeren JSON öğesini ' `isRegistered` `true`ye eşit değere sahip döndürür. `false`Maddeyi `null` `Undefined` `<number>` `<array>`sonuçtan hariç tutar. `<string>` `<object>`

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [IN anahtar sözcük](sql-query-keywords.md#in)
- [FROM yan tümcesi](sql-query-from.md)