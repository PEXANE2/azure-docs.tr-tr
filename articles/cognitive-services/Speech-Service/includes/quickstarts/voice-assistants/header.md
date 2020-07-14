---
title: 'Hızlı başlangıç: özel bir ses Yardımcısı oluşturma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 03/20/2020
ms.author: travisw
ms.openlocfilehash: bb88ef7f048b2454f05ef33286e59277070c3ea0
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80241146"
---
Bu hızlı başlangıçta, zaten yazdığınız ve yapılandırdığınız bir bot 'a bağlanan özel bir ses Yardımcısı uygulaması oluşturmak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Bir bot oluşturmanız gerekiyorsa, daha kapsamlı bir kılavuz için [ilgili öğreticiye](~/articles/cognitive-services/speech-service/tutorial-voice-enable-your-bot-speech-sdk.md) bakın.

Birkaç önkoşulu karşıladıktan sonra, özel sesli yardımcınızı bağlamak yalnızca birkaç adım sürer:
> [!div class="checklist"]
> * `BotFrameworkConfig`Abonelik anahtarınızdan ve bölgenizde bir nesne oluşturun.
> * `DialogServiceConnector`Yukarıdaki nesneyi kullanarak bir nesne oluşturun `BotFrameworkConfig` .
> * Nesnesini kullanarak `DialogServiceConnector` tek bir söyme için dinleme sürecini başlatın.
> * Döndürülen ' i inceleyin `ActivityReceivedEventArgs` .
