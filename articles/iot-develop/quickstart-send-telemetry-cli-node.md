---
title: Azure IoT Hub hızlı başlangıç 'a cihaz telemetrisi gönderme (Node.js)
description: Bu hızlı başlangıçta, bir cihazdan IoT Hub 'ına telemetri göndermek için Node.js Azure IoT Hub cihaz SDK 'sını kullanırsınız.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: node
ms.topic: quickstart
ms.date: 01/11/2021
ms.openlocfilehash: 895b49779a202b817af1ede5082a11a5b1736afd
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654998"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-nodejs"></a>Hızlı başlangıç: bir cihazdan IoT Hub 'ına telemetri gönderme (Node.js)

Uygulama **hedefi**: [cihaz uygulaması geliştirme](about-iot-develop.md#device-application-development)

Bu hızlı başlangıçta, temel bir IoT cihaz uygulama geliştirme iş akışı öğrenirsiniz. Azure CLı 'yı kullanarak Azure IoT Hub 'ı ve sanal cihazı oluşturun, ardından cihaza erişmek ve hub 'a telemetri göndermek için Azure IoT Node.js SDK 'sını kullanırsınız.

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .
- Azure CLI. Bu hızlı başlangıçta, tarayıcınızda çalışan etkileşimli bir CLı kabuğu olan Azure Cloud Shell kullanarak tüm komutları çalıştırabilirsiniz. Cloud Shell kullanıyorsanız, herhangi bir şey yüklemeniz gerekmez. CLı 'yi yerel olarak kullanmayı tercih ediyorsanız bu hızlı başlangıç, Azure CLı sürüm 2.0.76 veya üstünü gerektirir. Sürümü bulmak için az --version komutunu çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).
- [Node.js 10 +](https://nodejs.org). Azure Cloud Shell kullanıyorsanız, Node.js yüklü sürümünü güncelleştirmeyin. Azure Cloud Shell en son Node.js sürümüne zaten sahip.

    Aşağıdaki komutu kullanarak, geliştirme makinenizde geçerli Node.js sürümünü doğrulayın:

    ```cmd/sh
        node --version
    ```

[!INCLUDE [iot-hub-include-create-hub-cli](../../includes/iot-hub-include-create-hub-cli.md)]

## <a name="use-the-nodejs-sdk-to-send-messages"></a>İleti göndermek için Node.js SDK 'sını kullanma
Bu bölümde, sanal cihazınızdan IoT Hub 'ınıza ileti göndermek için Node.js SDK 'sını kullanacaksınız. 

1. Yeni bir terminal penceresi açın. Bu terminali Node.js SDK 'Yı yüklemek ve Node.js örnek kodla çalışmak için kullanacaksınız. Şu anda iki terminalin açık olması gerekir: Node.js ile çalışmak için açtığınız bir tane ve önceki bölümlerde Azure CLı komutları girmek için kullandığınız CLı kabuğu. 

1. [Azure ıot Node.js SDK cihaz örneklerini](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) yerel makinenize kopyalayın:

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-node
    ```

1. *Azure-IoT-SDK-node/Device/Samples* dizinine gidin:

    ```console
    cd azure-iot-sdk-node/device/samples
    ```
1. Azure IoT Node.js SDK 'sını ve gerekli bağımlılıkları yükler:

    ```console
    npm install
    ```
    Bu komut, aygıt örnekleri dizinindeki *package.js* dosyasında belirtilen şekilde doğru bağımlılıkları kurar.

1. Cihaz bağlantı dizesini adında bir ortam değişkeni olarak ayarlayın `DEVICE_CONNECTION_STRING` . Kullanılacak dize değeri, sanal Node.js cihazınızı oluşturduktan sonra, önceki bölümde edindiğiniz dizedir. 

    **Windows (cmd)**

    ```console
    set DEVICE_CONNECTION_STRING=<your connection string here>
    ```

    > [!NOTE]
    > Windows CMD için bağlantı dizesini çevreleyen tırnak işareti yoktur.

    **Linux (Bash)**

    ```bash
    export DEVICE_CONNECTION_STRING="<your connection string here>"
    ```

1. Açık CLı kabuğunuzun içinde, sanal IoT cihazınızdan olayları izlemeye başlamak için [az IoT Hub Monitor-Events](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub?view=azure-cli-latest#ext-azure-iot-az-iot-hub-monitor-events&preserve-view=true) komutunu çalıştırın.  Olay iletileri, geldikçe terminalde yazdırılacaktır.

    ```azurecli
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. Node.js terminalinizde, yüklü örnek dosya *simple_sample_device.js* için kodu çalıştırın. Bu kod, sanal IoT cihazına erişir ve IoT Hub 'ına bir ileti gönderir.

    Terminalden Node.js örneğini çalıştırmak için:
    ```console
    node ./simple_sample_device.js
    ```

    İsteğe bağlı olarak, JavaScript IDE 'inizdeki örnekten Node.js kodu çalıştırabilirsiniz:
    ```javascript
    'use strict';

    const Protocol = require('azure-iot-device-mqtt').Mqtt;
    // Uncomment one of these transports and then change it in fromConnectionString to test other transports
    // const Protocol = require('azure-iot-device-amqp').AmqpWs;
    // const Protocol = require('azure-iot-device-http').Http;
    // const Protocol = require('azure-iot-device-amqp').Amqp;
    // const Protocol = require('azure-iot-device-mqtt').MqttWs;
    const Client = require('azure-iot-device').Client;
    const Message = require('azure-iot-device').Message;

    // String containing Hostname, Device Id & Device Key in the following formats:
    //  "HostName=<iothub_host_name>;DeviceId=<device_id>;SharedAccessKey=<device_key>"
    const deviceConnectionString = process.env.DEVICE_CONNECTION_STRING;
    let sendInterval;

    function disconnectHandler () {
    clearInterval(sendInterval);
    client.open().catch((err) => {
        console.error(err.message);
    });
    }

    // The AMQP and HTTP transports have the notion of completing, rejecting or abandoning the message.
    // For example, this is only functional in AMQP and HTTP:
    // client.complete(msg, printResultFor('completed'));
    // If using MQTT calls to complete, reject, or abandon are no-ops.
    // When completing a message, the service that sent the C2D message is notified that the message has been processed.
    // When rejecting a message, the service that sent the C2D message is notified that the message won't be processed by the device. the method to use is client.reject(msg, callback).
    // When abandoning the message, IoT Hub will immediately try to resend it. The method to use is client.abandon(msg, callback).
    // MQTT is simpler: it accepts the message by default, and doesn't support rejecting or abandoning a message.
    function messageHandler (msg) {
    console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
    client.complete(msg, printResultFor('completed'));
    }

    function generateMessage () {
    const windSpeed = 10 + (Math.random() * 4); // range: [10, 14]
    const temperature = 20 + (Math.random() * 10); // range: [20, 30]
    const humidity = 60 + (Math.random() * 20); // range: [60, 80]
    const data = JSON.stringify({ deviceId: 'myFirstDevice', windSpeed: windSpeed, temperature: temperature, humidity: humidity });
    const message = new Message(data);
    message.properties.add('temperatureAlert', (temperature > 28) ? 'true' : 'false');
    return message;
    }

    function errorCallback (err) {
    console.error(err.message);
    }

    function connectCallback () {
    console.log('Client connected');
    // Create a message and send it to the IoT Hub every two seconds
    sendInterval = setInterval(() => {
        const message = generateMessage();
        console.log('Sending message: ' + message.getData());
        client.sendEvent(message, printResultFor('send'));
    }, 2000);

    }

    // fromConnectionString must specify a transport constructor, coming from any transport package.
    let client = Client.fromConnectionString(deviceConnectionString, Protocol);

    client.on('connect', connectCallback);
    client.on('error', errorCallback);
    client.on('disconnect', disconnectHandler);
    client.on('message', messageHandler);

    client.open()
    .catch(err => {
    console.error('Could not connect: ' + err.message);
    });

    // Helper function to print results in the console
    function printResultFor(op) {
    return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
    };
    }
    ```

Node.js kodu, cihazınızdan IoT Hub 'ına sanal bir telemetri iletisi gönderdiğinden, bu ileti, olayları izleyen CLı kabuğinizde görünür:

```output
event:
  component: ''
  interface: ''
  module: ''
  origin: <your device name>
  payload: '{"deviceId":"myFirstDevice","windSpeed":11.853592092144627,"temperature":22.62484121157508,"humidity":66.17960805575937}'
```

Cihazınız artık güvenli bir şekilde bağlı ve Azure IoT Hub telemetri gönderiyor.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu hızlı başlangıçta oluşturulan Azure kaynaklarına artık ihtiyacınız yoksa, Azure CLı 'yi kullanarak bunları silebilirsiniz.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

Bir kaynak grubunu adıyla silmek için:
1. [Az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete&preserve-view=true) komutunu çalıştırın. Bu komut, kaynak grubunu, IoT Hub ve oluşturduğunuz cihaz kaydını kaldırır.

    ```azurecli
    az group delete --name MyResourceGroup
    ```
1. Kaynak grubunun silindiğini onaylamak için [az Group List](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-list&preserve-view=true) komutunu çalıştırın.  

    ```azurecli
    az group list
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir cihazı buluta güvenli bir şekilde bağlamak ve cihazdan buluta telemetri göndermek için temel bir Azure IoT uygulama iş akışı öğrendiniz. Azure CLı 'yi bir IoT Hub 'ı ve sanal cihaz oluşturmak için kullandınız, ardından cihaza erişmek ve hub 'a telemetri göndermek için Azure IoT Node.js SDK 'sını kullandınız. 

Sonraki adım olarak, uygulama örnekleri aracılığıyla Azure IoT Node.js SDK 'sını keşfedebilirsiniz.

- [Daha fazla Node.js örnek](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples): Bu dizin, IoT Hub senaryoları göstermek IÇIN Node.js SDK deposundan daha fazla örnek içerir. 
