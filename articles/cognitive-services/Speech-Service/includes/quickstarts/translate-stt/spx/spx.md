---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: 57383668ef025b46d0bae1f98c5ed5cd75417c63
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715044"
---
## <a name="run-the-spx-tool"></a>SPX aracını çalıştırma

Artık, farklı bir dilde konuşmayı metne dönüştürmek için SPX aracını çalıştırmaya hazırsınız demektir.

Komut satırından, SPX aracı ikili dosyasını içeren dizine geçin ve şunu yazın:

```bash
spx translate --microphone --target de-DE
```

SPX Aracı, Ingilizce 'ye konuşulan doğal dili, Almanca dilinde basılan metne dönüştürür.
Aracı durdurmak için ENTER tuşuna basın.

> [!NOTE]
> SPX aracı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
> Örneğin, `--source ja-JP` Japonca konuşmayı tanımak için ekleyin.
