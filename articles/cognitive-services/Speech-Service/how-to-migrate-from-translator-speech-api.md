---
title: Çevirmen Konuşma API'sinden Konuşma hizmetine geçiş
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı Çevirmen Konuşma API'sinden Konuşma hizmetine nasıl geçirteceklerinizi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: aahi
ms.openlocfilehash: 75a456c4a297b0465c34b8e0af2e87056ad565b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77560907"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Çevirmen Konuşma API'sinden Konuşma hizmetine geçiş

Uygulamalarınızı Microsoft Translator Speech API'sinden [Konuşma hizmetine](index.yml)geçirmek için bu makaleyi kullanın. Bu kılavuz, Çevirmen Konuşma API'si ve Konuşma hizmeti arasındaki farkları özetler ve uygulamalarınızı geçirmek için stratejiler önerir.

> [!NOTE]
> Çevirmen Konuşma API abonelik anahtarınız Konuşma hizmeti tarafından kabul edilmez. Yeni bir Konuşma hizmeti aboneliği oluşturmanız gerekir.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırılması

| Özellik                                           | Translator Konuşma Çevirisi API’si                                  | Konuşma hizmeti | Ayrıntılar                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Metne çeviri                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Konuşmaya çeviri                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Genel bitiş noktası                                   | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Konuşma hizmeti genel bir bitiş noktası sunmaz. Genel uç nokta trafiği otomatik olarak en yakın bölgesel bitiş noktasına yönlendirerek uygulamanızdaki gecikme süresini azaltır.                                                    |
| Bölgesel uç noktalar                                | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Bağlantı süresi sınırı                             | 90 dakika                                               | SDK ile sınırsız. WebSockets bağlantısı ile 10 dakika.                                                                                                                                                                                                                                                                                   |
| Üstbilgide Auth tuşu                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Tek bir istekte çevrilmiş birden çok dil | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDK'lar kullanılabilir                                    | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Kullanılabilir SDK'lar için [Konuşma hizmeti belgelerine](index.yml) bakın.                                                                                                                                                    |
| WebSockets bağlantıları                            | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Diller API                                     | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Konuşma hizmeti, [Translator API dilleri başvuru](../translator-speech/languages-reference.md) makalesinde açıklanan aynı dil aralığını destekler. |
| Küfür Filtresi ve Marker                       | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| . Giriş olarak WAV/PCM                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Giriş olarak diğer dosya türleri                         | :heavy_minus_sign:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Kısmi sonuçlar                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Zamanlama bilgileri                                       | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                 |
| Bağıntı Kimliği                                    | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Özel konuşma modelleri                              | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Konuşma hizmeti, konuşma tanımayı kuruluşunuzun benzersiz sözlüğüne göre özelleştirmenize olanak tanıyan özel konuşma modelleri sunar.                                                                                                                                           |
| Özel çeviri modelleri                         | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Microsoft Metin Çevirisi API'sine abone olmak, daha doğru çeviriler için kendi verilerinizi kullanmak için [Özel Çevirmen'i](https://www.microsoft.com/translator/business/customization/) kullanmanıza olanak tanır.                                                 |

## <a name="migration-strategies"></a>Geçiş stratejileri

Sizin veya kuruluşunuzun Geliştirme veya Üretimde Çevirmen Konuşma API'sini kullanan uygulamaları varsa, bunları Konuşma hizmetini kullanmak üzere güncelleştirmeniz gerekir. Kullanılabilir SDK'lar, kod örnekleri ve öğreticiler için [Konuşma hizmeti](index.yml) belgelerine bakın. Geçiş yaparken aşağıdakileri göz önünde bulundurun:

* Konuşma hizmeti genel bir bitiş noktası sunmaz. Uygulamanızın tüm trafiği için tek bir bölgesel bitiş noktası kullandığında verimli bir şekilde işlev göreilip çalışmayolacağını belirleyin. Değilse, en verimli bitiş noktasını belirlemek için coğrafi konumu kullanın.

* Uygulamanız uzun ömürlü bağlantılar kullanıyorsa ve kullanılabilir SDK'ları kullanamıyorsa, WebSockets bağlantısını kullanabilirsiniz. Uygun zamanlarda yeniden bağlanarak 10 dakikalık zaman ödeme sınırını yönetin.

* Uygulamanız özel çeviri modellerini etkinleştirmek için Çevirmen Metin API'si ve Çevirmen Konuşma API'sını kullanıyorsa, Konuşma hizmetini kullanarak doğrudan Kategori İngilizcesi ekleyebilirsiniz.

* Çevirmen Konuşma API'sinin aksine, Konuşma hizmeti tek bir istekte birden çok dile çeviri yapabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetini ücretsiz deneyin](get-started.md)
* [Hızlı başlatma: Konuşma SDK'sını kullanarak bir UWP uygulamasındaki konuşmayı tanıma](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)

## <a name="see-also"></a>Ayrıca bkz.

* [Konuşma hizmeti nedir](overview.md)
* [Konuşma hizmeti ve Konuşma SDK belgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
