---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: 8d70d282ffb1a39f9ffb3eb6addf694ac9d0d060
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81422278"
---
Bu hızlı başlangıçta, bir mikrofondan yakalanan ses verilerinden amaçları tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) ve Language UNDERSTANDING (Luu) hizmetini kullanacaksınız. Özellikle, konuşmayı yakalamak için konuşma SDK 'sını ve bir ışığı açma ve kapatma gibi LUYA 'ın giriş Otomasyonu amaçlarını belirlemek için de bir ön derlenmiş etki alanı kullanacaksınız. 

Birkaç önkoşulu karşıladıktan sonra, konuşma ve tanımlama amaçlarını bir mikrofondan tanımayı yalnızca birkaç adım sürer:

> [!div class="checklist"]
>
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesne oluşturun.
> * Yukarıdaki `SpeechConfig` nesneyi `IntentRecognizer` kullanarak bir nesne oluşturun.
> * `IntentRecognizer` Nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * `IntentRecognitionResult` Döndürülen ' i inceleyin.
