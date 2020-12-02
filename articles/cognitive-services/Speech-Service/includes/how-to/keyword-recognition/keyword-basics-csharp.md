---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 11/03/2020
ms.author: trbye
ms.openlocfilehash: d71a7f6451cda5e2e50b5410140ac88361bf1735
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509517"
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
> Burada gösterilen örnek, `SpeechConfig` kimlik doğrulama bağlamı için bir nesne gerektirmediğinden ve arka uca başvurmadığından yerel anahtar sözcük tanımayı kullanır. Ancak, her iki anahtar sözcük tanıma ve doğrulamayı [sürekli bir arka uç bağlantısı](../../../tutorial-voice-enable-your-bot-speech-sdk.md#view-the-source-code-that-enables-keyword)kullanarak çalıştırabilirsiniz.