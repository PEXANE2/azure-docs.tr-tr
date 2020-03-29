---
title: Veri ayıklama - LUIS
description: Kasıtlı ve varlıklarla birlikte, söyleyiş metninden veri ayıklayın. Dil Anlama (LUIS)'den ne tür verilerin çıkarılabildiği hakkında bilgi edinin.
author: diberry
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: 1c1a744c06e5347625fb96518bd809481ee797e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79221089"
---
# <a name="extract-data-from-utterance-text-with-intents-and-entities"></a>Kasıtlı ve varlıklarla birlikte söyleyiş metninden veri ayıklama
LUIS, bir kullanıcının doğal dil söyleyişlerinden bilgi alma olanağı sağlar. Bilgiler, harekete geçmek için bir program, uygulama veya sohbet botu tarafından kullanılabilecek şekilde ayıklanır. Aşağıdaki bölümlerde, niyet ve kuruluşlardan json örnekleriyle hangi verilerin döndürülddöndüğünü öğrenin.

Ayıklanması en zor veri makineden öğrenilen verilerdir, çünkü tam bir metin eşleşmesi değildir. Makinede öğrenilen [varlıkların](luis-concept-entity-types.md) veri ayıklanması, beklediğiniz verileri aldığınızdan emin olana kadar [yazma döngüsünün](luis-concept-app-iteration.md) bir parçası olmalıdır.

## <a name="data-location-and-key-usage"></a>Veri konumu ve anahtar kullanımı
LUIS, yayınlanan bitiş [noktasından](luis-glossary.md#endpoint)verileri sağlar. **HTTPS isteği** (POST veya GET), düzenleme veya üretim ortamları gibi bazı isteğe bağlı yapılandırmaların yanı sıra söyleyiyi de içerir.

#### <a name="v2-prediction-endpoint-request"></a>[V2 tahmini bitiş noktası isteği](#tab/V2)

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

#### <a name="v3-prediction-endpoint-request"></a>[V3 tahmini bitiş noktası isteği](#tab/V3)

`https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/<appID>/slots/<slot-type>/predict?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&query=book 2 tickets to paris`

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

Luis `appID` uygulamanızın **Ayarlar** sayfasında ve bu LUIS uygulamasını düzenlerken `/apps/`URL'nin bir bölümünde (sonra) kullanılabilir. Uygulamanızı `subscription-key` sorgulamak için kullanılan bitiş noktası anahtarıdır. LUIS'i öğrenirken ücretsiz yazma/başlangıç anahtarınızı kullanabilirsiniz, ancak bitiş noktası anahtarını beklenen LUIS [kullanımınızı](luis-boundaries.md#key-limits)destekleyen bir anahtarla değiştirmek önemlidir. Birim `timezoneOffset` dakikalar.

**HTTPS yanıtı,** LUIS'in evreleme veya üretim bitiş noktasının geçerli yayımlanmış modeline göre belirleyebileceği tüm amaç ve varlık bilgilerini içerir. Bitiş noktası URL'si [LUIS](luis-reference-regions.md) web sitesinde, **Yönet** bölümünde, **Anahtarlar ve uç noktalar** sayfasında bulunur.

## <a name="data-from-intents"></a>Niyetlerden gelen veriler
Birincil veri en yüksek puanlama **niyet adıdır.** Bitiş noktası yanıtı:

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

```JSON
{
  "query": "when do you open next?",
  "prediction": {
    "normalizedQuery": "when do you open next?",
    "topIntent": "GetStoreInfo",
    "intents": {
        "GetStoreInfo": {
            "score": 0.984749258
        }
    }
  },
  "entities": []
}
```

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

|Veri Nesnesi|Veri Türü|Veri Konumu|Değer|
|--|--|--|--|
|Amaç|Dize|topScoringIntent.intent|"GetStoreInfo"|

Chatbot veya LUIS arama uygulamanız birden fazla niyet puanına dayalı bir karar veriyorsa, tüm niyet puanlarıiade edin.


#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

Sorgu string parametresini `verbose=true`ayarlayın, . Bitiş noktası yanıtı:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

Sorgu string parametresini `show-all-intents=true`ayarlayın, . Bitiş noktası yanıtı:

```JSON
{
    "query": "when do you open next?",
    "prediction": {
        "normalizedQuery": "when do you open next?",
        "topIntent": "GetStoreInfo",
        "intents": {
            "GetStoreInfo": {
                "score": 0.984749258
            },
            "None": {
                 "score": 0.2040639
            }
        },
        "entities": {
        }
    }
}
```

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

Niyetler en yüksekten en düşük puana kadar sıralanır.

|Veri Nesnesi|Veri Türü|Veri Konumu|Değer|Puan|
|--|--|--|--|:--|
|Amaç|Dize|niyet[0].niyet|"GetStoreInfo"|0.984749258|
|Amaç|Dize|niyet[1].niyet|"Yok"|0.0168218873|

Önceden oluşturulmuş etki alanları eklerseniz, niyet adı, `Utilties` amaç `Communication` gibi etki alanını veya yanı sıra, aşağıdakileri gösterir:

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

```JSON
{
    "query": "Turn on the lights next monday at 9am",
    "prediction": {
        "normalizedQuery": "Turn on the lights next monday at 9am",
        "topIntent": "Utilities.ShowNext",
        "intents": {
            "Utilities.ShowNext": {
                "score": 0.07842206
            },
            "Communication.StartOver": {
                "score": 0.0239675418
            },
            "None": {
                "score": 0.00085447653
            }
        },
        "entities": []
    }
}
```

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

|Domain|Veri Nesnesi|Veri Türü|Veri Konumu|Değer|
|--|--|--|--|--|
|Yardımcı Programlar|Amaç|Dize|niyet[0].niyet|"<b>Yardımcı Programlar</b>. ShowNext"|
|İletişim|Amaç|Dize|niyet[1].niyet|<b>İletişim</b>. Başlangıç"|
||Amaç|Dize|niyet[2].niyet|"Yok"|


## <a name="data-from-entities"></a>Varlıklardan gelen veriler
Çoğu chatbot ve uygulama niyet adından daha fazlasına ihtiyaç duyar. Bu ek, isteğe bağlı veriler, sözcükte bulunan varlıklardan gelir. Her varlık türü eşleşme hakkında farklı bilgiler döndürür.

Bir sözcük veya tümcecik birden fazla varlıkla eşleşebilir. Bu durumda, eşleşen her varlık kendi puanı ile döndürülür.

Tüm varlıklar bitiş noktasından yanıtın **varlıklar** dizisinde döndürülür:

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Name",
    "startIndex": 0,
    "endIndex": 8,
    "score": 0.473899543
  },
  {
    "entity": "3",
    "type": "builtin.number",
    "startIndex": 16,
    "endIndex": 16,
    "resolution": {
      "value": "3"
    }
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

```JSON
"entities": {
    "name":["bob jones"],
    "number": [3]
}
```
[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

## <a name="tokenized-entity-returned"></a>Tokenized varlık döndü

LUIS'teki [belirteç desteğini](luis-language-support.md#tokenization) gözden geçirin.

## <a name="simple-entity-data"></a>Basit varlık verileri

Basit bir [varlık](reference-entity-simple.md) makineden öğrenilen bir değerdir. Bu bir kelime ya da tümcecik olabilir.

## <a name="composite-entity-data"></a>Bileşik varlık verileri

[Bileşik varlık,](reference-entity-composite.md) önceden oluşturulmuş varlıklar, basit, düzenli ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar bütün bir varlık oluşturur.

## <a name="list-entity-data"></a>Varlık verilerini listele

[Liste varlıkları,](reference-entity-list.md) eşanlamlılarıyla birlikte sabit, kapalı bir ilişkili sözcük kümesini temsil ederler. LUIS liste varlıkları için ek değerler keşfetmez. Geçerli listeye dayalı yeni sözcükler için öneriler görmek için **Öner** özelliğini kullanın. Aynı değere sahip birden fazla liste varlığı varsa, her varlık bitiş noktası sorgusunda döndürülür.

## <a name="prebuilt-entity-data"></a>Önceden oluşturulmuş varlık verileri
[Önceden oluşturulmuş](luis-concept-entity-types.md) varlıklar, açık kaynak [tanırlar-Metin](https://github.com/Microsoft/Recognizers-Text) projesini kullanarak normal bir ifade eşleşmesi temel alınarak keşfedilir. Önceden oluşturulmuş varlıklar varlıklar dizisinde döndürülür ve `builtin::`önceden önceden belirlenmiş tür adını kullanır. Aşağıdaki metin, döndürülen önceden oluşturulmuş varlıklarla ilgili örnek bir sözdür:

`Dec 5th send to +1 360-555-1212`

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
"entities": [
    {
      "entity": "dec 5th",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2017-12-05"
          },
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2018-12-05"
          }
        ]
      }
    },
    {
      "entity": "1",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 18,
      "resolution": {
        "value": "1"
      }
    },
    {
      "entity": "360",
      "type": "builtin.number",
      "startIndex": 20,
      "endIndex": 22,
      "resolution": {
        "value": "360"
      }
    },
    {
      "entity": "555",
      "type": "builtin.number",
      "startIndex": 26,
      "endIndex": 28,
      "resolution": {
        "value": "555"
      }
    },
    {
      "entity": "1212",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 35,
      "resolution": {
        "value": "1212"
      }
    },
    {
      "entity": "5th",
      "type": "builtin.ordinal",
      "startIndex": 4,
      "endIndex": 6,
      "resolution": {
        "value": "5"
      }
    },
    {
      "entity": "1 360 - 555 - 1212",
      "type": "builtin.phonenumber",
      "startIndex": 18,
      "endIndex": 35,
      "resolution": {
        "value": "1 360 - 555 - 1212"
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

Querystring parametresi `verbose=true`olmadan, :

```json
"entities": {
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "XXXX-12-05",
                    "value": "2018-12-05"
                },
                {
                    "timex": "XXXX-12-05",
                    "value": "2019-12-05"
                }
            ]
        }
    ],
    "ordinal": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "ordinalV2": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "number": [
        1360,
        555,
        1212
    ],
    "phonenumber": [
        "1 360-555-1212"
    ]
}
```

Querystring parametresi `verbose=true`ile:

```json

"entities": {
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "XXXX-12-05",
                    "value": "2018-12-05"
                },
                {
                    "timex": "XXXX-12-05",
                    "value": "2019-12-05"
                }
            ]
        }
    ],
    "ordinal": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "ordinalV2": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "number": [
        1360,
        555,
        1212
    ],
    "phonenumber": [
        "1 360-555-1212"
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.date",
                "text": "Dec 5th",
                "startIndex": 0,
                "length": 7,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "ordinal": [
            {
                "type": "builtin.ordinal",
                "text": "5th",
                "startIndex": 4,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2",
                "text": "5th",
                "startIndex": 4,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "number": [
            {
                "type": "builtin.number",
                "text": "1 360",
                "startIndex": 17,
                "length": 5,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.number",
                "text": "555",
                "startIndex": 23,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.number",
                "text": "1212",
                "startIndex": 27,
                "length": 4,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "phonenumber": [
            {
                "type": "builtin.phonenumber",
                "text": "1 360-555-1212",
                "startIndex": 17,
                "length": 14,
                "score": 1.0,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *
## <a name="regular-expression-entity-data"></a>Normal ifade varlık verileri

[Normal bir ifade varlığı,](reference-entity-regular-expression.md) sağladığınız normal bir ifade desenine göre bir varlığı ayıklar.

## <a name="extracting-names"></a>Adları ayıklama
Bir ad hemen hemen herhangi bir harf ve kelime kombinasyonu olabilir, çünkü bir söyleyiş isimleri almak zordur. Ayıklamakta olduğunuz ad türüne bağlı olarak, birkaç seçeneğiniz var. Aşağıdaki öneriler kurallar değil, daha fazla yönergedir.

### <a name="add-prebuilt-personname-and-geographyv2-entities"></a>Önceden oluşturulmuş PersonName ve GeographyV2 varlıkları ekleme

[PersonName](luis-reference-prebuilt-person.md) ve [GeographyV2](luis-reference-prebuilt-geographyV2.md) varlıkları bazı [dil kültürlerinde](luis-reference-prebuilt-entities.md)mevcuttur.

### <a name="names-of-people"></a>Kişilerin adları

İnsanların adı, dil ve kültüre bağlı olarak hafif bir biçime sahip olabilir. Ad ve soyad [rolleri](luis-concept-roles.md) olan önceden oluşturulmuş bir **[kişi adı](luis-reference-prebuilt-person.md)** varlığı veya basit bir **[varlık](luis-concept-entity-types.md#simple-entity)** kullanın.

Basit varlığı kullanıyorsanız, söyleyincin farklı bölümlerinde, farklı uzunluklarda ve None niyeti de dahil olmak üzere tüm niyetler arasında söyleyerek ad ve soyad kullanan örnekler verdiğinizden emin olun. Doğru tahmin edilemedi herhangi bir ad etiketlemek için düzenli olarak bitiş noktası söyleyişlerini [gözden geçirin.](luis-how-to-review-endoint-utt.md)

### <a name="names-of-places"></a>Yerlerin adları

Konum adları şehirler, ilçeler, eyaletler, eyaletler ve ülkeler/bölgeler gibi ayarlanır ve bilinir. Konum bilgilerini ayıklamak için önceden oluşturulmuş varlık **[coğrafyasıV2'yi](luis-reference-prebuilt-geographyv2.md)** kullanın.

### <a name="new-and-emerging-names"></a>Yeni ve yeni çıkan isimler

Bazı uygulamaların ürün veya şirket gibi yeni ve yeni çıkan adları bulabilmeleri gerekir. Bu tür adlar en zor veri ayıklama türüdür. Basit bir **[varlıkla](luis-concept-entity-types.md#simple-entity)** başlayın ve [bir ifade listesi](luis-concept-feature.md)ekleyin. Doğru tahmin edilemedi herhangi bir ad etiketlemek için düzenli olarak bitiş noktası söyleyişlerini [gözden geçirin.](luis-how-to-review-endoint-utt.md)

## <a name="pattern-roles-data"></a>Desen rolleri verileri
Roller varlıkların bağlamsal farklılıklarıdır.


#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

Varlık adı `Location`, iki `Origin` rol `Destination`ile ve .

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Employee",
    "startIndex": 5,
    "endIndex": 13,
    "score": 0.922820568,
    "role": ""
  },
  {
    "entity": "seattle",
    "type": "Location",
    "startIndex": 20,
    "endIndex": 26,
    "score": 0.948008537,
    "role": "Origin"
  },
  {
    "entity": "redmond",
    "type": "Location",
    "startIndex": 31,
    "endIndex": 37,
    "score": 0.7047979,
    "role": "Destination"
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

V3'te **rol adı** nesnenin birincil adıdır.

Varlık adı `Location`, iki `Origin` rol `Destination`ile ve .

Querystring parametresi `verbose=true`olmadan, :

```json
"entities": {
    "Employee": [
        "bob jones"
    ],
    "Origin": [
        "seattle"
    ],
    "Destination": [
        "redmond"
    ]
}
```

Querystring parametresi `verbose=true`ile:

```json
"entities": {
    "Employee": [
        "bob jones"
    ],
    "LocationOrigin": [
        "seattle"
    ],
    "LocationDestination": [
        "redmond"
    ],
    "$instance": {
        "Employee": [
            {
                "type": "Employee",
                "text": "bob jones",
                "startIndex": 5,
                "length": 9,
                "score": 0.982873261,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Origin": [
            {
                "role": "Origin",
                "type": "Location",
                "text": "seattle",
                "startIndex": 20,
                "length": 7,
                "score": 0.9913306,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Destination": [
            {
                "role": "Destination",
                "type": "Location",
                "text": "redmond",
                "startIndex": 31,
                "length": 7,
                "score": 0.898179531,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]

}
```

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

## <a name="patternany-entity-data"></a>Desen.herhangi bir varlık verileri

[Desen.any,](reference-entity-pattern-any.md) varlığın nerede başlayıp nerede bittiğini işaretlemek için yalnızca bir desenin şablon uyrmasında kullanılan değişken uzunlukta bir yer tutucudur.

## <a name="sentiment-analysis"></a>Yaklaşım analizi
Duygusallık analizi yapılandırılırsa, LUIS json yanıtı duygusallık analizini içerir. [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) belgelerinde duygu analizi hakkında daha fazla bilgi edinin.

### <a name="sentiment-data"></a>Duyarlılık verileri
Duyarlılık verileri, verilerin pozitif (1'e yakın) veya negatif (0'a yakın) duyarlılığını gösteren 1 ile 0 arasında bir puandır.

Kültür olduğunda, `en-us`yanıt:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Diğer tüm kültürler için, yanıt:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Anahtar tümcecik çıkarma varlık verileri
Anahtar tümcecik çıkarma varlığı, [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/)tarafından sağlanan sözcükteki anahtar tümcecikleri döndürür.


#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
{
  "query": "Is there a map of places with beautiful views on a favorite trail?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.764368951
  },
  "intents": [
    ...
  ],
  "entities": [
    {
      "entity": "beautiful views",
      "type": "builtin.keyPhrase",
      "startIndex": 30,
      "endIndex": 44
    },
    {
      "entity": "map of places",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 23
    },
    {
      "entity": "favorite trail",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 64
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

Querystring parametresi `verbose=true`olmadan, :

```json
"entities": {
    "keyPhrase": [
        "map of places",
        "beautiful views",
        "favorite trail"
    ]
}
```

Querystring parametresi `verbose=true`ile:

```json
"entities": {
    "keyPhrase": [
        "map of places",
        "beautiful views",
        "favorite trail"
    ],
    "$instance": {
        "keyPhrase": [
            {
                "type": "builtin.keyPhrase",
                "text": "map of places",
                "startIndex": 11,
                "length": 13,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "beautiful views",
                "startIndex": 30,
                "length": 15,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "favorite trail",
                "startIndex": 51,
                "length": 14,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *


## <a name="data-matching-multiple-entities"></a>Birden çok varlıkla eşleşen veriler

LUIS, söyleyiş içinde keşfedilen tüm varlıkları döndürür. Sonuç olarak, chatbot'unuzun sonuçlara göre karar vermesi gerekebilir. Bir söyleyiş, bir söyleyiş içinde birçok varlık olabilir:

`book me 2 adult business tickets to paris tomorrow on air france`

LUIS bitiş noktası farklı varlıklarda aynı verileri keşfedebilirsiniz.

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
{
  "query": "book me 2 adult business tickets to paris tomorrow on air france",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 1.0
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 1.0
    },
    {
      "intent": "Concierge",
      "score": 0.04216196
    },
    {
      "intent": "None",
      "score": 0.03610297
    }
  ],
  "entities": [
    {
      "entity": "air france",
      "type": "Airline",
      "startIndex": 54,
      "endIndex": 63,
      "score": 0.8291798
    },
    {
      "entity": "adult",
      "type": "Category",
      "startIndex": 10,
      "endIndex": 14,
      "resolution": {
        "values": [
          "adult"
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 36,
      "endIndex": 40,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    },
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 42,
      "endIndex": 49,
      "resolution": {
        "values": [
          {
            "timex": "2018-02-21",
            "type": "date",
            "value": "2018-02-21"
          }
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 36,
      "endIndex": 40,
      "score": 0.9730773
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "business",
      "type": "Seat",
      "startIndex": 16,
      "endIndex": 23,
      "resolution": {
        "values": [
          "business"
        ]
      }
    },
    {
      "entity": "2 adult business",
      "type": "TicketSeatOrder",
      "startIndex": 8,
      "endIndex": 23,
      "score": 0.8784727
    }
  ],
  "compositeEntities": [
    {
      "parentType": "TicketSeatOrder",
      "value": "2 adult business",
      "children": [
        {
          "type": "Category",
          "value": "adult"
        },
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Seat",
          "value": "business"
        }
      ]
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

Sorgu `verbose=true` dize parametresi olarak olmadan.

```json
"entities": {
    "TicketsOrder": [
        {
            "number": [
                2
            ],
            "PassengerCategory": [
                "adult"
            ],
            "TravelClass": [
                "business"
            ]
        }
    ],
    "Location::LocationTo": [
        "paris"
    ],
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "2019-09-28",
                    "value": "2019-09-28"
                }
            ]
        }
    ],
    "Airline": [
        "air france"
    ]
}
```

Sorgudizmi `verbose=true` parametresi olarak.


```json
"entities": {
    "TicketsOrder": [
        {
            "number": [
                2
            ],
            "PassengerCategory": [
                "adult"
            ],
            "TravelClass": [
                "business"
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "2",
                        "startIndex": 8,
                        "length": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "PassengerCategory": [
                    {
                        "type": "PassengerCategory",
                        "text": "adult",
                        "startIndex": 10,
                        "length": 5,
                        "score": 0.9503733,
                        "modelTypeId": 3,
                        "modelType": "Hierarchical Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "TravelClass": [
                    {
                        "type": "TravelClass",
                        "text": "business",
                        "startIndex": 16,
                        "length": 8,
                        "score": 0.950095,
                        "modelTypeId": 3,
                        "modelType": "Hierarchical Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    ],
    "Location::LocationTo": [
        "paris"
    ],
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "2019-09-28",
                    "value": "2019-09-28"
                }
            ]
        }
    ],
    "Airline": [
        "air france"
    ],
    "$instance": {
        "TicketsOrder": [
            {
                "type": "TicketsOrder",
                "text": "2 adult business",
                "startIndex": 8,
                "length": 16,
                "score": 0.942183256,
                "modelTypeId": 4,
                "modelType": "Composite Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Location::LocationTo": [
            {
                "type": "Location::LocationTo",
                "text": "paris",
                "startIndex": 36,
                "length": 5,
                "score": 0.9905354,
                "modelTypeId": 3,
                "modelType": "Hierarchical Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.date",
                "text": "tomorrow",
                "startIndex": 42,
                "length": 8,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Airline": [
            {
                "type": "Airline",
                "text": "air france",
                "startIndex": 54,
                "length": 10,
                "score": 0.9455415,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

## <a name="data-matching-multiple-list-entities"></a>Birden çok liste varlığıyla eşleşen veriler

Bir sözcük veya tümcecik birden fazla liste varlığıyla eşleşirse, bitiş noktası sorgusu her Liste varlığını döndürür.

Sorgu `when is the best time to go to red rock?`için ve uygulama birden `red` fazla listede kelime vardır, LUIS tüm varlıkları tanır ve JSON bitiş noktası yanıtının bir parçası olarak varlıkların bir dizi döndürür:

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```



#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

Sorgu `verbose=true` dizesinde olmadan:

```JSON
{
    "query": "when is the best time to go to red rock",
    "prediction": {
        "normalizedQuery": "when is the best time to go to red rock",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "Colors": [
                [
                    "red"
                ]
            ],
            "Cities": [
                [
                    "Destinations"
                ]
            ]
        }
    }
}
```


Sorgu `verbose=true` dizesinde:

```JSON
{
    "query": "when is the best time to go to red rock",
    "prediction": {
        "normalizedQuery": "when is the best time to go to red rock",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "Colors": [
                [
                    "red"
                ]
            ],
            "Cities": [
                [
                    "Destinations"
                ]
            ],
            "$instance": {
                "Colors": [
                    {
                        "type": "Colors",
                        "text": "red",
                        "startIndex": 31,
                        "length": 3,
                        "modelTypeId": 5,
                        "modelType": "List Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "Cities": [
                    {
                        "type": "Cities",
                        "text": "red rock",
                        "startIndex": 31,
                        "length": 8,
                        "modelTypeId": 5,
                        "modelType": "List Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    }
}
```

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

## <a name="next-steps"></a>Sonraki adımlar

Bkz. LUIS uygulamanıza nasıl varlık ekleyeceğiniz hakkında daha fazla bilgi edinmek için [taraf ekle'](luis-how-to-add-entities.md) ye bakın.
