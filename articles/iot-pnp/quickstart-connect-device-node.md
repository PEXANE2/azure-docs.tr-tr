---
title: IoT Tak ve Kullan örnek Node.js Cihaz kodunu Azure IoT Hub bağlayın | Microsoft Docs
description: IoT Hub 'ına bağlanan IoT Tak ve Kullan örnek cihaz kodu derlemek ve çalıştırmak için Node.js kullanın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: ericmitt
ms.author: ericmitt
ms.date: 07/10/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc, devx-track-js
ms.openlocfilehash: e9ab4f2639569537b7c5967235a926c567aca0d5
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91576141"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-device-application-to-iot-hub-nodejs"></a>Hızlı başlangıç: bir örnek IoT Tak ve Kullan cihaz uygulamasını IoT Hub 'ye bağlama (Node.js)

[!INCLUDE [iot-pnp-quickstarts-device-selector.md](../../includes/iot-pnp-quickstarts-device-selector.md)]

Bu hızlı başlangıçta örnek bir IoT Tak ve Kullan cihaz uygulaması oluşturma, IoT Hub 'ınıza bağlama ve Azure IoT Explorer aracını kullanarak gönderdiği Telemetriyi görüntüleme gösterilmektedir. Örnek uygulama Node.js yazılır ve Node.js için Azure IoT cihaz SDK 'sına dahildir. Bir çözüm Oluşturucusu, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenizde Node.js gerekir. [NodeJS.org](https://nodejs.org)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
```

## <a name="download-the-code"></a>Kodu indirme

Bu hızlı başlangıçta, Node.js için Azure IoT Hub cihaz SDK 'sını klonlamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

Seçtiğiniz dizinde bir komut istemi açın. Node.jsGitHub deposunun [Microsoft Azure ıOT SDK 'sını ](https://github.com/Azure/azure-iot-sdk-node) bu konuma kopyalamak için aşağıdaki komutu yürütün:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node
```

## <a name="install-required-libraries"></a>Gerekli kitaplıkları yükler

Dahil edilen örnek kodu oluşturmak için cihaz SDK 'sını kullanın. Oluşturduğunuz uygulama, IoT Hub 'ına bağlanan bir cihaza benzetir. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. Yerel bir Terminal penceresinde, kopyalanmış deponuzdaki klasöre gidin ve */Azure-iot-SDK-node/Device/Samples/PNP* klasörüne gidin. Ardından gerekli kitaplıkları yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install
    ```

1. Ortam değişkenini, daha önce bir değişiklik yaptığınız cihaz bağlantı dizesiyle yapılandırın:

    ```cmd/sh
    set IOTHUB_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

Bu örnek, basit bir IoT Tak ve Kullan termostat cihazı uygular. Bu örneğin uyguladığı model IoT Tak ve Kullan [bileşenleri](concepts-components.md)kullanmaz. [Termostat cihazının Dtdl model dosyası](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) , cihazın uyguladığı telemetri, özellik ve komutları tanımlar.

_simple_thermostat.js_ dosyasını açın. Bu dosyada nasıl yapılacağını görebilirsiniz:

1. Gerekli arabirimleri içeri aktarın.
1. Bir özellik güncelleştirme işleyicisi ve bir komut işleyicisi yazın.
1. İstenen özellik düzeltme eklerini işleyin ve telemetri gönderin.
1. İsteğe bağlı olarak, cihazınızı Azure cihaz sağlama hizmeti 'ni (DPS) kullanarak sağlayın.

Ana işlevde, tümünün nasıl bir araya geldiğini görebilirsiniz:

1. Cihazı bağlantı dizinizden oluşturun veya DPS kullanarak sağlayın.)
1. IoT Tak ve Kullan cihaz modelini belirtmek için **ModelId** seçeneğini kullanın.
1. Komut işleyicisini etkinleştirin.
1. Cihazdan hub 'ınıza telemetri gönderin.
1. Cihazları ikizi edin ve bildirilen özellikleri güncelleştirin.
1. İstenen özellik güncelleştirme işleyicisini etkinleştirin.

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

Örnek yapılandırma hakkında daha fazla bilgi edinmek için bkz. [örnek Readme](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/pnp/readme.md).

IoT Hub 'ınıza telemetri gönderen bir IoT Tak ve Kullan cihazının benzetimini yapmak için örnek uygulamayı çalıştırın. Örnek uygulamayı çalıştırmak için aşağıdaki komutu kullanın:

```cmd\sh
node simple_thermostat.js
```

Cihazın, telemetri verilerini hub 'a göndermeyi başladığını ve artık komutları ve özellik güncelleştirmelerini almaya hazır olduğunu belirten aşağıdaki çıktıyı görürsünüz.

![Cihaz onay iletileri](media/quickstart-connect-device-node/device-confirmation-node.png)

Sonraki adımları tamamladıktan sonra örneği çalışır durumda tutun.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT Gezginini Kullanma

Cihaz istemcisi örneği başladıktan sonra, çalıştığını doğrulamak için Azure IoT gezgin aracını kullanın.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Hub 'ına IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Çözümünüze bağlı olan IoT Tak ve Kullan cihazından etkileşime geçin](quickstart-service-node.md)
