---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: v-demjoh
ms.openlocfilehash: 398dfe36314f86614515168986023146d695e69b
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806499"
---
## <a name="find-a-file-that-contains-speech"></a>Konuşmayı içeren bir dosya bulun

Konuşma CLı, konuşmayı birçok dosya biçiminde ve doğal dillerde tanıyabilir. Bu hızlı başlangıçta, Ingilizce konuşma içeren bir WAV dosyası (16kHz veya 8kHz, 16 bit ve mono PCM) kullanabilirsiniz.

1. <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/whatstheweatherlike.wav" download="whatstheweatherlike" target="_blank">. Wav <span class="docon docon-download x-hidden-focus"></span> gibi whatsthehava </a>.
2. `whatstheweatherlike.wav`Dosyayı konuşma CLI ikili dosyası ile aynı dizine kopyalayın.

## <a name="run-the-speech-cli"></a>Konuşma CLı 'sını çalıştırma

Artık ses dosyasında bulunan konuşmayı tanımak için konuşma CLı 'sını çalıştırmaya hazırsınız demektir.

Komut satırından, konuşma CLı ikili dosyasını içeren dizine geçin ve şunu yazın:

```bash
spx recognize --file whatstheweatherlike.wav
```

> [!NOTE]
> Konuşma CLı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
> Örneğin, `--source de-DE` Almanya konuşmanızı tanımak için ekleyin.

Konuşma CLı, ekranda konuşmanın bir metin dökümünü gösterir. Ardından konuşma CLı kapatılacak.
