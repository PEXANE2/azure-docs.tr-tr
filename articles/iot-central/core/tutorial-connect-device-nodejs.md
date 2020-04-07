---
title: Öğretici - Genel bir Node.js istemci uygulamasını Azure IoT Central'a bağlayın | Microsoft Dokümanlar
description: Bu öğretici, bir aygıt geliştiricisi olarak, Node.js istemci uygulaması çalıştıran bir aygıtı Azure IoT Merkezi uygulamanıza nasıl bağlayabildiğinizi gösterir. Aygıt yeteneği modelini içe aktararak bir aygıt şablonu oluşturur ve bağlı bir aygıtla etkileşim kurmanıza izin veren görünümler eklersiniz
author: dominicbetts
ms.author: dobett
ms.date: 03/24/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: b7aebb5b9c1aa6566cedda869f97f2d1aa20df83
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673960"
---
# <a name="tutorial-create-and-connect-a-nodejs-client-application-to-your-azure-iot-central-application-nodejs"></a>Öğretici: Bir Düğüm.js istemci sebeb uygulaması oluşturun ve Azure IoT Merkezi uygulamanıza bağlayın (Node.js)

[!INCLUDE [iot-central-selector-tutorial-connect](../../../includes/iot-central-selector-tutorial-connect.md)]

Bu öğretici, bir aygıt geliştiricisi olarak Bir Düğüm.js istemci uygulamasını Azure IoT Merkezi uygulamanıza nasıl bağlayacaklarını gösterir. Düğüm.js uygulaması bir çevre sensör cihazının davranışını simüle eder. IoT Central'da bir _aygıt şablonu_ oluşturmak için örnek bir _aygıt yetenek modeli_ kullanırsınız. Bir işleç bir aygıtla etkileşim etemesini sağlamak için aygıt şablonuna görünümler eklersiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Aygıt şablonu oluşturmak için aygıt yeteneği modelini aktarın.
> * Aygıt şablonuna varsayılan ve özel görünümler ekleyin.
> * Bir aygıt şablonu yayımlayın ve IoT Central uygulamanıza gerçek bir aygıt ekleyin.
> * Düğüm.js aygıt kodunu oluşturun ve çalıştırın ve IoT Central uygulamanıza bağlandığına bakın.
> * Aygıttan gönderilen benzetimli telemetriyi görüntüleyin.
> * Aygıt özelliklerini yönetmek için bir görünüm kullanın.
> * Cihazı kontrol etmek için senkron ve eşzamanlı komutları arayın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* **Özel uygulama** şablonu kullanılarak oluşturulan bir Azure IoT Merkezi uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* [Node.js](https://nodejs.org/) sürüm 10.0.0 veya daha sonra yüklü bir geliştirme makinesi. Sürümünüzü `node --version` kontrol etmek için komut satırında çalıştırabilirsiniz. Bu öğreticideki talimatlar, Windows komut isteminde **düğüm** komutunu çalıştırdığınızı varsayar. Ancak, diğer birçok işletim sisteminde Node.js kullanabilirsiniz.

[!INCLUDE [iot-central-add-environmental-sensor](../../../includes/iot-central-add-environmental-sensor.md)]

### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Aşağıdaki adımlar, uygulamaya eklediğiniz gerçek aygıta bağlanan bir Düğüm istemcisi uygulamasını nasıl oluşturabileceğinizi gösterir. Bu Düğüm.js uygulaması gerçek bir aygıtın davranışını simüle eder.

1. Komut satırı ortamınızda, daha `environmental-sensor` önce oluşturduğunuz klasöre gidin.

1. Düğüm.js projenizi başlatmave gerekli bağımlılıkları yüklemek için aşağıdaki komutları çalıştırın - çalıştırdığınızda `npm init`tüm varsayılan seçenekleri kabul edin:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt azure-iot-provisioning-device-mqtt azure-iot-security-symmetric-key --save
    ```

1. Klasörde **environmentalSensor.js** adlı `environmental-sensor` bir dosya oluşturun.

1. `require` **environmentalSensor.js** dosyasının başında aşağıdaki ifadeleri ekleyin:

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

    Yer tutucuları `{your Scope ID}` `{your Device ID}`ve `{your Primary Key}` daha önce not aldığınız değerlerle güncelleştirin. Bu örnekte, sıfıra başlatmak, `targetTemperature` aygıttan geçerli okuma veya aygıt ikiz bir değer kullanabilirsiniz.

1. Azure IoT Central uygulamanıza benzetimli telemetri göndermek için dosyaya aşağıdaki işlevi ekleyin:

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

    Telemetri öğelerinin adları`temp` ( `humid`ve ) aygıt şablonunda kullanılan adlarıyla eşleşmelidir.

1. Azure IoT Merkezi uygulamanıza aygıt ikiz özellikleri göndermek için dosyanıza aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device twin reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

    IoT Central, aygıt ve IoT Merkezi uygulaması arasındaki özellik değerlerini eşitlemek için aygıt ikizlerini kullanır. Aygıt özellik değerleri aygıt ikiz bildirilen özellikleri kullanın. Yazılabilir özellikler hem aygıt ikiz ilerler ve istenilen özellikleri kullanın.

1. Cihazınızın yanıtlabildiği yazılabilir özellikleri tanımlamak ve işlemek için aşağıdaki kodu ekleyin:

    ```javascript
    // Add any writeable properties your device supports,
    // mapped to a function that's called when the writeable property
    // is updated in the IoT Central application.
    var writeableProperties = {
      'name': (newValue, callback) => {
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'brightness': (newValue, callback) => {
        setTimeout(() => {
            callback(newValue, 'completed');
        }, 5000);
      }
    };

    // Handle writeable property updates that come from IoT Central via the device twin.
    function handleWriteablePropertyUpdates(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (writeableProperties[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            writeableProperties[setting](desiredChange[setting].value, (newValue, status) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version
                }
              }
              sendDeviceProperties(twin, patch);
            });
          }
        }
      });
    }
    ```

    Operatör IoT Merkezi uygulamasında yazılabilir bir özellik ayarladığında, uygulama değeri aygıta göndermek için iki zıt bir aygıt kullanır. Aygıt daha sonra ikiz bildirilen bir aygıt özelliği kullanarak yanıt verir. IoT Central bildirilen özellik değerini aldığında, özellik görünümünü eşitlenmiş bir durumla **güncelleştirir.**

    Özelliklerin adları (`name` `brightness`ve ) aygıt şablonunda kullanılan adlar eşleşmelidir.

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

    Komutların`blink`adları ( , `turnon` `turnoff`, `rundiagnostics`, ve ) aygıt şablonunda kullanılan adlarla eşleşmelidir.

    Şu anda, IoT Central aygıt yeteneği modelinde tanımlanan yanıt şeasını kullanmıyor. Senkron bir komut için yanıt yükü geçerli bir JSON olabilir. Bir eşzamanlı komut için, aygıt hemen bir 202 yanıtı döndürmelidir ve ardından iş bittiğinde bildirilen özellik güncelleştirmesi. Bildirilen özellik güncelleştirmesinin biçimi:

    ```json
    {
      [command name] : {
        value: 'response message'
      }
    }
    ```

    Bir işleç komut geçmişinde yanıt yükünü görüntüleyebilir.

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
              state: 'true'
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

Aygıt istemcisi uygulamasını başlatmak için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
node environmentalSensor.js
```

Aygıtın Azure IoT Merkezi uygulamanıza bağlandığıve telemetri göndermeye başladığı nızı görebilirsiniz:

![İstemci uygulamasını çalıştırma](media/tutorial-connect-device-nodejs/run-application.png)

[!INCLUDE [iot-central-monitor-environmental-sensor](../../../includes/iot-central-monitor-environmental-sensor.md)]

Aygıtın komutlara ve özellik güncelleştirmelerine nasıl yanıt verebileceğini görebilirsiniz:

![İstemci uygulamasını gözlemleyin](media/tutorial-connect-device-nodejs/run-application-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Aygıt yeteneği modelleri ve kendi cihaz şablonlarınızı nasıl oluşturup oluşturabilirsiniz hakkında daha fazla bilgi edinmek için nasıl yapılacağınız kılavuzuna devam edin:

> [!div class="nextstepaction"]
> [Yeni bir IoT aygıt türü tanımlama](./howto-set-up-template.md)
