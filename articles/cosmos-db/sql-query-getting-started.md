---
title: Azure Cosmos DB'de SQL sorguları ile başlarken
description: Azure Cosmos DB'den verileri sorgulamak için SQL sorgularını nasıl kullanacağınızı öğrenin. Azure Cosmos DB'deki bir kapsayıcıya örnek verileri yükleyebilir ve sorgulayabilirsiniz.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: tisande
ms.openlocfilehash: 1d24261edea843fa928ad00e3ce7babcb84acd3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873344"
---
# <a name="getting-started-with-sql-queries"></a>SQL sorguları ile başlarken

Azure Cosmos DB SQL API hesapları, JSON sorgu dili olarak Yapılandırılmış Sorgu Dili (SQL) kullanarak öğeleri sorgulamayı destekler. Azure Cosmos DB sorgu dilinin tasarım hedefleri şunlardır:

* Yeni bir sorgu dili icat etmek yerine en tanıdık ve popüler sorgu dillerinden biri olan SQL'i destekleyin. SQL, JSON öğeleri üzerinde zengin sorgular için resmi bir programlama modeli sağlar.  

* Sorgu dilinin temeli olarak JavaScript programlama modelini kullanın. JavaScript'in tür sistemi, ifade değerlendirmesi ve işlev çağırması SQL API'nin kökleridir. Bu kökler, ilişkisel projeksiyonlar, JSON öğeleri arasında hiyerarşik gezinme, kendi kendine birleştirmeler, uzamsal sorgular ve tamamen JavaScript'te yazılmış kullanıcı tanımlı işlevlerin (UDF' ler) çağrılması gibi özellikler için doğal bir programlama modeli sağlar.

## <a name="upload-sample-data"></a>Örnek verileri yükleme

SQL API Cosmos DB hesabınızda `Families`. Kapsayıcıda iki basit JSON öğesi oluşturun. Bu veri kümesini kullanarak Azure Cosmos DB sorgu dokümanlarında örnek sorguların çoğunu çalıştırabilirsiniz.

### <a name="create-json-items"></a>JSON öğeleri oluşturma

Aşağıdaki kod aileler hakkında iki basit JSON öğesi oluşturur. Andersen ve Wakefield aileleri için basit JSON öğeleri ebeveynler, çocuklar ve evcil hayvanları, adres ve kayıt bilgilerini içerir. İlk öğedizelleri, numaraları, Booleans, diziler ve iç içe özellikleri vardır.


```json
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow",
         "gender": "female",
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "Seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

İkinci öğe `givenName` kullanır `familyName` ve `firstName` `lastName`yerine ve .

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

### <a name="query-the-json-items"></a>JSON öğelerini sorgula

Azure Cosmos DB'nin SQL sorgu dilinin bazı önemli yönlerini anlamak için JSON verilerine karşı birkaç sorgu deneyin.

Aşağıdaki sorgu, `id` alanın eşleştiği `AndersenFamily`öğeleri döndürür. Bu bir `SELECT *` sorgu olduğundan, sorgunun çıktısı tam JSON öğesidir. SELECT sözdizimi hakkında daha fazla bilgi için [SELECT deyimine](sql-query-select.md)bakın. 

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sorgu sonuçları şunlardır: 

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

Aşağıdaki sorgu, JSON çıktısını farklı bir şekle dönüştürür. Sorgu, `Name` `City`seçili iki `Family` alanla birlikte yeni bir JSON nesnesi ve adres şehri durumla aynı olduğunda projeler. "NY, NY" bu davayla eşleşiyor.

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

Sorgu sonuçları şunlardır:

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

Aşağıdaki sorgu, eşleşen ailedeki `id` çocukların verilen tüm `WakefieldFamily`adlarını döndürür, şehir tarafından sipariş edilir.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

Sonuçlar:

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

## <a name="remarks"></a>Açıklamalar

Önceki örnekler Cosmos DB sorgu dilinin çeşitli yönlerini gösterir:  

* SQL API JSON değerleri üzerinde çalıştığından, satır lar ve sütunlar yerine ağaç şeklindeki varlıklarla ilgilenir. Ansi SQL'deki iki bölümlü başvuruya `Node1.Node2.Node3…..Nodem` `<table>.<column>` benzer şekilde, herhangi bir rasgele derinlikte ağaç düğümlerine başvurabilirsiniz.

* Sorgu dili şemasız verilerle çalıştığından, tür sistemi dinamik olarak bağlanmalıdır. Aynı ifade farklı öğelerüzerinde farklı türlerde verim verebilir. Sorgunun sonucu geçerli bir JSON değeridir, ancak sabit bir şema olduğu garanti edilmez.  

* Azure Cosmos DB yalnızca katı JSON öğelerini destekler. Tür sistemi ve ifadeler yalnızca JSON türleri ile uğraşmak için sınırlıdır. Daha fazla bilgi için [JSON belirtimine](https://www.json.org/)bakın.  

* Cosmos konteynerjson öğeleri bir şema ücretsiz bir koleksiyondur. Konteyner öğeleri içindeki ve içindeki ilişkiler, birincil anahtar ve yabancı anahtar ilişkileri tarafından değil, çevreleme tarafından örtülü olarak yakalanır. Bu özellik, bu makalede daha sonra tartışılan madde içi birleştirmeler için önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye Giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [SELECT yan tümcesi](sql-query-select.md)
