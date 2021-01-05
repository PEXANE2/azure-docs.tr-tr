---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 01/04/2021
ms.author: trbye
ms.openlocfilehash: 786f9587ab223cf87a48cd791f366049b94af59b
ms.sourcegitcommit: aeba98c7b85ad435b631d40cbe1f9419727d5884
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97866054"
---
İlk olarak, `FromFile()` bir döndüren statik işlevini kullanarak anahtar sözcük modeli dosyanızı yükleyin `KeywordRecognitionModel` . `.table`Konuşma Studio 'dan indirdiğiniz dosyanın yolunu kullanın. Ayrıca, `AudioConfig` varsayılan mikrofonu kullanarak bir oluşturun ve ardından Ses Yapılandırması ' nı kullanarak yeni bir örneğini oluşturun `KeywordRecognizer` .

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var keywordModel = KeywordRecognitionModel.FromFile("your/path/to/Activate_device.table");
using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using var keywordRecognizer = new KeywordRecognizer(audioConfig);
```

Ardından, anahtar sözcük tanımayı çalıştırmak, `RecognizeOnceAsync()` model nesneniz geçirerek bir çağrısıyla yapılır. Bu, anahtar sözcük tanınana kadar olan bir anahtar sözcük tanıma oturumu başlatır. Bu nedenle, genellikle bu tasarım modelini çok iş parçacıklı uygulamalarda veya bir uyandırma sözcüğünün süresiz olarak bekleyebilirsiniz kullanım durumlarında kullanırsınız.

```csharp
KeywordRecognitionResult result = await keywordRecognizer.RecognizeOnceAsync(keywordModel);
```

> [!NOTE]
> Burada gösterilen örnek, `SpeechConfig` kimlik doğrulama bağlamı için bir nesne gerektirmediğinden ve arka uca başvurmadığından yerel anahtar sözcük tanımayı kullanır. Bununla birlikte, [doğrudan arka uç bağlantısı kullanan](../../../tutorial-voice-enable-your-bot-speech-sdk.md#view-the-source-code-that-enables-keyword)anahtar sözcük tanımayı ve doğrulamayı çalıştırabilirsiniz.

### <a name="continuous-recognition"></a>Sürekli tanıma

Konuşma SDK 'sında diğer sınıflar, anahtar sözcük tanıma özelliği ile sürekli tanımayı destekler (hem konuşma hem de amaç tanıma için). Bu, `.table` anahtar sözcük modelinize yönelik bir dosyaya başvuruda bulunmak için normalde sürekli tanıma için kullandığınız kodu kullanmanıza olanak sağlar.

Konuşmayı metne dönüştürme için [hızlı başlangıç](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started-speech-to-text?tabs=script%2Cbrowser%2Cwindowsinstall&pivots=programming-language-csharp#continuous-recognition) bölümünde gösterilen tasarım deseninin aynısını izleyerek sürekli tanımayı ayarlayın. Sonra, ile olan çağrısını değiştirin `recognizer.StartContinuousRecognitionAsync()` `recognizer.StartKeywordRecognitionAsync(KeywordRecognitionModel)` ve `KeywordRecognitionModel` nesneniz geçirin. Anahtar sözcük biriktirimiyle sürekli tanımayı durdurmak için yerine kullanın `recognizer.StopKeywordRecognitionAsync()` `recognizer.StopContinuousRecognitionAsync()` .

Amaç tanıma, ve işlevleri ile özdeş bir model kullanır [`StartKeywordRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.intent.intentrecognizer.startkeywordrecognitionasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_Intent_IntentRecognizer_StartKeywordRecognitionAsync_Microsoft_CognitiveServices_Speech_KeywordRecognitionModel_) [`StopKeywordRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.intent.intentrecognizer.stopkeywordrecognitionasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_Intent_IntentRecognizer_StopKeywordRecognitionAsync) .