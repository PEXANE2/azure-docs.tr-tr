---
title: Etiket varlık örneği söylenişi
description: LUE portalının amaç ayrıntısı sayfasında örnek bir örnekte bulunan alt bileşenlere sahip bir makine öğrenimi varlığını nasıl etiketleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 05/17/2020
ms.openlocfilehash: 0181057bd693280223806e9b5b7cd8c7f7345f79
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683768"
---
# <a name="label-machine-learning-entity-in-an-example-utterance"></a>Örnek bir örnekte makine öğrenimi varlığı etiketleme

Bir varlığın bir örnek içinde etiketlenmesi, LUYA varlığın ne olduğu ve varlığın utterde görünebileceği bir örnektir.

## <a name="label-example-utterances-from-the-intent-detail-page"></a>Amaç ayrıntısı sayfasından etiket örneği

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. Bir varlıkla ayıklama için etiketlemek istediğiniz örnek bir amaç seçin.
1. Etiketlemek istediğiniz metni seçin ve ardından varlığı seçin.

## <a name="label-with-the-entity-palette-visible"></a>Varlık paleti görünür olan etiket

Şemanızı varlıklar ile planladıktan sonra etiketleme sırasında **varlık paletini** görünür tutun. **Varlık paleti** , hangi varlıklarınızın ayıklanmasını istediğinizi hatırlatır.

**Varlık paletine**erişmek için, **@** örnek araç listesinin yukarıdaki bağlam araç çubuğundan sembolünü seçin.

> [!div class="mx-imgBorder"]
> ![Amaç ayrıntıları sayfasındaki varlık paleti ekran görüntüsü.](media/label-utterances/entity-palette-from-tool-bar.png)

## <a name="label-entity-from-in-place-menu"></a>Yerinde menüden varlık etiketle

Örnek utterance 'i göz önünde bulundurun `hi, please I want a cheese pizza in 20 minutes` .

1. En soldaki metni seçin, ardından varlığın en sağ metnini seçin, sonra yerinde menüsünde, etiketlemek istediğiniz varlığı seçin.

    > [!div class="mx-imgBorder"]
    > ![Etiket tam makine öğrenimi varlığı](media/label-utterances/label-steps-in-place-menu.png)


## <a name="label-entity-from-entity-palette"></a>Varlık paletindeki varlık etiketi

Varlık paleti, önceki etiketleme deneyimine bir alternatif sunar. Bir varlıkla anında etiketlemek için metnin üzerine fırçanızı sağlar.

1. **@** Söylenişi tablosunun sağ üst köşesindeki simgeyi seçerek varlık paletini açın.

2. Etiketlemek istediğiniz paletten varlığı seçin. Bu eylem, görsel olarak yeni bir imlece belirtilir. İmleç, LULAR portalında hareket ettirdiği şekilde fareyi izler.

3. Örnekte, varlığı imlece _boyayın_ .

    > [!div class="mx-imgBorder"]
    > ![Makine öğrenimi varlığı için varlık paleti](media/label-utterances/example-1-label-machine-learned-entity-palette-label-action.png)

## <a name="adding-entity-as-a-feature-from-the-entity-palette"></a>Varlık paletindeki varlık özelliği olarak ekleme

Varlık paletinin alt bölümü, şu anda seçili varlığa özellikler eklemenize olanak tanır. Tüm mevcut varlıklar ve tümcecik listelerinden seçim yapabilir veya yeni bir tümcecik listesi oluşturabilirsiniz.

> [!div class="mx-imgBorder"]
> ![Özellik olarak varlık olan varlık paleti ekran görüntüsü](media/label-utterances/entity-palette-entity-as-a-feature.png)

## <a name="labeling-entity-roles"></a>Varlık rollerini etiketleme

Varlık rolleri, **varlık paleti**kullanılarak etiketlenir.

1. Amaç ayrıntısı sayfasında bağlam araç çubuğundan **varlık paleti** ' ni seçin.
1. Varlık paleti açıldıktan sonra varlık listesinden varlığı seçin.
1. Varlık listesinin altında var olan bir rolü seçin.
1. Örnek söylenişi metninde, metni varlık rolüyle etiketleyin.

## <a name="review-labeled-text"></a>Etiketli metni gözden geçirme

Etiketledikten sonra, örnek bir şekilde gözden geçirin ve seçilen varlık için seçili varlıkla metnin altı çizili olduğundan emin olun. Düz çizgi, metnin etiketlendiği anlamına gelir.

> [!div class="mx-imgBorder"]
> ![Tam makine öğrenimi varlığı etiketlendi](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="confirm-predicted-entity"></a>Tahmin edilen varlığı Onayla

Metnin yayılması etrafında noktalı çizgili bir kutu varsa, metnin tahmin _edildiğini ancak henüz etiketlenmediğini_belirtir. Tahmine bir etiketi açmak için, söylenişi satırını seçin, sonra bağlamsal araç çubuğundan **varlıkları Onayla** ' yı seçin.

## <a name="relabeling-over-existing-entities"></a>Mevcut varlıkları ele alınıyor

Zaten etiketlenmiş bir metin varsa, LUYA var olan etiketleri bölebilir veya birleştirebilir.

## <a name="labeling-for-punctuation"></a>Noktalama işaretleri için etiketleme

Noktalama için etiketlemenize gerek yoktur. Noktalama işaretlerinin söylenişi tahminleri etkileyip etkilemesinin ne olduğunu denetlemek için [uygulama ayarları](luis-reference-application-settings.md) _sözcük formları_ ' nı kullanın.

## <a name="unlabel-entities"></a>Varlıkların etiketini kaldır

> [!NOTE]
> Yalnızca makine tarafından öğrenilen varlıkların etiketi etiketsiz olabilir.

Bir varlığın etiketini kaldırmak için varlığı seçin ve yerinde menüden **etiketi kaldır** ' ı seçin.

> [!div class="mx-imgBorder"]
> ![Etiketlemeyi kaldırma varlığını gösteren ekran görüntüsü](media/label-utterances/unlabel-entity-using-in-place-menu.png)

## <a name="automatic-labeling-for-parent-and-child-entities"></a>Üst ve alt varlıklar için otomatik etiketleme

Bir üst varlığı etiketlendirmekte olduğunuz sırada, eğitilen sürüme göre tahmin edilebilir olan herhangi bir alt varlık etiketlenecek.

Bir alt varlık için etiketleme yapıyorsanız, üst öğe otomatik olarak etiketlenir.

## <a name="automatic-labeling-for-non-machine-learned-entities"></a>Makine tarafından öğrenilen varlıkların otomatik etiketlenmesi

Makine tarafından öğrenilen varlıklar, önceden oluşturulmuş varlıklar, normal ifade varlıkları, liste varlıkları ve model. herhangi bir varlık içerir. Bunlar, LULAR tarafından otomatik olarak etiketlendirildiklerinden, kullanıcılar tarafından el ile etiketlenmesi gerekmez.

## <a name="entity-prediction-errors"></a>Varlık tahmin hataları

Varlık tahmin hataları, tahmin edilen varlığın etiketlenmiş varlıkla eşleşmediği anlamına gelebilir. Bu, utterance 'in yanında dikkatli bir göstergeyle görselleştirilir.

> [!div class="mx-imgBorder"]
> ![Makine öğrenimi varlığı için varlık paleti](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı tahmin kalitesini artırmak için [panoyu](luis-how-to-use-dashboard.md) kullanın ve [uç nokta utlerini gözden geçirin](luis-how-to-review-endpoint-utterances.md) .