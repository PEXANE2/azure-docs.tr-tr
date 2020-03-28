---
title: Bing Video Arama API'si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Video Arama API'sini kullanarak web'de videoları nasıl arayacağınızı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: 8377f0f5d586212c94bb763598b6e7a9e391073c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75382727"
---
# <a name="what-is-the-bing-video-search-api"></a>Bing Video Arama API'si nedir?

Bing Video Arama API'si, hizmetlerinize ve uygulamalarınız için video arama özellikleri eklemeyi kolaylaştırır. API ile kullanıcı arama sorguları göndererek, [Bing Video'ya](https://www.bing.com/video)benzer alakalı ve yüksek kaliteli videolar alabilir ve görüntüleyebilirsiniz. Yalnızca video içeren arama sonuçları için bu API'yi kullanın. [Bing Web Arama API'sı,](../bing-web-search/search-the-web.md) web sayfaları, videolar, haberler ve resimler de dahil olmak üzere diğer web içeriği türlerini döndürebilir.

## <a name="bing-video-search-api-features"></a>Bing Video Arama API özellikleri

| Özellik                                                                                                                                                                                 | Açıklama                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](concepts/sending-requests.md#suggest-search-terms-with-the-bing-autosuggest-api) | Yazılmaya başladıkları anda önerilen arama terimleri görüntülemek için [Bing Otomatik Öneri API](../bing-autosuggest/get-suggested-search-terms.md)'sini kullanarak uygulama deneyimini iyileştirin. |
| [Video sonuçlarını filtreleme ve kısıtlama](concepts/get-videos.md#filtering-videos)                      | Sorgu parametrelerini düzenleyerek döndürülen videoları filtreleyin.                                                                                                       |
| [Küçük resimleri kırpma, yeniden boyutlandırma ve görüntüleme](../bing-web-search/resize-and-crop-thumbnails.md)                                                | Bing Video Search API tarafından döndürülen videolar için küçük resim önizlemelerini düzenleme ve görüntüleme.                                                                                      |
| [Popüler videoları alma](trending-videos.md) | Dünyanın dört bir yanından popüler videolar arayın.                                                                                                          |
| [Video içgörüleri edinme](video-insights.md) | Dünyanın dört bir yanından popüler videolar için bir arama özelleştirin.                                                                                                          |

## <a name="workflow"></a>İş akışı

Bing Video Arama API'si, HTTP isteklerini gerçekleştirebilen ve JSON'u ayrıştırabilen herhangi bir programlama dilinden aramayı kolaylaştıran, yeniden hizmet veren bir web hizmetidir. Hizmeti [REST API](csharp.md) veya [SDK](video-search-sdk-quickstart.md) kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. İstekleri geçerli bir arama sorgusu ile API'ye gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.


## <a name="next-steps"></a>Sonraki adımlar

Bing Video Arama API [etkileşimli demo,](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) bir arama sorgusunu nasıl özelleştirebileceğinizi ve web'de video araması yapabileceğinizi gösterir.

API'yi çağırmaya hazır olduğunuzda, bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Hızlı [başlangıç](csharp.md) ile hızlı bir şekilde ilk API isteği ile başlamak için kullanın.

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Video Arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference) başvuru sayfası, arama sonuçlarını istemek için kullanılan uç noktaların, üstbilgi ve sorgu parametrelerinin listesini içerir.

* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./useanddisplayrequirements.md) konusunda belirtilmektedir.

* Diğer kullanılabilir API'leri keşfetmek için [Bing Arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.