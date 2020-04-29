---
title: 'Öğretici: amaçları tahmin etme-LUO'
description: Bu öğreticide, kullanıcının amaç 'ı tahmin eden özel bir uygulama oluşturun. E-posta adresleri veya tarihler gibi konuşma metinlerinden çeşitli veri öğeleri ayıklamadığından bu uygulama en basit LUIS uygulaması türüdür.
ms.topic: tutorial
ms.date: 03/24/2020
ms.openlocfilehash: c58c96f717de77c065d7f844928714eb4fb3e4db
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80286753"
---
# <a name="tutorial-build-a-luis-app-to-determine-user-intentions"></a>Öğretici: Kullanıcı amaçları 'nı öğrenmek için bir LUSıS uygulaması derleme

Bu öğreticide, bir kullanıcının, söylenişi (metin) temelinde bir amaç tahmin eden özel bir uygulama oluşturacaksınız.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Yeni bir uygulama oluşturma
> * Amaç oluşturma
> * Örnek konuşmalar ekleme
> * Uygulamayı eğitme
> * Uygulama yayımlama
> * Uç noktadan amaç tahminini al

## <a name="user-intentions-as-intents"></a>Amaç olarak Kullanıcı amaçları

Uygulamanın amacı, konuşma, doğal dil metinleriyle bir amaç belirlemektir:

`I'd like to order a veggie pizza with a salad on the side.`

Bunlar **Amaçlar** şeklinde kategorilere ayrılır.

|Amaç|Amaç|
|--|--|
|`ModifyOrder`|Kullanıcının pizza sırasını belirleme.|
|`Greeting`|Bot konuşmasına başla.|
|`ConfirmOrder`|Pizza sırasını onaylayın.|
|`None`|Kullanıcının uygulamanın yanıt vermesi beklenen bir şeyi isteyip istemediğini sorar. Bu amaç, uygulama oluşturmanın bir parçası olarak sağlanmışsa ve silinemezler. |

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](includes/create-pizza-app.md)]

## <a name="create-a-new-intent"></a>Yeni amaç oluşturma

Amaç, kullanıcının doğal dil metinden belirlenen amaç temelinde sınıflandırılır.

Bir utterance sınıflandırmak için, amaç bu amaca göre sınıflandırılması gereken Kullanıcı arasları örneklerine ihtiyaç duyuyor.

1. **Oluşturma** bölümünde, **amaçlar** sayfasında, yeni bir amaç oluşturmak için **+ Oluştur** ' u seçin. Yeni amaç adını `OrderPizza`girip **bitti**' yi seçin.

    `OrderPizza` Amaç, bir Kullanıcı bir pizza siparişi vermek istediğinde tahmin edilir.

1. Bir kullanıcının sormasını beklediğinizi bu amaca birkaç örnek ekleyin:

    |`OrderPizza`örnek söyleyceler|
    |--|
    |`can i get a pepperoni pizza and a can of coke please`|
    |`can i get a small pizza with onions peppers and olives`|
    |`delivery for a small pepperoni pizza`|
    |`pickup a cheddar cheese pizza large with extra anchovies`|
    |`i need 2 large cheese pizzas 6 large pepperoni pizzas and 1 large supreme pizza`|
    |`Order a pizza for me`|

    ![Örnek konuşmalar ekleme](media/tutorial-intents-only/add-example-utterances-for-pizza-order.png)

    _Örnek_söyleyeni sunarak, bu amaç için ne tür bir tahmin GEREKTIĞINE dair şanssıs hakkında bilgi sahibi olursunuz.

    [!INCLUDE [Do not use too few utterances](includes/do-not-use-too-few-utterances.md)]

## <a name="create-remaining-intents"></a>Kalan amaçları oluştur

1. `Greeting` Amacı oluşturun ve aşağıdaki örnek söyleyeni ekleyin. Bu, bir kullanıcının yeni bir pizza siparişi konuşmasıyla başlayıp kullanmadığını belirleme amacınızı belirlemektir.

    |`Greeting`örnek söyleyceler|
    |--|
    |`Hi`|
    |`Hello`|
    |`Hey`|
    |`Start`|
    |`Begin`|

1. `Confirm` Amacı oluşturun ve aşağıdaki örnek söyleyeni ekleyin. Bu, bir kullanıcının sıralamayı ve sipariş ayrıntılarını kabul edip etmeyeceğini belirleme amacınızı belirlemektir.

    |`Confirm`örnek söyleyceler|
    |--|
    |`Go ahead`|
    |`ok`|
    |`Yes`|
    |`Sure`|


## <a name="none-intent-example-utterances"></a>Hiçbiri amacı örnek söyliği

[!INCLUDE [Follow these steps to add the None intent to the app](includes/add-example-utterances-none-intent.md)]

## <a name="train-the-app"></a>Uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app"></a>Uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-prediction"></a>Amaç tahminini al

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğunda URL 'nin sonuna gidin ve şunu girin:

    `get a medium vegetarian pizza for delivery`

    Bu, bir örnek ile tam olarak aynı değildir. bu nedenle, LUYA 'nın bu amaçla ne tahmin edilebilir olduğunu öğreniyor olup olmadığını görmek için iyi bir test olması gerekir.

    Son sorgu dizesi parametresi konuşma **sorgusu** olan `query` öğesidir. Bu konuşma, örnek konuşmalarından hiçbiriyle aynı değil. İyi bir test olduğundan `OrderPizza` amacını en yüksek puanlı amaç olarak döndürmelidir.

    ```JSON
    {
        "query": "get a medium vegetarian pizza for delivery",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.6488959
                },
                "None": {
                    "score": 0.139966831
                },
                "Confirm": {
                    "score": 0.00736504374
                },
                "Greeting": {
                    "score": 0.003970454
                }
            },
            "entities": {}
        }
    }
    ```

    Bu uygulamanın Şu anda hiç varlık (Ayıklanacak veri birimi) olmadığından, varlıklar dizisi boş.

    JSON sonucu, en üst Puanlama amacını özelliği olarak **`prediction.topIntent`** tanımlar. Tüm puanlar 1 ile 0 arasındadır ve 1 ' e daha iyi puan elde edilir.

1. **Selamlama** hedefini HEDEFLEMEK için URL **sorgu** parametresini değiştirin:

    `Howdy`

    Bu, bir örnek ile tam olarak aynı değildir. bu nedenle, LUYA 'nın bu amaçla ne tahmin edilebilir olduğunu öğreniyor olup olmadığını görmek için iyi bir test olması gerekir.

    ```json
    {
        "query": "howdy",
        "prediction": {
            "topIntent": "Greeting",
            "intents": {
                "Greeting": {
                    "score": 0.446016937
                },
                "Confirm": {
                    "score": 0.2390079
                },
                "None": {
                    "score": 0.09119555
                },
                "OrderPizza": {
                    "score": 0.00109590159
                }
            },
            "entities": {}
        }
    }
    ```

    Bu tahminde %44 Güvenirlik puanı vardır. Güven Puanını artırmak için, 15 ila 30 örnek Aralık ekleyin.

## <a name="client-application-next-steps"></a>İstemci-uygulama sonraki adımları

JSON yanıtı döndürdükten sonra LUIS’in istekle işi biter. LUIS kullanıcı konuşmalarını yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. Konuşma izleme, Azure bot gibi istemci uygulaması tarafından sağlanır.


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık türleri](luis-concept-entity-types.md)
* [Eğitme](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)
* [Azure bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, bir LUSıS uygulaması, oluşturma amaçları, her amaca yönelik eklenen örnek, eklenen örnek ve son noktadaki yok etme, eğitilen, yayımlanan ve test edilen bir örnek ile oluşturulmuştur. Bunlar, LUIS modeli oluşturmanın temel adımlarıdır.

> [!div class="nextstepaction"]
> [Bu uygulamaya birleştirilebilir bir varlık ekleyin](tutorial-machine-learned-entity.md)
