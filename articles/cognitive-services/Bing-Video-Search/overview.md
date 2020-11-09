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
ms.openlocfilehash: d5a4c3110186329516f10465c5e80a10b0ceb7b7
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94379829"
---
# <a name="what-is-the-bing-video-search-api"></a>Bing Video Arama API'si nedir?

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Video Arama API'si, hizmet ve uygulamalarınıza video arama özellikleri eklemenizi kolaylaştırır. API ile Kullanıcı arama sorguları göndererek, [Bing video](https://www.bing.com/video)'ya benzer ilgili ve yüksek kaliteli videoları alabilir ve görüntüleyebilirsiniz. Yalnızca videoları içeren arama sonuçları için bu API 'YI kullanın. [Bing Web araması API'si](../bing-web-search/overview.md) , Web sayfaları, videolar, Haberler ve görüntüler dahil olmak üzere diğer Web içeriği türlerini döndürebilir.

## <a name="bing-video-search-api-features"></a>Bing Video Arama API'si özellikleri

| Özellik                                                                                                                                                                                 | Açıklama                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](concepts/sending-requests.md#suggest-search-terms-with-the-bing-autosuggest-api) | Yazılmaya başladıkları anda önerilen arama terimleri görüntülemek için [Bing Otomatik Öneri API](../bing-autosuggest/get-suggested-search-terms.md)'sini kullanarak uygulama deneyimini iyileştirin. |
| [Video sonuçlarını filtreleme ve kısıtlama](concepts/get-videos.md#filtering-videos)                      | Sorgu parametrelerini düzenleyerek döndürülen videoları filtreleyin.                                                                                                       |
| [Küçük resimleri kırpma, yeniden boyutlandırma ve görüntüleme](../bing-web-search/resize-and-crop-thumbnails.md)                                                | Bing Video Arama API'si tarafından döndürülen videoların küçük resim önizlemelerini düzenleyin ve görüntüleyin.                                                                                      |
| [Popüler videoları alma](trending-videos.md) | Dünyanın dört bir yanındaki popüler videolar için arama yapın.                                                                                                          |
| [Video içgörüleri edinme](video-insights.md) | Dünyanın dört bir yanındaki popüler videolar için bir arama özelleştirin.                                                                                                          |

## <a name="workflow"></a>İş akışı

Bing Video Arama API'si, yeniden bir Web hizmetidir ve HTTP istekleri yapıp JSON 'u ayrıştırabilen herhangi bir programlama dilinden çağrı yapmayı kolaylaştırır. Hizmeti [REST API](./quickstarts/csharp.md) veya [SDK](./quickstarts/client-libraries.md?pivots=programming-language-csharp%253fpivots%253dprogramming-language-csharp) kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](../cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/free/cognitive-services/).
2. İstekleri geçerli bir arama sorgusu ile API'ye gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.


## <a name="next-steps"></a>Sonraki adımlar

Bing Video Arama API'si [Etkileşimli Tanıtım](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) , bir arama sorgusunu nasıl özelleştireceğinizi ve Web 'de videoları nasıl araybileceğinizi gösterir.

İlk API isteğinizi hızlıca kullanmaya [başlamak için hızlı](./quickstarts/csharp.md) başlangıcı kullanın.

## <a name="see-also"></a>Ayrıca bkz.

* [Bing video arama API'si v7](/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference) başvuru sayfası, arama sonuçları istemek için kullanılan bitiş noktaları, üst bilgiler ve sorgu parametrelerinin listesini içerir.

* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](../bing-web-search/use-display-requirements.md) konusunda belirtilmektedir.

* Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/overview.md) ziyaret edin.