---
title: Özel görünümlerinizden görüntüleri alın-Bing Özel Arama
titleSuffix: Azure Cognitive Services
description: Web 'in özel görünümünden görüntü almak için Bing Özel Arama kullanma hakkında üst düzey genel bakış.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: scottwhi
ms.openlocfilehash: 5fa71c62969e0f1ff7e98f374eca63efa051c041
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "66390341"
---
# <a name="get-images-from-your-custom-view"></a>Özel görünümlerinizden görüntüleri alın

Bing özel görüntüler arama, özel arama deneyiminizi görüntülerle zenginleştirmenizi sağlar. Özel arama, web sonuçlarına benzer şekilde örneğinizin web sitesi listesinde görüntü aramayı destekler. Resimleri, Bing 'in özel görüntülerini arama API 'sini kullanarak veya barındırılan Kullanıcı Arabirimi özelliği aracılığıyla alabilirsiniz. Barındırılan Kullanıcı arabirimi özelliğinin kullanılması basittir ve arama deneyiminizi kısa bir sırada çalışır duruma getirmek için önerilir.  Barındırılan Kullanıcı arabirimini görüntüler içerecek şekilde yapılandırma hakkında daha fazla bilgi için bkz. [barındırılan Kullanıcı arabirimi deneyiminizi yapılandırma](hosted-ui.md).

Arama sonuçlarının görüntülenmesi üzerinde daha fazla denetime sahip olmak istiyorsanız, Bing 'in özel görüntüler arama API 'sini kullanabilirsiniz. API 'YI çağırmak Bing Resim Arama API'si çağırmaya benzer olduğundan, API 'YI çağıran örnekler için [Bing resim arama](../Bing-Image-Search/overview.md) kullanıma alın. Ancak bunu yapmadan önce [özel görüntüler arama API 'si başvuru](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference) içeriği hakkında bilgi edinin. Ana farklılıklar, desteklenen sorgu parametreleridir (customConfig sorgu parametresini dahil etmeniz gerekir) ve isteklerin gönderileceği uç nokta.

<!--
## Next steps

[Call your custom view](search-your-custom-view.md)
-->
