---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: 161138ba09baacdd2a3d18d35c930c645ee78103
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715196"
---
## <a name="run-the-spx-tool"></a>SPX aracını çalıştırma

Artık, konuşmayı metinden yeni bir ses dosyasına eklemek için SPX aracını çalıştırmaya hazırsınız.

Komut satırından, SPX aracı ikili dosyasını içeren dizine geçin ve şunu yazın:

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

SPX Aracı, Ses dosyasında Ingilizce olarak doğal dil oluşturacaktır `greetings.wav` .
Windows 'da, ses dosyasını girerek oynatabilirsiniz `start greetings.wav` .
