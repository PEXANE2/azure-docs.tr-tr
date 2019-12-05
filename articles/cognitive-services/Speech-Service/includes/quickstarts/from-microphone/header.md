---
title: 'Hızlı başlangıç: bir mikrofon konuşma hizmetinden konuşmayı tanıma'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 11/20/2019
ms.author: erhopf
ms.openlocfilehash: a72597163a8c11cd8b515d052dc69992784e61e0
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818814"
---
Bu hızlı başlangıçta, bir mikrofon girişinden konuşmayı etkileşimli olarak tanımak ve yakalanan sesten metin dökümünü almak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Bu özelliği, uygulama ve cihazlarınızla ilgili sık sorulan konuşma konuşmaları gibi yaygın tanıma görevleri için tümleştirmeniz kolaydır. Ayrıca, ses yardımcıları oluşturmak için konuşma SDK 'Sı ile bot çerçevesini kullanma gibi daha karmaşık Tümleştirmeler için de kullanılabilir.

Birkaç önkoşul karşıladıktan sonra, konuşmayı bir mikrofondan tanımayı yalnızca dört adım sürer:

> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesnesi oluşturun.
> * Yukarıdaki `SpeechConfig` nesnesini kullanarak bir `SpeechRecognizer` nesnesi oluşturun.
> * `SpeechRecognizer` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen `SpeechRecognitionResult` inceleyin.
