---
title: 'Öğretici: AZURE lekeleri üzerinde REST ve AI'
titleSuffix: Azure Cognitive Search
description: Postacı ve Azure Bilişsel Arama REST API'lerini kullanarak Blob depolamasındaki içerik üzerinden metin çıkarma ve doğal dil işleme örneğine bir adım atın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/26/2020
ms.openlocfilehash: 8acafa14afab507b704806056efac0f877a47684
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78190731"
---
# <a name="tutorial-use-rest-and-ai-to-generate-searchable-content-from-azure-blobs"></a>Öğretici: Azure lekelerinden aranabilir içerik oluşturmak için REST ve AI'yi kullanın

Azure Blob depolama alanında yapılandırılmamış metin veya resimlervarsa, bir [AI zenginleştirme ardışık aygıtı](cognitive-search-concept-intro.md) bilgi ayıklayabilir ve tam metin arama veya bilgi madenciliği senaryoları için yararlı olan yeni içerik oluşturabilir. Bir ardışık işlem hattı görüntüleri işleyebilir, ancak bu REST öğreticisi, sorgularda, farklı şekillerde ve filtrelerde yararlanabileceğiniz yeni alanlar oluşturmak için dil algılama ve doğal dil işleme uygulayarak metne odaklanır.

Bu öğretici, aşağıdaki görevleri gerçekleştirmek için Postacı ve [Arama REST API'larını](https://docs.microsoft.com/rest/api/searchservice/) kullanır:

> [!div class="checklist"]
> * Azure Blob depolama alanında PDF, HTML, DOCX ve PPTX gibi tüm belgelerle (yapılandırılmamış metin) başlayın.
> * Metni ayıklayan, dili algılayan, varlıkları tanıyan ve anahtar tümcecikleri algılayan bir ardışık sözcük tanımla.
> * Çıktıyı depolamak için bir dizin tanımlayın (ham içerik, artı boru hattı tarafından oluşturulan ad değeri çiftleri).
> * Dönüşümleri ve çözümlemesi başlatmak ve dizini oluşturmak ve yüklemek için ardışık hattı çalıştırın.
> * Tam metin aramave zengin bir sorgu sözdizimini kullanarak sonuçları keşfedin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap açın.

## <a name="prerequisites"></a>Ön koşullar

+ [Azure Depolama](https://azure.microsoft.com/services/storage/)
+ [Postman masaüstü uygulaması](https://www.getpostman.com/)
+ [Varolan bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturma](search-create-service-portal.md) veya bulma 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz bir arama hizmeti sizi üç dizin, üç dizin leyici ve üç veri kaynağıyla sınırlar. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, yeni kaynakları kabul etmek için hizmetinizde yer olduğundan emin olun.

## <a name="download-files"></a>Dosyaları indirme

1. Bu [OneDrive klasörünü](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) açın ve sol üst köşede dosyaları bilgisayarınıza kopyalamak için **İndir'i** tıklatın. 

1. Zip dosyasına sağ tıklayın ve **Tümünü Ayıkla'yı**seçin. Çeşitli türlerde 14 dosya vardır. Bu egzersiz için 7'yi kullanacaksın.

## <a name="1---create-services"></a>1 - Hizmet oluşturma

Bu öğretici, dizin oluşturma ve sorgular için Azure Bilişsel Arama, AI zenginleştirme için arka uçtaki Bilişsel Hizmetler ve verileri sağlamak için Azure Blob depolamasını kullanır. Bu öğretici, Bilişsel Hizmetler'de her gün endeksleyici başına 20 işlemin ücretsiz tahsisi altında kalır, bu nedenle oluşturmanız gereken tek hizmetler arama ve depolamadır.

Mümkünse, yakınlık ve yönetilebilirlik için hem aynı bölgede hem de kaynak grubunda oluşturun. Uygulamada, Azure Depolama hesabınız herhangi bir bölgede olabilir.

### <a name="start-with-azure-storage"></a>Azure Depolama ile başlayın

1. [Azure portalında oturum açın](https://portal.azure.com/) ve **+ Kaynak Oluştur'a**tıklayın.

1. Depolama *hesabı* arayın ve Microsoft'un Depolama Hesabı teklifini seçin.

   ![Depolama hesabı oluşturma](media/cognitive-search-tutorial-blob/storage-account.png "Depolama hesabı oluşturma")

1. Temel Bilgiler sekmesinde aşağıdaki öğeler gereklidir. Diğer her şey için varsayılanları kabul edin.

   + **Kaynak grubu.** Varolan bir tane seçin veya yeni bir tane oluşturun, ancak toplu olarak yönetebilmeniz için tüm hizmetler için aynı grubu kullanın.

   + **Depolama hesabı adı.** Aynı türde birden fazla kaynağınız olabileceğini düşünüyorsanız, örneğin *blobstoragewestus*gibi türüne ve bölgesine göre ayrıştırmak için adı kullanın. 

   + **Konum**. Mümkünse, Azure Bilişsel Arama ve Bilişsel Hizmetler için kullanılan aynı konumu seçin. Tek bir konum bant genişliği ücretlerini geçersiz kılar.

   + **Hesap Türü**. Varsayılan, *StorageV2 (genel amaçlı v2)* seçin.

1. Hizmeti oluşturmak için **Gözden Geçir + Oluştur'u** tıklatın.

1. Oluşturulduktan sonra, Genel Bakış sayfasını açmak **için kaynağa git'i** tıklatın.

1. **Blobs** hizmetini tıklatın.

1. Bir kapsayıcı oluşturmak ve buna *dişli arama-demo*adını vermek için **+ Kapsayıcı'yı** tıklatın.

1. *Cog-search-demo'yu* seçin ve ardından indirme dosyalarını kaydettiğiniz klasörü açmak için **Yükle'yi** tıklatın. Görüntü olmayan dosyaların tümünü seçin. 7 dosyanız olmalı. Yüklemek için **Tamam'ı** tıklatın.

   ![Örnek dosyaları yükleme](media/cognitive-search-tutorial-blob/sample-files.png "Örnek dosyaları yükleme")

1. Azure Depolama'dan ayrılmadan önce, Azure Bilişsel Arama'da bağlantı formüle edebilmeniz için bir bağlantı dizesi alın. 

   1. Depolama hesabınızın Genel Bakış sayfasına göz atın (örnek olarak *blobstragewestus* kullandık). 
   
   1. Sol gezinti bölmesinde **Access tuşlarını** seçin ve bağlantı dizelerinden birini kopyalayın. 

   Bağlantı dizesi aşağıdaki örneğe benzer bir URL'dir:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

1. Bağlantı dizesini Notepad'e kaydedin. Veri kaynağı bağlantısını kurarken daha sonra ihtiyacınız olacak.

### <a name="cognitive-services"></a>Bilişsel hizmetler

AI zenginleştirme, doğal dil ve görüntü işleme için Metin Analizi ve Bilgisayar Vizyonu da dahil olmak üzere Bilişsel Hizmetler tarafından desteklenmektedir. Amacınız gerçek bir prototipi veya projeyi tamamlamaksa, bu noktada Bilişsel Hizmetler'i (Azure Bilişsel Arama ile aynı bölgede) dizin oluşturma işlemlerine ekleyebilmeniz için sayılırdınız.

Ancak bu alıştırma için kaynak sağlama işlemini atlayabilirsiniz, çünkü Azure Bilişsel Arama arka planda Bilişsel Hizmetler'e bağlanabilir ve dizin oluşturma işlemi başına 20 ücretsiz işlem sağlayabilir. Bu öğretici 7 işlem kullandığından, ücretsiz ayırma yeterlidir. Daha büyük projeler için, bilişsel hizmetleri öde-as-you-go S0 katmanında sağlama yı planlayın. Daha fazla bilgi için [bkz.](cognitive-search-attach-cognitive-services.md)

### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Üçüncü bileşen, [portalda oluşturabileceğiniz](search-create-service-portal.md)Azure Bilişsel Arama'dır. Bu izbiyi tamamlamak için Ücretsiz katmanı kullanabilirsiniz. 

Azure Blob depolamada olduğu gibi, erişim anahtarını toplamak için bir dakikaayırın. Ayrıca, istekleri yapılandırmaya başladığınızda, her isteğin kimliğini doğrulamak için kullanılan bitiş noktası ve yönetici api anahtarını sağlamanız gerekir.

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Azure Bilişsel Arama için yönetici api anahtarı ve URL'si alın

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda arama hizmetinizin adını alın. Bitiş noktası URL'sini inceleyerek hizmet adınızı onaylayabilirsiniz. Bitiş noktası URL'niz olsaydı, `https://mydemo.search.windows.net`hizmet `mydemo`adınız .

2. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

   Sorgu anahtarını da alın. Salt okunur erişimle sorgu isteklerini vermek en iyi yöntemdir.

   ![Hizmet adını ve yöneticive sorgu anahtarlarını alın](media/search-get-started-nodejs/service-name-and-keys.png)

Tüm istekler, hizmetinize gönderilen her isteğin üstbilgisinde bir api anahtarı gerektirir. Geçerli bir anahtar, istek bazında, isteği gönderen uygulama ile onu işleyen hizmet arasında güven oluşturur.

## <a name="2---set-up-postman"></a>2 - Postacı kurmak

Postman’i başlatın ve bir HTTP isteği ayarlayın. Bu araca aşina değilseniz, [Postacı kullanarak Azure Bilişsel Arama REST API'lerini keşfedin'e](search-get-started-postman.md)bakın.

Bu öğreticide kullanılan istek yöntemleri **POST**, **PUT**ve **GET**vardır. Arama hizmetinize dört API çağrısı yapmak için yöntemleri kullanırsınız: bir veri kaynağı, bir skillset, dizin ve dizin oluştur.

Üstbilgide, Azure Bilişsel Arama `application/json` hizmetinizin yönetici api anahtarına "İçerik türü" ayarlayın ve ayarlayın. `api-key` Üstbilgileri ayarladıktan sonra, bunları bu alıştırmadaki her istek için kullanabilirsiniz.

  ![Postacı istek URL'si ve üstbilgi](media/search-get-started-postman/postman-url.png "Postacı istek URL'si ve üstbilgi")

## <a name="3---create-the-pipeline"></a>3 - Boru hattını oluşturun

Azure Bilişsel Arama'da, AI işleme dizin oluşturma (veya veri alımı) sırasında gerçekleşir. İzleyicinin bu bölümü dört nesne oluşturur: veri kaynağı, dizin tanımı, skillset, indexer. 

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

Veri [kaynağı nesnesi,](https://docs.microsoft.com/rest/api/searchservice/create-data-source) dosyaları içeren Blob kapsayıcısına bağlantı dizesini sağlar.

1. HIZMET-Ad'ınızı hizmetin gerçek adı ile değiştirerek **POST'u** ve aşağıdaki URL'yi kullanın.

   ```http
   https://[YOUR-SERVICE-NAME].search.windows.net/datasources?api-version=2019-05-06
   ```

1. İstek **Gövdesi'nde,** depolama hesabınızın gerçek `connectionString` bağlantısını değiştirerek aşağıdaki JSON tanımını kopyalayın. 

   Kapsayıcı adını da da yapmayı unutmayın. Daha önceki bir adımda konteyner adı için "dişli arama-demo" önerdik.

    ```json
    {
      "name" : "cog-search-demo-ds",
      "description" : "Demo files to demonstrate cognitive search capabilities.",
      "type" : "azureblob",
      "credentials" :
      { "connectionString" :
        "DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT>;AccountKey=<YOUR-ACCOUNT-KEY>;"
      },
      "container" : { "name" : "<YOUR-BLOB-CONTAINER-NAME>" }
    }
    ```
1. İsteği gönderin. Başarıyı onaylayan 201 durum kodunu görmelisiniz. 

Bir 403 veya 404 hatası aldıysanız, istek yapısını denetleyin: `api-version=2019-05-06`, uç nokta üzerinde olmalıdır, `api-key`, Üst bilgide `Content-Type` öğesinden sonra gelmelidir ve değeri bir arama hizmeti için geçerli olmalıdır. Sözdiziminin doğru olduğundan emin olmak için JSON belgesini çevrimiçi json doğrulayıcısı aracılığıyla çalıştırmak isteyebilirsiniz. 

### <a name="step-2-create-a-skillset"></a>Adım 2: Bir beceri oluşturun

[Skillset](https://docs.microsoft.com/rest/api/searchservice/create-skillset) nesnesi, içeriğinize uygulanan zenginleştirme adımlarını içerir. 

1. SİzE-HİzMET-İsİm'i hizmetinizin gerçek adı ile değiştirerek **PUT'u** ve aşağıdaki URL'yi kullanın.

    ```http
    https://[YOUR-SERVICE-NAME].search.windows.net/skillsets/cog-search-demo-ss?api-version=2019-05-06
    ```

1. İstek **Gövdesi,** aşağıdaki JSON tanımını kopyalayın. Bu beceri aşağıdaki yerleşik becerilerden oluşur.

   | Beceri                 | Açıklama    |
   |-----------------------|----------------|
   | [Varlık Tanıma](cognitive-search-skill-entity-recognition.md) | Blob kapsayıcısındaki içerikten kişilerin, kuruluşların ve konumların adlarını ayıklar. |
   | [Dil Algılama](cognitive-search-skill-language-detection.md) | İçeriğin dilini algılar. |
   | [Metin Bölme](cognitive-search-skill-textsplit.md)  | Anahtar tümcecik çıkarma becerisini çağırmadan önce büyük içeriği daha küçük parçalara ayırır. Anahtar tümcecik ayıklama, 50.000 veya daha az karakterden oluşan girişi kabul eder. Bu sınıra uymak için örnek dosyaların birkaç tanesinin bölünmesi gerekir. |
   | [Anahtar İfade Ayıklama](cognitive-search-skill-keyphrases.md) | En önemli anahtar ifadeleri çıkarır. |

   Her beceri, belge içeriğinde yürütülür. İşlem sırasında Azure Bilişsel Arama, farklı dosya biçimlerinden gelen içeriği okumak için her belgeyi çatlaklar. Kaynak dosyadan gelen, bulunan metin, oluşturulan ```content``` alanına her belge için birer birer yerleştirilir. Bu nedenle, giriş ```"/document/content"```olur.

   Anahtar tümcecik ayıklama için, daha büyük dosyaları sayfalara bölmek için metin ayırıcı ```"document/pages/*"``` becerisini kullandığımız için, anahtar ```"/document/content"```tümcecik çıkarma becerisinin bağlamı (belgedeki her sayfa için) yerine .

    ```json
    {
      "description": "Extract entities, detect language and extract key-phrases",
      "skills":
      [
        {
          "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
          "categories": [ "Person", "Organization", "Location" ],
          "defaultLanguageCode": "en",
          "inputs": [
            { "name": "text", "source": "/document/content" }
          ],
          "outputs": [
            { "name": "persons", "targetName": "persons" },
            { "name": "organizations", "targetName": "organizations" },
            { "name": "locations", "targetName": "locations" }
          ]
        },
        {
          "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
          "inputs": [
            { "name": "text", "source": "/document/content" }
          ],
          "outputs": [
            { "name": "languageCode", "targetName": "languageCode" }
          ]
        },
        {
          "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
          "textSplitMode" : "pages",
          "maximumPageLength": 4000,
          "inputs": [
            { "name": "text", "source": "/document/content" },
            { "name": "languageCode", "source": "/document/languageCode" }
          ],
          "outputs": [
            { "name": "textItems", "targetName": "pages" }
          ]
        },
        {
          "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
          "context": "/document/pages/*",
          "inputs": [
            { "name": "text", "source": "/document/pages/*" },
            { "name":"languageCode", "source": "/document/languageCode" }
          ],
          "outputs": [
            { "name": "keyPhrases", "targetName": "keyPhrases" }
          ]
        }
      ]
    }
    ```
    Beceri kümesinin grafiksel gösterimi aşağıda gösterilmektedir. 

    ![Bir skillset'i anlama](media/cognitive-search-tutorial-blob/skillset.png "Bir skillset'i anlama")

1. İsteği gönderin. Postacı başarıyı onaylayan 201 bir durum kodu döndürmelidir. 

> [!NOTE]
> Çıktılar bir dizine eşlenebilir, aşağı akış becerisine yönelik giriş olarak kullanılır veya dil kodunda olduğu gibi her iki şekilde de kullanılabilir. Dizinde bir dil kodu, filtreleme için yararlıdır. Giriş olarak dil kodu, sözcük bölünmesiyle ilgili dilbilgisi kurallarını bildirmek için metin analizi becerileri tarafından kullanılır. Beceri kümesi temelleri hakkında daha fazla bilgi için bkz. [Beceri kümesini tanımlama](cognitive-search-defining-skillset.md).

### <a name="step-3-create-an-index"></a>Adım 3: Dizin oluşturma

[Dizin,](https://docs.microsoft.com/rest/api/searchservice/create-index) Azure Bilişsel Arama'da ters dizinlerde ve diğer yapılarda içeriğinizin fiziksel ifadesini oluşturmak için kullanılan şemayı sağlar. Bir dizinen büyük bileşeni, veri türü ve özniteliklerinin Azure Bilişsel Arama'da içeriği ve davranışları belirlediği alanlar koleksiyonudur.

1. Dizininizi adlandırmak için Sİzİn-HİzMET-İsİm'i hizmetinizin gerçek adı ile değiştirerek **PUT'u** ve aşağıdaki URL'yi kullanın.

   ```http
   https://[YOUR-SERVICE-NAME].search.windows.net/indexes/cog-search-demo-idx?api-version=2019-05-06
   ```

1. İstek **Gövdesi'nde**aşağıdaki JSON tanımını kopyalayın. Alan `content` belgenin kendisini depolar. Skillset `languageCode`tarafından `keyPhrases`oluşturulan `organizations` yeni bilgiler (alanlar ve değerler) için ek alanlar.

    ```json
    {
      "fields": [
        {
          "name": "id",
          "type": "Edm.String",
          "key": true,
          "searchable": true,
          "filterable": false,
          "facetable": false,
          "sortable": true
        },
        {
          "name": "metadata_storage_name",
          "type": "Edm.String",
          "searchable": false,
          "filterable": false,
          "facetable": false,
          "sortable": false
        },
        {
          "name": "content",
          "type": "Edm.String",
          "sortable": false,
          "searchable": true,
          "filterable": false,
          "facetable": false
        },
        {
          "name": "languageCode",
          "type": "Edm.String",
          "searchable": true,
          "filterable": false,
          "facetable": false
        },
        {
          "name": "keyPhrases",
          "type": "Collection(Edm.String)",
          "searchable": true,
          "filterable": false,
          "facetable": false
        },
        {
          "name": "persons",
          "type": "Collection(Edm.String)",
          "searchable": true,
          "sortable": false,
          "filterable": true,
          "facetable": true
        },
        {
          "name": "organizations",
          "type": "Collection(Edm.String)",
          "searchable": true,
          "sortable": false,
          "filterable": true,
          "facetable": true
        },
        {
          "name": "locations",
          "type": "Collection(Edm.String)",
          "searchable": true,
          "sortable": false,
          "filterable": true,
          "facetable": true
        }
      ]
    }
    ```

1. İsteği gönderin. Postacı başarıyı onaylayan 201 bir durum kodu döndürmelidir. 

### <a name="step-4-create-and-run-an-indexer"></a>Adım 4: Bir dizin oluştur ve çalıştır

Bir [Dizinleyici,](https://docs.microsoft.com/rest/api/searchservice/create-indexer) ardışık hattı kullanır. Şimdiye kadar oluşturduğunuz üç bileşen (veri kaynağı, skillset, index) bir dizin oluşturucuya girişlerdir. Azure Bilişsel Arama'da dizin oluşturucu oluşturmak, tüm ardışık ardışık alanı harekete dönüştüren olaydır. 

1. Dizinleyicinizi adlandırmak için Sİzİn-HİzMET-İsİm'i hizmetinizin gerçek adı ile değiştirerek **PUT'u** ve aşağıdaki URL'yi kullanın.

   ```http
   https://[servicename].search.windows.net/indexers/cog-search-demo-idxr?api-version=2019-05-06
   ```

1. İstek **Gövdesi,** aşağıdaki JSON tanımını kopyalayın. Alan eşleme öğelerine dikkat edin; veri akışını tanımladıkları için bu eşlemeler önemlidir. 

   Beceriden `fieldMappings` önce işlenir ve bir dizindeki hedef alanlara veri kaynağından içerik gönderir. Varolan, değiştirilmemiş içeriği dizine göndermek için alan eşlemelerini kullanırsınız. Alan adları ve türleri her iki uçta da aynıysa, eşleme gerekmez.

   Beceriler `outputFieldMappings` tarafından oluşturulan alanlar içindir ve böylece skillset çalıştırıldıktan sonra işlenir. Belge çatlama `sourceFieldNames` `outputFieldMappings` veya zenginleştirme bunları oluşturana kadar başvurular yok. Dizin `targetFieldName` şemasında tanımlanan bir dizin alanıdır.

    ```json
    {
      "name":"cog-search-demo-idxr",    
      "dataSourceName" : "cog-search-demo-ds",
      "targetIndexName" : "cog-search-demo-idx",
      "skillsetName" : "cog-search-demo-ss",
      "fieldMappings" : [
        {
          "sourceFieldName" : "metadata_storage_path",
          "targetFieldName" : "id",
          "mappingFunction" :
            { "name" : "base64Encode" }
        },
        {
          "sourceFieldName" : "metadata_storage_name",
          "targetFieldName" : "metadata_storage_name",
          "mappingFunction" :
            { "name" : "base64Encode" }
        },
        {
          "sourceFieldName" : "content",
          "targetFieldName" : "content"
        }
      ],
      "outputFieldMappings" :
      [
        {
          "sourceFieldName" : "/document/persons",
          "targetFieldName" : "persons"
        },
        {
          "sourceFieldName" : "/document/organizations",
          "targetFieldName" : "organizations"
        },
        {
          "sourceFieldName" : "/document/locations",
          "targetFieldName" : "locations"
        },
        {
          "sourceFieldName" : "/document/pages/*/keyPhrases/*",
          "targetFieldName" : "keyPhrases"
        },
        {
          "sourceFieldName": "/document/languageCode",
          "targetFieldName": "languageCode"
        }
      ],
      "parameters":
      {
        "maxFailedItems":-1,
        "maxFailedItemsPerBatch":-1,
        "configuration":
        {
          "dataToExtract": "contentAndMetadata",
          "parsingMode": "default",
          "firstLineContainsHeaders": false,
          "delimitedTextDelimiter": ","
        }
      }
    }
    ```

1. İsteği gönderin. Postacı 201 başarılı işleme onaylayan bir durum kodu döndürmelidir. 

   Bu adımın tamamlanması birkaç dakika sürebilir. Veri kümesi küçük olsa da, analiz becerileri bilgi işlem açısından yoğundur. 

> [!NOTE]
> Bir dizin oluşturucu oluşturulduğunda, işlem hattı çağrılır. Verilere ulaşılırken, eşleme girişleri ve çıktıları veya işlemlerin sırası ile ilgili sorun olursa bunlar bu aşamada görüntülenir. İşlem hattını, kod veya betik değişiklikleriyle yeniden çalıştırmak için önce nesneleri bırakmanız gerekebilir. Daha fazla bilgi için bkz. [Sıfırlama ve yeniden çalıştırma](#reset).

#### <a name="about-indexer-parameters"></a>Dizinleyici parametreleri hakkında

Betik, ```"maxFailedItems"``` değerini -1 olarak ayarlayarak dizin oluşturma motoruna, veri içeri aktarma sırasında hataları yoksaymasını bildirir. Demo veri kaynağında çok az belge olduğundan bu kabul edilebilir. Daha büyük bir veri kaynağı için değeri, 0’dan daha büyük bir değere ayarlarsınız.

İfade, ```"dataToExtract":"contentAndMetadata"``` dizinleyiciye içeriği farklı dosya biçimlerinin yanı sıra her dosyayla ilgili meta verileri otomatik olarak ayıklamasını söyler. 

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metni ayıklamak için ```imageAction``` değerini ayarlayabilirsiniz. Yapılandırma, ```"imageAction":"generateNormalizedImages"``` OCR Beceri ve Metin Birleştirme Becerisi ile birlikte, dizinleyiciye resimlerden metin ayıklamasını (örneğin, trafik Durdur işaretinden "dur" sözcüğü) ayıklamasını ve içerik alanının bir parçası olarak gömmesini söyler. Bu davranış hem belgelerde gömülü olan görüntüler (örneğin, bir PDF’teki görüntü) hem de veri kaynağında bulunan görüntüler (örneğin, bir JPG dosyası) için geçerlidir.

## <a name="4---monitor-indexing"></a>4 - İzleme indeksleme

Dizin oluşturma ve zenginleştirme, Dizin Oluştur isteğini gönderir göndermez başlar. Tanımladığınız bilişsel becerilere bağlı olarak, dizin oluşturma biraz zaman alabilir. Dizin oluşturucunun halen çalışıp çalışmadığını öğrenmek için aşağıdaki isteği göndererek dizin oluşturucu durumunu denetleyin.

1. Dizinleyicinizi adlandırmak için HIZMET INsIadını hizmetingerçek adı ile değiştirerek **GET'i** ve aşağıdaki URL'yi kullanın.

   ```http
   https://[YOUR-SERVICE-NAME].search.windows.net/indexers/cog-search-demo-idxr/status?api-version=2019-05-06
   ```

1. Dizinleyicinin çalışıp çalışmadığını öğrenmek veya hata ve uyarı bilgilerini görüntülemek için yanıtı gözden geçirin.  

Ücretsiz katmanı kullanıyorsanız, aşağıdaki ileti beklenir: '"Belgenizden içerik veya meta veri ayıklayamadı. '32768' karakterleri için çıkarılan metin kesildi". Bu ileti, Serbest katmandaki blob dizin oluşturmanın[karakter ayıklamada 32K sınırı](search-limits-quotas-capacity.md#indexer-limits)olduğundan görüntülenir. Bu veri kümesinin daha yüksek katmanlarda ayarlanan iletilerini görmezsiniz. 

> [!NOTE]
> Uyarılar bazı senaryolarda yaygındır ve her zaman bir sorun göstermez. Örneğin, bir blob kapsayıcısı görüntü dosyaları içeriyorsa ve ardışık işlem, görüntülerin işlenmediğini belirten bir uyarı alırsınız.

## <a name="5---search"></a>5 - Arama

Artık yeni alanlar ve bilgiler oluşturduğunuza göre, bilişsel aramanın değerini anlamak için bazı sorgular çalıştıralım, çünkü tipik bir arama senaryosuyla ilgilidir.

Tüm belgenin tek `content` bir alana paketlendiği blob içeriğiyle başladığımızı hatırlayın. Bu alanda arama yapabilir ve sorgularınızın eşleşmelerini bulabilirsiniz.

1. **Bir** terim veya tümcecik örneğini aramak, `content` alanı döndürmek ve eşleşen belgelerin sayısını aramak için, HIZMET-AD'ı hizmetinizin gerçek adı ile değiştirerek GET ve aşağıdaki URL'yi kullanın.

   ```http
   https://[YOUR-SERVICE-NAME].search.windows.net/indexes/cog-search-demo-idx?search=*&$count=true&$select=content?api-version=2019-05-06
   ```
   
   Bu sorgu, bilişsel arama ardışık lığı olmadan blob dizinleyici kullanılırsa alacağınız aynı sonucu belge içeriğini döndürün. Bu alan aranabilir, ancak fason, filtre veya otomatik tamamlama kullanmak istiyorsanız kullanılamaz.

   ![İçerik alanı çıktısı](media/cognitive-search-tutorial-blob/content-output.png "İçerik alanı çıktısı")
   
1. İkinci sorgu için, ardışık hatlar tarafından oluşturulan yeni alanların bazılarını (kişiler, kuruluşlar, konumlar, languageCode) döndürün. Kısalık için anahtar Ifadeleri atlıyoruz, ancak bu değerleri görmek istiyorsanız eklemeniz gerekir.

   ```http
   https://mydemo.search.windows.net/indexes/cog-search-demo-idx/docs?search=*&$count=true&$select=metadata_storage_name,persons,organizations,locations,languageCode&api-version=2019-05-06
   ```
   $select bildirimindeki alanlar, Bilişsel Hizmetler'in doğal dil işleme yeteneklerinden oluşturulan yeni bilgiler içerir. Tahmin edebileceğiniz gibi, sonuçlarda bazı gürültü ler ve belgeler arasında farklılıklar vardır, ancak birçok durumda analitik modeller doğru sonuçlar üretir.

   Aşağıdaki resim, Microsoft'ta CEO rolünü üstlenerek Satya Nadella'nın açık mektubunun sonuçlarını göstermektedir.

   ![Boru hattı çıkışı](media/cognitive-search-tutorial-blob/pipeline-output.png "Boru hattı çıkışı")

1. Bu alanlardan nasıl yararlanabileceğinizi görmek için, eşleşen belgelerin konuma göre bir agregasyonunu döndürmek için bir yönlü parametre ekleyin.

   ```http
   https://[YOUR-SERVICE-NAME].search.windows.net/indexes/cog-search-demo-idx/docs?search=*&facet=locations&api-version=2019-05-06
   ``` 

   Bu örnekte, her konum için 2 veya 3 eşleşme vardır.

   ![Fason çıktı](media/cognitive-search-tutorial-blob/facet-output.png "Fason çıktı")
   

1. Bu son örnekte, NASDAQ'ı temel alan filtre ölçütleri için iki eşleşme döndürerek kuruluşlar koleksiyonuna bir filtre uygulayın.

   ```http
   cog-search-demo-idx/docs?search=*&$filter=organizations/any(organizations: organizations eq 'NASDAQ')&$select=metadata_storage_name,organizations&$count=true&api-version=2019-05-06
   ```

Bu sorgular, bilişsel arama tarafından oluşturulan yeni alanlarda sorgu sözdizimi ve filtreleri ile çalışabilirsiniz yollarından birkaçını göstermektedir. Daha fazla sorgu örneği için bkz: [Arama Belgeleri NDEKI Örnekler REST API,](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples)Basit [sözdizimi sorgu örnekleri](search-query-simple-examples.md)ve Tam [Lucene sorgu örnekleri.](search-query-lucene-examples.md)

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmenin ilk deneysel aşamalarında, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama'dan silmek ve kodunuzu yeniden oluşturmasına izin vermektir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Portalı dizinleri, dizinleyicileri, veri kaynaklarını ve beceri kümelerini silmek için kullanabilirsiniz. Dizinleyiciyi sildiğinizde, isteğe bağlı olarak dizin, skillset ve veri kaynağını aynı anda seçilen silebilirsiniz.

![Arama nesnelerini silme](./media/cognitive-search-tutorial-blob-python/py-delete-indexer-delete-all.png "Portaldaki arama nesnelerini silme")

Veya **DELETE'i** kullanın ve her nesneye URL sağlayın. Aşağıdaki komut bir dizinleyicisi siler.

```http
DELETE https://[YOUR-SERVICE-NAME].search.windows.net/indexers/cog-search-demo-idxr?api-version=2019-05-06
```

Silme işlemi başarılı olduğunda durum kodu 204 döndürülür.

## <a name="takeaways"></a>Paketler

Bu öğreticide, veri kaynağı, beceri kümesi, dizin ve dizin oluşturucu gibi bileşen parçalarının oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma işlem hattı oluşturmaya yönelik temel adımlar gösterilmektedir.

[Yerleşik beceriler,](cognitive-search-predefined-skills.md) skillset tanımı ve zincirleme becerilerin mekaniği ile birlikte giriş ve çıktılar yoluyla bir araya getirilmiştir. Ayrıca, dizin leyici `outputFieldMappings` tanımında, zenginleştirilmiş değerleri ardışık kaynaktan azure bilişsel arama hizmetinde aranabilir bir dizine yönlendirmenin gerekli olduğunu da öğrendiniz.

Son olarak, daha fazla yineleme için sonuçların nasıl test edileceğini ve sistemin nasıl sıfırlanacağını öğrendiniz. Dizine karşı sorgular düzenlendiğinde, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktının döndürüldüğünü öğrendiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda, artık ihtiyacınız olmayan kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki Tüm kaynaklar veya Kaynak grupları bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir AI zenginleştirme boru hattındaki tüm nesnelere aşina olduğunuza göre, beceri tanımlarına ve bireysel becerilere daha yakından bakalım.

> [!div class="nextstepaction"]
> [Nasıl bir skillset oluşturmak için](cognitive-search-defining-skillset.md)