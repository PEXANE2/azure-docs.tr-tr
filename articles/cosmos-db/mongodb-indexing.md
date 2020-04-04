---
title: MongoDB için Azure Cosmos DB'nin API'sinde dizin oluşturma
description: Azure Cosmos DB'nin MongoDB için API'si ile dizin oluşturma özelliklerine genel bir bakış sunar.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/03/2020
author: timsander1
ms.author: tisande
ms.openlocfilehash: f3f369928270c77557337bfdb1037cc5174c39f2
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637956"
---
# <a name="indexing-using-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'sini kullanarak dizin oluşturma

Azure Cosmos DB'nin MongoDB için API'si, Azure Cosmos DB'nin temel dizin yönetimi özelliklerinden yararlanır. Bu belge, Azure Cosmos DB'nin MongoDB için API'sini kullanarak dizin eklemeye odaklanır. Azure [Cosmos DB'de](index-overview.md) tüm API'lerde alakalı olan dizin oluşturma yla ilgili bir genel bakış da okuyabilirsiniz.

## <a name="indexing-for-version-36"></a>Sürüm 3.6 için dizin oluşturma

Alan `_id` her zaman otomatik olarak dizine eklenir ve bırakılamaz. Azure Cosmos DB'nin MongoDB için API'si, parça anahtarı başına `_id` alanın benzersizliğini otomatik olarak zorlar.

Ek alanları dizine koymak için MongoDB dizin yönetimi komutlarını uygulamanız gerekir. MongoDB'de olduğu `_id` gibi, yalnızca alan otomatik olarak dizine eklenir. Bu varsayılan dizin oluşturma ilkesi, varsayılan olarak tüm alanları dizine ekleyen Azure Cosmos DB SQL API'den farklıdır.

Bir sorguya bir sıralama uygulamak için, sıralama işleminde kullanılan alanlarda bir dizin oluşturulması gerekir.

## <a name="index-types"></a>Dizin türleri

### <a name="single-field"></a>Tek alan

Herhangi bir alanda dizinler oluşturabilirsiniz. Tek alan dizininin sıralama sırası önemli değildir. Aşağıdaki komut alanında `name`bir dizin oluşturur:

`db.coll.createIndex({name:1})`

Bir sorgu, varsa birden çok tek alan dizinleri kullanır. Kapsayıcı başına en fazla 500 tek alan dizini oluşturabilirsiniz.

### <a name="compound-indexes-36"></a>Bileşik endeksler (3.6)

Bileşik dizinler 3,6 havale protokolü kullanılarak hesaplar için desteklenir. Bileşik dizine en fazla 8 alan ekleyebilirsiniz. MongoDB'nin aksine, yalnızca sorgunuzun aynı anda birden çok alanda verimli bir şekilde sıralanması gerekiyorsa bileşik dizini oluşturmanız gerekir. Sıralaması gerekmeyen birden çok filtresi olan sorgular için, tek bir bileşik dizin yerine birden çok tek alan dizinleri oluşturmanız gerekir.

Aşağıdaki komut alanlarda `name` bileşik bir indeks oluşturur ve: `age`

`db.coll.createIndex({name:1,age:1})`

Bileşik dizinler, birden çok alanda aynı anda verimli bir şekilde sıralamak için kullanılabilir, örneğin:

`db.coll.find().sort({name:1,age:1})`

Yukarıdaki bileşik dizini, tüm alanlarda ters sıralama sırasına sahip bir sorguda verimli sıralama için de kullanılabilir. Bir örneği aşağıda verilmiştir:

`db.coll.find().sort({name:-1,age:-1})`

Ancak, bileşik dizindeki yolların sırası sorguyla tam olarak eşleşmelidir. Ek bir bileşik dizin gerektiren bir sorgu örneği aşağıda verilmiştir:

`db.coll.find().sort({age:1,name:1})`

### <a name="multikey-indexes"></a>Çok anahtarlı dizinler

Azure Cosmos DB, dizilerde depolanan içeriği dizileri dizilere diziletmek için çok anahtarlı dizinler oluşturur. Bir alanı dizi değeriyle diziye diziye diziye diziye diziye diziye diziye diziye diziye diziyle diziye otomatik olarak dizilersiniz.

### <a name="geospatial-indexes"></a>Jeouzamsal indeksler

Birçok jeouzamsal operatör jeouzamsal indekslerden yararlanacaktır. Şu anda Azure Cosmos DB'nin MongoDB için API'si dizinleri destekler. `2dsphere` `2d`dizinler henüz desteklenmez.

`location` Aşağıda, sahada bir jeouzamsal indeks oluşturmak için bir örnek verilmiştir:

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Metin dizinleri

Metin dizinleri şu anda desteklenmez. Dizelerdeki metin arama sorguları için [Azure Bilişsel Arama'nın](https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb) Azure Cosmos DB ile tümleştirmesi kullanmalısınız.

## <a name="index-properties"></a>Dizin özellikleri

Aşağıdaki işlemler, hem havale protokolü sürüm 3.6'yı sunan hesaplar hem de önceki havale protokolü sürümlerine hizmet veren hesaplar için yaygındır. [Desteklenen dizinler ve dizinlenmiş özellikler](mongodb-feature-support-36.md#indexes-and-index-properties)hakkında daha fazla bilgi edinebilirsiniz.

### <a name="unique-indexes"></a>Benzersiz dizinler

[Benzersiz dizinler](unique-keys.md), iki veya daha fazla belgeyi dizini oluşturulmuş alanlar için aynı değeri içermemeye zorlarken yararlıdır.

>[!Important]
> Benzersiz dizinler yalnızca koleksiyon boşolduğunda (belge içermediğinde) oluşturulabilir.

Aşağıdaki komut "student_id" alanında benzersiz bir dizin oluşturur:

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

Parçalı koleksiyonlar için, benzersiz bir dizin oluşturmak için parça (bölüm) anahtarı nın sağlanması gerekir. Diğer bir deyişle, parçalı bir koleksiyondaki tüm benzersiz dizinler, alanlardan birinin bölüm anahtarı olduğu bileşik dizinlerdir.

Aşağıdaki komutlar student_id ve university alanlarında benzersiz bir dizin ile ```coll``` parçalı koleksiyonu (parça anahtarı ```university```) oluşturur:

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

Yukarıdaki örnekte ```"university":1``` yan tümcesi çıkarılsaydı aşağıdaki iletiyle bir hata döndürülürdü:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

### <a name="ttl-indexes"></a>TTL dizinleri

Belirli bir koleksiyonda belge geçerlilik süresini etkinleştirmek için bir ["TTL dizini" (yaşam süresi dizini)](../cosmos-db/time-to-live.md) oluşturulması gerekir. TTL dizini, _ts alanında "expireAfterSeconds" değerini içeren bir dizindir.

Örnek:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Yukarıdaki komut, ```db.coll``` koleksiyonunda son 10 saniye içinde değiştirilmemiş tüm belgelerin silinmesine neden olur.

> [!NOTE]
> **_ts** Azure Cosmos DB'ye özgü bir alandır ve MongoDB istemcilerinden erişilemez. Belgenin son değiştirme tarihinin zaman damgasını içeren ayrılmış (sistem) bir özelliktir.

## <a name="track-the-index-progress"></a>Dizin ilerlemesini izleme

Azure Cosmos DB'nin MongoDB hesapları için API'sinin `currentOp()` 3,6 sürümü, bir veritabanı örneğinde dizin ilerlemesini izleme komutunu destekler. Bu komut, veritabanı örneğinde devam eden işlemler hakkında bilgi içeren bir belge döndürür. Komut, `currentOp` yerel MongoDB'deki tüm devam eden işlemleri izlemek için kullanılırken, Azure Cosmos DB'nin MongoDB için API'sinde bu komut yalnızca dizin işlemini izlemeyi destekler.

Dizin ilerlemesini izlemek için komutun nasıl kullanılacağını `currentOp` gösteren bazı örnekler aşağıda verilmiştir:

* Koleksiyon için dizin ilerlemesini alın:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* Veritabanındaki tüm koleksiyonlar için dizin ilerleme sini alın:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* Azure Cosmos hesabındaki tüm veritabanları ve koleksiyonlar için dizin ilerlemesine izin alın:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

Dizin ilerleme ayrıntıları, geçerli dizin çalışması için ilerleme yüzdesi içerir. Aşağıdaki örnek, dizin ilerlemenin farklı aşamaları için çıktı belge biçimini gösterir:

1. Bir 'foo' koleksiyonundaki dizin işlemi ve %60 dizin oluşturma işlemini tamamlayan 'çubuk' veritabanı aşağıdaki çıktı belgesine sahip olacak. `Inprog[0].progress.total`hedef tamamlama olarak 100 gösterir.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 60 %",
                "progress" : {
                        "done" : 60,
                        "total" : 100
                },
                …………..…..
        }
        ],
        "ok" : 1
   }
   ```

2. 'Foo' koleksiyonu ve 'çubuk' veritabanında yeni başlayan bir dizin işlemi için, çıktı belgesi ölçülebilir bir düzeye ulaşana kadar %0 ilerleme gösterebilir.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 0 %",
                "progress" : {
                        "done" : 0,
                        "total" : 100
                },
                …………..…..
        }
        ],
       "ok" : 1
   }
   ```

3. Devam eden dizin işlemi tamamlandığında, çıktı belgesi boş inprog işlemleri gösterir.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

### <a name="background-index-updates"></a>Arka plan dizini güncelleştirmeleri

**Arka Plan** dizin özelliği için belirtilen değerne bakılmaksızın, dizin güncelleştirmeleri her zaman arka planda yapılır. Dizin güncelleştirmeleri RU'ları diğer veritabanı işlemlerine göre daha düşük bir öncelikte tüketir. Bu nedenle, dizin değişiklikleri yazma, güncelleştirme veya silme için herhangi bir kapalı kalma süresineden olmaz.

Yeni bir dizin eklerken, sorgular hemen kullanır. Bu, sorguların eşleşen tüm sonuçları döndüremeyebilir ve bunu herhangi bir hata döndürmeden yapacağı anlamına gelir. Dizin dönüşümü tamamlandıktan sonra, sorgu sonuçları tutarlı olacaktır. [Dizin ilerlemesini izleyebilirsiniz.](#track-the-index-progress)

## <a name="migrating-collections-with-indexes"></a>Koleksiyonları dizinlerle geçirme

Şu anda benzersiz dizinler yalnızca koleksiyon hiçbir belge içermediğinde oluşturulabilir. Popüler MongoDB geçiş araçları, verileri içeri aktardıktan sonra benzersiz dizin oluşturmaya çalışır. Bu sorunu aşmak için, kullanıcıların geçiş aracına izin vermek yerine ilgili koleksiyonları ve benzersiz dizinleri ```mongorestore``` el ile oluşturmaları `--noIndexRestore` önerilir (bu davranış için komut satırındaki bayrak kullanılarak elde edilir).

## <a name="indexing-for-version-32"></a>Sürüm 3.2 için dizin oluşturma

MongoDB havale protokolünün 3.2 sürümüyle uyumlu Azure Cosmos DB hesapları için kullanılabilir dizin oluşturma özellikleri ve varsayılanları farklıdır. [Hesabınızın sürümünü kontrol](mongodb-feature-support-36.md#protocol-support)edebilirsiniz. Bir [destek isteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)doldurarak 3.6 sürümüne yükseltebilirsiniz.

Sürüm 3.2 kullanıyorsanız, bu bölümde sürüm 3.6 ile ilgili temel farklar özetlenir.

### <a name="dropping-the-default-indexes-32"></a>Varsayılan dizinleri düşürme (3,2)

Azure Cosmos DB'nin MongoDB için API'sinin 3,6 sürümünün aksine, 3,2 sürümü varsayılan olarak her özelliği dizinler. Aşağıdaki komut, bir koleksiyon ```coll```için bu varsayılan dizinleri bırakmak için kullanılabilir:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

Varsayılan dizinleri bıraktıktan sonra, Sürüm 3.6'da yapılan ek dizinleri ekleyebilirsiniz.

### <a name="compound-indexes-32"></a>Bileşik endeksler (3.2)

Bileşik dizinler bir belgenin birden çok alanına başvurular içerir. Bileşik dizin oluşturmak istiyorsanız, lütfen [bir destek isteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)sunarak 3.6 sürümüne yükseltin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’de dizin oluşturma](../cosmos-db/index-policy.md)
* [Azure Cosmos DB'deki verilerin süresi, yaşamak için zaman içinde otomatik olarak sona eriyor](../cosmos-db/time-to-live.md)
