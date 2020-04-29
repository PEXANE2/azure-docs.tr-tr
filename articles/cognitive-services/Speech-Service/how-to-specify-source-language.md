---
title: Konuşmayı metne dönüştürme için kaynak dilini belirtme
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'Sı, konuşmayı metne dönüştürürken kaynak dilini belirtmenize olanak tanır. Bu makalede, konuşma hizmetinin kaynak dili bilmesini ve özel bir model hedefi sağlamasını sağlamak için FromConfig ve SourceLanguageConfig yöntemlerinin nasıl kullanılacağı açıklanır.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80235959"
---
# <a name="specify-source-language-for-speech-to-text"></a>Konuşmadan metne yönelik kaynak dilini belirtin

Bu makalede, konuşma tanıma için konuşma SDK 'sına geçilen bir ses girişi için kaynak dilin nasıl belirtildiğini öğreneceksiniz. Ayrıca, geliştirilmiş tanıma için özel bir konuşma modeli belirtmek üzere örnek kod sağlanır.

::: zone pivot="programming-language-csharp"

## <a name="how-to-specify-source-language-in-c"></a>C 'de kaynak dilini belirtme #

Bu örnekte, kaynak dili açıkça yapı kullanılarak `SpeechRecognizer` bir parametre olarak sağlanır.

```csharp
var recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dili kullanılarak `SourceLanguageConfig`sunulmaktadır. `sourceLanguageConfig` Ardından, yapı için `SpeechRecognizer` bir parametre olarak geçirilir.

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak `SourceLanguageConfig`sağlanır. `sourceLanguageConfig` Ardından, yapı için `SpeechRecognizer` bir parametre olarak geçirilir.

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE", "The Endpoint ID for your custom model.");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SpeechRecognitionLanguage`ve `EndpointId` set yöntemleri C# ' deki `SpeechConfig` sınıftan kullanımdan kaldırılmıştır. Bu yöntemlerin kullanımı önerilmez ve bir `SpeechRecognizer`oluştururken kullanılmamalıdır.

::: zone-end

::: zone pivot="programming-language-cpp"


## <a name="how-to-specify-source-language-in-c"></a>C++ ' da kaynak dilini belirtme

Bu örnekte kaynak dili, `FromConfig` yöntemi kullanılarak açıkça bir parametre olarak sağlanır.

```C++
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dili kullanılarak `SourceLanguageConfig`sunulmaktadır. `sourceLanguageConfig` Ardından,, oluşturma `FromConfig` sırasında parametresi olarak geçirilir `recognizer`.

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak `SourceLanguageConfig`sağlanır. `sourceLanguageConfig` , Oluşturma `FromConfig` sırasında parametresi olarak geçirilir `recognizer`.

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE", "The Endpoint ID for your custom model.");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SetSpeechRecognitionLanguage`ve `SetEndpointId` , C++ ve Java 'daki `SpeechConfig` sınıfından kullanım dışı yöntemlerdir. Bu yöntemlerin kullanımı önerilmez ve bir `SpeechRecognizer`oluştururken kullanılmamalıdır.

::: zone-end

::: zone pivot="programming-language-java"

## <a name="how-to-specify-source-language-in-java"></a>Java 'da kaynak dilini belirtme

Bu örnekte, yeni `SpeechRecognizer`bir oluşturma sırasında kaynak dili açık olarak sağlanır.

```Java
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dili kullanılarak `SourceLanguageConfig`sunulmaktadır. `sourceLanguageConfig` Ardından, yeni `SpeechRecognizer`bir oluşturma sırasında parametresi olarak geçirilir.

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak `SourceLanguageConfig`sağlanır. `sourceLanguageConfig` Ardından, yeni `SpeechRecognizer`bir oluşturma sırasında parametresi olarak geçirilir.

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE", "The Endpoint ID for your custom model.");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `setSpeechRecognitionLanguage`ve `setEndpointId` , C++ ve Java 'daki `SpeechConfig` sınıfından kullanım dışı yöntemlerdir. Bu yöntemlerin kullanımı önerilmez ve bir `SpeechRecognizer`oluştururken kullanılmamalıdır.

::: zone-end

::: zone pivot="programming-language-python"

## <a name="how-to-specify-source-language-in-python"></a>Python 'da kaynak dilini belirtme

Bu örnekte, kaynak dili açıkça yapı kullanılarak `SpeechRecognizer` bir parametre olarak sağlanır.

```Python
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, language="de-DE", audio_config=audio_config)
```

Bu örnekte, kaynak dili kullanılarak `SourceLanguageConfig`sunulmaktadır. `SourceLanguageConfig` Ardından, yapı için `SpeechRecognizer` bir parametre olarak geçirilir.

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak `SourceLanguageConfig`sağlanır. `SourceLanguageConfig` Ardından, yapı için `SpeechRecognizer` bir parametre olarak geçirilir.

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE", "The Endpoint ID for your custom model.")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

>[!Note]
> `speech_recognition_language`ve `endpoint_id` Özellikler Python 'daki `SpeechConfig` sınıftan kullanımdan kaldırılmıştır. Bu özelliklerin kullanımı önerilmez ve bir `SpeechRecognizer`oluştururken kullanılmamalıdır.

::: zone-end

::: zone pivot="programming-language-more"

## <a name="how-to-specify-source-language-in-javascript"></a>JavaScript 'te kaynak dilini belirtme

İlk adım `SpeechConfig`şunları oluşturmaktır:

```Javascript
var speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionkey", "YourRegion");
```

Sonra, sesinizin kaynak dilini belirtin `speechRecognitionLanguage`:

```Javascript
speechConfig.speechRecognitionLanguage = "de-DE";
```

Tanıma için özel bir model kullanıyorsanız, uç noktayı şu şekilde `endpointId`belirtebilirsiniz:

```Javascript
speechConfig.endpointId = "The Endpoint ID for your custom model.";
```

## <a name="how-to-specify-source-language-in-objective-c"></a>Amaç-C ' d e kaynak dilini belirtme

İlk adım `speechConfig`şunları oluşturmaktır:

```Objective-C
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithSubscription:@"YourSubscriptionkey" region:@"YourRegion"];
```

Sonra, sesinizin kaynak dilini belirtin `speechRecognitionLanguage`:

```Objective-C
speechConfig.speechRecognitionLanguage = @"de-DE";
```

Tanıma için özel bir model kullanıyorsanız, uç noktayı şu şekilde `endpointId`belirtebilirsiniz:

```Objective-C
speechConfig.endpointId = @"The Endpoint ID for your custom model.";
```

::: zone-end

## <a name="see-also"></a>Ayrıca bkz.

* Konuşma için desteklenen dillerin ve yerel ayarların listesi için bkz. [dil desteği](language-support.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma SDK başvuru belgeleri](speech-sdk.md)
