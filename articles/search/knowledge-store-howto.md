---
title: Bilgi deposu (Önizleme) ile çalışmaya başlama-Azure Search
description: Azure Depolama hesabınızdaki bir bilgi deposuna Azure Search AI dizin oluşturma işlem hatları tarafından oluşturulan zenginleştirilmiş belgeleri gönderme adımlarını öğrenin. Buradan, Azure Search ve diğer uygulamalarda zenginleştirilmiş belgeleri görüntüleyebilir, yeniden şekillendirebilirsiniz ve kullanabilirsiniz.
manager: nitinme
author: HeidiSteen
services: search
ms.service: search
ms.topic: tutorial
ms.date: 06/29/2019
ms.author: heidist
ms.openlocfilehash: 7568742c4a01903eddf7681da71e4f243cac1e56
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265621"
---
# <a name="how-to-get-started-with-knowledge-store-in-azure-search"></a>Azure Search 'de bilgi deposu ile çalışmaya başlama

> [!Note]
> Bilgi deposu önizleme aşamasındadır ve üretim kullanımı için tasarlanmamıştır. [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>
[Bilgi deposu](knowledge-store-concept-intro.md) , diğer uygulamalardaki aşağı akış veri madenciliği için Azure depolama hesabınıza dizin oluşturma SıRASıNDA oluşturulan AI zenginleştirilmiş belgelerini kaydeder. Ayrıca, Azure Search dizin oluşturma işlem hattını anlamak ve iyileştirmek için kaydedilmiş zenginler de kullanabilirsiniz. 

Bilgi deposu bir *beceri* tarafından tanımlanır ve bir *Dizin Oluşturucu*tarafından oluşturulur. Bilgi deposunun fiziksel ifadesi, depolamada veri yapılarını tespit eden *projeksiyonlar* aracılığıyla belirtilir. Bu yönergeyi bitirdiğinizde, tüm bu nesneleri oluşturmuş olacaksınız ve bunların tümünün bir araya nasıl uyduğunu anlarsınız. 

Bu alıştırmada, ilk bilgi deponuzu oluşturmaya ve kullanmaya yönelik temel iş akışını öğrenmek için örnek verileri, hizmetleri ve araçları kullanmaya başlayın ve beceri tanımını kullanın.

## <a name="prerequisites"></a>Önkoşullar

Bilgi deposu, Azure Blob depolama ve Azure Tablo depolama ile birden fazla hizmetin merkezinde ve nesne oluşturma ve güncelleştirmeler için Azure Search ve bilişsel hizmetler sunar. [Temel mimariyle](knowledge-store-concept-intro.md) benzerlik, Bu izlenecek yol için bir önkoşuldur.

Bu hızlı başlangıçta aşağıdaki hizmetler ve araçlar kullanılır. 

+ Azure Search HTTP istekleri göndermek için kullanılan [Postman masaüstü uygulamasını alın](https://www.getpostman.com/).

+ Örnek verileri ve bilgi deposunu depolamak için [bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) . Bilgi depolduğunuz Azure Storage 'da mevcut olacaktır.

+ En geniş bir yelpazede, AI zenginleştirmelerinde kullanılan yeteneklerin tamamına çok sayıda erişim sağlamak için S0 Kullandıkça Öde katmanında bilişsel [Hizmetler kaynağı oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) . Bilişsel hizmetler ve Azure Search hizmetinizin aynı bölgede olması gerekir.

+ Geçerli aboneliğinizde [bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz. 

Örnek JSON belgeleri ve Postman koleksiyon dosyası da gereklidir. Tamamlayıcı dosyaları bulma ve yükleme yönergeleri [örnek verileri hazırlama](#prepare-sample-data) bölümünde verilmiştir.

## <a name="get-a-key-and-url"></a>Anahtar ve URL al

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

    ![Http uç noktası ve erişim anahtarı al](media/search-get-started-postman/get-url-key.png "Http uç noktası ve erişim anahtarı al")

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. Aşağıdaki bölümlerde, her HTTP isteğinde hizmet adı ve API anahtarı sağlarsınız.

<a name="prepare-sample-data"></a>

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

Bilgi deposu bir zenginleştirme işlem hattının çıktısını içerir. Girişler, sonunda işlem hattında ilerledikçe "kullanılamaz" verilerden oluşur. Kullanılamaz verilere örnek olarak, metin veya görüntü özellikleri için çözümlenmesi gereken resim dosyaları ya da varlıklar, anahtar ifadeler veya yaklaşım için çözümlenebilen yoğun metin dosyaları bulunabilir. 

Bu alıştırma, [Caselaw Access Project](https://case.law/bulk/download/) genel toplu veri indirme sayfasından kaynaklanan yoğun metin dosyalarını (büyük/küçük harf yasaları bilgileri) kullanır. Bu alıştırma için GitHub 'a 10 belgeli bir örnek yüklediniz. 

Bu görevde, bu belgeler için işlem hattına giriş olarak kullanılacak bir Azure Blob kapsayıcısı oluşturacaksınız. 

1. [Caselaw veri kümesini](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw)almak Için [Azure Search örnek veri](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw) deposunu indirip ayıklayın. 

1. [Azure Portal oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, Bloblar ' a tıklayınve ardından **+ Container**' a tıklayın.

1. Örnek veri içeren [bir blob kapsayıcısı oluşturun](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) : 

   1. Kapsayıcıyı `caselaw-test`adlandırın. 
   
   1. Ortak erişim düzeyini geçerli değerlerinden herhangi birine ayarlayın.

1. Kapsayıcı oluşturulduktan sonra açın ve komut çubuğunda **karşıya yükle** ' yi seçin.

   ![Komut çubuğuna yükle](media/search-semi-structured-data/upload-command-bar.png "Komut çubuğuna yükle")

1. **Caselaw-Sample. JSON** örnek dosyasını içeren klasöre gidin. Dosyayı seçin ve ardından **karşıya yükle**' ye tıklayın.

1. Azure depolarken bağlantı dizesini ve kapsayıcı adını alın.  [Veri kaynağı oluştur](#create-data-source)bölümünde Bu dizelerin her ikisine de ihtiyacınız olacak:

   1. Genel Bakış sayfasında, **erişim anahtarları** ' na tıklayın ve *bağlantı dizesini*kopyalayın. İle `DefaultEndpointsProtocol=https;` başlar ve ile `EndpointSuffix=core.windows.net`sonlanır. Hesap adınız ve anahtarınız arasında bulunur. 

   1. Kapsayıcı adı `caselaw-test` ya da atadığınız ad olmalıdır.



## <a name="set-up-postman"></a>Postman’i ayarlama

Postman, Azure Search istek ve JSON belgelerini göndermek için kullanacağınız istemci uygulamasıdır. İsteklerin birkaçı yalnızca bu makaledeki bilgiler kullanılarak formüle eklenebilir. Ancak, en büyük isteklerin ikisi (Dizin oluşturma, Beceri oluşturma), bir makaleye eklemek için çok büyük olan ayrıntılı JSON ' u içerir. 

Tüm JSON belgelerini ve isteklerini tam olarak kullanılabilir hale getirmek için bir Postman koleksiyon dosyası oluşturduk. Bu dosyayı indirmek ve sonra içeri aktarmak, istemcisini ayarlarken ilk göreviniz olur.

1. [Azure Search Postman örnekleri](https://github.com/Azure-Samples/azure-search-postman-samples) deposunu indirip sıkıştırmasını açın.

1. Postman 'ı başlatın ve Caselaw Postman koleksiyonunu içeri aktarın:

   1. İçeri aktarma > **dosyalarını**İçeriAktar'atıklayındosyaları > **seçin**. 

   1. \Azure-search-postman-samples-master\azure-search-postman-samples-master\Caselaw klasörüne gidin.

   1. **Caselaw. postman_collection_v2. JSON**öğesini seçin. Koleksiyonda dört **Post** isteği görmeniz gerekir.

   ![Caselaw tanıtımı Için Postman koleksiyonu](media/knowledge-store-howto/postman-collection.png "Caselaw tanıtımı Için Postman koleksiyonu")
   

## <a name="create-an-index"></a>Dizin oluşturma
    
İlk istek, tüm aranabilir verileri depolayan bir Azure Search Dizin [oluşturmak Için CREATE INDEX API](https://docs.microsoft.com/rest/api/searchservice/create-data-source)'sini kullanır. Dizin, tüm alanları, parametreleri ve öznitelikleri belirtir.

Bilgi madenciliği için bir dizine ihtiyacınız yoktur, ancak dizin sağlanmadığı takdirde bir Dizin Oluşturucu çalıştırılmaz. 

1. URL `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/indexes?api-version=2019-05-06-Preview`'de, öğesini arama `YOUR-AZURE-SEARCH-SERVICE-NAME` hizmetinizin adıyla değiştirin. 

1. Üst bilgi bölümünde, Azure Search için `<YOUR AZURE SEARCH ADMIN API-KEY>` bir yönetici API anahtarı ile değiştirin.

1. Gövde bölümünde JSON belgesi bir dizin şemadır. Görünürlük için daraltılmış, bir dizinin dış kabuğu aşağıdaki öğelerden oluşur. Alanlar koleksiyonu, Caselaw veri kümesindeki alanlara karşılık gelir.

   ```json
   {
      "name": "caselaw",
      "defaultScoringProfile": null,
      "fields": [],
      "scoringProfiles": [],
      "corsOptions": null,
      "suggesters": [],
      "analyzers": [],
      "tokenizers": [],
      "tokenFilters": [],
      "charFilters": [],
      "encryptionKey": null
   }
   ```

1. `fields` Koleksiyonu genişletin. Basit alanlardan, iç içe geçmiş alt yapılarla [karmaşık alanlardan](search-howto-complex-data-types.md) ve koleksiyonlardan oluşan Dizin tanımının toplu öğesini içerir.

   302-384. satırlardaki `casebody` karmaşık alanın alan tanımını gözden geçirmek için bir dakikanızı ayırın. Karmaşık bir alanın hiyerarşik temsiller gerektiğinde diğer karmaşık alanları içerebileceğini unutmayın. Hiyerarşik yapılar, burada gösterildiği gibi bir dizinde modellenebilir ve ayrıca bir beceri projeksiyon olarak bilgi deposunda iç içe geçmiş veri yapısı oluşturabilir.

   ```json
   {
    "name": "casebody",
    "type": "Edm.ComplexType",
    "fields": [
        {
            "name": "status",
            "type": "Edm.String",
            "searchable": true,
            "filterable": true,
            "retrievable": true,
            "sortable": true,
            "facetable": true,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "data",
            "type": "Edm.ComplexType",
            "fields": [
                {
                    "name": "head_matter",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "retrievable": true,
                    "sortable": false,
                    "facetable": false,
                    "key": false,
                    "indexAnalyzer": null,
                    "searchAnalyzer": null,
                    "analyzer": null,
                    "synonymMaps": []
                },
                {
                    "name": "opinions",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "author",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": true,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": true,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        },
                        {
                            "name": "text",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": false,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": false,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        },
                        {
                            "name": "type",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": true,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": true,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        }
                    ]
                },
    . . .
   ```

1. İsteği yürütmek için **Gönder** ' e tıklayın.  Bir **durum almanız gerekir: 201 yanıt** olarak ileti oluşturdu.

<a name="create-data-source"></a>

## <a name="create-a-data-source"></a>Bir veri kaynağı oluşturun

İkinci istek Azure Blob depolamaya bağlanmak için [veri kaynağı oluşturma API](https://docs.microsoft.com/rest/api/searchservice/create-data-source) 'sini kullanır. 

1. URL `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/datasources?api-version=2019-05-06-Preview`'de, öğesini arama `YOUR-AZURE-SEARCH-SERVICE-NAME` hizmetinizin adıyla değiştirin. 

1. Üst bilgi bölümünde, Azure Search için `<YOUR AZURE SEARCH ADMIN API-KEY>` bir yönetici API anahtarı ile değiştirin.

1. Gövde bölümünde JSON belgesi, depolama hesabınızın bağlantı dizesini ve BLOB kapsayıcısı adını içerir. Bağlantı dizesi, depolama hesabınızın **erişim anahtarlarının**içindeki Azure Portal bulunabilir. 

    ```json
    {
        "name": "caselaw-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT>;AccountKey=<YOUR-STORAGE-KEY>;EndpointSuffix=core.windows.net"
        },
        "container": {
            "name": "<YOUR-BLOB-CONTAINER-NAME>",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

1. İsteği yürütmek için **Gönder** ' e tıklayın.  Bir **durum almanız gerekir: 201 yanıt** olarak ileti oluşturdu.



<a name="create-skillset"></a>

## <a name="create-a-skillset-and-knowledge-store"></a>Beceri ve bilgi deposu oluşturma

Üçüncü istek, [oluşturma BECERI API](https://docs.microsoft.com/rest/api/searchservice/create-skillset)'sini kullanarak, hangi bilişsel yeteneklerin çağrılacağını, yeteneklerin nasıl birlikte zincirleyeceğinizi ve bu izlenecek yol için en önemlisi (bilgi deposu belirtme) için bir Azure Search nesnesi oluşturur.

1. URL `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/skillsets?api-version=2019-05-06-Preview`'de, öğesini arama `YOUR-AZURE-SEARCH-SERVICE-NAME` hizmetinizin adıyla değiştirin. 

1. Üst bilgi bölümünde, Azure Search için `<YOUR AZURE SEARCH ADMIN API-KEY>` bir yönetici API anahtarı ile değiştirin.

1. Gövde bölümünde JSON belgesi bir beceri tanımıdır. Görünürlük için daraltılmış, bir beceri öğesinin dıştaki kabuğu aşağıdaki öğelerden oluşur. Koleksiyon, bellek içi zenginleştirme tanımlar, `knowledgeStore` ancak tanım çıktının nasıl depolandığını belirtir. `skills` `cognitiveServices` Tanım, AI zenginleştirme altyapılarıyla bağlantınız olur.

   ```json
   {
    "name": "caselaw-ss",
    "description": null,
    "skills": [],
    "cognitiveServices": [],
    "knowledgeStore": []
   }
   ```

1. Bağlantı `cognitiveServices` bilgilerini `knowledgeStore` sağlayabilmeniz için ve ' ı genişletin. Örnekte, bu dizeler beceri tanımından sonra, istek gövdesinin sonuna doğru bulunur. 

   İçin `cognitiveServices`, Azure Search ile aynı bölgede bulunan S0 katmanında bir kaynak sağlayın. Biliveservices adını ve anahtarını Azure portal aynı sayfadan alabilirsiniz. 
   
   İçin `knowledgeStore`, Caselaw blob kapsayıcısı için kullanılan bağlantı dizesini kullanabilirsiniz.

    ```json
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "YOUR-SAME-REGION-S0-COGNITIVE-SERVICES-RESOURCE",
        "key": "YOUR-COGNITIVE-SERVICES-KEY"
    },
    "knowledgeStore": {
        "storageConnectionString": "YOUR-STORAGE-ACCOUNT-CONNECTION-STRING",
    ```

1. Yetenekler koleksiyonunu, sırasıyla 85 ve 179 satırlarına göre belirli bir beceri olarak genişletin. Bilgi madenciliği için istediğiniz veri yapılarını bir şekilde ayrıştırır olduğundan, Beceri başına mil önemlidir. Beceri yürütme sırasında, bu yapılar yalnızca bellek içinde bulunur, ancak bir sonraki adıma geçtiğinizde, bu çıkışın daha fazla araştırma için bir bilgi deposuna nasıl kaydedileyebileceğinizi görürsünüz.

   Aşağıdaki kod parçacığı 217 satırından yapılır. 

    ```json
    "name": "Opinions",
    "source": null,
    "sourceContext": "/document/casebody/data/opinions/*",
    "inputs": [
        {
            "name": "Text",
            "source": "/document/casebody/data/opinions/*/text"
        },
        {
            "name": "Author",
            "source": "/document/casebody/data/opinions/*/author"
        },
        {
            "name": "Entities",
            "source": null,
            "sourceContext": "/document/casebody/data/opinions/*/text/pages/*/entities/*",
            "inputs": [
                {
                    "name": "Entity",
                    "source": "/document/casebody/data/opinions/*/text/pages/*/entities/*/value"
                },
                {
                    "name": "EntityType",
                    "source": "/document/casebody/data/opinions/*/text/pages/*/entities/*/category"
                }
            ]
        }
    ]
   . . .
   ```

1. 262 `projections` satırından başlayarak `knowledgeStore`öğesinde öğesini genişletin. Tahminler bilgi deposu birleşimini belirler. Projeksiyonlar tablolar-nesneler çiftlerinde, ancak şu anda yalnızca bir kez belirtilir. İlk projeksiyde görebileceğiniz gibi, `tables` belirtilir ancak `objects` değildir. İkincisi ise bunun tersidir.

   Azure depolama 'da tablolar, oluşturduğunuz her tablo için tablo depolamada oluşturulur ve her nesne blob depolamada bir kapsayıcı alır.

   Blob nesneleri genellikle bir zenginleştirme 'nin tam ifadesini içerir. Tablolar genellikle belirli amaçlar için düzenlediğiniz birleşimlerde kısmi zenginler içerir. Bu örnekte, bir servis talebi tablosu ve bir Oplanons tablosu gösterilmektedir, ancak gösterilmez; varlıklar, attorneys, yargılar ve taraflar gibi diğer tablolar.

    ```json
    "projections": [
        {
            "tables": [
                {
                    "tableName": "Cases",
                    "generatedKeyName": "CaseId",
                    "source": "/document/Case"
                },
                {
                    "tableName": "Opinions",
                    "generatedKeyName": "OpinionId",
                    "source": "/document/Case/OpinionsSnippets/*"
                }
            ],
            "objects": []
        },
        {
            "tables": [],
            "objects": [
                {
                    "storageContainer": "enrichedcases",
                    
                    "source": "/document/CaseFull"
                }
            ]
        }
    ]
    ```

1. İsteği yürütmek için **Gönder** ' e tıklayın. Yanıt **201** olmalıdır ve yanıtın ilk kısmını gösteren aşağıdaki örneğe benzer şekilde görünmelidir.

    ```json
    {
    "name": "caselaw-ss",
    "description": null,
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
            "name": "SplitSkill#1",
            "description": null,
            "context": "/document/casebody/data/opinions/*/text",
            "defaultLanguageCode": "en",
            "textSplitMode": "pages",
            "maximumPageLength": 5000,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/casebody/data/opinions/*/text
                }
            ],
            "outputs": [
                {
                    "name": "textItems",
                    "targetName": "pages"
                }
            ]
        },
        . . .
    ```

## <a name="create-and-run-an-indexer"></a>Dizin Oluşturucu oluşturma ve çalıştırma

Dördüncü istek, bir Azure Search Dizin Oluşturucu oluşturarak [Create ıNDEXER API](https://docs.microsoft.com/rest/api/searchservice/create-indexer)'sini kullanır. Dizin Oluşturucu, dizin oluşturma işlem hattının yürütme altyapısıdır. Şimdiye kadar oluşturduğunuz tüm tanımlar bu adımla hareket içine konur.

1. URL `https://YOUR-AZURE-SEARCH-SERVICE-NAME.search.windows.net/indexers?api-version=2019-05-06-Preview`'de, öğesini arama `YOUR-AZURE-SEARCH-SERVICE-NAME` hizmetinizin adıyla değiştirin. 

1. Üst bilgi bölümünde, Azure Search için `<YOUR AZURE SEARCH ADMIN API-KEY>` bir yönetici API anahtarı ile değiştirin.

1. Gövde bölümünde JSON belgesi, Dizin Oluşturucu adını belirtir. Dizin Oluşturucu için bir veri kaynağı ve dizin gereklidir. Bir beceri, bir Dizin Oluşturucu için isteğe bağlıdır, ancak AI zenginleştirme için gereklidir.

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": { },
        "fieldMappings": [],
        "outputFieldMappings": [ ]
    ```

1. OutputFieldMappings öğesini genişletin. Bir veri kaynağındaki alanlar ve dizindeki alanlar arasında özel eşleme için kullanılan fieldMappings 'nin aksine, outputFieldMappings, işlem hattı tarafından oluşturulan ve doldurulan ve bir dizin veya projeksiyonun çıktısını almak için kullanılır.

    ```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/people/*",
            "targetFieldName": "people",
            "mappingFunction": null
        },
        {
            "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/organizations/*",
            "targetFieldName": "orginizations",
            "mappingFunction": null
        },
        {
            "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/locations/*",
            "targetFieldName": "locations",
            "mappingFunction": null
        },
        {
            "sourceFieldName": "/document/Case/OpinionsSnippets/*/Entities/*",
            "targetFieldName": "entities",
            "mappingFunction": null
        },
        {
            "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/keyPhrases/*",
            "targetFieldName": "keyPhrases",
            "mappingFunction": null
        }
    ]
    ```

1. İsteği yürütmek için **Gönder** ' e tıklayın. Yanıt **201** olmalıdır ve yanıt gövdesi, verdiğiniz istek yüküyle neredeyse özdeş olmalıdır (breçekimi için kırpılmış).

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": { },
        "fieldMappings": [],
        "outputFieldMappings": [ ]
    }
    ```

## <a name="explore-knowledge-store"></a>Bilgi deposunu keşfet

İlk belge alındıktan hemen sonra keşfetmeye başlayabilirsiniz. Bu görev için portalda [**Depolama Gezgini**](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer) kullanın.

Bilgi deposunun Azure Search tamamen ayrıldığına dikkat etmek önemlidir. Azure Search dizini ve bilgi deposu her ikisi de veri temsili ve içerikleri içerir, ancak bunların bir parçası. Tam metin araması, filtrelenmiş arama ve Azure Search desteklenen tüm senaryolar için dizini kullanın. Ya da yalnızca bilgi deponuzda ileri gidin ve içerikleri çözümlemek için diğer araçları ekleyerek.

## <a name="takeaways"></a>Paketler

Azure Storage 'da ilk bilgi deponuzi oluşturdunuz ve Depolama Gezgini, zenginleştirme bilgilerini görüntülemek için kullandınız. Bu, depolanan enzenginler ile çalışmaya yönelik temel deneyimdir. 

## <a name="next-steps"></a>Sonraki adımlar

Her beceri, yeni şekillere birleştirilebilecek parçalı veri formları oluşturmaya yönelik ağır bir kaldırma yapar. Bir sonraki adım olarak, nasıl kullanıldığına ilişkin ayrıntılar için bu beceri için başvuru sayfasını gözden geçirin.

> [!div class="nextstepaction"]
> [Beceri başına mil başvurusu](cognitive-search-skill-shaper.md)


<!---
## Keep This

How to convert unformatted JSON into an indented JSON document structure that allows you to quickly identify nested structures. Useful for creating an index that includes complex types.

1. Use Visual Studio Code.
2. Open data.jsonl
--->
