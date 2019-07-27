---
title: Konuşma SDK-konuşma hizmeti ile sıkıştırılmış ses codec bileşeni
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'Sı ile Azure konuşma Hizmetleri 'ne sıkıştırılmış ses akışını nasıl sağlayacağınızı öğrenin. Linux için C++, C#ve Java için kullanılabilir.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: amishu
ms.openlocfilehash: b29b42dea9522526d49c1bda017a522855946def
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559553"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile codec ile sıkıştırılmış ses girişi kullanma

Konuşma SDK 'sının **sıkıştırılmış ses giriş akışı** API 'si, sıkıştırılmış ses ' i, pullstream veya PushStream kullanarak konuşma hizmetine akışa almanın bir yolunu sağlar.

> [!IMPORTANT]
> Sıkıştırılmış ses, Linux üzerinde yalnızca, C++ C#ve Java (Ubuntu 16,04, Ubuntu 18,04, de, 9) için desteklenir.
> Konuşma SDK sürümü 1.4.0 veya üzeri gereklidir.

WAV/PCM için ana hat konuşma belgelerine bakın.  WAV/PCM dışında, aşağıdaki codec sıkıştırılmış giriş biçimleri desteklenir:

- MP3
- OPUS/OGG

## <a name="prerequisites-to-using-codec-compressed-audio-input"></a>Codec ile sıkıştırılmış ses girişini kullanma önkoşulları

Linux için konuşma SDK 'Sı ile sıkıştırılmış ses girişi kullanmak için bu ek bağımlılıkları yükler:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>Codec ile sıkıştırılmış ses girişini kullanan örnek kod

Konuşma hizmetlerine sıkıştırılmış bir ses biçiminde akış yapmak için, veya `PullAudioInputStream` `PushAudioInputStream`oluşturun. Daha sonra akış sınıfınızın `AudioConfig` bir örneğinden oluşturun ve akışın sıkıştırma biçimini belirtin.

Adlandırılmış `myPushStream` bir giriş akışı sınıfınız olduğunu ve Opus/OGG kullandığınızı varsayalım. Kodunuz şöyle görünebilir:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
var audioFormat = AudioStreamFormat.GetCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
var audioConfig = AudioConfig.FromStreamInput(myPushStream, audioFormat);

var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

var result = await recognizer.RecognizeOnceAsync();

var text = result.GetText();
```

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
