---
title: Yanlış yazılmış kelimeleri Düzelt-LUSıS
titleSuffix: Azure Cognitive Services
description: Bing yazım denetimi API'si V7 LUIS uç nokta sorgulara ekleyerek konuşma doğru yanlış yazılan sözcükleri.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: 51b0d02443df872a7fae13116ea77b13d05055fa
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225460"
---
# <a name="correct-misspelled-words-with-bing-spell-check"></a>Bing yazım denetimi ile doğru yanlış yazılan sözcükleri

Luve 'nın, utterlik 'in Puanını ve varlıklarını tahmin edebilmesi için söyleştirilen yanlış yazılan sözcükleri düzeltmek üzere [Bing yazım denetimi API'si v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) ile bir tümleştirme yapabilirsiniz. 

[!INCLUDE [Not supported in V3 API prediction endpoint](./includes/v2-support-only.md)]


## <a name="create-first-key-for-bing-spell-check-v7"></a>Bing yazım denetimi V7'için ilk tuşu oluşturma

[İlk Bing yazım denetimi API'si v7 anahtarınız](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) ücretsizdir. 

![Ücretsiz anahtarı oluşturma](./media/luis-tutorial-bing-spellcheck/free-key.png)

<a name="create-subscription-key"></a>

## <a name="create-endpoint-key"></a>Uç noktası anahtarı oluşturma
Ücretsiz anahtarınızı süresi bir uç noktası anahtarı oluşturun.

1. [Azure Portal](https://portal.azure.com)’da oturum açın. 

2. Sol üst köşedeki **kaynak oluştur** ' u seçin.

3. Arama kutusuna `Bing Spell Check API V7` yazın.

    ![Arama için Bing yazım denetimi API'si V7](./media/luis-tutorial-bing-spellcheck/portal-search.png)

4. Hizmeti seçin. 

5. Yasal bildirimi bilgilerini içeren sağ bilgileri bölmesi görünür. Abonelik oluşturma işlemini başlatmak için **Oluştur** ' u seçin. 

6. Sonraki panelinde hizmet ayarlarınızı girin. Hizmet oluşturma işleminin tamamlanması için bekleyin.

    ![Hizmet ayarlarını girin](./media/luis-tutorial-bing-spellcheck/subscription-settings.png)

7. Sol taraftaki gezinmede **Sık Kullanılanlar** başlığı altındaki **tüm kaynaklar** ' ı seçin.

8. Yeni hizmeti seçin. Türü bilişsel **Hizmetler** ve konum **geneldir**. 

9. Yeni anahtarlarınızı görmek için ana bölmede **anahtarlar** ' ı seçin.

    ![Anahtarları alın](./media/luis-tutorial-bing-spellcheck/grab-keys.png)

10. İlk anahtarınızı kopyalayın. Yalnızca iki anahtarlarından biri gerekir. 

<!--
## Using the key in LUIS test panel
There are two places in LUIS to use the key. The first is in the [test panel](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). The key isn't saved into LUIS but instead is a session variable. You need to set the key every time you want the test panel to apply the Bing Spell Check API v7 service to the utterance. See [instructions](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel) in the test panel for setting the key.
-->
## <a name="adding-the-key-to-the-endpoint-url"></a>Anahtar uç noktası URL'ye ekleniyor
Uç nokta sorgu yazım düzeltmeleri uygulamak istediğiniz her sorgu için sorgu dizesi parametrelerinde geçirilen anahtar gerekir. LUIS çağıran bir sohbet Robotu olabilir veya LUIS API'si uç noktası doğrudan çağırabilir. Uç nokta nasıl çağrıldığında bağımsız olarak her çağrı için yazım düzeltmeleri düzgün çalışması gereken bilgileri içermelidir.

Uç nokta URL'si doğru geçirilmesi gereken birkaç değer sahiptir. Bing yazım denetimi API'si v7 bunlardan yalnızca başka bir anahtardır. **SpellCheck** parametresini true olarak ayarlamanız gerekir ve **Bing-yazım denetimi-abonelik-anahtarının** değerini anahtar değere ayarlamanız gerekir:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appID}?subscription-key={luisKey}&spellCheck=**true**&bing-spell-check-subscription-key=**{bingKey}**&verbose=true&timezoneOffset=0&q={utterance}`

## <a name="send-misspelled-utterance-to-luis"></a>LUIS için yazılmış utterance Gönder
1. Bir Web tarayıcısında, önceki dizeyi kopyalayın ve `region`, `appId`, `luisKey`ve `bingKey` kendi değerlerinizle değiştirin. Yayımlama [bölgeinizden](luis-reference-regions.md)farklıysa, uç nokta bölgesini kullandığınızdan emin olun.

2. Yanlış yazılmış utterance gibi "ne kadar mountainn?" ekleyin. Ingilizce, tek bir `n``mountain`doğru yazıdır. 

3. LUIS için sorgu göndermeyi seçin girin.

4. Lua, `How far is the mountain?`için bir JSON sonucuyla yanıt verir. Bing Yazım Denetimi API'si v7 yanlış bir sorun algılarsa, LUSıS uygulamasının JSON yanıtında bulunan `query` alanı özgün sorguyu içerir ve `alteredQuery` alanı LUO 'ya gönderilen düzeltilen sorguyu içerir.

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

Bing Yazım Denetimi API'si v7 hizmetini kullanmak istemiyorsanız, doğru ve yanlış yazım denetimi eklemeniz gerekir. 

İki çözüm vardır:

* Her türlü farklı yazımlar içeren, LUTO 'ın doğru yazım ve yazım hataları öğrenmeleri için etiketlenmesi gereken her türlü etiket örneği. Bu seçenek bir yazım denetleyicisi kullanmaktan daha fazla etiketleme çaba gerektirir.
* Sözcüğün tüm çeşitlemelerine sahip bir tümcecik listesi oluşturun. Bu çözümle, örnek çeşitlerdeki sözcük çeşitlemelerini etiketlemenize gerek yoktur. 

## <a name="publishing-page"></a>Yayımlama Sayfası
[Yayımlama](luis-how-to-publish-app.md) sayfasında **Bing yazım denetimcisini etkinleştir** onay kutusu bulunur. Anahtar oluşturun ve uç nokta URL'sini nasıl değiştiğini anlamak için bir kolaylık budur. Yine de her utterance için düzeltilen yazım sahip olmak için doğru uç noktaya parametreleri kullanmak zorunda. 

> [!div class="nextstepaction"]
> [Örnek araslar hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
