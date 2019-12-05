---
title: 'Hızlı başlangıç: Sentezleştirme konuşma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak konuşmayı nasıl sentezleyeceğinizi öğrenin
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: d19f779f67cc0dea8cc7f06aa275885d75c3092e
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818349"
---
Bu hızlı başlangıçta, metni sentezleştirilmiş konuşmaya dönüştürmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşul karşıladıktan sonra, birleştirilmiş konuşmayı varsayılan hoparlörlere işleme yalnızca dört adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechConfig```` nesnesi oluşturun.
> * Yukarıdaki ````SpeechConfig```` nesnesini kullanarak bir ````SpeechSynthesizer```` nesnesi oluşturun.
> * Metni konuşmak için ````SpeechSynthesizer```` nesnesini kullanma.
> * Hatalar için döndürülen ````SpeechSynthesisResult```` denetleyin.
