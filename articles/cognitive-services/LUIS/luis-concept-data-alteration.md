---
title: Veri değişikliği-LUSıS
description: Verilerin Language Understanding tahminlerden önce nasıl değiştirilebileceğinizi öğrenin (LUSıS)
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: b3b36351a64a4e1a0bd13d5785a4e0609a80901d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80292073"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>Söylenişi verilerini tahmine göre veya tahmin sırasında değiştirme
LUO, tahmine göre veya tahmin sırasında zaman içinde değişiklik yapmak için yollar sağlar. Bunlar, [yazımı düzeltmeyi](luis-tutorial-bing-spellcheck.md)ve önceden oluşturulmuş [datetimeV2](luis-reference-prebuilt-datetimev2.md)için saat dilimi sorunlarını düzeltmeyi içerir.

## <a name="correct-spelling-errors-in-utterance"></a>Söylenişi 'te yazım hatalarını düzeltme


### <a name="v3-runtime"></a>V3 çalışma zamanı

Deterance 'i LUSıS 'e göndermeden önce yazım düzeltmeleri için metin ön işleme. Doğru tahminlere sahip olduğunuzdan emin olmak için doğru yazılarla örnek söylemeleri kullanın.

[Bing yazım denetimi](../bing-spell-check/overview.md) kullanarak lusıs 'e göndermeden önce metni düzeltin.

### <a name="prior-to-v3-runtime"></a>V3 çalışma zamanından önce

LUO, yazıdaki yazım hatalarını düzeltmek için [Bing yazım denetimi API'si v7](../Bing-Spell-Check/overview.md) kullanır. LUSıS bu hizmetle ilişkili anahtara ihtiyaç duyuyor. Anahtarı oluşturun ve sonra anahtar [noktasında](https://go.microsoft.com/fwlink/?linkid=2092356)QueryString parametresi olarak anahtarı ekleyin.

Uç nokta, yazım düzeltmelerinin çalışması için iki params gerektirir:

|Param|Değer|
|--|--|
|`spellCheck`|boole|
|`bing-spell-check-subscription-key`|[Bing yazım denetimi API'si v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) uç noktası anahtarı|

[Bing yazım denetimi API'si v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) bir hata algıladığında, orijinal söylenişi ve düzeltilen söylenişi, uç noktadan tahmine göre birlikte döndürülür.

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

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

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)

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

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Önceden oluşturulmuş datetimeV2 varlığının saat dilimini değiştirme
Bir LUSıS uygulaması önceden oluşturulmuş [datetimeV2](luis-reference-prebuilt-datetimev2.md) varlığını kullandığında, tahmin yanıtında bir tarih saat değeri döndürülebilir. İsteğin saat dilimi, döndürülecek doğru tarih/saati belirlemekte kullanılır. İstek bir bot 'tan veya başka bir merkezi uygulamadan geliyorsa, lusıs 'e geçmeden önce, LUSıS 'nin kullandığı saat dilimini düzeltin.

### <a name="endpoint-querystring-parameter"></a>Endpoint QueryString parametresi
Saat dilimi, kullanıcının saat dilimine `timezoneOffset` param kullanılarak [uç noktaya](https://go.microsoft.com/fwlink/?linkid=2092356) eklenerek düzeltilir. Değeri `timezoneOffset` , zaman değiştirmek için dakika cinsinden pozitif veya negatif bir sayı olmalıdır.

|Param|Değer|
|--|--|
|`timezoneOffset`|dakika cinsinden pozitif veya negatif sayı|

### <a name="daylight-savings-example"></a>Günışığından tasarruf örneği
Gün ışığından yararlanma süresini ayarlamak için döndürülen önceden oluşturulmuş datetimeV2 gerekiyorsa, [uç nokta](https://go.microsoft.com/fwlink/?linkid=2092356) sorgusu için dakika cinsinden bir `timezoneOffset` +/-değeri ile QueryString parametresini kullanmanız gerekir.

#### <a name="v2-prediction-endpoint-request"></a>[V2 tahmin uç noktası isteği](#tab/V2)

60 dakika ekle:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

60 dakika kaldır:

`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q=Turn the lights on?**timezoneOffset=-60**&verbose={boolean}&spellCheck={boolean}&staging={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

#### <a name="v3-prediction-endpoint-request"></a>[V3 tahmin uç noktası isteği](#tab/V3)

60 dakika ekle:

`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

60 dakika kaldır:

`https://{region}.api.cognitive.microsoft.com/luis/v3.0-preview/apps/{appId}/slots/production/predict?query=Turn the lights on?**timezoneOffset=-60**&spellCheck={boolean}&bing-spell-check-subscription-key={string}&log={boolean}`

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

* * *

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C# kodu, Timezonesapmasını doğru değerini belirler
Aşağıdaki C# kodu, sistem saatine göre doğru `timezoneOffset` şekilde belirlenmesi Için [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo) sınıfının [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples) metodunu kullanır:

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
> [Bu öğreticide yazım hatalarını düzeltin](luis-tutorial-bing-spellcheck.md)
