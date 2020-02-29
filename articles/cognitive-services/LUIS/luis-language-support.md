---
title: Dil desteği-LUSıS
titleSuffix: Azure Cognitive Services
description: LUIS, çeşitli hizmetinde özellikleri vardır. Aynı dil eşliğine tüm özellikleridir. İlgilendiğiniz özellikleri hedeflediğiniz dil kültürünü desteklendiğinden emin olun. Bir LUIS uygulaması kültüre özgü olan ve ayarlandıktan sonra değiştirilemez.
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
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77916600"
---
# <a name="language-and-region-support-for-luis"></a>LUIS dil ve bölge desteği

LUIS, çeşitli hizmetinde özellikleri vardır. Aynı dil eşliğine tüm özellikleridir. İlgilendiğiniz özellikleri hedeflediğiniz dil kültürünü desteklendiğinden emin olun. Bir LUIS uygulaması kültüre özgü olan ve ayarlandıktan sonra değiştirilemez.

## <a name="multi-language-luis-apps"></a>Çok dilli LUIS uygulamaları

Çok dilli LUIS istemci uygulama bir sohbet Robotu gibi gerekiyorsa, birkaç seçeneğiniz vardır. LUIS, tüm diller destekliyorsa, her dil için bir LUIS uygulaması geliştirin. Her LUIS uygulamanın benzersiz uygulama kimliği ve uç nokta günlük vardır. Bir dil için dil desteği sağlamanız gerekiyorsa, bu bir dilin desteklenmeyen bir dile dönüştürülmesi için [Microsoft Translator API 'sini](../Translator/translator-info-overview.md) kullanabilir, UTTERANLIĞI, Halme uç noktasına gönderebilir ve elde edilen puanları alabilirsiniz.

## <a name="languages-supported"></a>Desteklenen diller

LUIS, konuşma şu dillerde anlar:

| Dil |Yerel Ayar  |  Önceden oluşturulmuş etki alanı | Önceden oluşturulmuş varlık | Tümcecik listesi önerileri | **[metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)<br>(Duygu ve<br>Anahtar sözcükleri)|
|--|--|:--:|:--:|:--:|:--:|
| Amerikan İngilizcesi |`en-US` | ✔ | ✔  |✔|✔|
| Arapça (Önizleme-modern Standart Arapça) |`ar-AR`|-|-|-|-|
| *[Çince](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Felemenkçe |`nl-NL` |✔|  -   |-|✔|
| Fransızca (Fransa) |`fr-FR` |✔| ✔ |✔ |✔|
| Fransızca (Kanada) |`fr-CA` |-|   -   |-|✔|
| Almanca |`de-DE` |✔| ✔ |✔ |✔|
| Hintçe | `hi-IN`|-|-|-|-|
| İtalyanca |`it-IT` |✔| ✔ |✔|✔|
| *[Japonca](#japanese-support-notes) |`ja-JP` |✔| ✔ |✔|Yalnızca anahtar ifade|
| Korece |`ko-KR` |✔|   -   |-|Yalnızca anahtar ifade|
| Portekizce (Brezilya) |`pt-BR` |✔| ✔ |✔ |tüm alt kültürler|
| İspanyolca (İspanya) |`es-ES` |✔| ✔ |✔|✔|
| İspanyolca (Meksika)|`es-MX` |-|  -   |✔|✔|
| Türkçe | `tr-TR` |✔|-|-|Yalnızca yaklaşım|

Dil desteği, [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md) ve [önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md)için farklılık gösterir.

[!INCLUDE [Chinese language support notes](includes/chinese-language-support-notes.md)]

### <a name="japanese-support-notes"></a>\* Japonca desteği notları

 - LUIS söz dizimi analizi sağlamaz ve Keigo resmi olmayan Japonca arasındaki farkı anlamak değil çünkü formality eğitim örnekler uygulamalarınız için farklı düzeylerde birleştirmek gerekir.
     - でございます です ile aynı değil.
     - です だ ile aynı değil.

[!INCLUDE [Text Analytics support notes](includes/text-analytics-support-notes.md)]

### <a name="speech-api-supported-languages"></a>Konuşma API'si desteklenen diller
Konuşma dikte modu dilleri için bkz. konuşma tarafından [desteklenen diller](../speech-service/speech-to-text.md) .

### <a name="bing-spell-check-supported-languages"></a>Desteklenen Bing yazım denetimi dilleri
Desteklenen dillerin ve durumun bir listesi için [desteklenen Bing yazım denetimi diller](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) bölümüne bakın.

## <a name="rare-or-foreign-words-in-an-application"></a>Bir uygulamada nadir veya yabancı kelimeler
`en-us` kültürü olan lu, Slang dahil olmak üzere çoğu Ingilizce sözcüğü ayırt etmek için öğrenir. `zh-cn` kültürü, Lu, çoğu Çince karakteri ayırt ediyor. `zh-cn``en-us` veya karakter içinde nadir bir kelime kullanırsanız ve LUBUNUN söz konusu sözcüğü veya karakteri ayırt edemediği görürseniz, bu sözcüğü veya karakteri bir [tümcecik listesi özelliğine](luis-how-to-add-features.md)ekleyebilirsiniz. Örneğin, bir ifade listesi özelliğini sözcükleri--yabancı kelimeler--kültürünü dışında eklenmesi gerekir.

<!--This phrase list should be marked non-interchangeable, to indicate that the set of rare words forms a class that LUIS should learn to recognize, but they are not synonyms or interchangeable with each other.-->

### <a name="hybrid-languages"></a>Karma dilleri
Karma dil İngilizce ve Çince gibi iki kültürün sözcük birleştirin. Uygulama tek bir kültürü temel aldığından, bu dillerden LUIS desteklenmez.

## <a name="tokenization"></a>Simgeleştirme
Makine öğrenimi yapmak için, Lua, kültürü temel alan [belirteçlere](luis-glossary.md#token) göre kesintiye karşı keser.

|Dil|  her alanı ya da özel karakter | karakter düzeyi|Bileşik sözcüklerin|[simgeleştirilmiş varlık döndürüldü](luis-concept-data-extraction.md#tokenized-entity-returned)
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

### <a name="custom-tokenizer-versions"></a>Özel Simgeleştirici sürümleri

Aşağıdaki kültürler özel belirteç Oluşturucu sürümlerine sahiptir:

|Kültür|Sürüm|Amaç|
|--|--|--|
|Almanca<br>`de-de`|1.0.0|Bileşik sözcüklerin tek bileşenlerine bölünmeye çalışan makine öğrenimi tabanlı bir belirteç Oluşturucu kullanarak onları bölerek sözcükleri simgeleştirir.<br>Kullanıcı `Ich fahre einen krankenwagen` bir utterance olarak girerse, `Ich fahre einen kranken wagen`açıktır. `kranken` ve `wagen` bağımsız olarak farklı varlıklar olarak işaretlemeye izin verme.|
|Almanca<br>`de-de`|1.0.2|Sözcükleri boşluklarla ayırarak simgeleştirir.<br> Kullanıcı `Ich fahre einen krankenwagen` bir utterance olarak girerse, tek bir belirteç kalır. Bu nedenle `krankenwagen` tek bir varlık olarak işaretlenir. |

### <a name="migrating-between-tokenizer-versions"></a>Belirteç Oluşturucu sürümler arasında geçiş yapma
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

Belirteç seçme, uygulama düzeyinde gerçekleşir. Sürüm düzeyi simgeleştirme desteği yoktur.

[Dosyayı sürüm yerine yeni bir uygulama olarak Içeri aktarın](luis-how-to-start-new-app.md). Bu eylem, yeni uygulamanın farklı bir uygulama KIMLIĞINE sahip olduğu ancak dosyada belirtilen Simgeleştirici sürümünü kullandığı anlamına gelir.
