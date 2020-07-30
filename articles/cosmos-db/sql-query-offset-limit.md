---
title: Azure Cosmos DB içindeki konum SıNıRı yan tümcesi
description: Azure Cosmos DB sorgulanırken belirli değerleri atlamak ve almak için, fark SıNıRı yan tümcesini kullanmayı öğrenin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/29/2020
ms.author: tisande
ms.openlocfilehash: 9342c0c10ed4f7287716d21823fddfe992e0568f
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87432715"
---
# <a name="offset-limit-clause-in-azure-cosmos-db"></a>Azure Cosmos DB içindeki konum SıNıRı yan tümcesi

Konum SıNıRı yan tümcesi, atlamak için isteğe bağlı bir yan tümce ve sonra sorgudan birkaç değer alır. Konum sayısı ve sınır sayısı, sınır SıNıRı yan tümcesinde gereklidir.

Sınır sınırı ORDER BY yan tümcesiyle birlikte kullanıldığında, sonuç kümesi, sıralı değerler üzerinde atlama ve alma işlemleri gerçekleştirerek oluşturulur. ORDER BY yan tümcesi kullanılmazsa, değerin belirleyici bir sırası oluşur.

## <a name="syntax"></a>Söz dizimi
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Bağımsız değişkenler

- `<offset_amount>`

   Sorgu sonuçlarının atlayacağı öğelerin tamsayı sayısını belirtir.

- `<limit_amount>`
  
   Sorgu sonuçlarının içermesi gereken öğelerin tamsayı sayısını belirtir

## <a name="remarks"></a>Açıklamalar
  
  `OFFSET`Tümce içinde hem Count hem de `LIMIT` Count gereklidir `OFFSET LIMIT` . İsteğe bağlı bir `ORDER BY` yan tümce kullanılırsa, sonuç kümesi sıralı değerlerin atlanarak oluşturulur. Aksi halde sorgu sabit bir değerler sırası döndürür.

  Bir sorgunun ile olan RU ücreti, `OFFSET LIMIT` kaydırılmakta olan koşulların sayısı arttıkça artacaktır. [Birden fazla sonuç sayfasına](sql-query-pagination.md)sahip sorgularda, genellikle [devamlılık belirteçlerini](sql-query-pagination.md#continuation-tokens)kullanmanızı öneririz. Devamlılık belirteçleri, sorgunun daha sonra sürdürülebileceği yerde bir "yer işaretidir". Kullanırsanız `OFFSET LIMIT` , "Bookmark" yoktur. Sorgunun sonraki sayfasını döndürmek isterseniz, baştan başlamanız gerekir.
  
  `OFFSET LIMIT`Öğeleri tamamen atlamak ve istemci kaynaklarını kaydetmek istediğinizde, bazı durumlarda ' i kullanmanız gerekir. Örneğin, `OFFSET LIMIT` 1000. sorgu sonucuna atlamak istiyorsanız ve sonuçları 1 ile 999 arasında görüntülemeniz gerekmiyorsa ' i kullanmanız gerekir. Arka uçta, `OFFSET LIMIT` Atlanan olanlar da dahil olmak üzere her bir öğeyi yine de yükler. Performans avantajı, gerekli olmayan öğeleri işlemeyi önleyerek istemci kaynaklarındaki tasarruf sağlar.

## <a name="examples"></a>Örnekler

Örneğin, ilk değeri atlayan ve ikinci değeri döndüren (yerleşik şehrin adının sırasıyla) bir sorgu aşağıda verilmiştir:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

Sonuçlar:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

İlk değeri atlayan ve ikinci değeri döndüren (sıralama olmadan) bir sorgu aşağıda verilmiştir:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

Sonuçlar:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [SELECT yan tümcesi](sql-query-select.md)
- [ORDER BY yan tümcesi](sql-query-order-by.md)
