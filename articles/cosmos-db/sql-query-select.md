---
title: Azure Cosmos DB yan tümce SEÇIN
description: Azure Cosmos DB için SQL SELECT yan tümcesi hakkında bilgi edinin. SQL 'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: ginarobinson
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: girobins
ms.openlocfilehash: d34b1c39d9789409dc365cd4cf07fdc3d5a780fd
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003520"
---
# <a name="select-clause"></a>SELECT yan tümcesi

Her sorgu, ANSI SQL standartlarına göre, SELECT yan tümcesi ve from ve [WHERE](sql-query-where.md) yan [tümcelerinden](sql-query-from.md) oluşur. Genellikle, FROM yan tümcesindeki kaynak numaralandırılır ve WHERE yan tümcesi JSON öğelerinin bir alt kümesini almak için kaynak üzerinde bir filtre uygular. SELECT yan tümcesi daha sonra seçim listesinde istenen JSON değerlerini projeler.

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
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<select_specification>`  

  Özellikleri veya sonuç kümesi için seçilecek değeri.  
  
- `'*'`  

  Değer herhangi bir değişiklik yapmadan alınması gerektiğini belirtir. İşlenen değer bir nesne ise, özellikle, tüm özellikleri alınır.  
  
- `<object_property_list>`  
  
  Alınacak özelliklerinin listesini belirtir. Her döndürülen değer, belirtilen özellikleri içeren bir nesne olacaktır.  
  
- `VALUE`  

  JSON değerinin yerine tam JSON nesne alınacağını belirtir. Bunun aksine `<property_list>` Öngörülen Değer nesneyi kaydırma değil.  
 
- `DISTINCT`
  
  Yansıtılan özelliklerin tekrarların kaldırılması gerektiğini belirtir.  

- `<scalar_expression>`  

  Hesaplanmasını değeri gösteren ifade. Bkz: [skaler ifadelerin](sql-query-scalar-expressions.md) ayrıntıları bölümü.  

## <a name="remarks"></a>Açıklamalar

`SELECT *` Söz dizimi FROM yan tümcesi tam olarak bir diğer ad bildirilmişlerse geçerli yalnızca. `SELECT *` projeksiyon gerekirse yararlı olabilecek bir kimlik yansıtma sağlar. SEÇİN * yalnızca FROM yan tümcesi belirtilmiş ise geçerlidir ve yalnızca tek bir giriş kaynağı kullanıma sunulmuştur.  
  
Her ikisi de `SELECT <select_list>` ve `SELECT *` "söz dizimi sugar" olan ve aşağıda gösterildiği gibi basit bir SELECT deyimi kullanarak alternatif olarak belirtilebilir.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   eşdeğerdir:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   eşdeğerdir:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
## <a name="examples"></a>Örnekler

Aşağıdaki SELECT sorgusu `address` örneği, `id` eşleşmelerin `Families` `AndersenFamily`döndürdüğü örnekleri:

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şunlardır:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="quoted-property-accessor"></a>Alıntılanmış özellik erişimcisi
[] Alıntı özelliği işlecini kullanarak özelliklere erişebilirsiniz. Örneğin, `SELECT c.grade` ve `SELECT c["grade"]` eşdeğerdir. Bu sözdizimi, boşluk, özel karakter veya bir SQL anahtar sözcüğüyle veya ayrılmış sözcükle aynı ada sahip bir özelliğin kaçış için yararlıdır.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

### <a name="nested-properties"></a>İç içe Özellikler

Aşağıdaki örnek, `f.address.state` iki iç içe geçmiş özelliği ve `f.address.city`.

```sql
    SELECT f.address.state, f.address.city
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şunlardır:

```json
    [{
      "state": "WA",
      "city": "Seattle"
    }]
```
### <a name="json-expressions"></a>JSON ifadeleri

Projeksiyon, aşağıdaki örnekte gösterildiği gibi JSON ifadelerini de destekler:

```sql
    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şunlardır:

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle",
        "name": "AndersenFamily"
      }
    }]
```

Önceki örnekte, SELECT yan tümcesinin bir JSON nesnesi oluşturması gerekir ve örnek hiçbir anahtar sağladığından, yan tümce örtük bağımsız değişken adını `$1`kullanır. Aşağıdaki sorgu iki örtük bağımsız değişken değişkeni döndürür: `$1` ve `$2`.

```sql
    SELECT { "state": f.address.state, "city": f.address.city },
           { "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şunlardır:

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [WHERE yan tümcesi](sql-query-where.md)