---
title: Async Java için Azure Cosmos DB performans ipuçları
description: Azure Cosmos veritabanı performansını artırmak için istemci yapılandırma seçeneklerini öğrenin
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.openlocfilehash: b892b1f4ff73679ab425d0e97f5361e0f3712252
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549179"
---
# <a name="performance-tips-for-azure-cosmos-db-and-async-java"></a>Azure Cosmos DB ve Async Java için performans ipuçları

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB, garantili gecikme sonu ve iş ortası ile sorunsuz ölçeklendirilebilen hızlı ve esnek dağıtılmış bir veritabanıdır. Veritabanınızı Azure Cosmos DB ile ölçeklendirmek için büyük mimari değişiklikleri yapmanız veya karmaşık kod yazmanız gerekmez. Yukarı ve aşağı ölçekleme tek bir API araması veya SDK yöntemi araması yapmak kadar kolaydır. Ancak, Azure Cosmos DB ağ aramaları üzerinden erişilen çünkü [SQL Async Java SDK](sql-api-sdk-async-java.md)kullanırken en yüksek performans elde etmek için yapabileceğiniz istemci tarafı optimizasyonları vardır.

"Veritabanı performansımı nasıl artırabilirim?" diye soruyorsanız. aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ

* **Bağlantı modu: Doğrudan modunu kullan**
<a id="direct-connection"></a>
    
    Bir istemcinin Azure Cosmos DB'ye nasıl bağlandığı, özellikle istemci tarafı gecikmesi açısından performans üzerinde önemli etkileri vardır. *ConnectionMode* istemci *ConnectionPolicy*yapılandırmak için kullanılabilir önemli bir yapılandırma ayarıdır. Async Java SDK için kullanılabilir iki Bağlantı Modu şunlardır:  
      
    * [Ağ geçidi (varsayılan)](/java/api/com.microsoft.azure.cosmosdb.connectionmode)  
    * [Direct](/java/api/com.microsoft.azure.cosmosdb.connectionmode)

    Ağ geçidi modu tüm SDK platformlarında desteklenir ve varsayılan olarak yapılandırılan seçenektir. Uygulamalarınız sıkı güvenlik duvarı kısıtlamaları olan bir şirket ağı içinde çalıştırılırsa, standart HTTPS bağlantı noktasını ve tek bir bitiş noktasını kullandığından Ağ Geçidi modu en iyi seçimdir. Ancak performans dengelemesi, Ağ Geçidi modunun azure Cosmos DB'ye her veri okunduveya yazıldığında ek bir ağ atlama içer. Bu nedenle, Doğrudan mod daha az ağ atlamaları nedeniyle daha iyi performans sunar.

    *ConnectionMode,* *ConnectionPolicy* parametresi ile *DocumentClient* örneğinin yapımı sırasında yapılandırılır.
    
    ```java
        public ConnectionPolicy getConnectionPolicy() {
          ConnectionPolicy policy = new ConnectionPolicy();
          policy.setConnectionMode(ConnectionMode.Direct);
          policy.setMaxPoolSize(1000);
          return policy;
        }

        ConnectionPolicy connectionPolicy = new ConnectionPolicy();
        DocumentClient client = new DocumentClient(HOST, MASTER_KEY, connectionPolicy, null);
    ```

* **Performans için istemcileri aynı Azure bölgesinde birlikte bulundurun** <a id="same-region"></a>

    Mümkün olduğunda, Azure Cosmos DB çağıran uygulamaları Azure Cosmos veritabanıyla aynı bölgeye yerleştirin. Yaklaşık bir karşılaştırma için, aynı bölgedeki Azure Cosmos DB çağrıları 1-2 ms içinde tamamlanır, ancak ABD'nin Batı ve Doğu kıyıları arasındaki gecikme 50 >. Bu gecikme gecikmesi, istemciden Azure veri merkezi sınırına geçerken isteğin aldığı rotaya bağlı olarak istekten isteğe değişebilir. Arama uygulamasının, sağlanan Azure Cosmos DB bitiş noktasıyla aynı Azure bölgesinde bulunmasını sağlayarak mümkün olan en düşük gecikme süresi elde edilir. Kullanılabilir bölgelerin listesi için [Azure Bölgeleri'ne](https://azure.microsoft.com/regions/#services)bakın.

    ![Azure Cosmos DB bağlantı ilkesinin çizimi](./media/performance-tips/same-region.png)

## <a name="sdk-usage"></a>SDK Kullanımı
* **En son SDK'yı yükleyin**

    Azure Cosmos DB SDK'lar en iyi performansı sağlamak için sürekli olarak geliştirilmektedir. En son SDK'yı belirlemek ve iyileştirmeleri gözden geçirmek için [Azure Cosmos DB SDK](sql-api-sdk-async-java.md) sayfalarına bakın.

* **Uygulamanızın kullanım ömrü boyunca tek ton Azure Cosmos DB istemcisi kullanın**

    Her AsyncDocumentClient örneği iş parçacığı için güvenlidir ve verimli bağlantı yönetimi ve adres önbelleğe alma gerçekleştirir. AsyncDocumentClient tarafından verimli bağlantı yönetimi ve daha iyi performans sağlamak için, uygulamanın ömrü boyunca AppDomain başına AsyncDocumentClient tek bir örnek kullanılması önerilir.

   <a id="max-connection"></a>

* **Tuning ConnectionPolicy**

    Varsayılan olarak, Async Java SDK kullanırken Doğrudan mod Cosmos DB istekleri TCP üzerinden yapılır. Dahili olarak SDK, ağ kaynaklarını dinamik olarak yönetmek ve en iyi performansı elde etmek için özel bir Doğrudan mod mimarisi kullanır.

    Async Java SDK'da, Doğrudan mod, çoğu iş yüküyle veritabanı performansını artırmak için en iyi seçimdir. 

    * ***Doğrudan moduna Genel Bakış***

        ![Doğrudan mod mimarisinin çizimi](./media/performance-tips-async-java/rntbdtransportclient.png)

        Doğrudan modda kullanılan istemci tarafı mimarisi, öngörülebilir ağ kullanımı ve Azure Cosmos DB yinelemelerine çok yönlü erişim sağlar. Yukarıdaki diyagram, Doğrudan mod isteklerini Cosmos DB arka ucundaki yinelemelere nasıl yönlendirenleri gösterir. Doğrudan mod mimarisi, istemci tarafında DB yinelemebaşına en fazla 10 **Kanal** ayırır. Kanal, 30 istek derinolan bir istek arabelleği nden önce gelen bir TCP bağlantısıdır. Bir yinelemeye ait kanallar, yinelemenin **Hizmet Bitiş Noktası**tarafından gerektiği gibi dinamik olarak ayrılır. Kullanıcı Doğrudan modda bir istek verdiğinde, **TransportClient** isteği bölüm anahtarını temel alınca uygun hizmet bitiş noktasına yönlendirir. **İstek Sırası** arabellekleri, Hizmet Bitiş Noktasından önce isteklerini yerine

    * ***ConnectionPolicy Doğrudan mod için Yapılandırma seçenekleri***

        İlk adım olarak, aşağıdaki önerilen yapılandırma ayarlarını kullanın. Bu konuyla ilgili sorunlarla karşılaştıysanız lütfen [Azure Cosmos DB ekibiyle](mailto:CosmosDBPerformanceSupport@service.microsoft.com) iletişime geçin.

        Azure Cosmos DB'yi başvuru veritabanı olarak kullanıyorsanız (diğer bir süre, veritabanı birçok nokta okuma işlemi ve birkaç yazma işlemi için kullanılır), *idleEndpointTimeout'u* 0 olarak ayarlamak kabul edilebilir (yani zaman aşımı yok).


        | Yapılandırma seçeneği       | Varsayılan    |
        | :------------------:       | :-----:    |
        | bufferPageSize             | 8192       |
        | Connectiontimeout          | "PT1M"     |
        | idleChannelTimeout         | "PT0S"     |
        | idleEndpointTimeout        | "PT1M10S"  |
        | maxBufferKapasite          | 8388608    |
        | maxChannelsPerEndpoint     | 10         |
        | maxRequestsPerChannel      | 30         |
        | receiveHangDetectionTime   | "PT1M5S"   |
        | requestExpiryInterval      | "PT5S"     |
        | requestTimeout             | "PT1M"     |
        | requestTimerResolution     | "PT0.5S"   |
        | sendHangDetectionTime      | "PT10S"    |
        | kapatmaTimeout            | "PT15S"    |

    * ***Doğrudan mod için programlama ipuçları***

        Azure Cosmos DB [Async Java SDK Sorun Giderme](troubleshoot-java-async-sdk.md) makalesini, Async Java SDK sorunlarını çözmek için temel olarak gözden geçirin.

        Doğrudan mod kullanırken bazı önemli programlama ipuçları:

        + **Verimli TCP veri aktarımı için uygulamanızda çoklu iş parçacığı kullanın** - Bir istekte bulunduktan sonra, uygulamanız başka bir iş parçacığı üzerinde veri almak için abone olmalıdır. Bunu yapmamak istenmeyen "yarı çift yönlü" işlemi zorlar ve sonraki istekler önceki isteğin yanıtı için bekleyerek engellenir.

        + **Özel bir iş parçacığı üzerinde bilgi işlem yoğun iş yükleri yürütmek** - Önceki uç benzer nedenlerle, karmaşık veri işleme gibi işlemler en iyi ayrı bir iş parçacığı yerleştirilir. Başka bir veri deposundan veri çeken bir istek (örneğin iş parçacığı Azure Cosmos DB ve Spark veri depolarını aynı anda kullanıyorsa) artan gecikme gecikmesi yaşayabilir ve diğer veri deposundan yanıt bekleyen ek bir iş parçacığı oluşturmak önerilir.

            + Async Java SDK temel ağ IO Netty tarafından yönetilir, [Netty IO konuları engellemek kodlama desenleri önlemek için](troubleshoot-java-async-sdk.md#invalid-coding-pattern-blocking-netty-io-thread)bu ipuçlarını görmek.

        + **Veri modelleme** - Azure Cosmos DB SLA belge boyutunun 1KB'den küçük olduğunu varsayar. Veri modelinizi ve programlamanızı daha küçük belge boyutu lehine optimize etmek genellikle gecikme nin azalmasına neden olur. 1KB'den büyük dokümanların depolanması ve alınması gerekiyorsa, önerilen yaklaşım belgelerin Azure Blob Depolama'daki verilere bağlanmasıiçindir.


* **Bölümlenmiş koleksiyonlar için paralel sorguları ala**

    Azure Cosmos DB SQL Async Java SDK, bölümlenmiş bir koleksiyonu paralel olarak sorgulamanızı sağlayan paralel sorguları destekler. Daha fazla bilgi için, SDK'larla çalışmayla ilgili [kod örneklerine](https://github.com/Azure/azure-cosmosdb-java/tree/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples) bakın. Paralel sorgular, sorgu gecikmesini ve seri karşılıkları üzerinde iş verisini geliştirmek için tasarlanmıştır.

    * ***Ayar setiMaxDegreeOfParallelism\:***
    
        Paralel sorgular, birden çok bölümü paralel olarak sorgulayarak çalışır. Ancak, tek tek bölümlenmiş koleksiyondaki veriler sorguyla ilgili olarak seri olarak getirilir. Yani, diğer tüm sistem koşulları nın aynı kalması koşuluyla, en çok performans gösteren sorguyu elde etme olasılığı en yüksek olan bölüm sayısını ayarlamak için setMaxDegreeOfParallelism'i kullanın. Bölüm sayısını bilmiyorsanız, yüksek bir sayı ayarlamak için setMaxDegreeOfParallelism'i kullanabilirsiniz ve sistem en az (bölüm sayısı, kullanıcı tarafından sağlanan giriş) en yüksek paralellik derecesi olarak seçer.

        Veriler sorguyla ilgili olarak tüm bölümlerarasında eşit olarak dağıtılırsa paralel sorguların en iyi yararları ürettiğini unutmayın. Bölümlenmiş koleksiyon, sorgu tarafından döndürülen verilerin tamamının veya çoğunluğunun birkaç bölüme (en kötü durumda bir bölüm) yoğunlaştığı şekilde bölümlere ayrılmıştır, sorgunun performansı bu bölümler tarafından darboğaza sürülür.

    * ***Ayarlama setiMaxBufferedItemCount\:***
    
        Paralel sorgu, geçerli sonuç grubu istemci tarafından işlenirken sonuçları önceden almak üzere tasarlanmıştır. Ön alma, bir sorgunun genel gecikme sayılma iyileştirilmesine yardımcı olur. setMaxBufferedItemCount önceden getirilen sonuç sayısını sınırlar. AyarMaxBufferedItemCount'ı döndürülen beklenen sonuç sayısına (veya daha yüksek bir sayıya) ayarlamak, sorgunun ön almadan maksimum fayda elde etmesini sağlar.

        Ön alma, MaxDegreeOfParallelism ne olursa olsun aynı şekilde çalışır ve tüm bölümlerden veri için tek bir arabellek vardır.

* **getRetryAfterInMilliseconds aralıklarında geri tepme uygulayın**

    Performans testi sırasında, küçük bir istek oranı azaltılına kadar yükü artırmalısınız. Daraltılırsa, istemci uygulaması sunucu tarafından belirtilen yeniden deneme aralığı için geri çekilmelidir. Geri tepmeye saygı duymak, yeniden denemeler arasında en az miktarda beklemenizi sağlar.

* **İstemci-iş yükünüzü ölçeklendirin**

    Yüksek iş verme düzeylerinde (>50.000 RU/s) test ediyorsanız, istemci uygulaması işlemci veya ağ kullanımı nda dışarı atlama makine nedeniyle darboğaz haline gelebilir. Bu noktaya ulaşırsanız, istemci uygulamalarınızı birden çok sunucuda ölçekleyerek Azure Cosmos DB hesabını daha da zorlamaya devam edebilirsiniz.

* **Ad tabanlı adresleme kullanma**

    Bağlantıların, bağlantıyı oluşturmak için kullanılan `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`tüm kaynakların Kaynak\_Id'lerini almaktan `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` kaçınmak için biçime sahip SelfLinks (self) yerine biçimine sahip olduğu ad tabanlı adresleme'yi kullanın. Ayrıca, bu kaynaklar yeniden oluşturuldukça (büyük olasılıkla aynı ada sahip), önbelleğe almayardımcı olmayabilir.

   <a id="tune-page-size"></a>

* **Daha iyi performans için sorgular/okuma akışları için sayfa boyutunu ayarlama**

    Okuma akışı işlevini kullanarak (örneğin, Belgeleri okuma) veya bir SQL sorgusu verirken belgelerin toplu okumasını gerçekleştirirken, sonuç kümesi çok büyükse sonuçlar parçalı bir şekilde döndürülür. Varsayılan olarak, sonuçlar 100 öğe veya 1 MB'den oluşan parçalar halinde döndürülür (önce hangi sınıra vurulursa).

    Tüm geçerli sonuçları almak için gereken ağ turu gezileri sayısını azaltmak için, [x-ms-max madde-count](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) istek üstbilgisini kullanarak sayfa boyutunu 1000'e kadar artırabilirsiniz. Yalnızca birkaç sonuç görüntülemeniz gereken durumlarda, örneğin, kullanıcı arabirimi veya uygulama API'niz bir seferde yalnızca 10 sonuç döndürürse, okuma lar ve sorgular için tüketilen iş buzunazaltılması için sayfa boyutunu 10'a düşürebilirsiniz.

    Sayfa boyutunu setMaxItemCount yöntemini kullanarak da ayarlayabilirsiniz.

* **Uygun Zamanlayıcı kullanın (Olay döngüsü IO Netty ipliklerini çalmaktan kaçının)**

    Async Java SDK, engelleyici olmayan IO için [netty](https://netty.io/) kullanır. SDK, IO işlemlerini yürütmek için sabit sayıda IO netty event loop iş parçacığı (makinenizin sahip olduğu birçok CPU çekirdeği gibi) kullanır. API tarafından döndürülen Gözlemlenebilir, sonucu paylaşılan IO olay döngüsü netty iş parçacığından birine yayır. Bu nedenle paylaşılan IO olay döngü netty konuları engellemek için önemlidir. IO olay döngü netty iş parçacığı cpu yoğun çalışma yapmak veya engelleme işlemi kilitlenme neden olabilir veya önemli ölçüde SDK iş parçacığı azaltmak.

    Örneğin, aşağıdaki kod olay döngüsü IO netty iş parçacığı üzerinde bir işlemci yoğun çalışma yürütür:

    ```java
    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribe(
      resourceResponse -> {
        //this is executed on eventloop IO netty thread.
        //the eventloop thread is shared and is meant to return back quickly.
        //
        // DON'T do this on eventloop IO netty thread.
        veryCpuIntensiveWork();
      });
    ```

    Sonuç alındıktan sonra eğer sonuç üzerinde CPU yoğun çalışma yapmak istiyorsanız olay döngüsü IO netty iş parçacığı üzerinde bunu yapmaktan kaçınmalısınız. Bunun yerine, çalışmanızı çalıştırmak için kendi iş parçacığısağlamak için kendi Zamanlayıcı sağlayabilir.

    ```java
    import rx.schedulers;

    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribeOn(Schedulers.computation())
    subscribe(
      resourceResponse -> {
        // this is executed on threads provided by Scheduler.computation()
        // Schedulers.computation() should be used only when:
        //   1. The work is cpu intensive 
        //   2. You are not doing blocking IO, thread sleep, etc. in this thread against other resources.
        veryCpuIntensiveWork();
      });
    ```

    Çalışmanızın türüne bağlı olarak, çalışmanız için uygun mevcut RxJava Zamanlayıcısı kullanmalısınız. Burada [``Schedulers``](http://reactivex.io/RxJava/1.x/javadoc/rx/schedulers/Schedulers.html)okuyun .

    Daha fazla bilgi için Lütfen Async Java SDK için [GitHub sayfasına](https://github.com/Azure/azure-cosmosdb-java) bakın.

* **Netty'nin günlüğe kaydetmeyi devre dışı**

    Netty kitaplık günlüğü gevezedir ve ek CPU maliyetlerini önlemek için kapatılması gerekir (yapılandırmadaki işareti bastırmayeterli olmayabilir). Hata ayıklama modunda değilseniz, Netty'nin günlüğe kaydetmeyi tamamen devre dışı edin. Yani netty tarafından tahakkuk eden ``org.apache.log4j.Category.callAppenders()`` ek CPU maliyetlerini kaldırmak için log4j kullanıyorsanız codebase için aşağıdaki satırı ekleyin:

    ```java
    org.apache.log4j.Logger.getLogger("io.netty").setLevel(org.apache.log4j.Level.OFF);
    ```

 * **İşletim Sistemi Açık dosyalar Kaynak Sınırı**
 
    Bazı Linux sistemleri (Red Hat gibi) açık dosya sayısı ve böylece bağlantıların toplam sayısı bir üst sınırı vardır. Geçerli sınırları görüntülemek için aşağıdakileri çalıştırın:

    ```bash
    ulimit -a
    ```

    Açık dosya sayısının (nofile) yapılandırılan bağlantı havuzu boyutunuz ve işletim sistemi tarafından diğer açık dosyalar için yeterli alana sahip olacak kadar büyük olması gerekir. Daha büyük bir bağlantı havuzu boyutuna izin verecek şekilde değiştirilebilir.

    limits.conf dosyasını açın:

    ```bash
    vim /etc/security/limits.conf
    ```
    
    Aşağıdaki satırları ekleme/değiştirme:

    ```
    * - nofile 100000
    ```

* **Netty için yerel TLS/SSL uygulamasını kullanma**

    Netty, daha iyi performans elde etmek için OpenSSL'yi doğrudan TLS uygulama yığını için kullanabilir. Bu yapılandırma netty yokluğunda Java'nın varsayılan TLS uygulamasına geri düşecek.

    Ubuntu üzerinde:
    ```bash
    sudo apt-get install openssl
    sudo apt-get install libapr1
    ```

    ve proje maven bağımlılıklarınıza aşağıdaki bağımlılığı ekleyin:
    ```xml
    <dependency>
      <groupId>io.netty</groupId>
      <artifactId>netty-tcnative</artifactId>
      <version>2.0.20.Final</version>
      <classifier>linux-x86_64</classifier>
    </dependency>
    ```

Diğer platformlar için (Red Hat, Windows, Mac, vb.) bu talimatlara bakınhttps://netty.io/wiki/forked-tomcat-native.html

## <a name="indexing-policy"></a>Dizin Oluşturma İlkesi
 
* **Daha hızlı yazma işlemleri için, kullanılmayan yolları dizin oluşturmanın dışında bırakma**

    Azure Cosmos DB'nin dizin oluşturma ilkesi, Dizin Oluşturma Yollarından (setIncludedPaths ve setExcludedPaths) yararlanarak hangi belge yollarının eklenecek veya dizin oluşturmadışında belirtebilirsiniz. Dizin oluşturma yollarının kullanımı, dizin oluşturma maliyetleri dizine eklenmiş benzersiz yolların sayısıyla doğrudan ilişkili olduğundan, sorgu desenleri önceden bilindiği senaryolar için gelişmiş yazma performansı ve daha düşük dizin depolama sunabilir. Örneğin, aşağıdaki kod, belgelerin tüm bölümünün (alt ağaç olarak da bilinir) "*" joker kartını kullanarak dizin oluşturmayı nasıl dışlayacağıgöster.

    ```Java
    Index numberIndex = Index.Range(DataType.Number);
    numberIndex.set("precision", -1);
    indexes.add(numberIndex);
    includedPath.setIndexes(indexes);
    includedPaths.add(includedPath);
    indexingPolicy.setIncludedPaths(includedPaths);
    collectionDefinition.setIndexingPolicy(indexingPolicy);
    ```

    Daha fazla bilgi için [Azure Cosmos DB dizin oluşturma ilkelerine](indexing-policies.md)bakın.

## <a name="throughput"></a>Aktarım hızı
<a id="measure-rus"></a>

* **Daha düşük istek birimleri/ikinci kullanım için ölçün ve ayarlayın**

    Azure Cosmos DB, UDF'lerle ilişkisel ve hiyerarşik sorgular, depolanmış yordamlar ve tetikleyiciler de dahil olmak üzere zengin bir veritabanı işlemleri kümesi sunar ve bunların tümü bir veritabanı koleksiyonundaki belgeler üzerinde çalışma sağlar. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, işlemi tamamlamak için gereken CPU, GÇ ve belleğe göre değişiklik gösterir. Donanım kaynaklarını düşünmek ve yönetmek yerine, bir istek birimini (RU) çeşitli veritabanı işlemlerini gerçekleştirmek ve bir uygulama isteğine hizmet etmek için gereken kaynaklar için tek bir ölçü olarak düşünebilirsiniz.

    Elde etme, her kapsayıcı için ayarlanan [istek birimlerinin](request-units.md) sayısına göre sağlanır. İstek birim tüketimi saniye başına bir oran olarak değerlendirilir. Konteynerleri için verilen istek birim oranını aşan uygulamalar, konteyner için verilen seviyenin altına düşene kadar sınırlıdır. Uygulamanız daha yüksek bir iş düzeyi gerektiriyorsa, ek istek birimleri sağlayarak iş inizi artırabilirsiniz.

    Sorgunun karmaşıklığı, bir işlem için kaç istek biriminin tüketilmesini etkiler. Yüklemlerin sayısı, yüklemlerin yapısı, UDF sayısı ve kaynak veri kümesinin boyutu sorgu işlemlerinin maliyetini etkiler.

    Herhangi bir işlemin genel yükünü ölçmek için (oluşturma, güncelleme veya silme), bu işlemler tarafından tüketilen istek birimlerinin sayısını ölçmek için [x-ms-request-charge](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) üstbilgisini inceleyin. Ayrıca ResourceResponse T> veya FeedResponse\<\<T>'daki eşdeğer RequestCharge özelliğine de bakabilirsiniz.

    ```Java
    ResourceResponse<Document> response = asyncClient.createDocument(collectionLink, documentDefinition, null,
                                                     false).toBlocking.single();
    response.getRequestCharge();
    ```

    Bu üstbilgide döndürülen istek ücreti, sağlanan iş ınızın bir kısmıdır. Örneğin, 2000 RU/s'niz varsa ve önceki sorgu 1000 1KB-belgedöndürürse, işlemin maliyeti 1000'dir. Bu nedenle, bir saniye içinde sunucu, sonraki istekleri sınırlamadan önce yalnızca iki tür isteği onurlandırır. Daha fazla bilgi [için, İstek birimleri](request-units.md) ve istek birimi hesap [makinesine](https://www.documentdb.com/capacityplanner)bakın.

<a id="429"></a>
* **Oran sınırlama/istek oranını çok büyük işleme**

    İstemci bir hesap için ayrılmış iş düzeyini aşmaya çalıştığında, sunucuda performans bozulması ve ayrılmış düzeyin ötesinde iş gücü kullanımı olmaz. Sunucu, RequestRateTooLarge (HTTP durum kodu 429) ile isteği önceden sonla tır ve kullanıcının isteği yeniden denemeden önce beklemesi gereken süreyi milisaniye cinsinden belirten [x-ms-retry-after-ms](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) üstbilgisini döndürür.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK'ların tümü bu yanıtı dolaylı olarak yakalar, sunucu tarafından belirtilen yeniden deneme üstbilgine saygı duyar ve isteği yeniden deneyin. Hesabınıza aynı anda birden çok istemci erişilmediği sürece, bir sonraki yeniden deneme başarılı olur.

    İstem oranının üzerinde sürekli olarak çalışan birden fazla istemciniz varsa, istemci tarafından dahili olarak şu anda 9 olarak ayarlanan varsayılan yeniden deneme sayısı yeterli olmayabilir; bu durumda, istemci uygulamaya durum kodu 429 içeren bir DocumentClientException atar. Varsayılan yeniden deneme sayısı ConnectionPolicy örneğinde setRetryOptions kullanılarak değiştirilebilir. Varsayılan olarak, durum kodu 429'a sahip DocumentClientException, istek isteği oranının üzerinde çalışmaya devam ederse, 30 saniyelik bir toplu bekleme süresinden sonra döndürülür. Bu, geçerli yeniden deneme sayısı en yüksek yeniden deneme sayısından daha az olsa bile, varsayılan 9 veya kullanıcı tanımlı değer olsun oluşur.

    Otomatik yeniden deneme davranışı çoğu uygulama için esnekliği ve kullanılabilirliği artırmaya yardımcı olsa da, özellikle gecikme süresi ölçerken performans kriterleri yaparken ters gidebilir. Deneme sunucu gaza çarparsa ve istemci SDK'nın sessizce yeniden denemesine neden olursa istemcitarafından gözlenen gecikme söker. Performans denemeleri sırasında gecikme artışlarını önlemek için, her işlem tarafından döndürülen ücreti ölçün ve isteklerin ayrılmış istek oranının altında çalıştığından emin olun. Daha fazla bilgi için [İstek birimlerine](request-units.md)bakın.

* **Daha yüksek aktarım hızı için daha küçük dosyalar tasarlayın**

    Belirli bir işlemin istek ücreti (istek işleme maliyeti) belgenin boyutuyla doğrudan ilişkilidir. Büyük belgelerdeki işlemler, küçük belgelere yönelik işlemlerden daha pahalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı ölçeklendirme ve yüksek performans için tasarlama hakkında daha fazla bilgi edinmek için [Azure Cosmos DB'de Bölümleme ve ölçeklendirme](partition-data.md)bölümüne bakın.
