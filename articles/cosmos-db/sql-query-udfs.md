---
title: Azure Cosmos DB 'de Kullanıcı tanımlı işlevler (UDF 'ler)
description: Azure Cosmos DB içindeki kullanıcı tanımlı işlevler hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: tisande
ms.openlocfilehash: 455f44fb365152b75a3811563b646c6243f686db
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81011132"
---
# <a name="user-defined-functions-udfs-in-azure-cosmos-db"></a>Azure Cosmos DB 'de Kullanıcı tanımlı işlevler (UDF 'ler)

SQL API 'SI, Kullanıcı tanımlı işlevler (UDF 'ler) için destek sağlar. Skaler UDF 'ler ile sıfır veya daha fazla bağımsız değişken geçirebilir ve tek bir bağımsız değişken sonucu döndürebilirsiniz. API, her bağımsız değişkeni geçerli JSON değerleri olarak denetler.  

## <a name="udf-use-cases"></a>UDF kullanım örnekleri

API, UDF 'Leri kullanarak özel uygulama mantığını desteklemek için SQL sözdizimini genişletir. UDF 'Leri SQL API ile kaydedebilir ve SQL sorgularında bunlara başvurabilirsiniz. Saklı yordamların ve tetikleyicilerin aksine, UDF 'ler salt okunurdur.

UDF 'Leri kullanarak Azure Cosmos DB sorgu dilini genişletebilirsiniz. UDF 'ler bir sorgunun projeksiyonundaki karmaşık iş mantığını ifade etmenin harika bir yoludur.

Ancak, şu durumlarda UDF 'ler önlenmelerden kaçınıyoruz:

- Azure Cosmos DB ' de eşdeğer bir [sistem işlevi](sql-query-system-functions.md) zaten var. Sistem işlevleri, eşdeğer UDF 'den her zaman daha az RU kullanır.
- UDF, sorgunuzun yan tümcesindeki tek filtredir `WHERE` . UDF 'nin dizinden yararlanın ve UDF 'yi değerlendirmek için yükleme belgelerinin yüklenmesi gerekir. Dizin kullanan ek filtre koşullarını birleştirmek, bir UDF ile birlikte, `WHERE` yan TÜMCESINDE UDF tarafından işlenen belgelerin sayısını azaltır.

Bir sorguda aynı UDF 'yi birden çok kez kullanmanız gerekiyorsa, UDF 'ye bir [alt sorgu](sql-query-subquery.md#evaluate-once-and-reference-many-times)içinde başvurmanız gerekir ve bir JOIN IFADESI kullanarak UDF 'yi bir kez değerlendirmek, ancak bu kadar çok kez başvuru yapmanız gerekir.

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, Cosmos veritabanındaki bir öğe kapsayıcısı altına bir UDF kaydeder. Örnek, adı olan bir UDF oluşturur `REGEX_MATCH` . İki JSON dize değerini kabul eder ve `input` `pattern` ilk kez, JavaScript 'in işlevi kullanılarak ikinci öğesinde belirtilen Düzenle eşleşip eşleşmediğini denetler `string.match()` .

```javascript
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) {
                      return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
           regexMatchUdf).Result;  
```

Şimdi bu UDF 'yi bir sorgu projeksiyonu içinde kullanın. UDF 'Leri sorgular içerisinden çağırırken büyük/küçük harfe duyarlı önekiyle nitelemeniz gerekir `udf.` .

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

Sonuçlar:

```json
    [
      {
        "$1": true
      },
      {
        "$1": false
      }
    ]
```

`udf.`Aşağıdaki örnekte olduğu gibi, bir filtrenin içindeki önekiyle NITELENMIŞ UDF 'yi kullanabilirsiniz:

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

Sonuçlar:

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

Temelde, UDF 'ler, hem projeksiyonde hem de filtrelerde kullanabileceğiniz geçerli skaler ifadeleridir.

UDF 'nin gücünden birini genişletmek için koşullu mantığa başka bir örneğe bakın:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'Seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
                seaLevelUdf);
```

Aşağıdaki örnek UDF 'yi alıştırmalar:

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f
```

Sonuçlar:

```json
     [
      {
        "city": "Seattle",
        "seaLevel": 520
      },
      {
        "city": "NY",
        "seaLevel": 410
      }
    ]
```

UDF parametreleri tarafından başvurulan Özellikler JSON değerinde kullanılamıyorsa, parametre tanımsız olarak değerlendirilir ve UDF çağrısı atlanır. Benzer şekilde, UDF 'nin sonucu tanımsızdır, sonuca dahil değildir.

Yukarıdaki örneklerde gösterildiği gibi, UDF 'ler, JavaScript dilinin gücünden SQL API 'SI ile tümleşir. UDF 'ler, yerleşik JavaScript çalışma zamanı özellikleri yardımıyla karmaşık yordamla, koşullu mantığa yönelik zengin bir programlanabilir arabirim sağlar. SQL API 'SI, geçerli WHERE veya SELECT yan tümcesi aşamasında her kaynak öğe için UDF 'ler için bağımsız değişkenler sağlar. Sonuç, genel yürütme ardışık düzenine sorunsuz bir şekilde dahil edilir. Özet olarak, UDF 'ler, sorguların bir parçası olarak karmaşık iş mantığı yapmak için harika araçlardır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB giriş](introduction.md)
- [Sistem işlevleri](sql-query-system-functions.md)
- [Toplamalar](sql-query-aggregates.md)