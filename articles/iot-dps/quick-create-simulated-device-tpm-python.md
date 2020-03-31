---
title: Quickstart - Python kullanarak Azure IoT Hub'ına simüle edilmiş TPM aygıtısağlama
description: Quickstart - IoT Hub Aygıt Sağlama Hizmeti (DPS) için Java cihazı SDK'yı kullanarak simüle edilmiş bir TPM aygıtı oluşturun ve tedarik edin. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: python
ms.custom: mvc
ms.openlocfilehash: 67206f36d5c9f08a2110b02f1d3681684cda8a66
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77605422"
---
# <a name="quickstart-create-and-provision-a-simulated-tpm-device-using-python-device-sdk-for-iot-hub-device-provisioning-service"></a>Quickstart: IoT Hub Aygıt Sağlama Hizmeti için Python cihazı SDK'yı kullanarak simüle edilmiş bir TPM aygıtı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

Bu hızlı başlatmada, Windows bilgisayarında simüle edilmiş bir IoT aygıtı oluşturursunuz. Benzetimli aygıt, Donanım Güvenlik Modülü (HSM) olarak bir TPM simülatörü içerir. Aygıt Sağlama Hizmeti (DPS) ile tek tek bir kayıt kullanarak bu simüle cihazı IoT hub'ınıza bağlamak için aygıt örneği Python kodunu kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Otomatik [sağlama kavramlarının](concepts-auto-provisioning.md)gözden geçirilmesi.
- Azure [portalı ile IoT Hub Aygıt Sağlama Hizmeti Ayarlama'nın](./quick-setup-auto-provision.md)tamamlanması.
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Visual Studio 2015+](https://visualstudio.microsoft.com/vs/) ile Masaüstü geliştirme ile C++.
- [CMake yapı sistemi](https://cmake.org/download/).
- [Git.](https://git-scm.com/download/)

> [!IMPORTANT]
> Bu makale yalnızca amortismana uygulanan V1 Python SDK için geçerlidir. Iot Hub Aygıt Sağlama Hizmeti için aygıt ve servis istemcileri v2'de henüz kullanıma sunulmadı. Takım şu anda v2 özelliği parite getirmek için iş başında.

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Visual Studio yüklemeniz için 'C++' iş yükü etkinken [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 veya sonraki lerini yüklediğinizden emin olun.

1. [CMake derleme sistemini](https://cmake.org/download/) indirin ve yükleyin.

1. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

1. Bir komut istemi veya Git Bash’i açın. Cihaz benzetim kod örneği için GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone --single-branch --branch v1-deprecated https://github.com/Azure/azure-iot-sdk-python.git --recursive
    ```

1. CMake oluşturma işlemi için bu GitHub deposunun yerel kopyasında bir klasör oluşturun. 

    ```cmd/sh
    cd azure-iot-sdk-python/c
    mkdir cmake
    cd cmake
    ```

1. Kod örneği, bir Windows TPM simülatörü kullanır. SAS belirteci kimlik doğrulamasını etkinleştirmek için aşağıdaki komutu çalıştırın. Sanal cihaz için ayrıca bir Visual Studio çözümü oluşturur.

    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
    ```

1. Ayrı bir komut isteminde, TPM simülatörü klasörüne gidin ve [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview) simülatörü simüle aygıtı için [HSM](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) olarak çalıştırın. **Erişime İzin Ver**'e tıklayın. 2321 ve 2322 bağlantı noktalarında bulunan bir yuva üzerinden dinler. Bu komut pencereyi kapatmayın; Bu hızlı başlatma kılavuzunun sonuna kadar bu simülatörü çalışır durumda tutmak gerekir. 

    ```cmd/sh
    .\azure-iot-sdk-python\c\provisioning_client\deps\utpm\tools\tpm_simulator\Simulator.exe
    ```

    ![TPM Simülatörü](./media/python-quick-create-simulated-device/tpm-simulator.png)


## <a name="create-a-device-enrollment-entry"></a>Cihaz kaydı girişi oluşturma

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:

- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Tek tek kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede, tek tek kayıtları gösterir.

1. `azure_iot_sdks.sln` adlı *cmake* klasöründe oluşturulan çözümü açın ve Visual Studio'da derleyin.

1. **tpm_device_provision** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. Çözümü çalıştırın. Çıktı **_penceresi, onay anahtarını_** ve aygıt kaydı için gereken **_Kayıt Kimliğini_** görüntüler. Bu değerleri not alın. 

    ![TPM kurulumu](./media/python-quick-create-simulated-device/tpm-setup.png)

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

     ![Portal dikey penceresinde cihaz kayıt bilgilerini girme](./media/python-quick-create-simulated-device/enterdevice-enrollment.png)  

   Kayıt başarıyla tamamlanınca cihazınızın *Kayıt Kimliği* listenin altında *Tek Tek Kayıtlar* sekmesinde görünür. 


## <a name="simulate-the-device"></a>Cihazı benzetme

1. [Python 2.x veya 3.x](https://www.python.org/downloads/) sürümünü indirip yükleyin. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python'u eklediğinizden emin olun.
    - Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketi](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads).

1. Python paketlerini derlemek için [bu yönergeleri](https://github.com/Azure/azure-iot-sdk-python/blob/v1-deprecated/doc/python-devbox-setup.md) uygulayın.

   > [!NOTE]
   > `build_client.cmd` çalıştırıyorsanız `--use-tpm-simulator` bayrağını kullandığınızdan emin olun.
   > 
   > [!NOTE]
   > `pip` kullanıyorsanız `azure-iot-provisioning-device-client` paketini de yüklemeyi unutmayın. Yayımlanan PIP paketlerinin simülatörü değil gerçek TPM uygulamasını kullandığına dikkat edin. Simülatörü kullanmak için `--use-tpm-simulator` işaretini kullanarak kaynaktan derlemeniz gerekir.

1. Örnekler klasörüne gidin.

    ```cmd/sh
    cd azure-iot-sdk-python/provisioning_device_client/samples
    ```

1. Python IDE'nizi kullanarak **provisioning\_device\_client\_sample.py** adlı Python betiğini düzenleyin. *GLOBAL\_PROV\_URI* ve *ID\_SCOPE* değişkenlerini önceden not ettiğiniz değerlerle değiştirin. Ayrıca *GÜVENLİK\_CİHAZ\_TÜRÜNÜN*`ProvisioningSecurityDeviceType.TPM` olarak ayarlandığından emin olun

    ```python
    GLOBAL_PROV_URI = "{globalServiceEndpoint}"
    ID_SCOPE = "{idScope}"
    SECURITY_DEVICE_TYPE = ProvisioningSecurityDeviceType.TPM
    PROTOCOL = ProvisioningTransportProvider.HTTP
    ```

    ![Hizmet bilgileri](./media/python-quick-create-simulated-device/extract-dps-endpoints.png)

1. Örnek uygulamayı çalıştırın. 

    ```cmd/sh
    python provisioning_device_client_sample.py
    ```

1. IoT hub bilgilerinizi almak için cihaz önyüklemesi ve Cihaz Sağlama Hizmetine bağlanma benzetimi gerçekleştiren iletilere dikkat edin. 

    ![Başarılı kayıt](./media/python-quick-create-simulated-device/registration-success.png)

1. Simüle edilmiş aygıtınızın sağlama hizmetiyle bağlantılı IoT hub'ına başarılı bir şekilde sağlanması üzerine, aygıt kimliği hub'ın **IoT aygıtlar** bıçak üzerinde görünür.

    ![Cihaz IOT hub'da kayıtlı](./media/python-quick-create-simulated-device/hubregistration.png) 

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
> [Azure quickstart - TPM aygıtını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-tpm-python.md)
