---
title: 'Quickstart: Konuşmayı birden çok dile çevirin - Konuşma hizmeti'
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
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74981202"
---
Bu hızlı başlangıçta, konuşmayı bir dilden konuşmaya başka bir dilde etkileşimli olarak çevirmek için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç ön koşulu yerine getirerek, konuşmayı birden çok dilde metne çevirmek yalnızca altı adımdan oluşur:
> [!div class="checklist"]
> * Abonelik ````SpeechTranslationConfig```` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Konuşma ````SpeechTranslationConfig```` tanıma kaynak dilini belirtmek için nesneyi güncelleştirin.
> * Birden ````SpeechTranslationConfig```` çok çeviri hedef dilbelirtmek için nesneyi güncelleştirin.
> * Nesneyi ````TranslationRecognizer```` yukarıdan ````SpeechTranslationConfig```` kullanarak bir nesne oluşturun.
> * Nesneyi ````TranslationRecognizer```` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade ````TranslationRecognitionResult```` edilenleri inceleyin.
