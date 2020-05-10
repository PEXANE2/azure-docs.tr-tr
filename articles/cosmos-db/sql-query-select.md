---
title: Azure Cosmos DB yan tümce SEÇIN
description: Azure Cosmos DB için SQL SELECT yan tümcesi hakkında bilgi edinin. SQL 'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: tisande
ms.openlocfilehash: f33cf20b76655a893fe7eebd9e6e6569d35de98f
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2020
ms.locfileid: "83005946"
---
# <a name="select-clause-in-azure-cosmos-db"></a>Azure Cosmos DB yan tümce SEÇIN

Her sorgu, ANSI SQL `SELECT` standartlarına göre her bir yan tümce ve from ve [WHERE](sql-query-where.md) yan [tümcelerinden](sql-query-from.md) oluşur. Genellikle, `FROM` yan tümcesindeki kaynak numaralandırılır ve `WHERE` yan tümce, JSON öğelerinin bir alt kümesini almak için kaynak üzerinde bir filtre uygular. `SELECT` Yan tümcesi daha sonra seçim LISTESINDE istenen JSON değerlerini projeler.

## <a name="syntax"></a>Sözdizimi

```sql
SELECT <select_specification>  

<select_specification> ::=
      '*'
      | [DISTINCT] <object_property_list>
      | [DISTINCT] VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
```  
  
## <a name="arguments"></a>Arguments
  
- `<select_specification>`  

  Sonuç kümesi için seçilecek özellikler veya değer.  
  
- `'*'`  

  Değerin herhangi bir değişiklik yapılmadan alınması gerektiğini belirtir. Özellikle işlenen değer bir nesnese, tüm özellikler alınır.  
  
- `<object_property_list>`  
  
  Alınacak özelliklerin listesini belirtir. Döndürülen her değer, belirtilen özelliklerle bir nesne olacaktır.  
  
- `VALUE`  

  Tüm JSON nesnesi yerine JSON değerinin alınması gerektiğini belirtir. Bu, aksine `<property_list>` , bir nesnedeki öngörülen değeri sarmaz.  

- `DISTINCT`
  
  Yansıtılan özelliklerin tekrarların kaldırılması gerektiğini belirtir.  

- `<scalar_expression>`  

  Hesaplanacağı değeri temsil eden ifade. Ayrıntılar için bkz. [skaler ifadeler](sql-query-scalar-expressions.md) bölümü.  

## <a name="remarks"></a>Açıklamalar

`SELECT *` SÖZDIZIMI yalnızca from yan tümcesi tam olarak bir diğer ad bildirmişse geçerlidir. `SELECT *`yansıtma gerekmiyorsa yararlı olabilecek bir kimlik projeksiyonu sağlar. SELECT * yalnızca FROM yan tümcesi belirtilmişse ve yalnızca tek bir giriş kaynağı tanıdığında geçerlidir.  
  
`SELECT *` Hem hem de `SELECT <select_list>` "sözdizimsel cukr" ve AŞAĞıDA gösterildiği gibi basit select deyimleri kullanılarak ifade edilebilir.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   eşittir:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   eşittir:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
## <a name="examples"></a>Örnekler

`address` Aşağıdaki SELECT sorgusu örneği, `Families` `id` eşleşmelerin `AndersenFamily`döndürdüğü örnekleri:

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

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [WHERE yan tümcesi](sql-query-where.md)
