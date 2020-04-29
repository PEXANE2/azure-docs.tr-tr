---
title: Azure Cosmos DB ORDER BY yan tümcesi
description: Azure Cosmos DB için SQL ORDER BY yan tümcesi hakkında bilgi edinin. SQL 'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/17/2020
ms.author: tisande
ms.openlocfilehash: 70702ee4a77e8b3c46de4354f3394bca4080d837
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81641389"
---
# <a name="order-by-clause-in-azure-cosmos-db"></a>Azure Cosmos DB ORDER BY yan tümcesi

İsteğe bağlı `ORDER BY` yan tümce, sorgu tarafından döndürülen sonuçlar için sıralama düzenini belirtir.

## <a name="syntax"></a>Sözdizimi
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
```  

## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<sort_specification>`  
  
   Sorgu sonuç kümesinin sıralaması yapılacak bir özellik veya ifade belirtir. Bir sıralama sütunu, ad veya özellik diğer adı olarak belirtilebilir.  
  
   Birden çok özellik belirtilebilir. Özellik adları benzersiz olmalıdır. `ORDER BY` Yan tümcesindeki sıralama özelliklerinin sırası, sıralanmış sonuç kümesinin organizasyonunu tanımlar. Diğer bir deyişle, sonuç kümesi ilk özelliğe göre sıralanır ve ardından sıralı liste ikinci özelliğe göre sıralanır ve bu şekilde devam eder.  
  
   `ORDER BY` Yan tümcesinde başvurulan özellik adları, select listesindeki bir özelliğe ya da herhangi bir belirsizlikleri olmadan `FROM` yan tümcesinde belirtilen koleksiyonda tanımlanan bir özelliğe karşılık gelmelidir.  
  
- `<sort_expression>`  
  
   Sorgu sonuç kümesinin sıralaması yapılacak bir veya daha fazla özelliği veya ifadeyi belirtir.  
  
- `<scalar_expression>`  
  
   Ayrıntılar için [skaler ifadeler](sql-query-scalar-expressions.md) bölümüne bakın.  
  
- `ASC | DESC`  
  
   Belirtilen sütundaki değerlerin artan veya azalan sırada sıralanması gerektiğini belirtir. `ASC`En düşük değerden en yüksek değere göre sıralar. `DESC`en yüksek değerden en düşük değere göre sıralar. `ASC`Varsayılan sıralama düzeni. Null değerler mümkün olan en düşük değer olarak değerlendirilir.  
  
## <a name="remarks"></a>Açıklamalar  
  
   `ORDER BY` Yan tümcesi, dizin oluşturma ilkesinin sıralanmakta olan alanlar için bir dizin içermesini gerektirir. Azure Cosmos DB sorgusu çalışma zamanı, hesaplanan özelliklere karşı değil, özellik adına göre sıralamayı destekler. Azure Cosmos DB birden çok `ORDER BY` özelliği destekler. Bir sorguyu birden çok sıra özellikleriyle çalıştırmak için, sıralanan alanlarda bir [bileşik dizin](index-policy.md#composite-indexes) tanımlamanız gerekir.

> [!Note]
> Sıralanan özellikler bazı belgeler için tanımsız olabilir ve bunları bir SıRALAMA sorgusuna almak istiyorsanız, bu yolu dizine açıkça eklemeniz gerekir. Varsayılan dizin oluşturma ilkesi, sıralama özelliğinin tanımsız olduğu belgelerin alınmasına izin vermez. [Bazı eksik alanları olan belgelerde örnek sorguları gözden geçirin](#documents-with-missing-fields).

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

Aşağıdaki sorgu, öğe oluşturma `id`tarihleri sırasına göre aile öğelerini alır. Öğe `creationDate` , *Dönem süresini*temsil eden bir sayı veya 1 Ocak 1970 ' den beri saniye cinsinden geçen süreyi temsil eder.

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

Bu sorgu, ailenizi `id` şehir adının artan sırasına göre alır. Birden çok öğe aynı şehir adına sahip ise, sorgu azalan sırada sıraya `creationDate` alınır.

## <a name="documents-with-missing-fields"></a>Eksik alanları olan belgeler

`ORDER BY` Varsayılan dizin oluşturma ilkesiyle kapsayıcılara karşı çalıştırılan sorgular, Sort özelliğinin tanımsız olduğu belgeleri döndürmeyecektir. Sıralama özelliğinin tanımsız olduğu belgeleri eklemek isterseniz, bu özelliği dizin oluşturma ilkesine açıkça eklemeniz gerekir.

Örneğin, `"/*"`aşağıdaki gibi herhangi bir yolu açıkça içermeyen bir dizin oluşturma ilkesiyle bir kapsayıcı aşağıda verilmiştir:

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

Yan tümcesine dahil `lastName` olan bir sorgu çalıştırırsanız, sonuçlar yalnızca tanımlanmış bir `lastName` özelliği olan belgeleri dahil eder. `Order By` İçin `lastName` açıkça eklenen bir yol tanımlamadık, `lastName` bu nedenle, olmayan tüm belgeler sorgu sonuçlarında görünmeyecek.

Aşağıda, biri `lastName` tanımlanmış olmayan iki belgeye `lastName` göre sıralama yapan bir sorgu verilmiştir:

```sql
    SELECT f.id, f.lastName
    FROM Families f
    ORDER BY f.lastName
```

Sonuçlar yalnızca tanımlı `lastName`bir belgeyi içerir:

```json
    [
        {
            "id": "AndersenFamily",
            "lastName": "Andersen"
        }
    ]
```

Kapsayıcının dizin oluşturma ilkesini için `lastName`açıkça bir yol içerecek şekilde güncelleştirdiğimiz takdirde sorgu sonuçlarında tanımsız sıralama özelliğine sahip belgeler dahil edeceğiz. Bu skaler değere (ve bundan sonra değil) yol açacak yolu açıkça tanımlamanız gerekir. Özelliği `lastName` açıkça dizinlemesini `?` ve bundan sonra başka iç içe yol olmamasını sağlamak için dizin oluşturma ilkesinde yol tanımınızdaki karakteri kullanmanız gerekir. `Order By` Sorgunuz [bileşik bir dizin](index-policy.md#composite-indexes)kullanıyorsa, sonuçlar her zaman sorgu sonuçlarında tanımsız bir sıralama özelliği olan belgeleri içerecektir.

Sorgu sonuçlarında tanımsız `lastName` bir belge olmasına izin veren örnek bir dizin oluşturma ilkesi aşağıda verilmiştir:

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

Aynı sorguyu yeniden çalıştırırsanız, eksik `lastName` olan belgeler sorgu sonuçlarında önce görünür:

```sql
    SELECT f.id, f.lastName
    FROM Families f
    ORDER BY f.lastName
```

Sonuçlar:

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

Sıralama düzenini olarak `DESC`değiştirirseniz, eksik `lastName` olan belgeler sorgu sonuçlarında son görünür:

```sql
    SELECT f.id, f.lastName
    FROM Families f
    ORDER BY f.lastName DESC
```

Sonuçlar:

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
