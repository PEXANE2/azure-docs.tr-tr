---
title: 'Öğretici: makine öğrenimi varlığı ile yapılandırılmış verileri ayıklama-LUSıS'
description: Makine öğrenimi varlığını kullanarak bir noktadan yapılandırılmış verileri ayıklayın. Ayıklama doğruluğunu artırmak için, özelliklerle alt varlıklar ekleyin.
ms.topic: tutorial
ms.date: 05/08/2020
ms.openlocfilehash: eb9761a3d3a98a3318fe0adc6fa170652639a9a1
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045612"
---
# <a name="tutorial-extract-structured-data-from-user-utterance-with-machine-learning-entities-in-language-understanding-luis"></a>Öğretici: Language Understanding (LUSıS) içinde makine öğrenimi varlıkları ile Kullanıcı aradıklarından yapılandırılmış verileri Ayıkla

Bu öğreticide, makine öğrenimi varlığını kullanarak bir noktadan yapılandırılmış verileri ayıklayın.

Makine öğrenimi varlığı, [özelliklerle](luis-concept-feature.md)alt varlık varlıkları sağlayarak [model ayrıştırma kavramını](luis-concept-model.md#v3-authoring-model-decomposition) destekler.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Örnek uygulamayı içeri aktar
> * Makine öğrenimi varlığı ekleme
> * Alt varlık ve Özellik Ekle
> * Eğitim, test, uygulama yayımlama
> * Uç noktadan varlık tahminini al

[!INCLUDE [LUIS Free account](includes/quickstart-tutorial-use-free-starter-key.md)]


## <a name="why-use-a-machine-learning-entity"></a>Makine öğrenimi varlığı neden kullanılmalıdır?

Bu öğretici, bir kullanıcının utanından veri ayıklamak için bir makine öğrenimi varlığı ekler.

Varlık, ayıklananlık içinden Ayıklanacak verileri tanımlar. Bu, verileri bir ad, bir tür (mümkünse), belirsizlik varsa verilerin herhangi bir çözümlemesini ve verileri oluşturan tam metni içerir.

Verileri tanımlamak için şunları yapmanız gerekir:
* Varlığı oluşturma
* Metni, varlığı temsil eden örnek olarak etiketleyin. Bu etiketlenmiş örnekler, LUID 'in varlığın ne olduğunu ve bir utterde nerede bulabileceklerini öğretin.

## <a name="entity-decomposability-is-important"></a>Varlık ayırıcı, önemli

Varlık ayırıcı, hem amaç tahmini hem de varlıkla veri ayıklama için önemlidir.

Veri ayıklama için başlangıç ve en üst düzey varlık olan makine öğrenimi varlığı ile başlayın. Sonra varlığı alt varlıklara parçalara ayırın.

Uygulamanıza başladığınızda varlığınızın ne kadar ayrıntılı olmasını bildiğinize karşın, en iyi uygulama bir makine öğrenimi varlığıyla başlamak ve sonra uygulamanız için alt varlıklarla birlikte oluşturmak olacaktır.

Bu öğreticide, pizza uygulaması için bir siparişi temsil eden bir makine öğrenimi varlığı oluşturacaksınız. Varlık, siparişle ilgili metnin ayıklanmasına, boyutun ve miktarın dışına çıkaracaktır.

Bir utterlik, `Please deliver one large cheese pizza to me` `one large cheese pizza` sıra olarak ayıklanmalı `1` ve ayrıca miktar ve boyut için de ayıklanmalıdır `large` .

## <a name="download-json-file-for-app"></a>Uygulama için JSON dosyasını indirin

[Uygulama json dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-intents-only.json)indirip kaydedin.

## <a name="import-json-file-for-app"></a>Uygulama için JSON dosyasını içeri aktar

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="create-machine-learned-entity"></a>Makine tarafından öğrenilen varlık oluştur

Bir pizza sırası hakkındaki ayrıntıları ayıklamak için, en üst düzey, makine öğrenimi bir `Order` varlık oluşturun.

1. **Amaçlar** sayfasında **orderpizza** hedefini seçin.

1. Örnek söyleyenlik listesinde, aşağıdaki utterance ' ı seçin.

    |Sipariş örneği|
    |--|
    |`pickup a cheddar cheese pizza large with extra anchovies`|

    En solundaki (#1) metinden hemen önce seçmeye başlayın `pickup` , sonra sağ taraftaki metnin hemen ötesine geçin `anchovies` (#2-bu etiketleme işlemini sonlandırır). Bir açılır menü görünür. Açılır kutuya varlığın adını `Order` (#3) olarak girin. Sonra `Order Create new entity` listeden (#4) öğesini seçin.

    ![Metnin başlangıç ve bitiş sırası için etiketi](media/tutorial-machine-learned-entity/mark-complete-order.png)

    > [!NOTE]
    > Bir varlık her zaman tüm söylik olmaz. Bu özel durumda, `pickup` siparişin nasıl alınacağını gösterir. Kavramsal bir perspektiften, `pickup` sıralama için etiketlenmiş varlığın bir parçası olmalıdır.

1. **Varlık türü seçin** kutusunda, **Yapı Ekle** ' yi seçin ve ardından **İleri**' yi seçin. Boyut ve miktar gibi alt varlıklar eklemek için yapı gereklidir.

    ![Varlığa yapı Ekle](media/tutorial-machine-learned-entity/add-structure-to-entity.png)

1. **Alt varlıklar Ekle (isteğe bağlı)** kutusunda, satırda ' ı seçin **+** `Order` , ardından `Size` `Quantity` alt varlıklar ekleyin ve ardından **Oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![Varlığa yapı Ekle](media/tutorial-machine-learned-entity/add-subentities-when-creating-machine-learned-entity.png)

## <a name="edit-subentities-to-improve-extraction"></a>Ayıklamayı geliştirmek için alt varlıkları düzenleme

Önceki adımlarda varlık ve alt varlık oluşturulur. Ayıklamayı geliştirmek için, alt varlıklara özellikler ekleyin.

### <a name="improve-size-extraction-with-phrase-list"></a>Tümcecik listesiyle boyut ayıklamayı iyileştirme

1. Sol menüden **varlıklar** ' ı seçin ve ardından **sipariş** varlığı ' nı seçin.

1. **Şema ve Özellikler** sekmesinde, alt varlık **boyutu** ' nu seçin ve **+ Özellik Ekle**' yi seçin.

1. Açılan menüden **Yeni tümcecik listesi oluştur** ' u seçin.

1. **Yeni tümcecik oluştur liste** kutusuna adı girin, `SizePhraselist` ardından şu değerleri girin: `small` , `medium` , ve `large` . **Öneriler** kutusu dolduğunda, `extra large` ve ' yi seçin `xl` . Yeni tümcecik listesini oluşturmak için **Oluştur** ' u seçin.

    Bu tümcecik listesi özelliği, `Size` alt varlığın boyutla ilgili sözcükleri örnek sözcükle birlikte girerek bulmasına yardımcı olur. Bu tümcecik listesinin her kelime içermesi gerekmez, ancak boyutu göstermek için beklenen sözcükleri içermesi gerekir.

### <a name="add-sizelist-entity"></a>SizeList varlığı Ekle

İstemci uygulamanın tanıdığı bilinen boyutların bir listesini eklemek, ayıklama işleminin de sağlanmasına yardımcı olur.

1. Sol menüden **varlıkları** seçin ve **+ Oluştur**' u seçin.

1. Varlık adını olarak ayarlayın `SizeListentity` ve bir önceki bölümde oluşturulan ile karşılaştırıldığında tanımlanması kolay olacak şekilde türü **liste** olarak ayarlayın `SizePhraselist` .

1. İstemci uygulamanın beklediği boyutları ekleyin: `Small` , `Medium` , `Large` ve `XLarge` ardından her biri için eş anlamlılar ekleyin. Eş anlamlılar, kullanıcının sohbet bot 'a girdiği terimler olmalıdır. Varlık, normalleştirilmiş değer veya eş anlamlıya tam olarak eşleştiğinde bir liste varlığı ile ayıklanır.

    |Normalleştirilmiş değer|Eş anlamlılar|
    |--|--|
    |Küçük|SM, SML, küçük, en küçük|
    |Orta|MD, MDM, normal, ortalama, orta|
    |Büyük|LG, LRG, Big|
    |XLarge|XL, büyük, çok büyük paketlerini|


    > [!div class="mx-imgBorder"]
    > ![Varlığa yapı Ekle](media/tutorial-machine-learned-entity/size-list-entity-with-synonyms.png)

### <a name="add-feature-of-sizelist-entity"></a>SizeList varlığının özelliğini ekleyin

1. Varlıklar listesine dönmek için sol menüden **varlıklar** ' ı seçin.

1. Varlıklar listesinden **sipariş** ' i seçin.

1. **Şema ve Özellikler** sekmesinde, varlık **boyutu** ' nu seçin ve **+ Özellik Ekle**' yi seçin.

1. Açılan listeden **@ Sizelistentity** ' ı seçin.

<a name="add-prebuilt-number-entity-to-app"></a>

### <a name="add-prebuilt-number-entity"></a>Önceden oluşturulmuş sayı varlığı Ekle

Önceden oluşturulmuş bir sayı varlığı eklemek ayıklamanın de sağlanmasına yardımcı olur.

1. Sol menüden **varlık** ' ı seçin ve ardından **önceden oluşturulmuş varlık Ekle**' yi seçin.

1. Listeden **sayı** ' yı seçin ve **bitti**' yi seçin.

1. Varlıklar listesine dönmek için sol menüden **varlıklar** ' ı seçin.

### <a name="add-feature-of-prebuilt-number-entity"></a>Önceden oluşturulmuş sayı varlığının özelliği Ekle

1. Varlıklar listesinden **sipariş** ' i seçin.

1. **Şema ve Özellikler** sekmesinde, **Miktar** varlığını seçin ve **+ Özellik Ekle**' yi seçin.

1. Açılan listeden **@ sayı** ' yı seçin.

<a name="create-subcomponent-entity-with-constraint-to-help-extract-data"></a>

## <a name="configure-required-features"></a>Gerekli özellikleri yapılandırma

**Düzen** varlığının varlık ayrıntısı sayfasında, `*` hem **@ sizelist** özelliği hem de **@ Number** özelliği için yıldız işaretini seçin. Yıldız işareti, özellik adı ile aynı etikette görüntülenir.

> [!div class="mx-imgBorder"]
> ![Varlığa yapı Ekle](media/tutorial-machine-learned-entity/set-required-feature-on-subentity.png)

<a name="label-text-as-entities-in-example-utterances"></a>
<a name="label-example-utterance-to-teach-luis-about-the-entity"></a>

## <a name="label-example-utterances"></a>Etiket örnek utbotları

Makine tarafından öğrenilen varlık oluşturulur ve alt varlıkların özellikleri vardır. Ayıklama iyileştirmesinin tamamlanabilmesi için, örnek dıklarınızın alt varlıklarla etiketlenmesi gerekir.

1. Sol gezinmede **amaçları** seçin, sonra **orderpizza** hedefini seçin.

1. **Varlık paletini**açmak için **@** bağlam araç çubuğunda sembolü seçin.

1. Paletteki her bir varlık satırını seçin ve ardından palet imlecini kullanarak her bir örnekteki varlığı seçin. İşiniz bittiğinde, varlık listesi aşağıdaki görüntüde olduğu gibi görünmelidir.

    > [!div class="mx-imgBorder"]
    > ![Gerekli özelliği yapılandırmanın kısmi ekran görüntüsü](media/tutorial-machine-learned-entity/labeled-example-utterances-for-machine-learned-entity.png)

## <a name="train-the-app"></a>Uygulamayı eğitme

Uygulamayı eğiteiçin **eğitme**' yi seçin. Eğitim, etkin modele yeni varlıklar ve etiketli utterler gibi değişiklikleri uygular.

## <a name="add-a-new-example-utterance"></a>Yeni bir örnek ekleyin

1. Eğitim sonrasında `OrderPizza` makine öğrenimi varlığını ne kadar iyi anladığını görmek için amaca uygun yeni bir örnek ekleyin.

    |Sipariş örneği|
    |--|
    |`I need a large pepperoni pizza`|

    Genel en üst varlık `Order` etiketlidir ve `Size` alt varlık de noktalı çizgilerle etiketlidir.

    > [!div class="mx-imgBorder"]
    > ![Varlıkla tahmin edilen yeni örnek ekran görüntüsü](media/tutorial-machine-learned-entity/new-example-utterance-predicted-with-entity.png)

    Noktalı çizgi, geçerli eğitilen uygulamaya göre tahmini gösterir.

1. Tahmini etiketli bir varlık olarak değiştirmek için, aynı satırdaki onay işaretini seçin.

    > [!div class="mx-imgBorder"]
    > ![Varlıkla tahmin edilen yeni örnek ekran görüntüsü](media/tutorial-machine-learned-entity/confirm-entity-prediction-for-new-example-utterance-added.png)

    Bu noktada, varlığı yeni bir örnek içinde bulabildiğinden makine öğrenimi varlığı çalışmaktadır. Örnek yazarken, varlık doğru şekilde tahmin edilmezse, varlığı ve alt varlıkları etiketleyin. Varlık doğru bir şekilde tahmin edildiğinde, tahminleri onaylamanız emin olun.


## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Uygulamaya varlık değişikliklerini uygulamak için uygulamayı eğitme

Uygulamayı bu yeni utterance ile eğitede **eğitme** ' yi seçin.

Bu noktada, sıralamada ayıklanabilen bazı ayrıntılar (boyut, miktar ve toplam sipariş metni) vardır. `Order`Varlıkla, pizza toppings, Crust türü ve yan emirler gibi daha ayrıntılı bir şekilde iyileştirilmesi vardır. Bunların her biri varlığın alt varlıkları olarak oluşturulmalıdır `Order` .

## <a name="test-the-app-to-validate-the-changes"></a>Değişiklikleri doğrulamak için uygulamayı test etme

Etkileşimli **Test** panelini kullanarak uygulamayı test edin. Bu işlem, yeni bir söylenişi girmenizi sağlar ve ardından, etkin, eğitilen uygulamanın ne kadar iyi çalıştığını görmek için tahmin sonuçlarını görüntüleyebilir. Amaç tahmini oldukça emin olmalıdır (%60 üzerinde) ve varlık ayıklama en az `Order` varlığı almalıdır. Bu birkaç işlem, her durumu işlemek için yeterli olmadığından, sipariş varlığının ayrıntıları eksik olabilir.

1. Üst gezinti bölmesinde **Test**'i seçin.
1. Söylenişi `2 small cheese pizzas for pickup` ' i girip ENTER ' u seçin. Etkin Model %60 güvenilirlikle doğru amacı tahmin eden tahmini.


1. Varlık tahminlerini görmek için **İnceleme** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Etkileşimli test panelinde varlık tahminlerinin görüntüleneceği kısmi ekran görüntüsü.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance-and-entity-predictions.png)

<a name="publish-with-sentiment-analysis"></a>

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Uygulamayı HTTP uç noktasından erişmek üzere yayımlayın

Bir sohbet robotunda veya başka bir istemci uygulamasında bir LUIS tahmini almak için uygulamayı uç noktasında yayımlamanız gerekir.

1. Sağ üst gezinti bölmesinde **Yayımla** ' yı seçin.

    ![En sağ menüdeki LUSıS uç noktaya Yayımla düğmesinin ekran görüntüsü](./media/howto-publish/publish-button.png)

1. **Üretim** yuvasını seçin, ardından **Ayarları Değiştir**' i seçin, **yaklaşım Analizi**seçin ve **bitti**' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Uç noktada bir HALSıS yayımlama ekran görüntüsü](./media/tutorial-machine-learned-entity/publish-with-sentiment-analysis.png)

1. **Azure kaynakları** sayfasına gitmek için bildirimde **uç nokta URL 'lerine erişin** bağlantısına tıklayın. Uç nokta URL 'Leri **örnek sorgu**olarak listelenir.

## <a name="get-intent-and-entity-prediction-from-http-endpoint"></a>HTTP uç noktasından amacı ve varlık tahminini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğunda URL 'nin sonuna gidin ve _YOUR_QUERY_HERE_ , etkileşimli test paneline girdiğiniz sorguyla değiştirin.

    `2 small cheese pizzas for pickup`

    Son sorgu dizesi parametresi ifade **s**orgusu olan `query` öğesidir.

    ```json
    {
        "query": "2 small cheese pizzas for pickup",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.7812769
                },
                "None": {
                    "score": 0.0314020254
                },
                "Confirm": {
                    "score": 0.009299271
                },
                "Greeting": {
                    "score": 0.007551549
                }
            },
            "entities": {
                "Order": [
                    {
                        "Size": [
                            "small"
                        ],
                        "Quantity": [
                            2
                        ]
                    }
                ]
            }
        },
        "sentimentAnalysis":{
            "label":"neutral",
            "score":0.98
       }
    }
    ```


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Öğretici-amaçlar](luis-quickstart-intents-only.md)
* [Kavram varlıkları](luis-concept-entity-types.md) kavramsal bilgileri
* [Kavram-Özellikler](luis-concept-feature.md) kavramsal bilgileri
* [Eğitme](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama, bir kullanıcının utterliği amacını bulmak ve bu noktadan ayrıntıları ayıklamak için bir makine öğrenimi varlığı kullanır. Machine Learning varlığının kullanılması, varlığın ayrıntılarını parçalara ayırmayı sağlar.

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş keyphrase varlığı ekleme](luis-quickstart-intent-and-key-phrase.md)
