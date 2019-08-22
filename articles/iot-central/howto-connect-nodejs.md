---
title: Genel bir Node. js istemci uygulamasını Azure IoT Central bağlama | Microsoft Docs
description: Bir cihaz geliştiricisi olarak, genel bir Node. js cihazını Azure IoT Central uygulamanıza bağlama.
author: dominicbetts
ms.author: dobett
ms.date: 06/14/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 3b73344a233182fe8366795cfa111b706c6d06ac
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876252"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Genel bir istemci uygulamasını Azure IoT Central uygulamanıza bağlama (node. js)

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Bu makalede, bir cihaz geliştiricisi olarak, Microsoft Azure IoT Central uygulamanıza gerçek bir cihazı temsil eden genel bir Node. js uygulamasının nasıl bağlanacağı açıklanır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Azure IoT Central uygulaması. Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central.md).
1. [Node. js](https://nodejs.org/) sürüm 4.0.0 veya üzeri yüklü bir geliştirme makinesi. Sürümünüzü denetlemek için `node --version` komut satırında çalıştırabilirsiniz. Node.js çeşitli işletim sistemleri için kullanılabilir.

## <a name="create-a-device-template"></a>Cihaz şablonu oluşturma

Azure IoT Central uygulamanızda, aşağıdaki ölçümler, cihaz özellikleri, ayarlar ve komutları içeren bir cihaz şablonuna ihtiyacınız vardır:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

**Ölçümler** sayfasına aşağıdaki Telemetriyi ekleyin:

| Görünen Ad | Alan Adı  | Birimler | Min | Maks | Ondalık Basamaklar |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Sıcaklık  | sıcaklık | C     | 60  | 110 | 0              |
| Nem     | Nem oranı    | %     | 0   | 100 | 0              |
| Basınç     | basınç    | kPa   | 80  | 110 | 0              |

> [!NOTE]
> Telemetri ölçüsünün veri türü bir kayan noktalı sayıdır.

Alan adlarını tam olarak tabloda gösterildiği gibi, cihaz şablonunda girin. Alan adları karşılık gelen aygıt kodundaki Özellik adlarıyla eşleşmezse telemetri uygulamada görüntülenemez.

### <a name="state-measurements"></a>Durum ölçümleri

**Ölçümler** sayfasına aşağıdaki durumu ekleyin:

| Görünen Ad | Alan Adı  | Değer 1 | Görünen Ad | Değer 2 | Görünen Ad |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Fan Modu     | fanmode     | 1\.       | Çalışıyor      | 0       | Durduruldu      |

> [!NOTE]
> Durum ölçüsünün veri türü dizedir.

Alan adlarını tam olarak tabloda gösterildiği gibi, cihaz şablonunda girin. Alan adları karşılık gelen aygıt kodundaki Özellik adlarıyla eşleşmezse, durum uygulamada görüntülenemez.

### <a name="event-measurements"></a>Olay ölçümleri

**Ölçümler** sayfasına aşağıdaki olayı ekleyin:

| Görünen Ad | Alan Adı  | severity |
| ------------ | ----------- | -------- |
| Aşırı ısınmasını  | aşırı ısı    | Hata    |

> [!NOTE]
> Olay ölçüsünün veri türü dizedir.

### <a name="location-measurements"></a>Konum ölçümleri

**Ölçümler** sayfasına aşağıdaki konum ölçümünü ekleyin:

| Görünen Ad | Alan Adı  |
| ------------ | ----------- |
| Location     | location    |

Konum ölçümü veri türü, boylam ve enlem için iki kayan nokta numarasından oluşur ve yükseklik için isteğe bağlı bir kayan nokta sayısıdır.

Alan adlarını tam olarak tabloda gösterildiği gibi, cihaz şablonunda girin. Alan adları karşılık gelen aygıt kodundaki Özellik adlarıyla eşleşmiyorsa, konum uygulamada görüntülenemez.

### <a name="device-properties"></a>Cihaz özellikleri

Aşağıdaki cihaz özelliklerini **Özellikler** sayfasına ekleyin:

| Görünen Ad        | Alan Adı        | Veri türü |
| ------------------- | ----------------- | --------- |
| Seri Numarası       | serialNumber      | text      |
| Cihaz üreticisi | üretici      | text      |

Alan adlarını tam olarak tabloda gösterildiği gibi, cihaz şablonunda girin. Alan adları karşılık gelen aygıt kodundaki Özellik adlarıyla eşleşmezse, Özellikler uygulamada görüntülenemez.

### <a name="settings"></a>Ayarlar

**Ayarlar** sayfasına aşağıdaki **sayı** ayarlarını ekleyin:

| Görünen Ad    | Alan Adı     | Birimler | In | Min | Maks  | İlk |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Fan hızı       | Fanın hızı       | RPM   | 0        | 0   | 3000 | 0       |
| Sıcaklığı Ayarla | setTemperature | C     | 0        | 20  | 200  | 80      |

Alan adını tam olarak tabloda gösterildiği gibi, cihaz şablonunda girin. Alan adları karşılık gelen aygıt kodundaki Özellik adlarıyla eşleşmezse, cihaz ayar değerini alamıyor.

### <a name="commands"></a>Komutlar

**Komutlar** sayfasına aşağıdaki komutu ekleyin:

| Görünen Ad    | Alan Adı     | Varsayılan Zaman Aşımı | Veri Türü |
| --------------- | -------------- | --------------- | --------- |
| Sayıma       | sayıma      | 30              | numarası    |

Aşağıdaki giriş alanını geri sayım komutuna ekleyin:

| Görünen Ad    | Alan Adı     | Veri Türü | Value |
| --------------- | -------------- | --------- | ----- |
| Sayım      | Sayaçdan      | numarası    | 10    |

Yalnızca tablolarda gösterildiği gibi, alan adlarını cihaz şablonuna girin. Alan adları karşılık gelen aygıt kodundaki Özellik adlarıyla eşleşmezse, cihaz komutu işleyemez.

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IoT Central uygulamanızda, önceki bölümde oluşturduğunuz cihaz şablonuna gerçek bir cihaz ekleyin.

Ardından "cihaz ekleme" öğreticisindeki yönergeleri izleyerek [gerçek cihaza yönelik bir bağlantı dizesi oluşturun](tutorial-add-device.md#generate-connection-string). Bu bağlantı dizesini aşağıdaki bölümde kullanacaksınız:

### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Aşağıdaki adımlarda, uygulamaya eklediğiniz gerçek cihazı uygulayan bir istemci uygulamasının nasıl oluşturulacağı gösterilmektedir. Burada Node. js uygulaması gerçek cihazı temsil eder. 

1. Makinenizde `connected-air-conditioner-adv` adlı bir klasör oluşturun. Komut satırı ortamınızda bu klasöre gidin.

1. Node. js projenizi başlatmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. `connected-air-conditioner-adv` Klasöründe **connectedAirConditionerAdv. js** adlı bir dosya oluşturun.

1. `require` **ConnectedAirConditionerAdv. js** dosyasının başlangıcına aşağıdaki deyimlerini ekleyin:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Aşağıdaki değişken bildirimlerini dosyaya ekleyin:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var locLong = -122.1215;
    var locLat = 47.6740;
    var client = clientFromConnectionString(connectionString);
    ```

    Yer tutucusunu `{your device connection string}` [Cihaz bağlantı dizesiyle](tutorial-add-device.md#generate-connection-string)güncelleştirin. Bu örnekte, sıfırdan başlatın `targetTemperature` , cihazdaki geçerli okumayı veya cihazdan ikizi bir değeri kullanabilirsiniz.

1. Azure IoT Central uygulamanıza telemetri, durum, olay ve konum ölçümleri göndermek için aşağıdaki işlevi dosyasına ekleyin:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var locationLong = locLong - (Math.random() / 100);
      var locationLat = locLat - (Math.random() / 100);
      var data = JSON.stringify({
        temperature: temperature,
        humidity: humidity,
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined,
        location: {
            lon: locationLong,
            lat: locationLat }
        });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
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

1. Cihazınızın yanıt verdiği ayarları tanımlamak için aşağıdaki tanımı ekleyin:

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Azure IoT Central uygulamanızdan güncelleştirilmiş ayarları işlemek için aşağıdakileri dosyasına ekleyin:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
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
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. IoT Central uygulamasından gönderilen bir geri sayım komutunu işlemek için aşağıdaki kodu ekleyin:

    ```javascript
    // Handle countdown command
    function onCountdown(request, response) {
      console.log('Received call to countdown');

      var countFrom = (typeof(request.payload.countFrom) === 'number' && request.payload.countFrom < 100) ? request.payload.countFrom : 10;

      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          client.getTwin((err, twin) => {
            function doCountdown(){
              if ( countFrom >= 0 ) {
                var patch = {
                  countdown:{
                    value: countFrom
                  }
                };
                sendDeviceProperties(twin, patch);
                countFrom--;
                setTimeout(doCountdown, 2000 );
              }
            }

            doCountdown();
          });
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

        // Create handler for countdown command
        client.onDeviceMethod('countdown', onCountdown);

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            var properties = {
              serialNumber: '123-ABC',
              manufacturer: 'Contoso'
            };
            sendDeviceProperties(twin, properties);

            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (connect it to Azure IoT Central).
    client.open(connectCallback);
    ```

## <a name="run-your-nodejs-application"></a>Node. js uygulamanızı çalıştırma

Komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
node connectedAirConditionerAdv.js
```

Azure IoT Central uygulamanızda, gerçek cihazınız için bir operatör olarak şunları yapabilirsiniz:

* **Ölçümler** sayfasında Telemetriyi görüntüleyin:

    ![Telemetri görüntüleme](media/howto-connect-nodejs/viewtelemetry.png)

* **Ölçümler** sayfasında konumu görüntüleyin:

    ![Konum ölçümlerini görüntüle](media/howto-connect-nodejs/viewlocation.png)

* Cihazınızdan gönderilen cihaz özelliği değerlerini **Özellikler** sayfasında görüntüleyin. Cihazın bağlandığı cihaz özelliği kutucukları güncelleştir:

    ![Cihaz özelliklerini görüntüle](media/howto-connect-nodejs/viewproperties.png)

* **Ayarlar** sayfasından fan hızını ve hedef sıcaklığını ayarlayın:

    ![Fan hızını ayarla](media/howto-connect-nodejs/setfanspeed.png)

* **Komutlar** sayfasından geri sayım komutunu çağırın:

    ![Çağrı geri sayımı komutu](media/howto-connect-nodejs/callcountdown.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamanıza genel bir Node. js istemcisi bağlamayı öğrendiğinize göre, önerilen sonraki adım, kendi IoT cihazınız için [özel bir cihaz şablonu ayarlamayı](howto-set-up-template.md) öğrenmektedir.
