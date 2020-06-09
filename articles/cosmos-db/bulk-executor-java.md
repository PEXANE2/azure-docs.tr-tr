---
title: Toplu içeri aktarma ve güncelleştirme işlemleri gerçekleştirmek için Azure Cosmos DB 'da toplu yürütücü Java kitaplığı 'nı kullanın
description: Toplu yürütücü Java kitaplığı kullanarak Azure Cosmos DB belgelerini toplu içe aktarma ve güncelleştirme
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: fec11b930be0487a7fbc0968c3c8a8003a2fb911
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84559275"
---
# <a name="use-bulk-executor-java-library-to-perform-bulk-operations-on-azure-cosmos-db-data"></a>Azure Cosmos DB verilerinde toplu işlemler yapmak için toplu yürütücü Java kitaplığını kullanma

Bu öğretici, Azure Cosmos DB belgelerini içeri aktarmak ve güncelleştirmek için Azure Cosmos DB toplu yürütücü Java Kitaplığı kullanma hakkında yönergeler sağlar. Toplu yürütücü Kitaplığı hakkında bilgi edinmek ve büyük/veya depolama özelliğinden yararlanarak nasıl yardım alabileceğinizi öğrenmek için bkz. [toplu yürütücü kitaplığı genel bakış](bulk-executor-overview.md) makalesi. Bu öğreticide, rastgele belgeler üreten ve bunlar bir Azure Cosmos kapsayıcısına toplu olarak içeri aktarılan bir Java uygulaması oluşturacaksınız. İçeri aktardıktan sonra bir belgenin bazı özelliklerini toplu olarak güncelleştirebilirsiniz. 

Şu anda, toplu yürütücü kitaplığı yalnızca Azure Cosmos DB SQL API ve Gremlin API hesapları tarafından desteklenir. Bu makalede, SQL API hesaplarıyla toplu yürütücü Java kitaplığı 'nın nasıl kullanılacağı açıklanır. Gremlin API ile toplu yürütücü .NET kitaplığı 'nı kullanma hakkında bilgi edinmek için bkz. [Azure Cosmos DB Gremlin API 'de toplu işlemler gerçekleştirme](bulk-executor-graph-dotnet.md). Tanımlanan toplu yürütücü kitaplığı yalnızca [Java Sync SDK v2 Azure Cosmos DB](sql-api-sdk-java.md) kullanılabilir ve Java toplu destek için önerilen geçerli çözümdür. Bu, şu anda 3. x, 4. x veya daha yüksek SDK sürümleri için kullanılamaz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.  

* Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyebilir](https://azure.microsoft.com/try/cosmosdb/) , ücretsiz ve taahhütlere sahip olabilirsiniz. Ya da [Azure Cosmos DB öykünücüsünü](https://docs.microsoft.com/azure/cosmos-db/local-emulator) `https://localhost:8081` uç noktayla kullanabilirsiniz. Birincil Anahtar, [Kimlik doğrulama istekleri](local-emulator.md#authenticating-requests) bölümünde sağlanır.  

* [Java Development Kit (JDK) 1.7+](/java/azure/jdk/?view=azure-java-stable)  
  - Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.  

  - JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.

* [Maven](https://maven.apache.org/) Ikili Arşivi [indirme](https://maven.apache.org/download.cgi) ve [yükleme](https://maven.apache.org/install.html)  
  
  - Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.

* Java hızlı başlangıç makalesinin [veritabanı hesabı oluştur](create-sql-api-java.md#create-a-database-account) bölümünde açıklanan adımları kullanarak Azure Cosmos DB BIR SQL API hesabı oluşturun.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi, GitHub 'dan örnek bir Java uygulaması indirerek kodla çalışmaya geçiş yapalım. Bu uygulama Azure Cosmos DB veri üzerinde toplu işlemler gerçekleştirir. Uygulamayı kopyalamak için, bir komut istemi açın, uygulamayı kopyalamak istediğiniz dizine gidin ve şu komutu çalıştırın:

```
 git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started.git 
```

Kopyalanmış depo, "\azure-cosmosdb-bulkexecutor-Java-getting-started\samples\bulkexecutor-sample\src\main\java\com\microsoft\azure\cosmosdb\bulkexecutor" klasörüne göre "BulkImport" ve "bulkupdate" iki örnek içerir. "BulkImport" uygulaması rastgele belgeler oluşturur ve bunları Azure Cosmos DB içe aktarır. "Bulkupdate" uygulaması Azure Cosmos DB içindeki bazı belgeleri güncelleştirir. Sonraki bölümlerde, bu örnek uygulamaların her birinde kodu gözden geçiyoruz. 

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Azure Cosmos DB verileri toplu içe aktarma

1. Azure Cosmos DB bağlantı dizeleri, bağımsız değişken olarak okunurdur ve CmdLineConfiguration. Java dosyasında tanımlanan değişkenlere atanır.  

2. Sonraki DocumentClient nesnesi aşağıdaki deyimler kullanılarak başlatılır:  

   ```java
   ConnectionPolicy connectionPolicy = new ConnectionPolicy();
   connectionPolicy.setMaxPoolSize(1000);
   DocumentClient client = new DocumentClient(
      HOST,
      MASTER_KEY, 
      connectionPolicy,
      ConsistencyLevel.Session)
   ```

3. Documentbulkyürütücü nesnesi, bekleme süresi ve kısıtlanmış istekler için yüksek yeniden deneme değerleriyle başlatılır. Ve sonra, yaşam süresi boyunca bir sıkışıklık denetimini Documentbulkyürütücü 'e geçirmek için 0 olarak ayarlanır.  

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

4. Bir Azure Cosmos kapsayıcısına toplu içeri aktarmaya yönelik rastgele belgeler üreten ImportAll API 'sini çağırın. Komut satırı yapılandırmalarını CmdLineConfiguration. Java dosyası içinde yapılandırabilirsiniz.

   ```java
   BulkImportResponse bulkImportResponse = bulkExecutor.importAll(documents, false, true, null);
   ```
   Toplu içeri aktarma API 'si JSON ile seri hale getirilmiş belgelerden oluşan bir koleksiyonu kabul eder ve aşağıdaki sözdizimine sahiptir, daha fazla ayrıntı için [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)bakın:

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
   |ıupsert    |   Belgelerin kaplamasıyla izin vermek için bir bayrak. Verilen KIMLIĞE sahip bir belge zaten varsa, güncelleştirilir.  |
   |Disableautomaticıdgeneration     |   Otomatik KIMLIK oluşturmayı devre dışı bırakmak için bayrak. Varsayılan olarak, true olarak ayarlanır.   |
   |maxConcurrencyPerPartitionRange    |  Bölüm anahtar aralığı başına en fazla eşzamanlılık derecesi. Varsayılan değer 20 ' dir.  |

   **Toplu içeri aktarma yanıtı nesne tanımı** Toplu içeri aktarma API çağrısının sonucu aşağıdaki Get yöntemlerini içerir:

   |**Parametre**  |**Açıklama**  |
   |---------|---------|
   |int getNumberOfDocumentsImported ()  |   Toplu içeri aktarma API çağrısına sağlanan belgelerden başarıyla içeri aktarılmış belgelerin toplam sayısı.      |
   |Double Gettotalrequestunitstüketilen ()   |  Toplu içeri aktarma API çağrısı tarafından tüketilen toplam istek birimi (RU).       |
   |Süre getTotalTimeTaken ()   |    Yürütmeyi tamamlamaya yönelik toplu içeri aktarma API çağrısı tarafından alınan toplam süre.     |
   |\<Exception>GetErrors () listesini |  Toplu içeri aktarma API 'SI çağrısına sağlanan toplu iş dışında bazı belgeler eklenmeden başarısız olursa hata listesini alır.       |
   |\<Object>Getbadınputdocuments () listesini listeleyin  |    Toplu içeri aktarma API çağrısında başarıyla içeri aktarılmayan hatalı biçimli belgelerin listesi. Kullanıcı döndürülen belgeleri düzelttikten sonra içeri aktarmayı yeniden dener. Hatalı biçimli belgeler, ID değeri dize olmayan belgeleri içerir (null veya başka bir veri türü geçersiz olarak kabul edilir).     |

5. Toplu alma uygulamasını hazırlayın, ' MVN Clean Package ' komutunu kullanarak kaynaktan komut satırı aracını oluşturun. Bu komut hedef klasörde bir jar dosyası oluşturur:  

   ```java
   mvn clean package
   ```

6. Hedef bağımlılıklar oluşturulduktan sonra, aşağıdaki komutu kullanarak toplu alma uygulamasını çağırabilirsiniz:  

   ```java
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint *<Fill in your Azure Cosmos DB's endpoint>*  -masterKey *<Fill in your Azure Cosmos DB's master key>* -databaseId bulkImportDb -collectionId bulkImportColl -operation import -shouldCreateCollection -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

   Toplu içe aktarıcı, App. config dosyasında belirtilen veritabanı adı, koleksiyon adı ve üretilen iş değerleriyle yeni bir veritabanı ve bir koleksiyon oluşturur. 

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Azure Cosmos DB verileri toplu güncelleştirme

Mevcut belgeleri BulkUpdateAsync API kullanarak güncelleştirebilirsiniz. Bu örnekte, ad alanını yeni bir değere ayarlanacak ve Açıklama alanını mevcut belgelerden kaldıracaksınız. Desteklenen alan güncelleştirme işlemlerinin tam kümesi için bkz. [API belgeleri](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor). 

1. İlgili alan güncelleştirme işlemleriyle birlikte güncelleştirme öğelerini tanımlar. Bu örnekte, açıklama alanını tüm belgelerden kaldırmak için Name alanını ve UnsetUpdateOperation öğesini güncelleştirmek üzere SetUpdateOperation kullanacaksınız. Ayrıca, belirli bir değere göre bir belge alanını artırma, belirli değerleri bir dizi alanına gönderme veya dizi alanından belirli bir değeri kaldırma gibi başka işlemler de gerçekleştirebilirsiniz. Toplu güncelleştirme API 'SI tarafından sunulan farklı yöntemler hakkında bilgi edinmek için [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)bakın.  

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

2. Daha sonra bir Azure Cosmos kapsayıcısına toplu olarak içeri aktarılacak rastgele belgeler üreten updateAll API 'yi çağırın. Komut satırı yapılandırmalarını CmdLineConfiguration. Java dosyasına geçirilecek şekilde yapılandırabilirsiniz.

   ```java
   BulkUpdateResponse bulkUpdateResponse = bulkExecutor.updateAll(updateItems, null)
   ```

   Toplu güncelleştirme API 'SI güncelleştirilecek bir öğe koleksiyonu kabul eder. Her güncelleştirme öğesi, bir KIMLIK ve bölüm anahtarı değeri tarafından tanımlanan bir belgede gerçekleştirilecek alan güncelleştirme işlemlerinin listesini belirtir. daha fazla ayrıntı için [API belgelerine](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)bakın:

   ```java
   public BulkUpdateResponse updateAll(
        Collection<UpdateItem> updateItems,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;
   ```

   UpdateAll yöntemi aşağıdaki parametreleri kabul eder:

   |**Parametre** |**Açıklama** |
   |---------|---------|
   |maxConcurrencyPerPartitionRange   |  Bölüm anahtar aralığı başına en fazla eşzamanlılık derecesi. Varsayılan değer 20 ' dir.  |
 
   **Toplu içeri aktarma yanıtı nesne tanımı** Toplu içeri aktarma API çağrısının sonucu aşağıdaki Get yöntemlerini içerir:

   |**Parametre** |**Açıklama**  |
   |---------|---------|
   |int getNumberOfDocumentsUpdated ()  |   Toplu güncelleştirme API 'SI çağrısına sağlanan belgelerden başarıyla güncelleştirilmiş toplam belge sayısı.      |
   |Double Gettotalrequestunitstüketilen () |  Toplu güncelleştirme API çağrısı tarafından tüketilen toplam istek birimi (RU).       |
   |Süre getTotalTimeTaken ()  |   Yürütmeyi tamamlamaya yönelik toplu güncelleştirme API çağrısı tarafından alınan toplam süre.      |
   |\<Exception>GetErrors () listesini   |       Toplu güncelleştirme API 'SI çağrısına sağlanan toplu iş dışında bazı belgeler eklenmeden başarısız olursa hata listesini alır.      |

3. Toplu güncelleştirme uygulamasını hazırlayın, ' MVN Clean Package ' komutunu kullanarak kaynaktan komut satırı aracını oluşturun. Bu komut hedef klasörde bir jar dosyası oluşturur:  

   ```
   mvn clean package
   ```

4. Hedef bağımlılıklar oluşturulduktan sonra, aşağıdaki komutu kullanarak toplu güncelleştirme uygulamasını çağırabilirsiniz:

   ```
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint **<Fill in your Azure Cosmos DB's endpoint>* -masterKey **<Fill in your Azure Cosmos DB's master key>* -databaseId bulkUpdateDb -collectionId bulkUpdateColl -operation update -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

## <a name="performance-tips"></a>Performans ipuçları 

Toplu yürütücü kitaplığı kullanırken daha iyi performans için aşağıdaki noktaları göz önünde bulundurun:

* En iyi performansı elde etmek için, uygulamanızı Cosmos DB hesabınızın yazma bölgesiyle aynı bölgedeki bir Azure VM 'sinden çalıştırın.  
* Daha yüksek performans elde etmek için:  

   * Çok sayıda belgeyi işlemede herhangi bir bellek sorununu önlemek için JVM 'nin yığın boyutunu yeterince büyük bir sayıya ayarlayın. Önerilen yığın boyutu: Max (3GB, 3 * sizeof (tüm belgeler tek bir toplu iş içinde toplu içeri aktarma API 'sine geçirilir)).  
   * Çok sayıda belgeyle toplu işlemler gerçekleştirirken daha yüksek aktarım hızı alacağınız için bir ön işleme süresi vardır. Bu nedenle, 10.000.000 belgelerini içeri aktarmak istiyorsanız, her bir 1.000.000 belgenin her birinde her bir boyut olan 10 ' un toplu içeri aktarma işlemi, her boyuttaki 100.000 belge 100 üzerinde toplu içe aktarma 100 ' i çalıştırmak yerine 10 ' u her bir belge için 10 kez çalıştırmak  

* Belirli bir Azure Cosmos kapsayıcısına karşılık gelen tek bir sanal makine içinde uygulamanın tamamı için tek bir Documentbulkyürütücü nesnesi örneği oluşturmanız önerilir.  

* Tek bir toplu işlem API yürütmesi, istemci makinenin CPU ve ağ GÇ 'sinin büyük bir öbeğini kullanır. Bu durum, birden çok görevi dahili olarak üretirken, her bir toplu işlem API çağrısı yürüten uygulama işleminizde birden çok eş zamanlı görevi oluşturmaktan kaçının. Tek bir sanal makinede çalışan tek bir toplu işlem API çağrısı, kapsayıcının üretilen iş verimini (kapsayıcının üretilen iş > 1.000.000 RU/sn) tüketmez ve aynı anda toplu işlem API çağrılarını yürütmek üzere ayrı sanal makineler oluşturmak tercih edilir.

    
## <a name="next-steps"></a>Sonraki adımlar
* Toplu yürütücü Java kitaplığı 'nın Maven paket ayrıntıları ve sürüm notları hakkında bilgi edinmek için bkz.[toplu yürütücü SDK ayrıntıları](sql-api-sdk-bulk-executor-java.md).


