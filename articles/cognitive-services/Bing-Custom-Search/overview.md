---
title: Bing Özel Arama API'si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Özel Arama API'si, önem verdiğiniz konular için özel arama deneyimleri oluşturmanıza olanak tanır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: aahi
ms.openlocfilehash: 12c255f0e4ed11ce8acbd762530604218f8bc12b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448733"
---
# <a name="what-is-the-bing-custom-search-api"></a>Bing Özel Arama API'si nedir?

Bing Özel Arama API'si, önem verdiğiniz konular için özel reklamsız arama deneyimleri oluşturmanıza olanak tanır. Bing'in arama sıiçin etki alanlarını ve web sayfalarını belirtebilir, web'in özel bir görünümünü oluşturmak ve kullanıcılarınızın alakalı arama sonuçlarını hızla bulmasına yardımcı olmak için belirli içeriği pinleyebilir, öne edebilir veya düşürebilirsiniz. 

## <a name="features"></a>Özellikler

|Özellik  |Açıklama  |
|---------|---------|
|[Özel gerçek zamanlı arama önerileri](define-custom-suggestions.md)     | Kullanıcıyazarlarınızın türü nde açılır liste olarak görüntülenebilen arama önerileri sağlayın.       | 
|[Özel görüntü arama deneyimleri](get-images-from-instance.md)     | Kullanıcılarınızın özel arama örneğinde belirtilen etki alanlarından ve web sitelerinden görüntüleri aramasını etkinleştirin.        |        
|[Özel video arama deneyimleri](get-videos-from-instance.md)     | Kullanıcılarınızın özel arama örneğinde belirtilen etki alanlarından ve sitelerden video aramalarına olanak tanır.        |    
|[Özel arama örneğinizi paylaşma](share-your-custom-search.md)     | Ekibinizin üyeleriyle paylaşarak arama örneğini birlikte düzenleme ve test edin.        | 
|[Uygulamalarınız ve web siteleriniz için bir Kullanıcı Arabirimi yapılandırma](hosted-ui.md)     | Ekibinizin üyeleriyle paylaşarak arama örneğini birlikte düzenleme ve test edin.        | 
## <a name="workflow"></a>İş akışı

[Bing Özel Arama portalını](https://customsearch.ai)kullanarak özelleştirilmiş bir arama örneği oluşturabilirsiniz. Portal, Bing'in aramasını istediğiniz etki alanlarını, web sitelerini ve web sayfalarını ve aramasını istemediğiniz alanları belirten özel bir arama örneği oluşturmanıza olanak tanır. Portalı şu şekilde de kullanabilirsiniz: arama deneyimini önizleme, API'nin sağladığı arama sıralamalarını ayarlamak ve isteğe bağlı olarak web sitelerinizde ve uygulamalarınızda oluşturulacak şekilde aranabilir bir kullanıcı arabirimi yapılandırın.

Arama örneğini oluşturduktan sonra, Bing Özel Arama API'sini arayarak bu örneği (ve isteğe bağlı olarak bir kullanıcı arabirimi) web sitenize veya uygulamanıza entegre edebilirsiniz:

![API üzerinden Bing özel aramabağlanabileceğinizi gösteren resim](media/BCS-Overview.png "Bing Özel Arama nasıl çalışır.")


## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya hızlıca başlamak için bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).

Arama örneğinizi özelleştirmeyle ilgili ayrıntılar için bkz. [Özel arama örneği tanımlama](define-your-custom-view.md).

Hizmetlerinizde ve uygulamalarınızda arama sonuçlarını kullanmak için [Bing Kullanım ve Görüntüleme Gereksinimlerini](./use-and-display-requirements.md) okuduğunuzdan emin olun.

Diğer kullanılabilir API'leri keşfetmek için [Bing Arama API hub sayfasını](../bing-web-search/search-the-web.md) ziyaret edin.

Özel arama uç noktalarının başvuru bilgilerini incelemeniz önerilir. Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaları, üst bilgiler ve sorgu parametreleri yer alır. Ayrıca yanıt nesnelerinin tanımları da bulunur.

[!INCLUDE [cognitive-services-bing-url-note](../../../includes/cognitive-services-bing-url-note.md)]

- [Özel Arama API’si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference)
- [Özel Görüntü API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference)
- [Özel Video API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference)
- [Özel Otomatik Öneri API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-autosuggest-api-v7-reference)

