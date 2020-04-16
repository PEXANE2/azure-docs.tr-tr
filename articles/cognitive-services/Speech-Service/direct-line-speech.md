---
title: Doğrudan Satır Konuşma - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma Yazılımı Geliştirme Kiti (SDK) ile Doğrudan Satır Konuşması kullanan Sesli asistanların özelliklerine, yeteneklerine ve kısıtlamalarına genel bakış.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: 6dc125b68af8f9a07731aaae9d858c19c51f934f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402387"
---
# <a name="what-is-direct-line-speech"></a>Direct Line Speech nedir?

**Direct Line Speech,** esnek ve genişletilebilir bir ses asistanı oluşturmak için güçlü, uçtan uca bir çözümdür. Bu Bot Framework ve doğrudan hat konuşma kanalı tarafından desteklenmektedir, bu ses-in için optimize edilmiş, botlar ile ses-out etkileşim.

[Sesli asistanlar](voice-assistants.md) kullanıcıları dinler ve yanıt olarak bir eylemde, sık sık geri dönerler. Kullanıcının konuşmasını yazıya geçirmek için [metinden metne](speech-to-text.md) sözcük kullanırlar, ardından metnin doğal dil anlayışı konusunda harekete geçerler. Bu eylem sık sık [metinden konuşmaya](text-to-speech.md)oluşturulan yardımcıdan sözlü çıktı içerir.

Direct Line Speech, ses asistanları için en yüksek düzeyde özelleştirme ve gelişmişlik sunar. Görev tamamlama veya komut ve denetim kullanımı yla ikisinin açık uçlu, doğal veya melezleri olan konuşma senaryoları için tasarlanmıştır. Bu yüksek esneklik derecesi daha büyük bir karmaşıklıkla birlikte gelir ve doğal dil girişi kullanarak iyi tanımlanmış görevlere yönelik senaryolar, kolaylaştırılmış bir çözüm deneyimi için [Özel Komutları (Önizleme)](custom-commands.md) olarak değerlendirmek isteyebilir.

## <a name="getting-started-with-direct-line-speech"></a>Doğrudan Satır Konuşması ile başlarken

Direct Line Speech kullanarak bir ses asistanı oluşturmak için ilk adımlar [bir konuşma abonelik anahtarı almak](get-started.md)için, bu abonelik ile ilişkili yeni bir bot oluşturmak ve Doğrudan Hat Konuşma kanalına bot bağlamak için.

   ![Doğrudan Satır Konuşma orkestrasyon hizmet akışının kavramsal diyagramı](media/voice-assistants/overview-directlinespeech.png "Konuşma Kanalı akışı")

Direct Line Speech kullanarak basit bir ses asistanı oluşturma konusunda eksiksiz, adım adım kılavuz için, [Konuşma SDK ve Doğrudan Hat Konuşma kanalı ile botunuzu etkinleştirme eğitimine](tutorial-voice-enable-your-bot-speech-sdk.md)bakın.

Ayrıca, kod çalıştırmanız ve API'leri hızlı bir şekilde öğrenmeniz için hızlı başlangıçlar da sunuyoruz. Bu tablo, dil ve platform tarafından düzenlenen sesli asistan hızlı başlatmalistesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| C#, UWP | Windows | [Gözat](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Gözat](https://aka.ms/csspeech/javaref) |
| Java | Android | [Gözat](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Örnek kod

Ses asistanı oluşturmak için örnek kod GitHub'da kullanılabilir. Bu örnekler, çeşitli popüler programlama dillerinde yardımcınıza bağlanmak için istemci uygulamasını kapsar.

* [Ses asistanı örnekleri (SDK)](https://aka.ms/csspeech/samples)
* [Öğretici: Ses Konuşma SDK, C ile asistanınızı etkinleştirin #](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="customization"></a>Özelleştirme

Konuşma hizmeti kullanılarak oluşturulmuş ses [yardımcıları, konuşmadan metne, metinden](speech-to-text.md) [konuşmaya](text-to-speech.md)ve [özel anahtar kelime seçimi](speech-devices-sdk-create-kws.md)için kullanılabilen tüm özelleştirme seçeneklerini kullanabilir.

> [!NOTE]
> Özelleştirme seçenekleri dile/yerele göre değişir [(bkz. desteklenen diller).](supported-languages.md)

Doğrudan Satır Konuşma ve ses asistanları için ilişkili işlevselliği [Sanal Yardımcısı Çözüm ve Kurumsal Şablon](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview)için ideal bir ektir. Direct Line Speech herhangi bir uyumlu botla çalışsa da, bu kaynaklar yüksek kaliteli konuşma deneyimleri için yeniden kullanılabilir bir temel inyanıtın yanı sıra hızlı bir şekilde başlamak için ortak destekleyici beceriler ve modeller sağlar.

## <a name="reference-docs"></a>Referans dokümanları

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)
* [Konuşma SDK alın](speech-sdk.md)
* [Temel bot oluşturma ve dağıtma](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Sanal Asistan Çözüm ve Kurumsal Şablon alın](https://github.com/Microsoft/AI)
