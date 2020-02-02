---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: cd8da5eb9313685361ca56b56c024c2dfb37276e
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76961582"
---
Bu hızlı başlangıçta, bir ses dosyasında metni sentezleştirilmiş konuşmaya dönüştürmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Metin okuma hizmeti, birleştirilmiş sesler için [metin okuma dili desteği](../../../language-support.md#text-to-speech)altında çok sayıda seçenek sunar. Birkaç önkoşul karşıladıktan sonra, konuşma bir dosyaya senile yalnızca beş adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesnesi oluşturun.
> * Öğesini belirten bir ses yapılandırma nesnesi oluşturun. WAV dosya adı.
> * Yukarıdaki yapılandırma nesnelerini kullanarak bir `SpeechSynthesizer` nesnesi oluşturun.
> * `SpeechSynthesizer` nesnesini kullanarak, metninizi sentezleştirilmiş konuşmaya dönüştürerek belirtilen ses dosyasına kaydederek metninizi birleştirilmiş konuşmaya dönüştürün.
> * Hatalar için döndürülen `SpeechSynthesizer` inceleyin.
