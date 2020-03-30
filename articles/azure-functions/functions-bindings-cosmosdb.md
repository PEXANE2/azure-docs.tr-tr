---
title: Fonksiyonlar için Azure Cosmos DB bağlamaları 1.x
description: Azure İşlevlerinde Azure Cosmos DB tetikleyicilerinin ve bağlamalarının nasıl kullanılacağını öğrenin.
author: craigshoemaker
ms.author: cshoe
ms.topic: reference
ms.date: 11/21/2017
ms.custom: seodec18
ms.openlocfilehash: e30b256d9fa43402c3b2c444aa1a0e0dc16cfdcf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277550"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-1x"></a>Azure İşlevler için Azure Cosmos DB bağlamaları 1.x

> [!div class="op_single_selector" title1="Kullandığınız Azure İşlevleri çalışma zamanı sürümünü seçin: "]
> * [Sürüm 1](functions-bindings-cosmosdb.md)
> * [Sürüm 2](functions-bindings-cosmosdb-v2.md)

Bu makalede, Azure İşlevlerinde [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md) bağlamalarıyla nasıl çalışılalı açıklanmaktadır. Azure Fonksiyonları, Azure Cosmos DB için tetikleyici, giriş ve çıktı bağlamaları destekler.

> [!NOTE]
> Bu makale Azure İşlevler 1.x içindir. Bu bağlamaların 2.x ve üzeri işlevlerde nasıl kullanılacağı hakkında bilgi için Azure [İşlevler 2.x için Azure Cosmos DB bağlamalarına](functions-bindings-cosmosdb-v2.md)bakın.
>
>Bu bağlama nın adı documentDB idi. Fonksiyonlar sürüm 1.x'te, yalnızca tetikleyicicosmos DB olarak yeniden adlandırıldı; giriş bağlama, çıktı bağlama ve NuGet paketi DocumentDB adını korur.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Azure Cosmos DB bağlamaları yalnızca SQL API ile kullanım için desteklenir. Diğer tüm Azure Cosmos DB API'leri için, [Azure Cosmos DB'nin MongoDB,](../cosmos-db/mongodb-introduction.md) [Cassandra API,](../cosmos-db/cassandra-introduction.md) [Gremlin API](../cosmos-db/graph-introduction.md)ve [Tablo API'si](../cosmos-db/table-introduction.md)dahil olmak üzere API'niz için statik istemciyi kullanarak işlevinizden veritabanına erişmelisiniz.

## <a name="packages---functions-1x"></a>Paketler - Fonksiyonlar 1.x

[Microsoft.Azure.WebJobs.Extensions.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB) NuGet paketi, sürüm 1.x'te, Işlevler sürüm 1.x için Azure Cosmos DB bağlamaları sağlanır. Bağlamaların kaynak kodu [azure-webjobs-sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.DocumentDB) GitHub deposundadır.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="trigger"></a>Tetikleyici

Azure Cosmos DB Trigger, ekler ve bölümler arasında güncelleştirmeleri dinlemek için [Azure Cosmos DB Change Feed'i](../cosmos-db/change-feed.md) kullanır. Değişiklik akışı ekler ve güncelleştirmeler yayımlar, silme değil.

## <a name="trigger---example"></a>Tetikleyici - örnek

# <a name="c"></a>[C #](#tab/csharp)

Aşağıdaki örnekte, belirtilen veritabanı ve koleksiyonda ekler veya güncelleştirmeler olduğunda çağrılan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV1
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
            TraceWriter log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.Info($"Documents modified: {documents.Count}");
                log.Info($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, bir *function.json* dosyasında cosmos DB tetikleyici bağlama ve bağlama kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir. Cosmos DB kayıtları değiştirildiğinde işlev günlük iletileri yazar.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

İşte C# komut dosyası kodu:

```cs
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
        log.Info("Documents modified " + documents.Count);
        log.Info("First document Id " + documents[0].Id);
    }
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *function.json* dosyasında cosmos DB tetikleyici bağlama ve bağlama kullanan bir [JavaScript işlevi](functions-reference-node.md) gösterir. Cosmos DB kayıtları değiştirildiğinde işlev günlük iletileri yazar.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

İşte JavaScript kodu:

```javascript
    module.exports = function (context, documents) {
        context.log('First document Id modified : ', documents[0].id);

        context.done();
    }
```

---

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikler

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu veritabanı adını ve koleksiyon adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında bilgi için [Bkz.](#trigger---configuration) Yöntem imzasında `CosmosDBTrigger` bir öznitelik örneği aşağıda verilmiştir:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
        ...
    }
```

Tam bir örnek için bkz: [Tetikleyici - C# örneği.](#trigger)

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

---

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `CosmosDBTrigger` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `cosmosDBTrigger`olmalı. |
|**Yön** | yok | Ayarlanmış `in`olmalı. Bu parametre, Azure portalında tetikleyiciyi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | Değişikliklerle birlikte belgelerin listesini temsil eden işlev kodunda kullanılan değişken adı. |
|**bağlantıStringSetting**|**ConnectionStringSetting** | İzlenen Azure Cosmos DB hesabına bağlanmak için kullanılan bağlantı dizesini içeren bir uygulama ayarının adı. |
|**Databasename**|**Databasename**  | Azure Cosmos DB veritabanının adı ve koleksiyon izleniyor. |
|**Toplamaadı** |**CollectionName** | İzlenen koleksiyonun adı. |
|**kiralamaConnectionStringSetting** | **KiralamaBağlantıStringSetting** | (İsteğe bağlı) Kira koleksiyonunu tutan hizmete bağlantı dizesini içeren bir uygulama ayarının adı. Ayarlanmadığında, `connectionStringSetting` değer kullanılır. Bağlama portalda oluşturulduğunda bu parametre otomatik olarak ayarlanır. Kiratoplama bağlantısı dizesi yazma izinleri olmalıdır.|
|**leaseDatabaseName** |**KiralamaVeritabanı Adı** | (İsteğe bağlı) Kiralamaları depolamak için kullanılan koleksiyonu tutan veritabanının adı. Ayarlanmadığında, `databaseName` ayarın değeri kullanılır. Bağlama portalda oluşturulduğunda bu parametre otomatik olarak ayarlanır. |
|**leaseCollectionName** | **LeaseCollectionName** | (İsteğe bağlı) Kiralamaları depolamak için kullanılan koleksiyonun adı. Ayarlanmadığında, değer `leases` kullanılır. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (İsteğe bağlı) `true`Ayarlandığında, kiralama koleksiyonu zaten yokken otomatik olarak oluşturulur. Varsayılan değer: `false`. |
|**leasesCollectionThroughput**| **LeasesCollectionThroughput**| (İsteğe bağlı) Kirakoleksiyonu oluşturulduğunda atanacak İstek Birimleri miktarını tanımlar. Bu ayar yalnızca `createLeaseCollectionIfNotExists` . `true` Bağlama portalı kullanılarak oluşturulduğunda bu parametre otomatik olarak ayarlanır.
|**leaseCollectionPrefix**| **LeaseCollection Önek**| (İsteğe bağlı) Ayarlandığında, bu İşlev için Kiralama koleksiyonunda oluşturulan kiralamalara bir önek ekleyerek iki ayrı Azure Işlevinin farklı önekler kullanarak aynı Kiralama koleksiyonunu paylaşmasına etkin bir şekilde olanak tanır.
|**feedPollDelay**| **FeedPollDelay**| (İsteğe bağlı) Ayarlandığında, tüm geçerli değişiklikler dolduktan sonra, akıştaki yeni değişiklikler için bir bölümü yoklama arasındaki gecikmemilisaniye cinsinden tanımlar. Varsayılan değer 5000 'dir (5 saniye).
|**leaseAcquireInterval**| **KiralamaAlma Aralığı**| (İsteğe bağlı) Ayarlandığında, milisaniye cinsinden, bölümler bilinen ana bilgisayar örnekleri arasında eşit olarak dağıtılırsa, bir görevi hesaplamak için başlatma aralığını tanımlar. Varsayılan değer 13000 (13 saniye) 'dir.
|**leaseExpirationInterval**| **LeaseExpirationInterval**| (İsteğe bağlı) Ayarlandığında, milisaniye cinsinden, bir bölümü temsil eden bir kira üzerinde kiranın alındığı aralığı tanımlar. Bu aralıkta kira yenilenmezse, süresinin dolmasına neden olur ve bölümün sahipliği başka bir örne taşınır. Varsayılan değer 60000 (60 saniye) 'dir.
|**leaseRenewInterval**| **KiralamaYenilemeInterval**| (İsteğe bağlı) Ayarlandığında, milisaniye cinsinden, şu anda bir örnek tarafından tutulan bölümleriçin tüm kiralamaların yenileme aralığını tanımlar. Varsayılan değer 17000 (17 saniye) 'dir.
|**checkpointFrekans**| **CheckpointFrekans**| (İsteğe bağlı) Ayarlandığında, milisaniye cinsinden kira denetim noktaları arasındaki aralığı tanımlar. Varsayılan her zaman her İşlev çağrısından sonradır.
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (İsteğe bağlı) Ayarlandığında, İşlev çağrısı başına alınan maksimum öğe miktarını özelleştirer.
|**başlangıçBaşlangıçBaşlangıç**| **Başlangıç Başlangıç**| (İsteğe bağlı) Ayarlandığında, Tetikleyici'ye geçerli saat yerine koleksiyon tarihinin başından itibaren değişiklikleri okumaya başlamasını söyler. Bu yalnızca tetikleyici ilk kez başlatılır, sonraki çalıştırmalarda olduğu gibi, denetim noktaları zaten depolanır. Bunu zaten `true` oluşturulmuş kiralamalar olduğunda ayarlamak hiçbir etki yaratmaz.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

Tetikleyici, bölümleri üzerinde _kiradepolamak_ için kullandığı ikinci bir koleksiyon gerektirir. Tetikleyicinin çalışması için hem denetimli koleksiyon hem de kiralamaları içeren koleksiyon kullanılabilir olmalıdır.

>[!IMPORTANT]
> Aynı koleksiyon için cosmos DB tetikleyicisi kullanacak şekilde yapılandırılan birden çok işlev varsa, işlevlerin her biri özel bir kira koleksiyonu kullanmalı veya her işlev için farklı `LeaseCollectionPrefix` bir işlev belirtmelidir. Aksi takdirde, yalnızca bir işlev tetiklenir. Önek hakkında bilgi için [Yapılandırma bölümüne](#trigger---configuration)bakın.

Tetikleyici, belgenin güncelleştirilip güncelleştirilip eklenmediğini göstermez, yalnızca belgenin kendisini sağlar. Güncelleştirmeleri ve ekleri farklı şekilde işlemeniz gerekiyorsa, ekleme veya güncelleştirme için zaman damgası alanlarını uygulayarak bunu yapabilirsiniz.

## <a name="input"></a>Giriş

Azure Cosmos DB giriş bağlaması SQL API'sini kullanarak bir veya daha fazla Azure Cosmos DB belgesini getirir ve işlevin giriş parametresine iletir. Belge kimliği veya sorgu parametreleri, işlevi çağıran tetikleyiciye göre belirlenebilir.

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
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

<a id="queue-trigger-look-up-id-from-json-c"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>Kuyruk tetikleyicisi, JSON'dan kimlik ara

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, JSON nesnesi içeren bir sıra iletisi tarafından tetiklenir. Sıra tetikleyicisi JSON'u, `ToDoItemLookup`yukarı bakmak için kimliği içeren adlı bir nesneye ayrışır. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

<a id="http-trigger-look-up-id-from-query-string-c"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP tetikleyici, sorgu dizesinden kimlik aramak

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, bir sorgu dizesi kullanarak aranacak kimliği belirten bir HTTP isteği tarafından tetiklenir. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

<a id="http-trigger-look-up-id-from-route-data-c"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleme, rota verilerinden kimlik arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, aramak için kimliği belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static HttpResponseMessage Run(
            [HttpTrigger(
                AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Giriş örneklerini atla](#input---attributes)

<a id="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP tetikleme, SqlQuery kullanarak, rota verilerinden kimlik aramak

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev, aramak için kimliği belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")] IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Giriş örneklerini atla](#input---attributes)

<a id="http-trigger-get-multiple-docs-using-sqlquery-c"></a>

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP tetikleme, SqlQuery kullanarak, birden fazla doc almak

Aşağıdaki örnekte, belgelerin listesini alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev bir HTTP isteği tarafından tetiklenir. Sorgu öznitelik özelliğinde `SqlQuery` belirtilir.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Giriş örneklerini atla](#input---attributes)

<a id="http-trigger-get-multiple-docs-using-documentclient-c"></a>

### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>DOCUMENTClient (C#) kullanarak HTTP tetikleyici, birden çok doc alın

Aşağıdaki örnekte, belgelerin listesini alan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. İşlev bir HTTP isteği tarafından tetiklenir. Kod, belgelerin `DocumentClient` listesini okumak için Azure Cosmos DB bağlama tarafından sağlanan bir örneği kullanır. Örnek `DocumentClient` yazma işlemleri için de kullanılabilir.

```cs
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
            string searchterm = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
                .Value;

            if (searchterm == null)
            {
                return req.CreateResponse(HttpStatusCode.NotFound);
            }

            log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.Info(result.Description);
                }
            }
            return req.CreateResponse(HttpStatusCode.OK);
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
namespace CosmosDBSamplesV1
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
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "in"
}
```

[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

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
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

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

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# komut dosyası işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, bir sorgu dizesi kullanarak aranacak kimliği belirten bir HTTP isteği tarafından tetiklenir. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

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
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true
}
```

İşte C# komut dosyası kodu:

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
        log.Info($"ToDo item not found");
    }
    else
    {
        log.Info($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-look-up-id-from-route-data-c-script"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP tetikleme, rota verilerinden kimlik arama

Aşağıdaki örnekte, tek bir belgeyi alan bir [C# komut dosyası işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, aramak için kimliği belirtmek için rota verilerini kullanan bir HTTP isteği tarafından tetiklenir. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

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
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

İşte C# komut dosyası kodu:

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
        log.Info($"ToDo item not found");
    }
    else
    {
        log.Info($"Found ToDo item, Description={toDoItem.Description}");
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
      "type": "documentDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
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

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.Info(toDoItem.Description);
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
      "type": "documentDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
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

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.Info(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Bu bölümde aşağıdaki örnekler yer almaktadır:

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
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

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

Aşağıdaki örnekte, tek bir belgeyi alan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, bir sorgu dizesi kullanarak aranacak kimliği belirten bir HTTP isteği tarafından tetiklenir. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

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
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true
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

Aşağıdaki örnekte, tek bir belgeyi alan bir [JavaScript işlevi](functions-reference-node.md) gösterilmektedir. İşlev, bir sorgu dizesi kullanarak aranacak kimliği belirten bir HTTP isteği tarafından tetiklenir. Bu kimlik, belirtilen `ToDoItem` veritabanından ve koleksiyondan bir belge almak için kullanılır.

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
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
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
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

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

---

## <a name="input---attributes"></a>Giriş - öznitelikler

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu veritabanı adını ve koleksiyon adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında bilgi için [aşağıdaki yapılandırma bölümüne](#input---configuration)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

---

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `DocumentDB` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü**     | yok | Ayarlanmış `documentdb`olmalı.        |
|**Yön**     | yok | Ayarlanmış `in`olmalı.         |
|**Adı**     | yok | İşlevdeki belgeyi temsil eden bağlayıcı parametrenin adı.  |
|**Databasename** |**Databasename** |Belgeyi içeren veritabanı.        |
|**Toplamaadı** |**CollectionName** | Belgeyi içeren koleksiyonun adı. |
|**Kimliği**    | **Kimlik** | Alınacak belgenin kimliği. Bu özellik [bağlayıcı ifadeleri](./functions-bindings-expressions-patterns.md)destekler. **Hem id** hem de **sqlQuery** özelliklerini ayarlamayın. İkisini de ayarlamazsanız, tüm koleksiyon alınır. |
|**Sqlquery**  |**Sqlquery**  | Birden çok belge almak için kullanılan bir Azure Cosmos DB SQL sorgusu. Özellik, bu örnekte olduğu gibi çalışma `SELECT * FROM c where c.departmentId = {departmentId}`zamanı bağlamaları destekler: . **Hem id** hem de **sqlQuery** özelliklerini ayarlamayın. İkisini de ayarlamazsanız, tüm koleksiyon alınır.|
|**bağlantı**     |**ConnectionStringSetting**|Azure Cosmos DB bağlantı dizenizi içeren uygulama ayarının adı.        |
|**partitionKey**|**PartitionKey**|Arama için bölüm anahtar değerini belirtir. Bağlama parametreleri içerebilir.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

# <a name="c"></a>[C #](#tab/csharp)

İşlev başarılı bir şekilde çıktığında, adlandırılmış giriş parametreleri aracılığıyla giriş belgesinde yapılan değişiklikler otomatik olarak kalıcıhale getirilir.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

İşlev başarılı bir şekilde çıktığında, adlandırılmış giriş parametreleri aracılığıyla giriş belgesinde yapılan değişiklikler otomatik olarak kalıcıhale getirilir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Güncelleştirmeler işlev çıkışında otomatik olarak yapılmaz. Bunun yerine, kullanmak `context.bindings.<documentName>In` ve `context.bindings.<documentName>Out` güncellemeleri yapmak. Giriş [örneğine](#input)bakın.

---

## <a name="output"></a>Çıktı

Azure Cosmos DB çıktı bağlama, SQL API'yi kullanarak Azure Cosmos DB veritabanına yeni bir belge yazmanızı sağlar.

# <a name="c"></a>[C #](#tab/csharp)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* Sıra tetikleyici, bir doc yazmak
* Sıra tetikleyici, kullanarak dokümanlar yazma`IAsyncCollector`

Örnekler basit `ToDoItem` bir türe başvurur:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

### <a name="queue-trigger-write-one-doc"></a>Sıra tetikleyici, bir doc yazmak

Aşağıdaki örnekte, Sıra depolama iletisinde sağlanan verileri kullanarak veritabanına belge ekleyen bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV1
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            TraceWriter log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.Info($"C# Queue trigger function inserted one row");
            log.Info($"Description={queueMessage}");
        }
    }
}
```

### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi, IAsyncCollector kullanarak dokümanlar yazma

Aşağıdaki örnekte, json sıra iletisinde sağlanan verileri kullanarak bir veritabanına belge koleksiyonu ekleyen bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.Info($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* Sıra tetikleyici, bir doc yazmak
* Sıra tetikleyici, kullanarak dokümanlar yazma`IAsyncCollector`

### <a name="queue-trigger-write-one-doc"></a>Sıra tetikleyici, bir doc yazmak

Aşağıdaki örnekte, *bir function.json* dosyasında azure cosmos DB çıktısı bağlama ve bağlamayı kullanan bir [C# komut dosyası işlevi](functions-reference-csharp.md) gösterilmektedir. İşlev, aşağıdaki biçimde JSON alan bir kuyruğa bir sıra girişi bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlev, her kayıt için aşağıdaki biçimde Azure Cosmos DB belgeleri oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
    "name": "employeeDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#output---configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;

    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        dynamic employee = JObject.Parse(myQueueItem);

        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }
```

### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi, IAsyncCollector kullanarak dokümanlar yazma

Birden çok belge oluşturmak için, `ICollector<T>` `IAsyncCollector<T>` desteklenen `T` türlerden birine veya nerede bağlanabilirsiniz.

Bu örnek basit `ToDoItem` bir türe başvurur:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

Burada function.json dosyası:

```json
{
  "bindings": [
    {
      "name": "toDoItemsIn",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "todoqueueforwritemulti",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "documentDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

İşte C# komut dosyası kodu:

```cs
using System;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.Info($"Description={toDoItem.Description}");
        await toDoItemsOut.AddAsync(toDoItem);
    }
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *bir function.json* dosyasında bir Azure Cosmos DB çıktısı bağlamave bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev, aşağıdaki biçimde JSON alan bir kuyruğa bir sıra girişi bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlev, her kayıt için aşağıdaki biçimde Azure Cosmos DB belgeleri oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

```json
{
    "name": "employeeDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#output---configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

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

---

## <a name="output---attributes"></a>Çıktı - öznitelikler

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu veritabanı adını ve koleksiyon adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında bilgi için [Çıktı - yapılandırmaya](#output---configuration)bakın. Yöntem imzasında `DocumentDB` bir öznitelik örneği aşağıda verilmiştir:

```csharp
    [FunctionName("QueueToDocDB")]
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Tam bir örnek için [Çıktı'ya](#output)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

---

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `DocumentDB` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü**     | yok | Ayarlanmış `documentdb`olmalı.        |
|**Yön**     | yok | Ayarlanmış `out`olmalı.         |
|**Adı**     | yok | İşlevdeki belgeyi temsil eden bağlayıcı parametrenin adı.  |
|**Databasename** | **Databasename**|Belgenin oluşturulduğu koleksiyonu içeren veritabanı.     |
|**Toplamaadı** |**CollectionName**  | Belgenin oluşturulduğu koleksiyonun adı. |
|**createIfNotExists**  |**CreateifNotExists**    | Koleksiyonun var olmadığında oluşturulup oluşturulmadığını belirtmek için bir boolean değeri. Yeni koleksiyonlar, maliyet etkileri olan ayrılmış iş ile oluşturulduğundan varsayılan *değer yanlıştır.* Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**partitionKey**|**PartitionKey** |Doğru `CreateIfNotExists` olduğunda, oluşturulan koleksiyon için bölüm anahtar yolunu tanımlar.|
|**toplamaThroughput**|**KoleksiyonThroughput**| Doğru `CreateIfNotExists` olduğunda, oluşturulan koleksiyonun [iş kısmını](../cosmos-db/set-throughput.md) tanımlar.|
|**bağlantı**    |**ConnectionStringSetting** |Azure Cosmos DB bağlantı dizenizi içeren uygulama ayarının adı.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

Varsayılan olarak, işlevinizdeki çıktı parametresine yazdığınızda veritabanınızda bir belge oluşturulur. Bu belge, belge kimliği olarak otomatik olarak oluşturulan bir GUID'e sahiptir. Çıktı parametresine geçen JSON nesnesindeki `id` özelliği belirterek çıktı belgesinin belge kimliğini belirtebilirsiniz.

> [!Note]
> Varolan bir belgenin kimliğini belirttiğiniz zaman, yeni çıktı belgesi tarafından üzerine yazılır.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve iade kodları

| Bağlama | Başvuru |
|---|---|
| CosmosDB | [CosmosDB Hata Kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>Sonraki adımlar

* [Cosmos DB ile sunucusuz veritabanı hesaplama sı hakkında daha fazla bilgi edinin](../cosmos-db/serverless-computing-database.md)
* [Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
