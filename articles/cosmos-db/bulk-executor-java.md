---
title: Toplu alma ve güncelleştirme işlemleri gerçekleştirmek için Azure Cosmos DB'deki toplu yürütme aracı Java kitaplığını kullanın
description: Toplu yürütme Java kitaplığı kullanarak Azure Cosmos DB belgelerini toplu olarak alma ve güncelleştirme
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: bf2a2385b3129ddf24ede7f6d851701186b0e33c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445705"
---
# <a name="use-bulk-executor-java-library-to-perform-bulk-operations-on-azure-cosmos-db-data"></a>Azure Cosmos DB verilerinde toplu işlemler yapmak için toplu yürütücü Java kitaplığını kullanma

Bu öğretici, Azure Cosmos DB'nin toplu yürütme aracı Java kitaplığını kullanarak Azure Cosmos DB belgelerini içe aktarma ve güncelleştirme hakkında yönergeler sağlar. Toplu yürütme kitaplığı ve bunun büyük iş artışı ve depolamadan yararlanmanıza nasıl yardımcı olduğu hakkında bilgi edinmek için [toplu yürütme alanı](bulk-executor-overview.md) genel bakış makalesine bakın. Bu öğreticide, rasgele belgeler üreten ve toplu olarak azure cosmos kapsayıcısına aktarılan bir Java uygulaması oluşturursunuz. İçe aktardıktan sonra, belgenin bazı özelliklerini toplu olarak güncelleştirebilirsiniz. 

Şu anda, toplu yürütme kitaplığı yalnızca Azure Cosmos DB SQL API ve Gremlin API hesapları tarafından desteklenir. Bu makalede, SQL API hesapları ile toplu yürütme Java kitaplığı nasıl kullanılacağı açıklanmaktadır. Gremlin API ile toplu yürütme .NET kitaplığını kullanma hakkında bilgi edinmek için Azure [Cosmos DB Gremlin API'de toplu işlemler gerçekleştirin'e](bulk-executor-graph-dotnet.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bir hesap oluşturun.  

* Azure [Cosmos DB'yi](https://azure.microsoft.com/try/cosmosdb/) Azure aboneliği olmadan, ücretsiz ve taahhütler olmadan ücretsiz olarak deneyebilirsiniz. Veya, son nokta yla [Birlikte Azure Cosmos DB Emülatörü'ni](https://docs.microsoft.com/azure/cosmos-db/local-emulator) `https://localhost:8081` kullanabilirsiniz. Birincil Anahtar, [Kimlik doğrulama istekleri](local-emulator.md#authenticating-requests) bölümünde sağlanır.  

* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks)  
  - Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.  

  - JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.

* [Maven](https://maven.apache.org/) ikili [arşivini indirin](https://maven.apache.org/download.cgi) ve [kurun](https://maven.apache.org/install.html)  
  
  - Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.

* Java quickstart makalesinin veritabanı hesabı oluşturma bölümünde açıklanan adımları kullanarak bir Azure Cosmos DB SQL API [hesabı oluşturun.](create-sql-api-java.md#create-a-database-account)

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan örnek bir Java uygulaması indirerek kodla çalışmaya geçelim. Bu uygulama, Azure Cosmos DB verilerinde toplu işlemler gerçekleştirir. Uygulamayı klonlamak için bir komut istemi açın, uygulamayı kopyalamak istediğiniz dizine gidin ve aşağıdaki komutu çalıştırın:

```
 git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started.git 
```

Klonlanmış depo ,"\azure-cosmosdb-bulkexecutor-java-getting-started\samples\bulkexecutor-sample\src\main\java\com\microsoft\azure\cosmosdb\bulkexecutor" klasörüne göre iki örnek "bulkimport" ve "bulkupdate" içerir. "Toplu alma" uygulaması rasgele belgeler oluşturur ve bunları Azure Cosmos DB'ye aktarır. "Toplu güncelleştirme" uygulaması Azure Cosmos DB'deki bazı belgeleri güncelleştirir. Sonraki bölümlerde, bu örnek uygulamaların her birinde kodu gözden geçireceğiz. 

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Azure Cosmos DB'ye toplu alma verileri

1. Azure Cosmos DB'nin bağlantı dizeleri bağımsız değişken olarak okunur ve CmdLineConfiguration.java dosyasında tanımlanan değişkenlere atanır.  

2. Sonraki DocumentClient nesnesi aşağıdaki ifadeler kullanılarak baş harfe çevrilir:  

   ```java
   ConnectionPolicy connectionPolicy = new ConnectionPolicy();
   connectionPolicy.setMaxPoolSize(1000);
   DocumentClient client = new DocumentClient(
      HOST,
      MASTER_KEY, 
      connectionPolicy,
      ConsistencyLevel.Session)
   ```

3. DocumentBulkExecutor nesnesi bekleme süresi ve daraltılmış istekler için yüksek yeniden deneme değerleri ile baş harfe olarak verilir. Ve sonra 0 için documentBulkExecutor için ömür boyu tıkanıklık kontrolü geçmek için ayarlanır.  

   ```java
   // Set client's retry options high for initialization
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(30);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(9);

   // Builder pattern
   Builder bulkExecutorBuilder = DocumentBulkExecutor.builder().from(
     client,
     DATABASE_NAME,
     COLLECTION_NAME,
     collection.getPartitionKey(),
     offerThroughput) // throughput you want to allocate for bulk import out of the container's total throughput

   // Instantiate DocumentBulkExecutor
   DocumentBulkExecutor bulkExecutor = bulkExecutorBuilder.build()

   // Set retries to 0 to pass complete control to bulk executor
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(0);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(0);
   ```

4. Azure Cosmos kapsayıcısına toplu alma için rasgele belgeler oluşturan tüm api'yi arayın. CmdLineConfiguration.java dosyasındaki komut satırı yapılandırmalarını yapılandırabilirsiniz.

   ```java
   BulkImportResponse bulkImportResponse = bulkExecutor.importAll(documents, false, true, null);
   ```
   Toplu alma API JSON serileştirilmiş belgelerin bir koleksiyon kabul eder ve daha fazla bilgi için, aşağıdaki sözdizimi vardır, [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)bakın:

   ```java
   public BulkImportResponse importAll(
        Collection<String> documents,
        boolean isUpsert,
        boolean disableAutomaticIdGeneration,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;   
   ```

   ImportAll yöntemi aşağıdaki parametreleri kabul eder:
 
   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |isUpsert    |   Belgelerin eklenmesini etkinleştirmek için bir bayrak. Verilen kimliği olan bir belge zaten varsa, güncelleştirilir.  |
   |devre dışı AutomaticIdGeneration     |   Otomatik kimlik oluşturmayı devre dışı kıvmak için bir bayrak. Varsayılan olarak, doğru olarak ayarlanır.   |
   |maxConcurrencyPerPartitionRange    |  Bölüm anahtar aralığı başına maksimum eşzamanlılık derecesi. Varsayılan değer 20'dir.  |

   **Toplu alma yanıtı nesne s›n›r›m›** Toplu alma API çağrısının sonucu aşağıdaki alma yöntemlerini içerir:

   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |int getNumberOfDocumentsImported()  |   Toplu alma API çağrısına sağlanan belgelerden başarıyla alınan belgelerin toplam sayısı.      |
   |çift getTotalRequestUnitsConsumed()   |  Toplu alma API çağrısı tarafından tüketilen toplam istek birimleri (RU).       |
   |Süre getTotalTimeTaken()   |    Toplu alma API'si tarafından alınan toplam süre yürütmeyi tamamlamak için çağrı da.     |
   |Özel\<Durum> listele Hatalar() |  Toplu alma API çağrısına verilen toplu iş parçasından bazı belgeler eklenmezse hata listesini alır.       |
   |Liste\<Nesne> getBadInputDocuments()  |    Toplu alma API çağrısında başarıyla alınmayan kötü biçimli belgelerin listesi. Kullanıcı döndürülen belgeleri düzeltmeli ve içe aktarmayı yeniden denemelidir. Kötü biçimlendirilmiş belgeler, kimlik değeri dize olmayan belgeleri içerir (null veya başka bir veri türü geçersiz kabul edilir).     |

5. Toplu alma uygulamasını hazırladıktan sonra 'mvn temiz paket' komutunu kullanarak komuta hattı aracını kaynaktan oluşturun. Bu komut, hedef klasörde bir kavanoz dosyası oluşturur:  

   ```java
   mvn clean package
   ```

6. Hedef bağımlılıklar oluşturulduktan sonra, aşağıdaki komutu kullanarak toplu içe aktarmacı uygulamasını çağırabilirsiniz:  

   ```java
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint *<Fill in your Azure Cosmos DB’s endpoint>*  -masterKey *<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkImportDb -collectionId bulkImportColl -operation import -shouldCreateCollection -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

   Toplu içe aktarıcı, App.config dosyasında belirtilen veritabanı adı, koleksiyon adı ve iş gücü değerlerini içeren yeni bir veritabanı ve koleksiyon oluşturur. 

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Azure Cosmos DB'de toplu güncelleme verileri

Varolan belgeleri BulkUpdateAsync API'sını kullanarak güncelleştirebilirsiniz. Bu örnekte, Ad alanını yeni bir değere ayarlar ve Varolan belgelerden Açıklama alanını kaldırırsınız. Desteklenen alan güncelleştirme işlemlerinin tam kümesi için [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)bakın. 

1. Güncelleştirme öğelerini ilgili alan güncelleştirme işlemleriyle birlikte tanımlar. Bu örnekte, Açıklama alanını tüm belgelerden kaldırmak için Ad alanını ve UnsetUpdateOperation'ı güncelleştirmek için SetUpdateOperation'ı kullanırsınız. Ayrıca, belge alanını belirli bir değerle artırarak, belirli değerleri bir dizi alanına itme veya belirli bir değeri bir dizi alanından kaldırma gibi diğer işlemleri de gerçekleştirebilirsiniz. Toplu güncelleştirme API'si tarafından sağlanan farklı yöntemler hakkında bilgi edinmek için [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)bakın.  

   ```java
   SetUpdateOperation<String> nameUpdate = new SetUpdateOperation<>("Name","UpdatedDocValue");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   ArrayList<UpdateOperationBase> updateOperations = new ArrayList<>();
   updateOperations.add(nameUpdate);
   updateOperations.add(descriptionUpdate);

   List<UpdateItem> updateItems = new ArrayList<>(cfg.getNumberOfDocumentsForEachCheckpoint());
   IntStream.range(0, cfg.getNumberOfDocumentsForEachCheckpoint()).mapToObj(j -> {                      
    return new UpdateItem(Long.toString(prefix + j), Long.toString(prefix + j), updateOperations);
    }).collect(Collectors.toCollection(() -> updateItems));
   ```

2. Azure Cosmos kapsayıcısına toplu olarak aktarılacak rasgele belgeler oluşturan updateAll API'yi arayın. CmdLineConfiguration.java dosyasında geçirilecek komut satırı yapılandırmalarını yapılandırabilirsiniz.

   ```java
   BulkUpdateResponse bulkUpdateResponse = bulkExecutor.updateAll(updateItems, null)
   ```

   Toplu güncelleştirme API'si güncelleştirilecek öğeler koleksiyonunu kabul eder. Her güncelleştirme öğesi, bir kimlik ve bölüm anahtar değeri tarafından tanımlanan bir belgede gerçekleştirilecek alan güncelleştirme işlemlerinin listesini belirtir. daha fazla bilgi [için, API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)bakın:

   ```java
   public BulkUpdateResponse updateAll(
        Collection<UpdateItem> updateItems,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;
   ```

   updateAll yöntemi aşağıdaki parametreleri kabul eder:

   |**Parametre** |**Açıklama** |
   |---------|---------|
   |maxConcurrencyPerPartitionRange   |  Bölüm anahtar aralığı başına maksimum eşzamanlılık derecesi. Varsayılan değer 20'dir.  |
 
   **Toplu alma yanıtı nesne s›n›r›m›** Toplu alma API çağrısının sonucu aşağıdaki alma yöntemlerini içerir:

   |**Parametre** |**Açıklama**  |
   |---------|---------|
   |int getNumberOfDocumentsUpdated()  |   Toplu güncelleştirme API çağrısına sağlanan belgelerden başarıyla güncelleştirilen toplam belge sayısı.      |
   |çift getTotalRequestUnitsConsumed() |  Toplu güncelleştirme API çağrısı tarafından tüketilen toplam istek birimleri (RU).       |
   |Süre getTotalTimeTaken()  |   Toplu güncelleştirme API tarafından alınan toplam süre yürütmeyi tamamlamak için çağrı.      |
   |Özel\<Durum> listele Hatalar()   |    Toplu güncelleştirme API çağrısına verilen toplu iş partisinden bazı belgeler eklenmezse hata listesini alır.      |

3. Toplu güncelleme uygulamasını hazırladıktan sonra 'mvn clean package' komutunu kullanarak komuta hattı aracını kaynaktan oluşturun. Bu komut, hedef klasörde bir kavanoz dosyası oluşturur:  

   ```
   mvn clean package
   ```

4. Hedef bağımlılıklar oluşturulduktan sonra, aşağıdaki komutu kullanarak toplu güncelleştirme uygulamasını çağırabilirsiniz:

   ```
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint **<Fill in your Azure Cosmos DB’s endpoint>* -masterKey **<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkUpdateDb -collectionId bulkUpdateColl -operation update -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

## <a name="performance-tips"></a>Performans ipuçları 

Toplu yürütme kitaplığı kullanırken daha iyi performans için aşağıdaki noktaları göz önünde bulundurun:

* En iyi performans için, uygulamanızı Cosmos DB hesap yazma bölgenizle aynı bölgede bulunan bir Azure VM'den çalıştırın.  
* Daha yüksek iş elde etmek için:  

   * Çok sayıda belgeyi işlemede herhangi bir bellek sorununu önlemek için JVM'nin yığın boyutunu yeterince büyük bir sayıya ayarlayın. Önerilen yığın boyutu: max(3GB, 3 * sizeof(tüm belgeler toplu alma API'sine tek bir toplu iş halinde geçirilir)).  
   * Çok sayıda belgeyle toplu işlemler gerçekleştirirken daha yüksek iş elde edeceğiniz bir ön işlem süresi vardır. Yani, 10.000.000 belge almak istiyorsanız, 1.000.000 boyutu her boyutu 100.000 belgelerin her biri 100 kez toplu alma çalışan daha 100.000.000 boyutu belgelerin 100 toplu belgelerin 100 kez dökme ithalat çalışan 10 kez toplu ithalat 10 kez toplu ithalat çalışan 100.000 000 belgelerin.  

* Belirli bir Azure Cosmos kapsayıcısına karşılık gelen tek bir sanal makine içinde tüm uygulama için tek bir DocumentBulkExecutor nesnesini anında kullanmak önerilir.  

* Tek bir toplu işlem API yürütme istemci makinenin CPU ve ağ IO büyük bir yığın tüketir beri. Bu, dahili olarak birden çok görev oluşturarak, her toplu işlem API çağrıları yürüten uygulama süreci içinde birden çok eşzamanlı görev yumurtlama önlemek olur. Tek bir sanal makinede çalışan tek bir toplu işlem API çağrısı, konteynerinizin tüm çıktısını tüketemiyorsa (kabınızın iş bölmesi 1 milyon RU/s> ise), toplu işlem API çağrılarını aynı anda yürütmek için ayrı sanal makineler oluşturmak tercih edilir.

    
## <a name="next-steps"></a>Sonraki adımlar
* Maven paket ayrıntıları ve toplu uygulayıcı Java kitaplığı sürüm notları hakkında bilgi edinmek[için, toplu uygulayıcı SDK ayrıntılarına](sql-api-sdk-bulk-executor-java.md)bakın.


