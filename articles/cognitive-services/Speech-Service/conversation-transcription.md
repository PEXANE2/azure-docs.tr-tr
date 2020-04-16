---
title: Konuşma Transkripsiyon (Önizleme) - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma Transkripsiyonu, herhangi bir konuşmanın transkripsiyonunu sağlamak için tanıma, konuşmacı kimliği ve ishal özelliğini birleştiren toplantılar için bir çözümdür.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: trbye
ms.openlocfilehash: dcc7721aec067c4de309e3fdd926245a9d240f0d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402505"
---
# <a name="what-is-conversation-transcription-in-meetings-preview"></a>Toplantılarda Konuşma Transkripsiyonu Nedir (Önizleme)?

Konuşma Transkripsiyonu, herhangi bir konuşmanın gerçek zamanlı ve/veya eşzamanlı transkripsiyonu sağlamak için her konuşmacıya _(ishal_olarak da bilinir) konuşma tanıma, konuşmacı tanımlama ve cümle atıfını birleştiren bir [konuşma-metin](speech-to-text.md) çözümüdür. Konuşma Transkripsiyonu, konuşmadaki konuşmacıları kimin ne zaman söylediğini belirlemek için ayırt eder ve geliştiricilerin çok hoparlörlü ishal gerçekleştiren uygulamalarına metinden metne eklemelerini kolaylaştırır.

## <a name="key-features"></a>Önemli özellikler

- **Zaman damgaları** - her konuşmacı nın bir zaman damgası vardır, böylece bir ifadenin ne zaman söylendiğini kolayca bulabilirsiniz.
- **Okunabilir transkriptler** - transkriptler, metnin söylenenlerle yakından eşleştiğinden emin olmak için otomatik olarak biçimlendirme ve noktalama işaretlerine sahiptir.
- **Kullanıcı profilleri** - kullanıcı ses örnekleri toplanıp imza oluşturmaya gönderilerek kullanıcı profilleri oluşturulur.
- **Hoparlör tanımlama** - hoparlörler kullanıcı profilleri kullanılarak tanımlanır ve her birine bir _hoparlör tanımlayıcısı_ atanır.
- **Çok hoparlörlü ishal** - her hoparlör tanımlayıcısı ile ses akışını sentezleyerek kimin ne söylediğini belirleyin.
- **Gerçek zamanlı transkripsiyon** - konuşma oluyor ken kim ne diyor ve ne zaman canlı transkript sağlar.
- **asynchronous transkripsiyon** – çok kanallı ses akışı kullanarak transkriptleri daha yüksek doğrulukla sağlar.

> [!NOTE]
> Konuşma Transkripsiyon odadaki hoparlör sayısı bir sınır koymak olmasa da, oturum başına 2-10 hoparlörler için optimize edin.

## <a name="use-cases"></a>Uygulama alanları

### <a name="inclusive-meetings"></a>Dahil toplantılar

Sağır ve işitme zorkatılımcılar gibi toplantılar herkes için kapsayıcı hale getirmek için, gerçek zamanlı olarak transkripsiyon olması önemlidir. Gerçek zamanlı modda konuşma transkripsiyonu toplantı ses alır ve kim ne diyor belirler, tüm toplantı katılımcıları transkript takip etmek ve gecikmeden toplantıya katılmak için izin.

### <a name="improved-efficiency"></a>Geliştirilmiş verimlilik

Toplantı katılımcıları toplantıya odaklanabilir ve not alma yı Konuşma Transkripsiyonuna bırakabilir. Katılımcılar, not almak ve toplantı sırasında bir şeyleri kaçırmak yerine transkripti kullanarak toplantıya aktif olarak katılabilir ve sonraki adımları hızlı bir şekilde takip edebilirler.

## <a name="how-it-works"></a>Nasıl çalışır?

Bu, Konuşma Transkripsiyonunun nasıl çalıştığına ilişkin üst düzey bir genel bakıştır.

![İthalat Konuşma Transkripsiyon Diyagramı](media/scenarios/conversation-transcription-service.png)

## <a name="expected-inputs"></a>Beklenen girişler

- **Çok kanallı ses akışı** – Belirtim ve tasarım ayrıntıları için [Microsoft Konuşma Aygıtı SDK Mikrofonu'na](https://aka.ms/cts/microphone)bakın. Daha fazla bilgi edinmek veya geliştirme kiti satın almak için [Bkz. Microsoft Konuşma Aygıtı SDK'yı edinin.](https://aka.ms/cts/getsdk)
- **Kullanıcı ses örnekleri** - Konuşma Transkripsiyon konuşma öncesinde kullanıcı profilleri gerekir. Her kullanıcıdan ses kayıtları toplamanız, ardından sesi doğrulamak ve kullanıcı profilleri oluşturmak için kayıtları [İmza Oluşturma Hizmeti'ne](https://aka.ms/cts/signaturegenservice) göndermeniz gerekir.

## <a name="real-time-vs-asynchronous"></a>Gerçek zamanlı vs asynchronous

Konuşma Transkripsiyon üç transkripsiyon modu sunar:

### <a name="real-time"></a>Gerçek zamanlı

Ses verileri hoparlör tanımlayıcısı + transkript dönmek için canlı işlenir. Transkripsiyon çözüm gereksiniminiz, konuşma katılımcılarına devam eden konuşmalarının canlı bir transkript görünümünü sağlamaksa bu modu seçin. Örneğin, toplantıları daha erişilebilir hale getirmek için bir uygulama oluşturmak, işitme engelli ve işitme zoru olan katılımcıların gerçek zamanlı transkripsiyon için ideal bir kullanım örneğidir.

### <a name="asynchronous"></a>Zaman uyumsuz

Ses verileri hoparlör tanımlayıcısı ve transkript dönmek için toplu işlenir. Transkripsiyon çözüm gereksiniminiz canlı transkript görünümü olmadan daha yüksek doğruluk sağlamaksa bu modu seçin. Örneğin, toplantı katılımcılarının kaçırılan toplantıları kolayca yakalamasına olanak sağlayacak bir uygulama oluşturmak istiyorsanız, yüksek doğruluklu transkripsiyon sonuçları elde etmek için eşzamanlı transkripsiyon modunu kullanın.

### <a name="real-time-plus-asynchronous"></a>Gerçek zamanlı artı asynchronous

Ses verileri hoparlör tanımlayıcısı + transkript döndürmek için canlı olarak işlenir ve buna ek olarak, bir istek de asynchronous işleme yoluyla yüksek doğrulukta transkript almak için oluşturulur. Uygulamanızın gerçek zamanlı transkripsiyon acısı varsa, ancak konuşma veya toplantı gerçekleştikten sonra kullanım için daha yüksek bir doğruluk dökümü gerektiriyorsa bu modu seçin.

## <a name="language-support"></a>Dil desteği

Şu anda, Konuşma Transkripsiyon aşağıdaki bölgelerde "en-US" ve "zh-CN" destekler: *centralus* ve *eastasia*. Ek yerel destek gerekiyorsa, [Konuşma Transkripsiyon Özelliği Ekibi'ne](mailto:CTSFeatureCrew@microsoft.com)başvurun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmaları gerçek zamanlı olarak aktarma](how-to-use-conversation-transcription-service.md)
