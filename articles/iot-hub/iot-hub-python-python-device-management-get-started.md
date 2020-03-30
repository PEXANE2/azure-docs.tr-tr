---
title: Azure IoT Hub aygıt yönetimi (Python) | Microsoft Dokümanlar
description: Uzak bir aygıtı yeniden başlatmak için IoT Hub aygıt yönetimi nasıl kullanılır. Python için Azure IoT SDK'yı kullanarak doğrudan bir yöntem ve doğrudan yöntemi çağıran bir hizmet uygulaması içeren simüle edilmiş bir aygıt uygulamasını uygularsınız.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 01/17/2020
ms.author: robinsh
ms.openlocfilehash: 6d6a50db42924d868b57cacc415246ee6990859c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110483"
---
# <a name="get-started-with-device-management-python"></a>Cihaz yönetimine (Python) başlayın

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IoT hub'ınızda bir IoT Hub'ı oluşturmak ve aygıt kimliği oluşturmak için Azure portalını kullanın.

* Bu aygıtı yeniden başlatan doğrudan bir yöntem içeren benzetimli bir aygıt uygulaması oluşturun. Buluttan doğrudan yöntemler çağrılır.

* IoT hub'ınız aracılığıyla simüle edilen cihaz uygulamasında yeniden başlatma yöntemini çağıran bir Python konsol uygulaması oluşturun.

Bu eğitimin sonunda, iki Python konsol uyrama uygulamanız var:

* **dmpatterns_getstarted_device.py**, daha önce oluşturulan aygıt kimliği ile IoT hub'ınıza bağlanır, yeniden başlatma doğrudan yöntemini alır, fiziksel bir yeniden başlatmayı simüle eder ve son yeniden başlatma süresini bildirir.

* benzetilen aygıt uygulamasında doğrudan bir yöntem çağıran **dmpatterns_getstarted_service.py**yanıtı görüntüler ve güncellenen bildirilen özellikleri görüntüler.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-v2-installation-notes.md)]

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde şunları yapacaksınız:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Python konsol uygulaması oluşturma

* Aygıtı yeniden başlatmayı simüle edin

* Aygıtikiz sorgularının aygıtları tanımlamasını ve en son ne zaman yeniden başlatılmasını sağlamak için bildirilen özellikleri kullanma

1. Komut isteminizde **azure-iot-device** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iot-device
    ```

2. Bir metin düzenleyicisi kullanarak, çalışma dizininizde **dmpatterns_getstarted_device.py** adlı bir dosya oluşturun.

3. `import` **dmpatterns_getstarted_device.py** dosyasının başında aşağıdaki ifadeleri ekleyin.

    ```python
    import threading
    import time
    import datetime
    from azure.iot.device import IoTHubDeviceClient, MethodResponse
    ```

4. **CONNECTION_STRING** değişkenini ekleyin. `{deviceConnectionString}` Yer tutucu değerini aygıt bağlantı dizenizle değiştirin. Bu bağlantı dizesini daha önce [IoT hub'ında yeni bir aygıtı kaydedin'de kopyaladınız.](#register-a-new-device-in-the-iot-hub)  

    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

5. Aygıtta doğrudan yöntemi uygulamak için aşağıdaki işlev geri aramaları ekleyin.

    ```python
    def reboot_listener(client):
        while True:
            # Receive the direct method request
            method_request = client.receive_method_request("rebootDevice")  # blocking call

            # Act on the method by rebooting the device...
            print( "Rebooting device" )
            time.sleep(20)
            print( "Device rebooted")

            # ...and patching the reported properties
            current_time = str(datetime.datetime.now())
            reported_props = {"rebootTime": current_time}
            client.patch_twin_reported_properties(reported_props)
            print( "Device twins updated with latest rebootTime")

            # Send a method response indicating the method request was resolved
            resp_status = 200
            resp_payload = {"Response": "This is the response from the device"}
            method_response = MethodResponse(method_request.request_id, resp_status, resp_payload)
            client.send_method_response(method_response)
    ```

6. Doğrudan yöntem dinleyicisini başlatın ve bekleyin.

    ```python
    def iothub_client_init():
        client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)
        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            # Start a thread listening for "rebootDevice" direct method invocations
            reboot_listener_thread = threading.Thread(target=reboot_listener, args=(client,))
            reboot_listener_thread.daemon = True
            reboot_listener_thread.start()

            while True:
                time.sleep(1000)

        except KeyboardInterrupt:
            print ( "IoTHubDeviceClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "IoTHubDeviceClient waiting for commands, press Ctrl-C to exit" )

        iothub_client_sample_run()
    ```

7. **dmpatterns_getstarted_device.py** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkeleri (üstel geri tepme gibi) uygulamanız gerekir.

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak aygıtta uzaktan yeniden başlatmayı tetikleme

Bu bölümde, doğrudan bir yöntem kullanarak bir aygıtta uzaktan yeniden başlatma başlatan bir Python konsol uygulaması oluşturursunuz. Uygulama, söz sözleşme nin son yeniden başlatma saatini bulmak için cihaz ikiz sorgularını kullanır.

1. Komut isteminizde **azure-iot-hub** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iot-hub
    ```

2. Bir metin düzenleyicisi kullanarak, çalışma dizininizde **dmpatterns_getstarted_service.py** adlı bir dosya oluşturun.

3. `import` **dmpatterns_getstarted_service.py** dosyasının başında aşağıdaki ifadeleri ekleyin.

    ```python
    import sys, time

    from azure.iot.hub import IoTHubRegistryManager
    from azure.iot.hub.models import CloudToDeviceMethod, CloudToDeviceMethodResult, Twin
    ```

4. Aşağıdaki değişken bildirimleri ekleyin. `{IoTHubConnectionString}` Yer tutucu [değerini, IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)daha önce kopyaladığınız IoT hub bağlantı dizesiyle değiştirin. Yer `{deviceId}` tutucu [değerini, IoT hub'ına yeni bir aygıt kaydedin'de](#register-a-new-device-in-the-iot-hub)kaydettiğiniz aygıt kimliğiyle değiştirin.

    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

5. Hedef aygıtı yeniden başlatmak için aygıt yöntemini çağırmak için aşağıdaki işlevi ekleyin, ardından aygıt ikizlerini sorgulayın ve son yeniden başlatma süresini alın.

    ```python
    def iothub_devicemethod_sample_run():
        try:
            # Create IoTHubRegistryManager
            registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

            print ( "" )
            print ( "Invoking device to reboot..." )

            # Call the direct method.
            deviceMethod = CloudToDeviceMethod(method_name=METHOD_NAME, payload=METHOD_PAYLOAD)
            response = registry_manager.invoke_device_method(DEVICE_ID, deviceMethod)

            print ( "" )
            print ( "Successfully invoked the device to reboot." )

            print ( "" )
            print ( response.payload )

            while True:
                print ( "" )
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    twin_info = registry_manager.get_twin(DEVICE_ID)

                    if twin_info.properties.reported.get("rebootTime") != None :
                        print ("Last reboot time: " + twin_info.properties.reported.get("rebootTime"))
                    else:
                        print ("Waiting for device to report last reboot time...")

                    time.sleep(5)
                    status_counter += 1

        except Exception as ex:
            print ( "" )
            print ( "Unexpected error {0}".format(ex) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceManagement Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicemethod_sample_run()
    ```

6. **dmpatterns_getstarted_service.py** dosyasını kaydedin ve kapatın.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık uygulamaları çalıştırmaya hazırsınız.

1. Komut isteminde, doğrudan yeniden başlatma yöntemini dinlemeye başlamak için aşağıdaki komutu çalıştırın.

    ```cmd/sh
    python dmpatterns_getstarted_device.py
    ```

2. Başka bir komut isteminde, uzaktan yeniden başlatmayı tetiklemek için aşağıdaki komutu çalıştırın ve aygıt ikizinin son yeniden başlatma süresini bulması için sorguyı çalıştırın.

    ```cmd/sh
    python dmpatterns_getstarted_service.py
    ```

3. Konsolda doğrudan yönteme cihaz yanıtını görürsünüz.

   Aşağıdaki, aygıtın doğrudan yeniden başlatma yöntemine verdiği yanıtı gösterir:

   ![Simüle cihaz uygulaması çıktısı](./media/iot-hub-python-python-device-management-get-started/device.png)

   Aşağıdaki, hizmetin doğrudan yeniden başlatma yöntemini çağırdığını ve durum için aygıt ikizini yoklamasını gösterir:

   ![Yeniden başlatma hizmeti çıktısını tetikleme](./media/iot-hub-python-python-device-management-get-started/service.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]