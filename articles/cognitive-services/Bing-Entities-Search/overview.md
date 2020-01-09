---
title: Bing Varlık Arama API'si nedir?
titleSuffix: Azure Cognitive Services
description: Arama sorgularından varlıkları ve yerleri ayıklamak ve aramak için Bing Varlık Arama API'si kullanın.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: 2f374e29f4dc5406956cd56d1bb0bd1466e65773
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75384528"
---
# <a name="what-is-bing-entity-search-api"></a>Bing Varlık Arama API'si nedir?

Bing Varlık Arama API'si, Bing'e bir arama sorgusu gönderip varlıkları ve yerleri içeren sonuçlar alır. Yer sonuçları restoranlar, oteller veya diğer yerel işletmeleri kapsar. Sorguda yerel işletmenin adı belirtildiğinde veya bir işletme türü istendiğinde (yakınımdaki restoranlar gibi) Bing, yerleri döndürür. Sorgu iyi bilinen kişileri, yerleri (toist, eyalet, ülke/bölge vb.) veya şeyleri belirtiyorsa Bing varlık döndürür.

|Özellik  |Açıklama  |
|---------|---------|
|[Gerçek zamanlı arama önerileri](concepts/search-for-entities.md#suggest-search-terms-with-the-bing-autosuggest-api)     | Kullanıcılarınızın yazmasıyla açılan bir liste olarak görüntülenebilen arama önerileri sağlar.       | 
| [Varlık Kesinleştirme](concepts/search-for-entities.md#the-bing-entity-search-api-response)  | Birden çok olası anlam içeren sorgular için birden çok varlık alın. |
| [Yerleri bul](concepts/search-for-entities.md#find-places) | Yerel işletmeler ve varlıklar hakkında bilgi arayın ve geri dönüş yapın  |

## <a name="workflow"></a>İş Akışı

Bing Varlık Arama API'si, yeniden bir Web hizmetidir ve HTTP istekleri yapıp JSON 'u ayrıştırabilen herhangi bir programlama dilinden çağrı yapmayı kolaylaştırır. REST API veya SDK kullanarak hizmeti kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. API 'ye, geçerli bir arama sorgusuyla bir istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bing Varlık Arama API'si için [etkileşimli tanıtımı](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) deneyin. 
* İlk isteğinizi hızlı bir şekilde kullanmaya başlamak için hızlı [Başlangıç](quickstarts/csharp.md)yapmayı deneyin.
* [Bing varlık arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) Reference bölümü.
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./use-display-requirements.md) konusunda belirtilmektedir.
* Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.