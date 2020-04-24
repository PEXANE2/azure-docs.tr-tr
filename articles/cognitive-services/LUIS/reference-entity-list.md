---
title: Liste varlık türü-LUSıS
description: Liste varlıkları, sabit ve kapalı bir ilgili sözcük kümesini eş anlamlılarıyla birlikte temsil eder. LUSıS, liste varlıkları için ek değerler bulamaz. Geçerli listeye göre yeni kelimelerin önerilerini görmek için öner özelliğini kullanın.
ms.topic: reference
ms.date: 03/12/2020
ms.openlocfilehash: 273fabae38f6682cfaaffcdcc19e62adc41b7a47
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82097583"
---
# <a name="list-entity"></a>Liste varlığı

Liste varlıkları, sabit ve kapalı bir ilgili sözcük kümesini eş anlamlılarıyla birlikte temsil eder. LUSıS, liste varlıkları için ek değerler bulamaz. Geçerli listeye göre yeni kelimelerin önerilerini görmek için **öner** özelliğini kullanın. Aynı değere sahip birden fazla liste varlığı varsa, her bir varlık uç nokta sorgusunda döndürülür.

Bir liste varlığı makine tarafından öğrenilen değildir. Tam metin eşleşmesi. LUO, yanıtta bir varlık olarak herhangi bir listedeki öğeyle eşleşen herhangi bir eşleşmeyi işaretler.

**Varlık, metin verileri olduğunda iyi bir uyum:**

* Bilinen bir küme.
* Genellikle değişmez. Listeyi sık olarak değiştirmeniz veya listenin kendi kendine genişlemesine ihtiyaç duyuyorsanız, tümcecik listesi ile birlikte bulunan basit bir varlık daha iyi bir seçimdir.
* Küme, bu varlık türü için maksimum LUIS [sınırlarını](luis-limits.md) aşmaz.
* Söylenişi 'teki metin, eş anlamlı veya kurallı bir ada sahip büyük/küçük harf duyarsız bir eşleşmedir. LUSıS, listeyi eşleşmenin ötesinde kullanmaz. Belirsiz eşleme, sözcük kökü oluşturma, plurals ve diğer çeşitlemeler bir liste varlığıyla çözümlenmez. Çeşitlemeleri yönetmek için, isteğe bağlı metin söz dizimine sahip bir [model](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance) kullanmayı düşünün.

![varlık listeleme](./media/luis-concept-entities/list-entity.png)

## <a name="example-json-to-import-into-list-entity"></a>Liste varlığına içeri aktarılacak örnek. JSON

  Aşağıdaki. JSON biçimini kullanarak, mevcut bir liste varlığına değerleri içeri aktarabilirsiniz:

  ```JSON
  [
      {
          "canonicalForm": "Blue",
          "list": [
              "navy",
              "royal",
              "baby"
          ]
      },
      {
          "canonicalForm": "Green",
          "list": [
              "kelly",
              "forest",
              "avacado"
          ]
      }
  ]
  ```

## <a name="example-json-response"></a>Örnek JSON yanıtı

Uygulamanın Havaalanı (deniz-Tac `Cities`), Havaalanı Kodu (SEA), posta posta kodu (98101) ve telefon alanı kodu (206) dahil olmak üzere şehir adlarının çeşitlemelerine izin veren, adında bir liste olduğunu varsayalım.

|Liste öğesi|Öğe eş anlamlılar|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

Önceki bir deyişle, sözcük `paris` , `Cities` liste varlığının bir parçası olarak Paris öğesiyle eşlenir. Liste varlığı hem öğenin normalleştirilmiş adının hem de öğe eş anlamlısını eşleştirir.

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

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

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)


Bu, sorgu dizesinde ayarlandıysa `verbose=false` JSON 'dir:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ]
}
```

Bu, sorgu dizesinde ayarlandıysa `verbose=true` JSON 'dir:

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

Bu [öğreticide](tutorial-list-entity.md), bilinen öğeler listesinden metnin tam eşleşmelerini çıkarmak için bir **liste varlığını** nasıl kullanacağınızı öğrenin.
