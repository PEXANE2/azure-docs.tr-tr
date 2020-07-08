---
title: Azure Cosmos DB içindeki konum SıNıRı yan tümcesi
description: Azure Cosmos DB sorgulanırken belirli değerleri atlamak ve almak için, fark SıNıRı yan tümcesini kullanmayı öğrenin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 3d23676885323e370cee1e9cc9e98c7128faf2e0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76771568"
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

  Bir sorgunun ile olan RU ücreti, `OFFSET LIMIT` kaydırılmakta olan koşulların sayısı arttıkça artacaktır. Birden fazla sonuç sayfasına sahip sorgularda, genellikle devamlılık belirteçlerini kullanmanızı öneririz. Devamlılık belirteçleri, sorgunun daha sonra sürdürülebileceği yerde bir "yer işaretidir". Kullanırsanız `OFFSET LIMIT` , "Bookmark" yoktur. Sorgunun sonraki sayfasını döndürmek isterseniz, baştan başlamanız gerekir.
  
  `OFFSET LIMIT`Belgeleri tamamen atlamak ve istemci kaynaklarını kaydetmek istediğinizde, bu durumlarda kullanmanız gerekir. Örneğin, `OFFSET LIMIT` 1000. sorgu sonucuna atlamak istiyorsanız ve sonuçları 1 ile 999 arasında görüntülemeniz gerekmiyorsa ' i kullanmanız gerekir. Arka uçta, `OFFSET LIMIT` Atlanan olanlar da dahil olmak üzere her bir belgeyi hala yükler. Performans avantajı, gerekli olmayan belgeleri işlemeyi önleyerek istemci kaynaklarındaki tasarruf sağlar.

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
