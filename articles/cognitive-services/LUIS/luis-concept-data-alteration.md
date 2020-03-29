---
title: Veri değişikliği - LUIS
description: Dil Anlama (LUIS) tahminlerinden önce verilerin nasıl değiştirilebileceğini öğrenin
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: b3b36351a64a4e1a0bd13d5785a4e0609a80901d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292073"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>Tahminden önce veya tahmin sırasında söyleyiş verilerini değiştirme
LUIS, tahminden önce veya tahmin sırasında söyleyiyi manipüle etmenin yollarını sağlar. Bunlar yazım [düzeltme](luis-tutorial-bing-spellcheck.md)ve önceden oluşturulmuş [datetimeV2](luis-reference-prebuilt-datetimev2.md)için saat dilimi sorunları sabitleme içerir.

## <a name="correct-spelling-errors-in-utterance"></a>Söyleyişteki yazım hatalarını düzeltme


### <a name="v3-runtime"></a>V3 çalışma süresi

Söyleyiyi LUIS'e göndermeden önce yazım düzeltmeleri için ön işlem metni. Doğru tahminleri aldığınızdan emin olmak için doğru yazımla örnek sözcükleri kullanın.

METNI LUIS'e göndermeden önce düzeltmek için [Bing Yazım Denetimi'ni](../bing-spell-check/overview.md) kullanın.

### <a name="prior-to-v3-runtime"></a>V3 çalışma saatinden önce

LUIS, söyleyişdeki yazım hatalarını düzeltmek için [Bing Yazım Denetimi API V7'yi](../Bing-Spell-Check/overview.md) kullanır. LUIS'in bu hizmetle ilişkili anahtara ihtiyacı vardır. Anahtarı oluşturun, ardından [bitiş noktasına](https://go.microsoft.com/fwlink/?linkid=2092356)sorgu sonu parametresi olarak anahtarı ekleyin.

Bitiş noktası çalışması için yazım düzeltmeleri için iki param gerektirir:

|Param|Değer|
|--|--|
|`spellCheck`|boole|
|`bing-spell-check-subscription-key`|[Bing Yazım Denetimi API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) uç noktası anahtarı|

[Bing Yazım Denetimi API V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) bir hata algıladığında, özgün söyleyiş ve düzeltilen söyleyiş bitiş noktasından gelen öngörülerle birlikte döndürülür.

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

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

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

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

### <a name="list-of-allowed-words"></a>İzin verilen sözcüklerlistesi
LUIS'te kullanılan Bing yazım denetimi API, yazım denetimi değişiklikleri sırasında göz ardı edilen sözcüklerin listesini desteklemez. Kelimelerin veya kısaltmaların listesine izin vermeniz gerekiyorsa, kasıtlı tahmin için söyleyişi LUIS'e göndermeden önce istemci uygulamasındaki söyleyiyi işlemeye devam edin.

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Önceden oluşturulmuş datetimeV2 varlığının saat dilimini değiştirme
Bir LUIS uygulaması önceden oluşturulmuş [datetimeV2](luis-reference-prebuilt-datetimev2.md) varlığını kullandığında, tahmin yanıtında bir datetime değeri döndürülebilir. İsteğin saat dilimi, dönmek için doğru tarih saatini belirlemek için kullanılır. İstek LUIS'e gelmeden önce bir bottan veya başka bir merkezi uygulamadan geliyorsa, LUIS'in kullandığı saat dilimini düzeltin.

### <a name="endpoint-querystring-parameter"></a>Uç nokta sorgusustring parametresi
Saat `timezoneOffset` dilimi, kullanıcının saat dilimini param kullanılarak [bitiş noktasına](https://go.microsoft.com/fwlink/?linkid=2092356) ekleyerek düzeltilir. Değeri pozitif `timezoneOffset` veya negatif sayı olmalıdır, dakika içinde, zaman değiştirmek için.

|Param|Değer|
|--|--|
|`timezoneOffset`|pozitif veya negatif sayı, dakika içinde|

### <a name="daylight-savings-example"></a>Gün ışığından yararlanma örneği
Gün ışığından yararlanma saatini ayarlamak için döndürülen önceden oluşturulmuş datetimeV2'ye ihtiyacınız varsa, bitiş `timezoneOffset` [noktası](https://go.microsoft.com/fwlink/?linkid=2092356) sorgusu için dakika içinde +/- değeri olan sorgu dize parametresini kullanmanız gerekir.

#### <a name="v2-prediction-endpoint-request"></a>[V2 tahmini bitiş noktası isteği](#tab/V2)

60 dakika ekleyin:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

60 dakika yıkın:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=-60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

#### <a name="v3-prediction-endpoint-request"></a>[V3 tahmini bitiş noktası isteği](#tab/V3)

60 dakika ekleyin:

`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

60 dakika yıkın:

`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=-60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C# kodu zaman dilimiOft'un doğru değerini belirler
Aşağıdaki C# kodu, sistem zamanına göre doğrubelirlemek `timezoneOffset` için [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo) sınıfının [FindSystemZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples) yöntemini kullanır:

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
> [Bu öğretici ile yazım hatalarını düzeltme](luis-tutorial-bing-spellcheck.md)
