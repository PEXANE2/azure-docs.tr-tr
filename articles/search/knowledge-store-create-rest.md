---
title: REST'i kullanarak bir bilgi deposu (önizleme) oluşturma
titleSuffix: Azure Cognitive Search
description: Bir AI zenginleştirme ardışık ardışık ardışık kalıcı zenginleştirmeler için bir Azure Bilişsel Arama bilgi deposu oluşturmak için REST API ve Postman kullanın. Bu özellik şu anda genel önizlemede dir.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 12/30/2019
ms.openlocfilehash: 478a7e03b432006b429c96e03307fd8e494c88ff
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77472324"
---
# <a name="create-a-knowledge-store-using-rest-and-postman"></a>REST ve Postacı kullanarak bir bilgi deposu oluşturun

> [!IMPORTANT] 
> Bilgi deposu şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) önizleme özellikleri sağlar. Şu anda sınırlı portal desteği ve .NET SDK desteği yoktur.

Bilgi deposu, daha sonraki analizler veya diğer alt akış işlemleri için Azure Bilişsel Arama zenginleştirme ardışık bölümünden çıktı içerir. AI ile zenginleştirilmiş bir ardışık işlem, görüntü dosyalarını veya yapılandırılmamış metin dosyalarını kabul eder, Azure Bilişsel Arama'yı kullanarak bunları dizine verir, Bilişsel Hizmetler'den (görüntü analizi ve doğal dil işleme gibi) AI zenginleştirmeleri uygular ve sonuçları bilgi deposu azure depolama. Bilgi deposunu keşfetmek için Azure portalında Power BI veya Storage Explorer gibi araçları kullanabilirsiniz.

Bu makalede, bir dizi otel incelemesine AI zenginleştirmelerini yutmak, dizine dizilemek ve uygulamak için REST API arabirimini kullanırsınız. Otel incelemeleri Azure Blob depolama alanına aktarılır. Sonuçlar Azure Tablo depolama alanında bilgi deposu olarak kaydedilir.

Bilgi deposunu oluşturduktan sonra, [Depolama Gezgini](knowledge-store-view-storage-explorer.md) veya [Power BI'yi](knowledge-store-connect-power-bi.md)kullanarak bilgi deposuna nasıl erişebileceğinizi öğrenebilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

> [!TIP]
> Bu makale için [Postacı masaüstü uygulamasını](https://www.getpostman.com/) öneririz. Bu makalenin [kaynak kodu,](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/knowledge-store) tüm istekleri içeren bir Postacı koleksiyonu içerir. 

## <a name="create-services-and-load-data"></a>Hizmet oluşturma ve veri yükleme

Bu hızlı başlangıç, AI için Azure Bilişsel Arama, Azure Blob depolama ve [Azure Bilişsel Hizmetleri'ni](https://azure.microsoft.com/services/cognitive-services/) kullanır. 

İş yükü çok küçük olduğundan, Bilişsel Hizmetler günde en fazla 20 işlem için ücretsiz işleme sağlamak için arka planda dokunulmaktadır. Veri kümesi çok küçük olduğundan, Bilişsel Hizmetler kaynağı oluşturmayı veya eklemeyi atlayabilirsiniz.

1. [Karşıdan HotelReviews_Free.csv .](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?sp=r&st=2019-11-04T01:23:53Z&se=2025-11-04T16:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=siQgWOnI%2FDamhwOgxmj11qwBqqtKMaztQKFNqWx00AY%3D) Bu veriler, bir CSV dosyasına kaydedilen (Kaggle.com kaynaklı) otel inceleme verileridir ve tek bir otel hakkında 19 müşteri geri bildirimi içerir. 

1. [Bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) veya geçerli aboneliğiniz altında [varolan bir hesap bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) Azure depolama alanını hem alınacak ham içerik hem de sonuçta elde edilecek bilgi deposu için kullanırsınız.

   **StorageV2 (genel amaçlı V2)** hesap türünü seçin.

1. Blob hizmetleri sayfalarını açın ve *otel incelemeleri*adlı bir konteyner oluşturun.

1. **Karşıya Yükle**'ye tıklayın.

    ![Verileri yükleyin](media/knowledge-store-create-portal/upload-command-bar.png "Otel yorumlarını yükleyin")

1. İlk adımda indirdiğiniz **HotelReviews-Free.csv** dosyasını seçin.

    ![Azure Blob kapsayıcısını oluşturma](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Azure Blob kapsayıcısını oluşturma")

1. Bu kaynakla neredeyse bitirdiniz, ancak bu sayfaları terk etmeden **önce, Erişim Tuşları** sayfasını açmak için sol gezinti bölmesindeki bir bağlantıyı kullanın. Blob depolamadan veri almak için bir bağlantı dizesi alın. Bağlantı dizesi aşağıdaki örneğe benzer:`DefaultEndpointsProtocol=https;AccountName=<YOUR-ACCOUNT-NAME>;AccountKey=<YOUR-ACCOUNT-KEY>;EndpointSuffix=core.windows.net`

1. Hala portalda, Azure Bilişsel Arama'ya geçin. [Yeni bir hizmet oluşturun](search-create-service-portal.md) veya [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu egzersiz için ücretsiz bir hizmet kullanabilirsiniz.

## <a name="configure-postman"></a>Postman'i yapılandırma

Postacı'yı yükleyin ve kurun.

### <a name="download-and-install-postman"></a>Postacı'yı indirin ve kurun

1. [Postacı toplama kaynak kodunu](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json)indirin.
1. Kaynak kodu Postacı'ya almak için **Dosya** > **İçe Aktarma'yı** seçin.
1. **Koleksiyonlar** sekmesini seçin ve sonra **...** (elips) düğmesini seçin.
1. **Edit'i**seçin. 
   
   ![Gezinmeyi gösteren postacı uygulaması](media/knowledge-store-create-rest/postman-edit-menu.png "Postacı'daki Edit menüsüne git")
1. **Yap** iletişim kutusunda **Değişkenler** sekmesini seçin. 

**Değişkenler** sekmesinde, Postacı'nın çift ayraç içinde belirli bir değişkenle her karşılaştığında değiştirdiği değerleri ekleyebilirsiniz. Örneğin, Postacı sembolü `{{admin-key}}` ayarladığınız geçerli değerle `admin-key`değiştirir. Postacı URL'lerde, üstbilgide, istek gövdesinde ve benzeri yerlerde ikame yapar. 

Değeri almak için `admin-key`Azure Bilişsel Arama hizmetine gidin ve **Anahtarlar** sekmesini `search-service-name` seçin. [Create services](#create-services-and-load-data) `storage-account-name` Depolama `storage-connection-string` hesabının **Erişim Anahtarları** sekmesindeki değeri kullanarak ayarlayın. Varsayılanları diğer değerler için bırakabilirsiniz.

![Postacı uygulaması değişkenleri sekmesi](media/knowledge-store-create-rest/postman-variables-window.png "Postacı değişkenleri penceresi")


| Değişken    | Nereden edinilir: |
|-------------|-----------------|
| `admin-key` | Azure Bilişsel Arama hizmetinin **Keys** sayfasında.  |
| `api-version` | **2019-05-06-Önizleme**olarak bırakın. |
| `datasource-name` | **Otel-reviews-ds**olarak bırakın. | 
| `indexer-name` | **Otel-reviews-ixr**olarak bırakın . | 
| `index-name` | **Otel-reviews-ix**olarak bırakın . | 
| `search-service-name` | Azure Bilişsel Arama hizmetinin adı. URL' `https://{{search-service-name}}.search.windows.net`nin adı. | 
| `skillset-name` | **Otel-reviews-ss**olarak bırakın . | 
| `storage-account-name` | Depolama hesabı adı. | 
| `storage-connection-string` | Depolama hesabında, Erişim **Tuşları** sekmesinde **key1** > **Bağlantı dizesini**seçin. | 
| `storage-container-name` | Otel **yorumları**olarak bırakın. | 

### <a name="review-the-request-collection-in-postman"></a>Postacı'daki istek koleksiyonunu gözden geçirin

Bir bilgi deposu oluşturduğunuzda, dört HTTP isteği vermeniz gerekir: 

- **Dizin oluşturmak için PUT isteği**: Bu dizin, Azure Bilişsel Arama'nın kullandığı ve döndürdettiği verileri tutar.
- **Veri kaynağını oluşturmak için POST isteği**: Bu veri kaynağı, Azure Bilişsel Arama davranışınızı veri ve bilgi deposunun depolama hesabına bağlar. 
- **SKILLSET oluşturmak için PUT isteği**: Becerik, verilerinize uygulanan zenginleştirmeleri ve bilgi deposunun yapısını belirtir.
- **Dizin oluşturabilmek için PUT isteği**: Dizin leyiciyi çalıştırmak verileri okur, skillset'i uygular ve sonuçları depolar. Bu isteği en son çalıştırmalısınız.

[Kaynak kodu,](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) dört istek içeren bir Postacı koleksiyonu içerir. İstekleri vermek için Postacı'da istek sekmesini seçin. Ardından, `api-key` üstbilgi ekleyin ve `Content-Type` isteyin. Değerini ' `api-key` ye `{{admin-key}}`ayarlayın. Değeri `Content-type` `application/json`' ye ayarlayın 

![Üstbilgi için Postacı arabirimini gösteren ekran görüntüsü](media/knowledge-store-create-rest/postman-headers-ui.png)

> [!Note]
> Tüm isteklerinizi ayarlamanız `api-key` ve `Content-type` üstbilgi almalısınız. Postacı bir değişken tanıyorsa, değişken önceki ekran `{{admin-key}}` görüntüsünde olduğu gibi turuncu metinde görünür. Değişken yanlış yazılmışsa, kırmızı metinde görünür.
>

## <a name="create-an-azure-cognitive-search-index"></a>Azure Bilişsel Arama dizini oluşturma

Arama, filtreleme ve geliştirmeuygulamakla ilgilendiğiniz verileri temsil etmek için bir Azure Bilişsel Arama dizini oluşturun. `https://{{search-service-name}}.search.windows.net/indexes/{{index-name}}?api-version={{api-version}}`Bir PUT isteği vererek dizini oluşturun. Postacı, çift parantez içinde `{{search-service-name}}`bulunan sembolleri (, `{{index-name}}`, `{{api-version}}`ve ) [Postacı'yı Yapılandır'da](#configure-postman)ayarladığınız değerlerle değiştirir. REST komutlarınızı vermek için farklı bir araç kullanıyorsanız, bu değişkenleri kendiniz değiştirmelisiniz.

Azure Bilişsel Arama dizininizin yapısını isteğin gövdesinde ayarlayın. Postacı'da, üstbilgi `api-key` `Content-type` ve başlıkları ayarladıktan sonra, isteğin **Vücut** bölmesine gidin. Aşağıdaki JSON görmelisiniz. Bunu yapmazsanız, **Raw** > **JSON (uygulama/json)** seçeneğini belirleyin ve ardından aşağıdaki kodu gövde olarak yapıştırın:

```JSON
{
    "name": "{{index-name}}",
    "fields": [
        { "name": "address", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "categories", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "city", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
        { "name": "country", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "latitude", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "longitude", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "name", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
        { "name": "postalCode", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "province", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_date", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_dateAdded", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_rating", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_text", "type": "Edm.String", "filterable": false,  "sortable": false, "facetable": false },
        { "name": "reviews_title", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_username", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "AzureSearch_DocumentKey", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false, "key": true },
        { "name": "metadata_storage_content_type", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_size", "type": "Edm.Int64", "searchable": false, "filterable": false, "sortable": false, "facetable": false},
        { "name": "metadata_storage_last_modified", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_name", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_path", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "Sentiment", "type": "Collection(Edm.Double)", "searchable": false, "filterable": true, "retrievable": true, "sortable": false, "facetable": true },
        { "name": "Language", "type": "Edm.String", "filterable": true, "sortable": false, "facetable": true },
        { "name": "Keyphrases", "type": "Collection(Edm.String)", "filterable": true, "sortable": false, "facetable": true }
    ]
}

```

Bu dizin tanımı, kullanıcıya sunmak istediğiniz verilerin (otelin adı, içeriği gözden geçirme, tarih), arama meta verileri ve AI geliştirme verilerinin (Sentiment, Keyphrases ve Language) bir leşimidir.

PUT isteğini vermek için **Gönder'i** seçin. Durumu `201 - Created`görmelisiniz. **Vücut** bölmesinde farklı bir durum görürseniz, hata iletisi içeren bir JSON yanıtı arayın. 

## <a name="create-the-datasource"></a>Veri kaynağını oluşturma

Ardından, Azure Bilişsel Arama'yı Blob depolama alanında depoladığınız otel verilerine bağlayın. Veri kaynağını oluşturmak için, `https://{{search-service-name}}.search.windows.net/datasources?api-version={{api-version}}`bir POST isteği gönderin. Ve `api-key` üstbilgidaha `Content-Type` önce tartışıldığı gibi ayarlamanız gerekir. 

Postacı'da, **Veri Kaynağı Oluştur** isteğine gidin ve ardından **Body** bölmesine gidin. Aşağıdaki kodu görmeniz gerekir:

```json
{
  "name" : "{{datasource-name}}",
  "description" : "Demo files to demonstrate knowledge store capabilities.",
  "type" : "azureblob",
  "credentials" : { "connectionString" : "{{storage-connection-string}}" },
  "container" : { "name" : "{{storage-container-name}}" }
}
```

POST isteğini vermek için **Gönder'i** seçin. 

## <a name="create-the-skillset"></a>Skillset oluşturun 

Bir sonraki adım, hem uygulanacak geliştirmeleri hem de sonuçların depolanacakbilgi deposunu belirten skillset'i belirtmektir. Postacı'da **Skillset sekmesini oluştur'u** seçin. Bu istek bir `https://{{search-service-name}}.search.windows.net/skillsets/{{skillset-name}}?api-version={{api-version}}`PUT gönderir. Daha `api-key` önce `Content-type` yaptığınız gibi üstbilgi ve üstbilgi ayarlayın. 

İki büyük üst düzey nesne `skills` vardır: ve `knowledgeStore`. Nesnenin `skills` içindeki her nesne bir zenginleştirme hizmetidir. Her zenginleştirme `inputs` hizmeti `outputs`vardır ve . Bir `LanguageDetectionSkill` çıktısı `targetName` `Language`vardır . Bu düğümün değeri, diğer becerilerin çoğu tarafından giriş olarak kullanılır. Kaynak. `document/Language` Bir düğümün çıktısını diğerine giriş olarak kullanma `ShaperSkill`yeteneği, verilerin bilgi deposunun tablolarına nasıl aktığını belirten, daha da belirgindir.

Nesne `knowledge_store` `{{storage-connection-string}}` Postacı değişkeni üzerinden depolama hesabına bağlanır. `knowledge_store`gelişmiş belge ile bilgi deposundaki tablolar ve sütunlar arasında bir eşleme kümesi içerir. 

Skillset oluşturmak için, postacı gönder **düğmesini** seçin ve isteği nizi belirtin:

```json
{
    "name": "{{skillset-name}}",
    "description": "Skillset to detect language, extract key phrases, and detect sentiment",
    "skills": [ 
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill", 
            "context": "/document/reviews_text", "textSplitMode": "pages", "maximumPageLength": 5000,
            "inputs": [ 
                { "name": "text", "source": "/document/reviews_text" },
                { "name": "languageCode", "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "textItems", "targetName": "pages" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
            "context": "/document/reviews_text/pages/*",
            "inputs": [
                { "name": "text", "source": "/document/reviews_text/pages/*" },
                { "name": "languageCode", "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "score", "targetName": "Sentiment" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
            "context": "/document",
            "inputs": [
                { "name": "text", "source": "/document/reviews_text" }
            ],
            "outputs": [
                { "name": "languageCode", "targetName": "Language" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
            "context": "/document/reviews_text/pages/*",
            "inputs": [
                { "name": "text",  "source": "/document/reviews_text/pages/*" },
                { "name": "languageCode",  "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "keyPhrases" , "targetName": "Keyphrases" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
            "context": "/document",
            "inputs": [
                { "name": "name",  "source": "/document/name" },
                { "name": "reviews_date",  "source": "/document/reviews_date" },
                { "name": "reviews_rating",  "source": "/document/reviews_rating" },
                { "name": "reviews_text",  "source": "/document/reviews_text" },
                { "name": "reviews_title",  "source": "/document/reviews_title" },
                { "name": "AzureSearch_DocumentKey",  "source": "/document/AzureSearch_DocumentKey" },
                { 
                    "name": "pages",
                    "sourceContext": "/document/reviews_text/pages/*",
                    "inputs": [
                        { "name": "SentimentScore", "source": "/document/reviews_text/pages/*/Sentiment" },
                        { "name": "LanguageCode", "source": "/document/Language" },
                        { "name": "Page", "source": "/document/reviews_text/pages/*" },
                        { 
                            "name": "keyphrase", "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                            "inputs": [
                                { "name": "Keyphrases", "source": "/document/reviews_text/pages/*/Keyphrases/*" }
                            ]
                        }
                    ]
                }
            ],
            "outputs": [
                { "name": "output" , "targetName": "tableprojection" }
            ]
        }
    ],
    "knowledgeStore": {
        "storageConnectionString": "{{storage-connection-string}}",
        "projections": [
            {
                "tables": [
                    { "tableName": "hotelReviewsDocument", "generatedKeyName": "Documentid", "source": "/document/tableprojection" },
                    { "tableName": "hotelReviewsPages", "generatedKeyName": "Pagesid", "source": "/document/tableprojection/pages/*" },
                    { "tableName": "hotelReviewsKeyPhrases", "generatedKeyName": "KeyPhrasesid", "source": "/document/tableprojection/pages/*/keyphrase/*" },
                    { "tableName": "hotelReviewsSentiment", "generatedKeyName": "Sentimentid", "source": "/document/tableprojection/pages/*/sentiment/*" }
                ],
                "objects": []
            },
            {
                "tables": [
                    { 
                        "tableName": "hotelReviewsInlineDocument", "generatedKeyName": "Documentid", "sourceContext": "/document",
                        "inputs": [
                            { "name": "name", "source": "/document/name"},
                            { "name": "reviews_date", "source": "/document/reviews_date"},
                            { "name": "reviews_rating", "source": "/document/reviews_rating"},
                            { "name": "reviews_text", "source": "/document/reviews_text"},
                            { "name": "reviews_title", "source": "/document/reviews_title"},
                            { "name": "AzureSearch_DocumentKey", "source": "/document/AzureSearch_DocumentKey" }
                        ]
                    },
                    { 
                        "tableName": "hotelReviewsInlinePages", "generatedKeyName": "Pagesid", "sourceContext": "/document/reviews_text/pages/*",
                        "inputs": [
                            { "name": "SentimentScore", "source": "/document/reviews_text/pages/*/Sentiment"},
                            { "name": "LanguageCode", "source": "/document/Language"},
                            { "name": "Page", "source": "/document/reviews_text/pages/*" }
                        ]
                    },
                    { 
                        "tableName": "hotelReviewsInlineKeyPhrases", "generatedKeyName": "kpidv2", "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                        "inputs": [
                            { "name": "Keyphrases", "source": "/document/reviews_text/pages/*/Keyphrases/*" }
                        ]
                    }
                ],
                "objects": []
            }
        ]
    }
}
```

## <a name="create-the-indexer"></a>Dizin oluştur

Son adım dizin oluşturabilirsiniz. Dizinleyici verileri okur ve becerileri etkinleştirir. Postacı'da, **Dizin Oluştur** isteğini seçin ve ardından gövdeyi gözden geçirin. Dizin oluşturucunun tanımı, zaten oluşturduğunuz birkaç başka kaynağı ifade eder: veri kaynağı, dizin ve beceri. 

Nesne, `parameters/configuration` dizinleyicinin verileri nasıl yutturacaklarını denetler. Bu durumda, giriş verileri üstbilgi satırı ve virgülden ayrılmış değerleri olan tek bir belgededir. Belge anahtarı, belge için benzersiz bir tanımlayıcıdır. Kodlamadan önce belge anahtarı kaynak belgenin URL'sidir. Son olarak, dil kodu, duygu ve anahtar tümcecikler gibi becerikçıkış değerleri belgedeki konumlarına eşlenir. Tek bir değer olmasına `Language`rağmen, `Sentiment` dizideki her öğeye `pages`uygulanır. `Keyphrases``pages` dizideki her öğeye de uygulanan bir dizidir.

Üstbilgi `api-key` ve `Content-type` üstbilgi ayarladıktan ve isteğin gövdesinin aşağıdaki kaynak koduna benzediğini doğruladıktan sonra Postacı'yı **Gönder'i** seçin. Postacı bir PUT `https://{{search-service-name}}.search.windows.net/indexers/{{indexer-name}}?api-version={{api-version}}`isteği gönderir . Azure Bilişsel Arama dizin oluşturup çalıştırıyor. 

```json
{
    "name": "{{indexer-name}}",
    "dataSourceName": "{{datasource-name}}",
    "skillsetName": "{{skillset-name}}",
    "targetIndexName": "{{index-name}}",
    "parameters": {
        "configuration": {
            "dataToExtract": "contentAndMetadata",
            "parsingMode": "delimitedText",
            "firstLineContainsHeaders": true,
            "delimitedTextDelimiter": ","
        }
    },
    "fieldMappings": [
        {
            "sourceFieldName": "AzureSearch_DocumentKey",
            "targetFieldName": "AzureSearch_DocumentKey",
            "mappingFunction": { "name": "base64Encode" }
        }
    ],
    "outputFieldMappings": [
        { "sourceFieldName": "/document/reviews_text/pages/*/Keyphrases/*", "targetFieldName": "Keyphrases" },
        { "sourceFieldName": "/document/Language", "targetFieldName": "Language" },
        { "sourceFieldName": "/document/reviews_text/pages/*/Sentiment", "targetFieldName": "Sentiment" }
    ]
}
```

## <a name="run-the-indexer"></a>Dizin oluşturucuyu çalıştırma 

Azure portalında, Azure Bilişsel Arama hizmetinin **Genel Bakış** sayfasına gidin. **Indexers** sekmesini seçin ve ardından **oteller-incelemeler-ixr'ı**seçin. Dizin leyici zaten çalışmadıysa **Çalıştır'ı**seçin. Dizin oluşturma görevi, dil tanımayla ilgili bazı uyarıları artırabilir. Veriler, henüz bilişsel beceriler tarafından desteklenmeyen dillerde yazılmış bazı incelemeler içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Artık Bilişsel Hizmetler'i kullanarak verilerinizi zenginleştirdiğinize ve sonuçları bir bilgi deposuna yansıttığınıza göre, zenginleştirilmiş veri setinizi keşfetmek için Storage Explorer veya Power BI'yi kullanabilirsiniz.

Depolama Gezgini'ni kullanarak bu bilgi deposunun nasıl keşfedilebildiğini öğrenmek için şu gözden geçirebilme şekline bakın:

> [!div class="nextstepaction"]
> [Depolama Gezgini ile görüntüleme](knowledge-store-view-storage-explorer.md)

Bu bilgi deposunun Power BI'ye nasıl bağlanılabildiğini öğrenmek için şu izilebine bakın:

> [!div class="nextstepaction"]
> [Power BI ile bağlanma](knowledge-store-connect-power-bi.md)

Bu alıştırmayı tekrarlamak veya farklı bir AI zenginleştirme gözden geçirme denemek istiyorsanız, **otel-reviews-idxr** indexer'ı silin. Dizinleyicinin silmesi, ücretsiz günlük işlem sayacını sıfıra ayarlar.
