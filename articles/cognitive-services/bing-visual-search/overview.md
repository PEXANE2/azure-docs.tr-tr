---
title: Bing Görsel Arama API’si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama bir resimle ilgili olarak benzer resimler veya alışveriş kaynakları gibi ayrıntılar veya içgörüler sağlar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 12/19/2019
ms.author: scottwhi
ms.openlocfilehash: 52e3e65fb4f42530373decca547674a28a60f759
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75446668"
---
# <a name="what-is-the-bing-visual-search-api"></a>Bing Görsel Arama API’si nedir?

Bing Görsel Arama API'si, görüntü yle ilgili öngörüleri döndürür. Bir resim yükleyebilir veya birine URL sağlayabilirsiniz. Öngörüler görsel olarak benzer görüntüler, alışveriş kaynakları, görüntüyü içeren web sayfaları ve daha fazlasıdır. Bing Görsel Arama API'si tarafından döndürülen öngörüler, Bing.com/images'da gösterilenlere benzer. 

Bing Resim [Arama API'sini](../bing-image-search/overview.md)kullanıyorsanız, resim yüklemek yerine Bing Görsel Aramanız için bu API'nin arama sonuçlarından gelen içgörü belirteçlerini kullanabilirsiniz.

> [!IMPORTANT]
> Bing Resim Arama API'sini kullanarak görüntü öngörüleri alırsanız, daha kapsamlı öngörüler sağlayan Bing Görsel Arama API'sine geçmeyi düşünün.

## <a name="insights"></a>Insights

Bing Görsel Arama'yı kullanarak aşağıdaki bilgileri keşfedebilirsiniz:

| İçgörü                              | Açıklama |
|--------------------------------------|-------------|
| Görsel olarak benzer görüntüler              | Giriş görüntüsüne görsel olarak benzeyen görüntülerin listesi. |
| Görsel olarak benzer ürünler            | Görsel olarak gösterilen ürüne benzer ürünler.            |
| Alışveriş kaynakları                     | Giriş görüntüsünde gösterilen öğeyi satın alabileceğiniz yerler.            |
| İlgili aramalar                     | Başkaları tarafından yapılan veya resmin içeriğine dayanan ilgili aramalar.            |
| Resmi içeren web sayfaları     | Giriş görüntüsünü içeren web sayfaları.            |
| Tarifler                              | Giriş görüntüsünde gösterilen yemeği yapmak için tarifler içeren web sayfaları.            |
| Varlıklar                             | Tanınmış insanlar, yerler ve diğer şeyler. |

Bing Görsel Arama, öngörülere ek olarak, giriş görüntüsünden türetilen çeşitli terimleri (diğer bir deyişle etiketler) döndürür. Etiketler, kullanıcıların resimde bulunan kavramları keşfetmesini sağlar. Örneğin, giriş görüntüsü ünlü bir atlete aitse, etiketlerden biri sporcunun adı olabilir, başka bir etiket Spor olabilir. Veya, giriş görüntüsü bir elmalı turta ise, etiketleri Elmalı Turta, Pasta ve Tatlılar olabilir.

Bing Görsel Arama sonuçları, görüntüyle ilgili bölgeler için sınırlayıcı kutuları da içerir. Örneğin, görüntü birden çok ünlü içeriyorsa, sonuçlar tanınan ünlülerin her biri için sınırlayıcı kutular içerebilir. Veya Bing görüntüde bir ürün veya giysi tanıyorsa, sonuç tanınan öğe için bir sınırlayıcı kutu içerebilir.

## <a name="workflow"></a>İş akışı

Bing Görsel Arama API'si, HTTP isteklerini gerçekleştirebilen ve JSON'u ayrıştırabilen herhangi bir programlama dilinden aramayı kolaylaştıran, yeniden hizmet veren bir web hizmetidir. Hizmet için REST API veya SDK'yı kullanabilirsiniz.

1. Bing Arama API'lerine erişmek için bir [Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa, ücretsiz bir [hesap oluşturabilirsiniz.](https://azure.microsoft.com/free/) Ücretsiz deneme sürümünü etkinleştirdikten sonra abonelik anahtarınızı Hesabınızı oluşturduktan sonra [Azure portalından](../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) veya [Azure web sitesinden](https://azure.microsoft.com/try/cognitive-services/my-apis) alabilirsiniz.
2. Geçerli bir arama sorgusuyla API'ye istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, Bing Görsel Arama API [etkileşimli demo](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/)deneyin.
Demo, bir arama sorgusunu nasıl hızlı bir şekilde özelleştirebileceğinizi ve resimler için web'i nasıl tarayabildiğinizi gösterir.

Hızlı bir şekilde ilk isteğiile başlamak için, quickstarts bakın: [C #](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [düğümü.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Görsel Ler - Görsel Arama](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) başvurusu, resim tabanlı arama sonuçları istemek için kullanabileceğiniz uç noktalar, istek üstbilgileri, yanıtlar ve sorgu parametreleri üzerindeki tanımları ve bilgileri açıklar.

* [Bing Arama API kullanımı ve görüntüleme gereksinimleri,](../bing-web-search/use-display-requirements.md) Bing arama API'leri aracılığıyla elde edilen içeriğin ve bilgilerin kabul edilebilir kullanımlarını belirtir.

* Diğer kullanılabilir API'leri keşfetmek için [Bing Arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.