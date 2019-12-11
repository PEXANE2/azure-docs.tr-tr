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
ms.openlocfilehash: f6b95f76af4c83459ac81ff1703d8588f649326c
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74970551"
---
# <a name="language-and-region-support-for-luis"></a>LUIS dil ve bölge desteği

LUIS, çeşitli hizmetinde özellikleri vardır. Aynı dil eşliğine tüm özellikleridir. İlgilendiğiniz özellikleri hedeflediğiniz dil kültürünü desteklendiğinden emin olun. Bir LUIS uygulaması kültüre özgü olan ve ayarlandıktan sonra değiştirilemez.

## <a name="multi-language-luis-apps"></a>Çok dilli LUIS uygulamaları

Çok dilli LUIS istemci uygulama bir sohbet Robotu gibi gerekiyorsa, birkaç seçeneğiniz vardır. LUIS, tüm diller destekliyorsa, her dil için bir LUIS uygulaması geliştirin. Her LUIS uygulamanın benzersiz uygulama kimliği ve uç nokta günlük vardır. Language Understanding LUIS desteklemez, bir dil için kullanabileceğiniz sağlamak gerekiyorsa [Microsoft Translator API'si](../Translator/translator-info-overview.md) utterance desteklenen bir dile çevirmek için utterance LUIS uç noktasına gönderme ve alma Sonuçta elde edilen puanları.

## <a name="languages-supported"></a>Desteklenen diller

LUIS, konuşma şu dillerde anlar:

| Dil |Yerel ayar  |  Önceden oluşturulmuş etki alanı | Önceden oluşturulmuş varlık | Tümcecik listesi önerileri | **[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)<br>(Duygu ve<br>Anahtar sözcükleri)|
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

Dil desteği değişir için [önceden oluşturulmuş varlıklarla](luis-reference-prebuilt-entities.md) ve [önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md).

[!INCLUDE [Chinese language support notes](includes/chinese-language-support-notes.md)]

### <a name="japanese-support-notes"></a>\* Japonca desteği notları

 - LUIS söz dizimi analizi sağlamaz ve Keigo resmi olmayan Japonca arasındaki farkı anlamak değil çünkü formality eğitim örnekler uygulamalarınız için farklı düzeylerde birleştirmek gerekir.
     - でございます です ile aynı değil.
     - です だ ile aynı değil.

[!INCLUDE [Text Analytics support notes](includes/text-analytics-support-notes.md)]

### <a name="speech-api-supported-languages"></a>Konuşma API'si desteklenen diller
Konuşma bkz [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode) konuşma dikte modu diller için.

### <a name="bing-spell-check-supported-languages"></a>Desteklenen Bing yazım denetimi dilleri
Bing yazım denetimi bkz [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) için desteklenen diller ve durum listesi.

## <a name="rare-or-foreign-words-in-an-application"></a>Bir uygulamada nadir veya yabancı kelimeler
İçinde `en-us` kültür LUIS öğrenir argo kullanımlar da dahil olmak üzere, en Türkçe kelimeler ayırt etmek. İçinde `zh-cn` kültür LUIS öğrenir çoğu Çince karakter ayırt etmek. Nadir bir sözcük kullanırsanız `en-us` veya karakter olarak `zh-cn`, ve LUIS gibi görünüyor, word veya karakter ayırt edemez, sözcük ekleyin veya için karakter gördüğünüz bir [tümcecik listesi özelliği](luis-how-to-add-features.md). Örneğin, bir ifade listesi özelliğini sözcükleri--yabancı kelimeler--kültürünü dışında eklenmesi gerekir.

<!--This phrase list should be marked non-interchangeable, to indicate that the set of rare words forms a class that LUIS should learn to recognize, but they are not synonyms or interchangeable with each other.-->

### <a name="hybrid-languages"></a>Karma dilleri
Karma dil İngilizce ve Çince gibi iki kültürün sözcük birleştirin. Uygulama tek bir kültürü temel aldığından, bu dillerden LUIS desteklenmez.

## <a name="tokenization"></a>Simgeleştirme
Makine öğrenimi için LUIS bir utterance keser [belirteçleri](luis-glossary.md#token) kültüre göre.

|Dil|  her alanı ya da özel karakter | karakter düzeyi|Bileşik sözcüklerin|[parçalanmış varlık döndürdü](luis-concept-data-extraction.md#tokenized-entity-returned)
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
