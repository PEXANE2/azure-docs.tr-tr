---
title: Özel bir görünüm ara-Bing Özel Arama
titleSuffix: Azure Cognitive Services
description: Özel arama deneyiminizi yapılandırdıktan sonra, Bing Özel Arama portalı içinden test edebilirsiniz.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: aahi
ms.openlocfilehash: f00ffee47e3eb6366d632d8b6ee9beb01f048442
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76983121"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Portaldan Bing Özel Arama örneğinizi çağırın

Özel arama deneyiminizi yapılandırdıktan sonra, Bing Özel Arama [portalı](https://customsearch.ai)içinden test edebilirsiniz. 

![Bing özel arama portalının ekran görüntüsü](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Arama sorgusu oluşturma 

Bing Özel Arama [portalında](https://customsearch.ai)oturum açtıktan sonra, arama örneğinizi seçin ve **Üretim** sekmesine tıklayın. **Uç noktalar**altında bir API uç noktası (örneğin, Web API) seçin. Aboneliğiniz hangi uç noktaların gösterileceğini belirler.

Bir arama sorgusu oluşturmak için, uç noktanızın parametre değerlerini girin. Portalda görüntülenen parametrelerin seçtiğiniz uç noktaya göre değişebileceğini unutmayın. Daha fazla bilgi için [özel arama API'si başvurusuna](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters) bakın. Arama örneğinizin kullandığı aboneliği değiştirmek için uygun abonelik anahtarını ekleyin ve uygun pazar ve/veya dil parametrelerini güncelleştirin.

Bazı önemli parametreler aşağıdadır:


|Parametre  |Açıklama  |
|---------|---------|
|Sorgu     | Aranacak arama terimi. Yalnızca Web, resim, video ve otomatik öneri uç noktaları için kullanılabilir |
|Özel yapılandırma KIMLIĞI | Seçilen özel arama örneğinin yapılandırma KIMLIĞI. Bu alan salt okunur. |
|Pazara     | Sonuçların kaynağı olarak verilecek Pazar. Yalnızca Web, görüntü, video ve barındırılan Kullanıcı arabirimi uç noktaları için kullanılabilir.        |
|Abonelik Anahtarı | Sınanacak abonelik anahtarı. Açılan listeden bir anahtar seçebilir veya bir el ile girebilirsiniz.          |

**Ek parametrelere** tıkladığınızda aşağıdaki parametreler görünür:  

|Parametre  |Açıklama  |
|---------|---------|
|Güvenli Arama     | Yetişkinlere yönelik içerik için Web sayfalarını filtrelemek üzere kullanılan filtre. Yalnızca Web, görüntü, video ve barındırılan Kullanıcı arabirimi uç noktaları için kullanılabilir. Bing özel Video Arama yalnızca iki değeri desteklediğini unutmayın: `moderate` ve. `strict`        |
|Kullanıcı Arabirimi Dili    | Kullanıcı arabirimi dizeleri için kullanılan dil. Örneğin, barındırılan Kullanıcı arabiriminde görüntüleri ve videoları etkinleştirirseniz, **görüntü** ve **video** sekmeleri belirtilen dili kullanır.        |
|Sayı     | Yanıtta döndürülecek arama sonuçlarının sayısı. Yalnızca Web, görüntü ve video uç noktaları için kullanılabilir.         |
|Uzaklık    | Sonuçları döndürmeden önce atlanacak arama sonuçlarının sayısı. Yalnızca Web, görüntü ve video uç noktaları için kullanılabilir.        |
    
Tüm gerekli seçenekleri belirledikten sonra, sağ bölmedeki JSON yanıtını görüntülemek için **çağır** ' a tıklayın. Barındırılan UI uç noktasını seçerseniz, arama deneyimini alt bölmede test edebilirsiniz.

## <a name="change-your-bing-custom-search-subscription"></a>Bing Özel Arama aboneliğinizi değiştirme

Bing Özel Arama örneğiniz ile ilişkili aboneliği yeni bir örnek oluşturmadan değiştirebilirsiniz. API çağrılarının gönderilmesini ve yeni bir aboneliğe ücretlendirildiği için, Azure portal yeni bir Bing Özel Arama kaynağı oluşturun. API isteklerinizin yeni abonelik anahtarını, örneğinizin özel yapılandırma KIMLIĞIYLE birlikte kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel görünümünüzü C ile çağırma #](./call-endpoint-csharp.md)
- [Java ile özel görünümünüzü çağırma](./call-endpoint-java.md)
- [NodeJs ile özel görünümünüzü çağırma](./call-endpoint-nodejs.md)
- [Python ile özel görünümünüzü çağırma](./call-endpoint-python.md)

- [C# SDK ile özel görünümünüzü çağırma](./sdk-csharp-quick-start.md)
