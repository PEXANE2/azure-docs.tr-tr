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
ms.date: 02/10/2020
ms.author: aahi
ms.openlocfilehash: 0622aca5579c64c6d840761abb151665af559eea
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2020
ms.locfileid: "77367647"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Metin Analizi içinde adlandırılmış varlık tanımayı kullanma

Metin Analizi API'si, yapılandırılmamış metin almanıza ve Web hakkında daha fazla bilgi için bağlantılarla birlikte, Kesinleştirme varlıklarının bir listesini döndürmenize olanak tanır. API, adlandırılmış varlık tanıma (NER) ve varlık bağlamayı destekler.

### <a name="entity-linking"></a>Varlık Bağlama

Varlık bağlama, metinde bulunan bir varlığın kimliğini belirleme ve ayırt etme olanağıdır (örneğin, bir sözcüğün `Mars` bir oluşumunu Planet 'e veya War 'ın gerçekleşen ' ine mi başvurduğunu belirlemek). Bu işlem, tanınan varlıkları metinde bağlamak için uygun bir dilde Bilgi Bankası 'nın bulunmasını gerektirir. Varlık bağlama, bu Bilgi Bankası olarak [Vikipedi](https://www.wikipedia.org/) kullanır.


### <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

Adlandırılmış varlık tanıma (NER), metinde farklı varlıkları belirleme ve bunları önceden tanımlanmış sınıflar veya türler, örneğin: kişi, konum, olay, ürün ve kuruluş gibi kategorilere ayırma yeteneğidir.  

Sürüm 3 ' te başlayarak Metin Analizi API'si bu özelliği, telefon numarası, sosyal güvenlik numarası, e-posta adresi ve banka hesap numarası gibi kişisel ve hassas bilgi türlerini de tanımlayabilir.  Bu varlıkların belirlenmesi, hassas belgeleri sınıflandırırken ve kişisel bilgilerin redakklarında yardımcı olabilir.

## <a name="named-entity-recognition-versions-and-features"></a>Adlandırılmış varlık tanıma sürümleri ve özellikleri

Metin Analizi API'si, adlandırılmış varlık tanıma-v2 ve v3 'in iki sürümünü sunmaktadır. Sürüm 3 (Genel Önizleme), tespit edilebilir ve kategorilere ayrılmamış varlıklarda daha fazla ayrıntı sağlar.

| Özellik                                                         | NER v2 | NER v3 |
|-----------------------------------------------------------------|--------|--------|
| Tek ve toplu istekler için Yöntemler                          | X      | X      |
| Çeşitli kategoriler genelinde temel varlık tanıma              | X      | X      |
| Tanınan varlıklar için genişletilmiş sınıflandırma                 |        | X      |
| Varlık bağlama ve NER istekleri göndermek için ayrı uç noktalar. |        | X      |
| Model sürümü oluşturma                                                |        | X      |

Bilgi için bkz. [dil desteği](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) .


#### <a name="version-30-previewtabversion-3"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

### <a name="entity-types"></a>Varlık türleri

Adlandırılmış varlık tanıma v3, birden çok tür genelinde genişletilmiş algılama sağlar. Şu anda NER v3 aşağıdaki varlık kategorilerini algılayabilir:

* Genel
* Kişisel bilgiler 

Desteklenen varlıkların ve dillerin ayrıntılı bir listesi için, [ner v3 desteklenen varlık türleri](../named-entity-types.md) makalesine bakın.

### <a name="request-endpoints"></a>İstek uç noktaları

Adlandırılmış varlık tanıma v3, NER ve varlık bağlama istekleri için ayrı uç noktalar kullanır. İsteğinize göre aşağıdan bir URL biçimi kullanın:

HI
* Genel varlıklar-`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Kişisel bilgiler-`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Varlık bağlama
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

### <a name="model-versioning"></a>Model sürümü oluşturma

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

#### <a name="version-21tabversion-2"></a>[Sürüm 2,1](#tab/version-2)

### <a name="entity-types"></a>Varlık türleri

> [!NOTE]
> Adlandırılmış varlık tanıma (NER) sürüm 2 yalnızca aşağıdaki varlıkları destekler. NER v3 genel önizlemededir ve metinde tanınan varlıkların sayısını ve derinliğini büyük ölçüde genişletir.   

| Tür  | SubType | Örnek |
|:-----------   |:------------- |:---------|
| Kişi        | Yok\*         | "Jeff", "Bill Gates"     |
| Konum      | Yok\*         | "Redmond, Washington", "Paris"  |
| Kuruluş  | Yok\*         | MICROSOFT   |
| Miktar      | Sayı        | "6", "altı"     |
| Miktar      | Yüzde    | "%50", "50 yüzdesi"|
| Miktar      | Numarasını       | "2.", "saniye"     |
| Miktar      | Yaş           | "90 gün eski", "30 yıl eski"    |
| Miktar      | Para birimi      | "$10,99"     |
| Miktar      | Boyut     | "10 mil", "40 cm"     |
| Miktar      | Sıcaklık   | "32 derece"    |
| DateTime      | Yok\*         | "6:12:30 4, 2012"      |
| DateTime      | Tarih          | "2 Mayıs, 2017", "05/02/2017"   |
| DateTime      | Zaman          | "08:00", "8:00"  |
| DateTime      | DateRange     | "2 Mayıs-5 Mayıs 'ta olabilir"    |
| DateTime      | TimeRange     | "6pm-7pm"     |
| DateTime      | Süre      | "1 dakika ve 45 saniye"   |
| DateTime      | Ayarla           | "her Salı"     |
| URL'si           | Yok\*         | "https:\//www.bing.com"    |
| Email         | Yok\*         | "support@contoso.com" |
| ABD telefon numarası  | Yok\*         | (Yalnızca ABD telefon numaraları) "(312) 555-0176" |
| IP Adresi    | Yok\*         | alana 10.0.0.100 |

giriş ve ayıklanan varlıklara göre \*, bazı varlıklar `SubType`yok edebilir.  Listelenen tüm desteklenen varlık türleri yalnızca Ingilizce, Çince-Basitleştirilmiş, Fransızca, Almanca ve Ispanyolca dillerde kullanılabilir.

### <a name="request-endpoints"></a>İstek uç noktaları

Adlandırılmış varlık tanıma v2, NER ve varlık bağlama istekleri için tek bir uç nokta kullanır:

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

---

## <a name="sending-a-rest-api-request"></a>REST API isteği gönderiliyor

### <a name="preparation"></a>Hazırlık

Bu biçimde JSON belgelerinize sahip olmanız gerekir: KIMLIK, metin, dil.

Her belge 5.120 karakterden oluşmalıdır ve koleksiyon başına en fazla 1.000 öğe (kimlik) olabilir. Koleksiyon, istek gövdesinde gönderilir.

### <a name="structure-the-request"></a>İsteği yapısı

POST isteği oluşturun. Hızlı bir şekilde yapılandırmak ve göndermek için aşağıdaki bağlantılardan [Postman](text-analytics-how-to-call-api.md) veya **API test konsolunu** kullanabilirsiniz. 

> [!NOTE]
> Azure portalında Metin Analizi kaynağınız için anahtarınızı ve uç noktanızı bulabilirsiniz. Kaynak **yönetimi**altında kaynağın **hızlı başlangıç** sayfasında yer alır. 

#### <a name="version-30-previewtabversion-3"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

[Adlandırılmış varlık tanıma v3 başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral)

Sürüm 3, NER ve varlık bağlama istekleri için ayrı uç noktalar kullanır. İsteğinize göre aşağıdan bir URL biçimi kullanın:

HI
* Genel varlıklar-`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Kişisel bilgi varlıkları-`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Varlık bağlama
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

#### <a name="version-21tabversion-2"></a>[Sürüm 2,1](#tab/version-2)

[Adlandırılmış varlık tanıma (NER) v2 başvurusu](https://eastus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

Sürüm 2, varlık bağlama ve NER istekleri için aşağıdaki uç noktayı kullanır: 

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

---

Metin Analizi API'si anahtarınızı dahil etmek için bir istek üst bilgisi ayarlayın. İstek gövdesinde, hazırladığınız JSON belgelerini sağlayın.

### <a name="example-ner-request"></a>Örnek NER isteği 

Aşağıda, API 'ye gönderebilecek içeriklere bir örnek verilmiştir. İstek biçimi, her iki API sürümü için de aynıdır.

```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "I had a wonderful trip to Seattle last week."
    }
  ]
}
```

## <a name="post-the-request"></a>İsteği gönder

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye başına gönderebilmeniz için isteklerin boyutu ve sayısı hakkında genel bakış konusundaki [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Metin Analizi API'si durum bilgisiz. Hesabınızda hiç veri depolanmaz ve sonuçlar yanıt içinde hemen döndürülür.

## <a name="view-results"></a>Sonuçları görüntüleme

Tüm POST istekleri, kimlik ve algılanan varlık özelliklerine sahip JSON biçimli bir yanıt döndürür.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz.


#### <a name="version-30-previewtabversion-3"></a>[Sürüm 3,0-Önizleme)](#tab/version-3)

### <a name="example-v3-responses"></a>Örnek v3 yanıtları

Sürüm 3, NER ve varlık bağlama için ayrı uç noktalar sağlar. Her iki işlem için de yanıtlar aşağıda verilmiştir.

#### <a name="example-ner-response"></a>Örnek NER yanıtı

```json
{
    "documents": [{
    "id": "1",
    "entities": [{
        "text": "Seattle",
        "type": "Location",
        "offset": 26,
        "length": 7,
        "score": 0.80624294281005859
    }, {
        "text": "last week",
        "type": "DateTime",
        "subtype": "DateRange",
        "offset": 34,
        "length": 9,
        "score": 0.8
    }]
    }],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```

#### <a name="example-entity-linking-response"></a>Örnek varlık bağlama yanıtı

```json
{
  "documents": [{
    "id": "1",
    "entities": [{
      "name": "Seattle",
      "matches": [{
        "text": "Seattle",
        "offset": 26,
        "length": 7,
        "score": 0.15046201222847677
      }],
      "language": "en",
      "id": "Seattle",
      "url": "https://en.wikipedia.org/wiki/Seattle",
      "dataSource": "Wikipedia"
    }]
  }],
  "errors": [],
  "modelVersion": "2019-10-01"
}
```

#### <a name="version-21tabversion-2"></a>[Sürüm 2,1](#tab/version-2)

### <a name="example-ner-v2-response"></a>Örnek NER v2 yanıtı
```json
{
  "documents": [{
    "id": "1",
    "entities": [{
      "name": "Seattle",
      "matches": [{
        "wikipediaScore": 0.15046201222847677,
        "entityTypeScore": 0.80624294281005859,
        "text": "Seattle",
        "offset": 26,
        "length": 7
      }],
      "wikipediaLanguage": "en",
      "wikipediaId": "Seattle",
      "wikipediaUrl": "https://en.wikipedia.org/wiki/Seattle",
      "bingId": "5fbba6b8-85e1-4d41-9444-d9055436e473",
      "type": "Location"
    }, {
      "name": "last week",
      "matches": [{
        "entityTypeScore": 0.8,
        "text": "last week",
        "offset": 34,
        "length": 9
      }],
      "type": "DateTime",
      "subType": "DateRange"
    }]
  }],
  "errors": []
}
```

---

## <a name="summary"></a>Özet

Bu makalede, bilişsel hizmetler 'deki Metin Analizi kullanarak varlık bağlama kavramlarını ve iş akışını öğrendiniz. Özet:

* Adlandırılmış varlık tanıma, iki sürümde seçili diller için kullanılabilir.
* İstek gövdesindeki JSON belgeleri bir KIMLIK, metin ve dil kodu içerir.
* POST istekleri bir veya daha fazla uç noktaya gönderilir, bu, kişiselleştirilmiş bir [erişim anahtarı ve aboneliğiniz için geçerli bir uç nokta](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) kullanılarak gönderilir.
* Bağlı varlıklardan oluşan yanıt çıkışı (her belge KIMLIĞI için güven puanları, uzaklıklar ve Web bağlantıları dahil) herhangi bir uygulamada kullanılabilir

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizine genel bakış](../overview.md)
* [Metin Analizi istemci kitaplığını kullanma](../quickstarts/text-analytics-sdk.md)
* [Yenilikler](../whats-new.md)
