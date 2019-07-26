---
title: Azure IoT Hub cihaz yönetimini kullanmaya başlama (node) | Microsoft Docs
description: Uzak cihaz yeniden başlatma işlemini başlatmak için IoT Hub cihaz yönetimi 'ni kullanma. Doğrudan yöntemini çağıran bir doğrudan yöntem ve bir hizmet uygulaması içeren bir sanal cihaz uygulaması uygulamak için Node. js için Azure IoT SDK 'sını kullanın.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.openlocfilehash: 868df2c8d1e14000f743686dcb6d4174d851be86
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68403259"
---
# <a name="get-started-with-device-management-node"></a>Cihaz yönetimini kullanmaya başlama (node)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Bu öğretici şunların nasıl yapıldığını gösterir:

* Bir IoT Hub oluşturmak ve IoT Hub 'ınızda bir cihaz kimliği oluşturmak için [Azure Portal](https://portal.azure.com) kullanın.

* Bu cihazı yeniden yükleyen doğrudan bir yöntem içeren bir sanal cihaz uygulaması oluşturun. Doğrudan Yöntemler buluttan çağrılır.

* IoT Hub 'ınız aracılığıyla sanal cihaz uygulamasındaki önyükleme doğrudan yöntemini çağıran bir Node. js konsol uygulaması oluşturun.

Bu öğreticinin sonunda iki Node. js konsol uygulaması vardır:

* daha önce oluşturulan cihaz kimliğiyle IoT Hub 'ınıza bağlanan **dmpatterns_getstarted_device. js**, bir yeniden başlatma doğrudan yöntemi alır, fiziksel yeniden başlatmanın benzetimini yapar ve son yeniden başlatmanın zamanını raporlar.

* **dmpatterns_getstarted_service. js**, sanal cihaz uygulamasında doğrudan bir yöntemi çağırır, yanıtı görüntüler ve güncelleştirilmiş bildirilen özellikleri görüntüler.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Node. js sürüm 10.0. x veya üzeri. [Geliştirme ortamınızı hazırlama](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) Windows veya Linux 'ta Bu öğretici için Node. js ' nin nasıl yükleneceğini açıklar.

* Etkin bir Azure hesabı. (Hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Sanal cihaz uygulaması oluşturma

Bu bölümde, aşağıdaki adımları yerine getirmeniz gerekir:

* Bulut tarafından çağrılan doğrudan bir yönteme yanıt veren bir Node.js konsol uygulaması oluşturma

* Sanal cihaz yeniden başlatma tetikleyin

* Cihazların ve en son yeniden başlatıldığı zaman cihaz ikizi sorgularını belirlemesine olanak tanımak için bildirilen özellikleri kullanın

1. **manageddevice** adlı boş bir klasör oluşturun.  Komut isteminizde aşağıdaki komutu kullanarak **manageddevice** klasöründe bir package.json dosyası oluşturun.  Tüm varsayılanları kabul edin:
      
    ```
    npm init
    ```

2. **Manageddevice** klasöründe komut istemindeki **Azure-IoT-Device** cihaz SDK paketini ve **Azure-IoT-Device-MQTT** paketini yüklemek için aşağıdaki komutu çalıştırın:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Bir metin düzenleyicisi kullanarak, **manageddevice** klasöründe bir **dmpatterns_getstarted_device. js** dosyası oluşturun.

4. **Dmpatterns_getstarted_device. js** dosyasının başlangıcında aşağıdaki ' gerektir ' deyimlerini ekleyin:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Bir **connectionString** değişkeni ekleyin ve bir **İstemci** örneği oluşturmak için bunu kullanın.  Bağlantı dizesini, cihaz bağlantı dizeniz ile değiştirin.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Cihaza doğrudan yöntemi uygulamak için aşağıdaki işlevi ekleyin
   
    ```
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

7. IoT Hub 'ınız bağlantısını açın ve doğrudan yöntem dinleyicisini başlatın:

   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```

8. **Dmpatterns_getstarted_device. js** dosyasını kaydedin ve kapatın.

> [!NOTE]
> Sade ve basit bir anlatım gözetildiği için bu öğretici herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda, [geçici hata işleme](/azure/architecture/best-practices/transient-faults)makalesinde önerildiği gibi yeniden deneme ilkelerini (üstel geri alma gibi) uygulamanız gerekir.

## <a name="get-the-iot-hub-connection-string"></a>IoT Hub bağlantı dizesini al

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Doğrudan bir yöntem kullanarak cihazda Uzaktan yeniden başlatma tetikleyin

Bu bölümde, doğrudan bir yöntemi kullanarak bir cihazda Uzaktan yeniden başlatma işlemini başlatan bir Node. js konsol uygulaması oluşturacaksınız. Uygulama, bu cihazın son yeniden başlatma zamanını saptamak için Device ikizi sorgularını kullanır.

1. **Triggerrebootondevice**adlı boş bir klasör oluşturun. Komut istemindeki aşağıdaki komutu kullanarak **triggerrebootondevice** klasöründe bir Package. JSON dosyası oluşturun. Tüm varsayılanları kabul edin:
   
    ```
    npm init
    ```

2. Komut istemindeki **triggerrebootondevice** klasöründe, **Azure-ıothub** cihaz SDK paketini ve **Azure-IoT-Device-MQTT** paketini yüklemek için aşağıdaki komutu çalıştırın:
   
    ```
    npm install azure-iothub --save
    ```

3. Bir metin düzenleyicisi kullanarak **triggerrebootondevice** klasöründe bir **dmpatterns_getstarted_service. js** dosyası oluşturun.

4. **Dmpatterns_getstarted_service. js** dosyasının başlangıcında aşağıdaki ' gerektir ' deyimlerini ekleyin:

  
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Aşağıdaki değişken bildirimlerini ekleyin ve yer tutucu değerlerini değiştirin:

   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```

6. Hedef cihazı yeniden başlatmak için cihaz yöntemini çağırmak üzere aşağıdaki işlevi ekleyin:
   
    ```
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

7. Cihaza yönelik sorgu için aşağıdaki işlevi ekleyin ve son yeniden başlatma zamanını alın:
   
    ```
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

8. Yeniden başlatma doğrudan yöntemini tetikleyip son yeniden başlatma zamanı için sorgu uygulayan işlevleri çağırmak için aşağıdaki kodu ekleyin:

   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```

9. **Dmpatterns_getstarted_service. js** dosyasını kaydedin ve kapatın.

## <a name="run-the-apps"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. **Manageddevice** klasöründeki komut isteminde, önyükleme doğrudan metodunu dinlemeye başlamak için aşağıdaki komutu çalıştırın.

   
    ```
    node dmpatterns_getstarted_device.js
    ```

2. **Triggerrebootondevice** klasöründeki komut isteminde, son yeniden başlatma zamanını bulmak üzere cihaz ikizi için uzaktan yeniden başlatma ve sorguyu tetiklemek üzere aşağıdaki komutu çalıştırın.

   
    ```
    node dmpatterns_getstarted_service.js
    ```

3. Konsolda doğrudan yönteme cihaz yanıtı görürsünüz.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]