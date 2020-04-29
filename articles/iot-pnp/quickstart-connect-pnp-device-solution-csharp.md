---
title: Azure IoT çözümünüze bağlı IoT Tak ve Kullan önizleme cihazından etkileşime geçin | Microsoft Docs
description: C# (.net) kullanarak Azure IoT çözümünüze bağlı IoT Tak ve Kullan önizleme cihazına bağlanın ve bunlarla etkileşime geçin.
author: dominicbetts
ms.author: dobett
ms.date: 12/30/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 0953f68839217c1c75eb86f8399ce023f3863ab4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76963980"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-c"></a>Hızlı başlangıç: çözümünüze bağlı olan IoT Tak ve Kullan önizleme cihazından etkileşim kurma (C#)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Tak ve Kullan önizlemesi, temeldeki cihaz uygulamasıyla ilgili bilgi sahibi olmadan bir cihazın özellikleri ile etkileşim kurmanızı sağlayarak IoT 'yi basitleştirir. Bu hızlı başlangıçta, çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanmak ve bunları denetlemek için C# ' nin (.NET ile) nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenize .NET Core (2. x. x veya 3. x. x) yüklemeniz gerekir. Birden çok platform için .NET Core SDK tercih ettiğiniz sürümünü [.NET Core](https://dotnet.microsoft.com/download/dotnet-core/)' u indirin.

Bir yerel Terminal penceresinde aşağıdaki komutu çalıştırarak geliştirme makinenize ait .NET sürümünü doğrulayabilirsiniz: 

```cmd/sh
dotnet --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak üzere):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

Bu hızlı başlangıçta, IoT Tak ve Kullan cihazı olarak C# dilinde yazılmış bir örnek ortam algılayıcısı kullanırsınız. Aşağıdaki yönergelerde, cihazın nasıl yükleneceği ve çalıştırılacağı gösterilmektedir:

1. Seçtiğiniz dizinde bir Terminal penceresi açın. [C# (.net) GitHub deposunu Azure IoT örnekleri](https://github.com/Azure-Samples/azure-iot-samples-csharp) ' ni bu konuma kopyalamak için aşağıdaki komutu yürütün:

    ```cmd/sh
    git clone https://github.com/Azure-Samples/azure-iot-samples-csharp
    ```

1. Bu terminal penceresi artık _cihaz_ terminalinize göre kullanılacaktır. Klonlanmış deponuzdaki klasöre gidin ve **/Azure-iot-Samples-CSharp/digitaltwin/Samples/Device/EnvironmentalSensorSample** klasörüne gidin.

1. _Cihaz bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Gerekli paketleri oluşturun ve örneği aşağıdaki komutla çalıştırın:

    ```cmd\sh
        dotnet run
    ```

1. Cihazın başarıyla kaydedildiğini ve buluttan güncelleştirme beklediğini belirten iletiler görürsünüz. Bu, cihazın artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu ve hub 'a telemetri verileri göndermeye başlamış olduğunu gösterir. Bu terminali kapatmayın, daha sonra Ayrıca, hizmet örneklerinin da çalıştığını onaylamanız gerekir.

## <a name="run-the-sample-solution"></a>Örnek çözümü çalıştırma

Bu hızlı başlangıçta, örnek cihazla etkileşim kurmak Için C# ' de örnek bir IoT çözümü kullanırsınız.

1. Başka bir Terminal penceresi açın (Bu _hizmet_ terminalinize eklenecektir). Klonlanmış deponuzdaki klasöre gidin ve **/Azure-iot-Samples-CSharp/digitaltwin/Samples/Service** klasörüne gidin.

1. Hizmetin her ikisine de bağlanmasına izin vermek için _IoT Hub bağlantı dizesini_ ve _cihaz kimliğini_ yapılandırın:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    set DEVICE_ID=<YourDeviceID>
    ```

### <a name="read-a-property"></a>Bir özelliği okuyun

1. _Cihazı_ terminalde bağladığınızda, çevrimiçi durumunu belirten aşağıdaki iletiyi gördünüz:

    ```cmd/sh
    Waiting to receive updates from cloud...
    ```

1. _Hizmet_ terminaline gidin ve cihaz bilgilerini okumak üzere örneği çalıştırmak için aşağıdaki komutları kullanın:

    ```cmd/sh
    cd GetDigitalTwin
    dotnet run
    ```

1. _Hizmet_ terminali çıkışında `environmentalSensor` bileşene kaydırın. Cihazın çevrimiçi olup olmadığını `state` göstermek için kullanılan özelliğinin, _doğru_olarak raporlandığını belirten özelliği görürsünüz:

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

### <a name="update-a-writable-property"></a>Yazılabilir bir özelliği güncelleştirme

1. _Hizmet_ terminaline gidin ve güncelleştirilecek özelliği tanımlamak için aşağıdaki değişkenleri ayarlayın:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set PROPERTY_NAME=brightness
    set PROPERTY_VALUE=42
    ```

1. Özelliği güncelleştirmek için örneği çalıştırmak üzere aşağıdaki komutları kullanın:

    ```cmd/sh
    cd ..\UpdateProperty
    dotnet run
    ```

1. _Hizmet_ terminali çıkışı, güncelleştirilmiş cihaz bilgilerini gösterir. 42 'nin yeni `environmentalSensor` parlaklık değerini görmek için bileşene kaydırın.

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

1. _Cihaz terminalinize_ gidin, cihazın güncelleştirmeyi aldığını görürsünüz:

    ```cmd/sh
    Received updates for property 'brightness'
    Desired brightness = '"42"'.
    Reported brightness = ''.
    Version is '2'.
    Sent pending status for brightness property.
    Sent completed status for brightness property.
    ```
2. _Hizmet_ terminalinize geri dönüp, özelliğin güncelleştirildiğini onaylamak için cihaz bilgilerini yeniden almak üzere aşağıdaki komutları çalıştırın.
    
    ```cmd/sh
    cd ..\GetDigitalTwin
    dotnet run
    ```
3. _Hizmet_ terminali çıkışında, `environmentalSensor` bileşen altında, güncelleştirilmiş parlaklık değerinin rapor edilmiş olduğunu görürsünüz. Note: cihazın güncelleştirmeyi tamamlaması biraz zaman alabilir. Cihaz özellik güncelleştirmesini gerçekten işleyene kadar bu adımı tekrarlayabilirsiniz.
    
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

1. _Hizmet_ terminali ' ne gidin ve çağrılacak komutu tanımlamak için aşağıdaki değişkenleri ayarlayın:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set COMMAND_NAME=blink
    ```

1. Komutu çağırmak için örneği çalıştırmak üzere aşağıdaki komutları kullanın:

    ```cmd/sh
    cd ..\InvokeCommand
    dotnet run
    ```

1. _Hizmet_ terminalinde çıktı aşağıdaki onayı göstermelidir:

    ```cmd/sh
    Invoking blink on device <YourDeviceID> with interface instance name environmentalSensor
    Command blink invoked on the device successfully, the returned status was 200 and the request id was <some ID value>
    The returned payload was
    {"description": "abc"}
    Enter any key to finish
    ```

1. _Cihaz_ terminali ' ne gidin, komutun kabul edilmiş olduğunu görürsünüz:

    ```cmd/sh
    Command - blink was invoked from the service
    Data - null
    Request Id - <some ID value>.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Tak ve Kullan cihazını IoT çözümüne bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Nasıl yapılır: bir cihaza bağlanma ve cihazla etkileşim kurma](howto-develop-solution.md)
