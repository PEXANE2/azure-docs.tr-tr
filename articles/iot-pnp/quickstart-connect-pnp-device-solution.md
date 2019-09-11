---
title: Azure IoT çözümünüze bağlı IoT Tak ve Kullan önizleme cihazından etkileşime geçin | Microsoft Docs
description: Node. js kullanarak Azure IoT çözümünüze bağlı IoT Tak ve Kullan önizleme cihazına bağlanın ve bunlarla etkileşime geçin.
author: miagdp
ms.author: miag
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: a082e4b7896b317bf2b28971d3693bada95a3445
ms.sourcegitcommit: b8578b14c8629c4e4dea4c2e90164e42393e8064
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70806549"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution"></a>Hızlı Başlangıç: Çözümünüze bağlı olan IoT Tak ve Kullan önizleme cihazından etkileşime geçin

IoT Tak ve Kullan önizlemesi, temeldeki cihaz uygulamasıyla ilgili bilgi sahibi olmadan bir cihazın özellikleri ile etkileşim kurmanızı sağlayarak IoT 'yi basitleştirir. Bu hızlı başlangıçta, Node. js ' nin çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanmak ve bunları denetlemek için nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

[NodeJS.org](https://nodejs.org)adresinden Node. js ' ye indirin ve yükleyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-an-iot-hub"></a>IoT Hub 'ı hazırlama

Ayrıca, bu hızlı başlangıcı tamamlayabilmeniz için Azure aboneliğinizde bir Azure IoT Hub 'ınız olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Genel Önizleme sırasında IoT Tak ve Kullan özellikleri yalnızca **Orta ABD**, **Kuzey Avrupa**ve **Japonya Doğu** bölgelerinde oluşturulan IoT Hub 'larında kullanılabilir.

Azure CLı için Microsoft Azure IoT uzantısını ekleyin:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

IoT Hub 'ınızda cihaz kimliğini oluşturmak için aşağıdaki komutu çalıştırın. **Youriothubname** ve **yourdevice** değerini gerçek adlarınızla değiştirin. Bir IoT Hub yoksa, oluşturmak için [aşağıdaki yönergeleri](../iot-hub/iot-hub-create-using-cli.md) izleyin:

```azurecli-interactive
az iot hub device-identity create --hub-name [YourIoTHubName] --device-id [YourDevice]
```

Yeni kaydettiğiniz cihazın _Cihaz bağlantı dizesini_ almak için aşağıdaki komutları çalıştırın:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name [YourIoTHubName] --device-id [YourDevice] --output table
```

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutları çalıştırın:

```azurecli-interactive
az iot hub show-connection-string --hub-name [YourIoTHubName] --output table
```

## <a name="connect-your-device"></a>Cihazınızı bağlama

Bu hızlı başlangıçta, IoT Tak ve Kullan cihazı olarak Node. js ' de yazılmış bir örnek ortam algılayıcısı kullanırsınız. Aşağıdaki yönergelerde, cihazın nasıl yükleneceği ve çalıştırılacağı gösterilmektedir:

1. GitHub deposunu kopyalayın:

    ```cmd/sh
    git clone https://github.com/azure-samples/azure-iot-samples-node
    ```

1. Bir terminalde, kopyalanmış deponuzdaki kök klasöre gidin, **/Azure-iot-Samples-node/Digital-Twins/QuickStarts/Device** klasörüne gidin ve ardından aşağıdaki komutu çalıştırarak tüm bağımlılıkları yüklemelisiniz:

    ```cmd/sh
    npm install
    ```

1. _Cihaz bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<your device connection string>
    ```

1. Aşağıdaki komutla örneği çalıştırın:

    ```cmd/sh
    node sample_device.js
    ```

1. Cihazın telemetri ve özelliklerini gönderdiğini söyleyen iletiler görürsünüz. Cihaz artık komutları ve özellik güncelleştirmelerini almaya hazırdır. Bu terminali kapatmayın, daha sonra Ayrıca, hizmet örneklerinin da çalıştığını onaylamanız gerekir.

## <a name="build-the-solution"></a>Çözümü derleme

Bu hızlı başlangıçta, örnek cihazla etkileşim kurmak için Node. js ' de örnek bir IoT çözümü kullanırsınız.

1. Başka bir Terminal açın. Klonlanmış deponuzdaki klasöre gidin ve **/Azure-iot-Samples-node/Digital-Twins/QuickStarts/Service** klasörüne gidin. Aşağıdaki komutu çalıştırarak tüm bağımlılıkları yükler:

    ```cmd/sh
    npm install
    ```

1. _Hub bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<your hub connection string>
    ```

### <a name="read-a-property"></a>Bir özelliği okuyun

1. Cihazı terminalde bağladığınızda aşağıdaki iletiyi görürsünüz:

    ```cmd/sh
    reported state property as online
    ```

1. **Get_digital_twin. js**dosyasını açın. `deviceID` Öğesini cihaz Kimliğinizle değiştirin ve dosyayı kaydedin.

1. Hizmet örneğini çalıştırmak için açtığınız terminale gidin ve şu komutu çalıştırın:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. Çıktıda, _Environmentalalgılayıcı_ bileşeni altında, aynı durumun rapor edilmiş olduğunu görürsünüz:

    ```JSON
    reported state property as online
    ```

### <a name="update-a-writable-property"></a>Yazılabilir bir özelliği güncelleştirme

1. **Update_digital_twin_property. js**dosyasını açın.

1. Dosyanın başlangıcında, büyük yertutucuları ile tanımlanmış sabitler kümesi vardır. **DeviceID** 'yi gerçek cihaz Kimliğinizle değiştirin, sabitleri aşağıdaki değerlerle güncelleştirin ve dosyayı kaydedin:

    ```javascript
    const componentName = 'environmentalSensor';
    const propertyName = 'brightness';
    const propertyValue = 60;
    ```

1. Hizmet örneği çalıştırmak için açtığınız terminale gidin ve örneği çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    node update_digital_twin_property.js
    ```

1. Terminalde aygıtınızla ilişkili dijital ikizi bilgilerini görürsünüz. _Environmentalalgılayıcı_bileşenini bulun, yeni parlaklık değeri 60 ' i görürsünüz.

    ```json
        "environmentalSensor": {
        "name": "environmentalSensor",
        "properties": {
          "brightness": {
            "reported": {
              "value": 60,
              "desiredState": {
                "code": 200,
                "version": 14,
                "description": "helpful descriptive text"
              }
            },
            "desired": {
              "value": 60
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

1. _Cihaz terminalinize_ gidin, cihazın güncelleştirmeyi aldığını görürsünüz:

    ```cmd/sh
    Received an update for brightness: 60
    updated the property
    ```
2. _Hizmet_ terminalinize geri dönün, özelliğin güncelleştirildiğini onaylamak için aşağıdaki komutu yeniden çalıştırın.
    
    ```cmd/sh
    node get_digital_twin.js
    ```
3. Çıktıda, Environmentalalgılayıcı bileşeni altında, güncelleştirilmiş parlaklık değerinin rapor edilmiş olduğunu görürsünüz. Note: cihazın güncelleştirmeyi tamamlaması biraz zaman alabilir. Cihaz özellik güncelleştirmesini gerçekten işleyene kadar bu adımı tekrarlayabilirsiniz.
    
    ```json
      "brightness": {
        "reported": {
          "value": 60,
          }
       }
    ```

### <a name="invoke-a-command"></a>Komut çağırma

1. **İnvoke_command. js**dosyasını açın.

1. Dosyanın başlangıcında, `deviceID` değerini gerçek cihaz Kimliğinizle değiştirin. Sabitleri aşağıdaki değerlerle güncelleştirin ve sonra dosyayı kaydedin:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const commandName = 'blink';
    ```

1. Hizmet örneği çalıştırmak için açtığınız terminale gidin. Örneği çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    node invoke_command.js
    ```

1. Terminalde, başarı aşağıdaki çıktı gibi görünür:

    ```cmd/sh
    invoking command blink on component environmentalSensor for device test...
    {
      "result": "helpful response text",
      "statusCode": 200,
      "requestId": "33e536d3-14f7-4105-88f3-629b9933851c",
      "_response": "helpful response text"
    }
    ```

1. _Cihaz_ terminali ' ne gidin, komutun kabul edilmiş olduğunu görürsünüz:

    ```cmd/sh
    received command: blink for component: environmentalSensor
    acknowledgement succeeded.
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki makalelerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta kullandığınız kaynakları koruyabilirsiniz. Aksi takdirde, ek ücretlerden kaçınmak için bu hızlı başlangıç için oluşturduğunuz kaynakları silebilirsiniz.

Hub ve kayıtlı cihazı silmek için Azure CLı 'yı kullanarak aşağıdaki adımları uygulayın:

```azurecli-interactive
az group delete --name <Your group name>
```

Yalnızca IoT Hub kaydettiğiniz cihazı silmek için Azure CLı 'yı kullanarak aşağıdaki adımları uygulayın:

```azurecli-interactive
az iot hub device-identity delete --hub-name [YourIoTHubName] --device-id [YourDevice]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Tak ve Kullan cihazını IoT çözümüne bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Nasıl yapılır: Cihaza bağlanma ve cihazla etkileşim kurma](howto-develop-solution.md)
