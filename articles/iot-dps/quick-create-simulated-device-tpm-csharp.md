---
title: "Quickstart - C kullanarak Azure IoT Hub'ına simüle edilmiş bir TPM aygıtı sağlama #"
description: Quickstart - Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) için C# cihazı SDK'yı kullanarak simüle edilmiş bir TPM aygıtı oluşturun ve sağlayın. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2018
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 82bd284ede23e8880f79c614f4a6e2f588a4293c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74977004"
---
# <a name="quickstart-create-and-provision-a-simulated-tpm-device-using-c-device-sdk-for-iot-hub-device-provisioning-service"></a>Hızlı başlatma: IoT Hub Aygıt Sağlama Hizmeti için C# cihazı SDK kullanarak simüle edilmiş bir TPM aygıtı oluşturma ve sağlama

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

Bu adımlar, Windows işletim sistemi çalıştıran bir geliştirme makinesinde bir TPM aygıtını simüle etmek [için C# için Azure IoT Örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp) nasıl kullanacağınızı gösterir. Örnek ayrıca, simüle edilen aygıtı Aygıt Sağlama Hizmeti'ni kullanarak bir IoT Hub'ına da bağlar. 

Örnek kodda cihazın [Donanım Güvenlik Modülü (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) olarak Windows TPM simülatörü kullanılmaktadır. 

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

## <a name="provision-the-simulated-device"></a>Sanal cihazı sağlama

1. Azure Portal’da oturum açın. Sol menüdeki **Tüm kaynaklar** düğmesini seçin ve Cihaz Sağlama hizmetinizi açın. Genel **Bakış** bıçağından, **_Kimlik Kapsamı_** değerine dikkat edin.

    ![Portal dikey penceresindeki sağlama hizmeti Kapsam Kimliğini kopyalama](./media/quick-create-simulated-device-tpm-csharp/copy-scope.png) 

1. Komut isteminde dizini TPM cihaz sağlama örneğinin proje dizini olacak şekilde değiştirin.

    ```cmd
    cd .\azure-iot-samples-csharp\provisioning\Samples\device\TpmSample
    ```

1. TPM cihaz sağlama örneğini derlemek ve çalıştırmak için aşağıdaki komutu yazın. `<IDScope>` değerinin yerine sağlama hizmetinizin Kimlik Kapsamını girin. 

    ```cmd
    dotnet run <IDScope>
    ```

    Bu komut, TPM yonga simülatörü ayrı bir komut istemi başlatacak. Windows'da, Simulator.exe'nin ortak ağlarda iletişim kurmasına izin vermek isteyip istemediğinizi soran bir Windows Güvenlik Uyarısı ile karşılaşabilirsiniz. Bu örneğin amaçları doğrultusunda, isteği iptal edebilirsiniz.

1. Özgün komut penceresi **_Onay anahtarını,_** **_Kayıt Kimliğini_** ve aygıt kaydı için gereken önerilen **_Aygıt Kimliğini_** görüntüler. Bu değerlere dikkat edin. Bu değeri, Aygıt Sağlama Hizmeti örneğinize tek tek bir kayıt oluşturmak için kullanırsınız. 
   > [!NOTE]
   > Komut çıktısını içeren pencereyi TPM simülatörü çıktısını içeren pencereyle karıştırmayın. Ön plana çıkarmak için orijinal komut penceresini seçmeniz gerekebilir.

    ![Komut penceresi çıktısı](./media/quick-create-simulated-device-tpm-csharp/output1.png) 

1. Azure portalında, Aygıt Sağlama Hizmeti menüsünden **kayıtları Yönet'i**seçin. Bireysel **Kayıtlar** sekmesini seçin ve üstteki **tek tek kayıt ekle** düğmesini seçin. 

1. Kayıt **Ekle** paneline aşağıdaki bilgileri girin:
   - Kimlik onay *Mekanizması* olarak **TPM** seçeneğini belirleyin.
   - TPM aygıtınızın *Kayıt Kimliği* ve *Onay anahtarını* daha önce belirttiğiniz değerlerden girin.
   - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
   - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
       - Benzersiz bir *Aygıt Kimliği* girin (önerileni kullanabilir veya kendi kimliğinizi sağlayabilirsiniz). Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. Bir tane sağlamamayı seçerseniz, bunun yerine aygıtı tanımlamak için kayıt kimliği kullanılır.
       - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
   - Tamamlandıktan sonra **Kaydet** düğmesine basın. 

     ![Portal dikey penceresinde cihaz kayıt bilgilerini girme](./media/quick-create-simulated-device-tpm-csharp/enterdevice-enrollment.png)  

   Kayıt başarıyla tamamlanınca cihazınızın *Kayıt Kimliği* listenin altında *Tek Tek Kayıtlar* sekmesinde görünür. 

1. Benzetimli aygıtı kaydetmek için komut penceresinde *Enter* tuşuna basın **_(Onay anahtarı_**, **_Kayıt Kimliği_** ve önerilen **_Aygıt Kimliği)_** görüntülenir. IoT hub bilgilerinizi almak için cihaz önyüklemesi ve Cihaz Sağlama Hizmetine bağlanma benzetimi gerçekleştiren iletilere dikkat edin. 

1. Cihazın sağlandığını doğrulayın. Simüle edilen aygıtın sağlama hizmetinizle bağlantılı IoT hub'ına başarılı bir şekilde sağlanması üzerine, aygıt kimliği hub'ın **IoT aygıtlar** bıçak üzerinde görünür. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device-tpm-csharp/hub_registration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için [Bkz. IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](../iot-hub/iot-hub-devguide-device-twins.md)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aygıt istemcisi örneği üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Makinenizde TPM simülatörü penceresini kapatın.
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından Aygıt Sağlama hizmetinizi seçin. **Genel Bakış** bıçağının üst kısmında, bölmenin üst kısmındaki **Sil'e** basın.  
1. Azure portalındaki sol menüden Tüm **kaynakları** seçin ve ardından IoT hub'ınızı seçin. **Genel Bakış** bıçağının üst kısmında, bölmenin üst kısmındaki **Sil'e** basın.  

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, makinenizde bir TPM simüle edilmiş aygıt oluşturdunuz ve IoT Hub Aygıt Sağlama Hizmeti'ni kullanarak IoT hub'ınıza sokulmuşsunuz. TPM aygıtınızı programlı olarak nasıl kaydedebilirsiniz öğrenmek için, bir TPM aygıtının programlı kaydı için hızlı bir şekilde başlamaya devam edin. 

> [!div class="nextstepaction"]
> [Azure quickstart - TPM aygıtını Azure IoT Hub Aygıt Sağlama Hizmetine kaydedin](quick-enroll-device-tpm-csharp.md)
