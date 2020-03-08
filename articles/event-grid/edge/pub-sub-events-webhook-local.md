---
title: Yayımlayın, olaylara yerel olarak abone olun-Azure Event Grid IoT Edge | Microsoft Docs
description: IoT Edge Event Grid ile Web kancasını kullanarak olaylara yerel olarak abone olma
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: ba82b1bea4753cd51e275a78b248247032d79a01
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78387210"
---
# <a name="tutorial-publish-subscribe-to-events-locally"></a>Öğretici: yayımlama, olaylara yerel olarak abone olma

Bu makalede, IoT Edge Event Grid kullanarak olayları yayımlamak ve bunlara abone olmak için gereken tüm adımlarda adım adım açıklanmaktadır.

> [!NOTE]
> Azure Event Grid konuları ve abonelikleri hakkında bilgi edinmek için bkz. [Event Grid kavramlar](concepts.md).

## <a name="prerequisites"></a>Önkoşullar 
Bu öğreticiyi tamamlayabilmeniz için şunlar gerekir:

* **Azure aboneliği** -henüz bir [hesabınız yoksa ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun. 
* **Azure IoT Hub ve IoT Edge cihazı** -henüz yoksa [Linux](../../iot-edge/quickstart-linux.md) veya [Windows cihazları](../../iot-edge/quickstart.md) için Hızlı Başlangıç bölümündeki adımları izleyin.

## <a name="deploy-event-grid-iot-edge-module"></a>Event Grid IoT Edge modülünü dağıt

IoT Edge bir cihaza modül dağıtmanın birkaç yolu vardır ve bunların hepsi IoT Edge Azure Event Grid için çalışır. Bu makalede Azure portal IoT Edge Event Grid dağıtma adımları açıklanmaktadır.

>[!NOTE]
> Bu öğreticide, Event Grid modülünü kalıcı olmadan dağıtacaksınız. Bu öğreticide oluşturduğunuz tüm konuların ve aboneliklerin, modülü yeniden dağıtırken silineceği anlamına gelir. Kalıcılığı ayarlama hakkında daha fazla bilgi için şu makalelere bakın: [Linux 'ta durumu kalıcı hale](persist-state-linux.md) getirin veya [Windows 'da durumu devam](persist-state-windows.md)ettir. Üretim iş yükleri için Event Grid modülünü Kalıcılık ile yüklemenizi öneririz.


### <a name="select-your-iot-edge-device"></a>IoT Edge cihazınızı seçin

1. [Azure portalda](https://portal.azure.com) oturum açma
1. IoT Hub gidin.
1. **Otomatik cihaz yönetimi** bölümündeki menüden **IoT Edge** ' yi seçin. 
1. Cihaz listesinden hedef cihazın KIMLIĞINE tıklayın
1. **Modülleri Ayarlama**'yı seçin. Sayfayı açık tutun. Sonraki bölümde bulunan adımlarla devam edersiniz.

### <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Azure portal, JSON belgesini el ile oluşturmak yerine bir dağıtım bildirimi oluşturma konusunda size yol gösteren bir sihirbaza sahiptir.  Üç adım vardır: **modüller ekleme**, **rotalar belirtme**ve **dağıtımı İnceleme**.

### <a name="add-modules"></a>Modül Ekle

1. **Dağıtım modülleri** bölümünde **Ekle** ' yi seçin.
1. Açılan listedeki modül türlerinden **IoT Edge modül** ' ı seçin.
1. Kapsayıcının adını, görüntüsünü, kapsayıcı oluşturma seçeneklerini belirtin:

   * **Ad**: eventgridmodule
   * **Görüntü URI 'si**: `mcr.microsoft.com/azure-event-grid/iotedge:latest`
   * **Kapsayıcı oluşturma seçenekleri**:

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
 1. **Kaydet**’e tıklayın
 1. Azure Event Grid abone modülünü birlikte dağıtmadan önce eklemek için sonraki bölüme geçin.

    >[!IMPORTANT]
    > Bu öğreticide, istemci kimlik doğrulaması devre dışı olarak Event Grid modülünü dağıtacaksınız. Üretim iş yükleri için, istemci kimlik doğrulamasını etkinleştirmenizi öneririz. Event Grid modülünü güvenli şekilde yapılandırma hakkında daha fazla bilgi için bkz. [güvenlik ve kimlik doğrulaması](security-authentication.md).
    > 
    > Bir Azure VM 'yi uç cihaz olarak kullanıyorsanız, 4438 numaralı bağlantı noktasında gelen trafiğe izin vermek için bir gelen bağlantı noktası kuralı ekleyin. Kuralı ekleme hakkında yönergeler için bkz. [BIR VM 'ye bağlantı noktalarını açma](../../virtual-machines/windows/nsg-quickstart-portal.md).
    

## <a name="deploy-event-grid-subscriber-iot-edge-module"></a>Event Grid abonesi IoT Edge modülünü dağıtma

Bu bölümde, olayların sunulabileceği bir olay işleyicisi olarak görev yapacak başka bir IoT modülünün nasıl dağıtılacağı gösterilmektedir.

### <a name="add-modules"></a>Modül Ekle

1. **Dağıtım modülleri** bölümünde yeniden **Ekle** ' yi seçin. 
1. Açılan listedeki modül türlerinden **IoT Edge modül** ' ı seçin.
1. Kapsayıcının adını, görüntüsünü ve kapsayıcı oluşturma seçeneklerini belirtin:

   * **Ad**: abone
   * **Görüntü URI 'si**: `mcr.microsoft.com/azure-event-grid/iotedge-samplesubscriber:latest`
   * **Kapsayıcı oluşturma seçenekleri**: yok
1. **Kaydet**’e tıklayın
1. Yönlendirmeler bölümüne devam etmek için **İleri** 'ye tıklayın

 ### <a name="setup-routes"></a>Kurulum yolları

Varsayılan yolları koruyun ve gözden geçirme bölümüne devam etmek için **İleri** ' yi seçin.

### <a name="submit-the-deployment-request"></a>Dağıtım isteğini gönder

1. İnceleme Bölümü, önceki bölümde yaptığınız seçimlere göre oluşturulan JSON dağıtım bildirimini gösterir. Her iki modülü de gördüistediğinizi onaylayın: **eventgridmodule** ve **abone** JSON içinde listelenmiştir. 
1. Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**' i seçin. Dağıtımı gönderdikten sonra **cihaz** sayfasına dönersiniz.
1. **Modüller bölümünde**, hem **eventgrid** hem de **abone** modüllerinin listelendiğini doğrulayın. Ve, **dağıtım sırasında belirtilen** ve **cihaz sütunlarının rapor** sütunları **Evet**olarak ayarlandığını doğrulayın.

    Modülün cihazda başlatılması ve sonra IoT Hub geri bildirilmesi birkaç dakika sürebilir. Güncelleştirilmiş durumu görmek için sayfayı yenileyin.

## <a name="create-a-topic"></a>Konu başlığı oluşturma

Bir olayın yayımcısı olarak bir olay Kılavuzu konusu oluşturmanız gerekir. Azure Event Grid, bir konu, yayımcıların olay gönderebileceği bir uç noktaya başvurur.

1. Aşağıdaki içerikle topic. JSON oluşturun. Yük hakkında daha fazla bilgi için bkz. [API](api.md)Belgelerimiz.

    ```json
        {
          "name": "sampleTopic1",
          "properties": {
            "inputschema": "eventGridSchema"
          }
        }
    ```

1. Bir olay Kılavuzu konusu oluşturmak için aşağıdaki komutu çalıştırın. HTTP durum kodu `200 OK`görtığınızdan emin olun.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1?api-version=2019-01-01-preview
    ```

1. Konunun başarıyla oluşturulduğunu doğrulamak için şu komutu çalıştırın. 200 Tamam HTTP durum kodu döndürülmelidir.

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

Aboneler, bir konuya yayımlanan olaylara kaydolabilirler. Herhangi bir olay almak için, ilgilendiğiniz konu başlığı için bir Event Grid aboneliği oluşturmanız gerekir.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-edge-persist-event-subscriptions.md)]

1. Aşağıdaki içerikle Subscription. JSON oluşturun. Yük hakkında daha fazla bilgi için [API belgelerimize](api.md) bakın

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
    > **EndpointType** özelliği, abonenin bir **Web kancası**olduğunu belirtir.  **EndpointUrl** , abonenin olayları dinlediği URL 'yi belirtir. Bu URL, daha önce dağıttığınız Azure abonesi örneğine karşılık gelir.
2. Konusu için bir abonelik oluşturmak üzere aşağıdaki komutu çalıştırın. HTTP durum kodu `200 OK`görtığınızdan emin olun.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1/eventSubscriptions/sampleSubscription1?api-version=2019-01-01-preview
    ```
3. Aboneliğin başarıyla oluşturulduğunu doğrulamak için şu komutu çalıştırın. 200 Tamam HTTP durum kodu döndürülmelidir.

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

## <a name="publish-an-event"></a>Olay yayımlama

1. Aşağıdaki içerikle Event. JSON oluşturun. Yük hakkında daha fazla bilgi için bkz. [API](api.md)Belgelerimiz.

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

1. IoT Edge sanal makinenizde SSH veya RDP.
1. Abone günlüklerini denetleyin:

    Windows 'ta aşağıdaki komutu çalıştırın:

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine container logs subscriber
    ```

   Linux 'ta aşağıdaki komutu çalıştırın:

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
Bu öğreticide, bir olay Kılavuzu konusu, abonelik ve yayımlanan olaylar oluşturdunuz. Artık temel adımları bildiğinize göre aşağıdaki makalelere bakın: 

- IoT Edge Azure Event Grid kullanmayla ilgili sorunları gidermek için bkz. [sorun giderme kılavuzu](troubleshoot.md).
- [Filtrelerle](advanced-filtering.md)abonelik oluşturun/güncelleştirin.
- [Linux](persist-state-linux.md) veya [Windows](persist-state-windows.md) üzerinde Event Grid modülünün kalıcılığını etkinleştirme
- İstemci kimlik doğrulamasını yapılandırmak için [belgeleri](configure-client-auth.md) izleyin
- Bu [öğreticiyi](pub-sub-events-webhook-cloud.md) izleyerek etkinlikleri Bulutta Azure işlevlerine iletin
- [IoT Edge BLOB depolama olaylarına tepki verme](react-blob-storage-events-locally.md)
- [Kenarda konuları ve abonelikleri izleyin](monitor-topics-subscriptions.md)

