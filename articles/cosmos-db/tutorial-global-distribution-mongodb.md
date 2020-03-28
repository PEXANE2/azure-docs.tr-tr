---
title: MongoDB için Azure Cosmos DB API ile küresel dağıtım ayarlama eğitimi
description: Azure Cosmos DB'nin MongoDB için API'sini kullanarak küresel dağıtımı nasıl ayarlayamaize cereyiş yapılacağını öğrenin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: tutorial
ms.date: 12/26/2018
ms.reviewer: sngun
ms.openlocfilehash: b446697977395aa9bbbcf2192aa232fbc85a0b68
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75444677"
---
# <a name="set-up-global-distributed-database-using-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'sını kullanarak genel dağıtılmış veritabanını ayarlama

Bu makalede, azure portalını küresel dağıtılmış bir veritabanı kurmak ve MongoDB için Azure Cosmos DB'nin API'sini kullanarak ona bağlanmak için nasıl kullanacağımızı gösteriyoruz.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * [MongoDB için Azure Cosmos DB'nin API'sini](mongodb-introduction.md) kullanarak küresel dağıtımı yapılandırın

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup"></a>Bölgesel kurulumunuzu doğrulama 
MongoDB için Cosmos DB'nin API'si ile küresel yapılandırmanızı kontrol etmenin basit bir yolu, Mongo Shell'den *isMaster()* komutunu çalıştırmaktır.

Mongo Kabuğunuzdan:

   ```
      db.isMaster()
   ```
   
Örnek sonuçlar:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region"></a>Tercih edilen bölgeye bağlanma 

Azure Cosmos DB'nin MongoDB için API'si, koleksiyonunuzun genel olarak dağıtılmış bir veritabanı için okuma tercihini belirtmenize olanak tanır. Hem düşük gecikmeli okumalar hem de genel yüksek kullanılabilirlik için, koleksiyonunuzun okuma tercihini *en yakın* olarak ayarlamanızı öneririz. En yakın bölgeden okumak için *en yakın* okuma tercihi yapılandırılır.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Birincil okuma/yazma bölgesi ve olağanüstü durum kurtarma (DR) senaryoları için ikincil bölge içeren uygulamalar için, koleksiyonunuzun okuma tercihini *ikincil tercih edilen* olarak ayarlamanızı öneririz. Birincil bölgenin kullanılamadığı ikincil bölgeden okumak için *ikincil tercih edilen* okuma tercihi yapılandırılır.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Son olarak, okuma bölgelerinizi kendiniz belirtmek istiyorsanız. Okuma tercihiniz içinde bölge Etiketini ayarlayabilirsiniz.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Hepsi bu kadar. Böylece bu öğretici tamamlanmış olur. [Azure Cosmos DB’deki tutarlılık düzeyleri](consistency-levels.md) bölümünü okuyarak genel olarak çoğaltılan hesabınızın tutarlılığının nasıl yönetileceğini öğrenebilirsiniz. Ayrıca genel veritabanı çoğaltmasının Azure Cosmos DB’de nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Cosmos DB ile verileri genel olarak dağıtma](distribute-data-globally.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * MongoDB için Cosmos DB'nin API'sini kullanarak küresel dağıtımı yapılandırın

Artık Azure Cosmos DB yerel öykünücüsünü kullanarak yerel olarak geliştirme konusunda bilgi almak için sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB emülatörü ile yerel olarak geliştirin](local-emulator.md)
