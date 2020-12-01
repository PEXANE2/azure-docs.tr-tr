---
title: Bing Varlık Arama API'si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Varlık Arama API'si ilgili ayrıntıları ve arama sorgularından varlıkları ve yerleri ayıklama ve arama hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: 2a3d971ce9a4f89555eb3ffa489f8b19172a4b83
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351478"
---
# <a name="what-is-bing-entity-search-api"></a>Bing Varlık Arama API'si nedir?

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Varlık Arama API'si, Bing'e bir arama sorgusu gönderip varlıkları ve yerleri içeren sonuçlar alır. Yer sonuçları restoranlar, oteller veya diğer yerel işletmeleri kapsar. Sorguda yerel işletmenin adı belirtildiğinde veya bir işletme türü istendiğinde (yakınımdaki restoranlar gibi) Bing, yerleri döndürür. Sorgu iyi bilinen kişileri, yerleri (toist, eyalet, ülke/bölge vb.) veya şeyleri belirtiyorsa Bing varlık döndürür.

|Özellik  |Açıklama  |
|---------|---------|
|[Gerçek zamanlı arama önerileri](concepts/search-for-entities.md#suggest-search-terms-with-the-bing-autosuggest-api)     | Kullanıcılarınızın yazmasıyla açılan bir liste olarak görüntülenebilen arama önerileri sağlar.       | 
| [Varlık Kesinleştirme](concepts/search-for-entities.md#the-bing-entity-search-api-response)  | Birden çok olası anlam içeren sorgular için birden çok varlık alın. |
| [Yerleri bul](concepts/search-for-entities.md#find-places) | Yerel işletmeler ve varlıklar hakkında bilgi arayın ve geri dönüş yapın  |

## <a name="workflow"></a>İş akışı

Bing Varlık Arama API'si, yeniden bir Web hizmetidir ve HTTP istekleri yapıp JSON 'u ayrıştırabilen herhangi bir programlama dilinden çağrı yapmayı kolaylaştırır. Hizmeti REST API veya SDK kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](../cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/free/cognitive-services/).
2. İstekleri geçerli bir arama sorgusu ile API'ye gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bing Varlık Arama API'si için [etkileşimli tanıtımı](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) deneyin. 
* İlk isteğinizi hızlı bir şekilde kullanmaya başlamak için hızlı [Başlangıç](quickstarts/csharp.md)yapmayı deneyin.
* [Bing varlık arama API'si v7](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) Reference bölümü.
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](../bing-web-search/use-display-requirements.md) konusunda belirtilmektedir.
* Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/overview.md) ziyaret edin.