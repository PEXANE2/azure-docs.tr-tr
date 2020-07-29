---
title: Azure IoT Hub ile buluttan cihaza iletiler (Python) | Microsoft Docs
description: Python için Azure IoT SDK 'larını kullanarak Azure IoT Hub 'ından bir cihaza buluttan cihaza ileti gönderme. Bir sanal cihaz uygulamasını buluttan cihaza iletiler alacak şekilde değiştirirsiniz ve bir arka uç uygulamasını, buluttan cihaza iletileri gönderecek şekilde değiştirirsiniz.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: robinsh
ms.custom: mqtt, tracking-python
ms.openlocfilehash: 12d25fd06ddfa8265dbf046093d3854f7fde4f33
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84609565"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>IoT Hub (Python) ile buluttan cihaza iletileri gönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub, milyonlarca cihaz ile bir çözüm arka ucu arasında güvenilir ve güvenli çift yönlü iletişimin sağlanmasına yardımcı olan, tam olarak yönetilen bir hizmettir. [Bir cihazdan IoT Hub 'ına yönelik Telemetriyi, bir](quickstart-send-telemetry-python.md) IoT Hub 'ı oluşturmayı, bu kodda bir cihaz kimliği sağlamayı ve cihazdan buluta iletiler gönderen bir sanal cihaz uygulamasını nasıl kodlayacağınızı gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici, [bir cihazdan IoT Hub 'ına telemetri gönderme hakkında bir](quickstart-send-telemetry-python.md)yapı oluşturur. Şunu gösterir:

* Çözüm arka ucundan, IoT Hub aracılığıyla buluttan cihaza iletileri tek bir cihaza gönderin.

* Bir cihazda buluttan cihaza iletiler alın.

[IoT Hub geliştirici kılavuzunda](iot-hub-devguide-messaging.md)buluttan cihaza iletiler hakkında daha fazla bilgi edinebilirsiniz.

Bu öğreticinin sonunda, iki Python konsol uygulaması çalıştırırsınız:

* **SimulatedDevice.py**, bir cihazdan Telemetriyi, IoT Hub 'ınıza bağlanan ve buluttan cihaza iletileri alan bir [IoT Hub 'ına bir cihaz aracılığıyla göndererek](quickstart-send-telemetry-python.md)oluşturulan bir uygulama sürümüdür.

* **SendCloudToDeviceMessage.py**, sanal cihaz uygulamasına IoT Hub aracılığıyla buluttan cihaza iletiler gönderir.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-v2-installation-notes](../../includes/iot-hub-include-python-v2-installation-notes.md)]

* Güvenlik duvarınızdaki 8883 numaralı bağlantı noktasını açık olduğundan emin olun. Bu makaledeki cihaz örneği, 8883 numaralı bağlantı noktası üzerinden iletişim kuran MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağ ortamlarında engellenebilir. Bu sorunu geçici olarak çözmek için daha fazla bilgi ve IoT Hub bkz. [bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="receive-messages-in-the-simulated-device-app"></a>Sanal cihaz uygulamasında ileti alma

Bu bölümde, IoT Hub 'ından cihazın benzetimini yapmak ve buluttan cihaza iletileri almak için bir Python konsol uygulaması oluşturacaksınız.

1. Çalışma dizininizde bir komut isteminden **Python Için Azure IoT Hub cihaz SDK 'sını**yükledikten sonra:

    ```cmd/sh
    pip install azure-iot-device
    ```

1. Bir metin düzenleyicisi kullanarak **SimulatedDevice.py**adlı bir dosya oluşturun.

1. `import` **SimulatedDevice.py** dosyasının başlangıcında aşağıdaki deyimleri ve değişkenleri ekleyin:

    ```python
    import threading
    import time
    from azure.iot.device import IoTHubDeviceClient

    RECEIVED_MESSAGES = 0
    ```

1. Aşağıdaki kodu **SimulatedDevice.py** dosyasına ekleyin. `{deviceConnectionString}`Yer tutucu değerini [bir cihazdan IoT Hub 'ına bir cihazdan Telemetriyi gönder](quickstart-send-telemetry-python.md) hızlı başlangıç bölümünde oluşturduğunuz cihaz bağlantı dizesiyle değiştirin:

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

1. İstemcisini başlatmak için aşağıdaki kodu ekleyin ve buluttan cihaza iletisini almaya bekleyin:

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

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

Bu makalede, [bir cihazdan IoT Hub 'ına telemetri gönderme](quickstart-send-telemetry-python.md)bölümünde oluşturduğunuz IoT Hub 'ı aracılığıyla buluttan cihaza iletileri göndermek için bir arka uç hizmeti oluşturursunuz. Buluttan cihaza iletiler göndermek için hizmetinize **hizmet bağlantısı** izni verilmesi gerekir. Varsayılan olarak, her IoT Hub, bu izni veren **hizmet** adlı paylaşılan bir erişim ilkesiyle oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="send-a-cloud-to-device-message"></a>Buluttan cihaza ileti gönderme

Bu bölümde, sanal cihaz uygulamasına buluttan cihaza iletiler gönderen bir Python konsol uygulaması oluşturacaksınız. [Bir cihazdan IoT Hub 'ına bir cihazdan Telemetriyi gönder](quickstart-send-telemetry-python.md) hızlı başlangıç bölümünde eklediğiniz CIHAZıN cihaz kimliği gereklidir. [IoT Hub bağlantı dizesini al](#get-the-iot-hub-connection-string)bölümünde daha önce kopyaladığınız IoT Hub bağlantı dizesine de ihtiyacınız vardır.

1. Çalışma dizininizde, bir komut istemi açın ve **Python Için Azure IoT Hub hizmet SDK 'sını**yükledikten sonra.

   ```cmd/sh
   pip install azure-iot-hub
   ```

1. Bir metin düzenleyicisi kullanarak **SendCloudToDeviceMessage.py**adlı bir dosya oluşturun.

1. `import` **SendCloudToDeviceMessage.py** dosyasının başlangıcında aşağıdaki deyimleri ve değişkenleri ekleyin:

    ```python
    import random
    import sys
    from azure.iot.hub import IoTHubRegistryManager

    MESSAGE_COUNT = 2
    AVG_WIND_SPEED = 10.0
    MSG_TXT = "{\"service client sent a message\": %.2f}"
    ```

1. Aşağıdaki kodu **SendCloudToDeviceMessage.py** dosyasına ekleyin. `{iot hub connection string}`Ve `{device id}` yer tutucu değerlerini daha önce not ettiğiniz IoT Hub bağlantı dizesi ve cihaz kimliği ile değiştirin:

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

1. **SendCloudToDeviceMessage.py** dosyasını kaydedin ve kapatın.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Çalışma dizininizde komut isteminde, buluttan cihaza iletileri dinlemek için aşağıdaki komutu çalıştırın:

    ```shell
    python SimulatedDevice.py
    ```

    ![Sanal cihaz uygulamasını çalıştırma](./media/iot-hub-python-python-c2d/device-1.png)

1. Çalışma dizininizde yeni bir komut istemi açın ve buluttan cihaza iletileri göndermek için aşağıdaki komutu çalıştırın:

    ```shell
    python SendCloudToDeviceMessage.py
    ```

    ![Buluttan cihaza komutunu göndermek için uygulamayı çalıştırın](./media/iot-hub-python-python-c2d/service.png)

1. Cihaz tarafından alınan iletileri aklınızda edin.

    ![İleti alındı](./media/iot-hub-python-python-c2d/device-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, buluttan cihaza iletileri gönderme ve alma hakkında daha fazla öğrendiniz.

IoT Hub kullanan uçtan uca çözümlerin tam örneklerini görmek için bkz. [Azure IoT uzaktan izleme Çözüm Hızlandırıcısı](https://azure.microsoft.com/documentation/suites/iot-suite/).

IoT Hub çözümleri geliştirme hakkında daha fazla bilgi edinmek için [IoT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)' na bakın.
