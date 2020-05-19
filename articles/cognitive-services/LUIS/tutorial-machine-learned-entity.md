---
title: 'Öğretici: makine tarafından öğrenilen varlık-LUSıS ile yapılandırılmış verileri Ayıkla'
description: Makine tarafından öğrenilen varlığı kullanarak bir utterden yapılandırılmış verileri ayıklayın. Ayıklama doğruluğunu artırmak için, özelliklerle alt varlıklar ekleyin.
ms.topic: tutorial
ms.date: 05/08/2020
ms.openlocfilehash: d1bc8fc6aac52e264cb4352ca05f9df45ccfc50e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588879"
---
# <a name="tutorial-extract-structured-data-from-user-utterance-with-machine-learned-entities-in-language-understanding-luis"></a>Öğretici: Language Understanding (LUSıS) içindeki makine tarafından öğrenilen varlıklarla yapılandırılmış verileri Kullanıcı utkiden Ayıkla

Bu öğreticide, makine tarafından öğrenilen varlığı kullanarak bir utterden yapılandırılmış verileri ayıklayın.

Makine tarafından öğrenilen varlık, [özelliklerle](luis-concept-feature.md)alt varlık varlıkları sağlayarak [model ayrıştırma kavramını](luis-concept-model.md#v3-authoring-model-decomposition) destekler.

**Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:**

> [!div class="checklist"]
> * Örnek uygulamayı içeri aktar
> * Makine tarafından öğrenilen varlık Ekle
> * Alt varlık ve Özellik Ekle
> * Eğitim, test, uygulama yayımlama
> * Uç noktadan varlık tahminini al

[!INCLUDE [LUIS Free account](includes/quickstart-tutorial-use-free-starter-key.md)]


## <a name="why-use-a-machine-learned-entity"></a>Makine tarafından öğrenilen bir varlık neden kullanılmalıdır?

Bu öğretici, bir kullanıcının utanından veri ayıklamak için makine tarafından öğrenilen bir varlık ekler.

Varlık, ayıklananlık içinden Ayıklanacak verileri tanımlar. Bu, verileri bir ad, bir tür (mümkünse), belirsizlik varsa verilerin herhangi bir çözümlemesini ve verileri oluşturan tam metni içerir.

Verileri tanımlamak için şunları yapmanız gerekir:
* Varlığı oluşturma
* Metni, varlığı temsil eden örnek olarak etiketleyin. Bu etiketlenmiş örnekler, LUID 'in varlığın ne olduğunu ve bir utterde nerede bulabileceklerini öğretin.

## <a name="entity-decomposability-is-important"></a>Varlık ayırıcı, önemli

Varlık ayırıcı, hem amaç tahmini hem de varlıkla veri ayıklama için önemlidir.

Veri ayıklama için başlangıç ve en üst düzey varlık olan makine tarafından öğrenilen bir varlık ile başlayın. Sonra varlığı alt varlıklara parçalara ayırın.

Uygulamanıza başladığınızda varlığınızın ne kadar ayrıntılı olmasını bildiğinize karşın, en iyi uygulama makine tarafından öğrenilen bir varlıkla başlamak ve sonra uygulamanız için alt varlıklarla birlikte oluşturmak olacaktır.

Bu öğreticide, pizza uygulaması için bir siparişi temsil eden makine tarafından öğrenilen bir varlık oluşturacaksınız. Varlık, siparişle ilgili metnin ayıklanmasına, boyutun ve miktarın dışına çıkaracaktır.

Bir utterlik, `Please deliver one large cheese pizza to me` `one large cheese pizza` sıra olarak ayıklanmalı `1` ve ayrıca miktar ve boyut için de ayıklanmalıdır `large` .

## <a name="download-json-file-for-app"></a>Uygulama için JSON dosyasını indirin

[Uygulama json dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-intents-only.json)indirip kaydedin.

## <a name="import-json-file-for-app"></a>Uygulama için JSON dosyasını içeri aktar

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="label-text-as-entities-in-example-utterances"></a>Metni örnek söyleyde varlıklar olarak etiketle

Bir pizza sırası hakkındaki ayrıntıları ayıklamak için, en üst düzey, makine tarafından öğrenilen bir `Order` varlık oluşturun.

1. **Amaçlar** sayfasında **orderpizza** hedefini seçin.

1. Örnek söyleyenlik listesinde, aşağıdaki utterance ' ı seçin.

    |Sipariş örneği|
    |--|
    |`pickup a cheddar cheese pizza large with extra anchovies`|

    En solundaki (#1) metinden hemen önce seçmeye başlayın `pickup` , sonra sağ taraftaki metnin hemen ötesine geçin `anchovies` (#2-bu etiketleme işlemini sonlandırır). Bir açılır menü görünür. Açılır kutuya varlığın adını `Order` (#3) olarak girin. Sonra `Order - Create new entity` listeden (#4) öğesini seçin.

    ![Metnin başlangıç ve bitiş sırası için etiketi](media/tutorial-machine-learned-entity/mark-complete-order.png)

    > [!NOTE]
    > Bir varlık her zaman tüm söylik olmaz. Bu özel durumda, `pickup` siparişin nasıl alınacağını gösterir. Kavramsal bir perspektiften, `pickup` sıralama için etiketlenmiş varlığın bir parçası olmalıdır.

1. **Varlık türü seçin** kutusunda, **Yapı Ekle** ' yi seçin ve ardından **İleri**' yi seçin. Boyut ve miktar gibi alt varlıklar eklemek için yapı gereklidir.

    ![Varlığa yapı Ekle](media/tutorial-machine-learned-entity/add-structure-to-entity.png)

1. Makine tarafından **öğrenilen varlık oluştur** kutusunda, **Yapı** kutusunda `Size` ENTER ' ı seçin.
1. Bir **özellik**eklemek Için, `+` **Özellikler** alanında öğesini seçin ve ardından **Yeni tümcecik listesi oluştur**' u seçin.

1. **Yeni tümcecik oluştur liste** kutusuna adı girin, `SizeFeature` ardından şu değerleri girin: `small` , `medium` , ve `large` . **Öneriler** kutusu dolduğunda, `extra large` ve ' yi seçin `xl` . Yeni tümcecik listesini oluşturmak için **bitti** ' yi seçin.

    Bu tümcecik listesi özelliği, `Size` alt varlığın boyutla ilgili sözcükleri örnek sözcükle birlikte girerek bulmasına yardımcı olur. Bu listenin her kelime içermesi gerekmez, ancak boyutu göstermek için beklenen sözcükleri içermesi gerekir.

    ![Alt varlık boyutu için bir özellik oluşturun](media/tutorial-machine-learned-entity/size-entity-size-descriptor-phrase-list.png)

1. Makine tarafından **öğrenilen varlık oluştur** penceresinde, alt varlık oluşturmayı bitirmeden **Oluştur** ' u seçin `Size` .

    Varlığı `Order` olan varlık `Size` oluşturulur, ancak yalnızca `Order` varlık utterance 'e uygulandı. `Size`Örnek utüterde varlık metnini etiketetmeniz gerekir.

1. Aynı örnek utterde, alt varlık **boyutunu** `large` etiketleyerek açılan listeden **Boyut** varlığını seçerek sözcüğü seçin.

    ![Boyut varlığını utterance 'teki metin için etiketleyin.](media/tutorial-machine-learned-entity/mark-and-create-size-entity.png)

    Metni _açıkça_ etiketlediğiniz için hem etiketleme hem de tahmin eşleşmesi nedeniyle satır metnin altında kalındır.

1. `Order`Kalan titerlerdeki varlığı boyut varlığıyla birlikte etiketleyin. Metindeki köşeli ayraç etiketli `Order` varlığı ve `Size` içindeki varlığı gösterir.

    |Sipariş örneği|
    |--|
    |`can i get [a pepperoni pizza and a can of coke] please`|
    |`can i get [a [small] pizza with onions peppers and olives]`|
    |`[delivery for a [small] pepperoni pizza]`|
    |`i need [2 [large] cheese pizzas 6 [large] pepperoni pizzas and 1 [large] supreme pizza]`|

    ![Tüm kalan örnekte varlık ve alt varlıklar oluşturun.](media/tutorial-machine-learned-entity/entity-subentity-labeled-not-trained.png)

    > [!CAUTION]
    > Tek bir pizza 'nin bir adı gibi kapsanan verileri nasıl değerlendirirsiniz `a` ? Veya, `pickup` `delivery` pizza 'nin nerede beklendiğine işaret ediyor mu? Ya da küçük veya büyük boyutlu varsayılan boyutunu belirten bir boyut eksikliği mı var? Açık Veri işlemeyi, LUTO 'ın yanı sıra, istemci uygulamasındaki iş kurallarınızın bir parçası olarak kabul etmeyi düşünün.

1. Uygulamayı eğiteiçin **eğitme**' yi seçin. Eğitim, etkin modele yeni varlıklar ve etiketli utterler gibi değişiklikleri uygular.

1. Eğitimin ardından, makinenin makine tarafından öğrenilen varlığı ne kadar iyi anladığını görmek için amaca uygun yeni bir örnek ekleyin.

    |Sipariş örneği|
    |--|
    |`pickup XL meat lovers pizza`|

    Genel en üst varlık `Order` etiketlidir ve `Size` alt varlık de noktalı çizgilerle etiketlidir.

    ![Varlıkla öngörülen yeni örnek söylenişi](media/tutorial-machine-learned-entity/new-example-utterance-predicted-with-entity.png)

    Noktalı çizgi, tahminini gösterir.

1. Tahmini etiketli bir varlık olarak değiştirmek için satırı seçin ve ardından **varlık tahminlerini Onayla**' yı seçin.

    ![Varlık tahminini Onayla ' yı seçerek tahmin kabul edin.](media/tutorial-machine-learned-entity/confirm-entity-prediction-for-new-example-utterance.png)

    Bu noktada, varlığı yeni bir örnek içinde bulabildiğinden makine tarafından öğrenilen varlık çalışır. Örnek yazarken, varlık doğru şekilde tahmin edilmezse, varlığı ve alt varlıkları etiketleyin. Varlık doğru bir şekilde tahmin edildiğinde, tahminleri onaylamanız emin olun.


<a name="create-subcomponent-entity-with-constraint-to-help-extract-data"></a>

## <a name="add-subentity-with-feature-of-prebuilt-entity"></a>Önceden oluşturulmuş varlık özelliğine sahip alt varlık ekleme

Sipariş bilgileri ayrıca, kaç tane Pizzas gibi bir öğenin sırada olduğunu de içermelidir. Bu verileri ayıklamak için, yeni bir makineyle öğrenilen alt varlık ' a eklenmelidir `Order` ve alt varlık önceden oluşturulmuş bir sayının gerekli bir özelliğine ihtiyaç duyuyor. Önceden oluşturulmuş bir varlığın bir özelliğini önceden oluşturulmuş bir sayıya kullanarak, varlık, metnin bir basamak, ya da metin olup olmadığını bulur ve ayıklar `2` `two` .

## <a name="add-prebuilt-number-entity-to-app"></a>Uygulamaya önceden oluşturulmuş sayı varlığı ekleme
Sipariş bilgileri aynı zamanda kaç tane öğe olduğunu (kaç tane Pizzas gibi) de içermelidir. Bu verileri ayıklamak için, makineye öğrenilen yeni bir alt bileşenin eklenmesi gerekir `Order` ve bu bileşenin önceden oluşturulmuş bir sayının gerekli bir özelliği olması gerekir. Varlığı önceden oluşturulmuş bir sayıyla karşılaştırarak, metin bir basamak, veya metin olan sayıları bulur ve ayıklar `2` `two` .

Uygulamaya önceden oluşturulmuş sayı varlığı ekleyerek başlayın.

1. Sol menüden **varlıklar** ' ı seçin ve ardından **+ önceden oluşturulmuş varlık Ekle**' yi seçin.

1. **Önceden oluşturulmuş varlıklar Ekle** kutusunda, **numarayı** arayıp seçin ve **bitti**' yi seçin.

    ![Önceden oluşturulmuş varlık Ekle](media/tutorial-machine-learned-entity/add-prebuilt-entity-as-constraint-to-quantity-subcomponent.png)

    Önceden oluşturulmuş varlık uygulamaya eklenir, ancak henüz bir özellik değildir.

## <a name="create-subentity-entity-with-required-feature-to-help-extract-data"></a>Veri ayıklamaya yardımcı olmak için gerekli özelliği olan alt varlık varlığı oluştur

`Order`Varlığın `Quantity` siparişte kaç tane öğe olduğunu belirleyebilmek için bir alt varlığa sahip olması gerekir. Çıkarılan verilerin istemci uygulaması adına göre hemen kullanılabilmesi için, miktarın önceden oluşturulmuş bir sayının gerekli bir özelliği kullanması gerekir.

Gerekli bir özellik, tam eşleşme (bir liste varlığı gibi) veya normal ifadeler (örneğin, bir normal ifade varlığı veya önceden oluşturulmuş bir varlık gibi) ile bir metin eşleşmesi olarak uygulanır.

Makine tarafından öğrenilen bir varlığı özellik olarak kullanarak, yalnızca eşleşen metin ayıklanır.

1. **Varlıklar** ' ı seçin ve ardından `Order` varlığı seçin.
1. Varlığa yeni alt varlık eklemek için **+ varlık Ekle** ' yi seçin ve `Quantity` ardından ENTER ' u seçin `Order` .
1. Başarı bildiriminden sonra, **Gelişmiş Seçenekler**' de kısıtlama kurşun kalem ' i seçin.
1. Açılan listede, önceden oluşturulmuş numarayı seçin.

    ![Önceden oluşturulmuş sayıyla kısıtlama olarak miktar varlığı oluşturun.](media/tutorial-machine-learned-entity/create-constraint-from-prebuilt-number.png)

    `Quantity`Varlık, önceden oluşturulmuş sayı varlığıyla eşleştiğinde uygulanır.

    Gerekli özelliği olan varlık oluşturulur ancak örnek utetlerini henüz uygulanmaz.

    > [!NOTE]
    > Alt varlık, 5 düzeye kadar bir alt varlık içinde iç içe olabilir. Bu makalede gösterilmediğinde Portal ve API 'den erişilebilir.

## <a name="label-example-utterance-to-teach-luis-about-the-entity"></a>Bir varlık hakkında LUSıS öğretmek için etiket örneği

1. Sol taraftaki gezinmede **amaçları** seçin, sonra **orderpizza** hedefini seçin. Aşağıdaki söyleyenlerdeki üç sayı etiketlidir ancak varlık satırının altında görsel olarak bulunur `Order` . Bu alt düzey, varlıkların bulunduğu ancak varlığın bir parçası olarak kabul edildiği anlamına gelir `Order` .

    ![Önceden oluşturulmuş sayı bulundu ancak sipariş varlığı henüz kabul edilmez.](media/tutorial-machine-learned-entity/prebuilt-number-not-part-of-order-entity.png)

1. `Quantity` `2` Örnekte, listeden öğesini seçerek, sayıları varlıkla etiketleyin `Quantity` . `6`Aynı örnekte ve ile etiketleyin `1` .

    ![Metin olarak miktar tüzel kişiliği etiketi.](media/tutorial-machine-learned-entity/mark-example-utterance-with-quantity-entity.png)

## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Uygulamaya varlık değişikliklerini uygulamak için uygulamayı eğitme

Uygulamayı bu yeni dıklarla eğitmek için **eğitme** ' yi seçin. Eğitim sonrasında, `Quantity` alt varlık varlıkta doğru şekilde tahmin edilir `Order` . Bu doğru tahmin, katı bir çizgi ile belirtilmiştir.

![Uygulamayı eğitme örnek, örnekleri gözden geçirin.](media/tutorial-machine-learned-entity/trained-example-utterances.png)

Bu noktada, sıralamada ayıklanabilen bazı ayrıntılar (boyut, miktar ve toplam sipariş metni) vardır. `Order`Varlıkla, pizza toppings, Crust türü ve yan emirler gibi daha ayrıntılı bir şekilde iyileştirilmesi vardır. Bunların her biri varlığın alt varlıkları olarak oluşturulmalıdır `Order` .

## <a name="test-the-app-to-validate-the-changes"></a>Değişiklikleri doğrulamak için uygulamayı test etme

Etkileşimli **Test** panelini kullanarak uygulamayı test edin. Bu işlem, etkin ve eğitilen uygulamanın ne kadar iyi çalıştığını görmek için yeni bir söylik girmenize ve tahmin sonuçlarını görüntülemenize imkan tanır. Amaç tahmini oldukça emin olmalıdır (%70 üzerinde) ve varlık ayıklama en az `Order` varlığı almalıdır. 5 utlaslar her durumda işlenecek kadar, sipariş varlığının ayrıntıları eksik olabilir.

1. Üst gezinti bölmesinde **Test**'i seçin.
1. Söylenişi `deliver a medium veggie pizza` ' i girip ENTER ' u seçin. Etkin Model %70 güvenilirlikle doğru amacı tahmin eden tahmini.

    ![Amacı test etmek için yeni bir söylenişi girin.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance.png)

1. Varlık tahminlerini görmek için **İnceleme** ' yi seçin.

    ![Etkileşimli test panelinde varlık tahminlerini görüntüleyin.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance-and-entity-predictions.png)

    Boyut doğru şekilde tanımlandı. Amaç 'daki örnek, `OrderPizza` `medium` boyut olarak bir örnektir ancak `SizeFeature` Orta içeren bir tümcecik listesinin bir özelliğini kullanın.

    Miktar doğru şekilde tahmin edilmez. Bu işlemi, lusıs tahminiyle bir boyut döndürülmezse, istemci uygulamanızda, boyutu bir (1) olarak en az olacak şekilde düzeltemedi.

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Uygulamayı HTTP uç noktasından erişmek üzere yayımlayın

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-http-endpoint"></a>HTTP uç noktasından amacı ve varlık tahminini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğunda URL 'nin sonuna gidin ve _YOUR_QUERY_HERE_ , etkileşimli test paneline girdiğiniz sorguyla değiştirin.

    `deliver a medium veggie pizza`

    Son sorgu dizesi parametresi ifade **s**orgusu olan `query` öğesidir.

    ```json
    {
        "query": "deliver a medium veggie pizza",
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
                            "medium"
                        ],
                        "$instance": {
                            "Size": [
                                {
                                    "type": "Size",
                                    "text": "medium",
                                    "startIndex": 10,
                                    "length": 6,
                                    "score": 0.9955588,
                                    "modelTypeId": 1,
                                    "modelType": "Entity Extractor",
                                    "recognitionSources": [
                                        "model"
                                    ]
                                }
                            ]
                        }
                    }
                ],
                "$instance": {
                    "Order": [
                        {
                            "type": "Order",
                            "text": "a medium veggie pizza",
                            "startIndex": 8,
                            "length": 21,
                            "score": 0.7983857,
                            "modelTypeId": 1,
                            "modelType": "Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
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

Bu öğreticide, uygulama, bir kullanıcının utterliği amacını bulmak ve bu noktadan ayrıntıları ayıklamak için makine tarafından öğrenilen bir varlık kullanır. Makine tarafından öğrenilen varlığın kullanılması, varlığın ayrıntılarını parçalara ayırmayı sağlar.

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş keyphrase varlığı ekleme](luis-quickstart-intent-and-key-phrase.md)
