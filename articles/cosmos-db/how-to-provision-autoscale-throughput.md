---
title: Azure Cosmos DB otomatik ölçeklendirme üretilen işi sağlama
description: Azure portal, CLı, PowerShell ve diğer diğer SDK 'Ları kullanarak Azure Cosmos DB kapsayıcı ve veritabanı düzeyinde otomatik ölçeklendirme üretimi sağlamayı öğrenin.
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/30/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 4e7c5f3f4bf84b7a267cb883df5f375f2a8cf981
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89017150"
---
# <a name="provision-autoscale-throughput-on-database-or-container-in-azure-cosmos-db"></a>Azure Cosmos DB veritabanında veya kapsayıcıda otomatik ölçeklendirme üretilen işi sağlama

Bu makalede, Azure Cosmos DB bir veritabanında veya kapsayıcıda (koleksiyon, grafik veya tablo) otomatik ölçeklendirme işleme sağlama açıklanmaktadır. Tek bir kapsayıcıda otomatik ölçeklendirmeyi etkinleştirebilir veya bir veritabanında otomatik ölçeklendirme üretilen işi sağlayabilir ve veritabanındaki tüm kapsayıcılar arasında paylaşabilirsiniz.

## <a name="azure-portal"></a>Azure portal

### <a name="create-new-database-or-container-with-autoscale"></a>Otomatik ölçeklendirme ile yeni veritabanı veya kapsayıcı oluşturma

1. [Azure Portal](https://portal.azure.com) veya [Azure Cosmos DB Gezgininde](https://cosmos.azure.com/) oturum açın.

1. Azure Cosmos DB hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. **Yeni kapsayıcı** ' yı seçin. Veritabanınız, Kapsayıcınız ve bölüm anahtarınız için bir ad girin. **Verimlilik**altında, **Otomatik ölçeklendirme** seçeneğini belirleyin ve veritabanının veya kapsayıcının ölçeklendirilmesini istediğiniz [en yüksek aktarım hızını (ru/sn)](provision-throughput-autoscale.md#how-autoscale-provisioned-throughput-works) ayarlayın.

   :::image type="content" source="./media/how-to-provision-autoscale-throughput/create-new-autoscale-container.png" alt-text="Kapsayıcı oluşturma ve otomatik ölçeklendirme sağlanan verimini yapılandırma":::

1. **Tamam**’ı seçin.

Paylaşılan aktarım hızı veritabanında otomatik ölçeklendirme sağlamak için yeni bir veritabanı oluştururken **veritabanı Işleme sağlama** seçeneğini belirleyin. 

### <a name="enable-autoscale-on-existing-database-or-container"></a>Mevcut veritabanı veya kapsayıcıda otomatik ölçeklendirmeyi etkinleştir

> [!IMPORTANT]
> Geçerli sürümde, otomatik ölçeklendirme ve standart (el ile) sağlanan aktarım hızı arasında geçiş yapmanın tek yolu Azure portal. 

1. [Azure Portal](https://portal.azure.com) veya [Azure Cosmos DB Gezgininde](https://cosmos.azure.com/) oturum açın.

1. Azure Cosmos DB hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. Kapsayıcınız için **ölçek ve ayarlar** ' ı veya veritabanınızın **ölçeğini** seçin.

1. **Ölçek**altında, **Otomatik ölçeklendirme** seçeneğini belirleyin ve **kaydedin**.

   :::image type="content" source="./media/how-to-provision-autoscale-throughput/autoscale-scale-and-settings.png" alt-text="Varolan bir kapsayıcıda otomatik ölçeklendirmeyi etkinleştirme":::

> [!NOTE]
> Varolan bir veritabanı veya kapsayıcıda otomatik ölçeklendirmeyi etkinleştirdiğinizde, en fazla RU/sn için başlangıç değeri sistem tarafından belirlenir ve geçerli el ile sağlanan aktarım hızı ayarları ve depolama alanınızı temel alır. İşlem tamamlandıktan sonra gerekirse en fazla RU/sn 'yi değiştirebilirsiniz. [Daha fazla bilgi edinin.](autoscale-faq.md#how-does-the-migration-between-autoscale-and-standard-manual-provisioned-throughput-work) 

## <a name="azure-cosmos-db-net-v3-sdk-for-sql-api"></a>SQL API için .NET v3 SDK Azure Cosmos DB

Otomatik ölçeklendirme kaynaklarını yönetmek için SQL API için Azure Cosmos DB .NET SDK 'sının [sürüm 3,9 veya üstünü](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) kullanın. 

> [!IMPORTANT]
> Yeni otomatik ölçeklendirme kaynakları oluşturmak için .NET SDK 'sını kullanabilirsiniz. SDK, otomatik ölçeklendirme ve standart (el ile) aktarım hızı arasında geçişi desteklemez. Geçiş senaryosu şu anda yalnızca Azure portal desteklenir. 

### <a name="create-database-with-shared-throughput"></a>Paylaşılan verimlilik ile veritabanı oluşturma

```csharp
// Create instance of CosmosClient
CosmosClient cosmosClient = new CosmosClient(Endpoint, PrimaryKey);
 
// Autoscale throughput settings
ThroughputProperties autoscaleThroughputProperties = ThroughputProperties.CreateAutoscaleThroughput(4000); //Set autoscale max RU/s

//Create the database with autoscale enabled
database = await cosmosClient.CreateDatabaseAsync(DatabaseName, throughputProperties: autoscaleThroughputProperties);
```

### <a name="create-container-with-dedicated-throughput"></a>Adanmış aktarım hızı ile kapsayıcı oluşturma

```csharp
// Get reference to database that container will be created in
Database database = await cosmosClient.GetDatabase("DatabaseName");

// Container and autoscale throughput settings
ContainerProperties autoscaleContainerProperties = new ContainerProperties("ContainerName", "/partitionKey");
ThroughputProperties autoscaleThroughputProperties = ThroughputProperties.CreateAutoscaleThroughput(4000); //Set autoscale max RU/s

// Create the container with autoscale enabled
container = await database.CreateContainerAsync(autoscaleContainerProperties, autoscaleThroughputProperties);
```

### <a name="read-the-current-throughput-rus"></a>Geçerli aktarım hızını (RU/s) okuyun

```csharp
// Get a reference to the resource
Container container = cosmosClient.GetDatabase("DatabaseName").GetContainer("ContainerName");

// Read the throughput on a resource
ThroughputProperties autoscaleContainerThroughput = await container.ReadThroughputAsync(requestOptions: null); 

// The autoscale max throughput (RU/s) of the resource
int? autoscaleMaxThroughput = autoscaleContainerThroughput.AutoscaleMaxThroughput;

// The throughput (RU/s) the resource is currently scaled to
int? currentThroughput = autoscaleContainerThroughput.Throughput;
```

### <a name="change-the-autoscale-max-throughput-rus"></a>Otomatik ölçeklendirme maksimum aktarım hızını değiştirme (RU/s)

```csharp
// Change the autoscale max throughput (RU/s)
await container.ReplaceThroughputAsync(ThroughputProperties.CreateAutoscaleThroughput(newAutoscaleMaxThroughput));
```

## <a name="azure-cosmos-db-java-v4-sdk-for-sql-api"></a>SQL API için Java v4 SDK Azure Cosmos DB

Otomatik ölçeklendirme kaynaklarını yönetmek için SQL API için Azure Cosmos DB Java SDK 'sının [4,0 veya sonraki bir sürümünü](https://mvnrepository.com/artifact/com.azure/azure-cosmos) kullanabilirsiniz.

> [!IMPORTANT]
> Yeni otomatik ölçeklendirme kaynakları oluşturmak için Java SDK 'sını kullanabilirsiniz. SDK, otomatik ölçeklendirme ve standart (el ile) aktarım hızı arasında geçişi desteklemez. Geçiş senaryosu şu anda yalnızca Azure portal desteklenir.

### <a name="create-database-with-shared-throughput"></a>Paylaşılan verimlilik ile veritabanı oluşturma

#### <a name="async"></a>[Eş](#tab/api-async)

```java
// Create instance of CosmosClient
CosmosAsyncClient client = new CosmosClientBuilder()
    .setEndpoint(HOST)
    .setKey(MASTER)
    .setConnectionPolicy(CONNECTIONPOLICY)
    .buildAsyncClient();

// Autoscale throughput settings
ThroughputProperties autoscaleThroughputProperties = ThroughputProperties.createAutoscaledThroughput(4000); //Set autoscale max RU/s

//Create the database with autoscale enabled
CosmosAsyncDatabase database = client.createDatabase(databaseName, autoscaleThroughputProperties).block().getDatabase();
```

#### <a name="sync"></a>[Eşitle](#tab/api-sync)

```java
// Create instance of CosmosClient
CosmosClient client = new CosmosClientBuilder()
    .setEndpoint(HOST)
    .setKey(MASTER)
    .setConnectionPolicy(CONNECTIONPOLICY)
    .buildClient();

// Autoscale throughput settings
ThroughputProperties autoscaleThroughputProperties = ThroughputProperties.createAutoscaledThroughput(4000); //Set autoscale max RU/s

//Create the database with autoscale enabled
CosmosDatabase database = client.createDatabase(databaseName, autoscaleThroughputProperties).getDatabase();
```

--- 

### <a name="create-container-with-dedicated-throughput"></a>Adanmış aktarım hızı ile kapsayıcı oluşturma

#### <a name="async"></a>[Eş](#tab/api-async)

```java
// Get reference to database that container will be created in
CosmosAsyncDatabase database = client.createDatabase("DatabaseName").block().getDatabase();

// Container and autoscale throughput settings
CosmosContainerProperties autoscaleContainerProperties = new CosmosContainerProperties("ContainerName", "/partitionKey");
ThroughputProperties autoscaleThroughputProperties = ThroughputProperties.createAutoscaledThroughput(4000); //Set autoscale max RU/s

// Create the container with autoscale enabled
CosmosAsyncContainer container = database.createContainer(autoscaleContainerProperties, autoscaleThroughputProperties, new CosmosContainerRequestOptions())
                                .block()
                                .getContainer();
```

#### <a name="sync"></a>[Eşitle](#tab/api-sync)

```java
// Get reference to database that container will be created in
CosmosDatabase database = client.createDatabase("DatabaseName").getDatabase();

// Container and autoscale throughput settings
CosmosContainerProperties autoscaleContainerProperties = new CosmosContainerProperties("ContainerName", "/partitionKey");
ThroughputProperties autoscaleThroughputProperties = ThroughputProperties.createAutoscaledThroughput(4000); //Set autoscale max RU/s

// Create the container with autoscale enabled
CosmosContainer container = database.createContainer(autoscaleContainerProperties, autoscaleThroughputProperties, new CosmosContainerRequestOptions())
                                .getContainer();
```

--- 

### <a name="read-the-current-throughput-rus"></a>Geçerli aktarım hızını (RU/s) okuyun

#### <a name="async"></a>[Eş](#tab/api-async)

```java
// Get a reference to the resource
CosmosAsyncContainer container = client.getDatabase("DatabaseName").getContainer("ContainerName");

// Read the throughput on a resource
ThroughputProperties autoscaleContainerThroughput = container.readThroughput().block().getProperties();

// The autoscale max throughput (RU/s) of the resource
int autoscaleMaxThroughput = autoscaleContainerThroughput.getAutoscaleMaxThroughput();

// The throughput (RU/s) the resource is currently scaled to
int currentThroughput = autoscaleContainerThroughput.Throughput;
```

#### <a name="sync"></a>[Eşitle](#tab/api-sync)

```java
// Get a reference to the resource
CosmosContainer container = client.getDatabase("DatabaseName").getContainer("ContainerName");

// Read the throughput on a resource
ThroughputProperties autoscaleContainerThroughput = container.readThroughput().getProperties();

// The autoscale max throughput (RU/s) of the resource
int autoscaleMaxThroughput = autoscaleContainerThroughput.getAutoscaleMaxThroughput();

// The throughput (RU/s) the resource is currently scaled to
int currentThroughput = autoscaleContainerThroughput.Throughput;
```

--- 

### <a name="change-the-autoscale-max-throughput-rus"></a>Otomatik ölçeklendirme maksimum aktarım hızını değiştirme (RU/s)

#### <a name="async"></a>[Eş](#tab/api-async)

```java
// Change the autoscale max throughput (RU/s)
container.replaceThroughput(ThroughputProperties.createAutoscaledThroughput(newAutoscaleMaxThroughput)).block();
```

#### <a name="sync"></a>[Eşitle](#tab/api-sync)

```java
// Change the autoscale max throughput (RU/s)
container.replaceThroughput(ThroughputProperties.createAutoscaledThroughput(newAutoscaleMaxThroughput));
```

---

## <a name="cassandra-api"></a>Cassandra API’si

Cassandra API için Azure Cosmos DB hesapları [CQL komutları](manage-scale-cassandra.md#use-autoscale), [Azure CLI](cli-samples.md), [Azure PowerShell](powershell-samples.md) veya [Azure Resource Manager şablonları](resource-manager-samples.md)kullanılarak otomatik ölçeklendirme için sağlanabilir.

## <a name="azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API

MongoDB API 'SI için Azure Cosmos DB hesapları, [MongoDB uzantı komutları](mongodb-custom-commands.md), [Azure CLI](cli-samples.md), [Azure PowerShell](powershell-samples.md) veya [Azure Resource Manager şablonları](resource-manager-samples.md)kullanılarak otomatik ölçeklendirme için sağlanabilir.

## <a name="azure-resource-manager"></a>Azure Resource Manager

Azure Resource Manager şablonlar, tüm Azure Cosmos DB API 'Leri için veritabanında veya kapsayıcı düzeyindeki kaynaklarda otomatik ölçeklendirme üretilen işi sağlamak için kullanılabilir. Örnekler için [Azure Cosmos DB için bkz. Azure Resource Manager şablonları](resource-manager-samples.md) .

## <a name="azure-cli"></a>Azure CLI

Azure CLı, tüm Azure Cosmos DB API 'Leri için bir veritabanı veya kapsayıcı düzeyinde bir kaynak üzerinde otomatik ölçeklendirme üretilen işi sağlamak üzere kullanılabilir. Örnekler için bkz. [Azure CLI örnekleri Azure Cosmos DB](cli-samples.md).

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell, tüm Azure Cosmos DB API 'Leri için bir veritabanında veya kapsayıcı düzeyindeki kaynaklarda otomatik ölçeklendirme üretilen işi sağlamak için kullanılabilir. Örnekler için bkz. [Azure Cosmos DB için Azure PowerShell örnekleri](powershell-samples.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Otomatik ölçeklendirme ile sağlanan aktarım hızının avantajları](provision-throughput-autoscale.md#benefits-of-autoscale)hakkında bilgi edinin.
* [El ile ve otomatik ölçeklendirme üretimi arasından seçim](how-to-choose-offer.md)yapmayı öğrenin.
* [Otomatik ÖLÇEKLENDIRME SSS](autoscale-faq.md)'sini inceleyin.
