---
title: LUIS Portalı'nda test uygulama
description: Language Understanding (LUIS), uygulamanızı geliştirebilirsiniz ve kendi dil anlama geliştirmek için sürekli olarak çalışmak için kullanın.
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: ba7e23a72cd308dd4393bf9a581571e2bc9f5fa0
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78361156"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>Lusıs uygulamanızı Lua portalında test etme

Bir uygulamayı [test etmek](luis-concept-test.md) , yinelemeli bir işlemdir. LUIS uygulamanızı eğitim sonra bu amaç ve varlıkları doğru tanınır olmadığını görmek için örnek Konuşma ile test edin. Değilseniz, güncelleştirmeleri LUIS uygulaması, eğitin ve test için yeniden yapın.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

<!-- anchors for H2 name changes -->
<a name="train-your-app"></a>
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="train-before-testing"></a>Sınamadan önce eğitme

Etkin uygulamanın en son sürümüne karşı test etmek için, test etmeden önce üstteki menüden **eğitme** ' yi seçin.

## <a name="test-an-utterance"></a>Bir utterance test

Test utterine, uygulamadaki herhangi bir örnek ile tam olarak aynı olmamalıdır. Test utterine, bir kullanıcı için beklediğinizi kelime seçimi, tümcecik uzunluğu ve varlık kullanımını içermelidir.

1. **Uygulamalarım** sayfasında adını seçerek uygulamanıza erişin.

1. **Test** slayt paneli paneline erişmek için uygulamanızın üst panelinde **Test** ' i seçin.

    > [!div class="mx-imgBorder"]
    > & test uygulaması sayfası](./media/luis-how-to-interactive-test/test.png) ![eğitme

1. Bir utterance metin kutusuna girin ve Enter tuşuna basın. **Testte**istediğiniz sayıda test ututlik yazabilirsiniz, ancak aynı anda yalnızca bir tane ile bir tane yazın.

1. Utterance, üst amacını ve score konuşma metin kutusunun altında listesine eklenir.

    ![Etkileşimli sınama yanlış amacını tanımlar](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-score"></a>Puan inceleyin

**İnceleme** panelinde test sonucunun ayrıntılarını inceleyebilirsiniz.

1. **Test** slayt paneli açıkken, karşılaştırmak istediğiniz bir utterlik için **İncele** ' yi seçin.

    ![Test sonuçlarıyla ilgili daha fazla ayrıntı görmek için Inceleme düğmesini seçin](./media/luis-how-to-interactive-test/inspect.png)

1. **İnceleme** paneli görüntülenir. Panelin amacını ve bunun yanı sıra tanımlanan herhangi bir varlık Puanlama üst içerir. Paneli, seçilen utterance sonucunu gösterir.

    ![Panelin amacını ve bunun yanı sıra tanımlanan herhangi bir varlık Puanlama üst içerir. Paneli, seçilen utterance sonucunu gösterir.](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Doğru üst hedefi Puanlama

1. En üst Puanlama hedefi yanlış ise **Düzenle** düğmesini seçin.

1.  Aşağı açılan listesinde, doğru utterance hedefini seçin.

    ![Doğru hedefini seçin](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Yaklaşım sonuçlarını görüntüle

**[Yayımlama](luis-how-to-publish-app.md#enable-sentiment-analysis)** sayfasında yaklaşım **Analizi** yapılandırılırsa, test sonuçları, utde bulunan yaklaşımı içerir.

![Test Bölmesi ile ilgili yaklaşım analizi görüntüsü](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Eşleşen desenin amacı düzeltin

[Desenler](luis-concept-patterns.md) kullanıyorsanız ve söyleyeni bir desenle eşleşirse, ancak yanlış amaç tahmin edildiğinde, deseni **Düzenle** bağlantısını seçin ve ardından doğru amacı seçin.

## <a name="compare-with-published-version"></a>Yayımlanan sürümle karşılaştır

Yayınlanan [uç nokta](luis-glossary.md#endpoint) sürümüyle uygulamanızın etkin sürümünü test edebilirsiniz. **İnceleme** panelinde, **yayımlandı ile karşılaştır**' ı seçin. Yayımlanan modele yönelik test, Azure abonelik kotası bakiyeden çıkarılır.

![Yayımlanan Karşılaştır](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Uç nokta JSON test panelinde görüntüleyin
Karşılaştırma için döndürülen JSON bitiş noktasını göster ' i seçerek **JSON görünümünü göster**' i seçebilirsiniz.

![Yayımlanan bir JSON yanıtı](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

## <a name="additional-settings-in-test-panel"></a>Test panelinde ek ayarlar

### <a name="luis-endpoint"></a>LUIS uç noktası

Birkaç LUO uç noktanız varsa, test için kullanılan uç noktayı değiştirmek için testin yayımlanan bölmesindeki **ek ayarlar** bağlantısını kullanın. Hangi uç noktanın kullanılacağı konusunda emin değilseniz, varsayılan **Starter_Key**seçin.

> [!div class="mx-imgBorder"]
> Ek ayarlar bağlantısı vurgulanmış şekilde test paneli ![](media/luis-how-to-interactive-test/additional-settings-v3-settings.png)


## <a name="batch-testing"></a>Toplu işe testi
Toplu iş testi [kavramlarını](luis-concept-batch-test.md) görün ve toplu iş türlerini [nasıl](luis-how-to-batch-test.md) test leyeceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Test LUIS uygulamanızı doğru amaç ve varlıkları tanımıyor gösteriyorsa, daha fazla konuşma etiketleme ya da özellikler ekleyerek LUIS uygulamanızın doğruluğunu artırmak için çalışabilir.

* [LUSıS ile önerilen bir şekilde etiketleme](luis-how-to-review-endpoint-utterances.md)
* [LUSıS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md)
