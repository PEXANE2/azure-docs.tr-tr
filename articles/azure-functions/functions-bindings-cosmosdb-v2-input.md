---
title: Fonksiyonlar için Azure Cosmos DB giriş bağlama 2.x
description: Azure İşlevlerinde Azure Cosmos DB giriş bağlamayı kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.openlocfilehash: eabcf40e28927919215979ccc46fa029d19adbfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943417"
---
# <a name="azure-cosmos-db-input-binding-for-azure-functions-2x"></a>Azure İşlevler 2.x için Azure Cosmos DB giriş bağlama

Azure Cosmos DB giriş bağlaması SQL API'sini kullanarak bir veya daha fazla Azure Cosmos DB belgesini getirir ve işlevin giriş parametresine iletir. Belge kimliği veya sorgu parametreleri, işlevi çağıran tetikleyiciye göre belirlenebilir.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-cosmosdb-v2.md)bakın.

> [!NOTE]
> Koleksiyon [bölümlenmişse,](../cosmos-db/partition-data.md#logical-partitions)arama işlemleri de bölüm anahtar değerini belirtmek gerekir.
>

<a id="example" name="example"></a>

# <a name="c"></a>[C #](#tab/csharp)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* [Kuyruk tetikleyicisi, JSON'dan kimlik ara](#queue-trigger-look-up-id-from-json-c)
* [HTTP tetikleyici, sorgu dizesinden kimlik aramak](#http-trigger-look-up-id-from-query-string-c)
* [HTTP tetikleme, rota verilerinden kimlik arama](#http-trigger-look-up-id-from-route-data-c)
* [HTTP tetikleme, SqlQuery kullanarak, rota verilerinden kimlik aramak](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP tetikleme, SqlQuery kullanarak, birden fazla doc almak](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [DOCUMENTClient'ı kullanarak HTTP tetikleyici, birden çok doküman alın](#http-trigger-get-multiple-docs-using-documentclient-c)

Örnekler basit `ToDoItem` bir türe başvurur:

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

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON'dan kimlik ara 

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, JSON nesnesi içeren bir sıra iletisi tarafından tetiklenir. Kuyruk tetikleyicisi, JSON'ı, `ToDoItemLookup`yukarı bakmak için kimlik ve bölüm anahtarı değerini içeren bir tür nesnesine ayırır. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

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

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyici, sorgu dizesinden kimlik aramak

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

>[!NOTE]
>HTTP sorgu dize parametresi büyük/küçük harf duyarlıdır.
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

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleme, rota verilerinden kimlik arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

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

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP tetikleme, SqlQuery kullanarak, rota verilerinden kimlik aramak

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, aramak için kimliği belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

Örnek, parametrede bağlayıcı bir `SqlQuery` ifadenin nasıl kullanılacağını gösterir. Rota verilerini gösterildiği gibi `SqlQuery` parametreye aktarabilirsiniz, ancak şu anda [sorgu dize değerlerini geçiremezsiniz.](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583)

> [!NOTE]
> Yalnızca kimlikle sorgulamanız gerekiyorsa, [önceki örneklerde](#http-trigger-look-up-id-from-query-string-c)olduğu gibi, daha az [istek birimi](../cosmos-db/request-units.md)tüketeceği için yukarı bakmanız önerilir. Nokta okuma işlemleri (GET), kimlik sorgusundan [daha verimlidir.](../cosmos-db/optimize-cost-queries.md)
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

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP tetikleme, SqlQuery kullanarak, birden fazla doc almak

Aşağıdaki örnekte, belgelerin listesini alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev bir HTTP isteği tarafından tetiklenir. Sorgu öznitelik özelliğinde `SqlQuery` belirtilir.

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

### <a name="http-trigger-get-multiple-docs-using-documentclient"></a>DOCUMENTClient'ı kullanarak HTTP tetikleyici, birden çok doküman alın

Aşağıdaki örnekte, belgelerin listesini alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev bir HTTP isteği tarafından tetiklenir. Kod, belgelerin `DocumentClient` listesini okumak için Azure Cosmos DB bağlama tarafından sağlanan bir örneği kullanır. Örnek `DocumentClient` yazma işlemleri için de kullanılabilir.

> [!NOTE]
> Testi kolaylaştırmak için [IDocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.idocumentclient?view=azure-dotnet) arabirimini de kullanabilirsiniz.

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* [Sıra tetikleyicisi, dizeden kimliği ara](#queue-trigger-look-up-id-from-string-c-script)
* [SqlQuery kullanarak sıra tetikleyicisi, birden çok doc alın](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP tetikleyici, sorgu dizesinden kimlik aramak](#http-trigger-look-up-id-from-query-string-c-script)
* [HTTP tetikleme, rota verilerinden kimlik arama](#http-trigger-look-up-id-from-route-data-c-script)
* [HTTP tetikleme, SqlQuery kullanarak, birden fazla doc almak](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [DOCUMENTClient'ı kullanarak HTTP tetikleyici, birden çok doküman alın](#http-trigger-get-multiple-docs-using-documentclient-c-script)

HTTP tetikleyici örnekleri basit `ToDoItem` bir türe başvurur:

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

### <a name="queue-trigger-look-up-id-from-string"></a>Sıra tetikleyicisi, dizeden kimliği ara

Aşağıdaki örnek, *bir function.json* dosyasında Cosmos DB giriş bağlama ve bağlamayı kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. İşlev tek bir belgeyi okur ve belgenin metin değerini güncelleştirir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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
[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {
      inputDocument.text = "This has changed.";
    }
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>SqlQuery kullanarak sıra tetikleyicisi, birden çok doc alın

Aşağıdaki örnekte, *bir function.json* dosyasında azure cosmos DB giriş bağlama ve bağlamayı kullanan bir [C# komut dosyası işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, sorgu parametrelerini özelleştirmek için bir sıra tetikleyicisi kullanarak, bir SQL sorgusu tarafından belirtilen birden çok belgeyi alır.

Sıra tetikleyicisi bir `departmentId`parametre sağlar. Bir sıra `{ "departmentId" : "Finance" }` iletisi finans departmanı için tüm kayıtları döndürecek.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

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

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyici, sorgu dizesinden kimlik aramak

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# komut dosyası işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

Burada *function.json* dosyası:

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

İşte C# komut dosyası kodu:

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

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleme, rota verilerinden kimlik arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# komut dosyası işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

Burada *function.json* dosyası:

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

İşte C# komut dosyası kodu:

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

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP tetikleme, SqlQuery kullanarak, birden fazla doc almak

Aşağıdaki örnekte, belgelerin listesini alan bir [C# komut dosyası işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev bir HTTP isteği tarafından tetiklenir. Sorgu öznitelik özelliğinde `SqlQuery` belirtilir.

Burada *function.json* dosyası:

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

İşte C# komut dosyası kodu:

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

### <a name="http-trigger-get-multiple-docs-using-documentclient"></a>DOCUMENTClient'ı kullanarak HTTP tetikleyici, birden çok doküman alın

Aşağıdaki örnekte, belgelerin listesini alan bir [C# komut dosyası işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev bir HTTP isteği tarafından tetiklenir. Kod, belgelerin `DocumentClient` listesini okumak için Azure Cosmos DB bağlama tarafından sağlanan bir örneği kullanır. Örnek `DocumentClient` yazma işlemleri için de kullanılabilir.

Burada *function.json* dosyası:

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

İşte C# komut dosyası kodu:

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

# <a name="javascript"></a>[Javascript](#tab/javascript)

Bu bölümde, çeşitli kaynaklardan bir kimlik değeri belirterek tek bir belgeyi okuyan aşağıdaki örnekler yer almaktadır:

* [Kuyruk tetikleyicisi, JSON'dan kimlik ara](#queue-trigger-look-up-id-from-json-javascript)
* [HTTP tetikleyici, sorgu dizesinden kimlik aramak](#http-trigger-look-up-id-from-query-string-javascript)
* [HTTP tetikleme, rota verilerinden kimlik arama](#http-trigger-look-up-id-from-route-data-javascript)
* [SqlQuery kullanarak sıra tetikleyicisi, birden çok doc alın](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

<a id="queue-trigger-look-up-id-from-json-javascript"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON'dan kimlik ara

Aşağıdaki örnek, *bir function.json* dosyasında cosmos DB giriş bağlama ve bağlama yı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev tek bir belgeyi okur ve belgenin metin değerini güncelleştirir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

<a id="http-trigger-look-up-id-from-query-string-javascript"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyici, sorgu dizesinden kimlik aramak

Aşağıdaki örnekte, tek bir belgeyi alan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

Burada *function.json* dosyası:

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

İşte JavaScript kodu:

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

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleme, rota verilerinden kimlik arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

Burada *function.json* dosyası:

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

İşte JavaScript kodu:

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

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>SqlQuery kullanarak sıra tetikleyicisi, birden çok doc alın

Aşağıdaki örnekte, *bir function.json* dosyasında azure cosmos DB giriş bağlama ve bağlamayı kullanan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, sorgu parametrelerini özelleştirmek için bir sıra tetikleyicisi kullanarak, bir SQL sorgusu tarafından belirtilen birden çok belgeyi alır.

Sıra tetikleyicisi bir `departmentId`parametre sağlar. Bir sıra `{ "departmentId" : "Finance" }` iletisi finans departmanı için tüm kayıtları döndürecek.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

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

Bu bölümde, çeşitli kaynaklardan bir kimlik değeri belirterek tek bir belgeyi okuyan aşağıdaki örnekler yer almaktadır:

* [Kuyruk tetikleyicisi, JSON'dan kimlik ara](#queue-trigger-look-up-id-from-json-python)
* [HTTP tetikleyici, sorgu dizesinden kimlik aramak](#http-trigger-look-up-id-from-query-string-python)
* [HTTP tetikleme, rota verilerinden kimlik arama](#http-trigger-look-up-id-from-route-data-python)
* [SqlQuery kullanarak sıra tetikleyicisi, birden çok doc alın](#queue-trigger-get-multiple-docs-using-sqlquery-python)

<a id="queue-trigger-look-up-id-from-json-python"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON'dan kimlik ara

Aşağıdaki örnek, *bir function.json* dosyasında cosmos DB giriş bağlama ve bağlama kullanan bir [Python işlevi](functions-reference-python.md) gösterir. İşlev tek bir belgeyi okur ve belgenin metin değerini güncelleştirir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

Python kodu aşağıdavelvere vermiştir:

```python
import azure.functions as func


def main(queuemsg: func.QueueMessage, documents: func.DocumentList) -> func.Document:
    if documents:
        document = documents[0]
        document['text'] = 'This was updated!'
        return document
```

<a id="http-trigger-look-up-id-from-query-string-python"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyici, sorgu dizesinden kimlik aramak

Aşağıdaki örnekte, tek bir belgeyi alan bir [Python işlevi](functions-reference-python.md) gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

Burada *function.json* dosyası:

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

Python kodu aşağıdavelvere vermiştir:

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

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleme, rota verilerinden kimlik arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [Python işlevi](functions-reference-python.md) gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri `ToDoItem` belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

Burada *function.json* dosyası:

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

Python kodu aşağıdavelvere vermiştir:

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

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>SqlQuery kullanarak sıra tetikleyicisi, birden çok doc alın

Aşağıdaki örnekte, *bir function.json* dosyasında azure cosmos DB giriş bağlama ve bağlamayı kullanan bir [Python işlevi](functions-reference-python.md) gösterilmektedir. İşlev, sorgu parametrelerini özelleştirmek için bir sıra tetikleyicisi kullanarak, bir SQL sorgusu tarafından belirtilen birden çok belgeyi alır.

Sıra tetikleyicisi bir `departmentId`parametre sağlar. Bir sıra `{ "departmentId" : "Finance" }` iletisi finans departmanı için tüm kayıtları döndürecek.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

Python kodu aşağıdavelvere vermiştir:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```

# <a name="java"></a>[Java](#tab/java)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* [HTTP tetikleyici, sorgu dizesinden kimlik aramak - String parametresi](#http-trigger-look-up-id-from-query-string---string-parameter-java)
* [HTTP tetikleme, sorgu dizesinden kimlik aramak - POJO parametresi](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)
* [HTTP tetikleme, rota verilerinden kimlik arama](#http-trigger-look-up-id-from-route-data-java)
* [HTTP tetikleme, SqlQuery kullanarak, rota verilerinden kimlik aramak](#http-trigger-look-up-id-from-route-data-using-sqlquery-java)
* [HTTP tetikleyici, SqlQuery kullanarak, rota verilerinden birden fazla doc almak](#http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java)

Örnekler basit `ToDoItem` bir türe başvurur:

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

### <a name="http-trigger-look-up-id-from-query-string---string-parameter"></a>HTTP tetikleyici, sorgu dizesinden kimlik aramak - String parametresi

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri, string formunda belirtilen veritabanı ve koleksiyondan bir belge almak için kullanılır.

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

Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri Cosmos DB'den gelecek olan işlev parametrelerindeki `@CosmosDBInput` ek açıklamayı kullanın.  Bu ek açıklama, yerel Java türleri, POJO'lar veya `Optional<T>`nullable değerleri kullanılarak kullanılabilir.

<a id="http-trigger-look-up-id-from-query-string---pojo-parameter-java"></a>

### <a name="http-trigger-look-up-id-from-query-string---pojo-parameter"></a>HTTP tetikleme, sorgu dizesinden kimlik aramak - POJO parametresi

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtar değerini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Belirtilen veritabanından ve koleksiyondan belge almak için kullanılan kimlik ve bölüm anahtarı değeri. Belge daha sonra daha önce oluşturulmuş ```ToDoItem``` POJO bir örneğine dönüştürülür ve işlev için bir bağımsız değişken olarak geçirilir.

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

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleme, rota verilerinden kimlik arama

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, aramak için kimlik ve bölüm anahtarı değerini belirtmek için bir rota parametresi kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik ve bölüm anahtarı değeri, bir belgeyi belirtilen veritabanından ve ```Optional<String>```koleksiyondan almak için kullanılır ve belgeyi .

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

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP tetikleme, SqlQuery kullanarak, rota verilerinden kimlik aramak

Aşağıdaki örnekte, tek bir belgeyi alan bir Java işlevi gösterilmektedir. İşlev, yukarı bakmak için kimliği belirtmek için bir rota parametresi kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik, sorgu ölçütlerine bağlı olarak birçok belge döndürülebileceğinden, belirtilen veritabanı ndan ve koleksiyondan bir belge almak için ```ToDoItem[]```kullanılır.

> [!NOTE]
> Yalnızca kimlikle sorgulamanız gerekiyorsa, [önceki örneklerde](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)olduğu gibi, daha az [istek birimi](../cosmos-db/request-units.md)tüketeceği için yukarı bakmanız önerilir. Nokta okuma işlemleri (GET), kimlik sorgusundan [daha verimlidir.](../cosmos-db/optimize-cost-queries.md)
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

### <a name="http-trigger-get-multiple-docs-from-route-data-using-sqlquery"></a>HTTP tetikleyici, SqlQuery kullanarak, rota verilerinden birden fazla doc almak

Aşağıdaki örnekte, birden çok belge alan bir Java işlevi gösterilmektedir. İşlev, ```description``` alanda aranacak dizeyi belirtmek ```desc``` için bir rota parametresi kullanan bir HTTP isteği tarafından tetiklenir. Arama terimi, belirtilen veritabanı ndan ve koleksiyondan bir belge koleksiyonunu almak, ```ToDoItem[]``` sonucu a'ya dönüştürmek ve işleve bağımsız değişken olarak aktarmak için kullanılır.

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

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu veritabanı adını ve koleksiyon adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında bilgi için [aşağıdaki yapılandırma bölümüne](#configuration)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığından](https://docs.microsoft.com/java/api/overview/azure/functions/runtime)Cosmos DB'ye yazan parametrelerle ilgili `@CosmosDBOutput` ek açıklamayı kullanın. Ek açıklama parametresi `OutputBinding<T>`türü, `T` yerel bir Java türü veya POJO olmalıdır.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `CosmosDB` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü**     | yok | Ayarlanmış `cosmosDB`olmalı.        |
|**Yön**     | yok | Ayarlanmış `in`olmalı.         |
|**Adı**     | yok | İşlevdeki belgeyi temsil eden bağlayıcı parametrenin adı.  |
|**Databasename** |**Databasename** |Belgeyi içeren veritabanı.        |
|**Toplamaadı** |**CollectionName** | Belgeyi içeren koleksiyonun adı. |
|**Kimliği**    | **Kimlik** | Alınacak belgenin kimliği. Bu özellik [bağlayıcı ifadeleri](./functions-bindings-expressions-patterns.md)destekler. Hem `id` **sqlQuery** özelliklerini ayarlamayın. İkisini de ayarlamazsanız, tüm koleksiyon alınır. |
|**Sqlquery**  |**Sqlquery**  | Birden çok belge almak için kullanılan bir Azure Cosmos DB SQL sorgusu. Özellik, bu örnekte olduğu gibi çalışma `SELECT * FROM c where c.departmentId = {departmentId}`zamanı bağlamaları destekler: . Hem özellikleri hem `id` de `sqlQuery` özellikleri ayarlamayın. İkisini de ayarlamazsanız, tüm koleksiyon alınır.|
|**bağlantıStringSetting**     |**ConnectionStringSetting**|Azure Cosmos DB bağlantı dizenizi içeren uygulama ayarının adı. |
|**partitionKey**|**PartitionKey**|Arama için bölüm anahtar değerini belirtir. Bağlama parametreleri içerebilir. [Bölümlenmiş](../cosmos-db/partition-data.md#logical-partitions) koleksiyonlarda arama yapmak için gereklidir.|
|**tercihYerleri**| **Tercih Edilen Yerler**| (İsteğe bağlı) Azure Cosmos DB hizmetinde coğrafi olarak çoğaltılan veritabanı hesapları için tercih edilen konumları (bölgeleri) tanımlar. Değerler virgülle ayrılmalıdır. Örneğin, "Doğu ABD,Güney Orta ABD, Kuzey Avrupa". |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

İşlev başarılı bir şekilde çıktığında, adlandırılmış giriş parametreleri aracılığıyla giriş belgesinde yapılan değişiklikler otomatik olarak kalıcıhale getirilir.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

İşlev başarılı bir şekilde çıktığında, adlandırılmış giriş parametreleri aracılığıyla giriş belgesinde yapılan değişiklikler otomatik olarak kalıcıhale getirilir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Güncelleştirmeler işlev çıkışında otomatik olarak yapılmaz. Bunun yerine, kullanmak `context.bindings.<documentName>In` ve `context.bindings.<documentName>Out` güncellemeleri yapmak. JavaScript örneğine bakın.

# <a name="python"></a>[Python](#tab/python)

Veriler bir `DocumentList` parametre aracılığıyla işleve verilir. Belgede yapılan değişiklikler otomatik olarak kalıcı değildir.

# <a name="java"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığından,](https://docs.microsoft.com/java/api/overview/azure/functions/runtime) [@CosmosDBInput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.cosmosdbinput) ek açıklama Cosmos DB verilerini işleve getirir. Bu ek açıklama, yerel Java türleri, POJO'lar veya `Optional<T>`nullable değerleri kullanılarak kullanılabilir.

---

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB belgesi oluşturulduğunda veya değiştirildiğinde bir işlev çalıştırma (Tetikleyici)](./functions-bindings-cosmosdb-v2-trigger.md)
- [Azure Cosmos DB belgesinde değişiklikleri kaydetme (Çıktı bağlama)](./functions-bindings-cosmosdb-v2-output.md)