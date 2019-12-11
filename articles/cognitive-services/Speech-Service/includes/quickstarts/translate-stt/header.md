---
title: 'Hızlı başlangıç: konuşmayı metne çevirme-metin okuma hizmeti'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: d4781808ce8e80f62e86ce1d0c6db9c38b2636d0
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74981035"
---
Bu hızlı başlangıçta konuşmayı bir dilden başka bir dilde metne etkileşimli bir şekilde çevirmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşulu karşıladıktan sonra, konuşmayı metne çevirme yalnızca beş adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechConfig```` nesnesi oluşturun.
> * Kaynak ve hedef dilleri belirtmek için ````SpeechConfig```` nesnesini güncelleştirin.
> * Yukarıdaki ````SpeechConfig```` nesnesini kullanarak bir ````TranslationRecognizer```` nesnesi oluşturun.
> * ````TranslationRecognizer```` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ````TranslationRecognitionResult```` inceleyin.
