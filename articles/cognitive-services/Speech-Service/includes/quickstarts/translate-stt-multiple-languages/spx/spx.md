---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: c73ee93d89a350763e8ced490187e1c47f8918de
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806494"
---
## <a name="run-the-speech-cli"></a>Konuşma CLı 'sını çalıştırma

Artık konuşmayı iki farklı dilde dönüştürmek için konuşma CLı 'yı çalıştırmaya hazır olursunuz.

Komut satırından, konuşma CLı ikili dosyasını içeren dizine geçin ve şunu yazın:

```bash
spx translate --microphone --target de-DE --target es-MX
```

Konuşma CLı, Ingilizce 'ye konuşulan doğal dili Almanca ve (Mexika) Ispanyolca olarak çevirecek metne dönüştürür.
Aracı durdurmak için ENTER tuşuna basın.

> [!NOTE]
> Konuşma CLı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
> Örneğin, `--source ja-JP` Japonca konuşmayı tanımak için ekleyin.
