---
title: Konuşma cihazları SDK 'Sı-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma cihazları SDK 'sını kullanmaya başlayın. Konuşma hizmeti çok çeşitli cihazlar ve ses kaynaklarıyla birlikte çalışmaktadır. Konuşma cihazları SDK 'Sı, amaç oluşturulmuş, mikrofon dizisi geliştirme setleriyle eşleştirilmiş önceden ayarlanmış bir kitaplıktır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: erhopf
ms.openlocfilehash: 4ec153a9c770b0001888c972dc44414470e3ddbf
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84730314"
---
# <a name="what-is-the-speech-devices-sdk"></a>Konuşma cihazları SDK 'Sı nedir?

[Konuşma hizmeti](overview.md) çok çeşitli cihazlar ve ses kaynaklarıyla birlikte çalışmaktadır. Artık, konuşma uygulamalarınızı eşleşen donanım ve yazılımla bir sonraki düzeye getirebilirsiniz. Konuşma cihazları SDK 'Sı, amaç oluşturulmuş, mikrofon dizisi geliştirme setleriyle eşleştirilmiş önceden ayarlanmış bir kitaplıktır.

Konuşma cihazları SDK 'Sı şunları yapmanıza yardımcı olabilir:

- Yeni sesli senaryoları hızla test edin.
- Bulut tabanlı konuşma hizmetini cihazınızla daha kolay bir şekilde tümleştirin.
- Müşterileriniz için olağanüstü bir kullanıcı deneyimi oluşturun.

Konuşma cihazları SDK 'sı [konuşma SDK 'sını](speech-sdk.md)kullanır. Sesi [konuşma hizmetine](overview.md)göndermek için cihazın mikrofon dizisiyle gelişmiş ses işleme algoritmalarınızı kullanma. Gürültü gizleme, yankı iptali, beamoluşturan ve dertal beration aracılığıyla doğru ve çok alan [konuşma tanıma özelliği](speech-to-text.md) sağlar.

Ayrıca, kendi [özelleştirilmiş anahtar kelime](speech-devices-sdk-create-kws.md)sahip çevresel aygıtlar oluşturmak Için konuşma cihazları SDK 'sını de kullanabilirsiniz. Özel bir anahtar sözcük, markanız için benzersiz olan bir kullanıcı etkileşimini Başlatan bir ipucu sağlar.

Konuşma cihazları SDK 'Sı, [sesli yardımcılar](https://aka.ms/bots/speech/va), sürücü temelli sıralama sistemleri, [konuşma dökümü](conversation-transcription-service.md)ve akıllı hoparlörler gibi çeşitli ses özellikli senaryolara izin vermez. Kullanıcılara bir metin ile yanıt verebilir, varsayılan veya [özel bir sesle](how-to-customize-voice-font.md)geri konuşabilirsiniz, arama sonuçları sağlayabilir, diğer dillere [çevirebilir](speech-translation.md) ve daha fazlasını yapabilirsiniz. Neleri geliştirdiklerinizi görmek için ileri bakıyoruz!

## <a name="get-the-speech-devices-sdk"></a>Konuşma Cihazları SDK’sını edinme

### <a name="android"></a>Android

Android için konuşma cihazları SDK 'Sı [Roobo v1](speech-devices-sdk-roobo-v1.md) ve eşdeğer cihazları destekler, bu, [Android konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download-android)en son sürümünü indirir.


Telefon veya mobil gibi farklı bir Android cihazınız varsa, [Android konuşma SDK 'sını](speech-sdk.md) kullanmaya başlayın


### <a name="windows"></a>Windows

Windows için örnek uygulama, platformlar arası bir Java uygulaması olarak sağlanır. [JRE konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download-JRE)en son sürümünü indirin.
Uygulama, konuşma SDK paketiyle oluşturulmuştur ve 64 bit Windows üzerinde Java IDE tutulma (v4). 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

### <a name="linux"></a>Linux

Linux için örnek uygulama, platformlar arası bir Java uygulaması olarak sağlanır. [JRE konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download-JRE)en son sürümünü indirin.
Uygulama, konuşma SDK paketiyle oluşturulmuştur ve 64-bit Linux (Ubuntu 16,04, Ubuntu 18,04, detem 9, RHEL 8, CentOS 8) üzerinde Java IDE tutulma (v4). 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

Gelecek cihazları, [Roobo v2 DDK](https://aka.ms/sdsdk-download-roobov2), [URDAHA iyi DDK](https://aka.ms/sdsdk-download-urbetter), [GGEC konuşmacı](https://aka.ms/sdsdk-download-speaker), [Linux ARM32](https://aka.ms/sdsdk-download-linux-arm32)ve [Linux ARM64](https://aka.ms/sdsdk-download-linux-arm64)desteklemek için ek ikili dosyalar verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihazınızı seçin](get-speech-devices-sdk.md)
> [!div class="nextstepaction"]
> [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
