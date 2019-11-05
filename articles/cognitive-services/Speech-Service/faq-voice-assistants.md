---
title: Sesli yardımcılar hakkında sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Özel komutlar (Önizleme) veya doğrudan hat konuşma kanalı kullanarak sesli yardımcılar hakkında en popüler sorulara yanıtlar alın.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: 7ad3f932e9a10723d6cc1bae2fc4854c932d4c64
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73507082"
---
# <a name="voice-assistants-frequently-asked-questions"></a>Ses yardımcıları: sık sorulan sorular

Bu belgede sorularınıza yanıt bulamazsanız, [diğer destek seçeneklerine](support.md)göz atın.

## <a name="general"></a>Genel

**S: Ses Yardımcısı nedir?**

Y **:** Cortana gibi, bir ses Yardımcısı, bir kullanıcının konuşulan duyululini dinleyen bir çözümdür, bu söyleyler için bu çıkışların içeriğini analiz eder, utterlik 'in amacına yanıt olarak bir veya daha fazla eylem gerçekleştirir ve daha sonra kullanıcıya bir yanıt sağlar , konuşulan bir bileşeni içerir. Bu, bir sistemle etkileşimde bulunmak için bir "sesli oturum, sesli çıkış" deneyimidir. Sesli yardım yazarları, [özel komutlar (Önizleme)](custom-commands.md) veya bot çerçevesinin [doğrudan çizgi konuşma](direct-line-speech.md) kanalı kullanılarak oluşturulan bir yardımcı Ile ILETIŞIM kurmak için konuşma SDK 'sindeki `DialogServiceConnector` kullanarak bir cihaz üzerinde uygulama oluşturur. Bu yardımcılar, marka veya ürününüze uyarlanmış bir deneyim sağlamak için özel anahtar sözcükler, özel konuşma ve özel ses kullanabilir.

**S: özel komutları (Önizleme) mi yoksa doğrudan konuşma hattını mi kullanmalıyım? Fark nedir?**

Y **:** [özel komutlar (Önizleme)](custom-commands.md) , görev tamamlama senaryolarına uygun olan bir yardımcıyı kolayca oluşturup barındırmak için daha düşük karmaşıklığa sahip bir araç kümesidir. [Doğrudan hat](direct-line-speech.md) konuşması, güçlü bir konuşma senaryolarını etkinleştirebilecek daha zengin, daha gelişmiş yetenekler sağlar. Daha fazla bilgi için [yardımcı çözümlerin karşılaştırmasına](voice-assistants.md#comparing-assistant-solutions) bakın.

**S: Nasıl yaparım? kullanmaya başlamak istiyor musunuz?**

Y **:** Özel komutlar (Önizleme) uygulaması veya temel bot Framework bot oluşturmaya başlamak için en iyi yol.

* [Özel komutlar (Önizleme) uygulaması oluşturma](quickstart-custom-speech-commands-create-new.md)
* [Temel bir bot Framework bot oluşturma](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Bir bot 'ı doğrudan hat konuşma kanalına bağlama](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)

## <a name="debugging"></a>Hata ayıklama

**S: bağlanırken bir 401 hatası alıyorum ve hiçbir şey işe yarar. Konuşma aboneliği anahtarımın geçerli olduğunu biliyorum. Ne var ne yok?**

Y **:** Aboneliğinizi Azure portal yönetirken, lütfen *bilişsel* **Hizmetler** kaynağını değil **konuşma** kaynağını (Microsoft. CognitiveServicesSpeechServices, "konuşma") kullandığınızdan emin olun ( Microsoft. Biliveservicesallinone, "tüm bilişsel hizmetler"). Ayrıca, [sesli yardımcılar Için konuşma hizmeti bölgesi desteğini](regions.md#voice-assistants)inceleyin.

![doğrudan hat konuşmayı için doğru abonelik](media/voice-assistants/faq-supported-subscription.png "uyumlu bir konuşma aboneliği örneği")

**S: `DialogServiceConnector`tanıma metnini geri aldım, ancak bir ' 1011 ' hatası ve botumun hiçbir şey görmem. Kaydol?**

Y **:** Bu hata, yardımcınızla ses Yardımcısı hizmeti arasında bir iletişim sorunu olduğunu gösterir.

* Özel komutlar (Önizleme) için, özel komutlar (Önizleme) uygulamanızın yayımlandığından emin olun
* Doğrudan hat konuşması için, [botunuzu doğrudan hat konuşma kanalına bağladığınıza](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech), bot 'a [akış protokolü desteği ekledik](https://aka.ms/botframework/addstreamingprotocolsupport) (ilgili Web yuvası desteğiyle) emin olun ve ardından bot 'un gelen isteklere yanıt verdiğini denetleyin kanaldan.

**S: Bu kod hala çalışmıyor ve/veya `DialogServiceConnector`kullanırken farklı bir hata alıyorum. Ne yapmam gerekir?**

Y **:** Dosya tabanlı günlük kaydı önemli ölçüde daha fazla ayrıntı sağlar ve destek isteklerini hızlandırmaya yardımcı olabilir. Bu işlevi etkinleştirmek için bkz. [dosya günlüğünü kullanma](how-to-use-logging.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Sorun giderme](troubleshooting.md)
* [Sürüm notları](releasenotes.md)
