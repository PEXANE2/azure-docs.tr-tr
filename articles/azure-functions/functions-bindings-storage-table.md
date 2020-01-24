---
title: Azure Işlevleri için Azure Tablo depolama bağlamaları
description: Azure Işlevleri 'nde Azure Tablo depolama bağlamalarını nasıl kullanacağınızı anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.openlocfilehash: 1be6420598e7983ef9014f617da1f87f5550fa6a
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705369"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Işlevleri için Azure Tablo depolama bağlamaları

Bu makalede Azure Işlevleri 'nde Azure Tablo depolama bağlamalarıyla nasıl çalışılacağı açıklanmaktadır. Azure Işlevleri, Azure Tablo depolaması için giriş ve çıkış bağlamalarını destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Tablo depolama bağlamaları [Microsoft. Azure. WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2. x ' de verilmiştir. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub deposu.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

Tablo depolama bağlamaları [Microsoft. Azure. WebJobs. Extensions. Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketi, sürüm 3. x içinde verilmiştir. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Girdi

Azure depolama hesabındaki bir tabloyu okumak için Azure Tablo depolama girişi bağlamasını kullanın.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

### <a name="one-entity"></a>Bir varlık

Aşağıdaki örnek, tek bir tablo satırını okuyan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir. 

"{QueueTrigger}" satır anahtarı değeri, satır anahtarının kuyruk ileti dizesinden geldiğini gösterir.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        ILogger log)
    {
        log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### <a name="iqueryable"></a>IQueryable

Aşağıdaki örnek, `MyPoco` sınıfının `TableEntity`türetildiği birden çok tablo satırını okuyan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        ILogger log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

### <a name="cloudtable"></a>CloudTable

`IQueryable`, [Functions v2 çalışma zamanında](functions-versions.md)desteklenmez. Alternatif olarak, Azure depolama SDK 'sını kullanarak tabloyu okumak için `CloudTable` yöntemi parametresi kullanılır. Aşağıda, Azure Işlevleri günlük tablosunu sorgulayan bir işleve örnek verilmiştir:

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;

namespace FunctionAppCloudTable2
{
    public class LogEntity : TableEntity
    {
        public string OriginalName { get; set; }
    }
    public static class CloudTableDemo
    {
        [FunctionName("CloudTableDemo")]
        public static async Task Run(
            [TimerTrigger("0 */1 * * * *")] TimerInfo myTimer, 
            [Table("AzureWebJobsHostLogscommon")] CloudTable cloudTable,
            ILogger log)
        {
            log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

            TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
                TableQuery.CombineFilters(
                    TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
                        "FD2"),
                    TableOperators.And,
                    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
                        "t")));

            // Execute the query and loop through the results
            foreach (LogEntity entity in 
                await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
            {
                log.LogInformation(
                    $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
            }
        }
    }
}
```

CloudTable 'ın nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Tablo Depolamayı kullanmaya başlama](../cosmos-db/table-storage-how-to-use-dotnet.md).

`CloudTable` bağlamaya ve bir hata mesajı almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

### <a name="one-entity"></a>Bir varlık

Aşağıdaki örnek, bir *function. JSON* dosyası ve [ C# ](functions-reference-csharp.md) bağlamayı kullanan betik kodu içindeki tablo girişi bağlamasını gösterir. İşlevi, tek bir tablo satırını okumak için bir kuyruk tetikleyicisi kullanır. 

*Function. JSON* dosyası bir `partitionKey` ve bir `rowKey`belirtir. "{QueueTrigger}" `rowKey` değeri, satır anahtarının kuyruk ileti dizesinden geldiğini gösterir.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```csharp
public static void Run(string myQueueItem, Person personEntity, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    log.LogInformation($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="iqueryable"></a>IQueryable

Aşağıdaki örnek, bir *function. JSON* dosyası ve [ C# ](functions-reference-csharp.md) bağlamayı kullanan betik kodu içindeki tablo girişi bağlamasını gösterir. İşlevi, bir kuyruk iletisinde belirtilen bölüm anahtarı için varlıkları okur.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# Betik kodu, varlık türünün `TableEntity`Türetibilmesi Için Azure depolama SDK 'sına bir başvuru ekler:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Extensions.Logging;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.LogInformation($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="cloudtable"></a>CloudTable

`IQueryable` [2. x ve üzeri sürümler](functions-versions.md)için işlevler çalışma zamanında desteklenmez. Alternatif olarak, Azure depolama SDK 'sını kullanarak tabloyu okumak için `CloudTable` yöntemi parametresi kullanılır. Aşağıda, Azure Işlevleri günlük tablosunu sorgulayan bir işleve örnek verilmiştir:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 */1 * * * *"
    },
    {
      "name": "cloudTable",
      "type": "table",
      "connection": "AzureWebJobsStorage",
      "tableName": "AzureWebJobsHostLogscommon",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, CloudTable cloudTable, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

    TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
            "FD2"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
            "a")));

    // Execute the query and loop through the results
    foreach (LogEntity entity in 
    await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
    {
        log.LogInformation(
            $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
    }
}

public class LogEntity : TableEntity
{
    public string OriginalName { get; set; }
}
```

CloudTable 'ın nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Tablo Depolamayı kullanmaya başlama](../cosmos-db/table-storage-how-to-use-dotnet.md).

`CloudTable` bağlamaya ve bir hata mesajı almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.


# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyası ve bağlamayı kullanan [JavaScript kodundaki](functions-reference-node.md) tablo girişi bağlamasını gösterir. İşlevi, tek bir tablo satırını okumak için bir kuyruk tetikleyicisi kullanır. 

*Function. JSON* dosyası bir `partitionKey` ve bir `rowKey`belirtir. "{QueueTrigger}" `rowKey` değeri, satır anahtarının kuyruk ileti dizesinden geldiğini gösterir.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Tek tablo satırı 

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "messageJSON",
      "type": "table",
      "tableName": "messages",
      "partitionKey": "message",
      "rowKey": "{id}",
      "connection": "AzureWebJobsStorage",
      "direction": "in"
    },
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ],
      "route": "messages/{id}"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ],
  "disabled": false
}
```

```python
import json

import azure.functions as func

def main(req: func.HttpRequest, messageJSON) -> func.HttpResponse:

    message = json.loads(messageJSON)
    return func.HttpResponse(f"Table row: {messageJSON}")
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıdaki örnek, tablo depolamada belirtilen bir bölümdeki öğelerin toplam sayısını döndüren HTTP ile tetiklenen bir işlev gösterir.

```java
@FunctionName("getallcount")
public int run(
   @HttpTrigger(name = "req",
                 methods = {HttpMethod.GET},
                 authLevel = AuthorizationLevel.ANONYMOUS) Object dummyShouldNotBeUsed,
   @TableInput(name = "items",
                tableName = "mytablename",  partitionKey = "myparkey",
                connection = "myconnvarname") MyItem[] items
) {
    return items.length;
}
```

---

## <a name="input---attributes-and-annotations"></a>Giriş öznitelikleri ve ek açıklamalar

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

 [ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda, bir tablo girişi bağlamasını yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  Özniteliğin Oluşturucusu tablo adını, Bölüm anahtarını ve satır anahtarını alır. Öznitelik, aşağıdaki örnekte gösterildiği gibi, bir `out` parametresinde veya işlevin dönüş değerinde kullanılabilir:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Aşağıdaki örnekte gösterildiği gibi, kullanılacak depolama hesabını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Tam bir örnek için bkz. giriş- C# örnek.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabını belirtmek için başka bir yol sağlar. Oluşturucu, depolama bağlantı dizesi içeren bir uygulama ayarının adını alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Kullanılacak depolama hesabı aşağıdaki sırayla belirlenir:

* `Table` Özniteliğin `Connection` özelliği.
* `StorageAccount` Özniteliği aynı parametresine uygulanan `Table` özniteliği.
* `StorageAccount` İşleve uygulanmış bir öznitelik.
* `StorageAccount` Sınıfına uygulanan bir öznitelik.
* İşlev uygulaması için varsayılan depolama hesabı ("AzureWebJobsStorage" uygulama ayarı).

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri tablo depolamadan gelen parametrelerde `@TableInput` ek açıklamasını kullanın.  Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Table` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | Yok | Ayarlanmalıdır `table`. Azure portal bağlama oluşturduğunuzda bu özellik otomatik olarak ayarlanır.|
|**direction** | Yok | Ayarlanmalıdır `in`. Azure portal bağlama oluşturduğunuzda bu özellik otomatik olarak ayarlanır. |
|**Adı** | Yok | İşlev kodundaki tabloyu veya varlığı temsil eden değişkenin adı. | 
|**tableName** | **TableName** | Tablonun adı.| 
|**partitionKey** | **partitionKey** |İsteğe bağlı. Okunacak tablo varlığının bölüm anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım](#input---usage) bölümüne bakın.| 
|**rowKey** |**RowKey** | İsteğe bağlı. Okunacak tablo varlığının satır anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım](#input---usage) bölümüne bakın.| 
|**take** |**Almanız** | İsteğe bağlı. JavaScript 'te okunacak en fazla varlık sayısı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım](#input---usage) bölümüne bakın.| 
|**filter (filtre)** |**Filtre** | İsteğe bağlı. JavaScript 'te tablo girişi için bir OData filtre ifadesi. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım](#input---usage) bölümüne bakın.| 
|**bağlantı** |**bağlantı** | Bu bağlama için kullanılacak depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adının geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyStorage" olarak ayarlarsanız, Işlevler çalışma zamanı "MyStorage" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı `AzureWebJobsStorage`adlı uygulama ayarında varsayılan depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

* **İçindeki bir satırı okuyun**

  `partitionKey` ve `rowKey`ayarlayın. `T <paramName>`yöntem parametresi kullanarak tablo verilerine erişin. Betik C# ' de, `paramName` *function. json*' nin `name` özelliğinde belirtilen değerdir. `T` genellikle `ITableEntity` uygulayan veya `TableEntity`türeten bir türdür. `filter` ve `take` özellikleri bu senaryoda kullanılmaz.

* **Bir veya daha fazla satırı okuyun**

  `IQueryable<T> <paramName>`yöntem parametresi kullanarak tablo verilerine erişin. Betik C# ' de, `paramName` *function. json*' nin `name` özelliğinde belirtilen değerdir. `T`, `ITableEntity` uygulayan veya `TableEntity`türeten bir tür olmalıdır. Gerekli filtreleme yapmak için `IQueryable` yöntemlerini kullanabilirsiniz. `partitionKey`, `rowKey`, `filter`ve `take` özellikleri bu senaryoda kullanılmaz.  

  > [!NOTE]
  > `IQueryable`, [Functions v2 çalışma zamanında](functions-versions.md)desteklenmez. Alternatif olarak, Azure depolama SDK 'sını kullanarak tabloyu okumak için [CloudTable paramName yöntemi parametresi kullanılır](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) . `CloudTable` bağlamaya ve bir hata mesajı almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

* **İçindeki bir satırı okuyun**

  `partitionKey` ve `rowKey`ayarlayın. `T <paramName>`yöntem parametresi kullanarak tablo verilerine erişin. Betik C# ' de, `paramName` *function. json*' nin `name` özelliğinde belirtilen değerdir. `T` genellikle `ITableEntity` uygulayan veya `TableEntity`türeten bir türdür. `filter` ve `take` özellikleri bu senaryoda kullanılmaz.

* **Bir veya daha fazla satırı okuyun**

  `IQueryable<T> <paramName>`yöntem parametresi kullanarak tablo verilerine erişin. Betik C# ' de, `paramName` *function. json*' nin `name` özelliğinde belirtilen değerdir. `T`, `ITableEntity` uygulayan veya `TableEntity`türeten bir tür olmalıdır. Gerekli filtreleme yapmak için `IQueryable` yöntemlerini kullanabilirsiniz. `partitionKey`, `rowKey`, `filter`ve `take` özellikleri bu senaryoda kullanılmaz.  

  > [!NOTE]
  > `IQueryable`, [Functions v2 çalışma zamanında](functions-versions.md)desteklenmez. Alternatif olarak, Azure depolama SDK 'sını kullanarak tabloyu okumak için [CloudTable paramName yöntemi parametresi kullanılır](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) . `CloudTable` bağlamaya ve bir hata mesajı almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

`filter` ve `take` özelliklerini ayarlayın. `partitionKey` veya `rowKey`ayarlama. `context.bindings.<BINDING_NAME>`kullanarak giriş tablosu varlığına (veya varlıklara) erişin. Seri durumdan çıkarılan nesneler `RowKey` ve `PartitionKey` özelliklere sahiptir.

# <a name="pythontabpython"></a>[Python](#tab/python)

Tablo verileri, işleve JSON dizesi olarak geçirilir. Giriş [örneğinde](#input)gösterildiği gibi `json.loads` çağırarak iletiyi seri hale getirme.

# <a name="javatabjava"></a>[Java](#tab/java)

[Tableınput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.tableinput) özniteliği, işlevi tetikleyen tablo satırına erişmenizi sağlar.

---

## <a name="output"></a>Çıktı

Azure depolama hesabındaki bir tabloya varlıklar yazmak için bir Azure Tablo depolama çıkış bağlaması kullanın.

> [!NOTE]
> Bu çıkış bağlaması mevcut varlıkların güncelleştirilmesini desteklemiyor. Mevcut bir varlığı güncelleştirmek için [Azure depolama SDK 'sının](../cosmos-db/tutorial-develop-table-dotnet.md#delete-an-entity) `TableOperation.Replace` işlemini kullanın.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Aşağıdaki örnek, tek bir tablo satırı yazmak için http tetikleyicisi kullanan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, ILogger log)
    {
        log.LogInformation($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyası ve [ C# ](functions-reference-csharp.md) bağlamayı kullanan betik kodu içindeki bir tablo çıkış bağlamasını gösterir. İşlev birden çok tablo varlığı yazar.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, ILogger log)
{
    for (int i = 1; i < 10; i++)
        {
            log.LogInformation($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir tablo çıkış bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev birden çok tablo varlığı yazar.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, tablo depolama çıkış bağlamasının nasıl kullanılacağını göstermektedir. `table` bağlama, `name`, `tableName`, `partitionKey`ve `connection`için değerler atayarak *function. JSON* içinde yapılandırılır:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "message",
      "type": "table",
      "tableName": "messages",
      "partitionKey": "message",
      "connection": "AzureWebJobsStorage",
      "direction": "out"
    },
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
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

Aşağıdaki işlev `rowKey` değeri için benzersiz bir UUı oluşturur ve iletiyi tablo depolama alanına devam ettirir.

```python
import logging
import uuid
import json

import azure.functions as func

def main(req: func.HttpRequest, message: func.Out[str]) -> func.HttpResponse:

    rowKey = str(uuid.uuid4())

    data = {
        "Name": "Output binding message",
        "PartitionKey": "message",
        "RowKey": rowKey
    }

    message.set(json.dumps(data))

    return func.HttpResponse(f"Message created with the rowKey: {rowKey}")
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıdaki örnekte, tek bir tablo satırı yazmak için HTTP tetikleyicisi kullanan bir Java işlevi gösterilmektedir.

```java
public class Person {
    private String PartitionKey;
    private String RowKey;
    private String Name;

    public String getPartitionKey() {return this.PartitionKey;}
    public void setPartitionKey(String key) {this.PartitionKey = key; }
    public String getRowKey() {return this.RowKey;}
    public void setRowKey(String key) {this.RowKey = key; }
    public String getName() {return this.Name;}
    public void setName(String name) {this.Name = name; }
}
    public class AddPerson {

    @FunctionName("addPerson")
    public HttpResponseMessage get(
            @HttpTrigger(name = "postPerson", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION, route="persons/{partitionKey}/{rowKey}") HttpRequestMessage<Optional<Person>> request,
            @BindingName("partitionKey") String partitionKey,
            @BindingName("rowKey") String rowKey,
            @TableOutput(name="person", partitionKey="{partitionKey}", rowKey = "{rowKey}", tableName="%MyTableName%", connection="MyConnectionString") OutputBinding<Person> person,
            final ExecutionContext context) {

        Person outPerson = new Person();
        outPerson.setPartitionKey(partitionKey);
        outPerson.setRowKey(rowKey);
        outPerson.setName(request.getBody().get().getName());

        person.setValue(outPerson);

        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(outPerson)
                        .build();
    }
}
```

Aşağıdaki örnekte, birden çok tablo satırı yazmak için HTTP tetikleyicisi kullanan bir Java işlevi gösterilmektedir.

```java
public class Person {
    private String PartitionKey;
    private String RowKey;
    private String Name;

    public String getPartitionKey() {return this.PartitionKey;}
    public void setPartitionKey(String key) {this.PartitionKey = key; }
    public String getRowKey() {return this.RowKey;}
    public void setRowKey(String key) {this.RowKey = key; }
    public String getName() {return this.Name;}
    public void setName(String name) {this.Name = name; }
}

public class AddPersons {

    @FunctionName("addPersons")
    public HttpResponseMessage get(
            @HttpTrigger(name = "postPersons", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION, route="persons/") HttpRequestMessage<Optional<Person[]>> request,
            @TableOutput(name="person", tableName="%MyTableName%", connection="MyConnectionString") OutputBinding<Person[]> persons,
            final ExecutionContext context) {

        persons.setValue(request.getBody().get());

        return request.createResponseBuilder(HttpStatus.OK)
                        .header("Content-Type", "application/json")
                        .body(request.getBody().get())
                        .build();
    }
}
```

---

## <a name="output---attributes-and-annotations"></a>Çıkış öznitelikleri ve ek açıklamaları

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)' u kullanın.

Özniteliğin Oluşturucusu tablo adını alır. Öznitelik, aşağıdaki örnekte gösterildiği gibi, bir `out` parametresinde veya işlevin dönüş değerinde kullanılabilir:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Aşağıdaki örnekte gösterildiği gibi, kullanılacak depolama hesabını belirtmek için `Connection` özelliğini ayarlayabilirsiniz:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output).

Sınıf, yöntem veya parametre düzeyinde depolama hesabını belirtmek için `StorageAccount` özniteliğini kullanabilirsiniz. Daha fazla bilgi için bkz. [giriş-öznitelikler](#input---attributes-and-annotations).

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değerleri tablo depolamaya yazmak Için parametrelerde [tableoutput](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/TableOutput.java/) ek açıklamasını kullanın.

[Daha fazla ayrıntı için örneğe](#output)bakın.

---

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Table` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | Yok | Ayarlanmalıdır `table`. Azure portal bağlama oluşturduğunuzda bu özellik otomatik olarak ayarlanır.|
|**direction** | Yok | Ayarlanmalıdır `out`. Azure portal bağlama oluşturduğunuzda bu özellik otomatik olarak ayarlanır. |
|**Adı** | Yok | Tablo veya varlığı temsil eden işlev kodunda kullanılan değişken adı. İşlev dönüş değerine başvurmak için `$return` olarak ayarlayın.| 
|**tableName** |**TableName** | Tablonun adı.| 
|**partitionKey** |**partitionKey** | Yazılacak tablo varlığının bölüm anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım bölümüne](#output---usage) bakın.| 
|**rowKey** |**RowKey** | Yazılacak tablo varlığının satır anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım bölümüne](#output---usage) bakın.| 
|**bağlantı** |**bağlantı** | Bu bağlama için kullanılacak depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adının geri kalanını belirtebilirsiniz. Örneğin, `connection` "MyStorage" olarak ayarlarsanız, Işlevler çalışma zamanı "MyStorage" adlı bir uygulama ayarı arar. `connection` boş bırakırsanız, Işlevler çalışma zamanı `AzureWebJobsStorage`adlı uygulama ayarında varsayılan depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Bir yöntem parametresi `ICollector<T> paramName` veya `T` `PartitionKey` ve `RowKey` özelliklerini içeren `IAsyncCollector<T> paramName` bir yöntem parametresi kullanarak çıkış tablosu varlığına erişin. Bu özellikler genellikle `ITableEntity` uygulayarak veya `TableEntity`devralmasından de yapılır.

Alternatif olarak, Azure Storage SDK kullanarak tabloya yazmak için `CloudTable` yöntemi parametresini kullanabilirsiniz. `CloudTable` bağlamaya ve bir hata mesajı almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Bir yöntem parametresi `ICollector<T> paramName` veya `T` `PartitionKey` ve `RowKey` özelliklerini içeren `IAsyncCollector<T> paramName` bir yöntem parametresi kullanarak çıkış tablosu varlığına erişin. Bu özellikler genellikle `ITableEntity` uygulayarak veya `TableEntity`devralmasından de yapılır. `paramName` değeri, *function. JSON*öğesinin `name` özelliğinde belirtilmiştir.

Alternatif olarak, Azure Storage SDK kullanarak tabloya yazmak için `CloudTable` yöntemi parametresini kullanabilirsiniz. `CloudTable` bağlamaya ve bir hata mesajı almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

`context.bindings.<name>` kullanarak çıkış olayına erişin; burada `<name>`, *function. JSON*' ın `name` özelliğinde belirtilen değerdir.

# <a name="pythontabpython"></a>[Python](#tab/python)

Bir işlevden tablo depolama satırı iletisi çıktısı için iki seçenek vardır:

- **Dönüş değeri**: *function. JSON* içindeki `name` özelliğini `$return`olarak ayarlayın. Bu yapılandırmayla, işlevin dönüş değeri tablo depolama satırı olarak kalıcıdır.

- **Zorunludur**: bir değeri, [Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python) türü olarak belirtilen parametresinin [set](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none) yöntemine geçirin. `set` geçirilen değer bir olay hub 'ı iletisi olarak kalıcıdır.

# <a name="javatabjava"></a>[Java](#tab/java)

[Tablestorageoutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.tableoutput?view=azure-java-stablet) ek açıklamasını kullanarak bir işlevden tablo depolama satırını almak için iki seçenek vardır:

- **Dönüş değeri**: ek açıklamanın işleve uygulanması için, işlevin dönüş değeri tablo depolama satırı olarak kalıcıdır.

- Zorunlu **: ileti**değerini açık olarak ayarlamak için, `T` `PartitionKey` ve `RowKey` özelliklerini içeren [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding)türünün belirli bir parametresine ek açıklamayı uygulayın. Bu özellikler genellikle `ITableEntity` uygulayarak veya `TableEntity`devralmasından de yapılır.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Tablo | [Tablo hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| Blob, tablo, kuyruk | [Depolama hatası kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, tablo, kuyruk | [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
