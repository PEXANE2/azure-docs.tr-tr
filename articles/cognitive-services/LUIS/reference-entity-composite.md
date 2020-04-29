---
title: Bileşik varlık türü-LUSıS
titleSuffix: Azure Cognitive Services
description: Bileşik bir varlık, önceden oluşturulmuş varlıklar, basit, normal ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar bir bütün varlığı oluşturur.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: a5a1ad467074ee0aa55d14d50ae153ac68304e6f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "71695161"
---
# <a name="composite-entity"></a>Bileşik varlık 

Bileşik bir varlık, önceden oluşturulmuş varlıklar, basit, normal ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar bir bütün varlığı oluşturur. 

**Bu varlık, veriler şu durumlarda iyi bir uyum:**

* Birbirleriyle ilişkilidir. 
* Konuşma bağlamında birbiriyle ilişkilidir.
* Çeşitli varlık türlerini kullanın.
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanıp işlenmelidir.
* Makine öğrenimi gerektiren çeşitli Kullanıcı yelpazimine sahiptir.

![Bileşik varlık](./media/luis-concept-entities/composite-entity.png)

## <a name="example-json"></a>Örnek JSON

Aşağıdaki utterance ile önceden oluşturulmuş `number` ve `Location::ToLocation` bileşik bir varlık göz önünde bulundurun:

`book 2 tickets to cairo`

Bu `2`sayı, ve `cairo`, tolocation 'ın, varlıkların herhangi bir parçası olmayan aralarında sözcükler içerdiğine dikkat edin. [Luo](luis-reference-regions.md) Web sitesinde etiketli bir şekilde kullanılan yeşil alt çizgi, bileşik bir varlığı gösterir.

![Bileşik varlık](./media/luis-concept-data-extraction/composite-entity.png)

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

Bileşik varlıklar bir `compositeEntities` dizide döndürülür ve Birleşik içindeki tüm varlıklar `entities` dizide de döndürülür:

```JSON
  "entities": [
    {
      "entity": "2 tickets to cairo",
      "type": "ticketinfo",
      "startIndex": 5,
      "endIndex": 22,
      "score": 0.9214487
    },
    {
      "entity": "cairo",
      "type": "builtin.geographyV2.city",
      "startIndex": 18,
      "endIndex": 22
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 5,
      "endIndex": 5,
      "resolution": {
        "subtype": "integer",
        "value": "2"
      }
    }
  ],
  "compositeEntities": [
    {
      "parentType": "ticketinfo",
      "value": "2 tickets to cairo",
      "children": [
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "builtin.geographyV2.city",
          "value": "cairo"
        }
      ]
    }
  ]
```    

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)

Bu, sorgu dizesinde ayarlandıysa `verbose=false` JSON 'dir:

```json
"entities": {
    "ticketinfo": [
        {
            "number": [
                2
            ],
            "geographyV2": [
                "cairo"
            ]
        }
    ]
}
```

Bu, sorgu dizesinde ayarlandıysa `verbose=true` JSON 'dir:

```json
"entities": {
    "ticketinfo": [
        {
            "number": [
                2
            ],
            "geographyV2": [
                "cairo"
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "2",
                        "startIndex": 5,
                        "length": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "geographyV2": [
                    {
                        "type": "builtin.geographyV2.city",
                        "text": "cairo",
                        "startIndex": 18,
                        "length": 5,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    ],
    "$instance": {
        "ticketinfo": [
            {
                "type": "ticketinfo",
                "text": "2 tickets to cairo",
                "startIndex": 5,
                "length": 18,
                "score": 0.9214487,
                "modelTypeId": 4,
                "modelType": "Composite Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * * 


|Veri nesnesi|Varlık adı|Değer|
|--|--|--|
|Önceden oluşturulmuş varlık numarası|"yerleşik. numara"|iki|
|Önceden oluşturulmuş varlık-GeographyV2|"Location:: ToLocation"|Cairo|

## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide](luis-tutorial-composite-entity.md), çeşitli türlerin ayıklanan verilerini tek bir kapsayan varlığa dönüştürmek için bir **Bileşik varlık** ekleyin. İstemci uygulaması, verileri paketleyerek ilgili verileri farklı veri türlerinde kolayca ayıklayabilir.
