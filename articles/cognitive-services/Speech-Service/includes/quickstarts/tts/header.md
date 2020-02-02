---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: e9f02f95693552180a0eed1550cc59d8f975416b
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76961398"
---
Bu hızlı başlangıçta, metni sentezleştirilmiş konuşmaya dönüştürmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Metin okuma hizmeti, birleştirilmiş sesler için [metin okuma dili desteği](../../../language-support.md#text-to-speech)altında çok sayıda seçenek sunar. Birkaç önkoşul karşıladıktan sonra, birleştirilmiş konuşmayı varsayılan hoparlörlere işleme yalnızca dört adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesnesi oluşturun.
> * Yukarıdaki `SpeechConfig` nesnesini kullanarak bir `SpeechSynthesizer` nesnesi oluşturun.
> * Metni konuşmak için `SpeechSynthesizer` nesnesini kullanma.
> * Hatalar için döndürülen `SpeechSynthesisResult` denetleyin.
