---
title: Azure Cosmos DB SQL sorgularında skaler ifadeler
description: Azure Cosmos DB için skaler ifade SQL sözdizimi hakkında bilgi edinin. Bu makalede, işleçleri kullanarak skaler ifadeleri karmaşık ifadeler içine birleştirmek için nasıl açıklanır.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: f8c98915ad3b682af00492acc7bc51672ec874a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74870743"
---
# <a name="scalar-expressions-in-azure-cosmos-db-sql-queries"></a>Azure Cosmos DB SQL sorgularında skaler ifadeler

[SELECT yan tümcesi](sql-query-select.md) skaler ifadeleri destekler. Skaler ifade, tek bir değer elde etmek için değerlendirilebilecek semboller ve işleçlerin birleşimidir. Skaler ifadelere örnek olarak şunlar verilebilir: sabitler, özellik başvuruları, dizi öğesi başvuruları, diğer ad başvuruları veya işlev çağrıları. Skaler ifadeler işleçleri kullanarak karmaşık ifadeler olarak birleştirilebilir.

## <a name="syntax"></a>Sözdizimi
  
```sql  
<scalar_expression> ::=  
       <constant>
     | input_alias
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>
     | <create_array_expression>  
     | (<scalar_expression>)
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```

## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<constant>`  
  
   Sabit bir değeri temsil eder. Ayrıntılar için [Sabitler](sql-query-constants.md) bölümüne bakın.  
  
- `input_alias`  
  
   Yan tümcede `input_alias` tanıtılan tarafından `FROM` tanımlanan bir değeri temsil eder.  
  Bu değer **tanımsız** değil garanti edilir - girişteki**tanımlanmamış** değerler atlanır.  
  
- `<scalar_expression>.property_name`  
  
   Bir nesnenin özelliğinin değerini temsil eder. Özellik yoksa veya özellik bir nesne olmayan bir değerüzerinde başvurulmuşsa, ifade **tanımlanmamış** değere değer değerlendirir.  
  
- `<scalar_expression>'['"property_name"|array_index']'`  
  
   Bir dizinin dizini `property_name` `array_index` ile ad veya dizi öğesi ile özelliğin bir değerini temsil eder. Özellik/dizi dizisi yoksa veya özellik/dizi dizidisi nesne/dizi olmayan bir değere başvurulmuşsa, ifade tanımlanmamış değere göre değerlendirilir.  
  
- `unary_operator <scalar_expression>`  
  
   Tek bir değere uygulanan bir işleci temsil eder. Ayrıntılar için [Operatörler](sql-query-operators.md) bölümüne bakın.  
  
- `<scalar_expression> binary_operator <scalar_expression>`  
  
   İki değere uygulanan bir işleci temsil eder. Ayrıntılar için [Operatörler](sql-query-operators.md) bölümüne bakın.  
  
- `<scalar_function_expression>`  
  
   İşlev çağrısının sonucuyla tanımlanan bir değeri temsil eder.  
  
- `udf_scalar_function`  
  
   Kullanıcı tanımlı skaler fonksiyonunun adı.  
  
- `builtin_scalar_function`  
  
   Yerleşik skaler işlevinin adı.  
  
- `<create_object_expression>`  
  
   Belirtilen özelliklere ve değerlerine sahip yeni bir nesne oluşturarak elde edilen bir değeri temsil eder.  
  
- `<create_array_expression>`  
  
   Öğe olarak belirtilen değerlere sahip yeni bir dizi oluşturarak elde edilen değeri temsil eder  
  
- `parameter_name`  
  
   Belirtilen parametre adının değerini temsil eder. Parametre adlarının ilk \@ karakter olarak tek bir olması gerekir.  
  
## <a name="remarks"></a>Açıklamalar
  
  Yerleşik veya kullanıcı tanımlı skaler işlevi çağırırken, tüm bağımsız değişkenler tanımlanmalıdır. Bağımsız değişkenlerden herhangi biri tanımlanmamışsa, işlev çağrılmaz ve sonuç tanımsız olur.  
  
  Bir nesne oluşturulurken, tanımlanmamış değer atanan herhangi bir özellik atlanır ve oluşturulan nesneye dahil edilmez.  
  
  Bir dizi oluştururken, **tanımlanmamış** değer atanan herhangi bir öğe değeri atlanır ve oluşturulan nesneye dahil edilmez. Bu, bir sonraki tanımlanmış öğenin, oluşturulan dizinin dizinleri atlanmamış olacak şekilde yerini almasına neden olur.  

## <a name="examples"></a>Örnekler

```sql
    SELECT ((2 + 11 % 7)-2)/3
```

Sonuçlar:

```json
    [{
      "$1": 1.33333
    }]
```

Aşağıdaki sorguda, skaler ifadenin sonucu bir Boolean'dır:

```sql
    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f
```

Sonuçlar:

```json
    [
      {
        "AreFromSameCityState": false
      },
      {
        "AreFromSameCityState": true
      }
    ]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye Giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Alt sorgular](sql-query-subquery.md)