---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 870dce55e79bf0169f19d31dfec6689c65fce9cd
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400317"
---
Bu hızlı başlangıçta, bir ses dosyasında metni sentezleştirilmiş konuşmaya dönüştürmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Metin okuma hizmeti, birleştirilmiş sesler için [metin okuma dili desteği](../../../language-support.md#text-to-speech)altında çok sayıda seçenek sunar. Birkaç önkoşul karşıladıktan sonra, konuşma bir dosyaya senile yalnızca beş adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesne oluşturun.
> * Öğesini belirten bir ses yapılandırma nesnesi oluşturun. WAV dosya adı.
> * Yukarıdaki yapılandırma `SpeechSynthesizer` nesnelerini kullanarak bir nesne oluşturun.
> * `SpeechSynthesizer` Nesnesini kullanarak, metninizi birleştirilmiş konuşmaya dönüştürerek, belirtilen ses dosyasına kaydederek metninizi sentezleştirilmiş konuşmaya dönüştürün.
> * Hata için `SpeechSynthesizer` döndürülen ' i inceleyin.
