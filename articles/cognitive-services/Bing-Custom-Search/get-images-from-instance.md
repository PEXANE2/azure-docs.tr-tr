---
title: Özel görünümünüzden resim alın - Bing Özel Arama
titleSuffix: Azure Cognitive Services
description: Web'in özel görünümünden resim almak için Bing Özel Arama'yı kullanma hakkında üst düzey genel bakış.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: scottwhi
ms.openlocfilehash: 5fa71c62969e0f1ff7e98f374eca63efa051c041
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "66390341"
---
# <a name="get-images-from-your-custom-view"></a>Özel görünümünüzden resim alın

Bing Özel Görseller Arama, özel arama deneyiminizi resimlerle zenginleştirmenizi sağlar. Özel arama, web sonuçlarına benzer şekilde örneğinizin web sitesi listesinde görüntü aramayı destekler. Görüntüleri Bing'in Özel Görseller Arama API'sini kullanarak veya Barındırılan Kullanıcı Arabirimi özelliğinden alabilirsiniz. Barındırılan Kullanıcı Arabirimi özelliğini kullanmak kolaydır ve arama deneyiminizi kısa sürede çalışır hale getirmek için önerilir.  Barındırılan Kullanıcı Arabirimi'nizi görüntüleri içerecek şekilde yapılandırma hakkında daha fazla bilgi [için, barındırılan Kullanıcı Arabirimi deneyiminizi yapılandırın'](hosted-ui.md)a bakın.

Arama sonuçlarını görüntüleme konusunda daha fazla denetim istiyorsanız, Bing'in Özel Görseller Arama API'sini kullanabilirsiniz. API'yi aramak Bing Resim Arama API'sini çağırmaya benzediği için, API'yi çağıran örnekler için Bing Resim Arama'yı kullanıma [çekin.](../Bing-Image-Search/overview.md) Ancak bunu yapmadan önce, Özel [Görseller Arama API başvuru](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference) içeriğine aşina olun. Ana farklar desteklenen sorgu parametreleri (özelConfig sorgu parametresini içermelisiniz) ve istek gönderdiğiniz bitiş noktasıdır.

<!--
## Next steps

[Call your custom view](search-your-custom-view.md)
-->
