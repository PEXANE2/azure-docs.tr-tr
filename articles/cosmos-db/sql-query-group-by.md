---
title: Azure Cosmos DB'deki GROUP BY yan tümcesi
description: Azure Cosmos DB için GROUP BY yan tümcesi hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: tisande
ms.openlocfilehash: e41e81457421bfe27e3c0313fc06e39e6df4cdce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73819099"
---
# <a name="group-by-clause-in-azure-cosmos-db"></a>Azure Cosmos DB'deki GROUP BY yan tümcesi

GROUP BY yan tümcesi, sorgunun sonuçlarını bir veya daha fazla belirtilen özelliğin değerlerine göre böler.

> [!NOTE]
> Azure Cosmos DB şu anda GROUP BY'yi .NET SDK 3.3 ve üzeri ve JavaScript SDK 3.4 ve üzeri destekler.
> Diğer dil SDK'lar için destek şu anda mevcut değildir, ancak planlanmaktadır.

## <a name="syntax"></a>Sözdizimi

```sql  
<group_by_clause> ::= GROUP BY <scalar_expression_list>

<scalar_expression_list> ::=
          <scalar_expression>
        | <scalar_expression_list>, <scalar_expression>
```  

## <a name="arguments"></a>Bağımsız Değişkenler

- `<scalar_expression_list>`

   Sorgu sonuçlarını bölmek için kullanılacak ifadeleri belirtir.

- `<scalar_expression>`
  
   Skaler alt sorgular ve skaler agregalar dışında herhangi bir skaler ifadeye izin verilir. Her skaler ifade en az bir özellik başvurusu içermelidir. Tek tek ifadelerin sayısı veya her ifadenin kardinalliği için bir sınır yoktur.

## <a name="remarks"></a>Açıklamalar
  
  Bir sorgu GROUP BY yan tümcesi kullandığında, SELECT yan tümcesi yalnızca GROUP BY yan tümcesinde yer alan özelliklerin ve sistem işlevlerinin alt kümesini içerebilir. Bir istisna, GROUP BY yan tümcesi içinde yer almadan SELECT yan tümcesinde görünebilen [toplam sistem işlevleridir.](sql-query-aggregates.md) Select yan tümcesi'ne her zaman gerçek değerleri ekleyebilirsiniz.

  GROUP BY yan tümcesi SELECT, FROM ve WHERE yan tümcesinden sonra ve OFSET LIMIT iyanından önce olmalıdır. Şu anda GROUP BY'yi order by yan tümcesi ile kullanamazsınız, ancak bu planlanmıştır.

  GROUP BY maddesi aşağıdakilerden hiçbirine izin vermez:
  
- Diğer ad özellikleri veya diğer adlama sistemi işlevleri (diğer adı SELECT yan tümcesi içinde hala izin verilir)
- Alt sorgular
- Toplam sistem işlevleri (bunlara yalnızca SELECT yan tümcesinde izin verilir)

## <a name="examples"></a>Örnekler

Bu örnekler, [Azure Cosmos DB Query Playground](https://www.documentdb.com/sql/demo)aracılığıyla kullanılabilen beslenme veri kümesini kullanır.

Örneğin, her foodGroup'taki toplam öğe sayısını döndüren bir sorgu aşağıda verilmiştir:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, f.foodGroup
FROM Food f
GROUP BY f.foodGroup
```

Bazı sonuçlar şunlardır (TOP anahtar kelime sonuçları sınırlamak için kullanılır):

```json
[{
  "foodGroup": "Fast Foods",
  "foodGroupCount": 371
},
{
  "foodGroup": "Finfish and Shellfish Products",
  "foodGroupCount": 267
},
{
  "foodGroup": "Meals, Entrees, and Side Dishes",
  "foodGroupCount": 113
},
{
  "foodGroup": "Sausages and Luncheon Meats",
  "foodGroupCount": 244
}]
```

Bu sorguda sonuçları bölmek için kullanılan iki ifade vardır:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, f.foodGroup, f.version
FROM Food f
GROUP BY f.foodGroup, f.version
```

Bazı sonuçlar şunlardır:

```json
[{
  "version": 1,
  "foodGroup": "Nut and Seed Products",
  "foodGroupCount": 133
},
{
  "version": 1,
  "foodGroup": "Finfish and Shellfish Products",
  "foodGroupCount": 267
},
{
  "version": 1,
  "foodGroup": "Fast Foods",
  "foodGroupCount": 371
},
{
  "version": 1,
  "foodGroup": "Sausages and Luncheon Meats",
  "foodGroupCount": 244
}]
```

Bu sorgu, GROUP BY yan tümcesinde bir sistem işlevine sahiptir:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, UPPER(f.foodGroup) AS upperFoodGroup
FROM Food f
GROUP BY UPPER(f.foodGroup)
```

Bazı sonuçlar şunlardır:

```json
[{
  "foodGroupCount": 371,
  "upperFoodGroup": "FAST FOODS"
},
{
  "foodGroupCount": 267,
  "upperFoodGroup": "FINFISH AND SHELLFISH PRODUCTS"
},
{
  "foodGroupCount": 389,
  "upperFoodGroup": "LEGUMES AND LEGUME PRODUCTS"
},
{
  "foodGroupCount": 113,
  "upperFoodGroup": "MEALS, ENTREES, AND SIDE DISHES"
}]
```

Bu sorgu, öğe özelliği ifadesinde hem anahtar kelimeleri hem de sistem işlevlerini kullanır:

```sql
SELECT COUNT(1) AS foodGroupCount, ARRAY_CONTAINS(f.tags, {name: 'orange'}) AS containsOrangeTag,  f.version BETWEEN 0 AND 2 AS correctVersion
FROM Food f
GROUP BY ARRAY_CONTAINS(f.tags, {name: 'orange'}), f.version BETWEEN 0 AND 2
```

Sonuçlar:

```json
[{
  "correctVersion": true,
  "containsOrangeTag": false,
  "foodGroupCount": 8608
},
{
  "correctVersion": true,
  "containsOrangeTag": true,
  "foodGroupCount": 10
}]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [SELECT yan tümcesi](sql-query-select.md)
- [Toplam fonksiyonlar](sql-query-aggregates.md)
