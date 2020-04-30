---
title: Azure IoT Hub cihaz TWINS (Python) ile çalışmaya başlama | Microsoft Docs
description: Azure IoT Hub cihaz ikimlerini kullanarak etiketler ekleyin ve ardından bir IoT Hub sorgusu kullanın. Python için Azure IoT SDK 'larını kullanarak, Etiketler ekleyen ve IoT Hub sorguyu çalıştıran sanal cihaz uygulamasını ve bir hizmet uygulamasını uygulayabilirsiniz.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: robinsh
ms.custom: mqtt
ms.openlocfilehash: 0bb1371de827fbb68afd5d114f49afa4acec0deb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759725"
---
# <a name="get-started-with-device-twins-python"></a>Cihaz ikizlerini kullanmaya başlama (Python)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda iki Python konsol uygulamanız olacaktır:

* Etiketler ve sorgu cihaz TWINS 'i ekleyen bir Python arka uç uygulaması olan **AddTagsAndQuery.py**.

* Daha önce oluşturulan cihaz kimliğiyle IoT Hub 'ınıza bağlanan bir cihazın benzetimini yapan ve bağlantı koşulunu raporlayan bir Python uygulaması **ReportConnectivity.py**.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-v2-installation-notes](../../includes/iot-hub-include-python-v2-installation-notes.md)]

* Güvenlik duvarınızdaki 8883 numaralı bağlantı noktasını açık olduğundan emin olun. Bu makaledeki cihaz örneği, 8883 numaralı bağlantı noktası üzerinden iletişim kuran MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağ ortamlarında engellenebilir. Bu sorunu geçici olarak çözmek için daha fazla bilgi ve IoT Hub bkz. [bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Hizmet uygulaması oluşturma

Bu bölümde, **{DEVICE ID}** ile ilişkili cihaz ikizi konum meta verilerini ekleyen bir Python konsol uygulaması oluşturacaksınız. Ardından, IoT Hub 'ında depolanan cihaz TWINS 'i, Redmond 'da bulunan cihazları ve ardından hücresel bağlantı bildirdikleri sorgular.

1. Çalışma dizininizde, bir komut istemi açın ve **Python Için Azure IoT Hub hizmet SDK 'sını**yükledikten sonra.

   ```cmd/sh
   pip install azure-iot-hub
   ```

2. Bir metin düzenleyicisi kullanarak yeni bir **AddTagsAndQuery.py** dosyası oluşturun.

3. Hizmet SDK’sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin:

   ```python
   import sys
   from time import sleep
   from azure.iot.hub import IoTHubRegistryManager
   from azure.iot.hub.models import Twin, TwinProperties, QuerySpecification, QueryResult
   ```

4. Aşağıdaki kodu ekleyin. `[IoTHub Connection String]` [IoT Hub bağlantı dizesini al](#get-the-iot-hub-connection-string)bölümünde kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin. `[Device Id]` [Yeni bir cihazı IoT Hub 'ına kaydetmek](#register-a-new-device-in-the-iot-hub)için kaydettiğiniz cihaz kimliğiyle değiştirin.
  
    ```python
    IOTHUB_CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"
    ```

5. Aşağıdaki kodu **AddTagsAndQuery.py** dosyasına ekleyin:

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

    **Iothubregistrymanager** nesnesi, hizmetten cihaz iksiyle etkileşimde bulunmak için gereken tüm yöntemleri kullanıma sunar. Kod ilk olarak **Iothubregistrymanager** nesnesini başlatır, sonra **DEVICE_ID**için cihaz ikizi güncelleştirir ve son olarak iki sorgu çalıştırır. İlki, **Redmond43** tesisinde bulunan cihazların yalnızca cihaz ikelerini seçer ve ikincisi ise yalnızca bir hücresel ağ aracılığıyla bağlanan cihazları seçecek şekilde sorguyu iyileştirir.

6. **İothub_service_sample_run** işlevini uygulamak için **AddTagsAndQuery.py** sonuna aşağıdaki kodu ekleyin:

    ```python
    if __name__ == '__main__':
        print("Starting the Python IoT Hub Device Twin service sample...")
        print()

        iothub_service_sample_run()
    ```

7. Uygulamayı ile çalıştırın:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    **Redmond43** ' de bulunan tüm cihazları isteyen sorgunun sonuçlarında bir cihaz görmeniz gerekir ve sonuçları hücresel ağ kullanan cihazlara kısıtlayan sorgu için yok.

    ![Redmond 'daki tüm cihazları gösteren ilk sorgu](./media/iot-hub-python-twin-getstarted/service-1.png)

Sonraki bölümde, bağlantı bilgilerini raporlayan ve önceki bölümde sorgunun sonucunu değiştiren bir cihaz uygulaması oluşturacaksınız.

## <a name="create-the-device-app"></a>Cihaz uygulamasını oluşturma

Bu bölümde, hub 'ınıza **{CIHAZ kimliğiniz}** olarak bağlanan bir Python konsol uygulaması oluşturursunuz ve ardından cihaz ikizi 'nin bildirilen özelliklerini bir hücresel ağ kullanılarak bağlı olduğu bilgileri içerecek şekilde güncelleştirir.

1. Çalışma dizininizde bir komut isteminden **Python Için Azure IoT Hub cihaz SDK 'sını**yükledikten sonra:

    ```cmd/sh
    pip install azure-iot-device
    ```

2. Bir metin düzenleyicisi kullanarak yeni bir **ReportConnectivity.py** dosyası oluşturun.

3. Cihaz SDK 'sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin:

    ```python
    import time
    import threading
    from azure.iot.device import IoTHubModuleClient
    ```

4. Aşağıdaki kodu ekleyin. Yer tutucu `[IoTHub Device Connection String]` değerini, [IoT Hub 'ına yeni bir cihaz kaydet](#register-a-new-device-in-the-iot-hub)' de kopyaladığınız cihaz bağlantı dizesiyle değiştirin.

    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    ```

5. Cihaz TWINS işlevlerini uygulamak için aşağıdaki kodu **ReportConnectivity.py** dosyasına ekleyin:

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

    **Iothubmoduleclient** nesnesi cihazdaki cihaz iksiyle etkileşimde bulunmak için gereken tüm yöntemleri ortaya koyar. Önceki kod, **ıothubmoduleclient** nesnesini başlattıktan sonra, cihazınız için cihaz ikizi alır ve bildirilen özelliğini bağlantı bilgileriyle güncelleştirir.

6. **İothub_client_sample_run** işlevini uygulamak için **ReportConnectivity.py** sonuna aşağıdaki kodu ekleyin:

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

    Cihazın ikizi bildirilen özelliklerinin güncelleştirildiğini onay görmeniz gerekir.

    ![bildirilen özellikleri cihaz uygulamasından Güncelleştir](./media/iot-hub-python-twin-getstarted/device-1.png)

8. Cihaz bağlantı bilgilerini raporladığı için, her iki sorgu da görünmelidir. Geri dönüp sorguları yeniden çalıştırın:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    Bu kez, **{DEVICE ID}** hesabınız her iki sorgu sonucunda da görünmelidir.

    ![hizmet uygulamasındaki ikinci sorgu](./media/iot-hub-python-twin-getstarted/service-2.png)

    Cihaz uygulamanızda, hizmet uygulaması tarafından gönderilen istenen özellikler ikizi düzeltme ekinin alındığını belirten bir onay görürsünüz.

    ![cihaz uygulamasında istenen özellikleri al](./media/iot-hub-python-twin-getstarted/device-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini bir arka uç uygulamasından Etiketler olarak eklediniz ve cihaz ikizi cihaz bağlantı bilgilerini raporlamak için bir sanal cihaz uygulaması yazmış olursunuz. Ayrıca, bu bilgileri kayıt defterini kullanarak sorgulama hakkında bilgi edinirsiniz.

Aşağıdakilerin nasıl yapılacağını öğrenmek için aşağıdaki kaynakları kullanın:

* [IoT Hub ile çalışmaya başlama](quickstart-send-telemetry-python.md) öğreticisini kullanarak cihazlardan telemetri gönderin.

* [Cihazları yapılandırmak için istenen özellikleri kullan](tutorial-device-twins.md) öğreticisiyle cihaz ikizi 'nin istenen özelliklerini kullanarak cihazları yapılandırın.

* Cihazları etkileşimli olarak (kullanıcı denetimli bir uygulamadan bir fanı açmak gibi) [doğrudan Yöntemler](quickstart-control-device-python.md) öğreticisini kullanarak kontrol edin.
