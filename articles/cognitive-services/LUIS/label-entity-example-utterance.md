---
title: Etiket varlık örneği söylenişi
description: Makine tarafından öğrenilen bir varlığı, LUS portalının amaç ayrıntısı sayfasında örnek bir örnekte bulunan alt bileşenler ile nasıl etiketleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: ea5fbea902c9694d9a8a6a8a5bffcf5e7234bbbd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81382412"
---
# <a name="label-machine-learned-entity-in-an-example-utterance"></a>Makine tarafından öğrenilen varlığı örnek bir mekan etiketleme

Bir varlığın bir örnek içinde etiketlenmesi, LUYA varlığın ne olduğu ve varlığın utterde görünebileceği bir örnektir.

## <a name="labeling-machine-learned-entity"></a>Makine tarafından öğrenilen Varlık etiketleme

İfadesini göz önünde bulundurun `hi, please I want a cheese pizza in 20 minutes`.

1. En soldaki metni seçin, ardından varlığın en sağ metnini seçip, ardından etiketlemek istediğiniz varlığı, bu durumda tam sırayı seçin. _Tüm sıra_ aşağıdaki görüntüde etiketlidir.

    > [!div class="mx-imgBorder"]
    > ![Etiket tam makine öğrenmiş varlık](media/label-utterances/example-1-label-machine-learned-entity-complete-order.png)

1. Açılır penceredeki varlığı seçin. Etiketli tamamlanmış pizza sırası varlığı, etiketli tüm sözcükleri (Ingilizce 'de soldan sağa) içerir.

## <a name="review-labeled-text"></a>Etiketli metni gözden geçirme

Etiketledikten sonra, örnek bir şekilde gözden geçirin ve seçilen varlık için seçili varlıkla metnin altı çizili olduğundan emin olun. Düz çizgi, metnin etiketlendiği anlamına gelir.

> [!div class="mx-imgBorder"]
> ![Makine tarafından öğrenilen tam varlık etiketi](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="confirm-predicted-entity"></a>Tahmin edilen varlığı Onayla

Metnin yayılımı etrafında noktalı çizgili bir kutu varsa ve varlık adı, utterance 'in üstündeyse, metnin tahmin _edildiğini ancak henüz etiketlenmediğini_belirtir. Tahmine bir etiketi açmak için, söylenişi satırını seçin ve ardından **varlık tahminlerini Onayla**' yı seçin.

> [!div class="mx-imgBorder"]
> ![Makine tarafından öğrenilen tüm varlığı tahmin etme](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted.png)

Alternatif olarak, metnin üzerindeki varlık adını seçip görüntülenen menüden **tahmin Onayla** ' yı seçebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Tüm makine tarafından öğrenilen varlığı menü ile tahmin edin](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted-menu.png)

## <a name="label-entity-by-painting-with-entity-palette-cursor"></a>Varlık paleti imletiyle boyayarak varlığı etiketle

Varlık paleti, önceki etiketleme deneyimine bir alternatif sunar. Bir varlıkla anında etiketlemek için metnin üzerine fırçanızı sağlar.

1. Söylenişi tablosunun sağ üst köşesindeki vurgulayıcı simgesini seçerek varlık paletini açın.

    > [!div class="mx-imgBorder"]
    > ![Makine tarafından öğrenilen varlık için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-palette.png)

2. Varlık bileşenini seçin. Bu eylem, görsel olarak yeni bir imlece belirtilir. İmleç, portalda hareket ettirdiği şekilde fareyi izler.

    > [!div class="mx-imgBorder"]
    > ![Makine tarafından öğrenilen varlık için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-palette-menu.png)

3. Örnekte, varlığı imlece _boyayın_ .

    > [!div class="mx-imgBorder"]
    > ![Makine tarafından öğrenilen varlık için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-palette-label-action.png)

## <a name="labeling-subcomponents-of-a-machine-learned-entity"></a>Makine tarafından öğrenilen bir varlığın alt bileşenlerini etiketleme

Varlıkların alt bileşenleri, en üst düzey varlıklarla tamamen aynı şekilde etiketlidir. Metin seçerken, açılır pencerede bulunan varlıklar, metnin göründüğü içeriğe göre değişir. Örneğin, 5 düzeyli bir makine tarafından öğrenilen bir varlığınız varsa ve 1. ve 2. düzey (örneğin, bir etiketli varlık adı ile belirtilir) ile etiketlenmiş bir metin seçiyorsanız, açılır penceredeki varlıkların 3. düzey bileşenlerinin içeriğiyle sınırlı olması gerekir. Metni diğer varlıklarla etiketlemek için, **başka bir varlık seçeneği olarak etiketle** seçeneğini belirleyin.

> [!div class="mx-imgBorder"]
> ![Makine tarafından öğrenilen varlık için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-subcomponent.png)

Alt bileşenler yalnızca üst öğe de etiketlendirilmişse etiketlenebilir.

## <a name="labeling-entity-roles"></a>Varlık rollerini etiketleme

Varlık rolleri, varlık paleti kullanılarak etiketlenir.

1. Amaç ayrıntısı sayfasında bağlam araç çubuğundan **varlık paleti** ' ni seçin.
1. Varlık paleti açıldıktan sonra varlık listesinden varlığı seçin.
1. **Varlık denetçisi**' ne gidin ve mevcut bir rol seçin ya da yeni bir rol oluşturun.
1. Örnek söylenişi metninde, metni varlık rolüyle etiketleyin.

## <a name="labeling-for-punctuation"></a>Noktalama işaretleri için etiketleme

Noktalama için etiketlemenize gerek yoktur. Noktalama işaretlerinin söylenişi öngörülerini etkileyip etkilememe etkilerini denetlemek için [uygulama ayarlarını](luis-reference-application-settings.md) kullanın.

## <a name="unlabel-entities"></a>Varlıkların etiketini kaldır

Bir varlığın etiketini kaldırmak için, metnin altındaki varlık adını seçin ve **etiketi kaldır**' ı seçin. Etiketi kaldırmak istediğiniz varlığın alt bileşenleri etiketlenmişse, alt bileşenlerin önce etiketlenmesi gerekir.

## <a name="editing-labels-using-the-entity-palette"></a>Varlık paletini kullanarak etiketleri düzenle

Etiketleme sırasında bir hata yaparsanız, varlık paleti hızlı düzenlemelere izin veren kolay bir araçtır. Örneğin, bir varlık etiketi yanlışlıkla ek bir sözcüğe yayılmışsa ve zaten alt bileşenleri etiketlenmişse, bu durumda sözcüklerin gerekli daha kısa yayılmasını sağlamak için varlık paletini kullanabilirsiniz.

Örneğin:

1. Pizza tür alt bileşeni "Cheese pizza" ile birlikte "birlikte" ek yanlış bir sözcük içerir

    > [!div class="mx-imgBorder"]
    > ![Makine tarafından öğrenilen varlık için varlık paleti](media/label-utterances/edit-label-with-palette-1.png)

2. Pizza türünü ve "Cheese pizza" üzerine fırçayı seçmek için varlık paletini kullanın. Sonuç yalnızca Cheese pizza, şimdi pizza türü olarak etiketlidir.

    > [!div class="mx-imgBorder"]
    > ![Makine tarafından öğrenilen varlık için varlık paleti](media/label-utterances/edit-label-with-palette-2.png)

## <a name="labels-for-matching-text-entities"></a>Eşleşen metin varlıklarının etiketleri

Eşleşen metin varlıkları, önceden oluşturulmuş varlıklar, normal ifade varlıkları, liste varlıkları ve model. herhangi bir varlık içerir. Bunlar, LULAR tarafından otomatik olarak etiketlendirildiklerinden, kullanıcılar tarafından el ile etiketlenmesi gerekmez.

## <a name="entity-prediction-errors"></a>Varlık tahmin hataları

Varlık tahmin hataları, tahmin edilen varlığın etiketlenmiş varlıkla eşleşmediği anlamına gelebilir. Bu, utterance 'in yanında dikkatli bir göstergeyle görselleştirilir.

> [!div class="mx-imgBorder"]
> ![Makine tarafından öğrenilen varlık için varlık paleti](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı tahmin kalitesini artırmak için [panoyu](luis-how-to-use-dashboard.md) kullanın ve [uç nokta utlerini gözden geçirin](luis-how-to-review-endpoint-utterances.md) .