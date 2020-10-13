---
title: Azure IoT çözümünüze (Python) bağlı olan IoT Tak ve Kullan cihazından etkileşim kurma | Microsoft Docs
description: Azure IoT çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanmak ve bunlarla etkileşim kurmak için Python kullanın.
author: elhorton
ms.author: elhorton
ms.date: 10/05/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: d04a1eda7dc414233075f5d70e29c967c8bdfc35
ms.sourcegitcommit: ba7fafe5b3f84b053ecbeeddfb0d3ff07e509e40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91946085"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-device-thats-connected-to-your-solution-python"></a>Hızlı başlangıç: çözümünüze bağlı olan IoT Tak ve Kullan cihazla etkileşim kurma (Python)

[!INCLUDE [iot-pnp-quickstarts-service-selector.md](../../includes/iot-pnp-quickstarts-service-selector.md)]

IoT Tak ve Kullan, temeldeki cihaz uygulamasıyla ilgili bilgi sahibi olmadan bir cihazın modeliyle etkileşim kurmanızı sağlayarak IoT 'yi basitleştirir. Bu hızlı başlangıçta, çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanmak ve bunları denetlemek için Python 'un nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenizde Python 3,7 gerekir. [Python.org](https://www.python.org/)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz. Python sürümünüzü aşağıdaki komutla kontrol edebilirsiniz:  

```cmd/sh
python --version
```

**Azure-IoT-Device** paketi bir PIP olarak yayımlandı.

Yerel Python ortamınızda paketini aşağıdaki gibi yüklemelisiniz:

```cmd/sh
pip install azure-iot-device
```

Aşağıdaki komutu çalıştırarak **Azure-IoT-Hub** paketini takın:

```cmd/sh
pip install azure-iot-hub
```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

Örnek yapılandırma hakkında daha fazla bilgi edinmek için bkz. [örnek Readme](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/pnp/README.md).

Bu hızlı başlangıçta, IoT Tak ve Kullan cihazı olarak Python 'da yazılmış örnek bir termostat cihazı kullanırsınız. Örnek cihazı çalıştırmak için:

1. Seçtiğiniz bir klasörde bir Terminal penceresi açın. [Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python) GitHub deposunu bu konuma kopyalamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

1. Bu terminal penceresi, **cihaz** terminali olarak kullanılır. Klonlanmış deponuzdaki klasöre gidin ve */Azure-iot-SDK-Python/Azure-iot-Device/Samples/PNP* klasörüne gidin.

1. Aşağıdaki komutla örnek termostat cihazını çalıştırın:

    ```cmd/sh
    python simple_thermostat.py
    ```

1. Cihazın bazı bilgileri gönderdiğini ve onun çevrimiçi olduğunu söyleyen iletiler görürsünüz. Bu iletiler, cihazın hub 'a telemetri verileri göndermeyi başladığını ve artık komut ve özellik güncelleştirmelerini almaya hazır olduğunu gösterir. Bu terminali kapatmayın, hizmet örneğinin çalıştığını onaylamanız gerekir.

## <a name="run-the-sample-solution"></a>Örnek çözümü çalıştırma

Bu hızlı başlangıçta, yeni ayarladığınız örnek cihazla etkileşim kurmak için Python 'da örnek bir IoT çözümü kullanırsınız.

1. **Hizmet** terminali olarak kullanmak için başka bir Terminal penceresi açın.

1. Kopyalanan Python SDK deposunun */Azure-iot-SDK-Python/Azure-iot-hub/Samples* klasörüne gidin.

1. *Registry_manager_pnp_sample. Kopyala* dosyasını açın ve kodu gözden geçirin. Bu örnek, IoT Tak ve Kullan cihazlarınızla etkileşim kurmak için **Iothubregistrymanager** sınıfının nasıl kullanılacağını gösterir.

> [!NOTE]
> Bu hizmet örnekleri, **IoT Hub hizmeti Istemcisinden** **Iothubregistrymanager** sınıfını kullanır. Dijital TWINS API 'SI de dahil olmak üzere API 'Ler hakkında daha fazla bilgi edinmek için bkz. [hizmet Geliştirici Kılavuzu](concepts-developer-guide-service.md).

### <a name="get-the-device-twin"></a>Cihaz ikizi al

Ortamınızı IoT Hub 'ınıza ve cihazınıza bağlanacak şekilde yapılandırmak için, [ıot Tak ve kullan hızlı başlangıçlarını ve öğreticilerini ayarlama](set-up-environment.md) bölümünde iki ortam değişkeni oluşturdunuz:

* **IOTHUB_CONNECTION_STRING**: IoT Hub bağlantı dizesi daha önce bir nota yaptınız.
* **IOTHUB_DEVICE_ID**: `"my-pnp-device"` .

Bu örneği çalıştırmak için **hizmet** terminalinde aşağıdaki komutu kullanın:

```cmd/sh
set IOTHUB_METHOD_NAME="getMaxMinReport"
set IOTHUB_METHOD_PAYLOAD="hello world"
python registry_manager_pnp_sample.py
```

> [!NOTE]
> Bu örneği Linux üzerinde çalıştırıyorsanız, yerine kullanın `export` `set` .

Çıktı, cihazı ikizi gösterir ve onun model KIMLIĞINI yazdırır:

```cmd/sh
The Model ID for this device is:
dtmi:com:example:Thermostat;1
```

Aşağıdaki kod parçacığında *registry_manager_pnp_sample. Kopyala*dosyasından örnek kod gösterilmektedir:

```python
    # Create IoTHubRegistryManager
    iothub_registry_manager = IoTHubRegistryManager(iothub_connection_str)

    # Get device twin
    twin = iothub_registry_manager.get_twin(device_id)
    print("The device twin is: ")
    print("")
    print(twin)
    print("")

    # Print the device's model ID
    additional_props = twin.additional_properties
    if "modelId" in additional_props:
        print("The Model ID for this device is:")
        print(additional_props["modelId"])
        print("")
```

### <a name="update-a-device-twin"></a>Cihaz ikizi güncelleştirme

Bu örnekte, `targetTemperature` cihazdaki yazılabilir özelliği güncelleştirme gösterilmektedir:

```python
    # Update twin
    twin_patch = Twin()
    twin_patch.properties = TwinProperties(
        desired={"targetTemperature": 42}
    )  # this is relevant for the thermostat device sample
    updated_twin = iothub_registry_manager.update_twin(device_id, twin_patch, twin.etag)
    print("The twin patch has been successfully applied")
    print("")
```

Aşağıdaki çıktıyı gösteren **cihaz** terminalinde, güncelleştirmenin uygulandığını doğrulayabilirsiniz:

```cmd/sh
the data in the desired properties patch was: {'targetTemperature': 42, '$version': 2}
```

**Hizmet** terminali, düzeltme ekinin başarılı olduğunu onaylar:

```cmd/sh
The twin patch has been successfully applied
```

### <a name="invoke-a-command"></a>Komut çağırma

Örnek daha sonra bir komutu çağırır:

**Hizmet** terminali cihazdan bir onay iletisi gösterir:

```cmd/sh
The device method has been successfully invoked
```

**Cihaz** terminalinde, cihazın şu komutu aldığını görürsünüz:

```cmd/sh
Command request received with payload
hello world
Will return the max, min and average temperature from the specified time hello to the current time
Done generating
{"tempReport": {"avgTemp": 34.2, "endTime": "09/07/2020 09:58:11", "maxTemp": 49, "minTemp": 10, "startTime": "09/07/2020 09:56:51"}}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Tak ve Kullan cihazını IoT çözümüne bağlamayı öğrendiniz. IoT Tak ve Kullan cihaz modelleri hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [IoT Tak ve Kullan modelleme Geliştirici Kılavuzu](concepts-developer-guide-device-csharp.md)
