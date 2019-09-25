---
title: Bilişsel arama ardışık düzeninde beceri oluşturma-Azure Search
description: Azure Search ' de kullanabilmeniz için veri ayıklama, doğal dil işleme veya görüntü analizi adımlarını tanımlayarak verileri verilerinize göre zenginleştirin ve ayıklayın.
manager: nitinme
author: luiscabrer
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.openlocfilehash: f78b8c3b9619b7eea92b6a4f04ed4f6543916efe
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265525"
---
# <a name="how-to-create-a-skillset-in-an-enrichment-pipeline"></a>Bir zenginleştirme ardışık düzeninde beceri oluşturma

Bilişsel arama, Azure Search içinde aranabilir hale getirmek için verileri ayıklar ve zenginleştirir. Ayıklama ve zenginleştirme adımları bilişsel *becerileri*, dizin oluşturma sırasında başvurulan bir *beceri* birleştirilir. Beceri, [yerleşik becerileri](cognitive-search-predefined-skills.md) veya özel becerileri kullanabilir (bkz [. örnek: Daha fazla bilgi için bilişsel arama](cognitive-search-create-custom-skill-example.md) için özel bir yetenek oluşturma.

Bu makalede, kullanmak istediğiniz yetenekler için bir zenginleştirme ardışık düzeni oluşturmayı öğreneceksiniz. Bir beceri Azure Search [dizin oluşturucuya](search-indexer-overview.md)eklenir. Bu makalede ele alınan işlem hattı tasarımının bir parçası, Beceri kendisini tasarlayabilmektedir. 

> [!NOTE]
> Ardışık düzen tasarımının başka bir bölümü, bir Dizin Oluşturucu belirleyerek bir [sonraki adımda](#next-step)ele alınmıştır. Dizin Oluşturucu tanımı, hedef dizindeki çıkışlara girdileri bağlamak için kullanılan beceri ve alan eşlemelerine yönelik bir başvuru içerir.

Anımsanması gereken önemli noktaları:

+ Dizin Oluşturucu başına yalnızca bir beceri olabilir.
+ Bir beceri en az bir yeteneğe sahip olmalıdır.
+ Aynı türde (örneğin, bir görüntü analizi becerilerinin türevleri) birden çok yetenek oluşturabilirsiniz.

## <a name="begin-with-the-end-in-mind"></a>Son göz önünde bulundurularak başlayın

Önerilen ilk adım, ham verilerinize hangi verilerin ayıklanacağını ve bu verileri bir arama çözümünde nasıl kullanmak istediğinizi saptarken. Tüm zenginleştirme işlem hattının bir resmini oluşturmak, gerekli adımları belirlemenize yardımcı olabilir.

Bir dizi finansal analist açıklamasını işlemek istediğinizi varsayalım. Her dosya için, yorumların şirket adlarını ve genel yaklaşımını ayıklamak istersiniz. Ayrıca şirket hakkında şirket hakkında ek bilgiler bulmak için Bing Varlık Arama hizmetini kullanan özel bir zenginte yazmak isteyebilirsiniz. Temelde, her belge için dizine alınmış aşağıdaki gibi bilgileri ayıklamak istersiniz:

| Kaydet-metin | firması | Yaklaşım | Şirket açıklamaları |
|--------|-----|-----|-----|
|örnek kayıt| ["Microsoft", "LinkedIn"] | 0.99 | ["Microsoft Corporation, çok uluslu bir teknoloji şirketidir...", "LinkedIn iş ve işe yönelik olarak çalışan bir sosyal ağ..."]

Aşağıdaki diyagramda bir kuramsal zenginleştirme işlem hattı gösterilmektedir:

![Kuramsal bir] zenginleştirme işlem hattı (media/cognitive-search-defining-skillset/sample-skillset.png "Kuramsal bir") zenginleştirme işlem hattı


İşlem hattında istediğiniz kadar fikir sahibi olduktan sonra, bu adımları sağlayan beceri ifade edebilirsiniz. İşlev, Dizin Oluşturucu tanımınızı Azure Search için yüklediğinizde beceri ifade edilir. Dizin oluşturucuyu karşıya yükleme hakkında daha fazla bilgi edinmek için bkz. [Dizin Oluşturucu-belgeler](https://docs.microsoft.com/rest/api/searchservice/create-indexer).


Diyagramda *belge çözme* adımı otomatik olarak gerçekleşir. Temelde, Azure Search iyi bilinen dosyaların nasıl açılacağını bilir ve her belgeden ayıklanan metni içeren bir *içerik* alanı oluşturur. Beyaz kutular yerleşik zenginler ve noktalı "Bing Varlık Arama" kutusu oluşturmakta olduğunuz özel bir zengini temsil eder. Gösterildiği gibi, beceri üç yetenek içerir.

## <a name="skillset-definition-in-rest"></a>Beceri tanımı REST

Bir beceri bir yetenek dizisi olarak tanımlanır. Her beceri, girişlerinin kaynağını ve üretilen çıktıların adını tanımlar. [Create beceri REST API](https://docs.microsoft.com/rest/api/searchservice/create-skillset)kullanarak, önceki diyagrama karşılık gelen bir beceri tanımlayabilirsiniz: 

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

Bir beceri oluştururken, Beceri kendi kendine belgelerinizi yapmak için bir açıklama sağlayabilirsiniz. Bir açıklama isteğe bağlıdır, ancak beceri ne yaptığını izlemek için yararlıdır. Beceri, açıklamalara izin vermediği bir JSON belgesi olduğundan, bunun için bir `description` öğe kullanmanız gerekir.

```json
{
  "description": 
  "This is our first skill set, it extracts sentiment from financial records, extract company names, and then finds additional information about each company mentioned.",
  ...
}
```

Beceri sonraki parçası bir yetenek dizisidir. Her bir beceriye, zenginleştirme için bir temel olarak düşünebilirsiniz. Her beceri, bu zenginleştirme ardışık düzeninde küçük bir görev gerçekleştirir. Her biri bir giriş (veya bir giriş kümesi) alır ve bazı çıktılar döndürür. Sonraki birkaç bölüm yerleşik ve özel yeteneklerin nasıl belirtildiğiyle ilgili olarak, becerileri giriş ve çıkış başvurularıyla birlikte zincirlemeye odaklanacaktır. Girişler, kaynak verilerden veya başka bir becerilerden gelebilir. Çıktılar, bir arama dizinindeki alanla eşleştirilebilir veya bir aşağı akış beceriye giriş olarak kullanılabilir.

## <a name="add-built-in-skills"></a>Yerleşik yetenekler ekleme

Yerleşik [varlık tanıma becerisi](cognitive-search-skill-entity-recognition.md)olan ilk beceriye bakalım:

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

* Her yerleşik beceri `odata.type`, `input`, ve `output` özelliklerdir. Beceriye özgü özellikler, bu beceriye uygun ek bilgiler sağlar. Varlık tanıma için, `categories` önceden eğitilen modelin tanıyabileceği sabit bir varlık türleri kümesi arasında bir varlıktır.

* Her yeteneğin bir ```"context"```olması gerekir. Bağlam, işlemlerin gerçekleştiği düzeyi temsil eder. Yukarıdaki becerideki bağlam tüm belgedir, yani varlık tanıma becerisi her belge için bir kez çağrılır. Çıkışlar da bu düzeyde oluşturulur. Daha özel olarak ```"organizations"``` , ```"/document"```üyesi olarak üretilir. Aşağı akış becerileri bölümünde bu yeni oluşturulan bilgilere olarak ```"/document/organizations"```başvurabilirsiniz.  ```"context"``` Alan açıkça ayarlanmamışsa, varsayılan bağlam belgedir.

* Yeteneğin, kaynak girişi olarak ```"/document/content"```ayarlanmış "metin" adlı bir giriş vardır. Yetenek (varlık tanıma), Azure Blob Indexer tarafından oluşturulan standart bir alan olan her belgenin *içerik* alanı üzerinde çalışır. 

* Yeteneğin adlı ```"organizations"```bir çıkış vardır. Çıkışlar yalnızca işlem sırasında mevcuttur. Bu çıktıyı bir aşağı akış becerisi girişine zincirlemek için, çıktıyı olarak ```"/document/organizations"```başvuru yapın.

* Belirli bir belge için değeri ```"/document/organizations"``` , metinden ayıklanan kuruluşların bir dizisidir. Örneğin:

  ```json
  ["Microsoft", "LinkedIn"]
  ```

Bazı durumlar, bir dizinin her öğesine ayrı olarak başvurmak için çağrı yapılır. Örneğin, her öğesini ```"/document/organizations"``` ayrı olarak başka bir beceriye (özel Bing varlık arama daha zengin gibi) geçirmek istediğinizi varsayalım. Yola bir yıldız işareti ekleyerek dizinin her öğesine başvurabilirsiniz:```"/document/organizations/*"``` 

Yaklaşım ayıklama için ikinci yetenek, ilk zenginleştirme ile aynı kalıbı izler. Giriş olarak ```"/document/content"``` alır ve her içerik örneği için bir yaklaşım puanı döndürür. ```"context"``` Alanı açıkça ayarlamazsanız, çıkış (mysentiment) artık bir ```"/document"```alt öğesidir.

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

## <a name="add-a-custom-skill"></a>Özel bir yetenek ekleyin

Özel Bing varlık arama 'nın yapısını daha zengin bir şekilde geri çekin:

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

Bu tanım, zenginleştirme sürecinin bir parçası olarak bir Web API 'SI çağıran [özel bir yetentandır](cognitive-search-custom-skill-web-api.md) . Bu yetenek, varlık tanıma tarafından tanımlanan her kuruluş için, bu kuruluşun açıklamasını bulmak için bir Web API 'SI çağırır. Web API 'sinin ne zaman çağrılacağını ve alınan bilgilerin nasıl Flow, enzenginleştirme altyapısı tarafından dahili olarak işlenir. Ancak, bu özel API 'yi çağırmak için gereken başlatma işlemi JSON 'da (URI, httpHeaders ve beklenen girişler gibi) sağlanmalıdır. Zenginleştirme işlem hattı için özel Web API 'SI oluşturma konusunda rehberlik için bkz. [özel bir arabirim tanımlama](cognitive-search-custom-skill-interface.md).

"Bağlam" alanının, altındaki ```"/document/organizations/*"``` ```"/document/organizations"``` *her kuruluş için* de zenginleştirme adımının çağrıldığı bir yıldız işaretiyle ayarlandığını unutmayın. 

Bu durumda, belirtilen her bir kuruluş için bir şirket açıklaması olan çıktı. Bir aşağı akış adımında (örneğin, anahtar tümceciği ayıklama içinde) açıklamaya başvururken, bunu yapmak için yolunu ```"/document/organizations/*/description"``` kullanırsınız. 

## <a name="add-structure"></a>Yapı Ekle

Beceri yapılandırılmamış verilerden yapılandırılmış bilgiler üretir. Aşağıdaki örnek göz önünde bulundurun:

*"Dördüncü çeyrekte, Microsoft, geçen yıl satın aldığı sosyal ağ şirketi olan LinkedIn 'ten gelir üzerinden $1.100.000.000 'e kaydedildi. Alım, Microsoft 'un LinkedIn yeteneklerini kendi CRM ve Office özellikleri ile birleştirmesine olanak sağlar. Stockholders şu ana kadar ilerleme durumuyla heyecanlanır. "*

Olası bir sonuç, aşağıdaki çizime benzer şekilde oluşturulmuş bir yapıdır:

![Örnek çıkış yapısı](media/cognitive-search-defining-skillset/enriched-doc.png "Örnek çıkış yapısı")

Şu anda bu yapı yalnızca iç, yalnızca bellek ve yalnızca Azure Search dizinlerde kullanılır. Bilgi deposunun eklenmesi, arama dışında kullanmak üzere şekillendirilmiş zenginleştirme tasarrufu için bir yol sağlar.

## <a name="add-a-knowledge-store"></a>Bilgi deposu ekleme

[Bilgi deposu](knowledge-store-concept-intro.md) , zenginleştirilmiş belgeyi kaydetmek için Azure Search bir önizleme özelliğidir. Azure depolama hesabı tarafından desteklenen, oluşturduğunuz bir bilgi deposu, verileri zenginleştirdiği depodur. 

Bir beceri öğesine bilgi deposu tanımı eklenir. İşlemin tamamına yönelik bir anlatım için bkz. [bilgi deposu ile çalışmaya başlama](knowledge-store-howto.md).

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

Zenginleştirilmiş belgeleri, hiyerarşik ilişkileri korunan tablolar olarak veya blob depolamada JSON belgeleri olarak kaydetmeyi seçebilirsiniz. Beceri içindeki yeteneklerin herhangi birinden çıkış, projeksiyonun için giriş olarak kaynak olarak oluşturulabilir. Verileri belirli bir şekle göre projeye eklemek istiyorsanız, [her yetenek](cognitive-search-skill-shaper.md) için güncelleştirilmiş mil, artık kullanmanız için karmaşık türleri modelleyebilir. 

<a name="next-step"></a>

## <a name="next-steps"></a>Sonraki adımlar

Artık enzenginleştirme işlem hattı ve becerileri hakkında bilgi sahibi olduğunuza göre, [bir beceri içindeki ek açıklamaların nasıl başvurulacağını](cognitive-search-concept-annotations-syntax.md) veya [bir dizindeki alanlara çıktıların nasıl eşlenilmeye](cognitive-search-output-field-mapping.md)devam edin. 
