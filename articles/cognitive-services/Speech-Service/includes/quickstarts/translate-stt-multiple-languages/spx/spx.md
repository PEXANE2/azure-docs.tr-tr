---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: 94d50a15b0d5d7bed2bd43516952036044d7f0c0
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715127"
---
## <a name="run-the-spx-tool"></a>SPX aracını çalıştırma

Artık iki farklı dilde konuşmayı metne dönüştürmek için SPX aracını çalıştırmaya hazırsınız.

Komut satırından, SPX aracı ikili dosyasını içeren dizine geçin ve şunu yazın:

```bash
spx translate --microphone --target de-DE --target es-MX
```

SPX Aracı, Ingilizce 'ye konuşulan doğal dili, Almanca ve (Meksika) Ispanyolca olarak basılan metne dönüştürür.
Aracı durdurmak için ENTER tuşuna basın.

> [!NOTE]
> SPX aracı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
> Örneğin, `--source ja-JP` Japonca konuşmayı tanımak için ekleyin.
