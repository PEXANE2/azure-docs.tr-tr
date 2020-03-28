---
title: 'Quickstart: Mikrofondan konuşmayı tanıma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/20/2019
ms.author: erhopf
ms.openlocfilehash: ab51fe0323ea23c16cb52aa7a0570f8d51d40b2f
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75468419"
---
Bu hızlı başlatmada, konuşmanın mikrofon girişiyle etkileşimli olarak tanınması ve yakalanan sesten metin transkripsiyonu almak için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanırsınız. Konuşmaları aktarma gibi yaygın tanıma görevleri için bu özelliği uygulamalarınız veya cihazlarınızla entegre etmek kolaydır. Ayrıca, ses asistanları oluşturmak için Konuşma SDK ile Bot Framework kullanarak gibi daha karmaşık entegrasyonlar için kullanılabilir.

Birkaç ön koşulu yerine getirerek, konuşmayı mikrofondan tanıma yalnızca dört adım dan geçer:

> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * Nesneyi `SpeechRecognizer` yukarıdan `SpeechConfig` kullanarak bir nesne oluşturun.
> * Nesneyi `SpeechRecognizer` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade `SpeechRecognitionResult` edilenleri inceleyin.
