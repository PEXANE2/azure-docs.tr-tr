---
title: Varlık türünü listele - LUIS
description: Liste varlıkları, eşanlamlılarıyla birlikte sabit, kapalı bir ilişkili sözcük kümesini temsil ederler. LUIS liste varlıkları için ek değerler keşfetmez. Geçerli listeye dayalı yeni sözcükler için öneriler görmek için Öner özelliğini kullanın.
ms.topic: reference
ms.date: 03/12/2020
ms.openlocfilehash: 795d16bc2e0c4223ff3ac283a72493923d3ab355
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297246"
---
# <a name="list-entity"></a>Liste varlığı

Liste varlıkları, eşanlamlılarıyla birlikte sabit, kapalı bir ilişkili sözcük kümesini temsil ederler. LUIS liste varlıkları için ek değerler keşfetmez. Geçerli listeye dayalı yeni sözcükler için öneriler görmek için **Öner** özelliğini kullanın. Aynı değere sahip birden fazla liste varlığı varsa, her varlık bitiş noktası sorgusunda döndürülür.

Bir liste varlığı makinede öğrenilen değildir. Tam bir metin eşleşmesi. LUIS, yanıttaki varlık olarak herhangi bir listedeki bir öğeyle eşleşen herhangi bir eşleşmeyi işaretler.

**Varlık, metin verileri aşağıdakinde iyi bir uyum sağlar:**

* Bilinen bir kümevardır.
* Sık sık değişmez. Listeyi sık sık değiştirmeniz gerekiyorsa veya listenin kendi kendine genişletilmesini istiyorsanız, bir ifade listesiyle öne itilen basit bir varlık daha iyi bir seçimdir.
* Küme, bu varlık türü için maksimum LUIS [sınırlarını](luis-boundaries.md) aşmaz.
* Söyleyişdeki metin, eşanlamlı veya kanonik adla yapılan büyük/küçük harf duyarsız bir eşleşmedir. LUIS listeyi maçın ötesinde kullanmıyor. Bulanık eşleştirme, saptırma, çoğul ve diğer varyasyonlar bir liste varlığı ile çözülmez. Varyasyonları yönetmek için isteğe bağlı metin sözdizimi içeren bir [desen](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance) kullanmayı düşünün.

![liste varlığı](./media/luis-concept-entities/list-entity.png)

## <a name="example-json-to-import-into-list-entity"></a>Örnek .json liste varlığına içe aktarMak için

  Değerleri aşağıdaki .json biçimini kullanarak varolan bir liste varlığına aktarabilirsiniz:

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

Uygulamanın havaalanı şehri (Sea-tac), havaalanı kodu (SEA), posta posta kodu (98101) ve telefon alanı kodu (206) dahil olmak üzere şehir adlarının varyasyonlarına izin veren bir listesi `Cities`olduğunu varsayalım.

|Liste öğesi|Öğe eşanlamlısı|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

Önceki söyleyiş, kelime `paris` `Cities` liste varlığının bir parçası olarak paris öğesine eşlenir. Liste varlığı hem öğenin normalleştirilmiş adı hem de öğe eşanlamlıları eşleşir.

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

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

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)


Bu JSON sorgu `verbose=false` dizesinde ayarlanırsa:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ]
}
```

Bu JSON sorgu `verbose=true` dizesinde ayarlanırsa:

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
|Varlık Listele|`Cities`|`paris`|


## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide,](tutorial-list-entity.md)bilinen öğeler listesinden metnin tam eşlerini ayıklamak için liste **varlığınınasıl** kullanacağınızı öğrenin.
