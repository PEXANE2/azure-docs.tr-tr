---
title: 'Hızlı başlangıç: İlk Bing Özel Arama örneğinizi oluşturma'
titleSuffix: Azure Cognitive Services
description: Tanımladığınız etki alanları ve web sayfalarında arama yapabilen özel bir Bing örneği oluşturmak için bu hızlı başlatmayı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: b8287250df4e278d4904e31121ed7d2df208e1c9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80238850"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Hızlı başlangıç: İlk Bing Özel Arama örneğinizi oluşturma

Bing Özel Arama hizmetini kullanmak için görünümünüzü veya web dilimini tanımlayan özel bir arama örneği oluşturmanız gerekir. Bu örnek, arama yapmak istediğiniz genel etki alanlarını, web sitelerini ve web sayfalarını ve istediğiniz sıralama ayarlamalarını içerir. 

Örneği oluşturmak için [Bing Özel Arama portalını](https://customsearch.ai)kullanın. 

![Bing Özel Arama portalının bir resmi](media/blockedCustomSrch.png)

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-a-custom-search-instance"></a>Özel arama örneği oluşturma

Bing Özel Arama örneği oluşturmak için:

1. [Bing Özel Arama portalı](https://customsearch.ai) web sayfasında **Başlat'ı** tıklatın ve Microsoft hesabınızla oturum açın.

2. **Yeni Örnek'i**tıklatın ve açıklayıcı bir ad girin. Örneğinizin adını istediğiniz zaman değiştirebilirsiniz.
 
3. **Arama Deneyimi** bölümündeki **Etkin** sekmesine aramak istediğiniz bir veya daha fazla web sitesinin URL'sini girin. 

    > [!NOTE]
    > Bing Özel Arama örnekleri yalnızca ortak olan ve Bing tarafından dizine eklenmiş etki alanları ve web sayfaları için sonuçlar döndürecektir.

4. Bir sorgu girmek ve arama örneğiniz tarafından döndürülen arama sonuçlarını incelemek için Bing Özel Arama portalının sağ tarafını kullanabilirsiniz. Sonuç döndürülmezse, farklı bir URL girmeyi deneyin.  

5. Değişikliklerinizi üretim ortamında yayımlamak ve örneğin uç noktalarını güncelleştirmek için **Yayımla'yı** tıklatın.

6.  **Bitiş Noktaları**altındaki **Üretim** sekmesine tıklayın ve Özel **Yapılandırma Kimliğinizi**kopyalayın. Bu kimliğin, aramalarınızdaki `customconfig=` sorgu parametresine ekleyerek Özel Arama API'sini araması gerekir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Quickstart: Bing Özel Arama bitiş noktanızı arayın](./call-endpoint-csharp.md)
