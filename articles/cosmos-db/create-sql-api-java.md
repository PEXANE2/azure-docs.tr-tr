---
title: Quickstart - Azure Cosmos DB kullanarak belge veritabanı oluşturmak için Java'yı kullanın
description: Bu hızlı başlatma, Azure Cosmos DB SQL API'ye bağlanmak ve sorgulamak için kullanabileceğiniz bir Java kodu örneği sunar
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: quickstart
ms.date: 10/31/2019
ms.author: sngun
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 489f241453436190213b99cb4e7be0688a8b6237
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240180"
---
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-sql-api-data"></a>Hızlı başlangıç: Azure Cosmos DB SQL API verilerini yönetmek için bir Java uygulaması oluşturun


> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta, Azure portalından ve GitHub'dan klonlanmış bir Java uygulamasını kullanarak bir Azure Cosmos DB SQL API hesabı oluşturur ve yönetirsiniz. Önce Azure portalını kullanarak bir Azure Cosmos DB SQL API hesabı oluşturursunuz, ardından SQL Java SDK'yı kullanarak bir Java uygulaması oluşturursunuz ve ardından Java uygulamasını kullanarak Cosmos DB hesabınıza kaynak eklersiniz. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/) Ayrıca Bir URI `https://localhost:8081` ve anahtarı `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`ile Azure [Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator) kullanabilirsiniz.
- [Java Geliştirme Kiti (JDK) 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk). Ortam `JAVA_HOME` değişkeninizi JDK'nın yüklü olduğu klasöre yönlendirin.
- [Bir Maven ikili arşiv](https://maven.apache.org/download.cgi). Ubuntu'da, `apt-get install maven` Maven'i yüklemek için koş.
- [Git.](https://www.git-scm.com/downloads) Ubuntu'da `sudo apt-get install git` Git'i yüklemek için koş.

## <a name="introductory-notes"></a>Tanıtım notları

*Cosmos DB hesabının yapısı.* API veya programlama dilinden bağımsız olarak, bir Cosmos DB *hesabı* sıfır veya daha fazla *veritabanları*içerir, bir *veritabanı* (DB) sıfır veya daha fazla *kapsayıcı*içerir ve bir *kapsayıcı* aşağıdaki diyagramda gösterildiği gibi sıfır veya daha fazla öğe içerir:

![Azure Cosmos hesap varlıkları](./media/databases-containers-items/cosmos-entities.png)

Burada veritabanları, kapsayıcılar ve öğeler hakkında daha fazla bilgi [edinebilirsiniz.](databases-containers-items.md) Birkaç önemli özellikleri kapsayıcı düzeyinde tanımlanır, aralarında *sağlanan iş ve* *bölüm anahtarı.* 

Sağlanan iş, parasal bir bedeli olan ve hesabın işletme maliyetinde önemli bir belirleyici faktör olan İstek Birimleri *(RUS)* cinsinden ölçülür. Sağlanan iş ortası, kapsayıcı başına taneciklilik veya veritabanı başına tanecikliolarak seçilebilir, ancak kapsayıcı düzeyinde iş ortası belirtimi genellikle tercih edilir. [Burada](set-throughput.md) iş artışı hakkında daha fazla bilgi edinebilirsiniz.

Öğeler bir Cosmos DB kapsayıcısına ekildikçe, istekleri işlemek için daha fazla depolama alanı ve bilgi işlem ekleyerek veritabanı yatay olarak büyür. Depolama ve bilgi işlem *kapasitesi, bölümler*olarak bilinen ayrı birimlere eklenir ve belgelerinizde her belgeyi bir bölümle eşleyen bölüm anahtarı olarak bir alan seçmeniz gerekir. Bölümlerin yönetilme şekli, her bölüme bölüm anahtar değerleri aralığından kabaca eşit bir dilim atanmasıdır; bu nedenle nispeten rasgele veya eşit dağıtılmış bir bölüm anahtarı seçmeniz önerilir. Aksi takdirde, bazı bölümler önemli ölçüde daha fazla istek görür *(sıcak bölüm*) diğer bölümler önemli ölçüde daha az istek *(soğuk bölüm)* görmek ise , ve bu kaçınılmalıdır. [Burada](partitioning-overview.md)bölümleme hakkında daha fazla bilgi edinebilirsiniz.

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

### <a name="managing-database-resources-using-the-synchronous-sync-api"></a>Senkron (eşitleme) API'sini kullanarak veritabanı kaynaklarını yönetme

* `CosmosClient` başlatma. Azure `CosmosClient` Cosmos veritabanı hizmeti için istemci tarafı mantıksal gösterimi sağlar. Bu istemci, istekleri hizmete göre yapılandırmak ve çalıştırmak için kullanılır.
    
    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateSyncClient)]

* `CosmosDatabase`Oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateDatabaseIfNotExists)]

* `CosmosContainer`Oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateContainerIfNotExists)]

* `createItem` Yöntemi kullanarak öğe oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateItem)]
   
* Nokta okuma yöntemi `readItem` kullanılarak gerçekleştirilir.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=ReadItem)]

* JSON üzerinden SQL sorguları `queryItems` yöntem kullanılarak gerçekleştirilir.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=QueryItems)]

### <a name="managing-database-resources-using-the-asynchronous-async-api"></a>Asynchronous (async) API kullanarak veritabanı kaynaklarını yönetme

* Async API çağrıları sunucudan yanıt beklemeden hemen geri döner. Bunun ışığında, aşağıdaki kod parçacıkları async API kullanarak önceki yönetim görevlerinin tümünü gerçekleştirmek için uygun tasarım desenleri gösterir.

* `CosmosAsyncClient` başlatma. Azure `CosmosAsyncClient` Cosmos veritabanı hizmeti için istemci tarafı mantıksal gösterimi sağlar. Bu istemci, hizmete karşı eşzamanlı istekleri yapılandırmak ve yürütmek için kullanılır.
    
    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=CreateAsyncClient)]

* `CosmosAsyncDatabase`Oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateDatabaseIfNotExists)]

* `CosmosAsyncContainer`Oluşturma.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateContainerIfNotExists)]

* Eşitleme API'nde olduğu gibi, madde `createItem` oluşturma yöntemi kullanılarak gerçekleştirilir. Bu örnek, istekleri veren ve `createItem` bildirimleri yazdıran bir Reaktif Akış'a abone olarak çok sayıda async isteğinin nasıl verimli bir şekilde verilebildiğini gösterir. Bu basit örnek tamamlanmak ve `CountDownLatch` sonlandırmak için çalıştığından, örnekler, öğe oluşturma sırasında programın sonlandırmamasını sağlamak için kullanılır. **Uygun asynchronous programlama uygulaması async aramaları engellemek için değil - gerçekçi kullanım örnekleri istekleri süresiz olarak yürüten bir ana() döngü oluşturulur, async aramaları mandal ihtiyacını ortadan kaldırarak.**

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=CreateItem)]
   
* Eşitleme API'nde olduğu gibi, `readItem` nokta okumayöntemi kullanılarak gerçekleştirilir.

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/async/AsyncMain.java?name=ReadItem)]

* Eşitleme API'nde olduğu gibi, JSON üzerindeki `queryItems` SQL sorguları yöntem kullanılarak gerçekleştirilir.

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

3. Git terminali penceresinde, Java uygulamasını başlatmak için aşağıdaki komutu kullanın `sync` `async` (SYNCASYNCMODE'u çalıştırmak istediğiniz örnek kodu değiştirin veya bağlı olarak, portaldan alıntılanan URI değeriyle YOUR_COSMOS_DB_HOSTNAME değiştirin ve YOUR_COSMOS_DB_MASTER_KEY portaldan alıntılanan birincil anahtarla değiştirin)

    ```bash
    mvn exec:java@SYNCASYNCMODE -DACCOUNT_HOST=YOUR_COSMOS_DB_HOSTNAME -DACCOUNT_KEY=YOUR_COSMOS_DB_MASTER_KEY

    ```

    Terminal penceresinde, FamilyDB veritabanının oluşturulduğunu belirten bir bildirim görüntülenir. 
    
4. Uygulama ad ile veritabanı oluşturur`AzureSampleFamilyDB`
5. Uygulama adile kapsayıcı oluşturur`FamilyContainer`
6. Uygulama, nesne adlarını ve bölüm anahtar değerini kullanarak nokta okumalarını gerçekleştirir (örneğimizde soyadı dır). 
7. Uygulama, soyadı soyadı olan tüm aileleri ('Andersen', 'Wakefield', 'Johnson') almak için öğeleri sorgular

7. Uygulama, oluşturulan kaynakları silmez. [Kaynakları temizlemek](#clean-up-resources) için portala dönün.  Ücret oluşmaması için kaynakları hesabınızdan silin.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Bir Azure Cosmos DB SQL API hesabı oluşturmayı, Veri Gezgini'ni kullanarak bir belge veritabanı ve kapsayıcı oluşturmayı ve aynı şeyi programlı olarak yapmak için bir Java uygulaması çalıştırmayı öğrendiniz. Artık Azure Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)
