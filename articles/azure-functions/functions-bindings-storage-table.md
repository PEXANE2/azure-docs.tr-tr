---
title: Azure İşlevler için Azure Tablo depolama bağlamaları
description: Azure İşlevlerinde Azure Tablo depolama bağlamalarının nasıl kullanılacağını öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.openlocfilehash: edeafb5730f06dac22fd9919ca42ea388d5fd0f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277186"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure İşlevler için Azure Tablo depolama bağlamaları

Bu makalede, Azure İşlevlerinde Azure Tablo depolama bağlamalarıyla nasıl çalışılalı ş. Azure İşlevler, Azure Tablo depolama alanı için giriş ve çıktı bağlamalarını destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - Fonksiyonlar 1.x

Tablo depolama ciltleri [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi sürüm 2.x'te sağlanır. Paketin kaynak kodu [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub deposundadır.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler - Fonksiyonlar 2.x ve üzeri

Tablo depolama ciltleri [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketi, sürüm 3.x'te sağlanır. Paketin kaynak kodu [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) GitHub deposundadır.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Giriş

Azure Depolama hesabındaki tabloyu okumak için Azure Tablo depolama girişi bağlamayı kullanın.

# <a name="c"></a>[C #](#tab/csharp)

### <a name="one-entity"></a>Tek bir varlık

Aşağıdaki örnekte, tek bir tablo satırı okuyan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir. Tabloya eklenen her kayıt için işlev tetiklenir.

Satır tuşu değeri "{queueTrigger}" satır anahtarının sıra iletisi dizesinden geldiğini gösterir.

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

Aşağıdaki örnekte, `MyPoco` sınıfın `TableEntity`türetildiği birden çok tablo satırı okuyan bir [C# işlevi](functions-dotnet-class-library.md) gösterilmektedir.

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

`IQueryable`[v2 çalışma zamanındaki işlevlerde](functions-versions.md)desteklenmez. Alternatif olarak, Azure `CloudTable` Depolama SDK'sını kullanarak tabloyu okumak için bir yöntem parametresi kullanmak vardır. Azure İşlevleri günlük tablosunu sorgulayan bir işlev örneği aşağıda verilmiştir:

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

CloudTable'ın nasıl kullanılacağı hakkında daha fazla bilgi için Azure [Table depolama alanına başlayın'](../cosmos-db/table-storage-how-to-use-dotnet.md)a bakın.

Bir hata iletisine `CloudTable` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

### <a name="one-entity"></a>Tek bir varlık

Aşağıdaki örnek, *bir function.json* dosyasında tablo girişi bağlamayı ve bağlamayı kullanan [C# komut dosyası](functions-reference-csharp.md) kodunu gösterir. İşlev, tek bir tablo satırını okumak için bir sıra tetikleyicisi kullanır. 

*Function.json* dosyası a ve `partitionKey` a. `rowKey` "{queueTrigger}" `rowKey` değeri satır anahtarının sıra iletisi dizesinden geldiğini gösterir.

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

[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

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

Aşağıdaki örnek, *bir function.json* dosyasında tablo girişi bağlamayı ve bağlamayı kullanan [C# komut dosyası](functions-reference-csharp.md) kodunu gösterir. İşlev, sıra iletisinde belirtilen bir bölüm anahtarının varlıklarını okur.

Burada *function.json* dosyası:

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

[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

C# komut dosyası kodu, varlık türünün aşağıdakilerden `TableEntity`kaynaklanabilmesi için Azure Depolama SDK'sına bir başvuru ekler:

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

`IQueryable`[2.x ve üzeri sürümler](functions-versions.md)için Fonksiyonlar çalışma zamanında desteklenmez) . Alternatif olarak, Azure `CloudTable` Depolama SDK'sını kullanarak tabloyu okumak için bir yöntem parametresi kullanmak vardır. Azure İşlevleri günlük tablosunu sorgulayan bir işlev örneği aşağıda verilmiştir:

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

CloudTable'ın nasıl kullanılacağı hakkında daha fazla bilgi için Azure [Table depolama alanına başlayın'](../cosmos-db/table-storage-how-to-use-dotnet.md)a bakın.

Bir hata iletisine `CloudTable` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.


# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *bir function.json* dosyasında ve bağlamayı kullanan [JavaScript kodunda](functions-reference-node.md) bir tablo girişi bağlamayı gösterir. İşlev, tek bir tablo satırını okumak için bir sıra tetikleyicisi kullanır. 

*Function.json* dosyası a ve `partitionKey` a. `rowKey` "{queueTrigger}" `rowKey` değeri satır anahtarının sıra iletisi dizesinden geldiğini gösterir.

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

[Yapılandırma](#input---configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

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

Aşağıdaki örnekte, Tablo depolama alanında belirli bir bölümde bulunan kişi nesnelerin listesini döndüren bir HTTP tetikleme işlevi gösterilmektedir. Örnekte, bölüm anahtarı http rotasından çıkarılır ve tablo Adı ve bağlantı işlev ayarlarından alınır. 

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

TableInput ek gösterimi, aşağıdaki örnekte görüldüğü gibi, isteğin json gövdesinden de ciltler ayıklayabilir.

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

Aşağıdaki örnekler, Azure Tablosunda belirli bir ada sahip kişilerin sorgulanması için Filtre'yi kullanır ve olası eşleşme sayısını 10 sonuçla sınırlar.

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

## <a name="input---attributes-and-annotations"></a>Giriş - öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

 [C# sınıfı kitaplıklarında,](functions-dotnet-class-library.md)tablo giriş bağlamayapılandırmak için aşağıdaki öznitelikleri kullanın:

* [Tableattribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  Özniteliğin oluşturucusu tablo adını, bölüm anahtarını ve satır anahtarını alır. Öznitelik, aşağıdaki örnekte `out` gösterildiği gibi bir parametrede veya işlevin geri dönüş değerinde kullanılabilir:

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

  `Connection` Aşağıdaki örnekte gösterildiği gibi, özelliği kullanılacak depolama hesabını belirtecek şekilde ayarlayabilirsiniz:

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

  Tam bir örnek için giriş - C# örneğine bakın.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabını belirtmek için başka bir yol sağlar. Oluşturucu, depolama bağlantısı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntem veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf düzeyini ve yöntem düzeyini gösterir:

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

Kullanılacak depolama hesabı aşağıdaki sırada belirlenir:

* `Table` Özniteliğin `Connection` özelliği.
* Öznitelik `StorageAccount` ile aynı parametreye `Table` uygulanan öznitelik.
* İşlev `StorageAccount` için uygulanan öznitelik.
* Sınıfa `StorageAccount` uygulanan öznitelik.
* İşlev uygulaması için varsayılan depolama hesabı ("AzureWebJobsStorage" uygulama ayarı).

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri Tablo depolamasından gelecek parametrelerdeki `@TableInput` ek açıklamayı kullanın.  Bu ek açıklama, yerel Java türleri, POJO'lar veya `Optional<T>`nullable değerleri kullanılarak kullanılabilir.

---

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Table` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `table`olmalı. Azure portalında bağlamayı oluşturduğunuzda bu özellik otomatik olarak ayarlanır.|
|**Yön** | yok | Ayarlanmış `in`olmalı. Azure portalında bağlamayı oluşturduğunuzda bu özellik otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodundaki tabloyu veya varlığı temsil eden değişkenin adı. | 
|**Tablename** | **TableName** | Masanın adı.| 
|**partitionKey** | **PartitionKey** |İsteğe bağlı. Okunacak tablo varlığının bölüm anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin kılavuzlar için [kullanım](#input---usage) bölümüne bakın.| 
|**rowKey** |**RowKey** | İsteğe bağlı. Okunacak tablo varlığının satır anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin kılavuzlar için [kullanım](#input---usage) bölümüne bakın.| 
|**almak** |**Take** | İsteğe bağlı. JavaScript'te okunacak maksimum varlık sayısı. Bu özelliğin nasıl kullanılacağına ilişkin kılavuzlar için [kullanım](#input---usage) bölümüne bakın.| 
|**Filtre** |**Filtrele** | İsteğe bağlı. JavaScript'te tablo girişi için Bir OData filtre ifadesi. Bu özelliğin nasıl kullanılacağına ilişkin kılavuzlar için [kullanım](#input---usage) bölümüne bakın.| 
|**bağlantı** |**Bağlantı** | Bu bağlama için kullanılacak Depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyStorage" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "MyStorage" adlı bir uygulama ayarı arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı, uygulama ayarında adı `AzureWebJobsStorage`verilen varsayılan Depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

# <a name="c"></a>[C #](#tab/csharp)

* **Bir satırı okuma**

  Ayarlayın `partitionKey` `rowKey`ve . Yöntem parametresi `T <paramName>`kullanarak tablo verilerine erişin. C# komut `paramName` dosyasında, `name` *function.json*özelliğibelirtilen değerdir. `T`genellikle `ITableEntity` `TableEntity`uygular veya türetilmiştir. Ve `filter` `take` özellikleri bu senaryoda kullanılmaz.

* **Bir veya daha fazla satır okuma**

  Yöntem parametresi `IQueryable<T> <paramName>`kullanarak tablo verilerine erişin. C# komut `paramName` dosyasında, `name` *function.json*özelliğibelirtilen değerdir. `T`'den türeyen `ITableEntity` veya türetilen bir tür olmalıdır. `TableEntity` Gerekli herhangi `IQueryable` bir filtreleme yapmak için yöntemleri kullanabilirsiniz. Bu `rowKey`senaryoda , , `filter`ve `take` özellikleri kullanılmaz. `partitionKey`  

  > [!NOTE]
  > `IQueryable`[v2 çalışma zamanındaki işlevlerde](functions-versions.md)desteklenmez. Alternatif olarak, Azure Depolama SDK'sını kullanarak tabloyu okumak için [CloudTable paramName yöntemi parametresi kullanmak](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) alternatiftir. Bir hata iletisine `CloudTable` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

* **Bir satırı okuma**

  Ayarlayın `partitionKey` `rowKey`ve . Yöntem parametresi `T <paramName>`kullanarak tablo verilerine erişin. C# komut `paramName` dosyasında, `name` *function.json*özelliğibelirtilen değerdir. `T`genellikle `ITableEntity` `TableEntity`uygular veya türetilmiştir. Ve `filter` `take` özellikleri bu senaryoda kullanılmaz.

* **Bir veya daha fazla satır okuma**

  Yöntem parametresi `IQueryable<T> <paramName>`kullanarak tablo verilerine erişin. C# komut `paramName` dosyasında, `name` *function.json*özelliğibelirtilen değerdir. `T`'den türeyen `ITableEntity` veya türetilen bir tür olmalıdır. `TableEntity` Gerekli herhangi `IQueryable` bir filtreleme yapmak için yöntemleri kullanabilirsiniz. Bu `rowKey`senaryoda , , `filter`ve `take` özellikleri kullanılmaz. `partitionKey`  

  > [!NOTE]
  > `IQueryable`[v2 çalışma zamanındaki işlevlerde](functions-versions.md)desteklenmez. Alternatif olarak, Azure Depolama SDK'sını kullanarak tabloyu okumak için [CloudTable paramName yöntemi parametresi kullanmak](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) alternatiftir. Bir hata iletisine `CloudTable` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

# <a name="javascript"></a>[Javascript](#tab/javascript)

`filter` Ve `take` özelliklerini ayarlayın. Ayarlama `partitionKey` ya da. `rowKey` Giriş tablosu varlığına (veya varlıklara) kullanarak erişin. `context.bindings.<BINDING_NAME>` Deserialized nesneler ve `RowKey` `PartitionKey` özellikleri vardır.

# <a name="python"></a>[Python](#tab/python)

Tablo verileri json dizesi olarak işleve aktarılır. Giriş [örneğinde](#input)gösterildiği gibi `json.loads` arayarak iletiyi seri dışı hale

# <a name="java"></a>[Java](#tab/java)

[TableInput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.tableinput) özniteliği, işlevi tetikleyen tablo satırına erişmenizi sağlar.

---

## <a name="output"></a>Çıktı

Varlıkları azure depolama hesabındaki bir tabloya yazmak için Azure Tablo depolama çıktısı bağlama yı kullanın.

> [!NOTE]
> Bu çıktı bağlama varolan varlıkların güncelleştirilmesini desteklemez. Varolan `TableOperation.Replace` bir varlığı güncelleştirmek için [Azure Depolama SDK'daki](../cosmos-db/tutorial-develop-table-dotnet.md#delete-an-entity) işlemi kullanın.

# <a name="c"></a>[C #](#tab/csharp)

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, *bir function.json* dosyasında tablo çıktısı bağlamave bağlamayı kullanan [C# komut dosyası](functions-reference-csharp.md) kodunu gösterir. İşlev birden çok tablo varlıkları yazar.

Burada *function.json* dosyası:

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

[Yapılandırma](#output---configuration) bölümü bu özellikleri açıklar.

İşte C# komut dosyası kodu:

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

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, *bir function.json* dosyasında bir tablo çıktısı bağlama ve bağlama yı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir. İşlev birden çok tablo varlıkları yazar.

Burada *function.json* dosyası:

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

[Yapılandırma](#output---configuration) bölümü bu özellikleri açıklar.

İşte JavaScript kodu:

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

Aşağıdaki örnek, Tablo depolama çıktısı bağlamanın nasıl kullanılacağını göstermektedir. `table` Bağlama, *function.json'da* `name`, , , `tableName` `partitionKey`, ve: `connection`

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

Aşağıdaki `rowKey` işlev, değer için benzersiz bir UUI oluşturur ve iletiyi Tablo depolama alanına devam ettirir.

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

Aşağıdaki örnekte, birden çok tablo satırı yazmak için BIR HTTP tetikleyicisi kullanan bir Java işlevi gösterilmektedir.

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

## <a name="output---attributes-and-annotations"></a>Çıktı - öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarında](functions-dotnet-class-library.md) [TableAttribute'i](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)kullanın.

Özniteliğin oluşturucusu tablo adını alır. Öznitelik, aşağıdaki örnekte `out` gösterildiği gibi bir parametrede veya işlevin geri dönüş değerinde kullanılabilir:

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

`Connection` Aşağıdaki örnekte gösterildiği gibi, özelliği kullanılacak depolama hesabını belirtecek şekilde ayarlayabilirsiniz:

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

Tam bir örnek için [bkz.](#output)

Sınıf, yöntem `StorageAccount` veya parametre düzeyinde depolama hesabını belirtmek için özniteliği kullanabilirsiniz. Daha fazla bilgi için [Giriş - özniteliklerine](#input---attributes-and-annotations)bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değerdepolama alanına değer yazmak için parametreler deki [Tablo Çıktısı](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/TableOutput.java/) ek açıklamasını kullanın.

Daha [fazla ayrıntı için örneğe](#output)bakın.

---

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama `Table` yapılandırma özellikleri ve öznitelik açıklanmaktadır.

|function.json özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** | yok | Ayarlanmış `table`olmalı. Azure portalında bağlamayı oluşturduğunuzda bu özellik otomatik olarak ayarlanır.|
|**Yön** | yok | Ayarlanmış `out`olmalı. Azure portalında bağlamayı oluşturduğunuzda bu özellik otomatik olarak ayarlanır. |
|**Adı** | yok | Tablo yu veya varlığı temsil eden işlev kodunda kullanılan değişken adı. İşlev `$return` geri dönüş değerine başvurmak üzere ayarlayın.| 
|**Tablename** |**TableName** | Masanın adı.| 
|**partitionKey** |**PartitionKey** | Yazmak için tablo varlığının bölüm anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin kılavuzlar için [kullanım bölümüne](#output---usage) bakın.| 
|**rowKey** |**RowKey** | Yazacak tablo varlığının satır anahtarı. Bu özelliğin nasıl kullanılacağına ilişkin kılavuzlar için [kullanım bölümüne](#output---usage) bakın.| 
|**bağlantı** |**Bağlantı** | Bu bağlama için kullanılacak Depolama bağlantı dizesini içeren bir uygulama ayarının adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, burada yalnızca adın geri kalanını belirtebilirsiniz. Örneğin, "MyStorage" olarak ayarlarsanız, `connection` İşlevler çalışma zamanı "MyStorage" adlı bir uygulama ayarı arar. Boş bırakırsanız, `connection` İşlevler çalışma zamanı, uygulama ayarında adı `AzureWebJobsStorage`verilen varsayılan Depolama bağlantı dizesini kullanır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

# <a name="c"></a>[C #](#tab/csharp)

Bir yöntem `ICollector<T> paramName` parametresi kullanarak veya `IAsyncCollector<T> paramName` nerede `T` `PartitionKey` ve `RowKey` özellikleri içerir çıktı tablosu varlık erişin. Bu özellikler genellikle uygulama `ITableEntity` veya devralma `TableEntity`eşlik ediyor.

Alternatif olarak, Azure `CloudTable` Depolama SDK'sını kullanarak tabloya yazmak için bir yöntem parametresi kullanabilirsiniz. Bir hata iletisine `CloudTable` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Bir yöntem `ICollector<T> paramName` parametresi kullanarak veya `IAsyncCollector<T> paramName` nerede `T` `PartitionKey` ve `RowKey` özellikleri içerir çıktı tablosu varlık erişin. Bu özellikler genellikle uygulama `ITableEntity` veya devralma `TableEntity`eşlik ediyor. Değer `paramName` `name` *function.json*özelliği belirtilir.

Alternatif olarak, Azure `CloudTable` Depolama SDK'sını kullanarak tabloya yazmak için bir yöntem parametresi kullanabilirsiniz. Bir hata iletisine `CloudTable` bağlanmaya ve bir hata iletisi almaya çalışırsanız, [doğru Depolama SDK sürümüne](#azure-storage-sdk-version-in-functions-1x)bir başvurunuz olduğundan emin olun.

# <a name="javascript"></a>[Javascript](#tab/javascript)

*function.json* `name` özelliğinde `<name>` belirtilen değerin nerede olduğunu kullanarak `context.bindings.<name>` çıktı olayına erişin.

# <a name="python"></a>[Python](#tab/python)

Bir işlevden Tablo depolama satırı iletisi çıkarmak için iki seçenek vardır:

- **İade değeri**: `name` *function.json'daki* özelliği `$return`. Bu yapılandırmaile, işlevin geri dönüş değeri tablo depolama satırı olarak kalıcıdır.

- **Imperative**: [Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python) türü olarak bildirilen parametrenin [ayarlanan](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none) yöntemine bir değer geçirin. Geçirilen değer `set` Olay Hub iletisi olarak kalıcıdır.

# <a name="java"></a>[Java](#tab/java)

[TabloDepolama Çıktısı](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.tableoutput?view=azure-java-stablet) ek açıklamasını kullanarak bir işlevden Tablo depolama satırı çıkarmak için iki seçenek vardır:

- **İade değeri**: Ek açıklama işlevin kendisine uygulanarak, işlevin geri dönüş değeri Tablo depolama satırı olarak devam eder.

- **Imperative**: İleti değerini açıkça ayarlamak için, ek açıklamayı, özelliklerive [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding) `RowKey` özellikleri `T` içeren `PartitionKey` türün belirli bir parametresine uygulayın. Bu özellikler genellikle uygulama `ITableEntity` veya devralma `TableEntity`eşlik ediyor.

---

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve iade kodları

| Bağlama | Başvuru |
|---|---|
| Tablo | [Tablo Hata Kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| Blob, Tablo, Sıra | [Depolama Hata Kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, Tablo, Sıra | [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
