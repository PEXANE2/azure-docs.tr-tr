---
title: Aygıtlardan Node ile Azure IoT Hub'ına dosya yükleme | Microsoft Dokümanlar
description: Node.js için Azure IoT aygıtı SDK'yı kullanarak bir cihazdan buluta dosya yükleme. Yüklenen dosyalar bir Azure depolama blob kapsayıcısında depolanır.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 06/28/2017
ms.custom: mqtt
ms.openlocfilehash: af9743233a61e8e6d816b362d35e6a38735df35b
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732242"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-nodejs"></a>IoT Hub (Node.js) ile cihazınızdan buluta dosya yükleme

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğretici, [IoT Hub öğreticisi ile buluttan cihaza gönder iletilerinde,](iot-hub-node-node-c2d.md) bir dosyayı [Azure blob depolamasına](../storage/index.yml)yüklemek için [IoT Hub'ın dosya yükleme özelliklerini](iot-hub-devguide-file-upload.md) nasıl kullanacağınızı göstermek için kod üzerine kurulur. Öğretici şunların nasıl yapıldığını göstermektedir:

* Bir dosyayı yüklemek için güvenli bir şekilde Azure blob URI içeren bir aygıt sağlayın.

* Uygulamanızın arka ucundaki dosyanın işlenmesini tetiklemek için IoT Hub dosya yükleme bildirimlerini kullanın.

[Bir aygıttan IoT hub'ına](quickstart-send-telemetry-node.md) hızlı başlatma gönder telemetrisi, IoT Hub'ın temel aygıttan buluta mesajlaşma işlevselliğini gösterir. Ancak, bazı senaryolarda, aygıtlarınızın gönderdiği verileri IoT Hub'ın kabul ettiği nispeten küçük aygıttan buluta iletilerle kolayca eşleyemezsiniz. Örneğin:

* Resim içeren büyük dosyalar
* Videolar
* Yüksek frekansta örneklenmiş titreşim verileri
* Önceden işlenmiş bir veri türü.

Bu dosyalar genellikle [Azure Veri Fabrikası](../data-factory/introduction.md) veya [Hadoop](../hdinsight/index.yml) yığını gibi araçlar kullanılarak bulutta toplu olarak işlenir. Bir aygıttan yayla dosyaları yapmanız gerektiğinde, IoT Hub'ın güvenliğini ve güvenilirliğini kullanmaya devam edebilirsiniz.

Bu öğreticinin sonunda iki Node.js konsol uygulaması çalıştırın:

* IoT hub'ınız tarafından sağlanan bir SAS URI kullanarak bir dosyayı depolama alanına yükleyen **SimulatedDevice.js.**

* **ReadFileUploadNotification.js**, IoT hub'ınızdan dosya yükleme bildirimleri alır.

> [!NOTE]
> IoT Hub, Azure IoT aygıt SDK'ları aracılığıyla birçok aygıt platformunu ve dili (C, .NET, Javascript, Python ve Java dahil) destekler. Cihazınızı Azure IoT Hub'ına nasıl bağlayacaklarınız hakkında adım adım talimatlar için [Azure IoT Geliştirici Merkezi] bakın.

## <a name="prerequisites"></a>Ön koşullar

* Düğüm.js sürüm 10.0.x veya daha sonra. [Geliştirme ortamınızı hazırlayın,](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) bu öğretici için Windows veya Linux'a Node.js'in nasıl yüklenir olduğunu açıklar.

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Aygıt uygulamasından dosya yükleme

Bu bölümde, bir dosyayı IoT hub'ına yüklemek için aygıt uygulamasını oluşturursunuz.

1. ```simulateddevice``` adlı bir boş klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak ```simulateddevice``` klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

2. Komut isteminizde ```simulateddevice``` klasöründe **azure-iot-device** Cihaz SDK paketini ve **azure-iot-device-mqtt** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Bir metin düzenleyicisi kullanarak ```simulateddevice``` klasöründe bir **SimulatedDevice.js** dosyası oluşturun.

4. Aşağıdaki ```require``` deyimlerini **SimulatedDevice.js** dosyasının başlangıcına ekleyin:

    ```javascript
    'use strict';

    var fs = require('fs');
    var mqtt = require('azure-iot-device-mqtt').Mqtt;
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    ```

5. Bir `deviceconnectionstring` değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  IoT Hub oluştur bölümünde oluşturduğunuz aygıtın adıyla `{deviceconnectionstring}` *değiştirin:*

    ```javascript
    var connectionString = '{deviceconnectionstring}';
    var filename = 'myimage.png';
    ```

    > [!NOTE]
    > Basitlik adına bağlantı dizesi koda dahildir: bu önerilen bir uygulama değildir ve kullanım durumunuza ve mimarinize bağlı olarak bu sırrı depolamanın daha güvenli yollarını göz önünde bulundurmak isteyebilirsiniz.

6. İstemciyi bağlamak için aşağıdaki kodu ekleyin:

    ```javascript
    var client = clientFromConnectionString(connectionString);
    console.log('Client connected');
    ```

7. Bir geri arama oluşturun ve dosyayı yüklemek için **uploadToBlob** işlevini kullanın.

    ```javascript
    fs.stat(filename, function (err, stats) {
        const rr = fs.createReadStream(filename);

        client.uploadToBlob(filename, rr, stats.size, function (err) {
            if (err) {
                console.error('Error uploading file: ' + err.toString());
            } else {
                console.log('File uploaded');
            }
        });
    });
    ```

8. **SimulatedDevice.js** dosyasını kaydedin ve kapatın.

9. Bir resim dosyasını `simulateddevice` klasöre kopyalayın ve yeniden `myimage.png`adlandırın.

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

Bu makalede, bir aygıttan bir [IoT hub'ına telemetri gönder'de](quickstart-send-telemetry-node.md)oluşturduğunuz IoT hub'ından dosya yükleme bildirim iletileri almak için bir arka uç hizmeti oluşturursunuz. Dosya yükleme bildirim iletileri almak için **hizmetinizin bağlanma** iznine ihtiyacı vardır. Varsayılan olarak, her IoT Hub'ı bu izni veren paylaşılan erişim ilkesi adlı **hizmetle** oluşturulur.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>Dosya yükleme bildirimi alma

Bu bölümde, IoT Hub'dan dosya yükleme bildirim iletileri alan bir Düğüm.js konsol uytun uygulaması oluşturursunuz.

Bu bölümü tamamlamak için IoT Hub'ınızdaki **iothubowner** bağlantı dizesini kullanabilirsiniz. Bağlantı dizesini **Paylaşılan erişim ilkesi** nde Azure [portalında](https://portal.azure.com/) bulabilirsiniz.

1. ```fileuploadnotification``` adlı bir boş klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak ```fileuploadnotification``` klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

2. ```fileuploadnotification``` Klasördeki komut isteminizde **azure-iothub** SDK paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak, `fileuploadnotification` klasörde bir **FileUploadNotification.js** dosyası oluşturun.

4. `require` **FileUploadNotification.js** dosyasının başında aşağıdaki ifadeleri ekleyin:

    ```javascript
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Bir `iothubconnectionstring` değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  `{iothubconnectionstring}` Yer tutucu değerini, IoT hub bağlantı dizesini al'da daha önce kopyaladığınız IoT hub bağlantı [dizesiyle değiştirin:](#get-the-iot-hub-connection-string)

    ```javascript
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > Basitlik adına bağlantı dizesi koda dahildir: bu önerilen bir uygulama değildir ve kullanım durumunuza ve mimarinize bağlı olarak bu sırrı depolamanın daha güvenli yollarını göz önünde bulundurmak isteyebilirsiniz.

6. İstemciyi bağlamak için aşağıdaki kodu ekleyin:

    ```javascript
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

7. İstemciyi açın ve durum güncelleştirmelerini almak için **getFileNotificationReceiver** işlevini kullanın.

    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFileNotificationReceiver(function receiveFileUploadNotification(err, receiver){
          if (err) {
            console.error('error getting the file notification receiver: ' + err.toString());
          } else {
            receiver.on('message', function (msg) {
              console.log('File upload from device:')
              console.log(msg.getData().toString('utf-8'));
            });
          }
        });
      }
    });
    ```

8. **FileUploadNotification.js** dosyasını kaydedin ve kapatın.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

Klasördeki komut isteminde `fileuploadnotification` aşağıdaki komutu çalıştırın:

```cmd/sh
node FileUploadNotification.js
```

Klasördeki komut isteminde `simulateddevice` aşağıdaki komutu çalıştırın:

```cmd/sh
node SimulatedDevice.js
```

Aşağıdaki ekran **görüntüsü, Simüle Aygıt** uygulamasından çıktıyı gösterir:

![Simüle edilmiş cihaz uygulamasından çıktı](./media/iot-hub-node-node-file-upload/simulated-device.png)

Aşağıdaki ekran görüntüsü **FileUploadNotification** uygulamasından çıktıyı gösterir:

![Okuma-dosya yükleme-bildirim uygulamasından çıktı](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

Yüklediğiniz dosyayı yapılandırılan depolama kapsayıcısında görüntülemek için portalı kullanabilirsiniz:

![Yüklenen dosya](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, cihazlardan gelen dosya yüklemelerini basitleştirmek için IoT Hub'ın dosya yükleme özelliklerini nasıl kullanacağınızı öğrendiniz. Aşağıdaki makalelerle IoT hub özelliklerini ve senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlı bir IoT hub'ı oluşturma](iot-hub-rm-template-powershell.md)

* [C SDK'ya Giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)
