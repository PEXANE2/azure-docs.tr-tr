---
title: Çözümünüze bağlı IoT Tak ve Kullan önizleme cihazından etkileşime geçin-Java | Microsoft Docs
description: Azure IoT çözümünüze bağlı IoT Tak ve Kullan önizleme cihazına bağlanmak ve bunlarla etkileşim kurmak için Java kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 43136e1a09cc771f39b92a83ca4c452fc1276091
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87044045"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-java"></a>Hızlı başlangıç: çözümünüze bağlı olan IoT Tak ve Kullan önizleme cihazından etkileşim kurma (Java)

[!INCLUDE [iot-pnp-quickstarts-3-selector.md](../../includes/iot-pnp-quickstarts-3-selector.md)]

IoT Tak ve Kullan önizlemesi, temeldeki cihaz uygulamasıyla ilgili bilgi sahibi olmadan bir cihazın özellikleri ile etkileşim kurmanızı sağlayarak IoT 'yi basitleştirir. Bu hızlı başlangıçta, çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanmak ve bunları denetlemek için Java 'nın nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenizde Java, 8 gerekir. Ayrıca Maven 3 ' ü yüklemeniz gerekir.

Bunlarla nasıl ayarlanacağına ilişkin ayrıntılar için bkz. Java için Microsoft Azure IoT cihaz SDK 'sında [geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-java/blob/preview/doc/java-devbox-setup.md) .

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak üzere):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

Bu hızlı başlangıçta, IoT Tak ve Kullan cihazı olarak Java 'da yazılmış bir örnek ortam algılayıcısı kullanırsınız. Aşağıdaki yönergelerde, cihazın nasıl yükleneceği ve çalıştırılacağı gösterilmektedir:

1. Seçtiğiniz dizinde bir Terminal penceresi açın. Java GitHub deposunun [Azure IoT örneklerini](https://github.com/Azure-Samples/azure-iot-samples-java) bu konuma kopyalamak için aşağıdaki komutu yürütün:

    ```cmd/sh
    git clone https://github.com/Azure-Samples/azure-iot-samples-java
    ```

1. Bu terminal penceresi artık _cihaz_ terminalinize göre kullanılacaktır. Klonlanmış deponuzdaki klasöre gidin ve **/Azure-iot-Samples-Java/Digital-Twin/Samples/Device/JdkSample** klasörüne gidin. Aşağıdaki komutu çalıştırarak gerekli kitaplıkları yükleyip sanal cihaz uygulaması oluşturun:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. _Cihaz bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set DIGITAL_TWIN_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Örneği cihaz klasöründen çalıştırmak için aşağıdaki komutu çalıştırın.

    ```cmd/sh
    java -jar environmental-sensor-sample\target\environmental-sensor-sample-with-deps.jar
    ```

1. Cihazın bağlı olduğunu, çeşitli kurulum adımları gerçekleştirdiğini ve hizmet güncelleştirmelerini beklediğini, ardından telemetri günlüklerini bildiren iletileri görürsünüz. Bu, cihazın artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu ve hub 'a telemetri verileri göndermeye başlamış olduğunu gösterir. Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun. Bu terminali kapatmayın, daha sonra Ayrıca, hizmet örneklerinin da çalıştığını onaylamanız gerekir.

## <a name="run-the-sample-solution"></a>Örnek çözümü çalıştırma

Bu hızlı başlangıçta, örnek cihazla etkileşim kurmak için Java 'da örnek bir IoT çözümü kullanırsınız.

1. Başka bir Terminal penceresi açın (Bu _hizmet_ terminalinize eklenecektir). Klonlanmış deponuzdaki klasöre gidin ve **/Azure-iot-Samples-java\digital-twin\Samples\service\JdkSample** klasörüne gidin.

1. Aşağıdaki komutu çalıştırarak gerekli kitaplıkları yükleyip hizmet örneği oluşturun:

    ```cmd/sh
    mvn clean install -DskipTests
    ```

1. Hizmetin her ikisine de bağlanmasına izin vermek için _IoT Hub bağlantı dizesini_ ve _cihaz kimliğini_ yapılandırın:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIoTHubConnectionString>
    set DEVICE_ID=<YourDeviceID>
    ```

### <a name="read-a-property"></a>Bir özelliği okuyun

1. _Cihazı_ terminalde bağladığınızda, çıkış iletilerinden biri çevrimiçi durumunu göstermek için aşağıdaki mesajdır. `state`Cihazın çevrimiçi olup olmadığını göstermek için kullanılan özelliği, _doğru_olsun:

    ```cmd/sh
    State of environmental sensor was set to true
    ```

1. _Hizmet_ terminali ' ne gidin ve cihaz bilgilerini okumak üzere hizmet örneğini çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    java -jar get-digital-twin/target/get-digital-twin-with-deps.jar
    ```

1. _Hizmet_ terminali çıkışında `environmentalSensor` bileşene kaydırın. `state`Özelliğin _doğru_olarak raporlanmışsa görürsünüz:
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

### <a name="update-a-writable-property"></a>Yazılabilir bir özelliği güncelleştirme

1. _Hizmet_ terminaline gidin ve güncelleştirilecek özelliği tanımlamak için aşağıdaki değişkenleri ayarlayın:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set PROPERTY_NAME=brightness
    set PROPERTY_VALUE=42
    ```

1. Özelliği güncelleştirmek üzere hizmet örneğini çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    java -jar update-digital-twin/target/update-digital-twin-with-deps.jar
    ```

1. _Hizmet_ terminali çıkışı, güncelleştirilmiş cihaz bilgilerini gösterir. `environmentalSensor`42 'nin yeni parlaklık değerini görmek için bileşene kaydırın.

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

1. _Cihaz terminalinize_ gidin, cihazın güncelleştirmeyi aldığını görürsünüz:

    ```cmd/sh
    OnPropertyUpdate called: propertyName=brightness, reportedValue=null, desiredVersion=2, desiredValue={"value":"42"}
    Report property: propertyName=brightness, reportedValue={"value":"42"}, desiredVersion=2 was DIGITALTWIN_CLIENT_OK
    ```
2. _Hizmet_ terminalinize geri dönüp, özelliğin güncelleştirildiğini onaylamak için cihaz bilgilerini yeniden almak üzere aşağıdaki komutu çalıştırın.
    
    ```cmd/sh
    java -jar get-digital-twin/target/get-digital-twin-with-deps.jar
    ```
3. _Hizmet_ terminali çıkışında, bileşen altında, `environmentalSensor` Güncelleştirilmiş parlaklık değerinin rapor edilmiş olduğunu görürsünüz. Note: cihazın güncelleştirmeyi tamamlaması biraz zaman alabilir. Cihaz özellik güncelleştirmesini gerçekten işleyene kadar bu adımı tekrarlayabilirsiniz.
    
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

1. _Hizmet_ terminali ' ne gidin ve çağrılacak komutu tanımlamak için aşağıdaki değişkenleri ayarlayın:
    ```cmd/sh
    set INTERFACE_INSTANCE_NAME=environmentalSensor
    set COMMAND_NAME=blink
    set PAYLOAD=10
    ```

1. Komutu çağırmak için hizmet örneğini çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    java -jar invoke-digital-twin-command/target/invoke-digital-twin-command-with-deps.jar
    ```

1. _Hizmet_ terminalinde çıktı aşağıdaki onayı göstermelidir:

    ```cmd/sh
    Invoking blink on device <YourDeviceID> with interface instance name environmentalSensor
    Command invoked on the device successfully, the returned status was 200 and the request id was <some ID value>
    The returned PAYLOAD was
    ```

1. _Cihaz_ terminali ' ne gidin, komutun kabul edilmiş olduğunu görürsünüz:

    ```cmd/sh
    OnCommandReceived called: commandName=blink, requestId=<some ID value>, commandPayload="10"
    EnvironmentalSensor is blinking every 10 seconds.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Tak ve Kullan cihazını IoT çözümüne bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Nasıl yapılır: bir cihaza bağlanma ve cihazla etkileşim kurma](howto-develop-solution.md)
