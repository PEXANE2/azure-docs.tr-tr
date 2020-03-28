---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: dapine
ms.openlocfilehash: d59a55a9b6d57d90ac6ae893b05e1064c11eb58b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76900443"
---
Bu hızlı başlangıçta, mikrofondan yakalanan ses verilerinden gelen sesleri tanımak için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) ve Dil Anlayışı (LUIS) hizmetini kullanırsınız. Özellikle, konuşma yakalamak için Speech SDK'yı ve LUIS'in önceden oluşturulmuş bir etki alanını, ışığı açıp kapatmak gibi ev otomasyonu niyetlerini belirlemek için kullanırsınız. 

Birkaç ön koşulu yerine getirerek, konuşmayı tanıma ve mikrofondan niyetlerini tanımlama yalnızca birkaç adım dan sonra:

> [!div class="checklist"]
>
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Nesneyi `IntentRecognizer` yukarıdan `SpeechConfig` kullanarak bir nesne oluşturun.
> * Nesneyi `IntentRecognizer` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade `IntentRecognitionResult` edilenleri inceleyin.
