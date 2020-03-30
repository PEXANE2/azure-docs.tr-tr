---
title: Beceri kümesi oluşturma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da kullanılmak üzere verilerinizdeki yapılandırılmış bilgileri zenginleştirmek ve ayıklamak için veri ayıklama, doğal dil işleme veya görüntü analizi adımlarını tanımlayın.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 43251783cbcd6501562913b7b9cafb4f9f7cb3f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75754554"
---
# <a name="how-to-create-a-skillset-in-an-ai-enrichment-pipeline-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da bir AI zenginleştirme boru hattında beceri oluşturma 

AI zenginleştirme, Azure Bilişsel Arama'da aranabilir hale getirmek için verileri ayıklar ve zenginleştirir. Biz çıkarma ve zenginleştirme adımları *bilişsel becerileri,* indeksleme sırasında başvurulan bir *beceri* içine kombine diyoruz. Bir skillset [yerleşik becerileri](cognitive-search-predefined-skills.md) veya özel becerileri kullanabilir (bkz. Örnek: Daha fazla bilgi için [bir AI zenginleştirme ardışık bölgesinde özel bir beceri oluşturma).](cognitive-search-create-custom-skill-example.md)

Bu makalede, kullanmak istediğiniz beceriler için bir zenginleştirme ardışık alanı oluşturmayı öğrenirsiniz. Bir skillset, Azure Bilişsel Arama [dizinine](search-indexer-overview.md)eklenir. Bu makalede ele alınan boru hattı tasarımının bir parçası, skillset kendisi inşa etmektir. 

> [!NOTE]
> Boru hattı tasarımının bir diğer parçası, [bir sonraki adımda](#next-step)kapsanan bir dizin leyici belirtilmesidir. Dizinleyici tanımı, skillset'e bir başvuru nun yanı sıra girişleri hedef dizindeki çıktılara bağlamak için kullanılan alan eşlemelerini içerir.

Hatırlanması gereken önemli noktalar:

+ Dizin leyici başına yalnızca bir beceriye sahip olabilirsiniz.
+ Bir beceri en az bir beceri olmalıdır.
+ Aynı türde birden çok beceri oluşturabilirsiniz (örneğin, görüntü analizi becerisinin türevleri).

## <a name="begin-with-the-end-in-mind"></a>Akılda sonu ile başlayın

Önerilen ilk adım, ham verilerinizden hangi verileri ayıklamaya ve bu verileri bir arama çözümünde nasıl kullanmak istediğinize karar vermektir. Tüm zenginleştirme ardışık bir illüstrasyon oluşturma gerekli adımları belirlemenize yardımcı olabilir.

Bir dizi finansal analist yorumunu işlemekle ilgilendiğinizi varsayalım. Her dosya için, şirket adlarını ve yorumların genel duyarlılığını ayıklamak istiyorsunuz. Ayrıca, şirket hakkında şirket hakkında ne tür bir iş yaptığı gibi ek bilgiler bulmak için Bing Varlık Arama hizmetini kullanan özel bir zenginleştirici de yazmak isteyebilirsiniz. Esasen, her belge için dizine eklenen aşağıdaki gibi bilgileri ayıklamak istiyorsunuz:

| kayıt metni | Şirket | Duyguları | şirket açıklamaları |
|--------|-----|-----|-----|
|örnek kayıt| ["Microsoft", "LinkedIn"] | 0.99 | ["Microsoft Corporation bir Amerikan çokuluslu teknoloji şirketi ..." , "LinkedIn bir iş ve istihdam odaklı sosyal ağ ..."]

Aşağıdaki diyagram varsayımsal bir zenginleştirme ardışık gösteriş:

![Varsayımsal bir zenginleştirme boru hattı](media/cognitive-search-defining-skillset/sample-skillset.png "Varsayımsal bir zenginleştirme boru hattı")


Boru hattında ne istediğinizhakkında adil bir fikre sahip olduktan sonra, bu adımları sağlayan becerileri ifade edebilirsiniz. İşlevsel olarak, dizin leyici tanımınızı Azure Bilişsel Arama'ya yüklediğinizde beceri ifade edilir. Dizin leyicinizi nasıl yükleyin hakkında daha fazla bilgi edinmek için [dizinleyici belgelerine](https://docs.microsoft.com/rest/api/searchservice/create-indexer)bakın.


Diyagramda, *belge çözme* adımı otomatik olarak gerçekleşir. Azure Bilişsel Arama, tanınmış dosyaların nasıl açılacağını bilir ve her belgeden çıkarılan metni içeren bir *içerik* alanı oluşturur. Beyaz kutular yerleşik zenginleştiricilerdir ve noktalı "Bing Varlık Arama" kutusu oluşturduğunuz özel bir zenginleştiriciyi temsil eder. Gösterildiği gibi, skillset üç beceri içerir.

## <a name="skillset-definition-in-rest"></a>REST'te Skillset tanımı

Bir skillset becerileri bir dizi olarak tanımlanır. Her beceri, girdilerinin kaynağını ve üretilen çıktıların adını tanımlar. Create [Skillset REST API'yi](https://docs.microsoft.com/rest/api/searchservice/create-skillset)kullanarak, önceki diyagrama karşılık gelen bir skillset tanımlayabilirsiniz: 

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```

```json
{
  "description": 
  "Extract sentiment from financial records, extract company names, and then find additional information about each company mentioned.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "Calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar"
      },
      "context": "/document/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
  ]
}
```

## <a name="create-a-skillset"></a>Beceri kümesi oluşturma

Bir skillset oluştururken, skillset kendini belgeleme yapmak için bir açıklama sağlayabilir. Açıklama isteğe bağlıdır, ancak bir skillset'in ne yaptığını izlemek için yararlıdır. Skillset yoruma izin vermeyen bir JSON belgesi olduğundan, `description` bunun için bir öğe kullanmanız gerekir.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

Skillset sonraki parça becerileri bir dizi. Her beceriyi ilkel bir zenginleştirme olarak düşünebilirsiniz. Her beceri bu zenginleştirme boru hattı küçük bir görev gerçekleştirir. Her biri bir giriş (veya bir dizi girdi) alır ve bazı çıktıları döndürür. Sonraki birkaç bölümde yerleşik ve özel becerilerin nasıl belirtilen, giriş ve çıktı referansları aracılığıyla becerileri birbirine zincirleme üzerinde duruluyor. Girişler kaynak verilerden veya başka bir beceriden gelebilir. Çıktılar bir arama dizinindeki bir alana eşlenebilir veya akış aşağı sına giriş olarak kullanılabilir.

## <a name="add-built-in-skills"></a>Yerleşik beceriler ekleme

Dahili [varlık tanıma becerisi](cognitive-search-skill-entity-recognition.md)olan ilk beceriye bakalım:

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
      "context": "/document",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    }
```

* Her yerleşik beceri `odata.type`, `input`ve `output` özellikleri vardır. Beceriye özel özellikler, bu beceri için geçerli olan ek bilgiler sağlar. Varlık tanıma `categories` için, önceden eğitilmiş modelin tanıyabileceği sabit bir varlık türleri kümesi arasındaki bir varlıktır.

* Her beceri nin ```"context"```bir . Bağlam, işlemlerin gerçekleştiği düzeyi temsil eder. Yukarıdaki beceride, bağlam belgenin tamamıdır, yani varlık tanıma becerisi belge başına bir kez çağrılır. Çıktılar da bu düzeyde üretilmektedir. Daha spesifik ```"organizations"``` olarak, bir ```"/document"```üyesi olarak oluşturulur. Downstream becerilerinde, bu yeni oluşturulan bilgilere ```"/document/organizations"```.  ```"context"``` Alan açıkça ayarlanmazsa, varsayılan bağlam belgedir.

* Becerinin "metin" adı verilen bir girişi vardır ve ```"/document/content"```kaynak girişi .' olarak ayarlanır. Beceri (varlık tanıma), Azure blob dizinleyicisi tarafından oluşturulan standart bir alan olan her belgenin *içerik* alanında çalışır. 

* Beceri denilen ```"organizations"```bir çıktı vardır. Çıktılar yalnızca işleme sırasında bulunur. Bu çıktıyı bir alt akım becerisinin girişine zincirlemek için çıktıyı ```"/document/organizations"```.

* Belirli bir belge için, ```"/document/organizations"``` değeri metinden ayıklanan bir dizi kuruluştür. Örnek:

  ```json
  ["Microsoft", "LinkedIn"]
  ```

Bazı durumlarda, bir dizinin her öğesine ayrı ayrı başvurmayı gerektirir. Örneğin, her öğeyi ```"/document/organizations"``` ayrı ayrı başka bir beceriye (özel Bing varlık arama zenginleştirici si gibi) aktarmak istediğinizi varsayalım. Yola bir yıldız işareti ekleyerek dizinin her öğesine başvurabilirsiniz:```"/document/organizations/*"``` 

Duygu çıkarma için ikinci beceri ilk zenginleştirici olarak aynı desen izler. Bu ```"/document/content"``` giriş olarak alır ve her içerik örneği için bir duyarlılık puanı döndürür. Alanı açıkça ayarlamadığınız ```"context"``` için, çıktı (mySentiment) artık ```"/document"```bir çocuk.

```json
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
```

## <a name="add-a-custom-skill"></a>Özel bir beceri ekleme

Özel Bing varlık arama zenginleştiriciyapısını hatırlayın:

```json
    {
      "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
     "description": "This skill calls an Azure function, which in turn calls Bing Entity Search",
      "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/InvokeTextAnalyticsV3?code=foo",
      "httpHeaders": {
          "Ocp-Apim-Subscription-Key": "foobar"
      },
      "context": "/document/organizations/*",
      "inputs": [
        {
          "name": "query",
          "source": "/document/organizations/*"
        }
      ],
      "outputs": [
        {
          "name": "description",
          "targetName": "companyDescription"
        }
      ]
    }
```

Bu tanım, zenginleştirme işleminin bir parçası olarak web API çağıran özel bir [beceridir.](cognitive-search-custom-skill-web-api.md) Varlık tanıma tarafından tanımlanan her kuruluş için, bu beceri, bu kuruluşun açıklamasını bulmak için bir web API çağırır. Web API'sinin ne zaman çağrılması ve alınan bilgilerin nasıl akıştırılacaya kadar adlandırılabilmek için yapılan orkestrasyon, zenginleştirme motoru tarafından dahili olarak ele alınır. Ancak, bu özel API'yi aramak için gerekli başlatma JSON'da sağlanmalıdır (uri, httpHeaders ve beklenen girişler gibi). Zenginleştirme ardışık birimi için özel bir web API oluşturma kılavuzu için, [özel bir arabirim nasıl tanımlanağa](cognitive-search-custom-skill-interface.md)bakın.

"Bağlam" alanının bir yıldız ```"/document/organizations/*"``` işaretiyle ayarlı olduğuna dikkat edin, yani ```"/document/organizations"```zenginleştirme adımı altındaki her kuruluş *için* çağrılır. 

Çıktı, bu durumda tanımlanan her kuruluş için bir şirket açıklaması oluşturulur. Bir akış adımında açıklamaya atıfta bulunduktan sonra (örneğin, anahtar tümcecik ```"/document/organizations/*/description"``` çıkarmada), bunu yapmak için yolu kullanırsınız. 

## <a name="add-structure"></a>Yapı ekleme

Skillset, yapılandırılmamış verilerden yapılandırılmış bilgiler üretir. Aşağıdaki örneği inceleyin:

*"Dördüncü çeyreğinde Microsoft, geçen yıl satın aldığı sosyal ağ şirketi LinkedIn'den 1,1 milyar dolar gelir elde etti. Satın alma, Microsoft'un LinkedIn yeteneklerini CRM ve Office özellikleriyle birleştirmesine olanak tanır. Hissedarlar bugüne kadar kaydedilen ilerlemeden heyecan duyuyoruz."*

Olası bir sonuç, aşağıdaki çizime benzer oluşturulmuş bir yapı olacaktır:

![Örnek çıktı yapısı](media/cognitive-search-defining-skillset/enriched-doc.png "Örnek çıktı yapısı")

Şimdiye kadar, bu yapı yalnızca dahili, yalnızca bellekte olmuştur ve yalnızca Azure Bilişsel Arama dizinlerinde kullanılmıştır. Bir bilgi deposunun eklenmesi, arama dışında kullanım için şekilli zenginleştirmeleri kaydetmeniz için bir yol sağlar.

## <a name="add-a-knowledge-store"></a>Bilgi deposu ekleme

[Bilgi deposu,](knowledge-store-concept-intro.md) zenginleştirilmiş belgenizi kaydetmek için Azure Bilişsel Arama'da bir önizleme özelliğidir. Azure depolama hesabı tarafından desteklenen bir bilgi deposu, zenginleştirilmiş verilerinizin bulunduğu depodur. 

Bir bilgi deposu tanımı bir skillset eklenir. Tüm sürecin bir walkthrough için REST [bir bilgi deposu oluştur](knowledge-store-create-rest.md)'a bakın.

```json
"knowledgeStore": {
  "storageConnectionString": "<an Azure storage connection string>",
  "projections" : [
    {
      "tables": [ ]
    },
    {
      "objects": [
        {
          "storageContainer": "containername",
          "source": "/document/EnrichedShape/",
          "key": "/document/Id"
        }
      ]
    }
  ]
}
```

Zenginleştirilmiş belgeleri hiyerarşik ilişkileri korunmuş tablolar olarak veya blob depolamada JSON belgeleri olarak kaydetmeyi seçebilirsiniz. Beceri becerisindeki becerilerden herhangi birinden elde edilen çıktı, projeksiyon için girdi olarak elde edilebilir. Verileri belirli bir şekle yansıtmak istiyorsanız, güncelleştirilmiş [şekillendirici becerisi](cognitive-search-skill-shaper.md) artık kullanmanız için karmaşık türleri modelleyebilir. 

<a name="next-step"></a>

## <a name="next-steps"></a>Sonraki adımlar

Şimdi zenginleştirme boru hattı ve beceri aşina, [nasıl bir skillset veya](cognitive-search-concept-annotations-syntax.md) nasıl [bir dizin alanları çıkışları eşleme](cognitive-search-output-field-mapping.md)ek açıklamaları başvuru ile devam edin. 
