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
ms.openlocfilehash: a199821c4db7fd8131ec54700b8c999dfe604a6e
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74222016"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>Öncesinde veya sırasında tahmin utterance verileri değiştirme
LUIS, öncesinde veya sırasında tahmin utterance işlemek için yöntemler sağlar. Bunlar, [yazımı düzeltmeyi](luis-tutorial-bing-spellcheck.md)ve önceden oluşturulmuş [datetimeV2](luis-reference-prebuilt-datetimev2.md)için saat dilimi sorunlarını düzeltmeyi içerir. 

## <a name="correct-spelling-errors-in-utterance"></a>Utterance yazarken yazım hataları

[!INCLUDE [Not supported in V3 API prediction endpoint](./includes/v2-support-only.md)]


LUO, yazıdaki yazım hatalarını düzeltmek için [Bing yazım denetimi API'si v7](../Bing-Spell-Check/overview.md) kullanır. LUIS, hizmetle ilişkili anahtar gerekir. Anahtarı oluşturun ve sonra anahtar [noktasında](https://go.microsoft.com/fwlink/?linkid=2092356)QueryString parametresi olarak anahtarı ekleyin. 

<!--
You can also correct spelling errors in the **Test** panel by [entering the key](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). The key is kept as a session variable in the browser for the Test panel. Add the key to the Test panel in each browser session you want spelling corrected. 

Usage of the key in the test panel and at the endpoint count toward the [key usage](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) quota. LUIS implements Bing Spell Check limits for text length. 

-->

Uç nokta için yazım düzeltmeleri çalışmak iki params gerektirir:

|param|Value|
|--|--|
|`spellCheck`|Boole değeri|
|`bing-spell-check-subscription-key`|[Bing yazım denetimi API'si v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) uç noktası anahtarı|

[Bing yazım denetimi API'si v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) bir hata algıladığında, orijinal söylenişi ve düzeltilen söylenişi, uç noktadan tahmine göre birlikte döndürülür.

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
Saat dilimi, kullanıcının saat dilimine `timezoneOffset` param kullanılarak [uç noktaya](https://go.microsoft.com/fwlink/?linkid=2092356) eklenerek düzeltilir. `timezoneOffset` değeri, zaman değiştirmek için dakika cinsinden pozitif veya negatif bir sayı olmalıdır.  

|param|Value|
|--|--|
|`timezoneOffset`|dakikalar içinde pozitif veya negatif sayı|

### <a name="daylight-savings-example"></a>Gün ışığından tasarruf örneği
Gün ışığından yararlanma süresini ayarlamak için döndürülen önceden oluşturulmuş datetimeV2 gerekiyorsa, [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356) sorgusu için dakikalar içinde `timezoneOffset` QueryString parametresini kullanmanız gerekir.

#### <a name="v2-prediction-endpoint-requesttabv2"></a>[V2 tahmin uç noktası isteği](#tab/V2)

60 dakika ekleyin: 

https://{Region}. api. bilişsel. Microsoft. com/lusıs/v 2.0/Apps/{AppID}? q = ışıkları açık duruma getirin mi? **Timezonekayması = 60**& verbose = {boolean} & SpellCheck = {boolean} & hazırlama = {boolean} & Bing-Yazım-Denetim-abonelik-anahtar = {string} & günlük = {Boolean}

60 dakika kaldırın: 

https://{Region}. api. bilişsel. Microsoft. com/lusıs/v 2.0/Apps/{AppID}? q = ışıkları açık duruma getirin mi? **Timezonekayması =-60**& verbose = {boolean} & SpellCheck = {boolean} & hazırlama = {boolean} & Bing-yazım denetimi-abonelik-anahtar = {string} & günlük = {Boolean}

#### <a name="v3-prediction-endpoint-requesttabv3"></a>[V3 tahmin uç noktası isteği](#tab/V3)

60 dakika ekleyin:

https://{Region}. api. bilişsel. Microsoft. com/LUG/v 3.0-Önizleme/uygulamalar/{AppID}/yuvalar/üretim/tahmin? sorgu = ışıkları açık duruma getirin mi? **Timezonekayması = 60**& SpellCheck = {boolean} & Bing-yazım denetimi-abonelik-anahtar = {string} & günlük = {Boolean}

60 dakika kaldırın: 

https://{Region}. api. bilişsel. Microsoft. com/LUG/v 3.0-Önizleme/uygulamalar/{AppID}/yuvalar/üretim/tahmin? sorgu = ışıkları açık duruma getirin mi? **Timezonekayması =-60**& SpellCheck = {boolean} & Bing-yazım denetimi-abonelik-anahtar = {dize} & Log = {Boolean}

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * * 

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C# kodu timezoneOffset doğru değerini belirler.
Aşağıdaki C# kod, sistem saatine göre doğru `timezoneOffset` belirlenmesi Için [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo) sınıfının [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples) metodunu kullanır:

```CSharp
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
> [Bu öğreticide yazım hatalarını düzeltin](luis-tutorial-bing-spellcheck.md)
