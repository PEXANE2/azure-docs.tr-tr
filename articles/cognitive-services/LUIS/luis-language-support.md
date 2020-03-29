---
title: Dil desteği - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS hizmet içinde çeşitli özelliklere sahiptir. Tüm özellikler aynı dil paritesi değildir. İlgilendiğiniz özelliklerin hedeflediğiniz dil kültüründe desteklendiğinden emin olun. LUIS uygulaması kültüre özgüdür ve ayarlandıktan sonra değiştirilemez.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: diberry
ms.openlocfilehash: 4b6d954d06f09bef5240bddc4860ddbc83513d69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220858"
---
# <a name="language-and-region-support-for-luis"></a>LUIS için dil ve bölge desteği

LUIS hizmet içinde çeşitli özelliklere sahiptir. Tüm özellikler aynı dil paritesi değildir. İlgilendiğiniz özelliklerin hedeflediğiniz dil kültüründe desteklendiğinden emin olun. LUIS uygulaması kültüre özgüdür ve ayarlandıktan sonra değiştirilemez.

## <a name="multi-language-luis-apps"></a>Çok dilli LUIS uygulamaları

Chatbot gibi çok dilli bir LUIS istemci uygulamasına ihtiyacınız varsa, birkaç seçeneğiniz vardır. LUIS tüm dilleri destekliyorsa, her dil için bir LUIS uygulaması geliştirirsiniz. Her LUIS uygulamasının benzersiz bir uygulama kimliği ve bitiş noktası günlüğü vardır. LUIS'in desteklemediği bir dil için dil bilgisi sağlamanız gerekiyorsa, söyleyiyi desteklenen bir dile çevirmek, söyleyiyi LUIS bitiş noktasına göndermek ve elde edilen puanları almak için [Microsoft Translator API'yi](../Translator/translator-info-overview.md) kullanabilirsiniz.

## <a name="languages-supported"></a>Desteklenen diller

LUIS aşağıdaki dillerdeki sözlerle anlar:

| Dil |Yerel Ayar  |  Önceden oluşturulmuş etki alanı | Önceden oluşturulmuş varlık | İfade listesi önerileri | **[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)<br>(Duygu ve<br>Anahtar Kelimeler)|
|--|--|:--:|:--:|:--:|:--:|
| Amerikan İngilizcesi |`en-US` | ✔ | ✔  |✔|✔|
| Arapça (önizleme - modern standart Arapça) |`ar-AR`|-|-|-|-|
| *[Çince](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Felemenkçe |`nl-NL` |✔|  -   |-|✔|
| Fransızca (Fransa) |`fr-FR` |✔| ✔ |✔ |✔|
| Fransızca (Kanada) |`fr-CA` |-|   -   |-|✔|
| Almanca |`de-DE` |✔| ✔ |✔ |✔|
| Hintçe | `hi-IN`|-|-|-|-|
| İtalyanca |`it-IT` |✔| ✔ |✔|✔|
| *[Japonca](#japanese-support-notes) |`ja-JP` |✔| ✔ |✔|Yalnızca anahtar tümcecik|
| Korece |`ko-KR` |✔|   -   |-|Yalnızca anahtar tümcecik|
| Portekizce (Brezilya) |`pt-BR` |✔| ✔ |✔ |tüm alt kültürler|
| İspanyolca (İspanya) |`es-ES` |✔| ✔ |✔|✔|
| İspanyolca (Meksika)|`es-MX` |-|  -   |✔|✔|
| Türkçe | `tr-TR` |✔|-|-|Sadece duygusallık|

Dil [desteği, önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md) ve önceden oluşturulmuş etki [alanları](luis-reference-prebuilt-domains.md)için değişir.

[!INCLUDE [Chinese language support notes](includes/chinese-language-support-notes.md)]

### <a name="japanese-support-notes"></a>*Japonca destek notları

 - LUIS sözdizimdizimi analizi sağlamadığından ve Keigo ile gayri resmi Japonca arasındaki farkı anlamadığından, farklı formalite düzeylerini uygulamalarınız için eğitim örneği olarak dahil etmiş olmanız gerekir.
     - The 中中中中中 aynı değildir .
     - Bu durum la aynı değildir.

[!INCLUDE [Text Analytics support notes](includes/text-analytics-support-notes.md)]

### <a name="speech-api-supported-languages"></a>Konuşma API desteklenen diller
Konuşma dikte modu dilleri için Konuşma [Destekli dillere](../speech-service/speech-to-text.md) bakın.

### <a name="bing-spell-check-supported-languages"></a>Bing Yazım Denetimi desteklenen diller
Desteklenen diller ve durumların listesi için Bing Büyüsü Denetimi [Destekli dillere](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) bakın.

## <a name="rare-or-foreign-words-in-an-application"></a>Bir uygulamada nadir veya yabancı sözcükler
Kültürde, `en-us` LUIS argo da dahil olmak üzere çoğu İngilizce kelimeyi ayırt etmeyi öğrenir. Kültürde, `zh-cn` LUIS çoğu Çince karakterleri ayırt etmeyi öğrenir. 'de `en-us` nadir bulunan bir sözcük veya karakter `zh-cn`kullanırsanız ve LUIS'in bu sözcüğü veya karakteri ayırt edemediğini görürseniz, bu sözcüğü veya karakteri bir ifade listesi [özelliğine](luis-how-to-add-features.md)ekleyebilirsiniz. Örneğin, uygulama kültürünün dışındaki sözcükler -yani yabancı sözcükler- bir ifade listesi özelliğine eklenmelidir.

<!--This phrase list should be marked non-interchangeable, to indicate that the set of rare words forms a class that LUIS should learn to recognize, but they are not synonyms or interchangeable with each other.-->

### <a name="hybrid-languages"></a>Hibrit diller
Karma diller İngilizce ve Çince gibi iki kültürden gelen sözcükleri birleştirir. Bir uygulama tek bir kültüre dayandığı ndan bu diller LUIS'te desteklenmez.

## <a name="tokenization"></a>Tokenization
Makine öğrenimini gerçekleştirmek için LUIS, kültüre dayalı [belirteçlere](luis-glossary.md#token) bir söz verir.

|Dil|  her alan veya özel karakter | karakter düzeyi|bileşik sözcükler|[tokenized varlık döndü](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|Arapça|||||
|Çince||✔||✔|
|Felemenkçe|||✔|✔|
|İngilizce (en-us)|✔ ||||
|Fransızca (fr-FR)|✔||||
|Fransızca (fr-CA)|✔||||
|Almanca|||✔|✔|
| Hintçe |✔|-|-|-|-|
|İtalyanca|✔||||
|Japonca||||✔|
|Korece||✔||✔|
|Portekizce (Brezilya)|✔||||
|İspanyolca (es-ES)|✔||||
|İspanyolca (es-MX)|✔||||

### <a name="custom-tokenizer-versions"></a>Özel belirteç sürümleri

Aşağıdaki kültürlerin özel belirteç sürümleri vardır:

|Kültür|Sürüm|Amaç|
|--|--|--|
|Almanca<br>`de-de`|1.0.0|Sözcükleri, bileşik sözcükleri tek bileşenlerine ayırmaya çalışan makine öğrenimi tabanlı bir belirteç kullanarak bölerek sözcükleri tokenize eder.<br>Bir kullanıcı `Ich fahre einen krankenwagen` bir söz olarak girerse, `Ich fahre einen kranken wagen`bu. Farklı varlıklar olarak `kranken` `wagen` ve bağımsız olarak işaretlenmesine izin verir.|
|Almanca<br>`de-de`|1.0.2|Sözcükleri boşluklara bölerek tokenize eder.<br> bir kullanıcı `Ich fahre einen krankenwagen` bir söz olarak girerse, tek bir belirteç kalır. Böylece `krankenwagen` tek bir varlık olarak işaretlenir. |

### <a name="migrating-between-tokenizer-versions"></a>Tokenizer sürümleri arasında geçiş
<!--
Your first choice is to change the tokenizer version in the app file, then import the version. This action changes how the utterances are tokenized but allows you to keep the same app ID.

Tokenizer JSON for 1.0.0. Notice the property value for  `tokenizerVersion`.

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.0",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.0",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 23,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```

Tokenizer JSON for version 1.0.1. Notice the property value for  `tokenizerVersion`.

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.1",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.1",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 16,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```
-->

Belirteçleştirme uygulama düzeyinde gerçekleşir. Sürüm düzeyinde belirteç leştirme desteği yoktur.

Dosyayı sürüm yerine [yeni bir uygulama olarak aktarın.](luis-how-to-start-new-app.md) Bu eylem, yeni uygulamanın farklı bir uygulama kimliğine sahip olduğu ancak dosyada belirtilen tokenizer sürümünü kullandığı anlamına gelir.
