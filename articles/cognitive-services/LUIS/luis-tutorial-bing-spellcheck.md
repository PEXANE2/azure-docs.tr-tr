---
title: Yanlış yazılmış kelimeleri Düzelt-LUSıS
titleSuffix: Azure Cognitive Services
description: Lug uç nokta sorgularını Bing Yazım Denetimi API'si v7 ekleyerek, yazılanlara yanlış yazılan sözcükleri düzeltin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 9c8babac8450bdfd170d3d18b338ba3d64383a67
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499037"
---
# <a name="correct-misspelled-words-with-bing-spell-check"></a>Yanlış yazılmış kelimeleri Bing Yazım Denetimi düzeltin

Luve 'nın, utterlik 'in Puanını ve varlıklarını tahmin edebilmesi için söyleştirilen yanlış yazılan sözcükleri düzeltmek üzere [Bing yazım denetimi API'si v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) ile bir tümleştirme yapabilirsiniz. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

[!INCLUDE [Not supported in V3 API prediction endpoint](./includes/v2-support-only.md)]


## <a name="create-first-key-for-bing-spell-check-v7"></a>Bing Yazım Denetimi v7 için ilk anahtar oluştur
[İlk Bing yazım denetimi API'si v7 anahtarınız](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) ücretsizdir. 

![Serbest anahtar oluştur](./media/luis-tutorial-bing-spellcheck/free-key.png)

"Create-Subscription-Key" > bir ad <</a>
## <a name="create-endpoint-key"></a>Uç nokta anahtarı oluştur
Ücretsiz anahtarınızın geçerliliği dolmuşsa bir uç nokta anahtarı oluşturun.

1. [Azure Portal](https://portal.azure.com)’da oturum açın. 

2. Sol üst köşedeki **kaynak oluştur** ' u seçin.

3. Arama kutusuna `Bing Spell Check API V7` yazın.

    ![Bing Yazım Denetimi API'si v7 arayın](./media/luis-tutorial-bing-spellcheck/portal-search.png)

4. Hizmeti seçin. 

5. Yasal bildirim dahil olmak üzere bilgiler içeren bir bilgi paneli görüntülenir. Abonelik oluşturma işlemini başlatmak için **Oluştur** ' u seçin. 

6. Sonraki panelde hizmet ayarlarınızı girin. Hizmet oluşturma işleminin bitmesini bekleyin.

    ![Hizmet ayarlarını girin](./media/luis-tutorial-bing-spellcheck/subscription-settings.png)

7. Sol taraftaki gezinmede **Sık Kullanılanlar** başlığı altındaki **tüm kaynaklar** ' ı seçin.

8. Yeni hizmeti seçin. Türü bilişsel **Hizmetler** ve konum **geneldir**. 

9. Yeni anahtarlarınızı görmek için ana bölmede **anahtarlar** ' ı seçin.

    ![Anahtarları al](./media/luis-tutorial-bing-spellcheck/grab-keys.png)

10. İlk anahtarı kopyalayın. Yalnızca iki anahtardan birine ihtiyacınız vardır. 

## <a name="using-the-key-in-luis-test-panel"></a>LUSıS test panelinde anahtarı kullanma
LUSıS 'de anahtarı kullanmak için iki konum vardır. İlki [Test panelinde](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel)bulunur. Anahtar, LUO 'da kaydedilmez ancak bunun yerine bir oturum değişkenidir. Test panelinin Bing Yazım Denetimi API'si v7 hizmetini utterance 'e uygulamasını istediğiniz her seferinde anahtarı ayarlamanız gerekir. Anahtarı ayarlamak için test panelindeki [yönergelere](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel) bakın.

## <a name="adding-the-key-to-the-endpoint-url"></a>Uç nokta URL 'sine anahtar ekleniyor
Uç nokta sorgusu, yazım denetimi uygulamak istediğiniz her sorgu için sorgu dizesi parametrelerinde geçirilen anahtara sahip olmalıdır. Luya çağıran bir sohbet botu olabilir veya doğrudan LUO Endpoint API 'sini çağırabilirsiniz. Uç noktanın nasıl çağrıldığına bakılmaksızın her bir çağrı, yazım düzeltmelerinin düzgün çalışması için gerekli bilgileri içermelidir.

Uç nokta URL 'sinin doğru geçirilmesi gereken birkaç değeri vardır. Bing Yazım Denetimi API'si v7 anahtarı, bunlardan yalnızca başka biridir. **SpellCheck** parametresini true olarak ayarlamanız gerekir ve **Bing-yazım denetimi-abonelik-anahtarının** değerini anahtar değere ayarlamanız gerekir:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appID}?subscription-key={luisKey}&spellCheck=**true**&bing-spell-check-subscription-key=**{bingKey}**&verbose=true&timezoneOffset=0&q={utterance}`

## <a name="send-misspelled-utterance-to-luis"></a>Yanlış yazılmış söylenişi 'i lusıs 'e gönder
1. Bir Web tarayıcısında, önceki dizeyi kopyalayın ve `region`, `appId`, `luisKey`ve `bingKey` kendi değerlerinizle değiştirin. Yayımlama [bölgeinizden](luis-reference-regions.md)farklıysa, uç nokta bölgesini kullandığınızdan emin olun.

2. "DAĞINCI nedir?" gibi yanlış yazılmış bir söylenişi ekleyin. Ingilizce, tek bir `n``mountain`doğru yazıdır. 

3. Sorguyu LUSıS 'e göndermek için ENTER ' u seçin.

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

* Her türlü farklı yazımlar içeren, LUTO 'ın doğru yazım ve yazım hataları öğrenmeleri için etiketlenmesi gereken her türlü etiket örneği. Bu seçenek, yazım denetleyicisi kullanmaktan daha fazla etiketleme çabası gerektirir.
* Sözcüğün tüm çeşitlemelerine sahip bir tümcecik listesi oluşturun. Bu çözümle, örnek çeşitlerdeki sözcük çeşitlemelerini etiketlemenize gerek yoktur. 

## <a name="publishing-page"></a>Yayımlama sayfası
[Yayımlama](luis-how-to-publish-app.md) sayfasında **Bing yazım denetimcisini etkinleştir** onay kutusu bulunur. Bu, anahtarı oluşturmak ve uç nokta URL 'sinin nasıl değiştirileceğini anlamak için kolaylık sağlar. Her bir yazımla ilgili yazım düzeltilmesi için doğru uç nokta parametrelerini yine de kullanmanız gerekir. 

> [!div class="nextstepaction"]
> [Örnek araslar hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
