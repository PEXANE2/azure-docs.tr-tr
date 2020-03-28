---
title: Bing Yazım Denetimi API’si nedir?
titleSuffix: Azure Cognitive Services
description: Bağlamsal yazım denetimi için makine öğrenimi ve istatistiksel makine çevirisini kullanan Bing Yazım Denetimi API'sı hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: overview
ms.date: 12/19/2019
ms.author: aahi
ms.openlocfilehash: 5586704e098fc568c714e779c2eed44aa3d1df9c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75383008"
---
# <a name="what-is-the-bing-spell-check-api"></a>Bing Yazım Denetimi API’si nedir?

Bing Yazım Denetimi API, metin üzerinde bağlamsal dilbilgisi ve yazım denetimi gerçekleştirmenize olanak tanır. Çoğu yazım denetleyicisi sözlük tabanlı kural kümelerine güvenirken, Bing yazım denetleyicisi doğru ve bağlamsal düzeltmeler sağlamak için makine öğrenimi ve istatistiksel makine çevirisinden yararlanır. 

## <a name="features"></a>Özellikler


|  |  |
|---------|---------|
|Birden çok yazım denetimi modu     | Birden çok yazım denetimi modu, düzeltmelerin dilbilgisi ve/veya yazım asına odaklanmasını sağlar. |
|Argo ve gayri resmi dil tanıma     | Metinde kullanılan ortak ifadeleri ve resmi olmayan terimleri tanıyın.         |
|Benzer sözcükler arasında ayrım     | Benzer görünen ancak anlam olarak farklı olan sözcükler arasındaki doğru kullanımı bulma (örneğin, "bkz" ve "deniz")        |
|Marka, unvan ve popüler kullanım desteği     | Yeni markaları, başlıkları ve diğer popüler ifadeleri ortaya çıktıklarında tanıyın |

## <a name="workflow"></a>İş akışı

Bing Yazım Denetimi API'sini HTTP isteklerini gerçekleştirebilen ve JSON yanıtlarını ayrıştırabilen herhangi bir programlama dilinden aramak kolaydır. Hizmete REST API veya Bing Yazım Denetimi SDK'ları kullanılarak erişilebilir. 

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](../cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa ücretsiz hesap oluşturabilirsiniz. 
2. Bing Web Araması API'sine yönelik bir istek gönderin.
3. Ayrışdırış JSON yanıtı

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, çeşitli metinleri nasıl hızlı bir şekilde kontrol edebileceğinizi görmek için Bing Büyüsü Denetimi Arama API [etkileşimli demoyu](https://azure.microsoft.com/services/cognitive-services/spell-check/) deneyin.

API'yi çağırmaya hazır olduğunuzda, bir [Bilişsel hizmetler API hesabı](../../cognitive-services/cognitive-services-apis-create-account.md) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Diğer kullanılabilir API'leri keşfetmek için [Bing Arama API hub sayfasını](../bing-web-search/search-the-web.md) da ziyaret edebilirsiniz.