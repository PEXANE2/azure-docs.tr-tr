---
title: 'Hızlı başlangıç: ses dosyasına konuşma Sentezleştirme-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 9b13d8fc3b77426a59dea5399223b79c4bb4b1a1
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75467691"
---
Bu hızlı başlangıçta, bir ses dosyasında metni sentezleştirilmiş konuşmaya dönüştürmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşul karşıladıktan sonra, konuşma bir dosyaya senile yalnızca beş adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechConfig```` nesnesi oluşturun.
> * Öğesini belirten bir ses yapılandırma nesnesi oluşturun. WAV dosya adı.
> * Yukarıdaki yapılandırma nesnelerini kullanarak bir ````SpeechSynthesizer```` nesnesi oluşturun.
> * ````SpeechSynthesizer```` nesnesini kullanarak, metninizi sentezleştirilmiş konuşmaya dönüştürerek belirtilen ses dosyasına kaydederek metninizi birleştirilmiş konuşmaya dönüştürün.
> * Hatalar için döndürülen ````SpeechSynthesizer```` inceleyin.
