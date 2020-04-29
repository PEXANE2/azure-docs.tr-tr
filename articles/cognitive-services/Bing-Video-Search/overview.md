---
title: Bing Video Arama API'si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Video Arama API'si kullanarak web genelinde videoları nasıl arayalabileceğinizi öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: 8377f0f5d586212c94bb763598b6e7a9e391073c
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75382727"
---
# <a name="what-is-the-bing-video-search-api"></a>Bing Video Arama API'si nedir?

Bing Video Arama API'si, hizmet ve uygulamalarınıza video arama özellikleri eklemenizi kolaylaştırır. API ile Kullanıcı arama sorguları göndererek, [Bing video](https://www.bing.com/video)'ya benzer ilgili ve yüksek kaliteli videoları alabilir ve görüntüleyebilirsiniz. Yalnızca videoları içeren arama sonuçları için bu API 'YI kullanın. [Bing Web araması API'si](../bing-web-search/search-the-web.md) , Web sayfaları, videolar, Haberler ve görüntüler dahil olmak üzere diğer Web içeriği türlerini döndürebilir.

## <a name="bing-video-search-api-features"></a>Bing Video Arama API'si özellikleri

| Özellik                                                                                                                                                                                 | Açıklama                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](concepts/sending-requests.md#suggest-search-terms-with-the-bing-autosuggest-api) | Yazılmaya başladıkları anda önerilen arama terimleri görüntülemek için [Bing Otomatik Öneri API](../bing-autosuggest/get-suggested-search-terms.md)'sini kullanarak uygulama deneyimini iyileştirin. |
| [Video sonuçlarını filtreleme ve kısıtlama](concepts/get-videos.md#filtering-videos)                      | Sorgu parametrelerini düzenleyerek döndürülen videoları filtreleyin.                                                                                                       |
| [Küçük resimleri kırpma, yeniden boyutlandırma ve görüntüleme](../bing-web-search/resize-and-crop-thumbnails.md)                                                | Bing Video Arama API'si tarafından döndürülen videoların küçük resim önizlemelerini düzenleyin ve görüntüleyin.                                                                                      |
| [Popüler videoları alma](trending-videos.md) | Dünyanın dört bir yanındaki popüler videolar için arama yapın.                                                                                                          |
| [Video içgörüleri edinme](video-insights.md) | Dünyanın dört bir yanındaki popüler videolar için bir arama özelleştirin.                                                                                                          |

## <a name="workflow"></a>İş akışı

Bing Video Arama API'si, yeniden bir Web hizmetidir ve HTTP istekleri yapıp JSON 'u ayrıştırabilen herhangi bir programlama dilinden çağrı yapmayı kolaylaştırır. Hizmeti [REST API](csharp.md) veya [SDK](video-search-sdk-quickstart.md) kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. İstekleri geçerli bir arama sorgusu ile API'ye gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.


## <a name="next-steps"></a>Sonraki adımlar

Bing Video Arama API'si [Etkileşimli Tanıtım](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) , bir arama sorgusunu nasıl özelleştireceğinizi ve Web 'de videoları nasıl araybileceğinizi gösterir.

API'yi çağırmaya hazır olduğunuzda, bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

İlk API isteğinizi hızlıca kullanmaya [başlamak için hızlı](csharp.md) başlangıcı kullanın.

## <a name="see-also"></a>Ayrıca bkz.

* [Bing video arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference) başvuru sayfası, arama sonuçları istemek için kullanılan bitiş noktaları, üst bilgiler ve sorgu parametrelerinin listesini içerir.

* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./useanddisplayrequirements.md) konusunda belirtilmektedir.

* Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.