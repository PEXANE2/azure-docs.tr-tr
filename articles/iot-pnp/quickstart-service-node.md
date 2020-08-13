---
title: Azure IoT çözümünüze bağlı olan IoT Tak ve Kullan önizleme cihazından etkileşim kurma (Node.js) | Microsoft Docs
description: Azure IoT çözümünüze bağlı bir IoT Tak ve Kullan önizleme cihazına bağlanmak ve bunlarla etkileşim kurmak için Node.js kullanın.
author: elhorton
ms.author: elhorton
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc, devx-track-javascript
ms.openlocfilehash: fd65dcc9ce0be07daa5848a0ac583cf795150e47
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88184780"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution-nodejs"></a>Hızlı başlangıç: çözümünüze bağlı olan IoT Tak ve Kullan önizleme cihazından etkileşim kurma (Node.js)

[!INCLUDE [iot-pnp-quickstarts-service-selector.md](../../includes/iot-pnp-quickstarts-service-selector.md)]

IoT Tak ve Kullan önizlemesi, temeldeki cihaz uygulamasıyla ilgili bilgi sahibi olmadan bir cihazın özellikleri ile etkileşim kurmanızı sağlayarak IoT 'yi basitleştirir. Bu hızlı başlangıçta, çözümünüze bağlı bir IoT Tak ve Kullan cihazına bağlanmak ve bunları denetlemek için Node.js nasıl kullanılacağı gösterilmektedir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için geliştirme makinenizde Node.js gerekir. [NodeJS.org](https://nodejs.org)adresinden birden çok platform için önerilen en son sürümü indirebilirsiniz.

Aşağıdaki komutu kullanarak geliştirme makinenizde geçerli Node.js sürümünü doğrulayabilirsiniz:

```cmd/sh
node --version
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

### <a name="clone-the-sdk-repository-with-the-sample-code"></a>Örnek kodla SDK deposunu kopyalayın

Hizmet SDK 'Sı önizlemede olduğundan, [düğüm SDK 'sının önizleme dalından](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh)örnekleri kopyalamanız gerekir. Seçtiğiniz bir klasörde bir Terminal penceresi açın. Node.jsGitHub deposu [için Microsoft Azure ıOT SDK 'sının](https://github.com/Azure/azure-iot-sdk-node) **pnp-Preview-Refresh** dalını kopyalamak için aşağıdaki komutu çalıştırın:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node -b pnp-preview-refresh
```

## <a name="run-the-sample-device"></a>Örnek cihazı çalıştırma

Bu hızlı başlangıçta, IoT Tak ve Kullan cihazı olarak Node.js yazılmış örnek bir termostat cihazı kullanabilirsiniz. Örnek cihazı çalıştırmak için:

1. Bir Terminal penceresi açın ve GitHub 'dan kopyaladığınız Node.js deposu için Microsoft Azure IoT SDK 'sını içeren yerel klasöre gidin.

1. Bu terminal penceresi, **cihaz** terminali olarak kullanılır. Klonlanmış deponuzdaki klasöre gidin ve */Azure-iot-SDK-node/Device/Samples/PNP* klasörüne gidin. Aşağıdaki komutu çalıştırarak tüm bağımlılıkları yükler:

    ```cmd/sh
    npm install
    ```

1. _Cihaz bağlantı dizesini_yapılandırın:

    ```cmd/sh
    set IOTHUB_DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
    ```

1. Aşağıdaki komutla örnek termostat cihazını çalıştırın:

    ```cmd/sh
    node simple_thermostat.js
    ```

1. Cihazın bazı bilgileri gönderdiğini ve onun çevrimiçi olduğunu söyleyen iletiler görürsünüz. Bu iletiler, cihazın hub 'a telemetri verileri göndermeyi başladığını ve artık komut ve özellik güncelleştirmelerini almaya hazır olduğunu gösterir. Bu terminali kapatmayın, hizmet örneğinin çalıştığını onaylamanız gerekir.

## <a name="run-the-sample-solution"></a>Örnek çözümü çalıştırma

Bu hızlı başlangıçta, yeni ayarladığınız örnek cihazla etkileşim kurmak için Node.js bir örnek IoT çözümü kullanırsınız.

1. **Hizmet** terminali olarak kullanmak için başka bir Terminal penceresi açın. Hizmet SDK 'Sı önizlemede olduğundan, [düğüm SDK 'sının önizleme dalından](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh)örnekleri kopyalamanız gerekir:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-node -b pnp-preview-refresh
    ```

1. Bu kopyalanmış depo dalının klasörüne gidin ve */Azure-iot-SDK-node/digitaltwins/Samples/Service/JavaScript* klasörüne gidin. Aşağıdaki komutu çalıştırarak tüm bağımlılıkları yükler:

    ```cmd/sh
    npm install
    ```

1. Cihaz KIMLIĞINIZ ve _IoT Hub bağlantı dizeniz_için ortam değişkenlerini yapılandırın:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<YourIOTHubConnectionString>
    set IOTHUB_DEVICE_ID=<Your device ID>
    ```

### <a name="read-a-property"></a>Bir özelliği okuyun

1. **Cihaz** terminalinde örnek termostat cihazını çalıştırdığınızda, çevrimiçi durumunu belirten aşağıdaki iletileri gördünüz:

    ```cmd/sh
    properties have been reported for component
    sending telemetry message 0...
    ```

1. **Hizmet** terminaline gidin ve cihaz bilgilerini okumak üzere örneği çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. **Hizmet** terminali çıkışında, dijital ikizi yanıtını görürsünüz. Cihazın model KIMLIĞINI ve bildirilen ilişkili özellikleri görürsünüz:

    ```json
    "$dtId": "mySimpleThermostat",
    "serialNumber": "123abc",
    "maxTempSinceLastReboot": 51.96167432818655,
    "$metadata": {
      "$model": "dtmi:com:example:Thermostat;1",
      "serialNumber": { "lastUpdateTime": "2020-07-09T14:04:00.6845182Z" },
      "maxTempSinceLastReboot": { "lastUpdateTime": "2020-07-09T14:04:00.6845182" }
    }
    ```

1. Aşağıdaki kod parçacığında, Device ikizi 'in model KIMLIĞINI alan *get_digital_twin.js* kodu gösterilmektedir:

    ```javascript
    console.log("Model Id: " + inspect(digitalTwin.$metadata.$model))
    ```

Bu senaryoda, çıkış çıkışları `Model Id: dtmi:com:example:Thermostat;1` .

### <a name="update-a-writable-property"></a>Yazılabilir bir özelliği güncelleştirme

1. Dosya *update_digital_twin.js* bir kod düzenleyicisinde açın.

1. Örnek kodu gözden geçirin. Cihazınızın dijital ikizi güncelleştirmek için nasıl JSON yaması oluşturacağınız hakkında bilgi alabilirsiniz. Bu örnekte, kod termostat 'nın sıcaklığını 42 değeriyle değiştirir:

    ```javascript
    const patch = [{
        op: 'add',
        path: '/targetTemperature',
        value: '42'
      }]
    ```

1. **Hizmet** terminalinde, özelliği güncelleştirme örneğini çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    node update_digital_twin.js
    ```

1. **Cihaz terminalinizde** , cihazın güncelleştirmeyi aldığını görürsünüz:

    ```cmd/sh
    The following properties will be updated for root interface:
    {
      targetTemperature: {
        value: 42,
        ac: 200,
        ad: 'Successfully executed patch for targetTemperature',
        av: 2
      }
    }
    updated the property
    Properties have been reported for component
    ```

1. **Hizmet** terminalinizde, özelliğin güncelleştirildiğini onaylamak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. **Hizmet** terminali çıkışında, bileşen altındaki dijital ikizi yanıtında `thermostat1` raporlanan hedef sıcaklığın olduğunu görürsünüz. Cihazın güncelleştirmeyi tamamlaması biraz zaman alabilir. Cihaz özellik güncelleştirmesini işleyene kadar bu adımı yineleyin:

    ```json
    targetTemperature: 42,
    ```

### <a name="invoke-a-command"></a>Komut çağırma

1. *invoke_command.js* dosyasını açın ve kodu gözden geçirin.

1. **Hizmet** terminali ' ne gidin. Komutu çağırmak için örneği çalıştırmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    set IOTHUB_COMMAND_NAME=getMaxMinReport
    set IOTHUB_COMMAND_PAYLOAD=commandpayload
    node invoke_command.js
    ```

1. **Hizmet** terminalinde çıktı aşağıdaki onayı gösterir:

    ```cmd/sh
    {
        xMsCommandStatuscode: 200,  
        xMsRequestId: 'ee9dd3d7-4405-4983-8cee-48b4801fdce2',  
        connection: 'close',  'content-length': '18',  
        'content-type': 'application/json; charset=utf-8',  
        date: 'Thu, 09 Jul 2020 15:05:14 GMT',  
        server: 'Microsoft-HTTPAPI/2.0',  vary: 'Origin',  
        body: 'min/max response'
    }
    ```

1. **Cihaz** terminalinde, komutun kabul edilen olduğunu görürsünüz:

    ```cmd/sh
    MaxMinReport commandpayload
    Response to method 'getMaxMinReport' sent successfully.
    ```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Tak ve Kullan cihazını IoT çözümüne bağlamayı öğrendiniz. IoT Tak ve Kullan cihaz modelleri hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [IoT Tak ve Kullan Preview modelleme Geliştirici Kılavuzu](concepts-developer-guide.md)
