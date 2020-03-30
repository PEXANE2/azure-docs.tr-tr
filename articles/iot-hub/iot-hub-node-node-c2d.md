---
title: Azure IoT Hub (Düğüm) ile buluttan cihaza iletiler | Microsoft Dokümanlar
description: Node.js için Azure IoT SDK'larını kullanarak bir Azure IoT hub'ından bir aygıta buluttan aygıta ileti gönderme. Simüle edilmiş bir aygıt uygulamasını, buluttan cihaza iletiler almak için ve buluttan cihaza iletigöndermek için bir arka uç uygulamasını değiştirmek için değiştirirsiniz.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: javascript
ms.topic: conceptual
ms.date: 06/16/2017
ms.openlocfilehash: 8071ddbc5f6073598daf0a08d359ccd19ccd1e4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110797"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-nodejs"></a>IoT Hub (Node.js) ile buluttan cihaza iletigönderme

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub, milyonlarca aygıt ve bir çözüm arka uç arasında güvenilir ve güvenli çift yönlü iletişim sağlamaya yardımcı olan tam olarak yönetilen bir hizmettir. Bir [aygıttan IoT hub'ına hızlı başlat'a gönder telemetrisi,](quickstart-send-telemetry-node.md) bir IoT hub'ı oluşturmanın, içinde aygıt kimliğini nasıl sağlayabilirsiniz ve aygıttan buluta iletigönderen simüle edilmiş bir aygıt uygulamasını kodlamayı gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici, [bir aygıttan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-node.md)biryapılsa. Nasıl yapılacağını gösterir:

* Çözümünüzden arka uçtan, IoT Hub üzerinden tek bir cihaza buluttan cihaza iletigönderin.
* Bir aygıttan buluttan aygıta iletiler alın.
* Çözümünüzden arka uçtan, IoT Hub'dan bir cihaza gönderilen iletiler için teslimat bildirimi *(geri bildirim)* isteyin.

[IoT Hub geliştirici kılavuzunda](iot-hub-devguide-messaging.md)buluttan cihaza iletiler hakkında daha fazla bilgi bulabilirsiniz.

Bu öğreticinin sonunda, iki Node.js konsol uygulaması çalıştırın:

* **Bir** [cihazdan IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-node.md)oluşturulan uygulamanın değiştirilmiş bir sürümü olan ve IoT hub'ınıza bağlanan ve buluttan cihaza iletiler alan bir IoT hub'ı.

* IoT Hub üzerinden simüle edilen aygıt uygulamasına buluttan cihaza ileti gönderen ve ardından teslim bildirimini alan **SendCloudToDeviceMessage.**

> [!NOTE]
> IoT Hub, Azure IoT aygıt SDK'ları aracılığıyla birçok aygıt platformu ve dili (C, Java, Python ve Javascript dahil) için SDK desteğine sahiptir. Cihazınızı bu öğreticinin koduna ve genellikle Azure IoT Hub'ına nasıl bağlayacaklarına ilişkin adım adım talimatlar için [Azure IoT Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/iot)bakın.
>

## <a name="prerequisites"></a>Ön koşullar

* Düğüm.js sürüm 10.0.x veya daha sonra. [Geliştirme ortamınızı hazırlayın,](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) bu öğretici için Windows veya Linux'a Node.js'in nasıl yüklenir olduğunu açıklar.

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial) oluşturabilirsiniz.)

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="receive-messages-in-the-simulated-device-app"></a>Benzetimli cihaz uygulamasından ileti alma

Bu bölümde, IoT hub'ından buluttan aygıta iletiler almak için telemetri gönder'de oluşturduğunuz simüle edilmiş aygıt uygulamasını [bir aygıttan IoT](quickstart-send-telemetry-node.md) hub'ına değiştirirsiniz.

1. Metin düzenleyicisi **kullanarak, SimulatedDevice.js** dosyasını açın. Bu dosya, bir aygıttan bir IoT hub quickstart'a [telemetri gönder'de](quickstart-send-telemetry-node.md) indirdiğiniz Düğüm.js örnek kodunun kök klasöründeki **iot-hub\Quickstarts\simüle-aygıt** klasöründe bulunur.

2. IoT Hub'dan gönderilen iletileri almak için aygıt istemcisine bir işleyici kaydedin. Aşağıdaki parçacıkta `client.on` olduğu gibi aygıt istemcisini oluşturan satırdan hemen sonraya çağrıyı ekleyin:

    ```javascript
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);

    client.on('message', function (msg) {
      console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
      client.complete(msg, function (err) {
        if (err) {
          console.error('complete error: ' + err.toString());
        } else {
          console.log('complete sent');
        }
      });
    });
    ```

    Bu örnekte, aygıt IoT Hub'a iletiyi işlediğini bildirmek için **tam** işlevi çağırır. MQTT taşımasını kullanıyorsanız ve atlanabilirseniz, **tamamlanması** gereken çağrı gerekli değildir. BU HTTPS ve AMQP için gereklidir.
  
   > [!NOTE]
   > Aktarım olarak MQTT veya AMQP yerine HTTPS kullanıyorsanız, **DeviceClient** örneği IoT Hub'ından gelen iletileri seyrek olarak denetler (her 25 dakikadan az). MQTT, AMQP ve HTTPS desteği ve IoT Hub azaltma arasındaki farklar hakkında daha fazla bilgi için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide-messaging.md)bakın.
   >

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, bir aygıttan bir [IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-node.md)oluşturduğunuz IoT hub'ı üzerinden buluttan aygıta iletigöndermek için bir arka uç hizmeti oluşturursunuz. Buluttan cihaza ileti göndermek için hizmetinizin **hizmet bağlantısı** namına ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="send-a-cloud-to-device-message"></a>Buluttan aygıta ileti gönderme

Bu bölümde, simüle edilen aygıt uygulamasına buluttan cihaza iletigönderen bir Düğüm.js konsol uygulaması oluşturursunuz. Bir [aygıttan IoT](quickstart-send-telemetry-node.md) hub'ına hızlı başlat'a telemetri gönder'e eklediğiniz aygıtın aygıt kimliğine ihtiyacınız vardır. Ayrıca, IoT hub bağlantı dizesini al'da daha önce kopyaladığınız [IoT hub bağlantı dizesini](#get-the-iot-hub-connection-string)de almanız gerekir.

1. **sendcloudtodevicemessage**adlı boş bir klasör oluşturun. **sendcloudtodevicemessage** klasöründe, komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```shell
    npm init
    ```

2. **sendcloudtodevicemessage** klasöründeki komut isteminizde **azure-iothub** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```shell
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak, **sendcloudtodevicemessage** ileti klasöründe bir **SendCloudToDeviceMessage.js** dosyası oluşturun.

4. `require` **SendCloudToDeviceMessage.js** dosyasının başında aşağıdaki ifadeleri ekleyin:

    ```javascript
    'use strict';

    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. **SendCloudToDeviceMessage.js** dosyasına aşağıdaki kodu ekleyin. "{iot hub bağlantı string}" ve "{device id}" yer tutucu değerlerini daha önce belirttiğiniz IoT hub bağlantı dizesi ve aygıt kimliğiyle değiştirin:

    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. İşlem sonuçlarını konsola yazdırmak için aşağıdaki işlevi ekleyin:

    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Teslim geri bildirim iletilerini konsola yazdırmak için aşağıdaki işlevi ekleyin:

    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Cihazınıza ileti göndermek ve aygıt buluttan aygıta iletiyi kabul ettiğinde geri bildirim iletisini işlemek için aşağıdaki kodu ekleyin:

    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

9. **SendCloudToDeviceMessage.js** dosyasını kaydedin ve kapatın.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. **Benzetimli aygıt** klasöründeki komut isteminde, IoT Hub'a telemetri göndermek ve buluttan aygıta iletileri dinlemek için aşağıdaki komutu çalıştırın:

    ```shell
    node SimulatedDevice.js
    ```

    ![Benzetimli cihaz uygulamasını çalıştırma](./media/iot-hub-node-node-c2d/receivec2d.png)

2. **sendcloudtodevicemessage** klasöründeki komut isteminde, buluttan aygıta ileti göndermek için aşağıdaki komutu çalıştırın ve bildirim geri bildirimini bekleyin:

    ```shell
    node SendCloudToDeviceMessage.js
    ```

    ![Bulut-aygıt komutunu göndermek için uygulamayı çalıştırın](./media/iot-hub-node-node-c2d/sendc2d.png)

   > [!NOTE]
   > Basitlik için, bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkeleri (üstel geri tepme gibi) uygulamanız gerekir.
   >

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, buluttan cihaza ileti göndermeyi ve almayı öğrendiniz.

IoT Hub'ı kullanan eksiksiz uçtan uca çözümlerin örneklerini görmek için [Azure IoT Uzaktan İzleme çözüm hızlandırıcısına](https://azure.microsoft.com/documentation/suites/iot-suite/)bakın.

IoT Hub ile çözüm geliştirme hakkında daha fazla bilgi edinmek için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.
