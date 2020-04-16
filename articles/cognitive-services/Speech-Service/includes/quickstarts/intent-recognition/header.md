---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: 8d70d282ffb1a39f9ffb3eb6addf694ac9d0d060
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422278"
---
Bu hızlı başlangıçta, mikrofondan yakalanan ses verilerinden gelen sesleri tanımak için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) ve Dil Anlayışı (LUIS) hizmetini kullanırsınız. Özellikle, konuşma yakalamak için Speech SDK'yı ve LUIS'in önceden oluşturulmuş bir etki alanını, ışığı açıp kapatmak gibi ev otomasyonu niyetlerini belirlemek için kullanırsınız. 

Birkaç ön koşulu yerine getirerek, konuşmayı tanıma ve mikrofondan niyetlerini tanımlama yalnızca birkaç adım dan sonra:

> [!div class="checklist"]
>
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Nesneyi `IntentRecognizer` yukarıdan `SpeechConfig` kullanarak bir nesne oluşturun.
> * Nesneyi `IntentRecognizer` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade `IntentRecognitionResult` edilenleri inceleyin.
