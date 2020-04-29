---
title: 'Hızlı başlangıç: konuşmayı metne çevirme-metin okuma hizmeti'
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
ms.openlocfilehash: d4781808ce8e80f62e86ce1d0c6db9c38b2636d0
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74981035"
---
Bu hızlı başlangıçta konuşmayı bir dilden başka bir dilde metne etkileşimli bir şekilde çevirmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşulu karşıladıktan sonra, konuşmayı metne çevirme yalnızca beş adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechConfig```` nesne oluşturun.
> * Kaynak ve ````SpeechConfig```` hedef dilleri belirtmek için nesneyi güncelleştirin.
> * Yukarıdaki ````SpeechConfig```` nesneyi ````TranslationRecognizer```` kullanarak bir nesne oluşturun.
> * ````TranslationRecognizer```` Nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * ````TranslationRecognitionResult```` Döndürülen ' i inceleyin.
