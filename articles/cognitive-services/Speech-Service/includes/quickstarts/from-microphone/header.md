---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: dapine
ms.openlocfilehash: 3775690802c89805ccf9df1ee6d6717a8818213f
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80659392"
---
Bu hızlı başlatmada, konuşmanın mikrofon girişiyle etkileşimli olarak tanınması ve yakalanan sesten metin transkripsiyonu almak için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanırsınız. Konuşmaları aktarma gibi yaygın tanıma görevleri için bu özelliği uygulamalarınız veya cihazlarınızla entegre etmek kolaydır. Ayrıca, ses asistanları oluşturmak için Konuşma SDK ile Bot Framework kullanarak gibi daha karmaşık entegrasyonlar için kullanılabilir.

Birkaç ön koşulu yerine getirerek, konuşmayı mikrofondan tanıma yalnızca dört adım dan geçer:

> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Nesneyi `SpeechRecognizer` yukarıdan `SpeechConfig` kullanarak bir nesne oluşturun.
> * Nesneyi `SpeechRecognizer` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade `SpeechRecognitionResult` edilenleri inceleyin.
