---
title: 2\. x Işlevleri için Azure Cosmos DB bağlamaları
description: Azure Cosmos DB Tetikleyicileri ve bağlamaları Azure işlevleri'nde nasıl kullanılacağını anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 3ef2fdcaefeedb0769eac34d292e67a99524a6f2
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77168072"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x"></a>Azure Işlevleri için Azure Cosmos DB bağlamaları 2. x

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Işlevleri çalışma zamanının sürümünü seçin: "]
> * [Sürüm 1](functions-bindings-cosmosdb.md)
> * [Sürüm 2](functions-bindings-cosmosdb-v2.md)

Bu makalede, Azure Işlevleri 2. x içinde [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md) bağlamalarıyla nasıl çalışılacağı açıklanmaktadır. Tetiklemek, giriş ve çıktı bağlaması Azure Cosmos DB için Azure işlevleri destekler.

> [!NOTE]
> Bu makale, [Azure işlevleri sürüm 2. x](functions-versions.md)içindir.  Bu bağlamaları 1. x Işlevleri içinde kullanma hakkında daha fazla bilgi için bkz. [Azure işlevleri için Azure Cosmos DB bağlamaları 1. x](functions-bindings-cosmosdb.md).
>
> Bu bağlama başlangıçta DocumentDB olarak adlandırılıyordu. Işlevler sürüm 2. x, tetikleyici, bağlamalar ve paketin adı Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-apis"></a>Desteklenen API 'Ler

[!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

2\. x Işlevleri için Azure Cosmos DB bağlamaları [Microsoft. Azure. WebJobs. Extensions. CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) NuGet paketi, sürüm 3. x içinde verilmiştir. Bağlamaların kaynak kodu, [Azure-WebJobs-SDK-Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/) GitHub deposunda bulunur.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Tetikleyici

Azure Cosmos DB tetikleyicisi, bölümler arasında eklemeleri ve güncelleştirmeleri dinlemek için [Azure Cosmos DB değişiklik akışını](../cosmos-db/change-feed.md) kullanır. Değişiklik akışı, ekler ve güncelleştirme, silme değil yayımlar.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek, belirtilen veritabanında ve koleksiyonda ekleme veya güncelleştirme olduğunda çağrılan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

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

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Cosmos DB tetikleyicisi bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi Cosmos DB kayıtları eklendiğinde veya değiştirildiğinde günlük iletilerini yazar.

Bu, *function. JSON* dosyasındaki bağlama verileri:

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

C# betik kodunu şu şekildedir:

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Cosmos DB tetikleyicisi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi Cosmos DB kayıtları eklendiğinde veya değiştirildiğinde günlük iletilerini yazar.

Bu, *function. JSON* dosyasındaki bağlama verileri:

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

JavaScript kod aşağıdaki gibidir:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Cosmos DB tetikleyicisi bağlamasını ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. Cosmos DB kayıt değiştirildiğinde işlevi günlük iletisi yazar.

Bu, *function. JSON* dosyasındaki bağlama verileri:

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

# <a name="javatabjava"></a>[Java](#tab/java)

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


[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri Cosmos DB geldiği parametrelerde `@CosmosDBTrigger` ek açıklamasını kullanın.  Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="trigger---attributes-and-annotations"></a>Tetikleyici-öznitelikler ve ek açıklamalar

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [cosmosdbtrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu, koleksiyon adı ve veritabanı adını alır. Yapılandırabileceğiniz bu ayarlar ve diğer özellikler hakkında daha fazla bilgi için bkz. [tetikleyici-yapılandırma](#trigger---configuration). Yöntem imzasında bir `CosmosDBTrigger` özniteliği örneği aşağıda verilmiştir:

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

Tüm bir örnek için bkz. [tetikleyici](#trigger).

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime), Cosmos DB verileri okuyan parametrelerde `@CosmosDBInput` ek açıklamasını kullanın.

---

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `CosmosDBTrigger` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | `cosmosDBTrigger`olarak ayarlanmalıdır. |
|**direction** | yok | `in`olarak ayarlanmalıdır. Azure portalında tetikleyicisi oluşturduğunuzda bu parametre otomatik olarak ayarlanır. |
|**ada** | yok | Değişken adı değişikliklerle belgelerin listesini temsil eden bir işlev kodunu kullanılır. |
|**connectionStringSetting**|**ConnectionStringSetting** | İzlenmekte olan Azure Cosmos DB hesabına bağlanmak için kullanılan bağlantı dizesi içeren bir uygulama ayarı adı. |
|**Dosyasında**|**Dosyasında**  | İzlenmekte olan toplama ile Azure Cosmos DB veritabanının adı. |
|**Ma** |**Ma** | İzlenmekte olan koleksiyonun adı. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | Seçim Kira toplamayı tutan Azure Cosmos DB hesaba bağlantı dizesi içeren bir uygulama ayarının adı. Ayarlanmamışsa, `connectionStringSetting` değeri kullanılır. Bu parametre, portalda bağlama oluşturulduğunda otomatik olarak ayarlanır. Bağlantı dizesini kiralarını koleksiyonuna yazma izinlerine sahip olmalıdır.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (İsteğe bağlı) Kiraları depolamak için kullanılan koleksiyonu içeren veritabanının adı. Ayarlamadığınızda `databaseName` ayarının değeri kullanılır. Bu parametre, portalda bağlama oluşturulduğunda otomatik olarak ayarlanır. |
|**leaseCollectionName** | **LeaseCollectionName** | (İsteğe bağlı) Kiraları depolamak için kullanılan koleksiyonun adı. Ayarlanmayan değer `leases` kullanılır. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | Seçim `true`olarak ayarlandığında, kiralamalar koleksiyonu zaten mevcut olmadığında otomatik olarak oluşturulur. Varsayılan değer: `false`. |
|**Leasescollectionüretilen Iş**| **Leasescollectionüretilen Iş**| Seçim Kiralamalar koleksiyonu oluşturulduğunda atanacak Istek birimi sayısını tanımlar. Bu ayar yalnızca `createLeaseCollectionIfNotExists` `true`olarak ayarlandığında kullanılır. Bu parametre, portalı kullanarak bağlama oluşturulduğunda otomatik olarak ayarlanır.
|**leaseCollectionPrefix**| **LeaseCollectionPrefix**| Seçim Ayarlandığında, bu Işlev için kira koleksiyonunda oluşturulan kiralamalar için bir ön ek olarak değer eklenir. Ön ek kullanmak, iki ayrı Azure Işlevinin aynı kira koleksiyonunu farklı ön ekler kullanarak paylaşmasına izin verir.
|**feedPollDelay**| **FeedPollDelay**| Seçim Tüm geçerli değişiklikler boşaltılır ve bu, bir bölümü akıştaki yeni değişiklikler için yoklamaya yönelik gecikme süresi (milisaniye cinsinden). Varsayılan değer 5.000 milisaniyedir veya 5 saniyedir.
|**Leaseacquireınterval**| **Leaseacquireınterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden aralığı bölümler bilinen barındırma örnekleri arasında eşit olarak dağıtılmış, işlem için bir görev başlatabilir tanımlar. 13000 (13 saniye) varsayılandır.
|**Leaseexpirationınterval**| **Leaseexpirationınterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden kira bir bölüm temsil eden bir kira alınmış aralığı tanımlar. Kira bu aralıkta yenilenmezse, süresi dolacak şekilde neden olur ve bölüm sahipliğini başka bir örneğine taşınır. 60000 (60 saniye) varsayılandır.
|**leaseRenewInterval**| **LeaseRenewInterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden geçerli bir örnek tarafından tutulan bölümler için tüm kira yenileme aralığı tanımlar. 17000 (17 saniye) varsayılandır.
|**Checkpointflik**| **Checkpointflik**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden kira kontrol noktaları arasındaki süreyi tanımlar. Varsayılan değer her bir Işlev çağrısından sonra olur.
|**Maxıtemsperınvocation**| **Maxıtemsperınvocation**| Seçim Ayarlandığında, bu özellik, Işlev çağrısı başına alınan en fazla öğe sayısını ayarlar. İzlenen koleksiyondaki işlemler saklı yordamlar aracılığıyla gerçekleştiriliyorsa, değişiklik akışından öğe okurken [işlem kapsamı](../cosmos-db/stored-procedures-triggers-udfs.md#transactions) korunur. Sonuç olarak, aynı işlem tarafından değiştirilen öğelerin bir atomik toplu işin parçası olarak döndürülmesi için alınan öğe sayısı belirtilen değerden daha yüksek olabilir.
|**Startfromstarted**| **Startfromstarted**| Seçim Bu seçenek tetikleyicisine, geçerli zamandan başlamak yerine koleksiyonun değişiklik geçmişinden başlayarak değişiklikleri okumasını söyler. Başlangıçtan itibaren okuma, sonraki çalışmalarda olduğu gibi, yalnızca tetikleyici başlatıldığında çalışır ve kontrol noktaları zaten depolanır. Zaten oluşturulan kiralamalar varsa `true` için bu seçeneğin ayarlanması etkisizdir.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

Tetikleyici, bölümler üzerinde _kiraları_ depolamak için kullanacağı ikinci bir koleksiyon gerektirir. İzlenmekte olan toplama hem kiraları içeren koleksiyonu tetikleyicinin çalışma için kullanılabilir olmalıdır.

>[!IMPORTANT]
> Birden çok işlev aynı koleksiyon için Cosmos DB tetikleyicisi kullanacak şekilde yapılandırıldıysa, işlevlerin her biri adanmış bir kira koleksiyonu kullanmalı veya her bir işlev için farklı bir `LeaseCollectionPrefix` belirtmelidir. Aksi takdirde, yalnızca bir işlev tetiklenir. Ön ek hakkında daha fazla bilgi için [yapılandırma bölümüne](#trigger---configuration)bakın.

Tetikleyici, bir belge güncelleştirildi veya eklenen belge yalnızca sağladığı göstermiyor. Güncelleştirmeler ve ekler farklı şekilde işlemek istiyorsanız, zaman damgası alanları ekleme veya güncelleştirme uygulayarak yapabilirsiniz.

## <a name="input"></a>Girdi

Azure Cosmos DB giriş bağlama, bir veya daha fazla Azure Cosmos DB belgesi alınacağını SQL API'sini kullanır ve bunları işlevin giriş parametresi geçirir. Belge kimliği veya sorgu parametreleri işlevi çağıran bir tetikleyiciye bağlı olarak belirlenebilir.

> [!NOTE]
> Koleksiyon [bölümlenmiş](../cosmos-db/partition-data.md#logical-partitions)ise, arama işlemlerinin bölüm anahtarı değerini de belirtmesi gerekir.
>

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama](#queue-trigger-look-up-id-from-json-c)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-look-up-id-from-query-string-c)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-c)
* [HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP tetikleyicisi, SqlQuery kullanarak birden çok belge al](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP tetikleyicisi, DocumentClient kullanarak birden çok belge edinme](#http-trigger-get-multiple-docs-using-documentclient-c)

Örnekler basit bir `ToDoItem` türüne başvurur:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string PartitionKey { get; set; }
        public string Description { get; set; }
    }
}
```

<a id="queue-trigger-look-up-id-from-json-c"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama 

Aşağıdaki örnek tek bir belgeyi alan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir. İşlev bir JSON nesnesi içeren bir kuyruk iletisi tarafından tetiklenir. Sıra tetikleyicisi, JSON öğesini, aranacak KIMLIĞI ve bölüm anahtarı değerini içeren `ToDoItemLookup`türünde bir nesne olarak ayrıştırır. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }

        public string ToDoItemPartitionKeyValue { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}",
                PartitionKey = "{ToDoItemPartitionKeyValue}")]ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId} Key={toDoItemLookup?.ToDoItemPartitionKeyValue}");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

<a id="http-trigger-look-up-id-from-query-string-c"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnek tek bir belgeyi alan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

>[!NOTE]
>HTTP sorgu dizesi parametresi, büyük/küçük harfe duyarlıdır.
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}",
                PartitionKey = "{Query.partitionKey}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-look-up-id-from-route-data-c"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnek tek bir belgeyi alan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{partitionKey}/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}",
                PartitionKey = "{partitionKey}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden KIMLIK arama

Aşağıdaki örnek tek bir belgeyi alan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir. İşlevi, kullandığı veri aramak için kimliği belirtmek için yol bir HTTP isteği tarafından tetiklenir. Bu KIMLIK, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

Örnek, `SqlQuery` parametresinde bağlama ifadesinin nasıl kullanılacağını gösterir. Yol verilerini gösterildiği gibi `SqlQuery` parametresine geçirebilirsiniz, ancak şu anda [sorgu dizesi değerlerini geçiremezsiniz](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).

> [!NOTE]
> Yalnızca KIMLIĞE göre sorgulama yapmanız gerekiyorsa, [Önceki örneklerde](#http-trigger-look-up-id-from-query-string-c)olduğu gibi, daha az [istek birimi](../cosmos-db/request-units.md)tükettiği için bir arama kullanmanız önerilir. Nokta okuma işlemleri (GET), KIMLIĞE göre sorgulardan [daha etkilidir](../cosmos-db/optimize-cost-queries.md) .
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-get-multiple-docs-using-sqlquery-c"></a>

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak birden çok belge al

Aşağıdaki örnek, belge listesini alan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir. İşlevi, bir HTTP isteği tarafından tetiklenir. Sorgu `SqlQuery` Attribute özelliğinde belirtilir.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

<a id="http-trigger-get-multiple-docs-using-documentclient-c"></a>

### <a name="http-trigger-get-multiple-docs-using-documentclient"></a>HTTP tetikleyicisi, DocumentClient kullanarak birden çok belge edinme

Aşağıdaki örnek, belge listesini alan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir. İşlevi, bir HTTP isteği tarafından tetiklenir. Kod, bir belge listesini okumak için Azure Cosmos DB bağlama tarafından sunulan bir `DocumentClient` örneğini kullanır. `DocumentClient` örneği, yazma işlemleri için de kullanılabilir.

> [!NOTE]
> Testi kolaylaştırmak için [ıdocumentclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.idocumentclient?view=azure-dotnet) arabirimini de kullanabilirsiniz.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.LogInformation($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.LogInformation(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, dizeden KIMLIK arama](#queue-trigger-look-up-id-from-string-c-script)
* [Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-look-up-id-from-query-string-c-script)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-c-script)
* [HTTP tetikleyicisi, SqlQuery kullanarak birden çok belge al](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP tetikleyicisi, DocumentClient kullanarak birden çok belge edinme](#http-trigger-get-multiple-docs-using-documentclient-c-script)

HTTP tetikleyicisi örnekleri basit bir `ToDoItem` türüne başvurur:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

<a id="queue-trigger-look-up-id-from-string-c-script"></a>

### <a name="queue-trigger-look-up-id-from-string"></a>Kuyruk tetikleyicisi, dizeden KIMLIK arama

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, tek bir belge okur ve belgenin metin değerini güncelleştirir.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
}
```
[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodunu şu şekildedir:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {
      inputDocument.text = "This has changed.";
    }
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Azure Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, belirtilen sorgu parametrelerini özelleştirmek için bir kuyruk tetikleyicisi kullanarak bir SQL sorgusu tarafından birden çok belge alır.

Sıra tetikleyicisi bir parametre `departmentId`sağlar. `{ "departmentId" : "Finance" }` kuyruk iletisi, finans departmanı için tüm kayıtları döndürür.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodunu şu şekildedir:

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {
        foreach (var doc in documents)
        {
            // operate on each document
        }
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

<a id="http-trigger-look-up-id-from-query-string-c-script"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnekte, tek bir belgeyi alan bir [ C# betik işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

İşte *function. JSON* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey" : "{Query.partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-look-up-id-from-route-data-c-script"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [ C# betik işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

İşte *function. JSON* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak birden çok belge al

Aşağıdaki örnek, belge listesini alan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, bir HTTP isteği tarafından tetiklenir. Sorgu `SqlQuery` Attribute özelliğinde belirtilir.

İşte *function. JSON* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.LogInformation(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>

### <a name="http-trigger-get-multiple-docs-using-documentclient"></a>HTTP tetikleyicisi, DocumentClient kullanarak birden çok belge edinme

Aşağıdaki örnek, belge listesini alan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, bir HTTP isteği tarafından tetiklenir. Kod, bir belge listesini okumak için Azure Cosmos DB bağlama tarafından sunulan bir `DocumentClient` örneğini kullanır. `DocumentClient` örneği, yazma işlemleri için de kullanılabilir.

İşte *function. JSON* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.LogInformation($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.LogInformation(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Bu bölüm, çeşitli kaynaklardan bir KIMLIK değeri belirterek tek bir belgeyi okuyan aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama](#queue-trigger-look-up-id-from-json-javascript)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-look-up-id-from-query-string-javascript)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-javascript)
* [Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

<a id="queue-trigger-look-up-id-from-json-javascript"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi, tek bir belge okur ve belgenin metin değerini güncelleştirir.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

<a id="http-trigger-look-up-id-from-query-string-javascript"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnekte, tek bir belgeyi alan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

İşte *function. JSON* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey": "{Query.partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

<a id="http-trigger-look-up-id-from-route-data-javascript"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

İşte *function. JSON* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Azure Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi, belirtilen sorgu parametrelerini özelleştirmek için bir kuyruk tetikleyicisi kullanarak bir SQL sorgusu tarafından birden çok belge alır.

Sıra tetikleyicisi bir parametre `departmentId`sağlar. `{ "departmentId" : "Finance" }` kuyruk iletisi, finans departmanı için tüm kayıtları döndürür.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
    module.exports = function (context, input) {
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }
        context.done();
    };
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Bu bölüm, çeşitli kaynaklardan bir KIMLIK değeri belirterek tek bir belgeyi okuyan aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama](#queue-trigger-look-up-id-from-json-python)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara](#http-trigger-look-up-id-from-query-string-python)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-python)
* [Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma](#queue-trigger-get-multiple-docs-using-sqlquery-python)

<a id="queue-trigger-look-up-id-from-json-python"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON 'dan KIMLIK arama

Aşağıdaki örnek, bir *function. JSON* dosyasında bir Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. İşlevi, tek bir belge okur ve belgenin metin değerini güncelleştirir.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "$return",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

Python kodu aşağıda verilmiştir:

```python
import azure.functions as func


def main(queuemsg: func.QueueMessage, documents: func.DocumentList) -> func.Document:
    if documents:
        document = documents[0]
        document['text'] = 'This was updated!'
        return document
```

<a id="http-trigger-look-up-id-from-query-string-python"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara

Aşağıdaki örnekte, tek bir belgeyi alan bir [Python işlevi](functions-reference-python.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

İşte *function. JSON* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey": "{Query.partitionKeyValue}"
    }
  ],
  "disabled": true,
  "scriptFile": "__init__.py"
}
```

Python kodu aşağıda verilmiştir:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])

    return 'OK'
```

<a id="http-trigger-look-up-id-from-route-data-python"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [Python işlevi](functions-reference-python.md) gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan `ToDoItem` bir belge almak için kullanılır.

İşte *function. JSON* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Python kodu aşağıda verilmiştir:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])
    return 'OK'
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-python"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>Kuyruk tetikleyicisi, SqlQuery kullanarak birden çok belge alma

Aşağıdaki örnek, bir *function. JSON* dosyasında bir Azure Cosmos DB girişi bağlamasını ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir. İşlevi, belirtilen sorgu parametrelerini özelleştirmek için bir kuyruk tetikleyicisi kullanarak bir SQL sorgusu tarafından birden çok belge alır.

Sıra tetikleyicisi bir parametre `departmentId`sağlar. `{ "departmentId" : "Finance" }` kuyruk iletisi, finans departmanı için tüm kayıtları döndürür.

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

Python kodu aşağıda verilmiştir:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```

# <a name="javatabjava"></a>[Java](#tab/java)

Bu bölüm aşağıdaki örnekleri içerir:

* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara dize parametresi](#http-trigger-look-up-id-from-query-string---string-parameter-java)
* [HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara-POJO parametresi](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)
* [HTTP tetikleyicisi, rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-java)
* [HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden KIMLIK arama](#http-trigger-look-up-id-from-route-data-using-sqlquery-java)
* [HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden birden çok belge alın](#http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java)

Örnekler basit bir `ToDoItem` türüne başvurur:

```java
public class ToDoItem {

  private String id;
  private String description;

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }

  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}
```

<a id="http-trigger-look-up-id-from-query-string---string-parameter-java"></a>

### <a name="http-trigger-look-up-id-from-query-string---string-parameter"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara dize parametresi

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan bir belgeyi dize biçiminde almak için kullanılır.

```java
public class DocByIdFromQueryString {

    @FunctionName("DocByIdFromQueryString")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            Optional<String> item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri Cosmos DB geldiği işlev parametrelerinde `@CosmosDBInput` ek açıklamayı kullanın.  Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

<a id="http-trigger-look-up-id-from-query-string---pojo-parameter-java"></a>

### <a name="http-trigger-look-up-id-from-query-string---pojo-parameter"></a>HTTP tetikleyicisi, sorgu dizesinden KIMLIĞI ara-POJO parametresi

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan bir belgeyi almak için kullanılır. Daha sonra belge daha önce oluşturulan ```ToDoItem``` POJO örneğine dönüştürülür ve işleve bağımsız değişken olarak geçirilir.

```java
public class DocByIdFromQueryStringPojo {

    @FunctionName("DocByIdFromQueryStringPojo")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Item from the database is " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

<a id="http-trigger-look-up-id-from-route-data-java"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleyicisi, rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aranacak KIMLIĞI ve bölüm anahtarı değerini belirtmek için yol parametresi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK ve bölüm anahtarı değeri, belirtilen veritabanından ve koleksiyondan bir belgeyi almak için kullanılır, bu da bir ```Optional<String>```olarak döndürülüyor.

```java
public class DocByIdFromRoute {

    @FunctionName("DocByIdFromRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems/{partitionKeyValue}/{id}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{id}",
              partitionKey = "{partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            Optional<String> item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

 <a id="http-trigger-look-up-id-from-route-data-using-sqlquery-java"></a>

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden KIMLIK arama

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aranacak KIMLIĞI belirtmek için yol parametresi kullanan bir HTTP isteği tarafından tetiklenir. Bu KIMLIK, sorgu ölçütlerine bağlı olarak birçok belge döndürüldüğünden, belirtilen veritabanından ve ```ToDoItem[]```koleksiyondan bir belgeyi almak için kullanılır.

> [!NOTE]
> Yalnızca KIMLIĞE göre sorgulama yapmanız gerekiyorsa, [Önceki örneklerde](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)olduğu gibi, daha az [istek birimi](../cosmos-db/request-units.md)tükettiği için bir arama kullanmanız önerilir. Nokta okuma işlemleri (GET), KIMLIĞE göre sorgulardan [daha etkilidir](../cosmos-db/optimize-cost-queries.md) .
>

```java
public class DocByIdFromRouteSqlQuery {

    @FunctionName("DocByIdFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems2/{id}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where r.id = {id}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem[] item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Items from the database are " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

 <a id="http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java"></a>

### <a name="http-trigger-get-multiple-docs-from-route-data-using-sqlquery"></a>HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden birden çok belge alın

Aşağıdaki örnekte, birden çok belgeyi alan bir Java işlevi gösterilmektedir. İşlevi, ```description``` alanında aranacak dizeyi belirtmek için ```desc``` yol parametresi kullanan bir HTTP isteği tarafından tetiklenir. Arama terimi, belirtilen veritabanından ve koleksiyondan bir belge koleksiyonunu almak, sonuç kümesini bir ```ToDoItem[]``` dönüştürmek ve işleve bağımsız değişken olarak geçirmek için kullanılır.

```java
public class DocsFromRouteSqlQuery {

    @FunctionName("DocsFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems3/{desc}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where contains(r.description, {desc})",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem[] items,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Number of items from the database is " + (items == null ? 0 : items.length));

        // Convert and display
        if (items == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("No documents found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(items)
                          .build();
        }
    }
}
 ```

 ---

## <a name="input---attributes-and-annotations"></a>Giriş öznitelikleri ve ek açıklamalar

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [cosmosdb](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu, koleksiyon adı ve veritabanı adını alır. Yapılandırabileceğiniz bu ayarlar ve diğer özellikler hakkında daha fazla bilgi için [aşağıdaki yapılandırma bölümüne](#input---configuration)bakın.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime), Cosmos DB yazılan parametrelerde `@CosmosDBOutput` ek açıklamasını kullanın. Ek açıklama parametre türü `OutputBinding<T>`olmalıdır; burada `T` yerel bir Java türü ya da POJO olur.

---

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `CosmosDB` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**     | yok | `cosmosDB`olarak ayarlanmalıdır.        |
|**direction**     | yok | `in`olarak ayarlanmalıdır.         |
|**ada**     | yok | İşlevinde belgeyi temsil eden bağlama parametresinin adı.  |
|**Dosyasında** |**Dosyasında** |Belge içeren veritabanı.        |
|**Ma** |**Ma** | Belgeyi içeren koleksiyon adı. |
|**numarasını**    | **Kimlik** | Alınacak belgenin kimliği. Bu özellik [bağlama ifadelerin](./functions-bindings-expressions-patterns.md)kullanılmasını destekler. Hem `id` hem de **SQLQuery** özelliklerini ayarlama. Tek ayarlamazsanız, tüm koleksiyon alınır. |
|**sqlQuery**  |**SqlQuery**  | Birden çok belge almak için kullanılan bir Azure Cosmos DB SQL sorgusu. Özelliği, şu örnekte olduğu gibi çalışma zamanı bağlamalarını destekler: `SELECT * FROM c where c.departmentId = {departmentId}`. Hem `id` hem de `sqlQuery` özelliklerini ayarlama. Tek ayarlamazsanız, tüm koleksiyon alınır.|
|**connectionStringSetting**     |**ConnectionStringSetting**|Azure Cosmos DB bağlantı dizenizi içeren uygulama ayarının adı.        |
|**partitionKey**|**PartitionKey**|Arama için bölüm anahtarı değeri belirtir. Bağlama parametrelerinde içerebilir. [Bölümlenmiş](../cosmos-db/partition-data.md#logical-partitions) koleksiyonlardaki aramalar için gereklidir.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

İşlev başarıyla çıktığında, giriş belgesinde adlandırılmış giriş parametreleri aracılığıyla yapılan tüm değişiklikler otomatik olarak kalıcı yapılır.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

İşlev başarıyla çıktığında, giriş belgesinde adlandırılmış giriş parametreleri aracılığıyla yapılan tüm değişiklikler otomatik olarak kalıcı yapılır.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

İşlev çıkışı sırasında güncelleştirmeler otomatik olarak yapılmaz. Bunun yerine, güncelleştirme yapmak için `context.bindings.<documentName>In` ve `context.bindings.<documentName>Out` kullanın. Bkz. JavaScript örneği.

# <a name="pythontabpython"></a>[Python](#tab/python)

Veriler bir `DocumentList` parametresi aracılığıyla işlev için kullanılabilir hale getirilir. Belgede yapılan değişiklikler otomatik olarak kalıcı olmaz.

# <a name="javatabjava"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime), [@CosmosDBInput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.cosmosdbinput) ek açıklaması işleve Cosmos DB verileri gösterir. Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="output"></a>Çıktı

Azure Cosmos DB çıkış sağlar bağlaması SQL API'sini kullanarak bir Azure Cosmos DB veritabanına yeni bir belge yazma.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, bir belge yaz](#queue-trigger-write-one-doc-c)
* [Kuyruk tetikleyicisi, ıasynccollector kullanarak belge yazma](#queue-trigger-write-docs-using-iasynccollector-c)

Örnekler basit bir `ToDoItem` türüne başvurur:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

<a id="queue-trigger-write-one-doc-c"></a>

### <a name="queue-trigger-write-one-doc"></a>Kuyruk tetikleyicisi, bir belge yazma

Aşağıdaki örnek, kuyruk depolamadan ileti içinde belirtilen verileri kullanarak bir veritabanına belge ekleyen bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            ILogger log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.LogInformation($"C# Queue trigger function inserted one row");
            log.LogInformation($"Description={queueMessage}");
        }
    }
}
```

<a id="queue-trigger-write-docs-using-iasynccollector-c"></a>

### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi yazma docs IAsyncCollector kullanma

Aşağıdaki örnek, bir veritabanı JSON iletisinde belirtilen verileri kullanarak veritabanına bir belge koleksiyonu ekleyen bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.LogInformation($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, bir belge yaz](#queue-trigger-write-one-doc-c-script)
* [Kuyruk tetikleyicisi, ıasynccollector kullanarak belge yazma](#queue-trigger-write-docs-using-iasynccollector-c-script)


<a id="queue-trigger-write-one-doc-c-script"></a>

### <a name="queue-trigger-write-one-doc"></a>Kuyruk tetikleyicisi, bir belge yazma

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Azure Cosmos db çıktı bağlamasını ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, JSON alan şu biçimde bir kuyruk için bir kuyruk giriş bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlevi Azure Cosmos DB belgeleri her kayıt için aşağıdaki biçimde oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#output---configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodunu şu şekildedir:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;
    using Microsoft.Extensions.Logging;

    public static void Run(string myQueueItem, out object employeeDocument, ILogger log)
    {
      log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

      dynamic employee = JObject.Parse(myQueueItem);

      employeeDocument = new {
        id = employee.name + "-" + employee.employeeId,
        name = employee.name,
        employeeId = employee.employeeId,
        address = employee.address
      };
    }
```

<a id="queue-trigger-write-docs-using-iasynccollector-c-script"></a>

### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi yazma docs IAsyncCollector kullanma

Birden çok belge oluşturmak için, `T` desteklenen türlerden biri olan `ICollector<T>` veya `IAsyncCollector<T>` bağlayabilirsiniz.

Bu örnek bir basit `ToDoItem` türü anlamına gelir:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

Function.json dosyası aşağıda verilmiştir:

```json
{
  "bindings": [
    {
      "name": "toDoItemsIn",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "todoqueueforwritemulti",
      "connectionStringSetting": "AzureWebJobsStorage"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
using System;
using Microsoft.Extensions.Logging;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.LogInformation($"Description={toDoItem.Description}");
        await toDoItemsOut.AddAsync(toDoItem);
    }
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir Azure Cosmos db çıktı bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi, JSON alan şu biçimde bir kuyruk için bir kuyruk giriş bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlevi Azure Cosmos DB belgeleri her kayıt için aşağıdaki biçimde oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Bu, *function. JSON* dosyasındaki bağlama verileri:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#output---configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, bir işlevin çıktısı olarak bir Azure CosmosDB veritabanına nasıl bir belge yazılacağını gösterir.

Bağlama tanımı, *Type* 'ın `cosmosDB`olarak ayarlandığı *function. JSON* içinde tanımlanır.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "cosmosDB",
      "direction": "out",
      "name": "doc",
      "databaseName": "demodb",
      "collectionName": "data",
      "createIfNotExists": "true",
      "connectionStringSetting": "AzureCosmosDBConnectionString"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

Veritabanına yazmak için, veritabanı parametresinin `set` metoduna bir belge nesnesi geçirin.

```python
import azure.functions as func

def main(req: func.HttpRequest, doc: func.Out[func.Document]) -> func.HttpResponse:

    request_body = req.get_body()

    doc.set(func.Document.from_json(request_body))

    return 'OK'
```

# <a name="javatabjava"></a>[Java](#tab/java)

* [Kuyruk tetikleyicisi, dönüş değeri aracılığıyla iletiyi veritabanına kaydet](#queue-trigger-save-message-to-database-via-return-value-java)
* [HTTP tetikleyicisi, bir belgeyi veritabanına dönüş değeri aracılığıyla Kaydet](#http-trigger-save-one-document-to-database-via-return-value-java)
* [HTTP tetikleyicisi, OutputBinding aracılığıyla bir belgeyi veritabanına kaydetme](#http-trigger-save-one-document-to-database-via-outputbinding-java)
* [HTTP tetikleyicisi, OutputBinding aracılığıyla birden çok belgeyi veritabanına kaydetme](#http-trigger-save-multiple-documents-to-database-via-outputbinding-java)


<a id="queue-trigger-save-message-to-database-via-return-value-java"></a>

### <a name="queue-trigger-save-message-to-database-via-return-value"></a>Kuyruk tetikleyicisi, dönüş değeri aracılığıyla iletiyi veritabanına kaydet

Aşağıdaki örnek, kuyruk depolama alanındaki bir iletiden alınan verilerle bir belgeyi veritabanına ekleyen bir Java işlevini gösterir.

```java
@FunctionName("getItem")
@CosmosDBOutput(name = "database",
  databaseName = "ToDoList",
  collectionName = "Items",
  connectionStringSetting = "AzureCosmosDBConnection")
public String cosmosDbQueryById(
    @QueueTrigger(name = "msg",
      queueName = "myqueue-items",
      connection = "AzureWebJobsStorage")
    String message,
    final ExecutionContext context)  {
     return "{ id: \"" + System.currentTimeMillis() + "\", Description: " + message + " }";
   }
```
<a id="http-trigger-save-one-document-to-database-via-return-value-java"></a>

#### <a name="http-trigger-save-one-document-to-database-via-return-value"></a>HTTP tetikleyicisi, bir belgeyi veritabanına dönüş değeri aracılığıyla Kaydet

Aşağıdaki örnek, imzasına ```@CosmosDBOutput``` eklenen ve ```String```türünde dönüş değerine sahip olan bir Java işlevini gösterir. İşlevin döndürdüğü JSON belgesi, ilgili CosmosDB koleksiyonuna otomatik olarak yazılır.

```java
    @FunctionName("WriteOneDoc")
    @CosmosDBOutput(name = "database",
      databaseName = "ToDoList",
      collectionName = "Items",
      connectionStringSetting = "Cosmos_DB_Connection_String")
    public String run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " +
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        return jsonDocument;
    }
```

<a id="http-trigger-save-one-document-to-database-via-outputbinding-java"></a>

### <a name="http-trigger-save-one-document-to-database-via-outputbinding"></a>HTTP tetikleyicisi, OutputBinding aracılığıyla bir belgeyi veritabanına kaydetme

Aşağıdaki örnek, bir ```OutputBinding<T>``` output parametresi aracılığıyla CosmosDB 'ye bir belge yazan Java işlevini gösterir. Bu örnekte, ```outputItem``` parametresine işlev imzasıyla değil, ```@CosmosDBOutput```ile açıklama eklenmelidir. ```OutputBinding<T>``` kullanmak, işlevinizin, belgeyi CosmosDB 'ye yazmak için bağlamadan faydalanmasını sağlar, Ayrıca, bir JSON veya XML belgesi gibi işleve farklı bir değer döndürmeyi sağlar.

```java
    @FunctionName("WriteOneDocOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            OutputBinding<String> outputItem,
            final ExecutionContext context) {

        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " +
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        // Set outputItem's value to the JSON document to be saved
        outputItem.setValue(jsonDocument);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Document created successfully.")
                      .build();
    }
```

<a id="http-trigger-save-multiple-documents-to-database-via-outputbinding-java"></a>

### <a name="http-trigger-save-multiple-documents-to-database-via-outputbinding"></a>HTTP tetikleyicisi, OutputBinding aracılığıyla birden çok belgeyi veritabanına kaydetme

Aşağıdaki örnekte, CosmosDB 'ye bir ```OutputBinding<T>``` output parametresi aracılığıyla birden çok belge yazan bir Java işlevi gösterilmektedir. Bu örnekte, ```outputItem``` parametresine, işlev imzasıyla değil, ```@CosmosDBOutput```açıklama eklenir. Çıkış parametresi ```outputItem```, şablon parametresi türü olarak ```ToDoItem``` nesnelerinin bir listesini içerir. ```OutputBinding<T>``` kullanmak, işlevinizin, bir JSON veya XML belgesi gibi, işleve farklı bir değer döndürmeye izin verirken CosmosDB 'ye belge yazmak için bağlamadan faydalanmasını sağlar.

```java
    @FunctionName("WriteMultipleDocsOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            OutputBinding<List<ToDoItem>> outputItem,
            final ExecutionContext context) {

        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Generate documents
        List<ToDoItem> items = new ArrayList<>();

        for (int i = 0; i < 5; i ++) {
          // Generate random ID
          final int id = Math.abs(new Random().nextInt());

          // Create ToDoItem
          ToDoItem item = new ToDoItem(String.valueOf(id), name);

          items.add(item);
        }

        // Set outputItem's value to the list of POJOs to be saved
        outputItem.setValue(items);
        context.getLogger().info("Document to be saved: " + items);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Documents created successfully.")
                      .build();
    }
```

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda Cosmos DB yazılacak parametrelerde `@CosmosDBOutput` ek açıklamasını kullanın.  Ek açıklama parametre türü ```OutputBinding<T>```olmalıdır; burada T, yerel bir Java türü ya da POJO.

---

## <a name="output---attributes-and-annotations"></a>Çıkış öznitelikleri ve ek açıklamaları

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [cosmosdb](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu, koleksiyon adı ve veritabanı adını alır. Yapılandırabileceğiniz bu ayarlar ve diğer özellikler hakkında daha fazla bilgi için bkz. [çıkış-yapılandırma](#output---configuration). Yöntem imzasında bir `CosmosDB` özniteliği örneği aşağıda verilmiştir:

```csharp
    [FunctionName("QueueToDocDB")]
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

`CosmosDBOutput` ek açıklaması, Cosmos DB veri yazmak için kullanılabilir. Ek açıklamayı işleve veya bağımsız bir işlev parametresine uygulayabilirsiniz. İşlev yönteminde kullanıldığında, işlevin dönüş değeri Cosmos DB için yazıldığı şeydir. Ek açıklamayı bir parametresiyle kullanırsanız, parametrenin türü, yerel bir Java türü veya POJO `T` `OutputBinding<T>` olarak bildirilmelidir.

---

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `CosmosDB` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**     | yok | `cosmosDB`olarak ayarlanmalıdır.        |
|**direction**     | yok | `out`olarak ayarlanmalıdır.         |
|**ada**     | yok | İşlevinde belgeyi temsil eden bağlama parametresinin adı.  |
|**Dosyasında** | **Dosyasında**|Belge oluşturulduğu koleksiyonu içeren veritabanı.     |
|**Ma** |**Ma**  | Belge oluşturulduğu koleksiyonun adı. |
|**Createıfnotexists çağırmanız**  |**Createıfnotexists çağırmanız**    | Mevcut değilse, koleksiyonun oluşturulup oluşturulmayacağını belirten bir Boole değeri. Varsayılan değer *false* 'dur çünkü yeni koleksiyonlar, maliyet etkilerine sahip olan ayrılmış aktarım hızı ile oluşturulmuştur. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).  |
|**partitionKey**|**PartitionKey** |`CreateIfNotExists` true olduğunda, oluşturulan koleksiyon için bölüm anahtarı yolunu tanımlar.|
|**Collectionverimlilik**|**Collectionverimlilik**| `CreateIfNotExists` true olduğunda, oluşturulan koleksiyonun [verimini](../cosmos-db/set-throughput.md) tanımlar.|
|**connectionStringSetting**    |**ConnectionStringSetting** |Azure Cosmos DB bağlantı dizenizi içeren uygulama ayarının adı.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

Çıkış parametresi işlevinizde yazdığınızda varsayılan olarak, veritabanınızdaki bir belge oluşturulur. Bu belge otomatik olarak oluşturulan bir GUID belge kimliğinin sahiptir. Çıkış parametresine geçirilen JSON nesnesindeki `id` özelliğini belirterek çıktı belgesinin belge KIMLIĞINI belirtebilirsiniz.

> [!Note]
> Var olan bir belgeyi kimliği belirttiğinizde, yeni çıkış belgesi tarafından üzerine.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| CosmosDB | [CosmosDB hata kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

<a name="host-json"></a>

## <a name="hostjson-settings"></a>Host.JSON ayarları

Bu bölümde sürümünde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır 2.x. Sürüm 2. x içindeki genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri sürüm 2. x için Host. JSON başvurusu](functions-host-json.md).

```json
{
    "version": "2.0",
    "extensions": {
        "cosmosDB": {
            "connectionMode": "Gateway",
            "protocol": "Https",
            "leaseOptions": {
                "leasePrefix": "prefix1"
            }
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|GatewayMode|Ağ geçidi|Azure Cosmos DB hizmetine bağlanırken işlev tarafından kullanılan bağlantı modu. Seçenekler `Direct` ve `Gateway`|
|Protokol|'Dir|Azure Cosmos DB hizmetine bağlantı sırasında işlev tarafından kullanılan bağlantı protokolü.  [Her iki modun açıklaması için buraya](../cosmos-db/performance-tips.md#networking) okuyun|
|leasePrefix|yok|Bir uygulamadaki tüm işlevler genelinde kullanılacak kira öneki.|

## <a name="next-steps"></a>Sonraki adımlar

* [Cosmos DB ile sunucusuz veritabanı hesaplama hakkında daha fazla bilgi edinin](../cosmos-db/serverless-computing-database.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
