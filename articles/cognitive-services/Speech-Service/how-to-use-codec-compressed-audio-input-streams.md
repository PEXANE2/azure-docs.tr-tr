---
title: Konuşma SDK-konuşma hizmeti ile sıkıştırılmış ses codec bileşeni
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'Sı ile konuşma hizmetine sıkıştırılmış ses akışını nasıl sağlayacağınızı öğrenin. Linux için C++, C#ve Java için, Android 'de Java ve amaç-C için kullanılabilir.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: amishu
ms.openlocfilehash: 2b530da06b02091ce66ff7c116f3e17ddcc22497
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78331119"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile codec ile sıkıştırılmış ses girişi kullanma

Konuşma SDK 'sının **sıkıştırılmış ses giriş akışı** API 'si, sıkıştırılmış ses ' i, pullstream veya PushStream kullanarak konuşma hizmetine akışa almanın bir yolunu sağlar.

> [!IMPORTANT]
> Linux (Ubuntu 16,04, Ubuntu 18,04, de, 9, RHEL 8, CentOS 8) üzerinde, C++ C#, ve Java için akış sıkıştırılmış giriş sesi şu anda desteklenmektedir. Ayrıca iOS platformunda Android ve [Amaç-C ' d a](how-to-use-codec-compressed-audio-input-streams-ios.md) [Java](how-to-use-codec-compressed-audio-input-streams-android.md) için de desteklenir.
> Konuşma SDK sürümü 1.7.0 veya üzeri gereklidir (RHEL 8, CentOS 8 için sürüm 1.10.0 veya üzeri).

WAV/PCM için ana hat konuşma belgelerine bakın.  WAV/PCM dışında, aşağıdaki codec sıkıştırılmış giriş biçimleri desteklenir:

- MP3
- OPUS/OGG
- FLAC
- WAV w kapsayıcısında
- WAV kapsayıcısında MULAW

## <a name="prerequisites"></a>Önkoşullar

Sıkıştırılmış ses işleme, [GStreamer](https://gstreamer.freedesktop.org)kullanılarak uygulanır. Lisanslama nedeni için GStreamer ikili dosyaları derlenmez ve konuşma SDK ile bağlantılı değildir. Bu nedenle, Application Developer 'ın sıkıştırılmış giriş sesini kullanabilmesi için 18,04, 16,04 ve de, 9 ' a, aşağıdakileri yüklemesi gerekir.

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

RHEL/CentOS 8 ' de:

```sh
sudo yum install gstreamer1 gstreamer1-plugins-base gstreamer1-plugins-good gstreamer1-plugins-bad-free gstreamer1-plugins-ugly-free
```

> [!NOTE]
> RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yönergelerini izleyin.

## <a name="example-code-using-codec-compressed-audio-input"></a>Codec ile sıkıştırılmış ses girişini kullanan örnek kod

Konuşma hizmetine sıkıştırılmış bir ses biçiminde akış yapmak için `PullAudioInputStream` veya `PushAudioInputStream`oluşturun. Sonra akış sınıfınızın bir örneğinden bir `AudioConfig` oluşturun ve akışın sıkıştırma biçimini belirtin.

`myPushStream` adlı bir giriş akışı sınıfınız olduğunu ve OPUS/OGG 'yi kullandığınızı varsayalım. Kodunuz şöyle görünebilir:

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
* [Java 'da konuşmayı tanımayı öğrenin](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java)
