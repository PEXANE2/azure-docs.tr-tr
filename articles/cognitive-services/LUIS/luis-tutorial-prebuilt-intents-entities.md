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
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 4697bad15a374bed0de08b7cabc5aceaad7f1259
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876717"
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

1. Tüm hedefleri seçin ve **bitti**' yi seçin. Bu amaçlar, görüşmede kullanıcının nerede olduğunu ve ne yapması gerektiğini belirlemek için yararlıdır. 

## <a name="add-prebuilt-entities-to-help-with-common-data-type-extraction"></a>Ortak veri türü ayıklamasıyla ilgili yardım için önceden oluşturulmuş varlıklar ekleme

LUIS, ortak veri ayıklama işlemi için önceden oluşturulmuş birkaç varlık sunar. 

1. Sol gezinti menüsünden **Entities** (Varlıklar) öğesini seçin.

1. **Add prebuilt entity** (Önceden oluşturulan varlık ekle) düğmesini seçin.

1. Önceden oluşturulmuş varlıklar listesinden aşağıdaki varlıkları seçin ve **bitti**' yi seçin:

   * **[Kişi adı](luis-reference-prebuilt-person.md)** 
   * **[GeographyV2](luis-reference-prebuilt-geographyV2.md)**

     ![Önceden oluşturulmuş varlıklar iletişim kutusunda seçilen sayının ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

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
      "query": "I want to cancel my trip to Seattle to see Bob Smith.",
      "topScoringIntent": {
        "intent": "Utilities.ReadAloud",
        "score": 0.100361854
      },
      "intents": [
        {
          "intent": "Utilities.ReadAloud",
          "score": 0.100361854
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.08102781
        },
        {
          "intent": "Utilities.SelectNone",
          "score": 0.0398852825
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0277276486
        },
        {
          "intent": "Utilities.SelectItem",
          "score": 0.0220712926
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0145813478
        },
        {
          "intent": "None",
          "score": 0.012434179
        },
        {
          "intent": "Utilities.Escalate",
          "score": 0.0122632384
        },
        {
          "intent": "Utilities.ShowNext",
          "score": 0.008534077
        },
        {
          "intent": "Utilities.ShowPrevious",
          "score": 0.00547111453
        },
        {
          "intent": "Utilities.SelectAny",
          "score": 0.00152912608
        },
        {
          "intent": "Utilities.Repeat",
          "score": 0.0005556819
        },
        {
          "intent": "Utilities.FinishTask",
          "score": 0.000169488427
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.000149565312
        },
        {
          "intent": "Utilities.GoBack",
          "score": 0.000141017343
        },
        {
          "intent": "Utilities.Reject",
          "score": 6.27324E-06
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

