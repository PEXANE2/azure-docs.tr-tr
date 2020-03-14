---
title: Doğrudan hat konuşma konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma yazılım geliştirme seti (SDK) ile doğrudan hat konuşmayı kullanan ses yardımcılarının özelliklerine, özelliklerine ve kısıtlamalarına genel bakış.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: dapine
ms.openlocfilehash: 714cdefa7100160a510d1922a2547ae53692d76c
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79367814"
---
# <a name="what-is-direct-line-speech"></a>Direct Line Speech nedir?

**Doğrudan hat konuşma** , esnek ve genişletilebilir bir ses Yardımcısı oluşturmaya yönelik sağlam, uçtan uca bir çözümdür. Bu, bot çerçevesi ve doğrudan hat konuşma kanalı tarafından desteklenir. Bu, botlar ile sesli ve sesli etkileşim için iyileştirilmiştir.

[Ses yardımcıları](voice-assistants.md) kullanıcıları dinler ve genellikle geri konuşarak bir işlem gerçekleştirebilir. Bu kişiler, kullanıcının konuşma işlemini yapmak için [konuşmayı metne](speech-to-text.md) dönüştürme özelliğini kullanır ve ardından metnin doğal dilde anlaşılmasına ilişkin işlem yapın. Bu eylem sıklıkla, [metinden konuşmaya](text-to-speech.md)ile oluşturulan yardımcdan konuşulan çıktıyı içerir.

Doğrudan hat konuşma, sesli Yardımcılar için en yüksek özelleştirme ve gelişmiş algoritmaların mümkündür düzeylerini sunmaktadır. Bu, görev tamamlama veya komut ve denetim kullanımı ile birlikte açık uçlu, doğal veya sonlandırımlar olan konuşma senaryoları için tasarlanmıştır. Bu yüksek düzeyde esneklik daha büyük bir karmaşıklıkla gelir ve doğal dil girişi kullanılarak iyi tanımlanmış görevler kapsamındaki senaryolar, kolaylaştırılmış bir çözüm deneyimi için [özel komutları (Önizleme)](custom-commands.md) düşünmek isteyebilir.

## <a name="getting-started-with-direct-line-speech"></a>Doğrudan hat konuşmaya başlarken

Doğrudan hat konuşmayı kullanarak bir ses Yardımcısı oluşturmaya yönelik ilk adım, [bir konuşma aboneliği anahtarı almak](get-started.md), bu abonelikle ilişkilendirilmiş yeni bir bot oluşturmak ve bot 'U doğrudan hat konuşma kanalına bağlamak içindir.

   ![Doğrudan hat konuşma düzenleme hizmeti akışının kavramsal diyagramı](media/voice-assistants/overview-directlinespeech.png "Konuşma kanalı akışı")

Doğrudan hat konuşmayı kullanarak basit bir ses Yardımcısı oluşturmaya yönelik tam bir adım adım kılavuz için, konuşma [için konuşma öğreticisine ve doğrudan hat konuşma kanalına sahip konuşmayı etkinleştirme öğreticisine](tutorial-voice-enable-your-bot-speech-sdk.md)bakın.

Ayrıca, kod çalıştırıp API 'Leri hızla öğrenmenizi sağlayacak hızlı başlangıçlara da sunuyoruz. Bu tablo, dile ve platforma göre düzenlenen sesli yardım hızlı başlangıçlarını bir listesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| C#, UWP | Windows | [Ata](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Ata](https://aka.ms/csspeech/javaref) |
| Java | Android | [Ata](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Örnek kod

Bir ses Yardımcısı oluşturmaya yönelik örnek kod GitHub ' da kullanılabilir. Bu örnekler, çeşitli popüler programlama dillerinde Yardımcınıza bağlanmak için istemci uygulamasını kapsar.

* [Ses Yardımcısı örnekleri (SDK)](https://aka.ms/csspeech/samples)
* [Öğretici: konuşma SDK 'Sı ile yardımcınızı etkinleştirme,C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="customization"></a>Özelleştirme

Konuşma hizmeti kullanılarak oluşturulan sesli yardımcılar, [konuşmadan metne](speech-to-text.md), [metinden konuşmaya](text-to-speech.md)ve [özel anahtar sözcük seçimi](speech-devices-sdk-create-kws.md)için kullanılabilen özelleştirme seçeneklerinin tam aralığını kullanabilir.

> [!NOTE]
> Özelleştirme seçenekleri dile/yerel ayara göre değişir (bkz. [desteklenen diller](supported-languages.md)).

Ses yardımcıları için doğrudan hat konuşmayı ve ilgili işlevselliği, [sanal yardımcı çözümü ve kurumsal şablon](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview)için ideal bir ektir. Doğrudan hat konuşması uyumlu bir bot ile çalışabilse de, bu kaynaklar yüksek kaliteli konuşma deneyimleri için yeniden kullanılabilir bir temel ve hızla çalışmaya başlamak için ortak destekleyici beceriler ve modeller sağlar.

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
* [Konuşma SDK 'sını alın](speech-sdk.md)
* [Temel bot oluşturma ve dağıtma](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Sanal yardımcı çözümü ve kurumsal şablonu al](https://github.com/Microsoft/AI)
