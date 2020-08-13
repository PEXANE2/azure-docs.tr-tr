---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 08/11/2020
ms.author: trbye
ms.openlocfilehash: bee28c946242ecf227287fb36b5b03aa6defb1c2
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88173151"
---
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