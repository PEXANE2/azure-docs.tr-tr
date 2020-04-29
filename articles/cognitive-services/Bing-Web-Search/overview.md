---
title: Bing Web Araması API'si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Web Araması API'si, Web araması sorgularına anında yanıt sağlayan bir yeniden hizmet veren hizmettir. Sonuçları Web sayfaları, görüntüler, videolar, Haberler ve daha fazlasını içerecek şekilde yapılandırın. Sonuçlar, JSON biçiminde sunulur ve aramayla ilgi düzeyini ve Bing Web Araması aboneliklerinizi temel alır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: overview
ms.date: 03/31/2020
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 5ea98a4ef759937c5962dc86f0384051b8fa0bb3
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80477471"
---
# <a name="what-is-the-bing-web-search-api"></a>Bing Web Araması API'si nedir?

Bing Web araması API'si, kullanıcı sorgularına anında yanıt sağlayan bir RESTful hizmetidir. Arama sonuçları; web sayfalarını, görüntüleri, videoları, haberleri, çevirileri ve daha fazlasını içerecek şekilde yapılandırılabilir. Bing Web Araması Sonuçları, arama ilgisi ve Bing Web Araması aboneliklerine göre JSON olarak sağlar.

Bu API, bir kullanıcının arama sorgusuyla ilişkili olan tüm içeriğe erişmesi gereken uygulamalar için idealdir. Yalnızca belirli bir sonuç türünü gerektiren bir uygulama oluşturuyorsanız [Bing Resim Arama API'si](../Bing-Image-Search/overview.md), [Bing Video Arama API'si](../Bing-Video-Search/search-the-web.md) veya [Bing Haber Arama API'sini](../Bing-News-Search/search-the-web.md) kullanabilirsiniz. Bing Arama API'lerinin tam listesi için bkz. [Bilişsel Hizmetler API'leri](https://docs.microsoft.com/azure/cognitive-services).

Nasıl çalıştığını görmek ister misiniz? [Bing Web Araması API'si tanıtımımızı](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) deneyin.

## <a name="features"></a>Özellikler  

Bing Web Araması yalnızca anında yanıtlar için erişim izni vermez. Ayrıca, kullanıcılarınız için arama sonuçlarını özelleştirmenize imkan tanıyan ek özellikler ve işlevler de sağlar.

| Özellik | Açıklama |
|---------|-------------|
| [Gerçek zamanlı olarak arama terimleri önerme](../bing-autosuggest/get-suggested-search-terms.md) | Yazılmaya başladıkları anda önerilen arama terimleri görüntülemek için Bing Otomatik Öneri API'sini kullanarak uygulama deneyimini iyileştirin. |
| [Sonuçları içerik türüne göre filtreleme ve kısıtlama](filter-answers.md) | Arama sonuçlarını web sayfalarına, görüntülere, videolara, güvenli aramaya ve daha fazlasına yönelik filtreler ve sorgu parametreleri ile özelleştirin ve geliştirin. |
| [Unicode karakterler için isabet vurgulama gerçekleştirme](hit-highlighting.md) | İsabet vurgulama sayesinde arama sonuçlarını kullanıcılara görüntülemeden önce içerdikleri istenmeyen Unicode karakterleri belirleyin ve kaldırın. |
| [Arama sonuçlarını ülkeye, bölgeye ve/veya pazara göre yerelleştirme](supported-countries-markets.md) | Bing Web Araması, otuz altıyı aşkın ülkeyi ve bölgeyi destekler. Arama sonuçlarını belirli bir ülke/bölge veya pazarla kısıtlamak için bu özelliği kullanın. |
| [Bing İstatistikleri ile arama ölçümlerini analiz etme](bing-web-stats.md) | Bing İstatistikleri, çağrı hacmi, en çok kullanılan sorgu dizeleri, coğrafi dağılım ve daha fazlası ile ilgili analizler sunan ücretli bir aboneliktir. |

## <a name="workflow"></a>İş akışı

Bing Web Araması API'si, HTTP isteklerinin yapılabildiği ve JSON yanıtlarının ayrıştırabildiği herhangi bir programlama dili ile kolayca çağrılabilir. Hizmete [REST API](quickstarts/python.md) veya [Bing Web araması istemci kitaplıkları](./quickstarts/client-libraries.md)kullanılarak erişilebilir.

1. Bing Arama API'leri için [bir Azure kaynağı oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) . Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).  
2. [Bing Web Araması API'sine yönelik bir istek](quickstarts/python.md) gönderin.
3. JSON yanıtını ayrıştırın.

## <a name="next-steps"></a>Sonraki adımlar

* Bing Web Araması API'sine ilk çağrınızı yapmak için [Python hızlı başlangıç](./quickstarts/client-libraries.md?pivots=programming-language-python) makalemizden yararlanın.  
* [Tek sayfalı bir Web uygulaması oluşturun](tutorial-bing-web-search-single-page-app.md).
* [Web Araması API'si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference) belgelerini gözden geçirin.  
* Bing Web Araması için [kullanım ve görüntüleme gereksinimleri](UseAndDisplayRequirements.md) hakkında daha fazla bilgi edinin.  
