---
title: Quickstart - Java kullanarak Azure IoT Hub'ına simüle edilmiş bir TPM aygıtı sağlama
description: Quickstart - Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) için Java cihazı SDK'yı kullanarak simüle edilmiş bir TPM aygıtı oluşturun ve sağlayın. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: ce67b5e254a62def5f8b024e960cea7f8780e8b8
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77605483"
---
# <a name="quickstart-create-and-provision-a-simulated-tpm-device-using-java-device-sdk-for-azure-iot-hub-device-provisioning-service"></a>Hızlı başlatma: Azure IoT Hub Aygıt Sağlama Hizmeti için Java cihazı SDK'yı kullanarak simüle edilmiş bir TPM aygıtı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

Bu hızlı başlatmada, Windows bilgisayarında simüle edilmiş bir IoT aygıtı oluşturursunuz. Benzetimli aygıt, Donanım Güvenlik Modülü (HSM) olarak bir TPM simülatörü içerir. Aygıt Sağlama Hizmeti (DPS) ile tek tek bir kayıt kullanarak bu simüle cihazı IoT hub'ınıza bağlamak için aygıt örneği Java kodunu kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Otomatik [sağlama kavramlarının](concepts-auto-provisioning.md)gözden geçirilmesi.
- Azure [portalı ile IoT Hub Aygıt Sağlama Hizmeti Ayarlama'nın](./quick-setup-auto-provision.md)tamamlanması.
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Java SE Geliştirme Kiti 8](https://aka.ms/azure-jdks).
- [Maven,](https://maven.apache.org/install.html)ne kadar.
- [Git.](https://git-scm.com/download/)

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Makinenizde [Java SE Development Kit 8](https://aka.ms/azure-jdks) uygulamasının yüklü olduğundan emin olun.

1. [Maven](https://maven.apache.org/install.html)'ı indirip yükleyin.

1. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

1. Bir komut istemi açın. Cihaz benzetim kod örneği için GitHub deposunu kopyalayın.
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. TPM [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview) simülatörünü simüle edilen aygıtın [HSM'si](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) olacak şekilde çalıştırın. **Erişime izin ver**'e tıklayarak _Windows Güvenlik Duvarı_ ayarlarında değişiklik yapılmasına izin verin. 2321 ve 2322 bağlantı noktalarında bulunan bir yuva üzerinden dinler. Bu pencereyi kapatmayın; Bu hızlı başlatma kılavuzunun sonuna kadar bu simülatörü çalışır durumda tutmak gerekir. 

    ```cmd/sh
    .\azure-iot-sdk-java\provisioning\provisioning-tools\tpm-simulator\Simulator.exe
    ```

    ![TPM Simülatörü](./media/java-quick-create-simulated-device/simulator.png)

1. Ayrı bir komut isteminde, kök klasörüne gidin ve örnek bağımlılıkları derleyin.

    ```cmd/sh
    cd azure-iot-sdk-java
    mvn install -DskipTests=true
    ```

1. Örnek klasörüne gidin.

    ```cmd/sh
    cd provisioning/provisioning-samples/provisioning-tpm-sample
    ```

1. Azure portalında oturum açın, sol menüdeki **Tüm kaynaklar** düğmesini seçin ve Cihaz Sağlama hizmetinizi açın. _Kimlik Kapsamı_ ve _Sağlama Hizmeti Genel Uç Noktası_ değerlerini not edin.

    ![Cihaz Sağlama Hizmeti bilgileri](./media/java-quick-create-simulated-device/extract-dps-endpoints.png)

1. `src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningTpmSample.java` girişini düzenleyerek önceden not ettiğiniz _Kimlik Kapsamı_ ve _Sağlama Hizmeti Genel Uç Noktası_ değerlerini girin.  

    ```java
    private static final String idScope = "[Your ID scope here]";
    private static final String globalEndpoint = "[Your Provisioning Service Global Endpoint here]";
    private static final ProvisioningDeviceClientTransportProtocol PROVISIONING_DEVICE_CLIENT_TRANSPORT_PROTOCOL = ProvisioningDeviceClientTransportProtocol.HTTPS;
    ```
    Dosyayı kaydedin.

1. Projeyi oluşturmak, hedef klasöre gitmek ve oluşturulan .jar dosyasını yürütmek için aşağıdaki komutları kullanın. Yer `version` tutucuyu Java sürümünüzle değiştirin.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-tpm-sample-{version}-with-deps.jar
    ```

1. Program çalışmaya başlar. Bir sonraki bölümün _Onay anahtarını_ ve _Kayıt Kimliği'ni_ not edin ve programı çalışır durumda bırakın.

    ![Java TPM cihazı programı](./media/java-quick-create-simulated-device/program.png)
    

## <a name="create-a-device-enrollment-entry"></a>Cihaz kaydı girişi oluşturma

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

     ![Portal dikey penceresinde cihaz kayıt bilgilerini girme](./media/java-quick-create-simulated-device/enterdevice-enrollment.png)  

   Kayıt başarıyla tamamlanınca cihazınızın *Kayıt Kimliği* listenin altında *Bireysel Kayıtlar* sekmesinde görünür. 


## <a name="simulate-the-device"></a>Cihazı benzetme

1. Makinenizdeki Java örnek kodunu çalıştıran komut penceresinde, uygulamayı çalıştırmaya devam etmek için *Enter* tuşuna basın. IoT hub bilgilerinizi almak için cihaz önyüklemesi ve Cihaz Sağlama Hizmetine bağlanma benzetimi gerçekleştiren iletilere dikkat edin.  

    ![Java TPM cihazı programının son hali](./media/java-quick-create-simulated-device/program-final.png)

1. Simüle edilmiş aygıtınızın sağlama hizmetiyle bağlantılı IoT hub'ına başarılı bir şekilde sağlanması üzerine, aygıt kimliği hub'ın **IoT aygıtlar** bıçak üzerinde görünür.

    ![Cihaz IOT hub'da kayıtlı](./media/java-quick-create-simulated-device/hubregistration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için [Bkz. IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](../iot-hub/iot-hub-devguide-device-twins.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Aygıt istemcisi örneği üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Makinenizde TPM simülatörü penceresini kapatın.
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından Aygıt Sağlama hizmetinizi seçin. Hizmetiniz için **Kayıtları Yönet'i** açın ve ardından **Bireysel Kayıtlar** sekmesini seçin. Bu hızlı başlangıç ta kayıt olduğunuz aygıtın *KAYIT KİmLİğİ'nin* yanındaki onay kutusunu seçin ve bölmenin üst kısmındaki **Sil** düğmesine basın. 
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından IoT hub'ınızı seçin. Hub'ınız için **IoT aygıtları** bıçağını açın, bu hızlı başlatmada kaydettiğiniz aygıtın *AYGıT Kimliğinin* yanındaki onay kutusunu seçin ve ardından bölmenin üst kısmındaki **Sil** düğmesine basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, makinenizde bir TPM simüle edilmiş aygıt oluşturdunuz ve IoT Hub Aygıt Sağlama Hizmeti'ni kullanarak IoT hub'ınıza sokulmuşsunuz. TPM aygıtınızı programlı olarak nasıl kaydedebilirsiniz öğrenmek için, bir TPM aygıtının programlı kaydı için hızlı bir şekilde başlamaya devam edin. 

> [!div class="nextstepaction"]
> [Azure quickstart - TPM aygıtını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-tpm-java.md)
