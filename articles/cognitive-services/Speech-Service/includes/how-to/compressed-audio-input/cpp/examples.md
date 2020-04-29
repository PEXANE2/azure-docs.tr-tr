---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: b565bf3bae61cf92133b957bb75fdae9545c030e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422045"
---
Konuşma hizmetine sıkıştırılmış bir ses biçiminde akış yapmak için, veya `PullAudioInputStream` `PushAudioInputStream`oluşturun. Daha sonra akış sınıfınızın `AudioConfig` bir örneğinden oluşturun ve akışın sıkıştırma biçimini belirtin.

Adlandırılmış `pushStream` bir giriş akışı sınıfınız olduğunu ve Opus/OGG kullandığınızı varsayalım. Kodunuz şöyle görünebilir:

```cpp
using namespace Microsoft::CognitiveServices::Speech;
using namespace Microsoft::CognitiveServices::Speech::Audio;

// ... omitted for brevity

 auto config =
    SpeechConfig::FromSubscription(
        "YourSubscriptionKey",
        "YourServiceRegion"
    );

auto audioFormat =
    AudioStreamFormat::GetCompressedFormat(
        AudioStreamContainerFormat::OGG_OPUS
    );
auto audioConfig =
    AudioConfig::FromStreamInput(
        pushStream,
        audioFormat
    );

auto recognizer = SpeechRecognizer::FromConfig(config, audioConfig);
auto result = recognizer->RecognizeOnceAsync().get();

auto text = result->Text;
```