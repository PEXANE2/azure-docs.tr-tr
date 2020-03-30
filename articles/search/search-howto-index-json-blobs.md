---
title: JSON lekeleri üzerinde arama yapın
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Blob dizinleyicisini kullanarak metin içeriği için Azure JSON bloblarını tarayın. Dizin leyiciler, Azure Blob depolama gibi seçili veri kaynakları için veri alımını otomatikleştirin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 37fc78971124240077a59d4ad99aa06cc408dbae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533965"
---
# <a name="how-to-index-json-blobs-using-a-blob-indexer-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da Blob dizinleyiciskullanarak JSON bloblarını dizinle oluşturma

Bu makalede, Azure Blob depolamasındaki JSON belgelerinden yapılandırılmış içerik ayıklamak ve Azure Bilişsel Arama'da aranabilir hale getirmek için azure bilişsel arama blob [dizinleyicisini](search-indexer-overview.md) nasıl yapılandırabileceğinizi gösterir. Bu iş akışı bir Azure Bilişsel Arama dizini oluşturur ve json lekeleri çıkarılan varolan metin ile yükler. 

JSON içeriğini dizine almak için [portal,](#json-indexer-portal) [REST API'leri](#json-indexer-rest)veya [.NET SDK'yı](#json-indexer-dotnet) kullanabilirsiniz. Tüm yaklaşımlarda yaygın olan, JSON belgelerinin bir Azure Depolama hesabında bir blob kapsayıcısında bulunmasıdır. Azure'a uygun olmayan diğer platformlardan JSON belgelerini zorlama kılavuzu için Azure [Bilişsel Arama'da Veri alma işlemi'ne](search-what-is-data-import.md)bakın.

Azure Blob depolama sundaki JSON lekeleri genellikle tek bir JSON `json`belgesi (ayrıştırma modudur) veya JSON varlıkları koleksiyonudur. Koleksiyonlar için, blob iyi biçimlendirilmiş JSON öğeleri bir **dizi** olabilir `jsonArray`(ayrıştırma modudur). Blobs da birden çok bireysel JSON varlıklar yeni bir çizgi (ayrıştırma modu) ile ayrılmış `jsonLines`oluşabilir. İstekteki **ayrıştırmaModu** parametresi çıkış yapılarını belirler.

> [!NOTE]
> Tek bir blob'dan birden çok arama belgesini dizine alma hakkında daha fazla bilgi [için](search-howto-index-one-to-many-blobs.md)bkz.

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>Portalı kullanma

JSON belgelerini dizine almanın en kolay [yöntemi, Azure portalında](https://portal.azure.com/)bir sihirbaz kullanmaktır. Katmanlar [**verisi,**](search-import-data-portal.md) Azure blob kapsayıcısındameta verileri ayrıştırarak varsayılan bir dizin oluşturabilir, kaynak alanlarını hedef dizin alanlarını eşleyebilir ve dizini tek bir işlemde yükleyebilir. Kaynak verilerin boyutuna ve karmaşıklığına bağlı olarak, dakika içinde operasyonel tam metin arama dizininolabilir.

Daha düşük gecikme süreleri için ve bant genişliği ücretlerinden kaçınmak için hem Azure Bilişsel Arama hem de Azure Depolama için aynı bölgeyi veya konumu kullanmanızı öneririz.

### <a name="1---prepare-source-data"></a>1 - Kaynak verileri hazırlama

[Azure portalında oturum açın](https://portal.azure.com/) ve verilerinizi içerecek [bir Blob kapsayıcısı oluşturun.](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) Ortak Erişim Düzeyi, geçerli değerlerinden herhangi biri için ayarlanabilir.

**Verilerinizi Alma veri** sihirbazında almak için depolama hesabı adı, kapsayıcı adı ve bir erişim anahtarına ihtiyacınız olacaktır.

### <a name="2---start-import-data-wizard"></a>2 - Veri alma sihirbazı başlat

Arama hizmetinizin Genel Bakış sayfasında, [sihirbazı](search-import-data-portal.md) komut çubuğundan başlatabilirsiniz.

   ![Portalda veri komutunu alma](./media/search-import-data-portal/import-data-cmd2.png "Veri Aktar sihirbazını başlatın")

### <a name="3---set-the-data-source"></a>3 - Veri kaynağını ayarlama

Veri **kaynağı** sayfasında, kaynak aşağıdaki özelliklere sahip **Azure Blob Depolama**olmalıdır:

+ **Çıkarılacak veriler** *İçerik ve meta veriler*olmalıdır. Bu seçeneği seçmek sihirbazın bir dizin şemasını çıkarmasını ve alma için alanları eşlemesine olanak tanır.
   
+ **Ayrışma modu** *JSON, JSON* *dizisi* veya *JSON satırlarına*ayarlanmalıdır. 

  *JSON,* her blob'u tek bir arama belgesi olarak ifade eder ve arama sonuçlarında bağımsız bir öğe olarak gösterir. 

  *JSON dizisi* iyi biçimlendirilmiş JSON verileri içeren lekeler içindir - iyi biçimlendirilmiş JSON nesnelerin bir dizi karşılık gelir veya nesnelerin bir dizi bir özelliği vardır ve her öğe bağımsız, bağımsız bir arama belgesi olarak ifade edilmesi istiyorum. Blobs karmaşık ve *JSON dizi* seçmezseniz tüm blob tek bir belge olarak yutulmuş.

  *JSON satırları,* her varlığın bağımsız bir arama belgesi olarak ifade edilmesini istediğiniz yeni bir satırla ayrılmış birden çok JSON varlığından oluşan lekeler içindir. Blobs karmaşık ve *JSON satırları* ayrıştırma modu seçmezseniz, o zaman tüm blob tek bir belge olarak yutulrülmez.
   
+ **Depolama kapsayıcısı,** depolama hesabınızı ve kapsayıcınızı veya kapsayıcıya çözülen bir bağlantı dizesini belirtmelidir. Blob servis portalı sayfasında bağlantı dizeleri alabilirsiniz.

   ![Blob veri kaynağı tanımı](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-enrich-content-page-in-the-wizard"></a>4 - Sihirbazdaki "İçeriği zenginleştir" sayfasını atla

Bilişsel becerileri (veya zenginleştirme) ekleme bir ithalat gereksinimi değildir. Dizin oluşturma ardınıza [AI zenginleştirme eklemeye](cognitive-search-concept-intro.md) özel bir gereksiniminiz yoksa, bu adımı atlamanız gerekir.

Adımı atlamak için sayfanın altındaki mavi düğmeleri "Sonraki" ve "Atla" için tıklatın.

### <a name="5---set-index-attributes"></a>5 - Dizin özniteliklerini ayarlama

**Dizin** sayfasında, veri türüne sahip alanların listesini ve dizin özniteliklerini ayarlamak için bir dizi onay kutusu görmeniz gerekir. Sihirbaz, meta verilere dayalı ve kaynak verileri örnekleyerek bir alan listesi oluşturabilir. 

Öznitelik sütununun üst kısmındaki onay kutusunu tıklatarak öznitelikleri toplu olarak seçebilirsiniz. İstemci uygulamasına döndürülmesi ve tam metin arama işlemine tabi tutulması gereken her alan için **Alınabilen** ve **Aranabilir'i** seçin. Tamsayılar tam metin veya bulanık aranabilir (sayılar kelimenin tam olarak değerlendirilir ve genellikle filtrelerde yararlıdır) olmadığını fark edeceksiniz.

Daha fazla bilgi için [dizin özniteliklerinin](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) ve [dil çözümleyicilerinin](https://docs.microsoft.com/rest/api/searchservice/language-support) açıklamasını gözden geçirin. 

Seçimlerinizi gözden geçirmek için bir dakikanızı ayırın. Sihirbazı çalıştırdıktan sonra, fiziksel veri yapıları oluşturulur ve tüm nesneleri düşürmeden ve yeniden oluşturmadan bu alanları kaldıramazsınız.

   ![Blob dizin tanımı](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6 - Dizin oluştur

Tam olarak belirtildiği gibi, sihirbaz arama hizmetinizde üç farklı nesne oluşturur. Bir veri kaynağı nesnesi ve dizin nesnesi, Azure Bilişsel Arama hizmetinizde adlandırılmış kaynaklar olarak kaydedilir. Son adım bir dizinleyici nesne oluşturur. Dizin oluşturucuya ad vermek, aynı sihirbaz dizisinde oluşturulan dizin ve veri kaynağı nesnesi tarafından bağımsız olarak zamanlayıp yönetebileceğiniz bağımsız bir kaynak olarak var olmasını sağlar.

Dizin leyicileri bilmiyorsanız, *dizin leyici,* Azure Bilişsel Arama'da aranabilir içerik için harici bir veri kaynağını tarayan bir kaynaktır. **Alma veri** sihirbazının çıktısı, JSON veri kaynağınızı tarayan, aranabilir içeriği ayıklayan ve Azure Bilişsel Arama'da bir dizin içine aktaran bir dizin leyicidir.

   ![Blob dizinleyici tanımı](media/search-howto-index-json/import-wizard-json-indexer.png)

Sihirbazı çalıştırmak ve tüm nesneleri oluşturmak için **Tamam'ı** tıklatın. Dizin oluşturma hemen başlar.

Portal sayfalarında veri alma izleme yapabilirsiniz. İlerleme bildirimleri dizin oluşturma durumunu ve kaç belgenin yüklendiğini gösterir. 

Dizin oluşturma tamamlandığında, dizininizi sorgulamak için [Arama gezgini'ni](search-explorer.md) kullanabilirsiniz.

> [!NOTE]
> Beklediğiniz verileri görmüyorsanız, daha fazla alanda daha fazla öznitelik ayarlamanız gerekebilir. Az önce oluşturduğunuz dizin ve dizin oluşturucuyu silin ve 5. 

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>REST API'lerini kullanma

Azure Bilişsel Arama'daki tüm dizin oluşturucular için ortak olan üç parçalı iş akışını izleyerek JSON bloblarını dizine dizine dizinlendirmek için REST API'sini kullanabilirsiniz: bir veri kaynağı oluşturmak, dizin oluşturmak, dizin oluşturmak. Blob depolamadan veri çıkarma, Dizin Oluştur isteğini gönderdiğinde oluşur. Bu istek tamamlandıktan sonra sorgulanabilir bir dizin olacaktır. 

Bu bölümün sonunda, üç nesnenin nasıl oluşturulabileceğini gösteren [REST örnek kodunu](#rest-example) gözden geçirebilirsiniz. Bu bölümde ayrıca [JSON ayrıştma modları,](#parsing-modes) [tek lekeler,](#parsing-single-blobs) [JSON dizileri](#parsing-arrays)ve [iç içe diziler](#nested-json-arrays)hakkında ayrıntılı bilgi içerir.

Kod tabanlı JSON dizini için, bu nesneleri oluşturmak için [Postacı](search-get-started-postman.md) ve REST API'yi kullanın:

+ [Dizin](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [veri kaynağı](https://docs.microsoft.com/rest/api/searchservice/create-data-source)
+ [Dizinleyici](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

İşlem sırası, nesneleri bu sırada oluşturmanızı ve aramanızı gerektirir. Portal iş akışının aksine, kod yaklaşımı, **Dizin Oluştur** isteği aracılığıyla gönderilen JSON belgelerini kabul etmek için kullanılabilir bir dizin gerektirir.

Azure Blob depolama sundaki JSON lekeleri genellikle tek bir JSON belgesi veya JSON "dizisi"dir. Azure Bilişsel Arama'daki blob dizinleyici, isteğe göre **ayrışma Modu** parametresini nasıl ayarladığınıza bağlı olarak her iki yapıyı da ayrışdırabilir.

| JSON belgesi | ayrışmaModu | Açıklama | Kullanılabilirlik |
|--------------|-------------|--------------|--------------|
| Blob başına bir | `json` | Ayrışdırır JSON tek bir metin yığını olarak lekeler. Her JSON blob tek bir Azure Bilişsel Arama belgesi olur. | Genellikle [hem REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API hem de [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK'da kullanılabilir. |
| Blob başına birden fazla | `jsonArray` | Dizinin her öğesinin ayrı bir Azure Bilişsel Arama belgesine dönüştüğü blob'daki bir JSON dizisini parses.  | Genellikle [hem REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API hem de [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK'da kullanılabilir. |
| Blob başına birden fazla | `jsonLines` | Birden çok JSON varlığı (bir "dizi") içeren ve her varlığın ayrı bir Azure Bilişsel Arama belgesi ne hale geldiği yeni bir satırla ayrılan bir blob'u ayrıştırır. | Genellikle [hem REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) API hem de [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK'da kullanılabilir. |

### <a name="1---assemble-inputs-for-the-request"></a>1 - İstek için girişleri birleştirin

Her istek için, Azure Bilişsel Arama (POST üstbilgisinde) için hizmet adı ve yönetici anahtarını ve blob depolama için depolama hesabı adı ve anahtarı sağlamanız gerekir. Azure Bilişsel Arama'ya HTTP istekleri göndermek için [Postacı'yı](search-get-started-postman.md) kullanabilirsiniz.

Aşağıdaki dört değeri Notepad'e kopyalayarak bir isteğe yapıştırabilirsiniz:

+ Azure Bilişsel Arama hizmet adı
+ Azure Bilişsel Arama admin anahtarı
+ Azure depolama hesabı adı
+ Azure depolama hesabı anahtarı

Bu değerleri portalda bulabilirsiniz:

1. Azure Bilişsel Arama'nın portal sayfalarında, Arama hizmeti URL'sini Genel Bakış sayfasından kopyalayın.

2. Sol gezinti bölmesinde, **Keys'i** tıklatın ve ardından birincil veya ikincil anahtarı kopyalayın (eşdeğerdir).

3. Depolama hesabınız için portal sayfalarına geçin. Soldaki gezinti bölmesinde, **Ayarlar'ın** **altında, Erişim Tuşları'nı**tıklatın. Bu sayfa hem hesap adını hem de anahtarı sağlar. Depolama hesabı nın adını ve Notepad'in anahtarlarından birini kopyalayın.

### <a name="2---create-a-data-source"></a>2 - Veri kaynağı oluşturma

Bu adım, dizinleyici tarafından kullanılan veri kaynağı bağlantı bilgilerini sağlar. Veri kaynağı, Bağlantı bilgilerini devam eden Azure Bilişsel Arama'da adlandırılmış bir nesnedir. Veri kaynağı türü, `azureblob`dizinleyici tarafından hangi veri ayıklama davranışlarının çağrıldığını belirler. 

Hizmet adı, yönetici anahtarı, depolama hesabı ve hesap anahtar yer sahipleri için geçerli değerleri değiştirin.

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key for Azure Cognitive Search]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="3---create-a-target-search-index"></a>3 - Hedef arama dizini oluşturma 

Dizin leyiciler bir dizin şemasıyla eşlenir. API'yi (portal yerine) kullanıyorsanız, dizin oluşturma işleminde belirtebilmeniz için önceden bir dizin hazırlayın.

Dizin, Azure Bilişsel Arama'da aranabilir içeriği depolar. Bir dizin oluşturmak için, bir belgedeki alanları belirten bir şema, öznitelikler ve arama deneyimini şekillendiren diğer yapılar sağlayın. Kaynakla aynı alan adları ve veri türlerine sahip bir dizin oluşturursanız, dizin oluşturucu kaynak ve hedef alanlarıyla eşleşip, alanları açıkça eşlemeniz gereken çalışmadan tasarruf sağlar.

Aşağıdaki örnekte bir [Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index) isteği gösterilmektedir. Dizin, lekelerden `content` çıkarılan metni depolamak için aranabilir bir alana sahip olacaktır:   

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key for Azure Cognitive Search]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="4---configure-and-run-the-indexer"></a>4 - Dizinleyiciyi yapılandırın ve çalıştırın

Bir dizin ve veri kaynağı nda olduğu gibi, dizin oluşturucu da bir Azure Bilişsel Arama hizmetinde oluşturduğunuz ve yeniden kullandığınız adlandırılmış bir nesnedir. Bir dizin oluşturabilirsiniz tam olarak belirtilen bir istek aşağıdaki gibi görünebilir:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key for Azure Cognitive Search]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Dizinleyici yapılandırması isteğin gövdesindedir. Azure Bilişsel Arama'da zaten var olan bir veri kaynağı ve boş bir hedef dizini gerektirir. 

Zamanlama ve parametreler isteğe bağlıdır. Bunları atlarsanız, dizinleyici ayrışma `json` modunu kullanarak hemen çalışır.

Bu özel dizinleyici alan eşlemeleri içermez. Dizinleyici tanımı içinde, kaynak JSON belgesinin özellikleri hedef arama dizininizdeki alanlarıeşile eşleşirse **alan eşlemelerini** dışarıda bırakabilirsiniz. 


### <a name="rest-example"></a>REST Örneği

Bu bölüm, nesneleri oluşturmak için kullanılan tüm isteklerin özetidir. Bileşen parçalarının tartışılması için bu makaledeki önceki bölümlere bakın.

### <a name="data-source-request"></a>Veri kaynağı isteği

Tüm dizinleyiciler, varolan verilere bağlantı bilgileri sağlayan bir veri kaynağı nesnesi gerektirir. 

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key for Azure Cognitive Search]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }  


### <a name="index-request"></a>Dizin isteği

Tüm dizinleyiciler verileri alan bir hedef dizini gerektirir. İsteğin gövdesi, aranabilir bir dizinde istenen davranışları desteklemek için atfedilen alanlardan oluşan dizin şemasını tanımlar. Dizinleyiciçalıştırdığınızda bu dizin boş olmalıdır. 

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key for Azure Cognitive Search]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="indexer-request"></a>Dizinleyici isteği

Bu istek tam olarak belirtilen dizinleyiciyi gösterir. Önceki örneklerde atlanan alan eşlemelerini içerir. Kullanılabilir bir varsayılan olduğu sürece "zamanlama", "parametreler" ve "alan Eşlemeleri"nin isteğe bağlı olduğunu hatırlayın. "Zamanlama" atlayarak dizinleyici hemen çalışmasına neden olur. "ParsingMode" atlayarak dizin "json" varsayılan kullanmasına neden olur.

Azure Bilişsel Arama'da dizin oluşturucu oluşturmak veri aktarını tetikler. Hemen çalışır ve daha sonra bir program alabiliyorsanız.

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key for Azure Cognitive Search]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }


<a name="json-indexer-dotnet"></a>

## <a name="use-net-sdk"></a>.NET SDK kullanma

.NET SDK, REST API ile tam eşitliğe sahiptir. Kavramları, iş akışını ve gereksinimleri öğrenmek için önceki REST API bölümünü gözden geçirmenizi öneririz. Daha sonra yönetilen kodda bir JSON dizinleyici uygulamak için .NET API başvuru belgelerini aşağıdaki başvurabilirsiniz.

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

<a name="parsing-modes"></a>

## <a name="parsing-modes"></a>Ayrışma modları

JSON lekeleri birden fazla form varsayılabilir. JSON dizinleyicideki **ayrıştırmaModu** parametresi, JSON blob içeriğinin Azure Bilişsel Arama dizininde nasıl ayrıştırılacağını ve yapılandırılacağını belirler:

| ayrışmaModu | Açıklama |
|-------------|-------------|
| `json`  | Her blob'u tek bir belge olarak dizine dizin. Bu varsayılandır. |
| `jsonArray` | Lekelerin JSON dizilerinden oluşuyorsa ve dizinin her öğesinin Azure Bilişsel Arama'da ayrı bir belge olması gerekiyorsa bu modu seçin. |
|`jsonLines` | Blob'larınız yeni bir satırla ayrılmış birden çok JSON varlığından oluşuyorsa ve her varlığın Azure Bilişsel Arama'da ayrı bir belge olması gerekiyorsa, bu modu seçin. |

Bir belgeyi arama sonuçlarında tek bir öğe olarak düşünebilirsiniz. Dizideki her öğenin arama sonuçlarında bağımsız bir öğe olarak gösterilmesini istiyorsanız, uygun olarak `jsonArray` veya seçeneği `jsonLines` kullanın.

Dizinleyici tanımı içinde, hedef arama dizininizi doldurmak için kaynak JSON belgesinin hangi özelliklerinin kullanıldığını seçmek için isteğe bağlı olarak [alan eşlemelerini](search-indexer-field-mappings.md) kullanabilirsiniz. `jsonArray` Ayrıştırma modu için, dizi alt düzey bir özellik olarak varsa, dizi blob içinde nereye yerleştirildiğini belirten bir belge kökü ayarlayabilirsiniz.

> [!IMPORTANT]
> Azure Bilişsel `json` `jsonArray` Arama, "veya `jsonLines` ayrıştma modunu" kullandığınızda, veri kaynağınızdaki tüm lekelerin JSON içerdiğini varsayar. Aynı veri kaynağında JSON ve json olmayan lekelerin bir karışımını desteklemeniz gerekiyorsa, [UserVoice sitemizde](https://feedback.azure.com/forums/263029-azure-search)bize bildirin.


<a name="parsing-single-blobs"></a>

## <a name="parse-single-json-blobs"></a>Ayrışdıran tek JSON lekeleri

Varsayılan olarak, [Azure Bilişsel Arama blob dizinleyicis](search-howto-indexing-azure-blob-storage.md) metin tek bir yığın olarak JSON lekelerparses. Genellikle, JSON belgelerinizin yapısını korumak istersiniz. Örneğin, Azure Blob depolama alanında aşağıdaki JSON belgeniz olduğunu varsayalım:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Blob dizinleyici, JSON belgesini tek bir Azure Bilişsel Arama belgesine benzetiyor. Dizinleyici, kaynaktan gelen "metin", "datePublished" ve "etiketleri" aynı adlandırılmış ve yazılan hedef dizin alanlarıyla eşleştirerek bir dizini yükler.

Belirtildiği gibi, alan eşlemeleri gerekli değildir. "Metin", "datePublished" ve "tags" alanları içeren bir dizin göz önüne alındığında, blob dizinleyici istekte bir alan eşleme sayılmadan doğru eşleciliği çıkartabilir.

<a name="parsing-arrays"></a>

## <a name="parse-json-arrays"></a>Ayrışdıran JSON dizileri

Alternatif olarak, JSON dizi seçeneğini kullanabilirsiniz. Bu seçenek, blobs *iyi biçimlendirilmiş JSON nesneleri*bir dizi içeriyorsa ve her öğeayrı bir Azure Bilişsel Arama belgesi olmak istiyorum yararlıdır. Örneğin, aşağıdaki JSON blob'u göz önüne alındığında, Azure Bilişsel Arama dizininizi her biri "id" ve "metin" alanları içeren üç ayrı belgeyle doldurabilirsiniz.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Bir JSON dizisi için dizinleyici tanımı aşağıdaki örneğe benzer olmalıdır. ParsingMode parametresinin parser'ı `jsonArray` belirlediğine dikkat edin. Doğru ayrıştırıcıyı belirtme ve doğru veri girişine sahip olmak, JSON bloblarını diziye ekin dizileme için yalnızca iki diziye özgü gereksinimdir.

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Yine, alan eşlemeleri atlanabilir dikkat edin. Aynı şekilde "id" ve "metin" alanları adlı bir dizin varsayarsak, blob dizinleyici açık bir alan eşleme listesi olmadan doğru eşleme çıkarabilirsiniz.

<a name="nested-json-arrays"></a>

## <a name="parse-nested-arrays"></a>İç içe dizileri ayrıştAma
İç içe öğeleri olan JSON dizileri `documentRoot` için, çok düzeyli bir yapıyı belirtmek için bir belirtebilirsiniz. Örneğin, lekelerin şuna benziyorsa:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Özellikte bulunan diziyi dizilemek `level2` için bu yapılandırmayı kullanın:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="parse-blobs-separated-by-newlines"></a>Yeni hatlarla ayrılan ayrıştArı lekeleri

Blob'unuz yeni bir satırla ayrılmış birden çok JSON varlığı içeriyorsa ve her öğenin ayrı bir Azure Bilişsel Arama belgesi olmasını istiyorsanız, JSON satırları seçeneğini tercih edebilirsiniz. Örneğin, aşağıdaki blob (üç farklı JSON varlığı nın bulunduğu) göz önüne alındığında, Azure Bilişsel Arama dizininizi her biri "id" ve "metin" alanları içeren üç ayrı belgeyle doldurabilirsiniz.

    { "id" : "1", "text" : "example 1" }
    { "id" : "2", "text" : "example 2" }
    { "id" : "3", "text" : "example 3" }

JSON satırları için dizinleyici tanımı aşağıdaki örneğe benzer olmalıdır. ParsingMode parametresinin parser'ı `jsonLines` belirlediğine dikkat edin. 

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonLines" } }
    }

Yine, alan eşlemelerinin `jsonArray` ayrıştırma moduna benzer şekilde atlananabildiğine dikkat edin.

## <a name="add-field-mappings"></a>Alan eşlemeleri ekleme

Kaynak ve hedef alanlar mükemmel şekilde hizalanmadığında, açık alandan alana ilişkilendirmeler için istek gövdesinde bir alan eşleme bölümü tanımlayabilirsiniz.

Şu anda Azure Bilişsel Arama, yalnızca ilkel veri türlerini, dize dizilerini ve GeoJSON noktalarını desteklediği için rasgele JSON belgelerini doğrudan diziye dizileyemez. Ancak, JSON belgenizin parçalarını seçmek ve bunları arama belgenizin üst düzey alanlarına "kaldırmak" için **alan eşlemelerini** kullanabilirsiniz. Alan eşleme temelleri hakkında bilgi edinmek için [Azure Bilişsel Arama dizinleyicilerinde Alan eşlemelerine](search-indexer-field-mappings.md)bakın.

Örnek JSON belgemizi yeniden ziyaret etmek:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Aşağıdaki alanları içeren bir arama `text` dizini varsayalım: `tags` türü, `Edm.String` `date` türü, `Edm.DateTimeOffset`ve türü `Collection(Edm.String)`. Dizindeki kaynak ve `date` alandaki "datePublished" arasındaki tutarsızlıka dikkat edin. JSON'unuzu istenilen şekle sokmak için aşağıdaki alan eşlemelerini kullanın:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Eşlemelerde kaynak alan adları [JSON Pointer](https://tools.ietf.org/html/rfc6901) gösterimi kullanılarak belirtilir. JSON belgenizin köküne başvurmak için ileri eğik çizgiyle başlarsınız, ardından ileri eğik çizgiyle ayrılmış yolu kullanarak istenen özelliği (iç içe geçme düzeyinde) seçersiniz.

Sıfır tabanlı bir dizin kullanarak tek tek dizi öğelerine de başvurabilirsiniz. Örneğin, yukarıdaki örnekten "etiketler" dizisinin ilk öğesini seçmek için aşağıdaki gibi bir alan eşlemi kullanın:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Alan eşleme yolundaki bir kaynak alan adı JSON'da bulunmayan bir özelliğe başvuruyorsa, bu eşleme hatasız atlanır. Bu, belgeleri farklı bir şema (yaygın bir kullanım örneği) ile destekleyebilmemiz için yapılır. Doğrulama olmadığından, alan eşleme belirtiminizdeki yazım hatalarını önlemek için dikkatli olmanız gerekir.
>
>

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama'daki Dizin Oluşturucular](search-indexer-overview.md)
+ [Azure Bilişsel Arama ile Azure Blob Depolamasına Dizin Oluşturma](search-howto-index-json-blobs.md)
+ [Azure Bilişsel Arama blob dizinleyici ile CSV blobs dizinoluşturma](search-howto-index-csv-blobs.md)
+ [Öğretici: Azure Blob depolamasından yarı yapılandırılmış verileri arayın](search-semi-structured-data.md)
