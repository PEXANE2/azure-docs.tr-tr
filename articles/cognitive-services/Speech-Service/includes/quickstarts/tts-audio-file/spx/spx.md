---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: bd48618a520f547c72bfba7ff04ae6a249d0a673
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806356"
---
## <a name="run-the-speech-cli"></a>Konuşma CLı 'sını çalıştırma

Artık konuşmayı metinden yeni bir ses dosyasına eklemek için konuşmayı CLı 'yı çalıştırmaya hazırsınız demektir.

Komut satırından, konuşma CLı ikili dosyasını içeren dizine geçin ve şunu yazın:

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

Konuşma CLı, Ses dosyasında Ingilizce olarak doğal dil oluşturacak `greetings.wav` .
Windows 'da, ses dosyasını girerek oynatabilirsiniz `start greetings.wav` .
