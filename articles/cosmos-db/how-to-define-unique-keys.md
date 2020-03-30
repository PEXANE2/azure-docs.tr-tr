---
title: Azure Cosmos kapsayıcısı için benzersiz anahtarları tanımlama
description: Azure portalı, PowerShell, .Net, Java ve diğer çeşitli SDK'ları kullanarak bir Azure Cosmos kapsayıcısı için benzersiz anahtarları nasıl tanımlayacağınızı öğrenin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: thweiss
ms.openlocfilehash: fa62495a7b51c9a06a91102299378c15e811eae0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74872120"
---
# <a name="define-unique-keys-for-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısı için benzersiz anahtarları tanımlama

Bu makalede, bir Azure Cosmos kapsayıcı oluştururken [benzersiz anahtarları](unique-keys.md) tanımlamak için farklı yollar sunar. Şu anda bu işlemi Azure portalını kullanarak veya SDK'lardan biri aracılığıyla gerçekleştirmek mümkündür.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account) veya varolan bir hesabı seçin.

1. Veri **Gezgini** bölmesini açın ve üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. Yeni **Konteyner**tıklayın.

1. Kapsayıcı **Ekle** iletişim kutusunda, benzersiz bir anahtar girişi eklemek için **+ Benzersiz tuş ekle'yi** tıklatın.

1. Benzersiz anahtar kısıtlamasının yolunu(lar) girin

1. Gerekirse , **+ Ekle benzersiz tuşuna** tıklayarak daha benzersiz anahtar girişleri ekleyin

    ![Azure portalında benzersiz anahtar kısıtlama girişinin ekran görüntüsü](./media/how-to-define-unique-keys/unique-keys-portal.png)

## <a name="use-powershell"></a>Powershell'i kullanma

Bkz. benzersiz tuşlara sahip bir kapsayıcı oluşturmak için, [benzersiz anahtar ve TTL ile bir Azure Cosmos kapsayıcı oluşturun](manage-with-powershell.md#create-container-unique-key-ttl)

## <a name="use-the-net-sdk-v2"></a>.NET SDK V2'yi kullanın

[.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)kullanarak yeni bir `UniqueKeyPolicy` kapsayıcı oluştururken, bir nesne benzersiz anahtar kısıtlamaları tanımlamak için kullanılabilir.

```csharp
client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("database"), new DocumentCollection
{
    Id = "container",
    PartitionKey = new PartitionKeyDefinition { Paths = new Collection<string>(new List<string> { "/myPartitionKey" }) },
    UniqueKeyPolicy = new UniqueKeyPolicy
    {
        UniqueKeys = new Collection<UniqueKey>(new List<UniqueKey>
        {
            new UniqueKey { Paths = new Collection<string>(new List<string> { "/firstName", "/lastName", "/emailAddress" }) },
            new UniqueKey { Paths = new Collection<string>(new List<string> { "/address/zipCode" }) }
        })
    }
});
```

## <a name="use-the-net-sdk-v3"></a>.NET SDK V3'ü kullanın

[.NET SDK v3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)kullanarak yeni bir kapsayıcı oluştururken, benzersiz anahtarları kısa ve okunabilir bir şekilde bildirmek için SDK'nın akıcı API'sini kullanın.

```csharp
await client.GetDatabase("database").DefineContainer(name: "container", partitionKeyPath: "/myPartitionKey")
    .WithUniqueKey()
        .Path("/firstName")
        .Path("/lastName")
        .Path("/emailAddress")
    .Attach()
    .WithUniqueKey()
        .Path("/address/zipCode")
    .Attach()
    .CreateIfNotExistsAsync();
```

## <a name="use-the-java-sdk"></a>Java SDK'yı kullanma

[Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb)kullanarak yeni bir kapsayıcı `UniqueKeyPolicy` oluştururken, bir nesne benzersiz anahtar kısıtlamaları tanımlamak için kullanılabilir.

```java
// create a new DocumentCollection object
DocumentCollection container = new DocumentCollection();
container.setId("container");

// create array of strings and populate them with the unique key paths
Collection<String> uniqueKey1Paths = new ArrayList<String>();
uniqueKey1Paths.add("/firstName");
uniqueKey1Paths.add("/lastName");
uniqueKey1Paths.add("/emailAddress");
Collection<String> uniqueKey2Paths = new ArrayList<String>();
uniqueKey2Paths.add("/address/zipCode");

// create UniqueKey objects and set their paths
UniqueKey uniqueKey1 = new UniqueKey();
UniqueKey uniqueKey2 = new UniqueKey();
uniqueKey1.setPaths(uniqueKey1Paths);
uniqueKey2.setPaths(uniqueKey2Paths);

// create a new UniqueKeyPolicy object and set its unique keys
UniqueKeyPolicy uniqueKeyPolicy = new UniqueKeyPolicy();
Collection<UniqueKey> uniqueKeys = new ArrayList<UniqueKey>();
uniqueKeys.add(uniqueKey1);
uniqueKeys.add(uniqueKey2);
uniqueKeyPolicy.setUniqueKeys(uniqueKeys);

// set the unique key policy
container.setUniqueKeyPolicy(uniqueKeyPolicy);

// create the container
client.createCollection(String.format("/dbs/%s", "database"), container, null);
```

## <a name="use-the-nodejs-sdk"></a>Düğüm.js SDK kullanın

[Düğüm.js SDK](https://www.npmjs.com/package/@azure/cosmos)kullanarak yeni bir kapsayıcı `UniqueKeyPolicy` oluştururken, bir nesne benzersiz anahtar kısıtlamaları tanımlamak için kullanılabilir.

```javascript
client.database('database').containers.create({
    id: 'container',
    uniqueKeyPolicy: {
        uniqueKeys: [
            { paths: ['/firstName', '/lastName', '/emailAddress'] },
            { paths: ['/address/zipCode'] }
        ]
    }
});
```

## <a name="use-the-python-sdk"></a>Python SDK'yı kullan

[Python SDK](https://pypi.org/project/azure-cosmos/)kullanarak yeni bir kapsayıcı oluştururken, benzersiz anahtar kısıtlamaları parametre olarak geçirilen sözlüğün bir parçası olarak belirtilebilir.

```python
client.CreateContainer('dbs/' + config['DATABASE'], {
    'id': 'container',
    'uniqueKeyPolicy': {
        'uniqueKeys': [
            {'paths': ['/firstName', '/lastName', '/emailAddress']},
            {'paths': ['/address/zipCode']}
        ]
    }
})
```

## <a name="next-steps"></a>Sonraki adımlar

- [Bölümleme](partition-data.md) hakkında daha fazla bilgi edinin
- [Dizin oluşturmanın nasıl çalıştığını](index-overview.md) keşfedin
