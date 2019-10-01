---
title: Liste varlık türü-LUSıS
titleSuffix: Azure Cognitive Services
description: Liste varlıkları, sabit ve kapalı bir ilgili sözcük kümesini eş anlamlılarıyla birlikte temsil eder. LUSıS, liste varlıkları için ek değerler bulamaz. Geçerli listeye göre yeni kelimelerin önerilerini görmek için öner özelliğini kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 1757faf8ab2be0b62956b6939ee068929f9275a4
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71695249"
---
# <a name="list-entity"></a>Varlık listeleme 

Liste varlıkları, sabit ve kapalı bir ilgili sözcük kümesini eş anlamlılarıyla birlikte temsil eder. LUSıS, liste varlıkları için ek değerler bulamaz. Geçerli listeye göre yeni kelimelerin önerilerini görmek için **öner** özelliğini kullanın. Aynı değere sahip birden fazla liste varlığı varsa, her bir varlık uç nokta sorgusunda döndürülür. 

Bir liste varlığı makine tarafından öğrenilen değildir. Tam metin eşleşmesi. LUO, yanıtta bir varlık olarak herhangi bir listedeki öğeyle eşleşen herhangi bir eşleşmeyi işaretler. 

**Varlık, metin verileri olduğunda iyi bir uyum:**

* Bilinen bir küme.
* Genellikle değişmez. Listeyi sık olarak değiştirmeniz veya listenin kendi kendine genişlemesine ihtiyaç duyuyorsanız, tümcecik listesi ile birlikte bulunan basit bir varlık daha iyi bir seçimdir. 
* Küme, bu varlık türü için en fazla LUO [sınırlarını](luis-boundaries.md) aşamaz.
* Söylenişi 'teki metin, eş anlamlı veya kurallı bir adla tam bir eşleşmedir. LUSıS, tam metin eşleştirmelerinin ötesinde listeyi kullanmaz. Belirsiz eşleştirme, büyük/küçük harfe duyarlı, sözcük kökü oluşturma, plurals ve diğer çeşitlemeler bir liste varlığıyla çözümlenmez. Çeşitlemeleri yönetmek için, isteğe bağlı metin söz dizimine sahip bir [model](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) kullanmayı düşünün.

![Varlık listeleme](./media/luis-concept-entities/list-entity.png)

## <a name="example-json"></a>Örnek JSON

Uygulamanın, Havaalanı (deniz-Tac), Havaalanı Kodu (SEA), posta posta kodu (98101) ve telefon alanı kodu (206) dahil olmak üzere şehir adlarının çeşitlemelerine izin veren `Cities` adlı bir liste olduğunu varsayalım.

|Liste öğesi|Öğe eş anlamlılar|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

Önceki bir deyişle, `paris` sözcüğü `Cities` liste varlığının bir parçası olarak Paris öğesiyle eşlenir. Liste varlığı hem öğenin normalleştirilmiş adının hem de öğe eş anlamlısını eşleştirir.

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 18,
      "endIndex": 22,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)


Sorgu dizesinde `verbose=false` ayarlanmışsa bu JSON olur:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ]
}
```

Sorgu dizesinde `verbose=true` ayarlanmışsa bu JSON olur:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ],
    "$instance": {
        "Cities": [
            {
                "type": "Cities",
                "text": "paris",
                "startIndex": 18,
                "length": 5,
                "modelTypeId": 5,
                "modelType": "List Entity Extractor",
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
|Varlık listeleme|`Cities`|`paris`|


## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide](luis-quickstart-intent-and-list-entity.md), bilinen öğeler listesinden metnin tam eşleşmelerini çıkarmak için bir **liste varlığını** nasıl kullanacağınızı öğrenin. 
