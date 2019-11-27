---
title: LUIS Portalı'nda test uygulama
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS), uygulamanızı geliştirebilirsiniz ve kendi dil anlama geliştirmek için sürekli olarak çalışmak için kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: 628547e8254bb0055cf1f09af50e79b68311a759
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74221773"
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

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Test panelinde ek ayarlar

### <a name="luis-endpoint"></a>LUIS uç noktası

Birkaç LUO uç noktanız varsa, test için kullanılan uç noktayı değiştirmek için testin yayımlanan bölmesindeki **ek ayarlar** bağlantısını kullanın. Hangi uç noktanın kullanılacağı konusunda emin değilseniz, varsayılan **Starter_Key**seçin. 

> [!div class="mx-imgBorder"]
> Ek ayarlar bağlantısı vurgulanmış şekilde test paneli ![](media/luis-how-to-interactive-test/additional-settings-v3-settings.png)

<!--
###  View Bing Spell Check corrections in test panel

Requirements to view the spelling corrections: 

* Published app
* Bing Spell Check [service key](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api). The service key is not stored and needs to be reset for each browser session. 

Use the following procedure to include the [Bing Spell Check v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) service  in the Test pane results. 

1. In the **Test** pane, enter an utterance. When the utterance is predicted, select **[Inspect](#inspect-score)** underneath the utterance you entered. 

1. When the **Inspect** panel opens, select **[Compare with Published](#compare-with-published-version)**. 

1. When the **Published** panel opens, select **[Additional Settings](#additional-settings-in-test-panel)**.

1. In the pop-up dialog, check **Enable Bing Spell Check** and enter the key, then select **Done**. 
    ![Enter Bing Spell Check service key](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key-text.png)

1. Enter a query with an incorrect spelling such as `book flite to seattle` and select enter. The incorrect spelling of the word `flite` is replaced in the query sent to LUIS and the resulting JSON shows both the original query, as `query`, and the corrected spelling in the query, as `alteredQuery`.

<a name="json-file-with-no-duplicates"></a>
<a name="import-a-dataset-file-for-batch-testing"></a>
<a name="export-rename-delete-or-download-dataset"></a>
<a name="run-a-batch-test-on-your-trained-app"></a>
<a name="access-batch-test-result-details-in-a-visualized-view"></a>
<a name="filter-chart-results-by-intent-or-entity"></a>
<a name="investigate-false-sections"></a>
<a name="view single-point utterance data"></a>
<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
-->

## <a name="batch-testing"></a>Toplu işe testi
Toplu iş testi [kavramlarını](luis-concept-batch-test.md) görün ve toplu iş türlerini [nasıl](luis-how-to-batch-test.md) test leyeceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Test LUIS uygulamanızı doğru amaç ve varlıkları tanımıyor gösteriyorsa, daha fazla konuşma etiketleme ya da özellikler ekleyerek LUIS uygulamanızın doğruluğunu artırmak için çalışabilir. 

* [LUSıS ile önerilen bir şekilde etiketleme](luis-how-to-review-endpoint-utterances.md) 
* [LUSıS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md) 
