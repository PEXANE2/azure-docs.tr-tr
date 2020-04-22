---
title: Azure IoT Hub (Düğüm) ile işleri zamanlama | Microsoft Dokümanlar
description: Birden çok aygıtta doğrudan bir yöntem çağırmak için Azure IoT Hub işi zamanlama. Benzetimli cihaz uygulamalarını ve işi yürütmek için bir hizmet uygulamasını uygulamak için Node.js için Azure IoT SDK'larını kullanırsınız.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 08/16/2019
ms.custom: mqtt
ms.openlocfilehash: d7f9ce37ad85d39388eea90af263f59ce312a6b8
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732261"
---
# <a name="schedule-and-broadcast-jobs-nodejs"></a>Zamanlama ve yayın işleri (Node.js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub, milyonlarca aygıtı zamanlayan ve güncelleştiren işleri oluşturmak ve izlemek için bir arka uç uygulamasısağlayan tam olarak yönetilen bir hizmettir.  İşler aşağıdaki eylemler için kullanılabilir:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştir
* Doğrudan yöntemleri çağırma

Kavramsal olarak, bir iş bu eylemlerden birini sarar ve aygıt ikiz sorgusu tarafından tanımlanan bir aygıt kümesine karşı yürütmenin ilerlemesini izler.  Örneğin, bir arka uç uygulaması, aygıt ikiz sorgusu tarafından belirtilen ve ileride zamanlanan 10.000 aygıtta yeniden başlatma yöntemini çağırmak için bir işi kullanabilir. Bu uygulama daha sonra bu aygıtların her biri yeniden başlatma yöntemini alır ve çalıştırır gibi ilerlemeyi izleyebilir.

Bu makalelerdeki bu özelliklerin her biri hakkında daha fazla bilgi edinin:

* Cihaz ikizi ve özellikleri: [Aygıt ikizleri](iot-hub-node-node-twin-getstarted.md) ve Öğretici ile [başlayın: Cihaz ikiz özellikleri nasıl kullanılır](tutorial-device-twins.md)

* Doğrudan yöntemler: [IoT Hub geliştirici kılavuzu - doğrudan yöntemler](iot-hub-devguide-direct-methods.md) ve [Öğretici: doğrudan yöntemler](quickstart-control-device-node.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Doğrudan bir yönteme sahip bir Düğüm.js simüle cihaz uygulaması oluşturun, **hangi lockDoor**sağlayan , çözüm arka uç tarafından çağrılabilir.

* Bir işi kullanarak simüle edilen aygıt uygulamasında **lockDoor** direct yöntemini çağıran ve bir aygıt işini kullanarak istenen özellikleri güncelleyen bir Düğüm.js konsol uygulaması oluşturun.

Bu eğitimin sonunda, iki Node.js uygulamanız vardır:

* ioT hub'ınıza aygıt kimliği ile bağlanan ve **lockDoor** direct yöntemini alan **simDevice.js.**

* **programJobService.js**, simüle cihaz uygulamasında doğrudan bir yöntem çağırır ve bir iş kullanarak cihaz ikiz istenen özellikleri günceller.

## <a name="prerequisites"></a>Ön koşullar

* Düğüm.js sürüm 10.0.x veya daha sonra. [Geliştirme ortamınızı hazırlayın,](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) bu öğretici için Windows veya Linux'a Node.js'in nasıl yüklenir olduğunu açıklar.

* Etkin bir Azure hesabı. (Hesabınız yoksa, birkaç dakika içinde ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu makaledeki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Yeni bir aygıtı IoT hub'ına kaydetme

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, bulut tarafından çağrılan ve benzetimli **lockDoor** yöntemini tetikleyen doğrudan bir yönteme yanıt veren bir Düğüm.js konsol uygulaması oluşturursunuz.

1. **simDevice**adlı yeni bir boş klasör oluşturun.  **simDevice** klasöründe, komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

   ```console
   npm init
   ```

2. **simDevice** klasöründeki komut isteminizde **azure-iot-device** Device SDK paketini ve **azure-iot-device-mqtt** paketini yüklemek için aşağıdaki komutu çalıştırın:

   ```console
   npm install azure-iot-device azure-iot-device-mqtt --save
   ```

3. Metin düzenleyicisi kullanarak, **simDevice** klasöründe yeni bir **simDevice.js** dosyası oluşturun.

4. **simDevice.js** dosyasının başında aşağıdaki 'gerek' ifadelerini ekleyin:

    ```javascript
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Bir **connectionString** değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın. `{yourDeviceConnectionString}` Yer tutucu değerini daha önce kopyaladığınız aygıt bağlantısı dizesiyle değiştirin.

    ```javascript
    var connectionString = '{yourDeviceConnectionString}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. **LockDoor** yöntemini işlemek için aşağıdaki işlevi ekleyin.

    ```javascript
    var onLockDoor = function(request, response) {

        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });

        console.log('Locking Door!');
    };
    ```

7. **LockDoor** yöntemi için işleyiciyi kaydetmek için aşağıdaki kodu ekleyin.

   ```javascript
   client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
   });
   ```

8. **simDevice.js** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkeleri (üstel geri tepme gibi) uygulamanız gerekir.
>

## <a name="get-the-iot-hub-connection-string"></a>IoT hub bağlantı dizesini alın

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Doğrudan bir yöntem çağırmak ve aygıt ikizinin özelliklerini güncelleştirmek için işleri zamanlama

Bu bölümde, doğrudan bir yöntem kullanarak bir aygıtta uzaktan **kilitKapı** başlatan ve aygıt ikizinin özelliklerini güncelleştiren bir Düğüm.js konsol uygulaması oluşturursunuz.

1. **scheduleJobService**adlı yeni bir boş klasör oluşturun.  **JobService programı** klasöründe, komut isteminizde aşağıdaki komutu kullanarak bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:

    ```console
    npm init
    ```

2. **JobService** klasöründeki komut isteminizde **azure-iothub** Device SDK paketini ve **azure-iot-device-mqtt** paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```console
    npm install azure-iothub uuid --save
    ```

3. Bir metin düzenleyicisi kullanarak, **zamanlamaJobService** klasöründe yeni bir **zamanlamaJobService.js** dosyası oluşturun.

4. **ProgramJobService.js** dosyasının başında aşağıdaki 'gerektiren' ifadeleri ekleyin:

    ```javascript
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Aşağıdaki değişken bildirimleri ekleyin. `{iothubconnectionstring}` Yer tutucu [değerini, IoT hub bağlantı dizesini al'da](#get-the-iot-hub-connection-string)kopyaladığınız değerle değiştirin. **myDeviceId'den**farklı bir aygıt kaydettiyseniz, sorgu koşulunda değiştirdiğinden emin olun.

    ```javascript
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  300;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```

6. İşin yürütülmesini izlemek için kullanılan aşağıdaki işlevi ekleyin:

    ```javascript
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Aygıt yöntemini çağıran işi zamanlamak için aşağıdaki kodu ekleyin:
  
    ```javascript
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable to process method
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```

8. Aygıtı ikizgüncelleştirmek için işi zamanlamak için aşağıdaki kodu ekleyin:

    ```javascript
    var twinPatch = {
       etag: '*',
       properties: {
           desired: {
               building: '43',
               floor: 3
           }
       }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```

9. **ProgramJobService.js** dosyasını kaydedin ve kapatın.

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. **simDevice** klasöründeki komut isteminde, doğrudan yeniden başlatma yöntemini dinlemeye başlamak için aşağıdaki komutu çalıştırın.

    ```console
    node simDevice.js
    ```

2. **JobService** klasöründeki komut isteminde, kapıyı kilitlemek ve ikizi güncelleştirmek için işleri tetiklemek için aşağıdaki komutu çalıştırın

    ```console
    node scheduleJobService.js
    ```

3. Doğrudan yönteme ve konsoldaki iş durumuna cihaz yanıtını görürsünüz.

   Aşağıdaki doğrudan yönteme aygıt yanıtı gösterir:

   ![Simüle cihaz uygulaması çıktısı](./media/iot-hub-node-node-schedule-jobs/sim-device.png)

   Aşağıda, doğrudan yöntem ve aygıt ikiz güncelleştirmesi için hizmet zamanlama işleri ve tamamlanmak üzere çalışan işler gösterilmektedir:

   ![Benzetimli cihaz uygulamasını çalıştırma](./media/iot-hub-node-node-schedule-jobs/schedule-job-service.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir aygıta doğrudan bir yöntem ve aygıt ikizözelliklerinin güncelliğini zamanlamak için bir iş kullandınız.

IoT Hub ve hava firmware güncellemesi üzerinden uzaktan gibi cihaz yönetimi desenleri ile başlarken devam [etmek için, Bkz. Öğretici: Nasıl bir firmware güncelleştirmesi yapmak.](tutorial-firmware-update.md)

IoT Hub'ı okumaya devam etmek için [Azure IoT Edge'e başlama](../iot-edge/tutorial-simulate-device-linux.md)'ya bakın.
