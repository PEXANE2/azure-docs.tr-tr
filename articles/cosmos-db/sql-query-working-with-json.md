---
title: Azure Cosmos DB 'de JSON ile çalışma
description: İç içe geçmiş JSON özelliklerine sorgu ve erişme ve Azure Cosmos DB özel karakterler kullanma hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: tisande
ms.openlocfilehash: a569b0122f9122b141b64ded21dbd9be1d766a41
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83699131"
---
# <a name="working-with-json-in-azure-cosmos-db"></a>Azure Cosmos DB 'de JSON ile çalışma

Azure Cosmos DB SQL (Core) API 'sinde, öğeler JSON olarak depolanır. Tür sistemi ve ifadeleri yalnızca JSON türleriyle uğraşmak üzere kısıtlanmıştır. Daha fazla bilgi için bkz. [JSON belirtimi](https://www.json.org/).

JSON ile çalışmanın bazı önemli yönlerini özetleyeceğiz:

- JSON nesneleri her zaman `{` sol ayraç ile başlar ve `}` sağ küme ayracı ile biter
- Bir diğeri içinde [iç içe geçmiş](#nested-properties) olan JSON özelliklerine sahip olabilirsiniz
- JSON özellik değerleri diziler olabilir
- JSON Özellik adları büyük/küçük harfe duyarlıdır
- JSON Özellik adı herhangi bir dize değeri olabilir (boşluk veya harf olmayan karakterler dahil)

## <a name="nested-properties"></a>İç içe Özellikler

Bir nokta erişimcisi kullanarak iç içe geçmiş JSON 'a erişebilirsiniz. Sorgularınızdaki iç içe JSON özelliklerini, diğer özellikleri kullanabileceğiniz şekilde kullanabilirsiniz.

İç içe geçmiş JSON içeren bir belge aşağıdadır:

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "address": {
      "state": "WA",
      "county": "King",
      "city": "Seattle"
      },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Bu durumda,, `state` `country` ve `city` özelliklerinin hepsi özelliği içinde iç içe `address` .

Aşağıdaki örnek iki iç içe geçmiş özelliği projeler: `f.address.state` ve `f.address.city` .

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

## <a name="working-with-arrays"></a>Dizilerle çalışma

İç içe özelliklere ek olarak JSON Ayrıca dizileri de destekler.

Aşağıda, bir dizi içeren örnek bir belge verilmiştir:

```json
{
  "id": "WakefieldFamily",
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
      },
      {
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8
      }
  ],
}
```

Dizilerle çalışırken, konumuna başvurarak dizi içindeki belirli bir öğeye erişebilirsiniz:

```sql
SELECT *
FROM Families f
WHERE f.children[0].givenName = "Jesse"
```

Ancak çoğu durumda, diziler ile çalışırken bir [alt sorgu](sql-query-subquery.md) veya [kendi kendine JOIN](sql-query-join.md) kullanırsınız.

Örneğin, bir müşterinin banka hesabının günlük bakiyesini gösteren bir belge aşağıda verilmiştir.

```json
{
  "id": "Contoso-Checking-Account-2020",
  "balance": [
      {
        "checkingAccount": 1000,
        "savingsAccount": 5000
      },
      {
        "checkingAccount": 100,
        "savingsAccount": 5000
      },
      {
        "checkingAccount": -10,
        "savingsAccount": 5000,
      },
      {
        "checkingAccount": 5000,
        "savingsAccount": 5000,
      }
         ...
  ]
}
```

Bir noktada negatif bakiyesi olan tüm müşterileri gösteren bir sorgu çalıştırmak istiyorsanız, bir alt [sorgu ile birlikte](sql-query-subquery.md#exists-expression) kullanabilirsiniz:

```sql
SELECT c.id
FROM c
WHERE EXISTS(
    SELECT VALUE n
    FROM n IN c.balance
    WHERE n.checkingAccount < 0
)
```

## <a name="reserved-keywords-and-special-characters-in-json"></a>JSON 'da ayrılmış anahtar sözcükler ve özel karakterler

Tırnak içine alınmış Özellik işlecini kullanarak özelliklere erişebilirsiniz `[]` . Örneğin, `SELECT c.grade` ve `SELECT c["grade"]` eşdeğerdir. Bu sözdizimi, boşluk, özel karakter veya bir SQL anahtar sözcüğüyle veya ayrılmış sözcükle aynı ada sahip bir özelliğin kaçış için yararlıdır.

Örneğin, adında bir özelliği `order` ve `price($)` özel karakterler içeren bir özelliği içeren bir belge:

```json
{
  "id": "AndersenFamily",
  "order": {
         "orderId": "12345",
         "productId": "A17849",
         "price($)": 59.33
   },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Özelliği veya özelliği içeren bir sorgu çalıştırırsanız `order` `price($)` , bir sözdizimi hatası alırsınız.

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

## <a name="json-expressions"></a>JSON ifadeleri

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

Önceki örnekte, `SELECT` yan tümcesinin BIR JSON nesnesi oluşturması gerekir ve örnek hiçbir anahtar sağladığından, yan tümce örtük bağımsız değişken adını kullanır `$1` . Aşağıdaki sorgu iki örtük bağımsız değişken değişkeni döndürür: `$1` ve `$2` .

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

## <a name="aliasing"></a>Diğer ad kullanımı

Sorgularda açıkça diğer ad değerleri ekleyebilirsiniz. Sorgunun aynı ada sahip iki özelliği varsa, bir veya her ikisini de yeniden adlandırmak için diğer adları kullanın.

### <a name="examples"></a>Örnekler

`AS`Diğer ad için kullanılan anahtar sözcük, aşağıdaki örnekte gösterildiği gibi isteğe bağlıdır `NameInfo` :

```sql
    SELECT
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

### <a name="aliasing-with-reserved-keywords-or-special-characters"></a>Ayrılmış anahtar sözcüklerle veya özel karakterlerle diğer adlar

Bir değeri boşluk, özel karakter veya ayrılmış sözcük olan bir özellik adı olarak proje için diğer adları kullanamazsınız. Bir değerin projeksiyonunu olarak değiştirmek istiyorsanız, örneğin bir boşluk içeren bir özellik adına sahip olmak için bir [JSON ifadesi](#json-expressions)kullanabilirsiniz.

İşte bir örnek:

```sql
    SELECT
           {"JSON expression with a space": { "state": f.address.state, "city": f.address.city }},
           {"JSON expression with a special character!": { "name": f.id }}
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [SELECT yan tümcesi](sql-query-select.md)
- [WHERE yan tümcesi](sql-query-where.md)
