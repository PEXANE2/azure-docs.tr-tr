---
title: Java için Azure Cosmos DB performans ipuçları
description: Azure Cosmos veritabanı performansını artırmak için istemci yapılandırma seçeneklerini öğrenin
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.openlocfilehash: a20b7d91a927d48a14812110ca714491cd726071
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548774"
---
# <a name="performance-tips-for-azure-cosmos-db-and-java"></a>Azure Cosmos DB ve Java için performans ipuçları

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB, garantili gecikme sonu ve iş ortası ile sorunsuz ölçeklendirilebilen hızlı ve esnek dağıtılmış bir veritabanıdır. Veritabanınızı Azure Cosmos DB ile ölçeklendirmek için büyük mimari değişiklikleri yapmanız veya karmaşık kod yazmanız gerekmez. Yukarı ve aşağı ölçekleme tek bir API araması yapmak kadar kolaydır. Daha fazla bilgi edinmek [için, kapsayıcı iş verisini nasıl sağtayabildiğini](how-to-provision-container-throughput.md) veya [veritabanı iş verisini nasıl sağlarınızı](how-to-provision-database-throughput.md)görün. Ancak, Azure Cosmos DB ağ aramaları üzerinden erişilen çünkü [SQL Java SDK](documentdb-sdk-java.md)kullanırken en yüksek performans elde etmek için yapabileceğiniz istemci tarafı optimizasyonları vardır.

"Veritabanı performansımı nasıl artırabilirim?" diye soruyorsanız. aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ
<a id="direct-connection"></a>

1. **Bağlantı modu: Direct'i kullanHttps**

    İstemcinin Azure Cosmos DB'ye nasıl bağlandığının performans üzerinde, özellikle de gözlemlenen istemci tarafı gecikme süresi olarak önemli etkileri olur. Bağlantı [Politikası](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy) - [ConnectionMode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode)- yapılandırmak için kullanılabilir bir anahtar yapılandırma ayarı vardır.  Kullanılabilir iki Bağlantı Modu şunlardır:

   1. [Ağ geçidi (varsayılan)](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode)
   2. [DoğrudanHttps](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionmode)

      Ağ geçidi modu tüm SDK platformlarında desteklenir ve yapılandırılan varsayılandır.  Uygulamanız sıkı güvenlik duvarı kısıtlamaları olan bir şirket ağında çalışıyorsa, standart HTTPS bağlantı noktasını ve tek bir bitiş noktasını kullandığından Ağ Geçidi en iyi seçimdir. Ancak performans dengelemesi, Ağ Geçidi modunun azure Cosmos DB'ye her veri okunduveya yazıldığında ek bir ağ atlama içer. Bu nedenle DirectHttps modu, daha az ağ atlaması sayesinde daha iyi performans sunar. 

      Java SDK, HTTPS'yi aktarım protokolü olarak kullanır. HTTPS, ilk kimlik doğrulama ve trafiği şifrelemek için TLS kullanır. Java SDK kullanırken, yalnızca HTTPS bağlantı noktası 443 açık olması gerekir. 

      ConnectionMode, ConnectionPolicy parametresi ile DocumentClient örneğinin yapımı sırasında yapılandırılır. 

      ```Java
      public ConnectionPolicy getConnectionPolicy() {
        ConnectionPolicy policy = new ConnectionPolicy();
        policy.setConnectionMode(ConnectionMode.DirectHttps);
        policy.setMaxPoolSize(1000);
        return policy;
      }
        
      ConnectionPolicy connectionPolicy = new ConnectionPolicy();
      DocumentClient client = new DocumentClient(HOST, MASTER_KEY, connectionPolicy, null);
      ```

      ![Azure Cosmos DB bağlantı ilkesinin çizimi](./media/performance-tips-java/connection-policy.png)

   <a id="same-region"></a>
2. **Performans için istemcileri aynı Azure bölgesinde birlikte bulundurun**

    Mümkün olduğunda, Azure Cosmos DB çağıran uygulamaları Azure Cosmos veritabanıyla aynı bölgeye yerleştirin. Yaklaşık bir karşılaştırma için, aynı bölgedeki Azure Cosmos DB çağrıları 1-2 ms içinde tamamlanır, ancak ABD'nin Batı ve Doğu kıyıları arasındaki gecikme 50 >. Bu gecikme gecikmesi, istemciden Azure veri merkezi sınırına geçerken isteğin aldığı rotaya bağlı olarak istekten isteğe değişebilir. Arama uygulamasının, sağlanan Azure Cosmos DB bitiş noktasıyla aynı Azure bölgesinde bulunmasını sağlayarak mümkün olan en düşük gecikme süresi elde edilir. Kullanılabilir bölgelerin listesi için [Azure Bölgeleri'ne](https://azure.microsoft.com/regions/#services)bakın.

    ![Azure Cosmos DB bağlantı ilkesinin çizimi](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>SDK Kullanımı
1. **En son SDK'yı yükleyin**

    Azure Cosmos DB SDK'lar en iyi performansı sağlamak için sürekli olarak geliştirilmektedir. En son SDK'yı belirlemek ve iyileştirmeleri gözden geçirmek için [Azure Cosmos DB SDK](documentdb-sdk-java.md) sayfalarına bakın.
2. **Uygulamanızın kullanım ömrü boyunca tek ton Azure Cosmos DB istemcisi kullanın**

    Her [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient) örneği iş parçacığı için güvenlidir ve Doğrudan Mod'da çalışırken verimli bağlantı yönetimi ve adres önbelleğe alma gerçekleştirir. DocumentClient tarafından verimli bağlantı yönetimi ve daha iyi performans sağlamak için, uygulamanın ömrü boyunca AppDomain başına DocumentClient'ın tek bir örneğini kullanmanız önerilir.

   <a id="max-connection"></a>
3. **Ağ Geçidi modunu kullanırken ana bilgisayar başına MaxPoolSize'ı artırın**

    Azure Cosmos DB istekleri Ağ Geçidi modunu kullanırken HTTPS/REST üzerinden yapılır ve ana bilgisayar adı veya IP adresi başına varsayılan bağlantı sınırına tabi tutulur. İstemci kitaplığı Azure Cosmos DB'ye birden çok eşzamanlı bağlantı kullanabilmek için MaxPoolSize'ı daha yüksek bir değere (200-1000) ayarlamanız gerekebilir. Java SDK'da [ConnectionPolicy.getMaxPoolSize](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.getmaxpoolsize) için varsayılan değer 100'dür. Değeri değiştirmek için [setMaxPoolSize'ı]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.setmaxpoolsize) kullanın.

4. **Bölümlenmiş koleksiyonlar için paralel sorguları ala**

    Azure Cosmos DB SQL Java SDK sürüm 1.9.0 ve üzeri paralel sorguları destekler, bu da bölümlenmiş bir koleksiyonu paralel olarak sorgulayabilmenizi sağlar. Daha fazla bilgi için, SDK'larla çalışmayla ilgili [kod örneklerine](https://github.com/Azure/azure-documentdb-java/tree/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples) bakın. Paralel sorgular, sorgu gecikmesini ve seri karşılıkları üzerinde iş verisini geliştirmek için tasarlanmıştır.

    (a) ***Ayarlama kümesiMaxDegreeOfParallelism\: *** Paralel sorgular paralel olarak birden fazla bölüm sorgulayarak çalışır. Ancak, tek tek bölümlenmiş koleksiyondaki veriler sorguyla ilgili olarak seri olarak getirilir. Yani, diğer tüm sistem koşulları nın aynı kalması koşuluyla, en çok performans gösteren sorguyu elde etme olasılığı en yüksek olan bölüm sayısını ayarlamak için [setMaxDegreeOfParallelism'i](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptions.setmaxdegreeofparallelism) kullanın. Bölüm sayısını bilmiyorsanız, yüksek bir sayı ayarlamak için setMaxDegreeOfParallelism'i kullanabilirsiniz ve sistem en az (bölüm sayısı, kullanıcı tarafından sağlanan giriş) en yüksek paralellik derecesi olarak seçer. 

    Veriler sorguyla ilgili olarak tüm bölümlerarasında eşit olarak dağıtılırsa paralel sorguların en iyi yararları ürettiğini unutmayın. Bölümlenmiş koleksiyon, sorgu tarafından döndürülen verilerin tamamının veya çoğunluğunun birkaç bölüme (en kötü durumda bir bölüm) yoğunlaştığı şekilde bölümlere ayrılmıştır, sorgunun performansı bu bölümler tarafından darboğaza sürülür.

    (b) ***Ayarlama kümesiMaxBufferedItemCount\: *** Paralel sorgu, geçerli sonuç grubu istemci tarafından işlenirken sonuçları önceden almak üzere tasarlanmıştır. Ön alma, bir sorgunun genel gecikme sayılma iyileştirilmesine yardımcı olur. setMaxBufferedItemCount önceden getirilen sonuç sayısını sınırlar. [MaxBufferedItemCount'ı](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptions.setmaxbuffereditemcount) beklenen sonuç sayısına (veya daha yüksek bir sayıya) ayarlayarak, sorgunun ön almadan maksimum fayda elde etmesini sağlar.

    Ön alma, MaxDegreeOfParallelism ne olursa olsun aynı şekilde çalışır ve tüm bölümlerden veri için tek bir arabellek vardır.  

5. **getRetryAfterInMilliseconds aralıklarında geri tepme uygulayın**

    Performans testi sırasında, küçük bir istek oranı azaltılına kadar yükü artırmalısınız. Daraltılırsa, istemci uygulaması sunucu tarafından belirtilen yeniden deneme aralığı için azaltma gerekir. Geri tepmeye saygı duymak, yeniden denemeler arasında en az miktarda beklemenizi sağlar. Yeniden deneme ilkesi [desteği, Java SDK'nın](documentdb-sdk-java.md)Sürüm 1.8.0 ve üstü sürümünde yer aldı. Daha fazla bilgi için [getRetryAfterInMilliseconds](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclientexception.getretryafterinmilliseconds)'a bakın.

6. **İstemci-iş yükünüzü ölçeklendirin**

    Yüksek iş verme düzeylerinde (>50.000 RU/s) test ediyorsanız, istemci uygulaması işlemci veya ağ kullanımı nda dışarı atlama makine nedeniyle darboğaz haline gelebilir. Bu noktaya ulaşırsanız, istemci uygulamalarınızı birden çok sunucuda ölçekleyerek Azure Cosmos DB hesabını daha da zorlamaya devam edebilirsiniz.

7. **Ad tabanlı adresleme kullanma**

    Bağlantıların, bağlantıyı oluşturmak için kullanılan `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`tüm kaynakların Kaynak\_Id'lerini almaktan `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` kaçınmak için biçime sahip SelfLinks (self) yerine biçimine sahip olduğu ad tabanlı adresleme'yi kullanın. Ayrıca, bu kaynaklar yeniden oluşturuldukça (büyük olasılıkla aynı ada sahip), bunları önbelleğe almak yardımcı olmayabilir.

   <a id="tune-page-size"></a>
8. **Daha iyi performans için sorgular/okuma akışları için sayfa boyutunu ayarlama**

    Okuma akışı işlevini kullanarak (örneğin, [Belgeleri okuma)](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclient.readdocuments)veya bir SQL sorgusu verirken, sonuç kümesi çok büyükse, sonuçlar parçalı bir şekilde döndürülür. Varsayılan olarak, sonuçlar 100 öğe veya 1 MB'den oluşan parçalar halinde döndürülür (önce hangi sınıra vurulursa).

    Tüm geçerli sonuçları almak için gereken ağ turu gezileri sayısını azaltmak için, [x-ms-max madde-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) istek üstbilgisini kullanarak sayfa boyutunu 1000'e kadar artırabilirsiniz. Yalnızca birkaç sonuç görüntülemeniz gereken durumlarda, örneğin, kullanıcı arabirimi veya uygulama API'niz bir seferde yalnızca 10 sonuç döndürürse, okuma lar ve sorgular için tüketilen iş buzunazaltılması için sayfa boyutunu 10'a düşürebilirsiniz.

    Sayfa boyutunu [setPageSize yöntemini](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedoptionsbase.setpagesize)kullanarak da ayarlayabilirsiniz.

## <a name="indexing-policy"></a>Dizin Oluşturma İlkesi
 
1. **Daha hızlı yazma işlemleri için, kullanılmayan yolları dizin oluşturmanın dışında bırakma**

    Azure Cosmos DB'nin dizin oluşturma ilkesi, Dizin Oluşturma Yollarından[(setIncludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.indexingpolicy.setincludedpaths) ve [setExcludedPaths)](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.indexingpolicy.setexcludedpaths)yararlanarak hangi belge yollarının eklenecek veya dizin oluşturmadışında belirtebilirsiniz. Dizin oluşturma yollarının kullanımı, dizin oluşturma maliyetleri dizine eklenmiş benzersiz yolların sayısıyla doğrudan ilişkili olduğundan, sorgu desenleri önceden bilindiği senaryolar için gelişmiş yazma performansı ve daha düşük dizin depolama sunabilir.  Örneğin, aşağıdaki kod belgelerin tüm bölümünün nasıl dışlanır (diğer bir örneğidir. bir alt ağaç) "*" joker kullanarak dizinlenme.

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

1. **Daha düşük istek birimleri/ikinci kullanım için ölçün ve ayarlayın**

    Azure Cosmos DB, UDF'lerle ilişkisel ve hiyerarşik sorgular, depolanmış yordamlar ve tetikleyiciler de dahil olmak üzere zengin bir veritabanı işlemleri kümesi sunar ve bunların tümü bir veritabanı koleksiyonundaki belgeler üzerinde çalışma sağlar. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, işlemi tamamlamak için gereken CPU, GÇ ve belleğe göre değişiklik gösterir. Donanım kaynaklarını düşünmek ve yönetmek yerine, bir istek birimini (RU) çeşitli veritabanı işlemlerini gerçekleştirmek ve bir uygulama isteğine hizmet etmek için gereken kaynaklar için tek bir ölçü olarak düşünebilirsiniz.

    Elde etme, her kapsayıcı için ayarlanan [istek birimlerinin](request-units.md) sayısına göre sağlanır. İstek birim tüketimi saniye başına bir oran olarak değerlendirilir. Konteynerleri için verilen istek birim oranını aşan uygulamalar, konteyner için verilen seviyenin altına düşene kadar sınırlıdır. Uygulamanız daha yüksek bir iş düzeyi gerektiriyorsa, ek istek birimleri sağlayarak iş inizi artırabilirsiniz. 

    Sorgunun karmaşıklığı, bir işlem için kaç istek biriminin tüketilmesini etkiler. Yüklemlerin sayısı, yüklemlerin yapısı, UDF sayısı ve kaynak veri kümesinin boyutu sorgu işlemlerinin maliyetini etkiler.

    Herhangi bir işlemin genel yükünü ölçmek için (oluşturma, güncelleme veya silme), [x-ms-request-charge](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üstbilgisini (veya bu işlemler tarafından tüketilen istek birimlerinin sayısını ölçmek için [\<ResourceResponse T>](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.resourceresponse) veya [FeedResponse\<T>'daki](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.feedresponse) eşdeğer RequestCharge özelliğini inceleyin.

    ```Java
    ResourceResponse<Document> response = client.createDocument(collectionLink, documentDefinition, null, false);

    response.getRequestCharge();
    ```             

    Bu üstbilgide döndürülen istek ücreti, sağlanan iş ınızın bir kısmıdır. Örneğin, 2000 RU/s'niz varsa ve önceki sorgu 1000 1KB-belgedöndürürse, işlemin maliyeti 1000'dir. Bu nedenle, bir saniye içinde sunucu, sonraki istekleri sınırlamadan önce yalnızca iki tür isteği onurlandırır. Daha fazla bilgi [için, İstek birimleri](request-units.md) ve istek birimi hesap [makinesine](https://www.documentdb.com/capacityplanner)bakın.
   <a id="429"></a>
1. **Oran sınırlama/istek oranını çok büyük işleme**

    İstemci bir hesap için ayrılmış iş düzeyini aşmaya çalıştığında, sunucuda performans bozulması ve ayrılmış düzeyin ötesinde iş gücü kullanımı olmaz. Sunucu, RequestRateTooLarge (HTTP durum kodu 429) ile isteği önceden sonla tır ve kullanıcının isteği yeniden denemeden önce beklemesi gereken süreyi milisaniye cinsinden belirten [x-ms-retry-after-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) üstbilgisini döndürür.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK'ların tümü bu yanıtı dolaylı olarak yakalar, sunucu tarafından belirtilen yeniden deneme üstbilgine saygı duyar ve isteği yeniden deneyin. Hesabınıza aynı anda birden çok istemci erişilmediği sürece, bir sonraki yeniden deneme başarılı olur.

    İstem oranının üzerinde sürekli olarak çalışan birden fazla istemciniz varsa, istemci tarafından dahili olarak şu anda 9 olarak ayarlanan varsayılan yeniden deneme sayısı yeterli olmayabilir; bu durumda, istemci uygulamaya durum kodu 429 içeren bir [DocumentClientException](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.documentclientexception) atar. Varsayılan yeniden deneme sayısı [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy) örneğinde [setRetryOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.connectionpolicy.setretryoptions) kullanılarak değiştirilebilir. Varsayılan olarak, durum kodu 429'a sahip DocumentClientException, istek isteği oranının üzerinde çalışmaya devam ederse, 30 saniyelik bir toplu bekleme süresinden sonra döndürülür. Bu, geçerli yeniden deneme sayısı en yüksek yeniden deneme sayısından daha az olsa bile, varsayılan 9 veya kullanıcı tanımlı değer olsun oluşur.

    Otomatik yeniden deneme davranışı çoğu uygulama için esnekliği ve kullanılabilirliği artırmaya yardımcı olsa da, özellikle gecikme süresi ölçerken performans kriterleri yaparken ters gidebilir.  Deneme sunucu gaza çarparsa ve istemci SDK'nın sessizce yeniden denemesine neden olursa istemcitarafından gözlenen gecikme söker. Performans denemeleri sırasında gecikme artışlarını önlemek için, her işlem tarafından döndürülen ücreti ölçün ve isteklerin ayrılmış istek oranının altında çalıştığından emin olun. Daha fazla bilgi için [İstek birimlerine](request-units.md)bakın.
3. **Daha yüksek aktarım hızı için daha küçük dosyalar tasarlayın**

    Belirli bir işlemin istek ücreti (istek işleme maliyeti) belgenin boyutuyla doğrudan ilişkilidir. Büyük belgelerdeki işlemler, küçük belgelere yönelik işlemlerden daha pahalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı ölçeklendirme ve yüksek performans için tasarlama hakkında daha fazla bilgi edinmek için [Azure Cosmos DB'de Bölümleme ve ölçeklendirme](partition-data.md)bölümüne bakın.
