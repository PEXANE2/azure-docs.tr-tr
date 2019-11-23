---
title: Quickstart - Provision simulated TPM device to Azure IoT Hub using Node.js
description: Quickstart - Create and provision a simulated TPM device using Node.js device SDK for Azure IoT Hub Device Provisioning Service. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 942bc64153e1c35e3fb9c5bbb989607b7e443e9a
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74423468"
---
# <a name="quickstart-create-and-provision-a-simulated-tpm-device-using-nodejs-device-sdk-for-iot-hub-device-provisioning-service"></a>Quickstart: Create and provision a simulated TPM device using Node.js device SDK for IoT Hub Device Provisioning Service

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

Bu adımlar, Windows işletim sistemi çalıştıran geliştirme makinenizde sanal makine oluşturmayı, Windows TPM simülatörünü cihazın [Donanım Güvenlik Modülü (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) olarak çalıştırmayı ve örnek kodlar kullanarak bu sanal cihazı Cihaz Sağlama Hizmeti ve IoT hub’ınızla bağlamayı gösterir. 

Otomatik sağlama işlemini bilmiyorsanız, [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirdiğinizden emin olun. Ayrıca devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. 

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:
- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Bireysel kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede bireysel kayıtlar gösterilmektedir.

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Makinenizde [Node.js v4.0 veya üzeri](https://nodejs.org) bir sürümün yüklü olduğundan emin olun.

1. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 


## <a name="simulate-a-tpm-device"></a>TPM cihazının benzetimini yapma

1. Bir komut istemi veya Git Bash’i açın. `azure-utpm-c` GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-utpm-c.git --recursive
    ```

1. GitHub kök klasörüne gidin ve [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview) simülatörünü çalıştırın. 2321 ve 2322 bağlantı noktalarında bulunan bir yuva üzerinden dinler. Do not close this command window; you need to keep this simulator running until the end of this quickstart guide: 

    ```cmd/sh
    .\azure-utpm-c\tools\tpm_simulator\Simulator.exe
    ```

1. **registerdevice** adlı yeni, boş bir klasör oluşturun. Komut isteminizde aşağıdaki komutu kullanarak **registerdevice** klasöründe bir package.json dosyası oluşturun. `npm` tarafından sorulan tüm soruları yanıtladığınızdan emin olun veya size uygun geliyorsa varsayılanları kabul edin:
   
    ```cmd/sh
    npm init
    ```

1. Aşağıdaki precursor paketlerini yükleyin:

    ```cmd/sh
    npm install node-gyp -g
    npm install ffi -g
    ```

    > [!NOTE]
    > Yukarıdaki paketleri yüklerken bazı bilinen sorunlarla karşılaşabilirsiniz. Bu sorunları çözmek için, **Yönetici olarak çalıştır** modunda açtığınız komut istemini kullanarak `npm install --global --production windows-build-tools` komutunu çalıştırın; yolu, yüklü sürümünüzle değiştirdikten sonra `SET VCTargetsPath=C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V140` komutunu çalıştırın ve ardından yukarıdaki yükleme komutlarını yeniden çalıştırın.
    >

1. Kayıt sırasında kullandığınız bileşenleri içeren aşağıdaki paketleri yükleyin:

   - TPM ile çalışan bir güvenlik istemcisi: `azure-iot-security-tpm`
   - cihazın, Cihaz Sağlama Hizmeti’ne bağlanması için bir aktarım: `azure-iot-provisioning-device-http` veya `azure-iot-provisioning-device-amqp`
   - aktarım veya güvenlik istemcisini kullanmak için bir istemci: `azure-iot-provisioning-device`

     Cihaz kaydedildikten sonra, kayıt sırasında sağlanan kimlik bilgilerini kullanarak cihazınızı bağlamak için her zamanki IoT Hub Cihaz İstemcisi paketlerini kullanabilirsiniz. Şunlar gerekir:

   - cihaz istemcisi: `azure-iot-device`
   - aktarım: `azure-iot-device-amqp`, `azure-iot-device-mqtt` veya `azure-iot-device-http` öğelerinden herhangi biri
   - önceden yüklemiş olduğunuz güvenlik istemcisi: `azure-iot-security-tpm`

     > [!NOTE]
     > Aşağıdaki örneklerde `azure-iot-provisioning-device-http` ve `azure-iot-device-mqtt` aktarımları kullanılır.
     > 

     **registerdevice** klasöründe komut istemcinizde aşağıdaki komutları çalıştırarak bu paketlerin hepsini aynı anda yükleyebilirsiniz:

       ```cmd/sh
       npm install --save azure-iot-device azure-iot-device-mqtt azure-iot-security-tpm azure-iot-provisioning-device-http azure-iot-provisioning-device
       ```

1. Bir metin düzenleyicisi kullanarak **registerdevice** klasöründe yeni bir **ExtractDevice.js** dosyası oluşturun.

1. Aşağıdaki `require` deyimlerini **ExtractDevice.js** dosyasının başlangıcına ekleyin:
   
    ```
    'use strict';

    var tpmSecurity = require('azure-iot-security-tpm');
    var tssJs = require("tss.js");

    var myTpm = new tpmSecurity.TpmSecurityClient(undefined, new tssJs.Tpm(true));
    ```

1. Yöntemi uygulamak için aşağıdaki işlevi ekleyin:
   
    ```
    myTpm.getEndorsementKey(function(err, endorsementKey) {
      if (err) {
        console.log('The error returned from get key is: ' + err);
      } else {
        console.log('the endorsement key is: ' + endorsementKey.toString('base64'));
        myTpm.getRegistrationId((getRegistrationIdError, registrationId) => {
          if (getRegistrationIdError) {
            console.log('The error returned from get registration id is: ' + getRegistrationIdError);
          } else {
            console.log('The Registration Id is: ' + registrationId);
            process.exit();
          }
        });
      }
    });
    ```

1. **ExtractDevice.js** dosyasını kaydedip kapatın. Örneği çalıştırın:

    ```cmd/sh
    node ExtractDevice.js
    ```

1. The output window displays the **_Endorsement key_** and the **_Registration ID_** needed for device enrollment. Bu değerleri not alın. 


## <a name="create-a-device-entry"></a>Cihaz girdisi oluşturma

1. Sign in to the Azure portal, select the **All resources** button on the left-hand menu and open your Device Provisioning service.

1. From the Device Provisioning Service menu, select **Manage enrollments**. Select **Individual Enrollments** tab and select the **Add individual enrollment** button at the top. 

1. In the **Add Enrollment** panel, enter the following information:
   - Kimlik onay *Mekanizması* olarak **TPM** seçeneğini belirleyin.
   - Enter the *Registration ID* and *Endorsement key* for your TPM device from the values you noted previously.
   - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
       - Enter a unique *Device ID*. Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. If you choose not to provide one, the registration ID will be used to identify the device instead.
       - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
   - Once complete, press the **Save** button. 

     ![Portal dikey penceresinde cihaz kayıt bilgilerini girme](./media/quick-create-simulated-device/enter-device-enrollment.png)  

   Kayıt başarıyla tamamlanınca cihazınızın *Kayıt Kimliği* listenin altında *Bireysel Kayıtlar* sekmesinde görünür. 


## <a name="register-the-device"></a>Cihazı kaydetme

1. In the Azure portal, select the **Overview** blade for your Device Provisioning service and note the **_Global Device Endpoint_** and **_ID Scope_** values.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device/extract-dps-endpoints.png) 

1. Bir metin düzenleyicisi kullanarak **registerdevice** klasöründe yeni bir **RegisterDevice.js** dosyası oluşturun.

1. Aşağıdaki `require` deyimlerini **RegisterDevice.js** dosyasının başlangıcına ekleyin:
   
    ```
    'use strict';

    var ProvisioningTransport = require('azure-iot-provisioning-device-http').Http;
    var iotHubTransport = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var tpmSecurity = require('azure-iot-security-tpm');
    var ProvisioningDeviceClient = require('azure-iot-provisioning-device').ProvisioningDeviceClient;
    ```

    > [!NOTE]
    > **Node.js için Azure IoT SDK'sı**, _AMQP_, _AMQP WS_ ve _MQTT WS_ gibi ek protokolleri destekler.  Diğer örnekler için bkz. [Node.js için Cihaz Sağlama Hizmeti SDK'sı örnekleri](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/device/samples).
    > 

1. **globalDeviceEndpoint** ve **idScope** değişkenlerini ekleyin ve **ProvisioningDeviceClient** örneğini oluştururken bunları kullanın. **{globalDeviceEndpoint}** ve **{idScope}** değerlerini **1. Adım**’daki **_Genel Cihaz Uç Noktası_** ve **_Kimlik Kapsamı_** değerleriyle değiştirin:
   
    ```
    var provisioningHost = '{globalDeviceEndpoint}';
    var idScope = '{idScope}';

    var tssJs = require("tss.js");
    var securityClient = new tpmSecurity.TpmSecurityClient('', new tssJs.Tpm(true));
    // if using non-simulated device, replace the above line with following:
    //var securityClient = new tpmSecurity.TpmSecurityClient();

    var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvisioningTransport(), securityClient);
    ```

1. Yöntemi cihazda uygulamak için aşağıdaki işlevi ekleyin:
   
    ```
    provisioningClient.register(function(err, result) {
      if (err) {
        console.log("error registering device: " + err);
      } else {
        console.log('registration succeeded');
        console.log('assigned hub=' + result.registrationState.assignedHub);
        console.log('deviceId=' + result.registrationState.deviceId);
        var tpmAuthenticationProvider = tpmSecurity.TpmAuthenticationProvider.fromTpmSecurityClient(result.registrationState.deviceId, result.registrationState.assignedHub, securityClient);
        var hubClient = Client.fromAuthenticationProvider(tpmAuthenticationProvider, iotHubTransport);

        var connectCallback = function (err) {
          if (err) {
            console.error('Could not connect: ' + err.message);
          } else {
            console.log('Client connected');
            var message = new Message('Hello world');
            hubClient.sendEvent(message, printResultFor('send'));
          }
        };

        hubClient.open(connectCallback);

        function printResultFor(op) {
          return function printResult(err, res) {
            if (err) console.log(op + ' error: ' + err.toString());
            if (res) console.log(op + ' status: ' + res.constructor.name);
            process.exit(1);
          };
        }
      }
    });
    ```

1. **RegisterDevice.js** dosyasını kaydedip kapatın. Örneği çalıştırın:

    ```cmd/sh
    node RegisterDevice.js
    ```

1. IoT hub bilgilerinizi almak için cihaz önyüklemesi ve Cihaz Sağlama Hizmetine bağlanma benzetimi gerçekleştiren iletilere dikkat edin. On successful provisioning of your simulated device to the IoT hub linked with your provisioning service, the device ID appears on the hub's **IoT devices** blade. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device/hub-registration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub'daki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme

If you plan to continue working on and exploring the device client sample, do not clean up the resources created in this quickstart. If you do not plan to continue, use the following steps to delete all resources created by this quickstart.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Makinenizde TPM simülatörü penceresini kapatın.
1. From the left-hand menu in the Azure portal, select **All resources** and then select your Device Provisioning service. Open the **Manage Enrollments** blade for your service, and then select the **Individual Enrollments** tab. Select the check box next to the *REGISTRATION ID* of the device you enrolled in this quickstart, and press the **Delete** button at the top of the pane. 
1. From the left-hand menu in the Azure portal, select **All resources** and then select your IoT hub. Open the **IoT devices** blade for your hub, select the check box next to the *DEVICE ID* of the device you registered in this quickstart, and then press the **Delete** button at the top of the pane.


## <a name="next-steps"></a>Sonraki adımlar

In this quickstart, you’ve created a TPM simulated device on your machine and provisioned it to your IoT hub using the IoT Hub Device Provisioning Service. To learn how to enroll your TPM device programmatically, continue to the quickstart for programmatic enrollment of a TPM device. 

> [!div class="nextstepaction"]
> [Azure quickstart - Enroll TPM device to Azure IoT Hub Device Provisioning Service](quick-enroll-device-tpm-node.md)
