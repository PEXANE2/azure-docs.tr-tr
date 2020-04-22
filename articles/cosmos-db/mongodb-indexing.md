---
title: Azure Cosmos DB'nin MongoDB için API'sinde dizin oluşturmayı yönetme
description: Bu makalede, MongoDB API'yi kullanarak Azure Cosmos DB dizin oluşturma özelliklerine genel bir bakış sunulur.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/03/2020
author: timsander1
ms.author: tisande
ms.openlocfilehash: fd602f88acf26e821e57e0a844f543aac08dad0d
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732700"
---
# <a name="manage-indexing-in-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB için API'sinde dizin oluşturmayı yönetme

Azure Cosmos DB'nin MongoDB için API'si, Azure Cosmos DB'nin temel dizin yönetimi özelliklerinden yararlanır. Bu makalede, MongoDB için Azure Cosmos DB'nin API'sini kullanarak dizin lerin nasıl eklenilen üzerinde duruluyor. [Azure Cosmos DB'de](index-overview.md) tüm API'lerde alakalı olan dizin oluşturmayla ilgili bir genel bakış da okuyabilirsiniz.

## <a name="indexing-for-mongodb-server-version-36"></a>MongoDB sunucu sürümü 3.6 için dizin oluşturma

Azure Cosmos DB'nin MongoDB sunucu sürümü için 3.6 `_id` API'si alanı otomatik olarak dizine dizine dizine dizine ekleyebilir ve bu da bırakılamaz. Parça anahtarı başına `_id` alanın benzersizliğini otomatik olarak zorlar.

Ek alanları dizine koymak için MongoDB dizin yönetimi komutlarını uygularsınız. MongoDB'de olduğu gibi, Azure Cosmos DB'nin MongoDB `_id` için api'si yalnızca alanı otomatik olarak dizine dizine dahil eder. Bu varsayılan dizin oluşturma ilkesi, varsayılan olarak tüm alanları dizine ekleyen Azure Cosmos DB SQL API'den farklıdır.

Bir sorguya bir sıralama uygulamak için, sıralama işleminde kullanılan alanlarda bir dizin oluşturmanız gerekir.

## <a name="index-types"></a>Dizin türleri

### <a name="single-field"></a>Tek alan

Herhangi bir alanda dizinler oluşturabilirsiniz. Tek alan dizininin sıralama sırası önemli değildir. Aşağıdaki komut alanında `name`bir dizin oluşturur:

`db.coll.createIndex({name:1})`

Bir sorgu, varsa birden çok tek alan dizini kullanır. Kapsayıcı başına en fazla 500 tek alan dizini oluşturabilirsiniz.

### <a name="compound-indexes-mongodb-server-version-36"></a>Bileşik dizinler (MongoDB sunucu sürümü 3.6)

Azure Cosmos DB'nin MongoDB için API'si, sürüm 3.6 wire protokolünü kullanan hesaplar için bileşik dizinleri destekler. Bileşik dizine en fazla sekiz alan ekleyebilirsiniz. MongoDB'nin aksine, yalnızca sorgunuzun aynı anda birden çok alanda verimli bir şekilde sıralanması gerekiyorsa bileşik dizini oluşturmanız gerekir. Sıralaması gerekmeyen birden çok filtresi olan sorgular için, tek bir bileşik dizin yerine birden çok tek alan dizinleri oluşturun.

Aşağıdaki komut alanlarda `name` bileşik bir indeks oluşturur ve: `age`

`db.coll.createIndex({name:1,age:1})`

Bileşik dizinleri, aşağıdaki örnekte gösterildiği gibi, aynı anda birden çok alanda verimli bir şekilde sıralamak için kullanabilirsiniz:

`db.coll.find().sort({name:1,age:1})`

Tüm alanlarda ters sıralama sırası ile bir sorguüzerinde verimli bir şekilde sıralamak için önceki bileşik dizini de kullanabilirsiniz. Bir örneği aşağıda verilmiştir:

`db.coll.find().sort({name:-1,age:-1})`

Ancak, bileşik dizindeki yolların sırası sorguyla tam olarak eşleşmelidir. Ek bir bileşik dizin gerektiren bir sorgu örneği aşağıda verilmiştir:

`db.coll.find().sort({age:1,name:1})`

### <a name="multikey-indexes"></a>Çok anahtarlı dizinler

Azure Cosmos DB, dizilerde depolanan içeriği dizileri dizilere diziletmek için çok anahtarlı dizinler oluşturur. Bir alanı dizi değeriyle diziye diziye diziye diziye diziye diziye diziye diziye diziye diziyle diziye otomatik olarak dizilersiniz.

### <a name="geospatial-indexes"></a>Jeouzamsal indeksler

Birçok jeouzamsal operatör jeouzamsal indekslerden yararlanacaktır. Şu anda Azure Cosmos DB'nin MongoDB için API'si dizinleri destekler. `2dsphere` API henüz dizinleri desteklemiyor. `2d`

Aşağıda, sahada bir jeouzamsal indeks oluşturmanın `location` bir örneği verilmiştir:

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Metin dizinleri

Azure Cosmos DB'nin MongoDB için API'si şu anda metin dizini desteklememektedir. Dizelerdeki metin arama sorguları için Azure Cosmos DB ile [Azure Bilişsel Arama](https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb) tümleştirmesi kullanmanız gerekir.

## <a name="index-properties"></a>Dizin özellikleri

Aşağıdaki işlemler, havale protokolü sürüm 3.6'yı sunan hesaplar ve önceki sürümler sunan hesaplar için yaygındır. [Desteklenen dizinler ve dizinlenmiş özellikler](mongodb-feature-support-36.md#indexes-and-index-properties)hakkında daha fazla bilgi edinebilirsiniz.

### <a name="unique-indexes"></a>Benzersiz dizinler

[Benzersiz dizinler,](unique-keys.md) iki veya daha fazla belgenin dizine alınan alanlar için aynı değeri içermediğini zorlamak için yararlıdır.

> [!IMPORTANT]
> Benzersiz dizinler yalnızca koleksiyon boşolduğunda (belge içermediğinde) oluşturulabilir.

Aşağıdaki komut alanında `student_id`benzersiz bir dizin oluşturur:

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

Parçalı koleksiyonlar için, benzersiz bir dizin oluşturmak için parça (bölüm) anahtarısağlamanız gerekir. Diğer bir deyişle, parçalı bir koleksiyondaki tüm benzersiz dizinler, alanlardan birinin bölüm anahtarı olduğu bileşik dizinlerdir.

Aşağıdaki ```coll``` komutlar, alanlarda ```university``` `student_id` benzersiz bir dizin içeren, parçalanmış bir koleksiyon `university`(parça anahtarı) oluşturur ve:

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

Önceki örnekte, yan tümceyi ```"university":1``` atlayarak aşağıdaki ileti ile bir hata döndürür:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

### <a name="ttl-indexes"></a>TTL dizinleri

Belirli bir koleksiyonda belgenin sona ermesini etkinleştirmek [için, zaman-to-live (TTL) dizini](../cosmos-db/time-to-live.md)oluşturmanız gerekir. TTL dizini, `_ts` alan üzerinde değeri `expireAfterSeconds` olan bir dizindir.

Örnek:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Önceki komut, ```db.coll``` koleksiyonda son 10 saniye içinde değiştirilmemiş tüm belgeleri siler.

> [!NOTE]
> **_ts** alanı Azure Cosmos DB'ye özgüdür ve MongoDB istemcilerinden erişilemez. Belgenin son modifikasyonunun zaman damgasını içeren ayrılmış (sistem) özelliğidir.

## <a name="track-index-progress"></a>Dizin ilerlemesini izleme

Azure Cosmos DB'nin MongoDB için API'sinin `currentOp()` 3.6 sürümü, bir veritabanı örneğinde dizin ilerlemesini izleme komutunu destekler. Bu komut, veritabanı örneğinde devam eden işlemler hakkında bilgi içeren bir belge döndürür. Yerel MongoDB'deki tüm devam eden işlemleri izlemek için `currentOp` komutu kullanırsınız. Azure Cosmos DB'nin MongoDB için API'sinde bu komut yalnızca dizin işlemini izlemeyi destekler.

Dizin ilerlemesini izlemek için komutun nasıl kullanılacağını `currentOp` gösteren bazı örnekler aşağıda verilmiştir:

* Koleksiyon için dizin ilerlemesini alın:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* Veritabanındaki tüm koleksiyonlar için dizin ilerleme sini alın:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* Azure Cosmos hesabındaki tüm veritabanları ve koleksiyonlar için dizin ilerlemesini alın:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

### <a name="examples-of-index-progress-output"></a>Dizin ilerleme çıktısı örnekleri

Dizin ilerleme ayrıntıları, geçerli dizin çalışması için ilerleme yüzdesini gösterir. Dizin ilerlemenin farklı aşamaları için çıktı belge biçimini gösteren bir örnek aşağıda verilmiştir:

- Yüzde 60'ı tamamlanmış bir "foo" koleksiyonu ve "çubuk" veritabanındaki dizin işlemi aşağıdaki çıktı belgesine sahip olacaktır. Alan, `Inprog[0].progress.total` hedef tamamlama yüzdesi olarak 100'u gösterir.

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

- Bir dizin işlemi bir "foo" koleksiyonu ve "çubuk" veritabanında yeni başladıysa, çıktı belgesi ölçülebilir bir düzeye ulaşana kadar yüzde 0 ilerleme gösterebilir.

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

- Devam eden dizin işlemi sona erdiğinde, çıktı `inprog` belgesi boş işlemleri gösterir.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

### <a name="background-index-updates"></a>Arka plan dizini güncelleştirmeleri

**Arka Plan** dizin özelliği için belirtilen değerne bakılmaksızın, dizin güncelleştirmeleri her zaman arka planda yapılır. Dizin güncelleştirmeleri İstek Birimlerini (RUs) diğer veritabanı işlemlerine göre daha düşük bir öncelikle tükettiği için, dizin değişiklikleri yazma, güncelleştirme veya silme için herhangi bir kapalı kalma süresine neden olmaz.

Yeni bir dizin eklediğinizde, sorgular hemen dizini kullanır. Bu, sorguların eşleşen tüm sonuçları döndürmeyebileceği ve bunu hata döndürmeden yapacağı anlamına gelir. Dizin dönüşümü tamamlandığında, sorgu sonuçları tutarlı olacaktır. [Dizin ilerlemesini izleyebilirsiniz.](#track-index-progress)

## <a name="migrate-collections-with-indexes"></a>Koleksiyonları dizinlerle geçirin

Şu anda, yalnızca koleksiyon belge içermediğinde benzersiz dizinler oluşturabilirsiniz. Popüler MongoDB geçiş araçları verileri aldıktan sonra benzersiz dizinler oluşturmaya çalışın. Bu sorunu aşmak için, geçiş aracının denemesine izin vermek yerine ilgili koleksiyonları ve benzersiz dizinleri el ile oluşturabilirsiniz. (Bu davranışı ```mongorestore``` komut satırındaki `--noIndexRestore` bayrağı kullanarak elde edebilirsiniz.)

## <a name="indexing-for-mongodb-version-32"></a>MongoDB sürüm 3.2 için dizin oluşturma

Kullanılabilir dizin oluşturma özellikleri ve varsayılanları, MongoDB havale protokolünün 3.2 sürümüyle uyumlu olan Azure Cosmos hesapları için farklıdır. [Hesabınızın sürümünü kontrol](mongodb-feature-support-36.md#protocol-support)edebilirsiniz. Bir [destek isteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)doldurarak 3.6 sürümüne yükseltebilirsiniz.

Sürüm 3.2 kullanıyorsanız, bu bölümde sürüm 3.6 ile ilgili temel farklılıklar özetler.

### <a name="dropping-default-indexes-version-32"></a>Varsayılan dizinleri düşürme (sürüm 3.2)

Azure Cosmos DB'nin MongoDB için API'sinin 3,6 sürümünün aksine, sürüm 3,2 varsayılan olarak her özelliği dizinler. Bir koleksiyon için bu varsayılan dizinleri bırakmak```coll```için aşağıdaki komutu kullanabilirsiniz ( ):

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

Varsayılan dizinleri bıraktıktan sonra, sürüm 3.6'da olduğu gibi daha fazla dizin ekleyebilirsiniz.

### <a name="compound-indexes-version-32"></a>Bileşik dizinler (sürüm 3.2)

Bileşik dizinler bir belgenin birden çok alanına başvurular içerir. Bileşik dizin oluşturmak istiyorsanız, [destek isteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)doldurarak sürüm 3.6'ya yükseltin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’de dizin oluşturma](../cosmos-db/index-policy.md)
* [Azure Cosmos DB'deki verilerin süresi, yaşamak için zaman içinde otomatik olarak sona eriyor](../cosmos-db/time-to-live.md)
