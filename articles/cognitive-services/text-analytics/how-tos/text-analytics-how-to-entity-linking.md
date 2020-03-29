---
title: Metin Analizi API'si ile varlık tanıma yı kullanma
titleSuffix: Azure Cognitive Services
description: Text Analytics REST API ile metinde bulunan bir varlığın kimliğini nasıl tanımlayıp ayrıştıracaklarını öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 02/10/2020
ms.author: aahi
ms.openlocfilehash: 243086ddaae47eba20eea6877fe6d7f8f9889290
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79203500"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Metin Analizinde Adlandırılmış Varlık Tanıma nasıl kullanılır?

Text Analytics API, yapılandırılmamış metin almanıza ve web'deki daha fazla bilgiye bağlantılar içeren, kararsız varlıkların listesini döndürmenize olanak tanır. API hem adlandırılmış varlık tanıma (NER) hem de varlık bağlantısını destekler.

### <a name="entity-linking"></a>Varlık Bağlama

Varlık bağlama, metinde bulunan bir varlığın kimliğini tanımlama ve ayrıştırabilme yeteneğidir (örneğin, kelimenin `Mars` oluşumunun gezegene mi yoksa Roma savaş tanrısına mı atıfta bulunduğunu belirlemek). Bu işlem, metinde tanınan varlıkları bağlamak için uygun bir dilde bir bilgi tabanının bulunmasını gerektirir. Entity Linking bu bilgi üssü olarak [Vikipedi'yi](https://www.wikipedia.org/) kullanır.


### <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık Tanıma (NER)

Adlandırılmış Varlık Tanıma (NER), metindeki farklı varlıkları belirleyebilme ve bunları kişi, konum, etkinlik, ürün ve kuruluş gibi önceden tanımlanmış sınıflar veya türler halinde kategorize etme yeteneğidir.  

Metin Analizi API'sinin bu özelliği sürüm 3'ten başlayarak, telefon numarası, Sosyal Güvenlik Numarası, e-posta adresi ve banka hesap numarası gibi kişisel ve hassas bilgi türlerini de tanımlayabilir.  Bu varlıkların tanımlanması, hassas belgelerin sınıflandırılmasına ve kişisel bilgilerin yeniden düzenlenmesine yardımcı olabilir.

## <a name="named-entity-recognition-versions-and-features"></a>Adlandırılmış Varlık Tanıma sürümleri ve özellikleri

Text Analytics API, Adlandırılmış Varlık Tanıma'nın v2 ve v3 olmak gibi iki versiyonunu sunar. Sürüm 3 (Genel önizleme), algılanabilen ve kategorilere ayrılabilen varlıklarda daha fazla ayrıntı sağlar.

| Özellik                                                         | NER v2 | NER v3 |
|-----------------------------------------------------------------|--------|--------|
| Tek ve toplu iş istekleri için yöntemler                          | X      | X      |
| Çeşitli kategorilerde temel varlık tanıma              | X      | X      |
| Tanınan varlıklar için genişletilmiş sınıflandırma                 |        | X      |
| Varlık bağlantı ve NER isteklerini göndermek için ayrı uç noktaları. |        | X      |
| Model sürümü                                                |        | X      |

Bilgi için [dil desteğine](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) bakın.


#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

### <a name="entity-types"></a>Varlık türleri

Adlandırılmış Varlık Tanıma v3, birden çok türde genişletilmiş algılama sağlar. Şu anda, NER v3 varlıkların aşağıdaki kategorileri tanıyabilir:

* Genel
* Kişisel Bilgiler 

Desteklenen varlıkların ve dillerin ayrıntılı bir listesi için [NER v3 desteklenen varlık türleri](../named-entity-types.md) makalesine bakın.

### <a name="request-endpoints"></a>Uç noktaları isteme

Adlandırılmış Varlık Tanıma v3, NER ve varlık bağlantı istekleri için ayrı uç noktalar kullanır. İsteğinize göre aşağıda bir URL biçimi kullanın:

NER
* Genel varlıklar -`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Kişisel bilgiler -`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Varlık bağlama
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

### <a name="model-versioning"></a>Model sürümü

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

### <a name="entity-types"></a>Varlık türleri

> [!NOTE]
> Adlandırılmış Varlık Tanıma(NER) sürüm 2 yalnızca aşağıdaki varlıkları destekler. NER v3 genel önizlemededir ve metinde tanınan varlıkların sayısını ve derinliğini büyük ölçüde genişletir.   

| Tür  | SubType | Örnek |
|:-----------   |:------------- |:---------|
| Kişi        | Yok\*         | "Jeff", "Bill Gates"     |
| Konum      | Yok\*         | "Redmond, Washington", "Paris"  |
| Kuruluş  | Yok\*         | "Microsoft"   |
| Miktar      | Sayı        | "6", "altı"     |
| Miktar      | Yüzde    | "%50", "yüzde elli"|
| Miktar      | Sıralı       | "2.", "ikinci"     |
| Miktar      | Yaş           | "90 günlük", "30 yaşında"    |
| Miktar      | Para birimi      | "$10.99"     |
| Miktar      | Boyut     | "10 mil", "40 cm"     |
| Miktar      | Sıcaklık   | "32 derece"    |
| DateTime      | Yok\*         | "4 Şubat 2012 6:30"      |
| DateTime      | Tarih          | "2 Mayıs 2017", "05.02.2017"   |
| DateTime      | Zaman          | "Sabah 8", "8:00"  |
| DateTime      | Tarih Aralığı     | "2 Mayıs - 5 Mayıs"    |
| DateTime      | Zaman Aralığı     | "18:00-19:00"     |
| DateTime      | Süre      | "1 dakika 45 saniye"   |
| DateTime      | Ayarla           | "Her Salı"     |
| URL'si           | Yok\*         | "https:\//www.bing.com"    |
| Email         | Yok\*         | "support@contoso.com" |
| ABD Telefon Numarası  | Yok\*         | (Yalnızca ABD telefon numaraları) "(312) 555-0176" |
| IP Adresi    | Yok\*         | "10.0.0.100" |

\*Girdi ve ayıklanan varlıklara bağlı olarak, bazı varlıklar `SubType`.  Listelenen desteklenen tüm varlık türleri yalnızca İngilizce, Çince Basitleştirilmiş, Fransızca, Almanca ve İspanyolca dilleri için kullanılabilir.

### <a name="request-endpoints"></a>Uç noktaları isteme

Adlandırılmış Varlık Tanıma v2, NER ve varlık bağlantı istekleri için tek bir uç nokta kullanır:

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

---

## <a name="sending-a-rest-api-request"></a>REST API isteği gönderme

### <a name="preparation"></a>Hazırlık

Bu formatta JSON belgeleriniz olmalıdır: kimlik, metin, dil.

Her belge 5.120 karakterin altında olmalıdır ve koleksiyon başına en fazla 1.000 öğe (kimlik) olabilir. Koleksiyon, istek gövdesinde gönderilir.

### <a name="structure-the-request"></a>İsteği yapılandırma

Bir POST isteği oluşturun. [Postacı'yı](text-analytics-how-to-call-api.md) veya **API test** konsolu'nu aşağıdaki bağlantılarda kullanarak hızlı bir şekilde bir tane sini yapılandırAbilir ve gönderebilirsiniz. 

> [!NOTE]
> Metin Analizi kaynağınızın anahtarını ve bitiş noktanızı azure portalında bulabilirsiniz. Bunlar, **kaynak yönetimi**altında kaynağın **Hızlı başlangıç** sayfasında yer alır. 

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

[Adlandırılmış Varlık Tanıma v3 başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral)

Sürüm 3, NER ve varlık bağlantı istekleri için ayrı uç noktalar kullanır. İsteğinize göre aşağıda bir URL biçimi kullanın:

NER
* Genel varlıklar -`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Kişisel bilgi kuruluşları -`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Varlık bağlama
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

[Adlandırılmış Varlık Tanıma (NER) v2 başvurusu](https://eastus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

Sürüm 2 varlık bağlama ve NER istekleri için aşağıdaki bitiş noktasını kullanır: 

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

---

Metin Analizi API anahtarınızı içerecek şekilde bir istek üstbilgisi ayarlayın. İstek gövdesinde, hazırladığınız JSON belgelerini sağlayın.

### <a name="example-ner-request"></a>Örnek NER isteği 

Aşağıda, API'ye gönderebilecek içerik örneği verilmiştir. İstek biçimi, API'nin her iki sürümü için de aynıdır.

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

## <a name="post-the-request"></a>İsteğin ivetini gönderme

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye başına gönderebileceğiniz isteklerin boyutu ve sayısı hakkında bilgi için genel bakışta [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Text Analytics API'si devletsizdir. Hesabınızda hiçbir veri depolanır ve sonuçlar yanıtta hemen döndürülür.

## <a name="view-results"></a>Sonuçları görüntüleme

Tüm POST istekleri, d'ler ve algılanan varlık özellikleriyle JSON biçimlendirilmiş bir yanıt döndürer.

Hemen çıktı döndürülür. Sonuçları, JSON kabul eden bir uygulamada akışa alabilir veya çıktıyı yerel sistemde bir dosyaya kaydedebilir, sonra da verileri sıralamanıza, aramanıza ve işlemenize olanak sağlayan bir uygulamaya içeri aktarabilirsiniz. Çok dilli ve emoji desteği nedeniyle, yanıt metin uzaklıkları içerebilir. Daha fazla bilgi [için metin uzaklıklarını nasıl](../concepts/text-offsets.md) işleyeceğinize bakın.

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme)](#tab/version-3)

### <a name="example-v3-responses"></a>Örnek v3 yanıtları

Sürüm 3, NER ve varlık bağlama için ayrı uç noktalar sağlar. Her iki işlem için yanıtlar aşağıdadır. 

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

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

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

Bu makalede, Bilişsel Hizmetlerde Metin Analizi'ni kullanarak bağlantı sağlayan varlık için kavramları ve iş akışını öğrendiniz. Özet:

* Adlandırılmış Varlık Tanıma, seçili diller için iki sürümde kullanılabilir.
* İstek gövdesindeki JSON belgeleri bir kimlik, metin ve dil kodu içerir.
* POSTA istekleri, kişiselleştirilmiş erişim anahtarı ve aboneliğiniz için geçerli [olan bir uç nokta](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) kullanılarak bir veya daha fazla uç noktaya gönderilir.
* Her belge kimliği için bağlantılı varlıklardan (güven puanları, uzaklıklar ve web bağlantıları dahil) oluşan yanıt çıktısı herhangi bir uygulamada kullanılabilir

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizine genel bakış](../overview.md)
* [Text Analytics istemci kitaplığını kullanma](../quickstarts/text-analytics-sdk.md)
* [Yenilikler](../whats-new.md)
