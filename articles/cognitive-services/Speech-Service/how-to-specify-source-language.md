---
title: Metinden metne konuşma için kaynak dil nasıl belirtilir?
titleSuffix: Azure Cognitive Services
description: Speech SDK, konuşmayı metne dönüştürürken kaynak dili belirtmenizi sağlar. Bu makalede, Konuşma hizmetinin kaynak dili bilmesi ve özel bir model hedefi sağlaması için FromConfig ve SourceLanguageConfig yöntemlerinin nasıl kullanılacağı açıklanmaktadır.
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/07/2020
ms.author: qiohu
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: f0723534d9d2187593cb73f058ffea62473b80a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235959"
---
# <a name="specify-source-language-for-speech-to-text"></a>Metinden metne konuşma için kaynak dili belirtin

Bu makalede, konuşma tanıma için Konuşma SDK'sına aktarılan bir ses girişinin kaynak dilini nasıl belirteceğinizi öğreneceksiniz. Ayrıca, geliştirilmiş tanıma için özel bir konuşma modeli belirtmek için örnek kod sağlanır.

::: zone pivot="programming-language-csharp"

## <a name="how-to-specify-source-language-in-c"></a>C'de kaynak dili nasıl belirtilir? #

Bu örnekte, kaynak dil açıkça yapı kullanılarak `SpeechRecognizer` bir parametre olarak sağlanır.

```csharp
var recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dil `SourceLanguageConfig`kullanılarak sağlanır. Daha sonra, oluşturmak için `sourceLanguageConfig` `SpeechRecognizer` bir parametre olarak geçirilir.

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dil ve özel `SourceLanguageConfig`bitiş noktası kullanılarak sağlanır. Daha sonra, oluşturmak için `sourceLanguageConfig` `SpeechRecognizer` bir parametre olarak geçirilir.

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE", "The Endpoint ID for your custom model.");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SpeechRecognitionLanguage`ve `EndpointId` set yöntemleri C#'daki `SpeechConfig` sınıftan çıkarılatılır. Bu yöntemlerin kullanımı önerilmez ve bir `SpeechRecognizer`.

::: zone-end

::: zone pivot="programming-language-cpp"


## <a name="how-to-specify-source-language-in-c"></a>C++'da kaynak dili nasıl belirtilir?

Bu örnekte, kaynak dil `FromConfig` yöntemi kullanılarak bir parametre olarak açıkça sağlanır.

```C++
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dil `SourceLanguageConfig`kullanılarak sağlanır. Daha sonra, bir parametre `FromConfig` olarak ne `recognizer`zaman oluştururken `sourceLanguageConfig` geçirilir .

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dil ve özel `SourceLanguageConfig`bitiş noktası kullanılarak sağlanır. Parametre `sourceLanguageConfig` olarak `FromConfig` aktarılır. `recognizer`

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE", "The Endpoint ID for your custom model.");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SetSpeechRecognitionLanguage`ve `SetEndpointId` C++ ve Java'daki sınıftan `SpeechConfig` amortismana alınan yöntemlerdir. Bu yöntemlerin kullanımı önerilmez ve bir `SpeechRecognizer`.

::: zone-end

::: zone pivot="programming-language-java"

## <a name="how-to-specify-source-language-in-java"></a>Java'da kaynak dil nasıl belirtilir?

Bu örnekte, kaynak dil yeni `SpeechRecognizer`bir .

```Java
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dil `SourceLanguageConfig`kullanılarak sağlanır. Daha sonra, yeni `sourceLanguageConfig` `SpeechRecognizer`bir parametre olarak geçirilir.

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dil ve özel `SourceLanguageConfig`bitiş noktası kullanılarak sağlanır. Daha sonra, yeni `sourceLanguageConfig` `SpeechRecognizer`bir parametre olarak geçirilir.

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE", "The Endpoint ID for your custom model.");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `setSpeechRecognitionLanguage`ve `setEndpointId` C++ ve Java'daki sınıftan `SpeechConfig` amortismana alınan yöntemlerdir. Bu yöntemlerin kullanımı önerilmez ve bir `SpeechRecognizer`.

::: zone-end

::: zone pivot="programming-language-python"

## <a name="how-to-specify-source-language-in-python"></a>Python'da kaynak dil nasıl belirtilir?

Bu örnekte, kaynak dil açıkça yapı kullanılarak `SpeechRecognizer` bir parametre olarak sağlanır.

```Python
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, language="de-DE", audio_config=audio_config)
```

Bu örnekte, kaynak dil `SourceLanguageConfig`kullanılarak sağlanır. Daha sonra, oluşturmak için `SourceLanguageConfig` `SpeechRecognizer` bir parametre olarak geçirilir.

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

Bu örnekte, kaynak dil ve özel `SourceLanguageConfig`bitiş noktası kullanılarak sağlanır. Daha sonra, oluşturmak için `SourceLanguageConfig` `SpeechRecognizer` bir parametre olarak geçirilir.

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE", "The Endpoint ID for your custom model.")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

>[!Note]
> `speech_recognition_language`ve `endpoint_id` özellikler Python'daki sınıftan `SpeechConfig` çıkarılatılır. Bu özelliklerin kullanımı önerilmez ve bir `SpeechRecognizer`.

::: zone-end

::: zone pivot="programming-language-more"

## <a name="how-to-specify-source-language-in-javascript"></a>Javascript'te kaynak dil nasıl belirtilir?

İlk adım oluşturmaktır: `SpeechConfig`

```Javascript
var speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionkey", "YourRegion");
```

Ardından, sesin kaynak dilini `speechRecognitionLanguage`şu ile belirtin:

```Javascript
speechConfig.speechRecognitionLanguage = "de-DE";
```

Tanıma için özel bir model kullanıyorsanız, bitiş noktasını `endpointId`aşağıdakilerle belirtebilirsiniz:

```Javascript
speechConfig.endpointId = "The Endpoint ID for your custom model.";
```

## <a name="how-to-specify-source-language-in-objective-c"></a>Objective-C'de kaynak dili nasıl belirtilir?

İlk adım oluşturmaktır: `speechConfig`

```Objective-C
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithSubscription:@"YourSubscriptionkey" region:@"YourRegion"];
```

Ardından, sesin kaynak dilini `speechRecognitionLanguage`şu ile belirtin:

```Objective-C
speechConfig.speechRecognitionLanguage = @"de-DE";
```

Tanıma için özel bir model kullanıyorsanız, bitiş noktasını `endpointId`aşağıdakilerle belirtebilirsiniz:

```Objective-C
speechConfig.endpointId = @"The Endpoint ID for your custom model.";
```

::: zone-end

## <a name="see-also"></a>Ayrıca bkz.

* Metin konuşma için desteklenen dillerin ve yerel dillerin listesi için [Dil desteğine](language-support.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma SDK referans belgeleri](speech-sdk.md)
