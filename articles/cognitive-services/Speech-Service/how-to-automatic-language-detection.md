---
title: Konuşmayı metne dönüştürme için otomatik dil algılamayı kullanma
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'Sı, konuşmadan metne yönelik otomatik dil algılamayı destekler. Bu özellik kullanılırken, belirtilen ses, belirtilen dil listesi ile karşılaştırılır ve en büyük olasılıkla eşleşme belirlenir. Döndürülen değer daha sonra, konuşma için kullanılan dil modelini seçmek için kullanılabilir.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81402197"
---
# <a name="automatic-language-detection-for-speech-to-text"></a>Konuşmayı metne dönüştürme için otomatik dil algılama

Otomatik dil algılama, sunulan dillerin listesine kıyasla konuşma SDK 'sına geçirilen sesle ilgili en büyük olasılıkla eşleşmeyi saptamak için kullanılır. Otomatik dil algılama tarafından döndürülen değer daha sonra konuşmayı metne dönüştürme için dil modelini seçmek ve daha doğru bir döküm sağlamak için kullanılır. Hangi dillerin kullanılabilir olduğunu görmek için bkz. [dil desteği](language-support.md).

Bu makalede, bir `AutoDetectSourceLanguageConfig` `SpeechRecognizer` nesnesi oluşturmak ve algılanan dili almak için kullanmayı öğreneceksiniz.

> [!IMPORTANT]
> Bu özellik yalnızca C#, C++, Java ve Python için konuşma SDK 'Sı için kullanılabilir.

## <a name="automatic-language-detection-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile otomatik dil algılama

Otomatik dil algılama 'nın Şu anda algılama başına iki dilin hizmet tarafı sınırı vardır. `AudoDetectSourceLanguageConfig` Nesnenizin oluşturulması sırasında bu sınırlamayı aklınızda bulundurun. Aşağıdaki örneklerde bir oluşturacak `AutoDetectSourceLanguageConfig`ve bunu bir `SpeechRecognizer`oluşturmak için kullanacaksınız.

> [!TIP]
> Ayrıca, metinde konuşma yaparken kullanılacak özel bir model de belirtebilirsiniz. Daha fazla bilgi için bkz. [Otomatik dil algılama için özel model kullanma](#use-a-custom-model-for-automatic-language-detection).

Aşağıdaki kod parçacıkları, uygulamalarınızda otomatik dil algılamayı nasıl kullanacağınızı göstermektedir:

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

## <a name="use-a-custom-model-for-automatic-language-detection"></a>Otomatik dil algılama için özel model kullanma

Konuşma hizmeti modellerini kullanarak dil algılamayı ayarlamanın yanı sıra, gelişmiş bir tanıma için özel bir model de belirtebilirsiniz. Özel bir model sağlanmazsa, hizmet varsayılan dil modelini kullanacaktır.

Aşağıdaki kod parçacıkları, konuşma hizmeti çağrınızla özel bir modelin nasıl belirtildiğini gösterir. Algılanan dil ise `en-US`, varsayılan model kullanılır. Algılanan dil ise `fr-FR`, özel model için uç nokta kullanılır:

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

- [Konuşma SDK başvuru belgeleri](speech-sdk.md)
