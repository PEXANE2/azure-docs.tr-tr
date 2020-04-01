---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: dapine
ms.openlocfilehash: 02227a0d4b9bf436632c5179696228ae865ff866
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409639"
---
Sıkıştırılmış ses biçiminde Konuşma hizmetine akış `PullAudioInputStream` yapmak `PushAudioInputStream`için, oluştur veya. Ardından, akış `AudioConfig` sınıfınızın sıkıştırma biçimini belirten bir örneğini oluşturun.

OPUS/OGG adlı `pushStream` bir giriş akışı sınıfınız olduğunu varsayalım. Kodunuz şu şekilde görünebilir:

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