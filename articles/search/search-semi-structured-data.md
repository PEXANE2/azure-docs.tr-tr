---
title: 'Öğretici: JSON Bloblarında yarı yapılandırılmış verilerin dizinini oluştur'
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama REST API 'Leri ve Postman kullanarak yarı yapılandırılmış Azure JSON bloblarını dizin oluşturma ve arama hakkında bilgi edinin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 06/23/2020
ms.openlocfilehash: 0e6759837519feccf6069e805e3fe0f72562fb7b
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "85559012"
---
# <a name="tutorial-index-json-blobs-from-azure-storage-using-rest"></a>Öğretici: REST kullanarak Azure Storage 'dan JSON bloblarını dizine

Azure Bilişsel Arama, yarı yapılandırılmış verilerin nasıl okunacağını bilen bir [Dizin Oluşturucu](search-indexer-overview.md) kullanarak Azure Blob depolamada JSON belgelerini ve dizilerini dizinedebilir. Yarı yapılandırılmış veriler, veriler içindeki içeriği ayıran etiketleri veya işaretleri içerir. Tam olarak dizin oluşturulması gereken yapılandırılmamış veriler arasındaki farkı ve bir ilişkisel veritabanı şeması gibi bir veri modeline bağlı olan, tek başına yapılandırılmış verileri alan temelinde dizinlenebilir şekilde ayırır.

Bu öğreticide, aşağıdaki görevleri gerçekleştirmek için Postman ve [arama REST API 'leri](https://docs.microsoft.com/rest/api/searchservice/) kullanılmaktadır:

> [!div class="checklist"]
> * Azure Blob kapsayıcısı için Azure Bilişsel Arama veri kaynağı yapılandırma
> * Aranabilir içerik içeren bir Azure Bilişsel Arama dizini oluşturma
> * Kapsayıcıyı okumak ve Azure Blob depolama alanından aranabilir içeriği ayıklamak için bir dizin oluşturucuyu yapılandırma ve çalıştırma
> * Oluşturduğunuz dizini arama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

+ [Azure Depolama](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
+ [Postman masaüstü uygulaması](https://www.getpostman.com/)
+ [Mevcut bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturun](search-create-service-portal.md) veya bulun 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz arama hizmeti, sizi üç Dizin, üç Dizin Oluşturucu ve üç veri kaynağı ile sınırlandırır. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, hizmetinize yeni kaynakları kabul etmek için yeriniz olduğundan emin olun.

## <a name="download-files"></a>Dosyaları indirme

[Clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip) Bu öğreticide kullanılan verileri içerir. Bu dosyayı kendi klasörüne indirip sıkıştırmasını açın. Veriler, bu öğretici için JSON 'a dönüştürüldü, [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results)'tan kaynaklanır.

## <a name="1---create-services"></a>1-hizmet oluşturma

Bu öğretici, verileri sağlamak üzere dizin oluşturma ve sorgular ve Azure Blob depolama için Azure Bilişsel Arama kullanır. 

Mümkünse, yakınlık ve yönetilebilirlik için aynı bölgede ve kaynak grubunda her ikisini de oluşturun. Uygulamada, Azure depolama hesabınız herhangi bir bölgede olabilir.

### <a name="start-with-azure-storage"></a>Azure Storage 'ı kullanmaya başlama

1. [Azure Portal oturum açın](https://portal.azure.com/) ve **+ kaynak oluştur**' a tıklayın.

1. *Depolama hesabı* araması yapın ve Microsoft 'un depolama hesabı teklifi ' ni seçin.

   ![Depolama hesabı oluştur](media/cognitive-search-tutorial-blob/storage-account.png "Depolama hesabı oluştur")

1. Temel bilgiler sekmesinde, aşağıdaki öğeler gereklidir. Diğer her şey için varsayılanları kabul edin.

   + **Kaynak grubu**. Mevcut bir tane seçin veya yeni bir tane oluşturun, ancak bunları topluca yönetebilmeniz için tüm hizmetler için aynı grubu kullanın.

   + **Depolama hesabı adı**. Aynı türde birden fazla kaynağınız olabileceğini düşünüyorsanız, tür ve bölgeye göre belirsizliği ortadan kaldırmak için adı kullanın, örneğin *blobstoragewestus*. 

   + **Konum**. Mümkünse, Azure Bilişsel Arama ve bilişsel hizmetler için kullanılan aynı konumu seçin. Tek bir konum, bant genişliği ücretlerini oylar.

   + **Hesap türü**. Varsayılan, *StorageV2 (genel amaçlı v2)* seçeneğini belirleyin.

1. Hizmeti oluşturmak için **gözden geçir + oluştur** ' a tıklayın.

1. Oluşturulduktan sonra genel bakış sayfasını açmak için **Kaynağa Git** ' e tıklayın.

1. **Bloblar** hizmeti ' ne tıklayın.

1. Örnek veri içeren [bir blob kapsayıcısı oluşturun](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) . Ortak erişim düzeyini geçerli değerlerinden herhangi birine ayarlayabilirsiniz.

1. Kapsayıcı oluşturulduktan sonra açın ve komut çubuğunda **karşıya yükle** ' yi seçin.

   ![Komut çubuğuna yükle](media/search-semi-structured-data/upload-command-bar.png "Komut çubuğuna yükle")

1. Örnek dosyaları içeren klasöre gidin. Tümünü seçip **karşıya yükle**' ye tıklayın.

   ![Dosyaları karşıya yükleme](media/search-semi-structured-data/clinicalupload.png "Dosyaları karşıya yükleme")

Yükleme tamamlandıktan sonra dosyalar veri kapsayıcısında kendi alt klasöründe görünmelidir.

### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Sonraki kaynak, [portalda oluşturabileceğiniz](search-create-service-portal.md)Azure bilişsel arama. Bu izlenecek yolu tamamlamak için ücretsiz katmanı kullanabilirsiniz. 

Azure Blob depolamada olduğu gibi, erişim anahtarını toplamak için biraz zaman ayırın. Ayrıca, istekleri raporlamaya başladığınızda, her bir isteğin kimliğini doğrulamak için kullanılan uç nokta ve yönetici API 'si anahtarını sağlamanız gerekir.

### <a name="get-a-key-and-url"></a>Anahtar ve URL al

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar**  >  **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

![HTTP uç noktası ve erişim anahtarı al](media/search-get-started-postman/get-url-key.png "HTTP uç noktası ve erişim anahtarı al")

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="2---set-up-postman"></a>2-Postman 'ı ayarlama

Postman’i başlatın ve bir HTTP isteği ayarlayın. Bu aracı hakkında bilginiz varsa bkz. [Postman kullanarak Azure BILIŞSEL arama REST API 'Lerini araştırma](search-get-started-postman.md).

Bu öğreticideki her çağrının istek yöntemleri **gönderi** ve **Get**' dir. Bir veri kaynağı, dizin ve Dizin Oluşturucu oluşturmak için arama hizmetinize üç API çağrısı yaparsınız. Veri kaynağı, depolama hesabınıza ve JSON verilerinize yönelik bir işaretçi içerir. Arama hizmetiniz, veriler yüklenirken bağlantı kurar.

Üst bilgiler ' de, "Content-Type" olarak ayarlayın `application/json` ve `api-key` Azure bilişsel arama hizmetinizin yönetim API anahtarı olarak ayarlayın. Üst bilgileri ayarladıktan sonra bu alýþtýrmadaki her istek için kullanabilirsiniz.

  ![Postman istek URL 'SI ve üstbilgisi](media/search-get-started-postman/postman-url.png "Postman istek URL 'SI ve üstbilgisi")

URI 'Ler bir api sürümü belirtmeli ve her çağrının **oluşturulan bir 201**döndürmesi gerekir. JSON dizilerini kullanmak için genel olarak kullanılabilen api-Version vardır `2020-06-30` .

## <a name="3---create-a-data-source"></a>3-veri kaynağı oluşturma

[Veri kaynağı oluşturma API 'si](https://docs.microsoft.com/rest/api/searchservice/create-data-source) , hangi verilerin dizine oluşturulacağını belirten bir Azure bilişsel arama nesnesi oluşturur.

1. Bu çağrının uç noktasını olarak ayarlayın `https://[service name].search.windows.net/datasources?api-version=2020-06-30` . `[service name]` değerini, arama hizmetinizin adıyla değiştirin. 

1. Aşağıdaki JSON öğesini istek gövdesine kopyalayın.

    ```json
    {
        "name" : "clinical-trials-json-ds",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
        "container" : { "name" : "[blob container name]"}
    }
    ```

1. Bağlantı dizesini, hesabınız için geçerli bir dize ile değiştirin.

1. "[Blob Container Name]" değerini, örnek veriler için oluşturduğunuz kapsayıcı ile değiştirin. 

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

## <a name="4---create-an-index"></a>4-dizin oluşturma
    
İkinci çağrı, tüm aranabilir verileri depolayan bir Azure Bilişsel Arama dizini oluşturan [Dizin API 'Si oluşturur](https://docs.microsoft.com/rest/api/searchservice/create-index). Dizin, tüm parametreleri ve parametrelerin özniteliklerini belirtir.

1. Bu çağrının uç noktasını olarak ayarlayın `https://[service name].search.windows.net/indexes?api-version=2020-06-30` . `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

1. Aşağıdaki JSON öğesini istek gövdesine kopyalayın.

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

## <a name="5---create-and-run-an-indexer"></a>5-Dizin Oluşturucu oluşturma ve çalıştırma

Bir dizin oluşturucu veri kaynağına bağlanır, verileri hedef arama dizinine aktarır ve isteğe bağlı olarak veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar. REST API [Dizin Oluşturucu oluşturur](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

1. Bu çağrının URI 'sini ayarlayın `https://[service name].search.windows.net/indexers?api-version=2020-06-30` . `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

1. Aşağıdaki JSON öğesini istek gövdesine kopyalayın.

    ```json
    {
      "name" : "clinical-trials-json-indexer",
      "dataSourceName" : "clinical-trials-json-ds",
      "targetIndexName" : "clinical-trials-json-index",
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }
    ```

1. İsteği gönderin. İstek hemen işlenir. Yanıt geri geldiğinde, tam metin ile aranabilir bir dizininiz olur. Yanıt şöyle görünmelidir:

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

## <a name="6---search-your-json-files"></a>6-JSON dosyalarınızda arama yapın

İlk belge yüklendikten hemen sonra aramaya başlayabilirsiniz.

1. **Alınacak**fiili ' i değiştirin.

1. Bu çağrının URI 'sini ayarlayın `https://[service name].search.windows.net/indexes/clinical-trials-json-index/docs?search=*&api-version=2020-06-30&$count=true` . `[service name]` değerini, arama hizmetinizin adıyla değiştirin.

1. İsteği gönderin. Bu, bir belge sayısıyla birlikte, dizinde alınabilir olarak işaretlenmiş tüm alanları döndüren belirtilmemiş bir tam metin arama sorgusudur. Yanıt şöyle görünmelidir:

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

1. `$select`Sonuçları daha az alanlarla sınırlamak için sorgu parametresini ekleyin: `https://[service name].search.windows.net/indexes/clinical-trials-json-index/docs?search=*&$select=Gender,metadata_storage_size&api-version=2020-06-30&$count=true` .  Bu sorgu için 100 belge eşleşir, ancak varsayılan olarak Azure Bilişsel Arama yalnızca sonuçlarda 50 ' i döndürür.

   ![Parametre tabanlı sorgu](media/search-semi-structured-data/lastquery.png "Paramlanmış sorgu")

1. Daha karmaşık sorguya örnek `$filter=MinimumAge ge 30 and MaximumAge lt 75` olarak, yalnızca en az bir parametre en az 30 ' a eşit veya daha büyük ve en az umage 75 ' den küçük olan sonuçları döndürür. `$select`İfadeyi `$filter` ifadesiyle değiştirin.

   ![Yarı yapılandırılmış arama](media/search-semi-structured-data/metadatashort.png)

Ayrıca, mantıksal işleçleri (ve, veya değil) ve karşılaştırma işleçlerini (EQ, ne, gt, lt, GE, Le) de kullanabilirsiniz. Dize karşılaştırmaları büyük/küçük harfe duyarlıdır. Daha fazla bilgi ve örnek için bkz. [basit sorgu oluşturma](search-query-simple-examples.md).

> [!NOTE]
> `$filter` parametresi yalnızca dizininiz oluşturulurken filtrelenebilir olarak işaretlenmiş olan meta verilerle birlikte çalışır.

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmede erken deneysel aşamalarda, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama silmek ve kodunuzun bunları yeniden oluşturması için izin verir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Dizinleri, Dizin oluşturucuyu ve veri kaynaklarını silmek için portalı kullanabilirsiniz. Ya da **Sil** ' i kullanın ve her bir nesnenin URL 'sini sağlayın. Aşağıdaki komut bir dizin oluşturucuyu siler.

```http
DELETE https://[YOUR-SERVICE-NAME].search.windows.net/indexers/clinical-trials-json-indexer?api-version=2020-06-30
```

Silme işlemi başarılı olduğunda durum kodu 204 döndürülür.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir projenin sonunda kendi aboneliğinizde çalışırken, artık ihtiyaç duyulmadığınızda kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki tüm kaynaklar veya kaynak grupları bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Blob dizin oluşturma hakkındaki temel bilgileri öğrenmiş olduğunuza göre, Azure Storage 'da JSON Blobları için Dizin Oluşturucu yapılandırmasına daha yakından bakalım.

> [!div class="nextstepaction"]
> [JSON blob Dizin oluşturmayı yapılandırma](search-howto-index-json-blobs.md)