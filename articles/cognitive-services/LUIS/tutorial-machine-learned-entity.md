---
title: 'Öğretici: makine öğrenilen varlık ile yapılandırılmış veri ayıklamak - LUIS'
titleSuffix: Azure Cognitive Services
description: Makinede öğrenilen varlığı kullanarak bir söyleyişten yapılandırılmış veri ayıklayın. Çıkarma doğruluğunu artırmak için, tanımlayıcılar ve kısıtlamalar içeren alt bileşenler ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: diberry
ms.openlocfilehash: e1709a5e86c8fed8d7f724ad1b105bd02df9fa56
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75381775"
---
# <a name="tutorial-extract-structured-data-from-user-utterance-with-machine-learned-entities-in-language-understanding-luis"></a>Öğretici: Dil Anlama (LUIS) makine öğrenilen varlıklar ile kullanıcı söyleyerek yapılandırılmış veri ayıklayın

Bu öğreticide, makinede öğrenilen varlığı kullanarak bir söyleyişten yapılandırılmış verileri ayıklayın.

Makinede öğrenilen varlık, alt bileşen varlıklarına tanımlayıcılarını ve kısıtlamalarını sağlayarak [model ayrıştırma kavramını](luis-concept-model.md#v3-authoring-model-decomposition) destekler.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Örnek uygulamayı içe aktarma
> * Makinede öğrenilen varlık ekleme
> * Alt bileşen ekleme
> * Alt bileşenin tanımlayıcısı ekleme
> * Alt bileşenin kısıtlaması ekleme
> * Uygulamayı eğitme
> * Test uygulaması
> * Uygulama yayımlama
> * Bitiş noktasından varlık tahmini alma

[!INCLUDE [LUIS Free account](includes/quickstart-tutorial-use-free-starter-key.md)]


## <a name="why-use-a-machine-learned-entity"></a>Neden makineden öğrenilen bir varlık kullanıyorsun?

Bu öğretici, bir söyleyişten veri ayıklamak için makineden öğrenilen bir varlık ekler.

Bir varlığın amacı ayıklamak için verileri tanımlamaktır. Buna, verilere bir ad, bir tür (mümkünse), belirsizlik varsa verilerin herhangi bir çözünürlüğü ve verileri oluşturan tam metin verilmesi dahildir.

Varlığı tanımlamak için, varlığı oluşturmanız ve ardından örnek söyleyişde varlığı temsil eden metni etiketlemeniz gerekir. Bu etiketli örnekler LUIS'e varlığın ne olduğunu ve bir söyleyiş içinde nerede bulunabileceğini öğretir.

## <a name="entity-decomposability-is-important"></a>Varlık decomposability önemlidir

Varlık ayrıştırılabilirliği hem niyet tahmini hem de veri ayıklama için önemlidir.

Veri ayıklama için başlangıç ve üst düzey varlık olan makine den öğrenilen bir varlıkla başlayın. Ardından varlığı istemci uygulamasının gerektirdiği parçalara ayırın.

Uygulamanızı başlattığınızda varlığınızı ne kadar ayrıntılı istediğinizi bilmeseniz de, en iyi uygulama makinede öğrenilen bir varlıkla başlamak, ardından uygulamanız olgunlaştıkça alt bileşenlerle ayrışmaktır.

Pratik açıdan, bir pizza uygulaması için bir sipariş temsil etmek için bir makine öğrenilen varlık oluşturacaktır. Sipariş, siparişi tam olarak yerine getirmek için gerekli olan tüm parçalara sahip olmalıdır. Başlamak için, varlık boyutu ve miktarı çekerek, sipariş ile ilgili metin ayıklanır.

Için `Please deliver one large cheese pizza to me` bir söyleyiş `one large cheese pizza` sipariş olarak ayıklamak `1` `large`gerekir, sonra da ayıklamak ve .

Topingler veya kabuk için alt bileşenler oluşturma gibi ekleyebilirsiniz daha fazla çürüme vardır. Bu eğitimden sonra, bu alt bileşenleri mevcut `Order` varlığınıza ekleyerek kendinizi güvende hissetmeniz gerekir.

## <a name="import-example-json-to-begin-app"></a>Uygulama başlatmak için alma örneği .json

1.  Uygulamayı indirin ve kaydedin [JSON dosyasını.](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-intents-only.json)

[!INCLUDE [Import app steps](includes/import-app-steps.md)]

## <a name="label-text-as-entities-in-example-utterances"></a>Metni örnek sözlerle varlıklar olarak etiketleme

Pizza siparişi hakkında ayrıntıları ayıklamak için, en `Order` üst düzeyde, makineden öğrenilen bir varlık oluşturun.

1. **Niyetler** sayfasında **OrderPizza** amacını seçin.

1. Örnek söyleyerek ler listesinde aşağıdaki söyleyiyi seçin.

    |Sipariş örneği söyleyiş|
    |--|
    |`pickup a cheddar cheese pizza large with extra anchovies`|

    (#1'ın en sol metninden `pickup` hemen önce seçim yapmaya başlayın, ardından `anchovies` en çok sağdaki metnin ötesine geçin (#2 - bu etiketleme işlemini sona erdiriyor). Açılır menü görüntülenir. Açılan kutuya varlığın adını `Order` (#3 olarak girin. Ardından `Order - Create new entity` listeden (#4) seçin.

    ![Tam sipariş için metnin başlayıp bitişini etiketleme](media/tutorial-machine-learned-entity/mark-complete-order.png)

    > [!NOTE]
    > Bir varlık her zaman tüm söyleyiş olmayacak. Bu özel durumda, `pickup` siparişin nasıl alınacağı gösterilir. Kavramsal bir bakış `pickup` açısıyla, sipariş için etiketli varlığın bir parçası olmalıdır.

1. Varlık türü kutusu **seç'te** **Yapı Ekle'yi** seçin ve **ardından İleri'yi**seçin. Boyut ve miktar gibi alt bileşenler eklemek için yapı gereklidir.

    ![Varlığa yapı ekleme](media/tutorial-machine-learned-entity/add-structure-to-entity.png)

1. Makine **öğrenilen varlık** kutusunda Yapı **kutusunda** ekle `Size` ve enter'u seçin.
1. Tanımlayıcı eklemek **descriptor**için, Boyut `+` alanı **için Tanımlayıcılar'ı** seçin ve ardından **yeni ifade listesi oluştur'u**seçin.

1. Yeni **ifade listesi açıklayıcı sıcadım** kutusuna, `SizeDescriptor` adı girin `small` `medium`sonra `large`değerlerini girin: , , ve . **Öneriler** kutusu dolduğunda `extra large`, ve `xl`. Yeni tümcecik listesini oluşturmak için **Bitti'yi** seçin.

    Bu ifade listesi tanımlayıcısı, alt bileşenin `Size` örnek sözcükler sunarak boyutla ilgili sözcükleri bulmasına yardımcı olur. Bu listenin her boyuttasözcüğü içermesi gerekmez, ancak boyutu göstermesi beklenen sözcükleri içermelidir.

    ![Boyut alt bileşeni için bir tanımlayıcı oluşturma](media/tutorial-machine-learned-entity/size-entity-size-descriptor-phrase-list.png)

1. Makine **öğrenilen varlık** penceresinde, `Size` alt bileşeni oluşturmak için **Oluştur'u** seçin.

    Bileşeni `Order` olan `Size` varlık oluşturulur, ancak `Order` yalnızca varlık söyleyeme uygulanmıştır. Varlık metnini `Size` örnek söyleyiş olarak etiketlemeniz gerekir.

1. Aynı örnekte, sözcüğü **Size** `large` seçerek ve ardından açılan listeden **Boyut** varlığını seçerek Boyut alt bileşenini etiketle.

    ![Metin için boyut varlığını söyleyiş olarak etiketlendi.](media/tutorial-machine-learned-entity/mark-and-create-size-entity.png)

    Metni açıkça etiketlediğiniz için hem etiketleme hem de tahmin eşleşip metnin altında düz bir çizgi bulunur.

1. `Order` Varlığı, büyük varlıkla birlikte kalan tümlonlarla etiketle. Metindeki kare ayraçlar, etiketli `Order` varlığı `Size` ve içindeki varlığı gösterir.

    |Sipariş örneği söyleyinmeleri|
    |--|
    |`can i get [a pepperoni pizza and a can of coke] please`|
    |`can i get [a [small] pizza with onions peppers and olives]`|
    |`[delivery for a [small] pepperoni pizza]`|
    |`i need [2 [large] cheese pizzas 6 [large] pepperoni pizzas and 1 [large] supreme pizza]`|

    ![Kalan tüm örnek tüm söyleyişlerde varlık ve alt bileşenler yapın.](media/tutorial-machine-learned-entity/entity-subentity-labeled-not-trained.png)

    > [!CAUTION]
    > Tek bir pizzayı ima eden `a` mektup gibi zımni verileri nasıl ele alasınız? Ya da `pickup` pizzanın `delivery` nerede beklendiğini niçin ve eksikliği? Veya küçük veya büyük varsayılan boyutunu belirtmek için bir boyut eksikliği? Luis yerine veya LUIS'e ek olarak istemci uygulamasında zımni veri işlemeyi iş kurallarınızın bir parçası olarak ele almayı düşünün.

1. Uygulamayı eğitmek için **Train'i**seçin. Eğitim, etkin modele yeni varlıklar ve etiketli sözcük ler gibi değişiklikleri uygular.

1. Eğitimden sonra, LUIS'in makineden öğrenilen varlığı ne kadar iyi anladığını görmek için niyete yeni bir örnek söyleyiş ekleyin.

    |Sipariş örneği söyleyiş|
    |--|
    |`pickup XL meat lovers pizza`|

    Genel üst varlık, `Order` etiketlenir `Size` ve alt bileşen de noktalı çizgilerle etiketlenir. Bu başarılı bir tahmindir.

    ![Varlıkla öngörülen yeni örnek](media/tutorial-machine-learned-entity/new-example-utterance-predicted-with-entity.png)

    Noktalı çizgi öngörüyü gösterir.

1. Öngörüyü etiketli bir varlığa dönüştürmek için satırı seçin ve ardından **varlık tahminlerini doğrula'yı**seçin.

    ![Varlık tahminini onayla'yı seçerek tahmini kabul edin.](media/tutorial-machine-learned-entity/confirm-entity-prediction-for-new-example-utterance.png)

    Bu noktada, makinede öğrenilen varlık, varlığı yeni bir örnek söyleyiş içinde bulabildiği için çalışır. Örnek sözcükler eklediğinizde, varlık doğru tahmin edilemediyse, varlığı ve alt bileşenleri etiketlayın. Varlık doğru tahmin edilirse, tahminleri doğruladıklarından emin olun.

## <a name="add-prebuilt-number-to-help-extract-data"></a>Veri ayıklama yardımcı olmak için önceden oluşturulmuş sayı ekleme

Sipariş bilgileri, siparişte kaç tane madde olduğu gibi kaç tane pizza olduğunu da içermelidir. Bu verileri ayıklamak için, makineden öğrenilen yeni `Order` bir alt bileşenin eklenmesi gerekir ve bu bileşenin önceden oluşturulmuş bir sayının kısıtlamasına ihtiyacı vardır. Varlığı önceden oluşturulmuş bir sayıyla zorlayarak, varlık metnin basamak `2`veya metin olup olmadığını `two`bulur ve ayıklar.

Uygulamaya önceden oluşturulmuş sayı varlığını ekleyerek başlayın.

1. Sol menüden **Varlıklar'ı** seçin, ardından **+ Önceden oluşturulmuş varlık ekle'yi**seçin.

1. Önceden **oluşturulmuş varlıklar ekle** kutusunda, arama ve **sayı** seçin sonra **Bitti**seçin.

    ![Önceden oluşturulmuş varlık ekleme](media/tutorial-machine-learned-entity/add-prebuilt-entity-as-constraint-to-quantity-subcomponent.png)

    Önceden oluşturulmuş varlık uygulamaya eklenir, ancak henüz bir kısıtlama değildir.

## <a name="create-subcomponent-entity-with-constraint-to-help-extract-data"></a>Veri ayıklamanıza yardımcı olmak için kısıtlanmış alt bileşen varlığı oluşturma

Bir `Order` öğenin `Quantity` kaç tanesinin sırada olduğunu belirlemek için bir alt bileşene sahip olması gerekir. Alınan verilerin istemci uygulaması tarafından hemen kullanılabilir olması için miktar bir sayıyla sınırlandırılmalıdır.

Kısıtlama, metin eşleşmesi olarak, tam eşleme (liste varlığı gibi) veya normal ifadeler (normal ifade varlığı veya önceden oluşturulmuş bir varlık gibi) aracılığıyla uygulanır.

Bir kısıtlama kullanılarak, yalnızca bu kısıtlamayla eşleşen metin ayıklanır.

1. **Varlıklar'ı** seçin `Order` ve ardından varlığı seçin.
1. + **Bileşen Ekle'yi** `Quantity` seçin ve ardından uygulamaya yeni varlığı eklemek için Adını girin'i seçin.
1. Başarı bildiriminden sonra `Quantity` alt bileşeni seçin ve ardından Kısıtlama kalemini seçin.
1. Açılan listede önceden oluşturulmuş numarayı seçin.

    ![Kısıtlama olarak önceden oluşturulmuş sayıile miktar varlığı oluşturun.](media/tutorial-machine-learned-entity/create-constraint-from-prebuilt-number.png)

    Varlık, `Quantity` yalnızca önceden oluşturulmuş sayı varlığıyla eşleşen bir metin bulunursa uygulanır.

    Kısıtlamalı varlık oluşturulur, ancak henüz örnek sözlemelere uygulanmaz.

    > [!NOTE]
    > Bir alt bileşen, 5 düzeye kadar bir alt bileşen içinde iç içe olabilir. Bu makalede gösterilmese de, portal ve API'den edinilebilir.

## <a name="label-example-utterance-to-teach-luis-about-the-entity"></a>Varlık hakkında LUIS öğretmek için etiket örneği söyleyiş

1. Sol daki gezintiden **Niyetler'i** seçin ve **ardından OrderPizza** amacını seçin. Aşağıdaki ifadelerdeki üç sayı etiketlenir, ancak görsel olarak `Order` varlık çizgisinin altındadır. Bu alt düzey, varlıkların bulunduğu, ancak varlığın `Order` dışında düşünülmemesi anlamına gelir.

    ![Önceden oluşturulmuş sayı bulunur, ancak henüz Sipariş varlığı dışında düşünülmez.](media/tutorial-machine-learned-entity/prebuilt-number-not-part-of-order-entity.png)

1. Listeden seçerek `Quantity` `2` `Quantity` örnekteki söyleyiyi seçerek sayıları varlıkla etiketlendirin. Etiket `6` ve `1` aynı örnek söyleyiş.

    ![Metni miktar varlığıyla etiketle.](media/tutorial-machine-learned-entity/mark-example-utterance-with-quantity-entity.png)

## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Varlık değişikliklerini uygulamaya uygulamak için uygulamayı eğitin

Bu yeni söyleyişlerle uygulamayı eğitmek için **Train'i** seçin.

![Uygulamayı eğitin ve örnek söyleyişleri inceleyin.](media/tutorial-machine-learned-entity/trained-example-utterances.png)

Bu noktada, sipariş ayıklanabilir bazı ayrıntıları (boyut, miktar ve toplam sipariş metni) vardır. Pizza topingler, kabuk `Order` türü ve yan siparişler gibi varlığın daha fazla rafine olduğunu. Bunların her biri varlığın `Order` alt bileşenleri olarak oluşturulmalıdır.

## <a name="test-the-app-to-validate-the-changes"></a>Değişiklikleri doğrulamak için uygulamayı test edin

İnteraktif **Test** panelini kullanarak uygulamayı test edin. Bu işlem, etkin ve eğitimli uygulamanın ne kadar iyi çalıştığını görmek için yeni bir sözcük girmenizi ve tahmin sonuçlarını görüntülemenizi sağlar. Niyet tahmini oldukça emin olmalıdır (%70'in üzerinde) ve varlık çıkarma en azından `Order` varlık almalısınız. 5 söyleyiş her durumda işlemek için yeterli olmadığından, sipariş varlığının ayrıntıları eksik olabilir.

1. Üst gezinti bölmesinde **Test**'i seçin.
1. Söyleyiyi `deliver a medium veggie pizza` girin ve Enter'u seçin. Aktif model% 70'in üzerinde güven ile doğru niyet öngördü.

    ![Niyeti test etmek için yeni bir söyleyiş girin.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance.png)

1. Varlık tahminlerini görmek için **Denetle'yi** seçin.

    ![Etkileşimli test panelinde varlık tahminlerini görüntüleyin.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance-and-entity-predictions.png)

    Boyutu doğru olarak tanımlanmış. `OrderPizza` Amaçtaki örnek ifadelerin boyut `medium` olarak bir örneği olmadığını, ancak orta içeren bir `SizeDescriptor` ifade listesinin tanımlayıcısını kullandığını unutmayın.

    Miktar doğru tahmin edilmez. Bunu düzeltmek için, miktar belirtmek ve bu sözcüğü `Quantity` varlık olarak etiketlemek için bu sözcüğü kullanarak daha fazla örnek sözcük ekleyebilirsiniz.

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>HTTP bitiş noktasından erişmek için uygulamayı yayımlayın

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-http-endpoint"></a>HTTP bitiş noktasından niyet ve varlık tahmini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adresteki URL'nin sonuna gidin ve etkileşimli test paneline girdiğiniz sorguyu girin.

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

* [Öğretici - niyet](luis-quickstart-intents-only.md)
* [Kavram - varlıklar](luis-concept-entity-types.md) kavramsal bilgi
* [Kavram - kavramsal](luis-concept-feature.md) bilgi özellikleri
* [Nasıl eğitilir?](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUIS portalında test nasıl](luis-interactive-test.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama, kullanıcının söyleyiş niyetini bulmak ve bu sözden ayrıntıları ayıklamak için makineden öğrenilen bir varlık kullanır. Makinede öğrenilen varlığı kullanmak, varlığın ayrıntılarını ayrıştırmanızı sağlar.

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş keyphrase varlığı ekleme](luis-quickstart-intent-and-key-phrase.md)
