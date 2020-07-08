---
title: Olay Işleyicileri ve hedefleri-Azure Event Grid IoT Edge | Microsoft Docs
description: Event Grid uç tarafında olay Işleyicileri ve hedefler
author: femila
ms.author: femila
ms.reviewer: spelluru
ms.date: 01/09/2020
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 4e144461b80dca2cf02c0cc06d7feee882293848
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84557629"
---
# <a name="event-handlers-and-destinations-in-event-grid-on-edge"></a>Event Grid uç tarafında olay Işleyicileri ve hedefler

Olay işleyicisi, daha fazla eylem veya olayı işlemek için olayın bulunduğu yerdir. Edge modülünde Event Grid, olay işleyicisi aynı sınır cihazında, başka bir cihazda veya bulutta olabilir. Herhangi bir Web kancasını kullanarak olayları idare edebilir veya Azure Event Grid gibi yerel işleyicilerden birine olay gönderebilirsiniz.

Bu makalede, her birinin nasıl yapılandırılacağı hakkında bilgi verilmektedir.

## <a name="webhook"></a>Web kancası

Bir Web kancası uç noktasına yayımlamak için, öğesini `endpointType` olarak ayarlayın `WebHook` ve şunları belirtin:

* endpointUrl: Web kancası uç noktası URL 'SI

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-webhook-endpoint>"
              }
            }
          }
        }
    ```

## <a name="azure-event-grid"></a>Azure Event Grid

Azure Event Grid bulut uç noktasına yayımlamak için, öğesini olarak ayarlayın `endpointType` `eventGrid` ve şunları sağlayın:

* endpointUrl: Event Grid konu URL 'SI buluta
* sasKey: Event Grid konunun SAS anahtarı
* topicName: Event Grid tüm giden olayları damgalamak için ad. Konu adı bir Event Grid etki alanı konusuna naklederken faydalıdır.

   ```json
        {
          "properties": {
            "destination": {
              "endpointType": "eventGrid",
              "properties": {
                 "endpointUrl": "<your-event-grid-cloud-topic-endpoint-url>?api-version=2018-01-01",
                 "sasKey": "<your-event-grid-topic-saskey>",
                 "topicName": null
              }
            }
          }
    }
   ```

## <a name="iot-edge-hub"></a>IoT Edge hub 'ı

Bir uç hub modülüne yayımlamak için, öğesini olarak ayarlayın `endpointType` `edgeHub` ve şunları belirtin:

* outputName: Event Grid modülünün, bu abonelikle eşleşen olayları edgeHub 'a yönlendireceğini belirten çıktı. Örneğin, aşağıdaki abonelikle eşleşen olaylar/messages/modules/eventgridmodule/outputs/sampleSub4. 'e yazılacak

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "edgeHub",
              "properties": {
                "outputName": "sampleSub4"
              }
            }
          }
        }
    ```

## <a name="event-hubs"></a>Event Hubs

Bir olay hub 'ına yayımlamak için, öğesini `endpointType` olarak ayarlayın `eventHub` ve şunları belirtin:

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

## <a name="service-bus-queues"></a>Service Bus Kuyrukları

Service Bus bir sıraya yayımlamak için, öğesini olarak ayarlayın `endpointType` `serviceBusQueue` ve şunları belirtin:

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

## <a name="service-bus-topics"></a>Service Bus Konuları

Service Bus bir konuya yayımlamak için, öğesini olarak ayarlayın `endpointType` `serviceBusTopic` ve şunları belirtin:

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

## <a name="storage-queues"></a>Depolama Kuyrukları

Bir depolama kuyruğuna yayımlamak için, öğesini olarak ayarlayın `endpointType` `storageQueue` ve şunları belirtin:

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