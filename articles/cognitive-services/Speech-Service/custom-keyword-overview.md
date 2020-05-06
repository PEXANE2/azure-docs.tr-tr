---
title: Özel anahtar sözcükler-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma yazılım geliştirme seti 'ni (SDK) kullanarak özel anahtar sözcüklere yönelik özelliklere, özelliklere ve sınırlamalara genel bakış.
services: cognitive-services
author: hasyashah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: hasshah
ms.openlocfilehash: dd5748cf8afe19a49e5ea406aea9b558432eeaf3
ms.sourcegitcommit: c8a0fbfa74ef7d1fd4d5b2f88521c5b619eb25f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82802186"
---
# <a name="what-is-a-keyword"></a>Anahtar sözcük nedir?

Anahtar sözcüğü, ürününüzün ses özellikli olmasını sağlayan bir kelime veya kısa bir tümceciktir. Örneğin, "Hey Cortana", Cortana yardımcısı için anahtar sözcüktür. Ses etkinleştirme, kullanıcılarınızın yalnızca anahtar sözcüğünü konuşarak tamamen ücretsiz olarak sorunsuz bir şekilde etkileşim kurmaya başlamasını sağlar. Ürününüz anahtar sözcüğü sürekli dinliyorsa, verilerin mümkün olduğunca özel kalmasını sağlamak için bir algılama gerçekleşene kadar tüm sesler yerel olarak Kullanıcı cihazında işlenir.

## <a name="core-features-of-custom-keyword"></a>Özel anahtar sözcüğünün temel özellikleri

Özel anahtar sözcüğünün özelleştirme, performans ve tümleştirme özellikleriyle, ses etkinleştirmeyi ürününüzün vizyonunun ve kullanıcılarınızın ihtiyaçlarına en iyi şekilde uyarlayabilirsiniz.

| Özellik | Açıklama |
|----------|----------|
| Anahtar sözcük özelleştirmesi | Markanızınızın bir uzantısı olarak, bir anahtar sözcük, müşterilerinizle oluşturduğunuz öz sermaye 'yi zorlar. Konuşma Studio 'daki özel anahtar sözcük portalı, markanızı en iyi şekilde temsil eden herhangi bir kelime veya kısa tümcecik belirtmenize olanak tanır. Anahtar kelimenizi, oluşturulan anahtar sözcük modeli tarafından kabul edilecek doğru söylenişleri seçerek daha fazla kişiselleştirmek için kullanabilirsiniz.
| Anahtar sözcük doğrulama | Yerel olarak algılanmakta olan anahtar kelimesinin yüksek güvenilirlikli olduğunda, kullanıcının anahtar sözcüğünü söylemesinin daha fazla doğrulanması için ses buluta gönderilir. Anahtar sözcük doğrulama, yanlış yerel algılamanın etkilerini azaltarak ve Kullanıcı gizliliğini koruarak ek bir güvenlik katmanı sağlar.
| Ses Yardımcısı & konuşma SDK tümleştirmesi | Konuşma Studio 'daki özel anahtar sözcükten oluşturulan anahtar sözcükler, konuşma SDK 'Sı aracılığıyla cihazınız veya uygulamanız dahilinde kolayca tümleştirilebilir. SDK 'yı yalnızca konuşma Studio tarafından sunulan anahtar sözcük modeline işaret eder ve ürününüz, anahtar sözcük doğrulaması tarafından desteklenen sesli olarak etkinleştirilir. Kendi [Ses yardımcınızı](voice-assistants.md)oluşturarak ürününüzün ses deneyimlerini tamamlayabilirsiniz.

## <a name="sample-code"></a>Örnek kod

Bir anahtar sözcük modelinin tümleştirilmesine yönelik örnek kod GitHub ' da kullanılabilir. Bu örnekler, anahtar keliinizi birçok popüler programlama dilinde tümleştirmek için istemci uygulamasını kapsar.

* [Öğretici: konuşma SDK 'Sı ile ürününüzü etkinleştirin, C #](tutorial-voice-enable-your-bot-speech-sdk.md)
* [Ses Yardımcısı örnekleri (SDK)](https://aka.ms/csspeech/samples)

## <a name="tutorial"></a>Eğitmen

* [Konuşma Studio kullanarak özel bir anahtar sözcük oluşturma](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws).
* [Konuşma SDK 'sını kullanarak ürününüzü nasıl etkinleştireceğinize](tutorial-voice-enable-your-bot-speech-sdk.md).

## <a name="reference-docs"></a>Başvuru belgeleri

* [Özel anahtar sözcük adlandırma yönergeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-kws-guidelines)
* [Konuşma SDK'sı](speech-sdk-reference.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
* [Özel anahtar sözcük oluşturma](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws)
* [Konuşma SDK 'sını alın](speech-sdk.md)
* [Sesli yardımcılar hakkında daha fazla bilgi edinin](voice-assistants.md)
