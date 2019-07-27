---
title: Normal ifade varlık türü-LUSıS
titleSuffix: Azure Cognitive Services
description: Normal ifade, ham söylenişi metin için en iyisidir. Küçük büyük harf duyarlı ve kültürel bir değişken yok sayar.  Normal ifadenin eşleştirilmesi, yazım denetimi değişiklikleri karakter düzeyinde belirteci düzeyinde değil sonra uygulanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 82cce359f2161800c53ccce7cdb0342bba759d43
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559942"
---
# <a name="regular-expression-entity"></a>Normal ifade varlığı 

Normal ifade varlığı, sağladığınız normal ifade düzenine göre bir varlığı ayıklar.

Normal ifade, ham söylenişi metin için en iyisidir. Küçük büyük harf duyarlı ve kültürel bir değişken yok sayar.  Normal ifadenin eşleştirilmesi, yazım denetimi değişiklikleri karakter düzeyinde belirteci düzeyinde değil sonra uygulanır. Normal ifade çok karmaşık ise (birçok köşeli ayraç kullanma gibi), bu ifadeyi modele ekleyemezsiniz. [.Net Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) kitaplığının tümünü değil, bölümünü kullanır. 

**Varlık, şu durumlarda iyi bir uyum:**

* Veriler tutarlı olarak tutarlı bir çeşitlerle biçimlendirilir.
* Normal ifadede 2 ' den fazla iç içe geçme düzeyi gerekmez. 

![Normal ifade varlığı](./media/luis-concept-entities/regex-entity.png)

## <a name="usage-considerations"></a>Kullanım konuları

Normal ifadeler, eşleştirmeye beklediğinizden daha fazla eşleşemez. Buna bir örnek, `one` ve `two`gibi sayısal kelime eşleştirmedir. Aşağıdaki Regex, diğer sayılarla birlikte sayıyla `one` eşleşen aşağıdaki normal ifade örneğidir:

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
``` 

Bu Regex ifadesi, `phone`gibi bu sayılarla biten sözcüklerle da eşleşir. Bu gibi sorunları gidermek için, Regex eşleşmelerin, hesap sözcük sınırlarına uyduğundan emin olun. Bu örnek için sözcük sınırları kullanmanın Regex ifadesi aşağıdaki Regex içinde kullanılır:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

### <a name="example-json"></a>Örnek JSON

Kullanırken `kb[0-9]{6}`, normal ifade varlık tanımı olarak aşağıdaki JSON yanıtı, sorgu `When was kb123456 published?`için döndürülen normal ifade varlıklarını içeren bir örnektir:

```JSON
{
  "query": "when was kb123456 published?",
  "topScoringIntent": {
    "intent": "FindKBArticle",
    "score": 0.933641255
  },
  "intents": [
    {
      "intent": "FindKBArticle",
      "score": 0.933641255
    },
    {
      "intent": "None",
      "score": 0.04397359
    }
  ],
  "entities": [
    {
      "entity": "kb123456",
      "type": "KB number",
      "startIndex": 9,
      "endIndex": 16
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide](luis-quickstart-intents-regex-entity.md), **normal ifade** varlığını kullanarak, düzenli olarak biçimlendirilen verileri bir noktadan ayıklamak için bir uygulama oluşturun.