---
title: Node.js-Azure 'da uzak Izlemeye cihaz sağlama | Microsoft Docs
description: Node.js ' de yazılmış bir uygulama kullanarak bir cihazı uzaktan Izleme çözümü hızlandırıcısına bağlamayı açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: dobett
ms.custom: mqtt, devx-track-javascript
ms.openlocfilehash: ab697778e00def1e23a3124b14458f1acb994041
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87423363"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-nodejs"></a>Cihazınızı uzaktan Izleme çözümü hızlandırıcısına bağlama (Node.js)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, gerçek bir cihazın uzaktan Izleme çözüm hızlandırıcısına nasıl bağlanacağı gösterilmektedir. Bu öğreticide, en az kaynak kısıtlamalarına sahip ortamlar için iyi bir seçenek olan Node.js kullanırsınız.

Bir cihazın benzetimini yapmayı tercih ediyorsanız, bkz. [Yeni bir sanal cihaz oluşturma ve test](iot-accelerators-remote-monitoring-create-simulated-device.md)etme.

## <a name="create-a-nodejs-solution"></a>Node.js çözümü oluşturma

Geliştirme makinenizde [Node.js](https://nodejs.org/) Version 4.0.0 veya üzeri sürümünün yüklü olduğundan emin olun. `node --version`Sürümü denetlemek için komut satırından çalıştırabilirsiniz.

1. Geliştirme makinenizde adlı bir klasör oluşturun `remotemonitoring` . Komut satırı ortamınızda bu klasöre gidin.

1. Örnek uygulamayı doldurmanız için gereken paketleri indirmek ve yüklemek için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm init
    npm install async azure-iot-device azure-iot-device-mqtt --save
    ```

1. `remotemonitoring`Klasöründe **remote_monitoring.js**adlı bir dosya oluşturun. Bu dosyayı bir metin düzenleyicisinde açın.

1. **remote_monitoring.js** dosyasında aşağıdaki `require` deyimleri ekleyin:

    ```javascript
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var async = require('async');
    ```

1. `require` deyimlerinden sonra aşağıdaki değişken bildirimlerini ekleyin. Yer tutucu değerini, `{device connection string}` Uzaktan izleme çözümünde sağladığınız cihaz için not ettiğiniz değerle değiştirin:

    ```javascript
    var connectionString = '{device connection string}';
    ```

1. Bazı temel telemetri verilerini tanımlamak için aşağıdaki değişkenleri ekleyin:

    ```javascript
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Bazı özellik değerlerini tanımlamak için aşağıdaki değişkenleri ekleyin:

    ```javascript
    var schema = "real-chiller;v1";
    var deviceType = "RealChiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    var deviceOnline = true;
    ```

1. Çözüme gönderilmek üzere bildirilen özellikleri tanımlamak için aşağıdaki değişkeni ekleyin. Bu özellikler, Web Kullanıcı arabiriminde görüntülenecek meta verileri içerir:

    ```javascript
    var reportedProperties = {
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        [schema]: ""
      },
      "Type": deviceType,
      "Firmware": deviceFirmware,
      "FirmwareUpdateStatus": deviceFirmwareUpdateStatus,
      "Location": deviceLocation,
      "Latitude": deviceLatitude,
      "Longitude": deviceLongitude,
      "Online": deviceOnline
    }
    ```

1. İşlem sonuçlarını yazdırmak için aşağıdaki yardımcı işlevi ekleyin:

    ```javascript
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Telemetri değerlerini rastgele olarak kullanmak için aşağıdaki yardımcı işlevi ekleyin:

     ```javascript
     function generateRandomIncrement() {
         return ((Math.random() * 2) - 1);
     }
     ```

1. Çözümden doğrudan Yöntem çağrılarını işlemek için aşağıdaki genel işlevi ekleyin. İşlevi, çağrılan doğrudan yöntem hakkında bilgi görüntüler, ancak bu örnekte cihazı hiçbir şekilde değiştirmez. Çözüm, cihazlar üzerinde işlem yapmak için doğrudan yöntemler kullanır:

     ```javascript
     function onDirectMethod(request, response) {
       // Implement logic asynchronously here.
       console.log('Simulated ' + request.methodName);

       // Complete the response
       response.send(200, request.methodName + ' was called on the device', function (err) {
         if (err) console.error('Error sending method response :\n' + err.toString());
         else console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');
       });
     }
     ```

1. Çözümden **Firmwareupdate** Direct Yöntem çağrılarını işlemek için aşağıdaki işlevi ekleyin. İşlevi doğrudan yöntem yükünde geçirilen parametreleri doğrular ve ardından bir bellenim güncelleştirme simülasyonu zaman uyumsuz olarak çalıştırır:

     ```javascript
     function onFirmwareUpdate(request, response) {
       // Get the requested firmware version from the JSON request body
       var firmwareVersion = request.payload.Firmware;
       var firmwareUri = request.payload.FirmwareUri;
      
       // Ensure we got a firmware values
       if (!firmwareVersion || !firmwareUri) {
         response.send(400, 'Missing firmware value', function(err) {
           if (err) console.error('Error sending method response :\n' + err.toString());
           else console.log('400 Response to method \'' + request.methodName + '\' sent successfully.');
         });
       } else {
         // Respond the cloud app for the device method
         response.send(200, 'Firmware update started.', function(err) {
           if (err) console.error('Error sending method response :\n' + err.toString());
           else {
             console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');

             // Run the simulated firmware update flow
             runFirmwareUpdateFlow(firmwareVersion, firmwareUri);
           }
         });
       }
     }
     ```

1. İlerlemeyi çözüme geri raporlayan uzun süre çalışan bir bellenim güncelleştirme akışının benzetimini yapmak için aşağıdaki işlevi ekleyin:

     ```javascript
     // Simulated firmwareUpdate flow
     function runFirmwareUpdateFlow(firmwareVersion, firmwareUri) {
       console.log('Simulating firmware update flow...');
       console.log('> Firmware version passed: ' + firmwareVersion);
       console.log('> Firmware URI passed: ' + firmwareUri);
       async.waterfall([
         function (callback) {
           console.log("Image downloading from " + firmwareUri);
           var patch = {
             FirmwareUpdateStatus: 'Downloading image..'
           };
           reportUpdateThroughTwin(patch, callback);
           sleep(10000, callback);
         },
         function (callback) {
           console.log("Downloaded, applying firmware " + firmwareVersion);
           deviceOnline = false;
           var patch = {
             FirmwareUpdateStatus: 'Applying firmware..',
             Online: false
           };
           reportUpdateThroughTwin(patch, callback);
           sleep(8000, callback);
         },
         function (callback) {
           console.log("Rebooting");
           var patch = {
             FirmwareUpdateStatus: 'Rebooting..'
           };
           reportUpdateThroughTwin(patch, callback);
           sleep(10000, callback);
         },
         function (callback) {
           console.log("Firmware updated to " + firmwareVersion);
           deviceOnline = true;
           var patch = {
             FirmwareUpdateStatus: 'Firmware updated',
             Online: true,
             Firmware: firmwareVersion
           };
           reportUpdateThroughTwin(patch, callback);
           callback(null);
         }
       ], function(err) {
         if (err) {
           console.error('Error in simulated firmware update flow: ' + err.message);
         } else {
           console.log("Completed simulated firmware update flow");
         }
       });

       // Helper function to update the twin reported properties.
       function reportUpdateThroughTwin(patch, callback) {
         console.log("Sending...");
         console.log(JSON.stringify(patch, null, 2));
         client.getTwin(function(err, twin) {
           if (!err) {
             twin.properties.reported.update(patch, function(err) {
               if (err) callback(err);
             });      
           } else {
             if (err) callback(err);
           }
         });
       }

       function sleep(milliseconds, callback) {
         console.log("Simulate a delay (milleseconds): " + milliseconds);
         setTimeout(function () {
           callback(null);
         }, milliseconds);
       }
     }
     ```

1. Çözüme telemetri verileri göndermek için aşağıdaki kodu ekleyin. İstemci uygulaması ileti şemasını belirlemek için iletiye özellikler ekler:

     ```javascript
     function sendTelemetry(data, schema) {
       if (deviceOnline) {
         var d = new Date();
         var payload = JSON.stringify(data);
         var message = new Message(payload);
         message.properties.add('iothub-creation-time-utc', d.toISOString());
         message.properties.add('iothub-message-schema', schema);

         console.log('Sending device message data:\n' + payload);
         client.sendEvent(message, printErrorFor('send event'));
       } else {
         console.log('Offline, not sending telemetry');
       }
     }
     ```

1. İstemci örneği oluşturmak için aşağıdaki kodu ekleyin:

     ```javascript
     var client = Client.fromConnectionString(connectionString, Protocol);
     ```

1. Aşağıdaki kodu öğesine ekleyin:

    * Bağlantıyı açın.
    * İstenen özellikler için bir işleyici ayarlayın.
    * Bildirilen özellikleri gönder.
    * Doğrudan yöntemler için işleyicileri kaydedin. Örnek, bellenim güncelleştirmesi doğrudan yöntemi için ayrı bir işleyici kullanır.
    * Telemetri göndermeye başlayın.

      ```javascript
      client.open(function (err) {
      if (err) {
        printErrorFor('open')(err);
      } else {
        // Create device Twin
        client.getTwin(function (err, twin) {
          if (err) {
            console.error('Could not get device twin');
          } else {
            console.log('Device twin created');

            twin.on('properties.desired', function (delta) {
              // Handle desired properties set by solution
              console.log('Received new desired properties:');
              console.log(JSON.stringify(delta));
            });

            // Send reported properties
            twin.properties.reported.update(reportedProperties, function (err) {
              if (err) throw err;
              console.log('Twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onFirmwareUpdate);
            client.onDeviceMethod('EmergencyValveRelease', onDirectMethod);
            client.onDeviceMethod('IncreasePressure', onDirectMethod);
          }
        });

        // Start sending telemetry
        var sendDeviceTelemetry = setInterval(function () {
          temperature += generateRandomIncrement();
          pressure += generateRandomIncrement();
          humidity += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit,
            'humidity': humidity,
            'humidity_unit': humidityUnit,
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, schema)
        }, 5000);

        client.on('error', function (err) {
          printErrorFor('client')(err);
          if (sendTemperatureInterval) clearInterval(sendTemperatureInterval);
          if (sendHumidityInterval) clearInterval(sendHumidityInterval);
          if (sendPressureInterval) clearInterval(sendPressureInterval);
          client.close(printErrorFor('client.close'));
        });
      }
      });
      ```

1. **remote_monitoring.js** dosyadaki değişiklikleri kaydedin.

1. Örnek uygulamayı başlatmak için komut isteminde aşağıdaki komutu çalıştırın:

     ```cmd/sh
     node remote_monitoring.js
     ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
