---
title: Öğretici-genel bir Node.js istemci uygulamasını Azure IoT Central bağlama | Microsoft Docs
description: Bu öğreticide, bir cihaz geliştiricisi olarak, Node.js istemci uygulaması çalıştıran bir cihaza Azure IoT Central uygulamanıza nasıl bağlanacağı gösterilmektedir. Cihaz yetenek modeli içeri aktararak ve bağlı bir cihazla etkileşime girebilen görünümler ekleyerek bir cihaz şablonu oluşturursunuz
author: dominicbetts
ms.author: dobett
ms.date: 07/07/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mqtt
ms.openlocfilehash: 08df3bce9d1ecce4d4b0cdfc3034355feef6a4ba
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87003381"
---
# <a name="tutorial-create-and-connect-a-client-application-to-your-azure-iot-central-application-nodejs"></a>Öğretici: Azure IoT Central uygulamanıza istemci uygulaması oluşturma ve bağlama (Node.js)

[!INCLUDE [iot-central-selector-tutorial-connect](../../../includes/iot-central-selector-tutorial-connect.md)]

*Bu makale, çözüm oluşturucular ve cihaz geliştiricileri için geçerlidir.*

Bu öğreticide, bir cihaz geliştiricisi olarak, Node.js istemci uygulamasını Azure IoT Central uygulamanıza nasıl bağlayabilmeniz gösterilmektedir. Node.js uygulaması bir ortam algılayıcısı cihazının davranışını taklit eder. IoT Central bir _cihaz şablonu_ oluşturmak için örnek bir _cihaz yetenek modeli_ kullanın. Bir işlecin bir cihazla etkileşime geçmesini sağlamak için cihaz şablonuna görünümler eklersiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz şablonu oluşturmak için bir cihaz yetenek modeli içeri aktarın.
> * Varsayılan ve özel görünümleri bir cihaz şablonuna ekleyin.
> * Bir cihaz şablonu yayımlayın ve IoT Central uygulamanıza gerçek bir cihaz ekleyin.
> * Node.js cihaz kodu oluşturup çalıştırın ve IoT Central uygulamanıza bağlanın.
> * Cihazdan gönderilen sanal Telemetriyi görüntüleyin.
> * Cihaz özelliklerini yönetmek için bir görünüm kullanın.
> * Cihazı denetlemek için zaman uyumlu ve zaman uyumsuz komutları çağırın.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* **Özel uygulama** şablonu kullanılarak oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md). Uygulamanın 07/14/2020 veya sonrasında oluşturulmuş olması gerekir.
* [Node.js](https://nodejs.org/) Version 10.0.0 veya üzeri yüklü bir geliştirme makinesi. `node --version`Sürümünüzü denetlemek için komut satırında çalıştırabilirsiniz. Bu öğreticideki yönergeler, Windows komut isteminde **node** komutunu çalıştırmakta olduğunuzu varsayar. Ancak, diğer birçok işletim sisteminde Node.js kullanabilirsiniz.

[!INCLUDE [iot-central-add-environmental-sensor](../../../includes/iot-central-add-environmental-sensor.md)]

### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Aşağıdaki adımlarda, uygulamaya eklediğiniz gerçek cihaza bağlanan bir Node.js istemci uygulamasının nasıl oluşturulacağı gösterilmektedir. Bu Node.js uygulama, gerçek bir cihazın davranışının benzetimini yapar.

1. Komut satırı ortamınızda, `environmental-sensor` daha önce oluşturduğunuz klasöre gidin.

1. Node.js projenizi başlatmak ve gerekli bağımlılıkları yüklemek için aşağıdaki komutları çalıştırın: çalıştırdığınızda tüm varsayılan seçenekleri kabul edin `npm init` :

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt azure-iot-provisioning-device-mqtt azure-iot-security-symmetric-key --save
    ```

1. Klasöründe **environmentalSensor.js** adlı bir dosya oluşturun `environmental-sensor` .

1. `require` **environmentalSensor.js** dosyasının başlangıcına aşağıdaki deyimlerini ekleyin:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var iotHubTransport = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var ProvisioningTransport = require('azure-iot-provisioning-device-mqtt').Mqtt;
    var SymmetricKeySecurityClient = require('azure-iot-security-symmetric-key').SymmetricKeySecurityClient;
    var ProvisioningDeviceClient = require('azure-iot-provisioning-device').ProvisioningDeviceClient;
    ```

1. Aşağıdaki değişken bildirimlerini dosyaya ekleyin:

    ```javascript
    var provisioningHost = 'global.azure-devices-provisioning.net';
    var idScope = '{your Scope ID}';
    var registrationId = '{your Device ID}';
    var symmetricKey = '{your Primary Key}';
    var provisioningSecurityClient = new SymmetricKeySecurityClient(registrationId, symmetricKey);
    var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvisioningTransport(), provisioningSecurityClient);
    var hubClient;

    var targetTemperature = 0;
    var ledOn = true;
    ```

    Yer tutucuları `{your Scope ID}` , `{your Device ID}` , ve `{your Primary Key}` daha önce bir notcuı yaptığınız değerlerle güncelleştirin. Bu örnekte, `targetTemperature` sıfırdan başlatın, cihazdaki geçerli okumayı veya cihazdan ikizi bir değeri kullanabilirsiniz.

1. Azure IoT Central uygulamanıza sanal telemetri göndermek için, dosyaya aşağıdaki işlevi ekleyin:

    ```javascript
    // Send simulated device telemetry.
    function sendTelemetry() {
      var temp = targetTemperature + (Math.random() * 15);
      var humid = 70 + (Math.random() * 10);
      var data = JSON.stringify({
        temp: temp,
        humid: humid,
        });
      var message = new Message(data);
      hubClient.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    Telemetri öğelerinin (ve) adları, `temp` `humid` cihaz şablonunda kullanılan adlarla aynı olmalıdır.

1. Azure IoT Central uygulamanıza cihaz ikizi özellikleri göndermek için, dosyanıza aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device twin reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

    IoT Central cihaz ve IoT Central uygulama arasındaki özellik değerlerini senkronize etmek için cihaz ikizlerini kullanır. Cihaz özelliği değerleri cihaz ikizi bildirilen özelliklerini kullanır. Yazılabilir Özellikler hem cihaz ikizi bildirilen hem de istenen özellikleri kullanır.

1. Cihazınızın yanıt verdiği yazılabilir özellikleri tanımlamak ve işlemek için aşağıdaki kodu ekleyin. [Yazılabilir Özellik güncelleştirmesine](concepts-telemetry-properties-commands.md#writeable-property-types) yanıt olarak cihazın gönderdiği ileti, `av` ve `ac` alanlarını içermelidir. `ad`Alan isteğe bağlıdır:

    ```javascript
    // Add any writeable properties your device supports,
    // mapped to a function that's called when the writeable property
    // is updated in the IoT Central application.
    var writeableProperties = {
      'name': (newValue, callback) => {
          setTimeout(() => {
            callback(newValue, 'completed', 200);
          }, 1000);
      },
      'brightness': (newValue, callback) => {
        setTimeout(() => {
            callback(newValue, 'completed', 200);
        }, 5000);
      }
    };

    // Handle writeable property updates that come from IoT Central via the device twin.
    function handleWriteablePropertyUpdates(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (writeableProperties[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting]}`);
            writeableProperties[setting](desiredChange[setting], (newValue, status, code) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  ad: status,
                  ac: code,
                  av: desiredChange.$version
                }
              }
              sendDeviceProperties(twin, patch);
            });
          }
        }
      });
    }
    ```

    İşleci IoT Central uygulamasında yazılabilir bir özellik ayarladığında, uygulama, değeri cihaza göndermek için bir Device ikizi istenen özelliğini kullanır. Cihaz daha sonra bir Device ikizi bildirilen özelliği kullanarak yanıt verir. IoT Central bildirilen özellik değerini aldığında, özellik görünümünü **eşitlenmiş**durumuyla güncelleştirir.

    Özelliklerin (ve) adları, `name` `brightness` cihaz şablonunda kullanılan adlarla eşleşmelidir.

1. IoT Central uygulamasından gönderilen komutları işlemek için aşağıdaki kodu ekleyin:

    ```javascript
    // Setup command handlers
    function setupCommandHandlers(twin) {

      // Handle synchronous LED blink command with request and response payload.
      function onBlink(request, response) {
        console.log('Received synchronous call to blink');
        var responsePayload = {
          status: 'Blinking LED every ' + request.payload  + ' seconds'
        }
        response.send(200, responsePayload, (err) => {
          if (err) {
            console.error('Unable to send method response: ' + err.toString());
          } else {
            console.log('Blinking LED every ' + request.payload  + ' seconds');
          }
        });
      }

      // Handle synchronous LED turn on command
      function turnOn(request, response) {
        console.log('Received synchronous call to turn on LED');
        if(!ledOn){
          console.log('Turning on the LED');
          ledOn = true;
        }
        response.send(200, (err) => {
          if (err) {
            console.error('Unable to send method response: ' + err.toString());
          }
        });
      }

      // Handle synchronous LED turn off command
      function turnOff(request, response) {
        console.log('Received synchronous call to turn off LED');
        if(ledOn){
          console.log('Turning off the LED');
          ledOn = false;
        }
        response.send(200, (err) => {
          if (err) {
            console.error('Unable to send method response: ' + err.toString());
          }
        });
      }

      // Handle asynchronous sensor diagnostics command with response payload.
      function diagnostics(request, response) {
        console.log('Starting asynchronous diagnostics run...');
        response.send(202, (err) => {
          if (err) {
            console.error('Unable to send method response: ' + err.toString());
          } else {
            var repetitions = 3;
            var intervalID = setInterval(() => {
              console.log('Generating diagnostics...');
              if (--repetitions === 0) {
                clearInterval(intervalID);
                var properties = {
                  rundiagnostics: {
                    value: 'Diagnostics run complete at ' + new Date().toLocaleString()
                  }
                };
                sendDeviceProperties(twin, properties);
              }
            }, 2000);
          }
        });
      }

      hubClient.onDeviceMethod('blink', onBlink);
      hubClient.onDeviceMethod('turnon', turnOn);
      hubClient.onDeviceMethod('turnoff', turnOff);
      hubClient.onDeviceMethod('rundiagnostics', diagnostics);
    }
    ```

    Komutların ( `blink` , `turnon` , ve) adları, `turnoff` `rundiagnostics` cihaz şablonunda kullanılan adlarla aynı olmalıdır.

    Şu anda IoT Central, cihaz yetenek modelinde tanımlanan yanıt şemasını kullanmıyor. Zaman uyumlu bir komut için, yanıt yükü geçerli bir JSON olabilir. Zaman uyumsuz bir komut için, cihazın hemen bir 202 yanıtı döndürmesi ve sonra iş tamamlandığında bildirilen özellik güncelleştirmesi gelmelidir. Bildirilen özellik güncelleştirmesinin biçimi:

    ```json
    {
      [command name] : {
        value: 'response message'
      }
    }
    ```

    Bir işleç, yanıt yükünü komut geçmişinde görüntüleyebilir.

1. Azure IoT Central bağlantısını tamamlamak ve istemci kodundaki işlevleri bağlamak için aşağıdaki kodu ekleyin:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Send telemetry to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        hubClient.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            var properties = {
              state: 'true',
              processorArchitecture: 'ARM',
              swVersion: '1.0.0'
            };
            sendDeviceProperties(twin, properties);

            handleWriteablePropertyUpdates(twin);

            setupCommandHandlers(twin);
          }
        });
      }
    };

    // Start the device (register and connect to Azure IoT Central).
    provisioningClient.register((err, result) => {
      if (err) {
        console.log('Error registering device: ' + err);
      } else {
        console.log('Registration succeeded');
        console.log('Assigned hub=' + result.assignedHub);
        console.log('DeviceId=' + result.deviceId);
        var connectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';SharedAccessKey=' + symmetricKey;
        hubClient = Client.fromConnectionString(connectionString, iotHubTransport);

        hubClient.open(connectCallback);
      }
    });
    ```

## <a name="run-your-nodejs-application"></a>Node.js uygulamanızı çalıştırın

Cihaz istemci uygulamasını başlatmak için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
node environmentalSensor.js
```

Cihazın Azure IoT Central uygulamanıza bağlandığını görebilir ve telemetri göndermeye başlar:

![İstemci uygulamasını çalıştırma](media/tutorial-connect-device-nodejs/run-application.png)

[!INCLUDE [iot-central-monitor-environmental-sensor](../../../includes/iot-central-monitor-environmental-sensor.md)]

Aygıtın komutlara ve özellik güncelleştirmelerine nasıl yanıt verdiğini görebilirsiniz:

![İstemci uygulamasını gözlemleyin](media/tutorial-connect-device-nodejs/run-application-2.png)

## <a name="view-raw-data"></a>Ham verileri görüntüleme

[!INCLUDE [iot-central-monitor-environmental-sensor-raw-data](../../../includes/iot-central-monitor-environmental-sensor-raw-data.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz geliştiricisi olarak, Node.js kullanarak bir cihazın nasıl oluşturulacağı hakkında temel bilgileri öğrendiğinize göre, önerilen bazı sonraki adımlar şunlardır:

* Bir [Mxyonga IoT DevKit cihazını Azure IoT Central uygulama](./howto-connect-devkit.md) nasıl yapılır makalesine bağlama hakkında IoT Central için gerçek bir cihazı nasıl bağlayacağınızı öğrenin.
* Cihaz kodunuzu uygularken cihaz şablonlarının rolü hakkında daha fazla bilgi edinmek için cihaz [şablonu nedir?](./concepts-device-templates.md) makalesini okuyun.
* Cihazların IoT Central nasıl kaydedileceği ve cihaz bağlantılarının güvenliğini IoT Central sağlama hakkında daha fazla bilgi edinmek için [Azure IoT Central 'ye](./concepts-get-connected.md) bağlanın.

IoT Central öğreticiler kümesine devam etmeyi tercih ediyorsanız ve bir IoT Central çözümü oluşturma hakkında daha fazla bilgi edinmek istiyorsanız, bkz.:

> [!div class="nextstepaction"]
> [Ağ geçidi cihaz şablonu oluşturma](./tutorial-define-gateway-device-type.md)
