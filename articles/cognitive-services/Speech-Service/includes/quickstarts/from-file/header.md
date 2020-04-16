---
title: 'Quickstart: Bir ses dosyasından konuşmayı tanıma - Konuşma hizmeti'
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400470"
---
Bu hızlı başlatmada, bir ses dosyasından konuşmayı tanımak için [Konuşma SDK'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç ön koşulu yerine getirerek, bir dosyadan konuşmayı tanıma yalnızca birkaç adım dan alır:
> [!div class="checklist"]
> * Abonelik `SpeechConfig` anahtarınızdan ve bölgenizden bir nesne oluşturun.
> * 'yi `AudioConfig` belirten bir nesne oluşturun. WAV dosya adı.
> * Yukarıdan `SpeechRecognizer` `SpeechConfig` ve `AudioConfig` nesneleri kullanarak bir nesne oluşturun.
> * Nesneyi `SpeechRecognizer` kullanarak, tek bir söyleyiş için tanıma işlemini başlatın.
> * İade `SpeechRecognitionResult` edilenleri inceleyin.
