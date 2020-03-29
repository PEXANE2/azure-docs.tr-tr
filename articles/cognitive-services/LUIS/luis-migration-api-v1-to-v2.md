---
title: v1 için v2 API Geçişi
titleSuffix: Azure Cognitive Services
description: Sürüm 1 bitiş noktası ve yazma Dil Anlama API'leri amortismana alınır. Sürüm 2 bitiş noktasına nasıl geçirilip API'leri yazarak nasıl geçirilenleri anlamak için bu kılavuzu kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: 2f67bf0951ef8928297c71e8fc9f924cf05c63f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68932685"
---
# <a name="api-v1-to-v2-migration-guide-for-luis-apps"></a>LUIS uygulamaları için API v1 - v2 Geçiş kılavuzu
Sürüm 1 [bitiş noktası](https://aka.ms/v1-endpoint-api-docs) ve [yazma](https://aka.ms/v1-authoring-api-docs) API'leri amortismana alınır. Sürüm 2 [bitiş noktasına](https://go.microsoft.com/fwlink/?linkid=2092356) nasıl geçirilip API'leri yazarak nasıl [geçirilenleri](https://go.microsoft.com/fwlink/?linkid=2092087) anlamak için bu kılavuzu kullanın. 

## <a name="new-azure-regions"></a>Yeni Azure bölgeleri
LUIS, LUIS API'leri için yeni [bölgelere](https://aka.ms/LUIS-regions) sahiptir. LUIS bölge grupları için farklı bir portal sağlar. Uygulama, sorgulamayı beklediğiniz aynı bölgede yazılmalıdır. Uygulamalar bölgeleri otomatik olarak geçirmiyor. Uygulamayı bir bölgeden dışa aktarırsınız, ardından yeni bir bölgede kullanılabilsi için başka bir bölgeye aktarırsınız.

## <a name="authoring-route-changes"></a>Rota değişikliklerini yazma
Yazma API rotası **prog** rotasını kullanmaktan **api** rotasını kullanmaya değiştirildi.


| version | yol |
|--|--|
|1|/luis/v1.0/**prog**/apps|
|2|/luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Bitiş noktası rota değişiklikleri
Bitiş noktası API'si yeni sorgu dize parametrelerinin yanı sıra farklı bir yanıta sahiptir. Ayrıntılı bayrak doğruysa, skorne bakılmaksızın tüm niyetler, topScoringIntent'e ek olarak niyet adlı bir dizide döndürülür.

| version | GET rotası |
|--|--|
|1|/luis/v1/application?ID={appId}&q={q}|
|2|/luis/v2.0/apps/{appId}?q={q}[&zaman dilimiOffset][&verbose][&yazımCheck][&evreleme][&bing-spell-check-subscription-key][&log]|


v1 uç nokta başarı yanıtı:
```json
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

v2 uç nokta başarı yanıtı:
```json
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>Anahtar yönetimi artık API'de yok
Abonelik bitiş noktası anahtarı API'leri amortismana alınır ve 410 GONE döndürülür.

| version | yol |
|--|--|
|1|/luis/v1.0/prog/abonelik|
|1|/luis/v1.0/prog/abonelikler/{abonelikKey}|

Azure [son nokta anahtarları](luis-how-to-azure-subscription.md) Azure portalında oluşturulur. Anahtarı **[Yayımla](luis-how-to-azure-subscription.md)** sayfasındaki bir LUIS uygulamasına atarsınız. Gerçek anahtar değerini bilmeniz gerekmez. LUIS atama yapmak için abonelik adını kullanır. 

## <a name="new-versioning-route"></a>Yeni sürüm rotası
V2 modeli artık bir [sürümde](luis-how-to-manage-versions.md)yer almaktadır. Sürüm adı, rotadaki 10 karakterdir. Varsayılan sürüm "0.1"dir.

| version | yol |
|--|--|
|1|/luis/v1.0/**prog**/apps/{appId}/varlıklar|
|2|/luis/**api**/v2.0/apps/{appId}/**sürümleri**/{versionId}/varlıklar|

## <a name="metadata-renamed"></a>Meta verilerin adı yeniden adlandırıldı
LUIS meta verilerini döndüren birkaç API'nin yeni adları vardır.

| v1 rota adı | v2 rota adı |
|--|--|
|PersonalAssistantApps |Asistan|
|uygulama kültürleri|kültürler|
|uygulama etki alanları|Etki alanları|
|uygulama kullanım senaryoları|kullanım senaryoları|


## <a name="sample-renamed-to-suggest"></a>"Örnek" "önermek" olarak yeniden adlandırıldı
LUIS modeli geliştirebilir mevcut [uç nokta söyleyişlerinden](luis-how-to-review-endpoint-utterances.md) gelen söyleyişler önerir. Önceki sürümde, bu **örnek**seçildi. Yeni sürümde, ad örnekten **önermek**için değiştirilir. Bu LUIS web sitesinde **[Gözden geçirin endpoint söyleyiş denir.](luis-how-to-review-endpoint-utterances.md)**

| version | yol |
|--|--|
|1|/luis/v1.0/**prog**/apps/{appId}/varlıklar/{entityId}/**örnek**|
|1|/luis/v1.0/**prog**/apps/{appId}/intents/{intentId}/**örnek**|
|2|/luis/**api**/v2.0/apps/{appId}/**sürümleri**/{versionId}/varlıklar/{entityId}/**öner**|
|2|/luis/**api**/v2.0/apps/{appId}/**sürümleri**/{versionId}/intents/{intentId}/**öner**|


## <a name="create-app-from-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarından uygulama oluşturma
[Önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) önceden tanımlanmış bir etki alanı modeli sağlar. Önceden oluşturulmuş etki alanları, ortak etki alanları için LUIS uygulamanızı hızla geliştirmenize olanak sağlar. Bu API, önceden oluşturulmuş bir etki alanına dayalı yeni bir uygulama oluşturmanıza olanak tanır. Yanıt yeni appID olduğunu.

|v2 rota|Fiil|
|--|--|
|/luis/api/v2.0/apps/customprebuiltdomains  |almak, posta|
|/luis/api/v2.0/apps/customprebuiltdomains/{culture}  |get|

## <a name="importing-1x-app-into-2x"></a>1.x uygulamasını 2.x'e aktarma
İhraç edilen 1.x uygulaması JSON'da [LUIS][LUIS] 2.0'a girmeden önce değiştirmeniz gereken bazı alanlar vardır. 

### <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar 
[Önceden oluşturulmuş varlıklar](luis-prebuilt-entities.md) değişti. V2 önceden oluşturulmuş varlıkları kullandığınızdan emin olun. Buna [datetimeV2 yerine datetimeV2](luis-reference-prebuilt-datetimev2.md)kullanımı dahildir. 

### <a name="actions"></a>Eylemler
Eylemler özelliği artık geçerli değildir. Boş olmalı. 

### <a name="labeled-utterances"></a>Etiketli söyleyinatlar
V1, etiketli sözcük lerin sözcüğün veya tümceciğin başında veya sonunda boşluk lar içermesine izin verdi. Boşlukları kaldırdım. 

## <a name="common-reasons-for-http-response-status-codes"></a>HTTP yanıt durum kodları için yaygın nedenler
Bkz. [LUIS API yanıt kodları.](luis-reference-response-codes.md)

## <a name="next-steps"></a>Sonraki adımlar

VAROLAN REST çağrılarını LUIS [bitiş noktasına](https://go.microsoft.com/fwlink/?linkid=2092356) ve API'lere [yazmaiçin](https://go.microsoft.com/fwlink/?linkid=2092087) v2 API belgelerini kullanın. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
