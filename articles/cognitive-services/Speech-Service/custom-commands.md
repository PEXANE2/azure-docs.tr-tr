---
title: Özel komutlar (Önizleme)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Sesli uygulamalar oluşturmaya yönelik bir çözüm olan özel komutlar (Önizleme) için özelliklere, özelliklere ve sınırlamalara genel bakış.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: travisw
ms.openlocfilehash: 2e1b6ee0bd6c392804915fac6ff23278a00b6d33
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79367848"
---
# <a name="what-are-custom-commands-preview"></a>Özel komutlar (Önizleme) nedir?

[Sesli yardımcılar](voice-assistants.md) gibi sesli uygulamalar kullanıcıları dinler ve yanıt olarak bir işlem gerçekleştirebilir, genellikle geri konuşuyor. Bu kişiler, kullanıcının konuşma işlemini yapmak için [konuşmayı metne](speech-to-text.md) dönüştürme özelliğini kullanır ve ardından metnin doğal dilde anlaşılmasına ilişkin işlem yapın. Bu eylem sıklıkla, [metinden konuşmaya](text-to-speech.md)ile oluşturulan yardımcdan konuşulan çıktıyı içerir. Cihazlar, konuşma SDK 'sının `DialogServiceConnector` nesnesiyle birlikte yardımcılar 'a bağlanır.

**Özel komutlar (Önizleme)** , sesli uygulamalar oluşturmaya yönelik kolaylaştırılmış bir çözümdür. Birleşik bir yazma deneyimi, bir otomatik barındırma modeli ve görece daha düşük karmaşıklığa karşı [doğrudan hat konuşma](direct-line-speech.md)gibi diğer seçeneklere kıyasla daha fazla karmaşıklık sağlar. Ancak bu basitleştirme, esneklik açısından bir düşüş ile birlikte gelir. Bu nedenle, özel komutlar (Önizleme) görev tamamlama veya komut ve denetim senaryoları için idealdir. Nesnelerin İnterneti (IoT) ve gözetimsiz cihazlar için oldukça iyi eşleştirilir.

Karmaşık konuşma etkileşimi ve [sanal yardımcı çözümü ve kurumsal şablon](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) gibi diğer çözümlerle tümleştirme Için doğrudan hat konuşmayı kullanmanız önerilir.

Özel komutlar (Önizleme) için iyi adaylarda, iyi tanımlanmış değişken kümelerine sahip sabit bir sözlük vardır. Örneğin, bir termostat denetimi gibi ana otomasyon görevleri idealdir.

   ![Görev tamamlama senaryolarına örnekler](media/voice-assistants/task-completion-examples.png "görev tamamlama örnekleri")

## <a name="getting-started-with-custom-commands-preview"></a>Özel komutlarla çalışmaya başlama (Önizleme)

Bir ses uygulaması oluşturmak için özel komutları (Önizleme) kullanmanın ilk adımı, [bir konuşma aboneliği anahtarı almak](get-started.md) ve [konuşma Studio](https://speech.microsoft.com)'Daki özel komutlar (Önizleme) oluşturucuya erişim kullanmaktır. Buradan, yeni bir özel komut (Önizleme) uygulaması yazabilir ve yayımlayabilirsiniz. bu sayede, bir cihaz üzerindeki uygulama konuşma SDK 'sını kullanarak onunla iletişim kurabilir.

   ![Özel komutlar için yazma akışı (Önizleme)](media/voice-assistants/custom-commands-flow.png "Özel komutlar (Önizleme) yazma akışı")

Kodu, 10 dakikadan kısa bir süre içinde çalıştırmak için tasarlanan hızlı başlangıç sunuyoruz.

* [Özel komutlar (Önizleme) uygulaması oluşturma](quickstart-custom-speech-commands-create-new.md)
* [Parametrelerle özel komutlar (Önizleme) uygulaması oluşturma](quickstart-custom-speech-commands-create-parameters.md)
* [Konuşma SDK 'Sı ile özel komutlar (Önizleme) uygulamasına bağlanma,C#](quickstart-custom-speech-commands-speech-sdk.md)

Hızlı başlangıçlarla işiniz bittiğinde, nasıl yapılacağını inceleyin.

- [Özel komut parametrelerine doğrulama ekleme](./how-to-custom-speech-commands-validations.md)
- [Konuşma SDK 'Sı ile istemcideki komutları yerine getirmek](./how-to-custom-speech-commands-fulfill-sdk.md)
- [Özel komuta onay ekleme](./how-to-custom-speech-commands-confirmations.md)
- [Özel komuta tek adımlı bir düzeltme ekleme](./how-to-custom-speech-commands-one-step-correction.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
* [Özel komutları denemek için konuşma Studio 'ya gidin](https://speech.microsoft.com)
* [Konuşma SDK 'sını alın](speech-sdk.md)
