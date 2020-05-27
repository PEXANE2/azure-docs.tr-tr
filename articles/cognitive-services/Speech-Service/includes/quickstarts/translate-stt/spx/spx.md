---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: 4d548c1b7614503919de2725acb02d7f808ceb93
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806521"
---
## <a name="run-the-speech-cli"></a>Konuşma CLı 'sını çalıştırma

Artık konuşmayı farklı bir dilde metne dönüştürmek için konuşma CLı 'yı çalıştırmaya hazırsınız.

Komut satırından, konuşma CLı ikili dosyasını içeren dizine geçin ve şunu yazın:

```bash
spx translate --microphone --target de-DE
```

Konuşma CLı, Ingilizce 'ye konuşulan doğal dili, Almanca dilinde basılan metne dönüştürür.
Aracı durdurmak için ENTER tuşuna basın.

> [!NOTE]
> Konuşma CLı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
> Örneğin, `--source ja-JP` Japonca konuşmayı tanımak için ekleyin.
