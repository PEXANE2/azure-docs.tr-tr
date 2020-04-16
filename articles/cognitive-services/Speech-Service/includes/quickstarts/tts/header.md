---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/31/2020
ms.author: trbye
ms.openlocfilehash: 005cf83508d25e8f44190e07336fbb4e444f8e6b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400423"
---
Bu hızlı başlangıçta, metni sentezlenmiş konuşmaya dönüştürmek için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanırsınız. Metinden konuşmaya hizmet, [metinden konuşmaya dil desteği](../../../language-support.md#text-to-speech)altında sentezlenen sesler için çok sayıda seçenek sunar. Birkaç ön koşulu yerine getirdikten sonra, sentezlenmiş konuşmayı varsayılan konuşmacılara işlemek yalnızca dört adımdan oluşur:
> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Nesneyi `SpeechSynthesizer` yukarıdan `SpeechConfig` kullanarak bir nesne oluşturun.
> * Metni `SpeechSynthesizer` konuşmak için nesneyi kullanma.
> * İade `SpeechSynthesisResult` edilen hataları denetleyin.
