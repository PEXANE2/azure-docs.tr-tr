---
title: Azure Cosmos DB'de kullanıcı tanımlı işlevler (UDF' ler)
description: Azure Cosmos DB'de Kullanıcı tanımlı işlevler hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: tisande
ms.openlocfilehash: 455f44fb365152b75a3811563b646c6243f686db
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011132"
---
# <a name="user-defined-functions-udfs-in-azure-cosmos-db"></a>Azure Cosmos DB'de kullanıcı tanımlı işlevler (UDF' ler)

SQL API, kullanıcı tarafından tanımlanan işlevler (UDF' ler) için destek sağlar. Skaler UDF'ler ile sıfır veya birçok bağımsız değişkeni geçebilir ve tek bir bağımsız değişken sonucu döndürebilirsiniz. API yasal JSON değerleri olduğu için her argüman denetler.  

## <a name="udf-use-cases"></a>UDF kullanım örnekleri

API, UDF'leri kullanarak özel uygulama mantığını desteklemek için SQL sözdizimini genişletir. UDF'leri SQL API'ye kaydedebilir ve bunları SQL sorgularında referans verebilirsiniz. Depolanan yordamların ve tetikleyicilerin aksine, UDF'ler salt okunur.

UDF'leri kullanarak Azure Cosmos DB'nin sorgu dilini genişletebilirsiniz. UDF'ler, sorgunun projeksiyonundaki karmaşık iş mantığını ifade etmenin harika bir yoludur.

Ancak, şu anda UDF'lerden kaçınmanızı öneririz:

- Azure Cosmos DB'de eşdeğer [bir sistem işlevi](sql-query-system-functions.md) zaten mevcut. Sistem fonksiyonları her zaman eşdeğer UDF daha az RU kullanır.
- UDF, sorgunuzun yan `WHERE` tümcesindeki tek filtredir. UDF'ler dizin kullanmadığı için UDF'nin değerlendirilmesi için belgelerin yüklenmesi gerekir. `WHERE` Dizin kullanan ek filtre etelerinin udf ile birlikte birleştirilmesi, yan tümcede UDF tarafından işlenen belge sayısını azaltacaktır.

Bir sorguda aynı UDF'yi birden çok kez kullanmanız gerekiyorsa, UDF'yi bir kez değerlendirmek için JOIN ifadesini kullanmanıza izin veren, ancak birçok kez başvuruda bulunan bir [alt sorguda](sql-query-subquery.md#evaluate-once-and-reference-many-times)UDF'ye başvurmanız gerekir.

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, Cosmos veritabanında bir madde kapsayıcısı altında bir UDF kaydeder. Örnek, adı `REGEX_MATCH`. İki JSON dize değerini `input` `pattern`kabul eder ve ,ilkinin JavaScript'in `string.match()` işlevini kullanarak ikinci deseninde eşleşip eşleşmeyemeyişece sini denetler.

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