---
title: MongoDB uzantılı komutları Azure Cosmos DB'nin MongoDB api'sindeki verileri yönetmek için
description: Bu makalede, Azure Cosmos DB'nin MongoDB için API'sinde depolanan verileri yönetmek için MongoDB uzantı komutlarının nasıl kullanılacağı açıklanmaktadır.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: sngun
ms.openlocfilehash: f57b274715eb1c8a4d517f5655c09c366574d412
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445213"
---
# <a name="use-mongodb-extension-commands-to-manage-data-stored-in-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB apisinde depolanan verileri yönetmek için MongoDB uzantı komutlarını kullanın 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Açık kaynak [MongoDB istemci sürücülerinden](https://docs.mongodb.org/ecosystem/drivers)herhangi birini kullanarak Azure Cosmos DB'nin MongoDB api'si ile iletişim kurabilirsiniz. Azure Cosmos DB'nin MongoDB için API'si, [MongoDB tel protokolüne](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol)bağlı kalarak varolan istemci sürücülerin kullanılmasını sağlar.

Azure Cosmos DB'nin MongoDB için API'sini kullanarak, yatırımlarınızı korurken küresel dağıtım, otomatik parçalama, yüksek kullanılabilirlik, gecikme sonu garantileri, otomatik, şifreleme, yedekleme ve daha birçok şey gibi Cosmos DB avantajlarından yararlanabilirsiniz MongoDB uygulamanızda.

## <a name="mongodb-protocol-support"></a>MongoDB protokol desteği

Varsayılan olarak, Azure Cosmos DB'nin MongoDB api'si MongoDB sunucu sürümü 3.2 ile uyumludur, daha fazla ayrıntı için [desteklenen özelliklere ve sözdizimine](mongodb-feature-support.md)bakın. MongoDB sürüm 3.4'e eklenen özellikler veya sorgu işleçleri şu anda Azure Cosmos DB'nin MongoDB için API'sinde önizleme olarak kullanılabilir. Aşağıdaki uzantı komutları, Azure Cosmos DB'nin MongoDB için API'sinde depolanan verilerde CRUD işlemleri gerçekleştirirken Azure Cosmos DB'ye özgü işlevselliği destekler:

* [Veritabanı oluşturma](#create-database)
* [Veritabanını güncelleştirme](#update-database)
* [Veritabanı nı edinin](#get-database)
* [Koleksiyon oluşturma](#create-collection)
* [Koleksiyonu güncelleştir](#update-collection)
* [Koleksiyon alın](#get-collection)

## <a name="create-database"></a><a id="create-database"></a>Veritabanı oluşturma

Veritabanı uzantısı oluşturma komutu yeni bir MongoDB veritabanı oluşturur. Veritabanı adı, komutun yürütüldürün veritabanları bağlamından kullanılır. CreateDatabase komutunun biçimi aşağıdaki gibidir:

```
{
  customAction: "CreateDatabase",
  offerThroughput: <Throughput that you want to provision on the database>
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| customAction   |  string  |   Özel komutun adı, "CreateDatabase" olmalıdır.      |
| teklifThroughput | int  | Veritabanında ayarladığınız sağlanan iş girdisi. Bu parametre isteğe bağlıdır. |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtı verir. Çıktıdaki parametreler için özel komutun [varsayılan çıktısını](#default-output) görün.

### <a name="examples"></a>Örnekler

**Veritabanı oluşturma**

"Test" adlı bir veritabanı oluşturmak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateDatabase"});
```

**İş veritabanı oluşturma**

"Test" adlı bir veritabanı oluşturmak ve 1000 RUs'un kullanılabilir iş verimini oluşturmak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

## <a name="update-database"></a><a id="update-database"></a>Veritabanını güncelleştirme

Güncelleştirme veritabanı uzantısı komutu belirtilen veritabanı ile ilişkili özellikleri güncelleştirir. Şu anda yalnızca "offerThroughput" özelliğini güncelleştirebilirsiniz.

```
{
  customAction: "UpdateDatabase",
  offerThroughput: <New throughput that you want to provision on the database> 
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| customAction    |    string     |   Özel komutun adı. "UpdateDatabase" olmalıdır.      |
|  teklifThroughput   |  int       |     Veritabanında ayarlamak istediğiniz yeni sağlanan iş girdisi.    |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtı verir. Çıktıdaki parametreler için özel komutun [varsayılan çıktısını](#default-output) görün.

### <a name="examples"></a>Örnekler

**Veritabanıyla ilişkili sağlanan iş ilişkisini güncelleştirme**

1200 RUs adı "test" ile bir veritabanının sağlanan iş verimini güncelleştirmek için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1200 });
```

## <a name="get-database"></a><a id="get-database"></a>Veritabanı nı edinin

Veritabanı uzantısı nı al komutu veritabanı nesnesini döndürür. Veritabanı adı, komutun yürütüldürün veritabanı bağlamından kullanılır.

```
{
  customAction: "GetDatabase"
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  customAction   |   string      |   Özel komutun adı. "GetDatabase" olmalıdır|
        
### <a name="output"></a>Çıktı

Komut başarılı olursa, yanıt aşağıdaki alanları içeren bir belge içerir:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |   `int`     |   Yanıt durumu. 1 == başarı. 0 == hata.      |
| `database`    |    `string`        |   Veritabanının adı.      |
|   `provisionedThroughput`  |    `int`      |    Veritabanında ayarlanan sağlanan iş girdisi. Bu isteğe bağlı bir yanıt parametresi.     |

Komut başarısız olursa, varsayılan özel komut yanıtı döndürülür. Çıktıdaki parametreler için özel komutun [varsayılan çıktısını](#default-output) görün.

### <a name="examples"></a>Örnekler

**Veritabanını edinin**

"Test" adlı bir veritabanı için veritabanı nesnesini almak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "GetDatabase"});
```

## <a name="create-collection"></a><a id="create-collection"></a>Koleksiyon oluşturma

Koleksiyon uzantısı oluşturma komutu yeni bir MongoDB koleksiyonu oluşturur. Veritabanı adı, komutun yürütüldürün veritabanları bağlamından kullanılır. CreateCollection komutunun biçimi aşağıdaki gibidir:

```
{
  customAction: "CreateCollection",
  collection: <Collection Name>,
  offerThroughput: <Throughput that you want to provision on the collection>,
  shardKey: <Shard key path>  
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| customAction    | string | Özel komutun adı. "CreateCollection" olmalıdır     |
|  koleksiyonu      | string | Koleksiyonun adı                                   |
| teklifThroughput | int    | Veritabanında ayarlamak için Sağlanan ThroughPut. İsteğe bağlı bir parametre. |
| shardKey        | string | Parçalanmış bir koleksiyon oluşturmak için Shard Key yolu. İsteğe bağlı bir parametre. |

### <a name="output"></a>Çıktı

Varsayılan özel komut yanıtı verir. Çıktıdaki parametreler için özel komutun [varsayılan çıktısını](#default-output) görün.

### <a name="examples"></a>Örnekler

**Sabitlenmemiş bir koleksiyon oluşturma**

"testCollection" adı ve 1000 RUs'lu sabitlenmemiş bir koleksiyon oluşturmak için aşağıdaki komutu kullanın: 

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000});
``` 

**Kırık bir koleksiyon oluşturma**

"testCollection" adı ve 1000 RUs'lu bir leştirilmiş iş bölümü oluşturmak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "CreateCollection", collection: "testCollection", offerThroughput: 1000, shardKey: "a.b" });
```

## <a name="update-collection"></a><a id="update-collection"></a>Koleksiyonu güncelleştir

Güncelleştirme koleksiyonu uzantısı komutu, belirtilen koleksiyonla ilişkili özellikleri güncelleştirir.

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
|  customAction   |   string      |   Özel komutun adı. "UpdateCollection" olmalıdır.      |
|   koleksiyonu   |   string      |   Koleksiyonun adı.       |
| teklifThroughput   |int|   Koleksiyona ayarlanabilmek için sağlanan iş kadar.|

## <a name="output"></a>Çıktı

Varsayılan özel komut yanıtı verir. Çıktıdaki parametreler için özel komutun [varsayılan çıktısını](#default-output) görün.

### <a name="examples"></a>Örnekler

**Bir koleksiyonla ilişkili sağlanan iş ilişkisini güncelleştirme**

"testCollection" adı taşıyan bir koleksiyonun sağlanan iş minib'ini 1200 RUs'a güncelleştirmek için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "UpdateCollection", collection: "testCollection", offerThroughput: 1200 });
```

## <a name="get-collection"></a><a id="get-collection"></a>Koleksiyon alın

Toplama özel komutu al, koleksiyon nesnesini döndürür.

```
{
  customAction: "GetCollection",
  collection: <Name of the collection>
}
```

Aşağıdaki tabloda komut içindeki parametreler açıklanmaktadır:


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
| customAction    |   string      |   Özel komutun adı. "GetCollection" olmalı.      |
|  koleksiyonu    |    string     |    Koleksiyonun adı.     |

### <a name="output"></a>Çıktı

Komut başarılı olursa, yanıt aşağıdaki alanları içeren bir belge içerir


|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Yanıt durumu. 1 == başarı. 0 == hata.      |
| `database`    |    `string`     |   Veritabanının adı.      |
| `collection`    |    `string`     |    Koleksiyonun adı.     |
|  `shardKeyDefinition`   |   `document`      |  Parça anahtarı olarak kullanılan dizin belirtimi belgesi. Bu isteğe bağlı bir yanıt parametresi.       |
|  `provisionedThroughput`   |   `int`      |    Toplama yı ayarlamak için Sağlanan İşlem. Bu isteğe bağlı bir yanıt parametresi.     |

Komut başarısız olursa, varsayılan özel komut yanıtı döndürülür. Çıktıdaki parametreler için özel komutun [varsayılan çıktısını](#default-output) görün.

### <a name="examples"></a>Örnekler

**Koleksiyonu alın**

"testCollection" adlı bir koleksiyon için koleksiyon nesnesini almak için aşağıdaki komutu kullanın:

```shell
use test
db.runCommand({customAction: "GetCollection", collection: "testCollection"});
```

## <a name="default-output-of-a-custom-command"></a><a id="default-output"></a>Özel bir komutun varsayılan çıktısı

Belirtilmemişse, özel bir yanıt aşağıdaki alanları içeren bir belge içerir:

|**Alan**|**Tür** |**Açıklama** |
|---------|---------|---------|
|  `ok`   |    `int`     |   Yanıt durumu. 1 == başarı. 0 == hata.      |
| `code`    |   `int`      |   Yalnızca komut başarısız olduğunda döndürülür (yani ok == 0). MongoDB hata kodunu içerir. Bu isteğe bağlı bir yanıt parametresi.      |
|  `errMsg`   |  `string`      |    Yalnızca komut başarısız olduğunda döndürülür (yani ok == 0). Kullanıcı dostu bir hata iletisi içerir. Bu isteğe bağlı bir yanıt parametresi.      |

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki Azure Cosmos DB kavramlarını öğrenmeye devam edebilirsiniz: 

* [Azure Cosmos DB’de dizin oluşturma](../cosmos-db/index-policy.md)
* [Azure Cosmos DB'deki verilerin süresi, yaşamak için zaman içinde otomatik olarak sona eriyor](../cosmos-db/time-to-live.md)
