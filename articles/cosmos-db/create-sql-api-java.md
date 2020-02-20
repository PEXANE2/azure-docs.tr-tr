---
title: Hızlı başlangıç-Azure Cosmos DB kullanarak bir belge veritabanı oluşturmak için Java kullanma
description: Bu hızlı başlangıçta, SQL API 'sine bağlanmak ve sorgu Azure Cosmos DB sorgulamak için kullanabileceğiniz bir Java kod örneği sunulmaktadır
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: quickstart
ms.date: 10/31/2019
ms.author: sngun
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: bd7801c84860ddba3c3991bce9352c595adb123f
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77469053"
---
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-sql-api-data"></a>Hızlı başlangıç: Azure Cosmos DB SQL API verilerini yönetmek için bir Java uygulaması oluşturma


> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta, Azure portal Azure Cosmos DB bir SQL API hesabı oluşturup, GitHub 'dan kopyalanmış bir Java uygulamasını kullanarak yönetirsiniz. İlk olarak, Azure portal kullanarak bir Azure Cosmos DB SQL API hesabı oluşturun, sonra SQL Java SDK 'sını kullanarak bir Java uygulaması oluşturun ve ardından Java uygulamasını kullanarak Cosmos DB hesabınıza kaynak eklersiniz. Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Veya Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) . [Azure Cosmos DB öykünücüsü](https://aka.ms/cosmosdb-emulator) ' nü BIR `https://localhost:8081` URI 'siyle ve anahtar `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`de kullanabilirsiniz.
- [Java Development Kit (JDK) 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk). `JAVA_HOME` ortam değişkeninizi JDK 'nin yüklü olduğu klasöre getirin.
- [Maven ikili Arşivi](https://maven.apache.org/download.cgi). Ubuntu 'da, Maven 'yi yüklemek için `apt-get install maven` çalıştırın.
- [Git](https://www.git-scm.com/downloads). Ubuntu 'da git 'i yüklemek için `sudo apt-get install git` çalıştırın.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile bir SQL API hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Kapsayıcı ekleme

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. GitHub'dan bir SQL API'si uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-java-getting-started.git
```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Uygulamayı çalıştırma](#run-the-app) konusuna atlayabilirsiniz. 

### <a name="managing-database-resources-using-the-synchronous-sync-api"></a>Zaman uyumlu (eşitleme) API 'sini kullanarak veritabanı kaynaklarını yönetme

* `CosmosClient` başlatma. `CosmosClient`, Azure Cosmos veritabanı hizmeti için istemci tarafı mantıksal temsili sağlar. Bu istemci, istekleri hizmete göre yapılandırmak ve çalıştırmak için kullanılır.
    
    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateSyncClient)]

* `CosmosDatabase` oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateDatabaseIfNotExists)]

* `CosmosContainer` oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateContainerIfNotExists)]

* `createItem` yöntemi kullanılarak öğe oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateItem)]
   
* Nokta okuma `readItem` yöntemi kullanılarak gerçekleştirilir.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=ReadItem)]

* JSON üzerinden SQL sorguları `queryItems` yöntemi kullanılarak gerçekleştirilir.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=QueryItems)]

### <a name="managing-database-resources-using-the-asynchronous-async-api"></a>Zaman uyumsuz (Async) API 'sini kullanarak veritabanı kaynaklarını yönetme

* Zaman uyumsuz API çağrıları, sunucudan yanıt beklemeden hemen döndürülür. Bu, aşağıdaki kod parçacıkları, zaman uyumsuz API kullanarak önceki yönetim görevlerinin tümünü yerine getirmeye yönelik uygun tasarım düzenlerini gösterir.

* `CosmosAsyncClient` başlatma. `CosmosAsyncClient`, Azure Cosmos veritabanı hizmeti için istemci tarafı mantıksal temsili sağlar. Bu istemci, hizmete karşı zaman uyumsuz istekleri yapılandırmak ve yürütmek için kullanılır.
    
    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=CreateAsyncClient)]

* `CosmosAsyncDatabase` oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=CreateDatabaseIfNotExists)]

* `CosmosAsyncContainer` oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=CreateContainerIfNotExists)]

* Eşitleme API 'sinde olduğu gibi, öğe oluşturma `createItem` yöntemi kullanılarak gerçekleştirilir. Bu örnek, istekleri veren ve bildirimleri yazdıran bir reaktif akışa abone olarak çok sayıda zaman uyumsuz `createItem` isteği nasıl verimli bir şekilde verecağını gösterir. Bu basit örnek tamamlanana ve sonlanacak şekilde çalıştığından, programın öğe oluşturma sırasında sonlandırmadığından emin olmak için `CountDownLatch` örnekleri kullanılır. **Uygun zaman uyumsuz bir programlama uygulaması, zaman uyumsuz çağrılar üzerinde engellenmemelidir; gerçekçi olmayan kullanım örnekleri istekleri, sonsuza kadar çalıştırılan bir Main () döngüsünden oluşturulur ve zaman uyumsuz çağrılarda mandal ihtiyacını ortadan kaldırır.**

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=CreateItem)]
   
* Eşitleme API 'sinde olduğu gibi, nokta okumaları `readItem` yöntemi kullanılarak gerçekleştirilir.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=ReadItem)]

* Eşitleme API 'sinde olduğu gibi, JSON üzerinden SQL sorguları `queryItems` yöntemi kullanılarak gerçekleştirilir.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=QueryItems)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Bu adımda Azure portala dönerek bağlantı dizesi bilgilerinizi alın ve uç nokta bilgileriyle uygulamayı çalıştırın. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır.

1. git terminal penceresinde `cd` komutuyla örnek kod klasörüne gidin.

    ```bash
    cd azure-cosmos-java-getting-started
    ```

2. Git terminal penceresinde aşağıdaki komutu kullanarak gerekli Java paketlerini yükleyin.

    ```bash
    mvn package
    ```

3. Git Terminal penceresinde, Java uygulamasını başlatmak için aşağıdaki komutu kullanın. (SYNCASYNCMODE 'u `sync` ile değiştirin veya `async`, çalıştırmak istediğiniz örnek koda göre YOUR_COSMOS_DB_HOSTNAME, portaldan alıntı yapılan URI değeriyle değiştirin ve YOUR_COSMOS_DB_MASTER_KEY, portaldan alıntı yapılan birincil anahtarla değiştirin)

    ```bash
    mvn exec:java@SYNCASYNCMODE -DACCOUNT_HOST=YOUR_COSMOS_DB_HOSTNAME -DACCOUNT_KEY=YOUR_COSMOS_DB_MASTER_KEY

    ```

    Terminal penceresinde, FamilyDB veritabanının oluşturulduğunu belirten bir bildirim görüntülenir. 
    
4. Uygulama `AzureSampleFamilyDB` adlı veritabanı oluşturuyor
5. Uygulama, adı `FamilyContainer` bir kapsayıcı oluşturuyor
6. Uygulama, nesne kimliklerini ve bölüm anahtarı değerini (örneğimizde soyadı olan) kullanarak nokta okuma işlemleri gerçekleştirir. 
7. Uygulama, içindeki soyadı (' Andersen ', ' Wakefield ', ' Johnson ') olan tüm aileleri almak için öğeleri sorgular.

7. Uygulama, oluşturulan kaynakları silmez. [Kaynakları temizlemek](#clean-up-resources) için portala dönün.  Ücret oluşmaması için kaynakları hesabınızdan silin.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Cosmos DB bir SQL API hesabı oluşturmayı, Veri Gezgini kullanarak bir belge veritabanı ve kapsayıcı oluşturmayı ve aynı şeyi programlı bir şekilde yapmak için bir Java uygulamasını çalıştırmayı öğrendiniz. Artık Azure Cosmos DB hesabınıza daha fazla veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)
