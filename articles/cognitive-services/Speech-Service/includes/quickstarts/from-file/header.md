---
title: 'Hızlı başlangıç: ses dosyasından konuşmayı tanıma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/14/2020
ms.author: trbye
ms.openlocfilehash: a7c91775411f100a92dfcb0a7199dd4b25f6eca4
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400470"
---
Bu hızlı başlangıçta, ses dosyasından konuşmayı tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşulu karşıladıktan sonra, bir dosyadan konuşmayı tanımayı yalnızca birkaç adım sürer:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir `SpeechConfig` nesne oluşturun.
> * Öğesini belirten `AudioConfig` bir nesne oluşturun. WAV dosya adı.
> * Yukarıdaki `SpeechRecognizer` `SpeechConfig` ve `AudioConfig` nesnelerini kullanarak bir nesne oluşturun.
> * `SpeechRecognizer` Nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * `SpeechRecognitionResult` Döndürülen ' i inceleyin.
