---
title: Öğretici - Genel bir Node.js istemci uygulamasını Azure IoT Central'a bağlayın | Microsoft Dokümanlar
description: Bu öğretici, bir aygıt geliştiricisi olarak, Node.js istemci uygulaması çalıştıran bir aygıtı Azure IoT Merkezi uygulamanıza nasıl bağlayabildiğinizi gösterir. Aygıt yeteneği modelini içe aktararak bir aygıt şablonu oluşturur ve bağlı bir aygıtla etkileşim kurmanıza izin veren görünümler eklersiniz
author: dominicbetts
ms.author: dobett
ms.date: 02/26/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 1bcfc949eff0639dd1b4a063687e2c198f480ea3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77624546"
---
# <a name="tutorial-create-and-connect-a-nodejs-client-application-to-your-azure-iot-central-application-nodejs"></a>Öğretici: Bir Düğüm.js istemci sebeb uygulaması oluşturun ve Azure IoT Merkezi uygulamanıza bağlayın (Node.js)

Bu öğretici, bir aygıt geliştiricisi olarak Bir Düğüm.js istemci uygulamasını Azure IoT Merkezi uygulamanıza nasıl bağlayacaklarını gösterir. Düğüm.js uygulaması gerçek bir aygıtın davranışını simüle eder. IoT _Central'da_ bir _aygıt şablonu_ oluşturmak için bir çevre sensörü aygıtı için örnek bir aygıt yeteneği modeli kullanırsınız. Aygıt telemetrisini görselleştirmenize, aygıt özelliklerini yönetmenize ve aygıtlarınızı denetlemek için komutları kullanmanıza izin vermek için aygıt şablonuna görünümler eklersiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Aygıt şablonu oluşturmak için aygıt yeteneği modelini aktarın.
> * Aygıt şablonuna varsayılan ve özel görünümler ekleyin.
> * Bir aygıt şablonu yayımlayın ve IoT Central uygulamanıza gerçek bir aygıt ekleyin.
> * Düğüm.js aygıt kodunu oluşturun ve çalıştırın ve IoT Central uygulamanıza bağlandığına bakın.
> * Aygıtın gönderdiği simüle edilmiş telemetriyi görüntüleyin.
> * Aygıt özelliklerini yönetmek için bir görünüm kullanın.
> * Aygıtı kontrol etmek için komutları arayın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

* **Özel uygulama **şablonu kullanılarak oluşturulan bir Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
* [Node.js](https://nodejs.org/) sürüm 10.0.0 veya daha sonra yüklü bir geliştirme makinesi. Sürümünüzü `node --version` kontrol etmek için komut satırında çalıştırabilirsiniz. Node.js çeşitli işletim sistemleri için kullanılabilir. Bu öğreticideki talimatlar, Windows komut isteminde **düğüm** komutunu çalıştırdığınızı varsayar. Node.js'yi çeşitli işletim sistemlerinde kullanabilirsiniz.

## <a name="create-a-device-template"></a>Cihaz şablonu oluşturma

Yerel makinenizde `environmental-sensor` çağrılan bir klasör oluşturun.

Çevre [sensörü yetenek modeli](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) JSON dosyasını `environmental-sensor` indirin ve klasöre kaydedin.

İndirdiğiniz `EnvironmentalSensorInline.capabilitymodel.json` dosyadaki iki örneği şirket `{YOUR_COMPANY_NAME_HERE}` adınız ile değiştirmek için bir metin düzenleyicisi kullanın.

Azure IoT Merkezi uygulamanızda, aygıt *Environmental sensor* yetenek modeli dosyasını `EnvironmentalSensorInline.capabilitymodel.json` içe aktararak Çevre sensörü adı verilen bir aygıt şablonu oluşturun:

![İçe aktarılan aygıt yetenek modeline sahip aygıt şablonu](./media/tutorial-connect-device/device-template.png)

Cihaz özelliği modeli iki arabirim içerir: standart **Aygıt Bilgileri** arabirimi ve özel **Çevre Sensörü** arabirimi. **Çevre Sensörü** arabirimi aşağıdaki yetenekleri tanımlar:

| Tür | Görünen Ad | Açıklama |
| ---- | ------------ | ----------- |
| Özellik | Cihaz Durumu     | Aygıtın durumu. Çevrimiçi/çevrimdışı iki durum mevcuttur. |
| Özellik | Müşteri Adı    | Aygıtı şu anda çalıştıran müşterinin adı. |
| Özellik | Parlaklık Düzeyi | Cihazdaki ışığın parlaklık düzeyi. 1 (yüksek), 2 (orta), 3 (düşük) olarak belirtilebilir. |
| Telemetri | Sıcaklık | Cihazdaki geçerli sıcaklık. |
| Telemetri | Nem oranı    | Cihazdaki geçerli nem. |
| Komut | Yanıp          | Belirli bir zaman aralığı için LED'i yanıp sönmeye başlayın. |
| Komut | turnon         | Cihazın LED ışığını açın. |
| Komut | kapatma        | Cihazın LED ışığını kapatın. |
| Komut | rundiagnostics | Bu komut bir tanılama çalışması başlatır. |

**IoT** Central uygulamanızda Aygıt Durumu özelliğinin nasıl görüntülenebildiğini özelleştirmek için aygıt şablonunda **Özelleştir'i** seçin. Aygıt **Durumu** girişini genişletin, **Doğru ad** olarak _Çevrimiçi'yi_ ve **False adı**olarak _Çevrimdışı_ girin. Ardından değişiklikleri kaydedin:

![Aygıt şablonu özelleştirme](media/tutorial-connect-device/customize-template.png)

## <a name="create-views"></a>Görünümler oluşturma

Görünümler, IoT Central uygulamanıza bağlı aygıtlarla etkileşim kurmanıza izin verir. Örneğin, telemetriyi görüntüleyen görünümler, özellikleri görüntüleyen görünümler ve yazılabilir ve bulut özelliklerini yeniden deletmenize izin veren görünümler olabilir. Görünümler aygıt şablonunun bir parçasıdır.

**Çevre sensörü** aygıt şablonuna bazı varsayılan görünümler eklemek için aygıt şablonunuza gidin, **Görünümler'i**seçin ve **Varsayılan Görünümler oluştur** döşemesini seçin. **Genel Bakış** ve **Hakkında'nın** **Açık**olduğundan emin olun ve ardından varsayılan **pano görünümlerini oluştur'u**seçin. Artık şablonunuzda tanımlanan iki varsayılan görünüm var.

**Çevre Sensörü** arabirimi iki yazılabilir özellik içerir - **Müşteri Adı** ve **Parlaklık Düzeyi.** Bir görünüm oluşturmak için, bu özellikleri yeniden kullanmak için kullanabilirsiniz:

1. **Görünümler'i** seçin ve ardından **Düzenleme aygıtını ve bulut veri** döşemesini seçin.

1. _Özellikler'i_ form adı olarak girin.

1. Parlaklık **Düzeyi** ve **Müşteri Adı** özelliklerini seçin. Ardından **Ekle bölümünü**seçin.

1. Yaptığınız değişiklikleri kaydedin.

![Özellik düzenlemeyi etkinleştirmek için görünüm ekleme](media/tutorial-connect-device/properties-view.png)

## <a name="publish-the-template"></a>Şablonu yayımlama

**Çevre sensörü** aygıtı şablonunu kullanan IoT Central uygulamanıza bir aygıt eklemeden önce şablonu yayımlamanız gerekir.

Aygıt şablonunda **Yayımla'yı**seçin. Yayımlanacak değişiklikleri gösteren panelde **Yayımla'yı**seçin.

Şablonun kullanıma hazır olup olmadığını kontrol etmek için, IoT Merkezi uygulamanızdaki **Cihazlar** sayfasına gidin. **Cihazlar** bölümünde, uygulamada yayınlanan aygıtların bir listesi gösterilmektedir:

![Aygıtlar sayfasında yayınlanan şablonlar](media/tutorial-connect-device/published-templates.png)

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IoT Merkezi uygulamanızda, önceki bölümde oluşturduğunuz aygıt şablonuna gerçek bir aygıt ekleyin:

1. **Cihazlar** sayfasında **Çevre sensörü** aygıtı şablonuna tıklayın.

1. Seçin **+ Yeni**.

1. **Benzetimkapalı** olduğundan **Off**emin olun. Ardından **Oluştur**’u seçin.

Aygıt adını tıklatın ve sonra **Bağlan'ı**seçin. **Aygıt Bağlantısı** sayfasındaki aygıt bağlantısı bilgilerini not edin - **kimlik kapsamı,** **Aygıt Kimliği**ve **Birincil anahtar**. Aygıt kodunuzu oluştururken bu değerlere ihtiyacınız vardır:

![Cihaz bağlantı bilgileri](./media/tutorial-connect-device/device-connection.png)

### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Aşağıdaki adımlar, uygulamaya eklediğiniz gerçek aygıtı uygulayan bir Düğüm istemcisi uygulamasını nasıl oluşturacağınızı gösterir. Bu Düğüm.js uygulaması gerçek bir aygıtın davranışını simüle eder.

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

1. Azure IoT Merkezi uygulamanıza telemetri göndermek için dosyaya aşağıdaki işlevi ekleyin:

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

1. Aygıt özelliklerini Azure IoT Merkezi uygulamanıza göndermek için dosyanıza aşağıdaki işlevi ekleyin:

    ```javascript
    // Send device reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Cihazınızın yanıtlabildiği yazılabilir özellikleri tanımlamak ve işlemek için aşağıdaki kodu ekleyin:

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

## <a name="run-your-nodejs-application"></a>Node.js uygulamanızı çalıştırın

Aygıt istemcisi uygulamasını başlatmak için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
node environmentalSensor.js
```

Aygıtın Azure IoT Merkezi uygulamanıza bağlandığıve telemetri göndermeye başladığı nızı görebilirsiniz:

![İstemci uygulamasını çalıştırma](media/tutorial-connect-device/run-application.png)

Azure IoT Merkezi uygulamanızda bir operatör olarak şunları yapabilirsiniz:

* Cihaz tarafından gönderilen telemetriyi **Genel Bakış** sayfasında görüntüleyin:

    ![Telemetri görüntüleme](media/tutorial-connect-device/view-telemetry.png)

* **Özellikler** sayfasında yazılabilir özellik değerlerini güncelleştirin:

    ![Özellikleri güncelleştirme](media/tutorial-connect-device/update-properties.png)

    ![Özellikleri aygıtı güncelleştirme](media/tutorial-connect-device/update-properties-device.png)

* **Komutlar** sayfasından komutları arayın:

    ![Göz kırpma komutunu arayın](media/tutorial-connect-device/call-command.png)

    ![Blink komut cihazını arayın](media/tutorial-connect-device/call-command-device.png)

* Cihaz özelliklerini **Hakkında** sayfasında görüntüleyin:

    ![Özellikleri görüntüle](media/tutorial-connect-device/about-properties.png)

## <a name="next-steps"></a>Sonraki adımlar

Aygıt yeteneği modelleri ve kendi cihaz şablonlarınızı nasıl oluşturup oluşturabilirsiniz hakkında daha fazla bilgi edinmek için nasıl yapılacağınız kılavuzuna devam edin:

> [!div class="nextstepaction"]
> [Yeni bir IoT aygıt türü tanımlama](./howto-set-up-template.md)
