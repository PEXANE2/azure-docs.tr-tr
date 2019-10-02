---
title: REST Azure Search kullanarak bilgi deposu oluşturma
description: Bilişsel arama işlem hattından, REST API ve Postman kullanarak kalıcı zenginler için Azure Search bilgi deposu oluşturun.
author: lobrien
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: tutorial
ms.date: 10/01/2019
ms.author: laobri
ms.openlocfilehash: e28fa919c4c656b9ceb1d34806c3ef08aec2df2c
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71719932"
---
# <a name="create-an-azure-search-knowledge-store-using-rest"></a>REST kullanarak Azure Search bilgi deposu oluşturma

Bilgi deposu, daha sonraki analizler veya diğer aşağı akış işlemleri için bir AI zenginleştirme ardışık düzeninde çıktıyı sürekli olarak sürdüren Azure Search bir özelliktir. Bir AI zenginleştirme işlem hattı, görüntü dosyalarını veya yapılandırılmamış metin dosyalarını kabul eder, Azure Search kullanarak dizinler, bilişsel hizmetlerden AI zenginleştirmelerini (görüntü analizi ve doğal dil işleme) uygular ve sonuçları Azure 'da bir bilgi deposuna kaydeder. Depo. Daha sonra bilgi deposunu araştırmak için Power BI veya Depolama Gezgini gibi araçları kullanabilirsiniz.

Bu makalede, REST API arabirimini kullanarak bir otel İncelemeleri kümesine AI zenginleştirme, dizin oluşturma ve uygulama işlemi gerçekleştirebilirsiniz. Otel İncelemeleri Azure Blob depolama alanına aktarılır ve sonuçlar Azure Tablo depolama alanına bir bilgi deposu olarak kaydedilir.

Bilgi deposunu oluşturduktan sonra, [Depolama Gezgini](knowledge-store-view-storage-explorer.md) veya [Power BI](knowledge-store-connect-power-bi.md)kullanarak bu bilgi deposuna erişme hakkında bilgi edinebilirsiniz.

## <a name="1---create-services"></a>1-hizmet oluşturma

+ Geçerli aboneliğinizde [bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

+ Örnek verileri ve bilgi deposunu depolamak için [bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) . Depolama hesabınızın Azure Search hizmetiniz için aynı konumu (ABD-Batı) kullanması gerekir. *Hesap türü* *StorageV2 (genel amaçlı v2)* (varsayılan) veya *depolama (genel amaçlı v1)* olmalıdır.

+ Önerilen: Azure Search istek göndermek için [Postman masaüstü uygulaması](https://www.getpostman.com/) . REST API, HTTP istekleri ve yanıtları ile çalışan herhangi bir araçla birlikte kullanabilirsiniz. Postman, REST API 'Leri keşfetmek için iyi bir seçimdir ve bu makalede kullanılacaktır. Ayrıca, bu makaleye ait [kaynak kodu](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) Isteklerin bir Postman koleksiyonunu içerir. 

## <a name="2---store-the-data"></a>2-verileri depolama

Bir Azure Search Indexer tarafından erişilebilmesi ve AI zenginleştirme ardışık düzeninde beslenmesini sağlamak için otel, CSV dosyasını Azure Blob depolama alanına yükleyin.

### <a name="create-an-azure-blob-container-with-the-data"></a>Verilerle bir Azure Blob kapsayıcısı oluşturma

1. [BIR CSV dosyasına kaydedilmiş otel gözden geçirme verilerini indirin (HotelReviews_Free. csv)](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Bu veriler Kaggle.com adresinden kaynaklanır ve oteller hakkında müşteri geri bildirimi içerir.
1. [Azure Portal oturum açın](https://portal.azure.com)ve Azure depolama hesabınıza gidin.
1. [BLOB kapsayıcısı oluşturun](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Kapsayıcıyı oluşturmak için, depolama hesabınızın sol gezinti çubuğunda, **Bloblar**' a tıklayın ve ardından komut çubuğunda **+ kapsayıcı** ' ya tıklayın.
1. Yeni kapsayıcı **adı**için `hotel-reviews` girin.
1. Herhangi bir **genel erişim düzeyi**seçin. Varsayılan değer kullandık.
1. Azure Blob kapsayıcısını oluşturmak için **Tamam** ' ı tıklatın.
1. Yeni `hotels-review` kapsayıcısını açın, **karşıya yükle**' ye tıklayın ve ilk adımda indirdiğiniz **HotelReviews-Free. csv** dosyasını seçin.

    ![Karşıya veri yükleme](media/knowledge-store-create-portal/upload-command-bar.png "otel incelemelerini karşıya yükleyin")

1. CSV dosyasını Azure Blob depolamaya aktarmak için **karşıya yükle** ' ye tıklayın. Yeni kapsayıcı görüntülenir.

    Azure Blob ![kapsayıcısını oluşturma Azure](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "BLOB kapsayıcısını oluşturma")

## <a name="3---configure-postman"></a>3-Postman 'ı yapılandırma

[Postman koleksiyonu kaynak kodunu](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) Indirin ve **Dosya, içeri aktar... öğesini**kullanarak Postman 'a aktarın. **Koleksiyonlar** sekmesine geçin ve **...** düğmesine tıklayın ve **Düzenle**' yi seçin. 

![Gezinmeyi gösteren Postman uygulaması](media/knowledge-store-create-rest/postman-edit-menu.png "Postman 'da Düzenle menüsüne git")

Elde edilen düzenleme iletişim kutusunda, **değişkenler** sekmesine gidin. 

**Değişkenler** sekmesi, Postman 'ın, çift ayraç içinde karşılaştığı her seferinde takas edecek değerler eklemenize olanak tanır. Örneğin Postman `{{admin-key}}` sembolünü `admin-key` ' in "geçerli değeri" ile değiştirir. Postman bu değişikliği URL 'Ler, üst bilgiler, istek gövdesi ve benzeri olarak yapar. 

@No__t-0 değerini Arama Hizmeti **anahtarlar** sekmesinde bulacaksınız. @No__t-2 ve `storage-account-name` ' i [1. adımda](#1---create-services)seçtiğiniz değerlere değiştirmeniz gerekir. Depolama hesabının **erişim anahtarları** sekmesindeki değeri `storage-connection-string` olarak ayarlayın. Başka değerler de değişmeden bırakabilirsiniz.

![Postman uygulama değişkenleri sekmesi](media/knowledge-store-create-rest/postman-variables-window.png "Postman 'ın değişkenler penceresi")


| Değişken    | Nereden alınır? |
|-------------|-----------------|
| `admin-key` | Arama Hizmeti, **anahtarlar** sekmesi              |
| `api-version` | "2019-05-06-Preview" olarak bırakın |
| `datasource-name` | "Otel-incelemeler-DS" olarak bırakın | 
| `indexer-name` | "Otel-incelemeler-ixR" olarak bırakın | 
| `index-name` | "Otel-incelemeler-ix" olarak bırakın | 
| `search-service-name` | Arama Hizmeti, ana ad. URL `https://{{search-service-name}}.search.windows.net` | 
| `skillset-name` | "Otel-incelemeler-ss" olarak bırakın | 
| `storage-account-name` | Depolama hesabı, ana ad | 
| `storage-connection-string` | Depolama hesabı, **erişim anahtarları** sekmesi, **KEY1** **bağlantı dizesi** | 
| `storage-container-name` | "Otel-incelemeler" olarak bırakın | 

### <a name="review-the-request-collection-in-postman"></a>Postman 'da istek koleksiyonunu gözden geçirin

Bilgi deposu oluşturmak için dört HTTP isteği oluşturmanız gerekir: 

1. Dizini oluşturmak için bir PUT isteği. Bu dizin, Azure Search tarafından kullanılan ve döndürülen verileri tutar.
1. Veri kaynağını oluşturmak için bir POST isteği. Bu veri kaynağı, Azure Search davranışını veri ve bilgi deposunun depolama hesabına bağlar. 
1. Beceri oluşturmaya yönelik bir PUT isteği. Beceri, verilerinize ve bilgi deposunun yapısına uygulanan zenginleştirme türlerini belirtir.
1. Dizin oluşturucuyu oluşturmak için bir PUT isteği. Dizin oluşturucuyu çalıştırmak, verileri okur, Beceri uygular ve sonuçları depolar. Bu isteği en son çalıştırmanız gerekir.

[Kaynak kodu](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) , bu dört istek Içeren bir Postman koleksiyonu içerir. İstekleri vermek için Postman 'daki isteğin sekmesine geçin ve `api-key` ve `Content-Type` istek üst bilgilerini ekleyin. @No__t-0 değerini `{{admin-key}}` olarak ayarlayın. @No__t-0 değerini `application/json` olarak ayarlayın. 

> [!div class="mx-imgBorder"]
> ![-1 üstbilgiler için Postman arabirimini gösteren-0Ekran görüntüsü

> [!Note]
> İsteklerinizin tümünde `api-key` ve `Content-type` üst bilgilerini ayarlamanız gerekecektir. Bir değişken Postman tarafından tanınırsa, ekran görüntüsünde `{{admin-key}}` ile birlikte turuncu metin olarak işlenir. Değişken yanlış yazıldığında, kırmızı metin olarak işlenir.
>

## <a name="4---create-an-azure-search-index"></a>4-Azure Search dizini oluşturma

Arama, filtreleme ve geliştirmeler yaparken ilgilendiğiniz verileri temsil etmek için bir Azure Search dizini oluşturmanız gerekir. @No__t-0 ' a bir PUT isteği vererek dizini oluşturun. Postman, `{{search-service-name}}`, `{{index-name}}` ve `{{api-version}}` gibi çift küme ayraçları içindeki sembolleri [3. adımda](#3---configure-postman)belirtilen değerlerle değiştirecek. REST komutlarınızı vermek için başka bir araç kullanıyorsanız, bu değişkenleri kendiniz yerine koymak zorunda olacaksınız.

İsteğin gövdesinde Azure Search dizininizin yapısını belirtin. Postman 'da, `api-key` ve `Content-type` üst bilgilerini ayarladıktan sonra isteğin **gövde** bölmesine geçin. Aşağıdaki JSON 'u görmeniz gerekir, ancak yoksa, **RAW** ve **JSON (Application/JSON)** öğesini seçin ve aşağıdaki kodu gövde olarak yapıştırın:

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

Bu dizin tanımının kullanıcıya sunmak istediğiniz verilerin bir birleşimi olduğunu görürsünüz (otel adı, içerik, tarih ve benzeri), arama meta verileri ve AI geliştirme verileri (yaklaşım, Keyphrases ve dil).

PUT isteğini vermek için **Gönder** düğmesine basın. @No__t-0 ' ın durum iletisini almalısınız. Farklı bir durum alırsanız, **gövde** bölmesi bir hata iletisiyle JSON yanıtı gösterir. 

## <a name="5---create-the-datasource"></a>5-veri kaynağını oluşturma

Şimdi [Adım 2](#2---store-the-data)' de depoladığınız otel verilerine Azure Search bağlamanız gerekir. Veri kaynağının oluşturulması `https://{{search-service-name}}.search.windows.net/datasources?api-version={{api-version}}` ' a bir GÖNDERIYLE yapılır. Yine, `api-key` ve `Content-Type` üst bilgilerini daha önce belirtildiği gibi ayarlamanız gerekir. 

Postman 'da "veri kaynağı oluşturma" isteğini açın. Aşağıdaki koda sahip olması gereken **gövde** bölmesine geçin:

```json
{
  "name" : "{{datasource-name}}",
  "description" : "Demo files to demonstrate knowledge store capabilities.",
  "type" : "azureblob",
  "credentials" : { "connectionString" : "{{storage-connection-string}}" },
  "container" : { "name" : "{{storage-container-name}}" }
}
```

POST isteğini vermek için **Gönder** düğmesine basın. 

## <a name="6---create-the-skillset"></a>6-beceri oluşturma 

Sonraki adım, uygulanacak geliştirmeleri ve sonuçların depolanacağı bilgi deposunu belirten beceri belirtmektir. Postman 'da "beceri oluştur" sekmesini açın. Bu istek, `https://{{search-service-name}}.search.windows.net/skillsets/{{skillset-name}}?api-version={{api-version}}` ' a bir PUT gönderir.
@No__t-0 ve `Content-type` üst bilgilerini daha önce yaptığınız gibi ayarlayın. 

İki büyük üst düzey nesne vardır: `"skills"` ve `"knowledgeStore"`. @No__t-0 nesnesi içindeki her nesne bir zenginleştirme hizmetidir. Her bir zenginleştirme hizmeti `"inputs"` ve `"outputs"` ' dir. @No__t-0 ' a bir çıkış @no__t nasıl `"Language"` ' ye sahip olduğuna dikkat edin. Bu düğümün değeri, diğer yeteneklerin çoğu tarafından, kaynağı `document/Language` olarak bir giriş olarak kullanılır. Bir düğümün başka bir düğüme giriş olarak kullanılması, bu `ShaperSkill` ' ın, verilerin bilgi deposunun tablolarına nasıl akabildiğini belirten daha da fazla olması anlamına gelir.

@No__t-0 nesnesi, `{{storage-connection-string}}` Postman değişkeni aracılığıyla depolama hesabına bağlanır. Daha sonra, gelişmiş belge ve bilgi deposu 'nda kullanılabilir olacak tablolar ile sütunlar arasında bir eşleme kümesi içerir. 

Beceri oluşturmak için Postman 'daki **Gönder** düğmesine basarak isteği koyun.

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

## <a name="7---create-the-indexer"></a>7-Dizin oluşturucuyu oluşturma

Son adım, aslında verileri okuyan ve beceri etkinleştiren Dizin oluşturucuyu oluşturmaktır. Postman 'da "Dizin Oluşturucu oluştur" isteğine geçiş yapın ve gövdesini gözden geçirin. Gördüğünüz gibi, dizin oluşturucunun tanımı, zaten oluşturmuş olduğunuz diğer diğer kaynaklara başvurur. veri kaynağı, dizin ve beceri. 

@No__t-0 nesnesi, dizin oluşturucunun verileri nasıl geri gediğini denetler. Bu durumda, giriş verileri üst bilgi satırı ve virgülle ayrılmış değerler içeren tek bir belgedir. Belge anahtarı, belge için benzersiz bir tanıtıcıdır. Bu, kodlama öncesinde kaynak belgenin URL 'sidir. Son olarak, dil kodu, yaklaşım ve anahtar ifadeler gibi beceri çıkış değerleri belgedeki uygun konumlarına eşlenir. @No__t-0 için tek bir değer olsa da, `Sentiment` ' in `pages` dizisindeki her öğeye uygulandığını unutmayın. `Keyphrases` bir dizidir ve `pages` dizisindeki her öğeye de uygulanır.

@No__t-0 ve `Content-type` üst bilgilerini ayarladıktan ve isteğin gövdesinin izleyen kaynak koda benzer olduğunu onayladıktan sonra Postman 'da **Gönder** ' e basın. Postman `https://{{search-service-name}}.search.windows.net/indexers/{{indexer-name}}?api-version={{api-version}}` ' a isteği koyar. Azure Search Dizin oluşturucuyu oluşturur ve çalıştırır. 

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

## <a name="8---run-the-indexer"></a>8-Dizin oluşturucuyu çalıştırma 

Azure portal Arama Hizmeti **Genel Bakış ' a** gidin ve **Dizin oluşturucular** sekmesini seçin. önceki adımda oluşturduğunuz **oteller-Incelemeler-ixR** ' e tıklayın. Dizin Oluşturucu zaten çalıştırılmışsa, **Çalıştır** düğmesine basın. Veriler, bilişsel yetenekler tarafından henüz desteklenmeyen dillerde yazılmış bazı İncelemeleri içerdiği için dizin oluşturma görevi, dil tanıma ile ilgili bazı uyarılar oluşturabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel hizmetler 'i kullanarak verilerinizi zenginleştirdiniz ve sonuçları bir bilgi deposuna yansıdığınıza göre, zenginleştirilmiş veri kümesini araştırmak için Depolama Gezgini veya Power BI kullanabilirsiniz.

Bu bilgi deposunu Depolama Gezgini kullanarak nasıl keşfedeceğinizi öğrenmek için aşağıdaki izlenecek yolu inceleyin.

> [!div class="nextstepaction"]
> [Depolama Gezgini ile görüntüle](knowledge-store-view-storage-explorer.md)

Bu bilgi deposunu Power BI nasıl bağlayacağınızı öğrenmek için aşağıdaki izlenecek yolu inceleyin.

> [!div class="nextstepaction"]
> [Power BI ile bağlanma](knowledge-store-connect-power-bi.md)

Bu alıştırmayı yinelemek veya farklı bir AI zenginleştirme Kılavuzu denemek istiyorsanız, *otel-incelemeler-ıdxr* Dizin oluşturucuyu silin. Dizin oluşturucunun silinmesi, ücretsiz günlük işlem sayacını yeniden sıfıra sıfırlar.
