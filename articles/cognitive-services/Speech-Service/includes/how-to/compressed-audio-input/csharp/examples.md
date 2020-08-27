---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.custom: devx-track-csharp
ms.openlocfilehash: 313d2717e6e40280ed37322d695a2129d833dd59
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934115"
---
Konuşma hizmetine sıkıştırılmış bir ses biçiminde akış yapmak için, `PullAudioInputStream` veya oluşturun `PushAudioInputStream` . Daha sonra akış `AudioConfig` sınıfınızın bir örneğinden oluşturun ve akışın sıkıştırma biçimini belirtin.

Adlandırılmış bir giriş akışı sınıfınız olduğunu `pushStream` ve Opus/OGG kullandığınızı varsayalım. Kodunuz şöyle görünebilir:

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