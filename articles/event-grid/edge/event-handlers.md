---
title: Etkinlik İşleyicileri ve varış noktaları - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: Kenardaki Olay Izgarasında Etkinlik İşleyicileri ve varış noktaları
author: banisadr
ms.author: babanisa
ms.reviewer: spelluru
ms.date: 01/09/2020
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 35bf5af90aa5f0456aa8d68f0e4e8aaacc6cf84f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76849753"
---
# <a name="event-handlers-and-destinations-in-event-grid-on-edge"></a>Kenardaki Olay Izgarasında Etkinlik İşleyicileri ve varış noktaları

Olay işleyicisi, olayın daha fazla eylem veya olayı işlemek için bulunduğu yerdir. Olay Izgarası Edge modülü ile, olay işleyicisi aynı kenar aygıtında, başka bir aygıtta veya bulutta olabilir. Olayları işlemek için herhangi bir WebHook'u kullanabilir veya Olayları Azure Olay Idamı gibi yerel işleyicilerinden birine gönderebilirsiniz.

Bu makalede, her yapılandırılması hakkında bilgi sağlar.

## <a name="webhook"></a>WebHook

Bir WebHook bitiş noktasına yayımlamak `endpointType` `WebHook` için, aşağıdakileri ayarlayın ve sağlayın:

* endpointUrl: WebHook uç nokta URL

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

Azure Olay Izgara bulutu bitiş `endpointType` noktasına `eventGrid` yayımlamak için aşağıdakileri ayarlayın ve sağlayın:

* endpointUrl: Bulutta Olay Izgara Konu URL
* sasKey: Olay Grid Topic's SAS anahtarı
* topicName: Olay Grid tüm giden olayları damgalamak için ad. Konu adı, Olay Izgara Etki Alanı konusuna deftere nakledilirken yararlıdır.

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

## <a name="iot-edge-hub"></a>IoT Kenar Hub

Edge Hub modülünde yayımlamak `endpointType` için `edgeHub` aşağıdakileri ayarlayın ve sağlayın:

* outputName: Olay Izgara modülünün bu abonelikle eşleşen olayları edgeHub'a yönlendireceği çıktı. Örneğin, aşağıdaki abonelikle eşleşen olaylar /messages/modules/eventgridmodule/outputs/sampleSub4'e yazılacaktır.

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

## <a name="service-bus-queues"></a>Service Bus Kuyrukları

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

## <a name="service-bus-topics"></a>Service Bus Konuları

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

## <a name="storage-queues"></a>Depolama Kuyrukları

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