---
title: Tümcecik listeleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: '`PhraseListGrammar`Konuşmayı metne dönüştürme sonuçlarını geliştirmek için nesnesini kullanarak bir tümcecik listesi Ile konuşma hizmetini nasıl sağlayacağınızı öğrenin.'
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: trbye
zone_pivot_groups: programming-languages-speech-services-one-nomore-no-go
ms.custom: devx-track-javascript, devx-track-csharp
ms.openlocfilehash: a71b0ba459d9363f6f479fd88ef10c8209eaf47c
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88918732"
---
# <a name="phrase-lists-for-speech-to-text"></a>Konuşmayı metne yönelik tümcecik listeleri

Konuşma hizmetini bir tümcecik listesi ile birlikte sunarak, konuşma tanımanın doğruluğunu artırabilirsiniz. Tümcecik listeleri, bir kişinin adı veya belirli bir konum gibi, ses verilerinde bilinen tümcecikleri belirlemek için kullanılır.

Örnek olarak, "taşı" komutuna ve söylenen "Ward" bir hedefe sahipseniz, "Ward 'e taşı" girişini ekleyebilirsiniz. Bir tümcecik eklemek, ses "ilerlemek için taşı" yerine "ilerlemek" yerine tanınabilmesi olasılığını artırır.

Tek sözcükler veya bütün ifadeler, bir tümcecik listesine eklenebilir. Tanıma sırasında, tüm tümcecik için tam eşleşme, sese ayrı bir tümcecik olarak dahil ediliyorsa tümcecik listesindeki bir giriş kullanılır. Tümcecikle tam eşleşme bulunamazsa, tanıma yardımlı değildir.

>[!Note]
> Şu anda, tümcecik listeleri yalnızca konuşmayı metin için destekler.

## <a name="how-to-use-phrase-lists"></a>Tümcecik listelerini kullanma

Aşağıdaki örneklerde, nesnesi kullanılarak bir tümcecik listesinin nasıl oluşturulacağı gösterilmektedir `PhraseListGrammar` .

::: zone pivot="programming-language-csharp"

```cs
PhraseListGrammar phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Move to Ward");
phraseList.AddPhrase("Move to Bill");
phraseList.AddPhrase("Move to Ted");
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
auto phraselist = PhraseListGrammar::FromRecognizer(recognizer);
phraselist->AddPhrase("Move to Ward");
phraselist->AddPhrase("Move to Bill");
phraselist->AddPhrase("Move to Ted");
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
PhraseListGrammar phraseListGrammar = PhraseListGrammar.fromRecognizer(recognizer);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Move to Ward")
phrase_list_grammar.addPhrase("Move to Bill")
phrase_list_grammar.addPhrase("Move to Ted")
```

::: zone-end

::: zone pivot="programming-language-javascript"

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

::: zone-end

>[!Note]
> Konuşma hizmetinin konuşmayı eşleştirmek için kullanacağı en fazla tümcecik listesi sayısı 1024 tümceciklerdir.

Ayrıca, Clear () öğesini çağırarak ile ilişkili tümcecikleri temizleyebilirsiniz `PhraseListGrammar` .

::: zone pivot="programming-language-csharp"

```cs
phraseList.Clear();
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
phraselist->Clear();
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
phraseListGrammar.clear();
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
phrase_list_grammar.clear()
```

::: zone-end

::: zone pivot="programming-language-javascript"

```JavaScript
phraseListGrammar.clear();
```

::: zone-end

> [!NOTE]
> Bir `PhraseListGrammar` nesne üzerinde yapılan değişiklikler, bir sonraki tanıma veya konuşma hizmetine yeniden bağlanma sonrasında etkili olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma SDK başvuru belgeleri](speech-sdk.md)
