---
title: Node. js kullanarak Azure IoT Hub sanal X. 509.440 cihazı sağlama
description: Azure IoT Hub Cihazı Sağlama Hizmeti için Node.js cihaz SDK'sını kullanarak sanal bir X.509 cihazı oluşturma ve sağlama. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: nodejs
ms.custom: mvc
ms.openlocfilehash: 6572b4bc7e6ca8c364d64e8da33fe9140dbfbbbc
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74276291"
---
# <a name="quickstart-create-and-provision-an-x509-simulated-device-using-nodejs-device-sdk-for-iot-hub-device-provisioning-service"></a>Hızlı başlangıç: IoT Hub cihaz sağlama hizmeti için Node. js cihaz SDK 'sını kullanarak bir X. 509.952 sanal cihazı oluşturma ve sağlama
[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Bu adımlar, [Azure IoT Hub Node.js Cihaz SDK’sını](https://github.com/Azure/azure-iot-sdk-node) kullanarak Cihaz Sağlama Hizmeti’nde bir kayıt girişi oluşturma, geliştirme makinenizde bir X.509 cihazının benzetimini yapma, sanal cihazı Cihaz Sağlama Hizmeti’ne bağlama ve IOT hub'ınızda da cihazı kaydetme işlemlerinin nasıl yapılacağını gösterir.

Otomatik sağlama işlemini bilmiyorsanız, [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirdiğinizden emin olun. Ayrıca devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. 

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:
- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Bireysel kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede bireysel kayıtlar gösterilmektedir.

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamlayın.

2. Makinenizde [Node.js v4.0 veya üzeri](https://nodejs.org) bir sürümün yüklü olduğundan emin olun.

3. Makinenizde [Git](https://git-scm.com/download/)’in yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. 

4. Makinenizde [OpenSSL](https://www.openssl.org/)’nin yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Bu kitaplık kaynaktan derlenip yüklenebilir veya [bunun](https://wiki.openssl.org/index.php/Binaries) gibi bir [üçüncü taraftan](https://sourceforge.net/projects/openssl/) indirilip yüklenebilir. 

    > [!NOTE]
    > _Kök_, _ara_ ve/veya _yaprak_ X.509 sertifikalarınızı zaten oluşturduysanız, bu adımı ve sertifika oluşturma ile ilgili aşağıdaki tüm adımları atlayabilirsiniz.
    >

## <a name="create-a-self-signed-x509-device-certificate-and-individual-enrollment-entry"></a>Otomatik olarak imzalanan X.509 cihazı sertifikası ve bireysel kayıt girişi oluşturma

Bu bölümde, otomatik olarak imzalanan X.509 sertifikası kullanacaksınız. Aşağıdaki konuları göz önünde bulundurmak önemlidir:

* Otomatik olarak imzalanan sertifikalar yalnızca test amaçlıdır ve üretimde kullanılmamalıdır.
* Otomatik olarak imzalanan sertifikanın varsayılan sona erme tarihi bir yıldır.

Simülasyon cihazının tek kayıt girdisiyle kullanılacak sertifikayı oluşturmak için [Node.js için Azure IoT SDK'sından](https://github.com/Azure/azure-iot-sdk-node.git) örnek kodu kullanacaksınız.


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

4. [Azure Portal](https://portal.azure.com)oturum açın, sol taraftaki menüden **tüm kaynaklar** düğmesini seçin ve cihaz sağlama hizmeti örneğinizi açın.

5. Cihaz sağlama hizmeti menüsünden kayıtları **Yönet**' i seçin. **Bireysel** kayıtlar sekmesini seçin ve üst kısımdaki **tek kayıt Ekle** düğmesini seçin. 

6. **Kayıt Ekle** panelinde, aşağıdaki bilgileri girin:
   - Kimlik onay **Mekanizması** olarak *X.509*'u seçin.
   - *Birincil sertifika. pek veya. cer dosyası*' nın altında, önceki adımlarda oluşturulan **{Certificate-Name} _cert. ped** sertifika dosyasını seçmek için *Dosya Seç* ' i seçin.  
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
     - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
     - Benzersiz bir cihaz kimliği girin. Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. 
     - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
     - Tamamlandıktan sonra **Kaydet** düğmesine basın. 

     [![Portalda X.509 kanıtı için tek kayıt ekleme](./media/quick-create-simulated-device-x509-node/device-enrollment.png)](./media/quick-create-simulated-device-x509-node/device-enrollment.png#lightbox)

     Kayıt başarılı olduğunda, X. 509.440 cihazınız *bireysel* kayıtlar SEKMESINDEKI *kayıt kimliği* sütununun altında **{CertificateName}** olarak görünür. daha sonra bu değeri aklınızda yapın.

## <a name="simulate-the-device"></a>Cihazı benzetme

[Azure IOT Hub Node.js Cihaz SDK'sı](https://github.com/Azure/azure-iot-sdk-node), bir cihazın benzetimini yapmak için kolay bir yol sağlar. Daha fazla bilgi için bkz. [Cihaz kavramları](https://docs.microsoft.com/azure/iot-dps/concepts-device).

1. Azure portal, cihaz sağlama hizmetiniz için **genel bakış** dikey penceresini seçin ve **_genel cihaz uç noktası_** ve **_kimlik kapsamı_** değerlerini aklınızda edin.

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
    - `provisioning host` öğesini yukarıdaki **1. Adım _’da belirtilen_** Genel Cihaz Uç Noktası ile değiştirin.
    - `id scope`, yukarıdaki **1. adımda** belirtilen **_kimlik kapsamıyla_** değiştirin. 
    - `registration id`, önceki bölümde belirtilen **_kayıt kimliğiyle_** değiştirin.
    - `cert filename` ve `key filename` öğelerini yukarıdaki **2. Adım**’da kopyaladığınız dosyalarla değiştirin. 

5. Betiği yürütün ve cihazın başarıyla sağlandığını doğrulayın.

    ```cmd/sh
    node register_x509.js
    ```   

6. Portalda, sağlama hizmetinize bağlı olan IoT Hub 'ına gidin ve **IoT cihazları** dikey penceresini açın. Benzetimli X. 509.952 cihazının hub 'a başarıyla sağlanması sırasında cihaz KIMLIĞI **IoT cihazları** dikey penceresinde *durumu* **etkinleştirilmiş**olarak görünür. Örnek cihaz uygulamasını çalıştırmadan önce dikey pencereyi zaten açtıysanız, en üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device-x509-node/hubregistration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub'ındaki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).


## <a name="clean-up-resources"></a>Kaynakları temizleme

Cihaz istemci örneğini üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
2. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından cihaz sağlama hizmetinizi seçin. Hizmetinizin kayıtları **yönetme** dikey penceresini açın ve sonra **bireysel** kayıtlar sekmesini seçin. bu hızlı BAŞLANGıÇTA kaydettiğiniz cihazın *kayıt kimliği* ' nin yanındaki onay kutusunu işaretleyin ve bölmenin en üstündeki **Sil** düğmesine basın. 
3. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından IoT Hub 'ınızı seçin. Hub 'ınız için **IoT cihazları** dikey penceresini açın, bu hızlı başlangıçta kaydettiğiniz CIHAZıN *cihaz kimliği* ' nin yanındaki onay kutusunu işaretleyin ve ardından bölmenin en üstündeki **Sil** düğmesine basın.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, sanal bir X. 509.952 cihazı oluşturdunuz ve portalda Azure IoT Hub cihaz sağlama hizmetini kullanarak IoT Hub 'ınıza sağladınız. X. 509.952 cihazınızı programlı bir şekilde kaydetmeyi öğrenmek için, X. 509.440 cihazlarının programlı kaydı için hızlı başlangıç 'a geçin. 

> [!div class="nextstepaction"]
> [Azure hızlı başlangıç-X. 509.440 cihazlarını Azure IoT Hub cihaz sağlama hizmeti 'ne kaydetme](quick-enroll-device-x509-node.md)
