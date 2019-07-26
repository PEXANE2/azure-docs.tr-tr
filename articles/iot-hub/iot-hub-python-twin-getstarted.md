---
title: Azure IoT Hub cihaz TWINS (Python) ile çalışmaya başlama | Microsoft Docs
description: Azure IoT Hub cihaz ikimlerini kullanarak etiketler ekleyin ve ardından bir IoT Hub sorgusu kullanın. Python için Azure IoT SDK 'larını kullanarak, Etiketler ekleyen ve IoT Hub sorguyu çalıştıran sanal cihaz uygulamasını ve bir hizmet uygulamasını uygulayabilirsiniz.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: kgremban
ms.openlocfilehash: 53e3d32497c7aae6c584d23b9baddbaeaf1bd822
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405868"
---
# <a name="get-started-with-device-twins-python"></a>Cihaz ikizlerini kullanmaya başlama (Python)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu öğreticinin sonunda iki Python konsol uygulamanız olacaktır:

* Etiketler ve sorgu cihaz TWINS 'i ekleyen bir Python arka uç uygulaması olan **AddTagsAndQuery.py**.

* Daha önce oluşturulan cihaz kimliğiyle IoT Hub 'ınıza bağlanan bir cihazın benzetimini yapan ve bağlantı koşulunu raporlayan bir Python uygulaması **ReportConnectivity.py**.

> [!NOTE]
> [Azure IoT SDK 'ları](iot-hub-devguide-sdks.md) makalesi, hem cihaz hem de arka uç uygulamaları oluşturmak Için kullanabileceğiniz Azure IoT SDK 'ları hakkında bilgi sağlar.

Bu öğreticiyi tamamlayabilmeniz için şunlar gerekir:

* [Python 2. x veya 3. x](https://www.python.org/downloads/). Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python’u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*'yi yüklemeniz veya yükseltmeniz](https://pip.pypa.io/en/stable/installing/) gerekebilir.

* Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi](https://www.microsoft.com/download/confirmation.aspx?id=48145).

* Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

> [!NOTE]
> `azure-iothub-service-client` ve `azure-iothub-device-client` için *PIP* paketleri şu anda yalnızca Windows İşletim Sistemi için mevcuttur. Linux/Mac OS için lütfen [geliştirme ortamınızı Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) gönderisini hazırlama bölümündeki linux ve Mac OS özgü bölümlere bakın.
>

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>IoT Hub 'a yeni bir cihaz kaydetme

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Hizmet uygulaması oluşturma

Bu bölümde, **{DEVICE ID}** ile ilişkili cihaz ikizi konum meta verilerini ekleyen bir Python konsol uygulaması oluşturacaksınız. Ardından, IoT Hub 'ında depolanan cihaz TWINS 'i, Redmond 'da bulunan cihazları ve ardından hücresel bağlantı bildirdikleri sorgular.

1. Komut istemini açın ve **Python için Azure IoT Hub Hizmeti SDK’sını** aşağıda gösterildiği gibi yükleyin. SDK’yı yükledikten sonra komut istemini kapatın.

   ```
   pip install azure-iothub-service-client
   ```

2. Bir metin düzenleyicisi kullanarak yeni bir **AddTagsAndQuery.py** dosyası oluşturun.

3. Hizmet SDK’sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin:

   ```python
   import sys
   import iothub_service_client
   from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
   from iothub_service_client import IoTHubDeviceTwin, IoTHubError
   ```

4. `[IoTHub Connection String]` Ve`[Device Id]` için yer tutucusunu, IoT Hub için bağlantı dizesiyle ve önceki bölümlerde oluşturduğunuz cihaz kimliğiyle değiştirerek aşağıdaki kodu ekleyin.
  
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"location\":\"Redmond\"}}}"

    UPDATE_JSON_SEARCH = "\"location\":\"Redmond\""
    UPDATE_JSON_CLIENT_SEARCH = "\"connectivity\":\"cellular\""
    ```

5. Aşağıdaki kodu **AddTagsAndQuery.py** dosyasına ekleyin:

     ```python
    def iothub_service_sample_run():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

            iothub_registry_statistics = iothub_registry_manager.get_statistics()
            print ( "Total device count                       : {0}".format(iothub_registry_statistics.totalDeviceCount) )
            print ( "Enabled device count                     : {0}".format(iothub_registry_statistics.enabledDeviceCount) )
            print ( "Disabled device count                    : {0}".format(iothub_registry_statistics.disabledDeviceCount) )
            print ( "" )

            number_of_devices = iothub_registry_statistics.totalDeviceCount
            dev_list = iothub_registry_manager.get_device_list(number_of_devices)

            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            for device in range(0, number_of_devices):
                if dev_list[device].deviceId == DEVICE_ID:
                    twin_info = iothub_twin_method.update_twin(dev_list[device].deviceId, UPDATE_JSON)

            print ( "Devices in Redmond: " )
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)

                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    print ( dev_list[device].deviceId )

            print ( "" )

            print ( "Devices in Redmond using cellular network: " )
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)

                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    if twin_info.find(UPDATE_JSON_CLIENT_SEARCH) > -1:
                        print ( dev_list[device].deviceId )

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "IoTHub sample stopped" )
    ```

    **Kayıt defteri** nesnesi, hizmetten cihaz ikikiyle etkileşimde bulunmak için gereken tüm yöntemleri kullanıma sunar. Kod, ilk olarak **kayıt defteri** nesnesini başlatır ve ardından, **DeviceID**için cihaz ikizi güncelleştirir ve son olarak iki sorgu çalıştırır. İlki, **Redmond43** tesisinde bulunan cihazların yalnızca cihaz ikelerini seçer ve ikincisi ise yalnızca hücresel ağ üzerinden de bağlı olan cihazları seçecek şekilde sorguyu iyileştirir.

6. **İothub_service_sample_run** işlevini uygulamak için **AddTagsAndQuery.py** sonuna aşağıdaki kodu ekleyin:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python service sample..." )

        iothub_service_sample_run()
    ```

7. Uygulamayı ile çalıştırın:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    **Redmond43** ' de bulunan tüm cihazları isteyen sorgunun sonuçlarında bir cihaz görmeniz gerekir ve sonuçları hücresel ağ kullanan cihazlara kısıtlayan sorgu için yok.

    ![Redmond 'daki tüm cihazları gösteren ilk sorgu](./media/iot-hub-python-twin-getstarted/1-device-twins-python-service-sample.png)

Sonraki bölümde, bağlantı bilgilerini raporlayan ve önceki bölümde sorgunun sonucunu değiştiren bir cihaz uygulaması oluşturacaksınız.

## <a name="create-the-device-app"></a>Cihaz uygulaması oluşturma

Bu bölümde, hub 'ınıza **{CIHAZ kimliğiniz}** olarak bağlanan bir Python konsol uygulaması oluşturursunuz ve ardından cihaz ikizi 'nin bildirilen özelliklerini bir hücresel ağ kullanılarak bağlı olduğu bilgileri içerecek şekilde güncelleştirir.

1. Komut istemini açın ve **Python için Azure IoT Hub Hizmeti SDK’sını** aşağıda gösterildiği gibi yükleyin. SDK’yı yükledikten sonra komut istemini kapatın.

    ```
    pip install azure-iothub-device-client
    ```

2. Bir metin düzenleyicisi kullanarak yeni bir **ReportConnectivity.py** dosyası oluşturun.

3. Hizmet SDK’sından gerekli modülleri içeri aktarmak için aşağıdaki kodu ekleyin:

    ```python
    import time
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError
    ```

4. Önceki bölümlerde oluşturduğunuz IoT Hub cihazı için bağlantı dizesiyle `[IoTHub Device Connection String]` birlikte yer tutucusunu değiştirerek aşağıdaki kodu ekleyin.

    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"

    # choose HTTP, AMQP, AMQP_WS or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT

    TIMER_COUNT = 5
    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0
    ```

5. Cihaz TWINS işlevlerini uygulamak için aşağıdaki kodu **ReportConnectivity.py** dosyasına ekleyin:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "" )
        print ( "Twin callback called with:" )
        print ( "    updateStatus: %s" % update_state )
        print ( "    payload: %s" % payload )

    def send_reported_state_callback(status_code, user_context):
        print ( "" )
        print ( "Confirmation for reported state called with:" )
        print ( "    status_code: %d" % status_code )

    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        if client.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            client.set_device_twin_callback(
                device_twin_callback, TWIN_CONTEXT)

        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            if client.protocol == IoTHubTransportProvider.MQTT:
                print ( "Sending data as reported property..." )

                reported_state = "{\"connectivity\":\"cellular\"}"

                client.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            while True:
                print ( "Press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= TIMER_COUNT:
                    status = client.get_send_status()
                    time.sleep(10)
                    status_counter += 1 
        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
     ```

    **İstemci** nesnesi, cihazdan cihaz ikikiyle etkileşimde bulunmak için gereken tüm yöntemleri kullanıma sunar. Önceki kod, **istemci** nesnesini başlattıktan sonra cihazınızla ilgili cihaz ikizi alır ve bildirilen özelliğini bağlantı bilgileriyle güncelleştirir.

6. **İothub_client_sample_run** işlevini uygulamak için **ReportConnectivity.py** sonuna aşağıdaki kodu ekleyin:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python client sample..." )

        iothub_client_sample_run()
    ```

7. Cihaz uygulamasını çalıştırın:

    ```cmd/sh
    python ReportConnectivity.py
    ```

    Cihaz ikikinin güncelleştirildiğinden onay görmeniz gerekir.

    ![güncelleştirme TWINS](./media/iot-hub-python-twin-getstarted/2-python-client-sample.png)

8. Cihaz bağlantı bilgilerini raporladığı için, her iki sorgu da görünmelidir. Geri dönüp sorguları yeniden çalıştırın:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    Bu kez, **{DEVICE ID}** hesabınız her iki sorgu sonucunda da görünmelidir.

    ![İkinci sorgu](./media/iot-hub-python-twin-getstarted/3-device-twins-python-service-sample.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Cihaz meta verilerini bir arka uç uygulamasından Etiketler olarak eklediniz ve cihaz ikizi cihaz bağlantı bilgilerini raporlamak için bir sanal cihaz uygulaması yazmış olursunuz. Ayrıca, bu bilgileri kayıt defterini kullanarak sorgulama hakkında bilgi edinirsiniz.

Aşağıdakilerin nasıl yapılacağını öğrenmek için aşağıdaki kaynakları kullanın:

* [IoT Hub ile çalışmaya başlama](quickstart-send-telemetry-python.md) öğreticisini kullanarak cihazlardan telemetri gönderin.

* [Cihazları yapılandırmak için istenen özellikleri kullan](tutorial-device-twins.md) öğreticisiyle cihaz ikizi 'nin istenen özelliklerini kullanarak cihazları yapılandırın.

* Cihazları etkileşimli olarak (kullanıcı denetimli bir uygulamadan bir fanı açmak gibi) [doğrudan Yöntemler](quickstart-control-device-python.md) öğreticisini kullanarak kontrol edin.