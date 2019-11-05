---
title: 'Öğretici: amaçları tahmin etme-LUO'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, kullanıcının amaç 'ı tahmin eden özel bir uygulama oluşturun. E-posta adresleri veya tarihler gibi konuşma metinlerinden çeşitli veri öğeleri ayıklamadığından bu uygulama en basit LUIS uygulaması türüdür.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 09/04/2019
ms.author: diberry
ms.openlocfilehash: 83ecf0767f2b21065c698421e3ad8f07f31d5b16
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73465293"
---
# <a name="tutorial-build-luis-app-to-determine-user-intentions"></a>Öğretici: Kullanıcı amaçları 'nı öğrenmek için LUSıS uygulaması oluşturma

Bu öğreticide, konuşmaya (metne) göre bir kullanıcının amacını tahmin eden özel bir İnsan Kaynakları (İK) uygulaması oluşturacaksınız. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Yeni bir uygulama oluşturma 
> * Amaç oluşturma
> * Örnek konuşmalar ekleme
> * Uygulamayı eğitme
> * Uygulama yayımlama
> * Uç noktadan amaç alma


[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="user-intentions-as-intents"></a>Amaç olarak Kullanıcı amaçları

Uygulamanın amacı, konuşma, doğal dil metinleriyle bir amaç belirlemektir: 

`Are there any new positions in the Seattle office?`

Bunlar **Amaçlar** şeklinde kategorilere ayrılır. 

Bu uygulamanın birkaç amacı vardır. 

|Amaç|Amaç|
|--|--|
|`ApplyForJob`|Kullanıcının bir iş için uygulama uygulayıp uygulamamadığını belirleme.|
|`GetJobInformation`|Kullanıcının genel veya belirli bir işteki işler hakkında bilgi arayıp aramadığını belirleme.|
|`None`|Kullanıcının bir uygulamanın yanıt vermesi gerekip gerekmediğini isteyip istemediğini belirleme. Bu amaç, uygulama oluşturmanın bir parçası olarak sağlanmışsa ve silinemezler. |

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-intent-for-job-information"></a>İş bilgileri için amaç oluştur

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin. Yeni amaç adı olarak `GetJobInformation` girin. Bu amaç, bir Kullanıcı şirketteki açık işler hakkında bilgi istediğinde tahmin edilir. 

    ![Language Understanding (LUA) yeni amaç iletişim kutusunun ekran görüntüsü](media/luis-quickstart-intents-only/create-intent.png "Language Understanding (LUA) yeni amaç iletişim kutusunun ekran görüntüsü")

1. **Done** (Bitti) öğesini seçin.

2. Bir kullanıcının sormasını beklediğinizi bu amaca birkaç örnek ekleyin:

    | Örnek konuşmalar|
    |--|
    |`Any new jobs posted today?`|
    |`Are there any new positions in the Seattle office?`|
    |`Are there any remote worker or telecommute jobs open for engineers?`|
    |`Is there any work with databases?`|
    |`I'm looking for a co-working situation in the tampa office.`|
    |`Is there an internship in the san francisco office?`|
    |`Is there any part-time work for people in college?`|
    |`Looking for a new situation with responsibilities in accounting`|
    |`Looking for a job in new york city for bilingual speakers.`|
    |`Looking for a new situation with responsibilities in accounting.`|
    |`New jobs?`|
    |`Show me all the jobs for engineers that were added in the last 2 days.`|
    |`Today's job postings?`|
    |`What accounting positions are open in the london office?`|
    |`What positions are available for Senior Engineers?`|
    |`Where is the job listings`|

    [![MyStore amacı için yeni uttersları girme ekran görüntüsü](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "MyStore amacı için yeni uttersları girme ekran görüntüsü")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

    _Örnek_bir amaç sunarak, bu amaçla ne tür bir anlamı tahmin etmeniz gerektiğini eğitme. 

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    

## <a name="add-example-utterances-to-the-none-intent"></a>Hiçbiri amacına örnek ekleme 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-before-testing-or-publishing"></a>Test veya yayımlamadan önce uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Uygulamayı bitiş noktasından sorgulamak üzere yayımlayın

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-prediction-from-the-endpoint"></a>Uç noktadan amaç tahminini al

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Adres çubuğundaki URL'nin sonuna gidip `I'm looking for a job with Natural Language Processing` yazın. Son sorgu dizesi parametresi konuşma `q`sorgusu**olan** öğesidir. Bu konuşma, örnek konuşmalarından hiçbiriyle aynı değil. İyi bir test olduğundan `GetJobInformation` amacını en yüksek puanlı amaç olarak döndürmelidir. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.9923871
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.9923871
        },
        {
          "intent": "None",
          "score": 0.007810574
        }
      ],
      "entities": []
    }
    ```

    `verbose=true` QueryString parametresi, uygulamanın sorgu sonuçlarındaki **Tüm amaçları** içerir. Bu uygulamanın şu anda bir varlığı olmadığından varlıklar dizisi boştur. 

    JSON sonucu, en yüksek puanlı amacı **`topScoringIntent`** özelliği olarak tanımlar. Tüm puanlar 1 ile 0 arasındadır ve 1'e yakın olan puanlar daha iyidir. 

## <a name="create-intent-for-job-applications"></a>İş uygulamaları için amaç oluşturma

LUE portalına dönün ve Kullanıcı için bir iş için uygulama hakkında bilgi gerekip gerekmediğini öğrenmek için yeni bir amaç oluşturun.

1. Uygulama derleme ekranına dönmek için sağ üstten **Build** (Derle) öğesini seçin.

1. Amaç listesine ulaşmak için sol taraftaki menüden **amaçlar** ' ı seçin.

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin ve `ApplyForJob` adını girin. 

    ![LUIS Create new intent (Yeni amaç oluştur) iletişim kutusu](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

1. Bu amaca kullanıcının sormasını beklediğiniz birkaç konuşma girin; örneğin:

    | Örnek konuşmalar|
    |--|
    |`Fill out application for Job 123456`|
    |`Here is my c.v. for position 654234`|
    |`Here is my resume for the part-time receptionist post.`|
    |`I'm applying for the art desk job with this paperwork.`|
    |`I'm applying for the summer college internship in Research and Development in San Diego`|
    |`I'm requesting to submit my resume to the temporary position in the cafeteria.`|
    |`I'm submitting my resume for the new Autocar team in Columbus, OH`|
    |`I want to apply for the new accounting job`|
    |`Job 456789 accounting internship paperwork is here`|
    |`Job 567890 and my paperwork`|
    |`My papers for the tulsa accounting internship are attached.`|
    |`My paperwork for the holiday delivery position`|
    |`Please send my resume for the new accounting job in seattle`|
    |`Submit resume for engineering position`|
    |`This is my c.v. for post 234123 in Tampa.`|


## <a name="train-again"></a>Yeniden eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Yeniden yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-prediction-again"></a>Amaç tahminini yeniden alın

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Yeni tarayıcı penceresinde URL'nin sonuna `Can I submit my resume for job 235986` yazın. 

    ```json
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9634406
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9634406
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0171300638
        },
        {
          "intent": "None",
          "score": 0.00670867041
        }
      ],
      "entities": []
    }
    ```

    Sonuçlar, yeni amaç olan **ApplyForJob** ile birlikte mevcut amaçları da içerir. 

## <a name="client-application-next-steps"></a>İstemci-uygulama sonraki adımları

JSON yanıtı döndürdükten sonra LUIS’in istekle işi biter. LUIS kullanıcı konuşmalarını yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. Konuşma izleme, Azure bot gibi istemci uygulaması tarafından sağlanır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık türleri](luis-concept-entity-types.md)
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUSıS portalında test etme](luis-interactive-test.md)
* [Azure bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, İnsan Kaynakları (İK) uygulamasını oluşturdu, 2 amaç oluşturdu, her amaca örnek konuşmalar ekledi, Yok amacına örnek konuşmalar ekledi, eğitti, yayımladı, ve uç noktada test etti. Bunlar, LUIS modeli oluşturmanın temel adımlarıdır. 

[Basit bir varlık ve tümcecik listesi ekleyerek](luis-quickstart-primary-and-secondary-data.md)bu uygulamayla devam edin.

> [!div class="nextstepaction"]
> [Bu uygulamaya önceden derlenmiş amaçlar ve varlıklar ekleme](luis-tutorial-prebuilt-intents-entities.md)
