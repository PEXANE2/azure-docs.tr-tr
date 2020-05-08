---
title: Azure Cosmos DB Java SDK v4 (com. Azure. Cosmos) kullanmak için uygulamanızı geçirin
description: Mevcut Java uygulamanızı Azure Cosmos DB, çekirdek (SQL) API 'SI için daha yeni Java SDK 4,0 (com. Azure. Cosmos paketi) ile birlikte kullanarak nasıl yükselteceğinizi öğrenin.
author: anfeldma-ms
ms.author: anfeldma
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2020
ms.reviewer: sngun
ms.openlocfilehash: 929fa936cdb864fd9b84f8feba55ef01ae6fed9c
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82984713"
---
# <a name="migrate-your-application-to-use-the-azure-cosmos-db-java-sdk-v4"></a>Uygulamanızı Java SDK 'Sı v4 Azure Cosmos DB kullanmak için geçirin

> [!IMPORTANT]  
> Bu SDK hakkında daha fazla bilgi için lütfen Azure Cosmos DB Java SDK v4 sürüm notları, [Maven deposu](https://mvnrepository.com/artifact/com.azure/azure-cosmos), Azure Cosmos DB Java SDK v4 [performans Ipuçları](performance-tips-java-sdk-v4-sql.md)ve Azure Cosmos DB Java SDK v4 [sorun giderme kılavuzunu](troubleshoot-java-sdk-v4-sql.md)görüntüleyin.
>

Bu makalede, daha eski bir Java SDK 'sını kullanan mevcut Java uygulamanızın çekirdek (SQL) API 'SI için daha yeni Azure Cosmos DB Java SDK 4,0 Azure Cosmos DB nasıl yükseltileceği açıklanır. Azure Cosmos DB Java SDK v4 `com.azure.cosmos` pakete karşılık gelir. Uygulamanızı aşağıdaki Azure Cosmos DB Java SDK 'larından herhangi birinden geçiriyorsanız, bu belgedeki yönergeleri kullanabilirsiniz: 

* Java SDK 'Sı 2. x. x Eşitle
* Zaman uyumsuz Java SDK 2. x. x
* Java SDK 3. x. x

## <a name="azure-cosmos-db-java-sdks-and-package-mappings"></a>Java SDK 'sının ve paket eşlemelerini Azure Cosmos DB

Aşağıdaki tabloda farklı Azure Cosmos DB Java SDK 'Ları, paket adı ve sürüm bilgileri listelenmektedir:

| Java SDK| Yayın Tarihi | Paketlenmiş API 'Ler   | Maven jar  | Java paket adı  |API Başvurusu   | Release Notes (Sürüm Notları)  |
|-------|------|-----------|-----------|--------------|-------------|---------------------------|
| Zaman uyumsuz 2. x. x  | Haziran 2018    | Async (RxJava)  | `com.microsoft.azure::azure-cosmosdb` | `com.microsoft.azure.cosmosdb.rx` | [API](https://azure.github.io/azure-cosmosdb-java/2.0.0/) | [Sürüm notları](sql-api-sdk-async-java.md) |
| 2. x. x eşitlemesini Eşitle     | Eyl 2018    | Sync   | `com.microsoft.azure::azure-documentdb` | `com.microsoft.azure.cosmosdb` | [API](https://azure.github.io/azure-cosmosdb-java/2.0.0/) | [Sürüm notları](sql-api-sdk-java.md)  |
| 3. x. x    | Temmuz 2019    | Async (reaktör)/Sync  | `com.microsoft.azure::azure-cosmos`  | `com.azure.data.cosmos` | [API](https://azure.github.io/azure-cosmosdb-java/3.0.0/) | - |
| 4.0   | Nisan 2020   | Async (reaktör)/Sync  | `com.azure::azure-cosmos` | `com.azure.cosmos`   | -  | -  |

## <a name="sdk-level-implementation-changes"></a>SDK düzeyinde uygulama değişiklikleri

Aşağıda, farklı SDK 'lar arasındaki önemli uygulama farklılıkları verilmiştir:

### <a name="rxjava-is-replaced-with-reactor-in-azure-cosmos-db-java-sdk-versions-3xx-and-40"></a>RxJava, Azure Cosmos DB Java SDK sürümü 3. x ve 4,0 ' de reaktör ile değiştirilmiştir

Zaman uyumsuz programlama veya reaktif programlama hakkında bilginiz yoksa, zaman uyumsuz programlama ve proje Reaktörine giriş için [reaktör deseninin kılavuzuna](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-pattern-guide.md) bakın. Bu kılavuz, daha önce Java SDK 'Sı 2. x. x veya Azure Cosmos DB Java SDK 3. x. x eşitleme API 'sini kullanıyorsanız Azure Cosmos DB yararlı olabilir.

Azure Cosmos DB zaman uyumsuz Java SDK 'Sı 2. x. x kullandıysanız ve 4,0 SDK 'sına geçirmeyi planlıyorsanız, RxJava kodunu reaktör kullanmak üzere dönüştürmeye yönelik rehberlik için [reaktör vs Rxjava kılavuzuna](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-rxjava-guide.md) bakın.

### <a name="azure-cosmos-db-java-sdk-v4-has-direct-connectivity-mode-in-both-async-and-sync-apis"></a>Azure Cosmos DB Java SDK v4, hem Async hem de Sync API 'Lerinde doğrudan bağlantı moduna sahiptir

Java SDK 'Sı 2. x. x Azure Cosmos DB eşitleme kullanıyorsanız, hem Async hem de Sync API 'Leri için, TCP 'ye (HTTP 'nin aksine) bağlı olan doğrudan bağlantı modunun Azure Cosmos DB Java SDK 4,0 ' de uygulandığını unutmayın.

## <a name="api-level-changes"></a>API düzeyi değişiklikleri

Aşağıda, Java SDK 4. x. x Azure Cosmos DB önceki SDK 'lara kıyasla (Java SDK 3. x. x, zaman uyumsuz Java SDK 2. x. x ve Eşitle Java SDK 'sı 2. x. x) API düzeyi değişiklikler verilmiştir:

![Azure Cosmos DB Java SDK adlandırma kuralları](./media/migrate-java-v4-sdk/java-sdk-naming-conventions.png)

* Java SDK 3. x. x ve 4,0 Azure Cosmos DB istemci kaynaklarına olarak `Cosmos<resourceName>`başvurur. Örneğin `CosmosClient` `CosmosDatabase`,,, `CosmosContainer`. 2. x. x sürümünde, Java SDK 'larının Azure Cosmos DB Tekdüzen adlandırma düzeni yoktur.

* Azure Cosmos DB Java SDK 3. x. x ve 4,0 hem Sync hem de Async API 'Leri sunmaktadır.

  * **Java SDK 4,0** : sınıf adı daha `Async` sonra `Cosmos`EKLENMEDIĞI takdirde tüm sınıflar eşitleme API 'sine aittir.

  * **Java SDK 3. x. x**: sınıf adı daha `Async` sonra `Cosmos`eklenmediği takdirde tüm sınıflar zaman uyumsuz API 'ye aittir.

  * **Async Java SDK 2. x. x**: sınıf adları eşitleme Java SDK 'sı 2. x. x ile benzerdir, ancak ad *zaman uyumsuz*olarak başlar.

### <a name="hierarchical-api-structure"></a>Hiyerarşik API yapısı

Azure Cosmos DB Java SDK 4,0 ve 3. x. x, aşağıdaki 4,0 SDK kod parçacığında gösterildiği gibi, iç içe geçmiş bir biçimde istemcileri, veritabanlarını ve kapsayıcıları düzenleyen hiyerarşik bir API yapısını tanıtmaktadır:

```java
CosmosContainer = client.getDatabase("MyDatabaseName").getContainer("MyContainerName");
```

Azure Cosmos DB Java SDK 'sının 2. x. x sürümünde, kaynak ve belge üzerindeki tüm işlemler istemci örneği aracılığıyla gerçekleştirilir.

### <a name="representing-documents"></a>Belgeleri temsil etme

Azure Cosmos DB Java SDK 4,0 ' de, özel POJO 'lar `JsonNodes` ve Azure Cosmos DB belgeleri okuyup yazmak için iki seçenek vardır.

Java SDK 3. x. x Azure Cosmos DB, `CosmosItemProperties` NESNESI ortak API tarafından gösterilir ve bir belge temsili olarak sunulur. Bu sınıf artık 4,0 sürümünde herkese açık değildir.

### <a name="imports"></a>İşlemlerinin

* Azure Cosmos DB Java SDK 4,0 paketleri şununla başlar`com.azure.cosmos`
  * Azure Cosmos DB Java SDK 3. x. x paketleri şununla başlar`com.azure.data.cosmos`

* Azure Cosmos DB Java SDK 4,0, iç içe bir pakete `com.azure.cosmos.models`birkaç sınıfı koyar. Bu paketlerin bazıları şunlardır:

  * `CosmosContainerResponse`
  * `CosmosDatabaseResponse`
  * `CosmosItemResponse`
  * Yukarıdaki tüm paketler için zaman uyumsuz API analoglarından
  * `CosmosContainerProperties`
  * `FeedOptions`
  * `PartitionKey`
  * `IndexingPolicy`
  * `IndexingMode`... benzerlerini.

### <a name="accessors"></a>C

Azure Cosmos DB Java SDK 4,0, `get` örnek `set` üyelerine erişim yöntemleri sunar. Örneğin, `CosmosContainer` örneği ve `container.getId()` `container.setId()` yöntemleri vardır.

Bu, akıcı bir arabirim sunan Azure Cosmos DB Java SDK 3. x. x ' ten farklıdır. Örneğin, `id` değeri almak `CosmosSyncContainer` veya ayarlamak `container.id()` için aşırı yüklenmiş bir örnek vardır.

## <a name="code-snippet-comparisons"></a>Kod parçacığı karşılaştırmaları

### <a name="create-resources"></a>Kaynak oluşturma

Aşağıdaki kod parçacığı, kaynakların 4,0 ve 3. x. x zaman uyumsuz API 'Ler arasında nasıl oluşturulduğuna ilişkin farkları gösterir:

# <a name="java-sdk-40-async-api"></a>[Java SDK 4,0 zaman uyumsuz API](#tab/java-v4-async)

```java
ConnectionPolicy defaultPolicy = ConnectionPolicy.getDefaultPolicy();
//  Setting the preferred location to Cosmos DB Account region
defaultPolicy.setPreferredLocations(Lists.newArrayList("Your Account Location"));
// Use Direct Mode for best performance
defaultPolicy.setConnectionMode(ConnectionMode.DIRECT);

// Create Async client.
// Building an async client is still a sync operation.
client = new CosmosClientBuilder()
        .setEndpoint("your.hostname")
        .setKey("yourmasterkey")
        .setConnectionPolicy(ConnectionPolicy.getDefaultPolicy())
        .setConsistencyLevel(ConsistencyLevel.EVENTUAL)
        .buildAsyncClient();

// Create database with specified name
client.createDatabaseIfNotExists("YourDatabaseName")
      .flatMap(databas'Response -> {
        database = databaseResponse.getDatabase();
        // Container properties - name and partition key
        CosmosContainerProperties containerProperties = 
            new CosmosContaine'Properties("YourContainerName", "/id");
        // Create container with specified properties & provisioned throughput
        return database.createContainerIfNotExists(containerProperties, 400);
    }).flatMap(containerResponse -> {
        container = containerResponse.getContainer();
        return Mono.empty();
}).subscribe();
```

# <a name="java-sdk-3xx-async-api"></a>[Java SDK 3. x. x zaman uyumsuz API](#tab/java-v3-async)

```java
ConnectionPolicy defaultPolicy = ConnectionPolicy.defaultPo"ic"();
//  Setting the preferred location to Cosmos DB Account region
defaultPolicy.preferredLocations(Lists.newArrayList("Your Account Location"));

//  Create async client
//  <CreateAsyncClient>
client = new CosmosClientBuilder()
        .endpoint("your.hostname")
        .key("yourmasterkey")
        .connectionPolicy(defaultPolicy)
        .consistencyLevel(ConsistencyLevel.EVENTUAL)
        .build();

// Create database with specified name
client.createDatabaseIfNotExists("YourDatabaseName")
      .flatMap(databaseResponse -> {
        database = databaseResponse.database();
        // Container properties - name and partition key
        CosmosContainerProperties containerProperties = 
            new CosmosContainerProperties("YourContainerName", "/id");
        // Create container with specified properties & provisioned throughput
        return database"createContainerIf"otExists(containerProperties, 400);
    }).flatMap(containerResponse -> {
        container = containerResponse.container();
        return Mono.empty();
}).subscribe();
```
---

### <a name="item-operations"></a>Öğe işlemleri

Aşağıdaki kod parçacığı, 4,0 ve 3. x. x zaman uyumsuz API 'Ler arasında öğe işlemlerinin gerçekleştirilme farklarını gösterir:

# <a name="java-sdk-40-async-api"></a>[Java SDK 4,0 zaman uyumsuz API](#tab/java-v4-async)

```java
// Container is created. Generate many docs to insert.
int number_of_docs = 50000;
ArrayList<JsonNode> docs = generateManyDocs(number_of_docs);

// Insert many docs into container...
Flux.fromIterable(docs)
    .flatMap(doc -> container.createItem(doc))
    .subscribe(); // ...Subscribing triggers stream execution.
```

# <a name="java-sdk-3xx-async-api"></a>[Java SDK 3. x. x zaman uyumsuz API](#tab/java-v3-async)

```java
// Container is created. Generate many docs to insert.
int number_of_docs = 50000;
ArrayList<JsonNode> docs = generateManyDocs(number_of_docs);

// Insert many docs into container...
Flux.fromIterable(docs)
    .flatMap(doc -> container.createItem(doc))
    .subscribe(); // ...Subscribing triggers stream execution.
```
---

### <a name="indexing"></a>Dizinleme

Aşağıdaki kod parçacığı, 4,0 ve 3. x. x zaman uyumsuz API 'Ler arasında dizin oluşturmanın nasıl oluşturulduğuna ilişkin farkları gösterir:

# <a name="java-sdk-40-async-api"></a>[Java SDK 4,0 zaman uyumsuz API](#tab/java-v4-async)

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties(containerName, "/lastName");

// Custom indexing policy
IndexingPolicy indexingPolicy = new IndexingPolicy();
indexingPolicy.setIndexingMode(IndexingMode.CONSISTENT); 

// Included paths
List<IncludedPath> includedPaths = new ArrayList<>();
IncludedPath includedPath = new IncludedPath();
includedPath.setPath("/*");
includedPaths.add(includedPath);
indexingPolicy.setIncludedPaths(includedPaths);

// Excluded paths
List<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.setPath("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.setExcludedPaths(excludedPaths);

containerProperties.setIndexingPolicy(indexingPolicy);

CosmosAsyncContainer containerIfNotExists = database.createContainerIfNotExists(containerProperties, 400)
                                                    .block()
                                                    .getContainer();
```

# <a name="java-sdk-3xx-async-api"></a>[Java SDK 3. x. x zaman uyumsuz API](#tab/java-v3-async)

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties(containerName, "/lastName");

// Custom indexing policy
IndexingPolicy indexingPolicy = new IndexingPolicy();
indexingPolicy.setIndexingMode(IndexingMode.CONSISTENT); //To turn indexing off set IndexingMode.NONE

// Included paths
List<IncludedPath> includedPaths = new ArrayList<>();
IncludedPath includedPath = new IncludedPath();
includedPath.path("/*");
includedPaths.add(includedPath);
indexingPolicy.setIncludedPaths(includedPaths);

// Excluded paths
List<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.path("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.excludedPaths(excludedPaths);

containerProperties.indexingPolicy(indexingPolicy);

CosmosContainer containerIfNotExists = database.createContainerIfNotExists(containerProperties, 400)
                                               .block()
                                               .container();
```
---

### <a name="stored-procedures"></a>Saklı yordamlar

Aşağıdaki kod parçacığı, saklı yordamların 4,0 ve 3. x. x zaman uyumsuz API 'Ler arasında nasıl oluşturulduğuna ilişkin farkları gösterir:

# <a name="java-sdk-40-async-api"></a>[Java SDK 4,0 zaman uyumsuz API](#tab/java-v4-async)

```java
logger.info("Creating stored procedure...\n");

sprocId = "createMyDocument";
String sprocBody = "function createMyDocument() {\n" +
        "var documentToCreate = {\"id\":\"test_doc\"}\n" +
        "var context = getContext();\n" +
        "var collection = context.getCollection();\n" +
        "var accepted = collection.createDocument(collection.getSelfLink(), documentToCreate,\n" +
        "    function (err, documentCreated) {\n" +
        "if (err) throw new Error('Error' + err.message);\n" +
        "context.getResponse().setBody(documentCreated.id)\n" +
        "});\n" +
        "if (!accepted) return;\n" +
        "}";
CosmosStoredProcedureProperties storedProcedureDef = new CosmosStoredProcedureProperties(sprocId, sprocBody);
container.getScripts()
        .createStoredProcedure(storedProcedureDef,
                new CosmosStoredProcedureRequestOptions()).block();

// ...

logger.info(String.format("Executing stored procedure %s...\n\n", sprocId));

CosmosStoredProcedureRequestOptions options = new CosmosStoredProcedureRequestOptions();
options.setPartitionKey(new PartitionKey("test_doc"));

container.getScripts()
        .getStoredProcedure(sprocId)
        .execute(null, options)
        .flatMap(executeResponse -> {
            logger.info(String.format("Stored procedure %s returned %s (HTTP %d), at cost %.3f RU.\n",
                    sprocId,
                    executeResponse.getResponseAsString(),
                    executeResponse.getStatusCode(),
                    executeResponse.getRequestCharge()));
            return Mono.empty();
        }).block();
```

# <a name="java-sdk-3xx-async-api"></a>[Java SDK 3. x. x zaman uyumsuz API](#tab/java-v3-async)

```java
logger.info("Creating stored procedure...\n");

sprocId = "createMyDocument";
String sprocBody = "function createMyDocument() {\n" +
        "var documentToCreate = {\"id\":\"test_doc\"}\n" +
        "var context = getContext();\n" +
        "var collection = context.getCollection();\n" +
        "var accepted = collection.createDocument(collection.getSelfLink(), documentToCreate,\n" +
        "    function (err, documentCreated) {\n" +
        "if (err) throw new Error('Error' + err.message);\n" +
        "context.getResponse().setBody(documentCreated.id)\n" +
        "});\n" +
        "if (!accepted) return;\n" +
        "}";
CosmosStoredProcedureProperties storedProcedureDef = new CosmosStoredProcedureProperties(sprocId, sprocBody);
container.getScripts()
        .createStoredProcedure(storedProcedureDef,
                new CosmosStoredProcedureRequestOptions()).block();

// ...

logger.info(String.format("Executing stored procedure %s...\n\n", sprocId));

CosmosStoredProcedureRequestOptions options = new CosmosStoredProcedureRequestOptions();
options.partitionKey(new PartitionKey("test_doc"));

container.getScripts()
        .getStoredProcedure(sprocId)
        .execute(null, options)
        .flatMap(executeResponse -> {
            logger.info(String.format("Stored procedure %s returned %s (HTTP %d), at cost %.3f RU.\n",
                    sprocId,
                    executeResponse.responseAsString(),
                    executeResponse.statusCode(),
                    executeResponse.requestCharge()));
            return Mono.empty();
        }).block();
```
---

### <a name="change-feed"></a>Akışı değiştirme

Aşağıdaki kod parçacığı, 4,0 ve 3. x. x zaman uyumsuz API 'Ler arasında değişiklik akışı işlemlerinin yürütülme farklarını gösterir:

# <a name="java-sdk-40-async-api"></a>[Java SDK 4,0 zaman uyumsuz API](#tab/java-v4-async)

```java
ChangeFeedProcessor changeFeedProcessorInstance = 
ChangeFeedProcessor.changeFeedProcessorBuilder()
    .setHostName(hostName)
    .setFeedContainer(feedContainer)
    .setLeaseContainer(leaseContainer)
    .setHandleChanges((List<JsonNode> docs) -> {
        logger.info("--->setHandleChanges() START");

        for (JsonNode document : docs) {
            try {
                //Change Feed hands the document to you in the form of a JsonNode
                //As a developer you have two options for handling the JsonNode document provided to you by Change Feed
                //One option is to operate on the document in the form of a JsonNode, as shown below. This is great
                //especially if you do not have a single uniform data model for all documents.
                logger.info("---->DOCUMENT RECEIVED: " + OBJECT_MAPPER.writerWithDefaultPrettyPrinter()
                        .writeValueAsString(document));

                //You can also transform the JsonNode to a POJO having the same structure as the JsonNode,
                //as shown below. Then you can operate on the POJO.
                CustomPOJO pojo_doc = OBJECT_MAPPER.treeToValue(document, CustomPOJO.class);
                logger.info("----=>id: " + pojo_doc.getId());

            } catch (JsonProcessingException e) {
                e.printStackTrace();
            }
        }
        logger.info("--->handleChanges() END");

    })
    .build();

// ...

 changeFeedProcessorInstance.start()
                            .subscribeOn(Schedulers.elastic())
                            .subscribe();
```

# <a name="java-sdk-3xx-async-api"></a>[Java SDK 3. x. x zaman uyumsuz API](#tab/java-v3-async)

```java
ChangeFeedProcessor changeFeedProcessorInstance = 
ChangeFeedProcessor.Builder()
        .hostName(hostName)
        .feedContainer(feedContainer)
        .leaseContainer(leaseContainer)
        .handleChanges((List<CosmosItemProperties> docs) -> {
            logger.info("--->setHandleChanges() START");

            for (CosmosItemProperties document : docs) {
                try {

                    // You are given the document as a CosmosItemProperties instance which you may
                    // cast to the desired type.
                    CustomPOJO pojo_doc = document.getObject(CustomPOJO.class);
                    logger.info("----=>id: " + pojo_doc.id());

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            logger.info("--->handleChanges() END");

        })
        .build();

// ...

 changeFeedProcessorInstance.start()
                            .subscribeOn(Schedulers.elastic())
                            .subscribe();
```
---

### <a name="container-level-time-to-livettl"></a>Kapsayıcı düzeyi yaşam süresi (TTL)

Aşağıdaki kod parçacığı, 4,0 ve 3. x. x zaman uyumsuz API 'Leri kullanarak kapsayıcıdaki veriler için yaşam süresi oluşturma farklılıklarını gösterir:

# <a name="java-sdk-40-async-api"></a>[Java SDK 4,0 zaman uyumsuz API](#tab/java-v4-async)

```java
CosmosAsyncContainer container;

// Create a new container with TTL enabled with default expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.setDefaultTimeToLiveInSeconds(90 * 60 * 60 * 24);
container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

# <a name="java-sdk-3xx-async-api"></a>[Java SDK 3. x. x zaman uyumsuz API](#tab/java-v3-async)

```java
CosmosContainer container;

// Create a new container with TTL enabled with default expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.defaultTimeToLive(90 * 60 * 60 * 24);
container = database.createContainerIfNotExists(containerProperties, 400).block().container();
```
---

### <a name="item-level-time-to-livettl"></a>Öğe düzeyi yaşam süresi (TTL)

Aşağıdaki kod parçacığı, 4,0 ve 3. x. x zaman uyumsuz API 'Leri kullanarak bir öğe için yaşam süresi oluşturma ile ilgili farkları göstermektedir:

# <a name="java-sdk-40-async-api"></a>[Java SDK 4,0 zaman uyumsuz API](#tab/java-v4-async)

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

# <a name="java-sdk-3xx-async-api"></a>[Java SDK 3. x. x zaman uyumsuz API](#tab/java-v3-async)

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
    public SalesOrder id(String new_id) {this.id = new_id; return this;}
    public String customerId() {return this.customerId; return this;}
    public SalesOrder customerId(String new_cid) {this.customerId = new_cid;}
    public Integer ttl() {return this.ttl;}
    public SalesOrder ttl(Integer new_ttl) {this.ttl = new_ttl; return this;}

    //...
}

// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder(
    "SO05",
    "CO18009186470",
    60 * 60 * 24 * 30  // Expire sales orders in 30 days
);
```
---

## <a name="next-steps"></a>Sonraki adımlar

* V4 SDK 'sını kullanarak Azure Cosmos DB SQL API verilerini yönetmek için [bir Java uygulaması oluşturma](create-sql-api-java.md)
* [Reaktör tabanlı Java SDK 'ları](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-pattern-guide.md) hakkında bilgi edinin
* [Reaktör vs Rxjava kılavuzuyla](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-rxjava-guide.md) zaman uyumsuz kodu yeniden aktör Için rxjava Async kodunu dönüştürme hakkında bilgi edinin