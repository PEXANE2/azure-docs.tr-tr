---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 870dce55e79bf0169f19d31dfec6689c65fce9cd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400317"
---
Bu hızlı başlatmada, metni ses dosyasında sentezlenmiş konuşmaya dönüştürmek için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanırsınız. Metinden konuşmaya hizmet, [metinden konuşmaya dil desteği](../../../language-support.md#text-to-speech)altında sentezlenen sesler için çok sayıda seçenek sunar. Birkaç ön koşulu yerine getirerek, konuşmayı bir dosyaya sentezleme yalnızca beş adımdan oluşur:
> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Bir Ses Yapılandırması nesnesi oluşturun ve bu nesneyi belirtir. WAV dosya adı.
> * Yukarıdan `SpeechSynthesizer` yapılandırma nesneleri kullanarak bir nesne oluşturun.
> * Nesneyi `SpeechSynthesizer` kullanarak, metninizi sentezlenmiş konuşmaya dönüştürerek belirtilen ses dosyasına kaydedin.
> * Döndürülen `SpeechSynthesizer` hataları inceleyin.
