---
title: v1-v2 API geçişi
titleSuffix: Azure Cognitive Services
description: Sürüm 1 uç noktası ve yazma Language Understanding API 'Leri kullanım dışıdır. Sürüm 2 uç noktası ve yazma API 'Lerine nasıl geçiş yapılacağını anlamak için bu kılavuzu kullanın.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68932685"
---
# <a name="api-v1-to-v2-migration-guide-for-luis-apps"></a>LUSıS uygulamaları için API v1-v2 geçiş kılavuzu
Sürüm 1 [uç noktası](https://aka.ms/v1-endpoint-api-docs) ve [yazma](https://aka.ms/v1-authoring-api-docs) API 'leri kullanım dışıdır. Sürüm 2 [uç noktası](https://go.microsoft.com/fwlink/?linkid=2092356) ve [yazma](https://go.microsoft.com/fwlink/?linkid=2092087) API 'lerine nasıl geçiş yapılacağını anlamak için bu kılavuzu kullanın. 

## <a name="new-azure-regions"></a>Yeni Azure bölgeleri
LUıS, LUıS API 'Leri için belirtilen yeni [bölgelere](https://aka.ms/LUIS-regions) sahiptir. LUSıS, bölge grupları için farklı bir portal sağlar. Uygulamanın, sorgulamak istediğiniz bölgede yazılması gerekir. Uygulamalar bölgeleri otomatik olarak geçirmez. Uygulamayı bir bölgeden dışarı aktarıp yeni bir bölgede kullanılabilir olması için başka bir bölgeden içeri aktarırsınız.

## <a name="authoring-route-changes"></a>Rota değişikliklerini yazma
Yazma API yolu, **API** yolunu kullanarak **Program** rotası kullanılarak değiştirilir.


| version | yol |
|--|--|
|1|/Luis/v1.0/**prog**/Apps|
|2|/Luis/**API**/v2.0/Apps|


## <a name="endpoint-route-changes"></a>Uç nokta rota değişiklikleri
Endpoint API 'sinin yeni sorgu dizesi parametreleri ve farklı bir yanıt vardır. Verbose bayrağı true ise, puandan bağımsız olarak tüm amaçlar, topScoringIntent adlı bir dizide döndürülür.

| version | Rotayı al |
|--|--|
|1|/Luis/v1/Application? ID = {AppID} &q = {q}|
|2|/Luis/v2.0/Apps/{AppID}? q = {q} [&Timezonekayması] [&verbose] [&spellCheck] [&hazırlama] [&Bing-yazım denetimi-abonelik-anahtar] [&günlüğü]|


V1 uç noktası başarı yanıtı:
```json
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

v2 uç noktası başarı yanıtı:
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

## <a name="key-management-no-longer-in-api"></a>Anahtar yönetimi artık API 'de yok
Abonelik uç noktası anahtar API 'Leri kullanımdan kalktı ve 410 döndürülüyor.

| version | yol |
|--|--|
|1|/Luis/v1.0/prog/abonelikler|
|1|/luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [uç nokta anahtarları](luis-how-to-azure-subscription.md) Azure Portal oluşturulur. **[Yayımla](luis-how-to-azure-subscription.md)** SAYFASıNDA bir Luo uygulamasına anahtar atarsınız. Gerçek anahtar değerini bilmeniz gerekmez. LUSıS, atamayı yapmak için abonelik adını kullanır. 

## <a name="new-versioning-route"></a>Yeni sürüm oluşturma yolu
V2 modeli artık bir [sürümde](luis-how-to-manage-versions.md)yer alıyor. Bir sürüm adı, rotada 10 karakterdir. Varsayılan sürüm "0,1" dir.

| version | yol |
|--|--|
|1|/Luis/v1.0/**prog**/Apps/{AppID}/Entities|
|2|/Luis/**API**/v2.0/Apps/{AppID}/**Versions**/{VersionId}/Entities|

## <a name="metadata-renamed"></a>Meta veriler yeniden adlandırıldı
LUıS meta verilerini döndüren bazı API 'Ler yeni adlara sahiptir.

| V1 yol adı | v2 yol adı |
|--|--|
|PersonalAssistantApps |yardımcıları|
|applicationkültürler|kültürler|
|applicationdomains|etki alanları|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Örnek", "öner" olarak yeniden adlandırıldı
LUO, modeli geliştirebilecek mevcut [uç nokta dıklarından](luis-how-to-review-endpoint-utterances.md) gelen söylenme önerisinde bulunur. Önceki sürümde, bu **örnek**olarak adlandırılmıştır. Yeni sürümde, ad örnekten **önerecek**şekilde değiştirilir. Bu, LUSıS Web sitesinde **[Gözden geçirme uç noktası utbotları](luis-how-to-review-endpoint-utterances.md)** olarak adlandırılır.

| version | yol |
|--|--|
|1|/Luis/v1.0/**prog**/Apps/{AppID}/Entities/{EntityId}/**Sample**|
|1|/Luis/v1.0/**prog**/Apps/{AppID}/Ints/{cütid}/**Sample**|
|2|/Luis/**API**/v2.0/Apps/{AppID}/**Versions**/{VersionId}/Entities/{EntityId}/**öner**|
|2|/Luis/**API**/v2.0/Apps/{AppID}/**Versions**/{VersionId}/BITS/{bittıd}/**öner**|


## <a name="create-app-from-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarından uygulama oluşturma
Önceden [oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) önceden tanımlanmış bir etki alanı modeli sağlar. Önceden oluşturulmuş etki alanları, genel etki alanları için LUSıS uygulamanızı hızlı bir şekilde geliştirmenize olanak tanır. Bu API, önceden oluşturulmuş bir etki alanını temel alan yeni bir uygulama oluşturmanıza olanak tanır. Yanıt, yeni AppID 'dir.

|v2 yolu|ü|
|--|--|
|/Luis/api/v2.0/Apps/customprebuiltdomains  |Al, postala|
|/luis/api/v2.0/apps/customprebuiltdomains/{culture}  |get|

## <a name="importing-1x-app-into-2x"></a>1. x uygulamasını 2. x 'e aktarma
Aktarılan 1. x uygulamasının JSON 'ı, [luın][LUIS] 2,0 'e aktarmadan önce değiştirmeniz gereken bazı alanlara sahiptir. 

### <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar 
[Önceden oluşturulmuş varlıklar](luis-prebuilt-entities.md) değiştirilmiştir. V2 önceden oluşturulmuş varlıkları kullandığınızdan emin olun. Bu, DateTime yerine [datetimeV2](luis-reference-prebuilt-datetimev2.md)kullanmayı içerir. 

### <a name="actions"></a>Eylemler
Actions özelliği artık geçerli değil. Boş olmalıdır 

### <a name="labeled-utterances"></a>Etiketlenmiş utterslar
V1, sözcük veya tümceciğin başlangıcında veya sonunda boşluk içerecek şekilde etiketlendi. Boşluklar kaldırıldı. 

## <a name="common-reasons-for-http-response-status-codes"></a>HTTP yanıt durum kodlarının genel nedenleri
Bkz. [LUSıS API yanıt kodları](luis-reference-response-codes.md).

## <a name="next-steps"></a>Sonraki adımlar

LUıS [uç noktası](https://go.microsoft.com/fwlink/?linkid=2092356) ve [yazma](https://go.microsoft.com/fwlink/?linkid=2092087) API 'lerine yönelik mevcut Rest ÇAĞRıLARıNı güncelleştirmek için v2 API belgelerini kullanın. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
