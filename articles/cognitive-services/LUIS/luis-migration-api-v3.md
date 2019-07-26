---
title: V2 'den v3 API 'ye geçiş
titleSuffix: Azure Cognitive Services
description: Sürüm 3 uç nokta API 'Leri değişti. Sürüm 3 uç nokta API 'Lerine nasıl geçiş yapılacağını anlamak için bu kılavuzu kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/22/2019
ms.author: diberry
ms.openlocfilehash: edaa36cf22e63d42eb347aea3da1816e2c93b45e
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479213"
---
# <a name="preview-migrate-to-api-version-3x-for-luis-apps"></a>Önizleme: LUSıS uygulamaları için API sürüm 3. x 'e geçiş

Sorgu tahmin uç noktası API 'Leri değişti. Sürüm 3 uç nokta API 'Lerine nasıl geçiş yapılacağını anlamak için bu kılavuzu kullanın. 

Bu v3 API, önemli JSON isteği ve/veya Yanıt değişikliklerini içeren aşağıdaki yeni özellikleri sağlar: 

* [Dış varlıklar](#external-entities-passed-in-at-prediction-time)
* [Dinamik listeler](#dynamic-lists-passed-in-at-prediction-time)
* [Önceden oluşturulmuş varlık JSON değişiklikleri](#prebuilt-entities-with-new-json)

<!--
* [Multi-intent detection of utterance](#detect-multiple-intents-within-single-utterance)
-->

Sorgu tahmin uç noktası [isteği](#request-changes) ve [yanıtı](#response-changes) , yukarıda listelenen yeni özellikleri desteklemek için aşağıdakiler de dahil önemli değişikliklere sahiptir:

* [Yanıt nesnesi değişiklikleri](#top-level-json-changes)
* [Varlık adı yerine varlık rolü adı başvuruları](#entity-role-name-instead-of-entity-name)
* [Varlıkları utterde olarak işaretlemek için Özellikler](#marking-placement-of-entities-in-utterances)

Aşağıdaki LUI özellikleri v3 API 'sinde **desteklenmez** :

* Bing Yazım Denetimi v7

[Başvuru belgeleri](https://aka.ms/luis-api-v3) v3 için kullanılabilir.

## <a name="endpoint-url-changes-by-slot-name"></a>Yuva adına göre uç nokta URL 'SI değişiklikleri

V3 uç nokta HTTP çağrısının biçimi değişti.

|YÖNTEMIDIR|URL|
|--|--|
|GET|https://<b>{REGION}</b>. api.Cognitive.Microsoft.com/Luis/<b>v 3.0-Önizleme</b>/Apps/<b>{uygulama kimliği}</b>/Slots/<b>{slot-adı}</b>/tahmin? sorgu =<b>{Query}</b>|
|POST|https://<b>{REGION}</b>. api.Cognitive.Microsoft.com/Luis/<b>v 3.0-Önizleme</b>/Apps/<b>{uygulama kimliği}</b>/Slots/<b>{slot-adı}</b>/tahmin|
|||

Yuvalar için geçerli değerler:

* `production`
* `staging`

## <a name="endpoint-url-changes-by-version-id"></a>Sürüm KIMLIĞINE göre uç nokta URL 'SI değişiklikleri

Sürümüne göre sorgulamak istiyorsanız, önce ile `"directVersionPublish":true` [API aracılığıyla yayımlamanız](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3b) gerekir. Yuva adı yerine sürüm KIMLIĞINE başvuran uç noktayı sorgulayın.


|YÖNTEMIDIR|URL|
|--|--|
|GET|https://<b>{REGION}</b>. api.Cognitive.Microsoft.com/Luis/<b>v 3.0-Önizleme</b>/Apps/<b>{app-id}</b>/Versions/<b>{sürüm-kimliği}</b>/tahmin? sorgu =<b>{Query}</b>|
|POST|https://<b>{REGION}</b>. api.Cognitive.Microsoft.com/Luis/<b>v 3.0-Önizleme</b>/Apps/<b>{app-id}</b>/Versions/<b>{sürüm-kimliği}</b>/tahmin|
|||

## <a name="prebuilt-entities-with-new-json"></a>Yeni JSON ile önceden oluşturulmuş varlıklar

V3 yanıt nesnesi değişiklikleri [önceden oluşturulmuş varlıkları](luis-reference-prebuilt-entities.md)içerir. 

## <a name="request-changes"></a>Değişiklikleri isteme 

### <a name="query-string-parameters"></a>Sorgu dizesi parametreleri

V3 API 'sinde farklı sorgu dizesi parametreleri vardır.

|Param adı|Type|Version|Varsayılan|Amaç|
|--|--|--|--|--|
|`log`|boolean|V2 & V3|false|Sorguyu günlük dosyasında sakla.| 
|`query`|dize|Yalnızca v3|Varsayılan değer yok-GET isteğinde gerekli|**V2 'de**, tahmin edilecek olan `q` söylenişi parametresi. <br><br>**V3 'de**, işlev `query` parametreye geçirilir.|
|`show-all-intents`|boolean|Yalnızca v3|false|**Tahmin. amaçlar** nesnesindeki karşılık gelen puanı içeren tüm hedefleri döndürün. Amaçlar bir üst `intents` nesnede nesneler olarak döndürülür. Bu, bir dizide amacı bulmaya gerek kalmadan programlı erişime izin verir: `prediction.intents.give`. V2 'de, bunlar bir dizide döndürülür. |
|`verbose`|boolean|V2 & V3|false|**V2 sürümünde**, true olarak ayarlandığında, tüm tahmin edilen amaçlar döndürülür. Tahmin edilen `show-all-intents`tüm amaçlar için Ihtiyaç duyuyorsanız, v3 param ' ı kullanın.<br><br>**V3 'de**, bu parametre yalnızca varlık tahmini varlık meta veri ayrıntılarını sağlar.  |



<!--
|`multiple-segments`|boolean|V3 only|Break utterance into segments and predict each segment for intents and entities.|
-->


### <a name="the-query-prediction-json-body-for-the-post-request"></a>`POST` İstek için sorgu tahmini JSON gövdesi

```JSON
{
    "query":"your utterance here",
    "options":{
        "datetimeReference": "2019-05-05T12:00:00",
        "overridePredictions": true
    },
    "externalEntities":[],
    "dynamicLists":[]
}
```

|Özellik|Type|Version|Varsayılan|Amaç|
|--|--|--|--|--|
|`dynamicLists`|array|Yalnızca v3|Gerekli değildir.|[Dinamik listeler](#dynamic-lists-passed-in-at-prediction-time) , zaten lusıs uygulamasında var olan eğitilen ve yayımlanmış bir liste varlığını genişletmenizi sağlar.|
|`externalEntities`|array|Yalnızca v3|Gerekli değildir.|[Dış varlıklar](#external-entities-passed-in-at-prediction-time) , lusıs uygulamasına çalışma zamanı sırasında varlıkları tanımlayabilir ve etiketleyebilir, bu da mevcut varlıkların özellikleri olarak kullanılabilir. |
|`options.datetimeReference`|dize|Yalnızca v3|Varsayılan değer yok|[DatetimeV2 sapmasını](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity)belirlemede kullanılır. DatetimeReference biçimi [ıso 8601](https://en.wikipedia.org/wiki/ISO_8601)' dir.|
|`options.overridePredictions`|boolean|Yalnızca v3|false|Kullanıcının [dış varlığının (var olan varlıkla aynı ada sahip)](#override-existing-model-predictions) kullanıldığını veya modeldeki mevcut varlığın tahmin için kullanıldığını belirtir. |
|`query`|dize|Yalnızca v3|Gerekli.|**V2 'de**, tahmin edilecek olan `q` söylenişi parametresi. <br><br>**V3 'de**, işlev `query` parametreye geçirilir.|



## <a name="response-changes"></a>Yanıt değişiklikleri

Sorgu yanıtı JSON, en sık kullanılan verilere daha fazla programlı erişim sağlamak için değişti. 

### <a name="top-level-json-changes"></a>Üst düzey JSON değişiklikleri

V2 `verbose` için en üstteki JSON özellikleri, `intents` özelliği true olarak ayarlandığında, özelliğin tüm amaçlarını ve puanlarını döndürür:

```JSON
{
    "query":"this is your utterance you want predicted",
    "topScoringIntent":{},
    "intents":[],
    "entities":[],
    "compositeEntities":[]
}
```

V3 için en üstteki JSON özellikleri şunlardır:

```JSON
{
    "query": "this is your utterance you want predicted",
    "prediction":{
        "normalizedQuery": "this is your utterance you want predicted - after normalization",
        "topIntent": "intent-name-1",
        "intents": {}, 
        "entities":{}
    }
}
```

Yazım denetimi düzeltmeleri içerir. `normalizedQuery` Bu, v2 API özelliğine `alteredQuery`karşılık gelir.  

`intents` Nesne sıralanmamış bir liste. ' Deki `intents` ilk alt öğenin `topIntent`öğesine karşılık geldiğini varsayın. Bunun yerine, puanı `topIntent` bulmak için değerini kullanın:

```nodejs
const topIntentName = response.prediction.topIntent;
const score = intents[topIntentName];
```

Response JSON şeması değişiklikleri için izin ver:

* Orijinal utterance, `query`ve döndürülen `prediction`tahmin arasındaki ayrımı temizleyin.
* Tahmin edilen verilere programlı erişim daha kolay. V2 'deki bir dizi aracılığıyla sıralama yapmak yerine, her iki amaç ve varlık için **adlandırılmış** değerlere erişebilirsiniz. Tahmin edilen varlık rolleri için, rol adı tüm uygulama genelinde benzersiz olduğundan döndürülür.
* Saptandığı takdirde veri türleri dikkate alınır. Numerics artık dizeler olarak döndürülmez.
* `$instance` Nesne içinde döndürülen ilk öncelikli tahmin bilgileri ve ek meta veriler arasındaki ayrım. 

### <a name="access-instance-for-entity-metadata"></a>Varlık `$instance` meta verileri erişimi

Varlık meta verileri gerekiyorsa, sorgu dizesinin `verbose=true` bayrağını kullanması gerekir ve yanıt `$instance` nesnedeki meta verileri içerir. Aşağıdaki bölümlerde JSON yanıtlarında örnekler gösterilmektedir.

### <a name="each-predicted-entity-is-represented-as-an-array"></a>Tahmin edilen her varlık bir dizi olarak temsil edilir

Her varlık, utterance 'de birden çok kez tahmin edilebileceği için nesnebirdiziiçerir.`prediction.entities.<entity-name>` 

### <a name="list-entity-prediction-changes"></a>Varlık tahmini değişikliklerini Listele

Bir liste varlık tahmini için JSON, dizi dizileri olacak şekilde değiştirilmiştir:

```JSON
"entities":{
    "my_list_entity":[
        ["canonical-form-1","canonical-form-2"],
        ["canonical-form-2"]
    ]
}
```
Her iç dizi, utterance içindeki metne karşılık gelir. Aynı metin bir liste varlığının birden fazla alt listesinde görünebildiğinden iç nesne bir dizidir. 

Nesneyle nesne arasında `entities` `$instance` eşleme yaparken, nesne sırası, liste varlık tahminleri için korunur.

```nodejs
const item = 0; // order preserved, use same enumeration for both
const predictedCanonicalForm = entities.my_list_entity[item];
const associatedMetadata = entities.$instance.my_list_entity[item];
```

### <a name="entity-role-name-instead-of-entity-name"></a>Varlık adı yerine varlık rolü adı 

V2 `entities` 'de dizi, benzersiz tanımlayıcı olan varlık adına sahip tüm tahmin edilen varlıkları döndürdü. V3 'de, varlık roller kullanıyorsa ve tahmin bir varlık rolü için ise, birincil tanımlayıcı rol adıdır. Bu, varlık rolü adlarının diğer model (amaç, varlık) adları da dahil olmak üzere tüm uygulama genelinde benzersiz olması gerektiğinden mümkündür.

Aşağıdaki örnekte: metnini `Yellow Bird Lane`içeren bir söylenişi düşünün. Bu metin, özel `Location` bir varlığın `Destination`rolü olarak tahmin edilir.

|Utterance metni|Varlık adı|Rol adı|
|--|--|--|
|`Yellow Bird Lane`|`Location`|`Destination`|

V2 'de, varlık _varlık adı_ tarafından nesnenin özelliği olarak rolüyle tanımlanır:

```JSON
"entities":[
    {
        "entity": "Yellow Bird Lane",
        "type": "Location",
        "startIndex": 13,
        "endIndex": 20,
        "score": 0.786378264,
        "role": "Destination"
    }
]
```

V3 'de, rolün tahmini rol için olması durumunda varlığa _varlık rolü_başvurulur:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ]
}
```

V3 'de, varlık meta verilerini döndürme `verbose` bayrağıyla aynı sonuç:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ],
    "$instance":{
        "Destination": [
            {
                "role": "Destination",
                "type": "Location",
                "text": "Yellow Bird Lane",
                "startIndex": 25,
                "length":16,
                "score": 0.9837309,
                "modelTypeId": 1,
                "modelType": "Entity Extractor"
            }
        ]
    }
}
```

## <a name="external-entities-passed-in-at-prediction-time"></a>Tahmin sırasında geçirilen dış varlıklar

Dış varlıklar, LUSıS uygulamasına çalışma zamanı sırasında varlıkları tanımlayabilir ve etiketleyebilir, bu da mevcut varlıkların özellikleri olarak kullanılabilir. Bu, sorguları tahmin uç noktanıza göndermeden önce kendi ayrı ve özel varlıklarınızı kullanmanıza olanak sağlar ayıklayıcıları. Bu sorgu tahmini uç noktasında yapıldığından, modelinizi yeniden eğitmeniz ve yayımlamanız gerekmez.

İstemci uygulaması, varlık eşleşmesini yönetip, bu eşleşen varlığın içindeki konumu belirleyerek ve sonra bu bilgileri istekle göndererek kendi varlık ayıklayıcısı 'nı sağlıyor. 

Dış varlıklar, hala roller, bileşik ve diğerleri gibi diğer modellere sinyal olarak kullanıldıkları sürece herhangi bir varlık türünü genişletmeye yönelik mekanizmadır.

Bu, yalnızca sorgu tahmini çalışma zamanında kullanılabilir verileri olan bir varlık için yararlıdır. Bu tür verilerin örnekleri, sürekli olarak verileri değiştirme veya Kullanıcı başına belirli bir örnektir. Bir LUSıS iletişim varlığını, bir kullanıcının kişi listesindeki dış bilgilerle genişletebilirsiniz. 

### <a name="entity-already-exists-in-app"></a>Varlık uygulamada zaten var

Uç nokta istek `entityName` gönderi gövdesinde geçirilen dış varlığın değeri, istek yapıldığı sırada eğitilen ve yayımlanmış uygulamada zaten mevcut olmalıdır. Varlık türü, tüm türler desteklenir.

### <a name="first-turn-in-conversation"></a>İlk olarak konuşmayı açın

Bir kullanıcının aşağıdaki eksik bilgileri girdiği bir sohbet bot görüşmesinde ilk göz önünde bulundurun:

`Send Hazem a new message`

Sohbet bot 'tan lusıs 'e gelen istek, Kullanıcı kişilerinin biri olarak doğrudan eşleştirildiğinden, ilgili `Hazem` posta gövdesinde bilgi gönderebilir.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 5,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

Tahmin yanıtı, istek içinde tanımlandığından, diğer tüm öngörülen varlıklar ile bu dış varlığı içerir.  

### <a name="second-turn-in-conversation"></a>İkinci konuşmayı aç

Sohbet bot 'ta bir sonraki Kullanıcı, daha fazla dönemi kullanır:

`Send him a calendar reminder for the party.`

Önceki bir deyişle, söylenişi için `him` `Hazem`başvuru olarak kullanılır. Gönderi gövdesinde konuşma sohbeti bot, ilk utterden `him` `Hazem`ayıklanan varlık değeriyle eşlenir.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 3,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

Tahmin yanıtı, istek içinde tanımlandığından, diğer tüm öngörülen varlıklar ile bu dış varlığı içerir.  

### <a name="override-existing-model-predictions"></a>Mevcut model tahminlerini geçersiz kıl

`overridePredictions` Options özelliği, Kullanıcı aynı ada sahip bir tahmin edilen varlıkla çakışan bir dış varlık gönderiyorsa, luya geçirilen varlığı veya modelde var olan varlığı seçer. 

Örneğin, sorguyu `today I'm free`göz önünde bulundurun. Luo, `today` aşağıdaki Yanıt ile bir datetimeV2 algılar:

```JSON
"datetimeV2": [
    {
        "type": "date",
        "values": [
            {
                "timex": "2019-06-21",
                "value": "2019-06-21"
            }
        ]
    }
]
```

Kullanıcı dış varlığı gönderirse:

```JSON
{
    "entityName": "datetimeV2",
    "startIndex": 0,
    "entityLength": 5,
    "resolution": {
        "date": "2019-06-21"
    }
}
```

, `overridePredictions` Olarak`false`ayarlanırsa, dış varlık gönderilmediği gibi lusıs bir yanıt döndürür. 

```JSON
"datetimeV2": [
    {
        "type": "date",
        "values": [
            {
                "timex": "2019-06-21",
                "value": "2019-06-21"
            }
        ]
    }
]
```

, `overridePredictions` Olarak`true`ayarlandıysa, lusıs aşağıdakiler dahil bir yanıt döndürür:

```JSON
"datetimeV2": [
    {
        "date": "2019-06-21"
    }
]
```



#### <a name="resolution"></a>Çözüm

_İsteğe bağlı_ `resolution` özelliği, tahmine yanıt olarak döner ve dış varlıkla ilişkili meta verileri geçirmenize olanak tanır ve ardından yanıtta geri alabilirsiniz. 

Birincil amaç, önceden oluşturulmuş varlıkların genişletilmesine karşın bu varlık türüyle sınırlı değildir. 

`resolution` Özelliği bir sayı, dize, nesne veya dizi olabilir:

* Şubesi
* {"metin": "değer"}
* 12345 
* ["a", "b", "c"]



## <a name="dynamic-lists-passed-in-at-prediction-time"></a>Tahmin sırasında geçirilen dinamik listeler

Dinamik listeler, zaten LUSıS uygulamasında var olan eğitilen ve yayımlanmış bir liste varlığını genişletmenizi sağlar. 

Liste varlık değerlerinizin düzenli aralıklarla değiştirilmesi gerektiğinde bu özelliği kullanın. Bu özellik, zaten eğitilen ve yayımlanmış bir liste varlığını genişletmenizi sağlar:

* Sorgu tahmin uç noktası isteği sırasında.
* Tek bir istek için.

Liste varlığı, LUSıS uygulamasında boş olabilir, ancak var olması gerekiyor. LUSıS uygulamasındaki liste varlığı değiştirilmez, ancak uç noktasındaki tahmin yeteneği, yaklaşık 1.000 öğe içeren en fazla 2 liste içerecek şekilde genişletilir.

### <a name="dynamic-list-json-request-body"></a>Dinamik liste JSON istek gövdesi

Aşağıdaki JSON gövdesine göndererek, listeye eş anlamlı olan yeni bir alt liste ekleyin ve `LUIS` `POST` sorgu tahmini isteğiyle metin için liste varlığını tahmin edin:

```JSON
{
    "query": "Send Hazem a message to add an item to the meeting agenda about LUIS.",
    "options":{
        "timezoneOffset": "-8:00"
    },
    "dynamicLists": [
        {
            "listEntityName":"ProductList",
            "requestLists":[
                {
                    "name": "Azure Cognitive Services",
                    "canonicalForm": "Azure-Cognitive-Services",
                    "synonyms":[
                        "language understanding",
                        "luis",
                        "qna maker"
                    ]
                }
            ]
        }
    ]
}
```

Tahmin yanıtı, istekte tanımlandığından, diğer tüm öngörülen varlıkların bulunduğu liste varlığını içerir. 

## <a name="timezoneoffset-renamed-to-datetimereference"></a>Timezonesapmayı datetimeReference olarak yeniden adlandırıldı

**V2 'de**, `timezoneOffset` istek bir get veya post isteği olarak gönderildiğine bakılmaksızın, [parametre](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) tahmin isteğinde bir sorgu dizesi parametresi olarak gönderilir. 

**V3 'de**, aynı işlevsellik, `datetimeReference`gövde gönderme parametresi ile birlikte sağlanır. 

## <a name="marking-placement-of-entities-in-utterances"></a>Varlıkların, utterlerdeki yerleşimini işaretleme

**V2 sürümünde**bir varlık `startIndex` ve `endIndex`ile bir utterlik olarak işaretlendi. 

**V3 'de**varlık ve `startIndex` `entityLength`ile işaretlenir.

## <a name="deprecation"></a>Kullanımdan kaldırma 

V2 API 'SI, v3 önizlemesi sonrasında en az 9 ay boyunca kullanım dışı olmayacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

LUıS [Endpoint](https://aka.ms/luis-api-v3) API 'LERINE mevcut Rest çağrılarını güncelleştirmek IÇIN v3 API belgelerini kullanın. 