---
title: 'Quickstart: Bir ses dosyasından konuşmayı tanıma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/14/2020
ms.author: dapine
ms.openlocfilehash: 2b6270535c0cf69549a7412bd38d9207454e5500
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76038082"
---
Bu hızlı başlatmada, bir ses dosyasından konuşmayı tanımak için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç ön koşulu yerine getirerek, bir dosyadan konuşmayı tanıma yalnızca birkaç adım dan alır:
> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * 'yi `AudioConfig` belirten bir nesne oluşturun. WAV dosya adı.
> * Yukarıdan `SpeechRecognizer` `SpeechConfig` ve `AudioConfig` nesneleri kullanarak bir nesne oluşturun.
> * Nesneyi `SpeechRecognizer` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade `SpeechRecognitionResult` edilenleri inceleyin.
