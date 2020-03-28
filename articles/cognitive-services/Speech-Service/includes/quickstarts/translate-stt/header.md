---
title: 'Quickstart: Konuşma-metin çevirme - Konuşma hizmeti'
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
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74981035"
---
Bu hızlı başlangıçta, konuşmayı bir dilden metne etkileşimli olarak çevirmek için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç ön koşulu yerine getirerek, konuşmadan metne çevirme yalnızca beş adımdan oluşur:
> [!div class="checklist"]
> * Abonelik ````SpeechConfig```` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Kaynak ````SpeechConfig```` ve hedef dilleri belirtmek için nesneyi güncelleştirin.
> * Nesneyi ````TranslationRecognizer```` yukarıdan ````SpeechConfig```` kullanarak bir nesne oluşturun.
> * Nesneyi ````TranslationRecognizer```` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade ````TranslationRecognitionResult```` edilenleri inceleyin.
