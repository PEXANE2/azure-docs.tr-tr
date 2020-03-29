---
title: Desen.herhangi bir varlık türü - LUIS
titleSuffix: Azure Cognitive Services
description: Desen.any, varlığın nerede başlayıp nerede bittiğini işaretlemek için yalnızca bir desenin şablon uyrmasında kullanılan değişken uzunlukta bir yer tutucudur.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 5164bf55ef8233cf34a470524da3bc852678d79a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75979173"
---
# <a name="patternany-entity"></a>Pattern.any varlığı

Desen.any, varlığın nerede başlayıp nerede bittiğini işaretlemek için yalnızca bir desenin şablon uyrmasında kullanılan değişken uzunlukta bir yer tutucudur.  

Desen.herhangi bir varlıkların amaç kullanıcı örneklerinde değil, [Desen](luis-how-to-model-intent-pattern.md) şablonu örneklerinde işaretlenmesi gerekir.

**Varlık iyi bir uyum zaman:**

* Varlığın sonu, söyleyinin kalan metniyle karıştırılabilir.

## <a name="usage"></a>Kullanım

Başlığa göre kitap arayan bir istemci uygulaması göz önüne alındığında, desen.herhangi bir başlık tam ayıklar. Desen kullanarak bir şablon söyleyiş.bu `Was {BookTitle} written by an American this year[?]`kitap arama için herhangi bir .

Aşağıdaki tabloda, her satırın söyleyinin iki sürümü vardır. En üstteki söyleyiş, LUIS'in başlangıçta söyleyiyi nasıl gördüğüdür. Kitabın başlığının nerede başlayıp nerede bittiği belli değil. Alttaki söyleyiş, varlığın başlangıcını ve sonunu işaretlemek için bir Desen kullanır.

|Kalın varlık ile Söyleyiş|
|--|
|`Was The Man Who Mistook His Wife for a Hat and Other Clinical Tales written by an American this year?`<br><br>**Karısını Bir Şapka ve Diğer Klinik Hikayelerle Yanlış Anons Eden Adam** Bu yıl bir Amerikalı tarafından mı yazıldı?|
|`Was Half Asleep in Frog Pajamas written by an American this year?`<br><br>**Kurbağa Pijamayarı Uykuda Bu** yıl bir Amerikalı tarafından mı yazıldı?|
|`Was The Particular Sadness of Lemon Cake: A Novel written by an American this year?`<br><br>**Limonlu Kekin Özel Hüznü:** Bir Amerikalı nın bu yıl yazdığı Bir Roman mı?|
|`Was There's A Wocket In My Pocket! written by an American this year?`<br><br>**Cebimde Bir Wocket var mıydı?** Bu yıl bir Amerikalı tarafından yazılmış?|
||



## <a name="example-json"></a>Örnek JSON

Şu sorguyu inceleyin:

`where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

Bir Desen olarak ayıklamak için gömülü form adı ile.any:

`Understand your responsibilities as a member of the community`

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
"entities": [
  {
    "entity": "understand your responsibilities as a member of the community",
    "type": "FormName",
    "startIndex": 18,
    "endIndex": 78,
    "role": ""
  }
```


#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

Bu JSON sorgu `verbose=false` dizesinde ayarlanırsa:

```json
"entities": {
    "FormName": [
        "Understand your responsibilities as a member of the community"
    ]
}
```

Bu JSON sorgu `verbose=true` dizesinde ayarlanırsa:

```json
"entities": {
    "FormName": [
        "Understand your responsibilities as a member of the community"
    ],
    "$instance": {
        "FormName": [
            {
                "type": "FormName",
                "text": "Understand your responsibilities as a member of the community",
                "startIndex": 18,
                "length": 61,
                "modelTypeId": 7,
                "modelType": "Pattern.Any Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide,](luis-tutorial-pattern.md) **deseni kullanın.herhangi bir** varlık, sözlerin iyi biçimlendirilmiş olduğu ve verilerin sonunun sözün kalan sözcükleri ile kolayca karıştırılabileceği sözlerden veri ayıklamak için kullanılır.
