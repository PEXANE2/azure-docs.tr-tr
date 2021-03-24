---
title: Konuşma çevirisine genel bakış-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma çevirisi, uygulamalarınıza, araçlara ve cihazlarınıza uçtan uca, gerçek zamanlı, çok dilli bir konuşma çevirisi eklemenize olanak tanır. Aynı API hem konuşmayı konuşmaya hem de konuşmayı metne çevirmek için kullanılabilir. Bu makale, konuşma çevirisi hizmetinin avantajları ve özelliklerine genel bir bakış sunar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: erhopf
ms.custom: devx-track-csharp, cog-serv-seo-aug-2020
keywords: konuşma çevirisi
ms.openlocfilehash: 94ddd06068513261b5b73b313877e273c7251d62
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "104954971"
---
# <a name="what-is-speech-translation"></a>Konuşma çevirisi nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Bu genel bakışta, ses akışlarının gerçek zamanlı, [çok dilli konuşmadan](language-support.md#speech-translation) konuşmaya ve konuşmaya metin çevirisi sağlayan konuşma çevirisi hizmetinin avantajları ve özellikleri hakkında bilgi edinebilirsiniz. Konuşma SDK 'Sı ile uygulamalarınızın, araçların ve cihazlarınızın, sunulan ses için kaynak dökümlerini ve çeviri çıkışlarını erişimi vardır. Okuma tespit edildiğinde ve nihai sonuçlar sentezleştirilmiş konuşmaya dönüştürülebileceğinden, geçici döküm ve çeviri sonuçları döndürülür.

## <a name="core-features"></a>Temel Özellikler

* Tanıma sonuçlarıyla konuşmaya metin çevirisi.
* Konuşmadan konuşmaya çevirisi.
* Birden çok hedef dile çeviri desteği.
* Ara tanıma ve çeviri sonuçları.

## <a name="get-started"></a>başlarken 

Konuşma çevirisi 'ni kullanmaya başlamak için [hızlı](get-started-speech-translation.md) başlangıca bakın. Konuşma çevirisi hizmeti, konuşma [SDK 'sı](speech-sdk.md) ve [konuşma CLI](spx-overview.md)aracılığıyla kullanılabilir.

## <a name="sample-code"></a>Örnek kod

Konuşma SDK 'Sı için örnek kod GitHub ' da kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek kararlı tanıma/çeviri gibi yaygın senaryoları kapsar ve özel modellerle çalışır.

* [Konuşmadan metne ve çeviri örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Geçiş kılavuzları

Uygulamalarınız, araçlar veya ürünleriniz [Translator konuşma çevirisi API'si](./how-to-migrate-from-translator-speech-api.md)kullanıyorsa, konuşma hizmetine geçiş yapmanıza yardımcı olacak kılavuzlar oluşturduk.

* [Translator Konuşma Çevirisi API'si konuşma hizmetine geçirme](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](./speech-sdk.md)
* [Konuşma Cihazları SDK’sı](speech-devices-sdk.md)
* [REST API: konuşmayı metne dönüştürme](rest-speech-to-text.md)
* [REST API: metinden konuşmaya](rest-text-to-speech.md)
* [REST API: toplu Iş dökümü ve özelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)

## <a name="next-steps"></a>Sonraki adımlar

* Konuşma çevirisi [hızlı](get-started-speech-translation.md) başlangıcını doldurun
* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](overview.md#try-the-speech-service-for-free)
* [Konuşma SDK 'sını alın](speech-sdk.md)