---
title: Azure Cosmos DB ORDER BY yan tümcesi
description: Azure Cosmos DB için SQL ORDER BY yan tümcesi hakkında bilgi edinin. SQL 'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 5cae2bdd7d1f2f26e626c81ea95d2cee3cc8ae13
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75444784"
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
  
   Bkz: [skaler ifadelerin](sql-query-scalar-expressions.md) ayrıntıları bölümü.  
  
- `ASC | DESC`  
  
   Belirtilen sütundaki değerleri artan veya azalan olarak sıralanması gerektiğini belirtir. ASC en düşük değerden yüksek değer sıralar. DESC en yüksek değerden en düşük değere doğru sıralar. ASC varsayılan sıralama sırasıdır. Null değerler, en düşük olası değerler kabul edilir.  
  
## <a name="remarks"></a>Açıklamalar  
  
   ORDER BY yan tümcesi, dizin oluşturma ilkesinin sıralanmakta olan alanlar için bir dizin içermesini gerektirir. Azure Cosmos DB sorgusu çalışma zamanı, hesaplanan özelliklere karşı değil, özellik adına göre sıralamayı destekler. Azure Cosmos DB birden çok SıRALAMAYı destekler. Bir sorguyu birden çok sıra özellikleriyle çalıştırmak için, sıralanan alanlarda bir [bileşik dizin](index-policy.md#composite-indexes) tanımlamanız gerekir.
   
> [!Note] 
> .NET SDK 3.4.0 veya üstünü kullanırken, sıralanan özellikler bazı belgeler için tanımsız olabileceğinden, bu özelliklerde açıkça bir dizin oluşturmanız gerekir. Varsayılan dizin oluşturma ilkesi, sıralama özelliğinin tanımsız olduğu belgelerin alınmasına izin vermez.

## <a name="examples"></a>Örnekler

Örneğin, aileleri, yerleşik şehir adının artan sırada alan bir sorgu aşağıda verilmiştir:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

Sonuçlar:

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

Sonuçlar:

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

Ayrıca, birden çok özelliğe göre sıralama yapabilirsiniz. Birden çok özelliğe göre sipariş eden bir sorgu [bileşik dizin](index-policy.md#composite-indexes)gerektirir. Şu sorguyu inceleyin:

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.address.city ASC, f.creationDate DESC
```

Bu sorgu, Aile `id` şehir adının artan sırasına göre alır. Birden çok öğe aynı şehir adına sahip ise, sorgu `creationDate` göre azalan sırada sıraya alınır.

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [SELECT yan tümcesi](sql-query-select.md)
- [Konum SıNıRı yan tümcesi](sql-query-offset-limit.md)
