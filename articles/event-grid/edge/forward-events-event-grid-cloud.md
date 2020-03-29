---
title: İleri kenar olayları Event Grid bulutu - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: İleri kenar olayları Event Grid bulutuna
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 7184fb5c45ce41de2bd63b55fb67cbd9ba6361e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844726"
---
# <a name="tutorial-forward-events-to-event-grid-cloud"></a>Öğretici: Olayları Olay Izgara bulutuna iletme

Bu makale, kenar olaylarını Azure bulutundaki Olay Izgara'ya iletmek için gereken tüm adımları gözden geçirir. Aşağıdaki nedenlerle yapmak isteyebilirsiniz:

* Buluttaki kenar olaylarına tepki ver.
* Olayları bulutta Olay Kılavuzu'na iletin ve olayları bulutta işlemeden önce arabellek sağlamak için Azure Olay Hub'larını veya Azure Depolama kuyruklarını kullanın.

 Bu öğreticiyi tamamlamak için, [kenardaki](concepts.md) Olay Izgara kavramlarını ve [Azure'u](../concepts.md)anlamanız gerekir. Ek hedef türleri için [olay işleyicilerine](event-handlers.md)bakın. 

## <a name="prerequisites"></a>Ön koşullar 
Bu öğreticiyi tamamlamak için şunları yapmanız gerekir:

* **Azure aboneliği** - Zaten hesabınız yoksa ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturun. 
* **Azure IoT Hub ve IoT Edge aygıtı** - Linux [veya](../../iot-edge/quickstart-linux.md) [Windows aygıtlarınız](../../iot-edge/quickstart.md) yoksa hızlı başlatma adımlarını izleyin.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-deploy-iot-edge.md)] 
## <a name="create-event-grid-topic-and-subscription-in-cloud"></a>Bulutta olay ızgarası konusu ve abonelik oluşturma

[Bu öğreticiyi](../custom-event-quickstart-portal.md)izleyerek bulutta bir olay ızgarası konusu ve abonelik oluşturun. Not `topicURL`aşağı `sasKey`, `topicName` ve daha sonra öğretici kullanacağınız yeni oluşturulan konu.

Örneğin, Batı ABD'de `testegcloudtopic` adlı bir konu oluşturduysanız, değerler aşağıdaki gibi görünür:

* **KonuUrl**:`https://testegcloudtopic.westus2-1.eventgrid.azure.net/api/events`
* **Konu Adı**:`testegcloudtopic`
* **SasKey**: Konunun **AccessKey** altında kullanılabilir. **Tuşkullanın1**.

## <a name="create-event-grid-topic-at-the-edge"></a>Kenarda olay ızgarası konusu oluşturma

1. Aşağıdaki içerikle topic3.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerimize](api.md) bakın.

    ```json
        {
          "name": "sampleTopic3",
          "properties": {
            "inputschema": "eventGridSchema"
          }
        }
    ```
1. Konuyu oluşturmak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic3.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3?api-version=2019-01-01-preview
    ```
1. Konunun başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3?api-version=2019-01-01-preview
    ```

   Örnek çıktı:

   ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic3",
            "name": "sampleTopic3",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/sampleTopic3/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
   ```
  
## <a name="create-event-grid-subscription-at-the-edge"></a>Kenarda Olay Izgara aboneliği oluşturma

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-edge-persist-event-subscriptions.md)]

1. Aşağıdaki içerikle subscription3.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerimize](api.md) bakın.

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

   >[!NOTE]
   > **EndpointUrl,** Buluttaki Olay Izgara konu URL'sini belirtir. **sasKey** Olay Grid bulut konunun anahtarı anlamına gelir. **TopicName'deki** değer, Tüm Giden Olayları Olay Izgarası'na damgalamak için kullanılır. Bu, Olay Izgara etki alanı konusuna deftere naklederken yararlı olabilir. Olay Izgara etki alanı konusu hakkında daha fazla bilgi için [Olay etki alanları](../event-domains.md)

    Örneğin,
  
    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "eventGrid",
              "properties": {
                "endpointUrl": "https://testegcloudtopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01",
                 "sasKey": "<your-event-grid-topic-saskey>",
                 "topicName": null
              }
            }
          }
        }
    ```

2. Aboneliği oluşturmak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

     ```sh
     curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription3.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3/eventSubscriptions/sampleSubscription3?api-version=2019-01-01-preview
     ```

3. Aboneliğin başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3/eventSubscriptions/sampleSubscription3?api-version=2019-01-01-preview
    ```

    Örnek çıktı:

    ```json
         {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic3/eventSubscriptions/sampleSubscription3",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription3",
          "properties": {
            "Topic": "sampleTopic3",
            "destination": {
              "endpointType": "eventGrid",
              "properties": {
                "endpointUrl": "https://testegcloudtopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01",
                "sasKey": "<your-event-grid-topic-saskey>",
                "topicName": null
              }
            }
          }
        }
    ```

## <a name="publish-an-event-at-the-edge"></a>Bir etkinliği kenarda yayınlama

1. Aşağıdaki içerikle event3.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerine](api.md) bakın.

    ```json
        [
          {
            "id": "eventId-egcloud-0",
            "eventType": "recordInserted",
            "subject": "myapp/vehicles/motorcycles",
            "eventTime": "2019-07-28T21:03:07+00:00",
            "dataVersion": "1.0",
            "data": {
              "make": "Ducati",
              "model": "Monster"
            }
          }
        ]
    ```

1. Şu komutu çalıştırın:

    ```sh
    curl -k -H "Content-Type: application/json" -X POST -g -d @event3.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3/events?api-version=2019-01-01-preview
    ```

## <a name="verify-edge-event-in-cloud"></a>Kenar olayını bulutta doğrulama

Bulut konusu tarafından sunulan olayları görüntüleme hakkında bilgi için [öğreticiye](../custom-event-quickstart-portal.md)bakın.

## <a name="cleanup-resources"></a>Kaynakları temizleme

* Konuyu ve tüm aboneliklerini silmek için aşağıdaki komutu çalıştırın

    ```sh
    curl -k -H "Content-Type: application/json" -X DELETE https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3?api-version=2019-01-01-preview
    ```

* Bulutta oluşturulan konuyu ve abonelikleri de silin (Azure Olay Idamı)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, kenarda bir etkinlik yayınladınız ve Azure bulutundaki Olay Izgara'sına iletildiniz. Artık bulutta Event Grid'e ilerlemeniz için gereken temel adımları bildiğinize göre:

* IoT Edge'de Azure Olay Kılavuzu'nu kullanmayla ilgili sorunları gidermek için [Sorun Giderme kılavuzuna](troubleshoot.md)bakın.
* Bu [öğreticiyi](forward-events-iothub.md) izleyerek olayları IoTHub'a iletin
* Bu öğreticiyi izleyerek olayları bulutta [Webhook'a](pub-sub-events-webhook-cloud.md) iletme
* [Konuları ve abonelikleri kenarda izleme](monitor-topics-subscriptions.md)
