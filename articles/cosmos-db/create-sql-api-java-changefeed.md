---
title: Değişiklik akışını kullanarak uçtan uca Azure Cosmos DB Java SDK 'Sı v4 uygulaması örneği oluşturma
description: Bu nasıl yapılır Kılavuzu, değişiklik akışını kullanarak kapsayıcının gerçekleştirilmiş bir görünümünü koruyarak Azure Cosmos DB kapsayıcısına belge ekleyen basit bir Java SQL API uygulaması boyunca size yol gösterir.
author: anfeldma
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: anfeldma
ms.openlocfilehash: 9e28eb4f766677ebbd5cfcc5f61fe54e53a45523
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82996503"
---
# <a name="how-to-create-a-java-application-that-uses-azure-cosmos-db-sql-api-and-change-feed-processor"></a>SQL API Azure Cosmos DB kullanan bir Java uygulaması oluşturma ve akış Işlemcisini değiştirme

> [!IMPORTANT]  
> Java SDK 'Sı v4 Azure Cosmos DB hakkında daha fazla bilgi için lütfen Azure Cosmos DB Java SDK v4 sürüm notları, [Maven deposu](https://mvnrepository.com/artifact/com.azure/azure-cosmos), Azure Cosmos DB Java SDK v4 [performans Ipuçları](performance-tips-java-sdk-v4-sql.md)ve Azure Cosmos DB Java SDK 'sı v4 [sorun giderme kılavuzunu](troubleshoot-java-sdk-v4-sql.md)görüntüleyin.
>

Bu nasıl yapılır Kılavuzu, bir Azure Cosmos DB kapsayıcısına belge eklemek için Azure Cosmos DB SQL API 'sini kullanan basit bir Java uygulamasında size kılavuzluk eder ve değişiklik akışı ve değişiklik akışı Işlemcisini kullanarak kapsayıcının gerçekleştirilmiş bir görünümünü sürdürmektedir. Java uygulaması, Azure Cosmos DB Java SDK v4 kullanarak SQL API Azure Cosmos DB ile iletişim kurar.

## <a name="prerequisites"></a>Ön koşullar

* Azure Cosmos DB hesabınız için URI ve anahtar

* Maven

* Java 8

## <a name="background"></a>Arka plan

Azure Cosmos DB değişiklik akışı, belge eklemeye yanıt olarak eylemleri tetiklemeye yönelik olay odaklı bir arabirim sağlar. Bunun birçok kullanımı vardır. Örneğin, hem okuma hem de yazma bilgisi olan uygulamalarda, değişiklik akışı kullanımı, belge alındığı sırada bir kapsayıcının gerçek zamanlı **gerçekleştirilmiş bir görünümünü** oluşturmaktır. Gerçekleştirilmiş görünüm kapsayıcısı aynı verileri tutar, ancak etkili okuma için bölümlenmiş, uygulamayı hem okuma hem de yazma verimli hale getirir.

Değişiklik akışı olaylarının yönetilmesi işi, SDK 'da yerleşik olarak bulunan değişiklik akışı Işlemci kitaplığı tarafından büyük ölçüde dikkate alınmıştır. Bu kitaplık, değişiklik akışı olaylarını birden çok çalışan arasında dağıtmak için yeterince güçlü bir değer vardır. Tek yapmanız gerekir değişiklik akışı kitaplığı bir geri çağırma işlemi sağlar.

Bu basit örnek, bir gerçekleştirilmiş görünümden belge oluşturma ve silme adlı tek bir çalışan akış Işlemcisi kitaplığı değişikliğini gösterir.

## <a name="setup"></a>Kurulum

Daha önce yapmadıysanız, uygulama örneği deposunu kopyalayın:

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example.git
```

Depo dizininde bir Terminal açın. Çalıştırarak uygulamayı oluşturun

```bash
mvn clean package
```

## <a name="walkthrough"></a>Kılavuz

1. İlk denetim olarak bir Azure Cosmos DB hesabınızın olması gerekir. Tarayıcınızda **Azure Portal** açın, Azure Cosmos DB hesabınıza gidin ve sol bölmedeki **Veri Gezgini**' a gidin.

    ![Azure Cosmos DB hesabı](media/create-sql-api-java-changefeed/cosmos_account_empty.JPG)

1. Uygulamayı terminalde aşağıdaki komutu kullanarak çalıştırın:

    ```bash
    mvn exec:java -Dexec.mainClass="com.azure.cosmos.workedappexample.SampleGroceryStore" -DACCOUNT_HOST="your-account-uri" -DACCOUNT_KEY="your-account-key" -Dexec.cleanupDaemonThreads=false
    ```

1. Gördüğünüz zaman ENTER tuşuna basın

    ```bash
    Press enter to create the grocery store inventory system...
    ```

    ardından Azure portal Veri Gezgini tarayıcınıza geri dönün. Üç boş kapsayıcıyla bir **Market veritabanı** eklendiğini göreceksiniz: 

    * **Inventorycontainer** -bir UUID olan öğe ```id``` üzerinde bölümlenen, bizim örnek Market depomız için Envanter kaydı.
    * **Inventorycontainer-pktype** -stok kaydının gerçekleştirilmiş bir görünümü, öğe üzerindeki sorgular için iyileştirilmiştir```type```
    * **Inventorycontainer-kiralamalar** -her zaman değişiklik akışı için bir kiralama kapsayıcısı gereklidir; kiralamalar değişiklik akışını okurken uygulamanın ilerlemesini izler.


    ![Boş kapsayıcılar](media/create-sql-api-java-changefeed/cosmos_account_resources_lease_empty.JPG)


1. Terminalde, şimdi bir istem görmeniz gerekir

    ```bash
    Press enter to start creating the materialized view...
    ```

    ENTER tuşuna basın. Aşağıdaki kod bloğu, başka bir iş parçacığında değişiklik akışı işlemcisini yürütecektir ve başlatacak: 

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    changeFeedProcessorInstance = getChangeFeedProcessor("SampleHost_1", feedContainer, leaseContainer);
    changeFeedProcessorInstance.start()
        .subscribeOn(Schedulers.elastic())
        .doOnSuccess(aVoid -> {
            isProcessorRunning.set(true);
        })
        .subscribe();

    while (!isProcessorRunning.get()); //Wait for Change Feed processor start
    ```

    ```"SampleHost_1"```, değişiklik akışı işlemci çalışanının adıdır. ```changeFeedProcessorInstance.start()```Değişiklik akışı işlemcisini asıl olarak başlatır.

    Tarayıcınızda Azure portal Veri Gezgini geri dönün. **Inventorycontainer-kiralamalar** kapsayıcısının altında, içeriğini görmek için **öğeler** ' e tıklayın. Değişiklik akışı Işlemcisinin kira kapsayıcısını doldurduğuna, yani işlemcinin bir ```SampleHost_1``` çalışan tarafından **ınventorycontainer**'in bazı bölümlerinde bir kira atamasıyla karşılaşırsınız.

    ![Kiralamalar](media/create-sql-api-java-changefeed/cosmos_leases.JPG)

1. Terminalde ENTER tuşuna basın. Bu işlem, 10 belgeyi **ınventorycontainer**içine eklenecek şekilde tetikler. Her belge ekleme, değişiklik akışında JSON olarak görünür; Aşağıdaki geri çağırma kodu, JSON belgelerini gerçekleştirilmiş bir görünüme yansıtarak bu olayları işler:

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    public static ChangeFeedProcessor getChangeFeedProcessor(String hostName, CosmosAsyncContainer feedContainer, CosmosAsyncContainer leaseContainer) {
        ChangeFeedProcessorOptions cfOptions = new ChangeFeedProcessorOptions();
        cfOptions.setFeedPollDelay(Duration.ofMillis(100));
        cfOptions.setStartFromBeginning(true);
        return ChangeFeedProcessor.changeFeedProcessorBuilder()
            .setOptions(cfOptions)
            .setHostName(hostName)
            .setFeedContainer(feedContainer)
            .setLeaseContainer(leaseContainer)
            .setHandleChanges((List<JsonNode> docs) -> {
                for (JsonNode document : docs) {
                        //Duplicate each document update from the feed container into the materialized view container
                        updateInventoryTypeMaterializedView(document);
                }

            })
            .build();
    }

    private static void updateInventoryTypeMaterializedView(JsonNode document) {
        typeContainer.upsertItem(document).subscribe();
    }
    ```

1. Kodun 5-10sn çalışmasına izin verin. Ardından Azure portal Veri Gezgini geri dönüp **ınventorycontainer > öğelerine**gidin. Öğelerin envanter kapsayıcısına eklenmekte olduğunu görmeniz gerekir; bölüm anahtarına (```id```) göz önünde.

    ![Akış kapsayıcısı](media/create-sql-api-java-changefeed/cosmos_items.JPG)

1. Şimdi Veri Gezgini ' de, **ınventorycontainer-pktype > öğelerine**gidin. Bu, bu kapsayıcıdaki öğeler, değişiklik akışı tarafından program aracılığıyla eklendiklerinden, bu kapsayıcıdaki öğelerin, **ınventorycontainer** yansıtmasının gerçekleştirilmiş görünümüdür. Bölüm anahtarına (```type```) göz önünde. Bu gerçekleştirilmiş görünüm ```type```, üzerinde bölümlenmiş olduğundan **ınventorycontainer** üzerinde verimsiz olacak sorgular üzerinde filtreleme için en iyi duruma getirilmiştir ```id```.

    ![Gerçekleştirilmiş görünüm](media/create-sql-api-java-changefeed/cosmos_materializedview2.JPG)

1. Yalnızca tek ```upsertItem()``` bir çağrı kullanarak hem **ınventorycontainer** hem de **ınventorycontainer-pktype** öğesinden bir belgeyi silelim. İlk olarak, Azure portal Veri Gezgini göz atın. Belgeyi sileceğiz ```/type == "plums"```; Aşağıda kırmızı renkle şifreli

    ![Gerçekleştirilmiş görünüm](media/create-sql-api-java-changefeed/cosmos_materializedview-emph-todelete.JPG)

    Örnek kodda işlevi ```deleteDocument()``` çağırmak için yeniden ENTER tuşuna basın. Aşağıda gösterilen bu işlev, belge yaşam süresi (TTL) ile 5 sn arasında olan ```/ttl == 5```belgenin yeni bir sürümünü kaplamıştır. 
    
    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    public static void deleteDocument() {

        String jsonString =    "{\"id\" : \"" + idToDelete + "\""
                + ","
                + "\"brand\" : \"Jerry's\""
                + ","
                + "\"type\" : \"plums\""
                + ","
                + "\"quantity\" : \"50\""
                + ","
                + "\"ttl\" : 5"
                + "}";

        ObjectMapper mapper = new ObjectMapper();
        JsonNode document = null;

        try {
            document = mapper.readTree(jsonString);
        } catch (Exception e) {
            e.printStackTrace();
        }

        feedContainer.upsertItem(document,new CosmosItemRequestOptions()).block();
    }    
    ```

    Değişiklik akışı ```feedPollDelay``` 100 ms olarak ayarlanır; Bu nedenle, değişiklik akışı bu güncelleştirmeyi neredeyse anında ve yukarıda gösterilen ```updateInventoryTypeMaterializedView()``` çağrılara yanıt verir. Bu son işlev çağrısı, 5 San TTL ile yeni belgeyi **ınventorycontainer-pktype**olarak kaplacaktır.

    Bu, yaklaşık 5 saniye sonra, belgenin süresi dolduğunda ve her iki kapsayıcıdan da silinmeyecektir.

    Değişiklik akışı, öğe silme işlemi sırasında değil yalnızca öğe ekleme veya güncelleştirme üzerindeki olayları verdiği için bu yordam gereklidir.

1. Programı kapatmak ve kaynaklarını temizlemek için bir kez daha ENTER tuşuna basın.
