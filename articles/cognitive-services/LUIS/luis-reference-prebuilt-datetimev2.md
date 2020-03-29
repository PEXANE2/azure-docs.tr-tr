---
title: DatetimeV2 Önceden oluşturulmuş varlıklar - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makalede, Dil Anlama (LUIS) datetimeV2 önceden oluşturulmuş varlık bilgileri vardır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 01/07/2020
ms.author: diberry
ms.openlocfilehash: 30132983f37323e798efd330f5cc8f15c0a9d2b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270730"
---
# <a name="datetimev2-prebuilt-entity-for-a-luis-app"></a>BIR LUIS uygulaması için DatetimeV2 önceden oluşturulmuş varlık

**DatetimeV2** önceden oluşturulmuş varlık tarih ve saat değerlerini ayıklar. Bu değerler, istemci programlarının tüketilmesi için standartlaştırılmış bir biçimde çözülür. Bir söyleyşinin tamamlanmamış bir tarih veya saati olduğunda, LUIS bitiş noktası yanıtında _hem geçmiş hem de gelecekteki değerleri_ içerir. Bu varlık zaten eğitilmiş olduğundan, uygulama amaçlarına datetimeV2 içeren örnek beyanlar eklemeniz gerekmez.

## <a name="types-of-datetimev2"></a>DatetimeV2 türleri
DatetimeV2, [Tanıyanlar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-DateTime.yaml) GitHub deposundan yönetilir.

## <a name="example-json"></a>Örnek JSON

Aşağıdaki söyleyiş ve kısmi JSON yanıtı aşağıda gösterilmiştir.

`8am on may 2nd 2019`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/1-1)

```json
"entities": {
    "datetimeV2": [
        {
            "type": "datetime",
            "values": [
                {
                    "timex": "2019-05-02T08",
                    "resolution": [
                        {
                            "value": "2019-05-02 08:00:00"
                        }
                    ]
                }
            ]
        }
    ]
}
```

#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/1-2)

```json

"entities": {
    "datetimeV2": [
        {
            "type": "datetime",
            "values": [
                {
                    "timex": "2019-05-02T08",
                    "resolution": [
                        {
                            "value": "2019-05-02 08:00:00"
                        }
                    ]
                }
            ]
        }
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.datetime",
                "text": "8am on may 2nd 2019",
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
```

#### <a name="v2-response"></a>[V2 yanıtı](#tab/1-3)

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

|Özellik adı |Özellik türü ve açıklaması|
|---|---|
|Varlık|**string** - Tarih, saat, tarih aralığı veya zaman aralığı türü ile söyleyişten çıkarılan metin.|
|type|**string** - [DatetimeV2 alt türlerinden](#subtypes-of-datetimev2) biri
|Startındex|**int** - Varlığın başladığı söyleyiş deki dizin.|
|endIndex|**int** - Varlığın sona erdiği söyleyişdeki dizin.|
|çözüm|Bir, `values` iki veya dört çözünürlük değeri olan bir dizi [vardır.](#values-of-resolution)|
|end|Bir saatin veya tarih aralığının bitiş `value`değeri, aynı biçimde . `type` Yalnızca `daterange`, `timerange`veya`datetimerange`|

* * *

## <a name="subtypes-of-datetimev2"></a>DatetimeV2 alt türleri

**DatetimeV2** önceden oluşturulmuş varlık aşağıdaki alt türleri vardır ve her biri örnekleri aşağıdaki tabloda sağlanır:
* `date`
* `time`
* `daterange`
* `timerange`
* `datetimerange`


## <a name="values-of-resolution"></a>Çözünürlük değerleri
* Söyleyişdeki tarih veya saat tam olarak belirtilmiş ve kesinse dizinin bir öğesi vardır.
* DatetimeV2 değeri belirsizse dizi iki öğeye sahiptir. Belirsizlik belirli bir yıl, zaman veya zaman aralığı eksikliği içerir. Örnekler için [Belirsiz tarihlere](#ambiguous-dates) bakın. A.M. için zaman belirsiz olduğunda. veya P.M., her iki değer dahildir.
* Söyleyiş belirsizliği ile iki öğe varsa dizi dört öğevardır. Bu belirsizlik, sahip olan öğeleri içerir:
  * Yıla göre belirsiz bir tarih veya tarih aralığı
  * A.M. olarak belirsiz bir zaman veya zaman aralığı. görüntülenir. Örneğin, 3 Nisan 03:00.

`values` Dizinin her öğesi aşağıdaki alanlara sahip olabilir:

|Özellik adı|Özellik açıklaması|
|--|--|
|Timex|[ISO 8601 standardını](https://en.wikipedia.org/wiki/ISO_8601) ve TimeML dilini kullanarak ek açıklama için TIMEX3 özniteliklerini izleyen TIMEX biçiminde ifade edilen saat, tarih veya tarih aralığı. Bu ek açıklama [TIMEX yönergelerinde](http://www.timeml.org/tempeval2/tempeval2-trial/guidelines/timex3guidelines-072009.pdf)açıklanmıştır.|
|mod|gibi değerin nasıl kullanılacağını açıklamak `before` `after`için kullanılan terim .|
|type|Aşağıdaki öğelerden biri olabilir alt türü: `datetime` `date`, `time` `daterange`, `timerange` `datetimerange`, `duration` `set`, , , .|
|value|**Isteğe bağlı.** Yyyy-MM-dd (tarih), HH:mm:ss (zaman) yyyy-MM-dd HH:mm:ss (datetime) biçiminde bir tarih nesnesi. `duration`Ise, `type` değer saniye sayısıdır (süre) <br/> Yalnızca , `type` `datetime` `date`, `time`veya 'süresi ise kullanılır.|

## <a name="valid-date-values"></a>Geçerli tarih değerleri

**DatetimeV2** aşağıdaki aralıklar arasındaki tarihleri destekler:

| Min | Maks |
|----------|-------------|
| 1 Ocak 1900   | 31 Aralık 2099 |

## <a name="ambiguous-dates"></a>Belirsiz tarihler

Tarih geçmişte veya gelecekte olabilirse, LUIS her iki değeri de sağlar. Bir örnek yıl olmadan ay ve tarih içeren bir söyleyiş.

Örneğin, aşağıdaki söyleyerek göz önüne alındığında:

`May 2nd`

* Bugünün tarihi 3 Mayıs 2017 ise, LUIS hem "2017-05-02" hem de "2018-05-02" değerlerini sağlar.
* Bugünün tarihi 1 Mayıs 2017 olduğunda, LUIS hem "2016-05-02" hem de "2017-05-02" değerlerini sağlar.

Aşağıdaki örnek, varlığın "2 Mayıs" çözümünü gösterir. Bu karar, bugünün tarihinin 2 Mayıs 2017 ile 1 Mayıs 2018 tarihleri arasında olduğunu varsayıyor.
`X` Alandaki alanlar, tarihin söyleyişte açıkça belirtilmeyen bölümleridir. `timex`

## <a name="date-resolution-example"></a>Tarih çözümlemesi örneği


Aşağıdaki söyleyiş ve kısmi JSON yanıtı aşağıda gösterilmiştir.

`May 2nd`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/2-1)

```json
"entities": {
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "XXXX-05-02",
                    "resolution": [
                        {
                            "value": "2019-05-02"
                        },
                        {
                            "value": "2020-05-02"
                        }
                    ]
                }
            ]
        }
    ]
}
```

#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/2-2)

```json
"entities": {
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "XXXX-05-02",
                    "resolution": [
                        {
                            "value": "2019-05-02"
                        },
                        {
                            "value": "2020-05-02"
                        }
                    ]
                }
            ]
        }
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.date",
                "text": "May 2nd",
                "startIndex": 0,
                "length": 7,
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

#### <a name="v2-response"></a>[V2 yanıtı](#tab/2-3)

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
* * *

## <a name="date-range-resolution-examples-for-numeric-date"></a>Sayısal tarih için tarih aralığı çözümörnekleri

Varlık `datetimeV2` tarih ve saat aralıklarını ayıklar. Ve `start` `end` alanlar aralığın başlangıcını ve sonunu belirtir. Söyleyiş `May 2nd to May 5th`için, LUIS hem geçerli yıl hem de gelecek yıl için **daterange** değerleri sağlar. Alandaki `timex` `XXXX` değerler yılın belirsizliğini gösterir. `P3D`sürenin üç gün sürdüğünü gösterir.

Aşağıdaki söyleyiş ve kısmi JSON yanıtı aşağıda gösterilmiştir.

`May 2nd to May 5th`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/3-1)

```json

"entities": {
    "datetimeV2": [
        {
            "type": "daterange",
            "values": [
                {
                    "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
                    "resolution": [
                        {
                            "start": "2019-05-02",
                            "end": "2019-05-05"
                        },
                        {
                            "start": "2020-05-02",
                            "end": "2020-05-05"
                        }
                    ]
                }
            ]
        }
    ]
}
```


#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/3-2)

```json

"entities": {
    "datetimeV2": [
        {
            "type": "daterange",
            "values": [
                {
                    "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
                    "resolution": [
                        {
                            "start": "2019-05-02",
                            "end": "2019-05-05"
                        },
                        {
                            "start": "2020-05-02",
                            "end": "2020-05-05"
                        }
                    ]
                }
            ]
        }
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.daterange",
                "text": "May 2nd to May 5th",
                "startIndex": 0,
                "length": 18,
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

#### <a name="v2-response"></a>[V2 yanıtı](#tab/3-3)

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
* * *

## <a name="date-range-resolution-examples-for-day-of-week"></a>Haftanın günü için tarih aralığı çözüm örnekleri

Aşağıdaki örnek, LUIS'in söyleyiyi `Tuesday to Thursday`çözmek için **datetimeV2'yi** nasıl kullandığını gösterir. Bu örnekte, geçerli tarih 19 Haziran'dır. LUIS, geçerli tarihten önce gelen ve izleyen tarih aralıklarının her ikisi için **tarih aralığı** değerlerini içerir.

Aşağıdaki söyleyiş ve kısmi JSON yanıtı aşağıda gösterilmiştir.

`Tuesday to Thursday`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/4-1)

```json
"entities": {
    "datetimeV2": [
        {
            "type": "daterange",
            "values": [
                {
                    "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
                    "resolution": [
                        {
                            "start": "2019-10-08",
                            "end": "2019-10-10"
                        },
                        {
                            "start": "2019-10-15",
                            "end": "2019-10-17"
                        }
                    ]
                }
            ]
        }
    ]
}
```

#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/4-2)

```json
"entities": {
    "datetimeV2": [
        {
            "type": "daterange",
            "values": [
                {
                    "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
                    "resolution": [
                        {
                            "start": "2019-10-08",
                            "end": "2019-10-10"
                        },
                        {
                            "start": "2019-10-15",
                            "end": "2019-10-17"
                        }
                    ]
                }
            ]
        }
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.daterange",
                "text": "Tuesday to Thursday",
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
```

#### <a name="v2-response"></a>[V2 yanıtı](#tab/4-3)

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
* * *

## <a name="ambiguous-time"></a>Belirsiz zaman
Zaman veya zaman aralığı belirsizse, değerler dizisinin iki zaman öğesi vardır. Belirsiz bir zaman olduğunda, değerlerin her ikisi de A.M. vardır. ve p.m. Kez.

## <a name="time-range-resolution-example"></a>Zaman aralığı çözümlü örnek

DatetimeV2 JSON yanıtı API V3 değişti. Aşağıdaki örnek, LUIS'in zaman aralığı olan söyleyiyi çözmek için **datetimeV2'yi** nasıl kullandığını gösterir.

API V2 değişiklikler:
* `datetimeV2.timex.type`özelliği artık döndürülmez, çünkü üst düzeyde `datetimev2.type`döndürülür, .
* Özellik' `datetimeV2.value` in adı `datetimeV2.timex`.

Aşağıdaki söyleyiş ve kısmi JSON yanıtı aşağıda gösterilmiştir.

`from 6pm to 7pm`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/5-1)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```JSON

"entities": {
    "datetimeV2": [
        {
            "type": "timerange",
            "values": [
                {
                    "timex": "(T18,T19,PT1H)",
                    "resolution": [
                        {
                            "start": "18:00:00",
                            "end": "19:00:00"
                        }
                    ]
                }
            ]
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/5-2)

Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json

"entities": {
    "datetimeV2": [
        {
            "type": "timerange",
            "values": [
                {
                    "timex": "(T18,T19,PT1H)",
                    "resolution": [
                        {
                            "start": "18:00:00",
                            "end": "19:00:00"
                        }
                    ]
                }
            ]
        }
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.timerange",
                "text": "from 6pm to 7pm",
                "startIndex": 0,
                "length": 15,
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
#### <a name="v2-response"></a>[V2 yanıtı](#tab/5-3)

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

* * *

## <a name="time-resolution-example"></a>Zaman çözümlemesi örneği

Aşağıdaki söyleyiş ve kısmi JSON yanıtı aşağıda gösterilmiştir.

`8am`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/6-1)

```json
"entities": {
    "datetimeV2": [
        {
            "type": "time",
            "values": [
                {
                    "timex": "T08",
                    "resolution": [
                        {
                            "value": "08:00:00"
                        }
                    ]
                }
            ]
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/6-2)

```json
"entities": {
    "datetimeV2": [
        {
            "type": "time",
            "values": [
                {
                    "timex": "T08",
                    "resolution": [
                        {
                            "value": "08:00:00"
                        }
                    ]
                }
            ]
        }
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.time",
                "text": "8am",
                "startIndex": 0,
                "length": 3,
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
#### <a name="v2-response"></a>[V2 yanıtı](#tab/6-3)

```json
"entities": [
  {
    "entity": "8am",
    "type": "builtin.datetimeV2.time",
    "startIndex": 0,
    "endIndex": 2,
    "resolution": {
      "values": [
        {
          "timex": "T08",
          "type": "time",
          "value": "08:00:00"
        }
      ]
    }
  }
]
```

* * *

## <a name="deprecated-prebuilt-datetime"></a>Amortismana kılmış önceden oluşturulmuş tarih zamanı

`datetime` Önceden oluşturulmuş varlık amortismana ve **datetimeV2**ile değiştirilir.

LUIS `datetime` uygulamanızda değiştirmek `datetimeV2` için aşağıdaki adımları tamamlayın:

1. LUIS web arabiriminin **Varlıklar** bölmesini açın.
2. Tarih **saati** önceden oluşturulmuş varlığı silin.
3. **Önceden oluşturulmuş varlık ekle'yi** tıklatın
4. **DatetimeV2'yi** seçin ve **Kaydet'i**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Boyut,](luis-reference-prebuilt-dimension.md) [e-posta](luis-reference-prebuilt-email.md) varlıkları ve [sayı](luis-reference-prebuilt-number.md)hakkında bilgi edinin.

