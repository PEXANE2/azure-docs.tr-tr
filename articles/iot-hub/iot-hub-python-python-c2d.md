---
title: Azure IoT Hub (Python) ile buluttan cihaza iletiler | Microsoft Dokümanlar
description: Python için Azure IoT SDK'larını kullanarak bir Azure IoT hub'ından bir aygıta buluttan aygıta ileti gönderme. Simüle edilmiş bir aygıt uygulamasını, buluttan cihaza iletiler almak için ve buluttan cihaza iletigöndermek için bir arka uç uygulamasını değiştirmek için değiştirirsiniz.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: robinsh
ms.openlocfilehash: 3613062cf8765a4aec3327b660bb5818898f2dd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110425"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>IoT Hub (Python) ile buluttan aygıta iletiler gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub, milyonlarca aygıt ve bir çözüm arka uç arasında güvenilir ve güvenli çift yönlü iletişim sağlamaya yardımcı olan tam olarak yönetilen bir hizmettir. Bir [aygıttan IoT hub'ına hızlı başlat'a gönder telemetrisi,](quickstart-send-telemetry-python.md) bir IoT hub'ı oluşturmanın, içinde aygıt kimliğini nasıl sağlayabilirsiniz ve aygıttan buluta iletigönderen simüle edilmiş bir aygıt uygulamasını kodlamayı gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici, [bir aygıttan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-python.md)biryapılsa. Nasıl yapılacağını gösterir:

* Çözümünüzden arka uçtan, IoT Hub üzerinden tek bir cihaza buluttan cihaza iletigönderin.

* Bir aygıttan buluttan aygıta iletiler alın.

* Çözümünüzden arka uçtan, IoT Hub'dan bir cihaza gönderilen iletiler için teslimat bildirimi *(geri bildirim)* isteyin.

[IoT Hub geliştirici kılavuzunda](iot-hub-devguide-messaging.md)buluttan cihaza iletiler hakkında daha fazla bilgi bulabilirsiniz.

Bu eğitimin sonunda, iki Python konsol uyrama uygulaması çalıştırın:

* **SimulatedDevice.py,** bir [cihazdan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-python.md)oluşturulan uygulamanın değiştirilmiş bir sürümü , IoT hub'ınıza bağlanır ve buluttan cihaza iletiler alır.

* **SendCloudToDeviceMessage.py,** IoT Hub üzerinden simüle edilen aygıt uygulamasına buluttan cihaza ileti gönderir ve ardından teslim bildirimini alır.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="receive-messages-in-the-simulated-device-app"></a>Benzetimli cihaz uygulamasından ileti alma

Bu bölümde, aygıtı simüle etmek ve IoT hub'ından buluttan cihaza iletiler almak için bir Python konsol uygulaması oluşturursunuz.

1. Metin düzenleyicisi **kullanarak, SimulatedDevice.py** bir dosya oluşturun.

2. `import` **SimulatedDevice.py** dosyasının başında aşağıdaki ifadeleri ve değişkenleri ekleyin:

   ```python
    import threading
    from azure.iot.device import IoTHubDeviceClient

    RECEIVED_MESSAGES = 0
    ```

3. **SimulatedDevice.py** dosyaya aşağıdaki kodu ekleyin. "{deviceConnectionString}" yer tutucu değerini, [bir aygıttan IoT hub'ına](quickstart-send-telemetry-python.md) hızlı başlatarak telemetri gönder'de oluşturduğunuz aygıt bağlantısı dizesiyle değiştirin:

    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

4. Alınan iletileri konsola yazdırmak için aşağıdaki işlevi ekleyin:

    ```python
    def message_listener(client):
        global RECEIVED_MESSAGES
        while True:
            message = client.receive_message()
            RECEIVED_MESSAGES += 1
            print("Message received")
            print( "    Data: <<{}>>".format(message.data) )
            print( "    Properties: {}".format(message.custom_properties))
            print( "    Total calls received: {}".format(RECEIVED_MESSAGES))
    ```

5. İstemciyi başlatmave buluttan aygıta iletiyi almayı beklemek için aşağıdaki kodu ekleyin:

    ```python
    def iothub_client_sample_run():
        try:
            client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)

            message_listener_thread = threading.Thread(target=message_listener, args=(client,))
            message_listener_thread.daemon = True
            message_listener_thread.start()

            while True:
                time.sleep(1000)

        except KeyboardInterrupt:
            print ( "IoTHubDeviceClient sample stopped" )
    ```

6. Aşağıdaki ana işlevi ekleyin:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "IoTHubDeviceClient waiting for commands, press Ctrl-C to exit" )

        iothub_client_sample_run()
    ```

7. Dosyayı kaydedin ve **kapatınSimulatedDevice.py.**

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, bir aygıttan bir [IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-python.md)oluşturduğunuz IoT hub'ı üzerinden buluttan aygıta iletigöndermek için bir arka uç hizmeti oluşturursunuz. Buluttan cihaza ileti göndermek için hizmetinizin **hizmet bağlantısı** namına ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="send-a-cloud-to-device-message"></a>Buluttan aygıta ileti gönderme

Bu bölümde, simüle edilen aygıt uygulamasına buluttan cihaza iletigönderen bir Python konsol uygulaması oluşturursunuz. Bir [aygıttan IoT](quickstart-send-telemetry-python.md) hub'ına hızlı başlat'a telemetri gönder'e eklediğiniz aygıtın aygıt kimliğine ihtiyacınız vardır. Ayrıca, IoT hub bağlantı dizesini al'da daha önce kopyaladığınız [IoT hub bağlantı dizesini](#get-the-iot-hub-connection-string)de almanız gerekir.

1. Metin düzenleyicisi **kullanarak, SendCloudToDeviceMessage.py** dosyası oluşturun.

2. `import` **SendCloudToDeviceMessage.py** dosyasının başında aşağıdaki ifadeleri ve değişkenleri ekleyin:

    ```python
    import random
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubMessaging, IoTHubMessage, IoTHubError

    OPEN_CONTEXT = 0
    FEEDBACK_CONTEXT = 1
    MESSAGE_COUNT = 1
    AVG_WIND_SPEED = 10.0
    MSG_TXT = "{\"service client sent a message\": %.2f}"
    ```

3. **SendCloudToDeviceMessage.py** dosyaya aşağıdaki kodu ekleyin. "{iot hub bağlantı string}" ve "{device id}" yer tutucu değerlerini daha önce belirttiğiniz IoT hub bağlantı dizesi ve aygıt kimliğiyle değiştirin:

    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"
    ```

4. Geri bildirim iletilerini konsola yazdırmak için aşağıdaki işlevi ekleyin:

    ```python
    def open_complete_callback(context):
        print ( 'open_complete_callback called with context: {0}'.format(context) )

    def send_complete_callback(context, messaging_result):
        context = 0
        print ( 'send_complete_callback called with context : {0}'.format(context) )
        print ( 'messagingResult : {0}'.format(messaging_result) )
    ```

5. Cihazınıza ileti göndermek ve aygıt buluttan aygıta iletiyi kabul ettiğinde geri bildirim iletisini işlemek için aşağıdaki kodu ekleyin:

    ```python
    def iothub_messaging_sample_run():
        try:
            iothub_messaging = IoTHubMessaging(CONNECTION_STRING)

            iothub_messaging.open(open_complete_callback, OPEN_CONTEXT)

            for i in range(0, MESSAGE_COUNT):
                print ( 'Sending message: {0}'.format(i) )
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))

                # optional: assign ids
                message.message_id = "message_%d" % i
                message.correlation_id = "correlation_%d" % i
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % i
                prop_map.add("Property", prop_text)

                iothub_messaging.send_async(DEVICE_ID, message, send_complete_callback, i)

            try:
                # Try Python 2.xx first
                raw_input("Press Enter to continue...\n")
            except:
                pass
                # Use Python 3.xx in the case of exception
                input("Press Enter to continue...\n")

            iothub_messaging.close()

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubMessaging sample stopped" )
    ```

6. Aşağıdaki ana işlevi ekleyin:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client Messaging Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_messaging_sample_run()
    ```

7. Dosyayı kaydedin ve kapatın **SendCloudToDeviceMessage.py.**

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Bir komut istemi açın ve **Python için Azure IoT Hub Aygıt SDK'sını**yükleyin.

    ```shell
    pip install azure-iothub-device-client
    ```

2. Komut isteminde, buluttan aygıta iletileri dinlemek için aşağıdaki komutu çalıştırın:

    ```shell
    python SimulatedDevice.py
    ```

    ![Benzetimli cihaz uygulamasını çalıştırma](./media/iot-hub-python-python-c2d/simulated-device.png)

3. Yeni bir komut istemi açın ve **Python için Azure IoT Hub Hizmeti SDK'sını**yükleyin.

    ```shell
    pip install azure-iothub-service-client
    ```

4. Komut isteminde, buluttan aygıta ileti göndermek ve ileti geri bildirimini beklemek için aşağıdaki komutu çalıştırın:

    ```shell
    python SendCloudToDeviceMessage.py
    ```

    ![Bulut-aygıt komutunu göndermek için uygulamayı çalıştırın](./media/iot-hub-python-python-c2d/send-command.png)

5. Aygıt tarafından alınan iletiyi not edin.

    ![Alınan ileti](./media/iot-hub-python-python-c2d/message-received.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, buluttan cihaza ileti göndermeyi ve almayı öğrendiniz.

IoT Hub'ı kullanan eksiksiz uçtan uca çözümlerin örneklerini görmek için [Azure IoT Uzaktan İzleme çözüm hızlandırıcısına](https://azure.microsoft.com/documentation/suites/iot-suite/)bakın.

IoT Hub ile çözüm geliştirme hakkında daha fazla bilgi edinmek için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.
