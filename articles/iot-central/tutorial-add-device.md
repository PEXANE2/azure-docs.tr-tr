---
title: Azure IoT Central uygulamasına gerçek bir cihaz ekleme | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanıza gerçek bir cihaz ekleyin.
author: sandeeppujar
ms.author: sandeepu
ms.date: 08/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 96b8090bd0e178be53cb49f42438951645def5d9
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960519"
---
# <a name="tutorial-add-a-real-device-to-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanıza gerçek bir cihaz ekleme

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Bu öğreticide, Microsoft Azure IoT Central uygulamanıza gerçek bir cihaz ekleme ve yapılandırma açıklanmaktadır. 
_* Bu öğreticiyi tamamlayabilmeniz için kendi dış cihazınıza ihtiyacınız yoktur. "Gerçek cihaz", komut satırı ortamında çalışan kodlanmış bir uygulama olarak oluşturulur._ 

Bu öğretici iki bölümden oluşur:

* İlk olarak, bir operatör olarak, Azure IoT Central uygulamanıza gerçek bir cihaz ekleme ve yapılandırma hakkında bilgi edineceksiniz. Bu bölümün sonunda, ikinci bölümde kullanmak üzere bir bağlantı dizesi alırsınız.
* Daha sonra, bir cihaz geliştiricisi olarak gerçek cihazınızdaki kodu öğrenirsiniz. Bağlantı dizesini ilk bölümden örnek koda eklersiniz.



Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir gerçek cihaz ekleyin
> * Gerçek cihazı yapılandırma
> * Uygulamadan gerçek cihaz için bağlantı dizesi al
> * İstemci kodunun uygulamayla nasıl eşlendiğini anlayın
> * Gerçek cihaz için istemci kodunu yapılandırma

## <a name="prerequisites"></a>Prerequisites

Başlamadan önce, oluşturucunun Azure IoT Central uygulamasını oluşturmak için en azından ilk Oluşturucu öğreticisini tamamlaması gerekir: [Yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) (gerekli)

Geliştirme makinenize [Node. js](https://nodejs.org/) sürüm 8.0.0 veya üstünü yükler. Sürümünüzü denetlemek için komut satırında `node --version` ' yı çalıştırabilirsiniz. Node. js, çok çeşitli işletim sistemleri için kullanılabilir.

## <a name="add-a-real-device"></a>Gerçek bir cihaz ekleme

Uygulamanıza gerçek bir cihaz eklemek için, [Yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisinde oluşturduğunuz **bağlı AIR conditioner** cihaz şablonunu kullanırsınız.

1. Yeni bir cihazı operatör olarak eklemek için sol gezinti menüsünde **cihazlar** ' ı seçin:

   ![Bağlı hava koşullayıcı gösteren cihaz Gezgini sayfası](media/tutorial-add-device/explorer.png)

   **Device Explorer** , **bağlantılı AIR conditioner** cihaz şablonunu ve sanal cihazı gösterir. Bir cihaz şablonu oluşturduğunuzda, IoT Central otomatik olarak sanal bir cihaz oluşturur.

2. **Bağlı hava layıcı** cihaz şablonunun **Device Explorer**seçili olduğunu unutmayın. Bu şablonu kullanan gerçek bir AIR klimaları cihazını bağlamaya başlamak için **+** ' i seçin ve ardından **Gerçek**:

   ![Yeni, gerçek bağlantılı bir AIR klimaları cihazı eklemeye başlayın](media/tutorial-add-device/newreal.png)

3. Kendi **CIHAZ kimliğinizi** girin (küçük harfle yazılmalıdır) veya önerilen değeri kullanın. Ayrıca, yeni cihazınız için bir **Cihaz adı** girip **Oluştur**' a tıklayın.

   ![Cihazı yeniden adlandırma](media/tutorial-add-device/rename.png)

## <a name="configure-a-real-device"></a>Gerçek bir cihazı yapılandırma

Gerçek cihaz, **bağlantılı hava koşullayıcı** cihaz şablonundan oluşturulur. Cihazınızı yapılandırmak için **ayarları** kullanabilir ve cihazlarınızın bilgilerini kaydetmek için özellik değerlerini ayarlayabilirsiniz.

1. **Ayarlar** sayfasında, **sıcaklık ayarlama** durumunun **güncelleştirme**olmadığına dikkat edin. Gerçek cihaz uygulamaya bağlanana kadar bu durumda kalır ve bu ayar üzerinde işlem yaptığı konusunda sizi onaylar.

    ![Ayarları eşitlemeyi göster](media/tutorial-add-device/settingssyncing.png)

2. Yeni, gerçek cihazınızın **Özellikler** sayfasında, hizmet konumu ve son hizmet tarihi düzenlenebilir özelliklerdir. Seri numarası ve bellenim sürümü alanları, cihaz uygulamaya bağlanana kadar boştur. Bu salt yazılır değerler cihazdan gönderilir ve düzenlenemez.

    ![Gerçek cihaz için cihaz özellikleri](media/tutorial-add-device/setproperties1.png)

3. Gerçek cihazınız için **ölçümleri**, **kuralları**ve **Pano** sayfalarını görüntüleyebilirsiniz.

## <a name="prepare-the-client-code"></a>İstemci kodunu hazırlama

Bu makaledeki örnek kod [Node. js](https://nodejs.org/) ' de yazılmıştır ve bir cihazın şunları yapmak için yeterli kodu gösterir:

* Azure IoT Central uygulamanıza bağlanın.
* Sıcaklık telemetrisini bağlı bir AIR klimaları cihazı olarak gönderin.
* Azure IoT Central uygulamanıza cihaz özellikleri gönderin.
* **Sıcaklık ayarla** ayarını kullanan bir işlece yanıt verir.
* Azure IoT Central uygulamanızdan yankı komutunu işleyin.

[Sonraki adımlar](#next-steps) bölümünde listelenen makaleler, daha fazla örnek içerir ve diğer programlama dillerini gösterir. Cihazların Azure IoT Central 'e nasıl bağlanacağı hakkında daha fazla bilgi için bkz. [cihaz bağlantısı](concepts-connectivity.md) makalesi.

Aşağıdaki adımlarda, [Node. js](https://nodejs.org/) örneğinin nasıl hazırlanacağı gösterilmektedir:

### <a name="get-the-device-connection-information"></a>Cihaz bağlantı bilgilerini al

1. Uygulamanızdaki bir cihaz örneği için bağlantı dizesi, IoT Central tarafından belirtilen cihaz bilgileriyle oluşturulur.

   Gerçek bağlantılı hava koşullarınızın cihaz ekranında **Bağlan**' ı seçin.

   ![Bağlantı bilgilerini görüntüle bağlantısını gösteren cihaz sayfası](media/tutorial-add-device/connectionlink.png)

1. **Cihaz bağlantısı** sayfasında, **Kapsam KIMLIĞI**, **cihaz kimliği** ve **birincil anahtar** değerlerini bir yere unutmayın. Bu değerleri daha sonra bu öğreticide kullanacaksınız.

   ![Bağlantı ayrıntıları](media/tutorial-add-device/device-connect.png)

### <a name="prepare-the-nodejs-project"></a>Node. js projesini hazırlama

1. Geliştirme makinenizde `connectedairconditioner` adlı bir klasör oluşturun.

1. Komut satırı ortamınızda, oluşturduğunuz `connectedairconditioner` klasörüne gidin.

1. Node. js projenizi başlatmak için, tüm varsayılanları kabul eden aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm init
      ```

1. Gerekli paketleri yüklemek için şu komutu çalıştırın:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt azure-iot-provisioning-device-mqtt azure-iot-security-symmetric-key --save
    ```

1. Bir metin düzenleyicisi kullanarak, `connectedairconditioner` klasöründe **ConnectedAirConditioner. js** adlı bir dosya oluşturun.

1. Aşağıdaki `require` deyimlerini **ConnectedAirConditioner. js** dosyasının başlangıcına ekleyin:

    ```javascript
    'use strict';

    var iotHubTransport = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var ProvisioningTransport = require('azure-iot-provisioning-device-mqtt').Mqtt;
    var SymmetricKeySecurityClient = require('azure-iot-security-symmetric-key').SymmetricKeySecurityClient;
    var ProvisioningDeviceClient = require('azure-iot-provisioning-device').ProvisioningDeviceClient;
    ```

1. Aşağıdaki değişken bildirimlerini dosyaya ekleyin. @No__t-0, `{your Device ID}` ve `{your Primary Key}` yer tutucuları, daha önce not ettiğiniz cihaz bağlantı bilgileriyle değiştirin:

    ```javascript
    var provisioningHost = 'global.azure-devices-provisioning.net';
    var idScope = '{your Scope ID}';
    var registrationId = '{your Device ID}';
    var symmetricKey = '{your Primary Key}';
    var provisioningSecurityClient = new SymmetricKeySecurityClient(registrationId, symmetricKey);
    var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvisioningTransport(), provisioningSecurityClient);
    var hubClient;
    var targetTemperature = 0;
    ```

1. Şimdiye kadar yaptığınız değişiklikleri kaydedin, ancak dosyayı açık tutun.

## <a name="review-client-code"></a>İstemci kodunu gözden geçir

Önceki bölümde, Azure IoT Central uygulamanıza bağlanan bir cihaz uygulaması için bir iskelet Node. js projesi oluşturdunuz. Sonraki adım, içine kod eklemektir:

* Azure IoT Central uygulamanıza bağlanın.
* Azure IoT Central uygulamanıza telemetri gönderin.
* Azure IoT Central uygulamanıza cihaz özellikleri gönderin.
* Azure IoT Central uygulamanızdan ayarları alın.
* Azure IoT Central uygulamanızdan yankı komutunu işleyin.

1. Azure IoT Central uygulamanıza sıcaklık telemetrisi göndermek için, **ConnectedAirConditioner. js** dosyasına aşağıdaki kodu ekleyin:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var data = JSON.stringify({ temperature: temperature });
      var message = new Message(data);
      hubClient.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    Göndereceğiniz JSON 'daki alanın adı, cihaz şablonunuzda sıcaklık telemetri için belirttiğiniz alanın adıyla eşleşmelidir. Bu örnekte, alanın adı **sıcaklık**olur.

1. **Firmwareversion** ve **SerialNumber**gibi cihaz özelliklerini göndermek için aşağıdaki tanımı ekleyin:

    ```javascript
    // Send device properties
    function sendDeviceProperties(twin) {
      var properties = {
        firmwareVersion: "9.75",
        serialNumber: "10001"
      };
      twin.properties.reported.update(properties, (errorMessage) =>
      console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
    }
    ```

1. Cihazınızın desteklediği ayarları **Setsıcaklık**gibi tanımlamak için aşağıdaki tanımı ekleyin:

    ```javascript
    // Add any settings your device supports
    // mapped to a function that is called when the setting is changed.
    var settings = {
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

1. Azure IoT Central gönderilen ayarları işlemek için, uygun cihaz kodunu bulan ve yürüten aşağıdaki işlevi ekleyin:

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

    Bu işlev:

    * İstenen bir özelliği göndermek için Azure IoT Central izler.
    * Ayar değişikliğini işlemek için çağrılacak uygun işlevi bulur.
    * Azure IoT Central uygulamanıza geri bildirim gönderir.

1. Azure IoT Central uygulamanızdan **yankı** gibi bir komuta yanıt vermek için aşağıdaki tanımı ekleyin:

    ```javascript
    // Respond to the echo command
    function onCommandEcho(request, response) {
      // Display console info
      console.log(' * Echo command received');
      // Respond
      response.send(10, 'Success', function (errorMessage) {});
    }
    ```

1. Azure IoT Central bağlantısını tamamlayıp istemci kodundaki işlevleri bağlamak için aşağıdaki kodu ekleyin:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Create handler for countdown command
        hubClient.onDeviceMethod('echo', onCommandEcho);

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        hubClient.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            sendDeviceProperties(twin);

            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };
    ```

1. Cihazı kaydedin ve IoT Central uygulamanıza bağlayın:

    ```javascript
    // Start the device (connect it to Azure IoT Central).
    provisioningClient.register((err, result) => {
      if (err) {
        console.log("error registering device: " + err);
      } else {
        console.log('registration succeeded');
        console.log('assigned hub=' + result.assignedHub);
        console.log('deviceId=' + result.deviceId);
        var connectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';SharedAccessKey=' + symmetricKey;
        hubClient = Client.fromConnectionString(connectionString, iotHubTransport);

        hubClient.open(connectCallback);
      }
    });
    ```

1. Yaptığınız değişiklikleri kaydedin.

## <a name="run-the-client-code"></a>İstemci kodunu çalıştırma

Artık istemci kodunu çalıştırabilir ve IoT Central uygulamanızla nasıl etkileşime gireceğini görebilirsiniz:

1. Örneği çalıştırmak için komut satırı ortamınıza aşağıdaki komutu girin:

    ```cmd/sh
    node ConnectedAirConditioner.js
    ```

    > [!NOTE]
    > Bu komutu çalıştırdığınızda `connectedairconditioner` klasöründe olduğunuzdan emin olun.

1. Uygulama çıktıyı konsola yazdırır:

   ![İstemci uygulama çıkışı](media/tutorial-add-device/output.png)

1. Yaklaşık 30 saniye sonra, cihaz **ölçümleri** sayfasında Telemetriyi görürsünüz:

   ![Gerçek telemetri](media/tutorial-add-device/realtelemetry.png)

1. **Ayarlar** sayfasında, ayarın artık eşitlenmiş olduğunu görebilirsiniz. Cihaz ilk kez bağlandığında, ayar değerini aldı ve değişikliği kabul edildi:

   ![Ayar eşitlendi](media/tutorial-add-device/settingsynced.png)

1. **Ayarlar** sayfasında, cihaz sıcaklığını **95** olarak ayarlayın ve **cihazı Güncelleştir**' i seçin. Örnek uygulamanız bu değişikliği alır ve işler:

   ![Alma ve işleme ayarı](media/tutorial-add-device/receivesetting.png)

   > [!NOTE]
   > İki "ayar güncelleştirmesi" iletisi vardır. @No__t-0 durumu gönderildiğinde ve `completed` durumu gönderildiğinde bir tane.

1. **Ölçümler** sayfasında, cihazın daha yüksek sıcaklık değerleri gönderdiğini görebilirsiniz:

    ![Sıcaklık telemetrisi artık daha yüksek](media/tutorial-add-device/highertemperature.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

> [!div class="nextstepaction"]
> * Yeni bir gerçek cihaz ekleyin
> * Yeni cihazı yapılandırma
> * Uygulamadan gerçek cihaz için bağlantı dizesi al
> * İstemci kodunun uygulamayla nasıl eşlendiğini anlayın
> * Gerçek cihaz için istemci kodunu yapılandırma

Azure IoT Central uygulamanıza gerçek bir cihaz bağladığınıza göre, sonraki adımlarda önerilen bazı adımları bulabilirsiniz.

Bir operatör olarak şunları yapmayı öğrenebilirsiniz:

* [Cihazlarınızı yönetin](howto-manage-devices.md)
* [Cihaz kümelerini kullanma](howto-use-device-sets.md)
* [Özel analiz oluştur](howto-use-device-sets.md)

Bir cihaz geliştiricisi olarak şunları yapmayı öğrenebilirsiniz:

* [Bir DevKit cihazını hazırlama ve bağlama (C)](howto-connect-devkit.md)
* [Raspberry Pi (Python) hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)
* [Raspberry Pi (C#) hazırlama ve bağlama](howto-connect-raspberry-pi-csharp.md)
* [Windows 10 IoT çekirdek cihazını hazırlama ve bağlama (C#)](howto-connect-windowsiotcore.md)
* [Azure IoT Central uygulamanıza genel bir Node. js istemcisini bağlama](howto-connect-nodejs.md)
