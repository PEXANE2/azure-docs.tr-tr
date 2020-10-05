---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: 8d70d282ffb1a39f9ffb3eb6addf694ac9d0d060
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "81422278"
---
Bu hızlı başlangıçta, bir mikrofondan yakalanan ses verilerinden amaçları tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) ve Language UNDERSTANDING (Luu) hizmetini kullanacaksınız. Özellikle, konuşmayı yakalamak için konuşma SDK 'sını ve bir ışığı açma ve kapatma gibi LUYA 'ın giriş Otomasyonu amaçlarını belirlemek için de bir ön derlenmiş etki alanı kullanacaksınız. 

Birkaç önkoşulu karşıladıktan sonra, konuşma ve tanımlama amaçlarını bir mikrofondan tanımayı yalnızca birkaç adım sürer:

> [!div class="checklist"]
>
> * `SpeechConfig`Abonelik anahtarınızdan ve bölgenizde bir nesne oluşturun.
> * `IntentRecognizer`Yukarıdaki nesneyi kullanarak bir nesne oluşturun `SpeechConfig` .
> * Nesnesini kullanarak `IntentRecognizer` , tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ' i inceleyin `IntentRecognitionResult` .
