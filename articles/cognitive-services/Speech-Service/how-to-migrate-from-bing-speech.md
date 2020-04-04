---
title: Bing Konuşmasından Konuşma hizmetine geçiş
titleSuffix: Azure Cognitive Services
description: Varolan bir Bing Konuşma aboneliğinden Azure Bilişsel Hizmetler'den Konuşma hizmetine nasıl geçiş yaptığınızı öğrenin.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: nitinme
ms.openlocfilehash: 7b78bdb070cdf1364fe7fbdc75f175be7ce145ff
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656458"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Bing Konuşmasından Konuşma hizmetine geçiş

Uygulamalarınızı Bing Konuşma API'sinden Konuşma hizmetine geçirmek için bu makaleyi kullanın.

Bu makalede, Bing Konuşma API'leri ile Konuşma hizmeti arasındaki farklar özetlenir ve uygulamalarınızı geçirmek için stratejiler önerir. Bing Konuşma API abonelik anahtarınız Konuşma hizmetiyle çalışmaz; yeni bir Konuşma hizmeti aboneliğine ihtiyacınız olacak.

Tek bir Konuşma hizmeti aboneliği anahtarı aşağıdaki özelliklere erişim sağlar. Hepsinin kullanımı bağımsız bir şekilde ölçüldüğünden yalnızca kullandığınız hizmetler için ödeme yapmış olursunuz.

* [Konuşmayı metne dönüştürme](speech-to-text.md)
* [Özel konuşmayı metne dönüştürme](https://cris.ai)
* [Metin okuma](text-to-speech.md)
* [Özel metin okuma sesleri](how-to-customize-voice-font.md)
* [Konuşma çevirisi ](speech-translation.md) ([Metin çevirisi](../translator/translator-info-overview.md) özelliğini kapsamaz)

[Speech SDK,](speech-sdk.md) Bing Konuşma istemci kitaplıklarının işlevsel bir yerine, ancak farklı bir API kullanır.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırılması

Konuşma hizmeti büyük ölçüde Bing Konuşma'ya benzer ve aşağıdaki farklılıklar la birlikte dir.

| Özellik | Bing Konuşma | Konuşma hizmeti | Ayrıntılar |
|--|--|--|--|
| C# SDK’sı | :heavy_check_mark: | :heavy_check_mark: | Konuşma hizmeti Windows 10, Evrensel Windows Platformu (UWP) ve .NET Standart 2.0'ı destekler. |
| C++ SDK | :heavy_minus_sign: | :heavy_check_mark: | Konuşma hizmeti Windows ve Linux'u destekler. |
| Java SDK | :heavy_check_mark: | :heavy_check_mark: | Konuşma hizmeti Android ve Konuşma Aygıtları destekler. |
| Sürekli konuşma tanıma | 10 dakika | Sınırsız (SDK ile) | Hem Bing Konuşma hem de Konuşma hizmeti WebSockets protokolleri çağrı başına 10 dakikaya kadar destek sağlar. Ancak, Konuşma SDK otomatik olarak zaman acısı veya bağlantısını keser. |
| Kısmi veya ara sonuçlar | :heavy_check_mark: | :heavy_check_mark: | WebSockets protokolü veya SDK ile. |
| Özel konuşma modelleri | :heavy_check_mark: | :heavy_check_mark: | Bing Konuşma ayrı bir Özel Konuşma aboneliği gerektirir. |
| Özel sesli yazı tipleri | :heavy_check_mark: | :heavy_check_mark: | Bing Konuşma ayrı bir Özel Ses aboneliği gerektirir. |
| 24 kHz sesler | :heavy_minus_sign: | :heavy_check_mark: |
| Konuşma niyeti tanıma | Ayrı LUIS API çağrısı gerektirir | Entegre (SDK ile) | Konuşma hizmetinde BIR LUIS tuşu kullanabilirsiniz. |
| Basit niyet tanıma | :heavy_minus_sign: | :heavy_check_mark: |
| Uzun ses dosyalarının toplu transkripsiyonu | :heavy_minus_sign: | :heavy_check_mark: |
| Tanıma modu | Son nokta URI ile manuel | Automatic | Tanıma modu Konuşma hizmetinde kullanılamaz. |
| Uç nokta yerelliği | Genel | Bölgesel | Bölgesel uç noktalar gecikme yi tir. |
| REST API'leri | :heavy_check_mark: | :heavy_check_mark: | Konuşma hizmeti REST API'leri Bing Speech (farklı uç nokta) ile uyumludur. REST API'leri metinden konuşmaya ve sınırlı konuşma-metin işlevselliğini destekler. |
| WebSockets protokolleri | :heavy_check_mark: | :heavy_check_mark: | Konuşma hizmeti WebSockets API Bing Konuşma (farklı bitiş noktası) ile uyumludur. Kodunuzu basitleştirmek için mümkünse Konuşma SDK'sına geçirin. |
| Servise hizmet API çağrıları | :heavy_check_mark: | :heavy_minus_sign: | C# Hizmet Kütüphanesi aracılığıyla Bing Konuşması'nda sağlanır. |
| Açık kaynak SDK | :heavy_check_mark: | :heavy_minus_sign: |

Konuşma hizmeti zaman tabanlı bir fiyatlandırma modeli (işlem tabanlı bir model yerine) kullanır. Ayrıntılar için [Konuşma hizmeti fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) için bkz.

## <a name="migration-strategies"></a>Geçiş stratejileri

Sizin veya kuruluşunuzun geliştirme veya üretimde Bing Konuşma API'si kullanan uygulamaları varsa, bunları konuşma hizmetini mümkün olan en kısa sürede kullanmak üzere güncelleştirmeniz gerekir. Kullanılabilir SDK'lar, kod örnekleri ve öğreticiler için [Konuşma hizmeti belgelerine](index.yml) bakın.

Konuşma hizmeti [REST API'leri](rest-apis.md) Bing Konuşma API'leri ile uyumludur. Şu anda Bing Konuşma REST API'lerini kullanıyorsanız, yalnızca REST bitiş noktasını değiştirmeniz ve Konuşma hizmeti abonelik anahtarına geçmeniz gerekir.

Konuşma hizmeti WebSockets protokolleri de Bing Konuşma tarafından kullanılanlarla uyumludur. Yeni geliştirme için WebSockets yerine Konuşma SDK'sını kullanmanızı öneririz. Varolan kodu SDK'ya da geçirmek iyi bir fikirdir. Ancak, REST API'lerinde olduğu gibi, WebSockets üzerinden Bing Konuşma'yı kullanan varolan kod yalnızca bitiş noktası ve güncelleştirilmiş bir anahtar değişikliği gerektirir.

Belirli bir programlama dili için Bing Konuşma istemci kitaplığı kullanıyorsanız, KONUŞMA [SDK'sına](speech-sdk.md) geçiş yapmak, UYGULAMANIZ farklı olduğundan uygulamanızda değişiklik gerektirir. Konuşma SDK kodunuzu basit hale getirebilir, aynı zamanda yeni özelliklere erişim sağlar. Konuşma SDK programlama dilleri geniş bir yelpazede mevcuttur. Tüm platformlardaki API'ler benzerdir ve çok platformlu geliştirmeyi kolaylaştırmaktadır.

Konuşma hizmeti genel bir bitiş noktası sunmaz. Uygulamanızın tüm trafiği için tek bir bölgesel bitiş noktası kullandığında verimli bir şekilde işlev göreilip çalışmayolacağını belirleyin. Değilse, en verimli bitiş noktasını belirlemek için coğrafi konumu kullanın. Kullandığınız her bölgede ayrı bir Konuşma hizmeti aboneliğine ihtiyacınız var.

Uygulamanız uzun ömürlü bağlantılar kullanıyorsa ve kullanılabilir bir SDK kullanamıyorsa, WebSockets bağlantısını kullanabilirsiniz. Uygun zamanlarda yeniden bağlanarak 10 dakikalık zaman ödeme sınırını yönetin.

SDK Konuşması'na başlamak için:

1. Konuşma [SDK](speech-sdk.md)indirin.
1. Konuşma hizmeti [hızlı başlangıç kılavuzları](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet) ve [öğreticiler](how-to-recognize-intents-from-speech-csharp.md)aracılığıyla çalışın. Ayrıca yeni API'ler ile deneyim almak için [kod örnekleri](samples.md) ne bakmak.
1. Konuşma hizmetini kullanmak için uygulamanızı güncelleştirin.

## <a name="support"></a>Destek

Bing Konuşma müşterileri bir destek [bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açarak müşteri desteğine başvurmalıdır. Destek ihtiyacınız teknik bir [destek planı](https://azure.microsoft.com/support/plans/)gerektiriyorsa da bizimle irtibata geçebilirsiniz.

Konuşma hizmeti, SDK ve API desteği için Konuşma hizmeti [destek sayfasını](support.md)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetini ücretsiz deneyin](get-started.md)
* [Hızlı başlatma: Konuşma SDK'sını kullanarak bir UWP uygulamasındaki konuşmayı tanıma](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)

## <a name="see-also"></a>Ayrıca bkz.
* [Konuşma hizmeti sürüm notları](releasenotes.md)
* [Konuşma hizmeti nedir](overview.md)
* [Konuşma hizmeti ve Konuşma SDK belgeleri](speech-sdk.md#get-the-speech-sdk)
