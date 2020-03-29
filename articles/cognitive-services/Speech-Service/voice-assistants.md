---
title: Ses asistanları - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma Yazılımı Geliştirme Kiti'ni (SDK) kullanarak sesli asistanların özelliklerine, yeteneklerine ve kısıtlamalarına genel bakış.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: travisw
ms.openlocfilehash: 9219c9a72ce9e1cfba3504b0b8e16ade77f8a5e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79369905"
---
# <a name="what-is-a-voice-assistant"></a>Ses yardımcısı nedir?

Konuşma hizmetini kullanan sesli asistanlar, geliştiricilerin uygulamaları ve deneyimleri için doğal, insana benzer konuşma arabirimleri oluşturmalarını sağlar.

Sesli asistan hizmeti, bir aygıt la (1) Bot Framework'ün Doğrudan Hat Konuşması kanalını veya görev tamamlama için tümleşik Özel Komutlar (Önizleme) hizmetini kullanan bir yardımcı uygulama arasında hızlı ve güvenilir etkileşim sağlar.

Uygulamalar Konuşma Yazılımı Geliştirme Kiti (SDK) ile sesli asistan hizmetine bağlanır.

   ![Ses asistanı orkestrasyon hizmet akışının kavramsal diyagramı](media/voice-assistants/overview.png "Ses asistanı akışı")

## <a name="choosing-an-assistant-solution"></a>Yardımcı çözüm seçme

Bir ses asistanı oluşturmanın ilk adımı ne yapması gerektiğine karar vermektir. Konuşma hizmeti, asistan etkileşimlerinizi oluşturmanız için birden fazla tamamlayıcı çözüm sağlar. İster Bot Framework'ün [Doğrudan Hat Konuşması](direct-line-speech.md) kanalının sağladığı esneklik ve çok yönlülük olsun, ister basit senaryolar için Özel [Komutların (Önizleme)](custom-commands.md) basitliğini istiyorsanız, doğru araçları seçmek başlamanızı sağlar.

| İstersen... | O zaman düşünün ... | Örneğin... |
|-------------------|------------------|----------------|
|Sağlam beceri entegrasyonu ve tam dağıtım denetimi ile açık uçlu konuşma | Bot Framework'ün [Doğrudan Hat Konuşma](direct-line-speech.md) kanalı | <ul><li>"Seattle'a gitmem gerek"</li><li>"Ne tür pizza sipariş edebilirim?"</li></ul>
|Basitleştirilmiş yazma ve barındırma ile komut ve kontrol veya görev odaklı konuşma | [Özel Komutlar (Önizleme)](custom-commands.md) | <ul><li>"Havai ışığı aç"</li><li>"5 derece daha sıcak olsun"</ul>

Yardımcınızın neleri işlemesini istediğinizden henüz emin [değilseniz, Doğrudan Satır Konuşması'nı](direct-line-speech.md) en iyi varsayılan seçenek olarak öneririz. Bu ortak kalıpları üzerine inşa etmek ve mevcut bilgi kaynaklarını kullanmak için [Sanal Yardımcısı Çözüm ve Kurumsal Şablon](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) ve [QnA Maker hizmeti](https://docs.microsoft.com/azure/cognitive-services/QnAMaker/Overview/overview) gibi araçları ve yazma yardımcıları zengin bir dizi ile entegrasyon sunuyor.

[Özel Komutlar (Önizleme),](custom-commands.md) doğal dil komutu ve denetimi senaryoları için özel olarak uyarlanmış, kolaylaştırılmış bir yazma ve barındırma deneyimi sağlar.

   ![Yardımcı çözümlerin karşılaştırılması](media/voice-assistants/assistant-solution-comparison.png "Yardımcı çözümlerin karşılaştırılması")

## <a name="core-features"></a>Temel özellikler

Asistan etkileşimlerinizi oluşturmak için [Doğrudan Satır Konuşması'nı](direct-line-speech.md) veya [Özel Komutları (Önizleme)](custom-commands.md) seçin, yardımcınızı markanıza, ürününüzüze ve kişiliğinize göre özelleştirmek için zengin bir özelleştirme özellikleri kümesik kullanabilirsiniz.

| Kategori | Özellikler |
|----------|----------|
|[Özel anahtar kelime](speech-devices-sdk-create-kws.md) | Kullanıcılar "Hey Contoso" gibi özel bir anahtar kelimeyle asistanlarla sohbetlere başlayabilirler. Bir uygulama, [burada oluşturabileceğiniz](speech-devices-sdk-create-kws.md)özel bir anahtar kelime ile yapılandırılabilir Konuşma SDK, özel bir anahtar kelime motoru ile yapar. Sesli asistanlar, anahtar kelime etkinleştirme sinin doğruluğunu artırmak için hizmet tarafındaki anahtar kelime doğrulamasını kullanabilir (tek başına aygıta karşı).
|[Metne konuşma](speech-to-text.md) | Ses asistanları, Konuşma hizmetinden [Metinden metne konuşma](speech-to-text.md) kullanarak gerçek zamanlı sesi tanınan metne dönüştürür. Bu metin, hem yardımcı uygulamanız hem de istemci uygulamanız için transkripsiyonu yla kullanılabilir.
|[Metinden konuşmaya](text-to-speech.md) | Yardımcınızın metin yanıtları Konuşma hizmetinden [Metin-konuşma](text-to-speech.md) kullanılarak sentezlenir. Bu sentez daha sonra istemci uygulamanız için bir ses akışı olarak kullanılabilir hale getirilir. Microsoft, markanıza ses veren kendi özel, yüksek kaliteli Nöral TTS sesinizi oluşturma olanağı sunar. Daha fazla bilgi için [bize ulaşın.](mailto:mstts@microsoft.com)

## <a name="getting-started-with-voice-assistants"></a>Sesli asistanlarla başlarken

Kod çalıştırmanız için 10 dakikadan kısa bir süre içinde tasarlanmış hızlı başlangıçlar sunuyoruz. Bu tablo, dile göre düzenlenmiş ses asistanı hızlı başlatmaların bir listesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| C#, UWP | Windows | [Gözat](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Gözat](https://aka.ms/csspeech/javaref) |
| Java | Android | [Gözat](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Örnek kod

Ses asistanı oluşturmak için örnek kod GitHub'da kullanılabilir. Bu örnekler, çeşitli popüler programlama dillerinde yardımcınıza bağlanmak için istemci uygulamasını kapsar.

* [Ses asistanı örnekleri (SDK)](https://aka.ms/csspeech/samples)
* [Öğretici: Ses Konuşma SDK, C ile asistanınızı etkinleştirin #](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="tutorial"></a>Öğretici

[Konuşma SDK ve Doğrudan Satır Konuşma kanalını kullanarak yardımcınızı sesli](tutorial-voice-enable-your-bot-speech-sdk.md)olarak etkinleştirme konusunda bir öğretici.

## <a name="customization"></a>Özelleştirme

Konuşma hizmeti kullanılarak oluşturulmuş ses [yardımcıları, konuşmadan metne, metinden](speech-to-text.md) [konuşmaya](text-to-speech.md)ve özel anahtar [kelime seçimi](speech-devices-sdk-create-kws.md)için kullanılabilen tüm özelleştirme seçeneklerini kullanabilir.

> [!NOTE]
> Özelleştirme seçenekleri dile/yerele göre değişir [(bkz. desteklenen diller).](supported-languages.md)

## <a name="reference-docs"></a>Referans dokümanları

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)
* [Konuşma SDK alın](speech-sdk.md)
* [Özel Komutlar (Önizleme) hakkında daha fazla bilgi edinin](custom-commands.md)
* [Doğrudan Satır Konuşması hakkında daha fazla bilgi edinin](direct-line-speech.md)
