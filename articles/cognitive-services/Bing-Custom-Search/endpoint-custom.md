---
title: Bing Özel Arama uç noktası
titleSuffix: Azure Cognitive Services
description: Bing Özel Arama API'si uç noktasının Özeti.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: e3b544d8d655b653383cc999de233e2163433d85
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405066"
---
# <a name="custom-search"></a>Özel Arama
Bing Özel Arama API’si, önemsediğiniz konulara özel olarak uyarlanmış arama deneyimleri oluşturmanızı sağlar. Kullanıcılarınız, arama sonucu sayfalarındaki alakasız içeriği ayıklamak zorunda kalmadan içeriğe göre oluşturulan arama sonuçlarını görebilir.

## <a name="custom-search-endpoint"></a>Özel arama uç noktası
Bing özel arama API'si kullanarak sonuçları almak için aşağıdaki uç noktaya bir `GET` istek gönderin. Daha fazla belirtim tanımlamak için üstbilgiler ve URL parametreleri kullanın.

Bkz Arama önerilerini kullanıcının tarafından `?q=""`tanımlanan girişle ilgili JSON sonuçları olarak döndürür.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Özel arama kaynaklarının nasıl ayarlanacağını anlatan örnekler için [öğreticiye](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page)bakın. Üstbilgiler, parametreler, Pazar kodları, yanıt nesneleri, hatalar vb. hakkında ayrıntılar için [Bing özel arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference) başvurusuna bakın.

## <a name="custom-search-response-json"></a>Özel arama yanıtı JSON
Özel bir arama isteği, sonuçları JSON nesneleri olarak döndürür, bkz. [Yanıt nesneleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Özel Otomatik Öneri
Özel otomatik öneri API 'SI, Bing 'e kısmi arama sorgusu dönemi göndermenizi ve yapılandırabileceğiniz önerilen sorguların bir listesini geri almanızı sağlar. Özel otomatik önerilerle, API tarafından döndürülen önerileri ekler ve isteğe bağlı olarak Bing tarafından oluşturulan önerilerin eklenip eklenmeyeceğini belirtin.

## <a name="custom-autosuggest-endpoint"></a>Özel otomatik öneri uç noktası
Özel sorgu önerileri istemek için şunu öğesine bir GET isteği gönderin:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Özel öneriler tanımlama hakkında daha fazla bilgi için bkz. [özel arama önerileri tanımlama](define-custom-suggestions.md).

## <a name="custom-image-search"></a>Özel Resim Arama
Özel Resim Arama API 'SI, Bing 'e bir arama sorgusu göndermenizi ve özel arama örneğinden ilgili görüntülerin bir listesini geri almanızı sağlar.

## <a name="custom-image-search-endpoint"></a>Özel Resim Arama uç noktası
Özel arama örneğinizin görüntülerini istemek için aşağıdaki URL 'ye bir GET isteği gönderin:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Özel arama örneği yapılandırma hakkında daha fazla bilgi için bkz. [özel arama deneyiminizi yapılandırma](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view).

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API 'leri türlerine göre sonuçlar döndüren arama eylemlerini destekler. Tüm arama uç noktaları sonuçları JSON yanıt nesneleri olarak döndürür.  Tüm uç noktalar, Boylam, enlem ve arama yarıçapı tarafından belirli bir dili ve/veya konumu döndüren sorguları destekler.

Her uç nokta tarafından desteklenen parametreler hakkında tüm bilgiler için, her tür için başvuru sayfalarına bakın.
Özel Arama API'si kullanan temel isteklerin örnekleri için bkz. [özel arama hızlı başlatır](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)
