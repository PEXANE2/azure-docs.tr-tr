---
title: 'Hızlı başlangıç: ses dosyasından konuşmayı tanıma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 2f7ec24667514fb131af29321a53f97210e58fc9
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74819420"
---
Bu hızlı başlangıçta, ses dosyasından konuşmayı tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşulu karşıladıktan sonra, bir dosyadan konuşmayı tanımayı yalnızca beş adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechConfig```` nesnesi oluşturun.
> * Öğesini belirten bir ````AudioConfig```` nesnesi oluşturun. WAV dosya adı.
> * Yukarıdaki ````SpeechConfig```` ve ````AudioConfig```` nesnelerini kullanarak bir ````SpeechRecognizer```` nesnesi oluşturun.
> * ````SpeechRecognizer```` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ````SpeechRecognitionResult```` inceleyin.
