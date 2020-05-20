---
title: Azure Cosmos DB Java SDK v4 için performans ipuçları
description: Java SDK v4 için Azure Cosmos veritabanı performansını iyileştirecek istemci yapılandırma seçeneklerini öğrenin
author: anfeldma-ms
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: anfeldma
ms.openlocfilehash: dca9babff198fc780e54df6e89149f2c4c8157bf
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83677710"
---
# <a name="performance-tips-for-azure-cosmos-db-java-sdk-v4"></a>Azure Cosmos DB Java SDK v4 için performans ipuçları

> [!div class="op_single_selector"]
> * [Java SDK v4](performance-tips-java-sdk-v4-sql.md)
> * [Zaman uyumsuz Java SDK v2](performance-tips-async-java.md)
> * [Zaman uyumlu Java SDK v2](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

> [!IMPORTANT]  
> Bu makaledeki performans ipuçları yalnızca Azure Cosmos DB Java SDK v4 içindir. Daha fazla bilgi için lütfen Azure Cosmos DB Java SDK v4 [sürüm notları](sql-api-sdk-java-v4.md), [Maven deposu](https://mvnrepository.com/artifact/com.azure/azure-cosmos)ve Azure Cosmos DB Java SDK 'sı v4 [sorun giderme kılavuzunu](troubleshoot-java-sdk-v4-sql.md) görüntüleyin. Şu anda v4 'den daha eski bir sürüm kullanıyorsanız, v4 'ye yükseltme konusunda yardım için [Azure Cosmos DB Java SDK 'sı v4](migrate-java-v4-sdk.md) Kılavuzu ' na bakın.
>

Azure Cosmos DB, garantili gecikme ve verimlilik ile sorunsuz bir şekilde ölçeklenen hızlı ve esnek bir dağıtılmış veritabanıdır. Veritabanınızı Azure Cosmos DB ölçeklendirmek için önemli mimari değişiklikler yapmanız veya karmaşık kod yazmanız gerekmez. Ölçeği artırma ve azaltma, tek bir API çağrısı veya SDK Yöntem çağrısı yapmak kadar kolaydır. Ancak, Azure Cosmos DB ağ çağrılarıyla erişildiği için, Azure Cosmos DB Java SDK 'Sı v4 kullanırken en yüksek performans elde etmek için, istemci tarafı iyileştirmeler vardır.

Bu nedenle "veritabanı performanmy nasıl iyileştirebilirim?" diye soruyoruz Aşağıdaki seçenekleri göz önünde bulundurun:

## <a name="networking"></a>Ağ

* **Bağlantı modu: doğrudan modu kullan**
<a id="direct-connection"></a>
    
    İstemcinin Azure Cosmos DB 'e bağlanması, özellikle de istemci tarafı gecikme süresi bakımından performans açısından önemli etkileri vardır. *Connectionmode* , Istemci *connectionpolicy*'yi yapılandırmak için kullanılabilen bir anahtar yapılandırma ayarıdır. Java SDK 'Sı v4 Azure Cosmos DB için, kullanılabilir iki *Connectionmode*s şunlardır:  
      
    * [Ağ Geçidi (varsayılan)](/java/api/com.microsoft.azure.cosmosdb.connectionmode)  
    * [Direct](/java/api/com.microsoft.azure.cosmosdb.connectionmode)

    Bu *Connectionmode*'lar temelde, istemci makinenizden Azure Cosmos DB arka uçta bulunan bölümlere gelen istek yolunu temel alır. Genellikle doğrudan modu, en iyi performans için tercih edilen seçenektir. istemcinin TCP bağlantılarını Azure Cosmos DB arka uçtaki bölümlerle doğrudan açmasına olanak sağlar ve istekleri doğrudan bir aracı olmadan *doğrudan*gönderebilirsiniz. Buna karşılık, ağ geçidi modunda, istemciniz tarafından yapılan istekler, Azure Cosmos DB Ön uçtaki "ağ geçidi" sunucusuna yönlendirilir ve bu da isteklerinizi Azure Cosmos DB arka uçta uygun bölümlere (ler) gönderir. Uygulamanız, katı güvenlik duvarı kısıtlamalarına sahip bir kurumsal ağda çalışıyorsa, standart HTTPS bağlantı noktasını ve tek bir uç noktayı kullandığından, ağ geçidi modu en iyi seçimdir. Ancak performans zorunluluğunu getirir, ağ geçidi modunun Azure Cosmos DB, verilerin her okunışında veya her yazıldığında ek bir ağ atlaması (bölüm için istemci ağ geçidine ve ağ geçidine) içerir. Bu nedenle, daha az ağ atlaması nedeniyle doğrudan mod daha iyi performans sunar.

    *Connectionmode* , Azure Cosmos db istemci örneğinin oluşturulması sırasında *connectionpolicy* parametresiyle yapılandırılır:
    
   #### <a name="async"></a>[Eş](#tab/api-async)

   ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-connection-policy-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    public ConnectionPolicy getConnectionPolicy() {
        ConnectionPolicy policy = new ConnectionPolicy();
        policy.setMaxPoolSize(1000);
        return policy;
    }

    ConnectionPolicy connectionPolicy = new ConnectionPolicy();
    CosmosAsyncClient client = new CosmosClientBuilder()
        .setEndpoint(HOST)
        .setKey(MASTER)
        .setConnectionPolicy(connectionPolicy)
        .buildAsyncClient();
    ```

    #### <a name="sync"></a>[Eşitle](#tab/api-sync)

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-sync-api"></a><a id="java4-connection-policy-sync"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) eşitleme API 'SI

    ```java
    public ConnectionPolicy getConnectionPolicy() {
        ConnectionPolicy policy = new ConnectionPolicy();
        policy.setMaxPoolSize(1000);
        return policy;
    }

    ConnectionPolicy connectionPolicy = new ConnectionPolicy();
    CosmosClient client = new CosmosClientBuilder()
        .setEndpoint(HOST)
        .setKey(MASTER)
        .setConnectionPolicy(connectionPolicy)
        .buildClient();
    ```

    --- 

<a name="collocate-clients"></a>
* **Performans için istemcileri aynı Azure bölgesinde birlikte bulundurun** <a id="same-region"></a>

    Mümkün olduğunda Azure Cosmos veritabanıyla aynı bölgedeki Azure Cosmos DB çağıran tüm uygulamaları yerleştirin. Yaklaşık bir karşılaştırma için, 1-2 ms içinde aynı bölgedeki Azure Cosmos DB çağrıları tamamlanır, ancak ABD 'nin batı ve Doğu yakası arasındaki gecikme 50 MS >. Bu gecikme süresi büyük olasılıkla istemciden Azure veri merkezi sınırına geçerken istek tarafından alınan yola bağlı olarak istek üzerine farklılık gösterebilir. Olası en düşük gecikme süresi, çağıran uygulamanın sağlanan Azure Cosmos DB uç noktası ile aynı Azure bölgesinde bulunduğundan emin olarak elde edilir. Kullanılabilir bölgelerin listesi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

    ![Azure Cosmos DB bağlantı ilkesinin çizimi](./media/performance-tips/same-region.png)

    Çok bölgeli Azure Cosmos DB hesabıyla etkileşim kuran bir uygulamanın, isteklerin birlikte bulunan bir bölgeye gitmesini sağlamak için [tercih edilen konumları](tutorial-global-distribution-sql-api.md#preferred-locations) yapılandırması gerekir.

* **Daha düşük gecikme için Azure sanal makinenizde hızlandırılmış ağı etkinleştirin.**

Performansı en üst düzeye çıkarmak için Windows kuruluşunuzda hızlandırılmış ağı etkinleştirmek [(yönergeler için tıklayın)](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) veya [Linux (yönergeler için tıklayın)](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) için yönergeleri izlemeniz önerilir.

Hızlandırılmış ağ olmadan, Azure VM 'niz ve diğer Azure kaynakları arasında geçiş yapan GÇ, VM ve ağ kartı arasında bulunan bir konak ve sanal anahtar aracılığıyla gereksiz yere yönlendirilebilir. Ana bilgisayarın ve sanal anahtarın veri yolunda satır içi olması, iletişim kanalında gecikme süresini ve değişimi artmakla kalmaz, VM 'den CPU döngülerini da artırır. Hızlandırılmış ağ ile, VM 'ler aracı olmadan doğrudan NIC ile birlikte arabirimler; konak ve sanal anahtar tarafından işlenmekte olan tüm ağ ilkesi ayrıntıları artık NIC 'de donanımda işlenir; konak ve sanal anahtar atlanır. Genellikle daha düşük gecikme süresi ve daha yüksek aktarım hızı, hızlandırılmış ağı etkinleştirdiğinizde daha *tutarlı* gecikme süresi ve azaltılmış CPU kullanımı sağlayabilirsiniz.

Sınırlamalar: hızlandırılmış ağ, VM IŞLETIM sisteminde desteklenmelidir ve yalnızca VM durdurulduğunda ve serbest bırakıldığında etkinleştirilebilir. VM, Azure Resource Manager ile dağıtılamıyor.

Daha fazla ayrıntı için lütfen [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) ve [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) yönergelerine bakın.

## <a name="sdk-usage"></a>SDK kullanımı
* **En son SDK 'Yı yükler**

    Azure Cosmos DB SDK 'Ları, en iyi performansı sağlamak için sürekli geliştirilmiştir. En son SDK 'Yı öğrenmek ve geliştirmeleri gözden geçirmek için [Azure Cosmos DB SDK](sql-api-sdk-async-java.md) sayfalarına bakın.

* **Uygulamanızın ömrü boyunca tek bir Azure Cosmos DB istemcisi kullanın**

    Her Azure Cosmos DB istemci örneği iş parçacığı açısından güvenlidir ve verimli bağlantı yönetimi ve adres önbelleği gerçekleştirir. Azure Cosmos DB istemcisi tarafından etkili bağlantı yönetimine ve daha iyi performansa izin vermek için, uygulamanın kullanım ömrü boyunca AppDomain başına Azure Cosmos DB istemcisinin tek bir örneğini kullanmanız önerilir.

   <a id="max-connection"></a>

* **Uygulamanız için gereken en düşük tutarlılık düzeyini kullanın**

    Bir *CosmosClient*oluşturduğunuzda, açıkça bir *oturum*ayarlanmamışsa kullanılan varsayılan tutarlılık. Uygulama mantığınızın *oturum* tutarlılığı gerekmiyorsa, *tutarlılığı* *nihai*olarak ayarlayın. Note: Azure Cosmos DB değişiklik akışı işlemcisini kullanan uygulamalarda en az *oturum* tutarlılığı kullanılması önerilir.

* **Sağlanan aktarım hızını mak için zaman uyumsuz API kullanma**

    Azure Cosmos DB Java SDK v4 paketleri iki API, eşitleme ve zaman uyumsuz. , Zaman uyumsuz API SDK işlevselliği uygular, ancak eşitleme API 'si, zaman uyumsuz API çağrılarını engellemeyi sağlayan bir ince sarmalayıcı olur. Bu, yalnızca zaman uyumsuz olan eski Azure Cosmos DB zaman uyumsuz Java SDK v2 'ye ve salt eşitlenmiş ve tamamen ayrı bir uygulamaya sahip olan eski Azure Cosmos DB Sync Java SDK v2 'ye karşılık gelir. 
    
    API seçimi, istemci başlatma sırasında belirlenir; bir *CosmosClient* eşitleme API 'sini Destekleirken *Cosmosasyncclient* zaman uyumsuz API 'yi destekler. 
    
    Zaman uyumsuz API, engelleyici olmayan GÇ uygular ve amacınız Azure Cosmos DB istek verirken en iyi seçimdir. 
    
    Eşitleme API 'sini kullanmak isterseniz veya her bir isteğe yanıt üzerinde engelleyen bir API 'niz varsa ya da zaman uyumlu işlem uygulamanızdaki bir baskın paradigma istiyorsanız doğru seçim olabilir. Örneğin, bir mikro hizmet uygulamasındaki Azure Cosmos DB verileri kalıcı hale getirmeniz durumunda eşitleme API 'sini isteyebilirsiniz, bu durum, belirtilen aktarım hızı kritik değildir.  
    
    Eşitleme API 'SI aktarım hızının artan istek yanıt süresi ile azaldığını ve zaman uyumsuz API 'nin, donanımınızın tam bant genişliği yeteneklerini doygunluğu artırabileceğini unutmayın. 
    
    Coğrafi birlikte bulundurma, eşitleme API 'si kullanırken size daha yüksek ve daha tutarlı bir aktarım hızı sağlayabilir (bkz. [performans için aynı Azure bölgesindeki istemcileri birlikte bulma](#collocate-clients)), ancak yine de zaman uyumsuz API kullanımına izin verilmeyen aktarım hızını aşmayı beklemeden

    Ayrıca, Java SDK v4 zaman uyumsuz API Azure Cosmos DB uygulamak için kullanılan reaktif akışlar çerçevesi olan [Proje reaktör](https://projectreactor.io/)ile bazı kullanıcılar da tanıdık gelebilir. Bu sorun devam ediyor ise, açıklayıcı [reaktör düzen kılavuzumuzu](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-pattern-guide.md) okumanızı ve daha sonra kendinizi tanımak Için bu [girişe](https://tech.io/playgrounds/929/reactive-programming-with-reactor-3/Intro) göz atmanızı öneririz. Zaten zaman uyumsuz bir arabirimle Azure Cosmos DB kullandıysanız ve kullandığınız SDK zaman uyumsuz Java SDK v2 Azure Cosmos DB, [reactivex](http://reactivex.io/) / [rxjava](https://github.com/ReactiveX/RxJava) ile ilgili bilgi sahibi olabilirsiniz ancak proje reaktörde nelerin değiştiğini tam olarak görebilirsiniz. Bu durumda, familiarized olmak için lütfen [reaktör vs. RxJava kılavuzumuza](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-rxjava-guide.md) göz atın.

    Aşağıdaki kod parçacıkları, sırasıyla Azure Cosmos DB istemcinizi zaman uyumsuz API veya eşitleme API 'SI işlemi için başlatmayı gösterir:

    #### <a name="async"></a>[Eş](#tab/api-async)

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-async-client"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    CosmosAsyncClient client = new CosmosClientBuilder()
        .setEndpoint(HOSTNAME)
        .setKey(MASTERKEY)
        .setConnectionPolicy(CONNECTIONPOLICY)
        .setConsistencyLevel(CONSISTENCY)
        .buildAsyncClient();
    ```

    #### <a name="sync"></a>[Eşitle](#tab/api-sync)
 
    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-sync-api"></a><a id="java4-sync-client"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) eşitleme API 'SI

    ```java
    CosmosClient client = new CosmosClientBuilder()
        .setEndpoint(HOSTNAME)
        .setKey(MASTERKEY)
        .setConnectionPolicy(CONNECTIONPOLICY)
        .setConsistencyLevel(CONSISTENCY)
        .buildClient();
    ```    

    ---

* **ConnectionPolicy ayarlama**

    Varsayılan olarak, Azure Cosmos DB Java SDK v4 kullanılırken doğrudan mod Cosmos DB istekleri TCP üzerinden yapılır. Dahili olarak SDK, ağ kaynaklarını dinamik olarak yönetmek ve en iyi performansı elde etmek için özel bir doğrudan mod mimarisi kullanır.

    Java SDK 'Sı v4 Azure Cosmos DB, doğrudan modu, en iyi iş yükleriyle veritabanı performansını geliştirmek için en iyi seçenektir. 

    * ***Doğrudan moda genel bakış***

        ![Doğrudan mod mimarisinin çizimi](./media/performance-tips-async-java/rntbdtransportclient.png)

        Doğrudan modda çalışan istemci tarafı mimarisi, tahmin edilebilir ağ kullanımı ve Azure Cosmos DB çoğaltmaya çoğullanmış erişim sağlar. Yukarıdaki diyagramda, doğrudan modunun Cosmos DB arka uçtaki çoğaltmalara istemci isteklerini nasıl yönlendirdiğini gösterir. Doğrudan mod mimarisi, VERITABANı çoğaltması başına istemci tarafında en fazla 10 **Kanal** ayırır. Kanal, 30 istekten oluşan bir istek arabelleğinin önünde yer aldığı bir TCP bağlantısıdır. Bir çoğaltmaya ait olan kanallar, çoğaltmanın **hizmet uç noktası**tarafından gerektiği şekilde dinamik olarak ayrılır. Kullanıcı doğrudan modda bir istek verdiği zaman, **Transportclient** , isteği bölüm anahtarına göre uygun hizmet uç noktasına yönlendirir. **Istek kuyruğu** , hizmet uç noktasından önceki istekleri arabelleğe alır.

    * ***Doğrudan mod için ConnectionPolicy yapılandırma seçenekleri***

        Bu yapılandırma ayarları, doğrudan Mod SDK davranışını yöneten RNTBD mimarisinin davranışını denetler.
        
        İlk adım olarak aşağıdaki önerilen yapılandırma ayarlarını kullanın. Bu *Connectionpolicy* SEÇENEKLERI, SDK performansını beklenmeyen yollarla etkileyebilecek gelişmiş yapılandırma ayarlarıdır; avantajları anlamak çok rahat ve kesinlikle gerekli olmadığı müddetçe, kullanıcıların bunları değiştirmelerini öneririz. Bu konuda sorun yaşıyorsanız lütfen [Azure Cosmos DB ekibine](mailto:CosmosDBPerformanceSupport@service.microsoft.com) başvurun.

        Bir başvuru veritabanı olarak Azure Cosmos DB kullanıyorsanız (diğer bir deyişle, veritabanı birçok nokta okuma işlemi ve birkaç yazma işlemi için kullanılırsa), *ıdtimeout zaman aşımını* 0 (yani zaman aşımı yok) olarak ayarlamak kabul edilebilir.


        | Yapılandırma seçeneği       | Varsayılan    |
        | :------------------:       | :-----:    |
        | bufferPageSize             | 8192       |
        | connectionTimeout          | "PT1M"     |
        | ıdtımechanneltimeout         | "PT0S"     |
        | ıdtımeendpointtimeout        | "PT1M10S"  |
        | maxBufferCapacity          | 8388608    |
        | maxChannelsPerEndpoint     | 10         |
        | maxRequestsPerChannel      | 30         |
        | receiveHangDetectionTime   | "PT1M5S"   |
        | Requestexpiryıınterval      | "PT5S"     |
        | requestTimeout             | "PT1M"     |
        | requestTimerResolution     | "PT 0,5 S"   |
        | sendHangDetectionTime      | "PT10S"    |
        | shutdownTimeout            | "PT15S"    |

* **Bölümlenmiş koleksiyonlar için Paralel sorguları ayarlama**

    Azure Cosmos DB Java SDK v4 Paralel sorguları destekler ve bu, bölümlenmiş bir koleksiyonu paralel olarak sorgulamanızı sağlar. Daha fazla bilgi için bkz. Azure Cosmos DB Java SDK 'Sı v4 ile çalışma ile ilgili [kod örnekleri](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples) . Paralel sorgular, kendi seri karşılarındaki sorgu gecikmesini ve aktarım hızını artırmak için tasarlanmıştır.

    * ***Setmaxdegreeofparalellik ayarlama\:***
    
        Paralel sorgular birden çok bölümü paralel olarak sorgulayarak çalışır. Ancak, tek bir bölümlenmiş koleksiyondaki veriler, sorguya göre işlem içine alınır. Bu nedenle, en iyi performansı elde etmek için en yüksek performansa sahip bölüm sayısını ayarlamak için Setmaxdegreeofparalellik kullanın, diğer tüm sistem koşulları aynı kalır. Bölüm sayısını bilmiyorsanız, yüksek bir sayı ayarlamak için Setmaxdegreeofparalellik kullanabilirsiniz ve sistem en az paralellik derecesi olarak en düşük (bölüm sayısı, Kullanıcı tarafından girilen giriş) değerini seçer.

        Verilerin sorguya göre tüm bölümler arasında eşit bir şekilde dağıtılması halinde paralel sorguların en iyi avantajları ürettiğine dikkat edin. Bölümlenmiş koleksiyon, bir sorgu tarafından döndürülen verilerin tümünün veya çoğunluğunun birkaç bölümde (en kötü durumda bir bölüm) yoğunlaşarak bir şekilde bölümlenmişse, sorgunun performansı bu bölümler tarafından bottlenecked olacaktır.

    * ***SetMaxBufferedItemCount ayarlama\:***
    
        Paralel sorgu, geçerli sonuç toplu işi istemci tarafından işlendiği sırada sonuçları önceden getirmek üzere tasarlanmıştır. Önceden getirme, bir sorgunun genel gecikme artışında yardımcı olur. setMaxBufferedItemCount, önceden getirilen sonuçların sayısını sınırlar. SetMaxBufferedItemCount değeri döndürülen beklenen sonuç sayısına (veya daha yüksek bir sayıya) ayarlandığında sorgunun ön alma işleminden en fazla avantaj almasına olanak sağlar.

        Önceden getirme, Maxdegreeofparalelliği ne olursa olsun aynı şekilde çalışıyor ve tüm bölümlerdeki veriler için tek bir arabellek vardır.

* **İstemcinizi genişletme-iş yükü**

    Yüksek aktarım hızı düzeylerinde test ediyorsanız, makine CPU veya ağ kullanımında kullanıma hazır hale geldiği için istemci uygulama performans sorunlarına neden olabilir. Bu noktaya ulaştığınızda, istemci uygulamalarınızı birden çok sunucu arasında ölçeklendirerek Azure Cosmos DB hesabını daha fazla göndermeye devam edebilirsiniz.

    Thumb 'in iyi bir kuralı, belirli bir sunucuda %50 CPU kullanımını aşmamak, gecikme süresini düşük tutmak için >.

   <a id="tune-page-size"></a>

* **Daha iyi performans için, sorguların/okunan akışların sayfa boyutunu ayarlayın**

    Belgeleri oku (örneğin, *readItems*) kullanarak veya bir SQL sorgusu (*QueryItems*) verirken belge okuma işlemi gerçekleştirirken sonuçlar, sonuç kümesi çok büyükse, bölümlenmiş bir biçimde döndürülür. Varsayılan olarak, sonuçlar 100 öğe veya 1 MB Öbekle döndürülür, bu sınır ilk önce dönüştürülür.

    Uygulamanızın Azure Cosmos DB bir sorgu sağladığını ve uygulamanızın, görevi tamamlaması için tam sorgu sonuçları kümesi gerektirdiğini varsayalım. Tüm geçerli sonuçları almak için gereken ağ gidiş dönüşlerin sayısını azaltmak için, [x-MS-Max-item-Count](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) istek üst bilgisi alanını ayarlayarak sayfa boyutunu artırabilirsiniz. 

    * Tek bölümlü sorgular için, [x-MS-Max-item-Count](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) alan değerinin-1 olarak ayarlanması (sayfa boyutunda sınır olmaması), sorgu yanıtı sayfalarının sayısını en aza indirerek gecikme süresini en üst düzeye çıkarır: tam sonuç kümesi tek bir sayfada dönecektir ya da sorgu zaman aşımı aralığından daha uzun sürerse, tam sonuç kümesi mümkün olan en az sayıda sayfaya döndürülür. Bu, istek gidiş dönüş süresinin katlarına kaydedilir.
    
    * Çapraz bölüm sorguları için, [x-MS-Max-item-Count](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) alanını-1 olarak ayarlamak ve sayfa boyut sınırı risklerini kaldırmak, SDK 'yı yönetilemez sayfa boyutlarına göre kaldırır. Çapraz bölüm söz konusu olduğunda, gecikme süresini azaltmak için sayfa boyutu sınırını büyük ancak büyük olasılıkla 1000 olan büyük ancak sınırlı bir değere yükseltmeyi öneririz. 
    
    Bazı uygulamalarda, sorgu sonuçlarının tam kümesini gerektirmeyebilirsiniz. Örneğin, Kullanıcı arabiriminiz veya uygulama API 'niz aynı anda yalnızca 10 sonuç döndürürse, okuma ve sorgular için tüketilen aktarım hızını azaltmak için sayfa boyutunu 10 ' a da azaltabilirsiniz.

    Ayrıca, REST üst bilgi alanını doğrudan değiştirmek yerine *bypage* yönteminin tercih edilen sayfa boyutu bağımsız değişkenini de ayarlayabilirsiniz. [X-MS-Max-item-Count](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) veya *bypage* 'in tercih edilen sayfa boyutu bağımsız değişkeninin mutlak bir gereksinim değil yalnızca sayfa boyutunda bir üst sınır ayarlamadığını unutmayın; Bu nedenle, tercih ettiğiniz sayfa boyutundan daha küçük olan Azure Cosmos DB geri dönüş sayfaları görebilirsiniz. 

* **Uygun zamanlayıcıyı kullan (olay döngüsü GÇ ağ parçacıklarının çalınmasını önleyin)**

    Azure Cosmos DB Java SDK 'sının zaman uyumsuz işlevselliği, [Netty](https://netty.io/) engellemeyen GÇ 'yi temel alır. SDK, GÇ işlemlerini yürütmek için sabit sayıda GÇ Netty olay döngüsü iş parçacığını (makinenizin sahip olduğu CPU çekirdekleri) kullanır. API tarafından döndürülen Flox, paylaşılan GÇ olay döngüsü Netty iş parçacıklarından birindeki sonucu yayar. Bu nedenle, paylaşılan GÇ olay döngüsü Netty iş parçacıklarının engellenmemesi önemlidir. GÇ olay döngüsü Netty iş parçacığı üzerinde CPU yoğun iş veya engelleme işlemi yapıldığında kilitlenme olabilir veya SDK verimlilik önemli ölçüde azalabilir.

    Örneğin, aşağıdaki kod olay döngüsü GÇ Netty iş parçacığı üzerinde yoğun bir CPU kullanımı işi yürütür:
    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-noscheduler"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    Mono<CosmosAsyncItemResponse<CustomPOJO>> createItemPub = asyncContainer.createItem(item);
    createItemPub.subscribe(
        itemResponse -> {
            //this is executed on eventloop IO netty thread.
            //the eventloop thread is shared and is meant to return back quickly.
            //
            // DON'T do this on eventloop IO netty thread.
            veryCpuIntensiveWork();                
        });
    ```

    Sonuç alındıktan sonra, CPU yoğun işi gerçekleştirmek istiyorsanız olay döngüsü GÇ ağ parçacığında bunu yapmaktan kaçınmalısınız. Bunun yerine, aşağıda gösterildiği gibi, işinizi çalıştırmak için kendi iş parçacığını sağlamak üzere kendi zamanlayıcısını sağlayabilirsiniz.

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-scheduler"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    import reactor.core.scheduler.Schedulers;
    Mono<CosmosAsyncItemResponse<CustomPOJO>> createItemPub = asyncContainer.createItem(item);
    createItemPub
        .subscribeOn(Schedulers.elastic())
        .subscribe(
        itemResponse -> {
            //this is executed on eventloop IO netty thread.
            //the eventloop thread is shared and is meant to return back quickly.
            //
            // DON'T do this on eventloop IO netty thread.
            veryCpuIntensiveWork();                
        });
    ```

    Çalışmanızın türüne bağlı olarak, çalışmanız için uygun olan yeniden aktör zamanlayıcısını kullanmanız gerekir. Buradan okuyun [``Schedulers``](https://projectreactor.io/docs/core/release/api/reactor/core/scheduler/Schedulers.html) .

    Java SDK 'Sı v4 Azure Cosmos DB hakkında daha fazla bilgi için lütfen [GitHub 'Da Java monorepo Için Azure SDK 'nın Cosmos DB dizinine](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-cosmos)bakın.

* **Uygulamanızdaki günlük ayarlarını iyileştirin**

    Çeşitli nedenlerle, yüksek istek aktarım hızı üreten bir iş parçacığında günlük kaydı eklemeniz gerekebilir. Amacınız, bu iş parçacığı tarafından oluşturulan isteklerle bir kapsayıcının sağlanmış iş verimini tamamen kısaltmak isterse, günlüğe kaydetme iyileştirmeleri performansı büyük ölçüde iyileştirebilir.

    * ***Zaman uyumsuz günlükçü yapılandırma***

        Zaman uyumlu bir günlükçü gecikmesi, istek oluşturma iş parçacığınız için genel gecikme süresi hesaplamasına yönelik bir etken olması halinde. Yüksek performanslı uygulama iş parçacıklarından günlüğe kaydetme ek yükünü ayırmak için [log4j2](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Flogging.apache.org%2Flog4j%2Flog4j-2.3%2Fmanual%2Fasync.html&data=02%7C01%7CCosmosDBPerformanceInternal%40service.microsoft.com%7C36fd15dea8384bfe9b6b08d7c0cf2113%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637189868158267433&sdata=%2B9xfJ%2BWE%2F0CyKRPu9AmXkUrT3d3uNA9GdmwvalV3EOg%3D&reserved=0) gibi bir zaman uyumsuz günlükçü önerilir.

    * ***Netty 'ın günlüğünü devre dışı bırak***

        Netty kitaplığı günlüğü geveze ve ek CPU maliyetlerinden kaçınmak için kapalı olması gerekir (yapılandırmada oturum açmayı gizleme yeterli olmayabilir). Hata ayıklama modunda değilseniz, netty 'ın günlüğünü tamamen devre dışı bırakın. Bu nedenle, netty 'den kaynaklanan ek CPU maliyetlerini kaldırmak için Log4J kullanıyorsanız ``org.apache.log4j.Category.callAppenders()`` , kod tabanınıza aşağıdaki satırı ekleyin:

        ```java
        org.apache.log4j.Logger.getLogger("io.netty").setLevel(org.apache.log4j.Level.OFF);
        ```

 * **İşletim sistemi açık dosyaları kaynak sınırı**
 
    Bazı Linux sistemleri (Red hat gibi), açık dosya sayısı için üst sınıra ve bu nedenle toplam bağlantı sayısına sahiptir. Geçerli sınırları görüntülemek için aşağıdakileri çalıştırın:

    ```bash
    ulimit -a
    ```

    Açık dosya (nofile) sayısı, yapılandırılmış bağlantı havuzu boyutunuz ve işletim sistemi tarafından diğer açık dosyalar için yeterli yere yetecek kadar büyük olmalıdır. Daha büyük bir bağlantı havuzu boyutu için izin verecek şekilde değiştirilebilir.

    Limit. conf dosyasını açın:

    ```bash
    vim /etc/security/limits.conf
    ```
    
    Aşağıdaki satırları ekleyin/değiştirin:

    ```
    * - nofile 100000
    ```

* **Nokta yazma işlemlerinde bölüm anahtarı belirtin**

    Nokta yazma performansını geliştirmek için, aşağıda gösterildiği gibi, nokta yazma API çağrısındaki öğe bölüm anahtarını belirtin:

    #### <a name="async"></a>[Eş](#tab/api-async)

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-createitem-good-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    asyncContainer.createItem(item,new PartitionKey(pk),new CosmosItemRequestOptions()).block();
    ```

    #### <a name="sync"></a>[Eşitle](#tab/api-sync)

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-sync-api"></a><a id="java4-createitem-good-sync"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) eşitleme API 'SI

    ```java
    syncContainer.createItem(item,new PartitionKey(pk),new CosmosItemRequestOptions());
    ```

    ---

    yalnızca öğe örneğini sağlamak yerine aşağıda gösterildiği gibi:

    #### <a name="async"></a>[Eş](#tab/api-async)

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-createitem-bad-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    asyncContainer.createItem(item).block();
    ```

    #### <a name="sync"></a>[Eşitle](#tab/api-sync)

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-sync-api"></a><a id="java4-createitem-bad-sync"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) eşitleme API 'SI

    ```java
    syncContainer.createItem(item);
    ```

    ---

    İkincisi desteklenir, ancak uygulamanıza gecikme süresi ekler; SDK 'nın öğeyi ayrıştırması ve bölüm anahtarını ayıklamalıdır.

## <a name="indexing-policy"></a>Dizin oluşturma ilkesi
 
* **Daha hızlı yazma işlemleri için, kullanılmayan yolları dizin oluşturmanın dışında bırakma**

    Azure Cosmos DB Dizin oluşturma ilkesi, dizin oluşturma yollarını (Setıncludedpaths ve setExcludedPaths) kullanarak hangi belge yollarının dizine eklenmesini veya dışlanacağını belirtmenize olanak tanır. Dizin oluşturma yollarının kullanımı, sorgu desenlerinin önceden bildiği senaryolar için gelişmiş yazma performansı ve daha düşük dizin depolaması sunabilir, dizin oluşturma maliyetleri doğrudan dizinli benzersiz yolların sayısıyla bağıntılı olur. Örneğin, aşağıdaki kod, "*" joker karakterini kullanarak, belgelerin tamamının (alt ağaç olarak da bilinir) bir bölümünün tamamını nasıl dışlayacak olduğunu gösterir.

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos"></a><a id="java4-indexing"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos)
    ```java
    Index numberIndex = Index.Range(DataType.Number);
    indexes.add(numberIndex);
    includedPath.setIndexes(indexes);
    includedPaths.add(includedPath);
    indexingPolicy.setIncludedPaths(includedPaths);        
    containerProperties.setIndexingPolicy(indexingPolicy);
    ``` 

    Daha fazla bilgi için bkz. [Azure Cosmos DB Dizin oluşturma ilkeleri](indexing-policies.md).

## <a name="throughput"></a>Aktarım hızı
<a id="measure-rus"></a>

* **Düşük istek birimleri/ikinci kullanım için ölçme ve ayarlama**

    Azure Cosmos DB, UDF 'ler, saklı yordamlar ve tetikleyicilerle ilişkisel ve hiyerarşik sorgular dahil olmak üzere zengin bir veritabanı işlemleri kümesi sunar ve bunların tümü bir veritabanı koleksiyonu içindeki belgeler üzerinde çalışıyor. Bu işlemlerden her biriyle ilişkilendirilmiş maliyet, işlemi tamamlamak için gereken CPU, GÇ ve belleğe göre değişiklik gösterir. Donanım kaynaklarını düşünmek ve yönetmek yerine, çeşitli veritabanı işlemlerini gerçekleştirmek ve bir uygulama isteğine hizmet vermek için gereken kaynaklar için bir istek birimi (RU) tek bir ölçü olarak düşünebilirsiniz.

    Aktarım hızı, her bir kapsayıcı için ayarlanan [istek birimi](request-units.md) sayısına göre sağlanır. İstek birimi tüketimi, saniye başına bir hız olarak değerlendirilir. Kapsayıcı için sağlanan istek birimi oranını aşan uygulamalar, oran için sağlanan düzeyin altına düşene kadar sınırlandırılır. Uygulamanız daha yüksek bir işleme düzeyi gerektiriyorsa, ek istek birimleri sağlayarak aktarım hızını artırabilirsiniz.

    Bir sorgunun karmaşıklığı, bir işlem için kaç istek biriminin tüketildiğini etkiler. Koşulların sayısı, koşulların doğası, UDF sayısı ve kaynak veri kümesinin boyutu, sorgu işlemlerinin maliyetini etkiler.

    Herhangi bir işlemin (oluşturma, güncelleştirme veya silme) yükünü ölçmek için, bu işlemler tarafından tüketilen istek birimi sayısını ölçmek üzere [x-MS-Request-şarj](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) üst bilgisini inceleyin. Ayrıca, Resourceresıset \<> veya FeedResponse T> ' de eşdeğer Requestücretözelliğine bakabilirsiniz \< .

    #### <a name="async"></a>[Eş](#tab/api-async)

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-async-api"></a><a id="java4-request-charge-async"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) zaman uyumsuz API

    ```java
    CosmosAsyncItemResponse<CustomPOJO> response = asyncContainer.createItem(item).block();

    response.getRequestCharge();
    ```     

    #### <a name="sync"></a>[Eşitle](#tab/api-sync)

    ### <a name="java-sdk-v4-maven-comazureazure-cosmos-sync-api"></a><a id="java4-request-charge-sync"></a>Java SDK v4 (Maven com. Azure:: Azure-Cosmos) eşitleme API 'SI    

    ```java
    CosmosItemResponse<CustomPOJO> response = syncContainer.createItem(item);

    response.getRequestCharge();
    ```     

    ---

    Bu üst bilgide döndürülen istek ücreti, sağlanan aktarım hızının bir bölümü. Örneğin, sağlanan 2000 RU/sn varsa ve önceki sorgu 1000 1KB-belgeler döndürürse, işlem maliyeti 1000 ' dir. Bu nedenle, bir saniye içinde sunucu, sonraki istekleri sınırlayan orandan önce yalnızca iki istek için geçerlidir. Daha fazla bilgi için bkz. [İstek birimleri](request-units.md) ve [istek birimi Hesaplayıcısı](https://www.documentdb.com/capacityplanner).

<a id="429"></a>
* **Tanıtıcı hız sınırlandırma/istek hızı çok büyük**

    Bir istemci bir hesap için ayrılan aktarım hızını aşmaya çalıştığında, sunucuda bir performans düşüşü olmaz ve ayrılan düzeyin ötesinde üretilen iş kapasitesi yoktur. Sunucu, isteği RequestRateTooLarge (HTTP durum kodu 429) ile sona erpreemptively ve kullanıcının isteği yeniden denemeden önce beklemesi gereken süreyi milisaniye olarak belirten [x-MS-retry-After-MS](/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) üst bilgisini döndürür.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    SDK 'lar, sunucu tarafından belirtilen yeniden deneme üst bilgisine göre bu yanıtı dolaylı olarak yakalar ve isteği yeniden dener. Hesabınız birden çok istemci tarafından aynı anda erişilmediği takdirde, sonraki yeniden deneme başarılı olur.

    İstek hızının sürekli olarak birden fazla istemciniz varsa, bu, şu anda istemci tarafından dahili olarak 9 olarak ayarlanmış olan varsayılan yeniden deneme sayısı çalışmayabilir; Bu durumda, istemci, uygulama 429 durum kodu ile *Cosmosclientexception* oluşturur. Varsayılan yeniden deneme sayısı, ConnectionPolicy örneğinde setRetryOptions kullanılarak değiştirilebilir. Varsayılan olarak, istek hızının üzerinde çalışmaya devam etmesi durumunda 429 durum kodu ile *Cosmosclientexception* , 30 saniyelik birikimli bir bekleme süresi dolduktan sonra döndürülür. Bu durum, geçerli yeniden deneme sayısı en fazla yeniden deneme sayısından az olduğunda bile, varsayılan olarak 9 veya Kullanıcı tanımlı bir değer olmalıdır.

    Otomatik yeniden deneme davranışı, çoğu uygulama için esneklik ve kullanılabilirliği artırmaya yardımcı olmakla birlikte, özellikle gecikmeyi ölçirken performans kıyaslamaları yapılırken gürültü 'ye gelebilir. Deneme sunucu tarafından azaldıysanız, istemci SDK 'sının sessizce yeniden denenmesine neden olursa istemci gözlenen gecikme süresi izlenir. Performans denemeleri sırasında gecikme sürelerini önlemek için, her bir işlem tarafından döndürülen ücreti ölçün ve isteklerin ayrılan istek oranının altında çalıştığından emin olun. Daha fazla bilgi için bkz. [İstek birimleri](request-units.md).

* **Daha yüksek aktarım hızı için daha küçük dosyalar tasarlayın**

    Belirli bir işlemin istek ücreti (istek işleme maliyeti), belgenin boyutuyla doğrudan bağıntılı olur. Büyük belgelerle ilgili işlemler, küçük belgeler için işlemlerden daha fazla maliyetlidir. İdeal olarak, uygulamanızın ve iş akışlarının boyutunu, öğe boyutunuzu ~ 1KB veya benzer bir sıra ya da büyüklük olacak şekilde mimarın. Gecikme süresine duyarlı uygulamalar için büyük öğelerin kaçınılması gerekir, çok MB 'lık belgeler uygulamanızı yavaşlatır.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı ölçek ve yüksek performans için tasarlama hakkında daha fazla bilgi için, bkz. [Azure Cosmos DB bölümleme ve ölçeklendirme](partition-data.md).
