---
title: X.509 aygıtını Node.js kullanarak Azure IoT Hub'ına sağlama
description: Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) için Node.js cihazı SDK'yı kullanarak simüle edilmiş bir X.509 aygıtı oluşturun ve sağlayın. Bu hızlı başlangıç, tek tek kayıtları kullanır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: nodejs
ms.custom: mvc
ms.openlocfilehash: 746f4adbf616f95c21874d7c1c48881f88c38d34
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77605411"
---
# <a name="quickstart-create-and-provision-an-x509-simulated-device-using-nodejs-device-sdk-for-iot-hub-device-provisioning-service"></a>Quickstart: IoT Hub Aygıt Sağlama Hizmeti için Node.js cihazı SDK kullanarak bir X.509 simüle cihazı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Bu hızlı başlatmada, Windows bilgisayarında simüle edilmiş bir X.509 aygıtı oluşturursunuz. Aygıt Sağlama Hizmeti (DPS) ile tek tek bir kayıt kullanarak bu simüle cihazı IoT hub'ınıza bağlamak için aygıt örneği Node.js kodunu kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Otomatik [sağlama kavramlarının](concepts-auto-provisioning.md)gözden geçirilmesi.
- Azure [portalı ile IoT Hub Aygıt Sağlama Hizmeti Ayarlama'nın](./quick-setup-auto-provision.md)tamamlanması.
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Düğüm.js v4.0+](https://nodejs.org).
- [Git.](https://git-scm.com/download/)
- [OpenSSL](https://www.openssl.org/).

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamlayın.

2. Makinenizde [Node.js v4.0 veya üzeri](https://nodejs.org) bir sürümün yüklü olduğundan emin olun.

3. [Git'in](https://git-scm.com/download/) makinenize yüklü olduğundan ve komut penceresine erişilebilen ortam değişkenlerine eklendiğinden emin olun. 

4. Makinenizde [OpenSSL](https://www.openssl.org/)’nin yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Bu kitaplık kaynaktan derlenip yüklenebilir veya [bunun](https://sourceforge.net/projects/openssl/) gibi bir [üçüncü taraftan](https://wiki.openssl.org/index.php/Binaries) indirilip yüklenebilir. 

    > [!NOTE]
    > _Kök_, _ara_ ve/veya _yaprak_ X.509 sertifikalarınızı zaten oluşturduysanız, bu adımı ve sertifika oluşturma ile ilgili aşağıdaki tüm adımları atlayabilirsiniz.
    >

## <a name="create-a-self-signed-x509-device-certificate-and-individual-enrollment-entry"></a>Otomatik olarak imzalanan X.509 cihazı sertifikası ve bireysel kayıt girişi oluşturma

Bu bölümde, otomatik olarak imzalanan X.509 sertifikası kullanacaksınız. Aşağıdaki konuları göz önünde bulundurmak önemlidir:

* Otomatik olarak imzalanan sertifikalar yalnızca test amaçlıdır ve üretimde kullanılmamalıdır.
* Otomatik olarak imzalanan sertifikanın varsayılan sona erme tarihi bir yıldır.

Simülasyon cihazının tek kayıt girdisiyle kullanılacak sertifikayı oluşturmak için [Node.js için Azure IoT SDK'sından](https://github.com/Azure/azure-iot-sdk-node.git) örnek kodu kullanacaksınız.

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:

- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Tek tek kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede, tek tek kayıtları gösterir.

1. Bir komut istemi açın. Kod örnekleri için GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-node.git --recursive
    ```

2. Sertifika oluşturucu betiğine gidin ve projeyi derleyin. 

    ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    npm install
    ```

3. Kendi _sertifikanızın adını_ kullanıp betiği çalıştırarak _yaprak_ X.509 sertifikasını oluşturun. Yaprak sertifikanın ortak adı [Kayıt Kimliği](https://docs.microsoft.com/azure/iot-dps/concepts-device#registration-id) olduğundan, yalnızca küçük harf alfasayısal karakterler ve kısa çizgiler kullandığınızdan emin olun.

    ```cmd/sh
    node create_test_cert.js device {certificate-name}
    ```

4. [Azure portalında](https://portal.azure.com)oturum açın, sol menüdeki **Tüm kaynaklar** düğmesini seçin ve Cihaz Sağlama Hizmeti örneğini açın.

5. Aygıt Sağlama Hizmeti **menüsünden, kayıtları Yönet'i**seçin. **Bireysel Kayıtlar** sekmesini seçin ve üstteki tek tek **kayıt ekle** düğmesini seçin. 

6. Kayıt **Ekle** paneline aşağıdaki bilgileri girin:
   - Kimlik onay *Mekanizması* olarak **X.509**'u seçin.
   - Birincil *sertifika .pem veya .cer dosyasıaltında,* önceki adımlarda oluşturulan sertifika **dosyasını {sertifika adı}_cert.pem'i** seçmek için *bir dosya* seçin' i seçin.  
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
     - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
     - Benzersiz bir cihaz kimliği girin. Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. 
     - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
     - Tamamlandıktan sonra **Kaydet** düğmesine basın. 

     [![Portalda X.509 attestation için bireysel kayıt ekleme](./media/quick-create-simulated-device-x509-node/device-enrollment.png)](./media/quick-create-simulated-device-x509-node/device-enrollment.png#lightbox)

     Başarılı kayıtta, X.509 aygıtınız *Bireysel Kayıtlar* sekmesindeki *Kayıt Kimliği* sütunu altında **{sertifika adı}** olarak görünür.

## <a name="simulate-the-device"></a>Cihazı benzetme

[Azure IOT Hub Node.js Cihaz SDK'sı](https://github.com/Azure/azure-iot-sdk-node), bir cihazın benzetimini yapmak için kolay bir yol sağlar. Daha fazla okumak için [Aygıt kavramlarına](https://docs.microsoft.com/azure/iot-dps/concepts-device)bakın.

1. Azure portalında, Aygıt Sağlama hizmetiniz için **Genel Bakış** bıçağını seçin ve **_GLobal Aygıt Bitiş Noktası_** ve Kimlik **_Kapsamı_** değerlerini not edin.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509-node/extract-dps-endpoints.png) 

2. _Sertifikanızı_ ve _anahtarınızı_ örnek klasöre kopyalayın.

    ```cmd/sh
    copy .\{certificate-name}_cert.pem ..\device\samples\{certificate-name}_cert.pem
    copy .\{certificate-name}_key.pem ..\device\samples\{certificate-name}_key.pem
    ```

3. Cihaz test betiğine gidin ve projeyi derleyin. 

    ```cmd/sh
    cd ..\device\samples
    npm install
    ```

4. **register\_x509.js** dosyasını düzenleyin. Aşağıdaki değişiklikleri yaptıktan sonra dosyayı kaydedin.
    - `provisioning host` öğesini yukarıdaki **1. Adım**’da belirtilen **_Genel Cihaz Uç Noktası_** ile değiştirin.
    - Yukarıdaki `id scope` **Adım 1'de** belirtilen **_Kimlik Kapsamı_** ile değiştirin. 
    - Önceki `registration id` bölümde belirtilen **_Kayıt Kimliği_** ile değiştirin.
    - `cert filename` ve `key filename` öğelerini yukarıdaki **2. Adım**’da kopyaladığınız dosyalarla değiştirin. 

5. Betiği yürütün ve cihazın başarıyla sağlandığını doğrulayın.

    ```cmd/sh
    node register_x509.js
    ```   

6. Portalda, sağlama hizmetinize bağlı IoT hub'ına gidin ve **IoT aygıtları** bıçaklarını açın. Simüle edilen X.509 aygıtının hub'a başarılı bir şekilde sağlanması üzerine, aygıt kimliği **IoT aygıtları** bıçağında görünür ve *STATUS* **etkindir.** Örnek cihaz uygulamasını çalıştırmadan önce bıçağı zaten açtıysanız üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device-x509-node/hubregistration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için [Bkz. IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](../iot-hub/iot-hub-devguide-device-twins.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Aygıt istemcisi örneği üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
2. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından Aygıt Sağlama hizmetinizi seçin. Hizmetiniz için **Kayıtları Yönet'i** açın ve ardından **Bireysel Kayıtlar** sekmesini seçin. Bu hızlı başlangıç ta kayıt olduğunuz aygıtın *KAYIT KİmLİğİ'nin* yanındaki onay kutusunu seçin ve bölmenin üst kısmındaki **Sil** düğmesine basın. 
3. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından IoT hub'ınızı seçin. Hub'ınız için **IoT aygıtları** bıçağını açın, bu hızlı başlatmada kaydettiğiniz aygıtın *AYGıT Kimliğinin* yanındaki onay kutusunu seçin ve ardından bölmenin üst kısmındaki **Sil** düğmesine basın.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, simüle edilmiş bir X.509 aygıtı oluşturdunuz ve portaldaki Azure IoT Hub Aygıt Sağlama Hizmetini kullanarak ioT hub'ınıza sokuldum. X.509 aygıtınızı programlı olarak nasıl kaydedeceğinizi öğrenmek için, X.509 aygıtlarının programlı kaydı için hızlı bir şekilde başlamaya devam edin. 

> [!div class="nextstepaction"]
> [Azure quickstart - X.509 aygıtlarını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-x509-node.md)
