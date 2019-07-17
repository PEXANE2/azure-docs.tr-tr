---
title: Konuşma uç noktası gözden geçirme
titleSuffix: Azure Cognitive Services
description: LUIS HTTP uç noktası üzerinden alınan ifadeleri doğrulayarak veya düzelterek LUIS'in emin olmadığı uygulama tahminlerini geliştirin. Bazı konuşmaların amacının, diğerlerinin ise varlığının doğrulanması gerekebilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 2994f7b19d5a104b129dc4d7aff29dabbc89f0f4
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275998"
---
# <a name="tutorial-fix-unsure-predictions-by-reviewing-endpoint-utterances"></a>Öğretici: Konuşma uç noktası inceleyerek emin değilseniz Öngörüler Düzelt
Bu öğreticide, LUIS HTTP uç noktası üzerinden alınan ifadeleri doğrulayarak veya düzelterek LUIS'in emin olmadığı uygulama tahminlerini geliştireceksiniz. Bazı konuşmaların amaç, diğerlerinin ise varlık için doğrulanması gerekebilir. Zamanlanmış LUIS bakımınızın normal bir parçası olarak uç noktası konuşmalarını gözden geçirmeniz gerekir. 

Bu gözden geçirme işlemi, LUIS için uygulama alanınızı öğrenmenin bir diğer yoludur. LUIS, gözden geçirme listesinde görünen konuşmaları seçmiştir. Bu liste:

* Uygulamaya özeldir.
* Uygulamanın tahmin doğruluğunu geliştirmek için hazırlanmıştır. 
* Düzenli aralıklarla gözden geçirilmelidir. 

Uç nokta ifadelerini gözden geçirerek, ifadenin tahmin edilen amacını doğrular veya düzeltirsiniz. Tahmin edilmeyen veya yanlış tahmin edilen özel varlıkları da etiketlersiniz. 

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Örnek uygulamayı içeri aktarma
> * Uç nokta ifadelerini gözden geçirme
> * Tümcecik listesini güncelleştirme
> * Uygulamayı eğitme
> * Uygulama yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Örnek uygulamayı içeri aktarma

Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Aşağıdaki adımları kullanın:

1.  [Uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-sentiment-HumanResources.json) indirip kaydedin.

1. JSON'ı yeni bir uygulamaya içeri aktarın.

1. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `review` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

1. Eğitim ve yeni uygulama yayımlama.

1. Aşağıdaki Konuşma ekleme için uç noktayı kullanın. Ya da bunu bir [betik](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) veya bir tarayıcıda uç noktasından. Eklenecek konuşmalar:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

    Öğretici serisi aracılığıyla uygulamanın tüm sürümlerine sahipseniz, **Review endpoint utterances** (Uç nokta ifadelerini gözden geçir) listesinin sürüme göre değişmediğini görmek sizi şaşırtabilir. Hangi sürümü etkin olarak düzenlemekte olduğunuzdan veya uç noktada uygulamanın hangi sürümünün yayımlandığından bağımsız olarak gözden geçirilecek tek bir konuşma havuzu vardır. 

## <a name="review-endpoint-utterances"></a>Uç nokta ifadelerini gözden geçirme

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Sol gezintiden **Uç nokta ifadelerini gözden geçir**'i seçin. Bu liste **ApplyForJob** amacı için filtrelenmiştir. 

    [![Sol gezinti ekran gözden uç nokta konuşma düğmesi](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png#lightbox)

1. Etiketlenmiş varlıkları görmek için **Varlıklar görünümüne** geçin. 
    
    [![Ekran gözden uç nokta konuşma varlıklarla vurgulanmış geçiş görüntüleyin](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png#lightbox)


    Bu utterance `I'm looking for a job with Natural Language Processing`, amaca doğru değil. 

    Utterance mispredicted neden olan **ApplyForJob** amacı güdülür; bu 7 konuşma karşılaştırıldığında 21 konuşma **GetJobInformation**. Daha fazla konuşma hedefle daha yüksek bir öngörü sahip olursunuz. Konuşma amaçları arasında kalitesini ve miktar dengeli önemlidir.

1.  Bu utterance hizalamak için doğru hedefini seçin ve işi varlık içindeki işaretleyin. Değiştirilen utterance yeşil onay kutusunu seçerek uygulamaya ekleyin. 

    |İfade|Doğru amaç|Eksik varlıklar|
    |:--|:--|:--|
    |`I'm looking for a job with Natural Language Processing`|GetJobInfo|İş - "Doğal Dil İşleme"|

    Utterance ekleyerek gelen utterance taşır **gözden geçirin, konuşma uç noktası** için **GetJobInformation** hedefi. Uç nokta ifadesi şimdi söz konusu amaç için örnek bir ifade olmuştur. 

    Bu utterance doğru hizalama yanı sıra daha fazla konuşma eklenmelidir **GetJobInformation** hedefi. Bu, kendi başınıza tamamlamanız için bir alıştırma olarak bırakılmıştır. **Hiçbiri** amacı dışındaki her amacın kabaca aynı sayıda örnek ifadesi olmalıdır. **Hiçbiri** amacındaki ifade sayısının, uygulamadaki ifadelerin %10'u kadar olması gerekir. 

1. Bu amaçtaki diğer ifadeleri gözden geçirin, ifadeleri etiketleyin ve **Eşleşmiş amaç** yanlışsa düzeltin.

1. Artık listede bu ifadeler yer almamalıdır. Başka konuşmalar da görünürse, liste boşalana kadar amaçları düzelterek ve eksik varlıkları etiketleyerek listede çalışmaya devam edin. 

1. Filtre listesinde bir sonraki amacı seçin, sonra ifadeleri düzeltmeye ve varlıkları etiketlemeye devam edin. Her amacın son adımının ifade satırında **Eşleşmiş amaca ekle**'yi seçmek veya her amacın yanındaki kutuyu işaretleyip yukarıdaki tabloda **Seçileni ekle**'yi seçmek olduğunu unutmayın.

    Filtre listesindeki tüm amaçların ve varlıkların boş bir listesi olana kadar devam edin. Bu çok küçük bir uygulamadır. Gözden geçirme işlemi yalnızca birkaç dakika sürer. 

## <a name="update-phrase-list"></a>Tümcecik listesini güncelleştirme
Tüm yeni eklenen iş adlarıyla tümcecik listesini güncel tutun. 

1. Sol gezintiden **Tümcecik listeleri**'ni seçin.

2. **Jobs** tümcecik listesini seçin.

3. `Natural Language Processing` öğesini bir değer olarak ekleyin ve ardından **Kaydet**'i seçin. 

## <a name="train"></a>Eğitim

LUIS eğitilene kadar bu değişiklikten haberdar olmaz. 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

Bu uygulamayı içeri aktardıysanız, **Yaklaşım analizi**'ni seçmelisiniz.

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma

Düzeltilmiş ifadeye yakın bir ifade deneyin. 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Are there any natural language processing jobs in my department right now?` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. 

   ```json
   {
    "query": "are there any natural language processing jobs in my department right now?",
    "topScoringIntent": {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    "intents": [
      {
        "intent": "GetJobInformation",
        "score": 0.9247605
      },
      {
        "intent": "ApplyForJob",
        "score": 0.129989788
      },
      {
        "intent": "FindForm",
        "score": 0.006438211
      },
      {
        "intent": "EmployeeFeedback",
        "score": 0.00408575451
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00194211153
      },
      {
        "intent": "None",
        "score": 0.00166400627
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00118593348
      },
      {
        "intent": "MoveEmployee",
        "score": 0.0007885918
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.0006373631
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0005980781
      },
      {
        "intent": "Utilities.Confirm",
        "score": 3.719905E-05
      }
    ],
    "entities": [
      {
        "entity": "right now",
        "type": "builtin.datetimeV2.datetime",
        "startIndex": 64,
        "endIndex": 72,
        "resolution": {
          "values": [
            {
              "timex": "PRESENT_REF",
              "type": "datetime",
              "value": "2018-07-05 15:23:18"
            }
          ]
        }
      },
      {
        "entity": "natural language processing",
        "type": "Job",
        "startIndex": 14,
        "endIndex": 40,
        "score": 0.9869922
      },
      {
        "entity": "natural language processing jobs",
        "type": "builtin.keyPhrase",
        "startIndex": 14,
        "endIndex": 45
      },
      {
        "entity": "department",
        "type": "builtin.keyPhrase",
        "startIndex": 53,
        "endIndex": 62
      }
    ],
    "sentimentAnalysis": {
      "label": "positive",
      "score": 0.8251864
    }
   }
   }
   ```

   Doğru amaç yüksek puanla tahmin edilmiş ve **Job** varlığı `natural language processing` olarak algılanmıştır. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>Gözden geçirme işleminin yerini daha fazla ifade ekleme alabilir mi? 
Neden daha fazla örnek ifade eklenmediğini merak ediyor olabilirsiniz. Uç nokta ifadelerini gözden geçirmenin amacı nedir? Gerçek dünyadaki bir LUIS uygulamasında, uç nokta ifadeleri henüz alışmadığınız sözcük seçimi ve yerleşimi olan kullanıcılara aittir. Aynı sözcük seçimini ve yerleşimini daha önce kullandıysanız, özgün tahminin yüzdesi yüksek olabilir. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>En üst amaç neden ifade listesinde yer alıyor? 
Bazı uç nokta konuşmalarının gözden geçirme listesinde yüksek bir tahmin puanı olacaktır. Bu ifadeleri yine de gözden geçirmeniz ve doğrulamanız gerekir. Bunların listede olmasının nedeni, bir sonraki yüksek amacın puanının en üst amaç puanına yakın olmasıdır. İlk iki amaç arasında yaklaşık %15 bir fark olmasını istersiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide uç noktada gönderilen LUIS'in emin olmadığı konuşmaları incelediniz. Bu konuşmalar doğrulandıktan ve örnek konuşmalar olarak doğru amaçlara taşındıktan sonra LUIS tahmin etme doğruluğunu artıracaktır.

> [!div class="nextstepaction"]
> [Desenleri kullanmayı öğrenin](luis-tutorial-pattern.md)
