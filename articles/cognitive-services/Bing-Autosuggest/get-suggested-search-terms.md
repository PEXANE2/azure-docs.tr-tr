---
title: Bing Otomatik Öneri nedir?
titleSuffix: Azure Cognitive Services
description: Bing Otomatik Öneri API'si, arama kutusundaki kısmi sorgu dizesine göre önerilen sorguların bir listesini döndürür.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: a0187a5d2be6b2f93897fc358e26ad095e5a70f7
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94364590"
---
# <a name="what-is-bing-autosuggest"></a>Bing Otomatik Öneri nedir?

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

Uygulamanız Bing Arama API'leri herhangi birine sorgu gönderiyorsa, kullanıcılarınızın arama deneyimini geliştirmek için Bing Otomatik Öneri API'si kullanabilirsiniz. Bing Otomatik Öneri API'si, arama kutusundaki kısmi sorgu dizesine göre önerilen sorguların bir listesini döndürür. Arama kutusuna karakterler girildiğinde, önerileri bir açılan listede görüntüleyebilirsiniz.

## <a name="bing-autosuggest-api-features"></a>Bing Otomatik Öneri API'si özellikleri

| Özellik                                                                                                                                                                                 | Açıklama                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](concepts/get-suggestions.md) | Otomatik öneri API 'sini kullanarak, girilen arama koşullarını göstermek için uygulama deneyiminizi geliştirebilirsiniz. |

## <a name="workflow"></a>İş akışı

Bing Otomatik Öneri API'si, HTTP istekleri yapan ve JSON 'u ayrıştırabilen herhangi bir programlama dilinden kolayca çağrılacak bir Web hizmetidir.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](../cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/free/cognitive-services/).
2. Her kullanıcı uygulamanızın arama kutusuna yeni bir karakter yazdığında, bu API 'ye bir istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

Genellikle, kullanıcı uygulamanızın arama kutusuna her yeni bir karakter yazdığında bu API 'YI çağırın. Daha fazla karakter girildiğinde, API daha ilgili önerilen arama sorgularını geri döndürür. Örneğin, API 'nin tek başına döndürebilecek öneriler, ' `s` den daha az ilgili olabilir `sail` .

Aşağıdaki örnek, Bing Otomatik Öneri API'si önerilen sorgu koşullarına sahip bir açılan arama kutusunu gösterir.

![Otomatik öneri açılır arama kutusu listesi](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Kullanıcı aşağı açılan listeden bir öneri seçtiğinde, Bing Arama API'leri birini kullanarak aramaya başlayabilir veya doğrudan Bing arama sonuçları sayfasına gidebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi hızlı bir şekilde başlatmak için bkz. [İlk Sorgunuzu Yapma](quickstarts/csharp.md).

[Bing Otomatik Öneri API’si v7](/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference) başvurusunu inceleyin. Başvuruda, önerilen sorgu terimlerini istemek için kullanacağınız uç noktaların, üst bilgilerin ve sorgu parametrelerinin listesinin yanı sıra yanıt nesnelerinin tanımları yer alır.

Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/overview.md) ziyaret edin.


[Bing Web araması API'si](../bing-web-search/overview.md)kullanarak Web 'de arama yapmayı öğrenin ve diğer[Bing Arama API'leri](../bing-web-search/index.yml)keşfedebilirsiniz.

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](../bing-web-search/use-display-requirements.md)'ni okumayı unutmayın.