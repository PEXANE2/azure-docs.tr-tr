---
title: Spring Data Azure Cosmos DB v3 for SQL API sürüm notları ve kaynakları
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve SQL zaman uyumsuz Java SDK 'sının her Azure Cosmos DB bir sürümü arasında yapılan değişiklikler dahil olmak üzere, SQL API 'SI için Azure Cosmos DB için v3 verileri hakkında bilgi edinin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/18/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: e67e6911eeac29036dee2b68c19395b34e1d11da
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89228044"
---
# <a name="spring-data-azure-cosmos-db-v3-for-core-sql-api-release-notes-and-resources"></a>Yay verileri Azure Cosmos DB v3 for Core (SQL) API: sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET değişiklik akışı SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK’sı v4](sql-api-sdk-java-v4.md)
> * [Zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md)
> * [Zaman uyumlu Java SDK v2](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Spark Bağlayıcısı](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

Çekirdek (SQL) için Spring Data Azure Cosmos DB v3, geliştiricilerin Spring uygulamalarında Azure Cosmos DB kullanmasına izin verir. Spring Data Azure Cosmos DB veritabanlarını ve koleksiyonları işlemek, belgelerle çalışmak ve sorgu vermek için Spring Data arabirimini kullanıma sunar. Aynı Maven yapıtı içinde hem eşitleme hem de Async (reaktif) API 'Leri desteklenir. 

Spring Data Azure Cosmos DB yay veri çerçevesine bir bağımlılık alır. Azure Cosmos DB SDK ekibi, Spring Data v 2.2 ve v 2.3 için Maven yapıtları yayınlar.

[Spring Framework](https://spring.io/projects/spring-framework) , Java uygulaması geliştirmeyi kolaylaştırın bir programlama ve yapılandırma modelidir. Kuruluşun Web sitesini teklife göre yay, bağımlılık ekleme kullanarak uygulamaların "sıhhi tesisat" düzeyini kolaylaştırır. Uygulama oluşturma ve test etme işlemleri daha basit hale geldiği için Spring gibi birçok geliştirici. [Spring Boot](https://spring.io/projects/spring-boot) , Web uygulaması ve mikro hizmet geliştirme ile ilgili bir gözle sıhhi tesisat 'yi işlemeye yönelik bu fikir düzeyini genişletiyor. [Spring Data](https://spring.io/projects/spring-data) , Spring veya Spring Boot uygulaması bağlamından Azure Cosmos DB gibi veri depolarına erişmek için bir programlama modeli ve çerçevesidir. 

Spring Data Azure Cosmos DB, [Azure Spring Cloud](https://azure.microsoft.com/services/spring-cloud/) uygulamalarınızda kullanabilirsiniz.

> [!IMPORTANT]  
> Bu sürüm notları, Azure Cosmos DB için v3 veri verilerini yöneliktir. V2 sürüm notlarını [buradan](sql-api-sdk-java-spring-v2.md)bulabilirsiniz. 
>
> Spring Data Azure Cosmos DB yalnızca SQL API 'sini destekler.
>
> Aşağıdaki kılavuzlar diğer Azure Cosmos DB API 'Lerinde Spring verilerini destekler:
> * [Azure Cosmos DB ile Apache Cassandra için yay verileri](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Azure Cosmos DB ile Spring Data MongoDB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Yay verileri Gremlin Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
>

## <a name="start-here"></a>Buradan başlayın

# <a name="explore"></a>[Keşfetme](#tab/explore)

<img src="media/sql-api-sdk-java-spring-v3/up-arrow.png" alt="explore the tabs above" width="80"/>

### <a name="navigate-the-tabs-above-for-basic-spring-data-azure-cosmos-db-samples"></a>Temel yay verileri Azure Cosmos DB örnekleri için yukarıdaki sekmelere gidin.

# <a name="pomxml"></a>[pom.xml](#tab/pom)

### <a name="configure-dependencies"></a>Bağımlılıkları yapılandırma

İki Spring Data Azure Cosmos DB v3 Maven yapıtları mevcuttur.

Spring Data Framework v 2.2 'e bağlı olan yapıt:
```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-spring-data-2-2-cosmos</artifactId>
    <version>latest</version>
</dependency>
```

Spring Data Framework v 2.3 'e bağlı olan yapıt:
```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-spring-data-2-3-cosmos</artifactId>
    <version>latest</version>
</dependency>
```

# <a name="connect"></a>[Bağlan](#tab/connect)

### <a name="connect"></a>Bağlan

Azure Cosmos DB hesabı ve kapsayıcı ayrıntılarını belirtin. Spring Data Azure Cosmos DB istemciyi otomatik olarak oluşturur ve kapsayıcıya bağlar.

[Application. Properties](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-2-cosmos-java-getting-started/src/main/resources/application.properties):
```
cosmos.uri=${ACCOUNT_HOST}
cosmos.key=${ACCOUNT_KEY}
cosmos.secondaryKey=${SECONDARY_ACCOUNT_KEY}

dynamic.collection.name=spel-property-collection
# Populate query metrics
cosmos.queryMetricsEnabled=true
```

# <a name="doc-ops"></a>[Belge Ops](#tab/docs)

### <a name="document-operations"></a>Belge işlemleri

[Oluşturma](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Create)]

[Sil](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Delete)]

# <a name="query"></a>[Sorgu](#tab/queries)

### <a name="query"></a>Sorgu

[Sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Query)]

---

## <a name="helpful-content"></a>Yardımcı içerik

| İçerik | Spring Data Framework v 2.2 | Spring Data Framework v 2.3 |
|---|---|
| **SDK indirmesi** | [Maven](https://mvnrepository.com/artifact/com.azure/azure-spring-data-2-2-cosmos) | [Maven](https://mvnrepository.com/artifact/com.azure/azure-spring-data-2-3-cosmos) |
|**SDK 'ya katkıda bulunma** | [Spring Data Azure Cosmos DB GitHub 'da depo](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-2-2-cosmos) | [Spring Data Azure Cosmos DB GitHub 'da depo](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-2-3-cosmos) | 
|**Öğretici**| [GitHub 'da Spring Data Azure Cosmos DB öğreticisi](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/tree/main/azure-spring-data-2-2-cosmos-java-getting-started) | [GitHub 'da Spring Data Azure Cosmos DB öğreticisi](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/tree/main/azure-spring-data-2-3-cosmos-java-getting-started) |

## <a name="release-history"></a>Yayın geçmişi

### <a name="300-beta2-unreleased"></a>3.0.0-Beta. 2 (serbest bırakılmamış)

### <a name="300-beta1-2020-08-17"></a>3.0.0-Beta. 1 (2020-08-17)
#### <a name="new-features"></a>Yeni özellikler
* Grup KIMLIĞI ile güncelleştirildi `com.azure` .
* Yapıt KIMLIĞI ile güncelleştirildi `azure-spring-data-2-3-cosmos` .
* Azure-Cosmos SDK bağımlılığı güncelleştirildi `4.3.2-beta.2` .
* Denetim varlıkları desteği-createdBy, createdDate, lastModifiedBy ve lastModifiedDate açıklamalı alanlarının otomatik yönetimi.
* `@GeneratedValue` türündeki KIMLIK alanları için otomatik kimlik oluşturmaya yönelik ek açıklama desteği `String` .
* Birden çok veritabanına sahip tek Cosmos hesabı ve birden çok veritabanı içeren birden çok Cosmos hesabı için çoklu veritabanı yapılandırma desteği.
* `@Version`Herhangi bir dize alanı üzerinde ek açıklama desteği.
* Güncelleştirilmiş eşitleme API 'Leri, türü `Iterable` yerine türlerine döndürülür `List` .
* `CosmosClientBuilder`Cosmos SDK 'den Spring çekirdeklere to Class olarak sunulur `@Configuration` .
* `CosmosConfig`Sorgu ölçümleri ve yanıt tanılama işlemcisi uygulamasını içerecek şekilde güncelleştirildi.
* `Optional`Tek sonuç sorguları için veri türü döndürme desteği.
#### <a name="renames"></a>Onların
* `CosmosDbFactory``CosmosFactory`.
* `CosmosDBConfig``CosmosConfig`.
* `CosmosDBAccessException``CosmosAccessException`.
* `Document` ek açıklamanın ek açıklaması `Container` .
* `DocumentIndexingPolicy` ek açıklamanın ek açıklaması `CosmosIndexingPolicy` .
* `DocumentQuery``CosmosQuery`.
* Application. Properties bayrağı `populateQueryMetrics` `queryMetricsEnabled` .
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* `Parallel`Netty g/ç iş parçacıklarını engellemeyi önlemek için tanılama günlüğü görevini iş parçacığına zamanlama.
* Silme işleminde iyimser kilitleme düzeltildi.
* Yan tümce için kaçış sorgularıyla sorun düzeltildi `IN` .
* `long`İçin veri türüne izin vererek sorun düzeltildi `@Id` .
* `boolean` `long` `int` `double` Ek açıklama için veri türleri olarak izin vererek sorun düzeltildi `@PartitionKey` .
* `IgnoreCase`  &  `AllIgnoreCase` Büyük/küçük harf sorgularını yoksay için sabit anahtar sözcükler.
* Otomatik olarak kapsayıcı oluştururken 4000 varsayılan istek birimi değeri kaldırıldı.

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Sonraki adımlar
Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

Spring Framework hakkında daha fazla bilgi için bkz. [proje giriş sayfası](https://spring.io/projects/spring-framework).

Spring Boot hakkında daha fazla bilgi için bkz. [proje giriş sayfası](https://spring.io/projects/spring-boot).

Yay verileri hakkında daha fazla bilgi için bkz. [proje giriş sayfası](https://spring.io/projects/spring-data).