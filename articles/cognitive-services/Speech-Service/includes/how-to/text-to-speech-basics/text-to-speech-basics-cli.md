---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 08/11/2020
ms.author: trbye
ms.openlocfilehash: b68025c1b33a94f03e2d84693a7d6407a18abd88
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91332461"
---
Bu hızlı başlangıçta, konuşma SDK 'sını kullanarak metinden konuşmaya senşlerini kullanmaya yönelik yaygın tasarım düzenlerini öğrenirsiniz. Temel yapılandırma ve birleştirme işlemleri gerçekleştirerek başlar ve aşağıdakiler de dahil olmak üzere özel uygulama geliştirme için daha gelişmiş örneklere geçin:

* Bellek içi akışlar olarak yanıt alma
* Çıkış örneği hızını ve bit hızını özelleştirme
* SSML kullanarak senssıs istekleri gönderme (konuşma birleştirme biçimlendirme dili)
* Sinir seslerini kullanma

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

[!INCLUDE [SPX Setup](../../spx-setup.md)]

## <a name="synthesize-speech-to-a-speaker"></a>Konuşmayı hoparlöre sentezleme

Artık metinden konuşmayı sentezleştirmek için konuşma CLı 'Yı çalıştırmaya hazırsınız. Komut satırından, konuşma CLı ikili dosyasını içeren dizine geçin. Ardından aşağıdaki komutu çalıştırın.

```bash
spx synthesize --text "The speech synthesizer greets you!"
```

Konuşma CLı, bilgisayar hoparlörü aracılığıyla Ingilizce 'de doğal dil oluşturacak.

## <a name="synthesize-speech-to-a-file"></a>Konuşmayı bir dosyaya sentezleştirme

Konuşmacının çıkışını bir dosyaya değiştirmek için aşağıdaki komutu çalıştırın `.wav` .

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

Konuşma CLı, Ses dosyasında Ingilizce olarak doğal dil oluşturacak `greetings.wav` .
Windows 'da, ses dosyasını girerek oynatabilirsiniz `start greetings.wav` .