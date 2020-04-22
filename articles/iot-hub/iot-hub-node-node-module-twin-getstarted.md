---
title: Azure IoT Hub modülü kimlik & modül ikizi (Node.js) ile başlayın
description: Node.js için IoT SDK'ları kullanarak modül kimliğini nasıl oluşturup modül ikizini nasıl güncelleştireceğimiz öğrenin.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/26/2018
ms.custom: amqp
ms.openlocfilehash: 8e1599b1bd5db5e410e8bbd76fffbe0beb5f066e
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732295"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-nodejs"></a>IoT Hub modülü kimliği ve modül ikizi (Node.js) ile başlayın

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Modül kimlikleri ve modül ikizleri](iot-hub-devguide-module-twins.md), Azure IoT Hub cihaz kimliğine ve cihaz ikizine benzer, ancak daha hassas ayrıntı düzeyi sağlar. Azure IoT Hub aygıt kimliği ve aygıt ikizi, arka uç uygulamasının bir aygıtı yapılandırmasını sağlarken ve aygıtın koşullarında görünürlük sağlarken, modül kimliği ve modül ikizi bu özellikleri bir aygıtın tek tek bileşenleri için sağlar. İşletim sistemi tabanlı cihazlar veya üretici yazılımı cihazları gibi, birden fazla bileşen içeren ve bu özelliklere sahip cihazlarda her bir bileşen için yalıtılmış yapılandırma ve koşullara olanak sağlar.

Bu eğitimin sonunda, iki Node.js uygulamanız vardır:

* Cihaz ve modül istemcilerinizi bağlamak için bir cihaz kimliği, bir modül kimliği ve ilişkili güvenlik anahtarı oluşturan **CreateIdentities**.

* Güncelleştirilmiş modül ikizi tarafından raporlanan özelliklerini IoT Hub’ınıza gönderen **UpdateModuleTwinReportedProperties**.

> [!NOTE]
> Hem cihazlarınızda hem de çözüm arka ucunuzda çalıştırılacak uygulamalar oluşturmak için kullanabileceğiniz Azure IoT SDK'ları hakkında bilgi için bkz. [Azure IoT SDK'ları](iot-hub-devguide-sdks.md).

## <a name="prerequisites"></a>Ön koşullar

* Düğüm.js sürüm 10.0.x veya daha sonra. [Geliştirme ortamınızı hazırlayın,](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) bu öğretici için Windows veya Linux'a Node.js'in nasıl yüklenir olduğunu açıklar.

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-module-twin-shared-access-policy-text](../../includes/iot-hub-howto-module-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>IoT Hub'da aygıt kimliği ve modül kimliği oluşturma

Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde bir aygıt kimliği ve modül kimliği oluşturan bir Düğüm.js uygulaması oluşturursunuz. Kimlik kayıt defterinde girişi olmayan bir cihaz veya modül, IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub geliştirici kılavuzunun](iot-hub-devguide-identity-registry.md) "Kimlik kayıt defteri" bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, hem cihaz hem de modül için benzersiz bir kimlik ve anahtar oluşturur. Cihazınız ve modülünüz, IoT Hub’ına cihazdan buluta iletileri gönderdiğinde kendisini tanımlamak için bu değerleri kullanır. Kimlikler büyük/küçük harfe duyarlıdır.

1. Kodunuzu tutmak için bir dizin oluşturun.

2. Bu dizinin içinde, varsayılanlı boş bir package.json oluşturmak için ilk **npm init -y** çalıştırın. Bu, kodunuz için proje dosyasıdır.

3. Hizmet SDK'yı **node_modules** alt dizinin içine yüklemek için **npm yükleme -S azure-iothub\@modüllerini-önizlemesini** çalıştırın.

    > [!NOTE]
    > alt dizin adı node_modules "düğüm kitaplığı" anlamına gelen modül sözcüğü kullanır. Buradaki terimin IoT Hub modülleriyle hiçbir ilgisi yoktur.

4. Dizininizde aşağıdaki .js dosyasını oluşturun. **Add.js**deyin. Hub bağlantı dizenizi ve hub adınızı kopyalayın ve yapıştırın.

    ```javascript
    var Registry = require('azure-iothub').Registry;
    var uuid = require('uuid');
    // Copy/paste your connection string and hub name here
    var serviceConnectionString = '<hub connection string from portal>';
    var hubName = '<hub name>.azure-devices.net';
    // Create an instance of the IoTHub registry
    var registry = Registry.fromConnectionString(serviceConnectionString);
    // Insert your device ID and moduleId here.
    var deviceId = 'myFirstDevice';
    var moduleId = 'myFirstModule';
    // Create your device as a SAS authentication device
    var primaryKey = new Buffer(uuid.v4()).toString('base64');
    var secondaryKey = new Buffer(uuid.v4()).toString('base64');
    var deviceDescription = {
      deviceId: deviceId,
      status: 'enabled',
      authentication: {
        type: 'sas',
        symmetricKey: {
          primaryKey: primaryKey,
          secondaryKey: secondaryKey
        }
      }
    };

    // First, create a device identity
    registry.create(deviceDescription, function(err) {
      if (err) {
        console.log('Error creating device identity: ' + err);
        process.exit(1);
      }
      console.log('device connection string = "HostName=' + hubName + ';DeviceId=' + deviceId + ';SharedAccessKey=' + primaryKey + '"');

      // Then add a module to that device
      registry.addModule({ deviceId: deviceId, moduleId: moduleId }, function(err) {
        if (err) {
          console.log('Error creating module identity: ' + err);
          process.exit(1);
        }

        // Finally, retrieve the module details from the hub so we can construct the connection string
        registry.getModule(deviceId, moduleId, function(err, foundModule) {
          if (err) {
            console.log('Error getting module back from hub: ' + err);
            process.exit(1);
          }
          console.log('module connection string = "HostName=' + hubName + ';DeviceId=' + foundModule.deviceId + ';ModuleId='+foundModule.moduleId+';SharedAccessKey=' + foundModule.authentication.symmetricKey.primaryKey + '"');
          process.exit(0);
        });
      });
    });

    ```

Bu uygulama kimliği **myFirstDevice** ile bir cihaz kimliği ve cihaz **myFirstDevice**altında kimliği **myFirstModule** ile bir modül kimliği oluşturur. (Bu modül kimliği kimlik kayıt defterinde zaten varsa, kod yalnızca varolan modül bilgilerini alır.) Uygulama daha sonra bu kimlik için birincil anahtarı görüntüler. IoT hub'ınıza bağlanmak için sanal modül uygulamasında bu anahtarı kullanırsınız.

Bu düğümü add.js kullanarak çalıştırın. Cihaz kimliğiniz için bir bağlantı dizesi ve modül kimliğiniz için başka bir bağlantı dizesi verecektir.

> [!NOTE]
> IoT Hub kimlik kayıt defteri yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz ve modül kimliklerini depolar. Kimlik kayıt defteri, cihaz kimliklerini ve anahtarlarını güvenlik kimlik bilgileri olarak kullanmak için depolar. Kimlik kayıt defterinin her cihaz için depoladığı etkin/devre dışı bayrağını kullanarak, ilgili cihaza erişimi devre dışı bırakabilirsiniz. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Modül kimlikleri için etkin/devre dışı bayrağı yoktur. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu](iot-hub-devguide-identity-registry.md).

## <a name="update-the-module-twin-using-nodejs-device-sdk"></a>Node.js cihazı SDK kullanarak modül ikizini güncelleştirin

Bu bölümde, simüle edilen cihazınızda, modül ikiz bildirilen özelliklerini güncelleyen bir Düğüm.js uygulaması oluşturursunuz.

1. **Modül bağlantı dizenizi alın** -- [Azure portalında](https://portal.azure.com/)oturum açın. IoT Hub’ınıza gidin ve IoT Cihazları’na tıklayın. MyFirstDevice'ı bulun, açın ve myFirstModule'in başarıyla oluşturulduğunu görün. Modül bağlantı dizesini kopyalayın. Sonraki adımda gerekecektir.

   ![Azure portalı modül ayrıntısı](./media/iot-hub-node-node-module-twin-getstarted/module-detail.png)

2. Yukarıdaki adımda yaptığınız gibi, aygıt kodunuz için bir dizin oluşturun ve nPM'yi kullanarak sdk'yı devreye sokar ve aygıtı SDK'yı **(npm install -S azure-iot-device-amqp\@modules-preview)** yükleyin.

   > [!NOTE]
   > npm yükleme komutu yavaş hissedebilirsiniz. Sabırlı ol, paket deposundan bir sürü kod çıkarıyor.

   > [!NOTE]
   > Eğer npm ERR diyor bir hata görürseniz! kayıt defteri hatası ayrıştırma json, bu göz ardı etmek güvenlidir. Eğer npm ERR diyor bir hata görürseniz! kayıt defteri hatası ayrıştırma json, bu göz ardı etmek güvenlidir.

3. twin.js adlı bir dosya oluşturun. Modül kimlik dizenizi kopyalayın ve yapıştırın.

    ```javascript
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-amqp').Amqp;
    // Copy/paste your module connection string here.
    var connectionString = '<insert module connection string here>';
    // Create a client using the Amqp protocol.
    var client = Client.fromConnectionString(connectionString, Protocol);
    client.on('error', function (err) {
      console.error(err.message);
    });
    // connect to the hub
    client.open(function(err) {
      if (err) {
        console.error('error connecting to hub: ' + err);
        process.exit(1);
      }
      console.log('client opened');
    // Create device Twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('error getting twin: ' + err);
          process.exit(1);
        }
        // Output the current properties
        console.log('twin contents:');
        console.log(twin.properties);
        // Add a handler for desired property changes
        twin.on('properties.desired', function(delta) {
            console.log('new desired properties received:');
            console.log(JSON.stringify(delta));
        });
        // create a patch to send to the hub
        var patch = {
          updateTime: new Date().toString(),
          firmwareVersion:'1.2.1',
          weather:{
            temperature: 72,
            humidity: 17
          }
        };
        // send the patch
        twin.properties.reported.update(patch, function(err) {
          if (err) throw err;
          console.log('twin state reported');
        });
      });
    });
    ```

4. Şimdi, komut düğümü **twin.js**kullanarak bu çalıştırın .

   ```cmd/sh
   F:\temp\module_twin>node twin.js
   ```

   Daha sonra göreceksiniz:

   ```console
   client opened
   twin contents:
   { reported: { update: [Function: update], '$version': 1 },
     desired: { '$version': 1 } }
   new desired properties received:
   {"$version":1}
   twin state reported
   ```

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub’ı kullanmaya başlamak ve diğer IoT senaryolarını keşfetmek için bkz:

* [Cihaz yönetimini kullanmaya başlama](iot-hub-node-node-device-management-get-started.md)

* [IoT Edge ile çalışmaya başlama](../iot-edge/tutorial-simulate-device-linux.md)