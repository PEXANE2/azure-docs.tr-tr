---
title: 'Hızlı başlangıç: ses dosyasından konuşmayı tanıma-konuşma hizmeti'
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
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76038082"
---
Bu hızlı başlangıçta, ses dosyasından konuşmayı tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşulu karşıladıktan sonra, bir dosyadan konuşmayı tanımayı yalnızca birkaç adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesnesi oluşturun.
> * Öğesini belirten bir `AudioConfig` nesnesi oluşturun. WAV dosya adı.
> * Yukarıdaki `SpeechConfig` ve `AudioConfig` nesnelerini kullanarak bir `SpeechRecognizer` nesnesi oluşturun.
> * `SpeechRecognizer` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen `SpeechRecognitionResult` inceleyin.
