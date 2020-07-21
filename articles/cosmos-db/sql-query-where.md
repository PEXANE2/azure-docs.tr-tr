---
title: Azure Cosmos DB WHERE yan tümcesi
description: Azure Cosmos DB için SQL WHERE yan tümcesi hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: tisande
ms.openlocfilehash: ceffb203ccc2cca1ff6e1c53644cde955c2e0acb
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523511"
---
# <a name="where-clause-in-azure-cosmos-db"></a>Azure Cosmos DB WHERE yan tümcesi

İsteğe bağlı WHERE yan tümcesi ( `WHERE <filter_condition>` ), sorgu için kaynak JSON öğelerinin sonuçlara dahil etmek için karşılaması gereken koşulları belirtir. Bir JSON öğesi `true` , sonuç için göz önünde bulundurulması için belirtilen koşulları değerlendirmelidir. Dizin Katmanı WHERE yan tümcesini kullanarak sonucun parçası olabilecek en küçük kaynak öğeleri alt kümesini tespit edebilir.
  
## <a name="syntax"></a>Söz dizimi
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler

- `<filter_condition>`  
  
   Döndürülecek belgelerin karşılanabileceği koşulu belirtir.  
  
- `<scalar_expression>`  
  
   Hesaplanacağı değeri temsil eden ifade. Ayrıntılar için bkz. [skalar ifadeler](sql-query-scalar-expressions.md) .  
  
## <a name="remarks"></a>Açıklamalar
  
  Belgenin döndürülmesi için filtre koşulu true olarak değerlendirilmelidir. Koşulu yalnızca Boole değeri `true` karşılanacak, diğer tüm değerler: tanımsız, null, yanlış, sayı, dizi veya nesne koşulu karşılamaz.

  Bölüm anahtarınızı `WHERE` bir eşitlik filtresinin parçası olarak yan tümcesine eklerseniz, sorgunuz otomatik olarak yalnızca ilgili bölümlere filtre uygulanır.

## <a name="examples"></a>Örnekler

Aşağıdaki sorgu, değeri olan bir özelliği içeren öğeleri ister `id` `AndersenFamily` . Özelliği olmayan veya değeri eşleşmeyen herhangi bir öğeyi dışlar `id` `AndersenFamily` .

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

Önceki örnekte basit bir eşitlik sorgusu gösteriliyordu. SQL API ayrıca çeşitli [Skalar ifadeleri](sql-query-scalar-expressions.md)destekler. En yaygın olarak kullanılan ikili ve birli ifadelerdir. Kaynak JSON nesnesinden Özellik başvuruları da geçerli ifadelerdir.

Aşağıdaki desteklenen ikili işleçleri kullanabilirsiniz:  

|**İşleç türü**  | **Değerler** |
|---------|---------|
|Tiğinin | +,-,*,/,% |
|Operatörün    | \|, &, ^,  <<,  >>,  >>>  (sıfır dolgulu sağa kaydırma) |
|Mantıksal    | VE, VEYA DEĞIL      |
|Karşılaştırma | =,! =, &lt; , &gt; , &lt; =, &gt; =,  <> |
|Dize     |  \|\|karakter |

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

Sorgularda Özellik başvurularını da kullanabilirsiniz. Örneğin, değerine `SELECT * FROM Families f WHERE f.isRegistered` eşit olan özelliği IÇEREN JSON öğesini döndürür `isRegistered` `true` . ,,,,,, Veya gibi herhangi bir değer, `false` `null` `Undefined` `<number>` `<string>` `<object>` `<array>` sonucu sonuçtan dışlar. Buna ek olarak, `IS_DEFINED` belirli BIR JSON özelliğinin varlığına veya yokluğuna göre sorgulamak için tür denetimi işlevini de kullanabilirsiniz. Örneğin, `SELECT * FROM Families f WHERE NOT IS_DEFINED(f.isRegistered)` değeri olmayan herhangi BIR JSON öğesini döndürür `isRegistered` .

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [IN anahtar sözcüğü](sql-query-keywords.md#in)
- [FROM yan tümcesi](sql-query-from.md)
