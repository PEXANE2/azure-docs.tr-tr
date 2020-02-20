---
title: Azure Cosmos DB yan tümce SEÇIN
description: Azure Cosmos DB için SQL SELECT yan tümcesi hakkında bilgi edinin. SQL 'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: ginarobinson
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: girobins
ms.openlocfilehash: 013ebdcdbac41825c10a1362f73ab4c94052400d
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
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

  Özellikleri veya sonuç kümesi için seçilecek değeri.  
  
- `'*'`  

  Değer herhangi bir değişiklik yapmadan alınması gerektiğini belirtir. İşlenen değer bir nesne ise, özellikle, tüm özellikleri alınır.  
  
- `<object_property_list>`  
  
  Alınacak özelliklerinin listesini belirtir. Her döndürülen değer, belirtilen özellikleri içeren bir nesne olacaktır.  
  
- `VALUE`  

  JSON değerinin yerine tam JSON nesne alınacağını belirtir. `<property_list>` aksine bu, bir nesnedeki öngörülen değeri sarmaz.  
 
- `DISTINCT`
  
  Yansıtılan özelliklerin tekrarların kaldırılması gerektiğini belirtir.  

- `<scalar_expression>`  

  Hesaplanmasını değeri gösteren ifade. Ayrıntılar için bkz. [skaler ifadeler](sql-query-scalar-expressions.md) bölümü.  

## <a name="remarks"></a>Açıklamalar

`SELECT *` sözdizimi yalnızca FROM yan tümcesi tam olarak bir diğer ad bildirmişse geçerlidir. `SELECT *`, projeksiyon gerekmiyorsa yararlı olabilecek bir kimlik projeksiyonu sağlar. SEÇİN * yalnızca FROM yan tümcesi belirtilmiş ise geçerlidir ve yalnızca tek bir giriş kaynağı kullanıma sunulmuştur.  
  
Hem `SELECT <select_list>` hem de `SELECT *` "sözdizimsel cukr" olur ve bunun yerine aşağıda gösterildiği gibi basit SELECT deyimleri kullanılarak ifade edilebilir.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   eşdeğerdir:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   eşdeğerdir:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
## <a name="examples"></a>Örnekler

Aşağıdaki SELECT sorgusu örneği, `id` `AndersenFamily`eşleşen `Families` `address` döndürür:

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

Aşağıdaki örnek, `f.address.state` ve `f.address.city`iki iç içe özelliği projeler.

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

Önceki örnekte, SELECT yan tümcesinin bir JSON nesnesi oluşturması gerekir ve örnek hiçbir anahtar sağladığından, yan tümce `$1`örtük bağımsız değişken adını kullanır. Aşağıdaki sorgu iki örtük bağımsız değişken değişkeni döndürüyor: `$1` ve `$2`.

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
## <a name="reserved-keywords-and-special-characters"></a>Ayrılmış anahtar sözcükler ve özel karakterler

Verileriniz, "Order" veya "Group" gibi ayrılmış anahtar sözcüklerle aynı adlara sahip özellikler içeriyorsa, bu belgelerde yapılan sorgular sözdizimi hatalarına neden olur. Sorguyu başarıyla çalıştırmak için, özelliği `[]` karaktere açıkça eklemeniz gerekir.

Örneğin, aşağıdaki `order` adlı bir belge ve özel karakterler içeren bir özellik `price($)`.

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

`order` özelliğini veya `price($)` özelliğini içeren bir sorgu çalıştırırsanız bir sözdizimi hatası alırsınız.

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
