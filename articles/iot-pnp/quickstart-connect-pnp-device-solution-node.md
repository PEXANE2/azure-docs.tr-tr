---
title: Azure IoT çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme aygıtıyla etkileşimde olun | Microsoft Dokümanlar
description: Azure IoT çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme aygıtına bağlanmak ve onlarla etkileşimde kalmak için Node.js'yi kullanın.
author: miagdp
ms.author: miag
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 9962763e647faddc5a2179f304aeb3fa8ca256e8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75550749"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-nodejs"></a>Quickstart: Çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme cihazıyla etkileşimkurun (Node.js)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Tak ve Çalıştır Önizleme, altta yatan aygıt uygulaması hakkında bilgi sahibi olmadan bir aygıtın yetenekleriyle etkileşimkurmanızı sağlayarak IoT'yi kolaylaştırır. Bu hızlı başlangıç, çözümünüze bağlı bir IoT Tak ve Çalıştır aygıtına bağlanmak ve bunları kontrol etmek için Node.js'i nasıl kullanacağınızı gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için geliştirme makinenizde Node.js'ye ihtiyacınız vardır. [Nodejs.org](https://nodejs.org)birden fazla platform için önerilen en son sürümü indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub'ınız için _IoT hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak için not):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Örnek aygıtı çalıştırma

Bu hızlı başlatmada, IoT Tak ve Çalıştır aygıtı olarak Düğüm.js ile yazılmış örnek bir çevre sensörü kullanırsınız. Aşağıdaki yönergeler, aygıtı nasıl yükleyip çalıştıracağınıznızı gösterir:

1. Seçtiğiniz dizinde bir terminal penceresi açın. [Node.js](https://github.com/azure-samples/azure-iot-samples-node) GitHub deposu için Azure IoT Örneklerini bu konuma klonlamak için aşağıdaki komutu uygulayın:

    ```cmd/sh
    git clone https://github.com/azure-samples/azure-iot-samples-node
    ```

1. Bu terminal penceresi artık _aygıt_ terminaliniz olarak kullanılacaktır. Klonlanmış deponuzun klasörüne gidin ve **/azure-iot-samples-node/digital-twins/Quickstarts/Device** klasörüne gidin. Aşağıdaki komutu çalıştırarak tüm bağımlılıkları yükleyin:

    ```cmd/sh
    npm install
    ```

1. _Aygıt bağlantı dizesini_yapılandır:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Örneği aşağıdaki komutla çalıştırın:

    ```cmd/sh
    node sample_device.js
    ```

1. Aygıtın bazı bilgiler gönderdiğini ve kendisini çevrimiçi olarak bildirdiğini belirten iletiler görürsünüz. Bu, aygıtın hub'a telemetri verileri göndermeye başladığını ve artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu gösterir. Bu terminali kapatmayın, servis örneklerinin de çalıştığını doğrulamak için daha sonra ihtiyacınız olacak.

## <a name="run-the-sample-solution"></a>Örnek çözümçalıştırma

Bu hızlı başlatmada, örnek aygıtla etkileşim kurmak için Düğüm.js'de örnek bir IoT çözümü kullanırsınız.

1. Başka bir terminal penceresi açın (bu sizin _servis_ terminaliniz olacak). Klonlanmış deponuzun klasörüne gidin ve **/azure-iot-samples-node/digital-twins/Quickstarts/Service** klasörüne gidin. Aşağıdaki komutu çalıştırarak tüm bağımlılıkları yükleyin:

    ```cmd/sh
    npm install
    ```

1. Hizmetin ona bağlanmasına izin verecek _şekilde IoT hub bağlantı dizesini_ yapılandırın:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    ```

### <a name="read-a-property"></a>Bir özelliği okuma

1. _Aygıtı_ terminaline bağladığınızda, çevrimiçi durumunu belirten aşağıdaki iletiyi gördünuz:

    ```cmd/sh
    reported state property as online
    ```

1. **/azure-iot-samples-node/digital-twins/Quickstarts/Service** klasöründe **get_digital_twin.js**dosyasını açın. Yer `<DEVICE_ID_GOES_HERE>` tutucuyu aygıt kimliğinizle değiştirin ve dosyayı kaydedin.

1. _Servis_ terminaline gidin ve aygıtın bilgilerini okumak için örneği çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. _Servis_ terminali çıkışında `environmentalSensor` bileşene gidin. Bu `state` özellik _online_olarak bildirilmiştir bakın:

    ```JSON
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "state": {
          "reported": {
            "value": "online"
          }
        }
      }
    }
    ```

### <a name="update-a-writable-property"></a>Yazılabilir özelliği güncelleştirme

1. Dosyayı **açın update_digital_twin_property.js**.

1. Dosyanın başında, büyük yer tutucularla tanımlanan bir sabit kümesi vardır. Yer `<DEVICE_ID_GOES_HERE>` tutucuyu gerçek aygıt kimliğinizle değiştirin, kalan sabitleri aşağıdaki değerlerle güncelleştirin ve dosyayı kaydedin:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const propertyName = 'brightness';
    const propertyValue = 42;
    ```

1. _Servis_ terminaline gidin ve özelliği güncelleştirmek için örneği çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    node update_digital_twin_property.js
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
            "value": "online"
          }
        }
      }
    }
    ```

1. _Aygıt_ terminalinize gidin, aygıtın güncelleştirmeyi aldığını görürsünüz:

    ```cmd/sh
    Received an update for brightness: 42
    updated the property
    ```
2. _Servis_ terminalinize geri dön ve özelliğin güncelleştirildiğini doğrulamak için cihaz bilgilerini yeniden almak için aşağıdaki komutu çalıştırın.
    
    ```cmd/sh
    node get_digital_twin.js
    ```

3. _Hizmet_ terminali çıkışında, `environmentalSensor` bileşenin altında, güncelleştirilmiş parlaklık değerinin raporlandığını görürsünüz. Not: Aygıtın güncelleştirmeyi tamamlaması biraz zaman alabilir. Aygıt özellik güncelleştirmesini gerçekten işleyene kadar bu adımı yineleyebilirsiniz.
    
    ```json
    "environmentalSensor": {
      "name": "environmentalSensor",
      "properties": {
        "brightness": {
          "reported": {
            "value": "42",
            "desiredState": {
              "code": 200,
              "version": 2,
              "description": "helpful descriptive text"
            }
          },
          "desired": {
            "value": "42"
          }
        },
        "state": {
          "reported": {
            "value": "online"
          }
        }
      }
    }
    ```

### <a name="invoke-a-command"></a>Komut çağırma

1. Dosyayı **invoke_command.js'yi**açın.

1. Dosyanın başında, yer tutucuyu `<DEVICE_ID_GOES_HERE>` gerçek aygıt kimliğinizle değiştirin. Kalan sabitleri aşağıdaki değerlerle güncelleştirin ve ardından dosyayı kaydedin:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const commandName = 'blink';
    const commandArgument = '<For the environmental sensor, this value does not matter. Any string will do.>'; 
    ```

1. _Servis_ terminaline git. Komutu çağırmak için örneği çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    node invoke_command.js
    ```

1. _Servis_ terminalindeki çıktı aşağıdaki onayı göstermelidir:

    ```cmd/sh
    invoking command blink on interface instanceenvironmentalSensor for device <device ID>...
    {
      "result": "helpful response text",
      "statusCode": 200,
      "requestId": "<some ID value>",
      "_response": "helpful response text"
    }
    ```

1. _Aygıt_ terminaline gidin, komutun kabul edildiğini görürsünüz:

    ```cmd/sh
    received command: blink for interfaceInstance: environmentalSensor
    acknowledgement succeeded.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT Tak ve Çalıştır aygıtını IoT çözümüne nasıl bağlayabileceğinizi öğrendiniz. IoT Tak ve Çalıştır aygıtlarınızla etkileşimedebilen bir çözümü nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Nasıl Yapılsın: Aygıta bağlanma ve aygıtla etkileşim kurma](howto-develop-solution.md)
