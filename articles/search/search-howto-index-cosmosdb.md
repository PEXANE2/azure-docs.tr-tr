---
title: Azure Cosmos DB verileri üzerinden arama yapın
titleSuffix: Azure Cognitive Search
description: Azure Cosmos DB'deki verileri Azure Bilişsel Arama'da aranabilir bir dizine aktarın. Dizin leyiciler, Azure Cosmos DB gibi belirli veri kaynakları için veri alımını otomatikleştirin.
author: mgottein
manager: nitinme
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/02/2020
ms.openlocfilehash: d1723b6c5d56554fbff576f6a07e37455845bda4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283010"
---
# <a name="how-to-index-cosmos-db-data-using-an-indexer-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da dizin oluşturucu kullanarak Cosmos DB verilerinden dizin oluşturma 

> [!IMPORTANT] 
> SQL API genellikle kullanılabilir.
> MongoDB API, Gremlin API ve Cassandra API desteği şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)doldurarak önizlemelere erişim isteğinde bulunabilirsiniz. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) önizleme özellikleri sağlar. Şu anda sınırlı portal desteği ve .NET SDK desteği yoktur.

> [!WARNING]
> Yalnızca [Tutarlı](https://docs.microsoft.com/azure/cosmos-db/index-policy#indexing-mode) olarak ayarlanmış bir [dizin oluşturma ilkesine](https://docs.microsoft.com/azure/cosmos-db/index-policy) sahip Cosmos DB koleksiyonları Azure Bilişsel Arama tarafından desteklenir. Koleksiyonları Tembel dizin oluşturma ilkesiyle dizine alma önerilmez ve verilerin eksik olması yla sonuçlanabilir. Dizin oluşturma devre dışı bırakılmış koleksiyonlar desteklenmez.

Bu makalede, azure cosmos DB [dizinleyicisini](search-indexer-overview.md) içerik ayıklamak ve Azure Bilişsel Arama'da aranabilir hale getirmek için nasıl yapılandırabileceğinizi gösterir. Bu iş akışı bir Azure Bilişsel Arama dizini oluşturur ve Azure Cosmos DB'den çıkarılan varolan metinle yükler. 

Terminoloji kafa karıştırıcı olabileceğinden, Azure [Cosmos DB dizin oluşturma](https://docs.microsoft.com/azure/cosmos-db/index-overview) ve [Azure Bilişsel Arama dizin oluşturmanın](search-what-is-an-index.md) her hizmete özgü farklı işlemler olduğunu belirtmekte yarar vardır. Azure Bilişsel Arama dizini oluşturmayı başlatmadan önce Azure Cosmos DB veritabanınızın zaten var olması ve veri içermesi gerekir.

Azure Bilişsel Arama'daki Cosmos DB dizinleyicisi, farklı protokoller aracılığıyla erişilen [Azure Cosmos DB öğelerini](https://docs.microsoft.com/azure/cosmos-db/databases-containers-items#azure-cosmos-items) tarayabilir. 

+ Genellikle kullanılabilir olan [SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-query-reference)için, veri kaynağı ve dizinleyici oluşturmak için [portal](#cosmos-indexer-portal), [REST API](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)veya [.NET SDK'yı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet) kullanabilirsiniz.

+ [MongoDB API (önizleme)](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction)için, veri kaynağı ve dizinleyici oluşturmak için [portal](#cosmos-indexer-portal) veya [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) kullanabilirsiniz.

+ [Cassandra API (önizleme)](https://docs.microsoft.com/azure/cosmos-db/cassandra-introduction) ve [Gremlin API (önizleme)](https://docs.microsoft.com/azure/cosmos-db/graph-introduction)için, veri kaynağı ve dizinleyici oluşturmak için yalnızca [REST API sürümünü kullanabilirsiniz 2019-05-06-Önizleme.](search-api-preview.md)


> [!Note]
> Azure Bilişsel Arama'da desteklenmiş görmek [istiyorsanız, Tablo API'si](https://feedback.azure.com/forums/263029-azure-search/suggestions/32759746-azure-search-should-be-able-to-index-cosmos-db-tab) için Kullanıcı Sesi'ni oylayabilirsiniz.
>

<a name="cosmos-indexer-portal"></a>

## <a name="use-the-portal"></a>Portalı kullanma

> [!Note]
> Portal şu anda SQL API ve MongoDB API (önizleme) destekler.

Azure Cosmos DB öğelerini dizine ekin ekine getirmek için en kolay [yöntem, Azure portalında](https://portal.azure.com/)bir sihirbaz kullanmaktır. Azure Bilişsel Arama'daki [**veri**](search-import-data-portal.md) örneklemesi ve kapsayıcıdaki meta verileri okuma sı, varsayılan dizin oluşturabilir, kaynak alanlarını hedef dizin alanlarını eşleyebilir ve dizini tek bir işlemde yükleyebilir. Kaynak verilerin boyutuna ve karmaşıklığına bağlı olarak, dakika içinde operasyonel tam metin arama dizininolabilir.

Daha düşük gecikme süreleri ve bant genişliği ücretlerinden kaçınmak için hem Azure Bilişsel Arama hem de Azure Cosmos DB için aynı bölgeyi veya konumu kullanmanızı öneririz.

### <a name="1---prepare-source-data"></a>1 - Kaynak verileri hazırlama

Bir Cosmos DB hesabınız, SQL API, MongoDB API (önizleme) veya Gremlin API (önizleme) ile veritabanında eşlenmiş bir Azure Cosmos DB veritabanına sahip olmalısınız.

Cosmos DB veritabanınızın veri içerdiğinden emin olun. [Alma veri sihirbazı](search-import-data-portal.md) meta verileri okur ve bir dizin şemasını çıkarmak için veri örneklemesi gerçekleştirir, ancak Cosmos DB'den de veri yükler. Veriler eksikse, sihirbaz bu hatayı durdurur "Veri kaynağından dizin şemasını algıla: Veri kaynağı 'boş toplama' veri döndürmediği için prototip dizini oluşturamaz".

### <a name="2---start-import-data-wizard"></a>2 - Veri alma sihirbazı başlat

[Sihirbazı](search-import-data-portal.md) Azure Bilişsel Arama hizmeti sayfasındaki komut çubuğundan başlatabilirsiniz veya Cosmos DB SQL API'ye bağlanıyorsanız Cosmos DB hesabınızın sol navigasyon bölmesinin **Ayarlar** bölümünde **Azure Bilişsel Arama** Ekle'yi tıklatabilirsiniz.

   ![Portalda veri komutunu alma](./media/search-import-data-portal/import-data-cmd2.png "Veri Aktar sihirbazını başlatın")

### <a name="3---set-the-data-source"></a>3 - Veri kaynağını ayarlama

Veri **kaynağı** sayfasında, kaynak Aşağıdaki özelliklere sahip **Cosmos DB**olmalıdır:

+ **Ad,** veri kaynağı nesnenin adıdır. Oluşturulduktan sonra, diğer iş yükleri için seçebilirsiniz.

+ **Cosmos DB hesabı,** Cosmos DB'nin birincil veya `AccountEndpoint` ikincil `AccountKey`bağlantı dizesi olmalıdır, bir ve bir . MongoDB koleksiyonları için, bağlantı dizesinin sonuna **ApiKind=MongoDb** ekleyin ve bağlantı dizesinden bir yarı nokta nokta lı olarak ayırın. Gremlin API ve Cassandra API [için, REST API](#cosmosdb-indexer-rest)için yönergeleri kullanın.

+ **Veritabanı** hesaptan varolan bir veritabanıdır. 

+ **Toplama** belgelerin bir kapsayıcı. İçe aktarmanın başarılı olabilmesi için belgelerin bulunması gerekir. 

+ Tüm belgeleri istiyorsanız **sorgu** boş olabilir, aksi takdirde belge alt kümesini seçen bir sorgu girişi yapabilirsiniz. **Sorgu** yalnızca SQL API için kullanılabilir.

   ![Cosmos DB veri kaynağı tanımı](media/search-howto-index-cosmosdb/cosmosdb-datasource.png "Cosmos DB veri kaynağı tanımı")

### <a name="4---skip-the-enrich-content-page-in-the-wizard"></a>4 - Sihirbazdaki "İçeriği zenginleştir" sayfasını atla

Bilişsel becerileri (veya zenginleştirme) ekleme bir ithalat gereksinimi değildir. Dizin oluşturma ardınıza [AI zenginleştirme eklemeye](cognitive-search-concept-intro.md) özel bir gereksiniminiz yoksa, bu adımı atlamanız gerekir.

Adımı atlamak için sayfanın altındaki mavi düğmeleri "Sonraki" ve "Atla" için tıklatın.

### <a name="5---set-index-attributes"></a>5 - Dizin özniteliklerini ayarlama

**Dizin** sayfasında, veri türüne sahip alanların listesini ve dizin özniteliklerini ayarlamak için bir dizi onay kutusu görmeniz gerekir. Sihirbaz, meta verilere dayalı ve kaynak verileri örnekleyerek bir alan listesi oluşturabilir. 

Öznitelik sütununun üst kısmındaki onay kutusunu tıklatarak öznitelikleri toplu olarak seçebilirsiniz. İstemci uygulamasına döndürülmesi ve tam metin arama işlemine tabi tutulması gereken her alan için **Alınabilen** ve **Aranabilir'i** seçin. Tamsayılar tam metin veya bulanık aranabilir (sayılar kelimenin tam olarak değerlendirilir ve genellikle filtrelerde yararlıdır) olmadığını fark edeceksiniz.

Daha fazla bilgi için [dizin özniteliklerinin](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) ve [dil çözümleyicilerinin](https://docs.microsoft.com/rest/api/searchservice/language-support) açıklamasını gözden geçirin. 

Seçimlerinizi gözden geçirmek için bir dakikanızı ayırın. Sihirbazı çalıştırdıktan sonra, fiziksel veri yapıları oluşturulur ve tüm nesneleri düşürmeden ve yeniden oluşturmadan bu alanları kaldıramazsınız.

   ![Cosmos DB dizin tanımı](media/search-howto-index-cosmosdb/cosmosdb-index-schema.png "Cosmos DB dizin tanımı")

### <a name="6---create-indexer"></a>6 - Dizin oluştur

Tam olarak belirtildiği gibi, sihirbaz arama hizmetinizde üç farklı nesne oluşturur. Bir veri kaynağı nesnesi ve dizin nesnesi, Azure Bilişsel Arama hizmetinizde adlandırılmış kaynaklar olarak kaydedilir. Son adım bir dizinleyici nesne oluşturur. Dizin oluşturucuya ad vermek, aynı sihirbaz dizisinde oluşturulan dizin ve veri kaynağı nesnesi tarafından bağımsız olarak zamanlayıp yönetebileceğiniz bağımsız bir kaynak olarak var olmasını sağlar.

Dizin leyicileri bilmiyorsanız, *dizin leyici,* Azure Bilişsel Arama'da aranabilir içerik için harici bir veri kaynağını tarayan bir kaynaktır. **Alma veri** sihirbazının çıktısı, Cosmos DB veri kaynağınızı tarayan, aranabilir içeriği ayıklayan ve Azure Bilişsel Arama'da bir dizin içine aktaran bir dizin leyicidir.

Aşağıdaki ekran görüntüsü varsayılan dizinleyici yapılandırmasını gösterir. Dizinleyiciyi bir kez çalıştırmak istiyorsanız **Bir Kez'e** geçebilirsiniz. Sihirbazı çalıştırmak ve tüm nesneleri oluşturmak için **Gönder'i** tıklatın. Dizin oluşturma hemen başlar.

   ![Cosmos DB dizinleyici tanımı](media/search-howto-index-cosmosdb/cosmosdb-indexer.png "Cosmos DB dizinleyici tanımı")

Portal sayfalarında veri alma izleme yapabilirsiniz. İlerleme bildirimleri dizin oluşturma durumunu ve kaç belgenin yüklendiğini gösterir. 

Dizin oluşturma tamamlandığında, dizininizi sorgulamak için [Arama gezgini'ni](search-explorer.md) kullanabilirsiniz.

> [!NOTE]
> Beklediğiniz verileri görmüyorsanız, daha fazla alanda daha fazla öznitelik ayarlamanız gerekebilir. Az önce oluşturduğunuz dizin ve dizin oluşturucuyu silin ve 5. 

<a name="cosmosdb-indexer-rest"></a>

## <a name="use-rest-apis"></a>REST API'lerini kullanma

Azure Bilişsel Arama'daki tüm dizin oluşturucular için ortak olan üç bölümlü iş akışını izleyerek Azure Cosmos DB verilerini dizinlendirmek için REST API'sini kullanabilirsiniz: bir veri kaynağı oluşturmak, dizin oluşturmak, dizin oluşturmak. Cosmos DB'den veri çıkarma, Dizin Oluştur isteğini gönderdiğiniz zaman oluşur. Bu istek tamamlandıktan sonra sorgulanabilir bir dizin olacaktır. 

> [!NOTE]
> Cosmos DB Gremlin API veya Cosmos DB Cassandra API verileri dizine almak için öncelikle [bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)doldurarak geçitli önizlemelere erişim talep etmelisiniz. İsteğiniz işlendikten sonra, veri kaynağını oluşturmak için [REST API sürümünün 2019-05-06-Önizleme](search-api-preview.md) sürümünü nasıl kullanacağınız la ilgili talimatlar alacaksınız.

Bu makalenin [başlarında, Azure Cosmos DB dizin oluşturma](https://docs.microsoft.com/azure/cosmos-db/index-overview) ve [Azure Bilişsel Arama dizin oluşturma](search-what-is-an-index.md) nın farklı işlemler olduğu belirtilmiştir. Cosmos DB dizinleme için varsayılan olarak tüm belgeler Cassandra API dışında otomatik olarak dizine alınır. Otomatik dizin oluşturmayı kapatırsanız, belgelere yalnızca kendi bağlantıları aracılığıyla veya belge kimliğini kullanarak sorgularla erişilebilir. Azure Bilişsel Arama dizin oluşturma, Azure Bilişsel Arama tarafından dizine eklenecek koleksiyonda Cosmos DB otomatik dizin oluşturmanın açık olmasını gerektirir. Cosmos DB Cassandra API dizinleyici önizlemesi için kaydolurken, Cosmos DB dizini oluşturmanın nasıl ayarlanacağına ilişkin talimatlar verilir.

> [!WARNING]
> Azure Cosmos DB, DocumentDB'nin yeni neslidir. Daha önce API sürümü **2017-11-11** `documentdb` ile sözdizimini kullanabilirsiniz. Bu, veri kaynağı türünü olarak `cosmosdb` belirtebileceğin anlamına geliyordu. `documentdb` API sürümü **2019-05-06'dan** başlayarak hem Azure Bilişsel `cosmosdb` Arama API'leri hem de Portal yalnızca bu makalede belirtildiği gibi sözdizimini destekler. Bu, bir Cosmos `cosmosdb` DB bitiş noktasına bağlanmak istiyorsanız veri kaynağı türünün olması gerektiği anlamına gelir.

### <a name="1---assemble-inputs-for-the-request"></a>1 - İstek için girişleri birleştirin

Her istek için, Azure Bilişsel Arama (POST üstbilgisinde) için hizmet adı ve yönetici anahtarını ve blob depolama için depolama hesabı adı ve anahtarı sağlamanız gerekir. Azure Bilişsel Arama'ya HTTP istekleri göndermek için [Postacı'yı](search-get-started-postman.md) kullanabilirsiniz.

Aşağıdaki dört değeri Notepad'e kopyalayarak bir isteğe yapıştırabilirsiniz:

+ Azure Bilişsel Arama hizmet adı
+ Azure Bilişsel Arama admin anahtarı
+ Cosmos DB bağlantı dizesi

Bu değerleri portalda bulabilirsiniz:

1. Azure Bilişsel Arama'nın portal sayfalarında, Arama hizmeti URL'sini Genel Bakış sayfasından kopyalayın.

2. Sol gezinti bölmesinde, **Keys'i** tıklatın ve ardından birincil veya ikincil anahtarı kopyalayın (eşdeğerdir).

3. Cosmos depolama hesabınız için portal sayfalarına geçin. Sol daki gezinti bölmesinde **Ayarlar'ın**altında **Tuşlar'ı**tıklatın. Bu sayfa bir URI, iki bağlantı dizeleri kümesi ve iki anahtar kümesi sağlar. Bağlantı dizelerinden birini Notepad'e kopyalayın.

### <a name="2---create-a-data-source"></a>2 - Veri kaynağı oluşturma

Bir **veri kaynağı,** verileri dizine, kimlik bilgilerini ve verilerdeki değişiklikleri (koleksiyonunuzdaki değiştirilmiş veya silinmiş belgeler gibi) tanımlamak için ilkeleri belirtir. Veri kaynağı, birden çok dizinleyici tarafından kullanılabilen bağımsız bir kaynak olarak tanımlanır.

Bir veri kaynağı oluşturmak için bir POST isteği formüle edin:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mycosmosdbdatasource",
        "type": "cosmosdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myCosmosDbEndpoint.documents.azure.com;AccountKey=myCosmosDbAuthKey;Database=myCosmosDbDatabaseId"
        },
        "container": { "name": "myCollection", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

İsteğin gövdesi, aşağıdaki alanları içermesi gereken veri kaynağı tanımını içerir:

| Alan   | Açıklama |
|---------|-------------|
| **Adı** | Gereklidir. Veri kaynağı nesnenizi temsil edecek herhangi bir ad seçin. |
|**Türü**| Gereklidir. Olmalı. `cosmosdb` |
|**Kimlik bilgi -leri** | Gereklidir. Cosmos DB bağlantı dizesi olmalı.<br/>SQL koleksiyonları için bağlantı dizeleri bu biçimdedir:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`<br/><br/>MongoDB koleksiyonları için bağlantı dizesine **ApiKind=MongoDb** ekleyin:<br/>`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>;ApiKind=MongoDb`<br/><br/>Gremlin grafikleri ve Cassandra tabloları için, önizlemeye ve kimlik bilgilerini nasıl biçimlendireceklerine ilişkin bilgilere erişmek için [kapılı dizinleyici önizlemesine](https://aka.ms/azure-cognitive-search/indexer-preview) kaydolun.<br/><br/>Bitiş noktası url'sinde bağlantı noktası numaralarından kaçının. Bağlantı noktası numarasını eklerseniz, Azure Bilişsel Arama Azure Cosmos DB veritabanınızı dizine ekleyemez.|
| **Kapsayıcı** | Aşağıdaki öğeleri içerir: <br/>**isim**: Gerekli. Dizinlenecek veritabanı koleksiyonunun kimliğini belirtin.<br/>**query**: İsteğe bağlı. Rasgele bir JSON belgesini Azure Bilişsel Arama'nın dizine ekedebileceği düz bir şemaya düzleştirmek için bir sorgu belirtebilirsiniz.<br/>MongoDB API, Gremlin API ve Cassandra API için sorgular desteklenmez. |
| **dataChangeDetectionPolicy** | Önerilen. [Bkz. Değiştirilen Belgeleri Dizine](#DataChangeDetectionPolicy) Alma bölümüne bakın.|
|**dataDeletionDetectionPolicy** | İsteğe bağlı. [Silinen Belgeleri Dizine Alma](#DataDeletionDetectionPolicy) bölümüne bakın.|

### <a name="using-queries-to-shape-indexed-data"></a>Dizinlenmiş verileri şekillendirmek için sorguları kullanma
İç içe geçmiş özellikleri veya dizileri düzleştirmek, JSON özelliklerini yansıtmak ve dizinlenecek verileri filtrelemek için bir SQL sorgusu belirtebilirsiniz. 

> [!WARNING]
> **MongoDB API,** **Gremlin API**ve **Cassandra API**için özel `container.query` sorgular desteklenmez: parametre null veya atlanır olarak ayarlanmalıdır. Özel bir sorgu kullanmanız gerekiyorsa, lütfen [Kullanıcı Sesi'nde](https://feedback.azure.com/forums/263029-azure-search)bize bildirin.

Örnek belge:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "cosmosdb", "search"]
    }

Filtre sorgusu:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Düzleme sorgusu:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Projeksiyon sorgusu:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Dizi düzleme sorgusu:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts


### <a name="3---create-a-target-search-index"></a>3 - Hedef arama dizini oluşturma 

Zaten bir [hedef azure bilişsel arama dizininizin](/rest/api/searchservice/create-index) yoksa oluşturun. Aşağıdaki örnek, kimlik ve açıklama alanı içeren bir dizin oluşturur:

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Hedef dizinizin şeasının kaynak JSON belgelerinin şemasıyla veya özel sorgu projeksiyonunuzun çıktısıyla uyumlu olduğundan emin olun.

> [!NOTE]
> Bölümlenmiş koleksiyonlar için varsayılan belge anahtarı, alan adları `_rid` alt karakterle başlayamadığı için `rid` Azure Bilişsel Arama'nın otomatik olarak yeniden adladığı Azure Cosmos DB özelliğidir. Ayrıca, Azure Cosmos `_rid` DB değerleri, Azure Bilişsel Arama anahtarlarında geçersiz olan karakterler içerir. Bu nedenle, `_rid` değerler Base64 kodlanır.
> 
> Azure Bilişsel Arama, MongoDB koleksiyonlarıiçin `_id` özelliği otomatik `id`olarak '' e yeniden adlandırır.  

### <a name="mapping-between-json-data-types-and-azure-cognitive-search-data-types"></a>JSON Veri Türleri ve Azure Bilişsel Arama Veri Türleri arasında eşleme
| JSON veri türü | Uyumlu hedef dizin alanı türleri |
| --- | --- |
| Bool |Edm.Boolean, Edm.String |
| Sayıca benzeyen numaralar |Edm.Int32, Edm.Int64, Edm.String |
| Kayan noktalara benzeyen sayılar |Edm.Double, Edm.String |
| Dize |Edm.String |
| Örneğin ["a", "b", "c"] |Collection(Edm.String) |
| Tarihlere benzeyen dizeleri |Edm.DateTimeOffset, Edm.String |
| GeoJSON nesneleri, örneğin { "type": "Point", "koordinatlar": [uzun, lat] } |Edm.GeographyPoint |
| Diğer JSON nesneleri |Yok |

### <a name="4---configure-and-run-the-indexer"></a>4 - Dizinleyiciyi yapılandırın ve çalıştırın

Dizin ve veri kaynağı oluşturulduktan sonra dizin oluşturabilirsiniz:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mycosmosdbindexer",
      "dataSourceName" : "mycosmosdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizinleyici her iki saatte bir çalışır (zamanlama aralığı "PT2H" olarak ayarlanır). Her 30 dakikada bir dizinleyici çalıştırmak için aralığı "PT30M" olarak ayarlayın. En kısa desteklenen aralık 5 dakikadır. Zamanlama isteğe bağlıdır - atlanırsa, dizin oluşturucu oluşturulduğunda yalnızca bir kez çalışır. Ancak, istediğiniz zaman isteğe bağlı bir dizinleyici çalıştırabilirsiniz.   

Create Indexer API hakkında daha fazla bilgi [için, Oluştur Dizinleyici'ye](https://docs.microsoft.com/rest/api/searchservice/create-indexer)göz atın.

Dizin oluşturma zamanlamaları tanımlama hakkında daha fazla bilgi için Azure Bilişsel Arama için dizin oluşturma yı [nasıl zamanlayınız.](search-howto-schedule-indexers.md)

## <a name="use-net"></a>.NET’i kullanma

Genel olarak kullanılabilir .NET SDK, genel olarak kullanılabilen REST API ile tam eşitliğe sahiptir. Kavramları, iş akışını ve gereksinimleri öğrenmek için önceki REST API bölümünü gözden geçirmenizi öneririz. Daha sonra yönetilen kodda bir JSON dizinleyici uygulamak için .NET API başvuru belgelerini aşağıdaki başvurabilirsiniz.

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

<a name="DataChangeDetectionPolicy"></a>

## <a name="indexing-changed-documents"></a>Değiştirilen belgeleri dizine alma

Veri değişikliği algılama ilkesinin amacı, değiştirilen veri öğelerini verimli bir şekilde tanımlamaktır. Şu anda desteklenen tek [`HighWaterMarkChangeDetectionPolicy`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.highwatermarkchangedetectionpolicy) ilke, Azure Cosmos DB tarafından sağlanan `_ts` ve aşağıdaki gibi belirtilen (zaman damgası) özelliğinin kullanılmasıdır:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

İyi bir dizin oluşturma performansı sağlamak için bu ilkeyi kullanmak önerilir. 

Özel bir sorgu kullanıyorsanız, özelliğin `_ts` sorgu tarafından yansıtıldığından emin olun.

<a name="IncrementalProgress"></a>

### <a name="incremental-progress-and-custom-queries"></a>Artımlı ilerleme ve özel sorgular

Dizin oluşturma sırasında artan ilerleme, dizinleyici yürütme geçici hatalar veya yürütme süresi sınırı tarafından kesilirse, dizinleyici sıfırdan tüm koleksiyonu yeniden dizine almak zorunda yerine, bir sonraki çalıştığında bıraktığı yerden alabilir sağlar. Bu, özellikle büyük koleksiyonları dizine ekrirken önemlidir. 

Özel bir sorgu kullanırken artımlı ilerlemeyi etkinleştirmek için, `_ts` sorgunuzun sonuçları sütuna göre emrettiğinden emin olun. Bu, Azure Bilişsel Arama'nın hataların varlığında artımlı ilerleme sağlamak için kullandığı periyodik olarak işaret etüt edilmesini sağlar.   

Bazı durumlarda, sorgunuz bir `ORDER BY [collection alias]._ts` yan tümce içerse bile, Azure Bilişsel Arama `_ts`sorgunun . Azure Bilişsel Arama'ya, sonuçların yapılandırma özelliğini `assumeOrderByHighWaterMarkColumn` kullanarak sıralı olduğunu söyleyebilirsiniz. Bu ipucunu belirtmek için dizin oluşturveya güncelleştirin: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>

## <a name="indexing-deleted-documents"></a>Silinen belgeleri dizine alma

Satırlar koleksiyondan silindiğinde, normalde bu satırları arama dizininden de silmek istersiniz. Veri silme algılama ilkesinin amacı, silinen veri öğelerini verimli bir şekilde tanımlamaktır. Şu anda desteklenen tek `Soft Delete` ilke, aşağıdaki gibi belirtilen ilkedir (silme bir tür bayrakla işaretlenir).

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Özel bir sorgu kullanıyorsanız, başvurulan özelliğin `softDeleteColumnName` sorgu tarafından yansıtıldığından emin olun.

Aşağıdaki örnek, yumuşak silme ilkesine sahip bir veri kaynağı oluşturur:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mycosmosdbdatasource",
        "type": "cosmosdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myCosmosDbEndpoint.documents.azure.com;AccountKey=myCosmosDbAuthKey;Database=myCosmosDbDatabaseId"
        },
        "container": { "name": "myCosmosDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="next-steps"></a><a name="NextSteps"></a>Sonraki adımlar

Tebrikler! Bir dizin leyici kullanarak Azure Cosmos DB ile Azure Bilişsel Arama'yı nasıl entegre acağınızı öğrendiniz.

* Azure Cosmos DB hakkında daha fazla bilgi edinmek için [Azure Cosmos DB hizmet sayfasına](https://azure.microsoft.com/services/cosmos-db/)bakın.
* Azure Bilişsel Arama hakkında daha fazla bilgi edinmek için [Arama hizmeti sayfasına](https://azure.microsoft.com/services/search/)bakın.
