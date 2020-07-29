---
title: Java kullanarak sanal bir X. 509.952 cihazını Azure IoT Hub sağlama
description: Azure hızlı başlangıç-IoT Hub cihaz sağlama hizmeti (DPS) için Java cihaz SDK 'sını kullanarak sanal bir X. 509.952 cihazı oluşturun ve sağlayın. Bu hızlı başlangıç, bireysel kayıtları kullanır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: java
ms.custom: mvc, devx-track-java
ms.openlocfilehash: f16ace3e54db4d30f89da7335f00153191a7e226
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87326758"
---
# <a name="quickstart-create-and-provision-a-simulated-x509-device-using-java-device-sdk-for-iot-hub-device-provisioning-service"></a>Hızlı başlangıç: IoT Hub cihaz sağlama hizmeti için Java cihaz SDK 'sını kullanarak sanal bir X. 509.952 cihazı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Bu hızlı başlangıçta, bir Windows bilgisayarda sanal bir X. 509.952 aygıtı oluşturacaksınız. Cihaz sağlama hizmeti (DPS) ile tek bir kayıt kullanarak bu sanal cihazı IoT Hub 'ınıza bağlamak için cihaz örneği Java kodunu kullanırsınız.

## <a name="prerequisites"></a>Önkoşullar

- [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md)gözden geçirme.
- [Azure Portal Ile cihaz sağlama hizmeti IoT Hub ayarlama](./quick-setup-auto-provision.md)işlemi tamamlandı.
- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Java SE Development Kit 8](https://aka.ms/azure-jdks).
- [Maven](https://maven.apache.org/install.html).
- [Git](https://git-scm.com/download/).

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Makinenizde [Java SE Development Kit 8](https://aka.ms/azure-jdks) uygulamasının yüklü olduğundan emin olun.

2. [Maven](https://maven.apache.org/install.html)'ı indirip yükleyin.

3. Makinenizde Git’in yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

4. Bir komut istemi açın. Cihaz benzetim kod örneği için GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```
5. Kök `azure-iot-sdk-`java` dizinine gidin ve gereken tüm paketleri indirmek için projeyi oluşturun.
   
   ```cmd/sh
   cd azure-iot-sdk-java
   mvn install -DskipTests=true
   ```
6. Sertifika oluşturucu projesine gidin ve projeyi derleyin. 

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator
    mvn clean install
    ```

## <a name="create-a-self-signed-x509-device-certificate-and-individual-enrollment-entry"></a>Otomatik olarak imzalanan X.509 cihazı sertifikası ve bireysel kayıt girişi oluşturma

Bu bölümde, otomatik olarak imzalanan X.509 sertifikası kullanacaksınız. Aşağıdaki konuları göz önünde bulundurmak önemlidir:

* Otomatik olarak imzalanan sertifikalar yalnızca test amaçlıdır ve üretimde kullanılmamalıdır.
* Otomatik olarak imzalanan sertifikanın varsayılan sona erme tarihi bir yıldır.

Simülasyon cihazının tek kayıt girdisiyle kullanılacak sertifikayı oluşturmak için [Java için Azure IoT SDK'sından](https://github.com/Azure/azure-iot-sdk-java.git) örnek kodu kullanacaksınız.

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:

- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Bireysel](concepts-service.md#individual-enrollment)kayıtlar: tek bir cihazı kaydetmek için kullanılır.

Bu makalede, bireysel kayıtlar gösterilmektedir.

1. Önceki adımlardan komut istemi ' ni kullanarak, `target` klasöre gidin ve ardından önceki adımda oluşturulan. jar dosyasını yürütün.

    ```cmd/sh
    cd target
    java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
    ```

2. _Do you want to input common name_ (Ortak ad girmek istiyor musunuz) istemi için **N** girin. `Client Cert` çıktısının *-----BEGIN CERTIFICATE-----* satırından *-----END CERTIFICATE-----* satırına kadar olan bölümü panoya kopyalayın.

   ![Bireysel sertifika oluşturucu](./media/java-quick-create-simulated-device-x509/individual.png)

3. Windows makinenizde **_X509individual.pem_** adlı bir dosya oluşturun, dosyayı dilediğiniz düzenleyicide ve panonun içeriğini bu dosyaya kopyalayın. Dosyayı kaydedin ve düzenleyicinizi kapatın.

4. Komut isteminde, kimlik _doğrulama kodunu_ girmek ve program çıktısını hızlı başlangıçta başvuru için açık tutmak Istiyorsanız, **N** için yazın. Daha sonra, sonraki bölümde kullanmak için `Client Cert` ve `Client Cert Private Key` değerlerini kopyalayın.

5. [Azure Portal](https://portal.azure.com)oturum açın, sol taraftaki menüden **tüm kaynaklar** düğmesini seçin ve cihaz sağlama hizmeti örneğinizi açın.

6. Cihaz sağlama hizmeti menüsünden kayıtları **Yönet**' i seçin. **Bireysel** kayıtlar sekmesini seçin ve üst kısımdaki **tek kayıt Ekle** düğmesini seçin. 

7. **Kayıt Ekle** panelinde, aşağıdaki bilgileri girin:
   - Kimlik onay *Mekanizması* olarak **X.509**'u seçin.
   - *Birincil sertifika. pek veya. cer dosyası*' nın altında, önceki adımlarda oluşturulmuş **X509individual. ped** sertifika dosyasını seçmek için *Dosya Seç* ' i seçin.  
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
     - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
     - Benzersiz bir cihaz kimliği girin. Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. 
     - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
     - Tamamlandıktan sonra **Kaydet** düğmesine basın. 

     [![Portalda X. 509.440 kanıtlama için bireysel kayıt ekleme](./media/java-quick-create-simulated-device-x509/device-enrollment.png)](./media/how-to-manage-enrollments/individual-enrollment.png#lightbox)

     Kayıt başarıyla tamamlandığında, X.509 cihazınız *Bireysel Kayıtlar* sekmesindeki *Kayıt Kimliği* sütununun altında **microsoftriotcore** olarak gösterilir. 



## <a name="simulate-the-device"></a>Cihazı benzetme

1. Cihaz sağlama hizmeti menüsünden **genel bakış** ' ı seçin ve _kimlik kapsamınızı_ ve _sağlama hizmeti genel uç noktasını_belirtin.

    ![Hizmet bilgileri](./media/java-quick-create-simulated-device-x509/extract-dps-endpoints.png)

2. Bir komut istemi açın. Java SDK deposunun örnek proje klasörüne gidin.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

3. Kodunuzdaki sağlama hizmetini ve X.509 kimlik bilgilerini girin. Bu otomatik sağlama sırasında, cihaz kaydından önce simülasyon cihazını kanıtlamak için kullanılır:

   - `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java`Daha önce belirtildiği gibi, _kimlik kapsamınızı_ ve _sağlama hizmeti genel uç noktasını_ dahil etmek için dosyayı düzenleyin. Ayrıca önceki bölümde not ettiğiniz _Client Cert_ ve _Client Cert Private Key_ değerlerini de dahil edin.

      ```java
      private static final String idScope = "[Your ID scope here]";
      private static final String globalEndpoint = "[Your Provisioning Service Global Endpoint here]";
      private static final ProvisioningDeviceClientTransportProtocol PROVISIONING_DEVICE_CLIENT_TRANSPORT_PROTOCOL = ProvisioningDeviceClientTransportProtocol.HTTPS;
      private static final String leafPublicPem = "<Your Public PEM Certificate here>";
      private static final String leafPrivateKey = "<Your Private PEM Key here>";
      ```

   - Sertifikanız ve özel anahtarınızı kopyalarken/yapıştırırken aşağıdaki biçimi kullanın:
        
      ```java
      private static final String leafPublicPem = "-----BEGIN CERTIFICATE-----\n" +
        "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
        "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
        "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
        "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
        "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
        "-----END CERTIFICATE-----\n";
      private static final String leafPrivateKey = "-----BEGIN PRIVATE KEY-----\n" +
            "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
            "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
            "XXXXXXXXXX\n" +
            "-----END PRIVATE KEY-----\n";
      ```

4. Örneği derleyin. `target`Klasöre gidin ve oluşturulan. jar dosyasını yürütün.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

5. Azure portalda, sağlama hizmetinize bağlı olan IoT hub'ına gidin ve **Device Explorer** dikey penceresini açın. X.509 sanal cihazının hub'a başarıyla sağlanması durumunda, cihaz kimliği **Device Explorer** dikey penceresinde *DURUM* değeri **etkinleştirildi** olarak gösterilir.  Örnek cihaz uygulamasını çalıştırmadan önce dikey pencereyi zaten açtıysanız, en üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/java-quick-create-simulated-device-x509/hubregistration.png) 

> [!NOTE]
> Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub cihaz TWINS 'ı anlama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).
>


## <a name="clean-up-resources"></a>Kaynakları temizleme

Cihaz istemci örneğini üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
2. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından cihaz sağlama hizmetinizi seçin. Hizmetinizin kayıtları **yönetme** dikey penceresini açın ve sonra **bireysel** kayıtlar sekmesini seçin. bu hızlı BAŞLANGıÇTA kaydettiğiniz cihazın *kayıt kimliği* ' nin yanındaki onay kutusunu işaretleyin ve bölmenin en üstündeki **Sil** düğmesine basın. 
3. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından IoT Hub 'ınızı seçin. Hub 'ınız için **IoT cihazları** dikey penceresini açın, bu hızlı başlangıçta kaydettiğiniz CIHAZıN *cihaz kimliği* ' nin yanındaki onay kutusunu işaretleyin ve ardından bölmenin en üstündeki **Sil** düğmesine basın.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Windows makinenizde sanal bir X. 509.952 cihazı oluşturdunuz. Azure IoT Hub Cihazı Sağlama Hizmetinize kaydını yapılandırdınız, ardından cihazı IoT hub’ınıza otomatik olarak sağladınız. X. 509.952 cihazınızı programlı bir şekilde kaydetmeyi öğrenmek için, X. 509.440 cihazlarının programlı kaydı için hızlı başlangıç 'a geçin. 

> [!div class="nextstepaction"]
> [Azure hızlı başlangıç-X. 509.440 cihazlarını Azure IoT Hub cihaz sağlama hizmeti 'ne kaydetme](quick-enroll-device-x509-java.md)
