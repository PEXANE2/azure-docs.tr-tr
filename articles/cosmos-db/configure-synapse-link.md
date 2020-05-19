---
title: Azure Cosmos DB için Azure SYNAPSE bağlantısı 'nı yapılandırma ve kullanma (Önizleme)
description: Azure Cosmos hesapları için SYNAPSE bağlantısının nasıl etkinleştirileceğini, analitik deponun etkinleştirildiği bir kapsayıcı oluşturmayı, Azure Cosmos veritabanını SYNAPSE çalışma alanına bağlamayı ve sorguları çalıştırmayı öğrenin.
author: SriChintala
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: srchi
ms.openlocfilehash: 384d4ebf211c80ac069889356f269407ba7d3d75
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598727"
---
# <a name="configure-and-use-azure-synapse-link-for-azure-cosmos-db-preview"></a>Azure Cosmos DB için Azure SYNAPSE bağlantısı 'nı yapılandırma ve kullanma (Önizleme)

Azure Cosmos DB için SYNAPSE bağlantısı, Azure Cosmos DB ' de işletimsel veriler üzerinde neredeyse gerçek zamanlı analizler çalıştırmanıza olanak sağlayan, bulutta yerel bir karma işlem ve analitik işleme (HTAP) özelliğidir. SYNAPSE link, Azure Cosmos DB ile Azure SYNAPSE Analytics arasında sıkı sorunsuz bir tümleştirme oluşturur.


> [!IMPORTANT]
> Azure SYNAPSE bağlantısını kullanmak için Azure Cosmos hesabınızı & Azure SYNAPSE Analytics çalışma alanına yukarıdaki desteklenen bölgelerden birinde sağladığınızdan emin olun. Desteklenen bölgelerin listesi için bkz. [Azure hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/). 

Azure Cosmos DB için SYNAPSE bağlantısıyla analitik sorgular çalıştırmak için aşağıdaki adımları kullanın:

* [Azure Cosmos hesaplarınız için SYNAPSE bağlantısını etkinleştirin](#enable-synapse-link)
* [Analitik mağaza etkin Azure Cosmos kapsayıcısı oluşturma](#create-analytical-ttl)
* [Azure Cosmos veritabanınızı bir Synapse çalışma alanına bağlama](#connect-to-cosmos-database)
* [SYNAPSE Spark kullanarak analitik depoyu sorgulama](#query-analytical-store)

## <a name="enable-azure-synapse-link-for-azure-cosmos-accounts"></a><a id="enable-synapse-link"></a>Azure Cosmos hesapları için Azure SYNAPSE bağlantısını etkinleştirme

### <a name="azure-portal"></a>Azure portal

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya mevcut bir Azure Cosmos hesabı seçin.

1. Azure Cosmos hesabınıza gidin ve **Özellikler** bölmesini açın.

1. Özellikler listesinden **SYNAPSE bağlantısı** ' nı seçin.

   ![SYNAPSE link Preview özelliğini bulma](./media/configure-synapse-link/find-synapse-link-feature.png)

1. Bundan sonra hesabınızda SYNAPSE bağlantısını etkinleştirmenizi ister. Etkinleştir’i seçin.

   ![SYNAPSE Link özelliğini etkinleştir](./media/configure-synapse-link/enable-synapse-link-feature.png)

1. Hesabınız artık SYNAPSE bağlantısını kullanacak şekilde etkinleştirilmiştir. Ardından, işletimsel verilerinizi işlemsel depodan analitik depoya çoğaltmaya otomatik olarak başlamak için bkz. analitik depo etkin kapsayıcıları oluşturma.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

[Azure Resource Manager şablonu](manage-sql-with-resource-manager.md#azure-cosmos-account-with-analytical-store) , SQL API 'si Için bir Synapse bağlantısı etkin Azure Cosmos hesabı oluşturur. Bu şablon, analitik TTL etkin olarak yapılandırılmış bir kapsayıcı içeren bir bölgede bir çekirdek (SQL) API hesabı oluşturur ve el ile veya otomatik ölçeklendirme üretimi kullanma seçeneği sağlar. Bu şablonu dağıtmak için Benioku sayfasında **Azure 'A dağıt** ' a tıklayın.

## <a name="create-an-azure-cosmos-container-with-analytical-store"></a><a id="create-analytical-ttl"></a>Analitik depo ile Azure Cosmos kapsayıcısı oluşturma

Kapsayıcıyı oluştururken Azure Cosmos kapsayıcısında analitik depoyu açabilirsiniz. Azure portal kullanabilir veya `analyticalTTL` Azure Cosmos DB SDK 'ları kullanarak kapsayıcı oluşturma sırasında özelliğini yapılandırabilirsiniz.

> [!NOTE]
> Şu anda analitik depoyu **Yeni** kapsayıcılar için (hem yeni hem de mevcut hesaplarda) etkinleştirebilirsiniz.

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com/) veya [Azure Cosmos Gezgini](https://cosmos.azure.com/)' nde oturum açın.

1. Azure Cosmos hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. **Yeni kapsayıcı** ' yı seçin ve veritabanınız, Kapsayıcınız, Bölüm anahtarınız ve verimlilik ayrıntılarınız için bir ad girin. **Analitik depo** seçeneğini açın. Analitik depoyu etkinleştirdikten sonra, `AnalyicalTTL` özelliği-1 varsayılan değerine ayarlanmış bir kapsayıcı oluşturur (sonsuz saklama). Kayıtların tüm geçmiş sürümlerini koruyan bu analitik depo.

   ![Azure Cosmos kapsayıcısı için analitik depoyu aç](./media/configure-synapse-link/create-container-analytical-store.png)

1. Bu hesapta daha önce SYNAPSE bağlantısı etkinleştirilmemişse, analitik depo özellikli bir kapsayıcı oluşturmak için önkoşul olduğundan bunu yapmanız istenir. İstenirse, **SYNAPSE bağlantısını etkinleştir**' i seçin.

1. Azure Cosmos kapsayıcısını etkin bir analitik mağaza oluşturmak için **Tamam**' ı seçin.

### <a name="net-sdk"></a>.NET SDK

Aşağıdaki kod, .NET SDK kullanarak analitik depolarla bir kapsayıcı oluşturur. Analitik TTL özelliğini gerekli değere ayarlayın. İzin verilen değerler listesi için bkz. [ANALITIK TTL desteklenen değerler](analytical-store-introduction.md#analytical-ttl) makalesi:

```csharp
// Create a container with a partition key, and analytical TTL configured to  -1 (infinite retention)
string containerId = “myContainerName”;
int analyticalTtlInSec = -1;
ContainerProperties cpInput = new ContainerProperties()
            {
Id = containerId,
PartitionKeyPath = "/id",
AnalyticalStorageTimeToLiveInSeconds = analyticalTtlInSec,
};
 await this. cosmosClient.GetDatabase("myDatabase").CreateContainerAsync(cpInput);
```

### <a name="java-v4-sdk"></a>Java v4 SDK 'Sı

Aşağıdaki kod, Java v4 SDK 'sını kullanarak analitik depolarla bir kapsayıcı oluşturur. `AnalyticalStoreTimeToLiveInSeconds`Özelliği gerekli değere ayarlayın:

```java
// Create a container with a partition key and  analytical TTL configured to  -1 (infinite retention) 
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

containerProperties.setAnalyticalStoreTimeToLiveInSeconds(-1);

container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

### <a name="python-v3-sdk"></a>Python v3 SDK

Aşağıdaki kod, Python SDK kullanarak analitik depo ile bir kapsayıcı oluşturur:

```python
import azure.cosmos.cosmos_client as cosmos_client
def create_collection_if_not_exists(cosmosEndpoint, cosmosKey, databaseName, collectionName):
    client = cosmos_client.CosmosClient(url_connection=cosmosEndpoint, auth={'masterKey': cosmosKey})

db = client.QueryDatabases("select * from c where c.id = '" + databaseName + "'").fetch_next_block()[0]
options = {
    'offerThroughput': 1000
}

container_definition = {
    'id': collectionName,
    "partitionKey": {  
        "paths": [  
        "/id"  
        ],  
        "kind": "Hash" 
    },
    "indexingPolicy": {  
    "indexingMode": "consistent",  
    "automatic": True,  
    "includedPaths": [],  
    "excludedPaths": [{
        "path": "/*"
    }]  
    },
    "defaultTtl": -1,
    "analyticalStorageTtl": -1
}

container = client.CreateContainer(db['_self'], container_definition, options)
```

### <a name="update-the-analytical-store-time-to-live"></a><a id="update-analytical-ttl"></a>Analitik depo zamanını canlı olarak güncelleştirme

Analitik depo belirli bir TTL değeri ile etkinleştirildikten sonra, daha sonra farklı bir geçerli değere güncelleştirebilirsiniz. Azure portal veya SDK 'Ları kullanarak değeri güncelleştirebilirsiniz. Çeşitli analitik TTL yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [ANALITIK TTL desteklenen değerler](analytical-store-introduction.md#analytical-ttl) makalesi.

#### <a name="azure-portal"></a>Azure portal

Azure portal aracılığıyla analitik mağaza etkin bir kapsayıcı oluşturduysanız, varsayılan bir analitik TTL-1 ' i içerir. Bu değeri güncelleştirmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com/) veya [Azure Cosmos Gezgini](https://cosmos.azure.com/)' nde oturum açın.

1. Azure Cosmos hesabınıza gidin ve **Veri Gezgini** sekmesini açın.

1. Analitik deponun etkinleştirildiği mevcut bir kapsayıcıyı seçin. Genişletin ve aşağıdaki değerleri değiştirin:

  * **Ölçek & ayarları** penceresini açın.
  * Bul **ayarı** altında, * * gerçek zamanlı analitik depolama süresi * *.
  * **Açık (varsayılan)** **seçeneğini belirleyin veya SEÇIN ve bir** TTL değeri ayarlayın
  * Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

#### <a name="net-sdk"></a>.NET SDK

Aşağıdaki kod, .NET SDK kullanarak analitik depo için TTL 'nin nasıl güncelleştirilmesini göstermektedir:

```csharp
// Get the container, update AnalyticalStorageTimeToLiveInSeconds 
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Update analytical store TTL
containerResponse.Resource. AnalyticalStorageTimeToLiveInSeconds = 60 * 60 * 24 * 180  // Expire analytical store data in 6 months;
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

#### <a name="java-v4-sdk"></a>Java v4 SDK 'Sı

Aşağıdaki kod, Java v4 SDK 'sını kullanarak analitik depo için TTL 'nin nasıl güncelleştirilmesini göstermektedir:

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

// Update analytical store TTL to expire analytical store data in 6 months;
containerProperties.setAnalyticalStoreTimeToLiveInSeconds (60 * 60 * 24 * 180 );  
 
// Update container settings
container.replace(containerProperties).block();
```

## <a name="connect-to-a-synapse-workspace"></a><a id="connect-to-cosmos-database"></a>Bir Synapse çalışma alanına bağlanma

Azure SYNAPSE link ile Azure SYNAPSE Analytics Studio 'dan bir Azure Cosmos DB veritabanına nasıl eriştiğine ilişkin Azure [SYNAPSE Connect]() 'teki yönergeleri kullanın.

## <a name="query-using-synapse-spark"></a><a id="query-analytical-store"></a>SYNAPSE Spark kullanarak sorgulama

SYNAPSE Spark ile sorgulama hakkında [analitik depo makalesindeki sorgu Azure Cosmos DB sorgulama]() bölümündeki yönergeleri kullanın. Bu makalede, SYNAPSE hareketlerinden analitik depoyla nasıl etkileşim kurabileceğine ilişkin bazı örnekler verilmektedir. Bir kapsayıcıya sağ tıkladığınızda bu hareketler görünür. Hareketlerle hızlı bir şekilde kod oluşturabilir ve bunu gereksinimlerinize ince ayar edebilirsiniz. Tek bir tıklama ile verileri bulmak için de mükemmeldir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure Cosmos DB için Azure SYNAPSE bağlantısı.](synapse-link.md)

* [Analitik depoya genel bakış Azure Cosmos DB.](analytical-store-introduction.md)

* [Azure Cosmos DB için SYNAPSE bağlantısı hakkında sık sorulan sorular.](synapse-link-frequently-asked-questions.md)

* [Azure SYNAPSE Analytics 'te Apache Spark](../synapse-analytics/spark/apache-spark-concepts.md).

* [Azure SYNAPSE Analytics 'Te SQL sunucusuz/isteğe](../synapse-analytics/sql/on-demand-workspace-overview.md)bağlı.
