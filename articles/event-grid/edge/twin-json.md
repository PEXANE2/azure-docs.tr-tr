---
title: Modül Ikizi-Azure Event Grid IoT Edge | Microsoft Docs
description: Modül Ikizi aracılığıyla yapılandırma.
author: HiteshMadan
manager: rajarv
ms.author: himad
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 5c23b9ef280a4a4e3458d279ecf060d2e3d50295
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "72992151"
---
# <a name="module-twin-json-schema"></a>Module ikizi JSON şeması

IoT Edge Event Grid, IoT Edge ekosistemiyle tümleştirilir ve Ikizi Module aracılığıyla konu ve abonelik oluşturmayı destekler. Ayrıca, tüm konuların ve olay aboneliklerinin geçerli durumunu, Ikizi modülündeki bildirilen özelliklere bildirir.

> [!WARNING]
> IoT Edge ekosistemindeki sınırlamalar nedeniyle, aşağıdaki JSON örnekteki tüm dizi öğeleri JSON dizeleri olarak kodlanır. Aşağıdaki `EventSubscription.Filter.EventTypes` örnekteki `EventSubscription.Filter.AdvancedFilters` ve anahtarlarına bakın.

## <a name="desired-properties-json"></a>İstenen özellikler JSON

* Konular bölümündeki her anahtar-değer çiftinin değeri, konu başlıkları oluştururken API üzerinde için `Topic.Properties` kullanılan JSON şemasına tamamen sahiptir.
* **Eventabonelikler** bölümündeki her anahtar-değer çiftinin değeri, konu BAŞLıKLARı oluştururken API üzerinde için `EventSubscription.Properties` kullanılan JSON şemasına tamamen sahiptir.
* Bir konuyu silmek için, değerini istenen özelliklerde olarak `null` ayarlayın.
* Olay aboneliklerinin istenen özellikler aracılığıyla silinmesi desteklenmiyor.

```json
{
    "topics": {
        "twinTopic1": {},
        "twinTopic2": {
            "inputSchema": "customEventSchema"
        }
    },
    "eventSubscriptions": {
        "twinTopic1WebhookSub": {
            "topic": "twinTopic1",
            "retryPolicy": {
                "eventExpiryInMinutes": 120,
                "maxDeliveryAttempts": 30
            },
            "destination": {
                "endpointType": "WebHook",
                "properties": {
                    "endpointUrl": "https://localhost:4438"
                }
            },
            "filter": {
                "subjectBeginsWith": "^",
                "subjectEndsWith": "$",
                "isSubjectCaseSensitive": false,
                "includedEventTypes": "[\"et1\",\"et2\"]",
                "advancedFilters": "[{\"value\":true,\"operatorType\":\"BoolEquals\",\"key\":\"data.b\"},{\"values\":[\"\\\"\",\"c\"],    \"operatorType\":\"StringContains\",\"key\":\"data.s\"}]"
            }
        },
        "twinTopic2EdgeHubSub": {
            "topic": "twinTopic2",
            "deliveryPolicy": {
                "approxBatchSizeInBytes": 200000,
                "maxEventsPerBatch": 25
            },
            "destination": {
                "endpointType": "EdgeHub",
                "properties": {
                    "outputName": "twinTopic2EdgeHubSub"
                }
            },
            "filter": {
                "advancedFilters": "[{\"value\":true,\"operatorType\":\"BoolEquals\",\"key\":\"dAt\\\"A.a\"},{\"values\":[\"\\\"\",    \"c\"],\"operatorType\":\"StringContains\",\"key\":\"dAt\\\"A.a\"}]"
            }
        }
    }
}
```

## <a name="reported-properties-json"></a>Raporlanan Özellikler JSON

İkizi modülünün bildirilen Özellikler bölümünde aşağıdaki bilgiler yer alır:

* Modülün deposunda mevcut olan konular ve abonelikler kümesi
* İstenen konular/olay abonelikleri oluşturulurken karşılaşılan hatalar
* Tüm önyükleme hataları (istenen özellikler JSON ayrıştırması başarısız oldu)

```json
{
    "topics": {
        "twinTopic1": {},
        "twinTopic2": {
            "inputSchema": "customEventSchema"
        }
    },
    "eventSubscriptions": {
        "twinTopic1WebhookSub": {
            "topic": "twinTopic1",
            "retryPolicy": {
                "eventExpiryInMinutes": 120,
                "maxDeliveryAttempts": 30
            },
            "destination": {
                "endpointType": "WebHook",
                "properties": {
                    "endpointUrl": "https://localhost:4438"
                }
            },
            "filter": {
                "subjectBeginsWith": "^",
                "subjectEndsWith": "$",
                "isSubjectCaseSensitive": false,
                "includedEventTypes": "[\"et1\",\"et2\"]",
                "advancedFilters": "[{\"value\":true,\"operatorType\":\"BoolEquals\",\"key\":\"data.b\"},{\"values\":[\"\\\"\",\"c\"],    \"operatorType\":\"StringContains\",\"key\":\"data.s\"}]"
            }
        },
        "twinTopic2EdgeHubSub": {
            "topic": "twinTopic2",
            "deliveryPolicy": {
                "approxBatchSizeInBytes": 200000,
                "maxEventsPerBatch": 25
            },
            "destination": {
                "endpointType": "EdgeHub",
                "properties": {
                    "outputName": "twinTopic2EdgeHubSub"
                }
            },
            "filter": {
                "advancedFilters": "[{\"value\":true,\"operatorType\":\"BoolEquals\",\"key\":\"dAt\\\"A.a\"},{\"values\":[\"\\\"\",    \"c\"],\"operatorType\":\"StringContains\",\"key\":\"dAt\\\"A.a\"}]"
            }
        }
    },
    "errors": {
        "bootupMessage": "",
        "bootupException": "",
        "topics": {},
        "eventSubscriptions": {
            "twinTopic1EventGridUserTopicSub": "HttpStatusCode='BadRequest' ErrorCode='InvalidDestination' Message='EventSubscription.Properties.Destination failed validation. Reason: EndpointUrl must target the /api/events API of Azure Event Grid in the cloud..'"
        }
    }
}
```
