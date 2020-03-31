---
title: Azure Cosmos DB'de SELECT yan tümcesi
description: Azure Cosmos DB için SQL SELECT yan tümcesi hakkında bilgi edinin. SQL'i Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: ginarobinson
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: girobins
ms.openlocfilehash: 013ebdcdbac41825c10a1362f73ab4c94052400d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77469944"
---
# <a name="select-clause-in-azure-cosmos-db"></a>Azure Cosmos DB'de SELECT yan tümcesi

Her sorgu, ANSI SQL standartlarına göre select yan tümcesi ve isteğe bağlı [FROM](sql-query-from.md) ve [WHERE](sql-query-where.md) yan tümcelerinden oluşur. Genellikle, FROM yan tümcesindeki kaynak numaralandırılır ve WHERE yan tümcesi JSON öğelerinin bir alt kümesini almak için kaynağa bir filtre uygular. SELECT yan tümcesi daha sonra istenen JSON değerlerini seçili listede gösterir.

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

  Değerin herhangi bir değişiklik yapmadan alınması gerektiğini belirtir. Özellikle işlenen değer bir nesneyse, tüm özellikler alınır.  
  
- `<object_property_list>`  
  
  Alınacak özelliklerin listesini belirtir. Döndürülen her değer, belirtilen özelliklere sahip bir nesne olacaktır.  
  
- `VALUE`  

  JSON değerinin tam JSON nesnesi yerine alınması gerektiğini belirtir. Bu, `<property_list>` aksine bir nesnede yansıtılan değeri sarmak değildir.  
 
- `DISTINCT`
  
  Öngörülen özelliklerin yinelenenlerinin kaldırılması gerektiğini belirtir.  

- `<scalar_expression>`  

  Hesaplanacak değeri temsil eden ifade. Ayrıntılar için [Skaler ifadeler](sql-query-scalar-expressions.md) bölümüne bakın.  

## <a name="remarks"></a>Açıklamalar

Sözdizimi yalnızca `SELECT *` FROM yan tümcesi tam olarak bir diğer ad beyan ettiyse geçerlidir. `SELECT *`hiçbir projeksiyon gerekli olduğunda yararlı olabilir bir kimlik projeksiyonu sağlar. SELECT * yalnızca FROM yan tümcesi belirtilmişse ve yalnızca tek bir giriş kaynağı tanıtılırsa geçerlidir.  
  
Her `SELECT <select_list>` `SELECT *` ikisi de ve "sözdizimsel şeker" ve alternatif olarak aşağıda gösterildiği gibi basit SELECT ifadeleri kullanılarak ifade edilebilir.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   eşdeğerdir:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   eşdeğerdir:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
## <a name="examples"></a>Örnekler

Aşağıdaki SELECT sorgu `address` örneği `Families` `id` eşleşen `AndersenFamily`eşlerden döner:

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

### <a name="quoted-property-accessor"></a>Teklif edilen özellik erişimcisi
Teklif edilen özellik işleci [] kullanarak özelliklere erişebilirsiniz. Örneğin, `SELECT c.grade` ve `SELECT c["grade"]` eşdeğerdir. Bu sözdizimi, boşluklar, özel karakterler içeren veya SQL anahtar sözcüğü veya ayrılmış sözcük ile aynı ada sahip bir özellikten kaçmak için yararlıdır.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

### <a name="nested-properties"></a>İç içe özellikleri

Aşağıdaki örnekte iki iç `f.address.state` içe `f.address.city`özellikleri projeleri ve .

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

Önceki örnekte, SELECT yan tümcesinin bir JSON nesnesi oluşturması gerekir ve örnek anahtar `$1`sağlamadığından, yan tümce örtülü bağımsız değişken değişken adını kullanır. Aşağıdaki sorgu iki örtük bağımsız `$1` `$2`değişken değişkeni döndürür: ve .

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
## <a name="reserved-keywords-and-special-characters"></a>Ayrılmış anahtar kelimeler ve özel karakterler

Verileriniz "sipariş" veya "Grup" gibi ayrılmış anahtar kelimelerle aynı adlara sahip özellikler içeriyorsa, bu belgelere karşı yapılan sorgular sözdizimi hatalarına neden olur. Sorguyu başarılı bir şekilde `[]` çalıştırmak için özelliği açıkça karaktere dahil etmelisiniz.

Örneğin, burada adlandırılmış `order` bir özellik ve özel `price($)` karakterler içeren bir özellik içeren bir belge aşağıda verilmiştir:

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

`order` Özelliği veya `price($)` özelliği içeren bir sorgu çalıştırıyorsanız, sözdizimi hatası alırsınız.

```sql
SELECT * FROM c where c.order.orderid = "12345"
```
```sql
SELECT * FROM c where c.order.price($) > 50
```
Sonuç şudur:

`
Syntax error, incorrect syntax near 'order'
`

Aşağıdaki gibi aynı sorguları yeniden yazmalısınız:

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
