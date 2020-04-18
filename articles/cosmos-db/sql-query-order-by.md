---
title: Azure Cosmos DB'de MaddeYE GÖRE SİPARİş
description: Azure Cosmos DB için SQL ORDER BY yan tümcesi hakkında bilgi edinin. SQL'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/17/2020
ms.author: tisande
ms.openlocfilehash: 70702ee4a77e8b3c46de4354f3394bca4080d837
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641389"
---
# <a name="order-by-clause-in-azure-cosmos-db"></a>Azure Cosmos DB'de MaddeYE GÖRE SİPARİş

İsteğe `ORDER BY` bağlı yan tümce, sorgu tarafından döndürülen sonuçlar için sıralama sırasını belirtir.

## <a name="syntax"></a>Sözdizimi
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
```  

## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<sort_specification>`  
  
   Sorgu sonucu kümesini sıralamak için bir özellik veya ifade belirtir. Sıralama sütunu ad veya özellik adı olarak belirtilebilir.  
  
   Birden çok özellik belirtilebilir. Özellik adları benzersiz olmalıdır. `ORDER BY` Yan tümcedeki sıralama özelliklerinin sırası, sıralanmış sonuç kümesinin organizasyonunu tanımlar. Diğer bir tarihte, sonuç kümesi ilk özelliğe göre sıralanır ve ardından sıralanan liste ikinci özelliğe göre sıralanır ve böyle devam edin.  
  
   `ORDER BY` Yan tümcede atıfta bulunulan özellik adları, seçili listedeki bir özelliğe veya `FROM` herhangi bir belirsizlik olmaksızın maddede belirtilen koleksiyonda tanımlanan bir özelliğe karşılık gelir.  
  
- `<sort_expression>`  
  
   Sorgu sonucu kümesini sıralamak için bir veya daha fazla özellik veya ifade belirtir.  
  
- `<scalar_expression>`  
  
   Ayrıntılar için [Skaler ifadeleri](sql-query-scalar-expressions.md) bölümüne bakın.  
  
- `ASC | DESC`  
  
   Belirtilen sütundaki değerlerin artan veya azalan sırada sıralanması gerektiğini belirtir. `ASC`en düşük değerden en yüksek değere sıralar. `DESC`en yüksek değerden en düşük değere sıralar. `ASC`varsayılan sıralama sırasıdır. Null değerleri mümkün olan en düşük değerler olarak kabul edilir.  
  
## <a name="remarks"></a>Açıklamalar  
  
   Yan `ORDER BY` tümce, dizin oluşturma ilkesinin sıralanan alanlar için bir dizin içermesini gerektirir. Azure Cosmos DB sorgu çalışma süresi, hesaplanmış özelliklere karşı değil, bir özellik adına karşı sıralamayı destekler. Azure Cosmos DB `ORDER BY` birden çok özelliği destekler. Birden çok ORDER BY özelliğine sahip bir sorguyu çalıştırmak için, sıralanan alanlar üzerinde [bileşik dizin](index-policy.md#composite-indexes) tanımlamanız gerekir.

> [!Note]
> Sıralanan özellikler bazı belgeler için tanımlanmamış olabilir ve bunları ORDER BY sorgusunda almak istiyorsanız, bu yolu açıkça dizine eklemeniz gerekir. Varsayılan dizin oluşturma ilkesi, tür özelliğinin tanımsız olduğu belgelerin alınmasına izin vermez. [Bazı eksik alanları olan belgelerdeki örnek sorguları gözden geçirin.](#documents-with-missing-fields)

## <a name="examples"></a>Örnekler

Örneğin, aileleri yerleşik şehrin adının artan sırasına göre alan bir sorgu aşağıda verilmiştir:

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

Aşağıdaki sorgu, aile `id`s'yi madde oluşturma tarihlerisırasına göre alır. Öğe, `creationDate` 1 Ocak 1970'ten bu yana geçen süreyi saniyecinsinden *geçen çağı*temsil eden bir sayıdır.

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

Ayrıca, birden çok özelle sipariş verebilirsiniz. Birden çok özelle sipariş veren bir sorgu [bileşik dizin](index-policy.md#composite-indexes)gerektirir. Şu sorguyu inceleyin:

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.address.city ASC, f.creationDate DESC
```

Bu sorgu, aileyi `id` şehir adının artan sırasına göre alır. Birden çok öğe aynı şehir adına sahipse, sorgu azalan sıraya göre `creationDate` sıralanır.

## <a name="documents-with-missing-fields"></a>Eksik alanları olan belgeler

`ORDER BY` Varsayılan dizin oluşturma ilkesine sahip kapsayıcılara karşı çalıştırılan sorgular, sıralama özelliğinin tanımlanmamış olduğu belgeleri döndürmez. Sıralama özelliğinin tanımlanmamış olduğu belgeleri eklemek istiyorsanız, bu özelliği açıkça dizin oluşturma ilkesine eklemeniz gerekir.

Örneğin, aşağıda, aşağıda açıkça herhangi bir yol içermeyen bir dizin `"/*"`oluşturma ilkesi ne sahip bir kapsayıcı bulunmaktadır:

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

Yan tümcede içeren `lastName` bir sorgu çalıştırursanız, sonuçlar yalnızca `lastName` tanımlanmış bir özelliği olan belgeleri içerir. `Order By` Sorgu sonuçlarında görünmeyen belgelerin `lastName` `lastName` olmaması için açık bir dahil yolu tanımlamadık.

Aşağıda, biri tanımlanmış `lastName` olmayan iki belgeye göre sıralanan `lastName` bir sorgu vesiye vesi;

```sql
    SELECT f.id, f.lastName
    FROM Families f
    ORDER BY f.lastName
```

Sonuçlar yalnızca tanımlanmış `lastName`bir belgeyi içerir:

```json
    [
        {
            "id": "AndersenFamily",
            "lastName": "Andersen"
        }
    ]
```

Kapsayıcının dizin oluşturma ilkesini açıkça bir yol `lastName`içerecek şekilde güncellersek, sorgu sonuçlarına tanımlanmamış sıralama özelliğine sahip belgeler ekleriz. Bu skaler değere giden yolu (ve ötesine geçmeyi) açıkça tanımlamanız gerekir. Özelliği açıkça `?` dizine aldığınızdan `lastName` ve bunun dışında ek iç içe giden yollar olmadığından emin olmak için dizin oluşturma ilkesindeki yol tanımınızdaki karakteri kullanmanız gerekir. Sorgunuz `Order By` bileşik [dizin](index-policy.md#composite-indexes)kullanıyorsa, sonuçlar her zaman sorgu sonuçlarında tanımlanmamış sıralama özelliğine sahip belgeleri içerir.

Burada, sorgu sonuçlarında tanımlanmamış `lastName` görünen belgelere sahip olan belgelere sahip olmasını sağlayan bir örnek dizin oluşturma ilkesi vermiştir:

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

Aynı sorguyu yeniden çalıştıran, eksik `lastName` olan belgeler önce sorgu sonuçlarında görünür:

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

Sıralama sırasını `DESC`, eksik `lastName` olan belgeler sorgu sonuçlarında en son görünür:

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
- [OFSET SINIRI yan tümcesi](sql-query-offset-limit.md)
