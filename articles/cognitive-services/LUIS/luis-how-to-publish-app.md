---
title: Uygulama yayınlama - LUIS
titleSuffix: Azure Cognitive Services
description: Etkin LUIS uygulamanızı oluşturmayı ve test etmeyi bitirdiğinizde, son noktaya kadar yayınlayarak istemci uygulamanızın kullanıma sunulmasını sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: c91a3ca73d70dd5fd2848bed0f43f14a817087d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80053427"
---
# <a name="publish-your-active-trained-app-to-a-staging-or-production-endpoint"></a>Etkin ve eğitimli uygulamanızı bir evreleme veya üretim bitiş noktasına kadar yayımlayın

Etkin LUIS uygulamanızı oluşturmayı, eğitmenizi ve test etmeyi bitirdiğinizde, uygulamayı bitiş noktasına kadar yayınlayarak istemci uygulamanızın kullanımına açın. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="publishing"></a>Yayımlama

1. Bitiş noktasına yayımlamak için, üst, sağ panelde **Yayımla'yı** seçin. 

    ![Üstte, sağ gezinme çubuğunda yayımlama düğmesi](./media/luis-how-to-publish-app/publish-top-nav-bar.png)

1. Yayınlanan tahmin bitiş noktası için ayarlarınızı seçin ve ardından **Yayımla'yı**seçin.

    ![Yayımlama ayarlarını seçin ve ardından Yayımla düğmesini seçin](./media/luis-how-to-publish-app/publish-pop-up.png)

### <a name="publishing-slots"></a>Yayın yuvaları

Açılır pencere görüntülendiğinde doğru yuvayı seçin: 

* Hazırlama
* Üretim 

Bu, her iki yayımlama yuvasını da kullanarak, uygulamanızın iki farklı sürümünü yayınlanan uç noktalarda veya aynı sürümü iki farklı uç noktada kullanılabilir hale getirmek için izin verir. 

### <a name="publishing-regions"></a>Yayımlama bölgeleri

Uygulama,**[Azure Kaynaklarını](luis-how-to-azure-subscription.md#assign-a-resource-to-an-app)** **Yönet** -> sayfasından LUIS portalına eklenen LUIS tahmini uç nokta kaynaklarıyla ilişkili tüm bölgelerde yayınlanır. 

Örneğin, [www.luis.ai'da](https://www.luis.ai)oluşturulan bir uygulama için, **westus** ve **eastus**olmak üzere iki bölgede bir LUIS kaynağı oluşturursanız ve bunları kaynak olarak uygulamaya eklerseniz, uygulama her iki bölgede de yayınlanır. LUIS bölgeleri hakkında daha fazla bilgi için [bölgelere](luis-reference-regions.md)bakın.

> [!TIP]
> 3 yazma bölgesi vardır. Yayınlamak istediğiniz bölgede yazar olmalısınız. Tüm bölgelere yayımlamanız gerekiyorsa, yazma işleminizi ve elde edilen eğitilmiş modeli 3 yazma bölgesinde yönetmeniz gerekir. 


## <a name="configuring-publish-settings"></a>Yayımlama ayarlarını yapılandırma

Yuvayı seçtikten sonra yayımlama ayarlarını şu şekilde yapılandırın:

* Yaklaşım analizi
* Yazım düzeltmesi - yalnızca v2 tahmini bitiş noktası
* Konuşma astarı 

Yayımladıktan sonra, bu ayarlar **Yönet** bölümünün **Ayarlar** sayfasını inceleyebilirsiniz. Her yayımlamada ayarları değiştirebilirsiniz. Bir yayımlamayı iptal ederseniz, yayımlama sırasında yaptığınız değişiklikler de iptal edilir. 

### <a name="when-your-app-is-published"></a>Uygulamanız yayınlandığında

Uygulamanız başarıyla yayımlandığında, tarayıcının üst kısmında bir başarı bildirimi görüntülenir. Bildirim, uç noktalara bir bağlantı da içerir. 

Bitiş noktası URL'ye ihtiyacınız varsa bağlantıyı seçin. Ayrıca üst menüde **Yönet'i** seçerek bitiş noktası URL'lerine, ardından sol menüde **Azure Kaynakları'nı** seçebilirsiniz. 

## <a name="sentiment-analysis"></a>Yaklaşım analizi

<a name="enable-sentiment-analysis"></a>

Duyarlılık analizi, LUIS'in [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) ile entegre edinip duyarlılık ve anahtar sözcük analizi sağlamasına olanak tanır. 

Metin Analizi anahtarı sağlamanız gerekmez ve bu hizmet için Azure hesabınıza fatura landırma ücreti alınmaz. 

Duyarlılık verileri, verilerin pozitif (1'e yakın) veya negatif (0'a yakın) duyarlılığını gösteren 1 ile 0 arasında bir puandır. Duyarlılık etiketi `positive`, `neutral`, `negative` ve desteklenen kültür başına. Şu anda, sadece İngilizce duygu etiketleri destekler. 

Duyarlılık analizi ile JSON uç nokta yanıtı hakkında daha fazla bilgi için, [Sentiment analizine](luis-concept-data-extraction.md#sentiment-analysis) bakın

## <a name="spelling-correction"></a>Yazım düzeltmesi

[!INCLUDE [Not supported in V3 API prediction endpoint](./includes/v2-support-only.md)]

Yazım düzeltmeleri LUIS kullanıcı söyleyiş tahmini önce yapılır. Yanıtta yazım da dahil olmak üzere orijinal söyleyişdeki değişiklikleri görebilirsiniz.

## <a name="speech-priming"></a>Konuşma astarı

Konuşma astarı, metnin konuşmaya dönüştürülmesinden önce LUIS modelini Konuşma hizmetlerine gönderme işlemidir. Bu, konuşma hizmetinin modeliniz için konuşma dönüştürmeyi daha doğru bir şekilde sağlamasına olanak tanır. Bu bot Konuşma ve LUIS istekleri ve yanıtları bir konuşma arama sı ve luis yanıt geri alarak bir arama sağlar. Genel olarak daha az gecikme alanı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* LUIS'e Azure abonelik anahtarına anahtar eklemek ve Bing Büyüsü Denetimi anahtarını nasıl ayarlayıp tüm amaçları sonuçlara dahil etmek için [Yönet tuşlarına](./luis-how-to-azure-subscription.md) bakın.
* Yayınlanan [uygulamanızı](luis-interactive-test.md) test konsolunda nasıl test edebilirsiniz yönergeleri için Train ve test edin.

