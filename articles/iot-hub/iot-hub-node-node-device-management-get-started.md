---
title: Azure IoT Hub aygıt yönetimi (Düğüm) | Microsoft Dokümanlar
description: Uzak bir aygıtı yeniden başlatmak için IoT Hub aygıt yönetimi nasıl kullanılır. Doğrudan bir yöntem ve doğrudan yöntemi çağıran bir hizmet uygulaması içeren simüle edilmiş bir aygıt uygulamasını uygulamak için Node.js için Azure IoT SDK'yı kullanırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: 5b2e4c03347020b5d5fc67927165403f06854e0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110922"
---
# <a name="get-started-with-device-management-nodejs"></a>Cihaz yönetimine başlayın (Node.js)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* IoT hub'ınızda bir IoT Hub'ı oluşturmak ve aygıt kimliği oluşturmak için [Azure portalını](https://portal.azure.com) kullanın.

* Bu aygıtı yeniden başlatan doğrudan bir yöntem içeren benzetimli bir aygıt uygulaması oluşturun. Buluttan doğrudan yöntemler çağrılır.

* IoT hub'ınız aracılığıyla simüle edilen cihaz uygulamasında yeniden başlatma yöntemini çağıran bir Düğüm.js konsol uygulaması oluşturun.

Bu öğreticinin sonunda, iki Node.js konsol uygulamanız vardır:

* Daha önce oluşturulan aygıt kimliğiyle IoT hub'ınıza bağlanan **dmpatterns_getstarted_device.js,** yeniden başlatma yöntemini alır, fiziksel yeniden başlatmayı simüle eder ve son yeniden başlatma süresini bildirir.

* **Dmpatterns_getstarted_service.js,** simüle edilen aygıt uygulamasında doğrudan bir yöntem çağırır, yanıtı görüntüler ve güncelleştirilmiş bildirilen özellikleri görüntüler.

## <a name="prerequisites"></a>Ön koşullar

* Düğüm.js sürüm 10.0.x veya daha sonra. [Geliştirme ortamınızı hazırlayın,](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) bu öğretici için Windows veya Linux'a Node.js'in nasıl yüklenir olduğunu açıklar.

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde şunları yapacaksınız:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Node.js konsol uygulaması oluşturma

* Benzetimli aygıtı yeniden başlatmayı tetikleme

* Aygıtikiz sorgularının aygıtları tanımlamasını ve en son ne zaman yeniden başlatılmasını sağlamak için bildirilen özellikleri kullanma

1. **manageddevice** adlı boş bir klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak **manageddevice** klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

2. **Yönetilen aygıt** klasöründeki komut isteminizde **azure-iot-device** Device SDK paketini ve **azure-iot-device-mqtt** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Metin düzenleyicisi kullanarak, **yönetilen aygıt** klasöründe **bir dmpatterns_getstarted_device.js** dosyası oluşturun.

4. **dmpatterns_getstarted_device.js** dosyasının başında aşağıdaki 'gerek' ifadelerini ekleyin:

    ```javascript
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Bir **connectionString** değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Yer `{yourdeviceconnectionstring}` tutucu [değerini, IoT hub'ında yeni bir aygıtı kaydedin'de](#register-a-new-device-in-the-iot-hub)daha önce kopyaladığınız aygıt bağlantısı dizesiyle değiştirin.  

    ```javascript
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Aygıtta doğrudan yöntemi uygulamak için aşağıdaki işlevi ekleyin

    ```javascript
    var onReboot = function(request, response) {

        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });

        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });

        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. IoT hub'ınıza bağlantıyı açın ve doğrudan yöntem dinleyicisini başlatın:

    ```javascript
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```

8. **dmpatterns_getstarted_device.js** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkeleri (üstel geri tepme gibi) uygulamanız gerekir.

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak aygıtta uzaktan yeniden başlatmayı tetikleme

Bu bölümde, doğrudan bir yöntem kullanarak bir aygıtta uzaktan yeniden başlatma başlatan bir Düğüm.js konsol uygulaması oluşturursunuz. Uygulama, söz sözleşme nin son yeniden başlatma saatini bulmak için cihaz ikiz sorgularını kullanır.

1. **Triggerrebootondevice**adlı boş bir klasör oluşturun. **Triggerrebootondevice** klasöründe, komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun. Tüm varsayılanları kabul edin:

    ```cmd/sh
    npm init
    ```

2. **Triggerrebootondevice** klasöründeki komut isteminizde **azure-iothub** Device SDK paketini ve **azure-iot-device-mqtt** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak, **triggerrebootondevice** klasöründe **bir dmpatterns_getstarted_service.js** dosyası oluşturun.

4. **dmpatterns_getstarted_service.js** dosyasının başında aşağıdaki 'gerek' ifadelerini ekleyin:

    ```javascript
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Aşağıdaki değişken bildirimleri ekleyin `{iothubconnectionstring}` ve yer tutucu değerini Daha önce [IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)kopyaladığınız IoT hub bağlantı dizesiyle değiştirin:

    ```javascript
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```

6. Hedef aygıtı yeniden başlatmak için aygıt yöntemini çağırmak için aşağıdaki işlevi ekleyin:

    ```javascript
    var startRebootDevice = function(twin) {

        var methodName = "reboot";

        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };

        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) {
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Aygıt sorgusuna aşağıdaki işlevi ekleyin ve son yeniden başlatma süresini alın:

    ```javascript
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```

8. Yeniden başlatma yöntemini tetikleyen işlevleri çağırmak için aşağıdaki kodu ekleyin ve son yeniden başlatma zamanı için sorguyı ekleyin:

    ```javascript
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```

9. **dmpatterns_getstarted_service.js** dosyasını kaydedin ve kapatın.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Artık uygulamaları çalıştırmaya hazırsınız.

1. **Yönetilen aygıt** klasöründeki komut isteminde, doğrudan yeniden başlatma yöntemini dinlemeye başlamak için aşağıdaki komutu çalıştırın.

    ```cmd/sh
    node dmpatterns_getstarted_device.js
    ```

2. **Triggerrebootondevice** klasöründeki komut isteminde, uzaktan yeniden başlatmayı tetiklemek için aşağıdaki komutu çalıştırın ve aygıt ikizinin son yeniden başlatma süresini bulması için sorgulayın.

    ```cmd/sh
    node dmpatterns_getstarted_service.js
    ```

3. Doğrudan yeniden başlatma yöntemine ve konsoldaki yeniden başlatma durumuna yönelik aygıt yanıtını görürsünüz.

   Aşağıdaki, hizmet tarafından gönderilen doğrudan yeniden başlatma yöntemine aygıt yanıtını gösterir:

   ![yönetilen cihaz uygulaması çıktısı](./media/iot-hub-node-node-device-management-get-started/device.png)

   Aşağıdaki, hizmetin yeniden başlatmayı tetiklediğini ve aygıt ikizini son yeniden başlatma süresi için yoklamasını gösterir:

   ![triggerrebootondevice uygulama çıktısı](./media/iot-hub-node-node-device-management-get-started/service.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
