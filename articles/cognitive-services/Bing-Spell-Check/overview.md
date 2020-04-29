---
title: Bing Yazım Denetimi API’si nedir?
titleSuffix: Azure Cognitive Services
description: Bağlama yazım denetimi için makine öğrenimi ve istatistiksel makine çevirisi kullanan Bing Yazım Denetimi API'si hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 12/19/2019
ms.author: aahi
ms.openlocfilehash: 5586704e098fc568c714e779c2eed44aa3d1df9c
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75383008"
---
# <a name="what-is-the-bing-spell-check-api"></a>Bing Yazım Denetimi API’si nedir?

Bing Yazım Denetimi API'si, metin üzerinde bağlamsal dilbilgisi ve yazım denetimi gerçekleştirmenize olanak sağlar. Çoğu yazım denetimcisi sözlük tabanlı kural kümelerine güvenirken, Bing yazım denetleyicisi doğru ve bağlamsal düzeltmeler sağlamak için makine öğrenimi ve istatistiksel makine çevirisi kullanır. 

## <a name="features"></a>Özellikler


|  |  |
|---------|---------|
|Birden çok yazım denetimi modu     | Birden çok yazım denetimi modu, dilbilgisi ve/veya yazım denetimine odaklanan düzeltmeler almanızı sağlar. |
|Slang ve resmi olmayan dil tanıma     | Metinde kullanılan ortak ifadeleri ve resmi olmayan terimleri TANIIN.         |
|Benzer sözcükler arasında ayrım yapın     | Sese benzer ancak anlamı farklıdır (örneğin, "bkz." ve "Sea") sözcükler arasında doğru kullanımı bulur        |
|Marka, başlık ve popüler kullanım desteği     | Yeni markalar, başlıkları ve diğer popüler ifadeleri ortaya çıktı olarak tanır |

## <a name="workflow"></a>İş akışı

Bing Yazım Denetimi API'si, HTTP istekleri yapan ve JSON yanıtlarını ayrıştırabilen herhangi bir programlama dilinden kolayca çağrılabilir. Hizmete REST API veya Bing Yazım Denetimi SDK 'Ları kullanılarak erişilebilir. 

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](../cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa ücretsiz hesap oluşturabilirsiniz. 
2. Bing Web Araması API'sine yönelik bir istek gönderin.
3. JSON yanıtını Ayrıştır

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, çeşitli metinleri hızlı bir şekilde nasıl kontrol etbileceğinizi görmek için Bing Yazım Denetimi Search API [etkileşimli tanıtımı](https://azure.microsoft.com/services/cognitive-services/spell-check/) ' nı deneyin.

API'yi çağırmaya hazır olduğunuzda, bir [Bilişsel hizmetler API hesabı](../../cognitive-services/cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Ayrıca, kullanılabilir diğer API 'Leri araştırmak için [BING arama API hub sayfasını](../bing-web-search/search-the-web.md) da ziyaret edebilirsiniz.