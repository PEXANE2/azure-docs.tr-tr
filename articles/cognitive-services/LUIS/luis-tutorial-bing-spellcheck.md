---
title: Yanlış yazılmış kelimeleri düzelt - LUIS
titleSuffix: Azure Cognitive Services
description: Luis uç nokta sorgularına Bing Yazım Denetimi API V7 ekleyerek sözcükleri sözcüklerde düzeltin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: a1e43cfc55611c432652055db2ac8411d835608b
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396826"
---
# <a name="correct-misspelled-words-with-bing-spell-check"></a>Bing Yazım Denetimi ile yanlış yazılmış sözcükleri düzeltme

LUIS, söyleyiş skoru ve varlıklarını tahmin etmeden önce yanlış yazılmış sözcükleri düzeltmek için LUIS uygulamanızı [Bing Spell Check API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) ile bütünleştirebilirsiniz. 

[!INCLUDE [Not supported in V3 API prediction endpoint](./includes/v2-support-only.md)]


## <a name="create-first-key-for-bing-spell-check-v7"></a>Bing Yazım Denetimi V7 için ilk anahtarı oluşturma

[İlk Bing Yazım Denetimi API v7 anahtarınız](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) ücretsizdir. 

![Ücretsiz anahtar oluşturun](./media/luis-tutorial-bing-spellcheck/free-key.png)

<a name="create-subscription-key"></a>

## <a name="create-endpoint-key"></a>Uç nokta tuşu oluşturma
Serbest anahtarınızın süresi dolduysa, bir bitiş noktası anahtarı oluşturun.

1. [Azure portalına](https://portal.azure.com)giriş yapın. 

2. Sol üst köşede **kaynak oluştur'u** seçin.

3. Arama kutusuna `Bing Spell Check API V7` yazın.

    ![Bing Yazım Denetimi API V7 ara](./media/luis-tutorial-bing-spellcheck/portal-search.png)

4. Hizmeti seçin. 

5. Yasal Uyarı da dahil olmak üzere bilgileri içeren bir bilgi paneli sağda görünür. Abonelik oluşturma işlemini başlatmak için **Oluştur'u** seçin. 

6. Bir sonraki panelde servis ayarlarınızı girin. Hizmet oluşturma işleminin tamamlanmasını bekleyin.

    ![Hizmet ayarlarını girin](./media/luis-tutorial-bing-spellcheck/subscription-settings.png)

7. Sol taraftaki gezintide **Sık Kullanılanlar** başlığı altındaki **tüm kaynakları** seçin.

8. Yeni hizmeti seçin. Onun türü **Bilişsel Hizmetler** ve konumu **küreseldir.** 

9. Ana panelde, yeni tuşlarınızı görmek için **Tuşlar'ı** seçin.

    ![Anahtarları kapma](./media/luis-tutorial-bing-spellcheck/grab-keys.png)

10. İlk anahtarı kopyala. İki anahtardan sadece birine ihtiyacın var. 

<!--
## Using the key in LUIS test panel
There are two places in LUIS to use the key. The first is in the [test panel](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). The key isn't saved into LUIS but instead is a session variable. You need to set the key every time you want the test panel to apply the Bing Spell Check API v7 service to the utterance. See [instructions](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel) in the test panel for setting the key.
-->
## <a name="adding-the-key-to-the-endpoint-url"></a>Bitiş noktası URL'sine anahtarı ekleme
Bitiş noktası sorgusu, yazım düzeltmesi uygulamak istediğiniz her sorgu için sorgu dize parametrelerinde geçirilen anahtarı ngerekir. LUIS'i arayan bir chatbot'unuz olabilir veya luis uç noktası API'sini doğrudan arayabilirsiniz. Bitiş noktası nasıl denirse çağrılsın, her çağrının düzgün çalışması için yazım düzeltmeleri için gerekli bilgileri içermesi gerekir.

Bitiş noktası URL'si, doğru geçirilmesi gereken birkaç değere sahiptir. Bing Yazım Denetimi API v7 anahtarı bunlardan sadece biri. **yazım Denetimi** parametresini doğru olarak ayarlamanız ve **bing-yazım denetimi-abonelik anahtarının** değerini anahtar değerine ayarlamanız gerekir:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appID}?subscription-key={luisKey}&spellCheck=true&bing-spell-check-subscription-key={bingKey}&verbose=true&timezoneOffset=0&q={utterance}`

## <a name="send-misspelled-utterance-to-luis"></a>LUIS'e yanlış yazılmış bir söz gönderin
1. Bir web tarayıcısında, önceki dizeyi `region` `appId`kopyalayın ve , , `luisKey`, ve `bingKey` kendi değerlerini değiştirin. Yayımlama [bölgenizden](luis-reference-regions.md)farklıysa, bitiş noktası bölgesini kullandığınızdan emin olun.

2. "Dağ ne kadar uzakta?" gibi yanlış yazılmış bir söz ekleyin. İngilizce'de, `mountain`bir `n`ile, doğru yazım. 

3. Sorguyu LUIS'e göndermek için enter'u seçin.

4. LUIS için `How far is the mountain?`bir JSON sonucu ile yanıt verir. Bing Yazım Denetimi API v7 bir yazım `query` hatası algılarsa, LUIS uygulamasının JSON yanıtındaki `alteredQuery` alan özgün sorguyu içerir ve alan LUIS'e gönderilen düzeltilmiş sorguyu içerir.

```json
{
  "query": "How far is the mountainn?",
  "alteredQuery": "How far is the mountain?",
  "topScoringIntent": {
    "intent": "Concierge",
    "score": 0.183866
  },
  "entities": []
}
```

## <a name="ignore-spelling-mistakes"></a>Yazım hatalarını yoksay

Bing Yazım Denetimi API v7 hizmetini kullanmak istemiyorsanız, doğru ve yanlış yazım eklemeniz gerekir. 

İki çözüm vardır:

* Luis'in yazım hatalarının yanı sıra doğru yazım hatalarını da öğrenebilmeleri için tüm farklı yazımlara sahip örnek sözcükleri etiketle. Bu seçenek, yazım denetleyicisi kullanmaktan daha fazla etiketleme çabası gerektirir.
* Sözcüğün tüm varyasyonlarını içeren bir tümcecik listesi oluşturun. Bu çözümle, örnek ifadelerdeki sözcük varyasyonlarını etiketlemeniz gerekmez. 

## <a name="publishing-page"></a>Yayımlama sayfası
[Yayımlama](luis-how-to-publish-app.md) sayfasında **Bing yazım denetleyicisini etkinleştirme** onay kutusu vardır. Bu, anahtarı oluşturmak ve bitiş noktası URL'sinin nasıl değiştiğini anlamak için bir kolaylıktır. Yine de yazım her söyleyiş için düzeltilmiş olması için doğru bitiş noktası parametrelerini kullanmanız gerekir. 

> [!div class="nextstepaction"]
> [Örnek sözlerle ilgili daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
