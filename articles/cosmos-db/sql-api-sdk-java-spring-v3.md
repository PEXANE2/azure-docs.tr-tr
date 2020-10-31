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
ms.openlocfilehash: 64054a2bb5c1f7e17eef87c3babb28137b6c912a
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097133"
---
# <a name="spring-data-azure-cosmos-db-v3-for-core-sql-api-release-notes-and-resources"></a>Yay verileri Azure Cosmos DB v3 for Core (SQL) API: sürüm notları ve kaynakları
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET Değişiklik Akışı SDK'sı v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK’sı v4](sql-api-sdk-java-v4.md)
> * [Zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md)
> * [Zaman uyumlu Java SDK v2](sql-api-sdk-java.md)
> * [Spring Verileri v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Verileri v3](sql-api-sdk-java-spring-v3.md)
> * [Spark Bağlayıcısı](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST kaynak sağlayıcısı](/rest/api/cosmos-db-resource-provider/)
> * [SQL](./sql-query-getting-started.md)
> * [Toplu yürütücü - .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü - Java](sql-api-sdk-bulk-executor-java.md)

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
> * [Azure Cosmos DB ile Apache Cassandra için yay verileri](/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Azure Cosmos DB ile Spring Data MongoDB](/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Yay verileri Gremlin Azure Cosmos DB](/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
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

---

## <a name="resources"></a>Kaynaklar

* **SDK 'Ya katkıda bulunun** : [Spring Data Azure Cosmos DB GitHub 'da depo](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos)

* **Öğretici** : [Spring Data Azure Cosmos DB GitHub 'da eğitim](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started) 

[!INCLUDE[Release notes](~/azure-sdk-for-java-cosmos-db/sdk/cosmos/azure-spring-data-cosmos/CHANGELOG.md)]

## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)hakkında daha fazla bilgi edinin.

[Spring Framework](https://spring.io/projects/spring-framework)hakkında daha fazla bilgi edinin.

[Spring Boot](https://spring.io/projects/spring-boot)hakkında daha fazla bilgi edinin.

[Yay verileri](https://spring.io/projects/spring-data)hakkında daha fazla bilgi edinin.