---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: cd8da5eb9313685361ca56b56c024c2dfb37276e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76961582"
---
Bu hızlı başlatmada, metni ses dosyasında sentezlenmiş konuşmaya dönüştürmek için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanırsınız. Metinden konuşmaya hizmet, [metinden konuşmaya dil desteği](../../../language-support.md#text-to-speech)altında sentezlenen sesler için çok sayıda seçenek sunar. Birkaç ön koşulu yerine getirerek, konuşmayı bir dosyaya sentezleme yalnızca beş adımdan oluşur:
> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Bir Ses Yapılandırması nesnesi oluşturun ve bu nesneyi belirtir. WAV dosya adı.
> * Yukarıdan `SpeechSynthesizer` yapılandırma nesneleri kullanarak bir nesne oluşturun.
> * Nesneyi `SpeechSynthesizer` kullanarak, metninizi sentezlenmiş konuşmaya dönüştürerek belirtilen ses dosyasına kaydedin.
> * Döndürülen `SpeechSynthesizer` hataları inceleyin.
