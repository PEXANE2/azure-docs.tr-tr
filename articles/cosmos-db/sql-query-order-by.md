---
title: Azure Cosmos DB ORDER BY yan tümcesi
description: Azure Cosmos DB için SQL ORDER BY yan tümcesi hakkında bilgi edinin. SQL 'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: tisande
ms.openlocfilehash: b88184be39a41ec42f8fb304a7511073f645f1cb
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77188743"
---
# <a name="order-by-clause-in-azure-cosmos-db"></a>Azure Cosmos DB ORDER BY yan tümcesi

İsteğe bağlı ORDER BY yan tümcesi sorgu tarafından döndürülen sonuçların sıralama düzenini belirtir.

## <a name="syntax"></a>Sözdizimi
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
```  

## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<sort_specification>`  
  
   Bir özellik ya da üzerinde sorgu sonucu kümesini sıralama yapılacak ifade belirtir. Bir sıralama sütunu, ad veya özellik diğer adı olarak belirtilebilir.  
  
   Birden çok özellik belirtilebilir. Özellik adları benzersiz olmalıdır. ORDER BY yan tümcesindeki sıralama özelliklerinin sırası, sıralanmış sonuç kümesinin organizasyonunu tanımlar. Diğer bir deyişle, sonuç kümesi ilk özelliği tarafından sıralanır ve ardından bu sıralı liste ikinci özelliği vb. göre sıralanır.  
  
   ORDER BY yan tümcesinde başvurulan özellik adları, select listesindeki bir özelliğe ya da herhangi bir belirsizlikleri olmadan FROM yan tümcesinde belirtilen koleksiyonda tanımlanan bir özelliğe karşılık gelmelidir.  
  
- `<sort_expression>`  
  
   Sorgu sonuç kümesinin sıralaması yapılacak bir veya daha fazla özelliği veya ifadeyi belirtir.  
  
- `<scalar_expression>`  
  
   Ayrıntılar için [skaler ifadeler](sql-query-scalar-expressions.md) bölümüne bakın.  
  
- `ASC | DESC`  
  
   Belirtilen sütundaki değerleri artan veya azalan olarak sıralanması gerektiğini belirtir. ASC en düşük değerden yüksek değer sıralar. DESC en yüksek değerden en düşük değere doğru sıralar. ASC varsayılan sıralama sırasıdır. Null değerler, en düşük olası değerler kabul edilir.  
  
## <a name="remarks"></a>Açıklamalar  
  
   `ORDER BY` yan tümcesi, dizin oluşturma ilkesinin sıralanmakta olan alanlar için bir dizin içermesini gerektirir. Azure Cosmos DB sorgusu çalışma zamanı, hesaplanan özelliklere karşı değil, özellik adına göre sıralamayı destekler. Azure Cosmos DB birden çok `ORDER BY` özelliği destekler. Bir sorguyu birden çok sıra özellikleriyle çalıştırmak için, sıralanan alanlarda bir [bileşik dizin](index-policy.md#composite-indexes) tanımlamanız gerekir.

> [!Note]
> Sıralanan özellikler bazı belgeler için tanımsız olabilir ve bunları bir SıRALAMA sorgusuna almak istiyorsanız, bu yolu dizine açıkça eklemeniz gerekir. Varsayılan dizin oluşturma ilkesi, sıralama özelliğinin tanımsız olduğu belgelerin alınmasına izin vermez. [Bazı eksik alanları olan belgelerde örnek sorguları gözden geçirin](#documents-with-missing-fields).

## <a name="examples"></a>Örnekler

Örneğin, aileleri, yerleşik şehir adının artan sırada alan bir sorgu aşağıda verilmiştir:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

Sonuçlar şunlardır:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Aşağıdaki sorgu, öğe oluşturma tarihleri sırasına göre aile `id`s 'yi alır. Öğe `creationDate`, *Dönem süresini*temsil eden bir sayı veya 1 Ocak 1970 ' den beri saniye cinsinden geçen süreyi temsil eder.

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.creationDate DESC
```

Sonuçlar şunlardır:

```json
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472
      }
    ]
```

Ayrıca, birden çok özelliğe göre sıralama yapabilirsiniz. Birden çok özelliğe göre sipariş eden bir sorgu [bileşik dizin](index-policy.md#composite-indexes)gerektirir. Aşağıdaki sorguyu göz önünde bulundurun:

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.address.city ASC, f.creationDate DESC
```

Bu sorgu, Aile `id` şehir adının artan sırasına göre alır. Birden çok öğe aynı şehir adına sahip ise, sorgu `creationDate` göre azalan sırada sıraya alınır.

## <a name="documents-with-missing-fields"></a>Eksik alanları olan belgeler

Varsayılan dizin oluşturma ilkesiyle kapsayıcılara karşı çalıştırılan `ORDER BY` sorguları, Sort özelliğinin tanımsız olduğu belgeleri döndürmeyecektir. Sıralama özelliğinin tanımsız olduğu belgeleri eklemek isterseniz, bu özelliği dizin oluşturma ilkesine açıkça eklemeniz gerekir.

Örneğin, aşağıdaki `"/*"`yanı sıra herhangi bir yolu açıkça içermeyen bir dizin oluşturma ilkesiyle bir kapsayıcı verilmiştir:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": []
}
```

`Order By` yan tümcesinde `lastName` içeren bir sorgu çalıştırırsanız, sonuçlar yalnızca tanımlı bir `lastName` özelliği olan belgeleri dahil eder. `lastName` için açıkça eklenmiş bir yol tanımlamadık, bu nedenle `lastName` olmayan tüm belgeler sorgu sonuçlarında görünmez.

Aşağıda, biri tanımlı `lastName` olmayan iki belge üzerinde `lastName` sıralayan bir sorgu verilmiştir:

```sql
    SELECT f.id, f.lastName
    FROM Families f
    ORDER BY f.lastName
```

Sonuçlar yalnızca tanımlı `lastName`olan belgeyi içerir:

```json
    [
        {
            "id": "AndersenFamily",
            "lastName": "Andersen"
        }
    ]
```

Kapsayıcının dizin oluşturma ilkesini `lastName`için açıkça bir yol içerecek şekilde güncelleştirdiğimiz takdirde sorgu sonuçlarında tanımsız sıralama özelliğine sahip belgeler dahil edeceğiz. Bu skaler değere (ve bundan sonra değil) yol açacak yolu açıkça tanımlamanız gerekir. Özelliği `lastName` açıkça dizinlemesini ve bundan sonra başka iç içe yol olmamasını sağlamak için dizin oluşturma ilkesinde yol tanımınızda `?` karakterini kullanmanız gerekir.

Sorgu sonuçlarında tanımsız `lastName` bulunan belgelerin olmasına izin veren örnek bir dizin oluşturma ilkesi aşağıda verilmiştir:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/lastName/?"
        },
        {
            "path": "/*"
        }
    ],
    "excludedPaths": []
}
```

Aynı sorguyu yeniden çalıştırırsanız, eksik olan belgeler sorgu sonuçlarında ilk olarak görünür `lastName`:

```sql
    SELECT f.id, f.lastName
    FROM Families f
    ORDER BY f.lastName
```

Sonuçlar şunlardır:

```json
[
    {
        "id": "WakefieldFamily"
    },
    {
        "id": "AndersenFamily",
        "lastName": "Andersen"
    }
]
```

Sıralama düzenini `DESC`olarak değiştirirseniz, eksik `lastName` belgeler sorgu sonuçlarında son görünür:

```sql
    SELECT f.id, f.lastName
    FROM Families f
    ORDER BY f.lastName DESC
```

Sonuçlar şunlardır:

```json
[
    {
        "id": "AndersenFamily",
        "lastName": "Andersen"
    },
    {
        "id": "WakefieldFamily"
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB'de dizin oluşturma ilkeleri](index-policy.md)
- [Konum SıNıRı yan tümcesi](sql-query-offset-limit.md)
