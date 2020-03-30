---
title: Quickstart - Java kullanarak simüle edilmiş cihazı Azure IoT Hub'ına sağlamak için simetrik anahtar kullanın
description: Bu hızlı başlatmada, Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) ile simetrik anahtar kullanan simüle edilmiş bir aygıt oluşturmak için Java cihazı SDK'yı kullanacaksınız.
author: wesmc7777
ms.author: wesmc
ms.date: 01/30/2020
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: eliotgra
ms.custom: mvc
ms.openlocfilehash: aaa1a4423363255536db7d53a1f8f8fa9ba686ff
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76941405"
---
# <a name="quickstart-provision-a-simulated-device-with-symmetric-keys"></a>Hızlı Başlangıç: Simetrik anahtarlar ile bir simülasyon cihazı sağlama

Bu hızlı başlangıçta, Windows geliştirme makinesi üzerinde bir cihaz simülatörü oluşturmayı ve çalıştırmayı öğreneceksiniz. Bu simüle edilmiş aygıtı, aygıt sağlama hizmeti (DPS) örneğinde kimlik doğrulaması yapmak için simetrik bir anahtar kullanacak ve bir IoT hub'ına atanacak şekilde yapılandıracaksınız. Java için [Microsoft Azure IoT SDK'larından](https://github.com/Azure/azure-iot-sdk-java) örnek kod, sağlama başlatan aygıt için önyükleme sırasını simüle etmek için kullanılır. Aygıt, dps hizmet örneğine sahip tek bir kayda göre tanınacak ve bir IoT hub'ına atanacaktır.

Bu makalede, tek bir kayıtla birlikte sağlama gösterilmiş olsa da, kayıt gruplarını kullanabilirsiniz. Kayıt gruplarını kullanırken bazı farklılıklar vardır. Örneğin, aygıt için benzersiz bir kayıt kimliğine sahip türemiş bir aygıt anahtarı kullanmanız gerekir. Simetrik anahtar kayıt grupları eski cihazlarla sınırlı olmasa da, [Simetrik anahtar kanıtı kullanarak eski cihazları sağlama](how-to-legacy-device-symm-key.md) bölümünde bir kayıt grubu örneği sağlanmaktadır. Daha fazla bilgi için, bkz. [Simetrik Anahtar Kanıtlama için Grup Kayıtları](concepts-symmetric-key-attestation.md#group-enrollments).

Otomatik sağlama işlemine aşina değilseniz, Otomatik sağlama kavramlarını gözden [geçirin.](concepts-auto-provisioning.md) 

Ayrıca, bu hızlı başlangıçla devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. Bu hızlı başlangıç, Cihaz Sağlama Hizmeti örneğinizi zaten oluşturmuş olmanızı gerektirir.

Bu makale Windows tabanlı bir iş istasyonuna yöneliktir. Ancak yordamları Linux üzerinde gerçekleştirebilirsiniz. Bir Linux örneği için, bkz. [Çoklu kiracı için sağlama](how-to-provision-multitenant.md).


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar

* [Java SE Geliştirme Kiti 8](https://aka.ms/azure-jdks) veya daha sonra makinenizde yüklü olduğundan emin olun.

* [Maven](https://maven.apache.org/install.html)'ı indirip yükleyin.

* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

<a id="setupdevbox"></a>

## <a name="prepare-the-java-sdk-environment"></a>Java SDK ortamını hazırlayın 

1. Makinenizde Git’in yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

2. Bir komut istemi açın. Cihaz benzetim kod örneği için GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```
3. Kök `azure-iot-sdk-java` dizinine gidin ve gerekli tüm paketleri indirmek için projeyi oluşturun.
   
   ```cmd/sh
   cd azure-iot-sdk-java
   mvn install -DskipTests=true
   ```

## <a name="create-a-device-enrollment"></a>Aygıt kaydı oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın, sol menüdeki **Tüm kaynaklar** düğmesini seçin ve Cihaz Sağlama hizmeti (DPS) örneğini açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından üstteki **tek tek kayıt ekle** düğmesini seçin. 

3. Kayıt **Ekle** paneline aşağıdaki bilgileri girin ve **Kaydet** düğmesine basın.

   - **Mekanizma**: Kimlik onay *Mekanizması* olarak **Simetrik Anahtar** seçeneğini belirleyin.

   - **Otomatik oluşturma tuşları**: Bu kutuyu işaretleyin.

   - **Kayıt Kimliği**: Kaydı tanımlamak için bir kayıt kimliği girin. Yalnızca küçük alfasayısal karakterler ile kısa çizgi ('-') karakterlerini kullanın. Örneğin, **symm-key-java-device-007**.

   - **IoT Hub Cihaz Kimliği:** Bir cihaz tanımlayıcısı girin. Örneğin, **java-device-007**.

     ![Portalda simetrik anahtar kanıtı için bireysel kayıt ekleme](./media/quick-create-simulated-device-symm-key-java/create-individual-enrollment-java.png)

4. Kaydınızı kaydettikten **sonra, Birincil Anahtar** ve **İkincil Anahtar** oluşturulur ve kayıt girişine eklenir. Simetrik anahtar aygıtı kaydınız, *Bireysel Kayıtlar* sekmesindeki *Kayıt Kimliği* sütunu altında **symm-key-java-device-007** olarak görünür. 

    Kaydı açın ve oluşturduğunuz **Birincil Anahtar** değerini kopyalayın. Aygıtın Java kodunu güncellediğinizde bu anahtar değerini ve **Kayıt Kimliği'ni** daha sonra kullanırsınız.



<a id="firstbootsequence"></a>

## <a name="simulate-device-boot-sequence"></a>Aygıt önyükleme sırasını simüle edin

Bu bölümde, aygıtın önyükleme sırasını DPS örneğinize göndermek için aygıt örnek kodunu güncelleştireceksiniz. Bu önyükleme sırası aygıtın tanınmasına, doğrulanmasına ve DPS örneğine bağlı bir IoT hub'ına atanmasına neden olur.

1. Cihaz Sağlama Hizmeti menüsünden **Genel Bakış'ı** seçin ve _Kimlik Kapsamınızı_ ve _Sağlama Hizmetigenel Bitiş Noktanıza_dikkat edin.

    ![Hizmet bilgileri](./media/java-quick-create-simulated-device-x509/extract-dps-endpoints.png)

2. Düzenleme için Java aygıt örnek kodunu açın. Aygıt örnek koduna tam yol:

    `azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-symmetrickey-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningSymmetricKeySampleSample.java`

   - DPS örneğinizin _Kimlik Kapsamı_ ve Sağlama Hizmeti Genel _Bitiş Noktasını_ ekleyin. Ayrıca birincil simetrik anahtarı ve bireysel kaydınız için seçtiğiniz kayıt kimliğini de ekleyin. Yaptığınız değişiklikleri kaydedin. 

      ```java
        private static final String SCOPE_ID = "[Your scope ID here]";
        private static final String GLOBAL_ENDPOINT = "[Your Provisioning Service Global Endpoint here]";
        private static final String SYMMETRIC_KEY = "[Enter your Symmetric Key here]";
        private static final String REGISTRATION_ID = "[Enter your Registration ID here]";
      ```

3. Oluşturmak için bir komut istemi açın. Java SDK deposunun sağlama örnek proje klasörüne gidin.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-symmetrickey-sample
    ```

4. Örneği oluşturun ve oluşturulan `target` .jar dosyasını yürütmek için klasöre gidin.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-symmetrickey-sample-{version}-with-deps.jar
    ```

5. Beklenen çıktı aşağıdakilere benzer olmalıdır:

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

6. Azure portalda, sağlama hizmetinize bağlı olan IoT hub'ına gidin ve **Device Explorer** dikey penceresini açın. Simüle edilmiş simetrik anahtar aygıtı hub'a başarıyla sağladıktan sonra, aygıt kimliği **Aygıt Gezgini** bıçak üzerinde görünür ve *STATUS* **etkindir.**  Örnek cihaz uygulamasını çalıştırmadan önce bıçağı zaten açtıysanız üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device-symm-key-java/hubregistration-java.png) 

> [!NOTE]
> Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için [Bkz. IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](../iot-hub/iot-hub-devguide-device-twins.md)
>


## <a name="clean-up-resources"></a>Kaynakları temizleme

Aygıt istemcisi örneği üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından Aygıt Sağlama hizmetinizi seçin. Hizmetiniz için **Kayıtları Yönet'i** açın ve ardından **Bireysel Kayıtlar** sekmesini seçin. Bu hızlı başlangıç ta kaydolduğunuz aygıtın *KAYIT KİmLİğİ'nin* yanındaki onay kutusunu seçin ve bölmenin üst kısmındaki **Sil** düğmesine basın. 
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından IoT hub'ınızı seçin. Hub'ınız için **IoT aygıtlarını** açın, bu hızlı başlatmada kaydettiğiniz aygıtın *AYGıT Kimliğinin* yanındaki onay kutusunu seçin ve ardından bölmenin üst kısmındaki **Sil** düğmesine basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Windows makinenizde simüle edilmiş bir aygıt oluşturdunuz ve portaldaki Azure IoT Hub Aygıt Sağlama Hizmeti ile Simetrik tuşu kullanarak IoT hub'ınıza sokuldunuz. Cihazınızı programlı olarak nasıl kaydedeceğinizi öğrenmek için X.509 aygıtlarının programlı kaydı için hızlı bir şekilde çalışmaya devam edin. 

> [!div class="nextstepaction"]
> [Azure quickstart - X.509 aygıtlarını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-x509-java.md)
