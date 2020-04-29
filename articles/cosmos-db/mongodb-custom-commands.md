---
title: MongoDB için Azure Cosmos DB API 'sindeki verileri yönetmek için MongoDB uzantı komutları
description: Bu makalede MongoDB için Azure Cosmos DB API 'sinde depolanan verileri yönetmek üzere MongoDB uzantı komutlarının nasıl kullanılacağı açıklanır.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: sngun
ms.openlocfilehash: f99c4d096bcbe1fbdc42cac80a491d6017266cb2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80583577"
---
# <a name="use-mongodb-extension-commands-to-manage-data-stored-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sinde depolanan verileri yönetmek için MongoDB uzantı komutlarını kullanma 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Açık kaynaklı [MongoDB istemci sürücülerinden](https://docs.mongodb.org/ecosystem/drivers)herhangi birini kullanarak MongoDB için Azure Cosmos DB API 'si ile iletişim kurabilirsiniz. Azure Cosmos DB MongoDB API 'SI, mevcut istemci sürücülerinin [MongoDB kablo protokolüne](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol)bağlanarak kullanımını sağlar.

MongoDB için Azure Cosmos DB API 'sini kullanarak, MongoDB uygulamanızda yatırımlarınızı korurken küresel dağıtım, otomatik parçalama, yüksek kullanılabilirlik, gecikme süreleri, otomatik, bekleyen şifreleme, yedeklemeler ve çok daha fazlası gibi avantajların Cosmos DB avantajlarından yararlanabilirsiniz.

## <a name="mongodb-protocol-support"></a>MongoDB protokol desteği

Varsayılan olarak, MongoDB için Azure Cosmos DB API 'SI MongoDB sunucu sürümü 3,2 ile uyumludur, daha fazla ayrıntı için bkz. [desteklenen özellikler ve sözdizimi](mongodb-feature-support.md). MongoDB sürüm 3,4 ' ye eklenen özellikler veya sorgu işleçleri Şu anda MongoDB için Azure Cosmos DB API 'sinde önizleme olarak sunulmaktadır. Aşağıdaki uzantı komutları, MongoDB için Azure Cosmos DB API 'sinde depolanan veriler üzerinde CRUD işlemleri gerçekleştirirken belirli Azure Cosmos DB işlevlerini destekler:

* [Veritabanı oluştur](#create-database)
* [Veritabanını güncelleştir](#update-database)
* [Veritabanını al](#get-database)
* [Koleksiyon Oluştur](#create-collection)
* [Koleksiyonu Güncelleştir](#update-collection)
* [Koleksiyonu al](#get-collection)

## <a name="create-database"></a><a id="create-database"></a>Veritabanı oluştur

Veritabanı uzantısı Oluştur komutu yeni bir MongoDB veritabanı oluşturur. Veritabanı adı, komutun yürütüldüğü veritabanları bağlamından kullanılır. CreateDatabase komutunun biçimi aşağıdaki gibidir:

```
{
  customAction: "CreateDatabase",
  offerThroughput: <Throughput that you want to provision on the database>
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| Şerit   |  string  |   Özel komutun adı, "CreateDatabase" olmalıdır.      |
| Offerüretilen Iş | int  | Veritabanında ayarladığınız üretilen iş hacmi. Bu parametre isteğe bağlıdır. |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtını döndürür. Çıktıda parametreler için özel komutun [varsayılan çıktısına](#default-output) bakın.

### <a name="examples"></a>Örnekler

**Veritabanı oluşturma**

"Test" adlı bir veritabanı oluşturmak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateDatabase"});
```

**Aktarım hızı ile veritabanı oluşturma**

"Test" adlı bir veritabanı oluşturmak ve 1000 ru 'nin sağlanan verimini sağlamak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

## <a name="update-database"></a><a id="update-database"></a>Veritabanını güncelleştir

Veritabanı uzantısını Güncelleştir komutu, belirtilen veritabanıyla ilişkili özellikleri güncelleştirir. Şu anda yalnızca "Offerüretilen Iş" özelliğini güncelleştirebilirsiniz.

```
{
  customAction: "UpdateDatabase",
  offerThroughput: <New throughput that you want to provision on the database> 
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| Şerit    |    string     |   Özel komutun adı. "UpdateDatabase" olmalıdır.      |
|  Offerüretilen Iş   |  int       |     Veritabanında ayarlamak istediğiniz yeni sağlanan aktarım hızı.    |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtını döndürür. Çıktıda parametreler için özel komutun [varsayılan çıktısına](#default-output) bakın.

### <a name="examples"></a>Örnekler

**Bir veritabanıyla ilişkili sağlanan aktarım hızını güncelleştirme**

"Test" adlı bir veritabanının sağlanan verimini 1200 ru 'ya güncelleştirmek için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1200 });
```

## <a name="get-database"></a><a id="get-database"></a>Veritabanını al

Veritabanı uzantısını Al komutu, veritabanı nesnesini döndürür. Veritabanı adı, komutun yürütüldüğü veritabanı bağlamından kullanılır.

```
{
  customAction: "GetDatabase"
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  Şerit   |   string      |   Özel komutun adı. "GetDatabase" olmalıdır|
        
### <a name="output"></a>Çıktı

Komut başarılı olursa, yanıt aşağıdaki alanları içeren bir belge içerir:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |   `int`     |   Yanıtın durumu. 1 = = başarılı. 0 = = hata.      |
| `database`    |    `string`        |   Veritabanının adı.      |
|   `provisionedThroughput`  |    `int`      |    Veritabanında ayarlanan üretilen iş hacmi. Bu, isteğe bağlı bir yanıt parametresidir.     |

Komut başarısız olursa, varsayılan bir özel komut yanıtı döndürülür. Çıktıda parametreler için özel komutun [varsayılan çıktısına](#default-output) bakın.

### <a name="examples"></a>Örnekler

**Veritabanını al**

"Test" adlı bir veritabanının veritabanı nesnesini almak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "GetDatabase"});
```

## <a name="create-collection"></a><a id="create-collection"></a>Koleksiyon Oluştur

Koleksiyon uzantısı Oluştur komutu yeni bir MongoDB koleksiyonu oluşturur. Veritabanı adı, komutun yürütüldüğü veritabanları bağlamından kullanılır. CreateCollection komutunun biçimi aşağıdaki gibidir:

```
{
  customAction: "CreateCollection",
  collection: <Collection Name>,
  offerThroughput: <Throughput that you want to provision on the collection>,
  shardKey: <Shard key path>  
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:

| **Alan** | **Tür** | **Gerekli** | **Açıklama** |
|---------|---------|---------|---------|
| Şerit | string | Gerekli | Özel komutun adı. "CreateCollection" olmalıdır.|
|  koleksiyonu | string | Gerekli | Koleksiyonun adı. Özel karakterlere izin verilmez.|
| Offerüretilen Iş | int | Seçim | Veritabanında ayarlanacak üretilen iş hacmi. Bu parametre sağlanmazsa, varsayılan olarak en az 400 RU/sn olur. * 10.000 RU/sn 'nin ötesinde üretilen iş miktarını belirtmek `shardKey` için parametresi gereklidir.|
| shardKey | string | Seçim | Parçalı koleksiyon için parça anahtarının yolu. İçinde `offerThroughput`10.000 ru/sn 'den fazla ayarlarsanız bu parametre gereklidir.  Belirtilmişse, ekli tüm belgelere bu değer gerekir. |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtını döndürür. Çıktıda parametreler için özel komutun [varsayılan çıktısına](#default-output) bakın.

### <a name="examples"></a>Örnekler

**Parçalı olmayan bir koleksiyon oluşturma**

"TestCollection" adlı ve 1000 ru tarafından sağlanan aktarım hızı ile birlikte bulunan bir koleksiyon oluşturmak için aşağıdaki komutu kullanın: 

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000});
``` 

**Parçalı koleksiyon oluşturma**

"TestCollection" adlı parçalı bir koleksiyon oluşturmak ve 1000 ru için üretilen iş üretimini ve "a. b" ile bir shardkey özelliği oluşturmak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000, shardKey: "a.b" });
```

## <a name="update-collection"></a><a id="update-collection"></a>Koleksiyonu Güncelleştir

Koleksiyon uzantısını Güncelleştir komutu, belirtilen koleksiyonla ilişkili özellikleri güncelleştirir.

```
{
  customAction: "UpdateCollection",
  collection: <Name of the collection that you want to update>,
  offerThroughput: <New throughput that you want to provision on the collection> 
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  Şerit   |   string      |   Özel komutun adı. "UpdateCollection" olması gerekir.      |
|   koleksiyonu   |   string      |   Koleksiyonun adı.       |
| Offerüretilen Iş   |int|   Koleksiyonda ayarlanacak sağlanan aktarım hızı.|

## <a name="output"></a>Çıktı

Varsayılan özel komut yanıtını döndürür. Çıktıda parametreler için özel komutun [varsayılan çıktısına](#default-output) bakın.

### <a name="examples"></a>Örnekler

**Bir koleksiyonla ilişkili sağlanan aktarım hızını güncelleştirme**

"TestCollection" adlı bir koleksiyonun sağlanan verimini 1200 ru olarak güncelleştirmek için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "UpdateCollection", collection: "testCollection", offerThroughput: 1200 });
```

## <a name="get-collection"></a><a id="get-collection"></a>Koleksiyonu al

Koleksiyonu al özel komutu koleksiyon nesnesini döndürür.

```
{
  customAction: "GetCollection",
  collection: <Name of the collection>
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| Şerit    |   string      |   Özel komutun adı. "GetCollection" olmalıdır.      |
|  koleksiyonu    |    string     |    Koleksiyonun adı.     |

### <a name="output"></a>Çıktı

Komut başarılı olursa, yanıt aşağıdaki alanları içeren bir belge içerir


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Yanıtın durumu. 1 = = başarılı. 0 = = hata.      |
| `database`    |    `string`     |   Veritabanının adı.      |
| `collection`    |    `string`     |    Koleksiyonun adı.     |
|  `shardKeyDefinition`   |   `document`      |  Parçalı anahtar olarak kullanılan dizin belirtim belgesi. Bu, isteğe bağlı bir yanıt parametresidir.       |
|  `provisionedThroughput`   |   `int`      |    Koleksiyonda ayarlanacak sağlanan aktarım hızı. Bu, isteğe bağlı bir yanıt parametresidir.     |

Komut başarısız olursa, varsayılan bir özel komut yanıtı döndürülür. Çıktıda parametreler için özel komutun [varsayılan çıktısına](#default-output) bakın.

### <a name="examples"></a>Örnekler

**Koleksiyonu al**

"TestCollection" adlı bir koleksiyon için koleksiyon nesnesini almak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "GetCollection", collection: "testCollection"});
```

## <a name="default-output-of-a-custom-command"></a><a id="default-output"></a>Özel bir komutun varsayılan çıkışı

Belirtilmemişse, özel bir yanıt aşağıdaki alanlara sahip bir belge içerir:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Yanıtın durumu. 1 = = başarılı. 0 = = hata.      |
| `code`    |   `int`      |   Yalnızca komut başarısız olduğunda (örneğin, ok = = 0) döndürülür. MongoDB hata kodunu içerir. Bu, isteğe bağlı bir yanıt parametresidir.      |
|  `errMsg`   |  `string`      |    Yalnızca komut başarısız olduğunda (örneğin, ok = = 0) döndürülür. Kullanıcı dostu bir hata iletisi içerir. Bu, isteğe bağlı bir yanıt parametresidir.      |

## <a name="next-steps"></a>Sonraki adımlar

Sonra aşağıdaki Azure Cosmos DB kavramları öğrenmek için devam edebilirsiniz: 

* [Azure Cosmos DB’de dizin oluşturma](../cosmos-db/index-policy.md)
* [Azure Cosmos DB verileri yaşam süresi ile otomatik olarak sona erer](../cosmos-db/time-to-live.md)
