---
title: 'Hızlı başlangıç: ses dosyasına konuşma Sentezleştirme-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 7d9a03f5a57473ce651560b261a4cf84b4ca4824
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818008"
---
Bu hızlı başlangıçta, bir ses dosyasında metni sentezleştirilmiş konuşmaya dönüştürmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşul karşıladıktan sonra, konuşma bir dosyaya senile yalnızca beş adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechConfig```` nesnesi oluşturun.
> * Öğesini belirten bir ses yapılandırma nesnesi oluşturun. WAV dosya adı.
> * Yukarıdaki yapılandırma nesnelerini kullanarak bir ````SpeechSynthesizer```` nesnesi oluşturun.
> * ````SpeechSynthesizer```` nesnesini kullanarak, metninizi sentezleştirilmiş konuşmaya dönüştürerek belirtilen ses dosyasına kaydederek metninizi birleştirilmiş konuşmaya dönüştürün.
> * Hatalar için döndürülen ````SpeechSynthesizer```` inceleyin.
