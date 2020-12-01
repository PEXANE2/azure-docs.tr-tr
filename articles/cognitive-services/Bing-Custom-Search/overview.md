---
title: Bing Özel Arama API'si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Özel Arama API'si, ilgilendiğiniz konularda özel arama deneyimleri oluşturmanıza olanak sağlar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: aahi
ms.openlocfilehash: e70d4d83fcd9641fa86f013688b5a3a6e3652919
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96338410"
---
# <a name="what-is-the-bing-custom-search-api"></a>Bing Özel Arama API'si nedir?

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Özel Arama API'si, ilgilendiğiniz konular için özel ad-ücretsiz arama deneyimleri oluşturmanıza olanak sağlar. Web 'in özel bir görünümünü oluşturmak ve kullanıcılarınızın ilgili arama sonuçlarını hızlıca bulmasına yardımcı olmak üzere, Bing için etki alanlarını ve Web sayfalarını, belirli içerikleri sabitlemek, artırmak veya indirgemek için de belirtebilirsiniz. 

## <a name="features"></a>Özellikler

|Özellik  |Açıklama  |
|---------|---------|
|[Özel gerçek zamanlı arama önerileri](define-custom-suggestions.md)     | Kullanıcılarınızın yazmasıyla açılan bir liste olarak görüntülenebilen arama önerileri sağlar.       | 
|[Özel görüntü arama deneyimleri](get-images-from-instance.md)     | Kullanıcılarınızın özel arama Örneğinizde belirtilen etki alanlarından ve Web sitelerinden görüntü aramasını sağlar.        |        
|[Özel video arama deneyimleri](get-videos-from-instance.md)     | Kullanıcılarınızın özel arama Örneğinizde belirtilen etki alanları ve sitelerde video aramasını sağlar.        |    
|[Özel arama örneğinizi paylaşma](share-your-custom-search.md)     | Ekip üyeleriniz ile paylaşarak arama örneğinizi işbirliği yaparak düzenleyin ve test edin.        | 
|[Uygulamalarınız ve Web siteleriniz için bir kullanıcı arabirimi yapılandırma](hosted-ui.md)     | , Web sayfalarınızı ve Web uygulamalarınızı JavaScript kod parçacığı olarak kolayca tümleştirebileceğinizi barındırılan bir kullanıcı arabirimi sağlar.        | 
## <a name="workflow"></a>İş akışı

[Bing özel arama portalını](https://customsearch.ai)kullanarak özelleştirilmiş bir arama örneği oluşturabilirsiniz. Portal, Bing aramasını istediğiniz etki alanlarını, Web sitelerini ve Web sayfalarını, arama yapılmasını istemediğiniz kişilerle birlikte belirten özel bir arama örneği oluşturmanızı sağlar. Ayrıca, Portal 'ı kullanarak şunları yapabilirsiniz: arama deneyimini önizleme, API 'nin sağladığı arama sıralamalarına göre ayarlama ve isteğe bağlı olarak, Web siteleriniz ve uygulamalarınızda işlenecek aranabilir bir kullanıcı arabirimi yapılandırma.

Arama örneğinizi oluşturduktan sonra, Bing Özel Arama API'si çağırarak onu (ve isteğe bağlı olarak bir kullanıcı arabirimini) Web sitenize veya uygulamanıza tümleştirebilirsiniz:

![API aracılığıyla Bing özel aramaya bağlanabildiğinizi gösteren resim](media/BCS-Overview.png "Bing Özel Arama nasıl kullanılır.")


## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya hızlıca başlamak için bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).

Arama örneğinizi özelleştirmeyle ilgili ayrıntılar için bkz. [Özel arama örneği tanımlama](define-your-custom-view.md).

Bing kullanımını okuduğunuzdan ve arama sonuçlarının hizmet ve uygulamalarınızda kullanılması için [gereksinimleri görüntülemenin gerekli](../bing-web-search/use-display-requirements.md) olduğundan emin olun.

Kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/overview.md) ziyaret edin.

Özel arama uç noktalarının başvuru bilgilerini incelemeniz önerilir. Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaları, üst bilgiler ve sorgu parametreleri yer alır. Ayrıca yanıt nesnelerinin tanımları da bulunur.

[!INCLUDE [cognitive-services-bing-url-note](../../../includes/cognitive-services-bing-url-note.md)]

- [Özel Arama API’si](/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference)
- [Özel Görüntü API'si](/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference)
- [Özel Video API'si](/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference)
- [Özel Otomatik Öneri API'si](/rest/api/cognitiveservices-bingsearch/bing-custom-autosuggest-api-v7-reference)