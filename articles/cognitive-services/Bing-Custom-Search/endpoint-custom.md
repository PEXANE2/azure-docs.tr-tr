---
title: Bing Özel Arama uç noktası
titleSuffix: Azure Cognitive Services
description: Önemsediğiniz konular için özel arama deneyimleri oluşturun. Kullanıcılar, önem verdikleri içeriğe göre uyarlanmış arama sonuçlarını görür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 03ec22caedd4e317b9e1fe781dc3d983febc7a6d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74072779"
---
# <a name="custom-search"></a>Özel Arama
Bing Özel Arama API’si, önemsediğiniz konulara özel olarak uyarlanmış arama deneyimleri oluşturmanızı sağlar. Kullanıcılarınız, arama sonucu sayfalarındaki alakasız içeriği ayıklamak zorunda kalmadan içeriğe göre oluşturulan arama sonuçlarını görebilir.

## <a name="custom-search-endpoint"></a>Özel Arama Bitiş Noktası
Bing Özel Arama API'sini kullanarak `GET` sonuç almak için aşağıdaki bitiş noktasına bir istek gönderin. Diğer belirtimleri tanımlamak için üstbilgi ve URL parametrelerini kullanın.

Bitiş Noktası: Kullanıcının `?q=""`'tarafından tanımlanan girişiyle alakalı JSON sonuçları olarak arama önerilerini verir.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Özel Arama kaynaklarının nasıl ayarlandığını açıklayan örnekler için [öğreticiye](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page)bakın. Üstbilgiler, parametreler, pazar kodları, yanıt nesneleri, hatalar, vb. hakkında ayrıntılı bilgi için [Bing Özel Arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference) referansına bakın.

## <a name="custom-search-response-json"></a>Özel Arama Yanıtı JSON
Özel bir arama isteği sonuçları JSON nesneleri olarak döndürür, [Bkz. Yanıt nesneleri.](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#response-objects) 

## <a name="custom-autosuggest"></a>Özel Otomatik Öner
Özel Otomatik Öner API, Bing'e kısmi bir arama sorgusu terimi göndermenize ve yapılandırabileceğiniz önerilen sorguların listesini geri almanıza olanak tanır. Özel Otomatik Öner ile, API tarafından döndürülen öneriler ekler ve isteğe bağlı olarak Bing tarafından oluşturulan önerileri ekleyip eklemeyin imal etmeyeceğini belirtirsiniz.

## <a name="custom-autosuggest-endpoint"></a>Özel Autosuggest Bitiş Noktası
Özel sorgu önerileri istemek için, bir GET isteği gönderin:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Özel önerileri tanımlama hakkında bilgi [için](define-custom-suggestions.md)bkz.

## <a name="custom-image-search"></a>Özel Resim Arama
Özel Resim Arama API'si, Bing'e bir arama sorgusu göndermenize ve Özel Arama örneğinizdeki alakalı resimlerin listesini geri almanıza olanak tanır.

## <a name="custom-image-search-endpoint"></a>Özel Resim Arama Bitiş Noktası
Özel Arama örneğinden resim istemek için aşağıdaki URL'ye GET isteği gönderin:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Özel Arama örneğini yapılandırma hakkında bilgi [için](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view)bkz.

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API'leri, sonuçları türüne göre döndüren arama eylemlerini destekler.Tüm arama bitiş noktaları JSON yanıt nesneleri olarak sonuçları döndürer. Tüm uç noktalar, boylam, enlem ve arama yarıçapına göre belirli bir dili ve/veya konumu döndüren sorguları destekler.

Her bitiş noktası tarafından desteklenen parametreler hakkında tam bilgi için her tür için başvuru sayfalarına bakın.
Özel Arama API'sini kullanarak temel istek örnekleri için Bkz. [Özel Arama Hızlı başlangıçlar](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)
