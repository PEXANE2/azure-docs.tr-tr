---
title: 2\. x Işlevleri için Azure Cosmos DB girişi bağlama
description: Azure Işlevlerinde Azure Cosmos DB giriş bağlamayı kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.openlocfilehash: 4fe04d491525b8119ca21ff1118a2ea460cc0795
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77606613"
---
# <a name="azure-cosmos-db-input-binding-for-azure-functions-2x"></a>Azure Işlevleri için Azure Cosmos DB girişi bağlama 2. x

Azure Cosmos DB giriş bağlama, bir veya daha fazla Azure Cosmos DB belgesi alınacağını SQL API'sini kullanır ve bunları işlevin giriş parametresi geçirir. Belge kimliği veya sorgu parametreleri işlevi çağıran bir tetikleyiciye bağlı olarak belirlenebilir.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-cosmosdb-v2.md).

> [!NOTE]
> Koleksiyon [bölümlenmiş](../cosmos-db/partition-data.md#logical-partitions)ise, arama işlemlerinin bölüm anahtarı değerini de belirtmesi gerekir.
>

<a id="example" name="example"></a>

# <a name="c"></a>[C#](#tab/csharp)

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

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

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
[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

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

# <a name="python"></a>[Python](#tab/python)

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

Python kodu aşağıda verilmiştir:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```

# <a name="java"></a>[Java](#tab/java)

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

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [cosmosdb](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu, koleksiyon adı ve veritabanı adını alır. Yapılandırabileceğiniz bu ayarlar ve diğer özellikler hakkında daha fazla bilgi için [aşağıdaki yapılandırma bölümüne](#configuration)bakın.

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime), Cosmos DB yazılan parametrelerde `@CosmosDBOutput` ek açıklamasını kullanın. Ek açıklama parametre türü `OutputBinding<T>`olmalıdır; burada `T` yerel bir Java türü ya da POJO olur.

---

## <a name="configuration"></a>Yapılandırma

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

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C#](#tab/csharp)

İşlev başarıyla çıktığında, giriş belgesinde adlandırılmış giriş parametreleri aracılığıyla yapılan tüm değişiklikler otomatik olarak kalıcı yapılır.

# <a name="c-script"></a>[C#SCRIPT](#tab/csharp-script)

İşlev başarıyla çıktığında, giriş belgesinde adlandırılmış giriş parametreleri aracılığıyla yapılan tüm değişiklikler otomatik olarak kalıcı yapılır.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

İşlev çıkışı sırasında güncelleştirmeler otomatik olarak yapılmaz. Bunun yerine, güncelleştirme yapmak için `context.bindings.<documentName>In` ve `context.bindings.<documentName>Out` kullanın. Bkz. JavaScript örneği.

# <a name="python"></a>[Python](#tab/python)

Veriler bir `DocumentList` parametresi aracılığıyla işlev için kullanılabilir hale getirilir. Belgede yapılan değişiklikler otomatik olarak kalıcı olmaz.

# <a name="java"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime), [@CosmosDBInput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.cosmosdbinput) ek açıklaması işleve Cosmos DB verileri gösterir. Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB bir belge oluşturulduğunda veya değiştirildiğinde (tetikleyici) bir işlev çalıştırma](./functions-bindings-cosmosdb-v2-trigger.md)
- [Azure Cosmos DB belgedeki değişiklikleri kaydetme (çıktı bağlama)](./functions-bindings-cosmosdb-v2-output.md)