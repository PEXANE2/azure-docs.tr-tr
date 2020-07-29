---
title: Azure Cosmos DB SQL sorgularında skaler ifadeler
description: Azure Cosmos DB için skalar ifade SQL sözdizimi hakkında bilgi edinin. Bu makalede, işleçler kullanılarak skalar ifadelerin karmaşık ifadelerle nasıl birleştirileceğini de açıklanmaktadır.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: f8c98915ad3b682af00492acc7bc51672ec874a8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74870743"
---
# <a name="scalar-expressions-in-azure-cosmos-db-sql-queries"></a>Azure Cosmos DB SQL sorgularında skaler ifadeler

[Select yan tümcesi](sql-query-select.md) skaler ifadeleri destekler. Skaler bir ifade, tek bir değer elde etmek için değerlendirilebilen bir sembol ve işleç birleşimidir. Skalar ifade örnekleri şunlardır: sabitler, özellik başvuruları, dizi öğesi başvuruları, diğer ad başvuruları veya işlev çağrıları. Skalar ifadeler, işleçler kullanılarak karmaşık ifadelerle birleştirilebilir.

## <a name="syntax"></a>Söz dizimi
  
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

## <a name="arguments"></a>Bağımsız değişkenler
  
- `<constant>`  
  
   Sabit bir değeri temsil eder. Ayrıntılar için [sabitler](sql-query-constants.md) bölümüne bakın.  
  
- `input_alias`  
  
   Yan tümcesinde tanıtılan tarafından tanımlanan bir değeri temsil eder `input_alias` `FROM` .  
  Bu değerin **tanımsız** olmaması garanti edilir – girişte**tanımsız** değerler atlanır.  
  
- `<scalar_expression>.property_name`  
  
   Bir nesnenin özelliğinin değerini temsil eder. Özellik yok veya özelliğe bir nesne olmayan bir değer üzerinde başvuruluyorsa, ifade **tanımsız** değer olarak değerlendirilir.  
  
- `<scalar_expression>'['"property_name"|array_index']'`  
  
   `property_name`Bir dizinin diziniyle birlikte Name veya Array öğesi olan bir özelliğin değerini temsil eder `array_index` . Özellik/dizi dizini yoksa veya nesne/dizi olmayan bir değer üzerinde özelliğe/dizi dizinine başvuruluyorsa, ifade tanımsız değer olarak değerlendirilir.  
  
- `unary_operator <scalar_expression>`  
  
   Tek bir değere uygulanan bir işleci temsil eder. Ayrıntılar için bkz. [işleçler](sql-query-operators.md) bölümü.  
  
- `<scalar_expression> binary_operator <scalar_expression>`  
  
   İki değere uygulanan bir işleci temsil eder. Ayrıntılar için bkz. [işleçler](sql-query-operators.md) bölümü.  
  
- `<scalar_function_expression>`  
  
   İşlev çağrısının sonucu tarafından tanımlanan bir değeri temsil eder.  
  
- `udf_scalar_function`  
  
   Kullanıcı tanımlı skaler işlevin adı.  
  
- `builtin_scalar_function`  
  
   Yerleşik skaler işlevin adı.  
  
- `<create_object_expression>`  
  
   Belirtilen özelliklerle ve bunların değerleriyle yeni bir nesne oluşturularak elde edilen değeri temsil eder.  
  
- `<create_array_expression>`  
  
   Öğe olarak belirtilen değerlere sahip yeni bir dizi oluşturarak elde edilen değeri temsil eder  
  
- `parameter_name`  
  
   Belirtilen parametre adının bir değerini temsil eder. Parametre adlarının ilk karakter olarak tek bir olması gerekir \@ .  
  
## <a name="remarks"></a>Açıklamalar
  
  Yerleşik veya Kullanıcı tanımlı skaler bir işlev çağrılırken, tüm bağımsız değişkenlerin tanımlanması gerekir. Bağımsız değişkenlerden herhangi biri tanımlanmamışsa, işlev çağrılmaz ve sonuç tanımsız olur.  
  
  Bir nesne oluştururken, tanımlanmamış değer atanan tüm özellikler atlanır ve oluşturulan nesneye eklenmez.  
  
  Bir dizi oluştururken, **tanımlanmamış** değer atanan tüm öğe değerleri atlanır ve oluşturulan nesneye eklenmez. Bu, sonraki tanımlanmış öğenin yerini oluşturulan dizinin atlanan dizine sahip olmayacak şekilde yerine geçirmesine neden olur.  

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

Aşağıdaki sorguda, skaler ifadenin sonucu bir Boolean değer:

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

- [Azure Cosmos DB giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Alt Sorgular](sql-query-subquery.md)