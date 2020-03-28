---
title: Bing Otomatik Öneri nedir?
titleSuffix: Azure Cognitive Services
description: Bing Autosuggest API, arama kutusundaki kısmi sorgu dizesini temel alan önerilen sorguların listesini döndürür.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: a90fa0a66fb32b2a885599f09458964188353880
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448843"
---
# <a name="what-is-bing-autosuggest"></a>Bing Otomatik Öneri nedir?

Uygulamanız Bing Arama API'lerinden herhangi biri için sorgu gönderirse, kullanıcılarınızın arama deneyimini geliştirmek için Bing Otomatik Öner API'sini kullanabilirsiniz. Bing Autosuggest API, arama kutusundaki kısmi sorgu dizesini temel alan önerilen sorguların listesini döndürür. Karakterler arama kutusuna girilirken, önerileri açılır listede görüntüleyebilirsiniz.

## <a name="bing-autosuggest-api-features"></a>Bing Autosuggest API özellikleri

| Özellik                                                                                                                                                                                 | Açıklama                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](concepts/get-suggestions.md) | Önerilen arama terimlerini yazarken görüntülemek için Otomatik Öner API'sini kullanarak uygulama deneyiminizi geliştirin. |

## <a name="workflow"></a>İş akışı

Bing Autosuggest API, HTTP isteklerini gerçekleştirebilen ve JSON'u ayrıştırabilen herhangi bir programlama dilinden çağrılması kolay, yeniden hizmet veren bir web hizmetidir. 

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Bir kullanıcı uygulamanızın arama kutusuna her yeni karakter yazdığında bu API'ye istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

Genellikle, kullanıcı uygulamanızın arama kutusuna her yeni karakter yazdığında bu API'yi çağırırsınız. Daha fazla karakter girilen gibi, API daha alakalı önerilen arama sorguları döndürecek. Örneğin, API'nin tek bir tek `s` için geri dönebileceği öneriler, `sail`'' için olanlardan daha az alakalı olabilir.

Aşağıdaki örnekte, Bing Otomatik Öner API'sinden önerilen sorgu terimlerini içeren açılır bir arama kutusu gösterilmektedir.

![Otomatik öneri açılır arama kutusu listesi](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Bir kullanıcı açılır listeden bir öneri seçtiğinde, bing arama API'lerinden biriyle aramaya başlamak veya doğrudan Bing arama sonuçları sayfasına gitmek için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi hızlı bir şekilde başlatmak için bkz. [İlk Sorgunuzu Yapma](quickstarts/csharp.md).

[Bing Otomatik Öneri API’si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference) başvurusunu inceleyin. Başvuruda, önerilen sorgu terimlerini istemek için kullanacağınız uç noktaların, üst bilgilerin ve sorgu parametrelerinin listesinin yanı sıra yanıt nesnelerinin tanımları yer alır.

Diğer kullanılabilir API'leri keşfetmek için [Bing Arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.


[Bing Web Arama API'sini](../bing-web-search/search-the-web.md)kullanarak web'de nasıl arama yapılacağını ve diğer Bing Arama[API'lerini](../bing-web-search/index.yml)nasıl keşfederiz öğrenin.

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./useanddisplayrequirements.md)'ni okumayı unutmayın.
