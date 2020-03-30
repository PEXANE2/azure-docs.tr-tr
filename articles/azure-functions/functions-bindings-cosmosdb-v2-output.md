---
title: Fonksiyonlar için Azure Cosmos DB çıktı bağlama 2.x
description: Azure İşlevlerinde Azure Cosmos DB çıktı bağlamayı kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.openlocfilehash: 636903c20e07f11a2fd919654cfaa62037171f20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277771"
---
# <a name="azure-cosmos-db-output-binding-for-azure-functions-2x"></a>Azure İşlevler 2.x için Azure Cosmos DB çıktı bağlama

Azure Cosmos DB çıktı bağlama, SQL API'yi kullanarak Azure Cosmos DB veritabanına yeni bir belge yazmanızı sağlar.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-cosmosdb-v2.md)bakın.

<a id="example" name="example"></a>

# <a name="c"></a>[C #](#tab/csharp)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* [Sıra tetikleyici, bir doc yazmak](#queue-trigger-write-one-doc-c)
* [Kuyruk tetikleyicisi, IAsyncCollector kullanarak dokümanlar yazma](#queue-trigger-write-docs-using-iasynccollector-c)

Örnekler basit `ToDoItem` bir türe başvurur:

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

### <a name="queue-trigger-write-one-doc"></a>Sıra tetikleyici, bir doc yazmak

Aşağıdaki örnekte, Sıra depolama iletisinde sağlanan verileri kullanarak veritabanına belge ekleyen bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

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

### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi, IAsyncCollector kullanarak dokümanlar yazma

Aşağıdaki örnekte, json sıra iletisinde sağlanan verileri kullanarak bir veritabanına belge koleksiyonu ekleyen bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* [Sıra tetikleyici, bir doc yazmak](#queue-trigger-write-one-doc-c-script)
* [Kuyruk tetikleyicisi, IAsyncCollector kullanarak dokümanlar yazma](#queue-trigger-write-docs-using-iasynccollector-c-script)


<a id="queue-trigger-write-one-doc-c-script"></a>

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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

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

### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi, IAsyncCollector kullanarak dokümanlar yazma

Birden çok belge oluşturmak için, `ICollector<T>` `IAsyncCollector<T>` desteklenen `T` türlerden birine veya nerede bağlanabilirsiniz.

Bu örnek basit `ToDoItem` bir türe başvurur:

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

Burada function.json dosyası:

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

İşte C# komut dosyası kodu:

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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[Yapılandırma](#configuration) bölümü bu özellikleri açıklar.

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

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir işlevin çıktısı olarak bir Belgenin Azure CosmosDB veritabanına nasıl yazılalış yapılacağını gösterir.

Bağlama *tanımı, işlev.json'da* *tanımlanır* `cosmosDB`ve bu tanım .

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

Veritabanına yazmak için, bir belge `set` nesnesini veritabanı parametre yöntemine geçirin.

```python
import azure.functions as func

def main(req: func.HttpRequest, doc: func.Out[func.Document]) -> func.HttpResponse:

    request_body = req.get_body()

    doc.set(func.Document.from_json(request_body))

    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

* [Sıra tetikleyicisi, return value ile iletiyi veritabanına kaydetme](#queue-trigger-save-message-to-database-via-return-value-java)
* [HTTP tetikleyici, iade değeri üzerinden veritabanına bir belge kaydedin](#http-trigger-save-one-document-to-database-via-return-value-java)
* [HTTP tetikleme, OutputBinding üzerinden veritabanına bir belge kaydedin](#http-trigger-save-one-document-to-database-via-outputbinding-java)
* [HTTP tetikleme, OutputBinding üzerinden veritabanına birden fazla belge kaydedin](#http-trigger-save-multiple-documents-to-database-via-outputbinding-java)


<a id="queue-trigger-save-message-to-database-via-return-value-java"></a>

### <a name="queue-trigger-save-message-to-database-via-return-value"></a>Sıra tetikleyicisi, return value ile iletiyi veritabanına kaydetme

Aşağıdaki örnekte, Sıra depolama alanında bir ileti verileri ile bir veritabanına bir belge ekleyen bir Java işlevi gösterilmektedir.

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

#### <a name="http-trigger-save-one-document-to-database-via-return-value"></a>HTTP tetikleyici, iade değeri üzerinden veritabanına bir belge kaydedin

Aşağıdaki örnekte, imzası ek açıklamalı ```@CosmosDBOutput``` olan ve türünde ```String```iade değeri olan bir Java işlevi gösterilmektedir. İşlev tarafından döndürülen JSON belgesi otomatik olarak ilgili CosmosDB koleksiyonuna yazılır.

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

### <a name="http-trigger-save-one-document-to-database-via-outputbinding"></a>HTTP tetikleme, OutputBinding üzerinden veritabanına bir belge kaydedin

Aşağıdaki örnekte, bir çıktı parametresi aracılığıyla CosmosDB'ye belge yazan bir ```OutputBinding<T>``` Java işlevi gösterilmektedir. Bu örnekte, ```outputItem``` parametrenin işlev imzasıyla ```@CosmosDBOutput```değil, açıklamayla ek olarak değiştirilmesi gerekir. Kullanma, ```OutputBinding<T>``` belgeyi CosmosDB'ye yazmak için bağlamadan yararlanırken json veya XML belgesi gibi işlev arayana farklı bir değer döndürmeye de olanak tanır.

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

### <a name="http-trigger-save-multiple-documents-to-database-via-outputbinding"></a>HTTP tetikleme, OutputBinding üzerinden veritabanına birden fazla belge kaydedin

Aşağıdaki örnekte, bir çıktı parametresi aracılığıyla CosmosDB'ye birden çok belge yazan bir ```OutputBinding<T>``` Java işlevi gösterilmektedir. Bu örnekte, ```outputItem``` parametre işlev imzası ```@CosmosDBOutput```ile değil, açıklamalı. Çıktı parametresi, ```outputItem``` şablon ```ToDoItem``` parametre türü olarak nesnelerin bir listesi vardır. Kullanma, ```OutputBinding<T>``` belgeyi CosmosDB'ye yazmak için bağlamadan yararlanırken json veya XML belgesi gibi işlev arayana farklı bir değer döndürmeye de olanak tanır.

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

Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)Cosmos DB'ye yazılacak parametrelerle ilgili açıklamayı kullanın. `@CosmosDBOutput`  Ek açıklama parametresi ```OutputBinding<T>```türü, T'nin yerel bir Java türü veya POJO olduğu olmalıdır.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarda](functions-dotnet-class-library.md) [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) özniteliğini kullanın.

Özniteliğin oluşturucusu veritabanı adını ve koleksiyon adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında bilgi için [Çıktı - yapılandırmaya](#configuration)bakın. Yöntem imzasında `CosmosDB` bir öznitelik örneği aşağıda verilmiştir:

```csharp
    [FunctionName("QueueToDocDB")]
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Ek `CosmosDBOutput` açıklama Cosmos DB veri yazmak için kullanılabilir. Ek açıklamayı işleve veya tek bir işlev parametresine uygulayabilirsiniz. İşlev yönteminde kullanıldığında, işlevin geri dönüş değeri Cosmos DB'ye yazılandır. Ek açıklamayı parametreyle kullanırsanız, parametrenin türü yerel `OutputBinding<T>` `T` bir Java türü veya POJO olarak bildirilmelidir.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `CosmosDB` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü**     | yok | Ayarlanmış `cosmosDB`olmalı.        |
|**Yön**     | yok | Ayarlanmış `out`olmalı.         |
|**Adı**     | yok | İşlevdeki belgeyi temsil eden bağlayıcı parametrenin adı.  |
|**Databasename** | **Databasename**|Belgenin oluşturulduğu koleksiyonu içeren veritabanı.     |
|**Toplamaadı** |**CollectionName**  | Belgenin oluşturulduğu koleksiyonun adı. |
|**createIfNotExists**  |**CreateifNotExists**    | Koleksiyonun var olmadığında oluşturulup oluşturulmadığını belirtmek için bir boolean değeri. Yeni koleksiyonlar, maliyet etkileri olan ayrılmış iş ile oluşturulduğundan varsayılan *değer yanlıştır.* Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).  |
|**partitionKey**|**PartitionKey** |Doğru `CreateIfNotExists` olduğunda, oluşturulan koleksiyon için bölüm anahtar yolunu tanımlar.|
|**toplamaThroughput**|**KoleksiyonThroughput**| Doğru `CreateIfNotExists` olduğunda, oluşturulan koleksiyonun [iş kısmını](../cosmos-db/set-throughput.md) tanımlar.|
|**bağlantıStringSetting**    |**ConnectionStringSetting** |Azure Cosmos DB bağlantı dizenizi içeren uygulama ayarının adı.        |
|**tercihYerleri**| **Tercih Edilen Yerler**| (İsteğe bağlı) Azure Cosmos DB hizmetinde coğrafi olarak çoğaltılan veritabanı hesapları için tercih edilen konumları (bölgeleri) tanımlar. Değerler virgülle ayrılmalıdır. Örneğin, "Doğu ABD,Güney Orta ABD, Kuzey Avrupa". |
|**kullanımıMultipleWriteLocations**| **MultipleWriteLocations kullanma**| (İsteğe bağlı) Azure `true` `PreferredLocations`Cosmos DB hizmetinde çok [bölgeli yazıyazmakiçin](../cosmos-db/how-to-manage-database-account.md#configure-multiple-write-regions) ayarlandığında, bu programdan yararlanılabilir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Varsayılan olarak, işlevinizdeki çıktı parametresine yazdığınızda veritabanınızda bir belge oluşturulur. Bu belge, belge kimliği olarak otomatik olarak oluşturulan bir GUID'e sahiptir. Çıktı parametresine geçen JSON nesnesindeki `id` özelliği belirterek çıktı belgesinin belge kimliğini belirtebilirsiniz.

> [!Note]
> Varolan bir belgenin kimliğini belirttiğiniz zaman, yeni çıktı belgesi tarafından üzerine yazılır.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve iade kodları

| Bağlama | Başvuru |
|---|---|
| CosmosDB | [CosmosDB Hata Kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

<a name="host-json"></a>

## <a name="hostjson-settings"></a>host.json ayarları

Bu bölümde, sürüm 2.x'te bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Sürüm 2.x'teki genel yapılandırma ayarları hakkında daha fazla bilgi için [Azure İşlevleri sürüm 2.x için host.json başvurusuna](functions-host-json.md)bakın.

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
|Ağ Geçidi Modu|Ağ geçidi|Azure Cosmos DB hizmetine bağlanırken işlev tarafından kullanılan bağlantı modu. Seçenekler `Direct` ve`Gateway`|
|Protokol|Https://tr.,|Azure Cosmos DB hizmetine bağlantı kurarken işlev tarafından kullanılan bağlantı protokolü.  [Her iki modun bir açıklama için burayı](../cosmos-db/performance-tips.md#networking) okuyun|
|kiralamaÖnek|yok|Bir uygulamadaki tüm işlevlerde kullanılacak önkekleri kiralayın.|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB belgesi oluşturulduğunda veya değiştirildiğinde bir işlev çalıştırma (Tetikleyici)](./functions-bindings-cosmosdb-v2-trigger.md)
- [Azure Cosmos DB belgesini okuma (Giriş bağlama)](./functions-bindings-cosmosdb-v2-input.md)