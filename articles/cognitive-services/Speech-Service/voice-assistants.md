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
ms.date: 03/11/2020
ms.author: travisw
ms.openlocfilehash: 18a9de8a2eaa2364e89e831db8dab5cbbb061c10
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85299254"
---
# <a name="what-is-a-voice-assistant"></a>Ses yardımcısı nedir?

Konuşma hizmetini kullanan sesli yardımcılar, geliştiricilerin uygulamalar ve deneyimleri için doğal ve insan benzeri konuşma arabirimleri oluşturmalarına olanak sağlar.

Ses Yardımcısı hizmeti, bir cihaz ile bir yardımcı uygulama arasında hızlı ve güvenilir bir etkileşim sağlar ve botlarınıza ses özellikleri eklemek [için (Azure](direct-line-speech.md) bot hizmeti aracılığıyla) veya ses verme senaryolarına yönelik özel komutlardır.

## <a name="choosing-an-assistant-solution"></a>Yardımcı çözümü seçme

Bir ses Yardımcısı oluşturmaya yönelik ilk adım, ne yapacağına karar vermelidir. Konuşma hizmeti, yardımcı etkileşimlerinizi üretmek için birden çok ve tamamlayıcı çözüm sunar. Azure bot hizmeti 'ni kullanarak [doğrudan hat konuşma](direct-line-speech.md) kanalı ile oluşturulmuş esnek ve çok yönlü bir bot 'a ses ve ses çıkışı özellikleri ekleyebilir veya basit sesli bilgi verme senaryoları için [özel bir komut](custom-commands.md) uygulaması yazmanın basitliğini kullanmaktan yararlanabilirsiniz.

| İstersen... | Göz önüne alın... | Örneğin... |
|-------------------|------------------|----------------|
|Güçlü beceriler tümleştirmesi ve tam dağıtım denetimi ile açık uçlu konuşma | [Doğrudan hat konuşma](direct-line-speech.md) kanalı Ile Azure bot hizmeti bot | <ul><li>"Seattle 'a gitmem gerekiyor"</li><li>"Ne tür bir pizza düzenleyebilir?"</li></ul>
|Basitleştirilmiş yazma ve barındırma ile sesli komutlama veya basit görev odaklı konuşmalar | [Özel komutlar](custom-commands.md) | <ul><li>"Ek yük ışığını aç"</li><li>"5 derece çarpıtma yapın"</li><li>[Burada kullanılabilen](https://speech.microsoft.com/customcommands) diğer örnekler</li></ul>

Yardımcınızın hangi özellikleri işlemesini istediğinizi bilmiyorsanız en iyi varsayılan seçenek olarak [doğrudan ses konuşmanızı](direct-line-speech.md) öneririz. Ortak desenleri oluşturmak ve mevcut bilgi kaynaklarınızı kullanmak için, [sanal yardımcı çözümü ve kurumsal şablon](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) ve [soru-cevap oluşturma hizmeti](https://docs.microsoft.com/azure/cognitive-services/QnAMaker/Overview/overview) gibi zengin araç ve yazma yardımlarıyla tümleştirme sunar.

[Özel komutlar](custom-commands.md) , sesli ilk etkileşim deneyimleri için iyileştirilmiş zengin sesli uygulama uygulamaları oluşturmayı kolaylaştırır. Birleştirilmiş bir yazma deneyimi, otomatik barındırma modeli ve görece daha düşük karmaşıklık sağlar. böylece, sesli komutlama senaryolarınız için en iyi çözümü oluşturmaya odaklanmanıza yardımcı olur.

   ![Yardımcı çözümlerin karşılaştırması](media/voice-assistants/assistant-solution-comparison.png "Yardımcı çözümlerin karşılaştırması")


## <a name="reference-architecture-for-building-a-voice-assistant-using-the-speech-sdk"></a>Konuşma SDK 'sını kullanarak bir ses Yardımcısı oluşturmaya yönelik başvuru mimarisi

   ![Ses Yardımcısı düzenleme hizmeti akışının kavramsal diyagramı](media/voice-assistants/overview.png "Sesli yardım akışı")

## <a name="core-features"></a>Temel Özellikler

Yardımcı etkileşimlerinizi oluşturmak için [doğrudan hat konuşmayı](direct-line-speech.md) veya [özel komutlar](custom-commands.md) seçin, yardımcınızı marka, ürün ve kişiliğinizi özelleştirmek için zengin bir özelleştirme özellikleri kümesi kullanabilirsiniz.

| Kategori | Özellikler |
|----------|----------|
|[Özel anahtar sözcük](speech-devices-sdk-create-kws.md) | Kullanıcılar, "Hey contoso" gibi özel bir anahtar sözcükle konuşmaları kullanarak konuşmalar başlatabilir. Uygulama bunu konuşma SDK 'sında özel bir anahtar sözcük altyapısı ile yapar ve [burada oluşturabileceğiniz](speech-devices-sdk-create-kws.md)özel bir anahtar sözcükle yapılandırılabilir. Sesli yardımcılar, anahtar sözcük etkinleştirmenin doğruluğunu artırmak için hizmet tarafı anahtar sözcük doğrulamasını kullanabilir (tek başına cihaz).
|[Konuşmayı metne dönüştürme](speech-to-text.md) | Ses yardımcıları, konuşma hizmetinden [konuşmaya metin](speech-to-text.md) kullanarak gerçek zamanlı sesi tanınan metne dönüştürür. Bu metin, hem yardımcınızın hem de istemci uygulamanızın bulunduğu bir şekilde kullanılabilir.
|[Metinden konuşmaya](text-to-speech.md) | Yardımcınızdan metin yanıtları, konuşma hizmetinden gelen [metinden konuşmaya](text-to-speech.md) kullanılarak birleştirilmiştir. Bu birleştirme daha sonra istemci uygulamanız için bir ses akışı olarak sunulur. Microsoft, markanızla ilgili bir ses sağlayan kendi özel, yüksek kaliteli sinir TTS sesinizi oluşturma olanağı sunar. Daha fazla bilgi edinmek için [bizimle iletişime geçin](mailto:mstts@microsoft.com).

## <a name="getting-started-with-voice-assistants"></a>Sesli yardımcılar ile çalışmaya başlama

Kodu, 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanan hızlı başlangıç sunuyoruz. Bu tablo, dile göre düzenlenen sesli yardım hızlı başlangıçlarını bir listesini içerir.

* [Hızlı başlangıç: doğrudan hat konuşmayı kullanarak özel bir ses Yardımcısı oluşturma](quickstarts/voice-assistants.md)
* [Hızlı başlangıç: özel komutlar kullanarak bir sesli verme uygulaması oluşturma](quickstart-custom-commands-application.md)

## <a name="sample-code-and-tutorials"></a>Örnek kod ve öğreticiler

Bir ses Yardımcısı oluşturmaya yönelik örnek kod GitHub ' da kullanılabilir. Bu örnekler, çeşitli popüler programlama dillerinde Yardımcınıza bağlanmak için istemci uygulamasını kapsar.

* [GitHub 'da sesli yardım örnekleri](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant)
* [Öğretici: C# konuşma SDK 'Sı ile Azure bot hizmeti kullanılarak geliştirilmiş yardımcınızı etkinleştirin](tutorial-voice-enable-your-bot-speech-sdk.md)
* [Öğretici: basit sesli komutlarla özel bir komut uygulaması oluşturma](how-to-custom-commands-create-application-with-simple-commands.md)

## <a name="customization"></a>Özelleştirme

Azure konuşma Hizmetleri kullanılarak oluşturulan sesli yardımcılar, özelleştirme seçeneklerinin tam aralığını kullanabilir.

* [Özel Konuşma Tanıma](how-to-custom-speech.md)
* [Özel ses](how-to-custom-voice.md)
* [Özel Anahtar Sözcük](custom-keyword-overview.md)

> [!NOTE]
> Özelleştirme seçenekleri dile/yerel ayara göre değişir (bkz. [desteklenen diller](language-support.md)).

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
* [Özel komutlar hakkında daha fazla bilgi edinin](custom-commands.md)
* [Doğrudan hat konuşma hakkında daha fazla bilgi edinin](direct-line-speech.md)
* [Konuşma SDK 'sını alın](speech-sdk.md)