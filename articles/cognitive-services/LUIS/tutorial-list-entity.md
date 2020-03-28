---
title: 'Öğretici: Liste varlık - LUIS'
description: Bir listedeki önceden tanımlanmış öğelerle eşleşen verileri alma. Listedeki her öğenin tam olarak eşleşen eş anlamlıları da olabilir
ms.topic: tutorial
ms.date: 03/12/2020
ms.openlocfilehash: 1cfeccbd54e8ef8ec315d53fc7a766760c92a0d1
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79297416"
---
# <a name="tutorial-get-exact-text-matched-data-from-an-utterance-with-list-entity"></a>Öğretici: Liste varlığıyla bir sözcükten tam metin eşleştirme verileri alın

Bu öğreticide, önceden tanımlanmış öğelerlistesiyle tam olarak eşleşen verileri nasıl elde edebilirsiniz.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Uygulamayı içe aktarma ve mevcut amacı kullanma
> * Liste varlığı ekleme
> * Çıkarılan verileri almak için uygulama, yayımlama ve sorgu uygulaması

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="what-is-a-list-entity"></a>Liste varlığı nedir?

Liste varlığı, sözcükteki sözcüklerle tam bir metin eşleşmesidir. Listeden her öğenin bir eşanlamlı sözcükler listesi olabilir. Tam eşleşme istediğinizde bir liste varlığı kullanın.

Bu alınan pizza uygulaması için, pizza kabuğu farklı türleri için bir liste varlık oluşturun.

Liste varlığı bu veri türü için iyi bir seçimdir:

* Veri değerleri bilinen bir kümedir.
* Küme, bu varlık türü için maksimum LUIS [sınırlarını](luis-boundaries.md) aşmaz.
* Söyleyişdeki metin, eşanlamlı veya kanonik adla yapılan büyük/küçük harf duyarsız bir eşleşmedir. LUIS listeyi maçın ötesinde kullanmıyor. Stemming, çoğulve diğer varyasyonlar yalnızca bir liste varlığı ile çözülmez. Varyasyonları yönetmek için isteğe bağlı metin sözdizimi içeren bir [desen](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance) kullanmayı düşünün.

> [!CAUTION]
> Bir liste varlığı veya açıklayıcı olarak bir ifade listesi ile makine öğrenilen bir varlık istiyorsanız emin değilseniz, en iyi ve en esnek uygulama bir ifade listesi açıklayıcı olarak bir makine öğrenilen varlık kullanmaktır. Bu yöntem, LUIS'in verilerin değerlerini ayıklamayı öğrenmesini ve genişletmesini sağlar.

## <a name="import-example-json-and-add-utterances"></a>İthalat örneği .json ve söyleyiş eklemek

1.  Uygulamayı indirin ve kaydedin [JSON dosyasını.](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json)

    [!INCLUDE [Import app steps](includes/import-app-steps.md)]

1. Alınan uygulamanın `OrderPizza` bir amacı vardır. Bu amacı seçin ve yeni kabuk türleri ile birkaç söyleyerek ekleyin:

    |Yeni söyleyişler|
    |--|--|
    |bir tava kabuk küçük pepperoni pizza sipariş edin|
    |3 ince kabuk hawaii pizza|
    |ekmek çubukları ile 2 doldurulmuş kabuk pizza teslim|
    |pikap için bir kalın kabuk pizza|
    |bir derin çanak pepperoni pizza|

## <a name="crust-list-entity"></a>Kabuk listesi varlığı

Şimdi **OrderPizza** niyet kabuk türleri ile örnek sözler vardır, LUIS hangi kelimelerin kabuk türleri temsil anlamak gerekir.

Birincil ad ve eşanlamlı örnekler şunlardır:

|Kanonik ad|Eş anlamlılar|
|--|--|
|Derin çanak|Derin<br>derin çanak kabuk<br>Kalın<br>kalın kabuk|
|Kaydır|Normal<br>Özgün<br>Normal<br>düzenli kabuk<br>orijinal kabuk<br>normal kabuk|
|Doldurulmuş|kabuk dolması|
|İnce|ince kabuk<br>Sıska<br>sıska kabuk|

1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

1. Seçin **+ Oluştur**.

1. Açılan varlık iletişim kutusunda varlık adı olarak `CrustList`, varlık türü olarak da **List** (Liste) seçin. **Sonraki'ni**seçin.

    > [!div class="mx-imgBorder"]
    > ![Yeni varlık oluşturma açılan iletişim kutusunun ekran görüntüsü](media/luis-quickstart-intent-and-list-entity/create-pizza-crust-list-entity.png)

1. Liste **varlık** oluştur sayfasında, her kanonik ad için kanonik adları ve eşanlamlıları girin ve ardından **Oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![Liste varlığına öğe ekleme ekran görüntüsü](media/luis-quickstart-intent-and-list-entity/add-pizza-crust-items-list-entity.png)

    Bir LUIS uygulamasına bir liste varlığı eklediğinizde, metni liste varlığıyla [etiketlemeniz](label-entity-example-utterance.md) gerekmez. Tüm niyetlerle tüm sözlerle uygulanır.

## <a name="train-the-app-before-testing-or-publishing"></a>Uygulamayı test etmeden veya yayınlamadan önce eğitin

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Uygulamayı bitiş noktasından sorgulamak için yayımlama

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Bitiş noktasından niyet ve varlık tahmini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

2. Adresteki URL'nin sonuna gidin ve aşağıdaki söyleyiyi girin:

    `Deliver 2 deep dish hawaiian pizzas and a thin pepperoni`

    Son sorgu dizesi parametresi ifade **s**orgusu olan `query` öğesidir.


    ```json
    {
        "query": "Deliver 2 deep dish hawaiian pizzas and a thin pepperoni",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.9957229
                },
                "None": {
                    "score": 0.016832687
                },
                "Confirm": {
                    "score": 0.0015708931
                },
                "Greeting": {
                    "score": 0.00057060417
                }
            },
            "entities": {
                "number": [
                    2
                ],
                "CrustList": [
                    [
                        "Deep dish"
                    ],
                    [
                        "Thin"
                    ]
                ],
                "$instance": {
                    "number": [
                        {
                            "type": "builtin.number",
                            "text": "2",
                            "startIndex": 8,
                            "length": 1,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "CrustList": [
                        {
                            "type": "CrustList",
                            "text": "deep dish",
                            "startIndex": 10,
                            "length": 9,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        },
                        {
                            "type": "CrustList",
                            "text": "thin",
                            "startIndex": 42,
                            "length": 4,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
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

    Kabuk türleri tam metin eşleşmeleri olarak bulundu ve JSON yanıtı döndü. Bu bilgiler, siparişi işlemek için istemci uygulaması tarafından kullanılır.

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* Varlık kavramsal bilgilerini [listele](luis-concept-entity-types.md#list-entity)
* [Nasıl eğitilir?](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUIS portalında test nasıl](luis-interactive-test.md)
* [Kavram - varlıklar](luis-concept-entity-types.md)
* [Normal ifade varlığının JSON referansı](reference-entity-regular-expression.md?tabs=V3)
* [Veri ayıklamak için varlıklar ekleme](luis-how-to-add-entities.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici, örnek ifadeler ekledi, sonra tam metin eşleşmeleri ifadeler ayıklamak için bir liste varlık oluşturdu. Uygulama eğitildikten ve yayımlandıktan sonra uç noktaya gönderilen bir sorgu amacı tanımladı ve ayıklanan verileri döndürdü.

> [!div class="nextstepaction"]
> [Rolü olan önceden oluşturulmuş varlık ekleme](tutorial-entity-roles.md)

