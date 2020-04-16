---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: 36243fd5db76a4706318f41b3e2cb3f557c17189
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400716"
---
Bu hızlı başlatmada, konuşmanın mikrofon girişiyle etkileşimli olarak tanınması ve yakalanan sesten metin transkripsiyonu almak için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanırsınız. Konuşmaları aktarma gibi yaygın tanıma görevleri için bu özelliği uygulamalarınız veya cihazlarınızla entegre etmek kolaydır. Ayrıca, ses asistanları oluşturmak için Konuşma SDK ile Bot Framework kullanarak gibi daha karmaşık entegrasyonlar için kullanılabilir.

Birkaç ön koşulu yerine getirerek, konuşmayı mikrofondan tanıma yalnızca dört adım dan geçer:

> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Nesneyi `SpeechRecognizer` yukarıdan `SpeechConfig` kullanarak bir nesne oluşturun.
> * Nesneyi `SpeechRecognizer` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade `SpeechRecognitionResult` edilenleri inceleyin.
