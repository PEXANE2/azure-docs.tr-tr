---
title: 'Hızlı başlangıç: konuşmayı konuşmaya çevirme-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: c5f0a0fe032d18cd4f01aebe9a5c736d6d511a74
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74980810"
---
Bu hızlı başlangıçta konuşmayı bir dilden başka bir dilde konuşmaya etkileşimli bir şekilde çevirmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşul karşıladıktan sonra, konuşmayı konuşmaya çevirme özelliği yalnızca altı adımdan yararlanır:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechTranslationConfig```` nesne oluşturun.
> * Kaynak ve ````SpeechTranslationConfig```` hedef dilleri belirtmek için nesneyi güncelleştirin.
> * Konuşma çıkışı ````SpeechTranslationConfig```` ses adını belirtmek için nesneyi güncelleştirin.
> * Yukarıdaki ````SpeechTranslationConfig```` nesneyi ````TranslationRecognizer```` kullanarak bir nesne oluşturun.
> * ````TranslationRecognizer```` Nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * ````TranslationRecognitionResult```` Döndürülen ' i inceleyin.
