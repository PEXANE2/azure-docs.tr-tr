---
title: 'Quickstart: Özel bir ses asistanı oluşturma - Konuşma hizmeti'
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
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80241146"
---
Bu hızlı başlatmada, daha önce yazdığınız ve yapılandırdığınız bir bota bağlanan özel bir ses asistanı uygulaması oluşturmak için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Bir bot oluşturmanız gerekiyorsa, daha kapsamlı bir kılavuz için [ilgili öğreticiye](~/articles/cognitive-services/speech-service/tutorial-voice-enable-your-bot-speech-sdk.md) bakın.

Birkaç ön koşulu yerine getirerek, özel sesli yardımcınızı bağlamak yalnızca birkaç adımdan oluşur:
> [!div class="checklist"]
> * Abonelik `BotFrameworkConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Nesneyi `DialogServiceConnector` yukarıdan `BotFrameworkConfig` kullanarak bir nesne oluşturun.
> * Nesneyi `DialogServiceConnector` kullanarak, tek bir söyleyiş için dinleme işlemini başlatın.
> * İade `ActivityReceivedEventArgs` edilenleri inceleyin.
