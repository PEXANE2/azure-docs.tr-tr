---
title: 'Hızlı başlangıç: bir mikrofon konuşma hizmetinden konuşmayı tanıma'
titleSuffix: Azure Cognitive Services
description: Henüz belirlenmedi.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 11/20/2019
ms.author: erhopf
ms.openlocfilehash: bee3b64ece3faac6258fe4d017f12833b12e370d
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74536344"
---
Bu hızlı başlangıçta, bir mikrofon girişinden konuşmayı etkileşimli olarak tanımak ve yakalanan sesten metin dökümünü almak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Bu özelliği, uygulama ve cihazlarınızla ilgili sık sorulan konuşma konuşmaları gibi yaygın tanıma görevleri için tümleştirmeniz kolaydır. Ayrıca, ses yardımcıları oluşturmak için konuşma SDK 'Sı ile bot çerçevesini kullanma gibi daha karmaşık Tümleştirmeler için de kullanılabilir.

Birkaç önkoşul karşıladıktan sonra, konuşmayı bir mikrofondan tanımayı yalnızca dört adım sürer:

> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesnesi oluşturun.
> * Yukarıdaki `SpeechConfig` nesnesini kullanarak bir `SpeechRecognizer` nesnesi oluşturun.
> * `SpeechRecognizer` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen `SpeechRecognitionResult` inceleyin.
