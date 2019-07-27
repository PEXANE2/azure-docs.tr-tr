---
title: Önceden oluşturulmuş amaçlar ve varlıklar-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, bir uygulamaya önceden oluşturulmuş amaçlar ve varlıklar ekleyerek, hızlı bir şekilde amaç ve veri ayıklama elde edin. Herhangi bir konuşmayı önceden oluşturulmuş varlıklarla etiketlemeniz gerekmez. Varlık otomatik olarak algılanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 07/16/2018
ms.author: diberry
ms.openlocfilehash: 8c3fa2a68018e0eb45f37b54b16b6e7797cfa7c4
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560044"
---
# <a name="tutorial-identify-common-intents-and-entities"></a>Öğretici: Ortak amaçları ve varlıkları tanımla

Bu öğreticide, hızlı bir şekilde amaç ve veri ayıklama kazanmak için bir Insan kaynakları öğretici uygulamasına önceden oluşturulmuş amaçlar ve varlıklar ekleyin. Varlık otomatik olarak algılandığından, önceden oluşturulmuş varlıklar ile herhangi bir söyleme işaretlemeniz gerekmez.

Önceden oluşturulmuş modeller (etki alanları, amaçlar ve varlıklar) modelinizi hızlı bir şekilde oluşturmanıza yardımcı olur.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Yeni uygulama oluşturma
> * Önceden oluşturulmuş amaçları ekleme 
> * Önceden oluşturulmuş varlıkları ekleme 
> * Eğitim 
> * Yayımlama 
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]


## <a name="add-prebuilt-intents-to-help-with-common-user-intentions"></a>Ortak Kullanıcı amaçları konusunda yardımcı olmak için önceden oluşturulmuş amaçlar ekleyin

LUIS, ortak kullanıcı amaçları konusunda yardımcı olmak için önceden oluşturulmuş birçok amaca sahiptir.  

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. **Add prebuilt domain intent** (Önceden oluşturulmuş etki alanı amacı ekle) öğesini seçin. 

1. `Utilities` arayın. 

    [![Arama kutusundaki yardımcı programlarla önceden oluşturulmuş amaçlar iletişim kutusunun ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

1. Aşağıdaki amaçları ve **Done** (Bitti) öğesini seçin: 

   * Utilities.Cancel
   * Utilities.Confirm
   * Utilities.Help
   * Utilities.StartOver
   * Utilities.Stop

     Bu amaçlar, görüşmede kullanıcının nerede olduğunu ve ne yapması gerektiğini belirlemek için yararlıdır. Bu amaçlar, varlıkların kullanımını içerir. Bu varlıklar, LUSıS uygulamasına otomatik olarak eklenir: Number, Ordinal ve Utilities. DirectionalReference. 


## <a name="add-prebuilt-entities-to-help-with-common-data-type-extraction"></a>Ortak veri türü ayıklamasıyla ilgili yardım için önceden oluşturulmuş varlıklar ekleme

LUIS, ortak veri ayıklama işlemi için önceden oluşturulmuş birkaç varlık sunar. 

1. Sol gezinti menüsünden **Entities** (Varlıklar) öğesini seçin.

1. **Add prebuilt entity** (Önceden oluşturulan varlık ekle) düğmesini seçin.

1. Önceden oluşturulmuş varlıklar listesinden aşağıdaki varlıkları seçin ve **bitti**' yi seçin:

   * **[Kişi adı](luis-reference-prebuilt-person.md)** 
   * **[GeographyV2](luis-reference-prebuilt-geographyV2.md)**

     ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

     Bu varlıklar, istemci uygulamanıza ad eklemenize ve tanımayı kullanmanıza yardımcı olur.

## <a name="add-example-utterances-to-the-none-intent"></a>Hiçbiri amacına örnek ekleme 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulamaya yapılan değişikliklerin test edilebilir olması için uygulamayı eğitme 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitilen modelin uç noktadan sorgulanabilir olması için uygulamayı yayımlayın

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktadan amacı ve varlık tahminini alın

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Tarayıcı adres çubuğundaki URL'nin sonuna gidip `I want to cancel my trip to Seattle to see Bob Smith` yazın. Son sorgu dizesi parametresi konuşma **sorgusu** olan `q` öğesidir. 

    ```json
    {
      "query": "I want to cancel my trip to Seattle to see Bob Smith",
      "topScoringIntent": {
        "intent": "Utilities.Cancel",
        "score": 0.807676256
      },
      "intents": [
        {
          "intent": "Utilities.Cancel",
          "score": 0.807676256
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0487322025
        },
        {
          "intent": "Utilities.Help",
          "score": 0.0208660364
        },
        {
          "intent": "None",
          "score": 0.008789532
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.006929268
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00136293867
        }
      ],
      "entities": [
        {
          "entity": "seattle",
          "type": "builtin.geographyV2.city",
          "startIndex": 28,
          "endIndex": 34
        },
        {
          "entity": "bob smith",
          "type": "builtin.personName",
          "startIndex": 43,
          "endIndex": 51
        }
      ]
    }
    ```

    Sonuç, yardımcı programları tahmin eder. Iptal amacını% 80 güvenilirlikle ve şehir ve kişi adı verilerini ayıklamıştır. 


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

Önceden oluşturulmuş modeller hakkında daha fazla bilgi edinin:

* [Önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md): Bunlar, genel lusıs uygulama Authoring 'i azaltan ortak etki alanlardır
* Önceden oluşturulmuş amaçlar: bunlar ortak etki alanlarının bireysel amaçlardır. Tüm etki alanını eklemek yerine amaçları tek tek ekleyebilirsiniz.
* [Önceden oluşturulmuş varlıklar](luis-prebuilt-entities.md): bunlar çoğu Luo uygulamaları için yararlı olan yaygın veri türleridir.

LUSıS uygulamanız ile çalışma hakkında daha fazla bilgi edinin:

* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)

## <a name="next-steps"></a>Sonraki adımlar

İstemci uygulama, önceden oluşturulmuş varlıklar ekleyerek yaygın kullanıcı amaçlarını belirleyebilir ve ortak veri türlerini ayıklayabilir.  

> [!div class="nextstepaction"]
> [Uygulamaya normal ifade varlığı ekleme](luis-quickstart-intents-regex-entity.md)

