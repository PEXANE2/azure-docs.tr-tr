---
title: 'Öğretici: List Entity-LUSıS'
titleSuffix: Azure Cognitive Services
description: Bir listedeki önceden tanımlanmış öğelerle eşleşen verileri alma. Listedeki her öğenin tam olarak eşleşen eş anlamlıları da olabilir
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/04/2019
ms.author: diberry
ms.openlocfilehash: 83644ac7144adaa664b13b6e7d7993c50da0e779
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74851893"
---
# <a name="tutorial-get-exact-text-matched-data-from-an-utterance-with-list-entity"></a>Öğretici: liste varlığıyla tam metin ile eşleşen verileri al

Bu öğreticide, önceden tanımlanmış bir öğe listesiyle tam olarak eşleşen verilerin nasıl alınacağını anlayın.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Uygulamayı içeri aktarın ve mevcut amacı kullanın
> * Liste varlığı ekleme
> * Ayıklanan verileri almak için uygulamayı eğitme, yayımlama ve sorgulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="what-is-a-list-entity"></a>Liste varlığı nedir?

Liste varlığı, söylenme içindeki sözcüklerle tam metin eşleşmelidir. Listeden her öğenin bir eşanlamlı sözcükler listesi olabilir. Tam bir eşleşme istediğinizde bir liste varlığı kullanın.

Bu içeri aktarılan pizza uygulaması için farklı tür pizza Crust türleri için bir liste varlığı oluşturun.

Liste varlığı bu veri türü için iyi bir seçimdir:

* Veri değerleri bilinen bir kümedir.
* Küme, bu varlık türü için maksimum LUIS [sınırlarını](luis-boundaries.md) aşmaz.
* Konuşmadaki metin bir eşanlamlı sözcük veya kurallı ad ile tam olarak eşleşiyor. LUSıS, tam metin eşleştirmelerinin ötesinde listeyi kullanmaz. Sözcük kökü, plurals ve diğer çeşitlemeler yalnızca bir liste varlığıyla çözümlenmez. Çeşitlemeleri yönetmek için, isteğe bağlı metin söz dizimine sahip bir [model](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance) kullanmayı düşünün.

> [!CAUTION]
> Bir liste varlığına veya bir tümcecik listesi olan makine tarafından öğrenilen bir varlık için tanımlayıcı olarak tercih ettiğinizden emin değilseniz, en iyi ve en esnek uygulama, tanımlayıcı olarak bir ifade listesi olan makine tarafından öğrenilen bir varlık kullanmaktır. Bu yöntem, LUSıS 'in Ayıklanacak verilerin değerlerini öğrenmesine ve genişletmesine izin verir.

## <a name="import-example-json-and-add-utterances"></a>Örnek. JSON öğesini içeri aktarın ve utterslar ekleyin

1.  [Uygulama json dosyasını](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/machine-learned-entity/pizza-tutorial-with-entities.json)indirip kaydedin.

    [!INCLUDE [Import app steps](includes/import-app-steps.md)]

1. İçeri aktarılan uygulamanın `OrderPizza` amacı vardır. Bu amacı seçin ve yeni Crust türleriyle birkaç tane ekleyin:

    |Yeni utterslar|
    |--|--|
    |Lütfen bir Pan Crust küçük pepperoni pizza sıralayın|
    |3 ince Crust Hawaii Haian Pizzas|
    |iki peluş Crust Pizzas 'i birlikte bulunan kişilerle sunun|
    |toplama için bir kalın Crust pizza|
    |tek derin bir pepperoni pizza|

## <a name="crust-list-entity"></a>Crust listesi varlığı

Artık **Orderpizza** hedefinin, Crust türleri ile örnek olarak olduğuna göre, lusıs 'in, Crust türlerini temsil eden kelimeleri anlaması gerekir.

Birincil ad ve eş anlamlılar örnekleri şunlardır:

|Kurallı ad|Eş anlamlılar|
|--|--|
|Derin çanak|derin<br>derin kırak<br>Mürekke<br>kalın Crust|
|Yatay/dikey kaydırma|normal<br>Özgün<br>olağan<br>düzenli Crust<br>özgün Crust<br>normal Crust|
|Peluş|peluş Crust|
|İnce|ince Crust<br>Skinny<br>Skinny Crust|

1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

1. **+ Oluştur**seçeneğini belirleyin.

1. Açılan varlık iletişim kutusunda varlık adı olarak `CrustList`, varlık türü olarak da **List** (Liste) seçin. **İleri**’yi seçin.

    > [!div class="mx-imgBorder"]
    > yeni varlık açılır menüsü oluşturma iletişim ![ekran görüntüsü](media/luis-quickstart-intent-and-list-entity/create-pizza-crust-list-entity.png)

1. **Liste varlığı oluştur** sayfasında, her bir kurallı ad için kurallı adları ve Eşanlamlı sözcükleri girip **Oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > liste varlığına öğe eklemenin ekran görüntüsünü ![](media/luis-quickstart-intent-and-list-entity/add-pizza-crust-items-list-entity.png)

    Bir LUSıS uygulamasına bir liste varlığı eklediğinizde, metni liste varlığıyla [etiketlemenize](label-entity-example-utterance.md) gerek yoktur. Tüm amaçlar için tüm söyleye uygulanır.

## <a name="train-the-app-before-testing-or-publishing"></a>Test veya yayımlamadan önce uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Uygulamayı bitiş noktasından sorgulamak üzere yayımlayın

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktadan amacı ve varlık tahminini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

2. Adresteki URL 'nin sonuna gidin ve aşağıdaki deterance 'i girin:

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

    Crust türleri tam metin eşleştirirken bulundu ve JSON yanıtında döndürüldü. Bu bilgiler, istemci uygulaması tarafından sırayı işlemek için kullanılır.

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Liste varlığı](luis-concept-entity-types.md#list-entity) kavramsal bilgileri
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)
* [Kavram-varlıklar](luis-concept-entity-types.md)
* [Normal ifade varlığının JSON başvurusu](reference-entity-regular-expression.md?tabs=V3)
* [Verileri ayıklamak için varlık ekleme](luis-how-to-add-entities.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici örnek bir şekilde eklenmiştir ve sonra gelen metinlerin tam metin eşleşmelerini ayıklamak için bir liste varlığı oluşturmuştur. Uygulama eğitildikten ve yayımlandıktan sonra uç noktaya gönderilen bir sorgu amacı tanımladı ve ayıklanan verileri döndürdü.

> [!div class="nextstepaction"]
> [Role sahip önceden oluşturulmuş varlık ekleme](tutorial-entity-roles.md)

