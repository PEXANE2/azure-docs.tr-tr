---
title: 'Öğretici: JSON blobs dizin yarı yapılandırılmış veri'
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama REST API'leri ve Postacı'yı kullanarak yarı yapılandırılmış Azure JSON bloblarını nasıl dizine ekleyip arayacağınızı öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/28/2020
ms.openlocfilehash: ce3b3839319de38020b968ff8db1ee6713b29c47
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78269971"
---
# <a name="tutorial-index-json-blobs-from-azure-storage-using-rest"></a>Öğretici: REST kullanarak Azure Depolama'dan Dizin JSON blobs

Azure Bilişsel Arama, yarı yapılandırılmış verileri okumayı bilen bir [dizinleyici](search-indexer-overview.md) kullanarak Azure blob depolamasındaJSON belgelerini ve dizilerini dizilere dizinleyebilir. Yarı yapılandırılmış veriler, veriler içindeki içeriği ayıran etiketleri veya işaretleri içerir. Tam olarak dizine ekili olması gereken yapılandırılmamış verilerle alan başına dizine ekilebilen ilişkisel veritabanı şeması gibi bir veri modeline uyan resmi olarak yapılandırılmış veriler arasındaki farkı böler.

Bu öğretici, aşağıdaki görevleri gerçekleştirmek için Postacı ve [Arama REST API'larını](https://docs.microsoft.com/rest/api/searchservice/) kullanır:

> [!div class="checklist"]
> * Azure blob kapsayıcısı için Azure Bilişsel Arama veri kaynağını yapılandırma
> * Aranabilir içerik leri içerecek şekilde Azure Bilişsel Arama dizini oluşturma
> * Kapsayıcıyı okumak ve Azure blob depolamadan aranabilir içeriği ayıklamak için bir dizin leyiciyi yapılandırma ve çalıştırma
> * Oluşturduğunuz dizini arama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

+ [Azure Depolama](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
+ [Postman masaüstü uygulaması](https://www.getpostman.com/)
+ [Varolan bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturma](search-create-service-portal.md) veya bulma 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz bir arama hizmeti sizi üç dizin, üç dizin leyici ve üç veri kaynağıyla sınırlar. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, yeni kaynakları kabul etmek için hizmetinizde yer olduğundan emin olun.

## <a name="download-files"></a>Dosyaları indirme

[Klinik-denemeler-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip) bu öğreticikullanılan verileri içerir. Bu dosyayı kendi klasörüne indirin ve açın. Veriler, bu öğretici için JSON'a dönüştürülen [clinicaltrials.gov'dan](https://clinicaltrials.gov/ct2/results)kaynaklanır.

## <a name="1---create-services"></a>1 - Hizmet oluşturma

Bu öğretici, verileri sağlamak için dizin oluşturma ve sorgular için Azure Bilişsel Arama ve Azure Blob depolamasını kullanır. 

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

1. Örnek verileri içerecek [bir Blob kapsayıcısı oluşturun.](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) Genel Erişim Düzeyini geçerli değerlerinden herhangi biri için ayarlayabilirsiniz.

1. Kapsayıcı oluşturulduktan sonra açın ve komut çubuğunda **Yükle'yi** seçin.

   ![Komut çubuğuna yükleme](media/search-semi-structured-data/upload-command-bar.png "Komut çubuğuna yükleme")

1. Örnek dosyaları içeren klasöre gidin. Hepsini seçin ve sonra **Yükle'yi**tıklatın.

   ![Dosyaları karşıya yükleme](media/search-semi-structured-data/clinicalupload.png "Dosyaları karşıya yükleme")

Yükleme tamamlandıktan sonra dosyalar veri kapsayıcısında kendi alt klasöründe görünmelidir.

### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Bir sonraki kaynak, [portalda oluşturabileceğiniz](search-create-service-portal.md)Azure Bilişsel Arama'dır. Bu izbiyi tamamlamak için Ücretsiz katmanı kullanabilirsiniz. 

Azure Blob depolamada olduğu gibi, erişim anahtarını toplamak için bir dakikaayırın. Ayrıca, istekleri yapılandırmaya başladığınızda, her isteğin kimliğini doğrulamak için kullanılan bitiş noktası ve yönetici api anahtarını sağlamanız gerekir.

### <a name="get-a-key-and-url"></a>Bir anahtar ve URL alın

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

![Http bitiş noktası ve erişim anahtarı alın](media/search-get-started-postman/get-url-key.png "Http bitiş noktası ve erişim anahtarı alın")

Tüm istekler, hizmetinize gönderilen her istekiçin bir api anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="2---set-up-postman"></a>2 - Postacı kurmak

Postman’i başlatın ve bir HTTP isteği ayarlayın. Bu araca aşina değilseniz, [Postacı kullanarak Azure Bilişsel Arama REST API'lerini keşfedin'e](search-get-started-postman.md)bakın.

Bu öğretici her arama için istek yöntemleri **POST** ve **GET**vardır. Bir veri kaynağı, dizin ve dizin oluşturabilirsiniz. Veri kaynağı, depolama hesabınıza ve JSON verilerinize yönelik bir işaretçi içerir. Arama hizmetiniz, veriler yüklenirken bağlantı kurar.

Üstbilgide, Azure Bilişsel Arama `application/json` hizmetinizin yönetici api anahtarına "İçerik türü" ayarlayın ve ayarlayın. `api-key` Üstbilgileri ayarladıktan sonra, bunları bu alıştırmadaki her istek için kullanabilirsiniz.

  ![Postacı istek URL'si ve üstbilgi](media/search-get-started-postman/postman-url.png "Postacı istek URL'si ve üstbilgi")

URI'ler bir api sürümü belirtmeli ve her çağrı **201 Oluşturulan'ı**döndürmelidir. JSON dizileri kullanmak için genellikle kullanılabilir api-sürümü. `2019-05-06`

## <a name="3---create-a-data-source"></a>3 - Veri kaynağı oluşturma

[Veri Kaynağı Oluştur API'si,](https://docs.microsoft.com/rest/api/searchservice/create-data-source) hangi verileri dizinlendirecek verileri belirten bir Azure Bilişsel Arama nesnesi oluşturur.

1. Bu aramanın bitiş noktasını `https://[service name].search.windows.net/datasources?api-version=2019-05-06`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin. 

1. Aşağıdaki JSON'u istek gövdesine kopyalayın.

    ```json
    {
        "name" : "clinical-trials-json-ds",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
        "container" : { "name" : "[blob container name]"}
    }
    ```

1. Bağlantı dizesini hesabınız için geçerli bir dizeyle değiştirin.

1. Örnek veriler için oluşturduğunuz kapsayıcıyla "[blob kapsayıcı adı]" 'nı değiştirin. 

1. İsteği gönderin. Yanıt şöyle görünmelidir:

    ```json
    {
        "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
        "@odata.etag": "\"0x8D505FBC3856C9E\"",
        "name": "clinical-trials-json-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
        },
        "container": {
            "name": "[mycontainernamehere]",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

## <a name="4---create-an-index"></a>4 - Bir dizin oluşturma
    
İkinci çağrı, tüm aranabilir verileri depolayan bir Azure Bilişsel Arama dizini oluşturarak [Dizin API](https://docs.microsoft.com/rest/api/searchservice/create-index)Oluştur'dur. Dizin, tüm parametreleri ve parametrelerin özniteliklerini belirtir.

1. Bu aramanın bitiş noktasını `https://[service name].search.windows.net/indexes?api-version=2019-05-06`. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

1. Aşağıdaki JSON'u istek gövdesine kopyalayın.

    ```json
    {
      "name": "clinical-trials-json-index",  
      "fields": [
      {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
      {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
      {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
      {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
      {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
      {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
      {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
      {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
      {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
      {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
      {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
      {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
      {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
      {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
      {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
      {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
      {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
      {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
      {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
      ]
    }
   ```

1. İsteği gönderin. Yanıt şöyle görünmelidir:

    ```json
    {
        "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
        "@odata.etag": "\"0x8D505FC00EDD5FA\"",
        "name": "clinical-trials-json-index",
        "fields": [
            {
                "name": "FileName",
                "type": "Edm.String",
                "searchable": false,
                "filterable": false,
                "retrievable": true,
                "sortable": true,
                "facetable": false,
                "key": false,
                "indexAnalyzer": null,
                "searchAnalyzer": null,
                "analyzer": null,
                "synonymMaps": []
            },
            {
                "name": "Description",
                "type": "Edm.String",
                "searchable": true,
                "filterable": false,
                "retrievable": false,
                "sortable": false,
                "facetable": false,
                "key": false,
                "indexAnalyzer": null,
                "searchAnalyzer": null,
                "analyzer": null,
                "synonymMaps": []
            },
            ...
          }
    ```

## <a name="5---create-and-run-an-indexer"></a>5 - Bir dizin oluştur ve çalıştır

Dizinleyici veri kaynağına bağlanır, verileri hedef arama dizinine aktarAr ve isteğe bağlı olarak veri yenilemesini otomatikleştirmek için bir zamanlama sağlar. REST API [Dizin Oluştur.](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

1. Bu arama için URI'yi `https://[service name].search.windows.net/indexers?api-version=2019-05-06`ayarla. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

1. Aşağıdaki JSON'u istek gövdesine kopyalayın.

    ```json
    {
      "name" : "clinical-trials-json-indexer",
      "dataSourceName" : "clinical-trials-json-ds",
      "targetIndexName" : "clinical-trials-json-index",
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }
    ```

1. İsteği gönderin. İstek hemen işlenir. Yanıt geri geldiğinde, tam metin aranabilir bir dizin olacaktır. Yanıt şöyle görünmelidir:

    ```json
    {
        "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
        "@odata.etag": "\"0x8D505FDE143D164\"",
        "name": "clinical-trials-json-indexer",
        "description": null,
        "dataSourceName": "clinical-trials-json-ds",
        "targetIndexName": "clinical-trials-json-index",
        "schedule": null,
        "parameters": {
            "batchSize": null,
            "maxFailedItems": null,
            "maxFailedItemsPerBatch": null,
            "base64EncodeKeys": null,
            "configuration": {
                "parsingMode": "jsonArray"
            }
        },
        "fieldMappings": [],
        "enrichers": [],
        "disabled": null
    }
    ```

## <a name="6---search-your-json-files"></a>6 - JSON dosyalarınızda arama yapın

İlk belge yüklenir yüklenmez aramaya başlayabilirsiniz.

1. Fiili **GET**olarak değiştirin.

1. Bu arama için URI'yi `https://[service name].search.windows.net/indexes/clinical-trials-json-index/docs?search=*&api-version=2019-05-06&$count=true`ayarla. `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

1. İsteği gönderin. Bu, diziniçinde alınabilecek olarak işaretlenmiş tüm alanları ve belge sayısını döndüren belirtilmemiş bir tam metin arama sorgusudur. Yanıt şöyle görünmelidir:

    ```json
    {
        "@odata.context": "https://exampleurl.search.windows.net/indexes('clinical-trials-json-index')/$metadata#docs(*)",
        "@odata.count": 100,
        "value": [
            {
                "@search.score": 1.0,
                "FileName": "NCT00000102.txt",
                "MinimumAge": 14,
                "Title": "Congenital Adrenal Hyperplasia: Calcium Channels as Therapeutic Targets",
                "MyURL": "https://azure.storagedemos.com/clinical-trials/NCT00000102.txt",
                "Gender": "Both",
                "MaximumAge": 35,
                "Summary": "This study will test the ability of extended release nifedipine (Procardia XL), a blood pressure medication, to permit a decrease in the dose of glucocorticoid medication children take to treat congenital adrenal hyperplasia (CAH).",
                "NCTID": "NCT00000102",
                "Phase": "Phase 1/Phase 2",
                "Date": "ClinicalTrials.gov processed this data on October 25, 2016",
                "OverallStatus": "Completed",
                "OrgStudyId": "NCRR-M01RR01070-0506",
                "HealthyVolunteers": "No",
                "Keywords": [],
                "metadata_storage_last_modified": "2019-04-09T18:16:24Z",
                "metadata_storage_size": "33060",
                "metadata_content_type": null
            },
            . . . 
    ```

1. Sonuçları `$select` daha az alanla sınırlamak için `https://[service name].search.windows.net/indexes/clinical-trials-json-index/docs?search=*&$select=Gender,metadata_storage_size&api-version=2019-05-06&$count=true`sorgu parametresini ekleyin: .  Bu sorgu için 100 belge eşleşir, ancak varsayılan olarak Azure Bilişsel Arama sonuçlarda yalnızca 50 belge döndürür.

   ![Parametre tabanlı sorgu](media/search-semi-structured-data/lastquery.png "Paramterized sorgu")

1. Daha karmaşık bir sorgu `$filter=MinimumAge ge 30 and MaximumAge lt 75`örneği, yalnızca Minimum Yaş parametrelerinin 30'dan büyük veya eşit olduğu ve Maksimum Yaş'ın 75'ten az olduğu sonuçları döndüren bir örnek içerir. İfadeyi `$select` ifadeyle `$filter` değiştirin.

   ![Yarı yapılandırılmış arama](media/search-semi-structured-data/metadatashort.png)

Ayrıca Mantıksal işleçleri (ve veya, değil) ve karşılaştırma işleçleri (eq, ne, gt, lt, ge, le) kullanabilirsiniz. Dize karşılaştırmaları büyük/küçük harfe duyarlıdır. Daha fazla bilgi ve örnek için [bkz.](search-query-simple-examples.md)

> [!NOTE]
> `$filter` parametresi yalnızca dizininiz oluşturulurken filtrelenebilir olarak işaretlenmiş olan meta verilerle birlikte çalışır.

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmenin ilk deneysel aşamalarında, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama'dan silmek ve kodunuzu yeniden oluşturmasına izin vermektir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Portalı dizinleri, dizinleyicileri ve veri kaynaklarını silmek için kullanabilirsiniz. Veya **DELETE'i** kullanın ve her nesneye URL sağlayın. Aşağıdaki komut bir dizinleyicisi siler.

```http
DELETE https://[YOUR-SERVICE-NAME].search.windows.net/indexers/clinical-trials-json-indexer?api-version=2019-05-06
```

Silme işlemi başarılı olduğunda durum kodu 204 döndürülür.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda, artık ihtiyacınız olmayan kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki Tüm kaynaklar veya Kaynak grupları bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Blob dizin oluşturmanın temellerini bildiğinize göre, Azure Depolama'daki JSON blob'ları için dizin oluşturma yapılandırmasına daha yakından bakalım.

> [!div class="nextstepaction"]
> [JSON blob dizini yapılandırma](search-howto-index-json-blobs.md)