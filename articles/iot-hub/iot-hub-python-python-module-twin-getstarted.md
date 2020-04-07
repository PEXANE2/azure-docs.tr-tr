---
title: Azure IoT Hub modülü kimlik ve modül ikizi (Python)
description: Python için IoT SDK'ları kullanarak modül kimliğini nasıl oluşturup modül ikizini nasıl güncelleştireceğimiz öğrenin.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: menchi
ms.openlocfilehash: f846af548913e0cb3e872560e4b8438da306a255
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756973"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-python"></a>IoT Hub modülü kimliği ve modül ikizi (Python) ile başlayın

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md) Azure IoT Hub aygıt kimliklerine ve aygıt ikizlerine benzer, ancak daha ince parçalılık sağlar. Azure IoT Hub aygıt kimlikleri ve aygıt ikizleri bir aygıtı yapılandırmak ve aygıtın koşullarında görünürlük sağlamak için bir arka uç uygulamasına olanak sağlarken, modül kimlikleri ve modül ikizleri bu özellikleri bir aygıtın tek tek bileşenleri için sağlar. İşletim sistemi tabanlı aygıtlar veya firmware aygıtları gibi birden çok bileşeni olan yetenekli aygıtlarda, her bileşen için yalıtılmış yapılandırma ve koşullara izin verirler.
>

Bu eğitimin sonunda üç Python uygulamanız var:

* Aygıt ve modül istemcilerinizi bağlamak için aygıt kimliği, modül kimliği ve ilişkili güvenlik anahtarları oluşturan **CreateModule.**

* **GüncellemeModuleTwinDesiredProperties**, hangi ioT Hub için güncelleştirilmiş modül ikiz istenilen özellikleri gönderir.

* **ReceiveModuleTwinDesiredPropertiesPatch**, modülü ikiz istenilen özellikleri yama alır cihazınızda.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [iot-hub-include-python-v2-installation-notes](../../includes/iot-hub-include-python-v2-installation-notes.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, kimlik kayıt defterine bir aygıt ekleyen ve sonra bu aygıta bir modül ekleyen bir arka uç hizmeti oluşturursunuz. Bu **hizmet, kayıt defteri yazma** izni gerektirir (kayıt **defteri okumayı**da içerir). Ayrıca, yeni oluşturulan modül için modül ikizine istenen özellikleri ekleyen bir hizmet oluşturursunuz. Bu hizmetin **hizmet bağlama** iznine ihtiyacı vardır. Bu izinleri tek tek veren varsayılan paylaşılan erişim ilkeleri olsa da, bu bölümde, bu izinlerin her ikisini de içeren özel bir paylaşılan erişim ilkesi oluşturursunuz.

[!INCLUDE [iot-hub-include-find-service-regrw-connection-string](../../includes/iot-hub-include-find-service-regrw-connection-string.md)]

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>IoT Hub'da aygıt kimliği ve modül kimliği oluşturma

Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde aygıt kimliği ve modül kimliği oluşturan bir Python hizmet uygulaması oluşturursunuz. Bir aygıt veya modül, kimlik kayıt defterinde bir giriş olmadığı sürece IoT hub'ına bağlayamaz. Daha fazla bilgi için Bkz. [IoT hub'ınızdaki kimlik kaydını anlayın.](iot-hub-devguide-identity-registry.md) Bu konsol uygulamasını çalıştırdığınızda, hem cihaz hem de modül için benzersiz bir kimlik ve anahtar oluşturur. Cihazınız ve modülünüz, IoT Hub’ına cihazdan buluta iletileri gönderdiğinde kendisini tanımlamak için bu değerleri kullanır. Kimlikler büyük/küçük harfe duyarlıdır.

1. Komut isteminizde **azure-iot-hub** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iot-hub
    ```

1. Komut **isteminizde, msrest** paketini yüklemek için aşağıdaki komutu çalıştırın. **HTTPOperationError** özel durumlarını yakalamak için bu pakete ihtiyacınız vardır.

    ```cmd/sh
    pip install msrest
    ```

1. Metin düzenleyicisi kullanarak, çalışma dizininizde **CreateModule.py** adlı bir dosya oluşturun.

1. Python dosyanıza aşağıdaki kodu ekleyin. *YourIoTHubConnectionString'i IoT* hub [bağlantı dizesini al'da](#get-the-iot-hub-connection-string)kopyala bağlantı dizesiyle değiştirin.

    ```python
    import sys
    from msrest.exceptions import HttpOperationError
    from azure.iot.hub import IoTHubRegistryManager

    CONNECTION_STRING = "YourIotHubConnectionString"
    DEVICE_ID = "myFirstDevice"
    MODULE_ID = "myFirstModule"

    try:
        # RegistryManager
        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

        try:
            # CreateDevice - let IoT Hub assign keys
            primary_key = ""
            secondary_key = ""
            device_state = "enabled"
            new_device = iothub_registry_manager.create_device_with_sas(
                DEVICE_ID, primary_key, secondary_key, device_state
            )
        except HttpOperationError as ex:
            if ex.response.status_code == 409:
                # 409 indicates a conflict. This happens because the device already exists.
                new_device = iothub_registry_manager.get_device(DEVICE_ID)
            else:
                raise

        print("device <" + DEVICE_ID +
              "> has primary key = " + new_device.authentication.symmetric_key.primary_key)

        try:
            # CreateModule - let IoT Hub assign keys
            primary_key = ""
            secondary_key = ""
            managed_by = ""
            new_module = iothub_registry_manager.create_module_with_sas(
                DEVICE_ID, MODULE_ID, managed_by, primary_key, secondary_key
            )
        except HttpOperationError as ex:
            if ex.response.status_code == 409:
                # 409 indicates a conflict. This happens because the module already exists.
                new_module = iothub_registry_manager.get_module(DEVICE_ID, MODULE_ID)
            else:
                raise

        print("device/module <" + DEVICE_ID + "/" + MODULE_ID +
              "> has primary key = " + new_module.authentication.symmetric_key.primary_key)

    except Exception as ex:
        print("Unexpected error {0}".format(ex))
    except KeyboardInterrupt:
        print("IoTHubRegistryManager sample stopped")
    ```

1. Komut isteminizde aşağıdaki komutu çalıştırın:

    ```cmd/sh
    python CreateModule.py
    ```

Bu uygulama kimliği **myFirstDevice** ile bir cihaz kimliği ve cihaz **myFirstDevice**altında kimliği **myFirstModule** ile bir modül kimliği oluşturur. (Aygıt veya modül kimliği kimlik kayıt defterinde zaten varsa, kod yalnızca varolan aygıt veya modül bilgilerini alır.) Uygulama, her kimlik için kimliği ve birincil anahtarı görüntüler.

> [!NOTE]
> IoT Hub kimlik kayıt defteri yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz ve modül kimliklerini depolar. Kimlik kayıt defteri, cihaz kimliklerini ve anahtarlarını güvenlik kimlik bilgileri olarak kullanmak için depolar. Kimlik kayıt defterinin her cihaz için depoladığı etkin/devre dışı bayrağını kullanarak, ilgili cihaza erişimi devre dışı bırakabilirsiniz. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Modül kimlikleri için etkin/devre dışı bayrağı yoktur. Daha fazla bilgi için Bkz. [IoT hub'ınızdaki kimlik kaydını anlayın.](iot-hub-devguide-identity-registry.md)
>

## <a name="update-the-module-twin-using-python-service-sdk"></a>Python hizmeti SDK kullanarak modül ikizini güncelleştirin

Bu bölümde, modülü ikiz istenen özellikleri güncelleyen bir Python hizmet uygulaması oluşturursunuz.

1. Komut isteminizde **azure-iot-hub** paketini yüklemek için aşağıdaki komutu çalıştırın. **Azure-iot-hub** paketini önceki bölüme yüklediyseniz bu adımı atlayabilirsiniz.

    ```cmd/sh
    pip install azure-iot-hub
    ```

1. Metin düzenleyicisi kullanarak, çalışma dizininizde **UpdateModuleTwinDesiredProperties.py** adlı bir dosya oluşturun.

1. Python dosyanıza aşağıdaki kodu ekleyin. *YourIoTHubConnectionString'i IoT* hub [bağlantı dizesini al'da](#get-the-iot-hub-connection-string)kopyala bağlantı dizesiyle değiştirin.

    ```python
    import sys
    from azure.iot.hub import IoTHubRegistryManager
    from azure.iot.hub.models import Twin, TwinProperties

    CONNECTION_STRING = "YourIoTHubConnectionString"
    DEVICE_ID = "myFirstDevice"
    MODULE_ID = "myFirstModule"

    try:
        # RegistryManager
        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

        module_twin = iothub_registry_manager.get_module_twin(DEVICE_ID, MODULE_ID)
        print ( "" )
        print ( "Module twin properties before update    :" )
        print ( "{0}".format(module_twin.properties) )

        # Update twin
        twin_patch = Twin()
        twin_patch.properties = TwinProperties(desired={"telemetryInterval": 122})
        updated_module_twin = iothub_registry_manager.update_module_twin(
            DEVICE_ID, MODULE_ID, twin_patch, module_twin.etag
        )
        print ( "" )
        print ( "Module twin properties after update     :" )
        print ( "{0}".format(updated_module_twin.properties) )

    except Exception as ex:
        print ( "Unexpected error {0}".format(ex) )
    except KeyboardInterrupt:
        print ( "IoTHubRegistryManager sample stopped" )
    ```

## <a name="get-updates-on-the-device-side"></a>Cihaz tarafında güncellemeler alın

Bu bölümde, modülün istenen özelliklerin cihazınızda güncelliğini sağlamak için bir Python uygulaması oluşturursunuz.

1. Modül bağlantı dizenizi alın. [Azure portalında,](https://portal.azure.com/)IoT Hub'ınıza gidin ve sol bölmedeki **IoT aygıtlarını** seçin. Aygıtlar listesinden **myFirstDevice'ı** seçin ve açın. **Modül kimlikleri** **altında, myFirstModule'i**seçin. Modül bağlantı dizesini kopyalayın. Bir sonraki adımda ihtiyacınız var.

   ![Azure portalı modül ayrıntısı](./media/iot-hub-python-python-module-twin-getstarted/module-detail.png)

1. Komut isteminizde **azure-iot-device** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    pip install azure-iot-device
    ```

1. Metin düzenleyicisi kullanarak, çalışma dizininizde **ReceiveModuleTwinDesiredPropertiesPatch.py** adlı bir dosya oluşturun.

1. Python dosyanıza aşağıdaki kodu ekleyin. *YourModuleConnectionString'i* adım 1'de kopyaladığınız modül bağlantı dizesiyle değiştirin.

    ```python
    import time
    import threading
    from azure.iot.device import IoTHubModuleClient

    CONNECTION_STRING = "YourModuleConnectionString"


    def twin_update_listener(client):
        while True:
            patch = client.receive_twin_desired_properties_patch()  # blocking call
            print("")
            print("Twin desired properties patch received:")
            print(patch)

    def iothub_client_sample_run():
        try:
            module_client = IoTHubModuleClient.create_from_connection_string(CONNECTION_STRING)

            twin_update_listener_thread = threading.Thread(target=twin_update_listener, args=(module_client,))
            twin_update_listener_thread.daemon = True
            twin_update_listener_thread.start()

            while True:
                time.sleep(1000000)

        except KeyboardInterrupt:
            print("IoTHubModuleClient sample stopped")


    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "IoTHubModuleClient waiting for commands, press Ctrl-C to exit" )

        iothub_client_sample_run()
    ```

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Bu bölümde, **ReceiveModuleTwinDesiredPropertiesPatch** cihaz uygulamasını çalıştırın ve modülünüzün istenilen özelliklerini güncellemek için **UpdateModuleTwinDesiredProperties** hizmet uygulamasını çalıştırın.

1. Komut istemini açın ve aygıt uygulamasını çalıştırın:

    ```cmd/sh
    python ReceiveModuleTwinDesiredPropertiesPatch.py
    ```

   ![Cihaz uygulaması ilk çıktısı](./media/iot-hub-python-python-module-twin-getstarted/device-1.png)

1. Ayrı bir komut istemi açın ve hizmet uygulamasını çalıştırın:

    ```cmd/sh
    python UpdateModuleTwinDesiredProperties.py
    ```

    **TelemetryInterval** istenilen özelliğin, hizmet uygulama çıktınızda güncelleştirilmiş modül ikizinde göründüğüne dikkat edin:

   ![Hizmet uygulaması çıktısı](./media/iot-hub-python-python-module-twin-getstarted/service.png)

    Aynı özellik, cihazınızın uygulama çıktısında alınan istenen özellikler yamasında görünür:

   ![Cihaz uygulaması çıktısı istenilen özellikleri gösterir yama](./media/iot-hub-python-python-module-twin-getstarted/device-2.png)

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihaz yönetimini kullanmaya başlama](iot-hub-node-node-device-management-get-started.md)

* [IoT Edge ile çalışmaya başlama](../iot-edge/tutorial-simulate-device-linux.md)