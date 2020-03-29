---
title: Sesli asistanlar sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Özel Komutlar (Önizleme) veya Doğrudan Satır Konuşması kanalını kullanarak sesli asistanlarla ilgili en popüler soruların yanıtlarını alın.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: 8480299c2c889a243150028ac9651f4b62656aec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74110349"
---
# <a name="voice-assistants-frequently-asked-questions"></a>Sesli asistanlar sık sorulan sorular

Bu belgede sorularınıza yanıt bulamıyorsanız, diğer [destek seçeneklerine](support.md)göz atın.

## <a name="general"></a>Genel

**S: Ses asistanı nedir?**

**A:** Cortana gibi, bir ses asistanı da kullanıcının konuşulan konuşmalarını dinleyen, anlam için bu söyleyişlerin içeriğini analiz eden, söyleyişin amacına yanıt olarak bir veya daha fazla eylem gerçekleştiren ve kullanıcıya genellikle konuşulan bir bileşeni içeren bir yanıt sağlayan bir çözümdür. Bir sistemle etkileşim için "seslendirme, ses çıkarma" deneyimidir. sesli asistan yazarları, [Özel Komutlar (Önizleme)](custom-commands.md) veya Bot Framework'ün [Doğrudan Hat Konuşması](direct-line-speech.md) kanalı kullanılarak oluşturulan bir asistanla iletişim kurmak için Konuşma SDK'sındaki leri kullanarak `DialogServiceConnector` cihaz üzerinde bir uygulama oluştururlar. Bu yardımcılar, markanıza veya ürününüze özel bir deneyim sağlamak için özel anahtar kelimeler, özel konuşma ve özel ses kullanabilir.

**S: Özel Komutlar (Önizleme) veya Doğrudan Satır Konuşması kullanmalı mıyım? Ne fark eder ki?**

**C:** [Özel Komutlar (Önizleme),](custom-commands.md) görev tamamlama senaryoları için çok uygun bir yardımcı oluşturmak ve barındırmak için daha düşük karmaşıklıkta bir araç kümesidir. [Doğrudan Satır Konuşması,](direct-line-speech.md) sağlam konuşma senaryoları etkinleştirebilecek daha zengin ve daha karmaşık özellikler sağlar. Daha fazla bilgi için [yardımcı çözümlerin karşılaştırması](voice-assistants.md#choosing-an-assistant-solution) bakın.

**S: Nasıl başlarım?**

**A:** Bir Özel Komutlar (Önizleme) uygulaması veya temel Bot Framework bot uyarak başlamak için en iyi yolu.

- [Özel Komutlar (Önizleme) uygulaması oluşturma](quickstart-custom-speech-commands-create-new.md)
- [Temel bir Bot Framework botu oluşturun](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
- [Bir botu Doğrudan Satır Konuşma kanalına bağlama](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)

## <a name="debugging"></a>Hata Ayıklama

**S: Kanal sırrım nerede?**

**A:** Doğrudan Satır Konuşması'nın önizleme sürümünü kullandıysanız veya ilgili belgeleri okuyorsanız, Doğrudan Satır Konuşması kanalı kayıt sayfasında gizli bir anahtar bulmayı bekleyebilirsiniz. Konuşma SDK `DialogServiceConfig` v1.7 fabrika yöntemi `FromBotSecret` de bu değeri bekliyor.

Direct Line Speech'in en son sürümü, bir cihazdan botuna başvurma işlemini kolaylaştırır. Kanal kayıt sayfasında, en üstteki açılır sayfa, Doğrudan Satır Konuşma kanalı kaydınızı bir konuşma kaynağıyla ilişkilendirer. Bir kez ilişkili, v1.8 Konuşma `BotFrameworkConfig::FromSubscription` SDK aboneliğiile `DialogServiceConnector` ilişkili olduğunuz bot temas a yapılandıracak bir fabrika yöntemi içerir.

İstemci uygulamanızı hala v1.7'den v1.8'e geçiriyorsanız, `DialogServiceConfig::FromBotSecret` kanal gizli parametresi (örneğin kullandığınız önceki gizli) için boş olmayan, null olmayan bir değerle çalışmaya devam edebilir. Yalnızca yeni bir kanal kaydıyla ilişkili bir konuşma aboneliği kullanırken göz ardı edilecektir. Servis tarafı ilişkilendirmesi ilgili olmadan önce cihazda bunlar denetlendirilmeden, değerin boş ve boş olmaması _gerektiğini_ lütfen unutmayın.

Daha ayrıntılı bir kılavuz için, lütfen kanal kaydında yürüyen [öğretici bölümüne](tutorial-voice-enable-your-bot-speech-sdk.md#register-the-direct-line-speech-channel) bakın.

**S: Bağlanırken 401 hata alıyorum ve hiçbir şey çalışmıyor. Konuşma abonelik anahtarımgeçerli olduğunu biliyorum. Ne var ne yok?**

**A:** Aboneliğinizi Azure portalında yönetirken, lütfen **Bilişsel Hizmetler** kaynağını (Microsoft.CognitiveServicesAllInOne, "Tüm Bilişsel Hizmetler") _değil,_ **Konuşma** kaynağını (Microsoft.CognitiveServicesSpeechServicesServices, "Speech") kullandığınızdan emin olun. Ayrıca, [sesli asistanlar için Konuşma hizmeti bölge desteği](regions.md#voice-assistants)kontrol edin.

![doğrudan satır konuşması için doğru abonelik](media/voice-assistants/faq-supported-subscription.png "uyumlu bir Konuşma aboneliği örneği")

**S: Benim tanıma metni `DialogServiceConnector`geri almak , ama bir '1011' hata ve benim bot hiçbir şey bakın. Neden?**

**A:** Bu hata, yardımcınız ve ses asistanı hizmeti arasında bir iletişim sorunu gösterir.

- Özel Komutlar (Önizleme) için, Özel Komutlar (Önizleme) Uygulamanızın yayınlandığından emin olun
- Doğrudan Satır Konuşması [için, botunuzu Doğrudan Hat Konuşması kanalına](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)bağladığınızdan, botunuza [Akış protokolü desteği eklediğinizden](https://aka.ms/botframework/addstreamingprotocolsupport) emin olun (ilgili Web Soketi desteğiyle) ve ardından botunuzun kanaldan gelen isteklere yanıt verilip vermeyini kontrol edin.

**S: Bu kod hala çalışmıyor ve/veya bir `DialogServiceConnector`. Ne yapmalıyım?**

**A:** Dosya tabanlı günlüğe kaydetme önemli ölçüde daha fazla ayrıntı sağlar ve destek isteklerini hızlandırmaya yardımcı olabilir. Bu işlevselliği etkinleştirmek [için, dosya günlüğe kaydetmenin nasıl kullanılacağına](how-to-use-logging.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorun giderme](troubleshooting.md)
- [Sürüm notları](releasenotes.md)
