---
title: 'Öğretici: Amaçları tahmin etme-LUO'
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
ms.openlocfilehash: 7139876f64841a877e688ec6faf03597c527d1f2
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70375814"
---
# <a name="tutorial-build-luis-app-to-determine-user-intentions"></a>Öğretici: Kullanıcı amaçları 'nı öğrenmek için LUSıS uygulaması oluşturun

Bu öğreticide, konuşmaya (metne) göre bir kullanıcının amacını tahmin eden özel bir İnsan Kaynakları (İK) uygulaması oluşturacaksınız. 

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

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
|ApplyForJob|Kullanıcının bir iş için uygulama uygulayıp uygulamamadığını belirleme.|
|Getjobınformation|Kullanıcının genel veya belirli bir işteki işler hakkında bilgi arayıp aramadığını belirleme.|
|Yok.|Kullanıcının bir uygulamanın yanıt vermesi gerekip gerekmediğini isteyip istemediğini belirleme. Bu amaç, uygulama oluşturmanın bir parçası olarak sağlanmışsa ve silinemezler. |

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-intent-for-job-information"></a>İş bilgileri için amaç oluştur

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin. Yeni amaç adı olarak `GetJobInformation` girin. Bu amaç, bir Kullanıcı şirketteki açık işler hakkında bilgi istediğinde tahmin edilir. 

    ![Ekran görüntüsü, arama Language Understanding (LUIS) yeni hedefi iletişim](media/luis-quickstart-intents-only/create-intent.png "ekran görüntüsü, arama Language Understanding (LUIS) yeni hedefi iletişim kutusu")

1. **Done** (Bitti) öğesini seçin.

2. Bir kullanıcının sormasını beklediğinizi bu amaca birkaç örnek ekleyin:

    | Örnek konuşmalar|
    |--|
    |Any new jobs posted today? (Bugün yayımlanan yeni iş ilanı var mı?)|
    |Are there any new positions in the Seattle office? (Seattle ofisinde yeni pozisyon var mı?)|
    |Mühendisler için uzak çalışan veya telecommute işleri açık mı?|
    |Is there any work in databases? (Veritabanı alanında iş var mı?)|
    |Bu ofiste, birlikte çalışan bir durum arıyor.|
    |San Francisco ofisinde bir internship var mı?|
    |Okul 'daki kişiler için bir yarı zamanlı iş var mı?|
    |Looking for a new situation with responsibilities in accounting (Muhasebe alanında yeni bir iş arayışım mevcut)|
    |İki dilli hoparlör için New York şehrinde bir iş aranıyor.|
    |Muhasebesinde sorumlulukların bulunduğu yeni bir durum aranıyor.|
    |New jobs? (Yeni iş var mı?)|
    |Son 2 güne eklenen mühendisler için tüm işleri göster.|
    |Bugünün iş nakilleri mı?|
    |Londra ofisinin hangi muhasebe konumları açık?|
    |What positions are available for Senior Engineers? (Kıdemli Mühendisler için uygun olan pozisyonlar hangileri?)|
    |Where is the job listings (İş ilanları nerede?)|

    [![Yeni Konuşma DepolamaAlanım hedefi için girme ekran görüntüsü](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "DepolamaAlanım hedefi için yeni konuşma girme ekran görüntüsü")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

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

1. Adres çubuğundaki URL'nin sonuna gidip `I'm looking for a job with Natural Language Processing` yazın. Son sorgu dizesi parametresi konuşma **sorgusu** olan `q` öğesidir. Bu konuşma, örnek konuşmalarından hiçbiriyle aynı değil. İyi bir test olduğundan `GetJobInformation` amacını en yüksek puanlı amaç olarak döndürmelidir. 

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

    QueryString parametresi, uygulamanın sorgu sonuçlarındaki **Tüm amaçları** içerir. `verbose=true` Bu uygulamanın şu anda bir varlığı olmadığından varlıklar dizisi boştur. 

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
    |Fill out application for Job 123456 (123456 numaralı iş için başvuru yap)|
    |Here is my c.v. for position 654234 (654234 numaralı pozisyon için özgeçmişimi gönderiyorum)|
    |Burada, bölüm zamanı Alım gönderisi için özgeçmişim.|
    |Bu PaperWork ile resim masası işine uygulamadım.|
    |San Diego 'de araştırma ve geliştirme bölümünde yaz üniversite internship için Uygulamam|
    |Demin 'i Cafeteria 'daki geçici konuma göndermek istiyorum.|
    |Yeni otomobil ekibi için özgeçmişimi Columbus, OH|
    |I want to apply for the new accounting job (Yeni muhasebe işine başvurmak istiyorum)|
    |İş 456789 muhasebe internship PaperWork burada|
    |Job 567890 and my paperwork (567890 numaralı iş için belgelerim)|
    |Tulsa Accounting internship için kağıtlarım eklendi.|
    |Tatil teslim konumu için paperçalışmam|
    |Lütfen Seattle 'daki yeni muhasebe işi için özgeçmişimi gönderin|
    |Submit resume for engineering position (Mühendislik pozisyonu için özgeçmişimi gönder)|
    |This is my c.v. Kura 'da 234123 sonrası için.|

<!--

    [![Screenshot of entering new utterances for ApplyForJob intent](media/luis-quickstart-intents-only/utterance-applyforjob.png "Screenshot of entering new utterances for ApplyForJob intent")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    The labeled intent is outlined in red because LUIS is currently uncertain the intent is correct. Training the app tells LUIS the utterances are on the correct intent. 

-->

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
