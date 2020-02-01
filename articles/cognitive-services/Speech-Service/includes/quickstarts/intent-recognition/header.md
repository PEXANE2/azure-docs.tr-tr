---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: dapine
ms.openlocfilehash: d59a55a9b6d57d90ac6ae893b05e1064c11eb58b
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900443"
---
Bu hızlı başlangıçta, bir mikrofondan yakalanan ses verilerinden amaçları tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) ve Language UNDERSTANDING (Luu) hizmetini kullanacaksınız. Özellikle, konuşmayı yakalamak için konuşma SDK 'sını ve bir ışığı açma ve kapatma gibi LUYA 'ın giriş Otomasyonu amaçlarını belirlemek için de bir ön derlenmiş etki alanı kullanacaksınız. 

Birkaç önkoşulu karşıladıktan sonra, konuşma ve tanımlama amaçlarını bir mikrofondan tanımayı yalnızca birkaç adım sürer:

> [!div class="checklist"]
>
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesnesi oluşturun.
> * Yukarıdaki `SpeechConfig` nesnesini kullanarak bir `IntentRecognizer` nesnesi oluşturun.
> * `IntentRecognizer` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen `IntentRecognitionResult` inceleyin.
