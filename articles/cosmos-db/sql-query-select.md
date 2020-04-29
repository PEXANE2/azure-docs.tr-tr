---
title: Azure Cosmos DB yan tümce SEÇIN
description: Azure Cosmos DB için SQL SELECT yan tümcesi hakkında bilgi edinin. SQL 'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: ginarobinson
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: girobins
ms.openlocfilehash: 013ebdcdbac41825c10a1362f73ab4c94052400d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77469944"
---
# <a name="select-clause-in-azure-cosmos-db"></a>Azure Cosmos DB yan tümce SEÇIN

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

Sonuçlar:

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

Sonuçlar:

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

Sonuçlar:

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
## <a name="reserved-keywords-and-special-characters"></a>Ayrılmış anahtar sözcükler ve özel karakterler

Verileriniz, "Order" veya "Group" gibi ayrılmış anahtar sözcüklerle aynı adlara sahip özellikler içeriyorsa, bu belgelerde yapılan sorgular sözdizimi hatalarına neden olur. Sorguyu başarılı bir şekilde çalıştırmak için özelliği `[]` karaktere açıkça eklemeniz gerekir.

Örneğin, adında `order` bir özelliği ve özel karakterler içeren bir özelliği `price($)` içeren bir belge:

```json
{
  "id": "AndersenFamily",
  "order": [
     {
         "orderId": "12345",
         "productId": "A17849",
         "price($)": 59.33
     }
  ],
  "creationDate": 1431620472,
  "isRegistered": true
}
```

`order` Özelliği veya `price($)` özelliği içeren bir sorgu çalıştırırsanız, bir sözdizimi hatası alırsınız.

```sql
SELECT * FROM c where c.order.orderid = "12345"
```
```sql
SELECT * FROM c where c.order.price($) > 50
```
Sonuç:

`
Syntax error, incorrect syntax near 'order'
`

Aynı sorguları aşağıdaki gibi yeniden yazmanız gerekir:

```sql
SELECT * FROM c WHERE c["order"].orderId = "12345"
```

```sql
SELECT * FROM c WHERE c["order"]["price($)"] > 50
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [WHERE yan tümcesi](sql-query-where.md)
