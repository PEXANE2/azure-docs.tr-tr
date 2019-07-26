---
title: Bileşik varlık türü
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bileşik bir varlık, önceden oluşturulmuş varlıklar, basit, normal ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar, tam bir varlık oluşturur.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 55dcc5666e63b8a87ddcb13696991fe02843fbbd
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480159"
---
# <a name="composite-entity"></a>Bileşik varlık 

Bileşik bir varlık, önceden oluşturulmuş varlıklar, basit, normal ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar, tam bir varlık oluşturur. 

**Bu varlık, veriler şu durumlarda iyi bir uyum:**

* Birbiriyle ilgilidir. 
* Konuşma bağlamında birbiriyle ilişkilidir.
* Varlık türleri çeşitli kullanın.
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanıp işlenmelidir.
* Makine öğrenimi gerektiren çeşitli Kullanıcı yelpazimine sahiptir.

![Bileşik varlık](./media/luis-concept-entities/composite-entity.png)

## <a name="example-json"></a>Örnek JSON

Aşağıdaki utterance ile önceden oluşturulmuş `number` ve `Location::ToLocation` bileşik bir varlık göz önünde bulundurun:

`book 2 tickets to paris`

Dikkat `2`, sayı ve `paris`, ToLocation varlıkları parçası olmayan aralarında sözcükler vardır. Etiketli bir utterance içinde kullanılan yeşil bir çizgi [LUIS](luis-reference-regions.md) Web sitesi, bileşik bir varlık gösterir.

![Bileşik varlık](./media/luis-concept-data-extraction/composite-entity.png)

Bileşik varlıkları döndürülür bir `compositeEntities` dizisi ve bileşik tüm varlıklarda da döndürülür içinde `entities` dizisi:

```JSON

"entities": [
    {
    "entity": "2 tickets to cairo",
    "type": "ticketInfo",
    "startIndex": 0,
    "endIndex": 17,
    "score": 0.67200166
    },
    {
    "entity": "2",
    "type": "builtin.number",
    "startIndex": 0,
    "endIndex": 0,
    "resolution": {
        "subtype": "integer",
        "value": "2"
    }
    },
    {
    "entity": "cairo",
    "type": "builtin.geographyV2",
    "startIndex": 13,
    "endIndex": 17
    }
],
"compositeEntities": [
    {
    "parentType": "ticketInfo",
    "value": "2 tickets to cairo",
    "children": [
        {
        "type": "builtin.geographyV2",
        "value": "cairo"
        },
        {
        "type": "builtin.number",
        "value": "2"
        }
    ]
    }
]
```    

|Veri nesnesi|Varlık adı|Değer|
|--|--|--|
|Önceden oluşturulmuş varlık - sayı|"builtin.number"|"2"|
|Önceden oluşturulmuş varlık-GeographyV2|"Location::ToLocation"|"paris"|

## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide](luis-tutorial-composite-entity.md), çeşitli türlerin ayıklanan verilerini tek bir kapsayan varlığa dönüştürmek için bir **Bileşik varlık** ekleyin. İstemci uygulama, verileri paketleme tarafından farklı veri türlerinde ilgili verileri kolayca ayıklayabilirsiniz.
