---
title: Bing Haber Arama API’si nedir?
titleSuffix: Azure Cognitive Services
description: Web'de manşetler ve popüler konular da dahil olmak üzere kategoriler arasında geçerli başlıkları aramak için Bing Haberler Arama API'sini nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: e0e99c7f677173c64afad3109b2f4accd7cb3cb9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448438"
---
# <a name="what-is-the-bing-news-search-api"></a>Bing Haber Arama API’si nedir?

Bing Haberler Arama API'si, Bing'in bilişsel haber arama yeteneklerini uygulamalarınız için entegre etmeyi kolaylaştırır. API, arama sorguları göndermenize ve ilgili haber makalelerini almanıza izin vererek [Bing Haberler'e](https://www.bing.com/news)benzer bir deneyim sağlar.

Bing Haberler Arama API'sinin yalnızca haber arama sonuçları sağladığını unutmayın. Diğer web içeriği türleri için [Bing Web Arama API'sini,](../bing-web-search/search-the-web.md)Video [Arama API'sini](../bing-video-search/search-the-web.md) ve [Resim Arama API'sini](../bing-image-search/overview.md) kullanın.

## <a name="bing-news-search-api-features"></a>Bing Haberler Arama API özellikleri

Bing Haber Arama API'si öncelikle alakalı haber makalelerini bulur ve döndürür, ancak web'de akıllı ve odaklanmış haber alma için çeşitli özellikler sağlar.

|Özellik  |Açıklama  |
|---------|---------|
|[Arama terimleri önerme ve kullanma](concepts/search-for-news.md#suggest-and-use-search-terms)     | Önerilen arama terimlerini yazarken görüntülemek için [Bing Autosuggest API'sını](../bing-autosuggest/get-suggested-search-terms.md) kullanarak arama deneyiminizi geliştirin.         |
|[Genel haberleri alın](concepts/search-for-news.md#get-general-news)     | Bing Haberler Arama API'sine bir arama sorgusu göndererek ve alakalı haber makalelerinin listesini geri alarak haberleri bulun.           |
|[Bugünün en iyi haberleri](concepts/search-for-news.md#get-todays-top-news)      | Tüm kategorilerde günün en iyi haberlerini alın.       |
|[Kategoriye göre haberler](concepts/search-for-news.md)     | Belirli kategorilerde haber arayın.        | 
|[Manşet haberleri](concepts/search-for-news.md)     | Tüm kategorilerdeki en iyi başlıkları arayın.         |

## <a name="workflow"></a>İş akışı

Bing Haberler Arama API kolay HTTP istekleri yapabilir ve JSON ayrıştırmak herhangi bir programlama dilinden aramak için yapım, bir RESTful web hizmetidir. Hizmeti REST API veya SDK kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir Bilişsel Hizmetler API'si hesabı oluşturun. Azure aboneliğiniz yoksa, ücretsiz bir [hesap oluşturabilirsiniz.](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-news-api)

2. İstekleri geçerli bir arama sorgusu ile API'ye gönderin.

3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, Bing Haberler Arama API'si için [etkileşimli demoyu](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) deneyin. Bu demo, bir arama sorgusunu nasıl hızlı bir şekilde özelleştirebileceğinizi ve web'de haberleri nasıl bulabileceğinizi gösterir.

İlk API isteğinize hızla başlamak için [REST API'sı](quickstart.md) veya [SDK'lardan](sdk.md)biri için hızlı bir başlangıç yapmayı deneyin.

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Haberler Arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) başvuru bölümü, resim tabanlı arama sonuçları istemek için kullanabileceğiniz uç noktalar, üstbilgiler, API yanıtları ve sorgu parametreleri hakkında tanımlar ve bilgiler içerir.
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./useanddisplayrequirements.md) konusunda belirtilmektedir.
* Diğer kullanılabilir API'leri keşfetmek için [Bing Arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.