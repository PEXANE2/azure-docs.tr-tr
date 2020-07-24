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
ms.openlocfilehash: 60bcdac0d7e19c424b007980294898638814d586
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87044875"
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

## <a name="get-started-with-custom-keywords"></a>Özel anahtar sözcükler ile çalışmaya başlama

* Bkz. temel kullanım ve tasarım desenleri için [özel anahtar sözcük temelleri](custom-keyword-basics.md) .
* [C# kullanarak ürününüzü konuşma SDK 'sı ile nasıl etkinleştireceğinize](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="see-samples-on-github"></a>GitHub 'da örneklere bakın

* [C kullanarak Evrensel Windows Platformu konuşma SDK 'Sı ile anahtar sözcükleri tanıma #](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/uwp/keyword-recognizer)
* [Java kullanarak Android 'de konuşma SDK 'Sı ile anahtar sözcükleri tanıma](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/keyword-recognizer)

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
* [Konuşma SDK 'sını alın](speech-sdk.md)
* [Sesli yardımcılar hakkında daha fazla bilgi edinin](voice-assistants.md)
