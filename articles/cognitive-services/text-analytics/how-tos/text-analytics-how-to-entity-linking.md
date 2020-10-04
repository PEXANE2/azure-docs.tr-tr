---
title: Metin Analizi API'si ile varlık tanımayı kullanın
titleSuffix: Azure Cognitive Services
description: Metin Analizi REST API metinde bulunan bir varlığın kimliğini belirleme ve ayırt etme hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 09/24/2020
ms.author: aahi
ms.openlocfilehash: d6820e890607ff16230ecf48e8318e6d1119a3a2
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91707515"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Metin Analizi içinde adlandırılmış varlık tanımayı kullanma

Metin Analizi API'si, yapılandırılmamış metin almanıza ve Web hakkında daha fazla bilgi için bağlantılarla birlikte, Kesinleştirme varlıklarının bir listesini döndürmenize olanak tanır. API, adlandırılmış varlık tanıma (NER) ve varlık bağlamayı destekler.

### <a name="entity-linking"></a>Varlık Bağlama

Varlık bağlama, metinde bulunan bir varlığın kimliğini belirleme ve ayırt etme olanağıdır (örneğin, "Mars" sözcüğünün bir oluşumunun Planet 'e veya War 'ın gerçekleşen ' ine mi başvurduğunu belirleme). Bu işlem, tanınan varlıkları metinde bağlamak için uygun bir dilde Bilgi Bankası 'nın bulunmasını gerektirir. Varlık bağlama, bu Bilgi Bankası olarak [Vikipedi](https://www.wikipedia.org/) kullanır.


### <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

Adlandırılmış varlık tanıma (NER), metinde farklı varlıkları belirleme ve bunları önceden tanımlanmış sınıflar veya türler, örneğin: kişi, konum, olay, ürün ve kuruluş gibi kategorilere ayırma yeteneğidir.  

## <a name="named-entity-recognition-versions-and-features"></a>Adlandırılmış varlık tanıma sürümleri ve özellikleri

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

| Özellik                                                         | NER v 3.0 | NER v 3.1-Önizleme. 2 |
|-----------------------------------------------------------------|--------|----------|
| Tek ve toplu istekler için Yöntemler                          | X      | X        |
| Çeşitli kategoriler genelinde Genişletilmiş varlık tanıma           | X      | X        |
| Varlık bağlama ve NER istekleri göndermek için ayrı uç noktalar. | X      | X        |
| Kişisel ( `PII` ) ve sağlık ( `PHI` ) bilgi varlıklarının tanınması        |        | X        |

Bilgi için bkz. [dil desteği](../language-support.md) .

### <a name="entity-types"></a>Varlık türleri

Adlandırılmış varlık tanıma v3, birden çok tür genelinde genişletilmiş algılama sağlar. Şu anda NER v 3.0, [genel varlık kategorisindeki](../named-entity-types.md)varlıkları tanıyabilir.

Adlandırılmış varlık tanıma v 3.1-Önizleme. 2, v 3.0 'ın algılama yeteneklerini ve `PII` uç noktayı kullanarak kişisel bilgileri () algılama olanağını içerir `v3.1-preview.2/entities/recognition/pii` . `domain=phi`Gizli sistem durumu bilgilerini () algılamak için isteğe bağlı parametresini kullanabilirsiniz `PHI` . Daha fazla bilgi için [varlık kategorileri](../named-entity-types.md) makalesine ve [istek uç noktaları](#request-endpoints) bölümüne bakın.


## <a name="sending-a-rest-api-request"></a>REST API isteği gönderiliyor

### <a name="preparation"></a>Hazırlık

Bu biçimde JSON belgelerinize sahip olmanız gerekir: KIMLIK, metin, dil.

Her belge 5.120 karakterden oluşmalıdır ve koleksiyon başına en fazla 1.000 öğe (kimlik) olabilir. Koleksiyon, istek gövdesinde gönderilir.

### <a name="structure-the-request"></a>İsteği yapısı

Bir POST isteği oluşturun. Hızlı bir şekilde yapılandırmak ve göndermek için aşağıdaki bağlantılardan [Postman](text-analytics-how-to-call-api.md) veya **API test konsolunu** kullanabilirsiniz. 

> [!NOTE]
> Azure portalında Metin Analizi kaynağınız için anahtarınızı ve uç noktanızı bulabilirsiniz. Kaynak **yönetimi**altında kaynağın **hızlı başlangıç** sayfasında yer alır. 


### <a name="request-endpoints"></a>İstek uç noktaları

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

Adlandırılmış varlık tanıma v3, NER ve varlık bağlama istekleri için ayrı uç noktalar kullanır. İsteğinize göre aşağıdan bir URL biçimi kullanın:

Varlık bağlama
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/linking`

[İçin adlandırılmış varlık tanıma sürüm 3,0 başvurusu `Linking`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/EntitiesRecognitionGeneral)

HI
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`

[İçin adlandırılmış varlık tanıma sürüm 3,0 başvurusu `General`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/EntitiesRecognitionGeneral)

#### <a name="version-31-preview2"></a>[Sürüm 3,1-Önizleme. 2](#tab/version-3-preview)

Adlandırılmış varlık tanıma `v3.1-preview.2` , ner ve varlık bağlama istekleri için ayrı uç noktalar kullanır. İsteğinize göre aşağıdan bir URL biçimi kullanın:

Varlık bağlama
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/linking`

[Adlandırılmış varlık tanıma sürüm 3,1-Önizleme başvurusu `Linking`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesLinking)

HI
* Genel varlıklar- `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/recognition/general`

[Adlandırılmış varlık tanıma sürüm 3,1-Önizleme başvurusu `General`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesRecognitionGeneral)

* Kişisel ( `PII` ) bilgileri- `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/recognition/pii`

Ayrıca, `domain=phi` metindeki sağlık () bilgilerini algılamak için isteğe bağlı parametresini de kullanabilirsiniz `PHI` . 

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/recognition/pii?domain=phi`

Lütfen, `redactedText` ALGıLANAN PII varlıklarının varlıkların her bir karakteri için bir * ile değiştirildiği değiştirilmiş giriş metnini içeren yanıt json ' da özelliğinin eklenmesini unutmayın.

[Adlandırılmış varlık tanıma sürüm 3,1-Önizleme başvurusu `PII`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesRecognitionPii)

---

Metin Analizi API'si anahtarınızı dahil etmek için bir istek üst bilgisi ayarlayın. İstek gövdesinde, hazırladığınız JSON belgelerini sağlayın.

### <a name="example-ner-request"></a>Örnek NER isteği 

Aşağıda, API 'ye gönderebilecek içeriklere bir örnek verilmiştir. İstek biçimi, her iki API sürümü için de aynıdır.

```json
{
  "documents": [
    {
        "id": "1",
        "language": "en",
        "text": "Our tour guide took us up the Space Needle during our trip to Seattle last week."
    }
  ]
}

```

## <a name="post-the-request"></a>İsteği gönder

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye başına gönderebilmeniz için isteklerin boyutu ve sayısı hakkında genel bakış konusundaki [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Metin Analizi API'si durum bilgisiz. Hesabınızda hiç veri depolanmaz ve sonuçlar yanıt içinde hemen döndürülür.

## <a name="view-results"></a>Sonuçları görüntüleme

Tüm POST istekleri, kimlik ve algılanan varlık özelliklerine sahip JSON biçimli bir yanıt döndürür.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz. Çok dilli ve Emoji desteği nedeniyle, yanıt metin uzaklıkları içerebilir. Daha fazla bilgi için bkz. [metin farklarını işleme](../concepts/text-offsets.md) .

### <a name="example-responses"></a>Örnek yanıtlar

Sürüm 3, genel NER, PII ve varlık bağlama için ayrı uç noktalar sağlar. Her iki işlem için de yanıtlar aşağıda verilmiştir. 

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

Genel bir NER yanıtı örneği:
```json
{
  "documents": [
    {
      "id": "1",
      "entities": [
        {
          "text": "tour guide",
          "category": "PersonType",
          "offset": 4,
          "length": 10,
          "confidenceScore": 0.45
        },
        {
          "text": "Space Needle",
          "category": "Location",
          "offset": 30,
          "length": 12,
          "confidenceScore": 0.38
        },
        {
          "text": "trip",
          "category": "Event",
          "offset": 54,
          "length": 4,
          "confidenceScore": 0.78
        },
        {
          "text": "Seattle",
          "category": "Location",
          "subcategory": "GPE",
          "offset": 62,
          "length": 7,
          "confidenceScore": 0.78
        },
        {
          "text": "last week",
          "category": "DateTime",
          "subcategory": "DateRange",
          "offset": 70,
          "length": 9,
          "confidenceScore": 0.8
        }
      ],
      "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-04-01"
}
```
#### <a name="version-31-preview"></a>[Sürüm 3,1-Önizleme](#tab/version-3-preview)

PII yanıtı örneği:
```json
{
  "documents": [
    {
    "redactedText": "You can even pre-order from their online menu at *************************, call ************ or send email to ***************************!",
    "id": "0",
    "entities": [
        {
        "text": "www.contososteakhouse.com",
        "category": "URL",
        "offset": 49,
        "length": 25,
        "confidenceScore": 0.8
        }, 
        {
        "text": "312-555-0176",
        "category": "Phone Number",
        "offset": 81,
        "length": 12,
        "confidenceScore": 0.8
        }, 
        {
        "text": "order@contososteakhouse.com",
        "category": "Email",
        "offset": 111,
        "length": 27,
        "confidenceScore": 0.8
        }
      ],
    "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-07-01"
}
```
Bir varlık bağlama yanıtı örneği:
```json
{
  "documents": [
    {
      "id": "1",
      "entities": [
        {
          "bingId": "f8dd5b08-206d-2554-6e4a-893f51f4de7e", 
          "name": "Space Needle",
          "matches": [
            {
              "text": "Space Needle",
              "offset": 30,
              "length": 12,
              "confidenceScore": 0.4
            }
          ],
          "language": "en",
          "id": "Space Needle",
          "url": "https://en.wikipedia.org/wiki/Space_Needle",
          "dataSource": "Wikipedia"
        },
        {
          "bingId": "5fbba6b8-85e1-4d41-9444-d9055436e473",
          "name": "Seattle",
          "matches": [
            {
              "text": "Seattle",
              "offset": 62,
              "length": 7,
              "confidenceScore": 0.25
            }
          ],
          "language": "en",
          "id": "Seattle",
          "url": "https://en.wikipedia.org/wiki/Seattle",
          "dataSource": "Wikipedia"
        }
      ],
      "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-02-01"
}
```
---


## <a name="summary"></a>Özet

Bu makalede, bilişsel hizmetler 'deki Metin Analizi kullanarak varlık bağlama kavramlarını ve iş akışını öğrendiniz. Özet:

* İstek gövdesindeki JSON belgeleri bir KIMLIK, metin ve dil kodu içerir.
* POST istekleri bir veya daha fazla uç noktaya gönderilir, bu, kişiselleştirilmiş bir [erişim anahtarı ve aboneliğiniz için geçerli bir uç nokta](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) kullanılarak gönderilir.
* Bağlı varlıklardan oluşan yanıt çıkışı (her belge KIMLIĞI için güven puanları, uzaklıklar ve Web bağlantıları dahil) herhangi bir uygulamada kullanılabilir

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizine genel bakış](../overview.md)
* [Metin Analizi istemci kitaplığını kullanma](../quickstarts/text-analytics-sdk.md)
* [Yenilikler](../whats-new.md)
