---
title: 'Quickstart: Provision a simulated TPM device to Azure IoT Hub using C'
description: Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır. Bu hızlı başlangıçta, Azure IoT Hub Cihazı Sağlama Hizmeti için C cihaz SDK'sını kullanarak bir simülasyon TPM cihazı oluşturur ve sağlarsınız.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 0330476650af205854b6d0d4be098c28b46e78a1
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74423184"
---
# <a name="quickstart-provision-a-simulated-tpm-device-using-the-azure-iot-c-sdk"></a>Hızlı başlangıç: Azure IoT C SDK'sını kullanarak simülasyon TPM cihazı sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

Bu hızlı başlangıçta, Windows geliştirme makinesi üzerinde Güvenilir Platform Modülü (TPM) cihazı simülatörünü oluşturmayı ve çalıştırmayı öğreneceksiniz. Cihaz Sağlama Hizmeti örneğini kullanarak bu simülasyon cihazını bir IoT hub'ına bağlayacaksınız. Cihazı Cihaz Sağlama Hizmeti örneğine kaydetmek ve cihaz için bir önyükleme sırası simülasyonu yapmak için [Azure IoT C SDK'sındaki](https://github.com/Azure/azure-iot-sdk-c) örnek kodu kullanacaksınız.

Otomatik sağlama işlemini bilmiyorsanız, [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirin. Ayrıca, bu hızlı başlangıçla devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. 

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:
- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Bireysel kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede bireysel kayıtlar gösterilmektedir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 or later with the ['Desktop development with C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) workload enabled.
* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.


<a id="setupdevbox"></a>

## <a name="prepare-a-development-environment-for-the-azure-iot-c-sdk"></a>Azure IoT C SDK'sı için geliştirme ortamını hazırlama

Bu bölümde, [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) ile [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview) cihaz simülatör örneğini oluşturmak için kullanılan geliştirme ortamını hazırlayacaksınız.

1. Download the [CMake build system](https://cmake.org/download/).

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.


3. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

## <a name="build-the-sdk-and-run-the-tpm-device-simulator"></a>SDK'yı derleme ve TPM cihaz simülatörünü çalıştırma

Bu bölümde, TPM cihaz simülatörü örnek kodunu içeren Azure IoT C SDK'sını oluşturacaksınız. Bu örnek, Paylaşılan Erişim İmzası (SAS) Belirteci kimlik doğrulaması yoluyla bir TPM [kanıtlama mekanizması](concepts-security.md#attestation-mechanism) sağlar.

1. azure-iot-sdk-c git deposunda oluşturduğunuz `cmake` alt dizininden aşağıdaki komutu çalıştırarak örneği derleyin. Bu derleme komutuyla simülasyon cihazı için bir de Visual Studio çözümü oluşturulur.

    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
    ```

    `cmake`, C++ derleyicinizi bulamazsa yukarıdaki komutu çalıştırırken derleme hatalarıyla karşılaşabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin. 

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

2. Kopyaladığınız git deposunun kök klasörüne gidin ve aşağıda gösterilen yolu kullanarak [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview) simülatörünü çalıştırın. Bu simülatör 2321 ve 2322 bağlantı noktalarında bulunan bir yuva üzerinden dinler. Bu komut penceresini kapatmayın; bu hızlı başlangıcın sonuna kadar simülatörü çalışır durumda tutmanız gerekir. 

   *cmake* klasöründeyseniz aşağıdaki komutları çalıştırın:

    ```cmd/sh
    cd ..
    .\provisioning_client\deps\utpm\tools\tpm_simulator\Simulator.exe
    ```

    Simülatörden hiçbir çıkış görmeyeceksiniz. TPM cihazının simülasyonunu yaparak çalışmaya devam etmesine izin verin.

<a id="simulatetpm"></a>

## <a name="read-cryptographic-keys-from-the-tpm-device"></a>TPM cihazından şifreleme anahtarlarını okuma

Bu bölümde, çalışır ve 2321 ile 2322 numaralı bağlantı noktaları üzerinden dinler durumda bıraktığınız TPM simülatöründen onay anahtarını ve kayıt kimliğini okuyan bir örnek derleyecek ve bu örneği yürüteceksiniz. Bu değerler, Cihaz Sağlama Hizmeti örneğinize cihaz kaydı için kullanılacak.

1. Visual Studio’yu başlatın ve `azure_iot_sdks.sln` adlı yeni çözüm dosyasını açın. Bu çözüm dosyası daha önce azure-iot-sdk-c git deposunun kökünde oluşturduğunuz `cmake` klasöründe yer alır.

2. Çözümdeki tüm projeleri derlemek için Visual Studio menüsünde **Derle** > **Çözümü Derle**'yi seçin.

3. Visual Studio'nun *Çözüm Gezgini* penceresinde **Sağlama\_Araçları** klasörüne gidin. **tpm_device_provision** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 

4. Çözümü çalıştırmak için Visual Studio menüsünde **Hata Ayıkla** > **Hata ayıklama olmadan başlat**'ı seçin. The app reads and displays a **_Registration ID_** and an **_Endorsement key_** . Note or copy these values. Bunlar, bir sonraki bölümde cihaz kaydı için kullanılacaktır. 


<a id="portalenrollment"></a>

## <a name="create-a-device-enrollment-entry-in-the-portal"></a>Portalda bir cihaz kaydı girişi oluşturma

1. Sign in to the Azure portal, select the **All resources** button on the left-hand menu and open your Device Provisioning service.

1. Select the **Manage enrollments** tab, and then select the **Add individual enrollment** button at the top. 

1. In the **Add Enrollment** panel, enter the following information:
   - Kimlik onay *Mekanizması* olarak **TPM** seçeneğini belirleyin.
   - Enter the *Registration ID* and *Endorsement key* for your TPM device from the values you noted previously.
   - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
       - Enter a unique *Device ID* (you can use the suggested **test-docs-device** or provide your own). Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. If you choose not to provide one, the registration ID will be used to identify the device instead.
       - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
   - Once complete, press the **Save** button. 

      ![Portala cihaz kayıt bilgilerini girme](./media/quick-create-simulated-device/enter-device-enrollment.png)  

      Kayıt başarıyla tamamlanınca cihazınızın *Kayıt Kimliği* listenin altında *Tek Tek Kayıtlar* sekmesinde görünür. 


<a id="firstbootsequence"></a>

## <a name="simulate-first-boot-sequence-for-the-device"></a>Cihazın ilk önyükleme sırasını benzetim olarak çalıştırma

Bu bölümde, örnek kodu yapılandırarak cihazın önyükleme sırasını Cihaz Sağlama Hizmeti örneğinize göndermek üzere [Gelişmiş İleti Sıraya Alma Protokolü (AMQP)](https://wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) kullanmasını sağlayacaksınız. Bu önyükleme sırası cihazın tanınmasına ve Cihaz Sağlama Hizmeti örneğine bağlı bir IoT hub'ına atanmasına neden olur.

1. Azure portalda, Cihaz Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini kopyalayın.

    ![Portaldan Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device/extract-dps-endpoints.png) 

2. Visual Studio'nun *Çözüm Gezgini* penceresinde **Sağlama\_Örnekleri** klasörüne gidin. **prov\_dev\_client\_sample** adlı örnek projeyi genişletin. **Kaynak Dosyalar**'ı genişletin ve **prov\_dev\_client\_sample.c** dosyasını açın.

3. Dosyanın üst kısmında, aşağıda gösterildiği gibi her cihaz protokolü için `#define` deyimlerini bulun. Yalnızca `SAMPLE_AMQP` öğesinin açıklama haline getirilmediğinden emin olun.

    Şu anda, [TPM Bireysel Kaydı için MQTT protokolü desteklenmemektedir](https://github.com/Azure/azure-iot-sdk-c#provisioning-client-sdk).

    ```c
    //
    // The protocol you wish to use should be uncommented
    //
    //#define SAMPLE_MQTT
    //#define SAMPLE_MQTT_OVER_WEBSOCKETS
    #define SAMPLE_AMQP
    //#define SAMPLE_AMQP_OVER_WEBSOCKETS
    //#define SAMPLE_HTTP
    ```

4. `id_scope` sabitini bulun ve değeri daha önce kopyalamış olduğunuz **Kimlik Kapsamı** değerinizle değiştirin. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Aynı dosyada `main()` işlevinin tanımını bulun. `hsm_type` değişkeninin aşağıda gösterildiği gibi `SECURE_DEVICE_TYPE_X509` yerine `SECURE_DEVICE_TYPE_TPM` değerine ayarlandığından emin olun.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

6. **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 

7. Çözümü çalıştırmak için Visual Studio menüsünde **Hata Ayıkla** > **Hata ayıklama olmadan başlat**'ı seçin. In the prompt to rebuild the project, select **Yes**, to rebuild the project before running.

    Aşağıdaki çıkış, başarıyla önyüklemesi yapılan cihaz istemci örneğini sağlama ve IoT hub bilgilerini almak üzere Cihaz Sağlama Hizmeti'ne bağlanıp kaydolma işlemlerinin bir örneğidir:

    ```cmd
    Provisioning API Version: 1.2.7
    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED

    Registering... Press enter key to interrupt.

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service:
    test-docs-hub.azure-devices.net, deviceId: test-docs-device
    ```

8. Once the simulated device is provisioned to the IoT hub by your provisioning service, the device ID appears with the hub's **IoT devices**. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device/hub-registration.png) 


## <a name="clean-up-resources"></a>Kaynakları temizleme

If you plan to continue working on and exploring the device client sample, do not clean up the resources created in this quickstart. If you do not plan to continue, use the following steps to delete all resources created by this quickstart.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
2. Makinenizde TPM simülatörü penceresini kapatın.
3. From the left-hand menu in the Azure portal, select **All resources** and then select your Device Provisioning service. Open **Manage Enrollments** for your service, and then select the **Individual Enrollments** tab. Select the check box next to the *REGISTRATION ID* of the device you enrolled in this quickstart, and press the **Delete** button at the top of the pane. 
4. From the left-hand menu in the Azure portal, select **All resources** and then select your IoT hub. Open **IoT devices** for your hub, select the check box next to the *DEVICE ID* of the device you registered in this quickstart, and then press the **Delete** button at the top of the pane.

## <a name="next-steps"></a>Sonraki adımlar

In this quickstart, you’ve created a TPM simulated device on your machine and provisioned it to your IoT hub using the IoT Hub Device Provisioning Service. To learn how to enroll your TPM device programmatically, continue to the quickstart for programmatic enrollment of a TPM device. 

> [!div class="nextstepaction"]
> [Azure quickstart - Enroll TPM device to Azure IoT Hub Device Provisioning Service](quick-enroll-device-tpm-java.md)
