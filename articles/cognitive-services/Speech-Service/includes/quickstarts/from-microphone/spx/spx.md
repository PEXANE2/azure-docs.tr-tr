---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: v-demjoh
ms.openlocfilehash: 9f1da13efc9f75d14fdf2d158e8b7547d4a9fa94
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715332"
---
## <a name="enable-microphone"></a>Mikrofonu etkinleştir

BILGISAYAR mikrofonunuzu takın ve açın ve mikrofonu da kullanabilir olabilecek tüm uygulamaları kapatın. Bazı bilgisayarlarda yerleşik bir mikrofon vardır, diğerleri ise Bluetooth cihazının yapılandırılması gerekir.

## <a name="run-the-spx-tool"></a>SPX aracını çalıştırma

Artık, mikrofonunuzdan konuşmayı tanımak için SPX aracını çalıştırmaya hazırsınız demektir.

1. **Uygulamanızı başlatın** -komut SATıRıNDAN, SPX aracı ikili dosyasını içeren dizine geçin ve şunu yazın:
    ```bash
    spx recognize --microphone
    ```

    > [!NOTE]
    > SPX aracı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
    > Örneğin, `--source de-DE` Almanya konuşmanızı tanımak için ekleyin.

2. Konuşmaya **başlayın** -mikrofona konuşun. Kelimelerinizin gerçek zamanlı olarak metne göre dökümünü görürsünüz. SPX aracı bir süre sessizden sonra durur veya CTRL-C tuşlarına basın.
