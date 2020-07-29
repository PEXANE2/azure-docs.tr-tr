---
title: Azure IoT çözümünüze (Python) bağlı olan IoT Tak ve Kullan önizleme cihazından etkileşime geçin | Microsoft Docs
description: Azure IoT çözümünüze bağlı IoT Tak ve Kullan önizleme cihazına bağlanmak ve bunlarla etkileşim kurmak için Python kullanın.
author: elhorton
ms.author: elhorton
ms.date: 7/13/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: e3f00bb601cce17721c5247941588be1c2de788d
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87353005"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-python"></a>Hızlı başlangıç: çözümünüze bağlı olan IoT Tak ve Kullan önizleme cihazından etkileşim kurma (Python)

[!INCLUDE [iot-pnp-quickstarts-service-selector.md](../../includes/iot-pnp-quickstarts-service-selector.md)]

IoT Tak ve Kullan önizlemesi, temeldeki cihaz uygulamasıyla ilgili bilgi sahibi olmadan bir cihazın modeliyle etkileşim kurmanızı sağlayarak IoT 'yi basitleştirir. Bu hızlı başlangıçta, çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanmak ve bunları denetlemek için Python 'un nasıl kullanılacağı gösterilmektedir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenizde Python 3,7 gerekir. [Python.org](https://www.python.org/)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz. Python sürümünüzü aşağıdaki komutla kontrol edebilirsiniz:  

```cmd/sh
python --version
```

Aşağıdaki komutu çalıştırarak [Python hizmeti SDK 'sı önizleme paketini](https://pypi.org/project/azure-iot-hub/2.2.1rc0/) yükledikten sonra:

```cmd/sh
pip3 install azure-iot-hub==2.2.1rc0
```

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu hızlı başlangıçta kullanacaksınız:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

Hub 'a eklediğiniz cihazın _Cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini, daha sonra bu hızlı başlangıçta kullanacaksınız:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output
```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

Bu hızlı başlangıçta, IoT Tak ve Kullan cihazı olarak Python 'da yazılmış örnek bir termostat cihazı kullanırsınız. Örnek cihazı çalıştırmak için:

1. Seçtiğiniz bir klasörde bir Terminal penceresi açın. [Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python) GitHub deposunu bu konuma kopyalamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

1. Bu terminal penceresi, **cihaz** terminali olarak kullanılır. Klonlanmış deponuzdaki klasöre gidin ve */Azure-iot-SDK-Python/Azure-iot-Device/Samples/PNP* klasörüne gidin.

1. _Cihaz bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set IOTHUB_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Aşağıdaki komutla örnek termostat cihazını çalıştırın:

    ```cmd/sh
    python pnp_thermostat.py
    ```

1. Cihazın bazı bilgileri gönderdiğini ve onun çevrimiçi olduğunu söyleyen iletiler görürsünüz. Bu iletiler, cihazın hub 'a telemetri verileri göndermeyi başladığını ve artık komut ve özellik güncelleştirmelerini almaya hazır olduğunu gösterir. Bu terminali kapatmayın, hizmet örneğinin çalıştığını onaylamanız gerekir.

## <a name="run-the-sample-solution"></a>Örnek çözümü çalıştırma

Bu hızlı başlangıçta, yeni ayarladığınız örnek cihazla etkileşim kurmak için Python 'da örnek bir IoT çözümü kullanırsınız.

1. **Hizmet** terminali olarak kullanmak için başka bir Terminal penceresi açın. Hizmet SDK 'Sı önizlemede olduğundan, [Python SDK 'sının önizleme dalından](https://github.com/Azure/azure-iot-sdk-python/tree/pnp-preview-refresh)örnekleri kopyalamanız gerekir:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-python -b pnp-preview-refresh
    ```

1. Bu kopyalanmış depo dalının klasörüne gidin ve */Azure-iot-SDK-Python/Azure-iot-hub/Samples* klasörüne gidin.

1. Cihaz KIMLIĞINIZ ve _IoT Hub bağlantı dizeniz_için ortam değişkenlerini yapılandırın:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIOTHubConnectionString>
    set IOTHUB_DEVICE_ID=<Your device ID>
    ```

1. Örnekler klasöründe, ön ek içeren dört örnek dosya vardır `pnp` . Bu örnekler, IoT Tak ve Kullan cihazlarıyla etkileşim kurmak için her bir API 'nin nasıl kullanılacağını gösterir:

### <a name="get-digital-twin"></a>Dijital ikizi al

Bu örneği çalıştırmak için **hizmet** terminalinde aşağıdaki komutu kullanın:

```cmd/sh
python pnp_get_digital_twin_sample.py
```

Çıktı, cihazın dijital ikizi gösterir ve model KIMLIĞINI yazdırır:

```cmd/sh
{'$dtId': 'mySimpleThermostat', '$metadata': {'$model': 'dtmi:com:example:Thermostat;1'}}
Model Id: dtmi:com:example:Thermostat;1
```

Aşağıdaki kod parçacığında *pnp_get_digital_twin_sample. Kopyala*dosyasından örnek kod gösterilmektedir:

```python
    # Get digital twin and retrieve the modelId from it
    digital_twin = iothub_digital_twin_manager.get_digital_twin(device_id)
    if digital_twin:
        print(digital_twin)
        print("Model Id: " + digital_twin["$metadata"]["$model"])
    else:
        print("No digital_twin found")
```

### <a name="update-a-digital-twin"></a>Dijital ikizi güncelleştirme

Bu örnekte, cihazınızın dijital ikizi özelliklerini güncelleştirmek için bir *düzeltme ekinin* nasıl kullanılacağı gösterilmektedir. *Pnp_update_digital_twin_sample. Kopyala* 'dan aşağıdaki kod parçacığında düzeltme ekinin nasıl oluşturulacağı gösterilmektedir:

```python
# If you already have a component thermostat1:
# patch = [{"op": "replace", "path": "/thermostat1/targetTemperature", "value": 42}]
patch = [{"op": "add", "path": "/targetTemperature", "value": 42}]
iothub_digital_twin_manager.update_digital_twin(device_id, patch)
print("Patch has been succesfully applied")
```

Bu örneği çalıştırmak için **hizmet** terminalinde aşağıdaki komutu kullanın:

```cmd/sh
python pnp_update_digital_twin_sample.py
```

Aşağıdaki çıktıyı gösteren **cihaz** terminalinde, güncelleştirmenin uygulandığını doğrulayabilirsiniz:

```cmd/sh
the data in the desired properties patch was: {'targetTemperature': 42, '$version': 2}
previous values
42
```

**Hizmet** terminali, düzeltme ekinin başarılı olduğunu onaylar:

```cmd/sh
Patch has been successfully applied
```

### <a name="invoke-a-command"></a>Komut çağırma

Bir komutu çağırmak için *pnp_invoke_command_sample. Kopyala* örneğini çalıştırın. Bu örnek, basit bir termostat cihazında bir komutun nasıl çağıralınacağını gösterir. Bu örneği çalıştırmadan önce, `IOTHUB_COMMAND_NAME` `IOTHUB_COMMAND_PAYLOAD` **hizmet** terminalinde ve ortam değişkenlerini ayarlayın:

```cmd/sh
set IOTHUB_COMMAND_NAME="getMaxMinReport" # this is the relevant command for the thermostat sample
set IOTHUB_COMMAND_PAYLOAD="hello world" # this payload doesn't matter for this sample
```

**Hizmet** terminalinde, örneği çalıştırmak için aşağıdaki komutu kullanın:
  
```cmd/sh
python pnp_invoke_command_sample.py
```

**Hizmet** terminali cihazdan bir onay iletisi gösterir:

```cmd/sh
{"tempReport": {"avgTemp": 34.5, "endTime": "13/07/2020 16:03:38", "maxTemp": 49, "minTemp": 11, "startTime": "13/07/2020 16:02:18"}}
```

**Cihaz** terminalinde, cihazın şu komutu aldığını görürsünüz:

```cmd/sh
Command request received with payload
hello world
Will return the max, min and average temperature from the specified time hello to the current time
Done generating
{"tempReport": {"avgTemp": 34.2, "endTime": "09/07/2020 09:58:11", "maxTemp": 49, "minTemp": 10, "startTime": "09/07/2020 09:56:51"}}
Sent message
```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Tak ve Kullan cihazını IoT çözümüne bağlamayı öğrendiniz. IoT Tak ve Kullan cihaz modelleri hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [IoT Tak ve Kullan Preview modelleme Geliştirici Kılavuzu](concepts-developer-guide.md)
