---
title: Varlık Ekle-LUSıS
description: Language Understanding (LUSıS) uygulamalarındaki Kullanıcı dıklarından anahtar verileri ayıklamak için varlıklar oluşturun. Ayıklanan varlık verileri, istemci uygulama tarafından fullfıbcustomer istekleri için kullanılır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: diberry
ms.openlocfilehash: 8be5dac2d1d5e8a5a3ceafff6b51b2d89e03993f
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83593299"
---
# <a name="add-entities-to-extract-data"></a>Verileri ayıklamak için varlık ekleme

Language Understanding (LUSıS) uygulamalarındaki Kullanıcı dıklarından anahtar verileri ayıklamak için varlıklar oluşturun. Ayıklanan varlık verileri, istemci uygulamanız tarafından fullfıbcustomer istekleri için kullanılır.

Varlık, ayıklanarak ayıklanmasını istediğiniz bir sözcük veya tümceciği temsil eder. Varlıklar, amaca uygun bilgileri betimler ve bazen uygulamanızın görevini gerçekleştirmesi için önemlidir. Hedefe bir örnek ekleme veya bir amaca göre örnek ekleme (önce veya sonrası) bir amaca bakış ekleyerek varlıklar oluşturabilirsiniz.

## <a name="plan-entities-then-create-and-label"></a>Varlıkları planlayın, ardından Oluştur ve etiketle

Makine tarafından öğrenilen varlıklar, örnek dıklarından oluşturabilir veya **varlıklar** sayfasından oluşturulabilir.

Genel olarak, portalda makine tarafından öğrenilen bir varlık oluşturmadan önce varlıkları planlama süresini harcamanız en iyi uygulamadır. Daha sonra, bu örnekte, daha fazla ayrıntı ve zaman içinde bildiğiniz alt varlıklarda ve özelliklerde daha fazla ayrıntı ile makine tarafından öğrenilen bir varlık oluşturun. [Birleştirilebilen varlık öğreticisi](tutorial-machine-learned-entity.md) , bu yöntemin nasıl kullanılacağını gösterir.

Varlıkları planlamanın bir parçası olarak, metin ile eşleşen varlıklara (önceden oluşturulmuş varlıklar, normal ifade varlıkları veya liste varlıkları gibi) ihtiyacınız olduğunu bilirsiniz. Bunları örnek bir şekilde etiketlendirmeleri için **varlıklar** sayfasından oluşturabilirsiniz.

Etiketleme sırasında tek tek varlıkları etiketleyebilir ve ardından bir üst makineye öğrenilen varlık oluşturabilirsiniz. Ya da bir üst makine öğrenmiş varlıkla başlayabilir ve alt varlıklarda bir varlık oluşturabilir.

> [!TIP]
>Sözcükler, istemci uygulamasında ayıklandığında kullanılmasa bile, bir varlığı gösterebilen tüm kelimeleri etiketleyebilir.

## <a name="creating-an-entity-before-or-with-labeling"></a>Etiketleme öncesinde veya etiketleyerek bir varlık oluşturma

Her bir varlığın hangi varlıkları oluşturmak veya uygulamaya ekleneceğini anlamak için aşağıdaki tabloyu kullanın.

|Varlık türü|LUSıS portalında varlık oluşturma|
|--|--|
|Makine tarafından öğrenilen varlık|Varlıklar veya amaç ayrıntısı|
|Liste varlığı|Varlıklar veya amaç ayrıntısı|
|Normal ifade varlığı|Varlıklar|
|Pattern.any varlığı|Varlıklar|
|Önceden oluşturulmuş varlık|Varlıklar|
|Önceden oluşturulmuş etki alanı varlığı|Varlıklar|

**Varlıklar** sayfasından tüm varlıkları oluşturabilir veya **Amaç ayrıntısı** sayfasında, varlığı etiketlemenin bir parçası olarak bir dizi varlık oluşturabilirsiniz. Bir varlığı yalnızca **Amaç ayrıntısı** sayfasından bir örnek olarak _etiketleyebilir_ .

## <a name="create-a-machine-learned-entity"></a>Makine tarafından öğrenilen bir varlık oluşturma

[!INCLUDE [Create and label entities in machine-learned tutorial](includes/decomposable-tutorial-links.md)]

## <a name="create-a-text-matching-entity"></a>Metin ile eşleşen bir varlık oluşturma

Metin eşleştirme varlıklarını kullanma, verileri ayıklamak için çeşitli yollar sağlar:

|Metin eşleştirme varlıkları|Amaç|
|--|--|
|[Liste varlığı](reference-entity-list.md)|alternatif formlar olarak eş anlamlılarla birlikte kurallı adların listesi|
|Normal ifade varlığı|normal ifade varlığı kullanarak metni Eşleştir|
|[Önceden oluşturulmuş varlık](luis-reference-prebuilt-entities.md)|sayı, e-posta, tarih gibi ortak veri türlerini Eşleştir|
|Önceden oluşturulmuş etki alanı varlığı|seçili konu etki alanlarını kullanarak Eşleştir|
|[Model. any](reference-entity-pattern-any.md)| çevreleyen metinle kolayca karışabilme varlıkları eşleştirmek için|

Önceden oluşturulmuş varlıklar, herhangi bir özel eğitim verisi sağlamadan çalışır. Diğer varlıkların, müşteri eğitim verileri (liste varlığının öğeleri gibi) veya bir ifade (normal ifade veya model gibi) sağlamanız gerekir.

<a name="add-list-entities"></a>

### <a name="how-to-create-a-new-custom-entity"></a>Yeni bir özel varlık oluşturma

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Varlıklar** sayfasını seçin.
1. **+ Oluştur**' u seçin ve varlık türünü seçin.
1. Varlığı yapılandırmaya devam edin, sonra işiniz bittiğinde **Oluştur** ' u seçin.

### <a name="add-list-entities-for-exact-matches"></a>Tam eşleşmeler için liste varlıkları ekleme

Liste varlıkları sabit ve kapalı bir ilgili sözcük kümesini temsil eder. Yazar olarak, yazar olarak listeyi değiştireyken, LUYA listeyi küçültmez veya küçülemez. Ayrıca, bir [list Entity. JSON biçimini](reference-entity-list.md#example-json-to-import-into-list-entity)kullanarak var olan bir liste varlığına de aktarabilirsiniz.

Aşağıdaki listede kurallı adı ve eş anlamlılar gösterilmektedir.

|Renk listesi öğe adı|Renk eşanlamlıları|
|--|--|
|Red|Crimson, kan, Apple, Fire-Engine|
|Mavi|gök, Azure, Cobalt|
|Yeşil|Kelly, limon sarısı|

Bir liste varlığı oluşturmak için yordamını kullanın. Liste varlığı oluşturulduktan sonra, bir amaç için örnek söyleyeni etiketlemenize gerek yoktur. Liste öğeleri ve eş anlamlılar, tam metin kullanılarak eşleştirilir.
1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin ve ardından **+ Oluştur**' u seçin.

1. **Varlık türü oluştur** iletişim kutusunda, varlık adını girin, örneğin `Colors` ve **liste**seçin.
1. **Liste varlığı oluştur** iletişim kutusunda **Yeni alt liste ekle...** öğesine, gibi bir liste öğesi adı girip `Green` eş anlamlılar ekleyin.

    > [!div class="mx-imgBorder"]
    > ![Varlık ayrıntısı sayfasında bir liste varlığı olarak renklerin bir listesini oluşturun.](media/how-to-add-entities/create-list-entity-of-colors.png)

1. Liste öğeleri ve eş anlamlılar ekleme işiniz bittiğinde **Oluştur**' u seçin.

    Uygulamada bir değişiklik grubuyla işiniz bittiğinde, uygulamayı **eğiten** unutmayın. Uygulamayı tek bir değişiklikten sonra eğmeyin.

    > [!NOTE]
    > Bu yordam, **Amaç ayrıntısı** sayfasında bir liste varlığının bir örnek ile oluşturulmasını ve etiketlenmesini gösterir. Aynı varlığı **varlıklar** sayfasından da oluşturabilirsiniz.

## <a name="add-a-role-for-an-entity"></a>Bir varlık için rol ekleme

Rol, bir varlığın bağlam temelinde adlandırılmış bir alt türüdür.

### <a name="add-a-role-to-distinguish-different-contexts"></a>Farklı bağlamları ayırt etmek için rol ekleme

Aşağıdaki söyleyde iki konum vardır ve her biri, ve gibi çeşitli sözcükler tarafından anlam olarak belirtilmiştir `to` `from` :

`Pick up the package from Seattle and deliver to New York City.`

Bu yordamda, `origin` `destination` önceden oluşturulmuş bir geographyV2 varlığına roller ekleyin.
1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin.

1. **+ Önceden oluşturulmuş varlık Ekle**' yi seçin. **GeographyV2** ' ı seçin ve **bitti**' yi seçin. Bu, uygulamaya önceden oluşturulmuş bir varlık ekler.

    Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](reference-pattern-syntax.md#explicit-lists) kullanın.

1. **Varlıklar sayfa listesinden** yeni eklenen önceden oluşturulmuş geographyV2 varlığını seçin.
1. Yeni bir rol eklemek için, **+** **hiçbir rolün eklenmediyse**ileri ' yi seçin.
1. **Rol...** metin kutusuna rolün adını girin ve `Origin` ardından ENTER tuşuna yazın. İkinci bir rol adı ekleyin ve `Destination` ardından girin.

    > [!div class="mx-imgBorder"]
    > ![Konum varlığına kaynak rolü ekleme ekran görüntüsü](media/how-to-add-entities//add-role-to-prebuilt-geographyv2-entity.png)

    Rol önceden oluşturulmuş varlığa eklenir, ancak bu varlık kullanılarak herhangi bir söyleye eklenmez.

### <a name="label-text-with-a-role-in-an-example-utterance"></a>Örnekte bir rol ile metin etiketle
1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. Rolü kullanan örnek bir parametre olan amaç ayrıntıları sayfasına gidin.
1. Rol ile etiketlemek için, örnekte varlık etiketini (metin altında Solid Line) seçin ve ardından açılır listeden **varlık paletinde görüntüle** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Varlık paletinde görünüm seçme ekran görüntüsü](media/how-to-add-entities/select-text-label-with-entity-palette-for-role.png)

    Varlık paleti sağ tarafta açılır.

1. Varlığı seçin, sonra paletin en altına gidin ve rolü seçin.

    > [!div class="mx-imgBorder"]
    > ![Varlık paletinde görünüm seçme ekran görüntüsü](media/how-to-add-entities/select-role-from-entity-palette-entity-inspector.png)

<a name="add-pattern-any-entities"></a>

## <a name="add-a-patternany-entity"></a>Bir model ekleyin. herhangi bir varlık

[Desen. tüm](luis-concept-entity-types.md) varlıklar yalnızca [desenlere](luis-how-to-model-intent-pattern.md)göre geçerlidir, amaç örnekleri değildir. Bu tür bir varlık, LUSıS 'in değişen uzunluk ve sözcük seçimine ait varlıkların sonunu bulmasına yardımcı olur. Bu varlık bir düzende kullanıldığı için, Lua, varlığın sonunun söylenişi şablonunda olduğunu bilir.

### <a name="steps-to-create-a-patternany-entity"></a>Bir model oluşturma adımları. herhangi bir varlık
1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin ve ardından **+ Oluştur**' u seçin.

1. **Varlık türü seç** iletişim kutusunda, **ad** kutusuna varlık adını girin ve ardından model ' i seçin **.** tür olarak **yazın** ve **Oluştur**' u seçin.

    Bu varlığı kullanarak [bir desen oluşturduktan](luis-how-to-model-intent-pattern.md) sonra varlık, birleştirilmiş bir makine tarafından öğrenilen ve metin eşleştirme algoritması ile ayıklanır.

### <a name="create-a-pattern-template-utterance-to-use-patternany-entity"></a>Desen kullanmak için desen şablonu oluşturma. herhangi bir varlık

Deseni kullanmak için. herhangi bir varlık, **Düzen sayfasında,** **uygulama performansını iyileştirme** bölümünde, gibi doğru küme ayracı sözdizimi ile bir desen ekleyin `Where is **{HumanResourcesFormTitle}** on the server?` .

Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](reference-pattern-syntax.md#explicit-lists) kullanın.

## <a name="do-not-change-entity-type"></a>Varlık türünü değiştirme

LUSıS, varlığı oluşturmak için nelerin ekleneceğini veya kaldırılacağını bilmediğinden, varlığın türünü değiştirmenize izin vermez. Türü değiştirmek için, biraz farklı bir ada sahip doğru türde yeni bir varlık oluşturmak daha iyidir. Varlık oluşturulduktan sonra, her seferde, etiketli eski varlık adını kaldırın ve yeni varlık adını ekleyin. Tüm dıklılık bir kez daha alındıktan sonra eski varlığı silin.

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş modelleri kullanma](howto-add-prebuilt-models.md)

Aşağıdakiler hakkında daha fazla bilgi edinin:
* [Eğitme](luis-how-to-train.md)
* [Test](luis-interactive-test.md) etme
* [Yayımlama](luis-how-to-publish-app.md)
* Lıklarını
    * [Kavramlar](luis-concept-patterns.md)
    * [Söz dizimi](reference-pattern-syntax.md)
* [Önceden oluşturulmuş varlıklar GitHub deposu](https://github.com/Microsoft/Recognizers-Text)
* [Veri ayıklama kavramları](luis-concept-data-extraction.md)



