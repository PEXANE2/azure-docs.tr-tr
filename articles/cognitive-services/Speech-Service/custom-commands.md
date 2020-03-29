---
title: Özel Komutlar (Önizleme) - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Ses uygulamaları oluşturmak için bir çözüm olan Özel Komutlar (Önizleme) için özellikler, yetenekler ve kısıtlamalara genel bakış.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: travisw
ms.openlocfilehash: 2e1b6ee0bd6c392804915fac6ff23278a00b6d33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367848"
---
# <a name="what-are-custom-commands-preview"></a>Özel Komutlar (Önizleme) nedir?

[Sesli asistanlar](voice-assistants.md) gibi sesli uygulamalar kullanıcıları dinler ve yanıt olarak bir eylemde, genellikle geri konuşma. Kullanıcının konuşmasını yazıya geçirmek için [metinden metne](speech-to-text.md) sözcük kullanırlar, ardından metnin doğal dil anlayışı konusunda harekete geçerler. Bu eylem sık sık [metinden konuşmaya](text-to-speech.md)oluşturulan yardımcıdan sözlü çıktı içerir. Aygıtlar, Speech SDK'nın `DialogServiceConnector` nesnesi ile yardımcılara bağlanır.

**Özel Komutlar (Önizleme),** sesli uygulamalar oluşturmak için kolaylaştırılmış bir çözümdür. Bu birleşik bir yazma deneyimi, otomatik barındırma modeli ve Doğrudan Satır [Konuşma](direct-line-speech.md)gibi diğer seçeneklere karşı nispeten daha düşük karmaşıklık sağlar. Bu basitleştirme, ancak, esneklik bir azalma ile birlikte gelir. Bu nedenle, Özel Komutlar (Önizleme) en iyi görev tamamlama veya komut ve denetim senaryoları için uygundur. Özellikle Nesnelerin İnterneti (IoT) ve başsız cihazlar için çok iyi eşlenmiştir.

[Sanal Yardımcı Çözümü](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) ve Kurumsal Şablon gibi diğer çözümlerle karmaşık konuşma etkileşimi ve tümleştirme için Doğrudan Satır Konuşması'nı kullanmanız alabilirsiniz.

Özel Komutlar (Önizleme) için iyi adaylar, iyi tanımlanmış değişken kümelerine sahip sabit bir kelime dağarcığına sahiptir. Örneğin, termostatı kontrol etmek gibi ev otomasyon görevleri idealdir.

   ![Görev tamamlama senaryolarına örnekler](media/voice-assistants/task-completion-examples.png "görev tamamlama örnekleri")

## <a name="getting-started-with-custom-commands-preview"></a>Özel Komutlarla Başlarken (Önizleme)

Bir ses uygulaması yapmak için Özel Komutlar (Önizleme) kullanmak için ilk adım [bir konuşma abonelik anahtarı almak](get-started.md) ve Konuşma [Stüdyosu'nda](https://speech.microsoft.com)Özel Komutlar (Önizleme) Oluşturucu erişmektir. Buradan, yeni bir Özel Komutlar (Önizleme) Uygulaması yazabilir ve yayımlayabilirsiniz, bundan sonra cihazdaki bir uygulama SDK Konuşma'yı kullanarak uygulamayla iletişim kurabilir.

   ![Özel Komutlar için yazma akışı (Önizleme)](media/voice-assistants/custom-commands-flow.png "Özel Komutlar (Önizleme) yazma akışı")

Kod çalıştırmanız için 10 dakikadan kısa bir süre içinde tasarlanmış hızlı başlangıçlar sunuyoruz.

* [Özel Komutlar (Önizleme) uygulaması oluşturma](quickstart-custom-speech-commands-create-new.md)
* [Parametrelerle Özel Komutlar (Önizleme) uygulaması oluşturma](quickstart-custom-speech-commands-create-parameters.md)
* [Konuşma SDK, C ile Özel Komutlar (Önizleme) uygulamasına bağlanın #](quickstart-custom-speech-commands-speech-sdk.md)

Bir kez hızlı başlar ile yapılır, bizim nasıl yapılır keşfedin.

- [Özel Komut parametrelerine doğrulama ekleme](./how-to-custom-speech-commands-validations.md)
- [Konuşma SDK ile müşteri üzerindeki Komutları Yerine Getir](./how-to-custom-speech-commands-fulfill-sdk.md)
- [Özel Komuta onay ekleme](./how-to-custom-speech-commands-confirmations.md)
- [Özel Komuta tek adımlı düzeltme ekleme](./how-to-custom-speech-commands-one-step-correction.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)
* [Özel Komutları denemek için Konuşma Stüdyosuna gidin](https://speech.microsoft.com)
* [Konuşma SDK alın](speech-sdk.md)
