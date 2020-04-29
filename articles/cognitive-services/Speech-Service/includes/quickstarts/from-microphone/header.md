---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: 36243fd5db76a4706318f41b3e2cb3f557c17189
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400716"
---
Bu hızlı başlangıçta, bir mikrofon girişinden konuşmayı etkileşimli olarak tanımak ve yakalanan sesten metin dökümünü almak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanırsınız. Bu özelliği, uygulama ve cihazlarınızla ilgili sık sorulan konuşma konuşmaları gibi yaygın tanıma görevleri için tümleştirmeniz kolaydır. Ayrıca, ses yardımcıları oluşturmak için konuşma SDK 'Sı ile bot çerçevesini kullanma gibi daha karmaşık Tümleştirmeler için de kullanılabilir.

Birkaç önkoşul karşıladıktan sonra, konuşmayı bir mikrofondan tanımayı yalnızca dört adım sürer:

> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesne oluşturun.
> * Yukarıdaki `SpeechConfig` nesneyi `SpeechRecognizer` kullanarak bir nesne oluşturun.
> * `SpeechRecognizer` Nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * `SpeechRecognitionResult` Döndürülen ' i inceleyin.
