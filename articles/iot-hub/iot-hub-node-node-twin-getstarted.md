---
title: Azure IoT Hub aygıt ikizleri (Düğüm) | Microsoft Dokümanlar
description: Etiketler eklemek ve ardından bir IoT Hub sorgusu kullanmak için Azure IoT Hub aygıt ikizleri nasıl kullanılır? Benzetimli aygıt uygulamasını ve etiketleri ekleyen ve IoT Hub sorgusunu çalıştıran bir hizmet uygulamasını uygulamak için Node.js için Azure IoT SDK'larını kullanırsınız.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: elioda
ms.custom: mqtt
ms.openlocfilehash: e65c781bd5cb62bdaa693b854caafd5f91fd497e
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732287"
---
# <a name="get-started-with-device-twins-nodejs"></a>Cihaz ikizleri (Node.js) ile başlayın

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Bu eğitimin sonunda, iki Node.js konsol uygulaması olacak:

* **AddTagsAndQuery.js**, etiketleri ve sorguları cihaz ikizler ekler bir Düğüm.js arka uç uygulaması.

* **TwinSimulatedDevice.js**, Daha önce oluşturulan aygıt kimliği ile IoT hub'ınıza bağlanan bir aygıtı taklit eden ve bağlantı durumunu bildiren bir Düğüm.js uygulamasıdır.

> [!NOTE]
> Azure [IoT SDK'lar](iot-hub-devguide-sdks.md) makalesi, hem aygıt hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz Azure IoT SK'ları hakkında bilgi sağlar.
>

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Düğüm.js sürüm 10.0.x veya daha sonra.

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Hizmet uygulamasını oluşturma

Bu bölümde, **myDeviceId**ile ilişkili aygıt ikizine konum meta verileri ekleyen bir Düğüm.js konsol uygulaması oluşturursunuz. Daha sonra, Abd'de bulunan aygıtları seçerek IoT hub'ında depolanan aygıt ikizleri ve hücresel bağlantı bildirenaygıtları sorgular.

1. **addtagsandqueryapp**adlı yeni bir boş klasör oluşturun. **addtagsandqueryapp** klasöründe, komut isteminizde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. `--yes` Parametre tüm varsayılanları kabul eder.

    ```cmd/sh
    npm init --yes
    ```

2. **addtagsandqueryapp** klasöründeki komut isteminizde **azure-iothub** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak, **addtagsandqueryapp** klasöründe yeni bir **AddTagsAndQuery.js** dosyası oluşturun.

4. **AddTagsAndQuery.js** dosyasına aşağıdaki kodu ekleyin. [IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)kopyaladığınız IoT Hub bağlantı dizesiyle değiştirin. `{iot hub connection string}`

   ``` javascript
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };

                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   ```

    **Kayıt Defteri nesnesi,** hizmetteki aygıt ikizleri ile etkileşim kurmak için gereken tüm yöntemleri ortaya çıkarır. Önceki kod önce Kayıt **Defteri** nesnesini başolarak adlandırır, sonra aygıt ikizini **myDeviceId**için alır ve son olarak etiketlerini istenen konum bilgileriyle güncelleştirir.

    Etiketleri güncelledikten sonra **queryTwins** işlevini çağırır.

5. **QueryTwins** işlevini uygulamak için **AddTagsAndQuery.js'nin** sonuna aşağıdaki kodu ekleyin:

   ```javascript
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });

            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   ```

    Önceki kod iki sorgu yürütür: ilki **Redmond43** tesisinde bulunan aygıtların yalnızca aygıt ikizlerini seçer ve ikincisi sorguyu yalnızca hücresel ağ üzerinden bağlanan aygıtları seçmek için geliştirir.

    Kod **sorgu** nesnesini oluşturduğunda, ikinci parametrede döndürülen en fazla belge sayısını belirtir. **Sorgu nesnesi,** tüm sonuçları almak için **nextAsTwin** yöntemlerini birden çok kez çağırmak için kullanabileceğiniz bir **hasMoreResults** boolean özelliği içerir. **Sonraki** adlı bir yöntem aygıt ikizi olmayan sonuçlar için kullanılabilir, örneğin, toplama sorgularının sonuçları.

6. Uygulamayı aşağıdakilerle çalıştırın:

    ```cmd/sh
        node AddTagsAndQuery.js
    ```

   **Redmond43'te** bulunan tüm aygıtları soran sorgunun sonuçlarında bir aygıt görmeniz ve sonuçları hücresel ağ kullanan aygıtlarla sınırlayan sorgu için hiçbir aygıt görmeniz gerekir.

   ![Sorgu sonuçlarındaki tek aygıta bakın](media/iot-hub-node-node-twin-getstarted/service1.png)

Sonraki bölümde, bağlantı bilgilerini bildiren ve önceki bölümdeki sorgunun sonucunu değiştiren bir aygıt uygulaması oluşturursunuz.

## <a name="create-the-device-app"></a>Cihaz uygulamasını oluşturma

Bu bölümde, hub'ınıza **myDeviceId**olarak bağlanan bir Düğüm.js konsol uygulaması oluşturur sunuz ve ardından aygıt ikizinin bildirilen özelliklerini günceller ve hücresel ağ kullanarak bağlı olduğu bilgileri içerir.

1. **Reportconnectivity**adlı yeni bir boş klasör oluşturun. **Reportconnectivity** klasöründe, komut isteminizde aşağıdaki komutu kullanarak yeni bir package.json dosyası oluşturun. `--yes` Parametre tüm varsayılanları kabul eder.

    ```cmd/sh
    npm init --yes
    ```

2. **Reportconnectivity** klasöründeki komut isteminizde **azure-iot-device**ve **azure-iot-device-mqtt** paketlerini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Bir metin düzenleyicisi kullanarak, **reportconnectivity** klasöründe yeni bir **ReportConnectivity.js** dosyası oluşturun.

4. **ReportConnectivity.js** dosyasına aşağıdaki kodu ekleyin. `{device connection string}` [IoT hub'ında yeni bir aygıtı kaydedin'de](#register-a-new-device-in-the-iot-hub) **myDeviceId** aygıt kimliğini oluşturduğunuzda kopyaladığınız aygıt bağlantısı dizesiyle değiştirin.

    ```javascript
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
    ```

    **İstemci** nesnesi, aygıttaki aygıt ikizleri ile etkileşim de olsa gereken tüm yöntemleri ortaya çıkarır. Önceki kod, **Istemci** nesnesini aldıktan sonra aygıtı **myDeviceId** için ikiz olarak alır ve bildirilen özelliğini bağlantı bilgileriyle güncelleştirir.

5. Aygıt uygulamasını çalıştırma

    ```cmd/sh
        node ReportConnectivity.js
    ```

    İletiyi `twin state reported`görmelisiniz.

6. Aygıt bağlantı bilgilerini bildirdiğine göre, her iki sorguda da görünmesi gerekir. **addtagsandqueryapp** klasörüne geri dönün ve sorguları yeniden çalıştırın:

    ```cmd/sh
        node AddTagsAndQuery.js
    ```

    Bu kez **myDeviceId** her iki sorgu sonuçlarında da görünmelidir.

    ![MyDeviceId'i her iki sorgu sonuçlarında da göster](media/iot-hub-node-node-twin-getstarted/service2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalında yeni bir IoT hub'ı yapılandırdınız ve ardından IoT hub'ının kimlik kayıt defterinde bir cihaz kimliği oluşturdunuz. Aygıt meta verilerini arka uç uygulamasından etiketler olarak eklediniz ve aygıt ikizindeki aygıt bağlantı bilgilerini bildirmek için simüle edilmiş bir aygıt uygulaması yazdınız. Ayrıca, SQL benzeri IoT Hub sorgu dilini kullanarak bu bilgileri nasıl sorgulayabileceğinizi de öğrendiniz.

Nasıl yapılacağını öğrenmek için aşağıdaki kaynakları kullanın:

* [IoT Hub öğreticiile başlatıla](quickstart-send-telemetry-node.md) ile cihazlardan telemetri göndermek,

* cihazları yapılandırmak [için istenilen özellikleri kullanarak cihaz](tutorial-device-twins.md) ikizinin istenilen özelliklerini kullanarak yapılandırMak,

* cihazları etkileşimli olarak kontrol edin (kullanıcı kontrolündeki bir uygulamadan bir fanı açmak gibi), [doğrudan yöntem](quickstart-control-device-node.md) kullanma öğreticisi ile.
