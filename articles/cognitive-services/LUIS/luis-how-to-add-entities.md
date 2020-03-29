---
title: Varlıklar ekle - LUIS
titleSuffix: Azure Cognitive Services
description: Dil Anlama (LUIS) uygulamalarında kullanıcı söyleyişlerinden önemli verileri ayıklamak için varlıklar oluşturun. Ayıklanan varlık verileri, istemci uygulaması tarafından müşteri isteklerini fullfil için kullanılır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/15/2019
ms.author: diberry
ms.openlocfilehash: 1f2b293acdc77e25e6b932c47d466cc28a04a2b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220913"
---
# <a name="add-entities-to-extract-data"></a>Veri ayıklamak için varlıklar ekleme 

Dil Anlama (LUIS) uygulamalarında kullanıcı söyleyişlerinden önemli verileri ayıklamak için varlıklar oluşturun. Ayıklanan varlık verileri, müşteri uygulamanız tarafından fullfil müşteri istekleriiçin kullanılır.

Varlık, ayıklanması istediğiniz sözcüğün veya tümceciği temsil eder. Varlıklar, amaç ile ilgili bilgileri açıklar ve bazen uygulamanızın görevini yerine getirmesi için gereklidir. Bir amaca örnek bir söz eklediğinizde veya bir niyete örnek bir söz eklediğinizde (önce veya sonra) bir niyete örnek bir söz ekleyerek varlıklar oluşturabilirsiniz.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="plan-entities-then-create-and-label"></a>Varlıkları planlayın, ardından oluşturma ve etiketleme

Makinede öğrenilen varlıklar örnek sözlerden oluşturulabilir veya **Varlıklar** sayfasından oluşturulabilir. 

Genel olarak, en iyi uygulama portalda makine öğrenilen bir varlık oluşturmadan önce varlıkları planlamak için zaman harcamaktır. Daha sonra örnek söyleyişmakineöğrenilen varlık alt bileşenleri ve tanımlayıcılar ve kısıtlamalar olarak o anda bildiğiniz kadar ayrıntı ile oluşturun. [Decomposable varlık öğretici](tutorial-machine-learned-entity.md) nasıl bu yönteminin kullanılacağını gösterir. 

Varlıkları planlamanın bir parçası olarak, metin eşleştirme varlıklara (önceden oluşturulmuş varlıklar, normal ifade varlıkları veya liste varlıkları gibi) ihtiyacınız olduğunu bilirsiniz. Bunları, örnek sözlerle etiketlenmeden önce **Varlıklar** sayfasından oluşturabilirsiniz. 

Etiketleme yaparken, tek tek varlıkları etiketleyebilir ve ardından üst makineden öğrenilen bir varlık oluşturabilirsiniz. Ya da üst makineden öğrenilen bir varlıkla başlayıp alt varlıklara ayrıştırabilirsiniz. 

> [!TIP] 
>İstemci uygulamasında ayıklandığında sözcükler kullanılmasa bile, varlığı gösterebilecek tüm sözcükleri etiketlendirin. 

## <a name="creating-an-entity-before-or-with-labeling"></a>Etiketlemeden önce veya etiketleme ile önce bir varlık oluşturma

Uygulamanın hangi varlıkların nerede oluşturulacağını veya uygulamaya ekleyebileceğinizi anlamak için aşağıdaki tabloyu kullanın. 

|Varlık türü|LUIS portalında varlık nerede oluşturulur?|
|--|--|
|Makine tarafından öğrenilen varlık|Varlıklar veya Niyet ayrıntısı|
|Liste varlığı|Varlıklar veya Niyet ayrıntısı|
|Normal ifade varlığı|Varlıklar|
|Pattern.any varlığı|Varlıklar|
|Önceden oluşturulmuş varlık|Varlıklar|
|Önceden oluşturulmuş etki alanı varlığı|Varlıklar|

**Varlıklar** sayfasından tüm varlıkları oluşturabilir veya **Amaç ayrıntı** sayfasındaki örnek teypteki varlığı etiketlemenin bir parçası olarak birkaç varlık oluşturabilirsiniz. Bir varlığı yalnızca **Niyet ayrıntısı** sayfasından örnek bir sözle _etiketleyebilirsiniz._ 

## <a name="create-a-machine-learned-entity"></a>Makinede öğrenilen bir varlık oluşturma

[!INCLUDE [Create and label entities in machine-learned tutorial](includes/decomposable-tutorial-links.md)]

## <a name="create-a-text-matching-entity"></a>Metin eşleştirme varlık oluşturma

Metin eşleştirme varlıkları kullanma veri ayıklamak için çeşitli yollar sağlar:

|Metin eşleştirme varlıkları|Amaç|
|--|--|
|[Liste varlığı](#add-list-entities-for-exact-matches)|alternatif formlar olarak eşanlamlıları ile birlikte kanonik adların listesi|
|Normal ifade varlığı|normal bir ifade varlığı kullanarak metni eşleştirme|
|[Önceden oluşturulmuş varlık](tutorial-machine-learned-entity.md#add-prebuilt-number-to-help-extract-data)|sayı, e-posta, tarih gibi yaygın veri türlerini eşleştirme|
|Önceden oluşturulmuş etki alanı varlığı|seçili konu etki alanlarını kullanarak eşleştirme|
|[Desen.herhangi bir](#add-a-patternany-entity)| çevreleyen metinle kolayca karıştırılabilecek varlıkları eşleştirmek için|  

Önceden oluşturulmuş varlıklar herhangi bir özel eğitim verisi sağlamadan çalışır. Diğer varlıkların müşteri eğitim verilerini (List entity'in öğeleri gibi) veya bir ifade (normal bir ifade veya desen.any gibi) sağlamanız gerekir.

<a name="add-list-entities"></a>

### <a name="how-to-create-a-new-custom-entity"></a>Yeni bir özel varlık oluşturma

1. LUIS portalında, **Yönet** bölümüne, ardından **Varlıklar** sayfasına gidin. 
1. + **Oluştur'u**seçin, ardından varlık türünü seçin. 
1. Varlığı yapılandırmaya devam edin ve bittiğinde **Oluştur'u** seçin. 

### <a name="add-list-entities-for-exact-matches"></a>Tam eşleşmeler için liste varlıkları ekleme

Liste varlıkları, sabit, kapalı bir ilgili sözcük kümesini temsil ediyor. Siz, yazar olarak, listeyi değiştirebilirsiniz, luis büyümek veya liste küçültmek olmaz. Ayrıca bir [list entity .json formatı (referans-entity-list.md#example-json-to-içe aktarma-liste içine-varlık) kullanarak varolan bir liste varlığı için içe aktarabilirsiniz. 

Aşağıdaki liste, kanonik adı ve eşanlamlıları gösterir. 

|Renk - liste öğesi adı|Renk - eşanlamlı|
|--|--|
|Red|kızıl, kan, elma, itfaiye|
|Mavi|gökyüzü, masmavi, kobalt|
|Yeşil|kelly, kireç|

Bir liste varlığı oluşturmak için yordamı kullanın. Liste varlığı oluşturulduktan sonra, örnek sözcüklerini bir niyetle etiketlemeniz gerekmez. Liste öğeleri ve eşanlamlılar tam metin kullanılarak eşlenir. 

1. **Yapı** bölümünden sol paneldeki **Varlıkları** seçin ve sonra **+ Oluştur'u**seçin.

1. Varlık **türü** iletişim kutusu oluştur'da, varlık adı girin, `Colors` gibi ve **Liste'yi**seçin.
1. Liste **varlık** iletişim kutusunda, **yeni alt liste ekle....** gibi liste öğesi adını `Green`girin, ardından eş anlamlılar ekleyin.

    > [!div class="mx-imgBorder"]
    > ![Varlık ayrıntı sayfasında liste varlığı olarak renklerin listesini oluşturun.](media/how-to-add-entities/create-list-entity-of-colors.png) 

1. Liste öğeleri ve eşanlamlılar eklemeyi bitirdiğinizde **Oluştur'u**seçin.

    Uygulamada bir grup değişiklik yaptığınızda, uygulamayı **eğitmeyi** unutmayın. Tek bir değişiklikten sonra uygulamayı eğitme. 

    > [!NOTE]
    > Bu yordam, **Niyet ayrıntı** sayfasındaki örnek bir sözcükten bir liste varlığı oluşturmayı ve etiketlemeyi gösterir. Varlıklar **sayfasından** da aynı varlığı oluşturabilirsiniz.

## <a name="add-a-role-for-an-entity"></a>Varlık için rol ekleme

Rol, bağlama dayalı bir varlığın adlandırılmış bir alt türüdür. 

### <a name="add-a-role-to-distinguish-different-contexts"></a>Farklı bağlamları ayırt etmek için rol ekleme

Aşağıdaki söyleyiş, iki konum vardır ve her biri gibi etrafında kelimelerle `to` semantik olarak belirtilir ve: `from` 

`Pick up the package from Seattle and deliver to New York City.`

Bu yordamda, `origin` `destination` önceden oluşturulmuş bir geographyV2 varlığına ekleme ve roller.

1. **Yapı** bölümünden sol paneldeki **Varlıkları** seçin.

1. Select **+ Önceden oluşturulmuş varlık ekle**. **GeographyV2'yi** seçin ve **Bitti'yi**seçin. Bu, uygulamaya önceden oluşturulmuş bir varlık ekler.
    
    Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](reference-pattern-syntax.md#explicit-lists) kullanın. 

1. **Varlıklar** sayfa listesinden yeni eklenen önceden oluşturulmuş geographyV2 varlığını seçin. 
1. Yeni bir rol eklemek **+** için, **eklenen rolleri yok'un**yanına seçin.
1. Türü **rolü...** textbox'ına, rolün `Origin` adını girin ve sonra girin. `Destination` Sonra ikinci bir rol adı girin. 

    > [!div class="mx-imgBorder"]
    > ![Konum varlığına Başlangıç rolü ekleme ekran görüntüsü](media/how-to-add-entities//add-role-to-prebuilt-geographyv2-entity.png)

    Rol önceden oluşturulmuş varlığa eklenir, ancak bu varlığı kullanan herhangi bir söze eklenmez. 

### <a name="label-text-with-a-role-in-an-example-utterance"></a>Örnek bir sözlemede rol içeren metni etiketleme

1. Rolü kullanan örnek sözcüklerle birlikte Niyet ayrıntıları sayfasına gidin. 
1. Rolü etiketlemek için, örnek söyleyişdeki varlık etiketini (metin altındaki düz çizgi) seçin ve ardından açılan listeden **varlık paletinde Görünüm'ü** seçin. 

    > [!div class="mx-imgBorder"]
    > ![Varlık Paletinde Görünüm'ü seçme ekran görüntüsü](media/how-to-add-entities/select-text-label-with-entity-palette-for-role.png)   

    Varlık paleti sağa açılır. 

1. Varlığı seçin, ardından paletin altına gidin ve rolü seçin. 

    > [!div class="mx-imgBorder"]
    > ![Varlık Paletinde Görünüm'ü seçme ekran görüntüsü](media/how-to-add-entities/select-role-from-entity-palette-entity-inspector.png)

<a name="add-pattern-any-entities"></a>

## <a name="add-a-patternany-entity"></a>Desen ekle.herhangi bir varlık

[Desen.herhangi bir](luis-concept-entity-types.md) varlık sadece [desenlerde](luis-how-to-model-intent-pattern.md)geçerlidir, niyetin örnek söyleyişlerinde değil. Bu tür bir varlık, LUIS'in değişen uzunluk ve sözcük seçimindeki varlıkların sonunu bulmasına yardımcı olur. Bu varlık bir desende kullanıldığından, LUIS varlığın sonunun sözcük şablonunda nerede olduğunu bilir.

### <a name="steps-to-create-a-patternany-entity"></a>Bir desen oluşturmak için adımlar.herhangi bir varlık

1. **Yapı** bölümünden sol paneldeki **Varlıkları** seçin ve sonra **+ Oluştur'u**seçin.

1. Varlık türü iletişim kutusunu **seç,** **Ad** kutusuna varlık adını girin ve **Desen.Any'yi** **Tür** olarak seçin ve ardından **Oluştur'u**seçin.

    Bu varlığı kullanarak [bir desen sözcük](luis-how-to-model-intent-pattern.md) oluşturmayaptığınızda, varlık birleştirilmiş makine öğrenilen ve metin eşleştirme algoritması ile ayıklanır. 

### <a name="create-a-pattern-template-utterance-to-use-patternany-entity"></a>Desen kullanmak için bir desen şablonu söyleyiş oluşturma.herhangi bir varlık

Deseni kullanmak için.herhangi bir varlık, **Desenler** sayfasında, **uygulama performansını geliştir** bölümünde, doğru kıvırcık `Where is **{HumanResourcesFormTitle}** on the server?`ayraç sözdizimi ile bir desen ekleyin.

Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](reference-pattern-syntax.md#explicit-lists) kullanın. 

## <a name="do-not-change-entity-type"></a>Varlık türünü değiştirmeyin

LUIS, varlığı oluşturmak için ne ekleyeceğini veya kaldıracağını bilmediği için varlığın türünü değiştirmenize izin vermez. Türü değiştirmek için, biraz farklı bir ada sahip doğru türde yeni bir varlık oluşturmak daha iyidir. Varlık oluşturulduktan sonra, her bir sözcükte, eski etiketli varlık adını kaldırın ve yeni varlık adını ekleyin. Tüm söyleyişler yeniden etiketlendikten sonra, eski varlığı silin. 

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Önceden oluşturulmuş modelleri kullanma](howto-add-prebuilt-models.md) 

Aşağıdakiler hakkında daha fazla bilgi edinin:
* Nasıl [eğitilir?](luis-how-to-train.md)
* Nasıl [test](luis-interactive-test.md) edile
* Nasıl [yayımlanır?](luis-how-to-publish-app.md)
* Desen:
    * [Kavramlar](luis-concept-patterns.md)
    * [Sözdizimi](reference-pattern-syntax.md)
* [Önceden inşa edilmiş varlıklar GitHub deposu](https://github.com/Microsoft/Recognizers-Text)
* [Veri Çıkarma kavramları](luis-concept-data-extraction.md)


 
