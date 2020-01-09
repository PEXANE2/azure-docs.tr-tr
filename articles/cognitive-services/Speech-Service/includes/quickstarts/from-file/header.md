---
title: 'Hızlı başlangıç: ses dosyasından konuşmayı tanıma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: e333b156eaf9c4b6e09e631513ea099018f0691b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75467535"
---
Bu hızlı başlangıçta, ses dosyasından konuşmayı tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşulu karşıladıktan sonra, bir dosyadan konuşmayı tanımayı yalnızca beş adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechConfig```` nesnesi oluşturun.
> * Öğesini belirten bir ````AudioConfig```` nesnesi oluşturun. WAV dosya adı.
> * Yukarıdaki ````SpeechConfig```` ve ````AudioConfig```` nesnelerini kullanarak bir ````SpeechRecognizer```` nesnesi oluşturun.
> * ````SpeechRecognizer```` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ````SpeechRecognitionResult```` inceleyin.
