---
title: Python kullanarak Azure IoT Hub'ına X.509 aygıtını sağlama
description: Quickstart - IoT Hub Aygıt Sağlama Hizmeti (DPS) için Python cihazı SDK'yı kullanarak simüle edilmiş bir X.509 aygıtı oluşturun ve tedarik edin. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: python
ms.custom: mvc
ms.openlocfilehash: 44f1a2cd3336eeae87878c333fb05d2e6b1f88e8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77605382"
---
# <a name="quickstart-create-and-provision-a-simulated-x509-device-using-python-device-sdk-for-iot-hub-device-provisioning-service"></a>Quickstart: IoT Hub Aygıt Sağlama Hizmeti için Python cihazı SDK'yı kullanarak simüle edilmiş bir X.509 aygıtı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Bu hızlı başlatmada, Windows bilgisayarında simüle edilmiş bir X.509 aygıtı oluşturursunuz. Aygıt Sağlama Hizmeti (DPS) ile tek tek bir kayıt kullanarak bu simüle cihazı IoT hub'ınıza bağlamak için aygıt örneği Python kodunu kullanırsınız.

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

2. [CMake derleme sistemini](https://cmake.org/download/) indirin ve yükleyin.

3. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

4. Bir komut istemi veya Git Bash’i açın. Cihaz benzetim kod örneği için GitHub deposunu kopyalayın.
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-python.git --recursive
    ```

5. CMake oluşturma işlemi için bu GitHub deposunun yerel kopyasında bir klasör oluşturun. 

    ```cmd/sh
    cd azure-iot-sdk-python/c
    mkdir cmake
    cd cmake
    ```

6. Aşağıdaki komutu çalıştırarak sağlama istemcisi için Visual Studio çözümü oluşturun.

    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON ..
    ```


## <a name="create-a-self-signed-x509-device-certificate-and-individual-enrollment-entry"></a>Otomatik olarak imzalanan X.509 cihazı sertifikası ve bireysel kayıt girişi oluşturma

Bu bölümde otomatik imzalanan bir X.509 sertifikası kullanacaksınız. Aşağıdaki noktaları göz önünde bulundurmanız önemlidir:

* Otomatik olarak imzalanan sertifikalar yalnızca test amaçlıdır ve üretimde kullanılmamalıdır.
* Otomatik olarak imzalanan sertifikanın varsayılan sona erme tarihi bir yıldır.

Benzetim cihazının tek kayıt girdisiyle kullanılacak sertifikayı oluşturmak için Azure IoT C SDK'sından örnek kodu kullanacaksınız.

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:

- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Tek tek kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede, tek tek kayıtları gösterir.

1. `azure_iot_sdks.sln` adlı *cmake* klasöründe oluşturulan çözümü açın ve Visual Studio'da derleyin.

2. **Sağlama\_Araçlar** klasörünün altında **dice\_device\_enrollment** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**'yı seçin. Çözümü çalıştırın. 

3. Çıktı penceresinde, istendiğinde tek kayıt için `i` girin. Çıktı penceresi, sanal cihazınız için yerel olarak oluşturulmuş X.509 sertifikasını görüntüler. 
    
    İlk sertifikayı panoya kopyalayın. Şunun ilk geçtiği yerden başlayın:
    
        -----BEGIN CERTIFICATE----- 
        
    Şunun ilk geçtiği yerde kopyalamayı bitirin:
    
        -----END CERTIFICATE-----
        
    Bu satırların da dahil ettiğinizden emin olun. 

    ![Dice cihazı kayıt uygulaması](./media/python-quick-create-simulated-device-x509/dice-device-enrollment.png)
 
4. Windows makinenizde **_X509testcertificate.pem_** adlı bir dosya oluşturun, dosyayı dilediğiniz düzenleyicide ve panonun içeriğini bu dosyaya kopyalayın. Dosyayı kaydedin. 

5. Azure portalında oturum açın, sol menüdeki **Tüm kaynaklar** düğmesini seçin ve sağlama hizmetinizi açın.

6. Aygıt Sağlama Hizmeti **menüsünden, kayıtları Yönet'i**seçin. **Bireysel Kayıtlar** sekmesini seçin ve üstteki tek tek **kayıt ekle** düğmesini seçin. 

7. Kayıt **Ekle** paneline aşağıdaki bilgileri girin:
   - Kimlik onay *Mekanizması* olarak **X.509**'u seçin.
   - Birincil *sertifika .pem veya .cer dosyası*altında, önceki adımlarda oluşturulan sertifika dosyası **X509testcertificate.pem** seçmek için *bir dosya seçin.*
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
     - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
     - Benzersiz bir cihaz kimliği girin. Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. 
     - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
   - Tamamlandıktan sonra **Kaydet** düğmesine basın. 

     [![Portalda X.509 attestation için bireysel kayıt ekleme](./media/python-quick-create-simulated-device-x509/device-enrollment.png)](./media/python-quick-create-simulated-device-x509/device-enrollment.png#lightbox)

   Kayıt başarıyla tamamlandığında, X.509 cihazınız **Bireysel Kayıtlar** sekmesindeki *Kayıt Kimliği* sütununun altında *riot-device-cert* olarak gösterilir. 

## <a name="simulate-the-device"></a>Cihazı benzetme

1. Cihaz Sağlama Hizmeti menüsünden **Genel Bakış'ı**seçin. Kimlik Kapsamınızı ve _Genel Hizmet Bitiş Noktanızı_not _edin._

    ![Hizmet bilgileri](./media/python-quick-create-simulated-device-x509/extract-dps-endpoints.png)

2. [Python 2.x veya 3.x](https://www.python.org/downloads/) sürümünü indirip yükleyin. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python'u eklediğinizden emin olun. Python 2.x kullanıyorsanız, [Python paket yönetim sistemi *pip*’yi yüklemeniz veya yükseltmeniz](https://pip.pypa.io/en/stable/installing/) gerekebilir.
    
    > [!NOTE] 
    > Windows kullanıyorsanız, ayrıca [Visual Studio 2015 için Visual C++ Yeniden Dağıtılabilir](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)‘i yükleyin. pip paketleri, C DLL’lerini yüklemek/yürütmek için yeniden dağıtılabilir gerektirir.

3. Python paketlerini derlemek için [bu yönergeleri](https://github.com/Azure/azure-iot-sdk-python/blob/v1-deprecated/doc/python-devbox-setup.md) uygulayın.

   > [!NOTE]
   > `pip` kullanıyorsanız `azure-iot-provisioning-device-client` paketini de yüklemeyi unutmayın.

4. Örnekler klasörüne gidin.

    ```cmd/sh
    cd azure-iot-sdk-python/provisioning_device_client/samples
    ```

5. Python IDE'nizi kullanarak **provisioning\_device\_client\_sample.py** adlı Python betiğini düzenleyin. _\_GLOBAL PROV\_URI_ ve ID _\_SCOPE_ değişkenlerini daha önce belirtilen değerlerle değiştirin.

    ```python
    GLOBAL_PROV_URI = "{globalServiceEndpoint}"
    ID_SCOPE = "{idScope}"
    SECURITY_DEVICE_TYPE = ProvisioningSecurityDeviceType.X509
    PROTOCOL = ProvisioningTransportProvider.HTTP
    ```

6. Örnek uygulamayı çalıştırın. 

    ```cmd/sh
    python provisioning_device_client_sample.py
    ```

7. Uygulama bağlantı kurar, cihazı kaydeder ve başarılı kayıt iletisi görüntüler.

    ![başarılı kayıt](./media/python-quick-create-simulated-device-x509/enrollment-success.png)

8. Portalda, sağlama hizmetinize bağlı olan IoT hub'ına gidin ve **Device Explorer** dikey penceresini açın. X.509 sanal cihazının hub'a başarıyla sağlanması durumunda, cihaz kimliği **Device Explorer** dikey penceresinde *DURUM* değeri **etkinleştirildi** olarak gösterilir. Örnek cihaz uygulamasını çalıştırmadan önce bıçağı zaten açtıysanız üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/python-quick-create-simulated-device-x509/registration.png) 

> [!NOTE]
> Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için [Bkz. IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](../iot-hub/iot-hub-devguide-device-twins.md)
>

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aygıt istemcisi örneği üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
2. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından Aygıt Sağlama hizmetinizi seçin. Hizmetiniz için **Kayıtları Yönet'i** açın ve ardından **Bireysel Kayıtlar** sekmesini seçin. Bu hızlı başlangıç ta kayıt olduğunuz aygıtın *KAYIT KİmLİğİ'nin* yanındaki onay kutusunu seçin ve bölmenin üst kısmındaki **Sil** düğmesine basın. 
3. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından IoT hub'ınızı seçin. Hub'ınız için **IoT aygıtları** bıçağını açın, bu hızlı başlatmada kaydettiğiniz aygıtın *AYGıT Kimliğinin* yanındaki onay kutusunu seçin ve ardından bölmenin üst kısmındaki **Sil** düğmesine basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Windows makinenizde simüle edilmiş bir X.509 aygıtı oluşturdunuz ve portaldaki Azure IoT Hub Aygıt Sağlama Hizmetini kullanarak IoT hub'ınıza sokuldunuz. X.509 aygıtınızı programlı olarak nasıl kaydedeceğinizi öğrenmek için, X.509 aygıtlarının programlı kaydı için hızlı bir şekilde başlamaya devam edin. 

> [!div class="nextstepaction"]
> [Azure quickstart - X.509 aygıtlarını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-x509-python.md)
