---
title: Azure Işlevleri için Azure Tablo depolama bağlamaları
description: Azure Işlevleri 'nde Azure Tablo depolama bağlamalarını nasıl kullanacağınızı anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.openlocfilehash: 1aa3537679ee37cbc6085344d2f31ae4043d32bb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80520668"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Işlevleri için Azure Tablo depolama bağlamaları

Bu makalede Azure Işlevleri 'nde Azure Tablo depolama bağlamalarıyla nasıl çalışılacağı açıklanmaktadır. Azure Işlevleri, Azure Tablo depolaması için giriş ve çıkış bağlamalarını destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler-Işlevler 1. x

Tablo depolama bağlamaları [Microsoft. Azure. WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2. x ' de verilmiştir. Paketin kaynak kodu, [Azure-WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub deposundadır.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

Tablo depolama bağlamaları [Microsoft. Azure. WebJobs. Extensions. Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketi, sürüm 3. x içinde verilmiştir. Paketin kaynak kodu, [Azure-WebJobs-SDK](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) GitHub deposundadır.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Girdi

Azure depolama hesabındaki bir tabloyu okumak için Azure Tablo depolama girişi bağlamasını kullanın.

# <a name="c"></a>[, #](#tab/csharp)

### <a name="one-entity"></a>Bir varlık

Aşağıdaki örnekte, tek bir tablo satırını okuyan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. Tabloya yerleştirilen her kayıt için, işlev tetiklenecektir.

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

Aşağıdaki örnek, `MyPoco` sınıfın türetildiği birden çok tablo satırını okuyan bir [C# işlevini](functions-dotnet-class-library.md) gösterir. `TableEntity`

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

`IQueryable`[Functions v2 çalışma zamanında](functions-versions.md)desteklenmez. Alternatif olarak, Azure depolama SDK `CloudTable` 'sını kullanarak tabloyu okumak için bir yöntem parametresi kullanılır. Aşağıda, Azure Işlevleri günlük tablosunu sorgulayan bir işleve örnek verilmiştir:

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

' A bağlanıp `CloudTable` bir hata iletisi almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

### <a name="one-entity"></a>Bir varlık

Aşağıdaki örnek, bir *function. JSON* dosyası ve bağlamayı kullanan [C# betik](functions-reference-csharp.md) kodundaki tablo girişi bağlamasını gösterir. İşlevi, tek bir tablo satırını okumak için bir kuyruk tetikleyicisi kullanır. 

*Function. JSON* dosyası bir `partitionKey` ve belirtir `rowKey`. " `rowKey` {QueueTrigger}" değeri, satır anahtarının kuyruk ileti dizesinden geldiğini gösterir.

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

[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodu aşağıda verilmiştir:

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

Aşağıdaki örnek, bir *function. JSON* dosyası ve bağlamayı kullanan [C# betik](functions-reference-csharp.md) kodundaki tablo girişi bağlamasını gösterir. İşlevi, bir kuyruk iletisinde belirtilen bölüm anahtarı için varlıkları okur.

İşte *function. JSON* dosyası:

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

[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodu, varlık türünün türeyebilmesi için Azure depolama SDK 'sına bir başvuru ekler `TableEntity`:

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

`IQueryable`[2. x ve üzeri sürümler](functions-versions.md)için işlevler çalışma zamanı 'nda desteklenmez. Alternatif olarak, Azure depolama SDK `CloudTable` 'sını kullanarak tabloyu okumak için bir yöntem parametresi kullanılır. Aşağıda, Azure Işlevleri günlük tablosunu sorgulayan bir işleve örnek verilmiştir:

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

' A bağlanıp `CloudTable` bir hata iletisi almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.


# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyası ve bağlamayı kullanan [JavaScript kodundaki](functions-reference-node.md) tablo girişi bağlamasını gösterir. İşlevi, tek bir tablo satırını okumak için bir kuyruk tetikleyicisi kullanır. 

*Function. JSON* dosyası bir `partitionKey` ve belirtir `rowKey`. " `rowKey` {QueueTrigger}" değeri, satır anahtarının kuyruk ileti dizesinden geldiğini gösterir.

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

[Yapılandırma](#input---configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

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

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, tablo depolamada belirtilen bir bölümde bulunan kişi nesnelerinin bir listesini döndüren HTTP ile tetiklenen bir işlev gösterir. Örnekte, bölüm anahtarı http rotasına ayıklanır ve tableName ve Connection işlevi işlev ayarlarından çıkarılır. 

```java
public class Person {
    private String PartitionKey;
    private String RowKey;
    private String Name;

    public String getPartitionKey() { return this.PartitionKey; }
    public void setPartitionKey(String key) { this.PartitionKey = key; }
    public String getRowKey() { return this.RowKey; }
    public void setRowKey(String key) { this.RowKey = key; }
    public String getName() { return this.Name; }
    public void setName(String name) { this.Name = name; }
}

@FunctionName("getPersonsByPartitionKey")
public Person[] get(
        @HttpTrigger(name = "getPersons", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="persons/{partitionKey}") HttpRequestMessage<Optional<String>> request,
        @BindingName("partitionKey") String partitionKey,
        @TableInput(name="persons", partitionKey="{partitionKey}", tableName="%MyTableName%", connection="MyConnectionString") Person[] persons,
        final ExecutionContext context) {

    context.getLogger().info("Got query for person related to persons with partition key: " + partitionKey);

    return persons;
}
```

Aşağıdaki örnekte gösterildiği gibi, Tableınput ek açıklaması isteğin JSON gövdesinden bağlamaları da ayıklayabilir.

```java
@FunctionName("GetPersonsByKeysFromRequest")
public HttpResponseMessage get(
        @HttpTrigger(name = "getPerson", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="query") HttpRequestMessage<Optional<String>> request,
        @TableInput(name="persons", partitionKey="{partitionKey}", rowKey = "{rowKey}", tableName="%MyTableName%", connection="MyConnectionString") Person person,
        final ExecutionContext context) {

    if (person == null) {
        return request.createResponseBuilder(HttpStatus.NOT_FOUND)
                    .body("Person not found.")
                    .build();
    }

    return request.createResponseBuilder(HttpStatus.OK)
                    .header("Content-Type", "application/json")
                    .body(person)
                    .build();
}
```

Aşağıdaki örnekler, bir Azure tablosunda belirli bir ada sahip kişileri sorgulamak için filtreyi kullanır ve olası eşleşmelerin sayısını 10 sonuçlarla sınırlar.

```java
@FunctionName("getPersonsByName")
public Person[] get(
        @HttpTrigger(name = "getPersons", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="filter/{name}") HttpRequestMessage<Optional<String>> request,
        @BindingName("name") String name,
        @TableInput(name="persons", filter="Name eq '{name}'", take = "10", tableName="%MyTableName%", connection="MyConnectionString") Person[] persons,
        final ExecutionContext context) {

    context.getLogger().info("Got query for person related to persons with name: " + name);

    return persons;
}
```

---

## <a name="input---attributes-and-annotations"></a>Giriş öznitelikleri ve ek açıklamalar

# <a name="c"></a>[, #](#tab/csharp)

 [C# sınıf kitaplıklarında](functions-dotnet-class-library.md), tablo girişi bağlamasını yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  Özniteliğin Oluşturucusu tablo adını, Bölüm anahtarını ve satır anahtarını alır. Öznitelik, aşağıdaki örnekte gösterildiği gibi, `out` bir parametre veya işlevin dönüş değeri üzerinde kullanılabilir:

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

  Aşağıdaki örnekte gösterildiği gibi `Connection` , kullanılacak depolama hesabını belirtmek için özelliğini ayarlayabilirsiniz:

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

  Tam bir örnek için bkz. Input-C# örneği.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabını belirtmek için başka bir yol sağlar. Oluşturucu, depolama bağlantı dizesi içeren bir uygulama ayarının adını alır. Özniteliği parametresi, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf düzeyi ve Yöntem düzeyini gösterir:

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
* Özniteliği `StorageAccount` ile `Table` aynı parametreye uygulanan öznitelik.
* İşleve `StorageAccount` uygulanan öznitelik.
* Sınıfına `StorageAccount` uygulanan öznitelik.
* İşlev uygulaması için varsayılan depolama hesabı ("AzureWebJobsStorage" uygulama ayarı).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri tablo `@TableInput` depolamadan gelen parametrelerde ek açıklama kullanın.  Bu ek açıklama, kullanılarak `Optional<T>`yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="input---configuration"></a>Giriş-yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `Table` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|function. JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle** | yok | Olarak `table`ayarlanmalıdır. Azure portal bağlama oluşturduğunuzda bu özellik otomatik olarak ayarlanır.|
|**Görünüm** | yok | Olarak `in`ayarlanmalıdır. Azure portal bağlama oluşturduğunuzda bu özellik otomatik olarak ayarlanır. |
|**ada** | yok | İşlev kodundaki tabloyu veya varlığı temsil eden değişkenin adı. | 
|**tableName** | **TableName** | Tablonun adı.| 
|**partitionKey** | **PartitionKey** |İsteğe bağlı. Okunacak tablo varlığının bölüm anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım](#input---usage) bölümüne bakın.| 
|**rowKey** |**RowKey** | İsteğe bağlı. Okunacak tablo varlığının satır anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım](#input---usage) bölümüne bakın.| 
|**almanız** |**Take** | İsteğe bağlı. JavaScript 'te okunacak en fazla varlık sayısı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım](#input---usage) bölümüne bakın.| 
|**filtreyle** |**Filtrele** | İsteğe bağlı. JavaScript 'te tablo girişi için bir OData filtre ifadesi. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım](#input---usage) bölümüne bakın.| 
|**bağlantı** |**Bağlanma** | Bu bağlama için kullanılacak depolama bağlantı dizesini içeren bir uygulama ayarının adı. Bu ayar bir "AzureWebJobs" önekli uygulama ayarının veya bağlantı dizesi adının adı olabilir. Örneğin, ayar adınız "AzureWebJobsMyStorage" ise burada "MyStorage" belirtebilirsiniz. Işlevler çalışma zamanı, "AzureWebJobsMyStorage" adlı bir uygulama ayarını otomatik olarak arar. Boş bırakırsanız `connection` , işlevler çalışma zamanı adlı `AzureWebJobsStorage`uygulama ayarında varsayılan depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş kullanımı

# <a name="c"></a>[, #](#tab/csharp)

* **İçindeki bir satırı okuyun**

  Ve `partitionKey` `rowKey`ayarlayın. Bir yöntem parametresi `T <paramName>`kullanarak tablo verilerine erişin. C# komut dosyasında, `paramName` `name` *function. JSON*özelliğinde belirtilen değerdir. `T`genellikle uygulayan `ITableEntity` veya ondan `TableEntity`türetilen bir türdür. `filter` Ve `take` özellikleri bu senaryoda kullanılmaz.

* **Bir veya daha fazla satırı okuyun**

  Bir yöntem parametresi `IQueryable<T> <paramName>`kullanarak tablo verilerine erişin. C# komut dosyasında, `paramName` `name` *function. JSON*özelliğinde belirtilen değerdir. `T`uygulayan `ITableEntity` veya ondan `TableEntity`türetilen bir tür olmalıdır. Yöntemleri, gerekli `IQueryable` filtrelemeleri yapmak için kullanabilirsiniz. `partitionKey` `rowKey`, `filter`, Ve `take` özellikleri bu senaryoda kullanılmaz.  

  > [!NOTE]
  > `IQueryable`[Functions v2 çalışma zamanında](functions-versions.md)desteklenmez. Alternatif olarak, Azure depolama SDK 'sını kullanarak tabloyu okumak için [CloudTable paramName yöntemi parametresi kullanılır](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) . ' A bağlanıp `CloudTable` bir hata iletisi almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

* **İçindeki bir satırı okuyun**

  Ve `partitionKey` `rowKey`ayarlayın. Bir yöntem parametresi `T <paramName>`kullanarak tablo verilerine erişin. C# komut dosyasında, `paramName` `name` *function. JSON*özelliğinde belirtilen değerdir. `T`genellikle uygulayan `ITableEntity` veya ondan `TableEntity`türetilen bir türdür. `filter` Ve `take` özellikleri bu senaryoda kullanılmaz.

* **Bir veya daha fazla satırı okuyun**

  Bir yöntem parametresi `IQueryable<T> <paramName>`kullanarak tablo verilerine erişin. C# komut dosyasında, `paramName` `name` *function. JSON*özelliğinde belirtilen değerdir. `T`uygulayan `ITableEntity` veya ondan `TableEntity`türetilen bir tür olmalıdır. Yöntemleri, gerekli `IQueryable` filtrelemeleri yapmak için kullanabilirsiniz. `partitionKey` `rowKey`, `filter`, Ve `take` özellikleri bu senaryoda kullanılmaz.  

  > [!NOTE]
  > `IQueryable`[Functions v2 çalışma zamanında](functions-versions.md)desteklenmez. Alternatif olarak, Azure depolama SDK 'sını kullanarak tabloyu okumak için [CloudTable paramName yöntemi parametresi kullanılır](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) . ' A bağlanıp `CloudTable` bir hata iletisi almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

`filter` Ve `take` özelliklerini ayarlayın. Veya `rowKey`ayarlama `partitionKey` . Kullanarak `context.bindings.<BINDING_NAME>`giriş tablosu varlığına (veya varlıklara) erişin. Seri durumdan çıkarılan nesneler `RowKey` ve `PartitionKey` özellikleri.

# <a name="python"></a>[Python](#tab/python)

Tablo verileri, işleve JSON dizesi olarak geçirilir. Giriş `json.loads` [örneğinde](#input)gösterildiği gibi çağırarak iletiyi seri durumdan kaldırın.

# <a name="java"></a>[Java](#tab/java)

[Tableınput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.tableinput) özniteliği, işlevi tetikleyen tablo satırına erişmenizi sağlar.

---

## <a name="output"></a>Çıktı

Azure depolama hesabındaki bir tabloya varlıklar yazmak için bir Azure Tablo depolama çıkış bağlaması kullanın.

> [!NOTE]
> Bu çıkış bağlaması mevcut varlıkların güncelleştirilmesini desteklemiyor. Mevcut bir `TableOperation.Replace` varlığı güncelleştirmek Için [Azure depolama SDK 'sının](../cosmos-db/tutorial-develop-table-dotnet.md#delete-an-entity) işlemini kullanın.

# <a name="c"></a>[, #](#tab/csharp)

Aşağıdaki örnekte, tek bir tablo satırı yazmak için HTTP tetikleyicisi kullanan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. 

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

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyası ve bağlamayı kullanan [C# betik](functions-reference-csharp.md) kodundaki tablo çıkış bağlamasını gösterir. İşlev birden çok tablo varlığı yazar.

İşte *function. JSON* dosyası:

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

[Yapılandırma](#output---configuration) bölümünde bu özellikler açıklanmaktadır.

C# betik kodu aşağıda verilmiştir:

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir tablo çıkış bağlamasını ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev birden çok tablo varlığı yazar.

İşte *function. JSON* dosyası:

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

[Yapılandırma](#output---configuration) bölümünde bu özellikler açıklanmaktadır.

JavaScript kodu aşağıda verilmiştir:

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

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, tablo depolama çıkış bağlamasının nasıl kullanılacağını göstermektedir. `table` `name`Bağlama `tableName`, `partitionKey`,, ve ' a değerler atanarak,,, ve `connection`' a değer atayarak *. JSON* içinde yapılandırılır:

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

Aşağıdaki işlev `rowKey` değer için benzersiz bir uuı oluşturur ve iletiyi tablo depolamaya devam ettirir.

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

# <a name="java"></a>[Java](#tab/java)

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

# <a name="c"></a>[, #](#tab/csharp)

[C# sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)' u kullanın.

Özniteliğin Oluşturucusu tablo adını alır. Öznitelik, aşağıdaki örnekte gösterildiği gibi, `out` bir parametre veya işlevin dönüş değeri üzerinde kullanılabilir:

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

Aşağıdaki örnekte gösterildiği gibi `Connection` , kullanılacak depolama hesabını belirtmek için özelliğini ayarlayabilirsiniz:

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

Tam bir örnek için bkz. [output-C# örneği](#output).

Sınıfı, yöntemi veya `StorageAccount` parametre düzeyinde depolama hesabını belirtmek için özniteliğini kullanabilirsiniz. Daha fazla bilgi için bkz. [giriş-öznitelikler](#input---attributes-and-annotations).

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Öznitelikler C# betiği tarafından desteklenmez.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değerleri tablo depolamaya yazmak Için parametrelerde [tableoutput](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/TableOutput.java/) ek açıklamasını kullanın.

[Daha fazla ayrıntı için örneğe](#output)bakın.

---

## <a name="output---configuration"></a>Çıkış-yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ve `Table` özniteliğinde ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır.

|function. JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türüyle** | yok | Olarak `table`ayarlanmalıdır. Azure portal bağlama oluşturduğunuzda bu özellik otomatik olarak ayarlanır.|
|**Görünüm** | yok | Olarak `out`ayarlanmalıdır. Azure portal bağlama oluşturduğunuzda bu özellik otomatik olarak ayarlanır. |
|**ada** | yok | Tablo veya varlığı temsil eden işlev kodunda kullanılan değişken adı. `$return` İşlev dönüş değerine başvuracak şekilde ayarlanır.| 
|**tableName** |**TableName** | Tablonun adı.| 
|**partitionKey** |**PartitionKey** | Yazılacak tablo varlığının bölüm anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım bölümüne](#output---usage) bakın.| 
|**rowKey** |**RowKey** | Yazılacak tablo varlığının satır anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin yönergeler için [kullanım bölümüne](#output---usage) bakın.| 
|**bağlantı** |**Bağlanma** | Bu bağlama için kullanılacak depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adının geri kalanını belirtebilirsiniz. Örneğin, "MyStorage" `connection` olarak ayarlarsanız, işlevler çalışma zamanı "Mystorage" adlı bir uygulama ayarı arar. Boş bırakırsanız `connection` , işlevler çalışma zamanı adlı `AzureWebJobsStorage`uygulama ayarında varsayılan depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış kullanımı

# <a name="c"></a>[, #](#tab/csharp)

Bir yöntem parametresi `ICollector<T> paramName` kullanarak çıkış tablosu varlığına erişin veya `IAsyncCollector<T> paramName` , `T` `PartitionKey` ve `RowKey` özelliklerini içerir. Bu özellikler genellikle uygulama `ITableEntity` veya devralma `TableEntity`ile birlikte sunulur.

Alternatif olarak, Azure Storage `CloudTable` SDK kullanarak tabloya yazmak için bir yöntem parametresi kullanabilirsiniz. ' A bağlanıp `CloudTable` bir hata iletisi almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

Bir yöntem parametresi `ICollector<T> paramName` kullanarak çıkış tablosu varlığına erişin veya `IAsyncCollector<T> paramName` , `T` `PartitionKey` ve `RowKey` özelliklerini içerir. Bu özellikler genellikle uygulama `ITableEntity` veya devralma `TableEntity`ile birlikte sunulur. `paramName` Değer, `name` *function. JSON*özelliğinde belirtilir.

Alternatif olarak, Azure Storage `CloudTable` SDK kullanarak tabloya yazmak için bir yöntem parametresi kullanabilirsiniz. ' A bağlanıp `CloudTable` bir hata iletisi almaya çalışırsanız, [doğru depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)başvurunuz olduğundan emin olun.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Output olayına, *function. JSON* `name` özelliğinde `<name>` belirtilen değerin nerede olduğunu kullanarak `context.bindings.<name>` erişin.

# <a name="python"></a>[Python](#tab/python)

Bir işlevden tablo depolama satırı iletisi çıktısı için iki seçenek vardır:

- **Dönüş değeri**: `name` *function. JSON* içindeki özelliği olarak `$return`ayarlayın. Bu yapılandırmayla, işlevin dönüş değeri tablo depolama satırı olarak kalıcıdır.

- **Zorunludur**: bir değeri, [Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python) türü olarak belirtilen parametresinin [set](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none) yöntemine geçirin. Geçirilen `set` değer bir olay hub 'ı iletisi olarak kalıcıdır.

# <a name="java"></a>[Java](#tab/java)

[Tablestorageoutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.tableoutput?view=azure-java-stablet) ek açıklamasını kullanarak bir işlevden tablo depolama satırını almak için iki seçenek vardır:

- **Dönüş değeri**: ek açıklamanın işleve uygulanması için, işlevin dönüş değeri tablo depolama satırı olarak kalıcıdır.

- **Kesinlik**: ileti değerini açık olarak ayarlamak için, [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding)ek açıklamayı türünün belirli bir parametresine uygulayın; burada `T` `PartitionKey` ve `RowKey` özellikleri de bulunur. Bu özellikler genellikle uygulama `ITableEntity` veya devralma `TableEntity`ile birlikte sunulur.

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
