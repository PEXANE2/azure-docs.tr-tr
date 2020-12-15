---
title: Metin Analizi API’sini çağırma
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure bilişsel hizmetler REST API ve Postman Metin Analizi nasıl çağrlayabileceğiniz açıklanır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: aahi
ms.openlocfilehash: 7b035af85e250d97fb05625bf386bec8dc94a74c
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505265"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Metin Analizi nasıl çağrılacağını REST API

Bu makalede, temel kavramları göstermek için Metin Analizi REST API ve [Postman](https://www.postman.com/downloads/) kullanırız. API, hizmetin özelliklerini kullanmak için birkaç zaman uyumlu ve zaman uyumsuz uç nokta sağlar. 

## <a name="using-the-api-asynchronously"></a>API 'YI zaman uyumsuz kullanma

V 3.1-Preview. 3 ' den başlayarak Metin Analizi API'si iki zaman uyumsuz uç nokta sağlar: 

* `/analyze`Metin analizi uç noktası, tek BIR API çağrısında birden çok metin analizi özellikleriyle aynı metin belgelerinin aynı kümesini analiz etmenizi sağlar. Daha önce, birden çok özelliği kullanmak için her işlem için ayrı API çağrıları yapmanız gerekir. Birden fazla Metin Analizi özelliği olan büyük belge kümelerini analiz etmeniz gerektiğinde bu yeteneği göz önünde bulundurun.

* `/health`Uygunluk belgelerindeki ilgili tıbbi bilgileri ayıklayabilen ve etiketleyen sistem durumu için metin analizi uç noktası.  

Hangi özelliklerin zaman uyumsuz olarak kullanılabileceğini görmek için aşağıdaki tabloya bakın. Uç noktadan yalnızca birkaç özelliğin çağrılabilecek olduğunu unutmayın `/analyze` . 

| Özellik | Zaman Uyumlu | Zaman uyumsuz |
|--|--|--|
| Dil algılama | ✔ |  |
| Yaklaşım analizi | ✔ |  |
| Görüşün madenciliği | ✔ |  |
| Anahtar ifade ayıklama | ✔ | ✔* |
| Adlandırılmış varlık tanıma (PII ve FI dahil) | ✔ | ✔* |
| Sistem durumu için Metin Analizi (kapsayıcı) | ✔ |  |
| Sistem durumu için Metin Analizi (API) |  | ✔  |

`*` -Uç nokta aracılığıyla zaman uyumsuz olarak çağırılır `/analyze` .


[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

## <a name="prerequisites"></a>Önkoşullar


> [!NOTE]
> * Veya uç noktalarını kullanmak istiyorsanız, standart (S) [fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/) kullanarak bir metin analizi kaynağına ihtiyacınız olacaktır `/analyze` `/health` .

1.  İlk olarak, [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) gidin ve henüz yoksa yeni bir metin analizi kaynağı oluşturun. Veya uç noktalarını kullanmak istiyorsanız, **Standart fiyatlandırma katmanını** seçin `/analyze` `/health` . `/analyze`Uç nokta [fiyatlandırma katmanınıza](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/)dahildir.

2.  Uç noktanız için kullanmak istediğiniz bölgeyi seçin.  Lütfen `/analyze` ve `/health` uç noktaların yalnızca şu bölgelerde kullanılabilir olduğunu unutmayın: Batı ABD 2, Doğu ABD 2, Orta ABD, Kuzey Avrupa ve Batı Avrupa.

3.  Metin Analizi kaynağını oluşturun ve sayfanın solundaki "anahtarlar ve uç nokta dikey penceresine" gidin. Daha sonra API 'Leri çağırdığınızda kullanılacak anahtarı kopyalayın. Bunu daha sonra üst bilgi için bir değer olarak eklersiniz `Ocp-Apim-Subscription-Key` .


<a name="json-schema"></a>

## <a name="api-request-format"></a>API istek biçimi

#### <a name="synchronous"></a>[Zaman Uyumlu](#tab/synchronous)

API isteklerinin biçimi tüm zaman uyumlu işlemler için aynıdır. Belgeler, bir JSON nesnesinde ham yapılandırılmamış metin olarak gönderilir. XML desteklenmiyor. JSON şeması aşağıda açıklanan öğelerden oluşur.

| Öğe | Geçerli değerler | Gerekli mi? | Kullanım |
|---------|--------------|-----------|-------|
|`id` |Veri türü dizedir, ancak uygulama belge kimlikleri ' nde tam sayı olarak eğilimlidir. | Gerekli | Sistem çıktıyı yapılandırmak için sağladığınız kimlikleri kullanır. İstekteki her bir KIMLIK için dil kodları, anahtar tümceleri ve yaklaşım puanları oluşturulur.|
|`text` | Yapılandırılmamış ham metin, en fazla 5.120 karakter. | Gerekli | Dil algılama için metin herhangi bir dilde ifade edilebilir. Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanımlama için, metin [desteklenen bir dilde](../language-support.md)olmalıdır. |
|`language` | 2 karakterlik [ıso 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) [desteklenen bir dil](../language-support.md) için kod | Değişir | Yaklaşım analizi, anahtar ifade ayıklama ve varlık bağlama için gereklidir; dil algılama için isteğe bağlı. Bunu dışladığınızda bir hata yoktur, ancak analiz bu olmadan zayıflatılmalıdır. Dil kodu, sağladığınız öğesine karşılık gelmelidir `text` . |

Aşağıda, zaman uyumlu Metin Analizi uç noktaları için bir API isteği örneği verilmiştir. 

```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Sample text to be sent to the text analytics api."
    }
  ]
}
```

#### <a name="analyze"></a>[Çözümleme](#tab/analyze)

> [!NOTE]
> Metin Analizi istemci kitaplığının en son sürümü, istemci nesnesini kullanarak zaman uyumsuz analiz işlemlerini çağırmanızı sağlar. GitHub 'da örnekleri bulabilirsiniz:
* [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics)
* [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/)
* [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)

`/analyze`Uç nokta, tek BIR API çağrısında kullanmak istediğiniz desteklenen metin analizi özelliklerinden hangisini seçmenizi sağlar. Bu uç nokta şu anda şunları destekler:

* anahtar tümceciği ayıklama 
* Adlandırılmış varlık tanıma (PII ve FI dahil)

| Öğe | Geçerli değerler | Gerekli mi? | Kullanım |
|---------|--------------|-----------|-------|
|`displayName` | Dize | İsteğe Bağlı | İşe benzersiz tanımlayıcı için görünen ad olarak kullanılır.|
|`analysisInput` | `documents`Aşağıdaki alanı içerir | Gerekli | Göndermek istediğiniz belgelerin bilgilerini içerir. |
|`documents` | `id` `text` Aşağıdaki ve alanlarını içerir | Gerekli | Gönderilen her belge için bilgileri ve belgenin ham metnini içerir. |
|`id` | Dize | Gerekli | Sağladığınız kimlikler çıktıyı yapılandırmak için kullanılır. |
|`text` | Yapılandırılmamış ham metin, en fazla 125.000 karakter. | Gerekli | Şu anda desteklenen tek dil olan Ingilizce dilde olmalıdır. |
|`tasks` | Aşağıdaki Metin Analizi özelliklerini içerir: `entityRecognitionTasks` , `keyPhraseExtractionTasks` veya `entityRecognitionPiiTasks` . | Gerekli | Kullanmak istediğiniz bir veya daha fazla Metin Analizi özelliği. `entityRecognitionPiiTasks` `domain` Ya da olarak ayarlanabilir isteğe bağlı bir parametreye sahip olduğunu unutmayın `pii` `phi` . Belirtilmemişse, sistem varsayılan olarak olur `pii` . |
|`parameters` | `model-version` `stringIndexType` Aşağıdaki ve alanlarını içerir | Gerekli | Bu alan, seçtiğiniz yukarıdaki özellik görevlerine dahil edilmiştir. Bunlar, kullanmak istediğiniz model sürümü ve Dizin türü hakkında bilgiler içerir. |
|`model-version` | Dize | Gerekli | Hangi modelin kullanmak istediğinizi çağırmakta olduğunu belirtin.  |
|`stringIndexType` | Dize | Gerekli | Programlama ortamınızla eşleşen metin kod çözücüsünü belirtin.  Desteklenen türler `textElement_v8` (varsayılan), `unicodeCodePoint` , `utf16CodeUnit` . Daha fazla bilgi için lütfen [metin uzaklık makalesine](../concepts/text-offsets.md#offsets-in-api-version-31-preview) bakın.  |
|`domain` | Dize | İsteğe Bağlı | Yalnızca göreve parametre olarak uygulanır `entityRecognitionPiiTasks` ve veya olarak ayarlanabilir `pii` `phi` . Belirtilmemişse, varsayılan olarak olur `pii` .  |

```json
{
    "displayName": "My Job",
    "analysisInput": {
        "documents": [
            {
                "id": "doc1",
                "text": "It's incredibly sunny outside! I'm so happy"
            },
            {
                "id": "doc2",
                "text": "Pike place market is my favorite Seattle attraction."
            }
        ]
    },
    "tasks": {
        "entityRecognitionTasks": [
            {
                "parameters": {
                    "model-version": "latest",
                    "stringIndexType": "TextElements_v8"
                }
            }
        ],
        "keyPhraseExtractionTasks": [{
            "parameters": {
                "model-version": "latest"
            }
        }],
        "entityRecognitionPiiTasks": [{
            "parameters": {
                "model-version": "latest"
            }
        }]
    }
}

```

#### <a name="text-analytics-for-health"></a>[Sistem durumu için Metin Analizi](#tab/health)

Durum barındırılan API için Metin Analizi API isteklerinin biçimi, kapsayıcısı için olan ile aynıdır. Belgeler, bir JSON nesnesinde ham yapılandırılmamış metin olarak gönderilir. XML desteklenmiyor. JSON şeması aşağıda açıklanan öğelerden oluşur.  Lütfen sistem durumu genel önizlemesi için Metin Analizi erişim istemek üzere bilişsel [Hizmetler istek formunu](https://aka.ms/csgate) doldurun ve iletin. Sistem durumu kullanımı için Metin Analizi faturalandırılmaz. 

| Öğe | Geçerli değerler | Gerekli mi? | Kullanım |
|---------|--------------|-----------|-------|
|`id` |Veri türü dizedir, ancak uygulama belge kimlikleri ' nde tam sayı olarak eğilimlidir. | Gerekli | Sistem çıktıyı yapılandırmak için sağladığınız kimlikleri kullanır. |
|`text` | Yapılandırılmamış ham metin, en fazla 5.120 karakter. | Gerekli | Şu anda yalnızca Ingilizce metnin desteklendiğini unutmayın. |
|`language` | 2 karakterlik [ıso 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) [desteklenen bir dil](../language-support.md) için kod | Gerekli | Yalnızca `en` Şu anda destekleniyor. |

Aşağıda, sistem durumu uç noktaları için Metin Analizi yönelik bir API isteği örneği verilmiştir. 

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Subject was administered 100mg remdesivir intravenously over a period of 120 min"
    }
  ]
}
```

---

>[!TIP]
> Metin Analizi API'si veri göndermeye yönelik ücretler ve boyut limitleri hakkında bilgi için [veri ve oran limitleri](../concepts/data-limits.md) makalesine bakın.


## <a name="set-up-a-request"></a>İstek ayarlama 

Postman 'da (veya başka bir Web API test aracında) kullanmak istediğiniz özelliğin uç noktasını ekleyin. Uygun uç nokta biçimini bulmak için aşağıdaki tabloyu kullanın ve kaynak uç noktanızla değiştirin `<your-text-analytics-resource>` . Örneğin:

`https://my-resource.cognitiveservices.azure.com/text/analytics/v3.0/languages`

#### <a name="synchronous"></a>[Zaman Uyumlu](#tab/synchronous)

| Özellik | İstek türü | Kaynak uç noktaları |
|--|--|--|
| Dil algılama | POST | `<your-text-analytics-resource>/text/analytics/v3.0/languages` |
| Yaklaşım analizi | POST | `<your-text-analytics-resource>/text/analytics/v3.0/sentiment` |
| Görüşün madenciliği | POST | `<your-text-analytics-resource>/text/analytics/v3.0/sentiment?opinionMining=true` |
| Anahtar ifade ayıklama | POST | `<your-text-analytics-resource>/text/analytics/v3.0/keyPhrases` |
| Adlandırılmış varlık tanıma-genel | POST | `<your-text-analytics-resource>/text/analytics/v3.0/entities/recognition/general` |
| Adlandırılmış varlık tanıma-PII | POST | `<your-text-analytics-resource>/text/analytics/v3.0/entities/recognition/pii` |
| Adlandırılmış varlık tanıma-FI | POST |  `<your-text-analytics-resource>/text/analytics/v3.0/entities/recognition/pii?domain=phi` |

#### <a name="analyze"></a>[Çözümleme](#tab/analyze)

| Özellik | İstek türü | Kaynak uç noktaları |
|--|--|--|
| Analiz işini gönder | POST | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/analyze` |
| Çözümleme durumunu ve sonuçlarını al | GET | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/analyze/jobs/<Operation-Location>` |

#### <a name="text-analytics-for-health"></a>[Sistem durumu için Metin Analizi](#tab/health)

| Özellik | İstek türü | Kaynak uç noktaları |
|--|--|--|
| Sistem durumu işi için Metin Analizi gönder  | POST | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/entities/health/jobs` |
| İş durumunu ve sonuçları al | GET | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/entities/health/jobs/<Operation-Location>` |
| İşi iptal et | DELETE | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/entities/health/jobs/<Operation-Location>` |

--- 

Uç noktanız, Postman 'da (veya başka bir Web API test aracında) oluşturulduktan sonra:

1. Kullanmak istediğiniz özelliğin istek türünü seçin.
2. Yukarıdaki tabloda istediğiniz uygun işlemin uç noktasını yapıştırın.
3. Üç istek üst bilgilerini ayarlayın:

   + `Ocp-Apim-Subscription-Key`: Azure portal adresinden elde edilen erişim anahtarınız
   + `Content-Type`: uygulama/JSON
   + `Accept`: uygulama/JSON

    Postman kullanıyorsanız, isteğiniz aşağıdaki ekran görüntüsüne benzer bir `/keyPhrases` uç nokta kabul etmelidir.
    
    ![Uç nokta ve üst bilgilerle ekran görüntüsü iste](../media/postman-request-keyphrase-1.png)
    
4. **Gövde** biçimi için **Ham** seçin
    
    ![Gövde ayarları ile ekran görüntüsü iste](../media/postman-request-body-raw.png)

5. Bazı JSON belgelerini geçerli bir biçimde yapıştırın. Yukarıdaki **API istek biçimi** bölümündeki örnekleri kullanın ve daha fazla bilgi ve örnek için aşağıdaki konulara bakın:

      + [Dil algılama](text-analytics-how-to-language-detection.md)
      + [Anahtar ifade ayıklama](text-analytics-how-to-keyword-extraction.md)
      + [Yaklaşım Analizi](text-analytics-how-to-sentiment-analysis.md)
      + [Varlık tanıma](text-analytics-how-to-entity-linking.md)

## <a name="send-the-request"></a>İsteği gönder

API isteğini gönder. Zaman uyumlu bir uç noktaya çağrı yaptıysanız, yanıt, istekte belirtilen her belge KIMLIĞI için bir öğe olan tek bir JSON belgesi olarak hemen görüntülenir.

Zaman uyumsuz `/analyze` veya `/health` uç noktalara çağrı yaptıysanız, bir 202 yanıt kodu aldığınızı kontrol edin. sonuçları görüntülemek için yanıtı almanız gerekir:

1. API yanıtında, `Operation-Location` API 'ye gönderdiğiniz işi tanımlayan üst bilgiden öğesini bulun. 
2. Kullandığınız uç nokta için bir GET isteği oluşturun. uç nokta biçimi için [yukarıdaki tabloya](#set-up-a-request) başvurun ve [API başvuru belgelerini](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/AnalyzeStatus)gözden geçirin. Örneğin:

    `https://my-resource.cognitiveservices.azure.com/text/analytics/v3.1-preview.3/analyze/jobs/<Operation-Location>`

3. Öğesini `Operation-Location` isteğe ekleyin.

4. Yanıt, istekte belirtilen her belge KIMLIĞI için bir öğe içeren tek bir JSON belgesi olacaktır.

Lütfen hem zaman uyumsuz hem de `/analyze` `/health` işlemler için yukarıdaki adım 2 ' deki GET isteğinin sonuçları, işin oluşturulduğu zamandan itibaren 24 saat için kullanılabilir olduğunu unutmayın.  Bu süre, `expirationDateTime` Get yanıtında değeri ile belirtilir.  Bu süre dolduktan sonra sonuçlar temizlenir ve artık alma için kullanılamaz.    

## <a name="example-api-responses"></a>Örnek API yanıtları
 
# <a name="synchronous"></a>[Zaman Uyumlu](#tab/synchronous)

Zaman uyumlu uç nokta yanıtları kullandığınız uç noktaya göre değişir. Örnek yanıtlar için aşağıdaki makalelere bakın.

+ [Dil algılama](text-analytics-how-to-language-detection.md#step-3-view-the-results)
+ [Anahtar ifade ayıklama](text-analytics-how-to-keyword-extraction.md#step-3-view-results)
+ [Yaklaşım Analizi](text-analytics-how-to-sentiment-analysis.md#view-the-results)
+ [Varlık tanıma](text-analytics-how-to-entity-linking.md#view-results)

# <a name="analyze"></a>[Çözümleme](#tab/analyze)

Başarılı olursa, uç noktaya yönelik GET isteği `/analyze` atanan görevleri içeren bir nesne döndürür. Örneğin, `keyPhraseExtractionTasks`. Bu görevler, uygun Metin Analizi özelliğinden gelen yanıt nesnesini içerir. Daha fazla bilgi için aşağıdaki makalelere bakın.

+ [Anahtar ifade ayıklama](text-analytics-how-to-keyword-extraction.md#step-3-view-results)
+ [Varlık tanıma](text-analytics-how-to-entity-linking.md#view-results)


```json
{
  "displayName": "My Analyze Job",
  "jobId": "dbec96a8-ea22-4ad1-8c99-280b211eb59e_637408224000000000",
  "lastUpdateDateTime": "2020-11-13T04:01:14Z",
  "createdDateTime": "2020-11-13T04:01:13Z",
  "expirationDateTime": "2020-11-14T04:01:13Z",
  "status": "running",
  "errors": [],
  "tasks": {
      "details": {
          "name": "My Analyze Job",
          "lastUpdateDateTime": "2020-11-13T04:01:14Z"
      },
      "completed": 1,
      "failed": 0,
      "inProgress": 2,
      "total": 3,
      "keyPhraseExtractionTasks": [
          {
              "name": "My Analyze Job",
              "lastUpdateDateTime": "2020-11-13T04:01:14.3763516Z",
              "results": {
                  "inTerminalState": true,
                  "documents": [
                      {
                          "id": "doc1",
                          "keyPhrases": [
                              "sunny outside"
                          ],
                          "warnings": []
                      },
                      {
                          "id": "doc2",
                          "keyPhrases": [
                              "favorite Seattle attraction",
                              "Pike place market"
                          ],
                          "warnings": []
                      }
                  ],
                  "errors": [],
                  "modelVersion": "2020-07-01"
              }
          }
      ]
  }
}
```

# <a name="text-analytics-for-health"></a>[Sistem durumu için Metin Analizi](#tab/health)

Durum bilgisi zaman uyumsuz API yanıtının Metin Analizi hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

+ [Sistem durumu için Metin Analizi](text-analytics-for-health.md#hosted-asynchronous-web-api-response)


--- 

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizine genel bakış](../overview.md)
* [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
* [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712)
* [Metin Analizi istemci kitaplığını kullanma](../quickstarts/client-libraries-rest-api.md)
* [Yenilikler](../whats-new.md)
