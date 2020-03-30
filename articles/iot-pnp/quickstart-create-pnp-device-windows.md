---
title: Bir IoT Tak ve Çalıştır Önizleme aygıtı (Windows) oluşturma | Microsoft Dokümanlar
description: Aygıt kodu oluşturmak için bir aygıt yeteneği modeli kullanın. Ardından aygıt kodunu çalıştırın ve aygıtın IoT Hub'ınıza bağlandığını görün.
author: miagdp
ms.author: miag
ms.date: 12/30/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: e80194f53a406b8b378d0fb787df627937125a27
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75867480"
---
# <a name="quickstart-use-a-device-capability-model-to-create-an-iot-plug-and-play-preview-device-windows"></a>Quickstart: Bir IoT Tak ve Çalıştır Önizleme aygıtı (Windows) oluşturmak için aygıt yeteneği modelini kullanın

[!INCLUDE [iot-pnp-quickstarts-1-selector.md](../../includes/iot-pnp-quickstarts-1-selector.md)]

_Aygıt özelliği modeli_ (DCM), bir IoT Tak ve Çalıştır aygıtının özelliklerini açıklar. Bir DCM genellikle bir ürün SKU ile ilişkilidir. DCM'de tanımlanan özellikler yeniden kullanılabilir arabirimler halinde düzenlenir. Bir DCM'den iskelet aygıt kodu oluşturabilirsiniz. Bu hızlı başlangıç, DCM kullanarak bir IoT Tak ve Çalıştır aygıtı oluşturmak için Windows'da VS Kodu'nun nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatmayı tamamlamak için, yerel makinenize aşağıdaki yazılımı yüklemeniz gerekir:

* **C++ oluşturma araçları** ve **NuGet paket yöneticisi bileşen** iş yükleriyle Visual Studio için Araçlar [oluşturun.](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) Veya visual [studio (Topluluk, Profesyonel veya Kurumsal)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 veya 2015'e sahipseniz ve aynı iş yüklerini yüklüyorsanız.
* [Git.](https://git-scm.com/download/)
* [CMake](https://cmake.org/download/).
* [Görsel Stüdyo Kodu](https://code.visualstudio.com/).

### <a name="install-azure-iot-tools"></a>Azure IoT Araçlarını Yükleme

VS Kodu uzantısı paketi [için Azure IoT Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) yüklemek için aşağıdaki adımları kullanın:

1. VS Kodu'nda **Uzantılar** sekmesini seçin.
1. Azure **IoT Araçlarını**arayın.
1. **Yükle**’yi seçin.

### <a name="install-the-azure-iot-explorer"></a>Azure IoT gezginini yükleme

En son güncelleştirme için "Varlıklar" altında .msi dosyasını seçerek aracın [depo](https://github.com/Azure/azure-iot-explorer/releases) sayfasından **Azure IoT explorer'ın** en son sürümünden indirin ve yükleyin.

### <a name="get-the-connection-string-for-your-company-model-repository"></a>Şirket model deponuz için bağlantı dizesini alın

Bir Microsoft çalışması veya okul hesabıyla oturum açtığınızda Azure [Sertifikalı IoT portalı portalında](https://preview.catalog.azureiotsolutions.com) veya varsa Microsoft İş Ortağı Kimliğinizde _şirket modeli depo bağlantı dizenizi_ bulabilirsiniz. Oturum açmadan sonra **Şirket deposunu** ve ardından **Bağlantı dizelerini**seçin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Hub'ınız için _IoT hub bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak için not):

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, Geliştirme ortamınıza Azure IoT C aygıtı SDK'yı yüklemek için [Vcpkg](https://github.com/microsoft/vcpkg) kitaplık yöneticisini kullanırsınız.

1. Bir komut istemi açın. Vcpkg yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    git clone https://github.com/Microsoft/vcpkg.git
    cd vcpkg

    .\bootstrap-vcpkg.bat
    ```

    Daha sonra, kullanıcı çapında [tümleştirme](https://github.com/microsoft/vcpkg/blob/master/docs/users/integration.md)bağlamak için, aşağıdakileri çalıştırın (not: ilk kullanımda yönetici gerektirir):

    ```cmd
    .\vcpkg.exe integrate install
    ```

1. Azure IoT C cihazı SDK Vcpkg'ı yükleyin:

    ```cmd
    .\vcpkg.exe install azure-iot-sdk-c[public-preview,use_prov_client]
    ```

## <a name="author-your-model"></a>Modelinizi yazar

Bu hızlı başlatmada, varolan bir örnek aygıt yetenek modeli ve ilişkili arabirimleri kullanırsınız.

1. Yerel `pnp_app` sürücünüzde bir klasör oluşturun. Bu klasörü aygıt modeli dosyaları ve aygıt kodu saplaması için kullanırsınız.

1. Aygıt [yetenek modelini ve arayüz örnek dosyalarını](https://github.com/Azure/IoTPlugandPlay/blob/master/samples/SampleDevice.capabilitymodel.json) ve `pnp_app` [arayüz örneğini](https://github.com/Azure/IoTPlugandPlay/blob/master/samples/EnvironmentalSensor.interface.json) indirin ve dosyaları klasöre kaydedin.

    > [!TIP]
    > GitHub'dan bir dosya indirmek için dosyaya gidin, **Raw'a**sağ tıklayın ve ardından **Kaydet bağlantısını**seçin.

1. VS `pnp_app` Kodu ile klasörü açın. Dosyaları IntelliSense ile görüntüleyebilirsiniz:

    ![Cihaz yetenek modeli](media/quickstart-create-pnp-device/dcm.png)

1. İndirdiğiniz dosyalarda, benzersiz `<YOUR_COMPANY_NAME_HERE>` bir `@id` `schema` değere sahip alanları ve alanları değiştirin. Yalnızca a-z, A-Z, 0-9 karakterlerini kullanın ve altını çizin. Daha fazla bilgi için [Bkz. Dijital İkiz tanımlayıcı biçimi.](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format)

## <a name="generate-the-c-code-stub"></a>C kodu saplamasını oluşturma

Artık bir DCM ve ilişkili arabirimleriniz olduğuna göre, modeli uygulayan aygıt kodunu oluşturabilirsiniz. VS Kodu'nda C kodu saplaması oluşturmak için:

1. VS `pnp_app` Kodu'nda klasör açıkken, komut paletini açmak için **Ctrl+Shift+P'yi** kullanın, **IoT Tak ve Çalıştır'ı**girin ve **Aygıt Kodu Saplama**oluştur'u seçin.

    > [!NOTE]
    > IoT Tak ve Play CodeGen CLI'yi ilk kez kullandığınızda, otomatik olarak indirip yüklemeniz birkaç saniye sürer.

1. Aygıt kodu saplaması oluşturmak için kullanılacak **SampleDevice.capabilitymodel.json** dosyasını seçin.

1. Proje adını **sample_device**girin. Bu, aygıt uygulamanızın adıdır.

1. Diliniz olarak **ANSI C'yi** seçin.

1. Bağlantı yöntemi olarak **Via IoT Hub aygıt bağlantı dizesini** seçin.

1. Proje şablonu olarak **Windows'da CMake Project'i** seçin.

1. SDK cihazını eklemenin yolu olarak **Via Vcpkg'ı** seçin.

1. **sample_device** adlı yeni bir klasör DCM dosyasıyla aynı konumda oluşturulur ve içinde oluşturulan aygıt kodu saplama dosyaları bulunur. VS Code bunları görüntülemek için yeni bir pencere açar.
    ![Cihaz kodu](media/quickstart-create-pnp-device/device-code.png)

## <a name="build-and-run-the-code"></a>Kodu oluşturma ve çalıştırma

Oluşturulan aygıt kodu saplaması oluşturmak için Vcpkg paketini kullanırsınız. Oluşturduğunuz uygulama, bir IoT hub'ına bağlanan bir aygıtı simüle eder. Uygulama telemetri ve özellikleri gönderir ve komutları alır.

1. `sample_device` Klasörde bir `cmake` alt dizin oluşturun ve bu klasöre gidin:

    ```cmd
    mkdir cmake
    cd cmake
    ```

1. Oluşturulan kod saplaması oluşturmak için aşağıdaki komutları çalıştırın (yer tutucuyu Vcpkg repo diziniile değiştirme):

    ```cmd
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"

    cmake --build .
    ```
    
    > [!NOTE]
    > Visual Studio 2017 veya 2015 kullanıyorsanız, kullandığınız yapı araçlarına göre CMake jeneratörü belirtmeniz gerekir:
    >```cmd
    ># Either
    >cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    ># or
    >cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    >```

    > [!NOTE]
    > Cmake C++ derleyicinizi bulamazsa, önceki komutu çalıştırdığınızda yapı hataları alırsınız. Bu durumda, [Visual Studio komut istemibu komutu](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)çalıştırmayı deneyin.

1. Yapı başarıyla tamamlandıktan sonra, ioT hub aygıt bağlantı dizesini parametre olarak geçirerek uygulamanızı çalıştırın.

    ```cmd\sh
    .\Debug\sample_device.exe "<YourDeviceConnectionString>"
    ```

1. Aygıt uygulaması IoT Hub'a veri göndermeye başlar.

    ![Cihaz uygulaması çalışıyor](media/quickstart-create-pnp-device/device-app-running.png)

## <a name="validate-the-code"></a>Kodu doğrulama

### <a name="publish-device-model-files-to-model-repository"></a>Aygıt modeli dosyalarını model deposuna yayımlama

Aygıt kodunu Azure **IoT Explorer**ile doğrulamak için dosyaları model deposunda yayımlamanız gerekir.

1. VS `pnp_app` Code'da klasör açıkken, komut paletini açmak için **Ctrl+Shift+P** kullanın, **IoT Fişi yazın ve play& seçin: Dosyaları Model Deposu'na gönderin.**

1. Seçin `SampleDevice.capabilitymodel.json` `EnvironmentalSensor.interface.json` ve dosyaları.

1. Şirket model depo bağlantı dizenizi girin.

    > [!NOTE]
    > Bağlantı dizesi yalnızca depoya ilk bağlandığınızda gereklidir.

1. VS Kodu çıkış penceresinde ve bildirimde, dosyaların başarıyla yayımlandığını denetleyebilirsiniz.

    > [!NOTE]
    > Aygıt modeli dosyalarını yayımlamada hata lar alırsanız, komut **IoT Tak ve Çalıştır:** Oturum açma ve adımları tekrar gözden geçirmek için Model Deposu'nu oturum alabilen komutunu kullanmayı deneyebilirsiniz.

### <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanma

[!INCLUDE [iot-pnp-iot-explorer-1.md](../../includes/iot-pnp-iot-explorer-1.md)]

4. Şirket deponuzu eklemek için **Ayarlar**, ardından **+ Modül tanım kaynağı ekle,** ardından **Şirket deposunu**seçin. Şirket model deposu bağlantı dizenizi ekleyin ve **Kaydet ve Bağla'yı**seçin.

1. **Cihazlara** genel bakış sayfasında, daha önce oluşturduğunuz aygıt kimliğini bulun. Aygıt uygulaması hala komut isteminde çalışırken, Aygıtın Azure IoT gezginindeki **Bağlantı durumunun** _Bağlı_ olarak bildirdiğini kontrol edin (değilse, yenilenene kadar **Yenile'ye** vurun). Daha fazla ayrıntı görüntülemek için aygıtı seçin.

1. IoT Tak ve İlkelleri - özellikleri, komutları ve telemetriyi - id vazosu yla arayüzü **genişletin:<YOUR_INTERFACE_NAME>:EnvironmentalSensor:1.** Görünecek arabirim adı, modelinizi yazarken koyduğunuz addır.

[!INCLUDE [iot-pnp-iot-explorer-2.md](../../includes/iot-pnp-iot-explorer-2.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, Bir DCM kullanarak bir IoT Tak ve Çalıştır aygıtı oluşturmayı öğrendiniz.

DCM'ler ve kendi modellerinizi nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için öğreticiye devam edin:

> [!div class="nextstepaction"]
> [Öğretici: Visual Studio Code kullanarak bir aygıt yetenek modeli oluşturma ve test etme](tutorial-pnp-visual-studio-code.md)
