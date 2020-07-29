---
title: Değişiklik akışını kullanarak uçtan uca Azure Cosmos DB Java SDK 'Sı v4 uygulaması örneği oluşturma
description: Bu kılavuzda, değişiklik akışını kullanarak kapsayıcının gerçekleştirilmiş bir görünümünü koruyarak Azure Cosmos DB kapsayıcısına belge ekleyen basit bir Java SQL API uygulaması adım adım gösterilmektedir.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: how-to
ms.date: 06/11/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 3f2dcefa8ed2f4b80ec66851cdc67ee2283a6ac7
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87322831"
---
# <a name="how-to-create-a-java-application-that-uses-azure-cosmos-db-sql-api-and-change-feed-processor"></a>SQL API Azure Cosmos DB kullanan bir Java uygulaması oluşturma ve akış işlemcisini değiştirme

Bu nasıl yapılır Kılavuzu, bir Azure Cosmos DB kapsayıcısına belge eklemek için Azure Cosmos DB SQL API 'sini kullanan basit bir Java uygulamasında size kılavuzluk eder ve değişiklik akışı ve değişiklik akışı Işlemcisini kullanarak kapsayıcının gerçekleştirilmiş bir görünümünü sürdürmektedir. Java uygulaması, Azure Cosmos DB Java SDK v4 kullanarak SQL API Azure Cosmos DB ile iletişim kurar.

> [!IMPORTANT]  
> Bu öğretici yalnızca Azure Cosmos DB Java SDK v4 içindir. Daha fazla bilgi için lütfen Azure Cosmos DB Java SDK v4 [sürüm notları](sql-api-sdk-java-v4.md), [maven deposu](https://mvnrepository.com/artifact/com.azure/azure-cosmos), Azure Cosmos DB Java SDK v4 [Performans ipuçları](performance-tips-java-sdk-v4-sql.md)ve Azure Cosmos DB Java SDK v4 [sorun giderme kılavuzunu](troubleshoot-java-sdk-v4-sql.md) görüntüleyin. Şu anda v4 'den daha eski bir sürüm kullanıyorsanız, v4 'ye yükseltme konusunda yardım için [Azure Cosmos DB Java SDK 'sı v4](migrate-java-v4-sdk.md) Kılavuzu ' na bakın.
>

## <a name="prerequisites"></a>Önkoşullar

* Azure Cosmos DB hesabınız için URI ve anahtar

* Maven

* Java 8

## <a name="background"></a>Arka Plan

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

   :::image type="content" source="media/create-sql-api-java-changefeed/cosmos_account_empty.JPG" alt-text="Azure Cosmos DB hesabı":::

1. Uygulamayı terminalde aşağıdaki komutu kullanarak çalıştırın:

    ```bash
    mvn exec:java -Dexec.mainClass="com.azure.cosmos.workedappexample.SampleGroceryStore" -DACCOUNT_HOST="your-account-uri" -DACCOUNT_KEY="your-account-key" -Dexec.cleanupDaemonThreads=false
    ```

1. Gördüğünüz zaman ENTER tuşuna basın

    ```bash
    Press enter to create the grocery store inventory system...
    ```

    ardından Azure portal Veri Gezgini tarayıcınıza geri dönün. Üç boş kapsayıcıyla bir **Market veritabanı** eklendiğini göreceksiniz: 

    * **Inventorycontainer** -bir UUID olan öğe üzerinde bölümlenen, bizim örnek Market depomız için Envanter kaydı ```id``` .
    * **Inventorycontainer-pktype** -stok kaydının gerçekleştirilmiş bir görünümü, öğe üzerindeki sorgular için iyileştirilmiştir```type```
    * **Inventorycontainer-kiralamalar** -her zaman değişiklik akışı için bir kiralama kapsayıcısı gereklidir; kiralamalar değişiklik akışını okurken uygulamanın ilerlemesini izler.

    :::image type="content" source="media/create-sql-api-java-changefeed/cosmos_account_resources_lease_empty.JPG" alt-text="Boş kapsayıcılar":::

1. Terminalde, şimdi bir istem görmeniz gerekir

    ```bash
    Press enter to start creating the materialized view...
    ```

    ENTER tuşuna basın. Aşağıdaki kod bloğu, başka bir iş parçacığında değişiklik akışı işlemcisini yürütecektir ve başlatacak: 

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    [!code-java[](~/azure-cosmos-java-sql-app-example/src/main/java/com/azure/cosmos/workedappexample/SampleGroceryStore.java?name=InitializeCFP)]

    ```"SampleHost_1"```, değişiklik akışı işlemci çalışanının adıdır. ```changeFeedProcessorInstance.start()```Değişiklik akışı işlemcisini asıl olarak başlatır.

    Tarayıcınızda Azure portal Veri Gezgini geri dönün. **Inventorycontainer-kiralamalar** kapsayıcısının altında, içeriğini görmek için **öğeler** ' e tıklayın. Değişiklik akışı Işlemcisinin kira kapsayıcısını doldurduğuna, yani işlemcinin ```SampleHost_1``` bir çalışan tarafından **ınventorycontainer**'in bazı bölümlerinde bir kira atamasıyla karşılaşırsınız.

    :::image type="content" source="media/create-sql-api-java-changefeed/cosmos_leases.JPG" alt-text="Lamalarını":::

1. Terminalde ENTER tuşuna basın. Bu işlem, 10 belgeyi **ınventorycontainer**içine eklenecek şekilde tetikler. Her belge ekleme, değişiklik akışında JSON olarak görünür; Aşağıdaki geri çağırma kodu, JSON belgelerini gerçekleştirilmiş bir görünüme yansıtarak bu olayları işler:

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    [!code-java[](~/azure-cosmos-java-sql-app-example/src/main/java/com/azure/cosmos/workedappexample/SampleGroceryStore.java?name=CFPCallback)]

1. Kodun 5-10sn çalışmasına izin verin. Ardından Azure portal Veri Gezgini geri dönüp **ınventorycontainer > öğelerine**gidin. Öğelerin envanter kapsayıcısına eklenmekte olduğunu görmeniz gerekir; bölüm anahtarına () göz önünde ```id``` .

    :::image type="content" source="media/create-sql-api-java-changefeed/cosmos_items.JPG" alt-text="Akış kapsayıcısı":::

1. Şimdi Veri Gezgini ' de, **ınventorycontainer-pktype > öğelerine**gidin. Bu, bu kapsayıcıdaki öğeler, değişiklik akışı tarafından program aracılığıyla eklendiklerinden, bu kapsayıcıdaki öğelerin, **ınventorycontainer** yansıtmasının gerçekleştirilmiş görünümüdür. Bölüm anahtarına () göz önünde ```type``` . Bu gerçekleştirilmiş görünüm ```type``` , üzerinde bölümlenmiş olduğundan **ınventorycontainer** üzerinde verimsiz olacak sorgular üzerinde filtreleme için en iyi duruma getirilmiştir ```id``` .

    :::image type="content" source="media/create-sql-api-java-changefeed/cosmos_materializedview2.JPG" alt-text="Gerçekleştirilmiş görünüm":::

1. Yalnızca tek bir çağrı kullanarak hem **ınventorycontainer** hem de **ınventorycontainer-pktype** öğesinden bir belgeyi silelim ```upsertItem()``` . İlk olarak, Azure portal Veri Gezgini göz atın. Belgeyi, ```/type == "plums"``` aşağıda kırmızı renkte olduğu gibi sileceğiz

    :::image type="content" source="media/create-sql-api-java-changefeed/cosmos_materializedview-emph-todelete.JPG" alt-text="Gerçekleştirilmiş görünüm":::

    Örnek kodda işlevi çağırmak için yeniden ENTER tuşuna basın ```deleteDocument()``` . Aşağıda gösterilen bu işlev, belge ```/ttl == 5``` yaşam süresi (TTL) ile 5 sn arasında olan belgenin yeni bir sürümünü kaplamıştır. 
    
    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    [!code-java[](~/azure-cosmos-java-sql-app-example/src/main/java/com/azure/cosmos/workedappexample/SampleGroceryStore.java?name=DeleteWithTTL)]

    Değişiklik akışı ```feedPollDelay``` 100 ms olarak ayarlanmıştır; bu nedenle, akış değişikliği bu güncelleştirmeye neredeyse anında ve yukarıda gösterilen çağrılara yanıt verir ```updateInventoryTypeMaterializedView()``` . Bu son işlev çağrısı, 5 San TTL ile yeni belgeyi **ınventorycontainer-pktype**olarak kaplacaktır.

    Bu, yaklaşık 5 saniye sonra, belgenin süresi dolduğunda ve her iki kapsayıcıdan da silinmeyecektir.

    Değişiklik akışı, öğe silme işlemi sırasında değil yalnızca öğe ekleme veya güncelleştirme üzerindeki olayları verdiği için bu yordam gereklidir.

1. Programı kapatmak ve kaynaklarını temizlemek için bir kez daha ENTER tuşuna basın.
