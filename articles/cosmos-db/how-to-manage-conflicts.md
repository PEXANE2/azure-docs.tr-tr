---
title: Azure Cosmos DB'deki bölgeler arasındaki çakışmaları yönetme
description: Son yazar kazançları veya özel bir çakışma çözümlemesi ilkesi oluşturarak Azure Cosmos DB'deki çakışmaları nasıl yönetebilirsiniz öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: mjbrown
ms.openlocfilehash: 6d364f1a9974d6d638bb0f824e88ed3866644c15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247416"
---
# <a name="manage-conflict-resolution-policies-in-azure-cosmos-db"></a>Azure Cosmos DB'de çakışma çözümlemesi ilkelerini yönetme

Çok günlük yazmayla, birden çok istemci aynı öğeye yazdığında çakışmalar oluşabilir. Çakışma oluştuğunda, farklı çakışma çözümlemesi ilkeleri kullanarak çakışmayı çözebilirsiniz. Bu makalede, çakışma çözümlemesi ilkelerinin nasıl yönetilen açıklanmaktadır.

## <a name="create-a-last-writer-wins-conflict-resolution-policy"></a>Son yazar-wins çakışma çözümilkesi oluşturma

Bu örnekler, son yazar-wins çakışma çözümilkesi içeren bir kapsayıcının nasıl ayarlanyapılacağını gösterir. Son yazar-kazançlar için varsayılan yol zaman damgası `_ts` alanı veya özelliğidir. SQL API için bu, sayısal türü olan kullanıcı tanımlı bir yola da ayarlanabilir. Bir çakışmada, en yüksek değer kazanır. Yol ayarlı değilse veya geçersizse, varsayılan olarak `_ts`. Bu ilke ile çözülen çakışmalar çakışma akışında görünmüyor. Bu ilke tüm API'ler tarafından kullanılabilir.

### <a name="net-sdk-v2"></a><a id="create-custom-conflict-resolution-policy-lww-dotnet"></a>.NET SDK V2

```csharp
DocumentCollection lwwCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.lwwCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.LastWriterWins,
          ConflictResolutionPath = "/myCustomId",
      },
  });
```

### <a name="net-sdk-v3"></a><a id="create-custom-conflict-resolution-policy-lww-dotnet-v3"></a>.NET SDK V3

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.lwwCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.LastWriterWins,
            ResolutionPath = "/myCustomId",
        }
    });
```

### <a name="java-async-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-java-async"></a>Java Async SDK’sı

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a name="java-sync-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-java-sync"></a>Java Sync SDK’sı

```java
DocumentCollection lwwCollection = new DocumentCollection();
lwwCollection.setId(this.lwwCollectionName);
ConflictResolutionPolicy lwwPolicy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
lwwCollection.setConflictResolutionPolicy(lwwPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, lwwCollection);
```

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const { container: lwwContainer } = await database.containers.createIfNotExists(
  {
    id: this.lwwContainerName,
    conflictResolutionPolicy: {
      mode: "LastWriterWins",
      conflictResolutionPath: "/myCustomId"
    }
  }
);
```

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-python"></a>Python SDK'sı

```python
udp_collection = {
    'id': self.udp_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'LastWriterWins',
        'conflictResolutionPath': '/myCustomId'
    }
}
udp_collection = self.try_create_document_collection(
    create_client, database, udp_collection)
```

## <a name="create-a-custom-conflict-resolution-policy-using-a-stored-procedure"></a>Depolanmış yordamı kullanarak özel çakışma çözümleme ilkesi oluşturma

Bu örnekler çakışmayı çözümlemek için saklı yordama içeren özel çakışma çözümleme ilkesine sahip bir kapsayıcı ayarlama adımlarını göstermektedir. Bu çakışmalar, depolanan yordamınızda bir hata olmadığı sürece çakışma akışında görünmüyor. İlke kapsayıcı ile oluşturulduktan sonra, depolanan yordamı oluşturmanız gerekir. Aşağıdaki .NET SDK örneği bir örnek göstermektedir. Bu ilke yalnızca Core (SQL) Api'de desteklenir.

### <a name="sample-custom-conflict-resolution-stored-procedure"></a>Örnek özel çakışma çözümleme saklı yordamı

Özel çakışma çözümleme saklı yordamları aşağıda gösterilen işlev imzası kullanılarak uygulanmalıdır. Işlev adının, depolanan yordamı kapsayıcıya kaydederken kullanılan adla eşleşmesi gerekmez, ancak adlandırmayı basitleştirir. Burada, bu depolanan yordam için uygulanması gereken parametrelerin açıklaması verilmiştir.

- **gelen Öğe**: Çakışmaları oluşturan commit'e eklenen veya güncelleştirilen öğe. Silme işlemleri için null' dur.
- **existingItem**: Şu anda taahhüt edilen öğe. Bu değer bir güncelleştirmede null değildir ve bir ekleme veya silme için null.
- **isTombstone**: Gelen Öğenin önceden silinmiş bir öğeyle çakışıp çelişmediğinden belirten boolean. Doğru olduğunda, existingItem de null.
- **çakışan Öğeler**: Kimlik veya diğer benzersiz dizin özellikleriüzerinde gelen Öğe ile çakışan kapsayıcıdaki tüm öğelerin işlenen sürümünün dizilimi.

> [!IMPORTANT]
> Depolanan tüm yordamlarda olduğu gibi, özel bir çakışma çözümlemesi yordamı da aynı bölüm anahtarına sahip herhangi bir veriye erişebilir ve çakışmaları çözmek için herhangi bir ekleme, güncelleştirme veya silme işlemi gerçekleştirebilir.

Bu örnek depolanan `/myCustomId` yordam, yoldan en düşük değeri seçerek çakışmaları giderir.

```javascript
function resolver(incomingItem, existingItem, isTombstone, conflictingItems) {
  var collection = getContext().getCollection();

  if (!incomingItem) {
      if (existingItem) {

          collection.deleteDocument(existingItem._self, {}, function (err, responseOptions) {
              if (err) throw err;
          });
      }
  } else if (isTombstone) {
      // delete always wins.
  } else {
      if (existingItem) {
          if (incomingItem.myCustomId > existingItem.myCustomId) {
              return; // existing item wins
          }
      }

      var i;
      for (i = 0; i < conflictingItems.length; i++) {
          if (incomingItem.myCustomId > conflictingItems[i].myCustomId) {
              return; // existing conflict item wins
          }
      }

      // incoming item wins - clear conflicts and replace existing with incoming.
      tryDelete(conflictingItems, incomingItem, existingItem);
  }

  function tryDelete(documents, incoming, existing) {
      if (documents.length > 0) {
          collection.deleteDocument(documents[0]._self, {}, function (err, responseOptions) {
              if (err) throw err;

              documents.shift();
              tryDelete(documents, incoming, existing);
          });
      } else if (existing) {
          collection.replaceDocument(existing._self, incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      } else {
          collection.createDocument(collection.getSelfLink(), incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      }
  }
}
```

### <a name="net-sdk-v2"></a><a id="create-custom-conflict-resolution-policy-stored-proc-dotnet"></a>.NET SDK V2

```csharp
DocumentCollection udpCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.udpCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
          ConflictResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver"),
      },
  });

//Create the stored procedure
await clients[0].CreateStoredProcedureAsync(
UriFactory.CreateStoredProcedureUri(this.databaseName, this.udpCollectionName, "resolver"), new StoredProcedure
{
    Id = "resolver",
    Body = File.ReadAllText(@"resolver.js")
});
```

### <a name="net-sdk-v3"></a><a id="create-custom-conflict-resolution-policy-stored-proc-dotnet-v3"></a>.NET SDK V3

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.udpCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.Custom,
            ResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver")
        }
    });

await container.Scripts.CreateStoredProcedureAsync(
    new StoredProcedureProperties("resolver", File.ReadAllText(@"resolver.js"))
);
```

### <a name="java-async-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-java-async"></a>Java Async SDK’sı

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy("resolver");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

Kapsayıcınız oluşturulduktan sonra, depolanan yordamı `resolver` oluşturmanız gerekir.

### <a name="java-sync-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-java-sync"></a>Java Sync SDK’sı

```java
DocumentCollection udpCollection = new DocumentCollection();
udpCollection.setId(this.udpCollectionName);
ConflictResolutionPolicy udpPolicy = ConflictResolutionPolicy.createCustomPolicy(
        String.format("dbs/%s/colls/%s/sprocs/%s", this.databaseName, this.udpCollectionName, "resolver"));
udpCollection.setConflictResolutionPolicy(udpPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, udpCollection);
```

Kapsayıcınız oluşturulduktan sonra, depolanan yordamı `resolver` oluşturmanız gerekir.

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const { container: udpContainer } = await database.containers.createIfNotExists(
  {
    id: this.udpContainerName,
    conflictResolutionPolicy: {
      mode: "Custom",
      conflictResolutionProcedure: `dbs/${this.databaseName}/colls/${
        this.udpContainerName
      }/sprocs/resolver`
    }
  }
);
```

Kapsayıcınız oluşturulduktan sonra, depolanan yordamı `resolver` oluşturmanız gerekir.

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-python"></a>Python SDK'sı

```python
udp_collection = {
    'id': self.udp_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'Custom',
        'conflictResolutionProcedure': 'dbs/' + self.database_name + "/colls/" + self.udp_collection_name + '/sprocs/resolver'
    }
}
udp_collection = self.try_create_document_collection(
    create_client, database, udp_collection)
```

Kapsayıcınız oluşturulduktan sonra, depolanan yordamı `resolver` oluşturmanız gerekir.

## <a name="create-a-custom-conflict-resolution-policy"></a>Özel bir çakışma çözümleme ilkesi oluşturma

Bu örnekler özel çakışma çözümleme ilkesine sahip bir kapsayıcı ayarlama adımlarını göstermektedir. Bu çakışmalar çakışma akışında ortaya çıkar.

### <a name="net-sdk-v2"></a><a id="create-custom-conflict-resolution-policy-dotnet"></a>.NET SDK V2

```csharp
DocumentCollection manualCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.manualCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
      },
  });
```

### <a name="net-sdk-v3"></a><a id="create-custom-conflict-resolution-policy-dotnet-v3"></a>.NET SDK V3

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.manualCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.Custom
        }
    });
```

### <a name="java-async-sdk"></a><a id="create-custom-conflict-resolution-policy-java-async"></a>Java Async SDK’sı

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy();
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a name="java-sync-sdk"></a><a id="create-custom-conflict-resolution-policy-java-sync"></a>Java Sync SDK’sı

```java
DocumentCollection manualCollection = new DocumentCollection();
manualCollection.setId(this.manualCollectionName);
ConflictResolutionPolicy customPolicy = ConflictResolutionPolicy.createCustomPolicy(null);
manualCollection.setConflictResolutionPolicy(customPolicy);
DocumentCollection createdCollection = client.createCollection(database.getSelfLink(), collection, null).getResource();
```

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const {
  container: manualContainer
} = await database.containers.createIfNotExists({
  id: this.manualContainerName,
  conflictResolutionPolicy: {
    mode: "Custom"
  }
});
```

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-python"></a>Python SDK'sı

```python
database = client.ReadDatabase("dbs/" + self.database_name)
manual_collection = {
    'id': self.manual_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'Custom'
    }
}
manual_collection = client.CreateContainer(database['_self'], collection)
```

## <a name="read-from-conflict-feed"></a>Çakışma akışından okuma

Bu örnekler, kapsayıcının çakışma akışından okuma yöntemlerini göstermektedir. Çakışmalar, yalnızca otomatik olarak çözülmediklerinde veya özel bir çakışma ilkesi kullanıyorsa çakışma akışında gösterir.

### <a name="net-sdk-v2"></a><a id="read-from-conflict-feed-dotnet"></a>.NET SDK V2

```csharp
FeedResponse<Conflict> conflicts = await delClient.ReadConflictFeedAsync(this.collectionUri);
```

### <a name="net-sdk-v3"></a><a id="read-from-conflict-feed-dotnet-v3"></a>.NET SDK V3

```csharp
FeedIterator<ConflictProperties> conflictFeed = container.Conflicts.GetConflictQueryIterator();
while (conflictFeed.HasMoreResults)
{
    FeedResponse<ConflictProperties> conflicts = await conflictFeed.ReadNextAsync();
    foreach (ConflictProperties conflict in conflicts)
    {
        // Read the conflicted content
        MyClass intendedChanges = container.Conflicts.ReadConflictContent<MyClass>(conflict);
        MyClass currentState = await container.Conflicts.ReadCurrentAsync<MyClass>(conflict, new PartitionKey(intendedChanges.MyPartitionKey));

        // Do manual merge among documents
        await container.ReplaceItemAsync<MyClass>(intendedChanges, intendedChanges.Id, new PartitionKey(intendedChanges.MyPartitionKey));

        // Delete the conflict
        await container.Conflicts.DeleteAsync(conflict, new PartitionKey(intendedChanges.MyPartitionKey));
    }
}
```

### <a name="java-async-sdk"></a><a id="read-from-conflict-feed-java-async"></a>Java Async SDK’sı

```java
FeedResponse<Conflict> response = client.readConflicts(this.manualCollectionUri, null)
                    .first().toBlocking().single();
for (Conflict conflict : response.getResults()) {
    /* Do something with conflict */
}
```

### <a name="java-sync-sdk"></a><a id="read-from-conflict-feed-java-sync"></a>Java Sync SDK’sı

```java
Iterator<Conflict> conflictsIterator = client.readConflicts(this.collectionLink, null).getQueryIterator();
while (conflictsIterator.hasNext()) {
    Conflict conflict = conflictsIterator.next();
    /* Do something with conflict */
}
```

### <a name="nodejsjavascripttypescript-sdk"></a><a id="read-from-conflict-feed-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const container = client
  .database(this.databaseName)
  .container(this.lwwContainerName);

const { result: conflicts } = await container.conflicts.readAll().toArray();
```

### <a name="python"></a><a id="read-from-conflict-feed-python"></a>Python

```python
conflicts_iterator = iter(client.ReadConflicts(self.manual_collection_link))
conflict = next(conflicts_iterator, None)
while conflict:
    # Do something with conflict
    conflict = next(conflicts_iterator, None)
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki Azure Cosmos DB kavramları hakkında bilgi edinin:

- [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
- [Uygulamalarınızda çoklu master nasıl yapılandırılatır](how-to-multi-master.md)
- [Çok eşleme için istemcileri yapılandırın](how-to-manage-database-account.md#configure-multiple-write-regions)
- [Azure Cosmos DB hesabınızdan bölgeler ekleme veya kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
- [Uygulamalarınızda çoklu master nasıl yapılandırılatır.](how-to-multi-master.md)
- [Bölümleme ve veri dağıtımı](partition-data.md)
- [Azure Cosmos DB’de dizin oluşturma](indexing-policies.md)
