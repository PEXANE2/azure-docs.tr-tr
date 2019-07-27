---
title: Veri ayıklama-LUSıS
titleSuffix: Azure Cognitive Services
description: Amaç ve varlıklar ile söylenişi metinlerdeki verileri ayıklayın. Language Understanding (LUSıS) öğesinden ne tür verilerin ayıklanabileceği hakkında bilgi edinin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 055cd25f534de5d3cc3ccbe44df88e7111e101a3
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560749"
---
# <a name="extract-data-from-utterance-text-with-intents-and-entities"></a>Amaç ve varlıklar ile söylenişi metinlerinde veri ayıklama
LUIS, bir kullanıcının doğal dil konuşma bilgi almak için sağlar. Bilgiler bir program, uygulama veya sohbet Robotu eyleme kullanılabilmesi için bir şekilde ayıklanır. Aşağıdaki bölümlerde, hangi verilerin hedefleri ve JSON örneklerini varlıklarla döndürülür öğrenin.

Bir tam metin eşleşmesi olmadığından Ayıklanacak en zor veriler makine tarafından öğrenilen veri. Makine tarafından öğrenilen [varlıkların](luis-concept-entity-types.md) veri ayıklamasıyla, beklediğiniz verileri elde edinceye kadar, [yazma döngüsünün](luis-concept-app-iteration.md) bir parçası olması gerekir.

## <a name="data-location-and-key-usage"></a>Veri konum ve anahtar kullanımı
LUIS, yayımlanan verilerden sağlar [uç nokta](luis-glossary.md#endpoint). **HTTPS isteğini** hazırlık veya üretim ortamları gibi bazı isteğe bağlı yapılandırmalar yanı sıra utterance (POST veya GET) içerir.

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

, `appID` Lusıs uygulamanızın **Ayarlar** sayfasında ve bu Luin uygulamasını düzenlediğinizde URL 'nin (sonrasında `/apps/`) bir parçası olarak kullanılabilir. `subscription-key` Uygulamanızı sorgulamak için kullanılan uç noktası anahtarı. LUSıS öğrenirken ücretsiz yazma/başlangıç anahtarınızı kullanabilirsiniz, ancak Endpoint tuşunun [beklenen lusıs kullanımınızı](luis-boundaries.md#key-limits)destekleyen bir anahtarla değiştirilmesi önemlidir. `timezoneOffset` Dakika birimidir.

**HTTPS yanıtı** LUIS belirleyebilir hedefi ve varlık bilgilerini herhangi birinin geçerli yayımlanan model tabanlı hazırlık veya üretim uç noktası içerir. Uç nokta URL'si bulundu [LUIS](luis-reference-regions.md) Web sitesi, **Yönet** üzerinde bölümünde **anahtarları ve uç noktaları** sayfası.

## <a name="data-from-intents"></a>Intents verileri
Üst birincil verilerdir Puanlama **hedefi adı**. Kullanarak `MyStore` [hızlı](luis-quickstart-intents-only.md), uç nokta yanıt:

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

|Veri nesnesi|Veri Türü|Veri Konumu|Değer|
|--|--|--|--|
|Amaç|Dize|topScoringIntent.intent|"GetStoreInfo"|

Sohbet botu veya arama LUIS uygulama birden fazla hedefi puanına göre karar verir, sorgu dizesi parametresini ayarlayarak ıntents puanlarını döndürür `verbose=true`. Uç nokta yanıt şöyledir:

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

Intents en yüksek öncelikten en düşük puan için sıralanır.

|Veri nesnesi|Veri Türü|Veri Konumu|Değer|Puan|
|--|--|--|--|:--|
|Amaç|Dize|ıntents [0] .intent|"GetStoreInfo"|0.984749258|
|Amaç|Dize|ıntents [1] .intent|"None"|0.0168218873|

Önceden oluşturulmuş etki alanları eklerseniz, hedefi adı etki alanı gibi gösterir `Utilties` veya `Communication` amaç yanı sıra:

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

|Domain|Veri nesnesi|Veri Türü|Veri Konumu|Değer|
|--|--|--|--|--|
|Altyapı Hizmetleri|Amaç|Dize|ıntents [0] .intent|"<b>Yardımcı programları</b>. ShowNext"|
|İletişim|Amaç|Dize|ıntents [1] .intent|<b>İletişim</b>. StartOver"|
||Amaç|Dize|[2] hedefleri .intent|"None"|


## <a name="data-from-entities"></a>Veri varlıkları
Çoğu sohbet robotları ve uygulamaların birden çok hedefi adı gerekir. Bu ek, isteğe bağlı veri varlıkları utterance içinde bulunan gelir. Her varlık türü eşleşme ile ilgili farklı bilgileri döndürür.

Bir tek sözcük veya tümcecik bir utterance içinde birden fazla varlık eşleşebilir. Bu durumda, eşleşen her varlık kendi puanıyla döndürülür.

Tüm varlıklar döndürülür **varlıkları** uç noktasından yanıt bir dizi:

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

## <a name="tokenized-entity-returned"></a>parçalanmış varlık döndürdü
Birkaç [kültürler](luis-language-support.md#tokenization) ile varlık nesnesi döndürür `entity` değer [simgeleştirilmiş](luis-glossary.md#token). LUIS ile varlık nesnesi içinde döndürülen endIndex ve startIndex yeni, parçalanmış değerine sırada, ham varlık program aracılığıyla ayıklamak özgün sorguya ancak bunun yerine eşlemeyin. 

Örneğin, Almanca, sözcük içinde `das Bauernbrot` içine simgeleştirilmiş `das bauern brot`. Parçalanmış değeri `das bauern brot`, döndürülür ve özgün değeri, program aracılığıyla startIndex ve özgün sorgunun, ayırabilir endIndex belirlenebilir `das Bauernbrot`.

## <a name="simple-entity-data"></a>Basit bir varlığın verilerinin

A [varlığın](reference-entity-simple.md) bir makine öğrenilen değerdir. Bir sözcük veya tümcecik olabilir.

## <a name="composite-entity-data"></a>Bileşik bir varlığın verilerinin

[Bileşik bir varlık](reference-entity-composite.md) , önceden oluşturulmuş varlıklar, basit, normal ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar, tam bir varlık oluşturur. 

## <a name="list-entity-data"></a>Varlık verilerini listesi

[Liste varlıkları](reference-entity-list.md) , sabit ve kapalı bir ilgili sözcük kümesini eş anlamlılarıyla birlikte temsil eder. LUIS, liste varlıkları için ek değerler bulmaz. Kullanım **önerilir** yeni sözcükleri sunabileceği önerileri görmek için özellik geçerli listede bağlı. Birden fazla liste varlığı ile aynı değeri varsa, her varlık uç nokta sorguda döndürülür. 

## <a name="prebuilt-entity-data"></a>Önceden oluşturulmuş bir varlığın verilerinin
[Önceden oluşturulmuş](luis-concept-entity-types.md) varlıkları kullanarak açık kaynaklı bir normal ifade eşleştirmesi üzerinde temel saptandığında [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) proje. Önceden oluşturulmuş varlıklarla varlık dizide döndürülen ve tür adı ön ekine sahip kullanın `builtin::`. Aşağıda bir örnek utterance döndürülen önceden oluşturulmuş varlıklarla gösterilmiştir:

`Dec 5th send to +1 360-555-1212`

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

## <a name="regular-expression-entity-data"></a>Normal ifade varlık verilerini

[Normal ifade varlığı](reference-entity-regular-expression.md) , sağladığınız normal ifade düzenine göre bir varlığı ayıklar.

## <a name="extracting-names"></a>Ayıklanan adları
Bir ad, harf ve sözcükler neredeyse her bir birleşimi olabilir çünkü bir utterance adları alınıyor zordur. Hangi tür adı ayıkladığınıza bağlı olarak, birkaç seçeneğiniz vardır. Aşağıdaki öneriler kurallar değildir ancak daha fazla yönerge değildir.

### <a name="add-prebuilt-personname-and-geographyv2-entities"></a>Önceden oluşturulmuş PersonName ve GeographyV2 varlıklarını ekleyin

[PersonName](luis-reference-prebuilt-person.md) ve [GeographyV2](luis-reference-prebuilt-geographyV2.md) varlıkları bazı [dil kültürleri](luis-reference-prebuilt-entities.md)içinde kullanılabilir. 

### <a name="names-of-people"></a>Kişilerin adları

Kişi adı, dil ve kültür bağlı olarak bazı küçük biçimi olabilir. Önceden oluşturulmuş bir **[PersonName](luis-reference-prebuilt-person.md)** varlığı veya ad ve soyadı [rollerine](luis-concept-roles.md) sahip **[basit bir varlık](luis-concept-entity-types.md#simple-entity)** kullanın. 

Basit varlığı kullanırsanız, farklı uzunluklarda Farklı uzunluklardaki ilk ve son adı kullanan örneklere ve hiçbir türlü amaç dahil olmak üzere tüm amaçlar genelinde dikkat edilecek örneklere sahip olun. [Gözden geçirme](luis-how-to-review-endoint-utt.md) doğru şekilde tahmin değil herhangi bir adı etiketlemek için düzenli olarak konuşma uç noktası.

### <a name="names-of-places"></a>Basamak adları

Konum adları, şehir, ilçe, eyalet, eyalet ve ülke/bölge gibi bir şekilde ayarlanır ve bilinmektedir. Konum bilgilerini ayıklamak için önceden oluşturulan **[geographyV2](luis-reference-prebuilt-geographyv2.md)** varlığını kullanın.

### <a name="new-and-emerging-names"></a>Yeni ve geliştirilmekte olan adları

Bazı uygulamalar, ürünleri veya şirketler gibi yeni ve geliştirilmekte olan adlarını bulmak gerekir. Bu tür adlar, veri ayıklamanın en zor türüdür. **[Basit bir varlıkla](luis-concept-entity-types.md#simple-entity)** başlayın ve [tümcecik listesi](luis-concept-feature.md)ekleyin. [Gözden geçirme](luis-how-to-review-endoint-utt.md) doğru şekilde tahmin değil herhangi bir adı etiketlemek için düzenli olarak konuşma uç noktası.

## <a name="pattern-roles-data"></a>Desen rolleri veri
Roller, varlık bağlamsal fark vardır.

```JSON
{
  "query": "move bob jones from seattle to redmond",
  "topScoringIntent": {
    "intent": "MoveAssetsOrPeople",
    "score": 0.9999998
  },
  "intents": [
    {
      "intent": "MoveAssetsOrPeople",
      "score": 0.9999998
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 6.12244548E-07
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 6.12244548E-07
    },
    {
      "intent": "FindForm",
      "score": 1.1E-09
    }
  ],
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
}
```

## <a name="patternany-entity-data"></a>Varlık verilerini pattern.Any

[Desen. any](reference-entity-pattern-any.md) , varlığın nerede başladığını ve bittiğini işaretlemek için yalnızca bir desen şablonunda kullanılan değişken uzunluklu bir yer tutucudur.  

## <a name="sentiment-analysis"></a>Yaklaşım analizi
Yaklaşım analizi yapılandırılmışsa, yaklaşım analizi LUIS json yanıtı içerir. Yaklaşım analizi hakkında daha fazla bilgi [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) belgeleri.

### <a name="sentiment-data"></a>Duygu verilerini
Yaklaşım verilerdir pozitif gösteren 0 ile 1 arasındaki bir puan (1 yakın) veya (0 yakın) negatif yaklaşım veri.

Kültür olduğunda `en-us`, yanıt:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Diğer tüm kültürler için yanıt şöyledir:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Anahtar ifade ayıklama varlık verilerini
Anahtar ifade ayıklama varlık tarafından sağlanan utterance, anahtar ifadeleri döndürür [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

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

## <a name="data-matching-multiple-entities"></a>Birden çok varlık eşleşen veri

LUIS utterance içinde bulunan tüm varlıkları döndürür. Sonuç olarak, sohbet botu sonuçlarına göre karar vermeniz gerekebilir. Bir utterance birçok varlığın bir utterance sahip olabilir:

`book me 2 adult business tickets to paris tomorrow on air france`

LUIS uç nokta farklı varlıklarda aynı verileri bulabilir:

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

## <a name="data-matching-multiple-list-entities"></a>Birden çok liste varlığı ile eşleşen veriler

Bir sözcük veya tümcecik birden fazla liste varlığı eşleşirse, uç nokta sorgu her liste varlığı döndürür.

Sorgu için `when is the best time to go to red rock?`, ve uygulama word `red` LUIS birden fazla listesinde, tüm varlıkları tanıyan ve JSON bitiş noktası yanıtın bir parçası varlıkları bir dizi döndürür: 

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

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [varlık Ekle](luis-how-to-add-entities.md) LUIS uygulamanızı varlıklar ekleme hakkında daha fazla bilgi için.
