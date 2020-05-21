---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: v-demjoh
ms.openlocfilehash: 9ff21105e61be59b0000d86455ee19ba6f0d1aeb
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714718"
---
## <a name="find-a-file-that-contains-speech"></a>Konuşmayı içeren bir dosya bulun

SPX Aracı, konuşmayı birçok dosya biçiminde ve doğal dillerde tanıyabilir. Bu hızlı başlangıçta, Ingilizce konuşma içeren bir WAV dosyası (16kHz veya 8kHz, 16 bit ve mono PCM) kullanabilirsiniz.

1. <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/whatstheweatherlike.wav" download="whatstheweatherlike" target="_blank">. Wav <span class="docon docon-download x-hidden-focus"></span> gibi whatsthehava </a>.
2. Dosyayı, `whatstheweatherlike.wav` SPX aracı ikili dosyası ile aynı dizine kopyalayın.

## <a name="run-the-spx-tool"></a>SPX aracını çalıştırma

Artık, Ses dosyasında bulunan konuşmayı tanımak için SPX aracını çalıştırmaya hazırsınız demektir.

Komut satırından, SPX aracı ikili dosyasını içeren dizine geçin ve şunu yazın:

```bash
spx recognize --file whatstheweatherlike.wav
```

> [!NOTE]
> SPX aracı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
> Örneğin, `--source de-DE` Almanya konuşmanızı tanımak için ekleyin.

SPX Aracı, ekranda konuşmanın bir metin dökümünü gösterir. Ardından, SPX aracı kapanır.
