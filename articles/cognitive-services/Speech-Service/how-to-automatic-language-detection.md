---
title: Metinden metne konuşma için otomatik dil algılama nasıl kullanılır?
titleSuffix: Azure Cognitive Services
description: Konuşma SDK metin konuşma için otomatik dil algılama destekler. Bu özelliği kullanırken, sağlanan ses verilen dil listesiyle karşılaştırılır ve en olası eşleşme belirlenir. Döndürülen değer daha sonra metin konuşma için kullanılan dil modelini seçmek için kullanılabilir.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: fefbe793fa4a6b90ba9bf8d468d42dcbd315759c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402197"
---
# <a name="automatic-language-detection-for-speech-to-text"></a>Metinden metne konuşma için otomatik dil algılama

Otomatik dil algılama, sağlanan dillerin listesiyle karşılaştırıldığında Konuşma SDK'sına aktarılan ses için en olası eşleşmeyi belirlemek için kullanılır. Otomatik dil algılama ile döndürülen değer, daha doğru bir transkripsiyon sağlayarak, metin konuşma için dil modelini seçmek için kullanılır. Hangi dillerin kullanılabildiği hakkında [bkz.](language-support.md)

Bu makalede, bir `AutoDetectSourceLanguageConfig` `SpeechRecognizer` nesne oluşturmak ve algılanan dili almak için nasıl kullanılacağını öğreneceksiniz.

> [!IMPORTANT]
> Bu özellik yalnızca C#, C++, Java ve Python için SDK Speech için kullanılabilir.

## <a name="automatic-language-detection-with-the-speech-sdk"></a>Konuşma SDK ile otomatik dil algılama

Otomatik dil algılama şu anda algılama başına iki dil hizmet tarafı sınırı vardır. Nesnenizi oluştururken bu `AudoDetectSourceLanguageConfig` sınırlamayı aklınızda bulundurun. Aşağıdaki örneklerde, bir `AutoDetectSourceLanguageConfig`, sonra oluşturmak `SpeechRecognizer`için kullanabilirsiniz .

> [!TIP]
> Metin konuşma yaparken kullanılacak özel bir model de belirtebilirsiniz. Daha fazla bilgi için [bkz.](#use-a-custom-model-for-automatic-language-detection)

Aşağıdaki parçacıklar uygulamalarınızda otomatik dil algılamanın nasıl kullanılacağını gösterir:

::: zone pivot="programming-language-csharp"

```csharp
var autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig.FromLanguages(
        new string[] { "en-US", "de-DE" });

using (var recognizer = new SpeechRecognizer(
    speechConfig,
    autoDetectSourceLanguageConfig,
    audioConfig))
{
    var speechRecognitionResult = await recognizer.RecognizeOnceAsync();
    var autoDetectSourceLanguageResult =
        AutoDetectSourceLanguageResult.FromResult(speechRecognitionResult);
    var detectedLanguage = autoDetectSourceLanguageResult.Language;
}
```

::: zone-end

::: zone pivot="programming-language-cpp"

```cpp
auto autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig::FromLanguages({ "en-US", "de-DE" });

auto recognizer = SpeechRecognizer::FromConfig(
    speechConfig,
    autoDetectSourceLanguageConfig,
    audioConfig);

speechRecognitionResult = recognizer->RecognizeOnceAsync().get();
auto autoDetectSourceLanguageResult =
    AutoDetectSourceLanguageResult::FromResult(speechRecognitionResult);
auto detectedLanguage = autoDetectSourceLanguageResult->Language;
```

::: zone-end

::: zone pivot="programming-language-java"

```java
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig.fromLanguages(Arrays.asList("en-US", "de-DE"));

SpeechRecognizer recognizer = new SpeechRecognizer(
    speechConfig,
    autoDetectSourceLanguageConfig,
    audioConfig);

Future<SpeechRecognitionResult> future = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = future.get(30, TimeUnit.SECONDS);
AutoDetectSourceLanguageResult autoDetectSourceLanguageResult =
    AutoDetectSourceLanguageResult.fromResult(result);
String detectedLanguage = autoDetectSourceLanguageResult.getLanguage();

recognizer.close();
speechConfig.close();
autoDetectSourceLanguageConfig.close();
audioConfig.close();
result.close();
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
auto_detect_source_language_config = \
        speechsdk.languageconfig.AutoDetectSourceLanguageConfig(languages=["en-US", "de-DE"])
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, 
        auto_detect_source_language_config=auto_detect_source_language_config, 
        audio_config=audio_config)
result = speech_recognizer.recognize_once()
auto_detect_source_language_result = speechsdk.AutoDetectSourceLanguageResult(result)
detected_language = auto_detect_source_language_result.language
```

::: zone-end

## <a name="use-a-custom-model-for-automatic-language-detection"></a>Otomatik dil algılama için özel bir model kullanma

Konuşma hizmeti modellerini kullanarak dil algılamaya ek olarak, gelişmiş tanıma için özel bir model belirtebilirsiniz. Özel bir model sağlanmadıysa, hizmet varsayılan dil modelini kullanır.

Aşağıdaki parçacıklar, Konuşma hizmetine yapılan çağrınızda özel bir modelin nasıl belirtilen olduğunu göstermektedir. Algılanan dil `en-US`ise, varsayılan model kullanılır. Algılanan dil `fr-FR`ise, özel model için bitiş noktası kullanılır:

::: zone pivot="programming-language-csharp"

```csharp
var sourceLanguageConfigs = new SourceLanguageConfig[]
{
    SourceLanguageConfig.FromLanguage("en-US"),
    SourceLanguageConfig.FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR")
};
var autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig.FromSourceLanguageConfigs(
        sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-cpp"

```cpp
std::vector<std::shared_ptr<SourceLanguageConfig>> sourceLanguageConfigs;
sourceLanguageConfigs.push_back(
    SourceLanguageConfig::FromLanguage("en-US"));
sourceLanguageConfigs.push_back(
    SourceLanguageConfig::FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));

auto autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig::FromSourceLanguageConfigs(
        sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-java"

```java
List sourceLanguageConfigs = new ArrayList<SourceLanguageConfig>();
sourceLanguageConfigs.add(
    SourceLanguageConfig.fromLanguage("en-US"));
sourceLanguageConfigs.add(
    SourceLanguageConfig.fromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));

AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig =
    AutoDetectSourceLanguageConfig.fromSourceLanguageConfigs(
        sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
 en_language_config = speechsdk.languageconfig.SourceLanguageConfig("en-US")
 fr_language_config = speechsdk.languageconfig.SourceLanguageConfig("fr-FR", "The Endpoint Id for custom model of fr-FR")
 auto_detect_source_language_config = speechsdk.languageconfig.AutoDetectSourceLanguageConfig(
        sourceLanguageConfigs=[en_language_config, fr_language_config])
```

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma SDK referans belgeleri](speech-sdk.md)
