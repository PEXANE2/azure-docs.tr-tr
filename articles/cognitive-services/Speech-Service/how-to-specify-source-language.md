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
ms.date: 05/19/2020
ms.author: qiohu
zone_pivot_groups: programming-languages-set-two
ms.custom: devx-track-javascript, devx-track-csharp
ms.openlocfilehash: 3d761bbad4cb2cd2cdd1c34459f25c811bb41c7e
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88918698"
---
# <a name="specify-source-language-for-speech-to-text"></a>Konuşmadan metne yönelik kaynak dilini belirtin

Bu makalede, konuşma tanıma için konuşma SDK 'sına geçilen bir ses girişi için kaynak dilin nasıl belirtildiğini öğreneceksiniz. Ayrıca, geliştirilmiş tanıma için özel bir konuşma modeli belirtmek üzere örnek kod sağlanır.

::: zone pivot="programming-language-csharp"

## <a name="how-to-specify-source-language-in-c"></a>C 'de kaynak dilini belirtme #

Bu örnekte, kaynak dili açıkça yapı kullanılarak bir parametre olarak sağlanır `SpeechRecognizer` .

```csharp
var recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dili kullanılarak sunulmaktadır `SourceLanguageConfig` . Ardından, `sourceLanguageConfig` Yapı için bir parametre olarak geçirilir `SpeechRecognizer` .

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak sağlanır `SourceLanguageConfig` . Ardından, `sourceLanguageConfig` Yapı için bir parametre olarak geçirilir `SpeechRecognizer` .

```csharp
var sourceLanguageConfig = SourceLanguageConfig.FromLanguage("de-DE", "The Endpoint ID for your custom model.");
var recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SpeechRecognitionLanguage` ve `EndpointId` set yöntemleri `SpeechConfig` C# ' deki sınıftan kullanımdan kaldırılmıştır. Bu yöntemlerin kullanımı önerilmez ve bir oluştururken kullanılmamalıdır `SpeechRecognizer` .

::: zone-end

::: zone pivot="programming-language-cpp"


## <a name="how-to-specify-source-language-in-c"></a>C++ ' da kaynak dilini belirtme

Bu örnekte kaynak dili, yöntemi kullanılarak açıkça bir parametre olarak sağlanır `FromConfig` .

```C++
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dili kullanılarak sunulmaktadır `SourceLanguageConfig` . Ardından,, `sourceLanguageConfig` oluşturma sırasında parametresi olarak geçirilir `FromConfig` `recognizer` .

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak sağlanır `SourceLanguageConfig` . , `sourceLanguageConfig` Oluşturma sırasında parametresi olarak geçirilir `FromConfig` `recognizer` .

```C++
auto sourceLanguageConfig = SourceLanguageConfig::FromLanguage("de-DE", "The Endpoint ID for your custom model.");
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `SetSpeechRecognitionLanguage` ve, `SetEndpointId` `SpeechConfig` C++ ve Java 'daki sınıfından kullanım dışı yöntemlerdir. Bu yöntemlerin kullanımı önerilmez ve bir oluştururken kullanılmamalıdır `SpeechRecognizer` .

::: zone-end

::: zone pivot="programming-language-java"

## <a name="how-to-specify-source-language-in-java"></a>Java 'da kaynak dilini belirtme

Bu örnekte, yeni bir oluşturma sırasında kaynak dili açık olarak sağlanır `SpeechRecognizer` .

```Java
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, "de-DE", audioConfig);
```

Bu örnekte, kaynak dili kullanılarak sunulmaktadır `SourceLanguageConfig` . Ardından, `sourceLanguageConfig` Yeni bir oluşturma sırasında parametresi olarak geçirilir `SpeechRecognizer` .

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak sağlanır `SourceLanguageConfig` . Ardından, `sourceLanguageConfig` Yeni bir oluşturma sırasında parametresi olarak geçirilir `SpeechRecognizer` .

```Java
SourceLanguageConfig sourceLanguageConfig = SourceLanguageConfig.fromLanguage("de-DE", "The Endpoint ID for your custom model.");
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, sourceLanguageConfig, audioConfig);
```

>[!Note]
> `setSpeechRecognitionLanguage` ve, `setEndpointId` `SpeechConfig` C++ ve Java 'daki sınıfından kullanım dışı yöntemlerdir. Bu yöntemlerin kullanımı önerilmez ve bir oluştururken kullanılmamalıdır `SpeechRecognizer` .

::: zone-end

::: zone pivot="programming-language-python"

## <a name="how-to-specify-source-language-in-python"></a>Python 'da kaynak dilini belirtme

Bu örnekte, kaynak dili açıkça yapı kullanılarak bir parametre olarak sağlanır `SpeechRecognizer` .

```Python
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, language="de-DE", audio_config=audio_config)
```

Bu örnekte, kaynak dili kullanılarak sunulmaktadır `SourceLanguageConfig` . Ardından, `SourceLanguageConfig` Yapı için bir parametre olarak geçirilir `SpeechRecognizer` .

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak sağlanır `SourceLanguageConfig` . Ardından, `SourceLanguageConfig` Yapı için bir parametre olarak geçirilir `SpeechRecognizer` .

```Python
source_language_config = speechsdk.languageconfig.SourceLanguageConfig("de-DE", "The Endpoint ID for your custom model.")
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, source_language_config=source_language_config, audio_config=audio_config)
```

>[!Note]
> `speech_recognition_language` ve `endpoint_id` Özellikler `SpeechConfig` Python 'daki sınıftan kullanımdan kaldırılmıştır. Bu özelliklerin kullanımı önerilmez ve bir oluştururken kullanılmamalıdır `SpeechRecognizer` .

::: zone-end

::: zone pivot="programming-language-more"

## <a name="how-to-specify-source-language-in-javascript"></a>JavaScript 'te kaynak dilini belirtme

İlk adım `SpeechConfig` şunları oluşturmaktır:

```Javascript
var speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionkey", "YourRegion");
```

Sonra, sesinizin kaynak dilini belirtin `speechRecognitionLanguage` :

```Javascript
speechConfig.speechRecognitionLanguage = "de-DE";
```

Tanıma için özel bir model kullanıyorsanız, uç noktayı şu şekilde belirtebilirsiniz `endpointId` :

```Javascript
speechConfig.endpointId = "The Endpoint ID for your custom model.";
```

## <a name="how-to-specify-source-language-in-objective-c"></a>Amaç-C ' d e kaynak dilini belirtme

Bu örnekte, kaynak dili açıkça yapı kullanılarak bir parametre olarak sağlanır `SPXSpeechRecognizer` .

```Objective-C
SPXSpeechRecognizer* speechRecognizer = \
    [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig language:@"de-DE" audioConfiguration:audioConfig];
```

Bu örnekte, kaynak dili kullanılarak sunulmaktadır `SPXSourceLanguageConfiguration` . Ardından, `SPXSourceLanguageConfiguration` Yapı için bir parametre olarak geçirilir `SPXSpeechRecognizer` .

```Objective-C
SPXSourceLanguageConfiguration* sourceLanguageConfig = [[SPXSourceLanguageConfiguration alloc]init:@"de-DE"];
SPXSpeechRecognizer* speechRecognizer = [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                                                                     sourceLanguageConfiguration:sourceLanguageConfig
                                                                              audioConfiguration:audioConfig];
```

Bu örnekte, kaynak dili ve özel uç nokta kullanılarak sağlanır `SPXSourceLanguageConfiguration` . Ardından, `SPXSourceLanguageConfiguration` Yapı için bir parametre olarak geçirilir `SPXSpeechRecognizer` .

```Objective-C
SPXSourceLanguageConfiguration* sourceLanguageConfig = \
        [[SPXSourceLanguageConfiguration alloc]initWithLanguage:@"de-DE"
                                                     endpointId:@"The Endpoint ID for your custom model."];
SPXSpeechRecognizer* speechRecognizer = [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                                                                     sourceLanguageConfiguration:sourceLanguageConfig
                                                                              audioConfiguration:audioConfig];
```

>[!Note]
> `speechRecognitionLanguage` ve `endpointId` Özellikleri, `SPXSpeechConfiguration` hedef-C ' deki sınıfından kullanım dışıdır. Bu özelliklerin kullanımı önerilmez ve bir oluştururken kullanılmamalıdır `SPXSpeechRecognizer` .

::: zone-end

## <a name="see-also"></a>Ayrıca bkz.

* Konuşma için desteklenen dillerin ve yerel ayarların listesi için bkz. [dil desteği](language-support.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma SDK başvuru belgeleri](speech-sdk.md)