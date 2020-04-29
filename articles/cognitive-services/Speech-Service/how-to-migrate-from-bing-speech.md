---
title: Bing Konuşma konuşma hizmetine geçirme
titleSuffix: Azure Cognitive Services
description: Mevcut bir Bing Konuşma aboneliğinden Azure bilişsel hizmetler 'den konuşma hizmetine nasıl geçeceğinizi öğrenin.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: nitinme
ms.openlocfilehash: 7b78bdb070cdf1364fe7fbdc75f175be7ce145ff
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80656458"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Bing Konuşma konuşma hizmetine geçirme

Uygulamalarınızı Bing Konuşma API'si konuşma hizmetine geçirmek için bu makaleyi kullanın.

Bu makalede, Bing Konuşma API 'Leri ile konuşma hizmeti arasındaki farklar özetlenmektedir ve uygulamalarınızın geçirilmesi için stratejiler önerilir. Bing Konuşma API'si abonelik anahtarınız konuşma hizmetiyle çalışmaz; Yeni bir konuşma hizmeti aboneliğine ihtiyacınız olacaktır.

Tek bir konuşma hizmeti abonelik anahtarı aşağıdaki özelliklere erişim verir. Hepsinin kullanımı bağımsız bir şekilde ölçüldüğünden yalnızca kullandığınız hizmetler için ödeme yapmış olursunuz.

* [Konuşmayı metne dönüştürme](speech-to-text.md)
* [Özel konuşmayı metne dönüştürme](https://cris.ai)
* [Metin okuma](text-to-speech.md)
* [Özel metin okuma sesleri](how-to-customize-voice-font.md)
* [Konuşma çevirisi ](speech-translation.md) ([Metin çevirisi](../translator/translator-info-overview.md) özelliğini kapsamaz)

[Konuşma SDK 'sı](speech-sdk.md) , Bing Konuşma istemci kitaplıklarının işlevsel bir yerini alır, ancak farklı bir API kullanır.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırması

Konuşma hizmeti, aşağıdaki farklılıklarla büyük ölçüde Bing Konuşma benzerdir.

| Özellik | Bing Konuşma | Konuşma hizmeti | Ayrıntılar |
|--|--|--|--|
| C# SDK’sı | :heavy_check_mark: | :heavy_check_mark: | Konuşma hizmeti, Windows 10, Evrensel Windows Platformu (UWP) ve .NET Standard 2,0 'yi destekler. |
| C++ SDK | : heavy_minus_sign: | :heavy_check_mark: | Konuşma hizmeti Windows ve Linux 'u destekler. |
| Java SDK | :heavy_check_mark: | :heavy_check_mark: | Konuşma hizmeti, Android ve konuşma cihazlarını destekler. |
| Sürekli konuşma tanıma | 10 dakika | Sınırsız (SDK ile) | Hem Bing Konuşma hem de konuşma hizmeti WebSockets protokolleri çağrı başına en fazla 10 dakika destekler. Ancak, konuşma SDK 'Sı zaman aşımına göre otomatik olarak yeniden bağlanır veya bağlantıyı keser. |
| Kısmi veya geçici sonuçlar | :heavy_check_mark: | :heavy_check_mark: | WebSockets Protocol veya SDK ile. |
| Özel konuşma modelleri | :heavy_check_mark: | :heavy_check_mark: | Bing Konuşma, ayrı bir Özel Konuşma Tanıma aboneliği gerektirir. |
| Özel ses yazı tipleri | :heavy_check_mark: | :heavy_check_mark: | Bing Konuşma ayrı bir özel ses aboneliği gerektirir. |
| 24-kHz sesler | : heavy_minus_sign: | :heavy_check_mark: |
| Konuşma amacı tanıma | Ayrı bir LUSıS API çağrısı gerektirir | Tümleşik (SDK ile) | Konuşma hizmetiyle bir LUO anahtarı kullanabilirsiniz. |
| Basit amaç tanıma | : heavy_minus_sign: | :heavy_check_mark: |
| Uzun ses dosyalarının toplu olarak dökümünü alma | : heavy_minus_sign: | :heavy_check_mark: |
| Tanıma modu | Uç nokta URI 'SI aracılığıyla el ile | Automatic | Tanıma modu konuşma hizmetinde kullanılamıyor. |
| Uç nokta konumu | Genel | Bölgesel | Bölgesel uç noktalar gecikme süresini geliştirir. |
| REST API'leri | :heavy_check_mark: | :heavy_check_mark: | Konuşma hizmeti REST API 'Leri Bing Konuşma (farklı uç nokta) ile uyumludur. REST API 'Leri, metinden konuşmaya ve sınırlı konuşmadan metne işlevselliği destekler. |
| WebSockets protokolleri | :heavy_check_mark: | :heavy_check_mark: | Konuşma hizmeti WebSockets API 'SI Bing Konuşma (farklı uç nokta) ile uyumludur. Mümkünse, kodunuzu basitleştirmek için konuşma SDK 'sına geçiş yapın. |
| Hizmetten hizmete API çağrıları | :heavy_check_mark: | : heavy_minus_sign: | C# hizmet kitaplığı aracılığıyla Bing Konuşma olarak sunulmaktadır. |
| Açık kaynaklı SDK | :heavy_check_mark: | : heavy_minus_sign: |

Konuşma hizmeti, zaman tabanlı bir fiyatlandırma modeli (işlem tabanlı model yerine) kullanır. Ayrıntılar için bkz. [konuşma hizmeti fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) .

## <a name="migration-strategies"></a>Geçiş stratejileri

Siz veya kuruluşunuzun bir Bing Konuşma API'si kullanan geliştirme veya üretimde uygulamalar varsa, bunları konuşma hizmetini mümkün olan en kısa sürede kullanacak şekilde güncelleştirmelisiniz. Kullanılabilir SDK 'lar, kod örnekleri ve öğreticiler için [konuşma hizmeti belgelerine](index.yml) bakın.

Konuşma hizmeti [REST API 'leri](rest-apis.md) Bing Konuşma API 'leriyle uyumludur. Şu anda Bing Konuşma REST API 'Lerini kullanıyorsanız, yalnızca REST uç noktasını değiştirmeniz ve bir konuşma hizmeti abonelik anahtarına geçmeniz gerekir.

Konuşma hizmeti WebSockets protokolleri, Bing Konuşma tarafından kullanılanlarla de uyumludur. Yeni geliştirme için WebSockets yerine konuşma SDK 'sını kullanmanızı öneririz. Mevcut kodu SDK 'ya de geçirmek iyi bir fikirdir. Ancak REST API 'Lerinde olduğu gibi, WebSockets aracılığıyla Bing Konuşma kullanan mevcut kod, yalnızca Endpoint ve güncelleştirilmiş bir anahtarda değişiklik gerektirir.

Belirli bir programlama dili için Bing Konuşma istemci kitaplığı kullanıyorsanız, [konuşma SDK 'sına](speech-sdk.md) GEÇIŞ, API farklı olduğundan uygulamanızda değişiklik yapılmasını gerektirir. Konuşma SDK 'Sı, kodunuzu daha basit hale getirir ve ayrıca yeni özelliklere erişmenizi sağlar. Konuşma SDK 'Sı çok çeşitli programlama dillerinde kullanılabilir. Tüm platformlardaki API 'Ler benzer, çok platformlu geliştirme için de benzerdir.

Konuşma hizmeti küresel bir uç nokta sunmaz. Uygulamanızın tüm trafiği için tek bir bölgesel uç nokta kullandığında uygulamanızın verimli bir şekilde işlev görür olup olmadığını belirleme. Aksi takdirde, en etkili uç noktayı öğrenmek için coğrafi konum kullanın. Kullandığınız her bölgede ayrı bir konuşma hizmeti aboneliğine sahip olmanız gerekir.

Uygulamanız uzun süreli bağlantılar kullanıyorsa ve kullanılabilir bir SDK kullanamıyoruz, bir WebSockets bağlantısı kullanabilirsiniz. 10 dakikalık zaman aşımı sınırını uygun saatlere yeniden bağlanarak yönetin.

Konuşma SDK 'sını kullanmaya başlamak için:

1. [Konuşma SDK 'sını](speech-sdk.md)indirin.
1. Konuşma hizmeti [hızlı başlangıç kılavuzlarından](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet) ve [öğreticilerle](how-to-recognize-intents-from-speech-csharp.md)çalışın. Ayrıca, yeni API 'lerle ilgili deneyim almak için [kod örneklerine](samples.md) bakın.
1. Konuşma hizmetini kullanmak için uygulamanızı güncelleştirin.

## <a name="support"></a>Destek

Bing Konuşma müşterilerin bir [destek bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açarak müşteri desteğiyle iletişim kurabilmesi gerekir. Destek gereksinimlerinizin [Teknik destek planı](https://azure.microsoft.com/support/plans/)gerektirmesi durumunda bize de başvurabilirsiniz.

Konuşma hizmeti, SDK ve API desteği için konuşma hizmeti [desteği sayfasını](support.md)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetini ücretsiz deneyin](get-started.md)
* [Hızlı başlangıç: konuşma SDK 'sını kullanarak UWP uygulamasındaki konuşmayı tanıma](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)

## <a name="see-also"></a>Ayrıca bkz.
* [Konuşma hizmeti sürüm notları](releasenotes.md)
* [Konuşma hizmeti nedir?](overview.md)
* [Konuşma hizmeti ve konuşma SDK belgeleri](speech-sdk.md#get-the-speech-sdk)
