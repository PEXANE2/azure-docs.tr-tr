---
title: Konuşmayı metne dönüştürme için otomatik dil algılamayı kullanma
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'Sı, konuşmadan metne yönelik otomatik dil algılamayı destekler. Bu özellik kullanılırken, belirtilen ses, belirtilen dil listesi ile karşılaştırılır ve en büyük olasılıkla eşleşme belirlenir. Döndürülen değer daha sonra, konuşma için kullanılan dil modelini seçmek için kullanılabilir.
services: cognitive-services
author: susanhu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: qiohu
zone_pivot_groups: programming-languages-set-seven
ms.openlocfilehash: bc438c3e606fefc10e9ffbb64c79f7167d9af062
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122058"
---
# <a name="automatic-language-detection-for-speech-to-text"></a>Konuşmayı metne dönüştürme için otomatik dil algılama

Otomatik dil algılama, sunulan dillerin listesine kıyasla konuşma SDK 'sına geçirilen sesle ilgili en büyük olasılıkla eşleşmeyi saptamak için kullanılır. Otomatik dil algılama tarafından döndürülen değer daha sonra konuşmayı metne dönüştürme için dil modelini seçmek ve daha doğru bir döküm sağlamak için kullanılır. Hangi dillerin kullanılabilir olduğunu görmek için bkz. [dil desteği](language-support.md).

Bu makalede, `SpeechRecognizer` nesnesini oluşturmak ve algılanan dili almak için `AutoDetectSourceLanguageConfig` kullanmayı öğreneceksiniz.

> [!IMPORTANT]
> Bu özellik yalnızca, C# C++ ve Java için konuşma SDK 'sı için kullanılabilir.

## <a name="automatic-language-detection-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile otomatik dil algılama

Otomatik dil algılama 'nın Şu anda algılama başına iki dilin hizmet tarafı sınırı vardır. `AudoDetectSourceLanguageConfig` nesneniz oluşturulurken bu sınırlamayı aklınızda bulundurun. Aşağıdaki örneklerde bir `AutoDetectSourceLanguageConfig`oluşturup `SpeechRecognizer`oluşturmak için kullanacaksınız.

> [!TIP]
> Ayrıca, metinde konuşma yaparken kullanılacak özel bir model de belirtebilirsiniz. Daha fazla bilgi için bkz. [Otomatik dil algılama için özel model kullanma](#use-a-custom-model-for-automatic-language-detection).

Aşağıdaki kod parçacıkları, uygulamalarınızda otomatik dil algılamayı nasıl kullanacağınızı göstermektedir:

::: zone pivot="programming-language-csharp"

```csharp
var autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.FromLanguages(new string[] { "en-US", "de-DE" });
using (var recognizer = new SpeechRecognizer(speechConfig, autoDetectSourceLanguageConfig, audioConfig))
{
    var speechRecognitionResult = await recognizer.RecognizeOnceAsync();
    var autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult.FromResult(speechRecognitionResult);
    var detectedLanguage = autoDetectSourceLanguageResult.Language;
}
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
auto autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig::FromLanguages({ "en-US", "de-DE" });
auto recognizer = SpeechRecognizer::FromConfig(speechConfig, autoDetectSourceLanguageConfig, audioConfig);
speechRecognitionResult = recognizer->RecognizeOnceAsync().get();
auto autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult::FromResult(speechRecognitionResult);
auto detectedLanguage = autoDetectSourceLanguageResult->Language;
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.fromLanguages(Arrays.asList("en-US", "de-DE"));
SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, autoDetectSourceLanguageConfig, audioConfig);
Future<SpeechRecognitionResult> future = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = future.get(30, TimeUnit.SECONDS);
AutoDetectSourceLanguageResult autoDetectSourceLanguageResult = AutoDetectSourceLanguageResult.fromResult(result);
String detectedLanguage = autoDetectSourceLanguageResult.getLanguage();

recognizer.close();
speechConfig.close();
autoDetectSourceLanguageConfig.close();
audioConfig.close();
result.close();
```

::: zone-end

## <a name="use-a-custom-model-for-automatic-language-detection"></a>Otomatik dil algılama için özel model kullanma

Konuşma hizmeti modellerini kullanarak dil algılamayı ayarlamanın yanı sıra, gelişmiş bir tanıma için özel bir model de belirtebilirsiniz. Özel bir model sağlanmazsa, hizmet varsayılan dil modelini kullanacaktır.

Aşağıdaki kod parçacıkları, konuşma hizmeti çağrınızla özel bir modelin nasıl belirtildiğini gösterir. Algılanan dil `en-US`, varsayılan model kullanılır. Algılanan dil `fr-FR`, özel model için uç nokta kullanılır:

::: zone pivot="programming-language-csharp"

```csharp
var sourceLanguageConfigs = new SourceLanguageConfig[]
{
    SourceLanguageConfig.FromLanguage("en-US"),
    SourceLanguageConfig.FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR")
};
var autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.FromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
std::vector<std::shared_ptr<SourceLanguageConfig>> sourceLanguageConfigs;
sourceLanguageConfigs.push_back(SourceLanguageConfig::FromLanguage("en-US"));
sourceLanguageConfigs.push_back(SourceLanguageConfig::FromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));
auto autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig::FromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

::: zone pivot="programming-language-java"

```Java
List sourceLanguageConfigs = new ArrayList<SourceLanguageConfig>();
sourceLanguageConfigs.add(SourceLanguageConfig.fromLanguage("en-US"));
sourceLanguageConfigs.add(SourceLanguageConfig.fromLanguage("fr-FR", "The Endpoint Id for custom model of fr-FR"));
AutoDetectSourceLanguageConfig autoDetectSourceLanguageConfig = AutoDetectSourceLanguageConfig.fromSourceLanguageConfigs(sourceLanguageConfigs);
```

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma SDK başvuru belgeleri](speech-sdk.md)
