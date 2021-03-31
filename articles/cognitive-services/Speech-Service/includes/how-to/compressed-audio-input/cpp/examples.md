---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: ad32204739d728006362ef55657a2f433be7aefc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "97978877"
---
Konuşma SDK 'sını sıkıştırılmış ses girişini kabul edecek şekilde yapılandırmak için, `PullAudioInputStream` veya oluşturun `PushAudioInputStream` . Daha sonra akış `AudioConfig` sınıfınızın bir örneğinden oluşturun ve akışın sıkıştırma biçimini belirtin.

Adlandırılmış bir giriş akışı sınıfınız olduğunu `pushStream` ve Opus/OGG kullandığınızı varsayalım. Kodunuz şöyle görünebilir:

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
