---
title: Telaffuz değerlendirmesi için konuşma SDK 'sını kullanma
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'Sı, konuşma girişi değerlendirir, doğruluk, akıcı, tamamlayıcı, vb. göstergelerini içeren telaffuz değerlendirmesini destekler.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/29/2020
ms.author: yulili
ms.custom: references_regions
zone_pivot_groups: programming-languages-set-nineteen
ms.openlocfilehash: 245a00acb07d1c0e769a243413fccdf64d544f5a
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133778"
---
# <a name="pronunciation-assessment"></a>Telaffuz değerlendirmesi

Söyleniş değerlendirmesi konuşma söylenişini değerlendirir ve konuşmayla ilgili doğruluk ve akıcı bir ses elde etmenizi sağlar.
Telaffuz değerlendirmesi sayesinde, konuşma, anında geri bildirim alabilir ve telaffuz hale gelecek ve güvenle sunabilmeleri için söylenişlerini geliştirebilirler.
Eğitimciler, birden çok konuşmacıdaki söylenişi gerçek zamanlı olarak değerlendirmek için özelliğini kullanabilir.

Bu makalede, `PronunciationAssessmentConfig` `PronunciationAssessmentResult` konuşma SDK 'sını kullanarak ayarlamayı ve almayı öğreneceksiniz.

> [!NOTE]
> Telaffuz değerlendirmesi özelliği şu anda yalnızca bölgelerde mevcuttur ve `westus` `eastasia` `centralindia` yalnızca dili destekler `en-US` .

## <a name="pronunciation-assessment-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile telaffuz değerlendirmesi

Aşağıdaki örneklerde bir oluşturacak `PronunciationAssessmentConfig` ve ardından bir ' a uygulayacaksınız `SpeechRecognizer` .

Aşağıdaki kod parçacıkları, uygulamalarınızda otomatik dil algılamayı nasıl kullanacağınızı göstermektedir:

::: zone pivot="programming-language-csharp"

```csharp
var pronunciationAssessmentConfig = new PronunciationAssessmentConfig(
    "reference text", GradingSystem.HundredMark, Granularity.Phoneme);

using (var recognizer = new SpeechRecognizer(
    speechConfig,
    audioConfig))
{
    // apply the pronunciation assessment configuration to the speech recognizer
    pronunciationAssessmentConfig.ApplyTo(recognizer);
    var speechRecognitionResult = await recognizer.RecognizeOnceAsync();
    var pronunciationAssessmentResult =
        PronunciationAssessmentResult.FromResult(speechRecognitionResult);
    var pronunciationScore = pronunciationAssessmentResult.PronunciationScore;
}
```

::: zone-end

::: zone pivot="programming-language-cpp"

```cpp
auto pronunciationAssessmentConfig =
    PronunciationAssessmentConfig::Create("reference text",
        PronunciationAssessmentGradingSystem::HundredMark,
        PronunciationAssessmentGranularity::Phoneme);

auto recognizer = SpeechRecognizer::FromConfig(
    speechConfig,
    audioConfig);

// apply the pronunciation assessment configuration to the speech recognizer
pronunciationAssessmentConfig->ApplyTo(recognizer);
speechRecognitionResult = recognizer->RecognizeOnceAsync().get();
auto pronunciationAssessmentResult =
    PronunciationAssessmentResult::FromResult(speechRecognitionResult);
auto pronunciationScore = pronunciationAssessmentResult->PronunciationScore;
```

::: zone-end

::: zone pivot="programming-language-java"

```java
PronunciationAssessmentConfig pronunciationAssessmentConfig =
    new PronunciationAssessmentConfig("reference text", 
        PronunciationAssessmentGradingSystem.HundredMark,
        PronunciationAssessmentGranularity.Phoneme);

SpeechRecognizer recognizer = new SpeechRecognizer(
    speechConfig,
    audioConfig);

// apply the pronunciation assessment configuration to the speech recognizer
pronunciationAssessmentConfig.applyTo(recognizer);
Future<SpeechRecognitionResult> future = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = future.get(30, TimeUnit.SECONDS);
PronunciationAssessmentResult pronunciationAssessmentResult =
    PronunciationAssessmentResult.fromResult(result);
Double pronunciationScore = pronunciationAssessmentResult.getPronunciationScore();

recognizer.close();
speechConfig.close();
audioConfig.close();
pronunciationAssessmentConfig.close();
result.close();
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
pronunciation_assessment_config = \
        speechsdk.PronunciationAssessmentConfig(reference_text='reference text',
                grading_system=msspeech.PronunciationAssessmentGradingSystem.HundredMark,
                granularity=msspeech.PronunciationAssessmentGranularity.Phoneme)
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, \
        audio_config=audio_config)

# apply the pronunciation assessment configuration to the speech recognizer
pronunciation_assessment_config.apply_to(speech_recognizer)
result = speech_recognizer.recognize_once()
pronunciation_assessment_result = speechsdk.PronunciationAssessmentResult(result)
pronunciation_score = pronunciation_assessment_result.pronunciation_score
```

::: zone-end

::: zone pivot="programming-language-objectivec"

```Objective-C
SPXPronunciationAssessmentConfiguration* pronunciationAssessmentConfig =
    [[SPXPronunciationAssessmentConfiguration alloc]init:@"reference text"
                                           gradingSystem:SPXPronunciationAssessmentGradingSystem_HundredMark
                                             granularity:SPXPronunciationAssessmentGranularity_Phoneme];

SPXSpeechRecognizer* speechRecognizer = \
        [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                                              audioConfiguration:audioConfig];

// apply the pronunciation assessment configuration to the speech recognizer
[pronunciationAssessmentConfig applyToRecognizer:speechRecognizer];

SPXSpeechRecognitionResult *result = [speechRecognizer recognizeOnce];
SPXPronunciationAssessmentResult* pronunciationAssessmentResult = [[SPXPronunciationAssessmentResult alloc] init:result];
double pronunciationScore = pronunciationAssessmentResult.pronunciationScore;
```

::: zone-end

### <a name="pronunciation-assessment-configuration-parameters"></a>Telaffuz değerlendirmesi yapılandırma parametreleri

Bu tablo, telaffuz değerlendirmesi için yapılandırma parametrelerini listeler.

| Parametre | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| ReferenceText | Telaffuz tarafından değerlendirilecek metin. | Gerekli |
| GradingSystem | Puan ayarlaması için nokta sistemi. Kabul edilen değerler `FivePoint` şunlardır `HundredMark` . Varsayılan ayar `FivePoint` değeridir. | İsteğe Bağlı |
| Ayrıntı düzeyi | Değerlendirme ayrıntı düzeyi. Kabul edilen değerler, tam metin ve sözcük düzeyindeki puanı gösteren ve tam metin düzeyinde puan gösteren, `Phoneme` Word ve Fonem düzeyindeki puanı gösteren kabul edilir `Word` `FullText` . Varsayılan ayar `Phoneme` değeridir. | İsteğe Bağlı |
| EnableMiscue | Hatalı işaret hesaplamasını etkinleştirilir. Bu etkinken, bulunan sözcükler başvuru metniyle karşılaştırılır ve karşılaştırmaya göre atlama/ekleme ile işaretlenir. Kabul edilen değerler `False` şunlardır `True` . Varsayılan ayar `False` değeridir. | İsteğe Bağlı |
| ScenarioId | Özelleştirilmiş bir nokta sistemini gösteren bir GUID. | İsteğe Bağlı |

### <a name="pronunciation-assessment-result-parameters"></a>Söyleniş değerlendirmesi sonuç parametreleri

Bu tablo, Söyleniş değerlendirmesinin sonuç parametrelerini listeler.

| Parametre | Açıklama |
|-----------|-------------|
| `AccuracyScore` | Konuşmayı telaffuz doğruluğu. Doğruluk, alfabesine 'in yerel konuşmacı söylenişi ile ne kadar yakın olduğunu gösterir. Word ve tam metin düzeyi doğruluk puanı, Fonem düzeyi doğruluk puanı ' ndan toplanır. |
| `FluencyScore` | Verilen konuşmayı akıcı olarak. Akıcı bir şekilde, konuşmanın bir yerel konuşmacının sözcükler arasında sessiz kesmeler kullanımıyla ne kadar yakın olduğunu gösterir. |
| `CompletenessScore` | Sözcük girişine başvuru metin girişi olarak bulunan sözcüklerin oranının hesaplanmasıyla belirlenen konuşma. |
| `PronunciationScore` | Verilen konuşmayı gösteren telaffuz kalitesini belirten genel puan. Bu `AccuracyScore` , `FluencyScore` ve `CompletenessScore` ağırlığıyla toplanır. |
| `ErrorType` | Bu değer, ile karşılaştırıldığında bir sözcüğün atlanıp atlanmadığını, ekleneceğini veya hatalı bir şekilde olduğunu gösterir `ReferenceText` . Olası değerler şunlardır `None` (Yani bu sözcükte hata yok), `Omission` `Insertion` ve `Mispronunciation` . |

## <a name="next-steps"></a>Sonraki adımlar

<!-- TODO: update the sample links after release -->

<!-- ::: zone pivot="programming-language-csharp"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#L741) on GitHub for automatic language detection
::: zone-end

::: zone pivot="programming-language-cpp"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#L507) on GitHub for automatic language detection
::: zone-end

::: zone pivot="programming-language-java"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#L521) on GitHub for automatic language detection
::: zone-end

::: zone pivot="programming-language-python"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_synthesis_sample.py#L434) on GitHub for automatic language detection
::: zone-end

::: zone pivot="programming-language-objectivec"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/objective-c/ios/speech-samples/speech-samples/ViewController.m#L494) on GitHub for automatic language detection
::: zone-end -->

* [Konuşma SDK başvuru belgeleri](speech-sdk.md)
