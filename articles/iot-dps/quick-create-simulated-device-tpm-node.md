---
title: Quickstart - Node.js kullanarak Azure IoT Hub'ına simüle edilmiş TPM aygıtısağlama
description: Quickstart - Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) için Node.js cihazı SDK'yı kullanarak simüle edilmiş bir TPM aygıtı oluşturun ve sağlayın. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 93e68246d1c978bdb1517922f0284524395c218a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77605465"
---
# <a name="quickstart-create-and-provision-a-simulated-tpm-device-using-nodejs-device-sdk-for-iot-hub-device-provisioning-service"></a>Quickstart: IoT Hub Aygıt Sağlama Hizmeti için Node.js cihazı SDK kullanarak simüle edilmiş bir TPM aygıtı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

Bu hızlı başlatmada, Windows bilgisayarında simüle edilmiş bir IoT aygıtı oluşturursunuz. Benzetimli aygıt, Donanım Güvenlik Modülü (HSM) olarak bir TPM simülatörü içerir. Aygıt Sağlama Hizmeti (DPS) ile tek tek bir kayıt kullanarak bu simüle cihazı IoT hub'ınıza bağlamak için aygıt örneği Node.js kodunu kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Otomatik [sağlama kavramlarının](concepts-auto-provisioning.md)gözden geçirilmesi.
- Azure [portalı ile IoT Hub Aygıt Sağlama Hizmeti Ayarlama'nın](./quick-setup-auto-provision.md)tamamlanması.
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Düğüm.js v4.0+](https://nodejs.org).
- [Git.](https://git-scm.com/download/)

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Makinenizde [Node.js v4.0 veya üzeri](https://nodejs.org) bir sürümün yüklü olduğundan emin olun.

1. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 


## <a name="simulate-a-tpm-device"></a>TPM cihazının benzetimini yapma

1. Bir komut istemi veya Git Bash’i açın. `azure-utpm-c` GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-utpm-c.git --recursive
    ```

1. GitHub kök klasörüne gidin ve [tpm](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview) simülatörünü simüle edilen aygıtın [HSM'si](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) olarak çalıştırın. 2321 ve 2322 bağlantı noktalarında bulunan bir yuva üzerinden dinler. Bu komut pencereyi kapatmayın; Bu hızlı başlangıç kılavuzunun sonuna kadar bu simülatörü çalışır durumda tutmanız gerekir: 

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

1. Çıktı **_penceresi, onay anahtarını_** ve aygıt kaydı için gereken **_Kayıt Kimliğini_** görüntüler. Bu değerleri not alın. 


## <a name="create-a-device-entry"></a>Cihaz girdisi oluşturma

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:

- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Tek tek kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede, tek tek kayıtları gösterir.

1. Azure portalında oturum açın, sol menüdeki **Tüm kaynaklar** düğmesini seçin ve Cihaz Sağlama hizmetinizi açın.

1. Aygıt Sağlama Hizmeti **menüsünden, kayıtları Yönet'i**seçin. **Bireysel Kayıtlar** sekmesini seçin ve üstteki tek tek **kayıt ekle** düğmesini seçin. 

1. Kayıt **Ekle** paneline aşağıdaki bilgileri girin:
   - Kimlik onay *Mekanizması* olarak **TPM** seçeneğini belirleyin.
   - TPM aygıtınızın *Kayıt Kimliği* ve *Onay anahtarını* daha önce belirttiğiniz değerlerden girin.
   - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
       - Benzersiz bir *Aygıt Kimliği*girin. Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. Bir tane sağlamamayı seçerseniz, bunun yerine aygıtı tanımlamak için kayıt kimliği kullanılır.
       - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
   - Tamamlandıktan sonra **Kaydet** düğmesine basın. 

     ![Portal dikey penceresinde cihaz kayıt bilgilerini girme](./media/quick-create-simulated-device/enter-device-enrollment.png)  

   Kayıt başarıyla tamamlanınca cihazınızın *Kayıt Kimliği* listenin altında *Bireysel Kayıtlar* sekmesinde görünür. 


## <a name="register-the-device"></a>Cihazı kaydetme

1. Azure portalında, Aygıt Sağlama hizmetiniz için **Genel Bakış** bıçağını seçin ve Genel Aygıt **_Bitiş Noktası_** ve Kimlik **_Kapsamı_** değerlerine dikkat edin.

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

1. IoT hub bilgilerinizi almak için cihaz önyüklemesi ve Cihaz Sağlama Hizmetine bağlanma benzetimi gerçekleştiren iletilere dikkat edin. Simüle edilmiş aygıtınızın sağlama hizmetiyle bağlantılı IoT hub'ına başarılı bir şekilde sağlanması üzerine, aygıt kimliği hub'ın **IoT aygıtlar** bıçak üzerinde görünür. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device/hub-registration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub'daki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Aygıt istemcisi örneği üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Makinenizde TPM simülatörü penceresini kapatın.
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından Aygıt Sağlama hizmetinizi seçin. Hizmetiniz için **Kayıtları Yönet'i** açın ve ardından **Bireysel Kayıtlar** sekmesini seçin. Bu hızlı başlangıç ta kayıt olduğunuz aygıtın *KAYIT KİmLİğİ'nin* yanındaki onay kutusunu seçin ve bölmenin üst kısmındaki **Sil** düğmesine basın. 
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından IoT hub'ınızı seçin. Hub'ınız için **IoT aygıtları** bıçağını açın, bu hızlı başlatmada kaydettiğiniz aygıtın *AYGıT Kimliğinin* yanındaki onay kutusunu seçin ve ardından bölmenin üst kısmındaki **Sil** düğmesine basın.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, makinenizde bir TPM simüle edilmiş aygıt oluşturdunuz ve IoT Hub Aygıt Sağlama Hizmeti'ni kullanarak IoT hub'ınıza sokulmuşsunuz. TPM aygıtınızı programlı olarak nasıl kaydedebilirsiniz öğrenmek için, bir TPM aygıtının programlı kaydı için hızlı bir şekilde başlamaya devam edin. 

> [!div class="nextstepaction"]
> [Azure quickstart - TPM aygıtını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-tpm-node.md)
