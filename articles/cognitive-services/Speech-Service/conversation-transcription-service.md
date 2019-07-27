---
title: Konuşma dökümü-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma dökümü, konuşma hizmetlerinin gerçek zamanlı konuşma tanımayı, konuşmacı tanımayı ve okumayı birleştiren gelişmiş bir özelliğidir. Konuşma dökümü, konuşmacılar tarafından yapılan toplantılar için idealdir. hoparlörleri ayırt etme olanağı sayesinde, kimin ne zaman ve ne zaman söylediğini, katılımcıların toplantıya odaklanmasına ve sonraki adımlarda hızlı bir şekilde ilerleyebilmesine izin verir. Bu özellik erişilebilirliği da geliştirir. Döküm ile katılımcıları işitme güçlüğü ile etkin bir şekilde gerçekleştirebilirsiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: acbf82335523ee226f4b899180f0f22cb94cca5e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562881"
---
# <a name="what-is-conversation-transcription"></a>Konuşma dökümü nedir?

Konuşma dökümü, konuşma hizmetlerinin gerçek zamanlı konuşma tanımayı, konuşmacı tanımayı ve okumayı birleştiren gelişmiş bir özelliğidir. Konuşma dökümü, konuşmacılar tarafından yapılan toplantılar için idealdir. hoparlörleri ayırt etme olanağı sayesinde, kimin ne zaman ve ne zaman söylediğini, katılımcıların toplantıya odaklanmasına ve sonraki adımlarda hızlı bir şekilde ilerleyebilmesine izin verir. Bu özellik erişilebilirliği da geliştirir. Döküm ile katılımcıları işitme güçlüğü ile etkin bir şekilde gerçekleştirebilirsiniz.   

Konuşma dökümü, sektör ve şirkete özgü sözlüğü anlamak için özelleştirebileceğiniz özelleştirilebilir konuşma modelleriyle doğru tanıma özelliği sunar. Bunlara ek olarak, çok mikrofonlı cihazlara yönelik deneyimi iyileştirmek için konuşma ve konuşma ile ilgili bir SDK 'yı kullanabilirsiniz.

>[!NOTE]
> Şu anda, küçük toplantılar için konuşma dökümü önerilir. Büyük toplantılar için konuşma dökümünü ölçeklendirmek üzere uzatmak istiyorsanız lütfen bizimle iletişime geçin.

Bu diyagram, konuşma dökümü ile birlikte çalışan donanım, yazılım ve Hizmetleri gösterir.

![Içeri aktarma konuşma dökümü diyagramı](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> Belirli geometri yapılandırmasına sahip bir dairesel yedi Microphone dizisi gereklidir. Belirtim ve tasarım ayrıntıları için bkz. [Microsoft Speech CIHAZ SDK mikrofonu](https://aka.ms/cts/microphone). Daha fazla bilgi edinmek veya bir geliştirme seti satın almak için bkz. [Microsoft Speech cihaz SDK 'Sını alma](https://aka.ms/cts/getsdk).

## <a name="get-started-with-conversation-transcription"></a>Konuşma dökümü ile çalışmaya başlama

Konuşma dökümünü kullanmaya başlamak için gerçekleştirmeniz gereken üç adım vardır.

1. Kullanıcılardan ses örnekleri toplayın.
2. Kullanıcı ses örneklerini kullanarak Kullanıcı profilleri oluşturma
3. Kullanıcıları (hoparlörleri) tanımlamak ve konuşma yapmak için konuşma SDK 'sını kullanma

## <a name="collect-user-voice-samples"></a>Kullanıcı ses örneklerini topla

İlk adım, her kullanıcının ses kayıtlarını toplamaktır. Kullanıcı konuşmayı arka plan gürültüsü olmadan sessiz bir ortama kaydedilmelidir. Her ses örneği için önerilen uzunluk 30 saniye ile iki dakika arasındadır. Daha uzun ses örnekleri, hoparlörleri tanımlarken doğruluk artmasından kaynaklanır. Ses, 16 KHz örnek oranına sahip mono kanalı olmalıdır.

Yukarıda bahsedilen yönergelerin ötesinde, ses nasıl kaydedilir ve depolanır, güvenli bir veritabanı önerilir. Bir sonraki bölümde, bu sesin hoparlörleri tanımak için konuşma SDK 'Sı ile birlikte kullanılan Kullanıcı profillerini oluşturmak için nasıl kullanıldığını inceliyoruz.

## <a name="generate-user-profiles"></a>Kullanıcı profilleri oluşturma

Daha sonra, sesleri doğrulamak ve Kullanıcı profilleri oluşturmak için, topladığınız ses kayıtlarını Imza oluşturma hizmetine göndermeniz gerekir. [Imza oluşturma hizmeti](https://aka.ms/cts/signaturegenservice) , Kullanıcı profilleri oluşturup almanızı sağlayan bir REST API kümesidir.

Bir kullanıcı profili oluşturmak için `GenerateVoiceSignature` API 'yi kullanmanız gerekir. Belirtim ayrıntıları ve örnek kod kullanılabilir:

> [!NOTE]
> Şu bölgelerde "en-US" ve "zh-CN" içinde konuşma dökümü şu anda kullanılabilir: `centralus` ve. `eastasia`

* [REST belirtimi](https://aka.ms/cts/signaturegenservice)
* [Konuşma dökümünü kullanma](https://aka.ms/cts/howto)

## <a name="transcribe-and-identify-speakers"></a>Hoparlörleri yeniden oluşturup tanımla

Konuşma dökümü, çok kanallı ses akışları ve Kullanıcı profillerini, döküm oluşturmak ve hoparlörleri belirlemek için giriş olarak bekler. Ses ve Kullanıcı profili verileri konuşma aygıtı SDK 'Sı kullanılarak konuşma dökümü hizmetine gönderilir. Daha önce belirtildiği gibi, konuşma dökümünü kullanmak için dairesel yedi bir mikrofon dizisi ve konuşma cihazları SDK 'Sı gerekir.

>[!NOTE]
> Belirtim ve tasarım ayrıntıları için bkz. [Microsoft Speech CIHAZ SDK mikrofonu](https://aka.ms/cts/microphone). Daha fazla bilgi edinmek veya bir geliştirme seti satın almak için bkz. [Microsoft Speech cihaz SDK 'Sını alma](https://aka.ms/cts/getsdk).

Konuşma bilgi işlem SDK 'Sı ile konuşma dökümünü nasıl kullanacağınızı öğrenmek için bkz. [konuşma dökümünü kullanma](https://aka.ms/cts/howto).


## <a name="quick-start-with-a-sample-app"></a>Örnek bir uygulamayla hızlı başlangıç

Microsoft konuşma cihaz SDK 'Sı, cihazla ilgili tüm örnekler için hızlı başlangıç örnek uygulamasına sahiptir. Konuşma dökümü bunlardan biridir. Bunu, örnek uygulama ve başvurunuz için kaynak kodu ile birlikte [konuşma cihaz SDK 'sı hızlı başlangıç](https://aka.ms/sdsdk-quickstart) bölümünde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihazları SDK 'Sı hakkında daha fazla bilgi edinin](speech-devices-sdk.md)
