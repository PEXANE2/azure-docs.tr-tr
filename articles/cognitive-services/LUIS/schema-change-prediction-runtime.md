---
title: Uygulamayı çalışma zamanında genişlet - LUIS
description: ''
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: c0f9d71f5d89d73d9cdce2a2f646859d8eba3adc
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538583"
---
# <a name="extend-app-at-prediction-runtime"></a>Tahmin çalışma zamanında uygulamayı genişletme

Uygulamanın şeması (modeller ve özellikler) eğitilmiş ve tahmin bitiş noktasına yayınlanır. Bu yayımlanmış model tahmin çalışma zamanında kullanılır. Öngörüyü artırmak için kullanıcının söyleyişiyle birlikte tahmin çalışma süresine yeni bilgiler iletebilirsiniz.

İki tahmin çalışma zamanı şema değişiklikleri şunlardır:
* [Dış varlıklar](#external-entities)
* [Dinamik listeler](#dynamic-lists)

<a name="external-entities-passed-in-at-prediction-time"></a>

## <a name="external-entities"></a>Dış varlıklar

Dış varlıklar, LUIS uygulamanıza çalışma zamanı sırasında varlıkları tanımlama ve etiketleme olanağı verir ve bu da mevcut varlıklar için özellik olarak kullanılabilir. Bu, tahmin bitiş noktanıza sorgu göndermeden önce kendi ayrı ve özel varlık çıkarıcılarınızı kullanmanıza olanak tanır. Bu sorgu tahmin bitiş noktasında yapıldığından, modelinizi yeniden e-yılve yayımlamanız gerekmez.

İstemci uygulaması, varlık eşleştirmesini yöneterek ve eşleşen varlığın söyleyerek konumunu belirleyerek ve bu bilgileri istekle birlikte göndererek kendi varlık çıkarıcısını sağlar.

Dış varlıklar, diğer modellere sinyal olarak kullanılırken herhangi bir varlık türünü genişletme mekanizmasıdır.

Bu, yalnızca sorgu tahmini çalışma zamanında kullanılabilir veri olan bir varlık için yararlıdır. Bu tür verilere örnek olarak, kullanıcı başına sürekli veri veya belirli veriler değişir. Bir LUIS iletişim kuruluşunu, kullanıcının kişi listesindeki dış bilgilerle genişletebilirsiniz.

Dış varlıklar, V3'ün API'yi yazmasının bir parçasıdır. Bu sürüme [geçiş](luis-migration-api-v3.md) hakkında daha fazla bilgi edinin.

### <a name="entity-already-exists-in-app"></a>Varlık zaten uygulamada var

Dış varlık `entityName` için değeri, bitiş noktası istek POST gövdesi geçti, zaten istek yapıldığı anda eğitimli ve yayınlanan uygulamada mevcut olmalıdır. Varlık türü önemli değil, her türlü desteklenir.

### <a name="first-turn-in-conversation"></a>Konuşmada ilk dönüş

Bir kullanıcı aşağıdaki eksik bilgileri girer bir sohbet bot konuşma ilk söyleyerek düşünün:

`Send Hazem a new message`

Sohbet robotundan LUIS'e gelen istek, POST `Hazem` gövdesindeki bilgileri aktarabilir, böylece kullanıcının kişilerinden biri olarak doğrudan eşleşir.

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

Tahmin yanıtı, istekte tanımlandığı için, diğer tüm öngörülen varlıklarla birlikte dış varlığı içerir.

### <a name="second-turn-in-conversation"></a>Konuşmada ikinci dönüş

Sohbet bot içine bir sonraki kullanıcı söyleyerek daha belirsiz bir terim kullanır:

`Send him a calendar reminder for the party.`

Konuşmanın bu dönüşünde, söyleyiş `him` bir referans `Hazem`olarak kullanır. Konuşma sohbet bot, POST gövdesinde, `him` varlık değeri ilk söyleyiş çıkarılan harita `Hazem`olabilir.

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

Tahmin yanıtı, istekte tanımlandığı için, diğer tüm öngörülen varlıklarla birlikte dış varlığı içerir.

### <a name="override-existing-model-predictions"></a>Varolan model tahminlerini geçersiz kılma

Seçenekler `preferExternalEntities` özelliği, kullanıcı nın aynı ada sahip tahmin edilen bir varlıkla çakışan bir dış varlık gönderirse, LUIS'in geçen varlığı veya modelde var olan varlığı seçtiğini belirtir.

Örneğin, sorguyu `today I'm free`göz önünde bulundurun. LUIS aşağıdaki `today` yanıt ile bir datetimeV2 olarak algılar:

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

`preferExternalEntities` Ayarlanan `false`, LUIS dış varlık gönderilmemiş gibi bir yanıt döndürür.

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

`true`Ayarlanmışsa, `preferExternalEntities` LUIS aşağıdakiler dahil olmak üzere bir yanıt verir:

```JSON
"datetimeV2": [
    {
        "date": "2019-06-21"
    }
]
```



#### <a name="resolution"></a>Çözüm

İsteğe _bağlı_ `resolution` özellik, dış varlıkla ilişkili meta verileri geçirmenize ve ardından yanıtta geri almanıza olanak tanıyan tahmin yanıtında geri döner.

Birincil amaç, önceden oluşturulmuş varlıkları genişletmektir, ancak bu varlık türüyle sınırlı değildir.

Özellik `resolution` bir sayı, bir dize, nesne veya bir dizi olabilir:

* "Dallas"
* {"text": "değer"}
* 12345
* ["a", "b", "c"]

<a name="dynamic-lists-passed-in-at-prediction-time"></a>

## <a name="dynamic-lists"></a>Dinamik listeler

Dinamik listeler, LUIS uygulamasında bulunan mevcut eğitimli ve yayınlanmış bir liste varlığını genişletmenize olanak sağlar.

Liste varlık değerlerinizin düzenli aralıklarla değişmesi gerektiğinde bu özelliği kullanın. Bu özellik, önceden eğitilmiş ve yayınlanmış bir liste varlığını genişletmenize olanak tanır:

* Sorgu tahmin bitiş noktası isteği sırasında.
* Tek bir istek için.

Liste varlığı LUIS uygulamasında boş olabilir, ancak var olması gerekir. LUIS uygulamasındaki liste varlığı değiştirilmez, ancak bitiş noktasındaki tahmin yeteneği yaklaşık 1.000 öğeiçeren en fazla 2 liste içerecek şekilde genişletilir.

### <a name="dynamic-list-json-request-body"></a>Dinamik liste JSON istek gövdesi

Listeye eşanlamlılarla birlikte yeni bir alt liste eklemek için aşağıdaki JSON gövdesini gönderin ve `LUIS`sorgu `POST` tahmini isteğiyle metin için liste varlığını tahmin edin:

```JSON
{
    "query": "Send Hazem a message to add an item to the meeting agenda about LUIS.",
    "options":{
        "timezoneOffset": "-8:00"
    },
    "dynamicLists": [
        {
            "listEntity*":"ProductList",
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

Tahmin yanıtı, istekte tanımlandığı için, diğer tüm öngörülen varlıklarla birlikte bu liste varlığını içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Tahmin puanı](luis-concept-prediction-score.md)
* [API V3 değişikliklerini yazma](luis-migration-api-v3.md)
