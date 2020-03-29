---
title: LUIS portalında test uygulaması
description: Uygulamanızı geliştirmek ve dil anlayışını geliştirmek için sürekli olarak çalışmak için Dil Anlayışını (LUIS) kullanın.
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: ba7e23a72cd308dd4393bf9a581571e2bc9f5fa0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219824"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>LUIS uygulamanızı LUIS portalında test edin

Bir uygulamayı [test](luis-concept-test.md) etmek yinelemeli bir işlemdir. LUIS uygulamanızı eğitdikten sonra, niyetlerin ve varlıkların doğru tanıyıp tanınmadınsını görmek için örnek sözlerle test edin. Değilse, LUIS uygulamasında güncellemeler yapın, eğitin ve tekrar test edin.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

<!-- anchors for H2 name changes -->
<a name="train-your-app"></a>
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="train-before-testing"></a>Test etmeden önce tren

Etkin uygulamanın en son sürümüne karşı test etmek için, test etmeden önce üst menüden **Tren'i** seçin.

## <a name="test-an-utterance"></a>Bir söyleyiyi test edin

Test inemi, uygulamadaki herhangi bir örnek sözle tam olarak aynı olmamalıdır. Test sözcüğü, kullanıcı için beklediğiniz sözcük seçimi, ifade uzunluğu ve varlık kullanımını içermelidir.

1. **Uygulamalarım** sayfasında adını seçerek uygulamanıza erişin.

1. **Test** slayt-out paneline erişmek için, uygulamanızın üst panelinde **Test'i** seçin.

    > [!div class="mx-imgBorder"]
    > ![Tren & Test Uygulaması sayfası](./media/luis-how-to-interactive-test/test.png)

1. Metin kutusuna bir sözcük girin ve Enter'u seçin. **Testte**istediğiniz kadar test sözcük yazabilirsiniz, ancak aynı anda yalnızca bir sözcük.

1. Söyleyiş, en üst niyet ve puan metin kutusunun altındaki sözcük listesine eklenir.

    ![Etkileşimli test yanlış niyeti tanımlar](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-score"></a>Skoru inceleyin

Test sonucunun ayrıntılarını **İncele** panelinde incelersiniz.

1. **Test** slayt dışı paneli açıkken, karşılaştırmak istediğiniz bir söyleyiş için **İncele'yi** seçin.

    ![Test sonuçları hakkında daha fazla bilgi için İncele düğmesini seçin](./media/luis-how-to-interactive-test/inspect.png)

1. **Teftiş** paneli görüntülenir. Panelde en yüksek puanlama niyetinin yanı sıra tanımlanmış tüm varlıklar da yer alıyor. Panel, seçilen söyleyiş sonucunu gösterir.

    ![Panelde en yüksek puanlama niyetinin yanı sıra tanımlanmış tüm varlıklar da yer alıyor. Panel, seçilen söyleyiş sonucunu gösterir.](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>En yüksek puanlama niyetini düzeltin

1. En üstteki puanlama amacı yanlışsa, **Edit** düğmesini seçin.

1.  Açılan listede, söyleyiş için doğru niyeti seçin.

    ![Doğru niyeti seçin](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Duyarlılık sonuçlarını görüntüleyin

**Duyarlılık analizi** **[Yayımla](luis-how-to-publish-app.md#enable-sentiment-analysis)** sayfasında yapılandırılırsa, test sonuçları söyleyişde bulunan duyguları içerir.

![Duygu analizi ile Test bölmesinin görüntüsü](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Eşleşen desenin amacını düzeltin

[Desenler](luis-concept-patterns.md) kullanıyorsanız ve söyleyiş bir desenle eşleşirse, ancak yanlış niyet tahmin edildiyse, desene göre **Düzenle** bağlantısını seçin ve ardından doğru niyeti seçin.

## <a name="compare-with-published-version"></a>Yayımlanmış sürümle karşılaştırın

Uygulamanızın etkin sürümünü yayınlanan [bitiş noktası](luis-glossary.md#endpoint) sürümüyle sınayabilirsiniz. **Denetle** panelinde, **yayınlananla karşılaştır'ı**seçin. Yayımlanmış modele yönelik tüm testler Azure abonelik kota bakiyenizden düşülür.

![Yayınlananla karşılaştırın](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Test panelinde bitiş noktası JSON'ı görüntüleyin
**JSON'u Göster görünümünü**seçerek karşılaştırma için döndürülen bitiş noktasını görüntüleyebilirsiniz.

![Yayınlanan JSON yanıtı](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

## <a name="additional-settings-in-test-panel"></a>Test panelinde ek ayarlar

### <a name="luis-endpoint"></a>LUIS bitiş noktası

Birkaç LUIS uç noktanız varsa, test için kullanılan bitiş noktasını değiştirmek için Test'in Yayımlanmış bölmesindeki **Ek Ayarlar** bağlantısını kullanın. Hangi bitiş noktasının kullanılacağından emin değilseniz, varsayılan **Starter_Key.**

> [!div class="mx-imgBorder"]
> ![Ek Ayarlar bağlantısı vurgulanmış test paneli](media/luis-how-to-interactive-test/additional-settings-v3-settings.png)


## <a name="batch-testing"></a>Toplu işe testi
Toplu test [kavramlarına](luis-concept-batch-test.md) bakın ve bir grup söyleyiyi nasıl test [edeceğiz](luis-how-to-batch-test.md) öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Test, LUIS uygulamanızın doğru amaçları ve varlıkları tanımadığını gösteriyorsa, daha fazla sözcük etiketleyerek veya özellikler ekleyerek LUIS uygulamanızın doğruluğunu artırmak için çalışabilirsiniz.

* [Etiket LUIS ile önerilen söyleyünmeler](luis-how-to-review-endpoint-utterances.md)
* [LUIS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md)
