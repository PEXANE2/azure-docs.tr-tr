---
title: Buluttaki etkinlikleri yayımla, abone ol - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: IoT Edge'de Event Grid ile Webhook'u kullanarak buluttaki etkinlikleri yayımlayın, abone olun
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: c82f1edfc3acd73c1d38425f963aaaf2976a1cc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844607"
---
# <a name="tutorial-publish-subscribe-to-events-in-cloud"></a>Öğretici: Yayımla, buluttaki etkinliklere abone ol

Bu makale, IoT Edge'deki Olay Izgarasını kullanarak etkinlikleri yayımlamak ve bunlara abone olmak için gereken tüm adımları gözden geçirir. Bu öğretici, Olay İşleyicisi olarak Azure İşi'ni kullanır. Ek hedef türleri için [olay işleyicilerine](event-handlers.md)bakın.

Devam etmeden önce olay ızgarası konusunun ve aboneliğinin ne olduğunu anlamak için [Olay Izgara Kavramları'na](concepts.md) bakın.

## <a name="prerequisites"></a>Ön koşullar 
Bu öğreticiyi tamamlamak için şunları yapmanız gerekir:

* **Azure aboneliği** - Zaten hesabınız yoksa ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturun. 
* **Azure IoT Hub ve IoT Edge aygıtı** - Linux [veya](../../iot-edge/quickstart-linux.md) [Windows aygıtlarınız](../../iot-edge/quickstart.md) yoksa hızlı başlatma adımlarını izleyin.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-deploy-iot-edge.md)]

## <a name="create-an-azure-function-in-the-azure-portal"></a>Azure portalında Azure işlevi oluşturma

Bir Azure işlevi oluşturmak için [öğreticide](../../azure-functions/functions-create-first-azure-function.md) belirtilen adımları izleyin. 

Kod parçacıkını aşağıdaki kodla değiştirin:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);

    log.LogInformation($"C# HTTP trigger received {data}.");
    return data != null
        ? (ActionResult)new OkResult()
        : new BadRequestObjectResult("Please pass in the request body");
}
```

Yeni işlevinizde sağ üstteki **İşlev URL'sini al'ı** seçin, varsayılan **(İşlev tuşu)** seçeneğini seçin ve ardından **Kopyala'yı**seçin. Daha sonra öğreticide işlev URL değerini kullanırsınız.

> [!NOTE]
> Bir EventGrid olayının tetikleyicilerini kullanarak olaylara tepki verme konusunda daha fazla örnek ve öğretici için [Azure İşlevler](../../azure-functions/functions-overview.md) belgelerine bakın.

## <a name="create-a-topic"></a>Konu başlığı oluşturma

Bir olayın yayımcısı olarak, bir olay ızgarası konusu oluşturmanız gerekir. Konu, yayıncıların olayları gönderebileceği bir bitiş noktası anlamına gelir.

1. Aşağıdaki içerikle topic2.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerimize](api.md) bakın.

    ```json
         {
          "name": "sampleTopic2",
          "properties": {
            "inputschema": "eventGridSchema"
          }
        }
    ```
1. Konuyu oluşturmak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic2.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2?api-version=2019-01-01-preview
    ```
1. Konunun başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2?api-version=2019-01-01-preview
    ```

   Örnek çıktı:

   ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic2",
            "name": "sampleTopic2",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/sampleTopic2/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
   ```

## <a name="create-an-event-subscription"></a>Olay aboneliği oluşturma

Aboneler bir konuyla yayınlanan etkinlikler için kayıt yaptırabilirler. Herhangi bir olay almak için, abonelerin ilgi duyulan bir konuda bir Olay ızgarası aboneliği oluşturmaları gerekir.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-edge-persist-event-subscriptions.md)]

1. Aşağıdaki içerikle subscription2.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerimize](api.md) bakın.

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-az-func-cloud-url>"
              }
            }
          }
        }
    ```

   >[!NOTE]
   > **Uç Nokta Türü,** abonenin bir Webhook olduğunu belirtir.  **EndpointUrl,** abonenin olayları dinlediği URL'yi belirtir. Bu URL, daha önce kurduğunuz Azure İşlevi örneğine karşılık gelir.
2. Aboneliği oluşturmak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription2.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2/eventSubscriptions/sampleSubscription2?api-version=2019-01-01-preview
    ```
3. Aboneliğin başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2/eventSubscriptions/sampleSubscription2?api-version=2019-01-01-preview
    ```

    Örnek çıktı:

   ```json
        {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic2/eventSubscriptions/sampleSubscription2",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription2",
          "properties": {
            "Topic": "sampleTopic2",
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-az-func-cloud-url>"
              }
            }
          }
        }
    ```

## <a name="publish-an-event"></a>Etkinlik yayınlama

1. Aşağıdaki içerikle event2.json oluşturun. Yük le ilgili ayrıntılar için [API belgelerimize](api.md) bakın.

    ```json
        [
          {
            "id": "eventId-func-1",
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
1. Olayı yayımlamak için aşağıdaki komutu çalıştırın

    ```sh
    curl -k -H "Content-Type: application/json" -X POST -g -d @event2.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2/events?api-version=2019-01-01-preview
    ```

## <a name="verify-event-delivery"></a>Olay teslimini doğrulama

Azure portalında teslim edilen etkinliği, işlevinizin **Monitör** seçeneği altında görüntüleyebilirsiniz.

## <a name="cleanup-resources"></a>Kaynakları temizleme

* Konuyu ve tüm aboneliklerini silmek için aşağıdaki komutu çalıştırın

    ```sh
    curl -k -H "Content-Type: application/json" -X DELETE https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2?api-version=2019-01-01-preview
    ```

* Azure portalında oluşturulan Azure işlevini silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir olay ızgarası konusu, abonelik ve yayımlanmış olaylar oluşturdunuz. Artık temel adımları bildiğinize göre, aşağıdaki makalelere bakın:

* IoT Edge'de Azure Olay Kılavuzu'nu kullanmayla ilgili sorunları gidermek için [Sorun Giderme kılavuzuna](troubleshoot.md)bakın.
* [Filtrelerle](advanced-filtering.md)abonelik oluşturma/güncelleme.
* [Linux](persist-state-linux.md) veya [Windows'da](persist-state-windows.md) Olay Izgara modülünün kalıcılığını ayarlama
* İstemci kimlik doğrulamasını yapılandırmak için [belgeleri](configure-client-auth.md) izleme
* Bu [öğreticiyi](forward-events-event-grid-cloud.md) izleyerek olayları buluttaki Azure Etkinlik Izgarasına iletin
* [Konuları ve abonelikleri kenarda izleme](monitor-topics-subscriptions.md)
