---
title: Öğretici - IoT Tak ve Çalıştır (önizleme) cihazını Azure IoT Central'a bağlayın
description: Bu öğretici, aygıt kodu oluşturmak için bir aygıt yeteneği modelini nasıl kullanacağınızı gösterir. Ardından aygıt kodunu çalıştırın, aygıtın IoT Merkezi uygulamanıza bağlandığını görün ve otomatik olarak oluşturulan görünümleri kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 42098d54725cc12691839b63c508efbecf042aa0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80064413"
---
# <a name="tutorial-use-a-device-capability-model-to-create-an-iot-plug-and-play-preview-device-and-connect-it-to-your-iot-central-application"></a>Öğretici: Bir IoT Tak ve Çalıştır (önizleme) aygıtı oluşturmak ve IoT Merkezi uygulamanıza bağlamak için bir aygıt yetenek modeli kullanın

_Aygıt özelliği modeli_ (DCM), bir [IoT Tak ve Çalıştır (önizleme)](../../iot-pnp/overview-iot-plug-and-play.md) aygıtının özelliklerini açıklar. IoT Central, aygıt ilk kez bağlandığında bir aygıt şablonu ve görselleştirmeoluşturmak için Bir DCM kullanabilir.

[IoT Tak ve Çalıştır](../../iot-pnp/overview-iot-plug-and-play.md) desteği önizlemededir ve yalnızca belirli bölgelerde desteklenir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * DCM kullanarak bir IoT Tak ve Çalıştır (önizleme) aygıtı oluşturmak için Visual Studio Code'u kullanın.
> * Aygıt kodunu Windows'da çalıştırın ve IoT Central uygulamanıza bağlandığına bakın.
> * Aygıtın gönderdiği simüle edilmiş telemetriyi görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

**Özel uygulama > Özel uygulama** şablonu kullanarak bir IoT Central uygulaması oluşturmak için Azure [IoT Central uygulamasını](./quick-deploy-iot-central.md) hızlı bir şekilde oluşturun'ı tamamlayın.

Bu öğreticiyi tamamlamak için, yerel makinenize aşağıdaki yazılımı yüklemeniz gerekir:

* **C++ oluşturma araçları** ve **Nuget paket yöneticisi bileşen** iş yükleriyle Visual Studio için Araçlar [oluşturun.](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) Veya visual [studio (Topluluk, Profesyonel veya Kurumsal)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 veya 2015'e sahipseniz ve aynı iş yüklerini yüklüyorsanız.
* [Git.](https://git-scm.com/download/)
* [CMake](https://cmake.org/download/) - **CMake**yüklediğinizde , **sistem PATH CMake ekle**seçeneğini seçin.
* [Görsel Stüdyo Kodu](https://code.visualstudio.com/).
* [Node.js](https://nodejs.org/)
* Yardımcı `dps-keygen` program:

    ```cmd/sh
    npm i -g dps-keygen
    ```

### <a name="install-azure-iot-tools"></a>Azure IoT Araçlarını Yükleme

Azure IoT Araçları uzantı paketini VS Kodu'na yüklemek için aşağıdaki adımları kullanın:

1. VS Kodu'nda **Uzantılar** sekmesini seçin.
1. Azure **IoT Araçlarını**arayın.
1. **Yükle**’yi seçin.

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu öğreticide, Geliştirme ortamınıza Azure IoT C aygıtı SDK'yı yüklemek için [Vcpkg](https://github.com/microsoft/vcpkg) kitaplık yöneticisini kullanırsınız.

1. Bir komut istemi açın. Vcpkg yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    git clone https://github.com/Microsoft/vcpkg.git
    cd vcpkg

    .\bootstrap-vcpkg.bat
    ```

    Daha sonra, kullanıcı çapında [tümleştirme](https://github.com/microsoft/vcpkg/blob/master/docs/users/integration.md)bağlamak için aşağıdaki komutu çalıştırın. Bu komutu ilk çalıştırdığınızda yönetim hakları gerektirir:

    ```cmd
    .\vcpkg.exe integrate install
    ```

1. Azure IoT C cihazı SDK Vcpkg'ı yükleyin:

    ```cmd
    .\vcpkg.exe install azure-iot-sdk-c[public-preview,use_prov_client]
    ```

## <a name="generate-device-key"></a>Aygıt anahtarı oluşturma

Bir aygıtı IoT Merkezi uygulamasına bağlamak için bir aygıt anahtarına ihtiyacınız vardır. Aygıt anahtarı oluşturmak için:

1. Azure IoT Central uygulaması hızlı başlat'ta **Özel uygulama** şablonu kullanarak oluşturduğunuz [IoT Central uygulamasında](./quick-deploy-iot-central.md) oturum açın.

1. **Yönetim** sayfasına gidin ve **Aygıt Bağlantısı'nı**seçin.

1. **Kimlik Kapsamını** ve **Görünüm Tuşları'nı**seçtiğinizde gördüğünüz **Birincil Anahtar'ı** not edin. Bu değerleri daha sonra bu öğreticide kullanırsınız.

    ![Cihaz bağlantısı](./media/tutorial-connect-pnp-device/device-connection.png)

1. Bir komut istemi açın ve aygıt anahtarı oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dps-keygen -di:mxchip-001 -mk:{Primary Key from previous step}
    ```

    Oluşturulan _aygıt anahtarıbir_not alın, bu öğretici daha sonraki bir adımda bu değeri kullanın.

## <a name="download-your-model"></a>Modelinizi indirin

Bu öğreticide, bir MxChip IoT DevKit aygıtı için genel DCM'yi kullanırsınız. Kodu çalıştırmak için gerçek bir DevKit aygıtına ihtiyacınız yoktur, bu öğreticide Windows'da çalıştırmak için kodu derlersiniz.

1. Çağrılan `central_app` bir klasör oluşturun ve VS Kodu'nda açın.

1. Komut paletini açmak için **Ctrl+Shift+P'yi** kullanın, **IoT Tak ve Çalıştır'ı**girin ve **Model Deposunu Aç'ı**seçin. **Genel depo seçin.** VS Kodu, ortak model deposundaki DCM'lerin listesini gösterir.

1. Kimliği olan **MXChip IoT DevKit** DCM'yi `urn:mxchip:mxchip_iot_devkit:1`seçin. Ardından **İndir'i**seçin. Artık `central_app` klasörde DCM'nin bir kopyası var.

![Model deposu ve DCM](./media/tutorial-connect-pnp-device/public-repository.png)

> [!NOTE]
> IoT Central ile çalışmak için aygıt yetenek modelinin aynı dosyada sıralı olarak tanımlanan tüm arabirimlere sahip olması gerekir.

## <a name="generate-the-c-code-stub"></a>C kodu saplamasını oluşturma

Şimdi **MXChip IoT DevKit** DCM ve ilişkili arayüzleri var, modeli uygulayan aygıt kodu oluşturabilirsiniz. VS kodunda C kodu saplaması oluşturmak için:

1. DCM dosyaları açık olan klasör açıkken, komut paletini açmak için **Ctrl+Shift+P'yi** kullanın, **IoT Tak ve Çalıştır'ı**girin ve **Aygıt Kodu Saplaması Oluştur'u**seçin.

    > [!NOTE]
    > IoT Tak ve Çalıştır Kodu Jeneratör yardımcı programını ilk kez kullandığınızda, indirmek birkaç saniye sürer.

1. Az önce indirdiğiniz **MXChip IoT DevKit** DCM dosyasını seçin.

1. Proje adını **devkit_device**girin.

1. Diliniz olarak **ANSI C'yi** seçin.

1. Bağlantı yöntemi olarak **Via DPS (Aygıt Sağlama Hizmeti) simetrik anahtarını** seçin.

1. Proje türünüz olarak **Windows'da CMake Project'i** seçin. **MXChip IoT DevKit Project'i**seçmeyin, bu seçenek gerçek bir DevKit aygıtınız olduğunda içindir.

1. SDK'yı eklemenin yolu olarak **Via Vcpkg'ı** seçin.

1. VS Code `devkit_device` klasöründe oluşturulan aygıt kodu saplama dosyaları ile yeni bir pencere açar.

![Oluşturulan aygıt kodu](./media/tutorial-connect-pnp-device/generated-code.png)

## <a name="build-the-code"></a>Kodu oluşturma

Oluşturulan aygıt kodu saplaması oluşturmak için SDK aygıtını kullanırsınız. Oluşturduğunuz uygulama bir **MXChip IoT DevKit** cihazını simüle eder ve IoT Central uygulamanıza bağlanır. Uygulama telemetri ve özellikleri gönderir ve komutları alır.

1. Komut isteminde, `cmake` `devkit_device` klasörde bir alt dizini oluşturun ve bu klasöre gidin:

    ```cmd
    mkdir cmake
    cd cmake
    ```

1. Oluşturulan kod saplaması oluşturmak için aşağıdaki komutları çalıştırın. Yer `<directory of your Vcpkg repo>` tutucuyu **Vcpkg** deposunun kopyasına giden yol ile değiştirin:

    ```cmd
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"

    cmake --build . -- /p:Configuration=Release
    ```

    Visual Studio 2017 veya 2015 kullanıyorsanız, kullandığınız yapı araçlarına göre CMake jeneratörü belirtmeniz gerekir:

    ```cmd
    # Either
    cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"
    # or
    cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"
    ```

1. Yapı başarıyla tamamlandıktan sonra, aynı komut istemiyle uygulamanızı çalıştırın. Daha `<scopeid>` `<devicekey>` önce belirttiğiniz değerleri değiştirin ve değiştirin:

    ```cmd
    .\Release\devkit_device.exe mxchip-001 <scopeid> <devicekey>
    ```

1. Aygıt uygulaması IoT Hub'a veri göndermeye başlar. Bazen hatayı `Error registering device for DPS` önceki komutu ilk çalıştırdığınızda görürsünüz. Bu hatayı görürseniz, komutu yeniden deneyin.

## <a name="view-the-device"></a>Cihazı görüntüleme

Cihaz kodunuz IoT Merkezinize bağlandıktan sonra, gönderdiği özellikleri ve telemetriyi görüntüleyebilirsiniz:

1. IoT Merkezi uygulamanızda **Cihazlar** sayfasına gidin ve **mxchip-01** cihazını seçin. Bu aygıt, aygıt kodu bağlandığında otomatik olarak eklendi:

    ![Genel bakış sayfası](./media/tutorial-connect-pnp-device/overview-page.png)

    Birkaç dakika sonra, bu sayfa aygıtın gönderdiği telemetrinin grafiklerini gösterir.

1. Aygıtın gönderdiği özellik değerlerini görmek için **Hakkında** sayfasını seçin.

1. Aygıttaki komutları çağırmak için **Komutlar** sayfasını seçin. Aygıtın, aygıt kodunu çalıştıran komut isteminde yanıt verdiğini görebilirsiniz.

1. Ortak depoda DCM'den oluşturulan IoT Central şablonlarını görmek için **Aygıt şablonları** sayfasına gidin:

    ![Aygıt şablonları sayfası](./media/tutorial-connect-pnp-device/device-template.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, ortak model deposunda bir DCM'den oluşturulan bir IoT Tak ve Çalıştır (önizleme) aygıtını nasıl bağlayabileceğinizi öğrendiniz.

DCM'ler ve kendi modellerinizi nasıl oluşturup oluşturabilirsiniz hakkında daha fazla bilgi edinmek için nasıl yapılacağınız kılavuzuna devam edin:

> [!div class="nextstepaction"]
> [Yeni bir IoT aygıt türü tanımlama](./howto-set-up-template.md)
