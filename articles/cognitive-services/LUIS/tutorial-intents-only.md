---
title: 'Öğretici: Niyetleri tahmin edin - LUIS'
description: Bu öğreticide, kullanıcının niyetini tahmin eden özel bir uygulama oluşturun. E-posta adresleri veya tarihler gibi konuşma metinlerinden çeşitli veri öğeleri ayıklamadığından bu uygulama en basit LUIS uygulaması türüdür.
ms.topic: tutorial
ms.date: 03/24/2020
ms.openlocfilehash: c58c96f717de77c065d7f844928714eb4fb3e4db
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80286753"
---
# <a name="tutorial-build-a-luis-app-to-determine-user-intentions"></a>Öğretici: Kullanıcı niyetlerini belirlemek için bir LUIS uygulaması oluşturun

Bu öğreticide, kullanıcının niyetini söyleyişe (metne) göre tahmin eden özel bir uygulama oluşturursunuz.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Yeni bir uygulama oluşturma
> * Amaç oluşturma
> * Örnek konuşmalar ekleme
> * Uygulamayı eğitme
> * Uygulama yayımlama
> * Bitiş noktasından niyet tahmini alma

## <a name="user-intentions-as-intents"></a>Niyet olarak kullanıcı niyetleri

Uygulamanın amacı konuşma, doğal dil metin niyetini belirlemektir:

`I'd like to order a veggie pizza with a salad on the side.`

Bunlar **Amaçlar** şeklinde kategorilere ayrılır.

|Amaç|Amaç|
|--|--|
|`ModifyOrder`|Kullanıcının pizza siparişini belirleyin.|
|`Greeting`|Bot sohbetine başlayın.|
|`ConfirmOrder`|Pizza siparişini onaylayın.|
|`None`|Kullanıcının uygulamanın yanıtlamaması gereken bir şey isteyip istmeyeceğini belirleyin. Bu amaç, uygulama oluşturmanın bir parçası olarak sağlanırsa ve silinemezse. |

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](includes/create-pizza-app.md)]

## <a name="create-a-new-intent"></a>Yeni amaç oluşturma

Bir amaç, kullanıcının doğal dil metninden belirlenen niyetine göre kullanıcı nın söyleyişlerini sınıflandırmak için kullanılır.

Bir söyleyiyi sınıflandırmak için, niyetin bu niyetle sınıflandırılması gereken kullanıcı söyleyiş örneklerine ihtiyacı vardır.

1. **Yapı** bölümünde, **Niyetler** sayfasında, yeni bir amaç oluşturmak için **+ Oluştur'u** seçin. Yeni niyet adını `OrderPizza`girin, ardından **Bitti'yi**seçin.

    Bir `OrderPizza` kullanıcı pizza sipariş etmek istediğinde amaç tahmin edilir.

1. Bir kullanıcının sormasını beklediğiniz bu amacla birkaç örnek sözcük ekleyin:

    |`OrderPizza`örnek söz|
    |--|
    |`can i get a pepperoni pizza and a can of coke please`|
    |`can i get a small pizza with onions peppers and olives`|
    |`delivery for a small pepperoni pizza`|
    |`pickup a cheddar cheese pizza large with extra anchovies`|
    |`i need 2 large cheese pizzas 6 large pepperoni pizzas and 1 large supreme pizza`|
    |`Order a pizza for me`|

    ![Örnek konuşmalar ekleme](media/tutorial-intents-only/add-example-utterances-for-pizza-order.png)

    _Örnek sözlerle,_ luis'e bu amaç için ne tür söyleyişler yapılması gerektiğini öğretmiş siniz.

    [!INCLUDE [Do not use too few utterances](includes/do-not-use-too-few-utterances.md)]

## <a name="create-remaining-intents"></a>Kalan amaçları oluşturma

1. `Greeting` Amacı oluşturun ve aşağıdaki örnek söyleyifadeler ekleyin. Bu, bir kullanıcının yeni bir pizza siparişi konuşmasına başedip başlamayanını belirlemek için bir amaçtır.

    |`Greeting`örnek söz|
    |--|
    |`Hi`|
    |`Hello`|
    |`Hey`|
    |`Start`|
    |`Begin`|

1. `Confirm` Amacı oluşturun ve aşağıdaki örnek söyleyifadeler ekleyin. Bu, bir kullanıcının sipariş verme nin yapIlip yapılmadığını belirlemek ve sipariş ayrıntılarını kabul etmek için bir amaçtır.

    |`Confirm`örnek söz|
    |--|
    |`Go ahead`|
    |`ok`|
    |`Yes`|
    |`Sure`|


## <a name="none-intent-example-utterances"></a>Hiçbir niyet örnek söyleyinmeler

[!INCLUDE [Follow these steps to add the None intent to the app](includes/add-example-utterances-none-intent.md)]

## <a name="train-the-app"></a>Uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app"></a>Uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-prediction"></a>Niyet tahmini alın

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Adres çubuğundaki URL'nin sonuna gidin ve girin:

    `get a medium vegetarian pizza for delivery`

    Bu tam olarak bir örnek söyleyiş olarak aynı değildir bu yüzden LUIS bu niyetle tahmin edilmelidir ne öğrenebilirsiniz görmek için iyi bir testtir.

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

    Varlıklar dizisi boştur, çünkü bu uygulamanın şu anda herhangi bir varlığı yoktur (ayıklamak için söyleyiş içindeki veri birimi).

    JSON sonucu, en yüksek **`prediction.topIntent`** puanlama niyetini özellik olarak tanımlar. Tüm skorlar 1 ile 0 arasındadır ve daha iyi skor 1'e yakındır.

1. **Karşılama** amacını hedeflemek için URL **sorgu** parametresini değiştirin:

    `Howdy`

    Bu tam olarak bir örnek söyleyiş olarak aynı değildir bu yüzden LUIS bu niyetle tahmin edilmelidir ne öğrenebilirsiniz görmek için iyi bir testtir.

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

    Bu tahmin % 44 güven puanına sahiptir. Güven puanını artırmak için, 15 ve 30 örnek söyleyiş ler ekleyin.

## <a name="client-application-next-steps"></a>İstemci uygulaması sonraki adımlar

JSON yanıtı döndürdükten sonra LUIS’in istekle işi biter. LUIS kullanıcı konuşmalarını yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. Konuşma takibi, Azure Bot gibi istemci uygulaması tarafından sağlanır.


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık türleri](luis-concept-entity-types.md)
* [Nasıl eğitilir?](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUIS portalında test nasıl](luis-interactive-test.md)
* [Azure Bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici bir LUIS uygulaması oluşturdu, niyetler oluşturdu, her amaca örnek ifadeler ekledi, None amacına örnek ifadeler ekledi, eğitildi, yayınlandı ve bitiş noktasında test edildi. Bunlar, LUIS modeli oluşturmanın temel adımlarıdır.

> [!div class="nextstepaction"]
> [Bu uygulamaya bağımsız bir varlık ekleme](tutorial-machine-learned-entity.md)
