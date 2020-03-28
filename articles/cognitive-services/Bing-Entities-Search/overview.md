---
title: Bing Varlık Arama API'si nedir?
titleSuffix: Azure Cognitive Services
description: Arama sorgularından varlıkları ve yerleri ayıklamak ve aramak için Bing Varlık Arama API'sini kullanın.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: 2f374e29f4dc5406956cd56d1bb0bd1466e65773
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75384528"
---
# <a name="what-is-bing-entity-search-api"></a>Bing Varlık Arama API'si nedir?

Bing Varlık Arama API'si, Bing'e bir arama sorgusu gönderip varlıkları ve yerleri içeren sonuçlar alır. Yer sonuçları restoranlar, oteller veya diğer yerel işletmeleri kapsar. Sorguda yerel işletmenin adı belirtildiğinde veya bir işletme türü istendiğinde (yakınımdaki restoranlar gibi) Bing, yerleri döndürür. Bing, sorguda tanınmış kişileri, yerleri (turistik yerler, eyaletler, ülkeler/bölgeler, vb.) veya başka şeyler belirtse, varlıkları döndürür.

|Özellik  |Açıklama  |
|---------|---------|
|[Gerçek zamanlı arama önerileri](concepts/search-for-entities.md#suggest-search-terms-with-the-bing-autosuggest-api)     | Kullanıcıyazarlarınızın türü nde açılır liste olarak görüntülenebilen arama önerileri sağlayın.       | 
| [Varlık ayrımı](concepts/search-for-entities.md#the-bing-entity-search-api-response)  | Birden çok olası anlamı olan sorgular için birden çok varlık alın. |
| [Yerleri bulma](concepts/search-for-entities.md#find-places) | Yerel işletmeler ve kuruluşlar hakkında bilgi arama ve iade  |

## <a name="workflow"></a>İş akışı

Bing Entity Search API, HTTP isteklerini gerçekleştirebilen ve JSON'u ayrıştırabilen herhangi bir programlama dilinden aramayı kolaylaştıran, yeniden hizmet veren bir web hizmetidir. Hizmeti REST API veya SDK kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. İstekleri geçerli bir arama sorgusu ile API'ye gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bing Entity Search API için [etkileşimli demoyu](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) deneyin. 
* İlk isteğinizle hızlı bir şekilde başlamak için [quickstart'ı](quickstarts/csharp.md)deneyin.
* [Bing Varlık Arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) başvuru bölümü.
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./use-display-requirements.md) konusunda belirtilmektedir.
* Diğer kullanılabilir API'leri keşfetmek için [Bing Arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.