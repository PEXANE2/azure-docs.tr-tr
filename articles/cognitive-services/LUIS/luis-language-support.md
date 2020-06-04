---
title: Dil desteği-LUSıS
titleSuffix: Azure Cognitive Services
description: LUSıS, hizmet içindeki çeşitli özelliklere sahiptir. Tüm özellikler aynı dil eşliği değildir. İlgilendiğiniz özelliklerin hedeflediğiniz dil kültürüyle desteklendiğinden emin olun. Bir LUSıS uygulaması kültüre özgüdür ve ayarlandıktan sonra değiştirilemez.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 12/09/2019
ms.author: diberry
ms.openlocfilehash: 43858015857303d404b1abc0fe410aa462671587
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84340207"
---
# <a name="language-and-region-support-for-luis"></a>LUSıS için dil ve bölge desteği

LUSıS, hizmet içindeki çeşitli özelliklere sahiptir. Tüm özellikler aynı dil eşliği değildir. İlgilendiğiniz özelliklerin hedeflediğiniz dil kültürüyle desteklendiğinden emin olun. Bir LUSıS uygulaması kültüre özgüdür ve ayarlandıktan sonra değiştirilemez.

## <a name="multi-language-luis-apps"></a>Çok dilli LUSıS uygulamaları

Sohbet botu gibi çok dilli bir LUSıS istemci uygulamasına ihtiyacınız varsa, birkaç seçeneğiniz vardır. LUSıS tüm dilleri destekliyorsa, her dil için bir LUO uygulaması geliştirirsiniz. Her bir LUıN uygulamasının benzersiz bir uygulama KIMLIĞI ve uç nokta günlüğü vardır. Bir dil için dil desteği sağlamanız gerekiyorsa, bu bir dilin desteklenmeyen bir dile dönüştürülmesi için [Translator hizmetini](../Translator/translator-info-overview.md) kullanarak, utterliği desteklenen bir dile çevirebilir, söylenişi 'i Luo uç noktasına gönderebilir ve elde edilen puanları alabilirsiniz.

## <a name="languages-supported"></a>Desteklenen diller

LUBU, aşağıdaki dillerdeki söyliği anlamıştır:

| Dil |Yerel Ayar  |  Önceden oluşturulmuş etki alanı | Önceden oluşturulmuş varlık | Tümcecik listesi önerileri | **[Metin Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)<br>(Yaklaşım ve<br>Lerimi|
|--|--|:--:|:--:|:--:|:--:|
| İngilizce (ABD) |`en-US` | ✔ | ✔  |✔|✔|
| Arapça (Önizleme-modern Standart Arapça) |`ar-AR`|-|-|-|-|
| *[Çince](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Felemenkçe |`nl-NL` |✔|-|-|✔|
| Fransızca (Fransa) |`fr-FR` |✔| ✔ |✔ |✔|
| Fransızca (Kanada) |`fr-CA` |-|-|-|✔|
| Almanca |`de-DE` |✔| ✔ |✔ |✔|
| Gucerat dili | `gu-IN`|-|-|-|-|
| Hintçe | `hi-IN`|-|✔|-|-|
| İtalyanca |`it-IT` |✔| ✔ |✔|✔|
| *[Japonca](#japanese-support-notes) |`ja-JP` |✔| ✔ |✔|Yalnızca anahtar tümceciği|
| Korece |`ko-KR` |✔|-|-|Yalnızca anahtar tümceciği|
| Marathi | `mr-IN`|-|-|-|-|
| Portekizce (Brezilya) |`pt-BR` |✔| ✔ |✔ |tüm alt kültürler değil|
| İspanyolca (İspanya) |`es-ES` |✔| ✔ |✔|✔|
| İspanyolca (Meksika)|`es-MX` |-|-|✔|✔|
| Tamil dili | `ta-IN`|-|-|-|-|
| Telugu dili | `te-IN`|-|-|-|-|
| Türkçe | `tr-TR` |✔|✔|-|Yalnızca yaklaşım|




Dil desteği, [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md) ve [önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md)için farklılık gösterir.

[!INCLUDE [Chinese language support notes](includes/chinese-language-support-notes.md)]

### <a name="japanese-support-notes"></a>* Japonca destek notları

 - LUL, sözdizimsel analiz sağlamadığından, Keigo ve resmi olmayan Japonca arasındaki farkı anlayamayacak, farklı düzeylerde formlik düzeylerini uygulamalarınız için eğitim örnekleri olarak eklemeniz gerekir.
     - でございます, です ile aynı değildir.
     - です, だ ile aynı değildir.

[!INCLUDE [Text Analytics support notes](includes/text-analytics-support-notes.md)]

### <a name="speech-api-supported-languages"></a>Konuşma API 'SI desteklenen diller
Konuşma dikte modu dilleri için bkz. konuşma tarafından [desteklenen diller](../speech-service/speech-to-text.md) .

### <a name="bing-spell-check-supported-languages"></a>Desteklenen Bing Yazım Denetimi diller
Desteklenen dillerin ve durumun bir listesi için [desteklenen Bing yazım denetimi diller](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) bölümüne bakın.

## <a name="rare-or-foreign-words-in-an-application"></a>Bir uygulamadaki nadir veya yabancı sözcükler
Kültür içinde `en-us` , Lu, Slang dahil olmak üzere çoğu İngilizce sözcüğü ayırt edebilir. `zh-cn`Kültürde, Lu, en çok Çince karakteri ayırt etmek için öğrenir. İçinde nadir bir kelime `en-us` veya içinde bir karakter kullanırsanız `zh-cn` ve lubunun sözcüğü veya karakteri ayırt edemediği görürseniz, bu sözcüğü veya karakteri bir [tümcecik listesi özelliğine](luis-how-to-add-features.md)ekleyebilirsiniz. Örneğin, uygulamanın kültürünün dışındaki kelimeler (yani, yabancı sözcükler) bir tümcecik listesi özelliğine eklenmelidir.

<!--This phrase list should be marked non-interchangeable, to indicate that the set of rare words forms a class that LUIS should learn to recognize, but they are not synonyms or interchangeable with each other.-->

### <a name="hybrid-languages"></a>Karma diller
Karma diller, Ingilizce ve Çince gibi iki kültürden kelimeleri birleştirir. Bu diller, bir uygulamanın tek bir kültürü temel aldığı için LUSıS 'de desteklenmez.

## <a name="tokenization"></a>Simgeleştirme
Makine öğrenimi yapmak için, Lua, kültürü temel alan [belirteçlere](luis-glossary.md#token) göre kesintiye karşı keser.

|Dil|  Her boşluk veya özel karakter | karakter düzeyi|bileşik sözcükler
|--|:--:|:--:|:--:|
|Arapça|✔|||
|Çince||✔||
|Felemenkçe|✔||✔|
|İngilizce (en-us)|✔ |||
|Fransızca (fr-FR)|✔|||
|Fransızca (fr-CA)|✔|||
|Almanca|✔||✔|
|Gucerat dili|✔|||
|Hintçe|✔|||
|İtalyanca|✔|||
|Japonca|||✔
|Korece||✔||
|Marathi|✔|||
|Portekizce (Brezilya)|✔|||
|İspanyolca (ES-ES)|✔|||
|İspanyolca (es-MX)|✔|||
|Tamil dili|✔|||
|Telugu dili|✔|||
|Türkçe|✔|||


### <a name="custom-tokenizer-versions"></a>Özel Simgeleştirici sürümleri

Aşağıdaki kültürler özel belirteç Oluşturucu sürümlerine sahiptir:

|Culture (Kültür)|Sürüm|Amaç|
|--|--|--|
|Almanca<br>`de-de`|1.0.0|Bileşik sözcüklerin tek bileşenlerine bölünmeye çalışan makine öğrenimi tabanlı bir belirteç Oluşturucu kullanarak onları bölerek sözcükleri simgeleştirir.<br>Bir Kullanıcı `Ich fahre einen krankenwagen` bir utterance olarak girerse, özelliği olarak ayarlanır `Ich fahre einen kranken wagen` . `kranken` `wagen` Farklı varlıklar olarak ve bağımsız olarak işaretlemeye izin verme.|
|Almanca<br>`de-de`|1.0.2|Sözcükleri boşluklarla ayırarak simgeleştirir.<br> Bir Kullanıcı bir `Ich fahre einen krankenwagen` utterance olarak girerse, tek bir belirteç kalır. Bu nedenle `krankenwagen` tek bir varlık olarak işaretlenir. |
|Felemenkçe<br>`de-de`|1.0.0|Bileşik sözcüklerin tek bileşenlerine bölünmeye çalışan makine öğrenimi tabanlı bir belirteç Oluşturucu kullanarak onları bölerek sözcükleri simgeleştirir.<br>Bir Kullanıcı `Ik ga naar de kleuterschool` bir utterance olarak girerse, özelliği olarak ayarlanır `Ik ga naar de kleuter school` . `kleuter` `school` Farklı varlıklar olarak ve bağımsız olarak işaretlemeye izin verme.|
|Felemenkçe<br>`de-de`|1.0.1|Sözcükleri boşluklarla ayırarak simgeleştirir.<br> Bir Kullanıcı bir `Ik ga naar de kleuterschool` utterance olarak girerse, tek bir belirteç kalır. Bu nedenle `kleuterschool` tek bir varlık olarak işaretlenir. |


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
