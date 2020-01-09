---
title: Veri değişikliği-LUSıS
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) Öngörüler önce verileri nasıl değiştirilebilir öğrenin
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: diberry
ms.openlocfilehash: 1bde70dadbe1e5b8ba9bf90bd9ca2f48a4c65491
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75381809"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>Öncesinde veya sırasında tahmin utterance verileri değiştirme
LUIS, öncesinde veya sırasında tahmin utterance işlemek için yöntemler sağlar. Bunlar, [yazımı düzeltmeyi](luis-tutorial-bing-spellcheck.md)ve önceden oluşturulmuş [datetimeV2](luis-reference-prebuilt-datetimev2.md)için saat dilimi sorunlarını düzeltmeyi içerir. 

## <a name="correct-spelling-errors-in-utterance"></a>Utterance yazarken yazım hataları

[!INCLUDE [Not supported in V3 API prediction endpoint](./includes/v2-support-only.md)]


LUIS kullanan [Bing yazım denetimi API'si V7](../Bing-Spell-Check/overview.md) utterance yazım hatalarını düzeltmek için. LUIS, hizmetle ilişkili anahtar gerekir. Anahtar oluşturun, sonra anahtarın bir sorgu dizesi parametresi olarak ekleyin [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356). 

<!--
You can also correct spelling errors in the **Test** panel by [entering the key](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). The key is kept as a session variable in the browser for the Test panel. Add the key to the Test panel in each browser session you want spelling corrected. 

Usage of the key in the test panel and at the endpoint count toward the [key usage](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) quota. LUIS implements Bing Spell Check limits for text length. 

-->

Uç nokta için yazım düzeltmeleri çalışmak iki params gerektirir:

|param|Değer|
|--|--|
|`spellCheck`|boole|
|`bing-spell-check-subscription-key`|[Bing yazım denetimi API'si V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) uç noktası anahtarı|

Zaman [Bing yazım denetimi API'si V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) bir hata, özgün utterance ve düzeltilmiş utterance döndürülür tahminlerinin yanı sıra uç noktasından algılar.

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)
 
```JSON
{
    "query": "Book a flite to London?",
    "prediction": {
        "normalizedQuery": "book a flight to london?",
        "topIntent": "BookFlight",
        "intents": {
            "BookFlight": {
                "score": 0.780123
            }
        },
        "entities": {},
    }
}
```

* * * 

### <a name="list-of-allowed-words"></a>İzin verilen sözcüklerin listesi
LUSıS 'de kullanılan Bing yazım denetimi API 'SI, yazım denetimi değişiklikleri sırasında yoksayılacak sözcüklerin listesini desteklemez. Sözcüklerin veya kısaltmalardan oluşan bir listeye izin vermeniz gerekiyorsa, duyun amaç tahmini için, duyun, duyun, duyun bir tahmin için gönderilmesi için istemci uygulamadaki söyliği işleyin.

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Önceden oluşturulmuş datetimeV2 varlık saat dilimini değiştirme
Bir LUSıS uygulaması önceden oluşturulmuş [datetimeV2](luis-reference-prebuilt-datetimev2.md) varlığını kullandığında, tahmin yanıtında bir tarih saat değeri döndürülebilir. Saat dilimi isteğin döndürmek için doğru datetime belirlemek için kullanılır. İstek bir bot veya alma için LUIS önce başka bir merkezi uygulamasından geliyorsa LUIS kullanır saat dilimi düzeltin. 

### <a name="endpoint-querystring-parameter"></a>Uç nokta querystring parametresi
Kullanıcının saat dilimine ekleyerek saat dilimi düzeltilene [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356) kullanarak `timezoneOffset` param. Değerini `timezoneOffset` saati değiştirmek için dakikalar içinde pozitif veya negatif sayı olmalıdır.  

|param|Değer|
|--|--|
|`timezoneOffset`|dakikalar içinde pozitif veya negatif sayı|

### <a name="daylight-savings-example"></a>Gün ışığından tasarruf örneği
Gün ışığından yararlanma saatine ayarlamak için döndürülen önceden oluşturulmuş datetimeV2 gerekiyorsa kullanmalısınız `timezoneOffset` querystring parametresi ile bir değer dakika cinsinden +/- [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356) sorgu.

#### <a name="v2-prediction-endpoint-requesttabv2"></a>[V2 tahmin uç noktası isteği](#tab/V2)

60 dakika ekleyin: 

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/Apps/{appId}?q=Turn ışıkları Aç? **timezoneOffset = 60**& ayrıntılı {boolean} = & Yazım denetimi {boolean} = & hazırlama {boolean} = & bing-yazım-onay-subscription-key = {string} gün & lüğü {boolean} =

60 dakika kaldırın: 

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/Apps/{appId}?q=Turn ışıkları Aç? **timezoneOffset = 60**& ayrıntılı {boolean} = & Yazım denetimi {boolean} = & hazırlama {boolean} = & bing-yazım-onay-subscription-key = {string} gün & lüğü {boolean} =

#### <a name="v3-prediction-endpoint-requesttabv3"></a>[V3 tahmin uç noktası isteği](#tab/V3)

60 dakika ekleyin:

https://{Region}. api. bilişsel. Microsoft. com/LUG/v 3.0-Önizleme/uygulamalar/{AppID}/yuvalar/üretim/tahmin? sorgu = ışıkları açık duruma getirin mi? **Timezonekayması = 60**& SpellCheck = {boolean} & Bing-yazım denetimi-abonelik-anahtar = {string} & günlük = {Boolean}

60 dakika kaldırın: 

https://{Region}. api. bilişsel. Microsoft. com/LUG/v 3.0-Önizleme/uygulamalar/{AppID}/yuvalar/üretim/tahmin? sorgu = ışıkları açık duruma getirin mi? **Timezonekayması =-60**& SpellCheck = {boolean} & Bing-yazım denetimi-abonelik-anahtar = {dize} & Log = {Boolean}

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * * 

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C# kodu timezoneOffset doğru değerini belirler.
Aşağıdaki C# kod [Timezoneınfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo) sınıfın [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples) doğru belirlemek için yöntemi `timezoneOffset` sistem saatini temel alan:

```csharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu öğretici ile doğru yazım hatalarını](luis-tutorial-bing-spellcheck.md)
