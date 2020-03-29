---
title: REST API - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: IoT Edge'deki Olay Izgara'da REST API.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 19f86b1d8233e05844201e1095c1f79324955cd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841838"
---
# <a name="rest-api"></a>REST API
Bu makalede, IoT Edge'deki Azure Olay Izgarasının REST API'leri açıklanmaktadır

## <a name="common-api-behavior"></a>Yaygın API davranışı

### <a name="base-url"></a>Temel URL
IoT Edge'deki Olay Izgarası, HTTP (port 5888) ve HTTPS (port 4438) üzerinde aşağıdaki API'ler maruz kalmıştır.

* HTTP için temel URL:http://eventgridmodule:5888
* HTTPS için temel URL:https://eventgridmodule:4438

### <a name="request-query-string"></a>Sorgu dizesi isteği
Tüm API istekleri aşağıdaki sorgu dize parametresini gerektirir:

```?api-version=2019-01-01-preview```

### <a name="request-content-type"></a>İçerik türü isteme
Tüm API isteklerinin bir **İçerik Türü**olmalıdır.

**EventGridSchema** veya **CustomSchema**durumunda, İçerik Türü değeri aşağıdaki değerlerden biri olabilir:

```Content-Type: application/json```

```Content-Type: application/json; charset=utf-8```

Yapılandırılmış modda **CloudEventSchemaV1_0** durumunda, İçerik Türü değeri aşağıdaki değerlerden biri olabilir:

```Content-Type: application/cloudevents+json```
    
```Content-Type: application/cloudevents+json; charset=utf-8```
    
```Content-Type: application/cloudevents-batch+json```
    
```Content-Type: application/cloudevents-batch+json; charset=utf-8```

İkili modda **CloudEventSchemaV1_0** durumunda, ayrıntılar için [belgelere](https://github.com/cloudevents/spec/blob/master/http-protocol-binding.md) bakın.

### <a name="error-response"></a>Hata yanıtı
Tüm API'ler aşağıdaki taşıma yüküyle ilgili bir hata döndürer:

```json
{
    "error":
    {
        "code": "<HTTP STATUS CODE>",
        "details": 
        {
            "code": "<Detailed Error Code>",
            "message": "..."
        }
    }
}
```

## <a name="manage-topics"></a>Konuları yönetme

### <a name="put-topic-create--update"></a>Konuyu koyun (oluşturma / güncelleştirme)

**İstek**:``` PUT /topics/<topic_name>?api-version=2019-01-01-preview ```

**Yük**:

```json
    {
        "name": "<topic_name>", // optional, inferred from URL. If specified must match URL topic_name
        "properties":
        {
            "inputSchema": "EventGridSchema | CustomEventSchema | CloudEventSchemaV1_0" // optional
        }
    }
```

**Cevap**: HTTP 200

**Yük**:

```json
{
    "id": "/iotHubs/<iot_hub_name>/devices/<iot_edge_device_id>/modules/<eventgrid_module_name>/topics/<topic_name>",
    "name": "<topic_name>",
    "type": "Microsoft.EventGrid/topics",
    "properties":
    {
        "endpoint": "<get_request_base_url>/topics/<topic_name>/events?api-version=2019-01-01-preview",
        "inputSchema": "EventGridSchema | CustomEventSchema | CloudEventSchemaV1_0" // populated with EventGridSchema if not explicitly specified in PUT request
    }
}
```

### <a name="get-topic"></a>Konuyu alın

**İstek**:``` GET /topics/<topic_name>?api-version=2019-01-01-preview ```

**Cevap**: HTTP 200

**Yük**:
```json
{
    "id": "/iotHubs/<iot_hub_name>/devices/<iot_edge_device_id>/modules/<eventgrid_module_name>/topics/<topic_name>",
    "name": "<topic_name>",
    "type": "Microsoft.EventGrid/topics",
    "properties":
    {
        "endpoint": "<request_base_url>/topics/<topic_name>/events?api-version=2019-01-01-preview",
        "inputSchema": "EventGridSchema | CustomEventSchema | CloudEventSchemaV1_0"
    }
}
```

### <a name="get-all-topics"></a>Tüm konuları alın

**İstek**:``` GET /topics?api-version=2019-01-01-preview ```

**Cevap**: HTTP 200

**Yük**:
```json
[
    {
        "id": "/iotHubs/<iot_hub_name>/devices/<iot_edge_device_id>/modules/<eventgrid_module_name>/topics/<topic_name>",
        "name": "<topic_name>",
        "type": "Microsoft.EventGrid/topics",
        "properties":
        {
            "endpoint": "<request_base_url>/topics/<topic_name>/events?api-version=2019-01-01-preview",
            "inputSchema": "EventGridSchema | CustomEventSchema | CloudEventSchemaV1_0"
        }
    },
    {
        "id": "/iotHubs/<iot_hub_name>/devices/<iot_edge_device_id>/modules/<eventgrid_module_name>/topics/<topic_name>",
        "name": "<topic_name>",
        "type": "Microsoft.EventGrid/topics",
        "properties":
        {
            "endpoint": "<request_base_url>/topics/<topic_name>/events?api-version=2019-01-01-preview",
            "inputSchema": "EventGridSchema | CustomEventSchema | CloudEventSchemaV1_0"
        }
    }
]
```

### <a name="delete-topic"></a>Konuyu silme

**İstek**:``` DELETE /topics/<topic_name>?api-version=2019-01-01-preview ```

**Yanıt**: HTTP 200, boş yük

## <a name="manage-event-subscriptions"></a>Etkinlik aboneliklerini yönetme
Bu bölümdeki `EndpointType=Webhook;`örnekler . Json örnekleri `EndpointType=EdgeHub / EndpointType=EventGrid` bir sonraki bölümde. 

### <a name="put-event-subscription-create--update"></a>Olay aboneliği koyma (oluşturma / güncelleştirme)

**İstek**:``` PUT /topics/<topic_name>/eventSubscriptions/<subscription_name>?api-version=2019-01-01-preview ```

**Yük**:
```json
{
    "name": "<subscription_name>", // optional, inferred from URL. If specified must match URL subscription_name
    "properties":
    {
        "topicName": "<topic_name>", // optional, inferred from URL. If specified must match URL topic_name
        "eventDeliverySchema": "EventGridSchema | CustomEventSchema | CloudEventSchemaV1_0", // optional
        "retryPolicy":  //optional
        {
            "eventExpiryInMinutes": 120,
            "maxDeliveryAttempts": 50
        },
        "persistencePolicy": "true",
        "destination":
        {
            "endpointType": "WebHook",
            "properties":
            {
                "endpointUrl": "<webhook_url>",
                "maxEventsPerBatch": 10, // optional
                "preferredBatchSizeInKilobytes": 1033 // optional
            }
        },
        "filter": // optional
        {
            "subjectBeginsWith": "...",
            "subjectEndsWith": "...",
            "isSubjectCaseSensitive": true|false,
            "includedEventTypes": ["...", "..."],
            "advancedFilters":
            [
                {
                    "OperatorType": "BoolEquals",
                    "Key": "...",
                    "Value": "..."
                },
                {
                    "OperatorType": "NumberLessThan",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberGreaterThan",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberLessThanOrEquals",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberGreaterThanOrEquals",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberIn",
                    "Key": "...",
                    "Values": [<number>, <number>, <number>]
                },
                {
                    "OperatorType": "NumberNotIn",
                    "Key": "...",
                    "Values": [<number>, <number>, <number>]
                },
                {
                    "OperatorType": "StringIn",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringNotIn",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringBeginsWith",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringEndsWith",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringContains",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                }
            ]
        }
    }
}
```

**Cevap**: HTTP 200

**Yük**:

```json
{
    "id": "/iotHubs/<iot_hub_name>/devices/<iot_edge_device_id>/modules/<eventgrid_module_name>/topics/<topic_name>/eventSubscriptions/<subscription_name>",
    "name": "<subscription_name>",
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "properties":
    {
        "topicName": "<topic_name>",
        "eventDeliverySchema": "EventGridSchema | CustomEventSchema  | CloudEventSchemaV1_0", // populated with EventGridSchema if not explicitly specified in PUT request
        "retryPolicy":  // only populated if specified in the PUT request
        {
            "eventExpiryInMinutes": 120,
            "maxDeliveryAttempts": 50
        },
        "destination":
        {
            "endpointType": "WebHook",
            "properties":
            {
                "endpointUrl": "<webhook_url>",
                "maxEventsPerBatch": 10, // optional
                "preferredBatchSizeInKilobytes": 1033 // optional
            }
        },
        "filter": // only populated if specified in the PUT request 
        {
            "subjectBeginsWith": "...",
            "subjectEndsWith": "...",
            "isSubjectCaseSensitive": true|false,
            "includedEventTypes": ["...", "..."],
            "advancedFilters":
            [
                {
                    "OperatorType": "BoolEquals",
                    "Key": "...",
                    "Value": "..."
                },
                {
                    "OperatorType": "NumberLessThan",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberGreaterThan",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberLessThanOrEquals",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberGreaterThanOrEquals",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberIn",
                    "Key": "...",
                    "Values": [<number>, <number>, <number>]
                },
                {
                    "OperatorType": "NumberNotIn",
                    "Key": "...",
                    "Values": [<number>, <number>, <number>]
                },
                {
                    "OperatorType": "StringIn",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringNotIn",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringBeginsWith",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringEndsWith",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringContains",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                }
            ]
        }
    }
}
```


### <a name="get-event-subscription"></a>Etkinlik aboneliği alın

**İstek**:``` GET /topics/<topic_name>/eventSubscriptions/<subscription_name>?api-version=2019-01-01-preview ```

**Cevap**: HTTP 200

**Yük**:
```json
{
    "id": "/iotHubs/<iot_hub_name>/devices/<iot_edge_device_id>/modules/<eventgrid_module_name>/topics/<topic_name>/eventSubscriptions/<subscription_name>",
    "name": "<subscription_name>",
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "properties":
    {
        "topicName": "<topic_name>",
        "eventDeliverySchema": "EventGridSchema | CustomEventSchema  | CloudEventSchemaV1_0", // populated with EventGridSchema if not explicitly specified in PUT request
        "retryPolicy":  // only populated if specified in the PUT request
        {
            "eventExpiryInMinutes": 120,
            "maxDeliveryAttempts": 50
        },
        "destination":
        {
            "endpointType": "WebHook",
            "properties":
            {
                "endpointUrl": "<webhook_url>",
                "maxEventsPerBatch": 10, // optional
                "preferredBatchSizeInKilobytes": 1033 // optional
            }
        },
        "filter": // only populated if specified in the PUT request 
        {
            "subjectBeginsWith": "...",
            "subjectEndsWith": "...",
            "isSubjectCaseSensitive": true|false,
            "includedEventTypes": ["...", "..."],
            "advancedFilters":
            [
                {
                    "OperatorType": "BoolEquals",
                    "Key": "...",
                    "Value": "..."
                },
                {
                    "OperatorType": "NumberLessThan",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberGreaterThan",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberLessThanOrEquals",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberGreaterThanOrEquals",
                    "Key": "...",
                    "Value": <number>
                },
                {
                    "OperatorType": "NumberIn",
                    "Key": "...",
                    "Values": [<number>, <number>, <number>]
                },
                {
                    "OperatorType": "NumberNotIn",
                    "Key": "...",
                    "Values": [<number>, <number>, <number>]
                },
                {
                    "OperatorType": "StringIn",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringNotIn",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringBeginsWith",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringEndsWith",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                },
                {
                    "OperatorType": "StringContains",
                    "Key": "...",
                    "Values": ["...", "...", "..."]
                }
            ]
        }
    }
}
```

### <a name="get-event-subscriptions"></a>Etkinlik abonelikleri alın

**İstek**:``` GET /topics/<topic_name>/eventSubscriptions?api-version=2019-01-01-preview ```

**Cevap**: HTTP 200

**Yük**:
```json
[
    {
        // same event-subscription json as that returned from Get-EventSubscription above
    },
    {
    },
    ...
]
```

### <a name="delete-event-subscription"></a>Olay aboneliğini silme

**İstek**:``` DELETE /topics/<topic_name>/eventSubscriptions/<subscription_name>?api-version=2019-01-01-preview ```

**Yanıt**: HTTP 200, yük yok


## <a name="publish-events-api"></a>Olayları yayımlama API'si

### <a name="send-batch-of-events-in-event-grid-schema"></a>Toplu etkinlik gönderme (Olay Izgara şemasında)

**İstek**:``` POST /topics/<topic_name>/events?api-version=2019-01-01-preview ```

```json
[
    {
        "id": "<user-defined-event-id>",
        "topic": "<topic_name>",
        "subject": "",
        "eventType": "",
        "eventTime": ""
        "dataVersion": "",
        "metadataVersion": "1",
        "data": 
            ...
    }
]
```

**Yanıt**: HTTP 200, boş yük


**Yük alanı açıklamaları**
- ```Id```zorunludur. Arayan tarafından doldurulan herhangi bir dize değeri olabilir. Olay Izgara sı bu alanda herhangi bir yinelenen algılama yapmaz veya herhangi bir semantik zorlamak.
- ```Topic```isteğe bağlıdır, ancak belirtilirse istek URL'sinden topic_name eşleşmesi gerekir
- ```Subject```zorunludur, herhangi bir dize değeri olabilir
- ```EventType```zorunludur, herhangi bir dize değeri olabilir
- ```EventTime```zorunludur, doğrulanmaz, ancak uygun bir DateTime olmalıdır.
- ```DataVersion```zorunludur
- ```MetadataVersion```isteğe bağlı, belirtilmişse değeri olan bir dize olmalıdır```"1"```
- ```Data```isteğe bağlıdır ve herhangi bir JSON belirteci (sayı, dize, boolean, dizi, nesne) olabilir

### <a name="send-batch-of-events-in-custom-schema"></a>Toplu etkinlik gönderme (özel şemada)

**İstek**:``` POST /topics/<topic_name>/events?api-version=2019-01-01-preview ```

```json
[
    {
        ...
    }
]
```

**Yanıt**: HTTP 200, boş yük


**Yük Kısıtlamaları**
- Olaylar bir dizi olmalıdır.
- Her dizi girişi bir JSON nesnesi olmalıdır.
- Başka kısıtlama yoktur (yük boyutu dışında).

## <a name="examples"></a>Örnekler

### <a name="set-up-topic-with-eventgrid-schema"></a>EventGrid şeması ile konuyu ayarlama
**Olayların eventgridschema'da**yayınlanmasını gerektirecek bir konu ayarlar.

```json
    {
        "name": "myeventgridtopic",
        "properties":
        {
            "inputSchema": "EventGridSchema"
        }
    }
```

### <a name="set-up-topic-with-custom-schema"></a>Özel şema ile konuyu ayarlama
Olayların 'da `customschema`yayımlanmasını gerektirecek bir konu ayarlar.

```json
    {
        "name": "mycustomschematopic",
        "properties":
        {
            "inputSchema": "CustomSchema"
        }
    }
```

### <a name="set-up-topic-with-cloud-event-schema"></a>Bulut olay şeması ile konuyu ayarlama
Olayların 'da `cloudeventschema`yayımlanmasını gerektirecek bir konu ayarlar.

```json
    {
        "name": "mycloudeventschematopic",
        "properties":
        {
            "inputSchema": "CloudEventSchemaV1_0"
        }
    }
```

### <a name="set-up-webhook-as-destination-events-to-be-delivered-in-eventgridschema"></a>WebHook'u hedef olarak ayarlama, eventgridschema'da teslim edilecek etkinlikler
Olayları başka bir modüle (http bitiş noktası barındıran) veya ağ/internetteki herhangi bir HTTP adreslenebilir bitiş noktasına göndermek için bu hedef türünü kullanın.

```json
{
    "properties":
    {
        "destination":
        {
            "endpointType": "WebHook",
            "properties":
            {
                "endpointUrl": "<webhook_url>",
                "eventDeliverySchema": "eventgridschema",
            }
        }
    }
}
```

`endpointUrl` Öznitelik üzerindeki kısıtlamalar:
- Hükümsüz olmalı.
- Mutlak bir URL olmalı.
- outbound__webhook__httpsOnly EventGridModule ayarlarında doğru ayarlanmışsa, yalnızca HTTPS olmalıdır.
- outbound__webhook__httpsOnly yanlış olarak ayarlanmışsa, http veya HTTPS olabilir.

`eventDeliverySchema` Özellik üzerindeki kısıtlamalar:
- Abone olan konunun giriş şemasıyla eşleşmesi gerekir.
- Hükümsüz olabilir. Bu konunun giriş şeması varsayılan.

### <a name="set-up-iot-edge-as-destination"></a>IoT Edge'i hedef olarak ayarlama

Olayları IoT Edge Hub'ına göndermek ve Edge Hub'ın yönlendirme/filtreleme/iletme alt sistemine tabi olmak için bu hedefi kullanın.

```json
{
    "properties":
    {
        "destination":
        {
            "endpointType": "EdgeHub",
            "properties":
            {
                "outputName": "<eventgridmodule_output_port_name>"
            }
        }
    }
}
```

### <a name="set-up-event-grid-cloud-as-destination"></a>Olay Izgara Bulutu'nun hedef olarak ayarla

Buluttaki Olay Izgarasına (Azure) olay görüntü göndermek için bu hedefi kullanın. Kenarda bir etkinlik aboneliği oluşturmadan önce bulutta öncelikle olayların hangi etkinliklere gönderilmesi gerektiğini gösteren bir kullanıcı konusu ayarlamanız gerekir.

```json
{
    "properties":
    {
        "destination":
        {
            "endpointType": "EventGrid",
            "properties":
            {
                "endpointUrl": "<eventgrid_user_topic_url>",
                "sasKey": "<user_topic_sas_key>",
                "topicName": "<new value to populate in forwarded EventGridEvent.Topic>" // if not specified, the Topic field on every event gets nulled out before being sent to Azure Event Grid
            }
        }
    }
}
```

EndpointUrl:
- Hükümsüz olmalı.
- Mutlak bir URL olmalı.
- Yol `/api/events` istek URL yolunda tanımlanmalıdır.
- Sorgu dizesinde olması `api-version=2018-01-01` gerekir.
- outbound__eventgrid__httpsOnly EventGridModule ayarlarında (varsayılan olarak geçerli) doğru olarak ayarlanmışsa, yalnızca HTTPS olmalıdır.
- outbound__eventgrid__httpsOnly yanlış olarak ayarlanmışsa, http veya HTTPS olabilir.
- outbound__eventgrid__allowInvalidHostnames false (varsayılan olarak false) olarak ayarlanmışsa, aşağıdaki uç noktalardan birini hedeflemelidir:
   - `eventgrid.azure.net`
   - `eventgrid.azure.us`
   - `eventgrid.azure.cn`

SasKey:
- Hükümsüz olmalı.

Konu Adı:
- Subscription.EventDeliverySchema EventGridSchema olarak ayarlanmışsa, buluttaki Olay Izgarası'na iletilmeden önce bu alandaki değer her etkinliğin Konu alanına konur.
- Subscription.EventDeliverySchema CustomEventSchema olarak ayarlanmışsa, bu özellik yoksayılır ve özel olay yükü tam olarak alındığı gibi iletilir.

## <a name="set-up-event-hubs-as-a-destination"></a>Etkinlik Hub'larını hedef olarak ayarlama

Bir Olay Hub'ında `endpointType` yayımlamak için aşağıdakileri `eventHub` ayarlayın ve sağlayın:

* connectionString: Hedeflediğiniz belirli Olay Hub'ı için bağlantı dizesi Paylaşılan Erişim İlkesi aracılığıyla oluşturulur.

    >[!NOTE]
    > Bağlantı dizesi varlığa özgü olmalıdır. Ad alanı bağlantı dizesi kullanmak çalışmaz. Azure Portalı'nda yayınlamak istediğiniz belirli Etkinlik Hub'ına giderek ve yeni bir varlığa özgü connecection dizesi oluşturmak için **Paylaşılan erişim ilkelerini** tıklatarak bir varlık özel bağlantı dizesi oluşturabilirsiniz.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "eventHub",
              "properties": {
                "connectionString": "<your-event-hub-connection-string>"
              }
            }
          }
        }
    ```

## <a name="set-up-service-bus-queues-as-a-destination"></a>Hizmet Veri Merkezi Kuyruklarını hedef olarak ayarlama

Hizmet Veri Servisi Kuyruğu'nda `endpointType` yayımlamak `serviceBusQueue` için aşağıdakileri ayarlayın ve aşağıdakileri sağlayın:

* connectionString: Hedeflediğiniz belirli Hizmet Veri Yolu Kuyruğu'nun bağlantı dizesi, Paylaşılan Erişim İlkesi aracılığıyla oluşturulur.

    >[!NOTE]
    > Bağlantı dizesi varlığa özgü olmalıdır. Ad alanı bağlantı dizesi kullanmak çalışmaz. Azure Portalı'nda yayınlamak istediğiniz belirli Hizmet Veri Servisi Kuyruğu'nda gezinerek ve yeni bir varlığa özgü connecection dizesini oluşturmak için **Paylaşılan erişim ilkelerini** tıklatarak bir varlık özel bağlantı dizesi oluşturun.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "serviceBusQueue",
              "properties": {
                "connectionString": "<your-service-bus-queue-connection-string>"
              }
            }
          }
        }
    ```

## <a name="set-up-service-bus-topics-as-a-destination"></a>Hizmet Veri Günü Konularını hedef olarak ayarlama

Bir Hizmet Veri Servisi Konu'da `endpointType` `serviceBusTopic` yayımlamak için aşağıdakileri ayarlayın ve sağlayın:

* connectionString: Hedeflediğiniz belirli Hizmet Veri Yolu Konusu için bağlantı dizesi Paylaşılan Erişim İlkesi aracılığıyla oluşturulur.

    >[!NOTE]
    > Bağlantı dizesi varlığa özgü olmalıdır. Ad alanı bağlantı dizesi kullanmak çalışmaz. Azure Portalı'nda yayınlamak istediğiniz belirli Hizmet Veri Servisi Konusu'nda gezinerek ve yeni bir varlığa özgü connecection dizesini oluşturmak için **Paylaşılan erişim ilkelerini** tıklatarak bir varlık özel bağlantı dizesi oluşturun.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "serviceBusTopic",
              "properties": {
                "connectionString": "<your-service-bus-topic-connection-string>"
              }
            }
          }
        }
    ```

## <a name="set-up-storage-queues-as-a-destination"></a>Depolama Kuyruklarını hedef olarak ayarlama

Depolama Kuyruğu'nda yayımlamak `endpointType` için `storageQueue` aşağıdakileri ayarlayın ve sağlayın:

* queueName: Yayımlamakta olduğunuz Depolama Kuyruğunun Adı.
* connectionString: Depolama Sırası'nın içinde olduğu Depolama Hesabı için bağlantı dizesi.

    >[!NOTE]
    > Sırasız Olay Hub'ları, Servis Veri Merkezi Kuyrukları ve Hizmet Veri Mesuli konular, Depolama Kuyrukları için kullanılan bağlantı dizesi entity özgü değildir. Bunun yerine, ancak Depolama Hesabı için bağlantı dizesi gerekir.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "storageQueue",
              "properties": {
                "queueName": "<your-storage-queue-name>",
                "connectionString": "<your-storage-account-connection-string>"
              }
            }
          }
        }
    ```