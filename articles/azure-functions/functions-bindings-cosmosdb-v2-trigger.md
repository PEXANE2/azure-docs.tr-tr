---
title: 2. x ve üzeri Işlevler için Azure Cosmos DB tetikleyicisi
description: Azure Işlevlerinde Azure Cosmos DB tetikleyiciyi kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 838d6244127bc1b3609ab5e925e54dbab7fe3a2d
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88212693"
---
# <a name="azure-cosmos-db-trigger-for-azure-functions-2x-and-higher"></a>Azure Işlevleri 2. x ve üzeri için Azure Cosmos DB tetikleyicisi

Azure Cosmos DB tetikleyicisi, bölümler arasında eklemeleri ve güncelleştirmeleri dinlemek için [Azure Cosmos DB değişiklik akışını](../cosmos-db/change-feed.md) kullanır. Değişiklik akışı, silme işlemleri değil, eklemeleri ve güncelleştirmeleri yayımlar.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-cosmosdb-v2.md).

<a id="example" name="example"></a>

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, belirtilen veritabanında ve koleksiyonda ekleme veya güncelleştirme olduğunda çağrılan bir [C# işlevini](functions-dotnet-class-library.md) göstermektedir.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents,
            ILogger log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.LogInformation($"Documents modified: {documents.Count}");
                log.LogInformation($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Aşağıdaki örnek, bir *function.js* bir Cosmos DB tetikleyici bağlamasını ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi Cosmos DB kayıtları eklendiğinde veya değiştirildiğinde günlük iletilerini yazar.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

C# betik kodu aşağıda verilmiştir:

```cs
    #r "Microsoft.Azure.DocumentDB.Core"

    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using Microsoft.Extensions.Logging;

    public static void Run(IReadOnlyList<Document> documents, ILogger log)
    {
      log.LogInformation("Documents modified " + documents.Count);
      log.LogInformation("First document Id " + documents[0].Id);
    }
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function.js* bir Cosmos DB tetikleyici bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi Cosmos DB kayıtları eklendiğinde veya değiştirildiğinde günlük iletilerini yazar.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

JavaScript kodu aşağıda verilmiştir:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function.js* bir Cosmos DB tetikleyici bağlamasını ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. İşlevi Cosmos DB kayıtları değiştirildiğinde günlük iletilerini yazar.

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

```json
{
    "name": "documents",
    "type": "cosmosDBTrigger",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Python kodu aşağıda verilmiştir:

```python
    import logging
    import azure.functions as func


    def main(documents: func.DocumentList) -> str:
        if documents:
            logging.info('First document Id modified: %s', documents[0]['id'])
```

# <a name="java"></a>[Java](#tab/java)

Belirtilen veritabanı ve koleksiyonda ekleme veya güncelleştirme olduğunda bu işlev çağrılır.

```java
    @FunctionName("cosmosDBMonitor")
    public void cosmosDbProcessor(
        @CosmosDBTrigger(name = "items",
            databaseName = "ToDoList",
            collectionName = "Items",
            leaseCollectionName = "leases",
            createLeaseCollectionIfNotExists = true,
            connectionStringSetting = "AzureCosmosDBConnection") String[] items,
            final ExecutionContext context ) {
                context.getLogger().info(items.length + "item(s) is/are changed.");
            }
```


[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, `@CosmosDBTrigger` değeri Cosmos DB geldiği parametrelerde ek açıklamayı kullanın.  Bu ek açıklama, kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir `Optional<T>` .

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md) [Cosmosdbtrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) özniteliğini kullanın.

Özniteliğin Oluşturucusu, veritabanı adını ve koleksiyon adını alır. Yapılandırabileceğiniz bu ayarlar ve diğer özellikler hakkında daha fazla bilgi için bkz. [tetikleyici-yapılandırma](#configuration). `CosmosDBTrigger`Bir yöntem imzasında bir öznitelik örneği aşağıda verilmiştir:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        ILogger log)
    {
        ...
    }
```

Tüm bir örnek için bkz. [tetikleyici](#example).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](/java/api/overview/azure/functions/runtime) `@CosmosDBInput` Cosmos DB verileri okuyan parametrelerde ek açıklamayı kullanın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `CosmosDBTrigger` .

|function.jsözelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle** | yok | Olarak ayarlanmalıdır `cosmosDBTrigger` . |
|**Görünüm** | yok | Olarak ayarlanmalıdır `in` . Bu parametre, Azure portal tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**ada** | yok | Değişiklik içeren belge listesini temsil eden işlev kodunda kullanılan değişken adı. |
|**connectionStringSetting**|**ConnectionStringSetting** | İzlenmekte olan Azure Cosmos DB hesabına bağlanmak için kullanılan bağlantı dizesini içeren uygulama ayarının adı. |
|**Dosyasında**|**Dosyasında**  | İzlenen koleksiyonun Azure Cosmos DB veritabanının adı. |
|**Ma** |**CollectionName** | İzlenen koleksiyonun adı. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | Seçim Kira toplamayı tutan Azure Cosmos DB hesaba bağlantı dizesi içeren bir uygulama ayarının adı. Ayarlanmayan `connectionStringSetting` değer kullanılır. Bu parametre, bağlama portalda oluşturulduğunda otomatik olarak ayarlanır. Kiralamalar koleksiyonunun bağlantı dizesinde yazma izinleri olmalıdır.|
|**leaseDatabaseName** |**LeaseDatabaseName** | Seçim Kiraları depolamak için kullanılan koleksiyonu tutan veritabanının adı. Ayarlamadığınızda, `databaseName` ayarın değeri kullanılır. Bu parametre, bağlama portalda oluşturulduğunda otomatik olarak ayarlanır. |
|**leaseCollectionName** | **LeaseCollectionName** | Seçim Kiralamaları depolamak için kullanılan koleksiyonun adı. Ayarlanmayan değer `leases` kullanılır. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | Seçim Olarak ayarlandığında `true` , kiralamalar koleksiyonu zaten mevcut olmadığında otomatik olarak oluşturulur. Varsayılan değer: `false`. |
|**Leasescollectionüretilen Iş**| **Leasescollectionüretilen Iş**| Seçim Kiralamalar koleksiyonu oluşturulduğunda atanacak Istek birimi sayısını tanımlar. Bu ayar yalnızca, `createLeaseCollectionIfNotExists` olarak ayarlandığında kullanılır `true` . Bu parametre, bağlama Portal kullanılarak oluşturulduğunda otomatik olarak ayarlanır.
|**leaseCollectionPrefix**| **LeaseCollectionPrefix**| Seçim Ayarlandığında, bu Işlev için kira koleksiyonunda oluşturulan kiralamalar için bir ön ek olarak değer eklenir. Ön ek kullanmak, iki ayrı Azure Işlevinin aynı kira koleksiyonunu farklı ön ekler kullanarak paylaşmasına izin verir.
|**feedPollDelay**| **FeedPollDelay**| Seçim Geçerli tüm değişiklikler boşaltılır sonra, bir bölümü akıştaki yeni değişiklikler için yoklayarak gecikme süresi (milisaniye cinsinden). Varsayılan değer 5.000 milisaniyedir veya 5 saniyedir.
|**Leaseacquireınterval**| **Leaseacquireınterval**| Seçim Ayarlandığında, bölümler bilinen konak örnekleri arasında eşit olarak dağıtılırsa bir görevi başlatma aralığını milisaniye cinsinden tanımlar. Varsayılan değer 13000 ' dir (13 saniye).
|**Leaseexpirationınterval**| **Leaseexpirationınterval**| Seçim Ayarlandığında, kiralamanın bir bölümü temsil eden bir kira üzerinde alındığı aralığı milisaniye cinsinden tanımlar. Kira bu Aralık dahilinde yenilenmezse, süresinin dolmasına ve bölümün sahipliğinin başka bir örneğe taşınmasına neden olur. Varsayılan değer 60000 ' dir (60 saniye).
|**leaseRenewInterval**| **LeaseRenewInterval**| Seçim Ayarlandığında, bir örnek tarafından şu anda tutulan bölümlerin tüm kiralamaları için yenileme aralığını milisaniye cinsinden tanımlar. Varsayılan değer 17000 ' dir (17 saniye).
|**Checkpointflik**| **Checkpointflik**| Seçim Ayarlandığında, kira kontrol noktaları arasındaki aralığı milisaniye cinsinden tanımlar. Varsayılan değer her bir Işlev çağrısından sonra olur.
|**Maxıtemsperınvocation**| **Maxıtemsperınvocation**| Seçim Ayarlandığında, bu özellik, Işlev çağrısı başına alınan en fazla öğe sayısını ayarlar. İzlenen koleksiyondaki işlemler saklı yordamlar aracılığıyla gerçekleştiriliyorsa, değişiklik akışından öğe okurken [işlem kapsamı](../cosmos-db/stored-procedures-triggers-udfs.md#transactions) korunur. Sonuç olarak, aynı işlem tarafından değiştirilen öğelerin bir atomik toplu işin parçası olarak döndürülmesi için alınan öğe sayısı belirtilen değerden daha yüksek olabilir.
|**Startfromstarted**| **Startfromstarted**| Seçim Bu seçenek tetikleyicisine, geçerli zamandan başlamak yerine koleksiyonun değişiklik geçmişinden başlayarak değişiklikleri okumasını söyler. Başlangıçtan itibaren okuma, sonraki çalışmalarda olduğu gibi, yalnızca tetikleyici başlatıldığında çalışır ve kontrol noktaları zaten depolanır. `true`Zaten oluşturulan kiralamalar varsa, bu seçeneğin ayarlanması etkisizdir. |
|**preferredLocations**| **PreferredLocations**| Seçim Azure Cosmos DB hizmetindeki coğrafi olarak çoğaltılan veritabanı hesapları için tercih edilen konumları (bölgeleri) tanımlar. Değerler virgülle ayrılmalıdır. Örneğin, "Doğu ABD, Orta Güney ABD, Kuzey Avrupa". |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Tetikleyici, bölümler üzerinde _kiraları_ depolamak için kullanacağı ikinci bir koleksiyon gerektirir. Tetikleyicinin çalışması için izlenmekte olan koleksiyonun ve kiralamaları içeren koleksiyonun kullanılabilir olması gerekir.

>[!IMPORTANT]
> Birden çok işlev aynı koleksiyon için bir Cosmos DB tetikleyicisi kullanacak şekilde yapılandırıldıysa, işlevlerin her biri adanmış bir kira koleksiyonu kullanmalı veya `LeaseCollectionPrefix` her bir işlev için farklı bir işlev belirtmelidir. Aksi takdirde, işlevlerden yalnızca biri tetiklenecektir. Ön ek hakkında daha fazla bilgi için [yapılandırma bölümüne](#configuration)bakın.

Tetikleyici bir belgenin güncelleştirildiğini veya eklendiğini göstermez, yalnızca belgenin kendisini sağlar. Güncelleştirmeleri işlemeniz ve farklı şekilde eklemeniz gerekiyorsa, bunu ekleme veya güncelleştirme için zaman damgası alanları uygulayarak yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB belge okuma (giriş bağlama)](./functions-bindings-cosmosdb-v2-input.md)
- [Azure Cosmos DB belgedeki değişiklikleri kaydetme (çıktı bağlama)](./functions-bindings-cosmosdb-v2-output.md)
