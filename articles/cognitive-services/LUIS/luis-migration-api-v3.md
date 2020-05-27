---
title: V3 API 'sindeki tahmin uç noktası değişiklikleri
description: Sorgu tahmin uç noktası v3 API 'Leri değişti. Sürüm 3 uç nokta API 'Lerine nasıl geçiş yapılacağını anlamak için bu kılavuzu kullanın.
ms.topic: conceptual
ms.date: 05/15/2020
ms.author: diberry
ms.openlocfilehash: 84afcbcd348c3fd91014096877de2315722b53a0
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83849340"
---
# <a name="prediction-endpoint-changes-for-v3"></a>V3 için tahmin uç noktası değişiklikleri

Sorgu tahmin uç noktası v3 API 'Leri değişti. Sürüm 3 uç nokta API 'Lerine nasıl geçiş yapılacağını anlamak için bu kılavuzu kullanın.

**Genel olarak kullanılabilen durum** -bu v3 API, v2 API 'SINDEN önemli JSON isteği ve yanıt değişiklikleri içerir.

V3 API 'SI aşağıdaki yeni özellikleri sağlar:

* [Dış varlıklar](schema-change-prediction-runtime.md#external-entities-passed-in-at-prediction-time)
* [Dinamik listeler](schema-change-prediction-runtime.md#dynamic-lists-passed-in-at-prediction-time)
* [Önceden oluşturulmuş varlık JSON değişiklikleri](#prebuilt-entity-changes)

Tahmin uç noktası [isteği](#request-changes) ve [yanıtı](#response-changes) , yukarıda listelenen yeni özellikleri desteklemek için aşağıdakiler de dahil önemli değişikliklere sahiptir:

* [Yanıt nesnesi değişiklikleri](#top-level-json-changes)
* [Varlık adı yerine varlık rolü adı başvuruları](#entity-role-name-instead-of-entity-name)
* [Varlıkları utterde olarak işaretlemek için Özellikler](#marking-placement-of-entities-in-utterances)

[Başvuru belgeleri](https://aka.ms/luis-api-v3) v3 için kullanılabilir.

## <a name="v3-changes-from-preview-to-ga"></a>V3, önizlemeden GA 'ye değişir

V3, GA 'ya taşıma kapsamında aşağıdaki değişiklikleri yaptı:

* Aşağıdaki önceden oluşturulmuş varlıkların farklı JSON yanıtları vardır:
    * [OrdinalV1](luis-reference-prebuilt-ordinal.md)
    * [GeographyV2](luis-reference-prebuilt-geographyv2.md)
    * [DatetimeV2](luis-reference-prebuilt-datetimev2.md)
    * İle ölçülebilir birim anahtar adı `units``unit`

* İstek gövdesi JSON değişikliği:
    * öğesinden `preferExternalEntities``preferExternalEntities`
    * `score`dış varlıklar için isteğe bağlı parametre

* Yanıt gövdesi JSON değişiklikleri:
    * `normalizedQuery`kaldırıldı

## <a name="suggested-adoption-strategy"></a>Önerilen benimseme stratejisi

Bot Framework kullanıyorsanız, v7 Bing Yazım Denetimi veya LUSıS uygulama Authoring-yalnızca geçiş yapmak istiyorsanız v2 uç noktasını kullanmaya devam edin.

İstemci uygulamanız veya tümleştirmelerinizin (bot Framework ve Bing Yazım Denetimi v7) etkilenmediğini biliyorsanız ve LUSıS uygulama Authoring ve tahmin uç noktanızı aynı anda geçiriyorsanız, v3 tahmin uç noktasını kullanmaya başlayın. V2 tahmin uç noktası hala kullanılabilir olmaya devam eder ve iyi bir geri dönüş stratejisidir.


## <a name="not-supported"></a>Desteklenmiyor

### <a name="bing-spell-check"></a>Bing Yazım Denetimi

Bu API v3 tahmin uç noktasında desteklenmez-yazım düzeltmeleri için v2 API tahmini uç noktasını kullanmaya devam edin. V3 API kullanırken yazım denetimi yapmanız gerekiyorsa, istemci uygulamanın [Bing yazım denetimi](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/overview) API 'sini çağırmasını ve metni LUSıS API 'sine göndermeden önce doğru yazımla değiştirmesini sağlayabilirsiniz.

## <a name="bot-framework-and-azure-bot-service-client-applications"></a>Bot Framework ve Azure bot hizmeti istemci uygulamaları

Bot çerçevesinin V 4.7 'i serbest bırakılana kadar v2 API tahmini uç noktasını kullanmaya devam edin.

## <a name="v2-api-deprecation"></a>V2 API 'sinin kullanımdan kaldırılması

V2 tahmin API 'SI v3 önizlemesi sonrasında 8 Haziran 2020 ' den sonra en az 9 ay boyunca kullanım dışı olmayacaktır.

## <a name="endpoint-url-changes"></a>Uç nokta URL 'SI değişiklikleri

### <a name="changes-by-slot-name-and-version-name"></a>Yuva adı ve sürüm adına göre değişiklikler

[V3 uç nokta http çağrısının biçimi](developer-reference-resource.md#rest-endpoints) değişti.

Sürümüne göre sorgulamak istiyorsanız, önce ile [API aracılığıyla yayımlamanız](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3b) gerekir `"directVersionPublish":true` . Yuva adı yerine sürüm KIMLIĞINE başvuran uç noktayı sorgulayın.

|İçin geçerli değerler`SLOT-NAME`|
|--|
|`production`|
|`staging`|

## <a name="request-changes"></a>Değişiklikleri isteme

### <a name="query-string-changes"></a>Sorgu dizesi değişiklikleri

V3 API 'sinde farklı sorgu dizesi parametreleri vardır.

|Param adı|Tür|Sürüm|Varsayılan|Amaç|
|--|--|--|--|--|
|`log`|boole|V2 & V3|yanlış|Sorguyu günlük dosyasında sakla. Varsayılan değer false 'dur.|
|`query`|string|Yalnızca v3|Varsayılan değer yok-GET isteğinde gerekli|**V2 'de**, tahmin edilecek olan söylenişi `q` parametresi. <br><br>**V3 'de**, işlev `query` parametreye geçirilir.|
|`show-all-intents`|boole|Yalnızca v3|yanlış|**Tahmin. amaçlar** nesnesindeki karşılık gelen puanı içeren tüm hedefleri döndürün. Amaçlar bir üst nesnede nesneler olarak döndürülür `intents` . Bu, bir dizide amacı bulmaya gerek kalmadan programlı erişime izin verir: `prediction.intents.give` . V2 'de, bunlar bir dizide döndürülür. |
|`verbose`|boole|V2 & V3|yanlış|**V2 sürümünde**, true olarak ayarlandığında, tüm tahmin edilen amaçlar döndürülür. Tahmin edilen tüm amaçlar için ihtiyaç duyuyorsanız, v3 param ' ı kullanın `show-all-intents` .<br><br>**V3 'de**, bu parametre yalnızca varlık tahmini varlık meta veri ayrıntılarını sağlar.  |
|`timezoneOffset`|string|V2|-|DatetimeV2 varlıklara uygulanan saat dilimi.|
|`datetimeReference`|string|Yüklemesinde|-|DatetimeV2 varlıklara uygulanan [saat dilimi](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) . `timezoneOffset`V2 'den değiştirilir.|


### <a name="v3-post-body"></a>V3 posta gövdesi

```JSON
{
    "query":"your utterance here",
    "options":{
        "datetimeReference": "2019-05-05T12:00:00",
        "preferExternalEntities": true
    },
    "externalEntities":[],
    "dynamicLists":[]
}
```

|Özellik|Tür|Sürüm|Varsayılan|Amaç|
|--|--|--|--|--|
|`dynamicLists`|array|Yalnızca v3|Gerekli değildir.|[Dinamik listeler](schema-change-prediction-runtime.md#dynamic-lists-passed-in-at-prediction-time) , zaten lusıs uygulamasında var olan eğitilen ve yayımlanmış bir liste varlığını genişletmenizi sağlar.|
|`externalEntities`|array|Yalnızca v3|Gerekli değildir.|[Dış varlıklar](schema-change-prediction-runtime.md#external-entities-passed-in-at-prediction-time) , lusıs uygulamasına çalışma zamanı sırasında varlıkları tanımlayabilir ve etiketleyebilir, bu da mevcut varlıkların özellikleri olarak kullanılabilir. |
|`options.datetimeReference`|string|Yalnızca v3|Varsayılan değer yok|[DatetimeV2 sapmasını](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity)belirlemede kullanılır. DatetimeReference biçimi [ıso 8601](https://en.wikipedia.org/wiki/ISO_8601)' dir.|
|`options.preferExternalEntities`|boole|Yalnızca v3|yanlış|Kullanıcının [dış varlığının (var olan varlıkla aynı ada sahip)](schema-change-prediction-runtime.md#override-existing-model-predictions) kullanıldığını veya modeldeki mevcut varlığın tahmin için kullanıldığını belirtir. |
|`query`|string|Yalnızca v3|Gereklidir.|**V2 'de**, tahmin edilecek olan söylenişi `q` parametresi. <br><br>**V3 'de**, işlev `query` parametreye geçirilir.|

## <a name="response-changes"></a>Yanıt değişiklikleri

Sorgu yanıtı JSON, en sık kullanılan verilere daha fazla programlı erişim sağlamak için değişti.

### <a name="top-level-json-changes"></a>Üst düzey JSON değişiklikleri



V2 için en üstteki JSON özellikleri, `verbose` özelliği true olarak ayarlandığında, özelliğin tüm amaçlarını ve puanlarını döndürür `intents` :

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
        "topIntent": "intent-name-1",
        "intents": {},
        "entities":{}
    }
}
```

`intents`Nesne sıralanmamış bir liste. ' Deki ilk alt öğenin `intents` öğesine karşılık geldiğini varsayın `topIntent` . Bunun yerine, `topIntent` puanı bulmak için değerini kullanın:

```nodejs
const topIntentName = response.prediction.topIntent;
const score = intents[topIntentName];
```

Response JSON şeması değişiklikleri için izin ver:

* Orijinal utterance, `query` ve döndürülen tahmin arasındaki ayrımı temizleyin `prediction` .
* Tahmin edilen verilere programlı erişim daha kolay. V2 'deki bir dizi aracılığıyla listelemek yerine, her iki amaç ve **varlık için değerlere** göre değerlere erişebilirsiniz. Tahmin edilen varlık rolleri için, rol adı tüm uygulama genelinde benzersiz olduğundan döndürülür.
* Saptandığı takdirde veri türleri dikkate alınır. Numerics artık dizeler olarak döndürülmez.
* Nesne içinde döndürülen ilk öncelikli tahmin bilgileri ve ek meta veriler arasındaki ayrım `$instance` .

### <a name="entity-response-changes"></a>Varlık yanıtı değişiklikleri

#### <a name="marking-placement-of-entities-in-utterances"></a>Varlıkların, utterlerdeki yerleşimini işaretleme

**V2 sürümünde**bir varlık ve ile bir utterlik olarak işaretlendi `startIndex` `endIndex` .

**V3 'de**varlık ve ile işaretlenir `startIndex` `entityLength` .

#### <a name="access-instance-for-entity-metadata"></a>`$instance`Varlık meta verileri erişimi

Varlık meta verileri gerekiyorsa, sorgu dizesinin bayrağını kullanması gerekir `verbose=true` ve yanıt nesnedeki meta verileri içerir `$instance` . Aşağıdaki bölümlerde JSON yanıtlarında örnekler gösterilmektedir.

#### <a name="each-predicted-entity-is-represented-as-an-array"></a>Tahmin edilen her varlık bir dizi olarak temsil edilir

`prediction.entities.<entity-name>`Her varlık, utterance 'de birden çok kez tahmin edilebileceği için nesne bir dizi içerir.

<a name="prebuilt-entities-with-new-json"></a>

#### <a name="prebuilt-entity-changes"></a>Önceden oluşturulmuş varlık değişiklikleri

V3 yanıtı nesnesi, önceden oluşturulmuş varlıklarda yapılan değişiklikleri içerir. Daha fazla bilgi edinmek için [önceden oluşturulmuş özel varlıkları](luis-reference-prebuilt-entities.md) gözden geçirin.

#### <a name="list-entity-prediction-changes"></a>Varlık tahmini değişikliklerini Listele

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

Nesneyle nesne arasında eşleme yaparken `entities` , nesne `$instance` sırası, liste varlık tahminleri için korunur.

```nodejs
const item = 0; // order preserved, use same enumeration for both
const predictedCanonicalForm = entities.my_list_entity[item];
const associatedMetadata = entities.$instance.my_list_entity[item];
```

#### <a name="entity-role-name-instead-of-entity-name"></a>Varlık adı yerine varlık rolü adı

V2 'de dizi, `entities` benzersiz tanımlayıcı olan varlık adına sahip tüm tahmin edilen varlıkları döndürdü. V3 'de, varlık roller kullanıyorsa ve tahmin bir varlık rolü için ise, birincil tanımlayıcı rol adıdır. Bu, varlık rolü adlarının diğer model (amaç, varlık) adları da dahil olmak üzere tüm uygulama genelinde benzersiz olması gerektiğinden mümkündür.

Aşağıdaki örnekte: metnini içeren bir söylenişi düşünün `Yellow Bird Lane` . Bu metin, özel bir varlığın rolü olarak tahmin edilir `Location` `Destination` .

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

V3 'de, `verbose` varlık meta verilerini döndürme bayrağıyla aynı sonuç:

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

<a name="external-entities-passed-in-at-prediction-time"></a>
<a name="override-existing-model-predictions"></a>

## <a name="extend-the-app-at-prediction-time"></a>Uygulamayı tahmin zamanında genişletme

Tahmin çalışma zamanında uygulamanın nasıl genişletileceği hakkında [kavramları](schema-change-prediction-runtime.md) öğrenin.

## <a name="deprecation"></a>Kullanımdan kaldırma

V2 API 'SI, v3 önizlemesi sonrasında en az 9 ay boyunca kullanım dışı olmayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

LUıS [Endpoint](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0/operations/5cb0a9459a1fe8fa44c28dd8) API 'LERINE mevcut Rest çağrılarını güncelleştirmek IÇIN v3 API belgelerini kullanın.
