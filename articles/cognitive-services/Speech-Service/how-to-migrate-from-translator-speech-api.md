---
title: Translator Konuşma Çevirisi API'si konuşma hizmetine geçirme
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı Translator Konuşma Çevirisi API'si konuşma hizmetine nasıl geçirebileceğinizi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: 7b61aef13b113d9b2502c24e3001da25fa186c76
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559570"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Translator Konuşma Çevirisi API'si konuşma hizmetine geçirme

Uygulamalarınızı Microsoft Translator Konuşma Çevirisi API'si 'den [konuşma hizmetine](index.yml)geçirmek için bu makaleyi kullanın. Bu kılavuzda, Translator Konuşma Çevirisi API'si ile konuşma hizmeti arasındaki farklar özetlenmektedir ve uygulamalarınızın geçirilmesi için stratejiler önerilir.

> [!NOTE]
> Translator Konuşma Çevirisi API'si abonelik anahtarınız konuşma hizmeti tarafından kabul edilmez. Yeni bir konuşma Hizmetleri aboneliği oluşturmanız gerekir.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırması

| Özellik                                           | Translator Konuşma Çevirisi API’si                                  | Konuşma Hizmetleri | Ayrıntılar                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Metne çeviri                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Konuşmaya çeviri                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Genel uç nokta                                   | :heavy_check_mark:                                              | : heavy_minus_sign:                 | Konuşma Hizmetleri küresel bir uç nokta sunmaz. Küresel bir uç nokta, uygulamanızda gecikme süresini azaltarak trafiği en yakın bölgesel uç noktaya otomatik olarak yönlendirebilir.                                                    |
| Bölgesel uç noktalar                                | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Bağlantı süresi sınırı                             | 90 dakika                                               | SDK ile sınırsız. WebSockets bağlantısıyla 10 dakika.                                                                                                                                                                                                                                                                                   |
| Üst bilgide kimlik doğrulama anahtarı                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Tek bir istekte birden çok dil çevrilmiş | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Kullanılabilir SDK 'lar                                    | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Kullanılabilir SDK 'lar için [konuşma Hizmetleri belgelerine](index.yml) bakın.                                                                                                                                                    |
| WebSockets bağlantıları                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Diller API 'SI                                     | :heavy_check_mark:                                              | : heavy_minus_sign:                 | Konuşma Hizmetleri, [Translator API dilleri başvuru](../translator-speech/languages-reference.md) makalesinde açıklanan dil aralığını destekler. |
| Küfür filtresi ve Işaretleyici                       | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| . Girdi olarak WAV/PCM                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Giriş olarak diğer dosya türleri                         | : heavy_minus_sign:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Kısmi sonuçlar                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Zamanlama bilgisi                                       | :heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                 |
| Bağıntı Kimliği                                    | :heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Özel konuşma modelleri                              | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Konuşma Hizmetleri, konuşma tanımayı kuruluşunuzun benzersiz sözlüğüne özelleştirmenizi sağlayan özel konuşma modelleri sunar.                                                                                                                                           |
| Özel çeviri modelleri                         | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Microsoft metin çevirisi API 'sine abone olmak, daha doğru çeviriler için kendi verilerinizi kullanmak üzere [özel çevirici](https://www.microsoft.com/translator/business/customization/) kullanmanıza olanak sağlar.                                                 |

## <a name="migration-strategies"></a>Geçiş stratejileri

Siz veya kuruluşunuzun Translator Konuşma Çevirisi API'si kullanan geliştirme veya üretim uygulamaları varsa, bunları konuşma hizmetini kullanacak şekilde güncelleştirmelisiniz. Kullanılabilir SDK 'lar, kod örnekleri ve öğreticiler için [konuşma hizmeti](index.yml) belgelerine bakın. Geçiş yaparken aşağıdakileri göz önünde bulundurun:

* Konuşma Hizmetleri küresel bir uç nokta sunmaz. Uygulamanızın tüm trafiği için tek bir bölgesel uç nokta kullandığında uygulamanızın verimli bir şekilde işlev görür olup olmadığını belirleme. Aksi takdirde, en etkili uç noktayı öğrenmek için coğrafi konum kullanın.

* Uygulamanız uzun süreli bağlantılar kullanıyorsa ve kullanılabilir SDK 'Ları kullanamıyoruz, bir WebSockets bağlantısı kullanabilirsiniz. 10 dakikalık zaman aşımı sınırını uygun saatlere yeniden bağlanarak yönetin.

* Uygulamanız özel çeviri modellerini etkinleştirmek için Translator Metin Çevirisi API'si ve Translator Konuşma Çevirisi API'si kullanıyorsa, konuşma hizmetini kullanarak kategori kimliklerini doğrudan ekleyebilirsiniz.

* Translator Konuşma Çevirisi API'si farklı olarak, konuşma hizmetleri tek bir istekte çevirileri birden çok dilde tamamlayabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetlerini ücretsiz deneyin](get-started.md)
* [Hızlı Başlangıç: Konuşma SDK 'sını kullanarak UWP uygulamasındaki konuşmayı tanıma](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Konuşma hizmeti nedir](overview.md)
* [Konuşma Hizmetleri ve konuşma SDK belgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
