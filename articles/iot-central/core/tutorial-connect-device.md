---
title: Öğretici-Azure 'a genel bir Node. js istemci uygulaması bağlama IoT Central | Microsoft Docs
description: Bu öğreticide, bir cihaz geliştiricisi olarak bir Node. js istemci uygulaması çalıştıran bir cihaza Azure IoT Central uygulamanıza nasıl bağlanacağı gösterilmektedir. Cihaz yetenek modeli içeri aktararak ve bağlı bir cihazla etkileşime girebilen görünümler ekleyerek bir cihaz şablonu oluşturursunuz
author: dominicbetts
ms.author: dobett
ms.date: 02/26/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 1bcfc949eff0639dd1b4a063687e2c198f480ea3
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77624546"
---
# <a name="tutorial-create-and-connect-a-nodejs-client-application-to-your-azure-iot-central-application-nodejs"></a>Öğretici: node. js istemci uygulamasını oluşturma ve Azure IoT Central uygulamanıza bağlama (node. js)

Bu öğreticide, bir cihaz geliştiricisi olarak bir Node. js istemci uygulamasını Azure IoT Central uygulamanıza nasıl bağlayabilmeniz gösterilmektedir. Node. js uygulaması gerçek bir cihazın davranışının benzetimini yapar. IoT Central ' de bir _cihaz şablonu_ oluşturmak için bir ortam algılayıcı cihazı için örnek bir _cihaz yetenek modeli_ kullanın. Cihaz telemetrisini görselleştirmenize, cihaz özelliklerini yönetmenize ve cihazlarınızı denetlemek için komutları kullanmanıza olanak sağlamak üzere cihaz şablonuna görünümler eklersiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Cihaz şablonu oluşturmak için bir cihaz yetenek modeli içeri aktarın.
> * Varsayılan ve özel görünümleri bir cihaz şablonuna ekleyin.
> * Bir cihaz şablonu yayımlayın ve IoT Central uygulamanıza gerçek bir cihaz ekleyin.
> * Node. js Cihaz kodunu oluşturup çalıştırın ve IoT Central uygulamanıza bağlanın.
> * Cihazın gönderdiği sanal Telemetriyi görüntüleyin.
> * Cihaz özelliklerini yönetmek için bir görünüm kullanın.
> * Cihazı denetlemek için komutları çağırın.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* \* * Özel uygulama * * şablonu kullanılarak oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* [Node. js](https://nodejs.org/) sürüm 10.0.0 veya üzeri yüklü bir geliştirme makinesi. Sürümünüzü denetlemek için komut satırında `node --version` çalıştırabilirsiniz. Node.js çeşitli işletim sistemleri için kullanılabilir. Bu öğreticideki yönergeler, Windows komut isteminde **node** komutunu çalıştırmakta olduğunuzu varsayar. Node. js ' nin çeşitli işletim sistemlerinde kullanılmasını sağlayabilirsiniz.

## <a name="create-a-device-template"></a>Cihaz şablonu oluşturma

Yerel makinenizde `environmental-sensor` adlı bir klasör oluşturun.

[Ortam algılayıcısı yetenek modeli](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) json dosyasını indirin ve `environmental-sensor` klasörüne kaydedin.

`{YOUR_COMPANY_NAME_HERE}` iki örneğini indirdiğiniz `EnvironmentalSensorInline.capabilitymodel.json` dosyasında şirket adınızla değiştirmek için bir metin düzenleyicisi kullanın.

Azure IoT Central uygulamanızda, `EnvironmentalSensorInline.capabilitymodel.json` cihaz yetenek modeli dosyasını içeri aktararak *çevresel algılayıcı* adlı bir cihaz şablonu oluşturun:

![İçeri aktarılan cihaz özelliği modeliyle cihaz şablonu](./media/tutorial-connect-device/device-template.png)

Cihaz yetenek modeli iki arabirim içerir: standart **cihaz bilgileri** arabirimi ve özel **çevresel algılayıcı** arabirimi. **Çevresel algılayıcı** arabirimi aşağıdaki özellikleri tanımlar:

| Tür | Görünen Ad | Açıklama |
| ---- | ------------ | ----------- |
| Özellik | Cihaz Durumu     | Cihazın durumu. Çevrimiçi/çevrimdışı iki durum kullanılabilir. |
| Özellik | Müşteri adı    | Müşterinin Şu anda cihazda çalışan adı. |
| Özellik | Parlaklık düzeyi | Cihazdaki ışığın parlaklık düzeyi. 1 (yüksek), 2 (orta), 3 (düşük) olarak belirtilebilir. |
| Telemetri | Sıcaklık | Cihazdaki geçerli sıcaklık. |
| Telemetri | Nem oranı    | Cihazda geçerli nem. |
| Komut | /          | Belirli bir zaman aralığı için yeniden yanıp sönbaşla. |
| Komut | turnon         | Cihazdaki ışığı açın. |
| Komut | kapatma        | Cihazdaki ışığı devre dışı bırakın. |
| Komut | rundiagnostics | Bu komut bir tanılama çalıştırması başlatır. |

**Cihaz durumu** özelliğinin IoT Central uygulamanızda nasıl görüntüleneceğini özelleştirmek için cihaz şablonunda **Özelleştir** ' i seçin. **Cihaz durumu** girişini genişletin, **doğru** ad olarak _çevrimiçi_ ve **yanlış ad**olarak _çevrimdışı_ girin. Sonra değişiklikleri kaydedin:

![Cihaz şablonunu özelleştirme](media/tutorial-connect-device/customize-template.png)

## <a name="create-views"></a>Görünümleri oluşturma

Görünümler, IoT Central uygulamanıza bağlı cihazlarla etkileşime geçmesini sağlar. Örneğin, Telemetriyi görüntüleyen görünümleriniz, özellikleri görüntüleyen görünümler ve yazılabilir ve bulut özelliklerini düzenlemenize olanak sağlayan görünümler bulunabilir. Görünümler bir cihaz şablonunun parçasıdır.

**Ortam algılayıcı** cihaz şablonunuza bazı varsayılan görünümler eklemek için cihaz şablonunuza gidin, **Görünümler**' i seçin ve **varsayılan görünümleri oluştur** kutucuğunu seçin. **Genel bakış** ve **hakkında** ' ün **Açık**olduğundan emin olun ve ardından **varsayılan pano görünümlerini oluştur**' u seçin. Artık, şablonunuzda tanımlanmış iki varsayılan görünümünüz vardır.

**Çevresel algılayıcı** arabirimi iki yazılabilir Özellik Içerir- **müşteri adı** ve **parlaklık düzeyi**. Bir görünüm oluşturmak için bu özellikleri düzenlemek üzere kullanabilirsiniz:

1. **Görünümler** ' i seçin ve ardından **cihazı ve bulut verilerini Düzenle** kutucuğunu seçin.

1. _Özellikleri_ form adı olarak girin.

1. **Parlaklık düzeyini** ve **müşteri adı** özelliklerini seçin. Ardından **Bölüm Ekle**' yi seçin.

1. Yaptığınız değişiklikleri kaydedin.

![Özellik düzenlemesini etkinleştirmek için bir görünüm ekleyin](media/tutorial-connect-device/properties-view.png)

## <a name="publish-the-template"></a>Şablonu yayımlama

**Ortam algılayıcısı** cihaz şablonunu kullanan IoT Central uygulamanıza bir cihaz eklemeden önce, şablonu yayımlamanız gerekir.

Cihaz şablonunda **Yayımla**' yı seçin. Yayımlanacak değişiklikleri gösteren panelde **Yayımla**' yı seçin.

Şablonun kullanıma hazırsa emin olmak için IoT Central uygulamanızdaki **cihazlar** sayfasına gidin. **Cihazlar** bölümü, uygulamadaki yayınlanan cihazların bir listesini gösterir:

![Cihazlar sayfasında yayımlanan şablonlar](media/tutorial-connect-device/published-templates.png)

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IoT Central uygulamanızda, önceki bölümde oluşturduğunuz cihaz şablonuna gerçek bir cihaz ekleyin:

1. **Cihazlar** sayfasında, **çevresel algılayıcı** cihaz şablonunu seçin.

1. **+ Yeni**seçeneğini belirleyin.

1. **Benzetimli** 'un **kapalı**olduğundan emin olun. Ardından **Oluştur**’u seçin.

Cihaz adına tıklayın ve ardından **Bağlan**' ı seçin. Cihaz **bağlantısı** sayfa **kimliği KAPSAMı**, **cihaz kimliği**ve **birincil anahtar**üzerindeki cihaz bağlantı bilgilerini bir yere unutmayın. Cihaz kodunuzu oluştururken bu değerlere ihtiyacınız vardır:

![Cihaz bağlantı bilgileri](./media/tutorial-connect-device/device-connection.png)

### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Aşağıdaki adımlarda, uygulamaya eklediğiniz gerçek cihazı uygulayan bir Node. js istemci uygulamasının nasıl oluşturulacağı gösterilmektedir. Bu Node. js uygulaması gerçek bir cihazın davranışının benzetimini yapar.

1. Komut satırı ortamınızda, daha önce oluşturduğunuz `environmental-sensor` klasöre gidin.

1. Node. js projenizi başlatmak ve gerekli bağımlılıkları yüklemek için aşağıdaki komutları çalıştırın: `npm init`çalıştırdığınızda tüm varsayılan seçenekleri kabul edin:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt azure-iot-provisioning-device-mqtt azure-iot-security-symmetric-key --save
    ```

1. `environmental-sensor` klasöründe **Environmentalalgılayıcı. js** adlı bir dosya oluşturun.

1. **Environmentalalgılayıcı. js** dosyasının başlangıcında aşağıdaki `require` deyimlerini ekleyin:

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

    `{your Scope ID}`, `{your Device ID}`ve `{your Primary Key}` yer tutucuları, daha önce bir nota yaptığınız değerlerle güncelleştirin. Bu örnekte, `targetTemperature` sıfırdan başlatın, cihazdaki geçerli okumayı veya cihazdan ikizi bir değeri kullanabilirsiniz.

1. Azure IoT Central uygulamanıza telemetri göndermek için, dosyaya aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device measurements.
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

1. Azure IoT Central uygulamanıza cihaz özellikleri göndermek için, dosyanıza aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Cihazınızın yanıt verdiği yazılabilir özellikleri tanımlamak ve işlemek için aşağıdaki kodu ekleyin:

    ```javascript
    // Add any writeable properties your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
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

    // Handle writeable property changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              sendDeviceProperties(twin, patch);
            });
          }
        }
      });
    }
    ```

1. IoT Central uygulamasından gönderilen komutları işlemek için aşağıdaki kodu ekleyin:

    ```javascript
    // Handle blink command
    function onBlink(request, response) {
      console.log('Received asynchronous call to blink');
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          console.log('Blinking LED every ' + request.payload  + ' seconds');
        }
      });
    }

    // Handle LED turn on command
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

    // Handle LED turn off command
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

    // Handle sensor diagnostics command
    function diagnostics(request, response) {
      console.log('Received asynchronous call to run diagnostics');
      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          console.log('Running diagnostics...');
        }
      });
    }
    ```

1. Azure IoT Central bağlantısını tamamlamak ve istemci kodundaki işlevleri bağlamak için aşağıdaki kodu ekleyin:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');
        // Create handlers for commands
        hubClient.onDeviceMethod('blink', onBlink);
        hubClient.onDeviceMethod('turnon', turnOn);
        hubClient.onDeviceMethod('turnoff', turnOff);
        hubClient.onDeviceMethod('rundiagnostics', diagnostics);
        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);
        // Get device twin from Azure IoT Central.
        hubClient.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send Environmental Sensor device properties once on device start up.
            var properties = {
              state: 'true'
            };
            sendDeviceProperties(twin, properties);
            handleSettings(twin);
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

## <a name="run-your-nodejs-application"></a>Node. js uygulamanızı çalıştırma

Cihaz istemci uygulamasını başlatmak için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
node environmentalSensor.js
```

Cihazın Azure IoT Central uygulamanıza bağlandığını görebilir ve telemetri göndermeye başlar:

![İstemci uygulamasını çalıştırma](media/tutorial-connect-device/run-application.png)

Azure IoT Central uygulamanızda bir operatör olarak şunları yapabilirsiniz:

* **Genel bakış** sayfasında cihaz tarafından gönderilen Telemetriyi görüntüleyin:

    ![Telemetri görüntüleme](media/tutorial-connect-device/view-telemetry.png)

* **Özellikler** sayfasında yazılabilir özellik değerlerini güncelleştir:

    ![Güncelleştirme özellikleri](media/tutorial-connect-device/update-properties.png)

    ![Güncelleştirme özellikleri cihazı](media/tutorial-connect-device/update-properties-device.png)

* **Komutlar sayfasından komutları** çağırın:

    ![BLINK komutunu çağır](media/tutorial-connect-device/call-command.png)

    ![BLINK komut cihazını çağır](media/tutorial-connect-device/call-command-device.png)

* **Hakkında** sayfasında cihaz özelliklerini görüntüleyin:

    ![Özellikleri görüntüle](media/tutorial-connect-device/about-properties.png)

## <a name="next-steps"></a>Sonraki adımlar

Cihaz yetenek modelleri ve kendi cihaz şablonlarınızı oluşturma hakkında daha fazla bilgi edinmek için nasıl yapılır kılavuzuna ilerleyin:

> [!div class="nextstepaction"]
> [Yeni bir IoT cihaz türü tanımla](./howto-set-up-template.md)
