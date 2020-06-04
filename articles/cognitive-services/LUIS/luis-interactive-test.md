---
title: LUSıS portalında test uygulaması
description: Uygulamanızı iyileştirmek ve dilini anlamak için uygulamanızda sürekli çalışmak üzere Language Understanding (LUO) kullanın.
ms.topic: how-to
ms.date: 05/20/2020
ms.openlocfilehash: 86ee90e2d3bb322a4f55439d105941cf43462d3e
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344161"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>Lusıs uygulamanızı Lua portalında test etme

Bir uygulamayı [test etmek](luis-concept-test.md) , yinelemeli bir işlemdir. LUSıS uygulamanıza eğitim ettikten sonra, amaçları ve varlıkların doğru şekilde tanınıp tanınmadığını görmek için örnek bir şekilde test edin. Aksi takdirde, LUSıS uygulaması, eğit ve test güncelleştirmelerini yeniden yapın.

<!-- anchors for H2 name changes -->
<a name="train-your-app"></a>
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="train-before-testing"></a>Sınamadan önce eğitme

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. Etkin uygulamanın en son sürümüne karşı test etmek için, test etmeden önce üstteki menüden **eğitme** ' yi seçin.

## <a name="test-an-utterance"></a>Bir söylenişi test etme

Test utterine, uygulamadaki herhangi bir örnek ile tam olarak aynı olmamalıdır. Test utterine, bir kullanıcı için beklediğinizi kelime seçimi, tümcecik uzunluğu ve varlık kullanımını içermelidir.

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.

1. **Test** slayt paneli paneline erişmek için uygulamanızın üst panelinde **Test** ' i seçin.

    > [!div class="mx-imgBorder"]
    > ![Eğitim & test uygulaması sayfası](./media/luis-how-to-interactive-test/test.png)

1. Metin kutusuna bir söylenişi girin ve ENTER ' u seçin. **Testte**istediğiniz sayıda test ututlik yazabilirsiniz, ancak aynı anda yalnızca bir tane ile bir tane yazın.

1. Utterance, en üst amacı ve puanı, metin kutusunun altındaki söylik listesine eklenir.

    > [!div class="mx-imgBorder"]
    > ![Etkileşimli test, yanlış amacı tanımlıyor](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-the-prediction"></a>Tahmini inceleyin

**İnceleme** panelinde test sonucunun ayrıntılarını inceleyebilirsiniz.

1. **Test** slayt paneli açıkken, karşılaştırmak istediğiniz bir utterlik için **İncele** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Test sonuçlarıyla ilgili daha fazla ayrıntı görmek için Inceleme düğmesini seçin](./media/luis-how-to-interactive-test/inspect.png)

1. **İnceleme** paneli görüntülenir. Panel, en iyi Puanlama amacını ve tanımlanan tüm varlıkları içerir. Panel, seçili utterlik 'in tahminini gösterir.

    > [!div class="mx-imgBorder"]
    > ![Test Incelemesi bölmesinin kısmi ekran görüntüsü](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="add-to-example-utterances"></a>Örnek utleye Ekle

İnceleme panelinden, **örnek Ekle**' yi seçerek test utterliğini bir amaca ekleyebilirsiniz.

## <a name="disable-required-features"></a>Gerekli özellikleri devre dışı bırak

Varlığın özelliği gerekmiyorsa tahmine ne olduğunu görmek için bu geçişi seçin.

Bu geçiş, eğitilen uygulamanın, varlıklarınızı gerekli özelliklere göre doğru şekilde tahmin etmesine olanak sağlar. Eğitilen uygulama, örnek gelişenlerin hatalı etiketlenmesini temel alarak, makine tarafından öğrenilen bir varlığı yanlış tahmin edebilir ya da gerekli özellik metinle eşleşmez.

## <a name="view-sentiment-results"></a>Yaklaşım sonuçlarını görüntüle

**[Yayımlama](luis-how-to-publish-app.md#enable-sentiment-analysis)** sayfasında yaklaşım **Analizi** yapılandırılırsa, test sonuçları, utde bulunan yaklaşımı içerir.

## <a name="correct-matched-patterns-intent"></a>Eşleşen düzenin hedefini düzeltin

[Desenler](luis-concept-patterns.md) kullanıyorsanız ve söyleyeni bir desenle eşleşirse, ancak yanlış amaç tahmin edildiğinde, deseni **Düzenle** bağlantısını seçin ve ardından doğru amacı seçin.

## <a name="compare-with-published-version"></a>Yayımlanan sürümle karşılaştır

Yayınlanan [uç nokta](luis-glossary.md#endpoint) sürümüyle uygulamanızın etkin sürümünü test edebilirsiniz. **İnceleme** panelinde, **yayımlandı ile karşılaştır**' ı seçin. Yayımlanan modele karşı herhangi bir test, Azure abonelik kotası bakiyesinden düşülür.

> [!div class="mx-imgBorder"]
> ![Yayımlanla Karşılaştır](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Test panelinde Endpoint JSON görüntüleme
Karşılaştırma için döndürülen JSON bitiş noktasını göster ' i seçerek **JSON görünümünü göster**' i seçebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Yayımlanan JSON yanıtı](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

## <a name="additional-settings-in-test-panel"></a>Test panelinde ek ayarlar

### <a name="luis-endpoint"></a>LUSıS uç noktası

Birkaç LUO uç noktanız varsa, test için kullanılan uç noktayı değiştirmek için testin yayımlanan bölmesindeki **ek ayarlar** bağlantısını kullanın. Hangi uç noktanın kullanılacağı konusunda emin değilseniz, varsayılan **Starter_Key**seçin.

> [!div class="mx-imgBorder"]
> ![Ek ayarlar bağlantısı vurgulanmış şekilde test paneli](media/luis-how-to-interactive-test/additional-settings-v3-settings.png)


## <a name="batch-testing"></a>Toplu işe testi
Toplu iş testi [kavramlarını](luis-concept-batch-test.md) görün ve toplu iş türlerini [nasıl](luis-how-to-batch-test.md) test leyeceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Test, LUP uygulamanızın doğru amaçları ve varlıkları algıladığını gösteriyorsa, daha fazla yer etiketleyerek veya özellik ekleyerek LUSıS uygulamanızın doğruluğunu iyileştirebilmek için çalışabilirsiniz.

* [LUSıS ile önerilen bir şekilde etiketleme](luis-how-to-review-endpoint-utterances.md)
* [LUSıS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md)
