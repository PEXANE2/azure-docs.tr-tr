---
title: 'REST öğreticisi: Bir AI zenginleştirme ardışık düzeninde bilişsel hizmetler çağırma-Azure Search'
description: Veri ayıklama için Azure Search dizin oluşturma, doğal dil ve görüntü AI işleme örneğinde, Postman ve REST API kullanarak JSON blob 'ları üzerinden dönüştürme işleminin bir örneğini adım adım yapın.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 05/28/2019
ms.author: luisca
ms.subservice: cognitive-search
ms.openlocfilehash: d431f0ced5b417e178e064dca347ae8d78f14e5d
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840883"
---
# <a name="rest-tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline"></a>REST öğreticisi: Azure Search dizin oluşturma ardışık düzeninde Bilişsel Hizmetler API'si çağrısı

Bu öğreticide, *bilişsel becerileri* kullanarak Azure Search’te programlama veri zenginleştirmesi mekanizmasını öğrenirsiniz. Yetenekler, bilişsel hizmetler 'de doğal dil işleme (NLP) ve görüntü analizi özellikleri tarafından desteklenir. Beceri kompozisyonu ve yapılandırması aracılığıyla bir görüntünün veya taranan belge dosyasının metin ve metin gösterimlerini ayıklayabilirsiniz. Ayrıca dili, varlıkları, anahtar tümcecikleri ve daha fazlasını tespit edebilirsiniz. Nihai sonuç, bir dizin oluşturma işlem hattında AI zenginleri ile oluşturulan bir Azure Search dizininde zengin ek içeriğe sahiptir. 

Bu öğreticide, aşağıdaki görevleri gerçekleştirmek için REST API çağrıları yaparsınız:

> [!div class="checklist"]
> * Bir dizine giden yolda örnek verileri zenginleştiren dizin oluşturma işlem hattı oluşturma
> * Yerleşik becerileri uygulama: varlık tanıma, dil algılama, metin işleme, anahtar tümcecik ayıklama
> * Bir beceri kümesindeki çıktılara girişleri eşleyerek becerilerin nasıl birbirine zincirleneceğini öğrenme
> * İstekleri yürütme ve sonuçları gözden geçirme
> * Daha fazla geliştirme için dizini ve dizin oluşturucuları sıfırlama

Çıktı, Azure Search üzerinde tam metin araması yapılabilir bir dizindir. [Eş anlamlılar](search-synonyms.md), [puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [çözümleyiciler](search-analyzers.md) ve [filtreler](search-filters.md) gibi diğer standart özelliklerle dizini geliştirebilirsiniz.

Bu öğretici ücretsiz hizmette çalışır, ancak ücretsiz işlem sayısı günde 20 belge ile sınırlıdır. Bu öğreticiyi aynı günde birden çok kez çalıştırmak istiyorsanız, daha fazla çalıştırmaya uyabilmeniz için daha küçük bir dosya kümesi kullanın.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel [Hizmetler kaynağı](cognitive-search-attach-cognitive-services.md)eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Search içinde belge çözme aşamasının bir parçası olarak görüntü ayıklama için tahakkuk ücretleri. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/)üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [Azure Search fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki hizmetler, Araçlar ve veriler bu öğreticide kullanılır. 

+ Örnek verileri depolamak için [bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) . Depolama hesabının Azure Search aynı bölgede bulunduğundan emin olun.

+ [Postman masaüstü uygulaması](https://www.getpostman.com/) , Azure Search için REST çağrıları yapmak için kullanılır.

+ [Örnek veriler](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) , küçük bir dosya kümesi farklı türlerden oluşur. 

+ Geçerli aboneliğinizde [bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

## <a name="get-a-key-and-url"></a>Anahtar ve URL al

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

![Http uç noktası ve erişim anahtarı al](media/search-get-started-postman/get-url-key.png "Http uç noktası ve erişim anahtarı al")

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

Zenginleştirme işlem hattı, Azure veri kaynaklarından çekme işlemi yapar. Kaynak veriler, [Azure Search dizin oluşturucunun](search-indexer-overview.md) desteklenen bir veri kaynağı türünden gelmelidir. Azure Tablo Depolaması bilişsel arama için desteklenmiyor. Bu alıştırmada, birden çok içerik türünü göstermek için blob depolama kullanırız.

1. [Azure Portal oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, Bloblar ' a tıklayınve ardından **+ Container**' a tıklayın.

1. Örnek veri içeren [bir blob kapsayıcısı oluşturun](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) . Ortak erişim düzeyini geçerli değerlerinden herhangi birine ayarlayabilirsiniz.

1. Kapsayıcı oluşturulduktan sonra dosyayı açın ve önceki bir adımda indirdiğiniz örnek dosyaları karşıya yüklemek için komut çubuğunda **karşıya yükle** ' yi seçin.

   ![Azure blob depolamadaki kaynak dosyalar](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Örnek dosyalar yüklendikten sonra Blob depolamanız için bir bağlantı dizesi ve kapsayıcı adını alın. Azure portalda depolama hesabınıza giderek bunu yapabilirsiniz. **Erişim anahtarları** bölümünde **Bağlantı Dizesi** alanını kopyalayın.

   Bağlantı dizesi, aşağıdaki örneğe benzer bir URL olmalıdır:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

Paylaşılan erişim imzası sağlama gibi, bağlantı dizesini belirtmenin başka birçok yolu vardır. Veri kaynağı kimlik bilgileri hakkında daha fazla bilgi edinmek için bkz. [Azure Blob Depolama Alanı dizinini oluşturma](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="set-up-postman"></a>Postman’i ayarlama

Postman’i başlatın ve bir HTTP isteği ayarlayın. Bu aracı hakkında bilginiz varsa bkz. [Postman kullanarak Azure Search REST API 'Leri araştırma](search-get-started-postman.md).

Bu öğreticide kullanılan istek metotları **gönderi**, **PUT**ve **Get**. Üst bilgi anahtarları "Content-Type", "Application/JSON" olarak ve bir "api-Key" olarak Azure Search hizmetinizin Yönetici anahtarına ayarlanır. Gövde, çağrınızın gerçek içeriklerini yerleştirdiğiniz yerdir. 

  ![Yarı yapılandırılmış arama](media/search-semi-structured-data/postmanoverview.png)

Veri kaynağı, Beceri, dizin ve Dizin Oluşturucu oluşturmak için arama hizmetinize dört API çağrısı yapmak üzere Postman kullanıyoruz. Veri kaynağı, depolama hesabınıza ve JSON verilerinize yönelik bir işaretçi içerir. Arama hizmetiniz, veriler yüklenirken bağlantı kurar.


## <a name="create-a-data-source"></a>Bir veri kaynağı oluşturun

Hizmetleriniz ve kaynak dosyalarınız hazırlandığına göre şimdi dizin oluşturma işlem hattınızın bileşenlerini derlemeye başlayabilirsiniz. Azure Search’e, dış kaynak verilerinin nasıl alınacağını bildiren bir [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) ile başlayın.

İstek üst bilgisinde, Azure Search hizmetini oluştururken kullandığınız hizmet adını ve arama hizmetiniz için oluşturulan api-anahtarını sağlayın. İstek gövdesinde, blob kapsayıcı adını ve bağlantı dizesini belirtin.

### <a name="sample-request"></a>Örnek İstek
```http
POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]
```
#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
```json
{
  "name" : "demodata",
  "description" : "Demo files to demonstrate cognitive search capabilities.",
  "type" : "azureblob",
  "credentials" :
  { "connectionString" :
    "DefaultEndpointsProtocol=https;AccountName=<your account name>;AccountKey=<your account key>;"
  },
  "container" : { "name" : "<your blob container name>" }
}
```
İsteği gönderin. Web testi aracı, 201 durum kodunu döndürerek işlemin başarılı olduğunu onaylamalıdır. 

Bu ilk isteğiniz olduğundan, veri kaynağının Azure Search’te oluşturulduğunu onaylamak için Azure portalını denetleyin. Arama hizmeti panosu sayfasında, veri kaynakları listesinin yeni bir öğe olduğunu doğrulayın. Portal sayfasının yenilenmesi için birkaç dakika beklemeniz gerekebilir. 

  ![Portaldaki veri kaynakları kutucuğu](./media/cognitive-search-tutorial-blob/data-source-tile.png "Portaldaki veri kaynakları kutucuğu")

Bir 403 veya 404 hatası aldıysanız, istek yapısını denetleyin: `api-version=2019-05-06`, uç nokta üzerinde olmalıdır, `api-key`, Üst bilgide `Content-Type` öğesinden sonra gelmelidir ve değeri bir arama hizmeti için geçerli olmalıdır. Bu öğreticide kalan adımlar için üst bilgiyi kullanabilirsiniz.

## <a name="create-a-skillset"></a>Beceri kümesi oluşturma

Bu adımda, verilerinize uygulamak istediğiniz bir zenginleştirme adımları kümesini tanımlarsınız. Her zenginleştirme adımını *beceri* olarak ve zenginleştirme adımları kümesini de *beceri kümesi* olarak adlandırabilirsiniz. Bu öğretici, beceri için yerleşik bilişsel [becerileri](cognitive-search-predefined-skills.md) kullanır:

+ İçeriğin dilini tanımlamak için [Dil Algılama](cognitive-search-skill-language-detection.md).

+ Anahtar tümcecik ayıklama becerisini çağırmadan önce büyük içeriği daha küçük öbeklere ayırmak için [Metni Böl](cognitive-search-skill-textsplit.md). Anahtar tümcecik ayıklama, 50.000 veya daha az karakterden oluşan girişi kabul eder. Bu sınıra uymak için örnek dosyaların birkaç tanesinin bölünmesi gerekir.

+ Blob kapsayıcısında bulunan içerikten kuruluşların adlarını ayıklamak için [varlık tanıma](cognitive-search-skill-entity-recognition.md) .

+ Üst anahtar tümcecikleri çekmek için [Anahtar İfade Ayıklama](cognitive-search-skill-keyphrases.md). 

### <a name="sample-request"></a>Örnek İstek
Bu REST çağrısını yapmadan önce, aracınız çağrılar arasında istek üst bilgisini korumuyorsa aşağıdaki istekte yer alan hizmet adını ve yönetici anahtarını değiştirmeyi unutmayın. 

Bu istek bir beceri kümesi oluşturur. Bu öğreticinin geri kalanında beceri kümesi adı ```demoskillset``` olarak ifade edilir.

```http
PUT https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
```json
{
  "description":
  "Extract entities, detect language and extract key-phrases",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations", "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "languageCode",
          "targetName": "languageCode"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
      "textSplitMode" : "pages",
      "maximumPageLength": 4000,
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        },
        {
          "name": "languageCode",
          "source": "/document/languageCode"
        }
      ],
      "outputs": [
        {
          "name": "textItems",
          "targetName": "pages"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
      "context": "/document/pages/*",
      "inputs": [
        {
          "name": "text", "source": "/document/pages/*"
        },
        {
          "name":"languageCode", "source": "/document/languageCode"
        }
      ],
      "outputs": [
        {
          "name": "keyPhrases",
          "targetName": "keyPhrases"
        }
      ]
    }
  ]
}
```

İsteği gönderin. Web testi aracı, 201 durum kodunu döndürerek işlemin başarılı olduğunu onaylamalıdır. 

#### <a name="explore-the-request-body"></a>İstek gövdesini keşfetme

Her bir sayfa için anahtar tümcecik ayıklama becerisinin nasıl uygulandığına dikkat edin. Bağlamı ```"document/pages/*"``` olarak ayarlayarak, belge/sayfa dizisinin her üyesi (belgedeki her sayfa) için bu zenginleştiriciyi çalıştırırsınız.

Her beceri, belge içeriğinde yürütülür. İşleme sırasında Azure Search, farklı dosya biçimlerinden içerik okumak için her bir belgeyi çözer. Kaynak dosyadan gelen, bulunan metin, oluşturulan ```content``` alanına her belge için birer birer yerleştirilir. Bu nedenle, girişi ```"/document/content"``` olarak ayarlayın.

Beceri kümesinin grafiksel gösterimi aşağıda gösterilmektedir. 

![Beceri kümesini anlama](media/cognitive-search-tutorial-blob/skillset.png "Beceri kümesini anlama")

Çıktılar bir dizine eşlenebilir, aşağı akış becerisine yönelik giriş olarak kullanılır veya dil kodunda olduğu gibi her iki şekilde de kullanılabilir. Dizinde bir dil kodu, filtreleme için yararlıdır. Giriş olarak dil kodu, sözcük bölünmesiyle ilgili dilbilgisi kurallarını bildirmek için metin analizi becerileri tarafından kullanılır.

Beceri kümesi temelleri hakkında daha fazla bilgi için bkz. [Beceri kümesini tanımlama](cognitive-search-defining-skillset.md).

## <a name="create-an-index"></a>Dizin oluşturma

Bu bölümde, aranabilir dizine dahil edilecek alanları ve her bir alana ilişkin arama özniteliklerini belirterek dizin şemasını tanımlarsınız. Alanlar bir türe sahiptir ve alanın nasıl kullanıldığını (aranabilir, sıralanabilir vb.) belirleyen öznitelikleri alabilir. Bir dizindeki alan adlarının, kaynaktaki alan adlarıyla tamamen aynı olması gerekmez. Sonraki bir adımda, kaynak-hedef alanlarını bağlamak için dizin oluşturucuda alan eşlemeleri eklersiniz. Bu adım için, arama uygulamanızla ilgili alan adlandırma kurallarını kullanarak dizini tanımlayın.

Bu çalışmada aşağıdaki alanlar ve alan türleri kullanılır:

| alan adları: | `id`       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="sample-request"></a>Örnek İstek
Bu REST çağrısını yapmadan önce, aracınız çağrılar arasında istek üst bilgisini korumuyorsa aşağıdaki istekte yer alan hizmet adını ve yönetici anahtarını değiştirmeyi unutmayın. 

Bu istek bir dizin oluşturur. Bu öğreticinin geri kalanında ```demoindex``` dizin adını kullanın.

```http
PUT https://[servicename].search.windows.net/indexes/demoindex?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi

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
      "name": "organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```
İsteği gönderin. Web testi aracı, 201 durum kodunu döndürerek işlemin başarılı olduğunu onaylamalıdır. 

Dizin tanımlama hakkında daha fazla bilgi için bkz. [Dizin Oluşturma (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).


## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Dizin oluşturucu oluşturma, alanları eşleme ve dönüştürmeler yürütme

Şu ana kadar bir veri kaynağı, beceri kümesi ve dizin oluşturdunuz. Bu üç bileşen, her bir parçayı birlikte tek bir çok aşamalı işleme çeken [dizin oluşturucunun](search-indexer-overview.md) parçası olur. Bunları bir dizin oluşturucu birbirine bağlamak için alan eşlemeleri tanımlamanız gerekir. 

+ FieldMappings, Beceri öğesinden önce işlenir, kaynak alanları veri kaynağından bir dizindeki hedef alanlara eşleniyor. Alan adları ve türleri her iki uçta da aynıysa, hiçbir eşleme gerekmez.

+ OutputFieldMappings, Beceri sonrasında işlenir ve belge çözme veya zenginleştirene kadar mevcut olmayan sourceFieldNames öğesine başvuruda bulunur. TargetFieldName, dizindeki bir alandır.

Çıkışlara girişlerin takılmasının yanı sıra, veri yapılarını düzleştirmek için alan eşlemelerini de kullanabilirsiniz. Daha fazla bilgi için bkz. [zenginleştirilmiş alanları aranabilir bir dizine eşleme](cognitive-search-output-field-mapping.md).

### <a name="sample-request"></a>Örnek İstek

Bu REST çağrısını yapmadan önce, aracınız çağrılar arasında istek üst bilgisini korumuyorsa aşağıdaki istekte yer alan hizmet adını ve yönetici anahtarını değiştirmeyi unutmayın. 

Ayrıca, dizin oluşturucunuzun adını sağlayın. Bu öğreticinin geri kalanında bunu ```demoindexer``` olarak ifade edebilirsiniz.

```http
PUT https://[servicename].search.windows.net/indexers/demoindexer?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
    {
      "sourceFieldName" : "metadata_storage_path",
      "targetFieldName" : "id",
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
      "sourceFieldName" : "/document/organizations",
      "targetFieldName" : "organizations"
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
      "imageAction": "generateNormalizedImages"
    }
  }
}
```

İsteği gönderin. Web testi aracı, 201 durum kodunu döndürerek işlemin başarılı olduğunu onaylamalıdır. 

Bu adımın tamamlanması birkaç dakika sürebilir. Veri kümesi küçük olsa da, analiz becerileri bilgi işlem açısından yoğundur. Görüntü analizi gibi bazı beceriler uzun sürer.

> [!TIP]
> Bir dizin oluşturucu oluşturulduğunda, işlem hattı çağrılır. Verilere ulaşılırken, eşleme girişleri ve çıktıları veya işlemlerin sırası ile ilgili sorun olursa bunlar bu aşamada görüntülenir. İşlem hattını, kod veya betik değişiklikleriyle yeniden çalıştırmak için önce nesneleri bırakmanız gerekebilir. Daha fazla bilgi için bkz. [Sıfırlama ve yeniden çalıştırma](#reset).

#### <a name="explore-the-request-body"></a>İstek gövdesini keşfetme

Betik, ```"maxFailedItems"``` değerini -1 olarak ayarlayarak dizin oluşturma motoruna, veri içeri aktarma sırasında hataları yoksaymasını bildirir. Demo veri kaynağında çok az belge olduğundan bu yararlıdır. Daha büyük bir veri kaynağı için değeri, 0’dan daha büyük bir değere ayarlarsınız.

Ayrıca yapılandırma parametrelerinde ```"dataToExtract":"contentAndMetadata"``` deyimine de dikkat edin. Bu deyim, dizin oluşturucuya, farklı dosya biçimlerinden içeriği ve her bir dosyayla ilgili meta verileri otomatik olarak ayıklamasını bildirir. 

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metni ayıklamak için ```imageAction``` değerini ayarlayabilirsiniz. ```"imageAction":"generateNormalizedImages"``` Yapılandırma, OCR becerisi ve metin birleştirme beceriyle birlikte kullanıldığında, dizin oluşturucunun görüntülerden metin ayıklamasını söyler (örneğin, trafiğin "Durdur" sözcüğünün oturum açmasını durdurur) ve içerik alanının bir parçası olarak katıştırmasını söyler. Bu davranış hem belgelerde gömülü olan görüntüler (örneğin, bir PDF’teki görüntü) hem de veri kaynağında bulunan görüntüler (örneğin, bir JPG dosyası) için geçerlidir.

## <a name="check-indexer-status"></a>Dizin oluşturucu durumunu denetleme

Dizin oluşturucu tanımlandıktan sonra, isteği gönderdiğinizde otomatik olarak çalıştırılır. Tanımladığınız bilişsel becerilere bağlı olarak dizin oluşturma beklediğinizden uzun sürebilir. Dizin oluşturucunun halen çalışıp çalışmadığını öğrenmek için aşağıdaki isteği göndererek dizin oluşturucu durumunu denetleyin.

```http
GET https://[servicename].search.windows.net/indexers/demoindexer/status?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

Yanıt, dizin oluşturucunun çalışıp çalışmadığını size bildirir. Dizin oluşturma işlemi tamamlandıktan sonra, STATUS uç noktasına yönelik başka bir HTTP GET kullanarak, zenginleştirme sırasında oluşan hata ve uyarıların raporlarını görün.  

Uyarılar bazı kaynak dosya ve beceri birleşimleri için geneldir ve her zaman bir sorunu belirtmez. Bu öğreticide, uyarılar zararsızdır (örneğin, JPEG dosyalarında bir metin girişi yok). Dizin oluşturma sırasında oluşan uyarılar hakkında ayrıntılı bilgi için durum yanıtını gözden geçirebilirsiniz.
 
## <a name="query-your-index"></a>Dizininizi sorgulama

Dizin oluşturma işlemi tamamlandıktan sonra tek tek alanların içeriklerini döndüren sorgular çalıştırın. Azure Search varsayılan olarak ilk 50 sonucu döndürür. Varsayılan değerin düzgün çalışması için örnek veriler küçüktür. Ancak, büyük veri kümeleriyle çalışırken, daha fazla sonuç döndürmek için sorgu dizesine parametreleri dahil etmeniz gerekebilir. Yönergeler için bkz. [Azure Search’te sonuçların sayfasını oluşturma](search-pagination-page-layout.md).

Doğrulama adımı olarak, tüm alanlar için dizini sorgulayın.

```http
GET https://[servicename].search.windows.net/indexes/demoindex?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

Çıktı, her bir alanın adını, türünü ve özniteliklerini içeren dizin şemasıdır.

`organizations` gibi tek bir alanın tüm içeriklerini döndürmek için ikinci bir `"*"` sorgusu gönderin.

```http
GET https://[servicename].search.windows.net/indexes/demoindex/docs?search=*&$select=organizations&api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

Bu alıştırmada içerik, languageCode, keyPhrases ve kuruluşlar için yineleyin. Virgülle ayrılmış bir liste kullanarak `$select` aracılığıyla birden fazla alan döndürebilirsiniz.

Sorgu dizesinin karmaşıklık ve uzunluğuna bağlı olarak GET veya POST dizesini kullanabilirsiniz. Daha fazla bilgi için bkz. [REST API kullanarak sorgu](https://docs.microsoft.com/rest/api/searchservice/search-documents).



<a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

İşlem hattı geliştirmenin ilk deneme aşamalarında, tasarım yinelemeleri için en pratik yaklaşım, Azure Search’ten nesneleri silmek ve kodunuzun bunları yeniden oluşturmasını sağlamaktır. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Yeni tanımlarla belgelerinizin yeniden dizinini oluşturmak için:

1. Dizini silerek kalıcı verileri kaldırın. Dizin oluşturucuyu silerek hizmetinizde yeniden oluşturun.
2. Bir beceri kümesi ve dizin tanımını değiştirin.
3. İşlem hattını çalıştırmak için hizmet üzerinde yeniden bir dizin ve dizin oluşturucu oluşturun. 

Dizinleri, Dizin oluşturucuyu ve becerileri silmek için portalı kullanabilirsiniz.

```http
DELETE https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2019-05-06
api-key: [api-key]
Content-Type: application/json
```

Silme işlemi başarılı olduğunda durum kodu 204 döndürülür.

Kodunuz geliştikçe bir yeniden derleme stratejisini iyileştirmek isteyebilirsiniz. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

## <a name="takeaways"></a>Paketler

Bu öğreticide, veri kaynağı, beceri kümesi, dizin ve dizin oluşturucu gibi bileşen parçalarının oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma işlem hattı oluşturmaya yönelik temel adımlar gösterilmektedir.

Girişler ve çıktılar yoluyla becerileri zincirleme mekanizmalarının ve beceri kümesi tanımının yanı sıra [önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) sunulmuştur. İşlem hattındaki zenginleştirilmiş değerleri bir Azure Search hizmetinde aranabilir bir dizine yönlendirmek için dizin oluşturucu tanımında `outputFieldMappings` gerektiğini öğrendiniz.

Son olarak, daha fazla yineleme için sonuçların nasıl test edileceğini ve sistemin nasıl sıfırlanacağını öğrendiniz. Dizine karşı sorgular düzenlendiğinde, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktının döndürüldüğünü öğrendiniz. Bu yayında, iç yapıları (sistem tarafından oluşturulan zenginleştirilmiş belgeler) görüntüleme mekanizması vardır. Ayrıca dizin oluşturucu durumunun nasıl denetleneceğini ve işlem hattı yeniden çalıştırılmadan önce hangi nesnelerin silineceğini de öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir öğretici tamamlandıktan sonra temizlemenin en hızlı yolu, Azure Search hizmetini ve Azure Blob hizmetini içeren kaynak grubunu silmektir. Her iki hizmeti de aynı gruba koyduğunuz varsayılarak, şimdi bu öğreticide oluşturduğunuz depolanan içerikler ve hizmetler de dahil olmak üzere, kaynak grubunun içindeki her şeyi silmek için kaynak grubunu silin. Portalda kaynak grubu adı, her bir hizmetin Genel Bakış sayfasındadır.

## <a name="next-steps"></a>Sonraki adımlar

Özel becerilerle işlem hattını özelleştirin veya genişletin. Özel bir beceri oluşturup bir beceri kümesine eklemeniz, kendi yazdığınız metin veya görüntü analizini eklemenize olanak sağlar. 

> [!div class="nextstepaction"]
> [Örnek: Bilişsel arama için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)
