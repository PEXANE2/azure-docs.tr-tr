---
title: Etiket varlık örneği söyleyiş
titleSuffix: Azure Cognitive Services
description: LUIS portalının kasıtlı ayrıntı sayfasında, makinede öğrenilen bir varlığı örnek bir sözcükte alt bileşenlerle nasıl etiketleyeceğiz öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: diberry
ms.openlocfilehash: 5499e8362e1107a18b997a6a485e11343c328d0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898367"
---
# <a name="label-machine-learned-entity-in-an-example-utterance"></a>Örnek bir sözle etiket makinesi öğrenilen varlık

Bir varlığı örnek bir sözle etiketlemek, LUIS'e varlığın ne olduğuna ve varlığın söyleyişde nerede görünebileceğine bir örnek verir.

## <a name="labeling-machine-learned-entity"></a>Makineden öğrenilen varlığı etiketleme

İfadeyi göz `hi, please I want a cheese pizza in 20 minutes`önünde bulundurun.

1. Soldaki en metni seçin, ardından varlığın en sağdaki metnini seçin ve bu durumda Tam Sıra'yı etiketlemek istediğiniz varlığı seçin. _Tam sipariş_ aşağıdaki resimde etiketlenir.

    > [!div class="mx-imgBorder"]
    > ![Tam makine öğrenilen varlığı etiketle](media/label-utterances/example-1-label-machine-learned-entity-complete-order.png)

1. Açılan pencereden varlığı seçin. Etiketli tam pizza sipariş idamı, etiketli tüm sözcükleri (İngilizce'de soldan sağa) içerir.

## <a name="review-labeled-text"></a>Etiketli metni gözden geçirme

Etiketlemeden sonra, örnek söyleyiyi gözden geçirin ve seçilen metin aralığının seçilen varlıkla birlikte altı çizildiğinden emin olun. Düz çizgi, metnin etiketlendiğini gösterir.

> [!div class="mx-imgBorder"]
> ![Etiketli tam makine öğrenilen varlık](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="confirm-predicted-entity"></a>Tahmin edilen varlığı onaylama

Metnin açıklığı etrafında noktalı çizgili bir kutu varsa ve varlık adı söyleyinin üzerindeyse, metnin tahmin edildiğini ancak _henüz etiketlenmediğini_gösterir. Öngörüyü bir etikete dönüştürmek için, sözcük satırını seçin ve ardından **varlık tahminlerini doğrula'yı**seçin.

> [!div class="mx-imgBorder"]
> ![Makinede öğrenilen varlığın tamamını tahmin edin](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted.png)

Alternatif olarak, metnin üzerindeki varlık adını seçebilir ve ardından görünen menüden **Tahmin'i onayla'yı** seçebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Menü ile makineden öğrenilen varlığın tamamını tahmin edin](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted-menu.png)

## <a name="label-entity-by-painting-with-entity-palette-cursor"></a>Entity palet imleci ile boyama yaparak varlık etiketi

Varlık paleti önceki etiketleme deneyimine bir alternatif sunar. Bir varlıkla anında etiketlemek için metni fırçalamanızı sağlar.

1. Söyleyiş tablosunun sağ üst kısmındaki Vurgulayıcı simgesini seçerek varlık paletini açın.

    > [!div class="mx-imgBorder"]
    > ![Makine de öğrenilen varlık için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-palette.png)

2. Varlık bileşenini seçin. Bu eylem görsel olarak yeni bir imleç ile gösterilir. Portalda hareket ederken imleç fareyi izler.

    > [!div class="mx-imgBorder"]
    > ![Makine de öğrenilen varlık için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-palette-menu.png)

3. Örnek teslemide, varlığı imleçle _boyayın._

    > [!div class="mx-imgBorder"]
    > ![Makine de öğrenilen varlık için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-palette-label-action.png)

## <a name="labeling-subcomponents-of-a-machine-learned-entity"></a>Makine öğrenilen varlığın alt bileşenlerini etiketleme

Varlıklardaki alt bileşenler, üst düzey varlıklarla aynı şekilde etiketlenir. Metin seçilirken, açılır pencerede bulunan varlıklar metnin göründüğü bağlamla görelidir. Örneğin, 5 düzeyli bir makine öğrenilen varsanız ve 1. 3. düzey bileşenlerin bağlamı. Metni diğer varlıklarla etiketlemek için **etiketseçeneğini başka bir varlık** seçeneği olarak etiketleyin'i seçin.

> [!div class="mx-imgBorder"]
> ![Makine de öğrenilen varlık için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-subcomponent.png)

Alt bileşenler yalnızca üst öğe de etiketlenmişse etiketlenebilir.

## <a name="labeling-entity-roles"></a>Varlık rollerini etiketleme

Varlık rolleri varlık paleti kullanılarak etiketlenir.

1. Niyet ayrıntısı sayfasında, bağlam araç çubuğundan **Varlık paletini** seçin.
1. Varlık paleti açıldıktan sonra, varlık listesinden varlığı seçin.
1. **Varlık denetçisine**geçin ve varolan bir rolü seçin veya yeni bir rol oluşturun.
1. Örnek sözcük metninde, metni varlık rolüyle etiketle.

## <a name="unlabel-entities"></a>Etiketlenmemiş varlıklar

Varlığı netiketini çıkarmak için metnin altındaki varlık adını seçin ve **Etiketi Künye'yi**seçin. Etiketini kaldırmaya çalıştığınız varlık alt bileşenleri etiketlediyse, önce alt bileşenlerin etiketlenmemiş olması gerekir.

## <a name="editing-labels-using-the-entity-palette"></a>Varlık paletini kullanarak etiketleri düzenleme

Etiketleme sırasında bir hata yaparsanız, varlık paleti hızlı düzenleme sağlayan kolay bir araçtır. Örneğin, bir varlık etiketi yanlışlıkla fazladan bir sözcüğü kapsıyorsa ve alt bileşenleri zaten etiketlemişse, gerekli daha kısa sözcük açıklarını fırçalamak için varlık paletini kullanabilirsiniz.

Örnek:

1. Pizza Türü alt bileşeni ekstra bir yanlış kelime içeren "peynirli pizza" yayılan - "ile"

    > [!div class="mx-imgBorder"]
    > ![Makine de öğrenilen varlık için varlık paleti](media/label-utterances/edit-label-with-palette-1.png)

2. Pizza Türü almak ve "peynir pizza" üzerinde fırça varlık paleti kullanın. Sonuç olarak sadece peynirpizza pizza pizza türü olarak şimdi etiketlenmiş olmasıdır.

    > [!div class="mx-imgBorder"]
    > ![Makine de öğrenilen varlık için varlık paleti](media/label-utterances/edit-label-with-palette-2.png)

## <a name="labels-for-matching-text-entities"></a>Eşleşen metin varlıkları için etiketler

Eşleşen metin varlıkları önceden oluşturulmuş varlıklar, normal ifade varlıkları, liste varlıkları ve desen.any varlıkları içerir. Bunlar LUIS tarafından otomatik olarak etiketlenir, böylece kullanıcılar tarafından el ile etiketlenmeleri gerekmez.

## <a name="entity-prediction-errors"></a>Varlık tahmin hataları

Varlık tahmin hataları, tahmin edilen varlığın etiketli varlıkla eşleşmediğini gösterir. Bu, söyleyiş yanında bir uyarı göstergesi ile görselleştirilmiştir.

> [!div class="mx-imgBorder"]
> ![Makine de öğrenilen varlık için varlık paleti](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızın tahmin kalitesini artırmak için [pano](luis-how-to-use-dashboard.md) ve [gözden bitiş noktası söyleyişlerini](luis-how-to-review-endpoint-utterances.md) kullanın.