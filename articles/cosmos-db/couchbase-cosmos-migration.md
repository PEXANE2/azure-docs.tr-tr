---
title: CouchBase'den Azure Cosmos DB SQL API'ye geçiş
description: CouchBase'den Azure Cosmos DB SQL API'ye geçiş için adım adım kılavuz
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: mansha
author: manishmsfte
ms.openlocfilehash: 9713d963978e34ad874dc032676a6e1f14e4657c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77210950"
---
# <a name="migrate-from-couchbase-to-azure-cosmos-db-sql-api"></a>CouchBase'den Azure Cosmos DB SQL API'ye geçiş

Azure Cosmos DB ölçeklenebilir, küresel olarak dağıtılmış, tam olarak yönetilen bir veritabanıdır. Verilerinize garantili düşük gecikme gecikmesi erişimi sağlar. Azure Cosmos DB hakkında daha fazla bilgi edinmek için [genel bakış](introduction.md) makalesine bakın. Bu makalede, Couchbase'e bağlı Java uygulamalarını Azure Cosmos DB'deki bir SQL API hesabına geçirmek için yönergeler verilmektedir.

## <a name="differences-in-nomenclature"></a>Adlandırma farklılıkları

Couchbase ile karşılaştırıldığında Azure Cosmos DB'de farklı çalışan temel özellikler şunlardır:

|   Couchbase     |   Azure Cosmos DB   |
| ---------------|-------------------|
|Couchbase sunucusu| Hesap       |
|Kova           | Database      |
|Kova           | Konteyner / Toplama |
|JSON Belgesi    | Öğe / Belge |

## <a name="key-differences"></a>Temel farklılıklar

* Azure Cosmos DB'nin belge içinde bir "Kimlik" alanı, Couchbase'in ise kovanın bir parçası olarak kimliği vardır. "Kimlik" alanı bölüm boyunca benzersizdir.

* Azure Cosmos DB, bölme veya parçalama tekniğini kullanarak ölçeklendirin. Bu da verileri birden çok parçaya/bölüme böldüğü anlamına gelir. Bu bölümler/kırıklar, sağladığınız bölüm anahtarı özelliğine göre oluşturulur. Okuma işlemlerini optimize etmek için bölüm tuşunu seçebilir veya en iyi duruma getirilmiş okuma/yazma işlemlerini de seçebilirsiniz. Daha fazla bilgi edinmek için [bölümleme](./partition-data.md) makalesine bakın.

* Azure Cosmos DB'de, koleksiyon adı zaten var olduğundan, üst düzey hiyerarşinin koleksiyonu belirtmesi gerekmez. Bu özellik JSON yapısını çok daha basit hale getirir. Aşağıda, Couchbase ve Azure Cosmos DB arasındaki veri modelifarklılıklarını gösteren bir örnek verilmiştir:

   **Couchbase**: Belge Kimliği = "99FF4444"

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

   **Azure Cosmos DB**: Aşağıda gösterildiği gibi belge içindeki "ID"e bakın

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

Azure Cosmos DB, farklı Java çerçevelerini desteklemek için SDK'ları takip etmiştir:

* Async SDK
* Bahar Çizme SDK

Aşağıdaki bölümlerde bu SDK'ların her birinin ne zaman kullanılacağı açıklanıyor. Üç tür iş yüküne sahip olduğumuz bir örnek düşünün:

## <a name="couchbase-as-document-repository--spring-data-based-custom-queries"></a>Belge deposu olarak Couchbase & yay veri tabanlı özel sorgular

Geçirtme nizin iş yükü Bahar Önyükleme Tabanlı SDK'ya dayanıyorsa, aşağıdaki adımları kullanabilirsiniz:

1. POM.xml dosyasına üst öğe ekleyin:

   ```java
   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.1.5.RELEASE</version>
      <relativePath/>
   </parent>
   ```

1. POM.xml dosyasına özellikler ekleyin:

   ```java
   <azure.version>2.1.6</azure.version>
   ```

1. POM.xml dosyasına bağımlılıklar ekleyin:

   ```java
   <dependency>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
       <version>2.1.6</version>
   </dependency>
   ```

1. Kaynakların altına uygulama özellikleri ekleyin ve aşağıdakileri belirtin. URL, anahtar ve veritabanı adı parametrelerini değiştirdiğinden emin olun:

   ```java
      azure.cosmosdb.uri=<your-cosmosDB-URL>
      azure.cosmosdb.key=<your-cosmosDB-key>
      azure.cosmosdb.database=<your-cosmosDB-dbName>
   ```

1. Modeldeki koleksiyonun adını tanımlayın. Ayrıca ek açıklamalar da belirtebilirsiniz. Örneğin, id, bölüm anahtarı açıkça belirtmek için:

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

CRUD işlemleri için kod parçacıkları şunlardır:

### <a name="insert-and-update-operations"></a>Ekleme ve güncelleştirme işlemleri

*Burada _repo* depo nesnesi ve *doc* POJO sınıfının nesnesidir. Eklemek veya `.save` eklemek için kullanabilirsiniz (belirtilen kimliği bulunan belge varsa). Aşağıdaki kod snippet nasıl bir doc nesnesi eklemek veya güncelleştirmegösterir:

```_repo.save(doc);```

### <a name="delete-operation"></a>Silme İşlemi

Doküman nesnesinin nesneyi bulmak ve silmek için kimlik ve bölüm anahtarının zorunlu olacağı aşağıdaki kod parçacıklarını düşünün:

```_repo.delete(doc);```

### <a name="read-operation"></a>Okuma İşlemi

Bölümü anahtarını belirtmeden veya belirtmeden belgeyi okuyabilirsiniz. Bölüm anahtarını belirtmezseniz, bu sorgu çapraz bölüm sorgusu olarak kabul edilir. Aşağıdaki kod örneklerini göz önünde bulundurun, ilk iDim ve bölüm anahtarı alanını kullanarak işlem gerçekleştirecektir. İkinci örnek, bölüm tuşu alanını belirtmeden düzenli bir alan & kullanır.

* ```_repo.findByIdAndName(objDoc.getId(),objDoc.getName());```
* ```_repo.findAllByStatus(objDoc.getStatus());```

Artık uygulamanızı Azure Cosmos DB ile kullanabilirsiniz. Bu dokümanda açıklanan örnek için tam kod örneği [CouchbaseToCosmosDB-SpringCosmos](https://github.com/Azure-Samples/couchbaseTocosmosdb/tree/master/SpringCosmos) GitHub repo'da mevcuttur.

## <a name="couchbase-as-a-document-repository--using-n1ql-queries"></a>N1QL sorgularını kullanarak belge deposu & olarak Couchbase

N1QL sorguları Couchbase sorguları tanımlamak için bir yoldur.

|N1QL Sorgusu | Azure CosmosDB Sorgusu|
|-------------------|-------------------|
|SELECT`TravelDocument`META( ).id `TravelDocument`AS ID, .* FROM `TravelDocument` WHERE `_type` = "com.xx.xx.xx.xxx.xxx.xxxx " ve ülke = 'Hindistan' ve HER HANGİ m Vizesi SATISFIES m.type == 'Multi-Entry' ve m.Country IN ['Hindistan', Bhutan'] SİPARİş ` Validity` DESC LIMIT 25 OFFSET 0   | SELECT c.id,c FROM c JOIN m in c.country='India' WHERE c._type = " com.xx.xx.xx.xxx.xxx.xxxx" ve c.country = 'Hindistan' ve m.type = 'Multi-Entry' ve m.Country IN ('Hindistan', 'Bhutan') SİPARİş C.Geçerlilik DESC OFSET 0 LIMIT 25 |

N1QL sorgularınızda aşağıdaki değişiklikleri fark edebilirsiniz:

* META anahtar sözcüklerini kullanmanız veya birinci düzey belgeye başvurmanız gerekmez. Bunun yerine kapsayıcı için kendi başvuru oluşturabilirsiniz. Bu örnekte, biz "c" (herhangi bir şey olabilir) olarak kabul ettik. Bu başvuru, tüm birinci düzey alanlar için bir önek olarak kullanılır. Fr örnek, c.id, c.country vb.

* "ANY" yerine artık alt belgeüzerinde birbirleştirme yapabilir ve "m" gibi özel bir takma adla başvurabilirsiniz. Bir alt belge için takma ad oluşturduktan sonra takma ad kullanmanız gerekir. Örneğin, m.Country.

* AZURE Cosmos DB sorgusunda OFSET dizisi farklıdır, önce OFSET sonra LIMIT belirtmeniz gerekir. Sorguyu Azure Cosmos DB'ye aktarırken istemci tarafında gereksiz ek yükü olabileceğinden, maksimum özel tanımlı sorgular kullanıyorsanız, Yay Veri SDK'sını kullanmamanız önerilir. Bunun yerine bu durumda çok verimli bir şekilde kullanılabilir doğrudan Async Java SDK, var.

### <a name="read-operation"></a>İşlemi okuma

Async Java SDK'yı aşağıdaki adımlarla kullanın:

1. Aşağıdaki bağımlılığı POM.xml dosyasına yapılandırın:

   ```java
   <!-- https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb -->
   <dependency>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-cosmos</artifactId>
       <version>3.0.0</version>
   </dependency>
   ```

1. Aşağıdaki örnekte gösterildiği `ConnectionBuilder` yöntemi kullanarak Azure Cosmos DB için bir bağlantı nesnesi oluşturun. Aşağıdaki kodun yalnızca bir kez yürütülmesi gerektiğini fasulye içine bu bildirimi koymak emin olun:

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

1. Sorguyu yürütmek için aşağıdaki kod parçacıklarını çalıştırmanız gerekir:

   ```java
   Flux<FeedResponse<CosmosItemProperties>> objFlux= container.queryItems(query, fo);
   ```

Şimdi, yukarıdaki yöntem yardımıyla birden çok sorguları geçirebilirsiniz ve herhangi bir güçlük olmadan yürütmek. Birden çok sorguya bölünebilen büyük bir sorgu yürütme gereksiniminiz varsa, bir önceki yerine aşağıdaki kod parçacıklarını deneyin:

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

Önceki kodla, sorguları paralel olarak çalıştırabilir ve en iyi duruma getirmek için dağıtılmış yürütmeleri artırabilirsiniz. Ayrıca ekleme ve güncelleştirme işlemlerini de çalıştırabilirsiniz:

### <a name="insert-operation"></a>Ekleme işlemi

Belgeyi eklemek için aşağıdaki kodu çalıştırın:

```java 
Mono<CosmosItemResponse> objMono= container.createItem(doc,ro);
```

Sonra Mono'ya şu şekilde abone olun:

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

### <a name="upsert-operation"></a>Yükseltme işlemi

Yukarı işlemi, güncelleştirilmesi gereken belgeyi belirtmenizi gerektirir. Belgenin tamamını almak için okuma işlemi başlığı altında belirtilen parçacığı kullanabilirsiniz ve ardından gerekli alan(lar) değiştirebilirsiniz. Aşağıdaki kod snippet belgeyi yukarılar:

```java
Mono<CosmosItemResponse> obs= container.upsertItem(doc, ro);
```
Sonra mono abone olun. Ekleme işlemindeki mono abonelik snippet'ine bakın.

### <a name="delete-operation"></a>İşlemi silme

Aşağıdaki snippet silme işlemi yapacaktır:

```java     
CosmosItem objItem= container.getItem(doc.Id, doc.Tenant);
Mono<CosmosItemResponse> objMono = objItem.delete(ro);
```

Sonra mono abone olun, ekleme işleminde mono abonelik snippet bakın. Tam kod örneği [CouchbaseToCosmosDB-AsyncInSpring](https://github.com/Azure-Samples/couchbaseTocosmosdb/tree/master/AsyncInSpring) GitHub repo mevcuttur.

## <a name="couchbase-as-a-keyvalue-pair"></a>Anahtar/değer çifti olarak Couchbase

Bu, sorgular yerine arama yapabileceğiniz basit bir iş yükü türüdür. Anahtar/değer çiftleri için aşağıdaki adımları kullanın:

1. "/ID"yi birincil anahtar olarak kullanmayı düşünün, bu da arama işlemini doğrudan belirli bir bölümde gerçekleştirebileceğinizden emin olur. Bir koleksiyon oluşturun ve bölüm anahtarı olarak "/ID" belirtin.

1. Dizin oluşturmayı tamamen kapatın. Arama işlemlerini yürüteceğiniz için, ek yükü dizin taşımanın bir anlamı yoktur. Dizin oluşturmayı kapatmak için Azure portalında oturum açın ve Azure Cosmos DB Hesabı'na gidin. Veri **Gezgini'ni**açın, **Veritabanınızı** ve **Kapsayıcı'yı**seçin. Ölçek **& Ayarlar** sekmesini açın ve **Dizin Oluşturma İlkesi'ni**seçin. Şu anda dizine alma ilkesi aşağıdaki gibi görünür:
    
   ```json
   {
       "indexingMode": "consistent",
       "includedPaths": 
       [
           {
            "path": "/*",
            "indexes": 
             [
                {
                  "kind": "Range",
                  "dataType": "Number"
                },
                {
                  "kind": "Range",
                  "dataType": "String"
                },
                {
                   "kind": "Spatial",
                   "dataType": "Point"
                }
             ]
          }
       ],
       "excludedPaths": 
       [
         {
             "path": "/path/to/single/excluded/property/?"
         },
         {
             "path": "/path/to/root/of/multiple/excluded/properties/*"
         }
      ]
   }
   ````

   Yukarıdaki dizin oluşturma ilkesini aşağıdaki ilkeile değiştirin:

   ```json
   {
       "indexingMode": "none"
   }
   ```

1. Bağlantı nesnesini oluşturmak için aşağıdaki kod parçacıklarını kullanın. Bağlantı Nesnesi (yerleştirilecek @Bean veya statik hale getirilecek):

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

Şimdi CRUD işlemlerini aşağıdaki gibi yürütebilirsiniz:

### <a name="read-operation"></a>İşlemi okuma

Öğeyi okumak için aşağıdaki parçacığı kullanın:

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

Bir öğe eklemek için aşağıdaki kodu gerçekleştirebilirsiniz:

```java 
Mono<CosmosItemResponse> objMono= container.createItem(doc,ro);
```

Sonra mono abone olarak:

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

### <a name="upsert-operation"></a>Yükseltme işlemi

Bir öğenin değerini güncelleştirmek için aşağıdaki kod parçacığına bakın:

```java
Mono<CosmosItemResponse> obs= container.upsertItem(doc, ro);
```
Sonra mono abone olun, ekleme işleminde mono abonelik snippet bakın.

### <a name="delete-operation"></a>İşlemi silme

Silme işlemini yürütmek için aşağıdaki snippet'i kullanın:

```java     
CosmosItem objItem= container.getItem(id, id);
Mono<CosmosItemResponse> objMono = objItem.delete(ro);
```

Sonra mono abone olun, ekleme işleminde mono abonelik snippet bakın. Tam kod örneği [CouchbaseToCosmosDB-AsyncKeyValue](https://github.com/Azure-Samples/couchbaseTocosmosdb/tree/master/AsyncKeyValue) GitHub repo mevcuttur.

## <a name="data-migration"></a>Veri Taşıma

Verileri geçirmenin iki yolu vardır.

* **Azure Veri Fabrikası'nı kullanın:** Bu, verileri geçirmek için en çok önerilen yöntemdir. Kaynağı Couchbase olarak yapılandırın ve Azure Cosmos DB SQL API olarak battı, ayrıntılı adımlar için Azure [Cosmos DB Data Factory bağlayıcısı](../data-factory/connector-azure-cosmos-db.md) makalesine bakın.

* **Azure Cosmos DB veri alma aracını kullanın:** Bu seçenek, daha az veri miktarı ile VM'ler kullanarak geçiş için önerilir. Ayrıntılı adımlar için [Veri aktarıcısı](./import-data.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki Adımlar

* Performans testi yapmak için [Azure Cosmos DB makalesiyle Performans ve ölçek testine](./performance-testing.md) bakın.
* Kodu optimize etmek [için Azure Cosmos DB](./performance-tips-async-java.md) makalesi için Performans ipuçlarına bakın.
* Java Async V3 SDK, [SDK referans](https://github.com/Azure/azure-cosmosdb-java/tree/v3) GitHub repo keşfedin.
