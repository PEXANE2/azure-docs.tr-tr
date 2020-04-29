---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 21c4f88083fab28ae132379eae1b5d8a2afa5d9f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422246"
---
Konuşma hizmetine sıkıştırılmış bir ses biçiminde akış yapmak için, veya `PullAudioInputStream` `PushAudioInputStream`oluşturun. Daha sonra akış sınıfınızın `AudioConfig` bir örneğinden oluşturun ve akışın sıkıştırma biçimini belirtin.

Adlandırılmış `pushStream` bir giriş akışı sınıfınız olduğunu ve Opus/OGG kullandığınızı varsayalım. Kodunuz şöyle görünebilir:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

// ... omitted for brevity

var speechConfig =
    SpeechConfig.FromSubscription(
        "YourSubscriptionKey",
        "YourServiceRegion");

// Create an audio config specifying the compressed
// audio format and the instance of your input stream class.
var audioFormat =
    AudioStreamFormat.GetCompressedFormat(
        AudioStreamContainerFormat.OGG_OPUS);
var audioConfig =
    AudioConfig.FromStreamInput(
        pushStream,
        audioFormat);

using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
var result = await recognizer.RecognizeOnceAsync();

var text = result.Text;
```