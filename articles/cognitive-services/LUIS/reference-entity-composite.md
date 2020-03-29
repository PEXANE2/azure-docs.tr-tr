---
title: Bileşik varlık türü - LUIS
titleSuffix: Azure Cognitive Services
description: Bileşik varlık, önceden oluşturulmuş varlıklar, basit, düzenli ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar bütün bir varlık oluşturur.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: a5a1ad467074ee0aa55d14d50ae153ac68304e6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71695161"
---
# <a name="composite-entity"></a>Bileşik varlık 

Bileşik varlık, önceden oluşturulmuş varlıklar, basit, düzenli ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar bütün bir varlık oluşturur. 

**Bu varlık iyi bir uyum zaman veri:**

* Birbirleri ile akrabalar. 
* Konuşma bağlamında birbiriyle ilişkilidir.
* Çeşitli varlık türleri kullanın.
* Bir bilgi birimi olarak istemci uygulaması tarafından gruplandırılmalı ve işlenmesi gerekir.
* Makine öğrenimi gerektiren çeşitli kullanıcı söyleyişlerine sahip olmak.

![kompozit varlık](./media/luis-concept-entities/composite-entity.png)

## <a name="example-json"></a>Örnek JSON

Önceden oluşturulmuş `number` ve `Location::ToLocation` aşağıdaki söyleyerek kompozit bir varlık düşünün:

`book 2 tickets to cairo`

`2`Not, numara ve `cairo`ToLocation'ın aralarında varlıkların hiçbirinin parçası olmayan sözcükler bulunduğuna dikkat edin. [LUIS](luis-reference-regions.md) web sitesinde etiketli bir sözcükte kullanılan yeşil altı çizili, bileşik bir varlığı gösterir.

![Kompozit Varlık](./media/luis-concept-data-extraction/composite-entity.png)

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

Bileşik varlıklar bir `compositeEntities` dizide döndürülür ve bileşik içindeki tüm `entities` varlıklar da dizide döndürülür:

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

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

Bu JSON sorgu `verbose=false` dizesinde ayarlanırsa:

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

Bu JSON sorgu `verbose=true` dizesinde ayarlanırsa:

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
|Önceden Oluşturulmuş Varlık - sayı|"builtin.number"|"2"|
|Önceden Oluşturulmuş Varlık - GeographyV2|"Konum::ToLocation"|"kahire"|

## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide,](luis-tutorial-composite-entity.md)çıkarılan çeşitli türleri tek bir varlık içine demetlemek için bileşik bir **varlık** ekleyin. İstemci uygulaması, verileri biraraya getirebilir ve ilgili verileri farklı veri türlerinde kolayca ayıklayabilir.
