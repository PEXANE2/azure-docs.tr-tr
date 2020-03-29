---
title: Olay Izgara olaylarını IoTHub'a iletme - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: Olay Izgara olaylarını IoTHub'a iletme
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: d0034810ff86de2a40e275ca54a2f0f9cbc856c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844709"
---
# <a name="tutorial-forward-events-to-iothub"></a>Öğretici: Etkinlikleri IoTHub'a iletin

Bu makalede, Olay Izgara olaylarını diğer IoT Edge modüllerine, ioTHub güzergahlarını kullanarak iletmek için gereken tüm adımları inceler. Aşağıdaki nedenlerle yapmak isteyebilirsiniz:

* edgeHub yönlendirmesi ile mevcut yatırımları kullanmaya devam edin
* Tüm olayları yalnızca IoT Hub üzerinden bir cihazdan yönlendirmeyi tercih etmeyi tercih edin

Bu öğreticiyi tamamlamak için aşağıdaki kavramları anlamanız gerekir:

- [Olay Izgara Kavramları](concepts.md)
- [IoT Edge hub](../../iot-edge/module-composition.md) 

## <a name="prerequisites"></a>Ön koşullar 
Bu öğreticiyi tamamlamak için şunları yapmanız gerekir:

* **Azure aboneliği** - Zaten hesabınız yoksa ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturun. 
* **Azure IoT Hub ve IoT Edge aygıtı** - Linux [veya](../../iot-edge/quickstart-linux.md) [Windows aygıtlarınız](../../iot-edge/quickstart.md) yoksa hızlı başlatma adımlarını izleyin.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-deploy-iot-edge.md)]

## <a name="create-topic"></a>Konu oluşturma

Bir olayın yayımcısı olarak, bir olay ızgarası konusu oluşturmanız gerekir. Konu, yayıncıların olayları daha sonra gönderebileceği bir bitiş noktası anlamına gelir.

1. Aşağıdaki içerikle topic4.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerimize](api.md) bakın.

   ```json
    {
          "name": "sampleTopic4",
          "properties": {
            "inputschema": "eventGridSchema"
          }
    }
    ```
1. Konuyu oluşturmak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic4.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4?api-version=2019-01-01-preview
    ```

1. Konunun başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4?api-version=2019-01-01-preview
    ```

   Örnek çıktı:

   ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic4",
            "name": "sampleTopic4",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/sampleTopic4/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
   ```

## <a name="create-event-subscription"></a>Etkinlik aboneliği oluşturma

Aboneler bir konuyla yayınlanan etkinlikler için kayıt yaptırabilirler. Herhangi bir etkinliği almak için, ilgi duyulan bir konuda Bir Olay Ağı aboneliği oluşturmaları gerekir.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-edge-persist-event-subscriptions.md)]

1. Aşağıdaki içerikle subscription4.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerimize](api.md) bakın.

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

   >[!NOTE]
   > Abonenin `endpointType` `edgeHub`. Olay `outputName` Izgara modülünün bu abonelikle eşleşen olayları edgeHub'a yönlendireceği çıktıyı belirtir. Örneğin, yukarıdaki abonelikle eşleşen olaylar `/messages/modules/eventgridmodule/outputs/sampleSub4`.
2. Aboneliği oluşturmak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription4.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4/eventSubscriptions/sampleSubscription4?api-version=2019-01-01-preview
    ```
3. Aboneliğin başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4/eventSubscriptions/sampleSubscription4?api-version=2019-01-01-preview
    ```

    Örnek çıktı:

   ```json
        {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic4/eventSubscriptions/sampleSubscription4",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription4",
          "properties": {
            "Topic": "sampleTopic4",
            "destination": {
                      "endpointType": "edgeHub",
                      "properties": {
                            "outputName": "sampleSub4"
                      }
                }
          }
        }
    ```

## <a name="set-up-an-edge-hub-route"></a>Kenar hub rotası ayarlama

Olay aboneliğinin olaylarını IoTHub'a iletilecek şekilde iletmek için kenar hub'ın rotasını aşağıdaki gibi güncelleştirin:

1. [Azure portalında](https://ms.portal.azure.com) oturum açın
1. **IoT Hub'ına**gidin.
1. Menüden **IoT Edge'i** seçin
1. Aygıtlar listesinden hedef aygıtın kimliğini seçin.
1. **Modülleri Ayarlama**'yı seçin.
1. **İleri** ve rotalar bölümünü seçin.
1. Rotalarda yeni bir rota ekleyin

  ```sh
  "fromEventGridToIoTHub":"FROM /messages/modules/eventgridmodule/outputs/sampleSub4 INTO $upstream"
  ```

  Örneğin,

  ```json
  {
      "routes": {
        "fromEventGridToIoTHub": "FROM /messages/modules/eventgridmodule/outputs/sampleSub4 INTO $upstream"
      }
  }
  ```

   >[!NOTE]
   > Yukarıdaki rota, bu abonelik için eşleşen tüm olayları IoT hub'ına iletilir. Daha fazla filtre lemek ve Olay Izgara olaylarını diğer IoT Edge modüllerine yönlendirmek için [Edge hub yönlendirme](../../iot-edge/module-composition.md) özelliklerini kullanabilirsiniz.

## <a name="setup-iot-hub-route"></a>Kurulum IoT Hub rotası

Olay Izgara modülünden iletilen olayları görüntüleyebilmeniz için IoT hub hub'ından bir rota ayarlamak için [IoT Hub yönlendirme öğreticisine](../../iot-hub/tutorial-routing.md) bakın. Öğreticiyi basit tutmak için sorguyu kullanın. `true`  



## <a name="publish-an-event"></a>Etkinlik yayınlama

1. Aşağıdaki içerikle event4.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerimize](api.md) bakın.

    ```json
        [
          {
            "id": "eventId-iothub-1",
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

1. Olayı yayımlamak için aşağıdaki komutu çalıştırın:

    ```sh
    curl -k -H "Content-Type: application/json" -X POST -g -d @event4.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4/events?api-version=2019-01-01-preview
    ```

## <a name="verify-event-delivery"></a>Olay teslimini doğrulama

Olayları görüntülemek için adımlar için IoT Hub [yönlendirme öğreticisine](../../iot-hub/tutorial-routing.md) bakın.

## <a name="cleanup-resources"></a>Kaynakları temizleme

* Konuyu ve tüm aboneliklerini silmek için aşağıdaki komutu çalıştırın:

    ```sh
    curl -k -H "Content-Type: application/json" -X DELETE https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4?api-version=2019-01-01-preview
    ```
* Bulutta IoTHub yönlendirmesini ayarlarken oluşturulan kaynakları da silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir olay ızgarası konusu, kenar hub aboneliği ve yayımlanmış olaylar oluşturdunuz. Artık bir kenar hub'ına ilerlemek için temel adımları bildiğinize göre, aşağıdaki makalelere bakın:

* IoT Edge'de Azure Olay Kılavuzu'nu kullanmayla ilgili sorunları gidermek için [Sorun Giderme kılavuzuna](troubleshoot.md)bakın.
* Olayları bölmek için [kenar hub](../../iot-edge/module-composition.md) rota filtrelerini kullanma
* [Linux](persist-state-linux.md) veya [Windows'da](persist-state-windows.md) Olay Izgara modülünün kalıcılığını ayarlama
* İstemci kimlik doğrulamasını yapılandırmak için [belgeleri](configure-client-auth.md) izleme
* Bu [öğreticiyi](forward-events-event-grid-cloud.md) izleyerek olayları buluttaki Azure Etkinlik Izgarasına iletin
* [Konuları ve abonelikleri kenarda izleme](monitor-topics-subscriptions.md)