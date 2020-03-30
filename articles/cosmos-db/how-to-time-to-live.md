---
title: Azure Cosmos DB'de Yaşama Süresini Yapılandırma ve Yönetme
description: Azure Cosmos DB'deki bir kapsayıcı da ve bir öğede yaşamak için zamanı nasıl yapılandırıp yöneteceklerinizi öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/27/2020
ms.author: anfeldma
ms.openlocfilehash: 72653a3b28181316a2bf7dd7e73f2685c3afcf73
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384271"
---
# <a name="configure-time-to-live-in-azure-cosmos-db"></a>Azure Cosmos DB'de yaşamak için zamanı yapılandırma

Azure Cosmos DB'de, Yaşam Süresi'ni (TTL) kapsayıcı düzeyinde yapılandırmayı seçebilir veya kapsayıcıyı ayarladıktan sonra öğe düzeyinde geçersiz kılabilirsiniz. Azure portalını veya dile özel SDK'ları kullanarak TTL'yi bir kapsayıcı için yapılandırabilirsiniz. Madde düzeyi TTL geçersiz kılar SDKs kullanılarak yapılandırılabilir.

## <a name="enable-time-to-live-on-a-container-using-azure-portal"></a>Azure portalLarını kullanarak bir kapsayıcıda yaşama süresini etkinleştirme

Son kullanma tarihi olmayan bir kapta yaşamak için zaman sağlamak için aşağıdaki adımları kullanın. TTL'nin madde düzeyinde geçersiz kılınmasını sağlamak için bunu etkinleştirin. TTL'yi saniyeler için sıfır olmayan bir değer girerek de ayarlayabilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. Yeni bir Azure Cosmos hesabı oluşturun veya varolan bir hesabı seçin.

3. Veri **Gezgini** bölmesini açın.

4. Varolan bir kapsayıcı seçin, genişletin ve aşağıdaki değerleri değiştirin:

   * Ayarlar **& Ölçekle** penceresini açın.
   * **Ayar** altında bulmak, **Zaman Yaşamak için**.
   * **On 'u seçin (varsayılan yok)** veya **A'yı** seçin ve TTL değeri ayarlayın
   * Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

   ![Azure portalında yaşamak için Zamanı Yapılandırma](./media/how-to-time-to-live/how-to-time-to-live-portal.png)

* DefaultTimeToLive boş aldığında, Yaşam Süreniz Kapalıdır
* DefaultTimeToLive -1 olduğunda, Yaşam Süreniz Ayarı Ayarı Ayarı (Varsayılan yok)
* DefaultTimeToLive başka bir Int değerine (0 hariç) sahipolduğunda, Yaşam Süreniz Ayarı Ayarı

## <a name="enable-time-to-live-on-a-container-using-azure-cli-or-powershell"></a>Azure CLI veya PowerShell kullanarak bir kapsayıcıda yaşama süresini etkinleştirme

Bir kapsayıcıda TTL oluşturmak veya etkinleştirmek için bkz.

* [Azure CLI kullanarak TTL içeren bir kapsayıcı oluşturma](manage-with-cli.md#create-a-container-with-ttl)
* [PowerShell kullanarak TTL içeren bir kapsayıcı oluşturma](manage-with-powershell.md#create-container-unique-key-ttl)

## <a name="enable-time-to-live-on-a-container-using-sdk"></a>SDK kullanarak bir kapta yaşamak için zaman etkinleştirme

### <a name="net-sdk-v2-microsoftazuredocumentdb"></a><a id="dotnet-enable-noexpiry"></a>.NET SDK V2 (Microsoft.Azure.DocumentDB)

```csharp
// Create a new container with TTL enabled and without any expiration value
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = -1; //(never expire by default)

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition);
```

### <a name="net-sdk-v3-microsoftazurecosmos"></a><a id="dotnet-enable-noexpiry"></a>.NET SDK V3 (Microsoft.Azure.Cosmos)

```csharp
// Create a new container with TTL enabled and without any expiration value
await client.GetDatabase("database").CreateContainerAsync(new ContainerProperties
{
    Id = "container",
    PartitionKeyPath = "/myPartitionKey",
    DefaultTimeToLive = -1 //(never expire by default)
});
```

### <a name="java-sdk-v4-maven-comazureazure-cosmos"></a><a id="java4-enable-noexpiry"></a>Java SDK V4 (Maven com.azure::azure-cosmos)

```java
CosmosAsyncContainer container;

// Create a new container with TTL enabled and without any expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.setDefaultTimeToLiveInSeconds(-1);
container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

### <a name="java-sdk-v3-maven-commicrosoftazureazure-cosmos"></a><a id="java3-enable-noexpiry"></a>Java SDK V3 (Maven com.microsoft.azure::azure-cosmos)

```java
CosmosContainer container;

// Create a new container with TTL enabled and without any expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.defaultTimeToLive(-1);
container = database.createContainerIfNotExists(containerProperties, 400).block().container();
```

## <a name="set-time-to-live-on-a-container-using-sdk"></a>SDK kullanarak bir kapta yaşamak için zaman ayarlama

Bir kapsayıcıda yaşama süresini ayarlamak için, saniye cinsinden zaman dilimini gösteren sıfır olmayan pozitif bir sayı sağlamanız gerekir. Yapılandırılan TTL değerine bağlı olarak, maddenin `_ts` son değiştirilen zaman damgası sonra kapsayıcıdaki tüm öğeler silinir.

### <a name="net-sdk-v2-microsoftazuredocumentdb"></a><a id="dotnet-enable-withexpiry"></a>.NET SDK V2 (Microsoft.Azure.DocumentDB)

```csharp
// Create a new container with TTL enabled and a 90 day expiration
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24 // expire all documents after 90 days

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition;
```

### <a name="net-sdk-v3-microsoftazurecosmos"></a><a id="dotnet-enable-withexpiry"></a>.NET SDK V3 (Microsoft.Azure.Cosmos)

```csharp
// Create a new container with TTL enabled and a 90 day expiration
await client.GetDatabase("database").CreateContainerAsync(new ContainerProperties
{
    Id = "container",
    PartitionKeyPath = "/myPartitionKey",
    DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
});
```

### <a name="nodejs-sdk"></a><a id="nodejs-enable-withexpiry"></a>NodeJS SDK

```javascript
const containerDefinition = {
          id: "sample container1",
        };

async function createcontainerWithTTL(db: Database, containerDefinition: ContainerDefinition, collId: any, defaultTtl: number) {
      containerDefinition.id = collId;
      containerDefinition.defaultTtl = defaultTtl;
      await db.containers.create(containerDefinition);
}
```

### <a name="java-sdk-v4-maven-comazureazure-cosmos"></a><a id="java4-enable-defaultexpiry"></a>Java SDK V4 (Maven com.azure::azure-cosmos)

```java
CosmosAsyncContainer container;

// Create a new container with TTL enabled with default expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.setDefaultTimeToLiveInSeconds(90 * 60 * 60 * 24);
container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

### <a name="java-sdk-v3-maven-commicrosoftazureazure-cosmos"></a><a id="java3-enable-defaultexpiry"></a>Java SDK V3 (Maven com.microsoft.azure::azure-cosmos)

```java
CosmosContainer container;

// Create a new container with TTL enabled with default expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.defaultTimeToLive(90 * 60 * 60 * 24);
container = database.createContainerIfNotExists(containerProperties, 400).block().container();
```

## <a name="set-time-to-live-on-an-item"></a>Bir öğede yaşamak için zamanı ayarlama

Bir kapsayıcıda yaşamak için varsayılan bir süre ayarlamaya ek olarak, bir öğe için yaşamak için bir zaman ayarlayabilirsiniz. Madde düzeyinde yaşamak için zaman ayarlama, o kapsayıcıdaki maddenin varsayılan TTL'sini geçersiz kılar.

* TTL'yi bir öğeüzerinde ayarlamak için, maddenin `_ts`son değiştirilen zaman damgası sonrasına saniyecinsinden süreyi gösteren sıfır olmayan pozitif bir sayı sağlamanız gerekir.

* Öğenin TTL alanı yoksa, varsayılan olarak, kapsayıcıya ayarlanan TTL öğesine uygulanır.

* TTL kapsayıcı düzeyinde devre dışı bırakılırsa, kapsayıcıda TTL yeniden etkinleştirilene kadar maddedeki TTL alanı yoksayılır.

### <a name="azure-portal"></a><a id="portal-set-ttl-item"></a>Azure portalında

Bir öğede yaşama süresini etkinleştirmek için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. Yeni bir Azure Cosmos hesabı oluşturun veya varolan bir hesabı seçin.

3. Veri **Gezgini** bölmesini açın.

4. Varolan bir kapsayıcı seçin, genişletin ve aşağıdaki değerleri değiştirin:

   * Ayarlar **& Ölçekle** penceresini açın.
   * **Ayar** altında bulmak, **Zaman Yaşamak için**.
   * **On 'u seçin (varsayılan yok)** veya **A'yı** seçin ve TTL değeri ayarlayın. 
   * Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

5. Daha sonra yaşamak için zaman ayarlamak istediğiniz öğeye `ttl` gidin, özelliği ekleyin ve **Güncelleştir'i**seçin. 

   ```json
   {
    "id": "1",
    "_rid": "Jic9ANWdO-EFAAAAAAAAAA==",
    "_self": "dbs/Jic9AA==/colls/Jic9ANWdO-E=/docs/Jic9ANWdO-EFAAAAAAAAAA==/",
    "_etag": "\"0d00b23f-0000-0000-0000-5c7712e80000\"",
    "_attachments": "attachments/",
    "ttl": 10,
    "_ts": 1551307496
   }
   ```

### <a name="net-sdk-any"></a><a id="dotnet-set-ttl-item"></a>.NET SDK (herhangi bir)

```csharp
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName="cid")]
    public string CustomerId { get; set; }
    // used to set expiration policy
    [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
    public int? ttl { get; set; }

    //...
}
// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder
{
    Id = "SO05",
    CustomerId = "CO18009186470",
    ttl = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days
};
```

### <a name="nodejs-sdk"></a><a id="nodejs-set-ttl-item"></a>NodeJS SDK

```javascript
const itemDefinition = {
          id: "doc",
          name: "sample Item",
          key: "value",
          ttl: 2
        };
```

### <a name="java-sdk-v4-maven-comazureazure-cosmos"></a><a id="java4-enable-itemexpiry"></a>Java SDK V4 (Maven com.azure::azure-cosmos)

```java
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    private String id;
    private String customerId;
    private Integer ttl;

    public SalesOrder(String id, String customerId, Integer ttl) {
        this.id = id;
        this.customerId = customerId;
        this.ttl = ttl;
    }

    public String getId() {return this.id;}
    public void setId(String new_id) {this.id = new_id;}
    public String getCustomerId() {return this.customerId;}
    public void setCustomerId(String new_cid) {this.customerId = new_cid;}
    public Integer getTtl() {return this.ttl;}
    public void setTtl(Integer new_ttl) {this.ttl = new_ttl;}

    //...
}

// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder(
    "SO05",
    "CO18009186470",
    60 * 60 * 24 * 30  // Expire sales orders in 30 days
);

```

### <a name="java-sdk-v3-maven-commicrosoftazureazure-cosmos"></a><a id="java3-enable-itemexpiry"></a>Java SDK V3 (Maven com.microsoft.azure::azure-cosmos)

```java
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    private String id;
    private String customerId;
    private Integer ttl;

    public SalesOrder(String id, String customerId, Integer ttl) {
        this.id = id;
        this.customerId = customerId;
        this.ttl = ttl;
    }

    public String id() {return this.id;}
    public void id(String new_id) {this.id = new_id;}
    public String customerId() {return this.customerId;}
    public void customerId(String new_cid) {this.customerId = new_cid;}
    public Integer ttl() {return this.ttl;}
    public void ttl(Integer new_ttl) {this.ttl = new_ttl;}

    //...
}

// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder(
    "SO05",
    "CO18009186470",
    60 * 60 * 24 * 30  // Expire sales orders in 30 days
);

```

## <a name="reset-time-to-live"></a>Yaşamak için sıfırlama zamanı

Öğeüzerinde bir yazma veya güncelleştirme işlemi gerçekleştirerek bir öğede yaşama süresini sıfırlayabilirsiniz. Yazma veya güncelleştirme işlemi `_ts` geçerli saati ayarlar ve maddenin süresinin dolması için TTL yeniden başlar. Bir öğenin TTL'sini değiştirmek isterseniz, alanı diğer alanları güncellediğiniz gibi güncelleştirebilirsiniz.

### <a name="net-sdk-v2-microsoftazuredocumentdb"></a><a id="dotnet-extend-ttl-item"></a>.NET SDK V2 (Microsoft.Azure.DocumentDB)

```csharp
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.ttl = 60 * 30 * 30; // update time to live
response = await client.ReplaceDocumentAsync(readDocument);
```

### <a name="net-sdk-v3-microsoftazurecosmos"></a><a id="dotnet-extend-ttl-item"></a>.NET SDK V3 (Microsoft.Azure.Cosmos)

```csharp
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
ItemResponse<SalesOrder> itemResponse = await client.GetContainer("database", "container").ReadItemAsync<SalesOrder>("SO05", new PartitionKey("CO18009186470"));

itemResponse.Resource.ttl = 60 * 30 * 30; // update time to live
await client.GetContainer("database", "container").ReplaceItemAsync(itemResponse.Resource, "SO05");
```

### <a name="java-sdk-v4-maven-comazureazure-cosmos"></a><a id="java4-enable-modifyitemexpiry"></a>Java SDK V4 (Maven com.azure::azure-cosmos)

```java
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
CosmosAsyncItemResponse<SalesOrder> itemResponse = container.readItem("SO05", new PartitionKey("CO18009186470"), SalesOrder.class)
        .flatMap(readResponse -> {
            SalesOrder salesOrder = readResponse.getItem();
            salesOrder.setTtl(60 * 30 * 30);
            return container.createItem(salesOrder);
}).block();
```

### <a name="java-sdk-v3-maven-commicrosoftazureazure-cosmos"></a><a id="java3-enable-modifyitemexpiry"></a>Java SDK V3 (Maven com.microsoft.azure::azure-cosmos)

```java
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
container.getItem("SO05", new PartitionKey("CO18009186470")).read()
        .flatMap(readResponse -> {
            SalesOrder salesOrder = null;
            try {
                salesOrder = readResponse.properties().getObject(SalesOrder.class);
            } catch (Exception err) {

            }
            salesOrder.ttl(60 * 30 * 30);
            return container.createItem(salesOrder);
}).block();
```

## <a name="turn-off-time-to-live"></a>Yaşamak için zamanı kapatın

Bir öğede yaşam süresi ayarlanmışsa ve artık bu öğenin süresinin dolmasını istemiyorsanız, öğeyi alabilir, TTL alanını kaldırabilir ve sunucudaki öğeyi değiştirebilirsiniz. TTL alanı maddeden kaldırıldığında, kapsayıcıya atanan varsayılan TTL değeri maddeye uygulanır. Bir maddenin süresinin dolmasını önlemek ve ttl değerini kapsayıcıdan devralmamak için TTL değerini -1 olarak ayarlayın.

### <a name="net-sdk-v2-microsoftazuredocumentdb"></a><a id="dotnet-turn-off-ttl-item"></a>.NET SDK V2 (Microsoft.Azure.DocumentDB)

```csharp
// This examples leverages the Sales Order class above.
// Read a document, turn off its override TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.ttl = null; // inherit the default TTL of the container

response = await client.ReplaceDocumentAsync(readDocument);
```

### <a name="net-sdk-v3-microsoftazurecosmos"></a><a id="dotnet-turn-off-ttl-item"></a>.NET SDK V3 (Microsoft.Azure.Cosmos)

```csharp
// This examples leverages the Sales Order class above.
// Read a document, turn off its override TTL, save it.
ItemResponse<SalesOrder> itemResponse = await client.GetContainer("database", "container").ReadItemAsync<SalesOrder>("SO05", new PartitionKey("CO18009186470"));

itemResponse.Resource.ttl = null; // inherit the default TTL of the container
await client.GetContainer("database", "container").ReplaceItemAsync(itemResponse.Resource, "SO05");
```

### <a name="java-sdk-v4-maven-comazureazure-cosmos"></a><a id="java4-enable-itemdefaultexpiry"></a>Java SDK V4 (Maven com.azure::azure-cosmos)

```java
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
CosmosAsyncItemResponse<SalesOrder> itemResponse = container.readItem("SO05", new PartitionKey("CO18009186470"), SalesOrder.class)
        .flatMap(readResponse -> {
            SalesOrder salesOrder = readResponse.getItem();
            salesOrder.setTtl(null);
            return container.createItem(salesOrder);
}).block();
```

### <a name="java-sdk-v3-maven-commicrosoftazureazure-cosmos"></a><a id="java3-enable-itemdefaultexpiry"></a>Java SDK V3 (Maven com.microsoft.azure::azure-cosmos)

```java
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
container.getItem("SO05", new PartitionKey("CO18009186470")).read()
        .flatMap(readResponse -> {
            SalesOrder salesOrder = null;
            try {
                salesOrder = readResponse.properties().getObject(SalesOrder.class);
            } catch (Exception err) {

            }
            salesOrder.ttl(null);
            return container.createItem(salesOrder);
}).block();
```

## <a name="disable-time-to-live"></a>Yaşama süresini devre dışı

Bir kapsayıcıda yaşamak için zamanı devre dışı bırakmak ve arka plan `DefaultTimeToLive` işleminin süresi dolmuş öğeleri denetlemesini durdurmak için, kapsayıcıdaki özellik silinmelidir. Bu özelliği silerken -1'e ayarlamak farklıdır. -1 olarak ayarladığınızda, kapsayıcıya eklenen yeni öğeler sonsuza kadar yaşar, ancak bu değeri kapsayıcıdaki belirli öğeler üzerinde geçersiz kılabilirsiniz. TTL özelliğini kapsayıcıdan kaldırdığınızda, önceki varsayılan TTL değerini açıkça geçersiz kılmış olsalar bile öğelerin süresi asla dolmaz.

### <a name="net-sdk-v2-microsoftazuredocumentdb"></a><a id="dotnet-disable-ttl"></a>.NET SDK V2 (Microsoft.Azure.DocumentDB)

```csharp
// Get the container, update DefaultTimeToLive to null
DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
// Disable TTL
collection.DefaultTimeToLive = null;
await client.ReplaceDocumentCollectionAsync(collection);
```

### <a name="net-sdk-v3-microsoftazurecosmos"></a><a id="dotnet-disable-ttl"></a>.NET SDK V3 (Microsoft.Azure.Cosmos)

```csharp
// Get the container, update DefaultTimeToLive to null
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Disable TTL
containerResponse.Resource.DefaultTimeToLive = null;
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

### <a name="java-sdk-v4-maven-comazureazure-cosmos"></a><a id="java4-enable-disableexpiry"></a>Java SDK V4 (Maven com.azure::azure-cosmos)

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
// Disable TTL
containerProperties.setDefaultTimeToLiveInSeconds(null);
// Update container settings
container.replace(containerProperties).block();
```

### <a name="java-sdk-v3-maven-commicrosoftazureazure-cosmos"></a><a id="java3-enable-disableexpiry"></a>Java SDK V3 (Maven com.microsoft.azure::azure-cosmos)

```java
CosmosContainer container;

// Create a new container with TTL enabled and without any expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
// Disable TTL
containerProperties.defaultTimeToLive(null);
// Update container settings
container = database.createContainerIfNotExists(containerProperties, 400).block().container();
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalede yaşamak için zaman hakkında daha fazla bilgi edinin:

* [Yaşam süresi](time-to-live.md)
