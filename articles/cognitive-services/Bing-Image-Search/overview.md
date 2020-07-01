---
title: Bing Resim Arama API’si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API'si uygulamanızda Bing'in bilişsel resim arama özellikleri kullanmanıza olanak tanır. API ile kullanıcı arama sorguları göndererek Bing Resimler’e benzeyen ilgili ve yüksek kaliteli resimler alıp görüntüleyebilirsiniz.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: eb657c16f6f3ff67f4379134f3aa478f10d8ef94
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85603545"
---
# <a name="what-is-the-bing-image-search-api"></a>Bing Resim Arama API’si nedir?

Bing Resim Arama API'si, uygulamanızdaki Bing resim arama yeteneklerini kullanmanıza imkan sağlar. API 'ye arama sorguları göndererek, [Bing.com/images](https://www.bing.com/images)benzer yüksek kaliteli görüntüler edinebilirsiniz.

Bing Resim Arama API'si, yalnızca görüntü arama sonuçları sağladığından, Web üzerinde birçok türde içerik bulmak için kullanılabilir diğer [Bing Arama API'leri](../bing-web-search/bing-api-comparison.md) birleştirebilir veya kullanabilirsiniz.

## <a name="bing-image-search-features"></a>Bing Resim Arama özellikleri

| Özellik                                                                                                                                                                                 | Description                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) | Yazılmaya başladıkları anda önerilen arama terimleri görüntülemek için [Bing Otomatik Öneri API](../bing-autosuggest/get-suggested-search-terms.md)'sini kullanarak uygulama deneyimini iyileştirin. |
| [Resim sonuçlarını filtreleme ve kısıtlama](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images)                       | Sorgu parametrelerini düzenleyerek Bing'in getirdiği resimleri filtreleyin.                                                                                                       |
| [Küçük resimleri kırpma, yeniden boyutlandırma ve görüntüleme](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/resize-and-crop-thumbnails)                                                | Bing Resim Arama tarafından getirilen resimlerin küçük resim önizlemelerini düzenleyin ve görüntüleyin.                                                                                      |
| [Kullanıcı arama sorgularını özetleme ve genişletme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries)               | Sorgulara Bing'in önerdiği arama terimlerini dahil ederek ve görüntüleyerek arama özelliklerinizi genişletin.                                                                    |
| [Popüler resimler alma](trending-images.md)                                                                     | Dünyanın her yanından popüler resimler için bir aramayı özelleştirin.                                                                                                          |

## <a name="workflow"></a>İş akışı

Bing Resim Arama API'si HTTP istekleri yapabilen ve JSON ayrıştırabilen tüm programlama dillerinden kolayca çağrılan bir RESTful web hizmetidir. Hizmeti [REST API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp?) veya [SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/free/cognitive-services/).
2. İstekleri geçerli bir [arama sorgusu](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) ile API'ye gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, Bing Resim Arama API'sinin [etkileşimli tanıtımını](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) deneyin.
Bu tanırım arama sorgusunu hızlı bir şekilde özelleştirmeyi ve web'de resim aramayı göstermektedir.

İlk API isteğinize hızlı bir şekilde başlamak için aşağıdakileri öğrenebilirsiniz:

* REST API'sini kullanarak [Bing'e arama sorgusu gönderme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp) veya
* Bing'in getirdiği resimleri SDK kullanarak [isteme ve filtreleme](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

## <a name="see-also"></a>Ayrıca bkz.

* Bing Arama API'leri için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) . 

* [Bing resim arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) Reference bölümü, API 'nin uç noktaları, ÜSTBILGILERI, API yanıtları ve sorgu parametreleriyle ilgili bilgiler içerir.

* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./useanddisplayrequirements.md) konusunda belirtilmektedir.

* [Bing resim arama API'si makalesinde Web 'den görüntüleri alma](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images) , Web 'den görüntülerin nasıl aranacağını ve alınacağını açıklar.

* [Arama sorguları Ile gönderme ve çalışma](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) makalesinde, arama sorgularının nasıl yapılacağı, özelleştirileceği ve özetleneceğini açıklanmaktadır.

* Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.
