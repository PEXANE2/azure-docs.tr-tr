---
title: 'Öğretici: SQL API hesabındaki verileri yönetmek için Azure Cosmos DB Async Java SDK ile bir Java uygulaması oluşturun'
description: Bu öğretici, Bir Async Java uygulaması kullanarak Azure Cosmos DB'deki bir SQL API hesabındaki verileri nasıl depolayıp erişireceğini gösterir.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: sngun
Customer intent: As a developer, I want to build a Java application with the Async Java SDK to access and manage Azure Cosmos DB resources so that customers can utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: 8704e399156b9cfc6b04ff47af49b956b597a539
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75444900"
---
# <a name="tutorial-build-a-java-app-with-the-async-java-sdk-to-manage-data-stored-in-a-sql-api-account"></a>Öğretici: SQL API hesabında depolanan verileri yönetmek için Async Java SDK ile bir Java uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Geliştirici olarak, NoSQL belge verilerini kullanan uygulamalarınız olabilir. Bu belge verilerini depolamak ve bunlara erişmek için Azure Cosmos DB'deki SQL API hesabını kullanabilirsiniz. Bu öğretici, belge verilerini depolamak ve yönetmek için Async Java SDK ile java uygulamasını nasıl oluşturabileceğinizi gösterir. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Cosmos hesabı oluşturma ve bağlanma
> * Çözümünüzü yapılandırma
> * Koleksiyon oluşturma
> * JSON belgeleri oluşturma
> * Koleksiyonu sorgulama

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 

* [Git.](https://git-scm.com/downloads)

* [Java Development Kit (JDK) 8+](https://aka.ms/azure-jdks).

* [Maven,](https://maven.apache.org/download.cgi)ne kadar.

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Aşağıdaki adımları kullanarak bir Azure Cosmos hesabı oluşturun:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="clone-the-github-repository"></a><a id="GitClone"></a>GitHub deposunu klonla

[Azure Cosmos DB ve Java ile Başlayın](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started)için GitHub deposunu klonla. Örneğin, yerel bir dizinden, örnek projeyi yerel olarak almak için aşağıdakileri çalıştırın.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started.git

cd azure-cosmos-db-sql-api-async-java-getting-started
cd azure-cosmosdb-get-started
```

Dizin, java `pom.xml` kaynak `src/main/java/com/microsoft/azure/cosmosdb/sample` kodu içeren bir dosya `Main.java`ve klasör içerir. Proje, belge oluşturma ve koleksiyon içindeki verileri sorgulama gibi Azure Cosmos DB ile işlemleri gerçekleştirmek için gereken kodu içerir. Dosya, `pom.xml` [Maven'deki Azure Cosmos DB Java SDK'ya](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb)bağımlılık içerir.

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-documentdb</artifactId>
  <version>2.0.0</version>
</dependency>
```

## <a name="connect-to-an-azure-cosmos-account"></a><a id="Connect"></a>Azure Cosmos hesabına bağlanma

Ardından, bitiş noktanızı ve birincil ana anahtarınızı almak için [Azure portalına](https://portal.azure.com) geri dön. Azure Cosmos DB uç noktası ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir. `AccountSettings.java` dosyasında birincil anahtar ve URI değerleri bulunur. 

Azure portalında Azure Cosmos hesabınıza gidin ve ardından **Keys'i**tıklatın. Portaldan URI ve BİRİNCİL ANAHTAR değerlerini kopyalayıp `AccountSettings.java` dosyasına yapıştırın. 

```java
public class AccountSettings 
{
  // Replace MASTER_KEY and HOST with values from your Azure Cosmos account.
    
  // <!--[SuppressMessage("Microsoft.Security", "CS002:SecretInNextLine")]-->
  public static String MASTER_KEY = System.getProperty("ACCOUNT_KEY", 
          StringUtils.defaultString(StringUtils.trimToNull(
          System.getenv().get("ACCOUNT_KEY")), "<Fill your Azure Cosmos account key>"));

  public static String HOST = System.getProperty("ACCOUNT_HOST",
           StringUtils.defaultString(StringUtils.trimToNull(
           System.getenv().get("ACCOUNT_HOST")),"<Fill your Azure Cosmos DB URI>"));
}
```

![Portaldan anahtarları alma ekran görüntüsü][keys]

## <a name="initialize-the-client-object"></a>İstemci nesnesini başlatma

"AccountSettings.java" dosyasında tanımlanan ana bilgisayar URI ve birincil anahtar değerlerini kullanarak istemci nesnesini başlatma.

```java
client = new AsyncDocumentClient.Builder()
         .withServiceEndpoint(AccountSettings.HOST)
         .withMasterKey(AccountSettings.MASTER_KEY)
         .withConnectionPolicy(ConnectionPolicy.GetDefault())
         .withConsistencyLevel(ConsistencyLevel.Session)
         .build();
```

## <a name="create-a-database"></a><a id="CreateDatabase"></a>Veritabanı oluşturma

DocumentClient sınıfının yöntemini `createDatabaseIfNotExists()` kullanarak Azure Cosmos veritabanınızı oluşturun. Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

```java
private void createDatabaseIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if database " + databaseName + " exists.");

    String databaseLink = String.format("/dbs/%s", databaseName);

    Observable<ResourceResponse<Database>> databaseReadObs = client.readDatabase(databaseLink, null);

    Observable<ResourceResponse<Database>> databaseExistenceObs = databaseReadObs
                .doOnNext(x -> {System.out.println("database " + databaseName + " already exists.");}).onErrorResumeNext(e -> {
   // if the database doesn't already exists
   // readDatabase() will result in 404 error
   if (e instanceof DocumentClientException) 
   {
       DocumentClientException de = (DocumentClientException) e;
       // if database 
       if (de.getStatusCode() == 404) {
       // if the database doesn't exist, create it.
       System.out.println("database " + databaseName + " doesn't existed," + " creating it...");
       Database dbDefinition = new Database();
       dbDefinition.setId(databaseName);
       return client.createDatabase(dbDefinition, null);
     }
     }

    // some unexpected failure in reading database happened.
    // pass the error up.
    System.err.println("Reading database " + databaseName + " failed.");
        return Observable.error(e);     
    });

   // wait for completion
   databaseExistenceObs.toCompletable().await();

   System.out.println("Checking database " + databaseName + " completed!\n");
}
```

## <a name="create-a-collection"></a><a id="CreateColl"></a>Koleksiyon oluşturma

DocumentClient sınıfının yöntemini `createDocumentCollectionIfNotExists()` kullanarak bir koleksiyon oluşturabilirsiniz. Koleksiyon, JSON belgeleri ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır.

> [!WARNING]
> **createCollection**, ayrılmış işleme ile yeni bir koleksiyon oluşturur, bu da ücret ödenmesini gerektirebilir. Daha ayrıntılı bilgi için [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.
> 
> 

```java
private void createDocumentCollectionIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if collection " + collectionName + " exists.");

    // query for a collection with a given id
    // if it exists nothing else to be done
    // if the collection doesn't exist, create it.

    String databaseLink = String.format("/dbs/%s", databaseName);

    // we know there is only single page of result (empty or with a match)
    client.queryCollections(databaseLink, new SqlQuerySpec("SELECT * FROM r where r.id = @id", 
            new SqlParameterCollection(new SqlParameter("@id", collectionName))), null).single() 
            .flatMap(page -> {
            if (page.getResults().isEmpty()) {
             // if there is no matching collection create the collection.
             DocumentCollection collection = new DocumentCollection();
             collection.setId(collectionName);
             System.out.println("Creating collection " + collectionName);

             return client.createCollection(databaseLink, collection, null);
            } 
            else {
              // collection already exists, nothing else to be done.
              System.out.println("Collection " + collectionName + "already exists");
              return Observable.empty();
            }
      }).toCompletable().await();

 System.out.println("Checking collection " + collectionName + " completed!\n");
    }
```

## <a name="create-json-documents"></a><a id="CreateDoc"></a>JSON belgeleri oluşturma

DocumentClient sınıfının createDocument yöntemini kullanarak bir belge oluşturun. Belgeler, kullanıcı tanımlı (rastgele) JSON içerikleridir. Şimdi bir veya daha fazla belge ekleyebiliriz. "src/main/java/com/microsoft/azure/cosmosdb/sample/Families.java" dosyası aile JSON belgelerini tanımlar. 

```java
public static Family getJohnsonFamilyDocument() {
     Family andersenFamily = new Family();
     andersenFamily.setId("Johnson" + System.currentTimeMillis());
     andersenFamily.setLastName("Johnson");

      Parent parent1 = new Parent();
      parent1.setFirstName("John");

      Parent parent2 = new Parent();
      parent2.setFirstName("Lili");

      return andersenFamily;
    }
```

## <a name="query-azure-cosmos-db-resources"></a><a id="Query"></a>Azure Cosmos DB kaynaklarını sorgula

Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için zengin sorguların gerçekleştirilmesini destekler. Aşağıdaki örnek kod, `queryDocuments` sql sözdizimini kullanarak Azure Cosmos DB'deki belgelerin nasıl sorgulanır yöntemini gösterir.

```java
private void executeSimpleQueryAsyncAndRegisterListenerForResult(CountDownLatch completionLatch) 
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setMaxItemCount(100);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(collectionLink,
           "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    queryObservable.subscribe(queryResultPage -> {
            System.out.println("Got a page of query result with " +
            queryResultPage.getResults().size() + " document(s)"
            + " and request charge of " + queryResultPage.getRequestCharge());
    },
   // terminal error signal
        e -> {
          e.printStackTrace();
          completionLatch.countDown();
        },

   // terminal completion signal
         () -> {
            completionLatch.countDown();
         });
}
```

## <a name="run-your-java-console-application"></a><a id="Run"></a>Java konsol uygulamanızı çalıştırın

Uygulamayı konsoldan çalıştırmak için proje klasörüne gidin ve Maven'i kullanarak derleyin:

```bash
mvn package
```

`mvn package` komutunu çalıştırdığınızda Maven’dan en yeni Azure Cosmos DB kitaplığı indirilir ve `GetStarted-0.0.1-SNAPSHOT.jar` üretilir. Ardından şu komutla uygulamayı çalıştırın:

```bash
mvn exec:java -DACCOUNT_HOST=<YOUR_COSMOS_DB_HOSTNAME> -DACCOUNT_KEY= <YOUR_COSMOS_DB_MASTER_KEY>
```

Şimdi bu NoSQL öğretici tamamladınız ve çalışan bir Java konsol uygulaması var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu, Azure Cosmos hesabını ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için, sanal makine için kaynak grubunu seçin, **Sil'i**seçin ve ardından silmek için kaynak grubunun adını onaylayın.


## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Cosmos DB'deki SQL API verilerini yönetmek için Async Java SDK ile java uygulaması oluşturmayı öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [JavaScript SDK’sı ve Azure Cosmos DB ile bir Node.js konsol uygulaması oluşturma](sql-api-nodejs-get-started.md)

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
