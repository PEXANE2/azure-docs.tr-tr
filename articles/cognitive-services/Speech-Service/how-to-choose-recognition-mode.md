---
title: Konuşma SDK ile konuşma tanıma modu seçin
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'sını kullanırken en iyi tanıma modunu nasıl seçeceğinizi öğrenin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 5fdca371e9188ef69068ddbcaa416cbb2b44054c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402146"
---
# <a name="choose-a-speech-recognition-mode"></a>Konuşma tanıma modu nu seçin

Konuşmadan metne tanıma işlemleri göz önüne alındığında, [Konuşma SDK](speech-sdk.md) konuşma işleme için birden çok mod sağlar. Kavramsal olarak, bazen *tanıma modu*denir. Bu makalede, çeşitli tanıma modları karşılaştırılır.

## <a name="recognize-once"></a>Bir kez tanıyın

Her söyleyişi bir defada bir "cümle" olarak işlemek istiyorsanız, "bir kez tanı" işlevini kullanın. Bu yöntem, algılanan konuşmanın başlangıcından bir sonraki duraklamaya kadar başlayarak girişten tanınan bir söylemi algılar. Genellikle, bir duraklama bir cümlenin veya düşünce çizgisinin sonunu işaretler.

Tanınan bir sözcük sonunda, hizmet bu istekten gelen sesi işlemeyi durdurur. Tanıma için maksimum sınır 20 saniyelik bir ceza süresidir.

::: zone pivot="programming-language-csharp"

İşlevin `RecognizeOnceAsync` kullanımı hakkında daha fazla bilgi için [.NET Speech SDK dokümanlarına](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_SpeechRecognizer_RecognizeOnceAsync)bakın.

```csharp
var result = await recognizer.RecognizeOnceAsync();
```

::: zone-end
::: zone pivot="programming-language-cpp"

İşlevin `RecognizeOnceAsync` kullanımı hakkında daha fazla bilgi için [C++ Konuşma SDK dokümanlarına](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#recognizeonceasync)bakın.

```cpp
auto result = recognize->RecognizeOnceAsync().get();
```

::: zone-end
::: zone pivot="programming-language-java"

İşlevin `recognizeOnceAsync` kullanımı hakkında daha fazla bilgi için [Java Konuşma SDK dokümanlarına](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.SpeechRecognizer.recognizeOnceAsync?view=azure-java-stable)bakın.

```java
SpeechRecognitionResult result = recognizer.recognizeOnceAsync().get();
```

::: zone-end
::: zone pivot="programming-language-python"

İşlevin `recognize_once` kullanımı hakkında daha fazla bilgi için [Python Speech SDK dokümanlarına](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python#recognize-once------azure-cognitiveservices-speech-speechrecognitionresult)bakın.

```python
result = speech_recognizer.recognize_once()
```

::: zone-end
::: zone pivot="programming-language-more"

Ek diller için [Konuşma SDK başvuru dokümanlarına](speech-to-text.md#speech-sdk-reference-docs)bakın.

::: zone-end

## <a name="continuous"></a>Sürekli

Uzun süreli tanımaya ihtiyacınız varsa, sürekli tanıma için başlangıç ve karşılık gelen durdurma işlevlerini kullanın. Başlatma işlevi, durdurma işlevini çağırana veya sessizlikte çok fazla zaman geçene kadar tüm tüm söyleyişleri başlatAcak ve işlemeye devam edecektir. Sürekli modu kullanırken, meydana geldiğinde ortaya çıkacak çeşitli olaylara kaydolduğunuzdan emin olun. Örneğin, konuşma tanıma gerçekleştiğinde "tanınan" olay yangınları. Tanımayı işlemek için bir olay işleyicisi olması gerekir.

::: zone pivot="programming-language-csharp"

```csharp
// Subscribe to event
recognizer.Recognized += (s, e) => 
{
    if (e.Result.Reason == ResultReason.RecognizedSpeech)
    {
        // Do something with the recognized text
        // e.Result.Text
    }
};

// Start continuous speech recognition
await recognizer.StartContinuousRecognitionAsync();

// Stop continuous speech recognition
await recognizer.StopContinuousRecognitionAsync();
```

::: zone-end
::: zone pivot="programming-language-cpp"

```cpp
// Connect to event
recognizer->Recognized.Connect([] (const SpeechRecognitionEventArgs& e)
{
    if (e.Result->Reason == ResultReason::RecognizedSpeech)
    {
        // Do something with the recognized text
        // e.Result->Text
    }
});

// Start continuous speech recognition
recognizer->StartContinuousRecognitionAsync().get();

// Stop continuous speech recognition
recognizer->StopContinuousRecognitionAsync().get();
```

::: zone-end
::: zone pivot="programming-language-java"

```java
recognizer.recognized.addEventListener((s, e) -> {
    if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
        // Do something with the recognized text
        // e.getResult().getText()
    }
});

// Start continuous speech recognition
recognizer.startContinuousRecognitionAsync().get();

// Stop continuous speech recognition
recognizer.stopContinuousRecognitionAsync().get();
```

::: zone-end
::: zone pivot="programming-language-python"

```python
def recognized_cb(evt):
    if evt.result.reason == speechsdk.ResultReason.RecognizedSpeech:
        # Do something with the recognized text
        # evt.result.text

speech_recognizer.recognized.connect(recognized_cb)

# Start continuous speech recognition
speech_recognizer.start_continuous_recognition()

# Stop continuous speech recognition
speech_recognizer.stop_continuous_recognition()
```

::: zone-end
::: zone pivot="programming-language-more"

Ek diller için [Konuşma SDK başvuru dokümanlarına](speech-to-text.md#speech-sdk-reference-docs)bakın.

::: zone-end

## <a name="dictation"></a>Dikte

Sürekli tanıma kullanırken, ilgili "dikte işlemini etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, noktalama işaretleri gibi tümce yapılarının sözcük açıklamalarını yorumlamak için konuşma config örneğine neden olur. Örneğin, "Şehirde mi yaşıyorsunuz soru işareti" sözü "Şehirde mi yaşıyorsunuz?" şeklinde yorumlanır.

::: zone pivot="programming-language-csharp"

İşlevin `EnableDictation` kullanımı hakkında daha fazla bilgi için [.NET Speech SDK dokümanlarına](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-dotnet#Microsoft_CognitiveServices_Speech_SpeechConfig_EnableDictation)bakın.

```csharp
// Enable diction
SpeechConfig.EnableDictation();
```

::: zone-end
::: zone pivot="programming-language-cpp"

İşlevin `EnableDictation` kullanımı hakkında daha fazla bilgi için [C++ Konuşma SDK dokümanlarına](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#enabledictation)bakın.

```cpp
// Enable diction
SpeechConfig->EnableDictation();
```

::: zone-end
::: zone pivot="programming-language-java"

İşlevin `enableDictation` kullanımı hakkında daha fazla bilgi için [Java Konuşma SDK dokümanlarına](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.SpeechConfig.enableDictation?view=azure-java-stable)bakın.

```java
// Enable diction
SpeechConfig.enableDictation();
```

::: zone-end
::: zone pivot="programming-language-python"

İşlevin `enable_dictation` kullanımı hakkında daha fazla bilgi için [Python Speech SDK dokümanlarına](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#enable-dictation--)bakın.

```python
# Enable diction
SpeechConfig.enable_dictation()
```

::: zone-end
::: zone pivot="programming-language-more"

Ek diller için [Konuşma SDK başvuru dokümanlarına](speech-to-text.md#speech-sdk-reference-docs)bakın.

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub'da ek Konuşma SDK örneklerini keşfedin](https://aka.ms/csspeech/samples)
