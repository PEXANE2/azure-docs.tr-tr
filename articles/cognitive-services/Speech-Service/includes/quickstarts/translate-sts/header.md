---
title: 'Quickstart: Konuşma-konuşma çevirme - Konuşma hizmeti'
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
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74980810"
---
Bu hızlı başlangıçta, konuşmayı bir dilden konuşmaya başka bir dilde etkileşimli olarak çevirmek için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç ön koşulu yerine getirerek, konuşma-konuşma çevirisi yalnızca altı adım dan oluşur:
> [!div class="checklist"]
> * Abonelik ````SpeechTranslationConfig```` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Kaynak ````SpeechTranslationConfig```` ve hedef dilleri belirtmek için nesneyi güncelleştirin.
> * Konuşma ````SpeechTranslationConfig```` çıktısı ses adını belirtmek için nesneyi güncelleştirin.
> * Nesneyi ````TranslationRecognizer```` yukarıdan ````SpeechTranslationConfig```` kullanarak bir nesne oluşturun.
> * Nesneyi ````TranslationRecognizer```` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade ````TranslationRecognitionResult```` edilenleri inceleyin.
