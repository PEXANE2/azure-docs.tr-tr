---
title: Azure IoT Hub aygıt ikizleri (Python) | Microsoft Dokümanlar
description: Etiketler eklemek ve ardından bir IoT Hub sorgusu kullanmak için Azure IoT Hub aygıt ikizleri nasıl kullanılır? Python için Azure IoT SDK'larını kullanarak simüle edilmiş aygıt uygulamasını ve etiketleri ekleyen ve IoT Hub sorgusunu çalıştıran bir hizmet uygulamasını uygularsınız.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: robinsh
ms.openlocfilehash: c1db7f1a891646ad29f6cae95ddb7e2cf3a42bfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409767"
---
# <a name="get-started-with-device-twins-python"></a>Aygıt ikizleri (Python) ile başlayın

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu eğitimin sonunda, iki Python konsol uygulaması olacak:

* **AddTagsAndQuery.py**, etiketleri ve sorguları cihaz ikizler ekler bir Python arka uç uygulaması.

* **ReportConnectivity.py**, IoT hub'ınıza daha önce oluşturulan aygıt kimliğiyle bağlanan bir aygıtı taklit eden ve bağlantı durumunu bildiren bir Python uygulamasıdır.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-v2-installation-notes](../../includes/iot-hub-include-python-v2-installation-notes.md)]

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Hizmet uygulamasını oluşturma

Bu bölümde, **{Aygıt Kimliği}** ile ilişkili aygıt ikizine konum meta verileri ekleyen bir Python konsol uygulaması oluşturursunuz. Daha sonra, Redmond'da bulunan aygıtları seçerek IoT hub'ında depolanan aygıt ikizleri ve hücresel bağlantı bildirencihazları sorgular.

1. Çalışma dizininizde bir komut istemi açın ve **Python için Azure IoT Hub Hizmeti SDK'sını**yükleyin.

   ```cmd/sh
   pip install azure-iot-hub
   ```

2. Metin düzenleyicisi kullanarak, yeni bir **AddTagsAndQuery.py** dosyası oluşturun.

3. Hizmet SDK’sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin:

   ```python
   import sys
   from time import sleep
   from azure.iot.hub import IoTHubRegistryManager
   from azure.iot.hub.models import Twin, TwinProperties, QuerySpecification, QueryResult
   ```

4. Aşağıdaki kodu ekleyin. Kopyala'da kopyaladığınız `[IoTHub Connection String]` [IoT hub bağlantı dizesini değiştirin.](#get-the-iot-hub-connection-string) IoT hub'ında yeni bir aygıt kaydedin'de kaydettiğiniz aygıt `[Device Id]` [kimliğiyle değiştirin.](#register-a-new-device-in-the-iot-hub)
  
    ```python
    IOTHUB_CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"
    ```

5. **AddTagsAndQuery.py** dosyasına aşağıdaki kodu ekleyin:

    ```python
    def iothub_service_sample_run():
        try:
            iothub_registry_manager = IoTHubRegistryManager(IOTHUB_CONNECTION_STRING)

            new_tags = {
                    'location' : {
                        'region' : 'US',
                        'plant' : 'Redmond43'
                    }
                }

            twin = iothub_registry_manager.get_twin(DEVICE_ID)
            twin_patch = Twin(tags=new_tags, properties= TwinProperties(desired={'power_level' : 1}))
            twin = iothub_registry_manager.update_twin(DEVICE_ID, twin_patch, twin.etag)

            # Add a delay to account for any latency before executing the query
            sleep(1)

            query_spec = QuerySpecification(query="SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'")
            query_result = iothub_registry_manager.query_iot_hub(query_spec, None, 100)
            print("Devices in Redmond43 plant: {}".format(', '.join([twin.device_id for twin in query_result.items])))

            print()

            query_spec = QuerySpecification(query="SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity = 'cellular'")
            query_result = iothub_registry_manager.query_iot_hub(query_spec, None, 100)
            print("Devices in Redmond43 plant using cellular network: {}".format(', '.join([twin.device_id for twin in query_result.items])))

        except Exception as ex:
            print("Unexpected error {0}".format(ex))
            return
        except KeyboardInterrupt:
            print("IoT Hub Device Twin service sample stopped")
    ```

    **IoTHubRegistryManager** nesnesi, hizmetten aygıt ikizleri ile etkileşim kurmak için gereken tüm yöntemleri ortaya çıkarır. Kod önce **IoTHubRegistryManager** nesnesini başolarak çözer, sonra aygıt ikizini **DEVICE_ID**için güncelleştirir ve son olarak iki sorgu çalıştırır. **İlki, Redmond43** tesisinde bulunan aygıtların yalnızca aygıt ikizlerini seçer ve ikincisi sorguyu yalnızca hücresel ağ üzerinden bağlanan aygıtları seçmek için geliştirir.

6. **iothub_service_sample_run** işlevini uygulamak için **AddTagsAndQuery.py** sonunda aşağıdaki kodu ekleyin:

    ```python
    if __name__ == '__main__':
        print("Starting the Python IoT Hub Device Twin service sample...")
        print()

        iothub_service_sample_run()
    ```

7. Uygulamayı aşağıdakilerle çalıştırın:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    **Redmond43'te** bulunan tüm aygıtları soran sorgunun sonuçlarında bir aygıt görmeniz ve sonuçları hücresel ağ kullanan aygıtlarla sınırlayan sorgu için hiçbir aygıt görmeniz gerekir.

    ![Redmond'daki tüm cihazları gösteren ilk sorgu](./media/iot-hub-python-twin-getstarted/service-1.png)

Sonraki bölümde, bağlantı bilgilerini bildiren ve önceki bölümdeki sorgunun sonucunu değiştiren bir aygıt uygulaması oluşturursunuz.

## <a name="create-the-device-app"></a>Cihaz uygulamasını oluşturma

Bu bölümde, hub'ınıza **{Aygıt Kimliği}** olarak bağlanan bir Python konsol uygulaması oluşturur sunuz ve ardından aygıt ikizinin bildirilen özelliklerini hücresel ağ kullanarak bağlı olduğu bilgileri içerecek şekilde güncelleştirirsiniz.

1. Çalışma dizininizdeki bir komut isteminden **Python için Azure IoT Hub Aygıt SDK'sını**yükleyin:

    ```cmd/sh
    pip install azure-iot-device
    ```

2. Metin düzenleyicisi kullanarak, yeni bir **ReportConnectivity.py** dosyası oluşturun.

3. SDK cihazından gerekli modülleri almak için aşağıdaki kodu ekleyin:

    ```python
    import time
    import threading
    from azure.iot.device import IoTHubModuleClient
    ```

4. Aşağıdaki kodu ekleyin. Yer `[IoTHub Device Connection String]` tutucu [değerini, IoT hub'ında yeni bir aygıtı kaydedin'de](#register-a-new-device-in-the-iot-hub)kopyaladığınız aygıt bağlantısı dizesiyle değiştirin.

    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    ```

5. Aygıt ikizişlevselliğini uygulamak için **ReportConnectivity.py** dosyasına aşağıdaki kodu ekleyin:

    ```python
    def twin_update_listener(client):
        while True:
            patch = client.receive_twin_desired_properties_patch()  # blocking call
            print("Twin patch received:")
            print(patch)

    def iothub_client_init():
        client = IoTHubModuleClient.create_from_connection_string(CONNECTION_STRING)
        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            twin_update_listener_thread = threading.Thread(target=twin_update_listener, args=(client,))
            twin_update_listener_thread.daemon = True
            twin_update_listener_thread.start()

            # Send reported 
            print ( "Sending data as reported property..." )
            reported_patch = {"connectivity": "cellular"}
            client.patch_twin_reported_properties(reported_patch)
            print ( "Reported properties updated" )

            while True:
                time.sleep(1000000)
        except KeyboardInterrupt:
            print ( "IoT Hub Device Twin device sample stopped" )
    ```

    **IoTHubModuleClient** nesnesi, aygıttaki aygıt ikizleri ile etkileşim de ihtiyacınız olan tüm yöntemleri ortaya çıkarır. Önceki kod, **IoTHubModuleClient** nesnesini aldıktan sonra aygıtınız için ikiz ilerler ve bildirilen özelliğini bağlantı bilgileriyle güncelleştirir.

6. **iothub_client_sample_run** işlevini uygulamak için **ReportConnectivity.py** sonunda aşağıdaki kodu ekleyin:

    ```python
    if __name__ == '__main__':
        print ( "Starting the Python IoT Hub Device Twin device sample..." )
        print ( "IoTHubModuleClient waiting for commands, press Ctrl-C to exit" )

        iothub_client_sample_run()
    ```

7. Cihaz uygulamasını çalıştırın:

    ```cmd/sh
    python ReportConnectivity.py
    ```

    Aygıt ikizbildirilen özelliklerinin güncelleştirilen onayını görmeniz gerekir.

    ![cihaz uygulamasından bildirilen özellikleri güncelleştirme](./media/iot-hub-python-twin-getstarted/device-1.png)

8. Aygıt bağlantı bilgilerini bildirdiğine göre, her iki sorguda da görünmesi gerekir. Geri dön ve sorguları yeniden çalıştır:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    Bu kez **{Aygıt Kimliği}** her iki sorgu sonuçlarında da görünmelidir.

    ![servis uygulamasında ikinci sorgu](./media/iot-hub-python-twin-getstarted/service-2.png)

    Cihazınızda, servis uygulaması tarafından gönderilen istenilen özelliklerde çift eki alındığına dair onay görürsünüz.

    ![cihaz uygulamasında istenilen özellikleri alma](./media/iot-hub-python-twin-getstarted/device-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Aygıt meta verilerini arka uç uygulamasından etiketler olarak eklediniz ve aygıt ikizindeki aygıt bağlantı bilgilerini bildirmek için simüle edilmiş bir aygıt uygulaması yazdınız. Ayrıca, kayıt defterini kullanarak bu bilgileri nasıl sorgulayabileceğinizi de öğrendiniz.

Nasıl yapılacağını öğrenmek için aşağıdaki kaynakları kullanın:

* [IoT Hub öğreticisiyle başlayın'a](quickstart-send-telemetry-python.md) sahip cihazlardan telemetri gönderin.

* Aygıtları, aygıtları [yapılandırmak için istenen özellikleri kullanarak](tutorial-device-twins.md) aygıtları yapılandırma.

* Cihazları etkileşimli olarak kontrol edin (kullanıcı kontrolündeki bir uygulamadan bir fanı açmak gibi), [doğrudan yöntem](quickstart-control-device-python.md) kullanma öğreticisiyle.
