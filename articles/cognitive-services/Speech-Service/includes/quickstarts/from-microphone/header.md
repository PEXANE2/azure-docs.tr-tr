---
title: 'Hızlı başlangıç: bir mikrofon konuşma hizmetinden konuşmayı tanıma'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/20/2019
ms.author: erhopf
ms.openlocfilehash: ab51fe0323ea23c16cb52aa7a0570f8d51d40b2f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75468419"
---
Bu hızlı başlangıçta, bir mikrofon girişinden konuşmayı etkileşimli olarak tanımak ve yakalanan sesten metin dökümünü almak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Bu özelliği, uygulama ve cihazlarınızla ilgili sık sorulan konuşma konuşmaları gibi yaygın tanıma görevleri için tümleştirmeniz kolaydır. Ayrıca, ses yardımcıları oluşturmak için konuşma SDK 'Sı ile bot çerçevesini kullanma gibi daha karmaşık Tümleştirmeler için de kullanılabilir.

Birkaç önkoşul karşıladıktan sonra, konuşmayı bir mikrofondan tanımayı yalnızca dört adım sürer:

> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesnesi oluşturun.
> * Yukarıdaki `SpeechConfig` nesnesini kullanarak bir `SpeechRecognizer` nesnesi oluşturun.
> * `SpeechRecognizer` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen `SpeechRecognitionResult` inceleyin.
