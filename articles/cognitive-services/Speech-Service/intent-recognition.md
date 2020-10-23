---
title: Amaç tanıma genel bakış-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Amaç tanıma, önceden tanımladığınız Kullanıcı hedeflerini tanımanıza olanak sağlar. Bu makale, amaç tanıma hizmetinin avantajları ve özelliklerine genel bir bakış sunar.
services: cognitive-services
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: v-demjoh
keywords: Amaç tanıma
ms.openlocfilehash: 0d718459e0fd0ea410232d3a165b560aa8c59cd1
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92174646"
---
# <a name="what-is-intent-recognition"></a>Amaç tanıma nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Bu genel bakışta, amaç tanımanın avantajları ve özellikleri hakkında bilgi edineceksiniz. Bilişsel Hizmetler Konuşma SDK’sıamaç tanımayı sağlamak için Language Understanding hizmetiyle (LUIS) tümleştirilmiştir. Amaç, kullanıcının yapmak istediği herhangi bir şeydir: uçak rezervasyonu, hava durumuna bakma veya telefon etme.
Amaç tanımayı kullanarak, uygulamalarınız, araçları ve cihazlarınız, LUO 'da tanımladığınız seçeneklere göre kullanıcının ne kadar veya ne yaptığını belirleyebilir.

## <a name="luis-key-required"></a>LUSıS anahtarı gereklidir

* LUIS, konuşmadaki amaçları tanımak için Konuşma hizmetiyle tümleştirilir. Konuşma hizmeti aboneliğine ihtiyacınız yoktur; LUIS yeterlidir.
* Konuşma amacı tanıma, SDK ile tümleşiktir. Konuşma hizmetiyle bir LUO anahtarı kullanabilirsiniz.
* Konuşma SDK 'Sı aracılığıyla amaç tanıma, [luya tarafından desteklenen bölgelerin bir alt kümesiyle sunulur](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#intent-recognition).

## <a name="get-started"></a>başlarken

Amaç tanımayı kullanmaya başlamak için [hızlı](quickstarts/intent-recognition.md) başlangıca bakın.

## <a name="sample-code"></a>Örnek kod

Amaç tanıma için örnek kod:

* [Hızlı başlangıç: Önceden oluşturulmuş ev otomasyonu uygulamasını kullanma](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app)
* [C için konuşma SDK 'sını kullanarak konuşma amaçlarını tanıma #](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp)
* [C 'de Unity kullanarak amaç tanıma ve diğer konuşma Hizmetleri #](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/unity/speechrecognizer)
* [Python için konuşma SDK 'sını kullanarak hedefleri tanıma](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/python/console)
* [Windows 'da C++ için konuşma SDK 'sını kullanan amaç tanıma ve diğer konuşma Hizmetleri](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/cpp/windows/console)
* [Windows veya Linux 'ta Java için konuşma SDK 'sını kullanan amaç tanıma ve diğer konuşma Hizmetleri](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/java/jre/console)
* [Web tarayıcısında JavaScript için konuşma SDK 'sını kullanan amaç tanıma ve diğer konuşma Hizmetleri](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/js/browser)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)

## <a name="next-steps"></a>Sonraki adımlar

* Amaç tanıma [hızlı](quickstarts/intent-recognition.md) başlangıcını doldurun
* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](overview.md#try-the-speech-service-for-free)
* [Konuşma SDK 'sını alın](speech-sdk.md)
