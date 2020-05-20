---
title: Couşbase 'ten Azure Cosmos DB SQL API 'sine geçiş
description: Couşbase 'ten Azure Cosmos DB SQL API 'sine geçiş için adım adım yönergeler
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: mansha
author: manishmsfte
ms.openlocfilehash: 248860ad6963fcd04526f0d94e52d6a6181463c5
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83657351"
---
# <a name="migrate-from-couchbase-to-azure-cosmos-db-sql-api"></a>Couşbase 'ten Azure Cosmos DB SQL API 'sine geçiş

Azure Cosmos DB, ölçeklenebilir, global olarak dağıtılmış, tam olarak yönetilen bir veritabanıdır. Verilerinize garantili düşük gecikme süresi erişimi sağlar. Azure Cosmos DB hakkında daha fazla bilgi edinmek için [genel bakış](introduction.md) makalesine bakın. Bu makalede, Couşbase 'e bağlı Java uygulamalarını Azure Cosmos DB 'deki bir SQL API hesabına geçirmeye yönelik yönergeler sağlanmaktadır.

## <a name="differences-in-nomenclature"></a>Terminolojiyle farklılıklar

Aşağıda, Couşbase ile karşılaştırıldığında Azure Cosmos DB farklı şekilde çalışan temel özellikler verilmiştir:

|   Couchbase     |   Azure Cosmos DB   |
| ---------------|-------------------|
|Couşbase sunucusu| Hesap       |
|Demet           | Veritabanı      |
|Demet           | Kapsayıcı/koleksiyon |
|JSON belgesi    | Öğe/belge |

## <a name="key-differences"></a>Temel farklılıklar

* Azure Cosmos DB belge içinde bir "ID" alanı vardır; Couşbase, demet 'nin bir parçası olarak KIMLIĞE sahiptir. "KIMLIK" alanı bölüm genelinde benzersizdir.

* Bölümlendirme veya parçalama tekniğini kullanarak Azure Cosmos DB ölçeklendirir. Yani, verileri birden çok parça/bölüme böler. Bu bölümler/parçalar, sağladığınız bölüm anahtarı özelliği temel alınarak oluşturulur. Okuma ve yazma işlemlerini iyileştirmek ya da çok iyileştirilmiş okuma/yazma işlemleri için bölüm anahtarını seçebilirsiniz. Daha fazla bilgi için [bölümlendirme](./partition-data.md) makalesine bakın.

* Azure Cosmos DB, koleksiyon adı zaten mevcut olduğundan üst düzey hiyerarşinin koleksiyonu belirtmek için gerekli değildir. Bu özellik, JSON yapısını çok daha kolay hale getirir. Aşağıdaki örnek, Couşbase ve Azure Cosmos DB arasında veri modelinde farkları gösteren bir örnektir:

   **Couşbase**: belge kimliği = "99FF4444"

    ```json
    {
      "TravelDocument":
      {
       "Country":"India",
      "Validity" : "2022-09-01",
        "Person":
        {
          "Name": "Manish",
          "Address": "AB Road, City-z"
        },
        "Visas":
        [
          {
          "Country":"India",
          "Type":"Multi-Entry",
          "Validity":"2022-09-01"
          },
          {
          "Country":"US",
          "Type":"Single-Entry",
          "Validity":"2022-08-01"
          }
        ]
      }
    }
   ```

   **Azure Cosmos DB**: belge içinde aşağıda gösterildiği gıbı "kimlik" öğesine bakın

    ```json
    {
      "id" : "99FF4444",
    
      "Country":"India",
       "Validity" : "2022-09-01",
        "Person":
        {
          "Name": "Manish",
          "Address": "AB Road, City-z"
        },
        "Visas":
        [
          {
          "Country":"India",
          "Type":"Multi-Entry",
          "Validity":"2022-09-01"
          },
          {
          "Country":"US",
          "Type":"Single-Entry",
          "Validity":"2022-08-01"
          }
        ]
      }
    
    ```
         
## <a name="java-sdk-support"></a>Java SDK desteği

Azure Cosmos DB, farklı Java çerçevelerini desteklemek için aşağıdaki SDK 'lara sahiptir:

* Zaman uyumsuz SDK
* Yay önyükleme SDK 'Sı

Aşağıdaki bölümlerde bu SDK 'ların her birinin ne zaman kullanılacağı açıklanır. Üç tür iş yükünden oluşan bir örnek düşünün:

## <a name="couchbase-as-document-repository--spring-data-based-custom-queries"></a>Belge deposu olarak couşbase & yay veri tabanlı özel sorgular

Geçirdiğiniz iş yükü Spring Boot tabanlı SDK 'yı temel alıyorsa aşağıdaki adımları kullanabilirsiniz:

1. Pod. xml dosyasına üst öğe ekleyin:

   ```java
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.1.5.RELEASE</version>
      <relativePath/>
   </parent>
   ```

1. Pod. xml dosyasına özellikler ekleyin:

   ```java
   <azure.version>2.1.6</azure.version>
   ```

1. Pod. xml dosyasına bağımlılıklar ekleyin:

   ```java
   <dependency>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
       <version>2.1.6</version>
   </dependency>
   ```

1. Kaynaklar altına uygulama özellikleri ekleyin ve aşağıdakileri belirtin. URL, anahtar ve veritabanı adı parametrelerini değiştirdiğinizden emin olun:

   ```java
      azure.cosmosdb.uri=<your-cosmosDB-URL>
      azure.cosmosdb.key=<your-cosmosDB-key>
      azure.cosmosdb.database=<your-cosmosDB-dbName>
   ```

1. Modeldeki koleksiyonun adını tanımlayın. Ayrıca, daha fazla ek açıklama da belirtebilirsiniz. Örneğin, KIMLIĞI, Bölüm anahtarını açıkça belirtmek için:

   ```java
   @Document(collection = "mycollection")
       public class User {
           @id
           private String id;
           private String firstName;
           @PartitionKey
           private String lastName;
       }
   ```

CRUD işlemlerine yönelik kod parçacıkları aşağıda verilmiştir:

### <a name="insert-and-update-operations"></a>Ekleme ve güncelleştirme işlemleri

Burada *_repo* deponun nesnesidir ve *doc* ise Pojo sınıfının nesnesidir. ' İ `.save` (BELIRTILEN kimliğe sahip bir belge bulunursa) eklemek veya daha fazla kullanmak için kullanabilirsiniz. Aşağıdaki kod parçacığı bir belge nesnesinin nasıl ekleneceğini veya güncelleştirilmesini göstermektedir:

```_repo.save(doc);```

### <a name="delete-operation"></a>Silme Işlemi

Aşağıdaki kod parçacığını göz önünde bulundurun. burada, belge nesnesinin, nesne bulmak ve silmek için KIMLIK ve bölüm anahtarı zorunludur.

```_repo.delete(doc);```

### <a name="read-operation"></a>Okuma Işlemi

Bölüm anahtarını belirtmeden veya belirtmeden belgeyi okuyabilirsiniz. Bölüm anahtarını belirtmezseniz, bu, çapraz bölümlü bir sorgu olarak kabul edilir. Aşağıdaki kod örneklerini göz önünde bulundurun, ilki KIMLIĞI ve bölüm anahtarı alanını kullanarak işlem gerçekleştirir. İkinci örnek, bölüm anahtarı alanını belirtmeden & düzenli bir alan kullanır.

* ```_repo.findByIdAndName(objDoc.getId(),objDoc.getName());```
* ```_repo.findAllByStatus(objDoc.getStatus());```

Bu, artık uygulamanızı Azure Cosmos DB kullanarak kullanabilirsiniz. Bu belgede açıklanan örnek için tam kod örneği [Couşbasetocosmosdb-SpringCosmos](https://github.com/Azure-Samples/couchbaseTocosmosdb/tree/master/SpringCosmos) GitHub deposunda mevcuttur.

## <a name="couchbase-as-a-document-repository--using-n1ql-queries"></a>N1QL sorguları kullanarak belge deposu olarak couşbase &

N1QL sorguları, Couşbase 'de sorguları tanımlamanın yoludur.

|N1QL sorgusu | Azure CosmosDB sorgusu|
|-------------------|-------------------|
|META ( `TravelDocument` ). ID as ID, `TravelDocument` . * from `TravelDocument` `_type` = "com. xx. xx. xx. xxx. xxx. xxxx" ve Country = ' Hindistan ' ve Vizas 'deki HERHANGI bir d 'yi karşılayan bir d. Type = = ' Multi-entry ' ve d. Country, ` Validity` DESC sınırına 25 fark 0   | C 'yi SEÇIN. c. Country = ' Hindistan ' ve c. _type = "com. xx. xx. xx. xxx. xxx. xxxx" ve c. Country = ' Hindistan ' ve m. Type = ' Multi-entry ' ve m. Country IN (' Hindistan ', ' Bhutan ') SıRASıYLA c. geçerlilik DESC KAYMASı 0 sınır 25 ' i r. |

N1QL sorgularınızda aşağıdaki değişiklikleri görebilirsiniz:

* META anahtar sözcüğünü kullanmanız veya ilk düzey belgeye başvurmanız gerekmez. Bunun yerine, kapsayıcıya kendi başvurunuz için bir başvuru oluşturabilirsiniz. Bu örnekte, bunu "c" olarak kabul ettik (herhangi bir şey olabilir). Bu başvuru, tüm ilk düzey alanların öneki olarak kullanılır. Fr example, c.id, c. ülke vb.

* Artık "ANY" yerine, alt belge üzerinde bir JOIN yapabilir ve "d" gibi özel bir diğer ad ile başvurabilirsiniz. Bir alt belge için diğer ad oluşturduktan sonra diğer adı kullanmanız gerekir. Örneğin, a. ülke.

* Azure Cosmos DB sorgusunda, fark sırası farklıdır, önce sapmayı belirtmeniz ve ardından SıNıRı belirtmeniz gerekir. Sorgu Azure Cosmos DB ' a geçirilirken istemci tarafında gerekli olan ek yüke sahip olduğundan, en fazla özel tanımlanmış sorgu kullanıyorsanız Spring Data SDK kullanılması önerilir. Bunun yerine, bu durumda çok verimli bir şekilde kullanılabilecek doğrudan zaman uyumsuz bir Java SDK 'Sı sunuyoruz.

### <a name="read-operation"></a>Okuma işlemi

Zaman uyumsuz Java SDK 'sını aşağıdaki adımlarla kullanın:

1. Pod. xml dosyası üzerinde aşağıdaki bağımlılığı yapılandırın:

   ```java
   <!-- https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb -->
   <dependency>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-cosmos</artifactId>
       <version>3.0.0</version>
   </dependency>
   ```

1. Aşağıdaki örnekte gösterildiği gibi yöntemini kullanarak Azure Cosmos DB için bir bağlantı nesnesi oluşturun `ConnectionBuilder` . Bu bildirimi, aşağıdaki kodun yalnızca bir kez yürütülmesi gereken şekilde çekirdeklere 'e yerleştirdiğinizden emin olun:

   ```java
   ConnectionPolicy cp=new ConnectionPolicy();
   cp.connectionMode(ConnectionMode.DIRECT);
    
   if(client==null)
    client= CosmosClient.builder()
        .endpoint(Host)//(Host, MasterKey, dbName, collName).Builder()
        .connectionPolicy(cp)
        .key(MasterKey)
        .consistencyLevel(ConsistencyLevel.EVENTUAL)
        .build();   
   
   container = client.getDatabase(_dbName).getContainer(_collName);
   ```

1. Sorguyu yürütmek için aşağıdaki kod parçacığını çalıştırmanız gerekir:

   ```java
   Flux<FeedResponse<CosmosItemProperties>> objFlux= container.queryItems(query, fo);
   ```

Şimdi, yukarıdaki yöntemin yardımıyla birden çok sorgu geçirebilir ve sorunsuz bir şekilde yürütebilirsiniz. Birden çok sorguya bölünebilen bir büyük sorgu yürütme gereksinimine sahipseniz, daha sonra bir önceki kod parçacığını deneyin:

```java
for(SqlQuerySpec query:queries)
{
    objFlux= container.queryItems(query, fo);
    objFlux .publishOn(Schedulers.elastic())
            .subscribe(feedResponse->
                {
                    if(feedResponse.results().size()>0)
                    {
                        _docs.addAll(feedResponse.results());
                    }
                
                },
                Throwable::printStackTrace,latch::countDown);
    lstFlux.add(objFlux);
}
                        
        Flux.merge(lstFlux);
        latch.await();
}
```

Önceki kodla sorguları paralel olarak çalıştırabilir ve iyileştirmek için dağıtılmış yürütmelerin artmasını sağlayabilirsiniz. Diğer bir deyişle ekleme ve güncelleştirme işlemlerini de çalıştırabilirsiniz:

### <a name="insert-operation"></a>Ekleme işlemi

Belgeyi eklemek için aşağıdaki kodu çalıştırın:

```java 
Mono<CosmosItemResponse> objMono= container.createItem(doc,ro);
```

Ardından mono 'ya abone ol:

```java
CountDownLatch latch=new CountDownLatch(1);
objMono .subscribeOn(Schedulers.elastic())
        .subscribe(resourceResponse->
        {
            if(resourceResponse.statusCode()!=successStatus)
                {
                    throw new RuntimeException(resourceResponse.toString());
                }
            },
        Throwable::printStackTrace,latch::countDown);
latch.await();              
```

### <a name="upsert-operation"></a>Upsert işlem

Upsert işlem, güncelleştirilmesi gereken belgeyi belirtmenizi gerektirir. Tüm belgeyi getirmek için, başlık okuma işlemi altında belirtilen kod parçacığını kullanabilir sonra gerekli alanları değiştirebilirsiniz. Aşağıdaki kod parçacığı belgeyi çıkarır:

```java
Mono<CosmosItemResponse> obs= container.upsertItem(doc, ro);
```
Ardından mono 'ya abone olun. Ekleme işleminde mono abonelik kod parçacığına bakın.

### <a name="delete-operation"></a>Silme işlemi

Aşağıdaki kod parçacığı, silme işlemini yapılacak:

```java     
CosmosItem objItem= container.getItem(doc.Id, doc.Tenant);
Mono<CosmosItemResponse> objMono = objItem.delete(ro);
```

Ardından mono 'ya abone olun, ekleme işleminde mono abonelik kod parçacığına bakın. Tam kod örneği, [Couşbasetocosmosdb-Asynınspring](https://github.com/Azure-Samples/couchbaseTocosmosdb/tree/master/AsyncInSpring) GitHub deposunda mevcuttur.

## <a name="couchbase-as-a-keyvalue-pair"></a>Anahtar/değer çifti olarak couşbase

Bu, sorgular yerine arama gerçekleştirebileceğiniz basit bir iş yükü türüdür. Anahtar/değer çiftleri için aşağıdaki adımları kullanın:

1. Birincil anahtar olarak "/ID" seçeneğini göz önünde bulundurun. Bu, arama işlemini doğrudan belirli bölümde gerçekleştirebilmeniz için sağlar. Bir koleksiyon oluşturun ve bölüm anahtarı olarak "/ID" belirtin.

1. Dizin oluşturma işleminin tamamen devre dışı bırakın. Arama işlemlerini yürütecağından, dizin oluşturma ek yükü taşıma noktası yoktur. Dizin oluşturmayı devre dışı bırakmak için Azure portal oturum açın, Azure Cosmos DB hesaba gidin. **Veri Gezgini**açın, **veritabanınızı** ve **kapsayıcıyı**seçin. **Ölçek & ayarları** sekmesini açın ve **Dizin oluşturma ilkesini**seçin. Şu anda dizin oluşturma ilkesi şu şekilde görünür:
    
   ```json
   {
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": [
        {
            "path": "/\"_etag\"/?"
        }
    ]
    }
   ````

   Yukarıdaki dizin oluşturma ilkesini aşağıdaki ilkeyle değiştirin:

   ```json
   {
    "indexingMode": "none",
    "automatic": false,
    "includedPaths": [],
    "excludedPaths": []
    }
   ```

1. Bağlantı nesnesini oluşturmak için aşağıdaki kod parçacığını kullanın. Bağlantı nesnesi (içine yerleştirilecek @Bean veya statik hale getirmek için):

   ```java
   ConnectionPolicy cp=new ConnectionPolicy();
   cp.connectionMode(ConnectionMode.DIRECT);
   
   if(client==null)
    client= CosmosClient.builder()
        .endpoint(Host)//(Host, MasterKey, dbName, collName).Builder()
        .connectionPolicy(cp)
        .key(MasterKey)
        .consistencyLevel(ConsistencyLevel.EVENTUAL)
        .build();
    
   container = client.getDatabase(_dbName).getContainer(_collName);
   ```

Artık CRUD işlemlerini aşağıdaki gibi çalıştırabilirsiniz:

### <a name="read-operation"></a>Okuma işlemi

Öğeyi okumak için aşağıdaki kod parçacığını kullanın:

```java        
CosmosItemRequestOptions ro=new CosmosItemRequestOptions();
ro.partitionKey(new PartitionKey(documentId));
CountDownLatch latch=new CountDownLatch(1);
        
var objCosmosItem= container.getItem(documentId, documentId);
Mono<CosmosItemResponse> objMono = objCosmosItem.read(ro);
objMono .subscribeOn(Schedulers.elastic())
        .subscribe(resourceResponse->
        {
            if(resourceResponse.item()!=null)
            {
                doc= resourceResponse.properties().toObject(UserModel.class);
            }
        },
        Throwable::printStackTrace,latch::countDown);
latch.await();
```

### <a name="insert-operation"></a>Ekleme işlemi

Bir öğe eklemek için aşağıdaki kodu uygulayabilirsiniz:

```java 
Mono<CosmosItemResponse> objMono= container.createItem(doc,ro);
```

Ardından mono 'ya abone ol:

```java
CountDownLatch latch=new CountDownLatch(1);
objMono.subscribeOn(Schedulers.elastic())
        .subscribe(resourceResponse->
        {
            if(resourceResponse.statusCode()!=successStatus)
                {
                    throw new RuntimeException(resourceResponse.toString());
                }
            },
        Throwable::printStackTrace,latch::countDown);
latch.await();
```

### <a name="upsert-operation"></a>Upsert işlem

Bir öğenin değerini güncelleştirmek için aşağıdaki kod parçacığını inceleyin:

```java
Mono<CosmosItemResponse> obs= container.upsertItem(doc, ro);
```
Ardından mono 'ya abone olun, ekleme işleminde mono abonelik kod parçacığına bakın.

### <a name="delete-operation"></a>Silme işlemi

Silme işlemini yürütmek için aşağıdaki kod parçacığını kullanın:

```java     
CosmosItem objItem= container.getItem(id, id);
Mono<CosmosItemResponse> objMono = objItem.delete(ro);
```

Ardından mono 'ya abone olun, ekleme işleminde mono abonelik kod parçacığına bakın. Tam kod örneği [Couşbasetocosmosdb-AsyncKeyValue](https://github.com/Azure-Samples/couchbaseTocosmosdb/tree/master/AsyncKeyValue) GitHub deposunda mevcuttur.

## <a name="data-migration"></a>Veri Taşıma

Veri geçişi için iki yol vardır.

* **Azure Data Factory kullanın:** Bu, verileri geçirmek için en önerilen yöntemdir. Kaynağı Couşbase ve Sink olarak Yapılandırma Azure Cosmos DB SQL API 'SI, ayrıntılı adımlar için bkz. Azure [Cosmos DB Data Factory Bağlayıcısı](../data-factory/connector-azure-cosmos-db.md) makalesi.

* **Azure Cosmos DB veri alma aracını kullanın:** Bu seçenek, daha az miktarda veri içeren VM 'Leri kullanarak geçiş yapmak için önerilir. Ayrıntılı adımlar için bkz. [veri alma](./import-data.md) makalesi.

## <a name="next-steps"></a>Sonraki Adımlar

* Performans testi yapmak için bkz. [Azure Cosmos DB makalesinde performans ve ölçek testi](./performance-testing.md) .
* Kodu iyileştirmek için [Azure Cosmos DB makalesine yönelik performans ipuçları](./performance-tips-async-java.md) bölümüne bakın.
* Java Async v3 SDK, [SDK başvuru](https://github.com/Azure/azure-cosmosdb-java/tree/v3) GitHub deposu ' nu keşfet.
