---
title: Azure IoT çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme aygıtıyla etkileşimde olun | Microsoft Dokümanlar
description: Azure IoT çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme aygıtına bağlanmak ve onlarla etkileşimde kalmak için Java'yı kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 53812e68fe397b81f29869565e0e4a0640a9ef23
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76964643"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-java"></a>Quickstart: Çözümünüze bağlı bir IoT Tak ve Çalıştır Önizleme cihazıyla etkileşimkurun (Java)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Tak ve Çalıştır Önizleme, altta yatan aygıt uygulaması hakkında bilgi sahibi olmadan bir aygıtın yetenekleriyle etkileşimkurmanızı sağlayarak IoT'yi kolaylaştırır. Bu hızlı başlangıç, çözümünüze bağlı bir IoT Tak ve Çalıştır aygıtına bağlanmak ve bunları kontrol etmek için Java'yı nasıl kullanacağınızı gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için geliştirme makinenizde Java SE 8'e ihtiyacınız vardır. Ayrıca Maven 3 yüklemeniz gerekir.

Bunlarla nasıl ayarlanacağınız hakkında ayrıntılı bilgi için, geliştirme ortamınızı Java için Microsoft Azure IoT aygıtı SDK'da [hazırlayın'](https://github.com/Azure/azure-iot-sdk-java/blob/preview/doc/java-devbox-setup.md) a bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub'ınız için _IoT hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak için not):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Örnek aygıtı çalıştırma

Bu hızlı başlatmada, Java'da IoT Tak ve Çalıştır aygıtı olarak yazılmış örnek bir çevre sensörü kullanırsınız. Aşağıdaki yönergeler, aygıtı nasıl yükleyip çalıştıracağınıznızı gösterir:

1. Seçtiğiniz dizinde bir terminal penceresi açın. Java GitHub deposu [için Azure IoT Örneklerini](https://github.com/Azure-Samples/azure-iot-samples-java) bu konuma klonlamak için aşağıdaki komutu uygulayın:

    ```cmd/sh
    git clone https://github.com/Azure-Samples/azure-iot-samples-java
    ```

1. Bu terminal penceresi artık _aygıt_ terminaliniz olarak kullanılacaktır. Klonlanmış deponuzun klasörüne gidin ve **/azure-iot-samples-java/digital-twin/Samples/device/JdkSample** klasörüne gidin. Gerekli kitaplıkları yükleyin ve aşağıdaki komutu çalıştırarak simüle aygıt uygulaması oluşturun:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. _Aygıt bağlantı dizesini_yapılandır:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Örneği aygıt klasöründen çalıştırmak için aşağıdaki komutu çalıştırın.

    ```cmd/sh
    java -jar environmental-sensor-sample\target\environmental-sensor-sample-with-deps.jar
    ```

1. Aygıtın bağlı olduğunu, çeşitli kurulum adımlarını gerçekleştirdiğini ve servis güncelleştirmelerini beklediğini ve ardından telemetri günlüklerini beklediğini belirten iletiler görürsünüz. Bu, aygıtın artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu ve hub'a telemetri verilerini göndermeye başladığını gösterir. Sonraki adımları tamamlarken numunenin çalışmasını engelleyin. Bu terminali kapatmayın, servis örneklerinin de çalıştığını doğrulamak için daha sonra ihtiyacınız olacak.

## <a name="run-the-sample-solution"></a>Örnek çözümçalıştırma

Bu hızlı başlatmada, örnek aygıtla etkileşim kurmak için Java'da örnek bir IoT çözümü kullanırsınız.

1. Başka bir terminal penceresi açın (bu sizin _servis_ terminaliniz olacak). Klonlanmış deponuzun klasörüne gidin ve **/azure-iot-samples-java\digital-twin\Samples\Service\JdkSample** klasörüne gidin.

1. Gerekli kitaplıkları yükleyin ve aşağıdaki komutu çalıştırarak hizmet örneğini oluşturun:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. _IoT hub bağlantı dizesini_ ve _aygıt kimliğini,_ hizmetin her ikisine de bağlanmasına izin verecek şekilde yapılandırın:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    set DEVICE_ID=<YourDeviceID>
    ```

### <a name="read-a-property"></a>Bir özelliği okuma

1. _Aygıtı_ terminaline bağladığınızda, çıktı iletilerinden biri çevrimiçi durumunu belirtmek için aşağıdaki iletiydi. Aygıtın `state` çevrimiçi olup olmadığını belirtmek için kullanılan özellik _doğrudur:_

    ```cmd/sh
    State of environmental sensor was set to true
    ```

1. _Servis_ terminaline gidin ve aygıt bilgilerini okumak için servis örneğini çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    java -jar get-digital-twin/target/get-digital-twin-with-deps.jar
    ```

1. _Servis_ terminali çıkışında `environmentalSensor` bileşene gidin. Mülkün doğru `state` olarak rapor edildiğini _true_görüyorsunuz:
    ```JSON
    "environmentalSensor" : {
      "name" : "environmentalSensor",
      "properties" : {
        "state" : {
          "reported" : {
            "value" : true,
            "desiredState" : null
          },
          "desired" : null
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

1. Özelliği güncelleştirmek için hizmet örneğini çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    java -jar update-digital-twin/target/update-digital-twin-with-deps.jar
    ```

1. _Servis_ terminali çıktısı güncelleştirilmiş aygıt bilgilerini gösterir. 42'nin yeni parlaklık değerini görmek için `environmentalSensor` bileşene gidin.

    ```json
    "environmentalSensor": {
        "name": "environmentalSensor",
        "properties": {
            "brightness": {
                "reported": null,
                "desired": {
                    "value": "42"
                }
            },
    ```

1. _Aygıt_ terminalinize gidin, aygıtın güncelleştirmeyi aldığını görürsünüz:

    ```cmd/sh
    OnPropertyUpdate called: propertyName=brightness, reportedValue=null, desiredVersion=2, desiredValue={"value":"42"}
    Report property: propertyName=brightness, reportedValue={"value":"42"}, desiredVersion=2 was DIGITALTWIN_CLIENT_OK
    ```
2. _Servis_ terminalinize geri dön ve özelliğin güncelleştirildiğini doğrulamak için cihaz bilgilerini yeniden almak için aşağıdaki komutu çalıştırın.
    
    ```cmd/sh
    java -jar get-digital-twin/target/get-digital-twin-with-deps.jar
    ```
3. _Hizmet_ terminali çıkışında, `environmentalSensor` bileşenin altında, güncelleştirilmiş parlaklık değerinin raporlandığını görürsünüz. Not: Aygıtın güncelleştirmeyi tamamlaması biraz zaman alabilir. Aygıt özellik güncelleştirmesini gerçekten işleyene kadar bu adımı yineleyebilirsiniz.
    
    ```json
    "environmentalSensor" : {
      "name" : "environmentalSensor",
      "properties" : {
        "brightness" : {
          "reported" : {
            "value" : {
              "value" : "42"
            },
            "desiredState" : {
              "code" : 200,
              "version" : 2,
              "description" : "OK"
            }
          },
          "desired" : {
            "value" : "42"
          }
        },
        "state" : {
          "reported" : {
            "value" : true,
            "desiredState" : null
          },
          "desired" : null
        }
      }
    },       
    ```

### <a name="invoke-a-command"></a>Komut çağırma

1. _Servis_ terminaline gidin ve hangi komutu çağıracaklarını belirlemek için aşağıdaki değişkenleri ayarlayın:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set COMMAND_NAME=blink
    set PAYLOAD=10
    ```

1. Komutu çağırmak için hizmet örneğini çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    java -jar invoke-digital-twin-command/target/invoke-digital-twin-command-with-deps.jar
    ```

1. _Servis_ terminalindeki çıktı aşağıdaki onayı göstermelidir:

    ```cmd/sh
    Invoking blink on device <YourDeviceID> with interface instance name environmentalSensor
    Command invoked on the device successfully, the returned status was 200 and the request id was <some ID value>
    The returned PAYLOAD was
    ```

1. _Aygıt_ terminaline gidin, komutun kabul edildiğini görürsünüz:

    ```cmd/sh
    OnCommandReceived called: commandName=blink, requestId=<some ID value>, commandPayload="10"
    EnvironmentalSensor is blinking every 10 seconds.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT Tak ve Çalıştır aygıtını IoT çözümüne nasıl bağlayabileceğinizi öğrendiniz. IoT Tak ve Çalıştır aygıtlarınızla etkileşimedebilen bir çözümü nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Nasıl Yapılsın: Aygıta bağlanma ve aygıtla etkileşim kurma](howto-develop-solution.md)
