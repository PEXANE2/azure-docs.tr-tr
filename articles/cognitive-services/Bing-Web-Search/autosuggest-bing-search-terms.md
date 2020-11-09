---
title: Otomatik öneri arama terimleri-Bing Web Araması API'si
titleSuffix: Azure Cognitive Services
description: Kullanıcılara gelişmiş bir arama deneyimi sağlamak için Bing Web Araması API'si Bing Otomatik Öneri API'si eşleştirin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 03/03/2019
ms.author: aahi
ms.openlocfilehash: 5bd5ff0e49b125277255ec8e5c216583d75043a0
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94381142"
---
# <a name="autosuggest-bing-search-terms-in-your-application"></a>Uygulamanızda Bing arama terimlerini otomatik önerme

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

Kullanıcıların arama terimlerini gireceği bir arama kutusu sağlıyorsanız deneyimi geliştirmek için [Bing Otomatik Öneri API'sini](../bing-autosuggest/get-suggested-search-terms.md) kullanın. API, kullanıcı yazarken kısmi arama terimlerine dayalı önerilen sorgu dizelerini yönetin.

Kullanıcı bir arama terimi girdikten sonra, [q](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#query) sorgu parametresi ayarlanmadan önce URL kodlamalı olmalıdır. Örneğin kullanıcı *sailing dinghies* terimini girerse `q` öğesini `sailing+dinghies` veya `sailing%20dinghies` olarak ayarlayın.

Sorgu terimi bir yazım hatası içeriyorsa, arama yanıtı bir [QueryContext](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#querycontext) nesnesi içerir. Nesne, özgün terimi ve Bing'in arama için kullandığı düzeltilmiş halini döndürür.

```json
"queryContext": {
    "originalQuery": "sialing dingy for sale",
    "alteredQuery": "sailing dinghy for sale",
    "alterationOverrideQuery": "+sialing +dingy for sale"
}
```

Bu bilgileri, kullanıcının arama sonuçlarını görüntülerken Sorgu dizelerini değiştirdiklerini bilmesini sağlamak için kullanabilirsiniz.

![Sorgu bağlamı UX örneği](./media/cognitive-services-bing-web-api/bing-query-context.PNG)  

## <a name="next-steps"></a>Sonraki adımlar  

* Örnek [Bing Web araması API'si yanıtlarını](search-responses.md)gözden geçirin.  

## <a name="see-also"></a>Ayrıca bkz.  

* [Bing Web Araması API'si Başvurusu](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference)