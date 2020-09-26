---
title: 'Azure Cosmos DB SQL API: Spring Data v3 örnekleri'
description: CRUD işlemleri de dahil olmak üzere Azure Cosmos DB SQL API 'sini kullanarak genel görevler için GitHub 'da Data v3 örneklerini bulun.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 09/23/2020
ms.custom: devx-track-java
ms.author: anfeldma
ms.openlocfilehash: adbec15e252a7b5cbd55f1b571d55455fb313a01
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91348724"
---
# <a name="azure-cosmos-db-sql-api-spring-data-azure-cosmos-db-v3-examples"></a>Azure Cosmos DB SQL API: Spring Data Azure Cosmos DB v3 örnekleri

> [!div class="op_single_selector"]
> * [.NET v2 SDK örnekleri](sql-api-dotnet-samples.md)
> * [.NET v3 SDK örnekleri](sql-api-dotnet-v3sdk-samples.md)
> * [Java v4 SDK örnekleri](sql-api-java-sdk-samples.md)
> * [Spring Data v3 SDK örnekleri](sql-api-spring-data-sdk-samples.md)
> * [Node.js Örnekleri](sql-api-nodejs-samples.md)
> * [Python Örnekleri](sql-api-python-samples.md)
> * [Azure Kod Örneği Galerisi](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

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

> [!IMPORTANT]  
>[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
>  
>- [Visual Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, her ay size ücretli Azure hizmetleri için kullanabileceğiniz krediler verir.
>
>[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]
>

Azure Cosmos DB kaynaklarında CRUD işlemlerini ve diğer genel işlemleri gerçekleştiren en son örnek uygulamalar, [Azure-Spring-Data-Cosmos-Java-SQL-API-Samples](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples) GitHub deposuna dahil edilmiştir. Bu makalede aşağıdakiler sunulmaktadır:

* Örnek her örnekteki görevlere yönelik bağlantılar proje dosyaları Azure Cosmos DB. 
* İlgili API başvurusu içeriğine bağlantılar.

**Önkoşullar**

Bu örnek uygulamayı çalıştırmanız için aşağıdakiler gereklidir:

* Java Development Kit 8
* Yay verileri Azure Cosmos DB v3

İsteğe bağlı olarak Maven kullanarak, en son Spring Data Azure Cosmos DB, projenizde kullanım için v3 ikililerini kullanabilirsiniz. Maven tüm gerekli bağımlılıkları otomatik olarak ekler. Aksi takdirde, **pom.xml** dosyasında listelenen bağımlılıkları doğrudan indirebilir ve yapı yolunuza ekleyebilirsiniz.

```bash
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-spring-data-cosmos</artifactId>
    <version>LATEST</version>
</dependency>
```

**Örnek uygulamaları çalıştırma**

Örnek depoyu kopyalama:
```bash
$ git clone https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples

$ cd azure-spring-data-cosmos-java-sql-api-samples
```

Örnekleri bir IDE (tutulma, IntelliJ veya VSCODE) kullanarak veya Maven kullanarak komut satırından çalıştırabilirsiniz.

**Application. Properties** içinde bu ortam değişkenlerinin ayarlanması gerekir

```xml
cosmos.uri=${ACCOUNT_HOST}
cosmos.key=${ACCOUNT_KEY}
cosmos.secondaryKey=${SECONDARY_ACCOUNT_KEY}

dynamic.collection.name=spel-property-collection
# Populate query metrics
cosmos.queryMetricsEnabled=true
```

örneklere, veritabanlarına ve kapsayıcılarınıza okuma/yazma erişimi vermek için.

IDE 'niz, Spring Data örnek kodunu yürütme özelliği sağlayabilir. Aksi takdirde, örneği yürütmek için aşağıdaki terminal komutunu kullanabilirsiniz:

```bash
mvn spring-boot:run
```

## <a name="document-crud-examples"></a>Belge CRUD örnekleri
[Örnek](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/SampleApplication.java) dosyası, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos belgeleri hakkında bilgi edinmek için bkz. [veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md) kavramsal makalesi.

| Görev | API başvurusu |
| --- | --- |
| [Belge oluşturma](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/SampleApplication.java#L46-L47) | CosmosRepository. Save |
| [Bir belgeyi KIMLIĞE göre oku](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/SampleApplication.java#L56-L58) | CosmosRepository. derivedQueryMethod |
| [Tüm belgeleri Sil](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/SampleApplication.java#L39-L41) | CosmosRepository. deleteAll |

## <a name="derived-query-method-examples"></a>Türetilmiş sorgu yöntemi örnekleri
[Örnek](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/SampleApplication.java) dosyası, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB sorguları hakkında bilgi edinmek için, [Spring makalesindeki Baeldung 'ın türetilmiş sorgu yöntemlerini](https://www.baeldung.com/spring-data-derived-queries) okumayı yararlı bulabilirsiniz.

| [Belgeler Için sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/SampleApplication.java#L73-L77) | CosmosRepository. derivedQueryMethod |

## <a name="custom-query-examples"></a>Özel sorgu örnekleri
[Örnek](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/SampleApplication.java) dosyası, SQL sorgu dilbilgisini kullanarak aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB SQL sorgu başvurusu hakkında bilgi edinmek için, bkz. [SQL sorgu örnekleri Azure Cosmos DB](how-to-sql-query.md). 


| Görev | API başvurusu |
| --- | --- |
| [Tüm belgeler için sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L20-L22) | @Query ek açıklama |
| [== kullanan eşitlik için sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L24-L26) | @Query ek açıklama |
| [!= ve NOT kullanan eşitsizlik için sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L28-L38) | @Query ek açıklama |
| [>, <, >=, <= gibi aralık işleçlerini kullanarak sorgulama](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L40-L42) | @Query ek açıklama |
| [Dizelere karşı aralık işleçlerini kullanarak sorgulama](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L44-L46) | @Query ek açıklama |
| [SıRALAMA ölçütü olan sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L48-L50) | @Query ek açıklama |
| [FARKLı sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L52-L54) | @Query ek açıklama |
| [Toplama işlevleriyle sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L56-L62) | @Query ek açıklama |
| [alt belgelerle çalışma](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L64-L66) | @Query ek açıklama |
| [Belgeler arası Birleştirmeler ile sorgulama](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L68-L85) | @Query ek açıklama |
| [String, Math ve Array işleçleri ile sorgu](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/springexamples/quickstart/sync/UserRepository.java#L87-L97) | @Query ek açıklama |