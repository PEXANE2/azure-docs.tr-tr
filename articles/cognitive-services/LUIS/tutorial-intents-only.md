---
title: 'Öğretici: amaçları tahmin etme-LUO'
description: Bu öğreticideki söylenişi (metin) temelinde bir kullanıcının amaç 'ı tahmin eden özel bir uygulama oluşturun.
ms.topic: tutorial
ms.date: 05/05/2020
ms.openlocfilehash: d0a625708e730094ab4dea8f705852f38ee6e1da
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959868"
---
# <a name="tutorial-build-a-luis-app-to-determine-user-intentions"></a>Öğretici: Kullanıcı amaçları 'nı öğrenmek için bir LUSıS uygulaması derleme

Bu öğreticide, bir kullanıcının, söylenişi (metin) temelinde bir amaç tahmin eden özel bir uygulama oluşturacaksınız.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Yeni uygulama oluşturma
> * Amaç oluşturma
> * Örnek konuşmalar ekleme
> * Uygulamayı eğitme
> * Uygulamayı yayımla
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
|`None`|Kullanıcının LUıN uygulamasının yanıt vermek üzere tasarlanmadığını bir şey isteyip istemediğini sorar. Bu amaç, uygulama oluşturmanın bir parçası olarak sağlanır ve silinemez. |

## <a name="create-a-new-app"></a>Yeni uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](includes/create-pizza-app.md)]

## <a name="create-a-new-intent"></a>Yeni amaç oluşturma

Amaç, kullanıcının doğal dil metinden belirlenen amaç temelinde sınıflandırılır.

Bir utterance sınıflandırmak için, amaç bu amaca göre sınıflandırılması gereken Kullanıcı arasları örneklerine ihtiyaç duyuyor.

1. **Oluşturma** bölümünde, **amaçlar** sayfasında, yeni bir amaç oluşturmak için **+ Oluştur** ' u seçin. Yeni amaç adını girip bitti ' yi `OrderPizza` seçin. **Done**

    `OrderPizza`Amaç, bir Kullanıcı bir pizza siparişi vermek istediğinde tahmin edilir.

1. Bir kullanıcının sormasını beklediğinizi bu amaca birkaç örnek ekleyin:

    |`OrderPizza`örnek söyleyceler|
    |--|
    |`can i get a pepperoni pizza and a can of coke please`|
    |`can i get a small pizza with onions peppers and olives`|
    |`delivery for a small pepperoni pizza`|
    |`pickup a cheddar cheese pizza large with extra anchovies`|
    |`i need 2 large cheese pizzas 6 large pepperoni pizzas and 1 large supreme pizza`|
    |`Order a pizza for me`|

    > [!div class="mx-imgBorder"]
    > ![Amaç sayfasında LUSıS portalında örnek ekleme ekran görüntüsü](media/tutorial-intents-only/add-example-utterances-for-pizza-order.png)

    _Örnek_söyleyeni sunarak, bu amaç için ne tür bir tahmin GEREKTIĞINE dair şanssıs hakkında bilgi sahibi olursunuz. Bunlar pozitif örneklerdir. Diğer tüm amaçlardaki söyleyler bu amaç için negatif örnekler olarak değerlendirilir.

    [!INCLUDE [Do not use too few utterances](includes/do-not-use-too-few-utterances.md)]

## <a name="create-remaining-intents"></a>Kalan amaçları oluştur

1. Amacı oluşturun `Greeting` ve aşağıdaki örnek söyleyeni ekleyin. Bu, bir kullanıcının yeni bir pizza siparişi konuşmasıyla başlayıp kullanmadığını belirleme amacınızı belirlemektir.

    |`Greeting`örnek söyleyceler|
    |--|
    |`Hi`|
    |`Hello`|
    |`Hey`|
    |`Start`|
    |`Begin`|

1. Amacı oluşturun `Confirm` ve aşağıdaki örnek söyleyeni ekleyin. Bu, bir kullanıcının sıralamayı ve sipariş ayrıntılarını kabul edip etmeyeceğini belirleme amacınızı belirlemektir.

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

  
    Sorgu dizesi parametresi `query` , söylenişi **sorgusu** URI 'ye geçirilir. Bu konuşma, örnek konuşmalarından hiçbiriyle aynı değil. Bu, LUOLUP olmadığını kontrol etmek için iyi bir test olmalıdır ve `OrderPizza` en üst puan hedefi olarak amacı tahmin eder.

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

    JSON sonucu, en üst Puanlama amacını özelliği olarak tanımlar **`prediction.topIntent`** . Tüm puanlar 1 ile 0 arasındadır ve 1 ' e daha iyi puan elde edilir.

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

Bu öğretici, bir LUSıS uygulaması, oluşturma amaçları, her amaca yönelik eklenen örnek, eklenen örnek ve son noktadaki yok etme, eğitilen, yayımlanan ve test edilen bir örnek ile oluşturulmuştur. Bunlar, LUIS modeli oluşturmanın temel adımlarıdır.

JSON yanıtı döndürdükten sonra LUIS’in istekle işi biter. LUIS kullanıcı konuşmalarını yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. Konuşma izleme, Azure bot gibi istemci uygulaması tarafından sağlanır.


[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık türleri](luis-concept-entity-types.md)
* [Eğitme](luis-how-to-train.md)
* [Yayımlama](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)
* [Azure bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu uygulamaya birleştirilebilir bir varlık ekleyin](tutorial-machine-learned-entity.md)
