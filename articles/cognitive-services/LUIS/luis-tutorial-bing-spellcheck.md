---
title: Yanlış yazılmış kelimeleri Düzelt-LUSıS
titleSuffix: Azure Cognitive Services
description: Lug uç nokta sorgularını Bing Yazım Denetimi API'si v7 ekleyerek, yazılanlara yanlış yazılan sözcükleri düzeltin.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 01/12/2021
ms.openlocfilehash: f416fe8ef4f6e89d07e6065d4c9435642d9bacb9
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98179648"
---
# <a name="correct-misspelled-words-with-bing-search-resource"></a>Hatalı yazılan kelimeleri Bing Arama kaynakla düzeltin

Luve 'nın, utterlik 'in Puanını ve varlıklarını tahmin edebilmesi için söyleştirilen yanlış yazılan sözcükleri düzeltmek üzere [Bing arama](https://ms.portal.azure.com/#create/Microsoft.BingSearch) ile lusıs uygulamanızı tümleştirebilirsiniz.

## <a name="create-endpoint-key"></a>Uç nokta anahtarı oluştur

Azure portal Bing Arama kaynak oluşturmak için aşağıdaki yönergeleri izleyin:

1. [Azure Portal](https://portal.azure.com)oturum açın.

2. Sol üst köşedeki **kaynak oluştur** ' u seçin.

3. Arama kutusuna `Bing Search V7` hizmeti girin ve hizmeti seçin.

4. Yasal bildirim dahil olmak üzere bilgiler içeren bir bilgi paneli görüntülenir. Abonelik oluşturma işlemini başlatmak için **Oluştur** ' u seçin.

    :::image type="content" source="./media/luis-tutorial-bing-spellcheck/bing-search-resource-portal.png" alt-text="Bing Yazım Denetimi API'si v7 kaynağı":::

5. Sonraki panelde hizmet ayarlarınızı girin. Hizmet oluşturma işleminin bitmesini bekleyin.

6. Kaynak oluşturulduktan sonra, sol taraftaki **anahtarlar ve uç nokta** dikey penceresine gidin. 

7. Tahmin isteğiniz üstbilgisine eklenecek anahtarlardan birini kopyalayın. Yalnızca iki anahtardan birine ihtiyacınız olacak.

8. Anahtarı `mkt-bing-spell-check-key` tahmin isteği üst bilgisinde ekleyin.

<!--
## Using the key in LUIS test panel
There are two places in LUIS to use the key. The first is in the [test panel](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). The key isn't saved into LUIS but instead is a session variable. You need to set the key every time you want the test panel to apply the Bing Spell Check API v7 service to the utterance. See [instructions](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel) in the test panel for setting the key.
-->
## <a name="adding-the-key-to-the-endpoint-url"></a>Uç nokta URL 'sine anahtar ekleniyor
Yazım denetimi uygulamak istediğiniz her sorgu için, uç nokta sorgusunun sorgu üst bilgisi parametresinde bulunan Bing yazım denetimi kaynak anahtarına ihtiyacı vardır. Luya çağıran bir sohbet botu olabilir veya doğrudan LUO Endpoint API 'sini çağırabilirsiniz. Uç noktanın nasıl çağrıldığına bakılmaksızın, her bir çağrı, yazım düzeltmelerinin düzgün şekilde çalışması için üstbilginin isteğine gerekli bilgileri içermelidir. Değeri, **Mkt-Bing-yazım denetimi-anahtarı** ile anahtar değerine ayarlamanız gerekir.


## <a name="send-misspelled-utterance-to-luis"></a>Yanlış yazılmış söylenişi 'i lusıs 'e gönder
1. Tahmin sorgusunda, "dağın ne kadar uzakta?" gibi gönderilecek bir söylenişi ekleyin. Ingilizce olarak,, `mountain` ile `n` doğru bir yazıdır.

2. Lua, için bir JSON sonucuyla yanıt verir `How far is the mountain?` . Bing Yazım Denetimi API'si v7 yanlış bir sorun algılarsa, `query` lusıs UYGULAMASıNıN JSON yanıtında bulunan alan özgün sorguyu içerir ve `alteredQuery` alan, Luo 'ya gönderilen düzeltilen sorguyu içerir.

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

Bing Arama API v7 hizmetini kullanmak istemiyorsanız doğru ve yanlış yazım denetimi eklemeniz gerekir.

İki çözüm vardır:

* Her türlü farklı yazımlar içeren, LUTO 'ın doğru yazım ve yazım hataları öğrenmeleri için etiketlenmesi gereken her türlü etiket örneği. Bu seçenek, yazım denetleyicisi kullanmaktan daha fazla etiketleme çabası gerektirir.
* Sözcüğün tüm çeşitlemelerine sahip bir tümcecik listesi oluşturun. Bu çözümle, örnek çeşitlerdeki sözcük çeşitlemelerini etiketlemenize gerek yoktur.


> [!div class="nextstepaction"]
> [Örnek araslar hakkında daha fazla bilgi edinin](./luis-how-to-add-entities.md)
