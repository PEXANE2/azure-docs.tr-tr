---
title: Konuşma cihazları SDK 'Sı hakkında-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma cihaz SDK'sı ile çalışmaya başlayın. Konuşma hizmeti çok çeşitli cihazlar ve ses kaynaklarıyla birlikte çalışmaktadır. Konuşma cihaz SDK'sı, amaca yönelik olarak tasarlanan, mikrofon dizi geliştirme takımları ile eşleştirilmiş önceden ayarlanmış bir kitaplıktır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: erhopf
ms.openlocfilehash: 854e7beb2afd8ae838455f77ff448f13d8b3fbea
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77188971"
---
# <a name="about-the-speech-devices-sdk"></a>Konuşma Cihazları SDK’sı hakkında

[Konuşma hizmeti](overview.md) çok çeşitli cihazlar ve ses kaynaklarıyla birlikte çalışmaktadır. Şimdi, konuşma uygulamalarınızın eşleşen donanım ve yazılım ile bir sonraki düzeye alabilir. Konuşma cihaz SDK'sı, amaca yönelik olarak tasarlanan, mikrofon dizi geliştirme takımları ile eşleştirilmiş pretuned bir kitaplıktır.

Konuşma cihaz SDK'sı yardımcı olabilir:

- Hızlı bir şekilde test: yeni ses senaryoları.
- Daha kolay bulut tabanlı konuşma hizmeti, cihazınızın içine tümleştirin.
- Müşterileriniz için bir olağanüstü bir kullanıcı deneyimi oluşturun.

Konuşma cihazları SDK 'sı [konuşma SDK 'sını](speech-sdk.md)kullanır. Sesi [konuşma hizmetine](overview.md)göndermek için cihazın mikrofon dizisiyle gelişmiş ses işleme algoritmalarınızı kullanma. Gürültü gizleme, yankı iptali, beamoluşturan ve dertal beration aracılığıyla doğru ve çok alan [konuşma tanıma özelliği](speech-to-text.md) sağlar.

Ayrıca, kendi [özelleştirilmiş anahtar kelime](speech-devices-sdk-create-kws.md)sahip çevresel aygıtlar oluşturmak Için konuşma cihazları SDK 'sını de kullanabilirsiniz. Özel bir anahtar sözcük, markanız için benzersiz olan bir kullanıcı etkileşimini Başlatan bir ipucu sağlar.

Konuşma cihazları SDK 'Sı, [sesli yardımcılar](https://aka.ms/bots/speech/va), sürücü temelli sıralama sistemleri, [konuşma dökümü](conversation-transcription-service.md)ve akıllı hoparlörler gibi çeşitli ses özellikli senaryolara izin vermez. Kullanıcılara bir metin ile yanıt verebilir, varsayılan veya [özel bir sesle](how-to-customize-voice-font.md)geri konuşabilirsiniz, arama sonuçları sağlayabilir, diğer dillere [çevirebilir](speech-translation.md) ve daha fazlasını yapabilirsiniz. Hangi derleme görmeye umuyoruz!

## <a name="get-the-speech-devices-sdk"></a>Konuşma Cihazları SDK’sını edinme

### <a name="android"></a>Android

Android için cihazlar, [Android konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download-android)en son sürümünü indirir.

### <a name="windows"></a>Windows

Windows için örnek uygulama, platformlar arası bir Java uygulaması olarak sağlanır. [JRE konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download-JRE)en son sürümünü indirin.
Uygulama, konuşma SDK paketiyle oluşturulmuştur ve 64 bit Windows üzerinde Java IDE tutulma (v4). 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

### <a name="linux"></a>Linux

Linux için örnek uygulama, platformlar arası bir Java uygulaması olarak sağlanır. [JRE konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download-JRE)en son sürümünü indirin.
Uygulama, konuşma SDK paketiyle oluşturulmuştur ve 64 bitlik Linux (Ubuntu 16,04, Ubuntu 18,04, detem 9) üzerinde Java IDE tutulma (v4). 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

Gelecek cihazları, [Roobo v2 DDK](https://aka.ms/sdsdk-download-roobov2)ve [URDAHA iyi DDK](https://aka.ms/sdsdk-download-urbetter)'yi desteklemek için ek ikili dosyalar verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihazınızı seçin](get-speech-devices-sdk.md)
> [!div class="nextstepaction"]
> [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
