---
title: Model. herhangi bir varlık türü-LUSıS
titleSuffix: Azure Cognitive Services
description: Desen. Any, varlığın nerede başladığını ve bittiğini işaretlemek için yalnızca bir desen şablonunda kullanılan değişken uzunluklu bir yer tutucudur.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: cda6c724a36a73dc34c2bf8e7158e3e3ec92d46b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563223"
---
# <a name="patternany-entity"></a>Pattern.Any varlık 

Desen. Any, varlığın nerede başladığını ve bittiğini işaretlemek için yalnızca bir desen şablonunda kullanılan değişken uzunluklu bir yer tutucudur.  

Model. tüm varlıkların, amaç Kullanıcı örnekleri değil, [model](luis-how-to-model-intent-pattern.md) şablonu örneklerinde işaretlenmesi gerekir.

**Varlık, şu durumlarda iyi bir uyum:**

* Varlığın sonunda, utterance 'in kalan metniyle karışıyor olabilir. 

## <a name="usage"></a>Kullanım

Başlığa göre kitap arayan bir istemci uygulaması verilirken, model. any tüm başlığı ayıklar. Bu kitap araması `Was {BookTitle} written by an American this year[?]`için desen. any kullanarak bir şablon söylenişi. 

Aşağıdaki tabloda, her satırda utterance 'in iki sürümü vardır. En üstteki söylenişi, luın ilk olarak söylenişi 'yi nasıl göreceğini görecektir. Kitap başlığının başladığı ve bittiği yerde net değildir. En alt, bir desen kullanır. varlığın başlangıcını ve bitişini işaretlemek için herhangi bir varlık. 

|Kalın olan varlık ile utterance|
|--|
|`Was The Man Who Mistook His Wife for a Hat and Other Clinical Tales written by an American this year?`<br><br>Bu yıl **için bir hat ve diğer klinik Tales 'e ait kadın geçen adam** mi?|
|`Was Half Asleep in Frog Pajamas written by an American this year?`<br><br>Bu yıl bir Amerikan tarafından yazılan **Frog Pagr s 'de yarım uyku** mi oldu?|
|`Was The Particular Sadness of Lemon Cake: A Novel written by an American this year?`<br><br>Açık **limon pastanın belirli bir sadyetiydi: Bu yıl bir** Amerikan tarafından yazılmış bir Noy mi?|
|`Was There's A Wocket In My Pocket! written by an American this year?`<br><br>**Cemdeki bir Woketi vardı!** Bu yıl bir Amerikan tarafından yazılmıştır mı?|
||

## <a name="example-json"></a>Örnek JSON

```JSON
{
  "query": "where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.999999464
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.999999464
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 4.883697E-06
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 9.278342E-07
    },
    {
      "intent": "MoveAssetsOrPeople",
      "score": 9.278342E-07
    }
  ],
  "entities": [
    {
      "entity": "understand your responsibilities as a member of the community",
      "type": "FormName",
      "startIndex": 18,
      "endIndex": 78,
      "role": ""
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide](luis-tutorial-pattern-any.md), **bir desen. her türlü** varlığı kullanarak, utülerin iyi biçimlendirildiğinden ve verilerin sonunun, utterlik 'in kalan sözcükleriyle kolayca karıştırılabileceği bir veri çıkartır.