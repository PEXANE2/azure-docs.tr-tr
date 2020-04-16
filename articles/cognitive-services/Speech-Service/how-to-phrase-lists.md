---
title: İfade Listeleri - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne tanıma sonuçlarını iyileştirmek için `PhraseListGrammar` nesneyi kullanarak Konuşma hizmetini bir İfade Listesi ile nasıl sağlayacağınızı öğrenin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 5a21358edae4c61f35993770c22634da9ac83633
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81401961"
---
# <a name="phrase-lists-for-speech-to-text"></a>Metin den sözle'ye için tümcecik Listeleri

Konuşma hizmetine bir tümcecik listesi sağlayarak, konuşma tanımanın doğruluğunu artırabilirsiniz. Tümcecik Listeleri, ses verilerindeki bilinen ifadeleri tanımlamak için kullanılır, örneğin bir kişinin adı veya belirli bir konum.

Örnek olarak, "Taşın" komutunuz ve konuşulabilecek olası bir "Ward" hedefiniz varsa, "Ward'a Taşı" girişi ekleyebilirsiniz. Bir tümcecik eklemek, ses "Ward'a Taşı" yerine "Koğuşa Taşı" olarak tanındığı tanılandığında "Doğruya Taşı" olasılığı artar.

Tümcecik Listesine tek sözcük veya tam tümcecik eklenebilir. Tanıma sırasında, tümceciğin tamamı için tam bir eşleşme sese ayrı bir tümcecik olarak eklenmişse, bir tümcecik listesindeki bir giriş kullanılır. İfadeyle tam eşleşme bulunamazsa, tanıma yardımcı olmaz.

>[!Note]
> Şu anda, Tümce listeleri yalnızca İngilizce'yi metin konuşma için destekler.

## <a name="how-to-use-phrase-lists"></a>İfade Listeleri nasıl kullanılır?

Aşağıdaki örnekler, nesneyi kullanarak Bir `PhraseListGrammar` İfade Listesi'nin nasıl oluşturulabildiğini göstermektedir.

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

::: zone pivot="programming-language-more"

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

::: zone-end

>[!Note]
> Konuşma hizmetinin konuşmayla eşleştirmek için kullanacağı en fazla İfade Listesi sayısı 1024'tür.

Ayrıca clear() `PhraseListGrammar` arayarak ilişkili tümcecikleri de temizleyebilirsiniz.

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

::: zone pivot="programming-language-more"

```JavaScript
phraseListGrammar.clear();
```

::: zone-end

> [!NOTE]
> Bir `PhraseListGrammar` nesnedeki değişiklikler, bir sonraki tanıma da veya Konuşma hizmetine yeniden bağlanmayı izleyen bir şekilde etkili olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma SDK referans belgeleri](speech-sdk.md)
