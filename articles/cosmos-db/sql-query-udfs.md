---
title: Azure Cosmos DB'de kullanıcı tanımlı işlevler (UDF' ler)
description: Azure Cosmos DB'de Kullanıcı tanımlı işlevler hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: b67202da7293ef55cfe3390ca676f7944da80fba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69614323"
---
# <a name="user-defined-functions-udfs-in-azure-cosmos-db"></a>Azure Cosmos DB'de kullanıcı tanımlı işlevler (UDF' ler)

SQL API, kullanıcı tarafından tanımlanan işlevler (UDF' ler) için destek sağlar. Skaler UDF'ler ile sıfır veya birçok bağımsız değişkeni geçebilir ve tek bir bağımsız değişken sonucu döndürebilirsiniz. API yasal JSON değerleri olduğu için her argüman denetler.  

API, UDF'leri kullanarak özel uygulama mantığını desteklemek için SQL sözdizimini genişletir. UDF'leri SQL API'ye kaydedebilir ve bunları SQL sorgularında referans verebilirsiniz. Aslında, UDFs zarif sorguları aramak için tasarlanmıştır. Sonuç olarak, UDF'lerin depolanan yordamlar ve tetikleyiciler gibi diğer JavaScript türleri gibi bağlam nesnesine erişimi yoktur. Sorgular salt okunur ve birincil veya ikincil yinelemelerde çalıştırılabilir. UDF'ler, diğer JavaScript türlerinin aksine, ikincil yinelemeler üzerinde çalışacak şekilde tasarlanmıştır.

Aşağıdaki örnek, Cosmos veritabanında bir madde kapsayıcısı altında bir UDF kaydeder. Örnek, adı `REGEX_MATCH`. İki JSON dize değerini `input` `pattern`kabul eder ve ,ilkinin JavaScript'in `string.match()` işlevini kullanarak ikinci deseninde eşleşip eşleşmeyemeyişece sini denetler.

## <a name="examples"></a>Örnekler

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

Şimdi, bu UDF'yi sorgu projeksiyonunda kullanın. UDF'leri sorgular içinden ararken `udf.` büyük/küçük harf duyarlı önekiyle nitelemelisiniz.

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

Aşağıdaki örnekte olduğu gibi, `udf.` bir filtrenin içindeki önek ile nitelikli UDF'yi kullanabilirsiniz:

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

Özünde, UDF'ler hem projeksiyonlarda hem de filtrelerde kullanabileceğiniz geçerli skaler ifadelerdir.

UDF'lerin gücünü genişletmek için koşullu mantıkla başka bir örneğe bakın:

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

Aşağıdaki örnekTE UDF alıştırmaları:

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

UDF parametreleri tarafından atıfta bulunulan özellikler JSON değerinde kullanılamıyorsa, parametre tanımsız olarak kabul edilir ve UDF çağırması atlanır. Benzer şekilde, UDF sonucu tanımlanmamışsa, sonuca dahil edilmez.

Önceki örneklerde de belirtildiği gibi, UDF'ler JavaScript dilinin gücünü SQL API ile tümleştirir. UDF'ler, yerleşik JavaScript çalışma zamanı özellikleri yardımıyla karmaşık yordamsal, koşullu mantık yapmak için zengin bir programlanabilir arabirim sağlar. SQL API, geçerli WHERE veya SELECT yan tümcesi aşamasındaki her kaynak öğe için bağımsız değişkenleri sağlar. Sonuç, genel yürütme ardışık hattına sorunsuz bir şekilde dahil edilir. Özetle, UDF'ler sorguların bir parçası olarak karmaşık iş mantığı yapmak için harika araçlardır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye Giriş](introduction.md)
- [Sistem işlevleri](sql-query-system-functions.md)
- [Toplamalar](sql-query-aggregates.md)