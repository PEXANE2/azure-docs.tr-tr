---
title: Modül İkiz - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: Modül Twin ile yapılandırma.
author: HiteshMadan
manager: rajarv
ms.author: himad
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 5c23b9ef280a4a4e3458d279ecf060d2e3d50295
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72992151"
---
# <a name="module-twin-json-schema"></a>Modül ikiz JSON şema

IoT Edge'deki Event Grid, IoT Edge ekosistemiyle bütünleşir ve Modül İkiz üzerinden konu ve abonelik oluşturmayı destekler. Ayrıca, Modül İkiz'de bildirilen özelliklere tüm konuların ve olay aboneliklerinin geçerli durumunu bildirir.

> [!WARNING]
> IoT Edge ekosistemindeki sınırlamalar nedeniyle, aşağıdaki json örneğindeki tüm dizi öğeleri json dizeleri olarak kodlanmıştır. Aşağıdaki `EventSubscription.Filter.EventTypes` `EventSubscription.Filter.AdvancedFilters` örnekte ve tuşlara bakın.

## <a name="desired-properties-json"></a>İstenilen özellikler JSON

* Konular bölümündeki her anahtar değer çiftinin değeri, konu oluştururken `Topic.Properties` API'de kullanılan JSON şemasına sahiptir.
* **EventSubscriptions** bölümündeki her anahtar değer çiftinin değeri, konu oluştururken `EventSubscription.Properties` API'de kullanılan json şemasına sahiptir.
* Bir konuyu silmek için, `null` değerini istenen özelliklere ayarlayın.
* İstenilen özellikler üzerinden olay aboneliklerinin silmesi desteklenmez.

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

## <a name="reported-properties-json"></a>Bildirilen özellikler JSON

Modül ikizinin bildirilen özellikler bölümü aşağıdaki bilgileri içerir:

* Modülün deposunda bulunan konular ve abonelikler kümesi
* İstenilen konular/olay abonelikleri oluşturulurken karşılaşılan hatalar
* Herhangi bir önyükleme hataları (istenilen özellikleri JSON ayrıştırma başarısız gibi)

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
