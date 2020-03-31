---
title: "C kullanarak Azure IoT Hub'ına X.509 aygıtının sağlanması #"
description: Quickstart - Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) için C# cihazı SDK kullanarak simüle edilmiş bir X.509 aygıtı oluşturun ve sağlayın. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: csharp
ms.custom: mvc
ms.openlocfilehash: 639ab07113a0e62cac43af5b79f052da1efd93ab
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74976528"
---
# <a name="quickstart-create-and-provision-a-simulated-x509-device-using-c-device-sdk-for-iot-hub-device-provisioning-service"></a>Quickstart: IoT Hub Aygıt Sağlama Hizmeti için C# cihazı SDK kullanarak simüle edilmiş bir X.509 cihazı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Bu adımlar, Windows işletim sistemi çalıştıran bir geliştirme makinesinde bir X.509 aygıtını simüle etmek [için C# için Azure IoT Örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp) nasıl kullanacağınızı gösterir. Örnek ayrıca, simüle edilen aygıtı Aygıt Sağlama Hizmeti'ni kullanarak bir IoT Hub'ına da bağlar.

Otomatik sağlama işlemini bilmiyorsanız, [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirdiğinizden emin olun. Ayrıca devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. 

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:
- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Bireysel kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

Bu makalede bireysel kayıtlar gösterilmektedir.

[!INCLUDE [IoT Device Provisioning Service basic](../../includes/iot-dps-basic.md)]

<a id="setupdevbox"></a>
## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama 

1. [.NET Core 2.1 SDK'nın](https://www.microsoft.com/net/download/windows) veya daha sonra makinenize yüklendiğinizden emin olun. 

1. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

1. Bir komut istemi veya Git Bash’i açın. C# GitHub repo için Azure IoT Örneklerini klonla:
    
    ```cmd
    git clone https://github.com/Azure-Samples/azure-iot-samples-csharp.git
    ```

## <a name="create-a-self-signed-x509-device-certificate-and-individual-enrollment-entry"></a>Otomatik olarak imzalanan X.509 cihazı sertifikası ve bireysel kayıt girişi oluşturma

Bu bölümde, otomatik olarak imzalanan X.509 sertifikası kullanacaksınız. Aşağıdaki konuları göz önünde bulundurmak önemlidir:

* Otomatik olarak imzalanan sertifikalar yalnızca test amaçlıdır ve üretimde kullanılmamalıdır.
* Otomatik olarak imzalanan sertifikanın varsayılan sona erme tarihi bir yıldır.

Simüle edilen cihazın tek tek kayıt girişiyle birlikte kullanılacak sertifikayı oluşturmak için [Proveing Device Client Sample - X.509 Attestation](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/device/X509Sample) örnek kodunu kullanacaksınız.


1. Komut isteminde dizini X.509 cihaz sağlama örneğinin proje dizini olacak şekilde değiştirin.

    ```cmd
    cd .\azure-iot-samples-csharp\provisioning\Samples\device\X509Sample
    ```

2. Örnek kod parola korumalı PKCS12 biçimindeki bir dosya (certificate.pfx) içinde depolanan X.509 sertifikalarını kullanacak şekilde ayarlanmıştır. Ayrıca, bu hızlı başlangıçta daha sonra tek bir kayıt oluşturmak için ortak anahtar sertifika dosyasına (certificate.cer) ihtiyacınız vardır. Otomatik olarak imzalanan sertifika ile ona bağlı .cer ve .pfx dosyalarını oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd
    powershell .\GenerateTestCertificate.ps1
    ```

3. Bu betik sizden PFX parolası ister. Örneği çalıştırdığında kullanmanız gerekeceğinden bu parolayı unutmayın.

    ![ PFX parolasını girme](./media/quick-create-simulated-device-x509-csharp/generate-certificate.png)  


4. Azure portalında oturum açın, sol menüdeki **Tüm kaynaklar** düğmesini seçin ve sağlama hizmetinizi açın.

5. Aygıt Sağlama Hizmeti **menüsünden, kayıtları Yönet'i**seçin. **Bireysel Kayıtlar** sekmesini seçin ve üstteki tek tek **kayıt ekle** düğmesini seçin. 

6. Kayıt **Ekle** paneline aşağıdaki bilgileri girin:
   - Kimlik onay *Mekanizması* olarak **X.509**'u seçin.
   - Birincil *sertifika .pem veya .cer dosyası*altında, önceki adımlarda oluşturulan sertifika dosyası **sertifikası.cer'i** seçmek için *bir dosya seçin'i* seçin.
   - **Cihaz Kimliği** alanını boş bırakın. Cihazınız, cihaz kimliği X.509 sertifikasındaki ortak ad (CN) olan **iothubx509device1** olacak şekilde sağlanır. Bu değer aynı zamanda bireysel kayıt girişi için kayıt kimliği olarak kullanılır. 
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
       - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
       - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
   - Tamamlandıktan sonra **Kaydet** düğmesine basın. 

     [![Portalda X.509 attestation için bireysel kayıt ekleme](./media/quick-create-simulated-device-x509-csharp/device-enrollment.png)](./media/quick-create-simulated-device-x509-csharp/device-enrollment.png#lightbox)
    
   Kayıt başarıyla tamamlandığında, X.509 kayıt girişiniz cihazınız *Bireysel Kayıtlar* sekmesindeki *Kayıt Kimliği* sütununun altında **iothubx509device1** olarak gösterilir. 

## <a name="provision-the-simulated-device"></a>Sanal cihazı sağlama

1. Sağlama hizmetiniz için **Genel Bakış** bıçağından, **_Kimlik Kapsamı_** değerine dikkat edin.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509-csharp/copy-scope.png) 


2. X.509 cihaz sağlama örneğini derlemek ve çalıştırmak için aşağıdaki komutu yazın. `<IDScope>` değerinin yerine sağlama hizmetinizin Kimlik Kapsamını girin. 

    ```cmd
    dotnet run <IDScope>
    ```

3. İstendiğinde önceden oluşturduğunuz PFX dosyasının parolasını girin. IoT hub bilgilerinizi almak için cihaz önyüklemesi ve Cihaz Sağlama Hizmetine bağlanma benzetimi gerçekleştiren iletilere dikkat edin. 

    ![Örnek cihaz çıktısı](./media/quick-create-simulated-device-x509-csharp/sample-output.png) 

4. Cihazın sağlandığını doğrulayın. Simüle edilen aygıtın sağlama hizmetinizle bağlantılı IoT hub'ına başarılı bir şekilde sağlanması üzerine, aygıt kimliği hub'ın **IoT aygıtlar** bıçak üzerinde görünür. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device-x509-csharp/registration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub'daki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Aygıt istemcisi örneği üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Makinenizde TPM simülatörü penceresini kapatın.
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından Aygıt Sağlama hizmetinizi seçin. **Genel Bakış** bıçağının üst kısmında, bölmenin üst kısmındaki **Sil'e** basın.  
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından IoT hub'ınızı seçin. **Genel Bakış** bıçağının üst kısmında, bölmenin üst kısmındaki **Sil'e** basın.  

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Windows makinenizde simüle edilmiş bir X.509 aygıtı oluşturdunuz ve portaldaki Azure IoT Hub Aygıt Sağlama Hizmetini kullanarak IoT hub'ınıza sokuldunuz. X.509 aygıtınızı programlı olarak nasıl kaydedeceğinizi öğrenmek için, X.509 aygıtlarının programlı kaydı için hızlı bir şekilde başlamaya devam edin. 

> [!div class="nextstepaction"]
> [Azure quickstart - X.509 aygıtlarını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-x509-csharp.md)
