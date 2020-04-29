---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 005cf83508d25e8f44190e07336fbb4e444f8e6b
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400423"
---
Bu hızlı başlangıçta, metni sentezleştirilmiş konuşmaya dönüştürmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Metin okuma hizmeti, birleştirilmiş sesler için [metin okuma dili desteği](../../../language-support.md#text-to-speech)altında çok sayıda seçenek sunar. Birkaç önkoşul karşıladıktan sonra, birleştirilmiş konuşmayı varsayılan hoparlörlere işleme yalnızca dört adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesne oluşturun.
> * Yukarıdaki `SpeechConfig` nesneyi `SpeechSynthesizer` kullanarak bir nesne oluşturun.
> * Metni konuşmak `SpeechSynthesizer` için nesnesini kullanma.
> * Hata için `SpeechSynthesisResult` döndürülen ' i kontrol edin.
