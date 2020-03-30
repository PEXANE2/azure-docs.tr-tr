---
title: Yerel olarak etkinliklere abone olun - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: IoT Edge'de Event Grid ile Webhook'u kullanarak etkinlikleri yerel olarak yayımlayın, bunlara abone olun
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: ba82b1bea4753cd51e275a78b248247032d79a01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281008"
---
# <a name="tutorial-publish-subscribe-to-events-locally"></a>Öğretici: Yayımla, etkinliklere yerel abone olun

Bu makale, IoT Edge'deki Olay Izgarasını kullanarak etkinlikleri yayımlamak ve bunlara abone olmak için gereken tüm adımları size iletebilirsiniz.

> [!NOTE]
> Azure Olay Izgara konuları ve abonelikleri hakkında bilgi edinmek için [Olay Kılavuz Kavramları'na](concepts.md)bakın.

## <a name="prerequisites"></a>Ön koşullar 
Bu öğreticiyi tamamlamak için şunları yapmanız gerekir:

* **Azure aboneliği** - Zaten hesabınız yoksa ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturun. 
* **Azure IoT Hub ve IoT Edge aygıtı** - Zaten sahip değilseniz [Linux](../../iot-edge/quickstart-linux.md) veya [Windows aygıtları](../../iot-edge/quickstart.md) için hızlı başlatma adımlarını izleyin.

## <a name="deploy-event-grid-iot-edge-module"></a>Olay Izgara IoT Edge modüllerini dağıtın

Modülleri bir IoT Edge aygıtına dağıtmanın çeşitli yolları vardır ve bunların tümü IoT Edge'deki Azure Olay Izgarası için çalışır. Bu makalede, Azure portalından IoT Edge'de Olay Izgarasını dağıtma adımları açıklanmaktadır.

>[!NOTE]
> Bu eğitimde, Etkinlik Izgara modüllerini kalıcılık olmadan dağıtmış olursunuz. Bu, modülü yeniden dağıttığınızda bu öğreticide oluşturduğunuz tüm konuların ve aboneliklerin silineceği anlamına gelir. Kalıcılığı nasıl düzenesin hakkında daha fazla bilgi için aşağıdaki makalelere bakın: [Linux'ta kalıcı durum](persist-state-linux.md) veya [Windows'da kalıcı durum.](persist-state-windows.md) Üretim iş yükleri için Olay Izgara modüllerini kalıcı olarak yüklemenizi öneririz.


### <a name="select-your-iot-edge-device"></a>IoT Edge cihazınızı seçin

1. [Azure portalında](https://portal.azure.com) oturum açın
1. IoT Hub'ınıza gidin.
1. **Otomatik Aygıt Yönetimi** bölümündeki menüden **IoT Edge'i** seçin. 
1. Aygıtlar listesinden hedef aygıtın kimliğine tıklayın
1. **Modülleri Ayarlama**'yı seçin. Sayfayı açık tutun. Sonraki bölümdeki adımlarla devam eceksiniz.

### <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılabildiğini, modüller arasında verilerin nasıl aktığını ve modülün istenilen özelliklerini açıklayan bir JSON belgesidir. Azure portalında, JSON belgesini el ile oluşturmak yerine dağıtım bildirimi oluşturmada size yol gösteren bir sihirbaz vardır.  Üç adımı vardır: **Modülekleme,** **Rotalar belirt**ve **dağıtımı gözden geçirin.**

### <a name="add-modules"></a>Modül ekle

1. Dağıtım **Modülleri** bölümünde Ekle'yi **seçin**
1. Açılan listedeki modül türlerine göre **IoT Edge Modülünü** seçin
1. Kapsayıcının adını, görüntüsünü, kapsayıcıyı oluşturma seçeneklerini sağlayın:

   * **Adı**: eventgridmodule
   * **Resim URI**:`mcr.microsoft.com/azure-event-grid/iotedge:latest`
   * **Konteyner Oluşturma Seçenekleri**:

   [!INCLUDE [event-grid-edge-module-version-update](../../../includes/event-grid-edge-module-version-update.md)]

    ```json
        {
          "Env": [
            "inbound__clientAuth__clientCert__enabled=false"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                    "HostPort": "4438"
                }
              ]
            }
          }
        }
    ```    
 1. **Kaydet'i** tıklatın
 1. Azure Olay Izgara Abone modüllerini birlikte dağıtmadan önce eklemek için bir sonraki bölüme devam edin.

    >[!IMPORTANT]
    > Bu eğitimde, istemci kimlik doğrulaması devre dışı bırakılmış Olay Izgara modüllerini dağıtmış olursunuz. Üretim iş yükleri için istemci kimlik doğrulamasını etkinleştirmenizi öneririz. Olay Izgara modüllerini güvenli bir şekilde yapılandırma hakkında daha fazla bilgi için [Güvenlik ve kimlik doğrulama](security-authentication.md)sına bakın.
    > 
    > Kenar aygıtı olarak bir Azure VM kullanıyorsanız, bağlantı noktası 4438'de gelen trafiğine izin vermek için gelen bağlantı noktası kuralını ekleyin. Kuralı ekleme yönergeleri için, [bağlantı noktalarını VM'ye nasıl açacağına](../../virtual-machines/windows/nsg-quickstart-portal.md)bakın.
    

## <a name="deploy-event-grid-subscriber-iot-edge-module"></a>Olay Izgara Abone IoT Edge modüldağıtma

Bu bölümde, olayların teslim edilebildiği bir olay işleyicisi olarak hareket edecek başka bir IoT modülü nasıl dağıtılacanız gösterilmektedir.

### <a name="add-modules"></a>Modül ekle

1. Dağıtım **Modülleri** bölümünde yeniden **ekle'yi** seçin. 
1. Açılan listedeki modül türlerine göre **IoT Edge Modülünü** seçin
1. Kapsayıcının ad, görüntü ve kapsayıcı oluşturma seçeneklerini sağlayın:

   * **Ad**: abone
   * **Resim URI**:`mcr.microsoft.com/azure-event-grid/iotedge-samplesubscriber:latest`
   * **Kapsayıcı Oluşturma Seçenekleri**: Yok
1. **Kaydet'i** tıklatın
1. Rotalar bölümüne devam etmek için **İleri'yi** tıklatın

 ### <a name="setup-routes"></a>Kurulum rotaları

Varsayılan yolları tutun ve inceleme bölümüne devam etmek için **Sonraki'ni** seçin

### <a name="submit-the-deployment-request"></a>Dağıtım isteğini gönderme

1. İnceleme bölümü, önceki bölümdeki seçimlerinize göre oluşturulan JSON dağıtım bildirimini gösterir. Her iki modülü de gördüğünüzü onaylayın: **eventgridmodule** ve JSON'da listelenen **abone.** 
1. Dağıtım bilgilerinizi gözden geçirin ve **ardından Gönder'i**seçin. Dağıtımı gönderdikten sonra **aygıt** sayfasına geri dönersiniz.
1. **Modüller bölümünde,** hem **eventgrid** hem de **abone** modüllerinin listeli olduğunu doğrulayın. Ayrıca, **dağıtımda Belirtilen** ve Aygıt Sütunları **Tarafından Bildirilen'in** **Evet**olarak ayarlı olduğunu doğrulayın.

    Modülün cihazda başlatılması ve ardından IoT Hub'a rapor edilmesi birkaç dakika sürebilir. Güncelleştirilmiş bir durumu görmek için sayfayı yenileyin.

## <a name="create-a-topic"></a>Konu başlığı oluşturma

Bir olayın yayımcısı olarak, bir olay ızgarası konusu oluşturmanız gerekir. Azure Olay Idamı'nda bir konu, yayıncıların olayları gönderebileceği bir bitiş noktası anlamına gelir.

1. Aşağıdaki içerikle topic.json oluşturun. Yük hakkında ayrıntılı bilgi için [API belgelerimize](api.md)bakın.

    ```json
        {
          "name": "sampleTopic1",
          "properties": {
            "inputschema": "eventGridSchema"
          }
        }
    ```

1. Olay ızgarası konusu oluşturmak için aşağıdaki komutu çalıştırın. HTTP durum kodunun . `200 OK`

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1?api-version=2019-01-01-preview
    ```

1. Konunun başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1?api-version=2019-01-01-preview
    ```

   Örnek çıktı:

   ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic1",
            "name": "sampleTopic1",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/sampleTopic1/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
   ```

## <a name="create-an-event-subscription"></a>Olay aboneliği oluşturma

Aboneler bir konuyla yayınlanan etkinlikler için kayıt yaptırabilirler. Herhangi bir etkinliği almak için, ilgi çeken bir konu için bir Olay Izgara aboneliği oluşturmanız gerekir.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-edge-persist-event-subscriptions.md)]

1. Aşağıdaki içerikle subscription.json oluşturun. Yük hakkında ayrıntılı bilgi için [API belgelerimize](api.md) bakın

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "https://subscriber:4430"
              }
            }
          }
        }
    ```

    >[!NOTE]
    > **EndpointType** özelliği, abonenin bir **Webhook**olduğunu belirtir.  **EndpointUrl,** abonenin olayları dinlediği URL'yi belirtir. Bu URL, daha önce dağıtmış olduğunuz Azure Abone örneğine karşılık gelir.
2. Konu için abonelik oluşturmak için aşağıdaki komutu çalıştırın. HTTP durum kodunun . `200 OK`

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1/eventSubscriptions/sampleSubscription1?api-version=2019-01-01-preview
    ```
3. Aboneliğin başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1/eventSubscriptions/sampleSubscription1?api-version=2019-01-01-preview
    ```

    Örnek çıktı:

   ```json
        {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic1/eventSubscriptions/sampleSubscription1",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription1",
          "properties": {
            "Topic": "sampleTopic1",
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "https://subscriber:4430"
              }
            }
          }
        }
    ```

## <a name="publish-an-event"></a>Etkinlik yayınlama

1. Aşağıdaki içerikle event.json oluşturun. Yük hakkında ayrıntılı bilgi için [API belgelerimize](api.md)bakın.

    ```json
        [
          {
            "id": "eventId-func-0",
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
1. Bir olayı yayımlamak için aşağıdaki komutu çalıştırın.

    ```sh
    curl -k -H "Content-Type: application/json" -X POST -g -d @event.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1/events?api-version=2019-01-01-preview
    ```

## <a name="verify-event-delivery"></a>Olay teslimini doğrulama

1. IoT Edge VM'nize SSH veya RDP.
1. Abone günlüklerini kontrol edin:

    Windows'da aşağıdaki komutu çalıştırın:

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine container logs subscriber
    ```

   Linux'ta aşağıdaki komutu çalıştırın:

    ```sh
    sudo docker logs subscriber
    ```

    Örnek çıktı:

    ```sh
        Received Event:
            {
              "id": "eventId-func-0",
              "topic": "sampleTopic1",
              "subject": "myapp/vehicles/motorcycles",
              "eventType": "recordInserted",
              "eventTime": "2019-07-28T21:03:07+00:00",
              "dataVersion": "1.0",
              "metadataVersion": "1",
              "data": {
                "make": "Ducati",
                "model": "Monster"
              }
            }
    ```

## <a name="cleanup-resources"></a>Kaynakları temizleme

* Konuyu ve tüm aboneliklerini silmek için aşağıdaki komutu çalıştırın.

    ```sh
    curl -k -H "Content-Type: application/json" -X DELETE https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1?api-version=2019-01-01-preview
    ```
* IoT Edge cihazınızdan abone modülünü silin.


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir olay ızgarası konusu, abonelik ve yayımlanmış olaylar oluşturdunuz. Artık temel adımları bildiğinize göre, aşağıdaki makalelere bakın: 

- IoT Edge'de Azure Olay Kılavuzu'nu kullanmayla ilgili sorunları gidermek için [Sorun Giderme kılavuzuna](troubleshoot.md)bakın.
- [Filtrelerle](advanced-filtering.md)abonelik oluşturma/güncelleme.
- [Linux](persist-state-linux.md) veya Windows'da Olay Izgara modülünün kalıcılığını [etkinleştirme](persist-state-windows.md)
- İstemci kimlik doğrulamasını yapılandırmak için [belgeleri](configure-client-auth.md) izleme
- Bu öğreticiyi izleyerek olayları buluttaki Azure [Işlevlerine](pub-sub-events-webhook-cloud.md) iletme
- [IoT Edge'deki Blob Depolama olaylarına tepki ver](react-blob-storage-events-locally.md)
- [Konuları ve abonelikleri kenarda izleme](monitor-topics-subscriptions.md)

