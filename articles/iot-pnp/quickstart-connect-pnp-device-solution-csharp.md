---
title: Azure IoT çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme aygıtıyla etkileşimde olun | Microsoft Dokümanlar
description: Azure IoT çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme aygıtına bağlanmak ve bunlarla etkileşimde kalmak için C# (.NET) kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/30/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 0953f68839217c1c75eb86f8399ce023f3863ab4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76963980"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-c"></a>Quickstart: Çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme cihazıyla etkileşimkurun (C#)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Tak ve Çalıştır Önizleme, altta yatan aygıt uygulaması hakkında bilgi sahibi olmadan bir aygıtın yetenekleriyle etkileşimkurmanızı sağlayarak IoT'yi kolaylaştırır. Bu hızlı başlangıç, çözümünüze bağlı bir IoT Tak ve Çalıştır aygıtına bağlanmak ve bunları kontrol etmek için C# (.NET ile) nasıl kullanacağınızı gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için geliştirme makinenize .NET Core (2.x.x veya 3.x.x) yüklemeniz gerekir. .NET Core SDK'nın tercih ettiğiniz sürümünü [download .NET Core'dan](https://dotnet.microsoft.com/download/dotnet-core/)birden fazla platform için indirebilirsiniz.

Geliştirme makinenizdeki .NET sürümünü yerel bir terminal penceresinde aşağıdaki komutu çalıştırarak doğrulayabilirsiniz: 

```cmd/sh
dotnet --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub'ınız için _IoT hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak için not):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Örnek aygıtı çalıştırma

Bu hızlı başlatmada, C# ile IoT Tak ve Çalıştır cihazı olarak yazılmış örnek bir çevre sensörü kullanırsınız. Aşağıdaki yönergeler, aygıtı nasıl yükleyip çalıştıracağınıznızı gösterir:

1. Seçtiğiniz dizinde bir terminal penceresi açın. [C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp) GitHub deposunu bu konuma klonlamak için aşağıdaki komutu uygulayın:

    ```cmd/sh
    git clone https://github.com/Azure-Samples/azure-iot-samples-csharp
    ```

1. Bu terminal penceresi artık _aygıt_ terminaliniz olarak kullanılacaktır. Klonlanmış deponuzun klasörüne gidin ve **/azure-iot-samples-csharp/digitaltwin/Sample/device/EnvironmentalSensorSample** klasörüne gidin.

1. _Aygıt bağlantı dizesini_yapılandır:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Gerekli paketleri oluşturun ve aşağıdaki komutla örneği çalıştırın:

    ```cmd\sh
        dotnet run
    ```

1. Aygıtın başarıyla kaydolduğunu ve buluttan güncelleştirmeler beklediğini belirten iletiler görürsünüz. Bu, aygıtın artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu ve hub'a telemetri verilerini göndermeye başladığını gösterir. Bu terminali kapatmayın, servis örneklerinin de çalıştığını doğrulamak için daha sonra ihtiyacınız olacak.

## <a name="run-the-sample-solution"></a>Örnek çözümçalıştırma

Bu hızlı başlatmada, örnek aygıtla etkileşim kurmak için C#'da örnek bir IoT çözümü kullanırsınız.

1. Başka bir terminal penceresi açın (bu sizin _servis_ terminaliniz olacak). Klonlanmış deponuzun klasörüne gidin ve **/azure-iot-samples-csharp/digitaltwin/Samples/service** klasörüne gidin.

1. _IoT hub bağlantı dizesini_ ve _aygıt kimliğini,_ hizmetin her ikisine de bağlanmasına izin verecek şekilde yapılandırın:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    set DEVICE_ID=<YourDeviceID>
    ```

### <a name="read-a-property"></a>Bir özelliği okuma

1. _Aygıtı_ terminaline bağladığınızda, çevrimiçi durumunu belirten aşağıdaki iletiyi gördünuz:

    ```cmd/sh
    Waiting to receive updates from cloud...
    ```

1. _Servis_ terminaline gidin ve aygıtın bilgilerini okumak için örneği çalıştırmak için aşağıdaki komutları kullanın:

    ```cmd/sh
    cd GetDigitalTwin
    dotnet run
    ```

1. _Servis_ terminali çıkışında `environmentalSensor` bileşene gidin. Aygıtın `state` çevrimiçi olup olmadığını belirtmek için kullanılan özelliğin _doğru_olarak raporlandığını görüyorsunuz:

    ```JSON
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "state": {
          "reported": {
            "value": true
          }
        }
      }
    }
    ```

### <a name="update-a-writable-property"></a>Yazılabilir özelliği güncelleştirme

1. _Servis_ terminaline gidin ve hangi özelliğin güncelleştirileni tanımlamak için aşağıdaki değişkenleri ayarlayın:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set PROPERTY_NAME=brightness
    set PROPERTY_VALUE=42
    ```

1. Özelliği güncelleştirmek için örneği çalıştırmak için aşağıdaki komutları kullanın:

    ```cmd/sh
    cd ..\UpdateProperty
    dotnet run
    ```

1. _Servis_ terminali çıktısı güncelleştirilmiş aygıt bilgilerini gösterir. 42'nin yeni parlaklık değerini görmek için `environmentalSensor` bileşene gidin.

    ```json
        "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "desired": {
            "value": "42"
          }
        },
        "state": {
          "reported": {
            "value": true
          }
        }
      }
    }
    ```

1. _Aygıt_ terminalinize gidin, aygıtın güncelleştirmeyi aldığını görürsünüz:

    ```cmd/sh
    Received updates for property 'brightness'
    Desired brightness = '"42"'.
    Reported brightness = ''.
    Version is '2'.
    Sent pending status for brightness property.
    Sent completed status for brightness property.
    ```
2. _Hizmet_ terminalinize geri dön ve özelliğin güncelleştirildiğini doğrulamak için cihaz bilgilerini yeniden almak için aşağıdaki komutları çalıştırın.
    
    ```cmd/sh
    cd ..\GetDigitalTwin
    dotnet run
    ```
3. _Hizmet_ terminali çıkışında, `environmentalSensor` bileşenin altında, güncelleştirilmiş parlaklık değerinin raporlandığını görürsünüz. Not: Aygıtın güncelleştirmeyi tamamlaması biraz zaman alabilir. Aygıt özellik güncelleştirmesini gerçekten işleyene kadar bu adımı yineleyebilirsiniz.
    
    ```json
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "desired": {
            "value": "42"
          },
          "reported": {
            "value": "42",
            "desiredState": {
              "code": 200,
              "description": "Request completed",
              "version": 2
            }
          }
        },
        "state": {
          "reported": {
            "value": true
          }
        }
      }
    },
    ```

### <a name="invoke-a-command"></a>Komut çağırma

1. _Servis_ terminaline gidin ve hangi komutu çağıracaklarını belirlemek için aşağıdaki değişkenleri ayarlayın:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set COMMAND_NAME=blink
    ```

1. Komutu çağırmak için örneği çalıştırmak için aşağıdaki komutları kullanın:

    ```cmd/sh
    cd ..\InvokeCommand
    dotnet run
    ```

1. _Servis_ terminalindeki çıktı aşağıdaki onayı göstermelidir:

    ```cmd/sh
    Invoking blink on device <YourDeviceID> with interface instance name environmentalSensor
    Command blink invoked on the device successfully, the returned status was 200 and the request id was <some ID value>
    The returned payload was
    {"description": "abc"}
    Enter any key to finish
    ```

1. _Aygıt_ terminaline gidin, komutun kabul edildiğini görürsünüz:

    ```cmd/sh
    Command - blink was invoked from the service
    Data - null
    Request Id - <some ID value>.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT Tak ve Çalıştır aygıtını IoT çözümüne nasıl bağlayabileceğinizi öğrendiniz. IoT Tak ve Çalıştır aygıtlarınızla etkileşimedebilen bir çözümü nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Nasıl Yapılsın: Aygıta bağlanma ve aygıtla etkileşim kurma](howto-develop-solution.md)
