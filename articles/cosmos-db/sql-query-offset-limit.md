---
title: Azure Cosmos DB'de OFSET SINIRI yan tümcesi
description: Azure Cosmos DB'de sorgu yaparken belirli değerleri atlamak ve almak için OFSET LIMIT yan tümcesini nasıl kullanacağınızı öğrenin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 3d23676885323e370cee1e9cc9e98c7128faf2e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76771568"
---
# <a name="offset-limit-clause-in-azure-cosmos-db"></a>Azure Cosmos DB'de OFSET SINIRI yan tümcesi

OFSET LIMIT yan tümcesi, sorgudan bir takım değer almak için atlamak için isteğe bağlı bir yan tümcedir. OFSET sayısı ve LIMIT sayısı OFSET LIMIT işurında gereklidir.

OFSET LIMITi bir ORDER BY yan tümcesi ile birlikte kullanıldığında, sonuç kümesi atlama ve sipariş edilen değerleri alarak üretilir. Order BY yan tümcesi kullanılmazsa, deterministic bir değerler sırasına neden olur.

## <a name="syntax"></a>Sözdizimi
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler

- `<offset_amount>`

   Sorgu sonuçlarının atlaması gereken tamsayı sayısını belirtir.

- `<limit_amount>`
  
   Sorgu sonuçlarının içermesi gereken tamsayı sayısını belirtir

## <a name="remarks"></a>Açıklamalar
  
  Hem `OFFSET` sayım hem `LIMIT` de sayım maddesi gereklidir. `OFFSET LIMIT` İsteğe `ORDER BY` bağlı bir yan tümce kullanılırsa, sonuç kümesi sıralanan değerlerin üzerinden atlama yaparak üretilir. Aksi takdirde, sorgu sabit bir değerler sırasını döndürür.

  Mahsup edilen terim `OFFSET LIMIT` sayısı arttıkça, sorgunun RU ücreti artar. Birden çok sayfa sonuç alan sorgular için genellikle devam belirteçlerini kullanmanızı öneririz. Devam belirteçleri, sorgunun daha sonra devam edebileceği yer için bir "yer imi"dir. Eğer kullanırsanız, `OFFSET LIMIT`hiçbir "yer imi" dir. Sorgunun bir sonraki sayfasını döndürmek istiyorsanız, baştan başlamanız gerekir.
  
  Belgeleri tamamen `OFFSET LIMIT` atlamak ve istemci kaynaklarını kaydetmek istediğiniz durumlar için kullanmalısınız. Örneğin, `OFFSET LIMIT` 1000. Arka uçta, `OFFSET LIMIT` atlananlar da dahil olmak üzere her belgeyi yükler. Performans avantajı, gerekli olmayan belgeleri işlemekten kaçınarak istemci kaynaklarında tasarruf etmektir.

## <a name="examples"></a>Örnekler

Örneğin, burada ilk değeri atlayan ve ikinci değeri döndüren (yerleşik şehrin adının sırasına göre) bir sorgu verilmiştir:

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

Burada, ilk değeri atlayan ve ikinci değeri (sipariş vermeden) döndüren bir sorgu aşağıda veda eder:

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
- [MADDEYE GÖRE SİPARİş](sql-query-order-by.md)
