---
title: Öğretici - Change Feed ile uçdan uca Async Java SQL API uygulama örneği
description: Bu öğretici, Belgeleri Bir Azure Cosmos DB kapsayıcısına eklerken, Değişiklik Akışı'nı kullanarak kapsayıcının somutlaştırılmış görünümünü koruyan basit bir Java SQL API uygulamasında size yol sunar.
author: anfeldma
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 04/01/2020
ms.author: anfeldma
ms.openlocfilehash: 5eab523dde2a13a85b0c8ff5bcbb3ecb5912e78e
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586704"
---
# <a name="tutorial---an-end-to-end-async-java-sql-api-application-sample-with-change-feed"></a>Öğretici - Change Feed ile uçdan uca Async Java SQL API uygulama örneği

Bu öğretici kılavuz, Belgeleri Bir Azure Cosmos DB kapsayıcısına eklerken, Değişiklik Akışı'nı kullanarak kapsayıcının somutlaştırılmış görünümünü koruyan basit bir Java SQL API uygulamasında size yol göstermektedir.

## <a name="prerequisites"></a>Ön koşullar

* Kişisel bilgisayar

* Azure Cosmos DB hesabınız için URI ve anahtar

* Maven

* Java 8

## <a name="background"></a>Arka plan

Azure Cosmos DB Değişiklik Yayını, belge eklemeye yanıt olarak eylemleri tetiklemek için olay odaklı bir arabirim sağlar. Bunun birçok faydası vardır. Örneğin, hem okunan hem de ağır yazan uygulamalarda, Change Feed'in baş kullanımı, belgeleri sindirirken bir kapsayıcının gerçek zamanlı **olarak gerçek zamanlı olarak somutlaştırılmış görünümünü** oluşturmaktır. Maddeleştirilmiş görünüm kapsayıcısı aynı verileri tutar, ancak verimli okumalar için bölümlenir, bu da uygulamanın hem okuma hem de yazma verimli hale getirilmesini sağlar.

Özet Akışı etkinliklerini yönetme çalışmaları büyük ölçüde SDK'da yerleşik Olan Değişim Akışı İşlemci kitaplığı tarafından halledilir. Bu kitaplık, istenirse, Özet Akışı olaylarını birden çok çalışan arasında dağıtacak kadar güçlüdür. Tek yapmanız gereken, Özet Akışı kitaplığını değiştir'e bir geri arama sağlamaktır.

Bu basit örnek, tek bir çalışanın belgeleri somutlaştırılmış bir görünümden oluşturması ve silmesiyle Özet Akışı İşlemci kitaplığını değiştir'i gösterir.

## <a name="setup"></a>Kurulum

Bunu daha önce yapmadıysanız, uygulama örneği repo'yu kopyala:

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example.git
```

> Java SDK 4.0 veya Java SDK 3.7.0 ile bu Quickstart üzerinden çalışmak için bir seçim var. **Java SDK 3.7.0 kullanmak istiyorsanız, terminal türünde. ```git checkout SDK3.7.0``` ** Aksi takdirde, ```master``` Java SDK 4.0 varsayılan şube, kalmak.

Repo dizininde bir terminal açın. Çalıştırarak uygulamayı oluşturun

```bash
mvn clean package
```

## <a name="walkthrough"></a>Kılavuz

1. İlk denetim olarak, bir Azure Cosmos DB hesabınız olmalıdır. Tarayıcınızda **Azure Portalı'nı** açın, Azure Cosmos DB hesabınıza gidin ve sol bölmede **Veri Gezgini'ne**gidin.

    ![Azure Cosmos DB hesabı](media/create-sql-api-java-changefeed/cosmos_account_empty.JPG)

1. Aşağıdaki komutu kullanarak uygulamayı terminalde çalıştırın:

    ```bash
    mvn exec:java -Dexec.mainClass="com.azure.cosmos.workedappexample.SampleGroceryStore" -DACCOUNT_HOST="your-account-uri" -DACCOUNT_KEY="your-account-key" -Dexec.cleanupDaemonThreads=false
    ```

1. Gördüğünüzde enter tuşuna basın

    ```bash
    Press enter to create the grocery store inventory system...
    ```

    ardından tarayıcınızdaki Azure Portal Veri Gezgini'ne geri dönün. Bir veritabanı **BakkalStoreDatabase** üç boş kapsayıcılar ile eklenmiştir göreceksiniz: 

    * **InventoryContainer** - Örnek bakkal, bir UUID olan ```id``` madde üzerinde bölümlenmiş için stok kaydı.
    * **InventoryContainer-pktype** - Madde üzerindeki sorgular için optimize edilmiş stok kaydının maddesel görünümü```type```
    * **InventoryContainer-leases** - Değişim Akışı için her zaman bir kiralama konteyneri gereklidir; kiralama, uygulamanın Değişiklik Akışı'nı okumadaki ilerlemesini izler.


    ![Boş kaplar](media/create-sql-api-java-changefeed/cosmos_account_resources_lease_empty.JPG)


1. Terminalde, şimdi bir istemi görmeniz gerekir

    ```bash
    Press enter to start creating the materialized view...
    ```

    Enter tuşuna basın. Şimdi aşağıdaki kod bloğu, Başka bir iş parçacığı üzerinde Feed işlemcideğiştir'i çalıştıracak ve başharfünü çalıştıracaktır: 


    **Java SDK 4.0**
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

    **Java SDK 3.7.0**
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

    ```"SampleHost_1"```Feed işlemci sini değiştir işlemci sicisinin adıdır. ```changeFeedProcessorInstance.start()```Aslında Feed işlemcideğiştir'i başlatan şeydir.

    Tarayıcınızdaki Azure Portal Veri Gezgini'ne geri dönün. **InventoryContainer-leases** kapsayıcısının altında, içeriğini görmek için **maddeleri** tıklatın. Feed İşlemcisi'ni değiştirin, kira kapsayıcısını doldurmuş, yani işlemcinin işçiye ```SampleHost_1``` **InventoryContainer'ın**bazı bölümleri üzerinde kira atadığını göreceksiniz.

    ![Kiralamalar](media/create-sql-api-java-changefeed/cosmos_leases.JPG)

1. Terminale tekrar girin tuşuna basın. Bu, **InventoryContainer'a**eklenecek 10 belgeyi tetikler. Her belge ekleme JSON olarak Değişiklik Akışı görünür; Aşağıdaki geri arama kodu, JSON belgelerini somutlaştırılmış bir görünüme yansıtarak bu olayları işler:

    **Java SDK 4.0**
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

    **Java SDK 3.7.0**
    ```java
    public static ChangeFeedProcessor getChangeFeedProcessor(String hostName, CosmosContainer feedContainer, CosmosContainer leaseContainer) {
        ChangeFeedProcessorOptions cfOptions = new ChangeFeedProcessorOptions();
        cfOptions.feedPollDelay(Duration.ofMillis(100));
        cfOptions.startFromBeginning(true);
        return ChangeFeedProcessor.Builder()
            .options(cfOptions)
            .hostName(hostName)
            .feedContainer(feedContainer)
            .leaseContainer(leaseContainer)
            .handleChanges((List<CosmosItemProperties> docs) -> {
                for (CosmosItemProperties document : docs) {
                        //Duplicate each document update from the feed container into the materialized view container
                        updateInventoryTypeMaterializedView(document);
                }

            })
            .build();
    }

    private static void updateInventoryTypeMaterializedView(CosmosItemProperties document) {
        typeContainer.upsertItem(document).subscribe();
    }    
    ```

1. Kodun 5-10sn çalışmasına izin verin. Ardından Azure Portal Veri Gezgini'ne dönün ve **> öğeleriyle InventoryContainer'a**gidin. Maddelerin stok kapsayıcısına eklendiğini görmeniz gerekir; bölüm anahtarını```id```not edin ( ).

    ![Besleme kabı](media/create-sql-api-java-changefeed/cosmos_items.JPG)

1. Şimdi, Veri Gezgini'nde **InventoryContainer-pktype > öğelerine**gidin. Bu maddeleşmiş görünümdür - Bu kapsayıcı **aynası InventoryContainer'taki** maddeler, Değişim Akışı tarafından programlı olarak takıldıkları için. Bölüm tuşuna```type```dikkat edin ( ). Yani bu somutlaştırılmış görünüm üzerinde bölümlenmiş ```type```çünkü **InventoryContainer** üzerinde verimsiz olurdu, üzerinde filtreleme sorguları için optimize ```id```edilmiştir.

    ![Gerçekleştirilmiş görünüm](media/create-sql-api-java-changefeed/cosmos_materializedview2.JPG)

1. Bir belgeyi hem InventoryContainer hem de ```upsertItem()``` **InventoryContainer-pktype'tan** tek bir çağrı kullanarak sileceğiz. **InventoryContainer** İlk olarak, Azure Portal Veri Gezgini'ne bir göz atın. Belgeyi sileceğiz; ```/type == "plums"``` aşağıda kırmızı ile çevrilidir

    ![Gerçekleştirilmiş görünüm](media/create-sql-api-java-changefeed/cosmos_materializedview-emph-todelete.JPG)

    Örnek koddaki işlevi ```deleteDocument()``` aramak için yeniden girin'e basın. Aşağıda gösterilen bu işlev, belgenin ```/ttl == 5```Canlı Yayın (TTL) ile 5 sn'ye ayarladığı yeni bir sürümünü ekler. 
    
    **Java SDK 4.0**
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

    **Java SDK 3.7.0**
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

    Değişiklik Akışı ```feedPollDelay``` 100ms olarak ayarlanır; bu nedenle, Change Feed bu güncelleştirmeye ```updateInventoryTypeMaterializedView()``` neredeyse anında yanıt verir ve yukarıda gösterilen çağrılara yanıt verir. Bu son işlev çağrısı, yeni belgeyi 5 sn'lik TTL ile **InventoryContainer-pktype'a**ekler.

    Etkisi, yaklaşık 5 saniye sonra belgenin süresidin dolması ve her iki kapsayıcıdan silineceğidir.

    Özet Akışını Değiştir, madde silme de değil, yalnızca madde ekleme veya güncelleştirme yle ilgili olayları sorunlattığı için bu yordam gereklidir.

1. Programı kapatmak ve kaynaklarını temizlemek için bir kez daha basın.
