---
title: Bing Haber Arama API’si nedir?
titleSuffix: Azure Cognitive Services
description: Web 'de başlıklar ve eğilim konuları dahil olmak üzere Kategoriler genelinde geçerli haber başlıklarını aramak için Bing Haber Arama API'si nasıl kullanacağınızı öğrenin.
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
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75448438"
---
# <a name="what-is-the-bing-news-search-api"></a>Bing Haber Arama API’si nedir?

Bing Haber Arama API'si, Bing 'in bilişsel haber arama yeteneklerini uygulamalarınıza tümleştirmeyi kolaylaştırır. API, [Bing Haberler](https://www.bing.com/news)'e benzer bir deneyim sunar ve bu sayede arama sorguları göndermenizi ve ilgili haber makalelerini almanızı sağlar.

Bing Haber Arama API'si yalnızca haber arama sonuçları sağladığını unutmayın. Diğer Web içeriği türleri için [Bing Web araması API'si](../bing-web-search/search-the-web.md), [Video Arama API](../bing-video-search/search-the-web.md) ve [resim arama API](../bing-image-search/overview.md) 'sini kullanın.

## <a name="bing-news-search-api-features"></a>Bing Haber Arama API'si özellikleri

Bing Haber Arama API'si öncelikle ilgili haber makalelerini bulup döndürse de, Web 'de akıllı ve odaklanmış haber alımı için çeşitli özellikler sağlar.

|Özellik  |Açıklama  |
|---------|---------|
|[Arama terimleri önerme ve kullanma](concepts/search-for-news.md#suggest-and-use-search-terms)     | Aranan arama terimlerini yazıldığı gibi göstermek için [Bing otomatik öneri API'si](../bing-autosuggest/get-suggested-search-terms.md) kullanarak arama deneyiminizi geliştirebilirsiniz.         |
|[Genel haberleri al](concepts/search-for-news.md#get-general-news)     | Bing Haber Arama API'si bir arama sorgusu göndererek ve ilgili haber makalelerinin listesini geri alarak haber bulun.           |
|[Bugünün en iyi haberleri](concepts/search-for-news.md#get-todays-top-news)      | Tüm Kategoriler genelinde güne ait en popüler haber hikayelerini alın.       |
|[Kategoriye göre Haberler](concepts/search-for-news.md)     | Belirli kategorilerdeki haberleri arayın.        | 
|[Başlık haberleri](concepts/search-for-news.md)     | Tüm Kategoriler genelinde en popüler başlıkları arayın.         |

## <a name="workflow"></a>İş Akışı

Bing Haber Arama API'si, yeniden bir Web hizmetidir ve HTTP istekleri yapıp JSON 'u ayrıştırabilen herhangi bir programlama dilinden çağrı yapmayı kolaylaştırır. REST API veya SDK kullanarak hizmeti kullanabilirsiniz.

1. Bing Arama API'leri erişimi olan bilişsel hizmetler API hesabı oluşturun. Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-news-api).

2. API 'ye, geçerli bir arama sorgusuyla bir istek gönderin.

3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, Bing Haber Arama API'si için [etkileşimli tanıtımı](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) deneyin. Bu demo, bir arama sorgusunu hızlı bir şekilde nasıl özelleştireceğinizi ve Web 'de haberleri nasıl bulabileceğinizi gösterir.

İlk API isteğinizi hızlıca kullanmaya başlamak için [REST API](quickstart.md) veya [SDK 'lardan](sdk.md)birine yönelik bir hızlı başlangıç yapın.

## <a name="see-also"></a>Ayrıca bkz.

* [Bing haber arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) Reference bölümü, görüntü tabanlı arama sonuçları istemek için kullanabileceğiniz uç noktalar, üst BILGILER, API yanıtları ve sorgu parametreleriyle ilgili tanımları ve bilgileri içerir.
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./useanddisplayrequirements.md) konusunda belirtilmektedir.
* Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.