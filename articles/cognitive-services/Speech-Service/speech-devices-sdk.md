---
title: Konuşma Cihazları SDK - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma Cihazları SDK ile başlayın. Konuşma hizmeti çok çeşitli aygıtlar ve ses kaynaklarıyla çalışır. Konuşma Aygıtları SDK, amaca yönelik olarak oluşturulmuş, mikrofon dizisi geliştirme kitleriyle eşleştirilmiş önceden ayarlanmış bir kitaplıktır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: erhopf
ms.openlocfilehash: add0d27ae6a612fed0320b5329d19236b7bbbd01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370653"
---
# <a name="what-is-the-speech-devices-sdk"></a>Konuşma Cihazları SDK nedir?

[Konuşma hizmeti](overview.md) çok çeşitli aygıtlar ve ses kaynaklarıyla çalışır. Şimdi, eşleşen donanım ve yazılım ile konuşma uygulamalarınızı bir sonraki seviyeye taşıyabilirsiniz. Konuşma Aygıtları SDK, amaca yönelik olarak oluşturulmuş, mikrofon dizisi geliştirme kitleriyle eşleştirilmiş önceden ayarlanmış bir kitaplıktır.

Konuşma Aygıtları SDK size yardımcı olabilir:

- Yeni ses senaryolarını hızla test edin.
- Bulut tabanlı Konuşma hizmetini cihazınıza daha kolay entegre edin.
- Müşterileriniz için olağanüstü bir kullanıcı deneyimi oluşturun.

Konuşma Cihazları SDK [Konuşma SDK](speech-sdk.md)tüketir. [Sesin Konuşma hizmetine](overview.md)gönderilmesi için cihazın mikrofon dizisi ile gelişmiş ses işleme algoritmalarımızı kullanarak. Gürültü bastırma, yankı iptali, ışın şekillendirme ve dereverberasyon yoluyla doğru uzak alan [konuşma tanıma](speech-to-text.md) sağlar.

Ayrıca, kendi [özelleştirilmiş anahtar kelimeniz](speech-devices-sdk-create-kws.md)olan ortam aygıtları oluşturmak için Konuşma Aygıtları SDK'yı da kullanabilirsiniz. Özel Anahtar Kelime, markanıza özgü bir kullanıcı etkileşimi başlatan bir ipucu sağlar.

Konuşma Aygıtları [SDK, sesli asistanlar,](https://aka.ms/bots/speech/va)sürücü-thru sipariş sistemleri, [konuşma transkripsiyonu](conversation-transcription-service.md)ve akıllı hoparlörler gibi ses özellikli çeşitli senaryolar sağlar. Kullanıcılara metinle yanıt verebilir, varsayılan veya [özel bir sesle](how-to-customize-voice-font.md)geri dönebilir, arama sonuçları sağlayabilir, diğer dillere çeviri [yapabilir](speech-translation.md) ve daha fazlasını yapabilirsiniz. Biz ne inşa görmek için sabırsızlanıyoruz!

## <a name="get-the-speech-devices-sdk"></a>Konuşma Cihazları SDK’sını edinme

### <a name="android"></a>Android

Android için, cihazlar [Android Konuşma Cihazları SDK](https://aka.ms/sdsdk-download-android)en son sürümünü indirin.

### <a name="windows"></a>Windows

Windows için örnek uygulama platformlar arası Java uygulaması olarak sağlanır. [JRE Konuşma Cihazları SDK](https://aka.ms/sdsdk-download-JRE)en son sürümünü indirin.
Uygulama Konuşma SDK paketi ve Eclipse Java IDE (v4) 64-bit Windows ile inşa edilmiştir. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

### <a name="linux"></a>Linux

Linux için örnek uygulama platformlar arası Java uygulaması olarak sağlanır. [JRE Konuşma Cihazları SDK](https://aka.ms/sdsdk-download-JRE)en son sürümünü indirin.
Uygulama Konuşma SDK paketi ve Eclipse Java IDE (v4) 64-bit Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 8, CentOS 8) üzerine inşa edilmiştir. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

Ek ikili ler yaklaşan cihazlar, [Roobo v2 DDK,](https://aka.ms/sdsdk-download-roobov2) [Urbetter DDK](https://aka.ms/sdsdk-download-urbetter)ve [GGEC Hoparlör](https://aka.ms/sdsdk-download-speaker)desteklemek için sağlanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma aygıtınızı seçin](get-speech-devices-sdk.md)
> [!div class="nextstepaction"]
> [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)
