---
title: Azure IoT Hub modül kimliği ve modülü ikizi (Python) ile çalışmaya başlama | Microsoft Docs
description: Python için IoT SDK 'larını kullanarak modül kimliği oluşturma ve modül ikizi güncelleştirme hakkında bilgi edinin.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: menchi
ms.openlocfilehash: 2c388ff86e782c916916bfb08c7a55ec5c845b13
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667924"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-python"></a>IoT Hub Module kimliği ve modülü ikizi (Python) ile çalışmaya başlama

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md), Azure IoT Hub cihaz kimliğine ve cihaz ikizine benzer, ancak daha hassas ayrıntı düzeyi sağlar. Azure IoT Hub cihaz kimliği ve cihaz ikizi, arka uç uygulamasının bir cihaz yapılandırmasına imkan tanıyıp cihazın koşullarına yönelik görünürlük sağlarken, modül kimliği ve modül ikizi de bir cihazın tek tek bileşenleri için bu özellikleri sağlar. İşletim sistemi tabanlı cihazlar veya üretici yazılımı cihazları gibi, birden fazla bileşen içeren ve bu özelliklere sahip cihazlarda her bir bileşen için yalıtılmış yapılandırma ve koşullara olanak sağlar.
>

Bu öğreticinin sonunda iki Python uygulamanız vardır:

* Cihaz ve modül istemcilerinizi bağlamak için bir cihaz kimliği, bir modül kimliği ve ilişkili güvenlik anahtarı oluşturan **CreateIdentities**.

* Güncelleştirilmiş modül ikizi tarafından raporlanan özelliklerini IoT Hub’ınıza gönderen **UpdateModuleTwinReportedProperties**.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

Önkoşullar için yükleme yönergeleri aşağıda verilmiştir.

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

[!INCLUDE [iot-hub-howto-module-twin-shared-access-policy-text](../../includes/iot-hub-howto-module-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>IoT Hub bir cihaz kimliği ve modül kimliği oluşturma

Bu bölümde, IoT Hub 'ınızdaki kimlik kayıt defterinde bir cihaz kimliği ve bir modül kimliği oluşturan bir Python uygulaması oluşturacaksınız. Kimlik kayıt defterinde girişi olmayan bir cihaz veya modül, IoT hub'ına bağlanamaz. Daha fazla bilgi için, [IoT Hub geliştirici kılavuzunun](iot-hub-devguide-identity-registry.md)"kimlik kayıt defteri" bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, hem cihaz hem de modül için benzersiz bir kimlik ve anahtar oluşturur. Cihazınız ve modülünüz, IoT Hub’ına cihazdan buluta iletileri gönderdiğinde kendisini tanımlamak için bu değerleri kullanır. Kimlikler büyük/küçük harfe duyarlıdır.

Aşağıdaki kodu Python dosyanıza ekleyin:

```python
import sys
import iothub_service_client
from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubError

CONNECTION_STRING = "YourConnString"
DEVICE_ID = "myFirstDevice"
MODULE_ID = "myFirstModule"

try:
    # RegistryManager
    iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

    # CreateDevice
    primary_key = ""
    secondary_key = ""
    auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
    new_device = iothub_registry_manager.create_device(
        DEVICE_ID, primary_key, secondary_key, auth_method)
    print("new_device <" + DEVICE_ID +
          "> has primary key = " + new_device.primaryKey)

    # CreateModule
    new_module = iothub_registry_manager.create_module(
        DEVICE_ID, primary_key, secondary_key, MODULE_ID, auth_method)
    print("device/new_module <" + DEVICE_ID + "/" + MODULE_ID +
          "> has primary key = " + new_module.primaryKey)

except IoTHubError as iothub_error:
    print("Unexpected error {0}".format(iothub_error))
except KeyboardInterrupt:
    print("IoTHubRegistryManager sample stopped")
```

Bu uygulama, Myfirstdevice KIMLIĞI ile bir cihaz kimliği ve myfirstdevice adlı cihaz altında **myfirstmodule** kimliğiyle bir modül kimliği **oluşturur.** (Bu modül kimliği, kimlik kayıt defterinde zaten varsa, kod yalnızca mevcut modül bilgilerini alır.) Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. IoT hub'ınıza bağlanmak için sanal modül uygulamasında bu anahtarı kullanırsınız.

> [!NOTE]
> IoT Hub kimlik kayıt defteri yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz ve modül kimliklerini depolar. Kimlik kayıt defteri, cihaz kimliklerini ve anahtarlarını güvenlik kimlik bilgileri olarak kullanmak için depolar. Kimlik kayıt defterinin her cihaz için depoladığı etkin/devre dışı bayrağını kullanarak, ilgili cihaza erişimi devre dışı bırakabilirsiniz. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Modül kimlikleri için etkin/devre dışı bayrağı yoktur. Daha fazla bilgi için bkz. [IoT Hub Geliştirici Kılavuzu](iot-hub-devguide-identity-registry.md).
>

## <a name="update-the-module-twin-using-python-device-sdk"></a>Python cihaz SDK 'sını kullanarak modül ikizi güncelleştirme

Bu bölümde, sanal cihazınızda modül ikizi bildirilen özelliklerini güncelleştiren bir Python uygulaması oluşturacaksınız.

1. **Modül Bağlantı dizenizi alın** -- [Azure Portal](https://portal.azure.com/)oturum açarsanız şimdi. IoT Hub’ınıza gidin ve IoT Cihazları’na tıklayın. MyFirstDevice ' ı bulun, açın ve myFirstModule başarıyla oluşturulduğunu görürsünüz. Modül bağlantı dizesini kopyalayın. Sonraki adımda gerekecektir.

   ![Azure portalı modül ayrıntısı](./media/iot-hub-python-python-module-twin-getstarted/module-detail.png)

2. **Updatemodületwinreportedproperties uygulaması oluşturma**

   Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubDeviceTwin, IoTHubError

    CONNECTION_STRING = "FILL IN CONNECTION STRING"
    DEVICE_ID = "MyFirstDevice"
    MODULE_ID = "MyFirstModule"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"telemetryInterval\":122}}}"

    try:
        iothub_twin = IoTHubDeviceTwin(CONNECTION_STRING)

        twin_info = iothub_twin.get_twin(DEVICE_ID, MODULE_ID)
        print ( "" )
        print ( "Twin before update    :" )
        print ( "{0}".format(twin_info) )

        twin_info_updated = iothub_twin.update_twin(DEVICE_ID, MODULE_ID, UPDATE_JSON)
        print ( "" )
        print ( "Twin after update     :" )
        print ( "{0}".format(twin_info_updated) )

    except IoTHubError as iothub_error:
        print ( "Unexpected error {0}".format(iothub_error) )
    except KeyboardInterrupt:
        print ( "IoTHubRegistryManager sample stopped" )
    ```

Bu kod örneği, AMQP protokolüyle raporlanan özellikleri güncelleştirme ve modül ikizini alma işlemini nasıl yapacağınızı gösterir.

## <a name="get-updates-on-the-device-side"></a>Cihaz tarafında güncelleştirmeleri al

Yukarıdaki koda ek olarak, cihazınızda ikizi Update iletisini almak için aşağıdaki kod bloğunu ekleyebilirsiniz.

```python
import random
import time
import sys
import iothub_client
from iothub_client import IoTHubModuleClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult

PROTOCOL = IoTHubTransportProvider.AMQP
CONNECTION_STRING = ""


def module_twin_callback(update_state, payload, user_context):
    print("")
    print("Twin callback called with:")
    print("updateStatus: %s" % update_state)
    print("context: %s" % user_context)
    print("payload: %s" % payload)


try:
    module_client = IoTHubModuleClient(CONNECTION_STRING, PROTOCOL)
    module_client.set_module_twin_callback(module_twin_callback, 1234)

    print("Waiting for incoming twin messages.  Hit Control-C to exit.")
    while True:

        time.sleep(1000000)

except IoTHubError as iothub_error:
    print("Unexpected error {0}".format(iothub_error))
except KeyboardInterrupt:
    print("module client sample stopped")
```

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihaz yönetimini kullanmaya başlama](iot-hub-node-node-device-management-get-started.md)

* [IoT Edge kullanmaya başlama](../iot-edge/tutorial-simulate-device-linux.md)