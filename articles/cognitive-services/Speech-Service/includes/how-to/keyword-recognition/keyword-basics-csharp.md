---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 01/04/2021
ms.author: trbye
ms.openlocfilehash: 4ec9c847158c9b8a1160cb6f4510912ed83a9fae
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98948645"
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

Konuşmayı metne dönüştürme için [hızlı başlangıç](../../../get-started-speech-to-text.md?pivots=programming-language-csharp&tabs=script%2cbrowser%2cwindowsinstall#continuous-recognition) bölümünde gösterilen tasarım deseninin aynısını izleyerek sürekli tanımayı ayarlayın. Sonra, ile olan çağrısını değiştirin `recognizer.StartContinuousRecognitionAsync()` `recognizer.StartKeywordRecognitionAsync(KeywordRecognitionModel)` ve `KeywordRecognitionModel` nesneniz geçirin. Anahtar sözcük biriktirimiyle sürekli tanımayı durdurmak için yerine kullanın `recognizer.StopKeywordRecognitionAsync()` `recognizer.StopContinuousRecognitionAsync()` .

Amaç tanıma, ve işlevleri ile özdeş bir model kullanır [`StartKeywordRecognitionAsync`](/dotnet/api/microsoft.cognitiveservices.speech.intent.intentrecognizer.startkeywordrecognitionasync#Microsoft_CognitiveServices_Speech_Intent_IntentRecognizer_StartKeywordRecognitionAsync_Microsoft_CognitiveServices_Speech_KeywordRecognitionModel_) [`StopKeywordRecognitionAsync`](/dotnet/api/microsoft.cognitiveservices.speech.intent.intentrecognizer.stopkeywordrecognitionasync#Microsoft_CognitiveServices_Speech_Intent_IntentRecognizer_StopKeywordRecognitionAsync) .
