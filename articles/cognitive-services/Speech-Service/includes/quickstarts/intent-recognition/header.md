---
title: 'Hızlı başlangıç: konuşmayı, amaçları ve varlıkları tanıma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 1/02/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a833d39ab91cd803f066d707306a6ff648d37e8f
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660560"
---
Bu hızlı başlangıçta, bir mikrofondan yakalanan ses verilerinden amaçları tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) ve Language UNDERSTANDING (Luu) hizmetini kullanacaksınız. Özellikle, konuşmayı yakalamak için konuşma SDK 'sını ve bir ışığı açma ve kapatma gibi LUYA 'ın giriş Otomasyonu amaçlarını belirlemek için de bir ön derlenmiş etki alanı kullanacaksınız. 

Birkaç önkoşulu karşıladıktan sonra, konuşma ve tanımlama amaçlarını bir mikrofondan tanımayı yalnızca birkaç adım sürer:

> [!div class="checklist"]
>
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechConfig```` nesnesi oluşturun.
> * Yukarıdaki ````SpeechConfig```` nesnesini kullanarak bir ````IntentRecognizer```` nesnesi oluşturun.
> * ````IntentRecognizer```` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ````IntentRecognitionResult```` inceleyin.
