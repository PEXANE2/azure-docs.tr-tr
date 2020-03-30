---
title: Azure Cosmos DB SDK'larında depolanmış yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri kaydedin ve kullanın
description: Azure Cosmos DB SDK'ları kullanarak depolanan yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri nasıl kaydedebilirsiniz ve çağırın
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: mjbrown
ms.openlocfilehash: 00740bc2255962089789682e3227ce414fd0ce64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77582509"
---
# <a name="how-to-register-and-use-stored-procedures-triggers-and-user-defined-functions-in-azure-cosmos-db"></a>Azure Cosmos DB'de saklı yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri kaydetme ve kullanma

Azure Cosmos DB'de SQL API JavaScript dilinde yazılmış saklı yordamların, tetikleyicilerin ve kullanıcı tanımlı işlevlerin (UDF) kaydedilmesini ve çağrılmasını destekler. SQL API [.NET](sql-api-sdk-dotnet.md), [.NET Core](sql-api-sdk-dotnet-core.md), [Java](sql-api-sdk-java.md), [JavaScript](sql-api-sdk-node.md), [Node.js](sql-api-sdk-node.md)veya [Python](sql-api-sdk-python.md) SDKs'yi kaydolmak ve depolanan yordamları çağırmak için kullanabilirsiniz. Bir veya daha fazla depolanmış yordam, tetikleyici ve kullanıcı tanımlı işlevi tanımladıktan sonra, Veri Gezgini'ni kullanarak [bunları Azure portalında](https://portal.azure.com/) yükleyebilir ve görüntüleyebilirsiniz.

## <a name="how-to-run-stored-procedures"></a><a id="stored-procedures"></a>Depolanan yordamlar nasıl çalıştırılalır?

Saklanan yordamlar JavaScript kullanılarak yazılır. Azure Cosmos kapsayıcısı içinde öğeler oluşturabilir, güncelleyebilir, okuyabilir, sorgulayabilir ve silebilir. Azure Cosmos DB'de depolanan yordamların nasıl yazılması hakkında daha fazla bilgi için Azure [Cosmos DB makalesinde depolanmış yordamları nasıl yazınız](how-to-write-stored-procedures-triggers-udfs.md#stored-procedures) bilgi sini öğrenin.

Aşağıdaki örnekler, Azure Cosmos DB SDK'larını kullanarak depolanan bir yordamı nasıl kaydedeceğimiz ve çağırılanın gösterilmektedir. Bu depolanan yordam için kaynak olarak bir belge `spCreateToDoItem.js` [oluştur'a](how-to-write-stored-procedures-triggers-udfs.md#create-an-item) bakın .

> [!NOTE]
> Bölümlenmiş kapsayıcılar için, depolanan yordamı yürürken, istek seçeneklerinde bir bölüm anahtarı değeri sağlanmalıdır. Depolanan yordamlar her zaman bir bölüm anahtarına kapsamlıdır. Farklı bir bölüm anahtar değerine sahip öğeler depolanan yordam tarafından görünür olmayacaktır. Bu da tetikleyiciler için de uygulanır.

### <a name="stored-procedures---net-sdk-v2"></a>Saklanan prosedürler - .NET SDK V2

Aşağıdaki örnekte,.NET SDK V2'yi kullanarak depolanan bir yordamın nasıl kaydedilen

```csharp
string storedProcedureId = "spCreateToDoItem";
StoredProcedure newStoredProcedure = new StoredProcedure
   {
       Id = storedProcedureId,
       Body = File.ReadAllText($@"..\js\{storedProcedureId}.js")
   };
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
var response = await client.CreateStoredProcedureAsync(containerUri, newStoredProcedure);
StoredProcedure createdStoredProcedure = response.Resource;
```

Aşağıdaki kod, .NET SDK V2 kullanarak depolanan bir yordamı nasıl çağırılanın gösterir:

```csharp
dynamic newItem = new
{
    category = "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
};

Uri uri = UriFactory.CreateStoredProcedureUri("myDatabase", "myContainer", "spCreateToDoItem");
RequestOptions options = new RequestOptions { PartitionKey = new PartitionKey("Personal") };
var result = await client.ExecuteStoredProcedureAsync<string>(uri, options, newItem);
```

### <a name="stored-procedures---net-sdk-v3"></a>Saklanan prosedürler - .NET SDK V3

Aşağıdaki örnekte,.NET SDK V3'ü kullanarak depolanan bir yordamın nasıl kaydedilen

```csharp
StoredProcedureResponse storedProcedureResponse = await client.GetContainer("database", "container").Scripts.CreateStoredProcedureAsync(new StoredProcedureProperties
{
    Id = "spCreateToDoItem",
    Body = File.ReadAllText(@"..\js\spCreateToDoItem.js")
});
```

Aşağıdaki kod, .NET SDK V3'ü kullanarak depolanan bir yordamın nasıl çağrıldığını gösterir:

```csharp
dynamic[] newItems = new dynamic[]
{
    new {
        category = "Personal",
        name = "Groceries",
        description = "Pick up strawberries",
        isComplete = false
    }
};

var result = await client.GetContainer("database", "container").Scripts.ExecuteStoredProcedureAsync<string>("spCreateToDoItem", new PartitionKey("Personal"), newItems);
```

### <a name="stored-procedures---java-sdk"></a>Saklanan prosedürler - Java SDK

Aşağıdaki örnek, Java SDK'yı kullanarak depolanan bir yordamın nasıl kaydedildiğini gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
StoredProcedure newStoredProcedure = new StoredProcedure(
    "{" +
        "  'id':'spCreateToDoItem'," +
        "  'body':" + new String(Files.readAllBytes(Paths.get("..\\js\\spCreateToDoItem.js"))) +
    "}");
//toBlocking() blocks the thread until the operation is complete and is used only for demo.  
StoredProcedure createdStoredProcedure = asyncClient.createStoredProcedure(containerLink, newStoredProcedure, null)
    .toBlocking().single().getResource();
```

Aşağıdaki kod, Java SDK'yı kullanarak depolanan bir yordamın nasıl çağrıldığını gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String sprocLink = String.format("%s/sprocs/%s", containerLink, "spCreateToDoItem");
final CountDownLatch successfulCompletionLatch = new CountDownLatch(1);

class ToDoItem {
    public String category;
    public String name;
    public String description;
    public boolean isComplete;
}

ToDoItem newItem = new ToDoItem();
newItem.category = "Personal";
newItem.name = "Groceries";
newItem.description = "Pick up strawberries";
newItem.isComplete = false;

RequestOptions requestOptions = new RequestOptions();
requestOptions.setPartitionKey(new PartitionKey("Personal"));

Object[] storedProcedureArgs = new Object[] { newItem };
asyncClient.executeStoredProcedure(sprocLink, requestOptions, storedProcedureArgs)
    .subscribe(storedProcedureResponse -> {
        String storedProcResultAsString = storedProcedureResponse.getResponseAsString();
        successfulCompletionLatch.countDown();
        System.out.println(storedProcedureResponse.getActivityId());
    }, error -> {
        successfulCompletionLatch.countDown();
        System.err.println("an error occurred while executing the stored procedure: actual cause: "
                + error.getMessage());
    });

successfulCompletionLatch.await();
```

### <a name="stored-procedures---javascript-sdk"></a>Saklanan prosedürler - JavaScript SDK

Aşağıdaki örnek, JavaScript SDK'yı kullanarak depolanan bir yordamın nasıl kaydedildiğini gösterir

```javascript
const container = client.database("myDatabase").container("myContainer");
const sprocId = "spCreateToDoItem";
await container.scripts.storedProcedures.create({
    id: sprocId,
    body: require(`../js/${sprocId}`)
});
```

Aşağıdaki kod, JavaScript SDK'yı kullanarak depolanan bir yordamın nasıl çağrıldığını gösterir:

```javascript
const newItem = [{
    category: "Personal",
    name: "Groceries",
    description: "Pick up strawberries",
    isComplete: false
}];
const container = client.database("myDatabase").container("myContainer");
const sprocId = "spCreateToDoItem";
const {body: result} = await container.scripts.storedProcedure(sprocId).execute(newItem, {partitionKey: newItem[0].category});
```

### <a name="stored-procedures---python-sdk"></a>Depolanan yordamlar - Python SDK

Aşağıdaki örnek, Python SDK'yı kullanarak depolanan bir yordamın nasıl kaydedildiğini gösterir

```python
with open('../js/spCreateToDoItem.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
sproc_definition = {
    'id': 'spCreateToDoItem',
    'serverScript': file_contents,
}
sproc = client.CreateStoredProcedure(container_link, sproc_definition)
```

Aşağıdaki kod Python SDK kullanarak depolanan bir yordamı nasıl çağırın gösterir

```python
sproc_link = 'dbs/myDatabase/colls/myContainer/sprocs/spCreateToDoItem'
new_item = [{
    'category':'Personal',
    'name':'Groceries',
    'description':'Pick up strawberries',
    'isComplete': False
}]
client.ExecuteStoredProcedure(sproc_link, new_item, {'partitionKey': 'Personal'}
```

## <a name="how-to-run-pre-triggers"></a><a id="pre-triggers"></a>Ön tetikleyiciler çalıştırma

Aşağıdaki örnekler, Azure Cosmos DB SDK'ları kullanarak ön tetikleyiciyi nasıl kaydedip çağırılamayı gösterir. Bu ön [tetikleyicinin](how-to-write-stored-procedures-triggers-udfs.md#pre-triggers) kaynağı olarak `trgPreValidateToDoItemTimestamp.js`kaydedilir gibi Ön tetikleyici örneğine bakın.

Yürütüldüğünde, Bir Liste nesnesinde tetikleyicinin adını belirterek `PreTriggerInclude` ve sonra geçirerek İstek Seçenekleri nesnesinde ön tetikleyiciler geçirilir.

> [!NOTE]
> Tetikleyicinin adı Liste olarak geçirilir olsa bile, yine de işlem başına yalnızca bir tetikleyici çalıştırabilirsiniz.

### <a name="pre-triggers---net-sdk-v2"></a>Ön tetikleyiciler - .NET SDK V2

Aşağıdaki kod, .NET SDK V2 kullanarak bir ön tetikleyicinin nasıl kaydedildiğini gösterir:

```csharp
string triggerId = "trgPreValidateToDoItemTimestamp";
Trigger trigger = new Trigger
{
    Id =  triggerId,
    Body = File.ReadAllText($@"..\js\{triggerId}.js"),
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Pre
};
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateTriggerAsync(containerUri, trigger);
```

Aşağıdaki kod, .NET SDK V2 kullanarak ön tetikleyicinin nasıl çağrıldığını gösterir:

```csharp
dynamic newItem = new
{
    category = "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
};

Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
RequestOptions requestOptions = new RequestOptions { PreTriggerInclude = new List<string> { "trgPreValidateToDoItemTimestamp" } };
await client.CreateDocumentAsync(containerUri, newItem, requestOptions);
```

### <a name="pre-triggers---net-sdk-v3"></a>Ön tetikleyiciler - .NET SDK V3

Aşağıdaki kod, .NET SDK V3 kullanarak bir ön tetikleyicinin nasıl kaydedildiğini gösterir:

```csharp
await client.GetContainer("database", "container").Scripts.CreateTriggerAsync(new TriggerProperties
{
    Id = "trgPreValidateToDoItemTimestamp",
    Body = File.ReadAllText("@..\js\trgPreValidateToDoItemTimestamp.js"),
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Pre
});
```

Aşağıdaki kod, .NET SDK V3'ü kullanarak ön tetikleyicinin nasıl çağrıldığını gösterir:

```csharp
dynamic newItem = new
{
    category = "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
};

await client.GetContainer("database", "container").CreateItemAsync(newItem, null, new ItemRequestOptions { PreTriggers = new List<string> { "trgPreValidateToDoItemTimestamp" } });
```

### <a name="pre-triggers---java-sdk"></a>Ön tetikleyiciler - Java SDK

Aşağıdaki kod, Java SDK'yı kullanarak bir ön tetikleyicinin nasıl kaydedildiğini gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String triggerId = "trgPreValidateToDoItemTimestamp";
Trigger trigger = new Trigger();
trigger.setId(triggerId);
trigger.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", triggerId)));
trigger.setTriggerOperation(TriggerOperation.Create);
trigger.setTriggerType(TriggerType.Pre);
//toBlocking() blocks the thread until the operation is complete and is used only for demo. 
Trigger createdTrigger = asyncClient.createTrigger(containerLink, trigger, new RequestOptions()).toBlocking().single().getResource();
```

Aşağıdaki kod, Java SDK'yı kullanarak ön tetikleyicinin nasıl çağrıldığını gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
    Document item = new Document("{ "
            + "\"category\": \"Personal\", "
            + "\"name\": \"Groceries\", "
            + "\"description\": \"Pick up strawberries\", "
            + "\"isComplete\": false, "
            + "}"
            );
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPreTriggerInclude(Arrays.asList("trgPreValidateToDoItemTimestamp"));
//toBlocking() blocks the thread until the operation is complete and is used only for demo. 
asyncClient.createDocument(containerLink, item, requestOptions, false).toBlocking();
```

### <a name="pre-triggers---javascript-sdk"></a>Ön tetikleyiciler - JavaScript SDK

Aşağıdaki kod, JavaScript SDK'yı kullanarak ön tetikleyicinin nasıl kaydedildiğini gösterir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPreValidateToDoItemTimestamp";
await container.triggers.create({
    id: triggerId,
    body: require(`../js/${triggerId}`),
    triggerOperation: "create",
    triggerType: "pre"
});
```

Aşağıdaki kod, JavaScript SDK'yı kullanarak ön tetikleyicinin nasıl çağrıldığını gösterir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPreValidateToDoItemTimestamp";
await container.items.create({
    category: "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
}, {preTriggerInclude: [triggerId]});
```

### <a name="pre-triggers---python-sdk"></a>Ön tetikleyiciler - Python SDK

Aşağıdaki kod Python SDK kullanarak bir ön tetikleyici nin nasıl kaydedildiğini gösterir:

```python
with open('../js/trgPreValidateToDoItemTimestamp.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
trigger_definition = {
    'id': 'trgPreValidateToDoItemTimestamp',
    'serverScript': file_contents,
    'triggerType': documents.TriggerType.Pre,
    'triggerOperation': documents.TriggerOperation.Create
}
trigger = client.CreateTrigger(container_link, trigger_definition)
```

Aşağıdaki kod Python SDK kullanarak bir ön tetikleyici nasıl çağırılanın gösterir:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
item = {'category': 'Personal', 'name': 'Groceries',
        'description': 'Pick up strawberries', 'isComplete': False}
client.CreateItem(container_link, item, {
                  'preTriggerInclude': 'trgPreValidateToDoItemTimestamp'})
```

## <a name="how-to-run-post-triggers"></a><a id="post-triggers"></a>Tetikleyici sonrası çalıştırma

Aşağıdaki örnekler, Azure Cosmos DB SDK'larını kullanarak bir tetikleyicinin nasıl kaydedilen olduğunu gösterir. Bu post-trigger için kaynak olarak `trgPostUpdateMetadata.js` [Post-tetikleyici örneğine](how-to-write-stored-procedures-triggers-udfs.md#post-triggers) bakın .

### <a name="post-triggers---net-sdk-v2"></a>Tetikleyicisonrası - .NET SDK V2

Aşağıdaki kod, .NET SDK V2 kullanarak bir post-tetikleyicinin nasıl kaydedildiğini gösterir:

```csharp
string triggerId = "trgPostUpdateMetadata";
Trigger trigger = new Trigger
{
    Id = triggerId,
    Body = File.ReadAllText($@"..\js\{triggerId}.js"),
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Post
};
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateTriggerAsync(containerUri, trigger);
```

Aşağıdaki kod, .NET SDK V2'yi kullanarak bir post-tetikleyicinin nasıl çağrıldığını gösterir:

```csharp
var newItem = { 
    name: "artist_profile_1023",
    artist: "The Band",
    albums: ["Hellujah", "Rotators", "Spinning Top"]
};

RequestOptions options = new RequestOptions { PostTriggerInclude = new List<string> { "trgPostUpdateMetadata" } };
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.createDocumentAsync(containerUri, newItem, options);
```

### <a name="post-triggers---net-sdk-v3"></a>Tetikleyicisonrası - .NET SDK V3

Aşağıdaki kod, .NET SDK V3 kullanarak bir post-tetikleyicinin nasıl kaydedildiğini gösterir:

```csharp
await client.GetContainer("database", "container").Scripts.CreateTriggerAsync(new TriggerProperties
{
    Id = "trgPostUpdateMetadata",
    Body = File.ReadAllText(@"..\js\trgPostUpdateMetadata.js"),
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Post
});
```

Aşağıdaki kod, .NET SDK V3'ü kullanarak bir post-tetikleyicinin nasıl çağrıldığını gösterir:

```csharp
var newItem = { 
    name: "artist_profile_1023",
    artist: "The Band",
    albums: ["Hellujah", "Rotators", "Spinning Top"]
};

await client.GetContainer("database", "container").CreateItemAsync(newItem, null, new ItemRequestOptions { PostTriggers = new List<string> { "trgPostUpdateMetadata" } });
```

### <a name="post-triggers---java-sdk"></a>Tetikleyicisonrası - Java SDK

Aşağıdaki kod, Java SDK'yı kullanarak bir tetikleyicinin nasıl kaydedildiğini gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String triggerId = "trgPostUpdateMetadata";
Trigger trigger = new Trigger();
trigger.setId(triggerId);
trigger.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", triggerId)))));
trigger.setTriggerOperation(TriggerOperation.Create);
trigger.setTriggerType(TriggerType.Post);
Trigger createdTrigger = asyncClient.createTrigger(containerLink, trigger, new RequestOptions()).toBlocking().single().getResource();
```

Aşağıdaki kod, Java SDK'yı kullanarak bir tetikleyicinin nasıl çağırılabildiğini gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
Document item = new Document(String.format("{ "
    + "\"name\": \"artist_profile_1023\", "
    + "\"artist\": \"The Band\", "
    + "\"albums\": [\"Hellujah\", \"Rotators\", \"Spinning Top\"]"
    + "}"
));
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPostTriggerInclude(Arrays.asList("trgPostUpdateMetadata"));
//toBlocking() blocks the thread until the operation is complete, and is used only for demo.
asyncClient.createDocument(containerLink, item, requestOptions, false).toBlocking();
```

### <a name="post-triggers---javascript-sdk"></a>Tetikleyicisonrası - JavaScript SDK

Aşağıdaki kod, JavaScript SDK'yı kullanarak bir tetikleyicinin nasıl kaydedildiğini gösterir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPostUpdateMetadata";
await container.triggers.create({
    id: triggerId,
    body: require(`../js/${triggerId}`),
    triggerOperation: "create",
    triggerType: "post"
});
```

Aşağıdaki kod, JavaScript SDK'yı kullanarak nasıl post-tetikleyici çağırılanın gösterir:

```javascript
const item = {
    name: "artist_profile_1023",
    artist: "The Band",
    albums: ["Hellujah", "Rotators", "Spinning Top"]
};
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPostUpdateMetadata";
await container.items.create(item, {postTriggerInclude: [triggerId]});
```

### <a name="post-triggers---python-sdk"></a>Tetikleyicisonrası - Python SDK

Aşağıdaki kod, Python SDK'yı kullanarak bir post-tetikleyicinin nasıl kaydedildiğini gösterir:

```python
with open('../js/trgPostUpdateMetadata.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
trigger_definition = {
    'id': 'trgPostUpdateMetadata',
    'serverScript': file_contents,
    'triggerType': documents.TriggerType.Post,
    'triggerOperation': documents.TriggerOperation.Create
}
trigger = client.CreateTrigger(container_link, trigger_definition)
```

Aşağıdaki kod, Python SDK'yı kullanarak nasıl post-tetikleyici çağırılanın gösterir:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
item = {'name': 'artist_profile_1023', 'artist': 'The Band',
        'albums': ['Hellujah', 'Rotators', 'Spinning Top']}
client.CreateItem(container_link, item, {
                  'postTriggerInclude': 'trgPostUpdateMetadata'})
```

## <a name="how-to-work-with-user-defined-functions"></a><a id="udfs"></a>Kullanıcı tanımlı işlevlerle çalışma

Aşağıdaki örnekler, Azure Cosmos DB SDK'larını kullanarak kullanıcı tarafından tanımlanan bir işlevin nasıl kaydedilebildiğini gösterir. Bu tetikleyici sonrası kaynağı olarak `udfTax.js`kaydedilir olarak bu Kullanıcı [tanımlı işlev örneğine](how-to-write-stored-procedures-triggers-udfs.md#udfs) bakın.

### <a name="user-defined-functions---net-sdk-v2"></a>Kullanıcı tanımlı fonksiyonlar - .NET SDK V2

Aşağıdaki kod, .NET SDK V2'yi kullanarak kullanıcı tanımlı bir işlevin nasıl kaydedildiğini gösterir:

```csharp
string udfId = "Tax";
var udfTax = new UserDefinedFunction
{
    Id = udfId,
    Body = File.ReadAllText($@"..\js\{udfId}.js")
};

Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateUserDefinedFunctionAsync(containerUri, udfTax);

```

Aşağıdaki kod, .NET SDK V2'yi kullanarak kullanıcı tanımlı bir işlevin nasıl çağrıldığını gösterir:

```csharp
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
var results = client.CreateDocumentQuery<dynamic>(containerUri, "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000"));

foreach (var result in results)
{
    //iterate over results
}
```

### <a name="user-defined-functions---net-sdk-v3"></a>Kullanıcı tanımlı fonksiyonlar - .NET SDK V3

Aşağıdaki kod, .NET SDK V3'ü kullanarak kullanıcı tanımlı bir işlevin nasıl kaydedildiğini gösterir:

```csharp
await client.GetContainer("database", "container").Scripts.CreateUserDefinedFunctionAsync(new UserDefinedFunctionProperties
{
    Id = "Tax",
    Body = File.ReadAllText(@"..\js\Tax.js")
});
```

Aşağıdaki kod, .NET SDK V3'ü kullanarak kullanıcı tanımlı bir işlevin nasıl çağrıldığını gösterir:

```csharp
var iterator = client.GetContainer("database", "container").GetItemQueryIterator<dynamic>("SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000");
while (iterator.HasMoreResults)
{
    var results = await iterator.ReadNextAsync();
    foreach (var result in results)
    {
        //iterate over results
    }
}
```

### <a name="user-defined-functions---java-sdk"></a>Kullanıcı tanımlı fonksiyonlar - Java SDK

Aşağıdaki kod, Java SDK'yı kullanarak kullanıcı tanımlı bir işlevin nasıl kaydedildiğini gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String udfId = "Tax";
UserDefinedFunction udf = new UserDefinedFunction();
udf.setId(udfId);
udf.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", udfId)))));
//toBlocking() blocks the thread until the operation is complete and is used only for demo.
UserDefinedFunction createdUDF = client.createUserDefinedFunction(containerLink, udf, new RequestOptions()).toBlocking().single().getResource();
```

Aşağıdaki kod, Java SDK'yı kullanarak kullanıcı tanımlı bir işlevin nasıl çağrıldığını gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(containerLink, "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000", new FeedOptions());
final CountDownLatch completionLatch = new CountDownLatch(1);
queryObservable.subscribe(
        queryResultPage -> {
            System.out.println("Got a page of query result with " +
                    queryResultPage.getResults().size());
        },
        // terminal error signal
        e -> {
            e.printStackTrace();
            completionLatch.countDown();
        },

        // terminal completion signal
        () -> {
            completionLatch.countDown();
        });
completionLatch.await();
```

### <a name="user-defined-functions---javascript-sdk"></a>Kullanıcı tanımlı fonksiyonlar - JavaScript SDK

Aşağıdaki kod, JavaScript SDK'yı kullanarak kullanıcı tanımlı bir işlevin nasıl kaydedildiğini gösterir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const udfId = "Tax";
await container.userDefinedFunctions.create({
    id: udfId,
    body: require(`../js/${udfId}`)
```

Aşağıdaki kod, JavaScript SDK'yı kullanarak kullanıcı tanımlı bir işlevin nasıl çağrıldığını gösterir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const sql = "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000";
const {result} = await container.items.query(sql).toArray();
```

### <a name="user-defined-functions---python-sdk"></a>Kullanıcı tanımlı fonksiyonlar - Python SDK

Aşağıdaki kod, Python SDK'yı kullanarak kullanıcı tanımlı bir işlevin nasıl kaydedildiğini gösterir:

```python
with open('../js/udfTax.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
udf_definition = {
    'id': 'Tax',
    'serverScript': file_contents,
}
udf = client.CreateUserDefinedFunction(container_link, udf_definition)
```

Aşağıdaki kod, Python SDK'yı kullanarak kullanıcı tanımlı bir işlevin nasıl çağrıldığını gösterir:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
results = list(client.QueryItems(
    container_link, 'SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000'))
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de depolanmış yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri nasıl yazkullanılacağını veya nasıl kullanılacağını öğrenin:

- [Azure Cosmos DB'de Azure Cosmos DB depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevlerle çalışma](stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB'de JavaScript dili tümleşik sorgu API'si ile çalışma](javascript-query-api.md)
- [Azure Cosmos DB'de depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler nasıl yazılır?](how-to-write-stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB'de Javascript Query API kullanarak depolanan yordamlar ve tetikleyiciler nasıl yazılır?](how-to-write-javascript-query-api.md)
