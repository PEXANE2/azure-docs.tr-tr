---
title: Konuşma SDK 'Sı ile konuşma tanıma modunu seçme
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanırken en iyi tanıma modunu seçme hakkında bilgi edinin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: dapine
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 51bf005bdad4197120fed96894ac1cdd150738ee
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75935232"
---
# <a name="choose-a-speech-recognition-mode"></a>Konuşma tanıma modunu seçin

Konuşmayı metne dönüştürme işlemlerini değerlendirirken, [konuşma SDK 'sı](speech-sdk.md) , konuşmayı işlemek için birden çok mod sağlar. Kavramsal olarak bazen *Tanıma modu*olarak da adlandırılır. Bu makalede çeşitli tanıma modları karşılaştırılır.

## <a name="recognize-once"></a>Bir kez tanı

Her seferinde bir "cümleyi" tek seferde işlemek istiyorsanız, "bir kez tanı" işlevini kullanın. Bu yöntem, bir sonraki duraklatmaya kadar algılanan konuşmanın başlangıcında başlayan girişten tanınan bir şekilde algılar. Genellikle, bir, bir tümcenin veya bir düşünün sonunu işaret eden bir duraklama.

Tanınan bir zaman sonunda hizmet, bu istekten ses işlemeyi durduruyor. Maksimum tanıma sınırı 20 saniyelik bir cümle süresi olur.

::: zone pivot="programming-language-csharp"

`RecognizeOnceAsync` işlevini kullanma hakkında daha fazla bilgi için bkz. [.net konuşma SDK belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_SpeechRecognizer_RecognizeOnceAsync).

```csharp
var result = await recognizer.RecognizeOnceAsync().ConfigureAwait(false);
```

::: zone-end
::: zone pivot="programming-language-cpp"

`RecognizeOnceAsync` işlevini kullanma hakkında daha fazla bilgi için [ C++ konuşma SDK belgelerine](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#recognizeonceasync)bakın.

```cpp
auto result = recognize->RecognizeOnceAsync().get();
```

::: zone-end
::: zone pivot="programming-language-java"

`recognizeOnceAsync` işlevini kullanma hakkında daha fazla bilgi için bkz. [Java konuşma SDK belgeleri](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.SpeechRecognizer.recognizeOnceAsync?view=azure-java-stable).

```java
SpeechRecognitionResult result = recognizer.recognizeOnceAsync().get();
```

::: zone-end
::: zone pivot="programming-language-python"

`recognize_once` işlevini kullanma hakkında daha fazla bilgi için bkz. [Python konuşma SDK belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python#recognize-once------azure-cognitiveservices-speech-speechrecognitionresult).

```python
result = speech_recognizer.recognize_once()
```

::: zone-end
::: zone pivot="programming-language-more"

Ek diller için bkz. [konuşma SDK 'sı başvuru belgeleri](speech-to-text.md#speech-sdk-reference-docs).

::: zone-end

## <a name="continuous"></a>Sürekli

Uzun süre çalışan tanıma gerekirse, sürekli tanıma için başlangıç ve karşılık gelen durdurma işlevlerini kullanın. Başlat işlevi başlar ve durdurma işlevini çağırana veya sessizlik içinde çok fazla zamana kadar tüm eşyaları işlemeye devam eder. Sürekli modu kullanırken, oluşumdan sonra tetiklendirilecektir çeşitli olaylara kaydolduğunuzdan emin olun. Örneğin, "tanınan" olay konuşma tanıma oluştuğunda ateşlenir. Tanımayı işlemek için bir olay işleyiciniz olması gerekir. Konuşma hizmeti tarafından oturum başına toplam konuşma tanıma süresinin 10 dakikalık sınırı.

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
await recognizer.StartContinuousRecognitionAsync().ConfigureAwait(false);

// Stop continuous speech recognition
await recognizer.StopContinuousRecognitionAsync().ConfigureAwait(false);
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

Ek diller için bkz. [konuşma SDK 'sı başvuru belgeleri](speech-to-text.md#speech-sdk-reference-docs).

::: zone-end

## <a name="dictation"></a>Dikte Etme

Sürekli tanıma kullanırken, ilgili "dikte etmeyi etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, konuşma yapılandırma örneğinin noktalama gibi tümce yapılarının sözcük açıklamalarını yorumlamasını sağlar. Örneğin, "kasadaki gerçek zamanlı olarak", "kasadaki canlı mısınız?" metni olarak yorumlanabilir.

::: zone pivot="programming-language-csharp"

`EnableDictation` işlevini kullanma hakkında daha fazla bilgi için bkz. [.net konuşma SDK belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-dotnet#Microsoft_CognitiveServices_Speech_SpeechConfig_EnableDictation).

```csharp
// Enable diction
SpeechConfig.EnableDictation();
```

::: zone-end
::: zone pivot="programming-language-cpp"

`EnableDictation` işlevini kullanma hakkında daha fazla bilgi için [ C++ konuşma SDK belgelerine](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#enabledictation)bakın.

```cpp
// Enable diction
SpeechConfig->EnableDictation();
```

::: zone-end
::: zone pivot="programming-language-java"

`enableDictation` işlevini kullanma hakkında daha fazla bilgi için bkz. [Java konuşma SDK belgeleri](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.SpeechConfig.enableDictation?view=azure-java-stable).

```java
// Enable diction
SpeechConfig.enableDictation();
```

::: zone-end
::: zone pivot="programming-language-python"

`enable_dictation` işlevini kullanma hakkında daha fazla bilgi için bkz. [Python konuşma SDK belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#enable-dictation--).

```python
# Enable diction
SpeechConfig.enable_dictation()
```

::: zone-end
::: zone pivot="programming-language-more"

Ek diller için bkz. [konuşma SDK 'sı başvuru belgeleri](speech-to-text.md#speech-sdk-reference-docs).

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da ek konuşma SDK 'Sı örneklerini keşfet](https://aka.ms/csspeech/samples)
