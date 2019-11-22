---
title: Kullanarak sanal X. 509.440 cihazını Azure IoT Hub sağlamaC#
description: Hızlı başlangıç-Azure IoT Hub cihaz sağlama hizmeti için cihaz SDK 'sını C# kullanarak sanal bir X. 509.440 cihazı oluşturun ve sağlayın. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: csharp
ms.custom: mvc
ms.openlocfilehash: 0513884e7869bc14627259ea98a85f4966147d9b
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74276309"
---
# <a name="quickstart-create-and-provision-a-simulated-x509-device-using-c-device-sdk-for-iot-hub-device-provisioning-service"></a>Hızlı başlangıç: IoT Hub cihaz sağlama hizmeti için cihaz SDK 'sını kullanarak C# sanal bir X. 509.952 cihazı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Bu adımlar, Windows işletim sistemini çalıştıran bir geliştirme makinesinde bir X. 509.952 cihazının benzetimini yapmak için [Azure IoT örneklerinin C# ](https://github.com/Azure-Samples/azure-iot-samples-csharp) nasıl kullanılacağını gösterir. Örnek ayrıca, cihaz sağlama hizmeti 'ni kullanarak sanal cihazı bir IoT Hub da bağlar.

Otomatik sağlama işlemini bilmiyorsanız, [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirdiğinizden emin olun. Ayrıca devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. 

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:
- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Bireysel kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede bireysel kayıtlar gösterilmektedir.

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

<a id="setupdevbox"></a>
## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama 

1. Makinenizde [.NET Core 2,1 SDK veya sonraki bir sürümünün](https://www.microsoft.com/net/download/windows) yüklü olduğundan emin olun. 

1. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek [ araçlarının son sürümleri için ](https://git-scm.com/download/)Software Freedom Conservancy’nin Git istemci araçlarına`git` bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

1. Bir komut istemi veya Git Bash’i açın. GitHub deposu için C# Azure IoT örneklerini kopyalayın:
    
    ```cmd
    git clone https://github.com/Azure-Samples/azure-iot-samples-csharp.git
    ```

## <a name="create-a-self-signed-x509-device-certificate-and-individual-enrollment-entry"></a>Otomatik olarak imzalanan X.509 cihazı sertifikası ve bireysel kayıt girişi oluşturma

Bu bölümde, otomatik olarak imzalanan X.509 sertifikası kullanacaksınız. Aşağıdaki konuları göz önünde bulundurmak önemlidir:

* Otomatik olarak imzalanan sertifikalar yalnızca test amaçlıdır ve üretimde kullanılmamalıdır.
* Otomatik olarak imzalanan sertifikanın varsayılan sona erme tarihi bir yıldır.

Sanal cihaz için bireysel kayıt girişiyle kullanılacak sertifikayı oluşturmak için, [aygıt Istemcisi örneği-X. 509.440 kanıtlama sağlayan](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/device/X509Sample) örnek kodu kullanacaksınız.


1. Komut isteminde dizini X.509 cihaz sağlama örneğinin proje dizini olacak şekilde değiştirin.

    ```cmd
    cd .\azure-iot-samples-csharp\provisioning\Samples\device\X509Sample
    ```

2. Örnek kod parola korumalı PKCS12 biçimindeki bir dosya (certificate.pfx) içinde depolanan X.509 sertifikalarını kullanacak şekilde ayarlanmıştır. Ayrıca, bu hızlı başlangıçta tek bir kayıt oluşturmak için ortak anahtar sertifika dosyası (Certificate. cer) gerekir. Otomatik olarak imzalanan sertifika ile ona bağlı .cer ve .pfx dosyalarını oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd
    powershell .\GenerateTestCertificate.ps1
    ```

3. Bu betik sizden PFX parolası ister. Örneği çalıştırdığında kullanmanız gerekeceğinden bu parolayı unutmayın.

    ![ PFX parolasını girme](./media/quick-create-simulated-device-x509-csharp/generate-certificate.png)  


4. Azure portal oturum açın, sol taraftaki menüden **tüm kaynaklar** düğmesini seçin ve sağlama hizmetinizi açın.

5. Cihaz sağlama hizmeti menüsünden kayıtları **Yönet**' i seçin. **Bireysel** kayıtlar sekmesini seçin ve üst kısımdaki **tek kayıt Ekle** düğmesini seçin. 

6. **Kayıt Ekle** panelinde, aşağıdaki bilgileri girin:
   - Kimlik onay **Mekanizması** olarak *X.509*'u seçin.
   - *Birincil sertifika. pek veya. cer dosyası*' nın altında, önceki adımlarda oluşturulan sertifika **. cer** sertifika dosyasını seçmek için *Dosya Seç* ' i seçin.
   - **Cihaz Kimliği** alanını boş bırakın. Cihazınız, cihaz kimliği X.509 sertifikasındaki ortak ad (CN) olan **iothubx509device1** olacak şekilde sağlanır. Bu değer aynı zamanda bireysel kayıt girişi için kayıt kimliği olarak kullanılır. 
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
       - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
       - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
   - Tamamlandıktan sonra **Kaydet** düğmesine basın. 

     [![Portalda X.509 kanıtı için tek kayıt ekleme](./media/quick-create-simulated-device-x509-csharp/device-enrollment.png)](./media/quick-create-simulated-device-x509-csharp/device-enrollment.png#lightbox)
    
   Kayıt başarıyla tamamlandığında, X.509 kayıt girişiniz cihazınız **Bireysel Kayıtlar** sekmesindeki *Kayıt Kimliği* sütununun altında *iothubx509device1* olarak gösterilir. 

## <a name="provision-the-simulated-device"></a>Sanal cihazı sağlama

1. Sağlama hizmetinizin **genel bakış** dikey penceresinden, **_kimlik kapsamı_** değerini aklınızda olun.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509-csharp/copy-scope.png) 


2. X.509 cihaz sağlama örneğini derlemek ve çalıştırmak için aşağıdaki komutu yazın. `<IDScope>` değerinin yerine sağlama hizmetinizin Kimlik Kapsamını girin. 

    ```cmd
    dotnet run <IDScope>
    ```

3. İstendiğinde önceden oluşturduğunuz PFX dosyasının parolasını girin. IoT hub bilgilerinizi almak için cihaz önyüklemesi ve Cihaz Sağlama Hizmetine bağlanma benzetimi gerçekleştiren iletilere dikkat edin. 

    ![Örnek cihaz çıktısı](./media/quick-create-simulated-device-x509-csharp/sample-output.png) 

4. Cihazın sağlandığını doğrulayın. Sanal cihazın, sağlama hizmetinize bağlı olan IoT Hub 'ına başarıyla sağlanması sırasında, cihaz KIMLIĞI hub 'ın **IoT cihazları** dikey penceresinde görünür. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device-x509-csharp/registration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub'daki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Cihaz istemci örneğini üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Makinenizde TPM simülatörü penceresini kapatın.
1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından cihaz sağlama hizmetinizi seçin. **Genel bakış** dikey penceresinin üst kısmında, bölmenin en üstünde bulunan **Sil** ' e basın.  
1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından IoT Hub 'ınızı seçin. **Genel bakış** dikey penceresinin üst kısmında, bölmenin en üstünde bulunan **Sil** ' e basın.  

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Windows makinenizde sanal bir X. 509.952 cihazı oluşturdunuz ve portaldaki Azure IoT Hub cihaz sağlama hizmetini kullanarak IoT Hub 'ınıza sağladınız. X. 509.952 cihazınızı programlı bir şekilde kaydetmeyi öğrenmek için, X. 509.440 cihazlarının programlı kaydı için hızlı başlangıç 'a geçin. 

> [!div class="nextstepaction"]
> [Azure hızlı başlangıç-X. 509.440 cihazlarını Azure IoT Hub cihaz sağlama hizmeti 'ne kaydetme](quick-enroll-device-x509-csharp.md)
