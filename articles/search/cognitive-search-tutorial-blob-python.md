---
title: 'Öğretici: Python ve AI Azure lekeleri üzerinde'
titleSuffix: Azure Cognitive Search
description: Jupyter Python dizüstü bilgisayar ve Azure Bilişsel Arama REST API'leri kullanarak Blob depolamadaki içerik üzerinden metin çıkarma ve doğal dil işleme örneğini gözden geçirin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: python
ms.topic: tutorial
ms.date: 02/26/2020
ms.openlocfilehash: e7708b0043b7f5baf2c12e813306595cc358a01d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78194063"
---
# <a name="tutorial-use-python-and-ai-to-generate-searchable-content-from-azure-blobs"></a>Öğretici: Azure lekelerinden aranabilir içerik oluşturmak için Python ve AI'yi kullanın

Azure Blob depolama alanında yapılandırılmamış metin veya resimlervarsa, bir [AI zenginleştirme ardışık aygıtı](cognitive-search-concept-intro.md) bilgi ayıklayabilir ve tam metin arama veya bilgi madenciliği senaryoları için yararlı olan yeni içerik oluşturabilir. Bir ardışık işlem hattı görüntüleri işleyebilir, ancak bu Python öğreticisi, sorgularda, farklı şekillerde ve filtrelerde yararlanabileceğiniz yeni alanlar oluşturmak için dil algılama ve doğal dil işleme uygulayarak metne odaklanır.

Bu öğretici, aşağıdaki görevleri gerçekleştirmek için Python ve [Search REST API'larını](https://docs.microsoft.com/rest/api/searchservice/) kullanır:

> [!div class="checklist"]
> * Azure Blob depolama alanında PDF, HTML, DOCX ve PPTX gibi tüm belgelerle (yapılandırılmamış metin) başlayın.
> * Metni ayıklayan, dili algılayan, varlıkları tanıyan ve anahtar tümcecikleri algılayan bir ardışık sözcük tanımla.
> * Çıktıyı depolamak için bir dizin tanımlayın (ham içerik, artı boru hattı tarafından oluşturulan ad değeri çiftleri).
> * Dönüşümleri ve çözümlemesi başlatmak ve dizini oluşturmak ve yüklemek için ardışık hattı çalıştırın.
> * Tam metin aramave zengin bir sorgu sözdizimini kullanarak sonuçları keşfedin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap açın.

## <a name="prerequisites"></a>Ön koşullar

+ [Azure Depolama](https://azure.microsoft.com/services/storage/)
+ [Anaconda 3.7](https://www.anaconda.com/distribution/#download-section)
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

## <a name="2---start-a-notebook"></a>2 - Not defteri başlatın

Aşağıdaki yönergeleri kullanarak not defterini oluşturun veya [Azure-Search-python örnekleri repo'sundan](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Tutorial-AI-Enrichment)tamamlanmış bir not defteri indirin.

Jupyter Notebook'u başlatmak ve yeni bir Python 3 dizüstü bilgisayar oluşturmak için Anaconda Navigator'ı kullanın.

Not defterinizde, JSON ile çalışmak ve HTTP isteklerini formüle etmek için kullanılan kitaplıkları yüklemek için bu komut dosyasını çalıştırın.

```python
import json
import requests
from pprint import pprint
```

Aynı not defterinde, veri kaynağı, dizin, dizin leyici ve skillset adlarını tanımlayın. Bu öğreticinin adlarını ayarlamak için bu komut dosyasını çalıştırın.

```python
# Define the names for the data source, skillset, index and indexer
datasource_name = "cogsrch-py-datasource"
skillset_name = "cogsrch-py-skillset"
index_name = "cogsrch-py-index"
indexer_name = "cogsrch-py-indexer"
```

Aşağıdaki komut dosyasında, arama hizmetiniz (YOUR-SEARCH-SERVICE-NAME) ve yönetici API anahtarı (YOUR-ADMIN-API-KEY) için yer tutucuları değiştirin ve ardından arama hizmeti bitiş noktasını ayarlamak için çalıştırın.

```python
# Setup the endpoint
endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/'
headers = {'Content-Type': 'application/json',
           'api-key': '<YOUR-ADMIN-API-KEY>'}
params = {
    'api-version': '2019-05-06'
}
```

## <a name="3---create-the-pipeline"></a>3 - Boru hattını oluşturun

Azure Bilişsel Arama'da, AI işleme dizin oluşturma (veya veri alımı) sırasında gerçekleşir. İzleyicinin bu bölümü dört nesne oluşturur: veri kaynağı, dizin tanımı, skillset, indexer. 

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

Veri [kaynağı nesnesi,](https://docs.microsoft.com/rest/api/searchservice/create-data-source) dosyaları içeren Blob kapsayıcısına bağlantı dizesini sağlar.

Aşağıdaki komut dosyasında, yer tutucuyour-BLOB-RESOURCE-CONNECTION-STRING'i önceki adımda oluşturduğunuz blob için bağlantı dizesiyle değiştirin. Kapsayıcı için yer tutucu metnini değiştirin. Ardından, komut dosyasını çalıştırıp `cogsrch-py-datasource`bir veri kaynağı oluşturarak .

```python
# Create a data source
datasourceConnectionString = "<YOUR-BLOB-RESOURCE-CONNECTION-STRING>"
datasource_payload = {
    "name": datasource_name,
    "description": "Demo files to demonstrate cognitive search capabilities.",
    "type": "azureblob",
    "credentials": {
        "connectionString": datasourceConnectionString
    },
    "container": {
        "name": "<YOUR-BLOB-CONTAINER-NAME>"
    }
}
r = requests.put(endpoint + "/datasources/" + datasource_name,
                 data=json.dumps(datasource_payload), headers=headers, params=params)
print(r.status_code)
```

İstek, başarıyı onaylayan 201 durum kodunu döndürmelidir.

Azure portalında, arama hizmeti panosu sayfasında, cogsrch-py-datasource'un **Veri kaynakları** listesinde göründüğünü doğrulayın. Sayfayı güncellemek için **Yenile'yi** tıklatın.

![Portaldaki veri kaynakları döşemesi](./media/cognitive-search-tutorial-blob-python/py-data-source-tile.png "Portaldaki veri kaynakları döşemesi")

### <a name="step-2-create-a-skillset"></a>Adım 2: Bir beceri oluşturun

Bu adımda, verilerinize uygulamak için bir dizi zenginleştirme adımı tanımlarsınız. Her zenginleştirme adımını *beceri* olarak ve zenginleştirme adımları kümesini de *beceri kümesi* olarak adlandırabilirsiniz. Bu öğretici, skillset için [yerleşik bilişsel becerileri](cognitive-search-predefined-skills.md) kullanır:

+ Blob kapsayıcısındaki içerikten kuruluşların adlarını ayıklamak için [Varlık Tanıma.](cognitive-search-skill-entity-recognition.md)

+ İçeriğin dilini tanımlamak için [Dil Algılama](cognitive-search-skill-language-detection.md).

+ Anahtar tümcecik ayıklama becerisini çağırmadan önce büyük içeriği daha küçük öbeklere ayırmak için [Metni Böl](cognitive-search-skill-textsplit.md). Anahtar tümcecik ayıklama, 50.000 veya daha az karakterden oluşan girişi kabul eder. Bu sınıra uymak için örnek dosyaların birkaç tanesinin bölünmesi gerekir.

+ Üst anahtar tümcecikleri çekmek için [Anahtar İfade Ayıklama](cognitive-search-skill-keyphrases.md). 

Adlı `cogsrch-py-skillset`bir beceri oluşturmak için aşağıdaki komut dosyasını çalıştırın.

```python
# Create a skillset
skillset_payload = {
    "name": skillset_name,
    "description":
    "Extract entities, detect language and extract key-phrases",
    "skills":
    [
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": ["Organization"],
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
            "textSplitMode": "pages",
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
                    "name": "languageCode", "source": "/document/languageCode"
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

r = requests.put(endpoint + "/skillsets/" + skillset_name,
                 data=json.dumps(skillset_payload), headers=headers, params=params)
print(r.status_code)
```

İstek, başarıyı onaylayan 201 durum kodunu döndürmelidir.

Anahtar tümcecik çıkarma becerisi her sayfa için uygulanır. Bağlamı `"document/pages/*"`ayarlayarak, belge/sayfa dizisinin her üyesi (belgedeki her sayfa için) için bu zenginleştiriciyi çalıştırın.

Her beceri, belge içeriğinde yürütülür. İşlem sırasında Azure Bilişsel Arama, farklı dosya biçimlerinden gelen içeriği okumak için her belgeyi çatlaklar. Kaynak dosyada bulunan metin, `content` her belge için bir alan içine yerleştirilir. Bu nedenle, girişi `"/document/content"`.

Beceri kümesinin grafiksel gösterimi aşağıda gösterilmektedir.

![Bir skillset'i anlama](media/cognitive-search-tutorial-blob/skillset.png "Bir skillset'i anlama")

Çıktılar, dil kodunda olduğu gibi, akış aşağı sına giriş olarak kullanılan bir dizin veya her ikisine de eşlenebilir. Dizinde bir dil kodu, filtreleme için yararlıdır. Giriş olarak dil kodu, sözcük bölünmesiyle ilgili dilbilgisi kurallarını bildirmek için metin analizi becerileri tarafından kullanılır.

Beceri kümesi temelleri hakkında daha fazla bilgi için bkz. [Beceri kümesini tanımlama](cognitive-search-defining-skillset.md).

### <a name="step-3-create-an-index"></a>Adım 3: Dizin oluşturma

Bu bölümde, aranabilir dizine dahil olacak alanları belirterek ve her alan için arama özniteliklerini ayarlayarak dizin şemasını tanımlarsınız. Alanlar bir türe sahiptir ve alanın nasıl kullanıldığını (aranabilir, sıralanabilir vb.) belirleyen öznitelikleri alabilir. Bir dizindeki alan adlarının, kaynaktaki alan adlarıyla tamamen aynı olması gerekmez. Sonraki bir adımda, kaynak-hedef alanlarını bağlamak için dizin oluşturucuda alan eşlemeleri eklersiniz. Bu adım için, arama uygulamanızla ilgili alan adlandırma kurallarını kullanarak dizini tanımlayın.

Bu çalışmada aşağıdaki alanlar ve alan türleri kullanılır:

| alan adları: | id         | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |

Adlı dizini oluşturmak için `cogsrch-py-index`bu komut dosyasını çalıştırın.

```python
# Create an index
index_payload = {
    "name": index_name,
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": "true",
            "searchable": "true",
            "filterable": "false",
            "facetable": "false",
            "sortable": "true"
        },
        {
            "name": "content",
            "type": "Edm.String",
            "sortable": "false",
            "searchable": "true",
            "filterable": "false",
            "facetable": "false"
        },
        {
            "name": "languageCode",
            "type": "Edm.String",
            "searchable": "true",
            "filterable": "false",
            "facetable": "false"
        },
        {
            "name": "keyPhrases",
            "type": "Collection(Edm.String)",
            "searchable": "true",
            "filterable": "false",
            "facetable": "false"
        },
        {
            "name": "organizations",
            "type": "Collection(Edm.String)",
            "searchable": "true",
            "sortable": "false",
            "filterable": "false",
            "facetable": "false"
        }
    ]
}

r = requests.put(endpoint + "/indexes/" + index_name,
                 data=json.dumps(index_payload), headers=headers, params=params)
print(r.status_code)
```

İstek, başarıyı onaylayan 201 durum kodunu döndürmelidir.

Bir dizin tanımlama hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-4-create-and-run-an-indexer"></a>Adım 4: Bir dizin oluştur ve çalıştır

Bir [Dizinleyici,](https://docs.microsoft.com/rest/api/searchservice/create-indexer) ardışık hattı kullanır. Şimdiye kadar oluşturduğunuz üç bileşen (veri kaynağı, skillset, index) bir dizin oluşturucuya girişlerdir. Azure Bilişsel Arama'da dizin oluşturucu oluşturmak, tüm ardışık ardışık alanı harekete dönüştüren olaydır. 

Bu nesneleri bir dizinleyicide birbirine bağlamak için alan eşlemelerini tanımlamanız gerekir.

+ Alan Haritalamalar skillset önce işlenir, bir dizindeki hedef alanlara veri kaynağından kaynak alanları eşleme. Alan adları ve türleri her iki uçta da aynıysa, eşleme gerekmez.

+ OutputFieldMappings skillset sonra işlenir, belge çatlama veya zenginleştirme onları oluşturur kadar var olmayan sourceFieldNames başvurulan. TargetFieldName bir dizinde bir alandır.

Girdileri çıktılara bağlamanın yanı sıra, veri yapılarını düzleştirmek için alan eşlemelerini de kullanabilirsiniz. Daha fazla bilgi [için, zenginleştirilmiş alanları aranabilir bir dizine nasıl eşlendirilir'](cognitive-search-output-field-mapping.md)e bakın.

Adlı bir dizin oluşturacak `cogsrch-py-indexer`şekilde bu komut dosyasını çalıştırın.

```python
# Create an indexer
indexer_payload = {
    "name": indexer_name,
    "dataSourceName": datasource_name,
    "targetIndexName": index_name,
    "skillsetName": skillset_name,
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction":
            {"name": "base64Encode"}
        },
        {
            "sourceFieldName": "content",
            "targetFieldName": "content"
        }
    ],
    "outputFieldMappings":
    [
        {
            "sourceFieldName": "/document/organizations",
            "targetFieldName": "organizations"
        },
        {
            "sourceFieldName": "/document/pages/*/keyPhrases/*",
            "targetFieldName": "keyPhrases"
        },
        {
            "sourceFieldName": "/document/languageCode",
            "targetFieldName": "languageCode"
        }
    ],
    "parameters":
    {
        "maxFailedItems": -1,
        "maxFailedItemsPerBatch": -1,
        "configuration":
        {
            "dataToExtract": "contentAndMetadata",
            "imageAction": "generateNormalizedImages"
        }
    }
}

r = requests.put(endpoint + "/indexers/" + indexer_name,
                 data=json.dumps(indexer_payload), headers=headers, params=params)
print(r.status_code)
```

İstek yakında 201 bir durum kodu döndürmelidir, ancak, işleme tamamlanması birkaç dakika sürebilir. Veri kümesi küçük olmasına rağmen, görüntü analizi gibi analitik beceriler hesaplama lı olarak yoğundur ve zaman alır.

Dizinleyicinin ne zaman çalıştığını veya ne zaman tamamlanacasını belirlemek için [dizinleyici durumunu izleyebilirsiniz.](#check-indexer-status)

> [!TIP]
> Bir dizin oluşturucu oluşturulduğunda, işlem hattı çağrılır. Verilere erişimi, giriş ve çıktıları eşlemede veya işlem sırasına göre bir sorun varsa, bu aşamada görünür. Kod veya komut dosyası değişiklikleriyle ardışık hattı yeniden çalıştırmak için önce nesneleri silmeniz gerekebilir. Daha fazla bilgi için bkz. [Sıfırlama ve yeniden çalıştırma](#reset).

#### <a name="about-the-request-body"></a>İstek gövdesi hakkında

Betik, `"maxFailedItems"` değerini -1 olarak ayarlayarak dizin oluşturma motoruna, veri içeri aktarma sırasında hataları yoksaymasını bildirir. Demo veri kaynağında çok az belge olduğundan bu yararlıdır. Daha büyük bir veri kaynağı için değeri, 0’dan daha büyük bir değere ayarlarsınız.

Ayrıca yapılandırma parametrelerinde `"dataToExtract":"contentAndMetadata"` deyimine de dikkat edin. Bu deyim, dizinleyiciye içeriği farklı dosya biçimlerinden ve her dosyayla ilgili meta verilerden ayıklamasını söyler.

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metni ayıklamak için `imageAction` değerini ayarlayabilirsiniz. Yapılandırma, `"imageAction":"generateNormalizedImages"` OCR Beceri ve Metin Birleştirme Becerisi ile birlikte, dizinleyiciye resimlerden metin ayıklamasını (örneğin, trafik Durdur işaretinden "dur" sözcüğü) ayıklamasını ve içerik alanının bir parçası olarak gömmesini söyler. Bu davranış, hem belgelere katıştırılmış görüntüler (PDF içindeki bir görüntüyü düşünün) hem de veri kaynağında bulunan görüntüler ( örneğin bir JPG dosyası için geçerlidir.

<a name="check-indexer-status"></a>

## <a name="4---monitor-indexing"></a>4 - İzleme indeksleme

Dizin oluşturucu tanımlandıktan sonra, isteği gönderdiğinizde otomatik olarak çalıştırılır. Tanımladığınız bilişsel becerilere bağlı olarak dizin oluşturma beklediğinizden uzun sürebilir. Dizin leyici işlemenin tamamlanıp tamamlanmadığını öğrenmek için aşağıdaki komut dosyasını çalıştırın.

```python
# Get indexer status
r = requests.get(endpoint + "/indexers/" + indexer_name +
                 "/status", headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Yanıtta, "durum" ve "endTime" değerleri için "lastResult"ı izleyin. Durumu denetlemek için komut dosyasını düzenli aralıklarla çalıştırın. Dizinleyici tamamlandığında, durum "başarı" olarak ayarlanır, bir "endTime" belirtilir ve yanıt zenginleştirme sırasında oluşan hataları ve uyarıları içerir.

![Dizin oluşturucu oluşturuldu](./media/cognitive-search-tutorial-blob-python/py-indexer-is-created.png "Dizin oluşturucu oluşturuldu")

Uyarılar bazı kaynak dosya ve beceri birleşimleri için geneldir ve her zaman bir sorunu belirtmez. Birçok uyarı iyi huylu. Örneğin, metin olmayan bir JPEG dosyasını dizine dizine dizine dizine, bu ekran görüntüsünde uyarıyı görürsünüz.

![Örnek dizinleyici uyarısı](./media/cognitive-search-tutorial-blob-python/py-indexer-warning-example.png "Örnek dizinleyici uyarısı")

## <a name="5---search"></a>5 - Arama

Dizin oluşturma işlemi tamamlandıktan sonra tek tek alanların içeriklerini döndüren sorgular çalıştırın. Varsayılan olarak, Azure Bilişsel Arama en iyi 50 sonucu döndürür. Varsayılan değerin düzgün çalışması için örnek veriler küçüktür. Ancak, büyük veri kümeleriyle çalışırken, daha fazla sonuç döndürmek için sorgu dizesine parametreleri dahil etmeniz gerekebilir. Talimatlar için Azure [Bilişsel Arama'da sonuçları sayfalamak için bkz.](search-pagination-page-layout.md)

Doğrulama adımı olarak, tüm alanları gösteren dizin tanımını alın.

```python
# Query the service for the index definition
r = requests.get(endpoint + "/indexes/" + index_name,
                 headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Sonuçlar aşağıdaki örneğe benzer olmalıdır. Ekran görüntüsü yalnızca yanıtın bir bölümünü gösterir.

![Tüm alanlar için sorgu dizini](./media/cognitive-search-tutorial-blob-python/py-query-index-for-fields.png "Tüm alanlar için dizini sorgula")

Çıktı, her bir alanın adını, türünü ve özniteliklerini içeren dizin şemasıdır.

`organizations` gibi tek bir alanın tüm içeriklerini döndürmek için ikinci bir `"*"` sorgusu gönderin.

```python
# Query the index to return the contents of organizations
r = requests.get(endpoint + "/indexes/" + index_name +
                 "/docs?&search=*&$select=organizations", headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Sonuçlar aşağıdaki örneğe benzer olmalıdır. Ekran görüntüsü yalnızca yanıtın bir bölümünü gösterir.

![Kuruluşların içeriği için sorgu dizini](./media/cognitive-search-tutorial-blob-python/py-query-index-for-organizations.png "Kuruluşların içeriğini döndürmek için dizini sorgula")

Ek alanlar için yineleyin: bu alıştırmada içerik, languageCode, keyPhrases ve kuruluşlar. Virgülle ayrılmış bir liste kullanarak `$select` aracılığıyla birden fazla alan döndürebilirsiniz.

Sorgu dizesinin karmaşıklık ve uzunluğuna bağlı olarak GET veya POST dizesini kullanabilirsiniz. Daha fazla bilgi için bkz. [REST API kullanarak sorgu](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmenin ilk deneysel aşamalarında, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama'dan silmek ve kodunuzu yeniden oluşturmasına izin vermektir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Portalı dizinleri, dizinleyicileri, veri kaynaklarını ve beceri kümelerini silmek için kullanabilirsiniz. Dizinleyiciyi sildiğinizde, isteğe bağlı olarak dizin, skillset ve veri kaynağını aynı anda seçilen silebilirsiniz.

![Arama nesnelerini silme](./media/cognitive-search-tutorial-blob-python/py-delete-indexer-delete-all.png "Portaldaki arama nesnelerini silme")

Ayrıca bir komut dosyası kullanarak bunları silebilirsiniz. Aşağıdaki komut dosyası, bir skillset'in nasıl silinir olduğunu gösterir. 

```python
# delete the skillset
r = requests.delete(endpoint + "/skillsets/" + skillset_name,
                    headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Silme işlemi başarılı olduğunda durum kodu 204 döndürülür.

## <a name="takeaways"></a>Paketler

Bu öğreticide, veri kaynağı, beceri kümesi, dizin ve dizin oluşturucu gibi bileşen parçalarının oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma işlem hattı oluşturmaya yönelik temel adımlar gösterilmektedir.

[Yerleşik beceriler,](cognitive-search-predefined-skills.md) beceri tanımları ve girdiler ve çıktılar yoluyla becerileri birbirine zincirlemenin bir yolu ile birlikte tanıtıldı. Ayrıca, dizin leyici `outputFieldMappings` tanımında, zenginleştirilmiş değerleri ardışık kaynaktan azure bilişsel arama hizmetinde aranabilir bir dizine yönlendirmenin gerekli olduğunu da öğrendiniz.

Son olarak, sonuçları test etmeyi ve daha fazla yineleme için sistemi sıfırlamayı öğrendiniz. Dizine karşı sorgular düzenlendiğinde, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktının döndürüldüğünü öğrendiniz. Bu yayında, iç yapıları (sistem tarafından oluşturulan zenginleştirilmiş belgeler) görüntüleme mekanizması vardır. Ayrıca, dizin leyici durumunu nasıl denetlersiniz ve bir ardışık yapı yı çalıştırmadan önce hangi nesnelerin silinmesi gerektiğini de öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda, artık ihtiyacınız olmayan kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki Tüm kaynaklar veya Kaynak grupları bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir AI zenginleştirme boru hattındaki tüm nesnelere aşina olduğunuza göre, beceri tanımlarına ve bireysel becerilere daha yakından bakalım.

> [!div class="nextstepaction"]
> [Nasıl bir skillset oluşturmak için](cognitive-search-defining-skillset.md)
