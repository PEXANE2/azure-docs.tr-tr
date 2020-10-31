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
ms.openlocfilehash: 38191c55afb87151b238069a75a6c86acd8904df
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93100550"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Portaldan Bing Özel Arama örneğinizi çağırın

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

Özel arama deneyiminizi yapılandırdıktan sonra, Bing Özel Arama [portalı](https://customsearch.ai)içinden test edebilirsiniz. 

![Bing özel arama portalının ekran görüntüsü](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Arama sorgusu oluşturma 

Bing Özel Arama [portalında](https://customsearch.ai)oturum açtıktan sonra, arama örneğinizi seçin ve **Üretim** sekmesine tıklayın. **Uç noktalar** altında bir API uç noktası (örneğin, Web API) seçin. Aboneliğiniz hangi uç noktaların gösterileceğini belirler.

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
|Güvenli Arama     | Yetişkinlere yönelik içerik için Web sayfalarını filtrelemek üzere kullanılan filtre. Yalnızca Web, görüntü, video ve barındırılan Kullanıcı arabirimi uç noktaları için kullanılabilir. Bing özel Video Arama yalnızca iki değeri desteklediğini unutmayın: `moderate` ve `strict` .        |
|Kullanıcı Arabirimi Dili    | Kullanıcı arabirimi dizeleri için kullanılan dil. Örneğin, barındırılan Kullanıcı arabiriminde görüntüleri ve videoları etkinleştirirseniz, **görüntü** ve **video** sekmeleri belirtilen dili kullanır.        |
|Count     | Yanıtta döndürülecek arama sonuçlarının sayısı. Yalnızca Web, görüntü ve video uç noktaları için kullanılabilir.         |
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
