---
title: DatetimeV2 önceden oluşturulmuş varlıklar-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makalede, Language Understanding (LUSıS) içinde datetimeV2 önceden oluşturulmuş varlık bilgileri bulunur.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 4f46efaeddb0bfe789ef752abdd133c14da514da
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677702"
---
# <a name="datetimev2-prebuilt-entity-for-a-luis-app"></a>LUSıS uygulaması için DatetimeV2 önceden oluşturulmuş varlık

**DatetimeV2** önceden oluşturulmuş varlık tarih ve saat değerlerini ayıklar. Bu değerler, istemci programlarının kullanması için standartlaştırılmış bir biçimde çözümlenir. Bir söylenişi 'nin tamamlanmamış bir tarih veya saati olduğunda, Luo, uç nokta yanıtında _hem geçmiş hem de gelecekteki değerleri_ içerir. Bu varlık zaten eğitiltiğinden, uygulama amaçlarını datetimeV2 içeren örnek bir değer eklemeniz gerekmez. 

## <a name="types-of-datetimev2"></a>DatetimeV2 türleri
DatetimeV2, [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-DateTime.yaml) GitHub deposundan yönetiliyor

## <a name="example-json"></a>Örnek JSON 
Aşağıdaki örnek JSON yanıtı, `datetime` ' in alt türüne sahip `datetimeV2` varlığına sahiptir. Diğer datetimeV2 varlık türlerinin örnekleri için bkz. datetimeV2 </a> alt [türleri](#subtypes-of-datetimev2).

```json
"entities": [
  {
    "entity": "8am on may 2nd 2019",
    "type": "builtin.datetimeV2.datetime",
    "startIndex": 0,
    "endIndex": 18,
    "resolution": {
      "values": [
        {
          "timex": "2019-05-02T08",
          "type": "datetime",
          "value": "2019-05-02 08:00:00"
        }
      ]
    }
  }
]
  ```

## <a name="json-property-descriptions"></a>JSON özelliği açıklamaları

|Özellik adı |Özellik türü ve açıklaması|
|---|---|
|Kurum|Tarih, saat, tarih aralığı veya zaman aralığı türü ile utterden çıkarılan **dize** metni.|
|type|**dize** - [datetimeV2 alt türlerinden](#subtypes-of-datetimev2) biri
|startIndex|**int** -varlığın başladığı noktada dizin.|
|endIndex|**int** -varlığın bittiği noktada dizin.|
|Çözünürlüğüne|, Bir, iki veya dört [Çözümleme değeri](#values-of-resolution)olan `values` dizisine sahiptir.|
|erer|Bir saatin veya tarih aralığının bitiş değeri `value` ile aynı biçimde. Yalnızca `type` `daterange`, `timerange` veya `datetimerange` ise kullanılır|

## <a name="subtypes-of-datetimev2"></a>DatetimeV2 alt türleri

**DatetimeV2** önceden oluşturulmuş varlık aşağıdaki alt türleri içerir ve aşağıdaki tabloda her birine örnek verilmiştir:
* `date`
* `time`
* `daterange`
* `timerange`
* `datetimerange`
* `duration`
* `set`

## <a name="values-of-resolution"></a>Çözüm değerleri
* Dizideki tarih veya saat tam olarak belirtilmişse ve belirsiz ise dizide bir öğesi vardır.
* DatetimeV2 değeri belirsiz ise dizide iki öğe vardır. Belirsizlik belirli yıl, saat veya zaman aralığı eksikliğinden oluşur. Örnekler için [belirsiz tarihlere](#ambiguous-dates) bakın. Saat A.M. için belirsiz olduğunda ya da P.M., her iki değer de dahildir.
* Söylenişi belirsizliğe sahip iki öğe içeriyorsa dizide dört öğe vardır. Bu belirsizlik, aşağıdaki öğeleri içerir:
  * Yıl olarak belirsiz bir tarih veya tarih aralığı
  * Saat veya saat aralığı, saat olarak belirsiz ya da P.M. Örneğin, 3:00 Nisan 3.

@No__t-0 dizisinin her öğesi aşağıdaki alanlara sahip olabilir: 

|Özellik adı|Özellik açıklaması|
|--|--|
|Timex|saat, tarih veya tarih aralığı, TIMEX3 için [ıso 8601 standardını](https://en.wikipedia.org/wiki/ISO_8601) Izleyen ve timeml dilini kullanarak ek açıklama için ÖZNITELIKLERINI izleyen TIMEX biçiminde ifade edilir. Bu ek açıklama [TIMEX yönergelerine](http://www.timeml.org/tempeval2/tempeval2-trial/guidelines/timex3guidelines-072009.pdf)göre açıklanmıştır.|
|type|Şu öğelerden biri olabilecek alt tür: `datetime`, `date`, `time`, `daterange`, `timerange`, `datetimerange`, `duration`, `set`.|
|değer|**Seçim.** Yyyy: DD: GG (Tarih), ss: DD: SS (Time) yyyy: DD: DD HH: mm: SS (DateTime) biçiminde bir DateTime nesnesi. @No__t-0 `duration` ise, değer saniye sayısıdır (süre) <br/> Yalnızca `type` `datetime` veya `date`, `time` veya ' süre ise kullanılır.|

## <a name="valid-date-values"></a>Geçerli tarih değerleri

**DatetimeV2** aşağıdaki aralıklar arasındaki tarihleri destekler:

| Min | Maks. |
|----------|-------------|
| 1 Ocak 1900   | 31 Aralık 2099 |

## <a name="ambiguous-dates"></a>Belirsiz tarihler

Tarih geçmişte veya gelecekte olabilir, LUYA her iki değer de sağlar. Yıl, yılı olmayan ayı ve tarihi içeren bir örnektir.  

Örneğin, söylenişi "2 Mayıs" olarak verilmiştir:
* Bugünün tarihi 3 Mayıs 2017 ise, Lu, değer olarak hem "2017-05-02" hem de "2018-05-02" sağlar. 
* Bugünün tarihi 1 Mayıs 2017 olduğunda, Lu, değer olarak hem "2016-05-02" hem de "2017-05-02" sağlar.

Aşağıdaki örnek "Mayıs 2" varlığının çözünürlüğünü gösterir. Bu çözüm bugünün tarihinin 1 Mayıs 2017 arasında bir tarih olduğunu varsayar ve 1 Mayıs 2018 olabilir.
@No__t-1 alanında `X` olan alanlar, utterde açıkça belirtilmeyen tarihin parçalarından oluşur.

```json
  "entities": [
    {
      "entity": "may 2nd",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2019-05-02"
          },
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2020-05-02"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-numeric-date"></a>Sayısal Tarih için tarih aralığı çözümleme örnekleri

@No__t-0 varlığı tarih ve saat aralıklarını ayıklar. @No__t-0 ve `end` alanları aralığın başlangıcını ve sonunu belirtir. "2 Mayıs 'ta 1 Mayıs 'ta" deterance için, Lua 'nın her ikisi de geçerli yıl ve sonraki yıl için **terterange** değerleri sağlar. @No__t-0 alanında, `XXXX` değerleri yılın belirsizliğin olduğunu gösterir. `P3D`, zaman döneminin üç gün uzunluğunda olduğunu gösterir.

```json
"entities": [
    {
      "entity": "may 2nd to may 5th",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 17,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2019-05-02",
            "end": "2019-05-05"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-day-of-week"></a>Tarih aralığı çözümleme için haftanın günü örnekleri

Aşağıdaki örnek, LUIN "Salı gününe kadar Salı" deterliği çözümlemek için **datetimeV2** nasıl kullandığını gösterir. Bu örnekte, geçerli tarih 19 Haziran. LUO, ' dan önce gelen tarih aralıklarının her ikisi için de **meterange** değerleri içerir ve geçerli tarihi izler.

```json
  "entities": [
    {
      "entity": "tuesday to thursday",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 19,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2019-04-30",
            "end": "2019-05-02"
          }
        ]
      }
    }
  ]
```
## <a name="ambiguous-time"></a>Belirsiz saat
Değer dizisinde, saat veya zaman aralığı belirsiz ise iki zaman öğesi vardır. Belirsiz bir zaman olduğunda, değerlerin her ikisi de ve P.M. sayısıdır.

## <a name="time-range-resolution-example"></a>Zaman aralığı çözümleme örneği

Aşağıdaki örnek, bir zaman aralığına sahip olan utterliği çözümlemek için LUIN **datetimeV2** 'in nasıl kullandığını gösterir.

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

```json
  "entities": [
    {
      "entity": "6pm to 7pm",
      "type": "builtin.datetimeV2.timerange",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          {
            "timex": "(T18,T19,PT1H)",
            "type": "timerange",
            "start": "18:00:00",
            "end": "19:00:00"
          }
        ]
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)

DatetimeV2 JSON yanıtı API v3 'de değiştirildi. 

API v2 'deki değişiklikler:
* `datetimeV2.timex.type` özelliği artık üst düzeyde döndürüldüğünden, `datetimev2.type`. 
* @No__t-0 özelliği `datetimeV2.value` olarak yeniden adlandırıldı.

DatetimeV2 'in v3 sürümü olan utterance, `8am on may 2nd 2017` ' dır:

```JSON
{
    "query": "8am on may 2nd 2017",
    "prediction": {
        "normalizedQuery": "8am on may 2nd 2017",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.6826963
            }
        },
        "entities": {
            "datetimeV2": [
                {
                    "type": "datetime",
                    "values": [
                        {
                            "timex": "2017-05-02T08",
                            "value": "2017-05-02 08:00:00"
                        }
                    ]
                }
            ]
        }
    }
}
```

Aşağıdaki JSON `verbose` parametresi `false` olarak ayarlanmıştır:

```json
{
    "query": "8am on may 2nd 2017",
    "prediction": {
        "normalizedQuery": "8am on may 2nd 2017",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.6826963
            }
        },
        "entities": {
            "datetimeV2": [
                {
                    "type": "datetime",
                    "values": [
                        {
                            "timex": "2017-05-02T08",
                            "value": "2017-05-02 08:00:00"
                        }
                    ]
                }
            ],
            "$instance": {
                "datetimeV2": [
                    {
                        "type": "builtin.datetimeV2.datetime",
                        "text": "8am on may 2nd 2017",
                        "startIndex": 0,
                        "length": 19,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
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


* * * 

## <a name="deprecated-prebuilt-datetime"></a>Kullanım dışı önceden oluşturulmuş tarih saat

@No__t-0 önceden oluşturulmuş varlık kullanım dışıdır ve **datetimeV2**ile değiştirilmiştir. 

@No__t-0 ' ı LUSıS uygulamanızda `datetimeV2` ile değiştirmek için aşağıdaki adımları izleyin:

1. LUSıS Web arabiriminin **varlıklar** bölmesini açın. 
2. **Tarih saat** önceden oluşturulmuş varlığı silin.
3. **Önceden oluşturulmuş varlık Ekle** ' ye tıklayın
4. **DatetimeV2** öğesini seçin ve **Kaydet**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Boyut](luis-reference-prebuilt-dimension.md), [e-posta](luis-reference-prebuilt-email.md) varlıkları ve [sayı](luis-reference-prebuilt-number.md)hakkında bilgi edinin. 

