---
title: 'Hızlı başlangıç: konuşmayı birden çok dile çevirme-konuşma hizmeti'
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
ms.openlocfilehash: 8c8cbc4e4f531d7a06ae3a33c33df9264c2cc6f2
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74981202"
---
Bu hızlı başlangıçta konuşmayı bir dilden başka bir dilde konuşmaya etkileşimli bir şekilde çevirmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşul karşıladıktan sonra, birden çok dilde konuşmayı metne çevirme yalnızca altı adımdan yararlanır:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechTranslationConfig```` nesnesi oluşturun.
> * Konuşma tanıma kaynak dilini belirtmek için ````SpeechTranslationConfig```` nesnesini güncelleştirin.
> * Birden çok çeviri hedefi dili belirtmek için ````SpeechTranslationConfig```` nesnesini güncelleştirin.
> * Yukarıdaki ````SpeechTranslationConfig```` nesnesini kullanarak bir ````TranslationRecognizer```` nesnesi oluşturun.
> * ````TranslationRecognizer```` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ````TranslationRecognitionResult```` inceleyin.
