---
title: Varlık Ekle-LUSıS
description: Language Understanding (LUSıS) uygulamalarındaki Kullanıcı dıklarından anahtar verileri ayıklamak için varlıklar oluşturun. Ayıklanan varlık verileri, istemci uygulama tarafından fullfıbcustomer istekleri için kullanılır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: 61e53e6110e545d253dae81e94f8738ee17c4141
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344484"
---
# <a name="add-entities-to-extract-data"></a>Verileri ayıklamak için varlık ekleme

Language Understanding (LUSıS) uygulamalarındaki Kullanıcı dıklarından anahtar verileri ayıklamak için varlıklar oluşturun. Ayıklanan varlık verileri, istemci uygulamanız tarafından fullfıbcustomer istekleri için kullanılır.

Varlık, ayıklanarak ayıklanmasını istediğiniz bir sözcük veya tümceciği temsil eder. Varlıklar, amaca uygun bilgileri betimler ve bazen uygulamanızın görevini gerçekleştirmesi için önemlidir. Hedefe bir örnek ekleme veya bir amaca göre örnek ekleme (önce veya sonrası) bir amaca bakış ekleyerek varlıklar oluşturabilirsiniz.

## <a name="plan-entities-then-create-and-label"></a>Varlıkları planlayın, ardından Oluştur ve etiketle

makine öğrenimi varlıkları, örnek dıklarından oluşturulabilir veya **varlıklar** sayfasından oluşturulabilir.

Genel olarak, portalda makine öğrenimi varlığı oluşturmadan önce varlıkları planlama süresini harcamanız en iyi uygulamadır. Daha sonra, örnek bir noktada makine öğrenimi varlığı oluşturarak bildiğiniz alt varlıklarda ve özelliklerde daha fazla ayrıntı sağlayabilirsiniz. [Birleştirilebilen varlık öğreticisi](tutorial-machine-learned-entity.md) , bu yöntemin nasıl kullanılacağını gösterir.

Varlıkları planlamanın bir parçası olarak, metin ile eşleşen varlıklara (önceden oluşturulmuş varlıklar, normal ifade varlıkları veya liste varlıkları gibi) ihtiyacınız olduğunu bilirsiniz. Bunları örnek bir şekilde etiketlendirmeleri için **varlıklar** sayfasından oluşturabilirsiniz.

Etiketleme sırasında, tek tek varlıkları etiketleyebilir ve bir üst makine öğrenimi varlığına oluşturabilirsiniz. Ya da bir üst makine öğrenimi varlığı ile başlayabilir ve alt varlıklar üzerinde düzenleme yapabilirsiniz.

> [!TIP]
>Sözcükler, istemci uygulamasında ayıklandığında kullanılmasa bile, bir varlığı gösterebilen tüm kelimeleri etiketleyebilir.

## <a name="when-to-create-an-entity"></a>Ne zaman bir varlık oluşturulur

Varlıklarınızı planlamadan sonra, Machine Learning varlıklarınızı ve alt varlıklarınızı oluşturmanız gerekir. Bu, makine öğrenimi varlıklarınız için özellikler sağlamak üzere önceden oluşturulmuş varlıkların veya metin ile eşleşen varlıkların eklenmesini gerektirebilir. Bunların etiketlenmesi önce yapılması gerekir.

Örnek gelişlerini etiketlemeye başladıktan sonra makine tarafından öğrenilen varlıklar oluşturabilir veya liste varlıklarını genişletebilirsiniz.

Her bir varlık türünün uygulama için nerede oluşturulacağını veya ekleneceğini anlamak için aşağıdaki tabloyu kullanın.

|Varlık türü|LUSıS portalında varlık oluşturma|
|--|--|
|makine öğrenimi varlığı|Varlıklar veya amaç ayrıntısı|
|Liste varlığı|Varlıklar veya amaç ayrıntısı|
|Normal ifade varlığı|Varlıklar|
|Pattern.any varlığı|Varlıklar|
|Önceden oluşturulmuş varlık|Varlıklar|
|Önceden oluşturulmuş etki alanı varlığı|Varlıklar|

**Varlıklar** sayfasından tüm varlıkları oluşturabilir veya **Amaç ayrıntısı** sayfasında, varlığı etiketlemenin bir parçası olarak bir dizi varlık oluşturabilirsiniz. Bir varlığı yalnızca **Amaç ayrıntısı** sayfasından bir örnek olarak _etiketleyebilir_ .



## <a name="how-to-create-a-new-custom-entity"></a>Yeni bir özel varlık oluşturma

Bu işlem makine tarafından öğrenilen varlıklar, liste varlıkları ve normal ifade varlıkları için geçerlidir.

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Varlıklar** sayfasını seçin.
1. **+ Oluştur**' u seçin ve varlık türünü seçin.
1. Varlığı yapılandırmaya devam edin, sonra işiniz bittiğinde **Oluştur** ' u seçin.

## <a name="create-a-machine-learned-entity"></a>Makine tarafından öğrenilen varlık oluşturma

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin ve ardından **+ Oluştur**' u seçin.
1. **Varlık türü oluştur** iletişim kutusunda varlığın adını girip **öğrenilen makine**' yi seçin. Alt varlıklar eklemek için **Yapı Ekle**' yi seçin. **Oluştur**'u seçin.

    > [!div class="mx-imgBorder"]
    > ![Makine tarafından öğrenilen varlık oluşturma ekran görüntüsü.](media/add-entities/machine-learned-entity-with-structure.png)

1. **Alt varlıklar Ekle**' de, üst varlık satırında öğesini seçerek bir alt varlık ekleyin **+** .

    > [!div class="mx-imgBorder"]
    > ![Alt varlık ekleme ekran görüntüsü.](media/add-entities/machine-learned-entity-with-subentities.png)

1. Oluşturma işlemini tamamlaması için **Oluştur** ' u seçin.

## <a name="add-a-feature-to-a-machine-learned-entity"></a>Makine tarafından öğrenilen varlığa özellik ekleme

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin ve ardından makine öğrendiği varlığı seçin.
1. Varlık veya alt varlık satırındaki **+ Ekle özelliğini** seçerek bir özellik ekleyin.
1. Mevcut varlıklar ve tümcecik listelerinden seçim yapın.
1. Varlık yalnızca özellik bulunursa ayıklanmalı ise, `*` Bu özellik için yıldız işaretini seçin.

    > [!div class="mx-imgBorder"]
    > ![Varlığa özellik ekleme ekranının ekran görüntüsü.](media/add-entities/machine-learned-entity-schema-with-features.png)

## <a name="create-a-regular-expression-entity"></a>Normal ifade varlığı oluşturma

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. **Derleme** bölümünde, sol paneldeki **varlıklar** ' ı seçin ve ardından **+ Oluştur**' u seçin.

1. **Varlık türü oluştur** iletişim kutusunda, varlığın adını girin ve **Regex**' ı seçin, **Regex** alanına normal ifadeyi girin ve **Oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Normal ifade varlığı oluşturma ekranının ekran görüntüsü.](media/add-entities/add-regular-expression-entity.png)


<a name="add-list-entities"></a>

## <a name="create-a-list-entity"></a>Liste varlığı oluşturma

Liste varlıkları sabit ve kapalı bir ilgili sözcük kümesini temsil eder. Yazar olarak, yazar olarak listeyi değiştireyken, LUYA listeyi küçültmez veya küçülemez. Ayrıca, bir [list Entity. JSON biçimini](reference-entity-list.md#example-json-to-import-into-list-entity)kullanarak var olan bir liste varlığına de aktarabilirsiniz.

Aşağıdaki listede kurallı adı ve eş anlamlılar gösterilmektedir.

|Renk listesi öğe adı|Renk eşanlamlıları|
|--|--|
|Red|Crimson, kan, Apple, Fire-Engine|
|Mavi|gök, Cobalt|
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

> [!TIP]
> Roller, makine öğrenimi varlıklarının alt varlıklarıyla etiketleyerek değiştirilebilir.

1. [Luo portalında](https://www.luis.ai)oturum açın ve bu yazma kaynağına atanmış uygulamaları görmek için **aboneliğinizi** ve **yazma kaynağını** seçin.
1. **Uygulamalarım** sayfasında adını seçerek uygulamanızı açın.
1. Rolü kullanan örnek bir parametre olan amaç ayrıntıları sayfasına gidin.
1. Rol ile etiketlemek için, örnekte varlık etiketini (metin altında Solid Line) seçin ve ardından açılır listeden **varlık bölmesinde görüntüle** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Varlık paletinde görünüm seçme ekran görüntüsü](media/add-entities/view-in-entity-pane.png)

    Varlık paleti sağ tarafta açılır.

1. Varlığı seçin, sonra paletin en altına gidin ve rolü seçin.

    > [!div class="mx-imgBorder"]
    > ![Varlık paletinde görünüm seçme ekran görüntüsü](media/add-entities/select-role-in-entity-palette.png)

<a name="add-pattern-any-entities"></a>
<a name="add-a-patternany-entity"></a>
<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-patternany-entity"></a>Bir model oluşturun. herhangi bir varlık

**Desen. herhangi bir** varlık yalnızca [desenlerle](luis-how-to-model-intent-pattern.md)kullanılabilir.


## <a name="do-not-change-entity-type"></a>Varlık türünü değiştirme

LUSıS, varlığı oluşturmak için nelerin ekleneceğini veya kaldırılacağını bilmediğinden, varlığın türünü değiştirmenize izin vermez. Türü değiştirmek için, biraz farklı bir ada sahip doğru türde yeni bir varlık oluşturmak daha iyidir. Varlık oluşturulduktan sonra, her seferde, etiketli eski varlık adını kaldırın ve yeni varlık adını ekleyin. Tüm dıklılık bir kez daha alındıktan sonra eski varlığı silin.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş modelleri kullanma](howto-add-prebuilt-models.md)

Aşağıdakiler hakkında daha fazla bilgi edinin:
* [Eğitme](luis-how-to-train.md)
* [Test](luis-interactive-test.md) etme
* [Yayımlama](luis-how-to-publish-app.md)
* Lıklarını
    * [Kavramlar](luis-concept-patterns.md)
    * [Sözdizimi](reference-pattern-syntax.md)
* [Önceden oluşturulmuş varlıklar GitHub deposu](https://github.com/Microsoft/Recognizers-Text)
* [Veri ayıklama kavramları](luis-concept-data-extraction.md)



