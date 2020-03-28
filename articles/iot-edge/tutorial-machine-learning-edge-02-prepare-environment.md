---
title: "Öğretici: Ortamı ayarlama - Azure IoT Edge'de Makine Öğrenimi"
description: 'Öğretici: Çevrenizi, makine öğrenimi için modüllerin geliştirilmesi ve dağıtımı için hazırlayın.'
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 3/12/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 42c776d4d6c3973e7c222c9c9adf3e5105f6c84f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79296820"
---
# <a name="tutorial-set-up-an-environment-for-machine-learning-on-iot-edge"></a>Öğretici: IoT Edge'de makine öğrenimi için bir ortam ayarlama

> [!NOTE]
> Bu makale, IoT Edge'de Azure Machine Learning'i kullanma yla ilgili bir öğretici için bir serinin parçasıdır. Bu makaleye doğrudan ulaştıysanız, en iyi sonuçlar için serinin [ilk makalesiile](tutorial-machine-learning-edge-01-intro.md) başlamanızı öneririz.

Bu makale, ortamınızı geliştirme ve dağıtım için hazırlamanıza yardımcı olur. İlk olarak, ihtiyacınız olan tüm araçları içeren bir geliştirme makinesi ayarlayın. Ardından Azure'da gerekli bulut kaynaklarını oluşturun.

## <a name="set-up-the-development-vm"></a>Geliştirme VM'sini ayarlama

Bu adım genellikle bir bulut geliştiricisi tarafından gerçekleştirilir. Bazı yazılımlar da bir veri bilim adamı için yararlı olabilir.

Önceden yapılandırılmış olan birçok ön koşula sahip bir Azure sanal makinesi oluşturan bir PowerShell komut dosyası oluşturduk. Oluşturduğumuz [VM'nin iç içe sanallaştırmayı](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization)ele alabilmeleri gerekiyor, bu yüzden [Standard_D8s_v3](../virtual-machines/dv3-dsv3-series.md) bir makine boyutu seçtik.

Geliştirme VM ile kurulacaktır:

* Windows 10
* [Çikolatalı](https://chocolatey.org/)
* [Windows için Docker Masaüstü](https://www.docker.com/products/docker-desktop)
* [Windows için Git](https://gitforwindows.org/)
* [Windows için Git Kimlik Yöneticisi](https://github.com/Microsoft/Git-Credential-Manager-for-Windows)
* [.NET Çekirdek SDK](https://dotnet.microsoft.com/)
* [Python 3](https://www.python.org/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.1.0)
* [VS Kod Uzantıları](https://marketplace.visualstudio.com/search?target=VSCode)
  * [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
  * [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  * [C #](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
  * [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
  * [Powershell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)

Geliştirici VM kesinlikle gerekli değildir - tüm geliştirme araçları yerel bir makine üzerinde çalıştırılabilir. Ancak, eşit bir oyun alanı sağlamak için VM'yi kullanmanızı şiddetle öneririz.

Sanal makinenin oluşturulması ve yapılandırılması yaklaşık 30 dakika sürer.

1. [Machine Learning ve IoT Edge](https://github.com/Azure-Samples/IoTEdgeAndMlSample) örnek deposunu yerel bilgisayarınıza klonla veya indirin.

1. PowerShell'i yönetici olarak açın ve kodu indirdiğiniz kök dizinin altında bulunan **\IoTEdgeAndMlSample\DevVM** dizinine gidin. Kaynağınızın kök dizinine " olarak `srcdir`atıfta bulunacağız.

    ```powershell
    cd c:\srcdir\IoTEdgeAndMlSample\DevVM
    ```

   DevVM dizini, bu öğreticiyi tamamlamak için uygun bir Azure sanal makinesi oluşturmak için gereken dosyaları içerir.

1. Komut dosyalarının yürütülmesine izin vermek için aşağıdaki komutu çalıştırın. İstendiğinde **Tümüne Evet'i** seçin.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process
    ```

1. Create-AzureDevVM.ps1 çalıştırın.

    ```powershell
    .\Create-AzureDevVm.ps1
    ```

    İstendiğinde, aşağıdaki bilgileri sağlayın:

    * **Azure Abonelik Kimliği**: Portaldaki Azure [Abonelikleri'nde](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) bulunan abonelik kimliğiniz.
    * **Kaynak Grubu Adı**: Azure'da yeni veya varolan bir kaynak grubunun adı.
    * **Konum**: Sanal makinenin oluşturulacağı bir Azure konumu seçin. Örneğin, 'Batı ABD 2' veya 'Kuzey Avrupa'. Daha fazla bilgi için [Azure konumları'na](https://azure.microsoft.com/global-infrastructure/locations/)bakın.
    * **Kullanıcı Adı**: VM için yönetici hesabı için unutulmaz bir ad sağlayın.
    * **Parola**: VM için yönetici hesabı için bir parola ayarlayın.

   Komut dosyası, aşağıdaki adımları yürütürken birkaç dakika çalışır:

    1. [Azure PowerShell Az modüllerini](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.1.0)yükler.
    1. Azure'da oturum açmanızı ister.
    1. VM'nizin oluşturulmasına ait bilgileri onaylar. Devam etmek için **y** tuşuna basın veya **Enter** tuşuna basın.
    1. Yoksa kaynak grubu oluşturur.
    1. Sanal makineyi dağıtıyor.
    1. VM'de Hyper-V sağlar.
    1. Geliştirme için yazılım gereksinimini yükler ve örnek deposunu klonlar.
    1. VM'yi yeniden başlatır.
    1. VM'ye bağlanmak için masaüstünüzde bir RDP dosyası oluşturur.

   VM adının yeniden başlatması istenirse, komut dosyası çıktısından adını kopyalayabilirsiniz. Çıktı, VM'ye bağlanmak için RDP dosyasına giden yolu da gösterir.

### <a name="set-auto-shutdown-schedule"></a>Otomatik kapatma zamanlamasını ayarlama

Maliyeti azaltmanıza yardımcı olmak için, geliştirme VM'si 1900 PST olarak ayarlanmış otomatik kapatma zamanlamasıyla oluşturulmuştur. Konumunuza ve zamanlamanıza bağlı olarak bu ayarı güncelleştirmeniz gerekebilir. Kapatma zamanlamasını güncelleştirmek için:

1. Azure portalında, komut dosyasının oluşturduğu VM'ye gidin.

1. Sol bölme menüsünden, **İşlemler**altında Otomatik **kapatma'yı**seçin.

1. **Zamanlanan kapatma** ve **Saat dilimini** istenilen şekilde ayarlayın ve **Kaydet'i**seçin.

## <a name="connect-to-the-development-vm"></a>Geliştirme VM'sine bağlanın

Şimdi biz öğretici tamamlamak için gerekli yazılımı yüklemeyi bitirmek gerekir bir VM oluşturduk.

1. Komut dosyasının masaüstünüzde oluşturduğu RDP dosyasına çift tıklayın.

1. Uzak bağlantının yayımcısının bilinmediğini belirten bir iletişim kutusu sunulur. Bu kabul edilebilir, bu nedenle **Bağlan'ı**seçin.

1. VM'yi oluşturmak için sağladığınız yönetici parolasını sağlayın ve **Tamam'ı**tıklatın.

1. VM sertifikasını kabul etmeniz istenir. **Evet'i**seçin.

## <a name="install-visual-studio-code-extensions"></a>Visual Studio Code uzantılarını yükleyin

Geliştirme makinesine bağlandığınızda, geliştirme deneyimini kolaylaştırmak için Visual Studio Code'a bazı kullanışlı uzantılar ekleyin.

1. Geliştirme VM'ine bağlanın, bir PowerShell penceresi açın ve **C:\source\IoTEdgeAndMlSample\DevVM** dizinine gidin. Bu dizin, VM'yi oluşturan komut dosyası tarafından oluşturulmuştur.

    ```powershell
    cd C:\source\IoTEdgeAndMlSample\DevVM
    ```

1. Komut dosyalarının yürütülmesine izin vermek için aşağıdaki komutu çalıştırın. İstendiğinde **Tümüne Evet'i** seçin.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process
    ```

1. Visual Studio Code uzantıları komut dosyasını çalıştırın.

    ```powershell
    .\Enable-CodeExtensions.ps1
    ```

1. Komut dosyası VS kod uzantıları yükleyerek birkaç dakika çalışacaktır:

    * Azure IoT Araçları
    * Python
    * C#
    * Docker
    * PowerShell

## <a name="set-up-iot-hub-and-storage"></a>IoT Hub ve Depolama'yı ayarlama

Bu adımlar genellikle bir bulut geliştiricisi tarafından gerçekleştirilir.

Azure IoT Hub, IoT aygıtları ile bulut arasındaki güvenli iletişimi işlediği için tüm IoT uygulamalarının kalbidir. IoT Edge makine öğrenme çözümünün çalışması için ana koordinasyon noktasıdır.

* IoT Hub, Gelen verileri IoT aygıtlarından diğer akış aşağı hizmetlerine yönlendirmek için rotalar kullanır. Aygıt verilerini Azure Depolama'ya göndermek için IoT Hub yollarından yararlanacağız. Azure Depolama'da cihaz verileri, kalan kullanışlı yaşam (RUL) sınıflandırıcımızı eğitmek için Azure Machine Learning tarafından tüketilir.

* Öğreticinin ilerleyen saatlerinde, Azure IoT Edge aygıtımızı yapılandırmak ve yönetmek için IoT Hub'ı kullanacağız.

Bu bölümde, bir Azure IoT hub'ı ve bir Azure Depolama hesabı oluşturmak için bir komut dosyası kullanırsınız. Azure portalında, hub tarafından alınan verileri bir Azure Depolama kapsayıcısına ileten bir rotayı yapılandırırsınız. Bu adımların tamamlanması yaklaşık 10 dakika sürer.

1. Geliştirme VM'sine bağlanın, bir PowerShell penceresi açın ve **IoTHub** dizinine gidin.

    ```powershell
    cd C:\source\IoTEdgeAndMlSample\IoTHub
    ```

1. Oluşturma komut dosyasını çalıştırın. Geliştirme VM'sini oluştururken abonelik kimliği, konum ve kaynak grubu için aynı değerleri kullanın.

    ```powershell
    .\New-HubAndStorage.ps1 -SubscriptionId <subscription id> -Location <location> -ResourceGroupName <resource group>
    ```

    * Azure'da oturum açmanız istenir.
    * Komut dosyası, Hub ve Depolama hesabınızın oluşturulmasına ait bilgileri onaylar. Devam etmek için **y** tuşuna basın veya **Enter** tuşuna basın.

Betiğin çalıştırılması yaklaşık iki dakika sürer. Tamamlandıktan sonra, komut dosyası IoT hub'ının ve depolama hesabının adını çıkartır.

## <a name="review-route-to-storage-in-iot-hub"></a>IoT Hub'daki depolama ya da depolama yolunu gözden geçirme

IoT hub'ını oluşturmanın bir parçası olarak, önceki bölümde çalıştırdığımız komut dosyası da özel bir bitiş noktası ve bir rota oluşturdu. IoT Hub yolları bir sorgu ifadesi ve bir bitiş noktasından oluşur. İleti ifadeyle eşleşirse, veriler ilişkili bitiş noktasına giden yol boyunca gönderilir. Uç noktalar Olay Hub'ları, Servis Veri Hizmetleri Sıraları ve Konular olabilir. Bu durumda, bitiş noktası bir depolama hesabındabir blob kapsayıcıdır. Komut dosyamızın oluşturduğu rotayı gözden geçirmek için Azure portalını kullanalım.

1. Azure [portalını](https://portal.azure.com) açın ve bu eğitim için kullandığınız kaynak grubuna gidin.

1. Kaynaklar listesinde, komut dosyasının oluşturduğu IoT Hub'ını seçin. Bu gibi rasgele karakterler ile biten `IotEdgeAndMlHub-jrujej6de6i7w`bir ad olacak.

1. Sol bölme menüsünden, **Mesajlaşma**altında İleti **yönlendirmesini**seçin.

1. İleti **yönlendirme** sayfasında, **Özel uç noktalar** sekmesini seçin.

1. **Depolama** bölümünü genişletin:

   ![TurbofanDeviceStorage'ın özel uç noktalar listesinde olduğunu doğrulayın](media/tutorial-machine-learning-edge-02-prepare-environment/custom-endpoints.png)

   **TurbofanDeviceStorage'ın** özel uç noktalar listesinde olduğunu görüyoruz. Bu bitiş noktası ile ilgili aşağıdaki özelliklere dikkat edin:

   * `devicedata` **Oluşturduğunuz**blob depolama kabına işaret ediyor.
   * **Dosya adı biçimi,** adındaki son öğe olarak bölümlenmiştir. Bu biçimi, daha sonra eğitimde Azure Notebook'larla yapacağımız dosya işlemleri için daha uygun buluyoruz.
   * **Durumu** sağlıklı olmalıdır.

1. **Rotalar** sekmesini seçin.

1. **turbofanDeviceDataToStorage**adlı rotayı seçin.

1. **Rotalar ayrıntıları** sayfasında, rotanın bitiş noktasının **turbofanDeviceStorage** bitiş noktası olduğunu unutmayın.

   ![TurbofanDeviceDataToStorage rotası hakkındaki ayrıntıları gözden geçirin](media/tutorial-machine-learning-edge-02-prepare-environment/route-details.png)

1. **Doğru**olarak ayarlanmış **Yönlendirme sorgusuna**bakın. Bu ayar, tüm aygıt telemetri iletilerinin bu rotayla eşleşeceği anlamına gelir; ve bu nedenle tüm mesajlar **turbofanDeviceStorage** bitiş noktasına gönderilecektir.

1. Hiçbir ediniş yapılmadığından, bu sayfayı kapatman.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir IoT Hub'ı oluşturduk ve bir Azure Depolama hesabına bir rota yapılandırıldık. Daha sonra, ioT Hub üzerinden bir dizi simüle edilmiş cihazdan gelen verileri depolama hesabına göndeririz. Öğreticinin ilerleyen dakikalarında, IoT Edge cihazımızı ve modüllerimizi yapılandırdıktan sonra rotaları yeniden ziyaret edeceğiz ve yönlendirme sorgusuna biraz daha bakacağız.

IoT Edge öğreticisinde Machine Learning'in bu bölümünde kapsanan adımlar hakkında daha fazla bilgi için bkz:

* [Azure IoT Temelleri](https://docs.microsoft.com/azure/iot-fundamentals/)
* [IoT Hub ile ileti yönlendirmeyi yapılandırma](../iot-hub/tutorial-routing.md)
* [Azure portalını kullanarak bir IoT hub'ı oluşturma](../iot-hub/iot-hub-create-through-portal.md)

İzlenecek benzetimli bir aygıt oluşturmak için sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Aygıt verileri oluşturma](tutorial-machine-learning-edge-03-generate-data.md)
