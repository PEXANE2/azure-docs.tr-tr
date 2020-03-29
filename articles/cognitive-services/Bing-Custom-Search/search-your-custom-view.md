---
title: Özel görünümde arama - Bing Özel Arama
titleSuffix: Azure Cognitive Services
description: Özel arama deneyiminizi yapılandırdıktan sonra Bing Özel Arama portalı içinden test edebilirsiniz.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: aahi
ms.openlocfilehash: f00ffee47e3eb6366d632d8b6ee9beb01f048442
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76983121"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Portaldan Bing Özel Arama örneğini arayın

Özel arama deneyiminizi yapılandırıldıktan sonra Bing Özel Arama [portalı](https://customsearch.ai)içinden test edebilirsiniz. 

![Bing özel arama portalının ekran görüntüsü](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Arama sorgusu oluşturma 

Bing Özel Arama [portalında](https://customsearch.ai)oturum imzaladıktan sonra arama örneğini seçin ve **Üretim** sekmesini tıklatın. **Uç Noktaları**altında bir API bitiş noktası (örneğin, Web API) seçin. Aboneliğiniz hangi uç noktaların gösterilenleri belirler.

Arama sorgusu oluşturmak için bitiş noktanızın parametre değerlerini girin. Portalda görüntülenen parametrelerin seçtiğiniz uç noktaya bağlı olarak değişebileceğini unutmayın. Daha fazla bilgi için [Özel Arama API başvurusuna](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters) bakın. Arama örneğinizin kullandığı aboneliği değiştirmek için uygun abonelik anahtarını ekleyin ve uygun pazar ve/veya dil parametrelerini güncelleştirin.

Bazı önemli parametreler aşağıda verilmiştir:


|Parametre  |Açıklama  |
|---------|---------|
|Sorgu     | Aranacak arama terimi. Yalnızca Web, Resim, Video ve Otomatik Olarak uç noktaları için kullanılabilir |
|Özel Yapılandırma Kimliği | Seçili Özel Arama örneğinin yapılandırma kimliği. Bu alan salt okunur. |
|Pazar     | Sonuçların kaynaklanacak pazar. Yalnızca Web, Resim, Video ve Barındırılan Web UI uç noktaları için kullanılabilir.        |
|Abonelik Anahtarı | Test etmek için abonelik anahtarı. Açılan listeden bir anahtar seçebilir veya el ile girebilirsiniz.          |

Ek **Parametreler'i** tıklatmaaşağıdaki parametreleri gösterir:  

|Parametre  |Açıklama  |
|---------|---------|
|Güvenli Arama     | Yetişkinlere uygun içeriğe yönelik web sayfalarını filtrelemek için kullanılan bir filtre. Yalnızca Web, Resim, Video ve Barındırılan Web UI uç noktaları için kullanılabilir. Bing Özel Video Arama'nın yalnızca `moderate` `strict`iki değeri desteklediğini unutmayın: ve.        |
|Kullanıcı Arabirimi Dili    | Kullanıcı arabirimi dizeleri için kullanılan dil. Örneğin, Barındırılan Web-Yüksek Bilgisi'nde görüntüleri ve videoları etkinleştiriseniz, **Resim** ve **Video** sekmeleri belirtilen dili kullanır.        |
|Sayı     | Yanıtta döndürülecek arama sonuçlarının sayısı. Yalnızca Web, Resim ve Video uç noktaları için kullanılabilir.         |
|Uzaklık    | Sonuçları döndürmeden önce atlayacak arama sonuçlarının sayısı. Yalnızca Web, Resim ve Video uç noktaları için kullanılabilir.        |
    
Gerekli tüm seçenekleri belirttikten sonra, JSON yanıtını sağ bölmede görüntülemek için **Ara'yı** tıklatın. Barındırılan UI bitiş noktasını seçerseniz, arama deneyimini alt bölmede sınayabilirsiniz.

## <a name="change-your-bing-custom-search-subscription"></a>Bing Özel Arama aboneliğinizi değiştirme

Bing Özel Arama örneğinizle ilişkili aboneliği yeni bir örnek oluşturmadan değiştirebilirsiniz. API çağrılarının yeni bir aboneye gönderilmesi ni ve ücretlendirildiğini görmek için Azure portalında yeni bir Bing Özel Arama kaynağı oluşturun. API isteklerinizdeki yeni abonelik anahtarını ve örneğinizin özel yapılandırma kimliğini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [C ile özel görünümünüzü arayın #](./call-endpoint-csharp.md)
- [Java ile özel görünümünüzü arayın](./call-endpoint-java.md)
- [NodeJ'lerle özel görünümünüzü arayın](./call-endpoint-nodejs.md)
- [Python ile özel görünümünüzü arayın](./call-endpoint-python.md)

- [C# SDK ile özel görünümünüzü arayın](./sdk-csharp-quick-start.md)
