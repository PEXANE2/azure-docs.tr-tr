---
title: Özel ses-ilk sanal yardımcılar (Önizleme)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bot Framework ve bilişsel hizmetler konuşma yazılım geliştirme seti (SDK) üzerindeki doğrudan hat konuşma kanalını kullanan özel ses-ilk sanal yardımcılarının özelliklerine, özelliklerine ve kısıtlamalarına genel bakış.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: travisw
ms.openlocfilehash: f77a95af1edcd37043ea1e067560577140a81a04
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967601"
---
# <a name="about-custom-voice-first-virtual-assistants-preview"></a>Özel ses-ilk sanal yardımcılar önizlemesi hakkında

Azure konuşma Hizmetleri 'ni kullanan özel sanal yardımcılar, geliştiricilerin uygulamaları ve deneyimleri için doğal ve insan benzeri konuşma arabirimleri oluşturmalarına olanak sağlar. Bot çerçevesinin doğrudan hat konuşma kanalı, daha düşük gecikme süresi ve yüksek güvenilirliğe sahip ses girişi, ses çıkış etkileşimini sağlayan uyumlu bir bot 'a Eşgüdümlü, düzenlenmiş bir giriş noktası sağlayarak bu özellikleri geliştirir. Bu botlar, doğal dil etkileşimi için Microsoft 'un Language Understanding (LUO) kullanabilir. Doğrudan hat konuşmaya, konuşma yazılım geliştirme seti (SDK) kullanılarak cihazlar tarafından erişilir.

   ![Doğrudan hat konuşma düzenleme hizmeti akışının kavramsal diyagramı](media/voice-first-virtual-assistants/overview.png "Konuşma kanalı akışı")


Özel ses-ilk sanal Yardımcılar için doğrudan hat konuşmayı ve ilişkili işlevselliği, [sanal yardımcı çözümü ve kurumsal şablonuna](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview)yönelik ideal bir ektir. Doğrudan hat konuşması uyumlu bir bot ile çalışabilse de, bu kaynaklar yüksek kaliteli konuşma deneyimleri için yeniden kullanılabilir bir temel ve hızla çalışmaya başlamak için ortak destekleyici beceriler ve modeller sağlar.


## <a name="core-features"></a>Temel Özellikler

| Category | Özellikler |
|----------|----------|
|[Özel uyandırma sözcüğü](speech-devices-sdk-create-kws.md) | Kullanıcıların "Hey contoso" gibi özel bir anahtar sözcüğü kullanarak botlarla konuşmaları başlatmaya olanak sağlayabilirsiniz. Bu görev, [burada oluşturabileceğiniz](speech-devices-sdk-create-kws.md)özel bir uyanma sözcüğüyle yapılandırılabilen konuşma SDK 'sında özel bir uyandırma sözcük altyapısı ile gerçekleştirilir. Doğrudan hat konuşma kanalı, uyandırma sözcüğünün yalnızca cihazdan ve tek başına uyandığı doğruluğu artıran hizmet tarafı uyandırma sözcük doğrulamasını içerir.
|[Konuşmayı metne dönüştürme](speech-to-text.md) | Doğrudan hat konuşma kanalı, Azure konuşma hizmetlerinden [konuşmaya metin](speech-to-text.md) kullanarak, tanınan metne ses olarak gerçek zamanlı bir şekilde dönüştürme işlemi içerir. Bu metin, hem bot 'niz hem de istemci uygulamanız tarafından kullanıma hazır hale gelir.
|[Metinden konuşmaya](text-to-speech.md) | Botunuzun metinsel yanıtları, Azure konuşma hizmetlerinden gelen [metinden konuşmaya](text-to-speech.md) kullanılarak birleştirilmiştir. Bu birleştirme daha sonra istemci uygulamanız için bir ses akışı olarak kullanılabilir hale getirilir. Microsoft, bize daha fazla [iletişim](mailto:mstts@microsoft.com)sağlamak için markanızla bir ses sağlayan kendi özel, yüksek kalıtelı sinir TTS sesinizi oluşturma olanağı sunar.
|[Doğrudan konuşma hattı](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) | Bot Framework içindeki bir kanal olarak, doğrudan hat konuşma, istemci uygulamanız, uyumlu bir bot ve Azure konuşma hizmetlerinin özellikleri arasında sorunsuz ve sorunsuz bir bağlantı imkanı sunar. Botunuzu doğrudan hat konuşma kanalını kullanacak şekilde yapılandırma hakkında daha fazla bilgi için, [bot Framework belgelerindeki sayfasına](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)bakın.

## <a name="get-started-with-virtual-assistants"></a>Sanal yardımcılar ile çalışmaya başlama

Kodu, 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanan hızlı başlangıç sunuyoruz. Bu tablo, dile göre düzenlenen ses-ilk Sanal Yardımcısı hızlı başlangıçlarını bir listesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| C#, UWP | Windows | [Göz atma](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Göz atma](https://aka.ms/csspeech/javaref) |
| Java | Android | [Göz atma](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Örnek kod

Bir ses-ilk Sanal Yardımcısı oluşturmak için örnek kod GitHub ' da kullanılabilir. Bu örnekler, çok sayıda popüler programlama dilinde bot 'a bağlanmak için istemci uygulamasını kapsar.

* [Ses-ilk Sanal Yardımcısı örnekleri (SDK)](https://aka.ms/csspeech/samples)
* [Öğretici: Konuşmayı konuşma SDK 'Sı ile etkinleştirin,C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="tutorial"></a>Öğretici
[Konuşma SDK 'sını ve doğrudan hat konuşma kanalını kullanarak botunuzu sesli olarak etkinleştirmeye](tutorial-voice-enable-your-bot-speech-sdk.md)yönelik bir öğretici.

## <a name="customization"></a>Özelleştirme

Azure konuşma Hizmetleri kullanılarak oluşturulan sesli ilk sanal yardımcılar, [konuşmadan metne](speech-to-text.md), [metinden konuşmaya](text-to-speech.md)ve [özel anahtar sözcük seçimi](speech-devices-sdk-create-kws.md)için kullanılabilen tüm özelleştirme seçeneklerini kullanabilir.

> [!NOTE]
> Özelleştirme seçenekleri dile/yerel ayara göre değişir (bkz. [desteklenen diller](supported-languages.md)).

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma Hizmetleri abonelik anahtarı alın](get-started.md)
* [Konuşma SDK 'sını alın](speech-sdk.md)
* [Temel bot oluşturma ve dağıtma](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Sanal yardımcı çözümü ve kurumsal şablonu al](https://github.com/Microsoft/AI)
