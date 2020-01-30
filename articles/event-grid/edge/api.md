---
title: REST API-Azure Event Grid IoT Edge | Microsoft Docs
description: IoT Edge Event Grid REST API.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 19f86b1d8233e05844201e1095c1f79324955cd7
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841838"
---
# <a name="rest-api"></a>REST API
Bu makalede IoT Edge Azure Event Grid REST API 'Leri açıklanmaktadır

## <a name="common-api-behavior"></a>Ortak API davranışı

### <a name="base-url"></a>Taban URL 'SI
IoT Edge Event Grid, HTTP (bağlantı noktası 5888) ve HTTPS (bağlantı noktası 4438) üzerinden sunulan aşağıdaki API 'Lere sahiptir.

* HTTP için temel URL: http://eventgridmodule:5888
* HTTPS için temel URL: https://eventgridmodule:4438

### <a name="request-query-string"></a>Sorgu dizesi iste
Tüm API istekleri aşağıdaki sorgu dizesi parametresini gerektirir:

```?api-version=2019-01-01-preview```

### <a name="request-content-type"></a>İstek içerik türü
Tüm API isteklerinin **Içerik türünde**olması gerekir.

**Eventgridschema** veya **CustomSchema**söz konusu olduğunda, Content-Type değeri aşağıdaki değerlerden biri olabilir:

```Content-Type: application/json```

```Content-Type: application/json; charset=utf-8```

Yapılandırılmış modda **CloudEventSchemaV1_0** olması durumunda, içerik türü değeri aşağıdaki değerlerden biri olabilir:

```Content-Type: application/cloudevents+json```
    
```Content-Type: application/cloudevents+json; charset=utf-8```
    
```Content-Type: application/cloudevents-batch+json```
    
```Content-Type: application/cloudevents-batch+json; charset=utf-8```

İkili modda **CloudEventSchemaV1_0** olması durumunda Ayrıntılar için [belgelere](https://github.com/cloudevents/spec/blob/master/http-protocol-binding.md) bakın.

### <a name="error-response"></a>Hata yanıtı
Tüm API 'Ler aşağıdaki yük ile bir hata döndürüyor:

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

### <a name="put-topic-create--update"></a>Konuyu Yerleştir (oluştur/güncelleştir)

**İstek**: ``` PUT /topics/<topic_name>?api-version=2019-01-01-preview ```

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

**Yanıt**: http 200

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

### <a name="get-topic"></a>Konuyu al

**İstek**: ``` GET /topics/<topic_name>?api-version=2019-01-01-preview ```

**Yanıt**: http 200

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

### <a name="get-all-topics"></a>Tüm konuları al

**İstek**: ``` GET /topics?api-version=2019-01-01-preview ```

**Yanıt**: http 200

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

### <a name="delete-topic"></a>Konuyu sil

**İstek**: ``` DELETE /topics/<topic_name>?api-version=2019-01-01-preview ```

**Yanıt**: http 200, boş yük

## <a name="manage-event-subscriptions"></a>Olay aboneliklerini yönetme
Bu bölümdeki örneklerde `EndpointType=Webhook;`kullanın. `EndpointType=EdgeHub / EndpointType=EventGrid` için JSON örnekleri sonraki bölümde bulunur. 

### <a name="put-event-subscription-create--update"></a>Olay aboneliği koy (oluştur/güncelleştir)

**İstek**: ``` PUT /topics/<topic_name>/eventSubscriptions/<subscription_name>?api-version=2019-01-01-preview ```

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

**Yanıt**: http 200

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


### <a name="get-event-subscription"></a>Olay aboneliğini al

**İstek**: ``` GET /topics/<topic_name>/eventSubscriptions/<subscription_name>?api-version=2019-01-01-preview ```

**Yanıt**: http 200

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

### <a name="get-event-subscriptions"></a>Olay abonelikleri al

**İstek**: ``` GET /topics/<topic_name>/eventSubscriptions?api-version=2019-01-01-preview ```

**Yanıt**: http 200

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

### <a name="delete-event-subscription"></a>Olay aboneliğini Sil

**İstek**: ``` DELETE /topics/<topic_name>/eventSubscriptions/<subscription_name>?api-version=2019-01-01-preview ```

**Yanıt**: http 200, yük yok


## <a name="publish-events-api"></a>Olayları yayımlama API 'SI

### <a name="send-batch-of-events-in-event-grid-schema"></a>Olay toplu işi gönderme (Event Grid şemasında)

**İstek**: ``` POST /topics/<topic_name>/events?api-version=2019-01-01-preview ```

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

**Yanıt**: http 200, boş yük


**Yük alanı açıklamaları**
- ```Id``` zorunludur. Bu, çağıran tarafından doldurulan herhangi bir dize değeri olabilir. Event Grid, hiçbir yinelenen algılama yapmaz veya bu alanda hiçbir semantiğe zorlanır.
- ```Topic``` isteğe bağlıdır, ancak belirtilirse, istek URL 'sinden topic_name eşleşmesi gerekir
- ```Subject``` zorunludur, herhangi bir dize değeri olabilir
- ```EventType``` zorunludur, herhangi bir dize değeri olabilir
- ```EventTime``` zorunludur, bu doğrulanmaz ve uygun bir DateTime olmalıdır.
- ```DataVersion``` zorunludur
- ```MetadataVersion``` isteğe bağlıdır, belirtilmişse değere sahip bir dize olmalıdır ```"1"```
- ```Data``` isteğe bağlıdır ve herhangi bir JSON belirteci (sayı, dize, Boolean, dizi, nesne) olabilir

### <a name="send-batch-of-events-in-custom-schema"></a>Olay toplu işlemini gönder (özel şemada)

**İstek**: ``` POST /topics/<topic_name>/events?api-version=2019-01-01-preview ```

```json
[
    {
        ...
    }
]
```

**Yanıt**: http 200, boş yük


**Yük kısıtlamaları**
- Olayların bir dizisi OLMALıDıR.
- Her dizi girişi bir JSON nesnesi OLMALıDıR.
- Başka kısıtlama yok (yük boyutu dışında).

## <a name="examples"></a>Örnekler

### <a name="set-up-topic-with-eventgrid-schema"></a>EventGrid şeması ile konuyu ayarlama
Olaylarınızın **eventgridschema**'da yayımlanmasını gerektirmek için bir konu ayarlar.

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
`customschema`' de olayların yayımlanmasını gerektirecek bir konu oluşturur.

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
`cloudeventschema`' de olayların yayımlanmasını gerektirecek bir konu oluşturur.

```json
    {
        "name": "mycloudeventschematopic",
        "properties":
        {
            "inputSchema": "CloudEventSchemaV1_0"
        }
    }
```

### <a name="set-up-webhook-as-destination-events-to-be-delivered-in-eventgridschema"></a>Web kancasını hedef olarak ayarlama, eventgridschema 'da teslim edilecek olaylar
Başka bir modüle (bir HTTP uç noktası barındıran) veya ağ/internet üzerindeki herhangi bir HTTP adreslenebilir uç noktasına olay göndermek için bu hedef türünü kullanın.

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

`endpointUrl` özniteliğinde kısıtlamalar:
- Null olmayan bir değer olmalıdır.
- Mutlak bir URL olmalıdır.
- Outbound__webhook__httpsOnly EventGridModule ayarlarında true olarak ayarlandıysa, yalnızca HTTPS olmalıdır.
- Outbound__webhook__httpsOnly false olarak ayarlandıysa, HTTP veya HTTPS olabilir.

`eventDeliverySchema` özelliğindeki kısıtlamalar:
- Abone olan konunun giriş şemasıyla eşleşmesi gerekir.
- Null olabilir. Bu, konunun giriş şemasını varsayılan olarak belirler.

### <a name="set-up-iot-edge-as-destination"></a>IoT Edge hedef olarak ayarlama

IoT Edge hub 'ına olay göndermek ve tabi to Edge hub 'ının yönlendirme/filtreleme/iletme alt sistemi olmak için bu hedefi kullanın.

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

### <a name="set-up-event-grid-cloud-as-destination"></a>Event Grid bulutu hedef olarak ayarlama

Buluta (Azure) Event Grid olayları göndermek için bu hedefi kullanın. Öncelikle, bir bulutta olay aboneliği oluşturmadan önce, hangi olayların gönderilmesi gerektiği için buluta bir Kullanıcı konusu ayarlamanız gerekir.

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

EndpointUrl
- Null olmayan bir değer olmalıdır.
- Mutlak bir URL olmalıdır.
- Yol `/api/events`, istek URL 'SI yolunda tanımlanmalıdır.
- Sorgu dizesinde `api-version=2018-01-01` olması gerekir.
- Outbound__eventgrid__httpsOnly EventGridModule ayarlarında true olarak ayarlanırsa (varsayılan olarak true), yalnızca HTTPS olmalıdır.
- Outbound__eventgrid__httpsOnly false olarak ayarlanırsa, HTTP veya HTTPS olabilir.
- Outbound__eventgrid__allowInvalidHostnames false olarak ayarlandıysa (varsayılan olarak false), aşağıdaki uç noktalardan birini hedeflemelidir:
   - `eventgrid.azure.net`
   - `eventgrid.azure.us`
   - `eventgrid.azure.cn`

SasKey:
- Null olmayan bir değer olmalıdır.

TopicName:
- Abonelik. EventDeliverySchema, EventGridSchema olarak ayarlandıysa, bu alandaki değer, bulutta Event Grid iletilmeden önce her olayın konu alanına konur.
- Abonelik. EventDeliverySchema, CustomEventSchema olarak ayarlandıysa, bu özellik yok sayılır ve özel olay yükü aynen alındığı gibi iletilir.

## <a name="set-up-event-hubs-as-a-destination"></a>Event Hubs hedef olarak ayarlama

Bir olay hub 'ına yayımlamak için `endpointType` `eventHub` ayarlayın ve şunları sağlayın:

* connectionString: hedeflediğiniz belirli olay hub 'ı için, paylaşılan erişim Ilkesi aracılığıyla oluşturulmuş bağlantı dizesi.

    >[!NOTE]
    > Bağlantı dizesi varlığa özgü olmalıdır. Ad alanı bağlantı dizesinin kullanılması çalışmayacak. Azure portalında yayınlamak istediğiniz belirli olay hub 'ına giderek ve **paylaşılan erişim ilkeleri** ' ne tıklayarak, varlığa özel bağlantı dizesi oluşturarak yeni bir varlığa özgü bağlama dizesi oluşturabilirsiniz.

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

## <a name="set-up-service-bus-queues-as-a-destination"></a>Service Bus kuyruklarını hedef olarak ayarlama

Service Bus kuyruğuna yayımlamak için `endpointType` `serviceBusQueue` olarak ayarlayın ve şunları sağlayın:

* connectionString: hedeflediğiniz belirli Service Bus kuyruğu için bağlantı dizesi, paylaşılan erişim Ilkesi aracılığıyla üretildi.

    >[!NOTE]
    > Bağlantı dizesi varlığa özgü olmalıdır. Ad alanı bağlantı dizesinin kullanılması çalışmayacak. Azure portalında yayımlamak istediğiniz belirli Service Bus kuyruğuna giderek ve **paylaşılan erişim ilkeleri** ' ne tıklayarak, yeni varlığa özel bağlantı dizesi oluşturmak için varlığa özgü bir bağlantı dizesi oluşturun.

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

## <a name="set-up-service-bus-topics-as-a-destination"></a>Service Bus konuları hedef olarak ayarlama

Service Bus bir konuya yayımlamak için `endpointType` `serviceBusTopic` olarak ayarlayın ve şunları sağlayın:

* connectionString: bir paylaşılan erişim Ilkesi aracılığıyla hedeflediğiniz özel Service Bus konusu için bağlantı dizesi.

    >[!NOTE]
    > Bağlantı dizesi varlığa özgü olmalıdır. Ad alanı bağlantı dizesinin kullanılması çalışmayacak. Azure portalında yayımlamak istediğiniz belirli Service Bus konusuna giderek ve **paylaşılan erişim ilkeleri** ' ne tıklayarak, yeni varlığa özel bir bağlantı dizesi oluşturmak için varlığa özel bağlantı dizesi oluşturun.

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

## <a name="set-up-storage-queues-as-a-destination"></a>Depolama kuyruklarını hedef olarak ayarlama

Bir depolama kuyruğuna yayımlamak için `endpointType` `storageQueue` ayarlayın ve şunları sağlayın:

* SıraAdı: yayımlamakta olduğunuz depolama kuyruğunun adı.
* connectionString: depolama sırasının bulunduğu depolama hesabı için bağlantı dizesi.

    >[!NOTE]
    > Satır Event Hubs, Service Bus kuyrukları ve Service Bus konuları, depolama kuyrukları için kullanılan bağlantı dizesi varlığa özgü değildir. Bunun yerine, depolama hesabı için bağlantı dizesi gerekir.

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