---
title: Ses yardımcıları-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma yazılım geliştirme seti 'ni (SDK) kullanan sesli Yardımcılar için özelliklere, özelliklere ve sınırlamalara genel bakış.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: c97f6414876441290cade68b8f9a054970586402
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73507742"
---
# <a name="about-voice-assistants"></a>Sesli yardımcılar hakkında

Azure konuşma Hizmetleri 'ni kullanan sesli yardımcılar, geliştiricilerin uygulamaları ve deneyimleri için doğal ve insan benzeri konuşma arabirimleri oluşturmalarına olanak sağlar.

Ses Yardımcısı hizmeti, (1) bot çerçevesinin doğrudan hat konuşma kanalını ya da (2) görev tamamlama için tümleşik özel komutlar (Önizleme) hizmetini kullanan bir cihaz ile yardımcı uygulama arasında hızlı ve güvenilir bir etkileşim sağlar.

Uygulamalar, konuşma yazılım geliştirme seti (SDK) ile ses Yardımcısı hizmetine bağlanır.

   ![Ses Yardımcısı düzenleme hizmeti akışının kavramsal diyagramı](media/voice-assistants/overview.png "Sesli yardım akışı")

## <a name="core-features"></a>Temel Özellikler

| Kategori | Özellikler |
|----------|----------|
|[Özel anahtar sözcük](speech-devices-sdk-create-kws.md) | Kullanıcılar, "Hey contoso" gibi özel bir anahtar sözcükle konuşmaları kullanarak konuşmalar başlatabilir. Uygulama bunu konuşma SDK 'sında özel bir anahtar sözcük altyapısı ile yapar ve [burada oluşturabileceğiniz](speech-devices-sdk-create-kws.md)özel bir anahtar sözcükle yapılandırılabilir. Sesli yardımcılar, anahtar sözcük etkinleştirmenin doğruluğunu artırmak için hizmet tarafı anahtar sözcük doğrulamasını kullanabilir (tek başına cihaz).
|[Konuşmayı metne dönüştürme](speech-to-text.md) | Sesli yardımcılar, Azure konuşma hizmetlerinden [konuşmaya metin](speech-to-text.md) kullanarak gerçek zamanlı sesi tanınan metne dönüştürür. Bu metin, hem yardımcınızın hem de istemci uygulamanızın bulunduğu bir şekilde kullanılabilir.
|[Metinden konuşmaya](text-to-speech.md) | Yardımcınızdan metinsel yanıtlar, Azure konuşma hizmetlerinden gelen [metinden konuşmaya](text-to-speech.md) kullanılarak dağıtılır. Bu birleştirme daha sonra istemci uygulamanız için bir ses akışı olarak sunulur. Microsoft, markanızla ilgili bir ses sağlayan kendi özel, yüksek kaliteli sinir TTS sesinizi oluşturma olanağı sunar. Daha fazla bilgi edinmek için [bizimle iletişime geçin](mailto:mstts@microsoft.com).

## <a name="comparing-assistant-solutions"></a>Yardımcı çözümlerini karşılaştırma

Ses Yardımcısı hizmeti, şirket içi uygulamanızı benzersiz yardımcı uygulamanıza bağlar. Geliştiriciler, (1) bot çerçevesinin [doğrudan hat konuşma](direct-line-speech.md) kanalını ya da (2) [özel komutlar (Önizleme)](custom-commands.md) çözümünü kullanarak sesli yardımcılar yazar.

   ![Yardımcı çözümlerin karşılaştırması](media/voice-assistants/assistant-solution-comparison.png "Yardımcı çözümlerin karşılaştırması")

| Çözüm | Özellikler |
|----------|----------|
|[Özel komutlar (Önizleme)](custom-commands.md) | Özel komutlar (Önizleme), sesli Yardımcılar için kolaylaştırılmış bir yazma ve barındırma çözümü sağlar. Görev tamamlama ve komut ve denetim senaryolarının ihtiyaçlarına göre tasarlanmıştır.
|[Doğrudan konuşma hattı](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) | Doğrudan hat konuşma, (1) istemci uygulamanız (2) ile uyumlu bir bot ve (3) Azure konuşma hizmetlerinin özellikleri arasında sorunsuz ve sorunsuz bir bağlantı imkanı sunar. Botunuzu doğrudan hat konuşma kanalını kullanacak şekilde yapılandırma hakkında daha fazla bilgi için lütfen [bot Framework belgelerindeki sayfasına](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)bakın.

Bu çözümlerden biriyle bir ses Yardımcısı oluşturduktan sonra, konuşma SDK 'sindeki `DialogServiceConnector` kullanarak, şirket içi uygulamanızı bu cihaza bağlayın. Daha fazla ayrıntı için lütfen her çözüm için hızlı başlangıçlara ve örneklere bakın.

## <a name="getting-started-with-voice-assistants"></a>Sesli yardımcılar ile çalışmaya başlama

Kodu, 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanan hızlı başlangıç sunuyoruz. Bu tablo, dile göre düzenlenen sesli yardım hızlı başlangıçlarını bir listesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| C#, UWP | Windows | [Ata](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Ata](https://aka.ms/csspeech/javaref) |
| Java | Android | [Ata](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Örnek kod

Bir ses Yardımcısı oluşturmaya yönelik örnek kod GitHub ' da kullanılabilir. Bu örnekler, çeşitli popüler programlama dillerinde Yardımcınıza bağlanmak için istemci uygulamasını kapsar.

* [Ses Yardımcısı örnekleri (SDK)](https://aka.ms/csspeech/samples)
* [Öğretici: konuşma SDK 'Sı ile yardımcınızı etkinleştirme,C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="tutorial"></a>Öğretici

[Konuşma SDK 'sını ve doğrudan hat konuşma kanalını kullanarak yardımcınızı sesli olarak nasıl etkinleştireceğinizi](tutorial-voice-enable-your-bot-speech-sdk.md)gösteren bir öğretici.

## <a name="customization"></a>Özelleştirme

Azure konuşma Hizmetleri kullanılarak oluşturulan sesli yardımcılar, [konuşmadan metne](speech-to-text.md), [metinden konuşmaya](text-to-speech.md)ve [özel anahtar sözcük seçimi](speech-devices-sdk-create-kws.md)için kullanılabilen özelleştirme seçeneklerinin tam aralığını kullanabilir.

> [!NOTE]
> Özelleştirme seçenekleri dile/yerel ayara göre değişir (bkz. [desteklenen diller](supported-languages.md)).

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma Hizmetleri abonelik anahtarı alın](get-started.md)
* [Konuşma SDK 'sını alın](speech-sdk.md)
* [Özel komutlar hakkında daha fazla bilgi edinin (Önizleme)](custom-commands.md)
* [Doğrudan hat konuşma hakkında daha fazla bilgi edinin](direct-line-speech.md)