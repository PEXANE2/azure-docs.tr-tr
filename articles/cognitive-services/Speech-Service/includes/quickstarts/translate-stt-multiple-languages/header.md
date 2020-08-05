---
title: 'Hızlı başlangıç: konuşmayı birden çok dile çevirme-konuşma hizmeti'
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
ms.openlocfilehash: 8c8cbc4e4f531d7a06ae3a33c33df9264c2cc6f2
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74981202"
---
Bu hızlı başlangıçta konuşmayı bir dilden başka bir dilde konuşmaya etkileşimli bir şekilde çevirmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşul karşıladıktan sonra, birden çok dilde konuşmayı metne çevirme yalnızca altı adımdan yararlanır:
> [!div class="checklist"]
> * ````SpeechTranslationConfig````Abonelik anahtarınızdan ve bölgenizde bir nesne oluşturun.
> * ````SpeechTranslationConfig````Konuşma tanıma kaynak dilini belirtmek için nesneyi güncelleştirin.
> * ````SpeechTranslationConfig````Birden çok çeviri hedefi dili belirtmek için nesnesini güncelleştirin.
> * ````TranslationRecognizer````Yukarıdaki nesneyi kullanarak bir nesne oluşturun ````SpeechTranslationConfig```` .
> * Nesnesini kullanarak ````TranslationRecognizer```` , tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ' i inceleyin ````TranslationRecognitionResult```` .
