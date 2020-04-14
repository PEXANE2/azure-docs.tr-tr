---
title: Azure IoT Hub (Python) ile buluttan cihaza iletiler | Microsoft Dokümanlar
description: Python için Azure IoT SDK'larını kullanarak bir Azure IoT hub'ından bir aygıta buluttan aygıta ileti gönderme. Simüle edilmiş bir aygıt uygulamasını, buluttan cihaza iletiler almak için ve buluttan cihaza iletigöndermek için bir arka uç uygulamasını değiştirmek için değiştirirsiniz.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: robinsh
ms.openlocfilehash: 0c3b35eeed85dd3a1c44dea6ec46203eb812e1e8
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257867"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>IoT Hub (Python) ile buluttan aygıta iletiler gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub, milyonlarca aygıt ve bir çözüm arka uç arasında güvenilir ve güvenli çift yönlü iletişim sağlamaya yardımcı olan tam olarak yönetilen bir hizmettir. Bir [aygıttan IoT hub'ına hızlı başlat'a gönder telemetrisi,](quickstart-send-telemetry-python.md) bir IoT hub'ı oluşturmanın, içinde aygıt kimliğini nasıl sağlayabilirsiniz ve aygıttan buluta iletigönderen simüle edilmiş bir aygıt uygulamasını kodlamayı gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici, [bir aygıttan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-python.md)biryapılsa. Nasıl yapılacağını gösterir:

* Çözümünüzden arka uçtan, IoT Hub üzerinden tek bir cihaza buluttan cihaza iletigönderin.

* Bir aygıttan buluttan aygıta iletiler alın.

[IoT Hub geliştirici kılavuzunda](iot-hub-devguide-messaging.md)buluttan cihaza iletiler hakkında daha fazla bilgi bulabilirsiniz.

Bu eğitimin sonunda, iki Python konsol uyrama uygulaması çalıştırın:

* **SimulatedDevice.py,** bir [cihazdan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-python.md)oluşturulan uygulamanın değiştirilmiş bir sürümü , IoT hub'ınıza bağlanır ve buluttan cihaza iletiler alır.

* **SendCloudToDeviceMessage.py**, IoT Hub üzerinden simüle edilen aygıt uygulamasına buluttan cihaza iletiler gönderir.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-v2-installation-notes](../../includes/iot-hub-include-python-v2-installation-notes.md)]

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="receive-messages-in-the-simulated-device-app"></a>Benzetimli cihaz uygulamasından ileti alma

Bu bölümde, aygıtı simüle etmek ve IoT hub'ından buluttan cihaza iletiler almak için bir Python konsol uygulaması oluşturursunuz.

1. Çalışma dizininizdeki bir komut isteminden **Python için Azure IoT Hub Aygıt SDK'sını**yükleyin:

    ```cmd/sh
    pip install azure-iot-device
    ```

1. Metin düzenleyicisi kullanarak, **SimulatedDevice.py**adlı bir dosya oluşturun.

1. `import` **SimulatedDevice.py** dosyasının başında aşağıdaki ifadeleri ve değişkenleri ekleyin:

    ```python
    import threading
    import time
    from azure.iot.device import IoTHubDeviceClient

    RECEIVED_MESSAGES = 0
    ```

1. **SimulatedDevice.py** dosyaya aşağıdaki kodu ekleyin. Yer tutucu değerini, bir aygıttan ioT hub'ına hızlı başlatarak telemetri gönder'de oluşturduğunuz aygıt için aygıt bağlantı dizesiyle [değiştirin:](quickstart-send-telemetry-python.md) `{deviceConnectionString}`

    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

1. Alınan iletileri konsola yazdırmak için aşağıdaki işlevi ekleyin:

    ```python
    def message_listener(client):
        global RECEIVED_MESSAGES
        while True:
            message = client.receive_message()
            RECEIVED_MESSAGES += 1
            print("\nMessage received:")

            #print data and both system and application (custom) properties
            for property in vars(message).items():
                print ("    {0}".format(property))

            print( "Total calls received: {}".format(RECEIVED_MESSAGES))
            print()
    ```

1. İstemciyi başlatmave buluttan aygıta iletiyi almayı beklemek için aşağıdaki kodu ekleyin:

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
            print ( "IoT Hub C2D Messaging device sample stopped" )
    ```

1. Aşağıdaki ana işlevi ekleyin:

    ```python
    if __name__ == '__main__':
        print ( "Starting the Python IoT Hub C2D Messaging device sample..." )
        print ( "Waiting for C2D messages, press Ctrl-C to exit" )

        iothub_client_sample_run()
    ```

1. **SimulatedDevice.py** dosyasını kaydedin ve kapatın.

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, bir aygıttan bir [IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-python.md)oluşturduğunuz IoT hub'ı üzerinden buluttan aygıta iletigöndermek için bir arka uç hizmeti oluşturursunuz. Buluttan cihaza ileti göndermek için hizmetinizin **hizmet bağlantısı** namına ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="send-a-cloud-to-device-message"></a>Buluttan aygıta ileti gönderme

Bu bölümde, simüle edilen aygıt uygulamasına buluttan cihaza iletigönderen bir Python konsol uygulaması oluşturursunuz. Bir [aygıttan IoT](quickstart-send-telemetry-python.md) hub'ına hızlı başlat'a telemetri gönder'e eklediğiniz aygıtın aygıt kimliğine ihtiyacınız vardır. Ayrıca, IoT hub bağlantı dizesini al'da daha önce kopyaladığınız [IoT hub bağlantı dizesini](#get-the-iot-hub-connection-string)de almanız gerekir.

1. Çalışma dizininizde bir komut istemi açın ve **Python için Azure IoT Hub Hizmeti SDK'sını**yükleyin.

   ```cmd/sh
   pip install azure-iot-hub
   ```

1. Metin düzenleyicisi **kullanarak, SendCloudToDeviceMessage.py**adlı bir dosya oluşturun.

1. `import` **SendCloudToDeviceMessage.py** dosyasının başında aşağıdaki ifadeleri ve değişkenleri ekleyin:

    ```python
    import random
    import sys
    from azure.iot.hub import IoTHubRegistryManager

    MESSAGE_COUNT = 2
    AVG_WIND_SPEED = 10.0
    MSG_TXT = "{\"service client sent a message\": %.2f}"
    ```

1. **SendCloudToDeviceMessage.py** dosyaya aşağıdaki kodu ekleyin. `{iot hub connection string}` Yer `{device id}` tutucu değerlerini daha önce belirttiğiniz IoT hub bağlantı dizesi ve aygıt kimliğiyle değiştirin:

    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"
    ```

1. Cihazınıza ileti göndermek için aşağıdaki kodu ekleyin:

    ```python
    def iothub_messaging_sample_run():
        try:
            # Create IoTHubRegistryManager
            registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

            for i in range(0, MESSAGE_COUNT):
                print ( 'Sending message: {0}'.format(i) )
                data = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))

                props={}
                # optional: assign system properties
                props.update(messageId = "message_%d" % i)
                props.update(correlationId = "correlation_%d" % i)
                props.update(contentType = "application/json")

                # optional: assign application properties
                prop_text = "PropMsg_%d" % i
                props.update(testProperty = prop_text)

                registry_manager.send_c2d_message(DEVICE_ID, data, properties=props)

            try:
                # Try Python 2.xx first
                raw_input("Press Enter to continue...\n")
            except:
                pass
                # Use Python 3.xx in the case of exception
                input("Press Enter to continue...\n")

        except Exception as ex:
            print ( "Unexpected error {0}" % ex )
            return
        except KeyboardInterrupt:
            print ( "IoT Hub C2D Messaging service sample stopped" )
    ```

1. Aşağıdaki ana işlevi ekleyin:

    ```python
    if __name__ == '__main__':
        print ( "Starting the Python IoT Hub C2D Messaging service sample..." )

        iothub_messaging_sample_run()
    ```

1. Dosyayı kaydedin ve kapatın **SendCloudToDeviceMessage.py.**

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Çalışma dizininizdeki komut isteminde, buluttan aygıta iletileri dinlemek için aşağıdaki komutu çalıştırın:

    ```shell
    python SimulatedDevice.py
    ```

    ![Benzetimli cihaz uygulamasını çalıştırma](./media/iot-hub-python-python-c2d/device-1.png)

1. Çalışma dizininizde yeni bir komut istemi açın ve buluttan aygıta iletigöndermek için aşağıdaki komutu çalıştırın:

    ```shell
    python SendCloudToDeviceMessage.py
    ```

    ![Bulut-aygıt komutunu göndermek için uygulamayı çalıştırın](./media/iot-hub-python-python-c2d/service.png)

1. Aygıt tarafından alınan iletilere dikkat edin.

    ![Alınan ileti](./media/iot-hub-python-python-c2d/device-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, buluttan cihaza ileti göndermeyi ve almayı öğrendiniz.

IoT Hub'ı kullanan eksiksiz uçtan uca çözümlerin örneklerini görmek için [Azure IoT Uzaktan İzleme çözüm hızlandırıcısına](https://azure.microsoft.com/documentation/suites/iot-suite/)bakın.

IoT Hub ile çözüm geliştirme hakkında daha fazla bilgi edinmek için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.
