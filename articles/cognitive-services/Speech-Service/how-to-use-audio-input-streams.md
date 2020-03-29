---
title: Konuşma SDK ses giriş akışı kavramları
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'nın ses girişi akışı API yeteneklerine genel bakış.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: fmegen
ms.openlocfilehash: 3039276a49e7bb41660d114e78ca047a3f77f279
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74109939"
---
# <a name="about-the-speech-sdk-audio-input-stream-api"></a>Konuşma SDK ses girişi akışı API hakkında

Speech SDK'nın **Ses Girişi Akışı** API'si, mikrofonu veya giriş dosyası API'lerini kullanmak yerine tanıyıcılara ses akışı sağlamanın bir yolunu sağlar.

Ses giriş akışları kullanırken aşağıdaki adımlar gereklidir:

- Ses akışının biçimini tanımlayın. Biçim Konuşma SDK ve Konuşma hizmeti tarafından desteklenmelidir. Şu anda yalnızca aşağıdaki yapılandırma desteklenir:

  PCM formatında ses örnekleri, bir kanal, saniyede 16000 örnek, saniyede 32000 bayt, iki blok hizalama (numune için dolgu dahil 16 bit), numune başına 16 bit.

  Ses biçimini oluşturmak için SDK'daki ilgili kod aşağıdaki gibi görünür:

  ```csharp
  byte channels = 1;
  byte bitsPerSample = 16;
  int samplesPerSecond = 16000;
  var audioFormat = AudioStreamFormat.GetWaveFormatPCM(samplesPerSecond, bitsPerSample, channels);
  ```

- Kodunuzun RAW ses verilerini bu özelliklere göre sağlayabileceğinden emin olun. Ses kaynağı verileriniz desteklenen biçimlerle eşleşmiyorsa, sesin gerekli biçime aktarılması gerekir.

- Türetilen kendi ses giriş akışı `PullAudioInputStreamCallback`sınıfOluşturun. Uygulayın `Read()` `Close()` ve üyeleri. Tam işlev imzası dile bağlıdır, ancak kod bu kod örneğine benzer görünür:

  ```csharp
   public class ContosoAudioStream : PullAudioInputStreamCallback {
      ContosoConfig config;

      public ContosoAudioStream(const ContosoConfig& config) {
          this.config = config;
      }

      public int Read(byte[] buffer, uint size) {
          // returns audio data to the caller.
          // e.g. return read(config.YYY, buffer, size);
      }

      public void Close() {
          // close and cleanup resources.
      }
   };
  ```

- Ses biçiminize ve giriş akışınızı temel alan bir ses yapılandırması oluşturun. Tanıyıcınızı oluştururken hem normal konuşma yapılandırmanızı hem de ses giriş yapılandırmanızı geçirin. Örnek:

  ```csharp
  var audioConfig = AudioConfig.FromStreamInput(new ContosoAudioStream(config), audioFormat);

  var speechConfig = SpeechConfig.FromSubscription(...);
  var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

  // run stream through recognizer
  var result = await recognizer.RecognizeOnceAsync();

  var text = result.GetText();
  ```

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C'deki konuşmayı nasıl tanıyacağınızı görme #](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
