---
title: Spring Data Azure Cosmos DB v3 for SQL API sürüm notları ve kaynakları
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve SQL zaman uyumsuz Java SDK 'sının her Azure Cosmos DB bir sürümü arasında yapılan değişiklikler dahil olmak üzere, SQL API için Azure Cosmos DB v3 verileri hakkında bilgi edinin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/18/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 9e5b92918d93109183740be555bb805877862407
ms.sourcegitcommit: 0fd1f3fe7817ad44d878d580ec167e1508051795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2020
ms.locfileid: "90817879"
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

Çekirdek (SQL) için Spring Data Azure Cosmos DB sürüm 3, geliştiricilerin Spring uygulamalarında Azure Cosmos DB kullanmasına izin verir. Spring Data Azure Cosmos DB veritabanlarını ve koleksiyonları işlemek, belgelerle çalışmak ve sorgu vermek için Spring Data arabirimini kullanıma sunar. Aynı Maven yapıtı içinde hem eşitleme hem de Async (reaktif) API 'Leri desteklenir. 

Spring Data Azure Cosmos DB yay veri çerçevesine bir bağımlılığı vardır. Azure Cosmos DB SDK ekibi, yay veri sürümleri 2,2 ve 2,3 için Maven yapıtları yayınlar.

[Spring Framework](https://spring.io/projects/spring-framework) , Java uygulaması geliştirmeyi kolaylaştırın bir programlama ve yapılandırma modelidir. Yay, bağımlılık ekleme kullanarak uygulamaların "sıhhi tesisat" düzeyini kolaylaştırır. Uygulama oluşturmayı ve test etmeyi daha basit hale getiren yay gibi birçok geliştirici. [Spring Boot](https://spring.io/projects/spring-boot) , sıhhi tesisat 'nin bu işlemesini, Web uygulaması ve mikro hizmet geliştirmeye yönelik bir gözle genişletiyor. [Spring Data](https://spring.io/projects/spring-data) , Spring veya Spring Boot uygulaması bağlamından Azure Cosmos DB gibi veri depolarına erişmek için bir programlama modeli ve çerçevesidir. 

Spring Data Azure Cosmos DB, [Azure Spring Cloud](https://azure.microsoft.com/services/spring-cloud/) uygulamalarınızda kullanabilirsiniz.

> [!IMPORTANT]  
> Bu sürüm notları, Spring Data Azure Cosmos DB sürüm 3 ' ü yöneliktir. [Sürüm 2 için sürüm notlarını buradan](sql-api-sdk-java-spring-v2.md)bulabilirsiniz. 
>
> Spring Data Azure Cosmos DB yalnızca SQL API 'sini destekler.
>
> Diğer Azure Cosmos DB API 'Lerinde yay verileri hakkında bilgi için şu makalelere bakın:
> * [Azure Cosmos DB ile Apache Cassandra için yay verileri](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Azure Cosmos DB ile Spring Data MongoDB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Yay verileri Gremlin Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
>

## <a name="start-here"></a>Buradan başlayın

# <a name="explore"></a>[Keşfetme](#tab/explore)

<img src="media/sql-api-sdk-java-spring-v3/up-arrow.png" alt="explore the tabs above" width="80"/>

#### <a name="these-tabs-contain-basic-spring-data-azure-cosmos-db-samples"></a>Bu sekmeler, temel Spring Data Azure Cosmos DB örnekleri içerir.

# <a name="pomxml"></a>[pom.xml](#tab/pom)

### <a name="configure-dependencies"></a>Bağımlılıkları yapılandırma

  ```xml
  <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-spring-data-cosmos</artifactId>
      <version>latest</version>
  </dependency>
  ```

# <a name="connect"></a>[Bağlan](#tab/connect)

### <a name="connect"></a>Bağlan

Azure Cosmos DB hesabı ve kapsayıcı ayrıntılarını belirtin. Spring Data Azure Cosmos DB istemciyi otomatik olarak oluşturur ve kapsayıcıya bağlar.

[Application. Properties](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-cosmos-java-getting-started/src/main/resources/application.properties):
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

Oluştur:
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Create":::

Sil:
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Delete":::

# <a name="query"></a>[Sorgu](#tab/queries)

Sorgu:
:::code language="java" source="~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java" ID="Query":::

---

## <a name="resources"></a>Kaynaklar

* **SDK 'Ya katkıda bulunun**: [Spring Data Azure Cosmos DB GitHub 'da depo](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos)

* **Öğretici**: [Spring Data Azure Cosmos DB GitHub 'da eğitim](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started) 

## <a name="release-history"></a>Yayın geçmişi

### <a name="300-beta2-september-17-2020"></a>3.0.0-Beta. 2 (17 Eylül 2020)

#### <a name="new-features"></a>Yeni özellikler

* Yapıt kimliği ile güncelleştirildi `azure-spring-data-cosmos` .
* Azure-Cosmos bağımlılığı güncelleştirildi `4.5.0` .
* `Query Annotation` Yerel sorgular için destek.
* Java 11 desteği.
* `partitionKeyPath`Ek açıklama içindeki alanı açığa çıkararak Iç Içe bölüm anahtarı için destek eklendi `@Container` .
* `limit` `top` Depo API 'leri tanımlarken kullanılmasına izin veren ve bu sorgu türü için destek eklendi `first` .

#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

* Ek açıklamayla kullanıldığında, iç içe geçmiş bölüm anahtarı hatası düzeltildi `@GeneratedValue` .

### <a name="300-beta1-august-17-2020"></a>3.0.0-Beta. 1 (17 Ağustos 2020)

#### <a name="new-features"></a>Yeni özellikler

* Grup KIMLIĞINI ile güncelleştirir `com.azure` .
* Yapıt KIMLIĞINI ile güncelleştirir `azure-spring-data-2-3-cosmos` .
* Azure-Cosmos SDK bağımlılığını güncelleştirir `4.3.2-beta.2` .
* Denetim varlıkları için destek ekler:,,, `createdBy` `createdDate` `lastModifiedBy` ve `lastModifiedDate` açıklamalı alanlarının otomatik yönetimi.
* `@GeneratedValue`TÜRÜNDEKI kimlik alanları için OTOMATIK kimlik oluşturma desteği ekler `String` .
* Birden çok veritabanına sahip tek Azure Cosmos DB hesapları için çoklu veritabanı yapılandırma desteği ve birden çok veritabanı içeren birden çok Azure Cosmos DB hesabı ekler.
* `@Version`Herhangi bir dize alanına ek açıklama desteği ekler.
* Eşitleme API 'SI dönüş türlerini `Iterable` yerine türlerine güncelleştirir `List` .
* `CosmosClientBuilder`Azure Cosmos DB SDK 'dan, sınıfa Spring çekirdeklere olarak sunar `@Configuration` .
* `CosmosConfig`Sorgu ölçümleri ve yanıt tanılama işlemcisi uygulamasını içerecek güncelleştirmeler.
* `Optional`Tek sonuç sorguları için veri türünü döndürmeye yönelik destek ekler.

#### <a name="renames"></a>Onların

* `CosmosDbFactory``CosmosFactory`.
* `CosmosDBConfig``CosmosConfig`.
* `CosmosDBAccessException``CosmosAccessException`.
* `Document` ek açıklamanın ek açıklaması `Container` .
* `DocumentIndexingPolicy` ek açıklamanın ek açıklaması `CosmosIndexingPolicy` .
* `DocumentQuery``CosmosQuery`.
* Application. Properties bayrağı `populateQueryMetrics` `queryMetricsEnabled` .

#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

* `Parallel`Netty g/ç iş parçacıklarını engellemeyi önlemek için tanılama günlüğü görevinin iş parçacıklarına zamanlaması.
* Silme işleminde iyimser kilitlemeyi düzeltir.
* Yan tümce için kaçış sorgularıyla sorunu düzeltir `IN` .
* `long`İçin veri türüne izin vererek sorunu düzeltir `@Id` .
* `boolean` `long` `int` `double` Ek açıklama için veri türlerine izin vererek,, ve gibi sorunları düzeltir `@PartitionKey` .
* `IgnoreCase` `AllIgnoreCase` Durum sorgularını yoksay için düzeltmeler ve anahtar sözcükler.
* Kapsayıcılar otomatik olarak oluşturulduğunda, varsayılan istek birimi değeri olan 4.000 ' i kaldırır.

## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)hakkında daha fazla bilgi edinin.

[Spring Framework](https://spring.io/projects/spring-framework)hakkında daha fazla bilgi edinin.

[Spring Boot](https://spring.io/projects/spring-boot)hakkında daha fazla bilgi edinin.

[Yay verileri](https://spring.io/projects/spring-data)hakkında daha fazla bilgi edinin.