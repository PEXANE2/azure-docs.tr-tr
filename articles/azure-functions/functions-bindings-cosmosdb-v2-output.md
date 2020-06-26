---
title: 2. x ve üzeri Işlevler için çıkış bağlamasını Azure Cosmos DB
description: Azure Işlevlerinde Azure Cosmos DB çıkış bağlamayı kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.custom: tracking-python
ms.openlocfilehash: 2228a9609b0e0325dc4e6f7ccbe88417c900b688
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85374347"
---
# <a name="azure-cosmos-db-output-binding-for-azure-functions-2x-and-higher"></a>Azure Işlevleri 2. x ve üzeri için çıkış bağlamasını Azure Cosmos DB

Azure Cosmos DB çıkış bağlaması, SQL API 'sini kullanarak bir Azure Cosmos DB veritabanına yeni bir belge yazmanıza olanak sağlar.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](./functions-bindings-cosmosdb-v2.md).

<a id="example" name="example"></a>

# <a name="c"></a>[C#](#tab/csharp)

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, bir belge yaz](#queue-trigger-write-one-doc-c)
* [Kuyruk tetikleyicisi, ıasynccollector kullanarak belge yazma](#queue-trigger-write-docs-using-iasynccollector-c)

Örnekler basit bir `ToDoItem` türe başvurur:

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

### <a name="queue-trigger-write-one-doc"></a>Kuyruk tetikleyicisi, bir belge yaz

Aşağıdaki örnek, kuyruk depolamadan ileti içinde belirtilen verileri kullanarak bir veritabanına belge ekleyen bir [C# işlevini](functions-dotnet-class-library.md) gösterir.

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

### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi, ıasynccollector kullanarak belge yazma

Aşağıdaki örnek, bir veritabanı JSON iletisinde belirtilen verileri kullanarak bir veritabanına bir belge koleksiyonu ekleyen bir [C# işlevini](functions-dotnet-class-library.md) gösterir.

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

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, bir belge yaz](#queue-trigger-write-one-doc-c-script)
* [Kuyruk tetikleyicisi, ıasynccollector kullanarak belge yazma](#queue-trigger-write-docs-using-iasynccollector-c-script)


<a id="queue-trigger-write-one-doc-c-script"></a>

### <a name="queue-trigger-write-one-doc"></a>Kuyruk tetikleyicisi, bir belge yaz

Aşağıdaki örnek, bir *function.js* dosyasında bir Azure Cosmos db çıktı bağlamayı ve bağlamayı kullanan bir [C# betik işlevini](functions-reference-csharp.md) gösterir. İşlevi, JSON 'yi aşağıdaki biçimde alan bir sıra için bir kuyruk girişi bağlaması kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlevi her kayıt için aşağıdaki biçimde Azure Cosmos DB belgeler oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodu aşağıda verilmiştir:

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

### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi, ıasynccollector kullanarak belge yazma

Birden çok belge oluşturmak için, `ICollector<T>` `IAsyncCollector<T>` `T` desteklenen türlerden biri olan veya ' ye bağlayabilirsiniz.

Bu örnek bir basit tür anlamına gelir `ToDoItem` :

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

Dosyada function.js:

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

C# betik kodu aşağıda verilmiştir:

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function.js* dosyasında bir Azure Cosmos DB çıkış bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlevi, JSON 'yi aşağıdaki biçimde alan bir sıra için bir kuyruk girişi bağlaması kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlevi her kayıt için aşağıdaki biçimde Azure Cosmos DB belgeler oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Dosyadaki *function.js* bağlama verileri aşağıda verilmiştir:

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

[Yapılandırma](#configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kodu aşağıda verilmiştir:

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

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir işlevin çıktısı olarak bir Azure CosmosDB veritabanına nasıl bir belge yazılacağını gösterir.

Bağlama tanımı, *türünün türü* olarak ayarlandığı *function.js* tanımlanmıştır `cosmosDB` .

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

Veritabanına yazmak için, veritabanı parametresinin yöntemine bir belge nesnesi geçirin `set` .

```python
import azure.functions as func

def main(req: func.HttpRequest, doc: func.Out[func.Document]) -> func.HttpResponse:

    request_body = req.get_body()

    doc.set(func.Document.from_json(request_body))

    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

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

Aşağıdaki örnek, imzasına açıklama eklenmiş olan ```@CosmosDBOutput``` ve türünde dönüş değeri olan bir Java işlevini gösterir ```String``` . İşlevin döndürdüğü JSON belgesi, ilgili CosmosDB koleksiyonuna otomatik olarak yazılır.

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

Aşağıdaki örnek, bir çıktı parametresi aracılığıyla CosmosDB 'ye bir belge yazan bir Java işlevini gösterir ```OutputBinding<T>``` . Bu örnekte, ```outputItem``` parametresinin ```@CosmosDBOutput``` işlev imzasıyla değil, ile açıklanması gerekir. Kullanmak ```OutputBinding<T>``` , işlevinizin, BIR JSON veya XML belgesi gibi, işleve farklı bir değer döndürmeye izin verirken bir belgeyi CosmosDB 'ye yazmak için bağlamadan faydalanmasını sağlar.

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

Aşağıdaki örnek, bir çıkış parametresi aracılığıyla CosmosDB 'ye birden çok belge yazan bir Java işlevini gösterir ```OutputBinding<T>``` . Bu örnekte, ```outputItem``` parametresi ```@CosmosDBOutput``` işlev imzasıyla değil, ile açıklama eklenir. Output parametresi, ```outputItem``` ```ToDoItem``` şablon parametresi türü olarak nesnelerin bir listesini içerir. Kullanmak ```OutputBinding<T>``` , işlevinizin, BIR JSON veya XML belgesi gibi, işleve farklı bir değer döndürmeye izin verirken CosmosDB 'ye belge yazmak için bağlamadan faydalanmasını sağlar.

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

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, `@CosmosDBOutput` Cosmos DB yazılacak parametrelerde ek açıklamayı kullanın.  Ek açıklama parametre türü olmalıdır ```OutputBinding<T>``` ; burada T, yerel bir Java türü ya da POJO.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C#](#tab/csharp)

[C# sınıf kitaplıklarında](functions-dotnet-class-library.md) [cosmosdb](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/dev/test/WebJobs.Extensions.CosmosDB.Tests) özniteliğini kullanın.

Özniteliğin Oluşturucusu, veritabanı adını ve koleksiyon adını alır. Yapılandırabileceğiniz bu ayarlar ve diğer özellikler hakkında daha fazla bilgi için bkz. [çıkış-yapılandırma](#configuration). `CosmosDB`Bir yöntem imzasında bir öznitelik örneği aşağıda verilmiştir:

```csharp
    [FunctionName("QueueToDocDB")]
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

`CosmosDBOutput`Ek açıklama Cosmos DB verileri yazmak için kullanılabilir. Ek açıklamayı işleve veya bağımsız bir işlev parametresine uygulayabilirsiniz. İşlev yönteminde kullanıldığında, işlevin dönüş değeri Cosmos DB için yazıldığı şeydir. Ek açıklamayı bir parametresiyle birlikte kullanırsanız, parametrenin türü `OutputBinding<T>` `T` yerel bir Java türü veya Pojo olarak bildirilmelidir.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, dosyasında ve özniteliğinde *function.js* ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır `CosmosDB` .

|function.jsözelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle**     | yok | Olarak ayarlanmalıdır `cosmosDB` .        |
|**Görünüm**     | yok | Olarak ayarlanmalıdır `out` .         |
|**ada**     | yok | İşlevdeki belgeyi temsil eden bağlama parametresinin adı.  |
|**Dosyasında** | **Dosyasında**|Belgenin oluşturulduğu koleksiyonu içeren veritabanı.     |
|**Ma** |**CollectionName**  | Belgenin oluşturulduğu koleksiyonun adı. |
|**Createıfnotexists çağırmanız**  |**Createıfnotexists çağırmanız**    | Koleksiyonun mevcut olmadığında oluşturulup oluşturulmayacağını belirten bir Boole değeri. Varsayılan değer *false* 'dur çünkü yeni koleksiyonlar, maliyet etkilerine sahip olan ayrılmış aktarım hızı ile oluşturulmuştur. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).  |
|**partitionKey**|**PartitionKey** |`CreateIfNotExists`Doğru olduğunda, oluşturulan koleksiyon için bölüm anahtarı yolunu tanımlar.|
|**Collectionverimlilik**|**Collectionverimlilik**| `CreateIfNotExists`Doğru olduğunda, oluşturulan koleksiyonun [verimini](../cosmos-db/set-throughput.md) tanımlar.|
|**connectionStringSetting**    |**ConnectionStringSetting** |Azure Cosmos DB Bağlantı dizenizi içeren uygulama ayarının adı.        |
|**preferredLocations**| **PreferredLocations**| Seçim Azure Cosmos DB hizmetindeki coğrafi olarak çoğaltılan veritabanı hesapları için tercih edilen konumları (bölgeleri) tanımlar. Değerler virgülle ayrılmalıdır. Örneğin, "Doğu ABD, Orta Güney ABD, Kuzey Avrupa". |
|**useMultipleWriteLocations**| **UseMultipleWriteLocations**| Seçim `true`İle birlikte olarak ayarlandığında `PreferredLocations` , Azure Cosmos DB hizmetindeki [çok bölgeli yazmaları](../cosmos-db/how-to-manage-database-account.md#configure-multiple-write-regions) kullanabilir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Varsayılan olarak, işlevinizdeki çıkış parametresine yazdığınızda, veritabanınızda bir belge oluşturulur. Bu belgede, belge KIMLIĞI olarak otomatik olarak oluşturulan bir GUID bulunur. Çıktı `id` parametresine GEÇIRILEN JSON nesnesindeki özelliği belirterek çıktı belgesinin belge kimliğini belirtebilirsiniz.

> [!Note]
> Mevcut bir belgenin KIMLIĞINI belirttiğinizde, yeni çıktı belgesiyle üzerine yazılır.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| CosmosDB | [CosmosDB hata kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

<a name="host-json"></a>

## <a name="hostjson-settings"></a>Ayarlar üzerinde host.js

Bu bölümde, sürüm 2. x içinde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. 2. x sürümündeki genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri sürüm 2. x için başvuruhost.js](functions-host-json.md).

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
|GatewayMode|Ağ geçidi|Azure Cosmos DB hizmetine bağlanırken işlev tarafından kullanılan bağlantı modu. Seçenekler `Direct` ve`Gateway`|
|Protokol|'Dir|Azure Cosmos DB hizmetine bağlantı sırasında işlev tarafından kullanılan bağlantı protokolü.  [Her iki modun açıklaması için buraya](../cosmos-db/performance-tips.md#networking) okuyun|
|leasePrefix|yok|Bir uygulamadaki tüm işlevler genelinde kullanılacak kira öneki.|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB bir belge oluşturulduğunda veya değiştirildiğinde (tetikleyici) bir işlev çalıştırma](./functions-bindings-cosmosdb-v2-trigger.md)
- [Azure Cosmos DB belge okuma (giriş bağlama)](./functions-bindings-cosmosdb-v2-input.md)
