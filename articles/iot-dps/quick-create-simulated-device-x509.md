---
title: C kullanarak Azure IoT Hub'ına X.509 aygıtının sağlanması
description: Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır. Bu hızlı başlangıçta, Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) için C aygıtı SDK'yı kullanarak simüle edilmiş bir X.509 aygıtı oluşturur ve sağlarsınız.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: f0c95e495e222cc72f0a6fc432404fcbaa47df65
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241167"
---
# <a name="quickstart-provision-an-x509-simulated-device-using-the-azure-iot-c-sdk"></a>Hızlı başlangıç: Azure IoT C SDK'sını kullanarak simülasyon X.509 cihazı sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Bu hızlı başlangıçta bir Windows geliştirme makinesi üzerinde bir X.509 cihazı simülatörü oluşturmayı ve çalıştırmayı öğreneceksiniz. Cihaz Sağlama Hizmeti örneği ile bir kayıt kullanarak bir IoT hub'ına atanacak bu simülasyon cihazını yapılandıracaksınız. Cihaz için önyükleme sırası simülasyonu yapmak için [Azure IoT C SDK'sından](https://github.com/Azure/azure-iot-sdk-c) alınan örnek kod kullanılacaktır. Cihaz, sağlama hizmeti ile kayıt durumuna göre tanınacak ve IoT hub'ına atanacaktır.

Otomatik sağlama işlemini bilmiyorsanız, [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirin. Ayrıca, bu hızlı başlangıçla devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalla ayarlama](quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. 

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:

* [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
* [Bireysel kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede bireysel kayıtlar gösterilmektedir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşullar Windows geliştirme ortamı içindir. Linux veya macOS için, SDK belgelerinde [geliştirme ortamınızı hazırlayın'daki](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) uygun bölüme bakın.

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2019 ile ['C++'](https://docs.microsoft.com/cpp/?view=vs-2019#pivot=workloads) iş yükü ile masaüstü geliştirme özelliğine sahip. Visual Studio 2015 ve Visual Studio 2017 de desteklendi.

* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

## <a name="prepare-a-development-environment-for-the-azure-iot-c-sdk"></a>Azure IoT C SDK'sı için geliştirme ortamını hazırlama

Bu bölümde, X.509 önyükleme dizisinin örnek kodunu içeren [Azure IoT C SDK'yı](https://github.com/Azure/azure-iot-sdk-c)oluşturmak için kullanılan bir geliştirme ortamı hazırlayacaktır.

1. [CMake yapı sistemini](https://cmake.org/download/)indirin.

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. SDK'nın [en son sürümü](https://github.com/Azure/azure-iot-sdk-c/releases/latest) için etiket adını bulun.

3. Komut istemini veya Git Bash kabuğunu açın. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunun en son sürümünde klonlamak için aşağıdaki komutları çalıştırın. Önceki adımda bulduğunuz etiketi `-b` parametre nin değeri olarak kullanın:

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

4. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. Dizinden aşağıdaki komutları `azure-iot-sdk-c` çalıştırın:

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

5. Kod örneği, X.509 kimlik doğrulaması aracılığıyla kanıtlama sağlamak için bir X.509 sertifikası kullanır. SDK'nın aygıt sağlama istemcisini içeren geliştirme platformunuza özgü bir sürümünü oluşturmak için aşağıdaki komutu çalıştırın. Dizinde simüle edilen aygıt için bir `cmake` Visual Studio çözümü oluşturulur.

    ```cmd
    cmake -Duse_prov_client:BOOL=ON ..
    ```

    `cmake`, C++ derleyicinizi bulamazsa yukarıdaki komutu çalıştırırken derleme hatalarıyla karşılaşabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin.

    Yapı başarılı olduktan sonra, son birkaç çıktı satırı aşağıdaki çıktıya benzer görünür:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON ..
    -- Building for: Visual Studio 16 2019
    -- The C compiler identification is MSVC 19.23.28107.0
    -- The CXX compiler identification is MSVC 19.23.28107.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: C:/code/azure-iot-sdk-c/cmake
    ```

## <a name="create-a-self-signed-x509-device-certificate"></a>Otomatik olarak imzalanan X.509 sertifikası oluşturma

Bu bölümde otomatik olarak imzalanan bir X.509 sertifikası kullanacaksınız. Aşağıdaki noktaları göz önünde bulundurmak önemlidir:

* Otomatik olarak imzalanan sertifikalar yalnızca test amaçlıdır ve üretimde kullanılmamalıdır.
* Otomatik olarak imzalanan sertifikanın varsayılan sona erme tarihi bir yıldır.

Benzetim cihazının tek kayıt girdisiyle kullanılacak sertifikayı oluşturmak için Azure IoT C SDK'sından örnek kodu kullanacaksınız.

1. Visual Studio’yu başlatın ve `azure_iot_sdks.sln` adlı yeni çözüm dosyasını açın. Bu çözüm dosyası daha önce azure-iot-sdk-c git deposunun kökünde oluşturduğunuz `cmake` klasöründe yer alır.

2. Visual Studio menüsünde, çözümdeki tüm projeleri oluşturmak için **Build** > **Solution'ı** seçin.

3. Visual Studio'nun *Çözüm Gezgini* penceresinde **Sağlama\_Araçları** klasörüne gidin. **dice\_device\_enrollment** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin.

4. Visual Studio menüsünde, çözümü çalıştırmak için hata ayıklama yapmadan **Hata Ayıklama** > **Başlat'ı** seçin. Çıktı penceresinde, istendiğinde tek kayıt için **i** girin.

    Çıktı penceresi, sanal cihazınız için yerel olarak oluşturulmuş otomatik olarak imzalanan X.509 sertifikasını görüntüler. **-----BEGIN CERTIFICATE-----** ile başlayıp ilk **-----END PUBLIC KEY-----** ile biten çıktıyı panoya kopyalayın ve bu iki satırı da dahil ettiğinizden emin olun. Yalnızca çıktı penceresindeki ilk sertifikayı kopyalamanız gerekir.

5. Metin düzenleyicisi kullanarak sertifikayı **_X509testcert.pem_** adlı yeni bir dosyaya kaydedin.

## <a name="create-a-device-enrollment-entry-in-the-portal"></a>Portalda bir cihaz kaydı girişi oluşturma

1. Azure portalında oturum açın, sol menüdeki **Tüm kaynaklar** düğmesini seçin ve Cihaz Sağlama hizmetinizi açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından üstteki **tek tek kayıt ekle** düğmesini seçin.

3. Kayıt **Ekle** paneline aşağıdaki bilgileri girin ve **kaydet** düğmesine basın.

    * **Mekanizma:** Kimlik onay *Mekanizması* olarak **X.509**'u seçin.
    * **Birincil sertifika .pem veya .cer dosyası:** Daha önce oluşturduğunuz sertifika dosyası X509testcert.pem'i seçmek için **bir dosya** seçin'i seçin.
    * **IoT Hub Cihaz kimliği:** Cihaza bir kimlik vermek için **test-docs-cert-device** girin.

      [![Portalda X.509 attestation için bireysel kayıt ekleme](./media/quick-create-simulated-device-x509/device-enrollment.png)](./media/quick-create-simulated-device-x509/device-enrollment.png#lightbox)

      Kayıt başarıyla tamamlandığında, X.509 cihazınız **Bireysel Kayıtlar** sekmesindeki *Kayıt Kimliği* sütununun altında *riot-device-cert* olarak gösterilir. 

## <a name="simulate-first-boot-sequence-for-the-device"></a>Cihazın ilk önyükleme sırasını benzetim olarak çalıştırma

Bu bölümde cihazın önyükleme sırasını Cihaz Sağlama Hizmeti örneğinize göndermek için örnek kodu güncelleştireceksiniz. Bu önyükleme sırası cihazın tanınmasına ve Cihaz Sağlama Hizmeti örneğine bağlı bir IoT hub'ına atanmasına neden olur.

1. Azure portalında, Aygıt Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini not edin.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Visual Studio'nun *Çözüm Gezgini* penceresinde **Sağlama\_Örnekleri** klasörüne gidin. **prov\_dev\_client\_sample** adlı örnek projeyi genişletin. **Kaynak Dosyalar**'ı genişletin ve **prov\_dev\_client\_sample.c** dosyasını açın.

3. `id_scope` sabitini bulun ve değeri daha önce kopyalamış olduğunuz **Kimlik Kapsamı** değerinizle değiştirin. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

4. Aynı dosyada `main()` işlevinin tanımını bulun. `hsm_type` değişkeninin aşağıda gösterildiği gibi `SECURE_DEVICE_TYPE_TPM` yerine `SECURE_DEVICE_TYPE_X509` değerine ayarlandığından emin olun.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

5. **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin.

6. Visual Studio menüsünde, çözümü çalıştırmak için hata ayıklama yapmadan **Hata Ayıklama** > **Başlat'ı** seçin. Projeyi yeniden oluşturma isteminde, çalıştırmadan önce projeyi yeniden oluşturmak için **Evet'i** seçin.

    Aşağıdaki çıkış, başarıyla önyüklemesi yapılan cihaz istemci örneğini sağlama ve IoT hub bilgilerini almak üzere sağlama Hizmeti örneğine bağlanıp kaydolma işlemlerinin bir örneğidir:

    ```cmd
    Provisioning API Version: 1.2.7

    Registering... Press enter key to interrupt.

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service:
    test-docs-hub.azure-devices.net, deviceId: test-docs-cert-device
    ```

7. Portalda, sağlama hizmetinize bağlı IoT hub'ına gidin ve **IoT aygıtları** sekmesini seçin. Simüle edilen X.509 aygıtının hub'a başarılı bir şekilde sağlanması üzerine, aygıt kimliği **IoT aygıtları** bıçağında görünür ve *STATUS* **etkindir.** En üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device/hub-registration.png) 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aygıt istemcisi örneği üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından Aygıt Sağlama hizmetinizi seçin. Hizmetiniz için **Kayıtları Yönet'i** açın ve ardından **Bireysel Kayıtlar** sekmesini seçin. Bu hızlı başlangıç ta kaydolduğunuz aygıtın *KAYIT KİmLİğİ'nin* yanındaki onay kutusunu seçin ve bölmenin üst kısmındaki **Sil** düğmesine basın. 
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından IoT hub'ınızı seçin. Hub'ınız için **IoT aygıtlarını** açın, bu hızlı başlatmada kaydettiğiniz aygıtın *AYGıT Kimliğinin* yanındaki onay kutusunu seçin ve ardından bölmenin üst kısmındaki **Sil** düğmesine basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Windows makinenizde simüle edilmiş bir X.509 aygıtı oluşturdunuz ve portaldaki Azure IoT Hub Aygıt Sağlama Hizmetini kullanarak IoT hub'ınıza sokuldunuz. X.509 aygıtınızı programlı olarak nasıl kaydedeceğinizi öğrenmek için, X.509 aygıtlarının programlı kaydı için hızlı bir şekilde başlamaya devam edin. 

> [!div class="nextstepaction"]
> [Azure quickstart - X.509 aygıtlarını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-x509-java.md)
