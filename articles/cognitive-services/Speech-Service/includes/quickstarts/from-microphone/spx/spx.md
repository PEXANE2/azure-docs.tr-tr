---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: v-demjoh
ms.openlocfilehash: a9beaea70dd3c8ce852344b11a50c5f20e5f8ca2
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806543"
---
## <a name="enable-microphone"></a>Mikrofonu etkinleştir

BILGISAYAR mikrofonunuzu takın ve açın ve mikrofonu da kullanabilir olabilecek tüm uygulamaları kapatın. Bazı bilgisayarlarda yerleşik bir mikrofon vardır, diğerleri ise Bluetooth cihazının yapılandırılması gerekir.

## <a name="run-the-speech-cli"></a>Konuşma CLı 'sını çalıştırma

Artık mikrofona konuşmayı tanımak için konuşma CLı 'sını çalıştırmaya hazırsınız demektir.

1. **Uygulamanızı başlatın** -komut satırından, konuşma CLI ikili dosyasını içeren dizine geçin ve şunu yazın:
    ```bash
    spx recognize --microphone
    ```

    > [!NOTE]
    > Konuşma CLı varsayılan olarak Ingilizce 'Dir. [Konuşma-metin tablosundan](../../../../language-support.md)farklı bir dil seçebilirsiniz.
    > Örneğin, `--source de-DE` Almanya konuşmanızı tanımak için ekleyin.

2. Konuşmaya **başlayın** -mikrofona konuşun. Kelimelerinizin gerçek zamanlı olarak metne göre dökümünü görürsünüz. Konuşma CLı, bir süre sessizden sonra durur veya CTRL-C tuşlarına basın.
