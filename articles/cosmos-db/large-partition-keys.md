---
title: Büyük bölme anahtarıyla Azure Cosmos kapsayıcıları oluşturma
description: Azure portalı ve farklı SDK'ları kullanarak büyük bölme anahtarına sahip Azure Cosmos DB'de nasıl bir kapsayıcı oluşturabilirsiniz öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: 7184a6b85e93c41dfe914813301a4b1a0c88f2cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887691"
---
# <a name="create-containers-with-large-partition-key"></a>Büyük bölme anahtarına sahip kapsayıcılar oluşturma

Azure Cosmos DB, verilerin yatay ölçeklemasını sağlamak için karma tabanlı bölümleme şeması kullanır. 3 Mayıs 2019'dan önce oluşturulan tüm Azure Cosmos kapsayıcıları, bölüm anahtarının ilk 100 baytını temel alarak karma işlem yapan bir karma işlev kullanır. Aynı ilk 100 bayt ait birden çok bölüm anahtarı varsa, bu mantıksal bölümler hizmet tarafından aynı mantıksal bölüm olarak kabul edilir. Bu, bölüm boyutu kotasının yanlış olmasına ve bölüm tuşları arasında benzersiz dizinler uygulanmasıgibi sorunlara neden olabilir. Bu sorunu çözmek için büyük bölme anahtarları tanıtılır. Azure Cosmos DB artık 2 KB'ye kadar değerlere sahip büyük bölme tuşlarını destekler.

Büyük bölme anahtarları, 2 KB'ye kadar büyük bölme tuşlarından benzersiz bir karma oluşturabilen karma işlevin geliştirilmiş bir sürümünün işlevselliği kullanılarak desteklenir. Bu karma sürüm, bölüm anahtarının boyutundan bağımsız olarak yüksek bölüm anahtarı na sahip senaryolar için de önerilir. Bir bölüm anahtar kardinalliği, örneğin bir kapsayıcıdaki ~30000 mantıksal bölüm sırasına göre benzersiz mantıksal bölümlerin sayısı olarak tanımlanır. Bu makalede, Azure portalı ve farklı SDK'lar kullanılarak büyük bir bölme anahtarına sahip bir kapsayıcının nasıl oluşturulacak olduğu açıklanmaktadır.

## <a name="create-a-large-partition-key-azure-portal"></a>Büyük bir bölüm anahtarı oluşturma (Azure portalı)

Azure portalını kullanarak yeni bir kapsayıcı oluşturduğunuzda, büyük bir bölme anahtarı oluşturmak için, **100 baytseçeneğinden daha büyük olan Benim bölümü anahtarını** denetleyin. Büyük bölüm tuşlarına ihtiyacınız yoksa veya 1,18'den büyük SDK'lar sürümünde çalışan uygulamalarınız varsa onay kutusunu seçin.

![Azure portalLarını kullanarak büyük bölüm anahtarları oluşturma](./media/large-partition-keys/large-partition-key-with-portal.png)

## <a name="create-a-large-partition-key-powershell"></a>Büyük bir bölüm tuşu oluşturma (PowerShell)

Büyük bölme anahtarı desteğine sahip bir kapsayıcı oluşturmak için bkz.

* [Büyük bölüm anahtar boyutuna sahip bir Azure Cosmos kapsayıcısı oluşturma](manage-with-powershell.md#create-container-big-pk)

## <a name="create-a-large-partition-key-net-sdk"></a>Büyük bir bölme anahtarı oluşturma (.Net SDK)

.NET SDK'yı kullanarak büyük bir bölme anahtarına `PartitionKeyDefinitionVersion.V2` sahip bir kapsayıcı oluşturmak için özelliği belirtin. Aşağıdaki örnek, PartitionKeyDefinition nesnesi içinde Sürüm özelliğini nasıl belirtecek lerini ve PartitionKeyDefinitionVersion.V2 olarak nasıl ayarlaylayamayı gösterir.

### <a name="v3-net-sdk"></a>v3 .NET SDK

```csharp
await database.CreateContainerAsync(
    new ContainerProperties(collectionName, $"/longpartitionkey")
    {
        PartitionKeyDefinitionVersion = PartitionKeyDefinitionVersion.V2,
    })
```

### <a name="v2-net-sdk"></a>v2 .NET SDK

```csharp
DocumentCollection collection = await newClient.CreateDocumentCollectionAsync(
database,
     new DocumentCollection
        {
           Id = Guid.NewGuid().ToString(),
           PartitionKey = new PartitionKeyDefinition
           {
             Paths = new Collection<string> {"/longpartitionkey" },
             Version = PartitionKeyDefinitionVersion.V2
           }
         },
      new RequestOptions { OfferThroughput = 400 });
```

## <a name="supported-sdk-versions"></a>Desteklenen SDK sürümleri

Büyük bölme tuşları, SDK'ların aşağıdaki minimum sürümleriyle desteklenir:

|SDK türü  | En düşük sürüm   |
|---------|---------|
|.NET     |    1.18     |
|Java eşitleme     |   2.4.0      |
|Java Async   |  2.5.0        |
| REST API | sürüm istek `2017-05-03` üstbilgisini `x-ms-version` kullanarak daha yüksek.|
| Resource Manager şablonu | nesne içindeki `"version":2` `partitionKey` özelliği kullanarak sürüm 2. |

Şu anda, Power BI ve Azure Logic Apps içinde büyük bölme anahtarı olan kapsayıcıları kullanamazsınız. Bu uygulamalardan büyük bir bölme anahtarı olmadan kapsayıcılar kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [Azure Cosmos DB'de İstek Birimleri](request-units.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](account-overview.md)
