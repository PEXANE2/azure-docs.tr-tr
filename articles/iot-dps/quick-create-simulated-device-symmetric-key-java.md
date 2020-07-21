---
title: Hızlı başlangıç-Java kullanarak Azure IoT Hub sanal cihaz sağlamak için simetrik anahtar kullanın
description: Bu hızlı başlangıçta, Azure IoT Hub cihaz sağlama hizmeti (DPS) ile simetrik anahtar kullanan bir sanal cihaz oluşturmak için Java cihaz SDK 'sını kullanacaksınız
author: wesmc7777
ms.author: wesmc
ms.date: 01/30/2020
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: eliotgra
ms.custom: mvc
ms.openlocfilehash: 8b54f216850b77473ea8c272311e3f135f256518
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536513"
---
# <a name="quickstart-provision-a-simulated-device-to-iot-hub-with-symmetric-keys"></a>Hızlı başlangıç: simetrik anahtarlarla IoT Hub için sanal cihaz sağlama

Bu hızlı başlangıçta, Windows geliştirme makinesi üzerinde bir cihaz simülatörü oluşturmayı ve çalıştırmayı öğreneceksiniz. Bu sanal cihazı, bir cihaz sağlama hizmeti (DPS) örneğiyle kimlik doğrulamak için bir simetrik anahtar kullanmak ve bir IoT Hub 'ına atanmak üzere yapılandıracaksınız. [Java için Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-java) 'larının örnek kodu, sağlamayı Başlatan cihaz için bir önyükleme sırasının benzetimini yapmak üzere kullanılacaktır. Cihaz, bir DPS hizmet örneğiyle bireysel bir kayda göre tanınacaktır ve bir IoT Hub 'ına atanır.

Bu makalede tek bir kayıt ile sağlama işlemi gösterilmektedir, ancak kayıt gruplarını kullanabilirsiniz. Kayıt grupları kullanılırken bazı farklılıklar vardır. Örneğin, cihaz için benzersiz bir kayıt KIMLIĞIYLE türetilmiş bir cihaz anahtarı kullanmanız gerekir. Simetrik anahtar kayıt grupları eski cihazlarla sınırlı olmasa da, [Simetrik anahtar kanıtı kullanarak eski cihazları sağlama](how-to-legacy-device-symm-key.md) bölümünde bir kayıt grubu örneği sağlanmaktadır. Daha fazla bilgi için, bkz. [Simetrik Anahtar Kanıtlama için Grup Kayıtları](concepts-symmetric-key-attestation.md#group-enrollments).

Otomatik sağlama işlemini bilmiyorsanız, [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md)gözden geçirin. 

Ayrıca, bu hızlı başlangıçla devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. Bu hızlı başlangıç, Cihaz Sağlama Hizmeti örneğinizi zaten oluşturmuş olmanızı gerektirir.

Bu makale Windows tabanlı bir iş istasyonuna yöneliktir. Ancak yordamları Linux üzerinde gerçekleştirebilirsiniz. Bir Linux örneği için, bkz. [Çoklu kiracı için sağlama](how-to-provision-multitenant.md).


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

* Makinenizde [Java SE Development Kit 8](https://aka.ms/azure-jdks) veya sonraki bir sürümünün yüklü olduğundan emin olun.

* [Maven](https://maven.apache.org/install.html)'ı indirip yükleyin.

* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

<a id="setupdevbox"></a>

## <a name="prepare-the-java-sdk-environment"></a>Java SDK ortamını hazırlama 

1. Makinenizde Git’in yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

2. Bir komut istemi açın. Cihaz benzetim kod örneği için GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```
3. Kök `azure-iot-sdk-java` dizine gidin ve gereken tüm paketleri indirmek için projeyi derleyin.
   
   ```cmd/sh
   cd azure-iot-sdk-java
   mvn install -DskipTests=true
   ```

## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma

1. [Azure Portal](https://portal.azure.com)oturum açın, sol taraftaki menüden **tüm kaynaklar** düğmesini seçin ve CIHAZ sağlama hizmeti (DPS) örneğinizi açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından üst kısımdaki **tek kayıt Ekle** düğmesini seçin. 

3. **Kayıt Ekle** panelinde, aşağıdaki bilgileri girin ve **Kaydet** düğmesine basın.

   - **Mekanizma**: Kimlik onay *Mekanizması* olarak **Simetrik Anahtar** seçeneğini belirleyin.

   - **Anahtarları otomatik oluştur**: Bu kutuyu işaretleyin.

   - **Kayıt Kimliği**: Kaydı tanımlamak için bir kayıt kimliği girin. Yalnızca küçük alfasayısal karakterler ile kısa çizgi ('-') karakterlerini kullanın. Örneğin, **SYMM-anahtar-Java-Device-007**.

   - **IoT Hub Cihaz Kimliği:** Bir cihaz tanımlayıcısı girin. Örneğin, **Java-Device-007**.

     ![Portalda simetrik anahtar kanıtı için bireysel kayıt ekleme](./media/quick-create-simulated-device-symm-key-java/create-individual-enrollment-java.png)

4. Kaydınız kaydedildikten sonra, **birincil anahtar** ve **İkincil anahtar** oluşturulur ve kayıt girişine eklenir. Simetrik anahtar cihaz kaydınız, *bireysel* kayıtlar SEKMESINDEKI *kayıt kimliği* sütununun altında **syaa-Key-Java-Device-007** olarak görünür. 

    Kaydı açın ve oluşturduğunuz **Birincil Anahtar** değerini kopyalayın. Bu anahtar değerini ve **kayıt kimliğini** daha sonra cihazın Java kodunu güncelleştirdiğinizde kullanacaksınız.



<a id="firstbootsequence"></a>

## <a name="simulate-device-boot-sequence"></a>Cihaz önyükleme sırasının benzetimini yap

Bu bölümde, cihazın önyükleme sırasını DPS örneğinize göndermek için cihaz örnek kodunu güncelleşolursunuz. Bu önyükleme sırası, cihazın tanınmasına, kimliğinin doğrulanmasını ve DPS örneğine bağlı bir IoT Hub 'ına atanmasını sağlar.

1. Cihaz sağlama hizmeti menüsünden **genel bakış** ' ı seçin ve _kimlik kapsamınızı_ ve _sağlama hizmeti genel uç noktasını_belirtin.

    ![Hizmet bilgileri](./media/java-quick-create-simulated-device-x509/extract-dps-endpoints.png)

2. Düzenlenmek üzere Java cihaz örnek kodunu açın. Cihaz örnek kodunun tam yolu:

    `azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-symmetrickey-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningSymmetricKeySampleSample.java`

   - DPS örneğinizin _kimlik kapsamını_ ve _sağlama hizmeti genel uç noktasını_ ekleyin. Ayrıca, bireysel kaydınız için seçtiğiniz birincil simetrik anahtarı ve kayıt KIMLIĞINI de içerir. Yaptığınız değişiklikleri kaydedin. 

      ```java
        private static final String SCOPE_ID = "[Your scope ID here]";
        private static final String GLOBAL_ENDPOINT = "[Your Provisioning Service Global Endpoint here]";
        private static final String SYMMETRIC_KEY = "[Enter your Symmetric Key here]";
        private static final String REGISTRATION_ID = "[Enter your Registration ID here]";
      ```

3. Oluşturmak için bir komut istemi açın. Java SDK deposunun sağlama örnek projesi klasörüne gidin.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-symmetrickey-sample
    ```

4. Örnek oluşturun ve ardından `target` oluşturulan. jar dosyasını yürütmek için klasöre gidin.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-symmetrickey-sample-{version}-with-deps.jar
    ```

5. Beklenen çıkış şuna benzer olmalıdır:

    ```cmd/sh
      Starting...
      Beginning setup.
      Waiting for Provisioning Service to register
      IotHUb Uri : <Your DPS Service Name>.azure-devices.net
      Device ID : java-device-007
      Sending message from device to IoT Hub...
      Press any key to exit...
      Message received! Response status: OK_EMPTY
    ```

6. Azure portalda, sağlama hizmetinize bağlı olan IoT hub'ına gidin ve **Device Explorer** dikey penceresini açın. Sanal bir simetrik anahtar cihazı hub 'a başarıyla sağlamaktan sonra, cihaz KIMLIĞI **Device Explorer** dikey penceresinde *durumu* **etkin**olarak görüntülenir.  Örnek cihaz uygulamasını çalıştırmadan önce dikey pencereyi zaten açtıysanız, en üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device-symm-key-java/hubregistration-java.png) 

> [!NOTE]
> Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub cihaz TWINS 'ı anlama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).
>


## <a name="clean-up-resources"></a>Kaynakları temizleme

Cihaz istemci örneğini üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından cihaz sağlama hizmetinizi seçin. Hizmetiniz için kayıtları **Yönet** ' i açın ve sonra **bireysel** kayıtlar sekmesini seçin. bu hızlı BAŞLANGıÇTA kaydettiğiniz cihazın *kayıt kimliği* ' nin yanındaki onay kutusunu işaretleyin ve bölmenin en üstündeki **Sil** düğmesine basın. 
1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından IoT Hub 'ınızı seçin. , Hub 'ınız için **IoT cihazlarını** açın, bu hızlı başlangıçta kaydettiğiniz CIHAZıN *cihaz kimliği* ' nin yanındaki onay kutusunu işaretleyin ve ardından bölmenin en üstündeki **Sil** düğmesine basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Windows makinenizde bir sanal cihaz oluşturdunuz ve portaldaki Azure IoT Hub cihaz sağlama hizmeti ile simetrik anahtar kullanarak IoT Hub 'ınıza sağladınız. Cihazınızı programlı bir şekilde kaydetmeyi öğrenmek için, X. 509.440 cihazlarının programlı kaydı için hızlı başlangıç 'a geçin. 

> [!div class="nextstepaction"]
> [Azure hızlı başlangıç-X. 509.440 cihazlarını Azure IoT Hub cihaz sağlama hizmeti 'ne kaydetme](quick-enroll-device-x509-java.md)
