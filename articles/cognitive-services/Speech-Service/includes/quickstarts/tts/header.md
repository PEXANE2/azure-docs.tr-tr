---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: e9f02f95693552180a0eed1550cc59d8f975416b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76961398"
---
Bu hızlı başlangıçta, metni sentezlenmiş konuşmaya dönüştürmek için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanırsınız. Metinden konuşmaya hizmet, [metinden konuşmaya dil desteği](../../../language-support.md#text-to-speech)altında sentezlenen sesler için çok sayıda seçenek sunar. Birkaç ön koşulu yerine getirdikten sonra, sentezlenmiş konuşmayı varsayılan konuşmacılara işlemek yalnızca dört adımdan oluşur:
> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Nesneyi `SpeechSynthesizer` yukarıdan `SpeechConfig` kullanarak bir nesne oluşturun.
> * Metni `SpeechSynthesizer` konuşmak için nesneyi kullanma.
> * İade `SpeechSynthesisResult` edilen hataları denetleyin.
