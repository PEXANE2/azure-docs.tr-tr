---
title: Öğretici-Azure 'a IoT Tak ve Kullan (Önizleme) cihazını bağlama IoT Central
description: Bu öğretici, cihaz kodu oluşturmak için bir cihaz yetenek modelinin nasıl kullanılacağını gösterir. Ardından Cihaz kodunu çalıştırın, cihaz IoT Central uygulamasına bağlanma sayfasına bakın ve otomatik olarak oluşturulan görünümleri kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 42098d54725cc12691839b63c508efbecf042aa0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80064413"
---
# <a name="tutorial-use-a-device-capability-model-to-create-an-iot-plug-and-play-preview-device-and-connect-it-to-your-iot-central-application"></a>Öğretici: IoT Tak ve Kullan (Önizleme) cihazı oluşturmak ve IoT Central uygulamanıza bağlamak için bir cihaz yetenek modeli kullanın

_Cihaz yetenek modeli_ (DCM), [IoT Tak ve kullan (Önizleme)](../../iot-pnp/overview-iot-plug-and-play.md) cihazının yeteneklerini açıklar. IoT Central, cihaz ilk kez bağlandığında cihaz için bir cihaz şablonu ve görselleştirmeler oluşturmak üzere bir DCM kullanabilir.

[Iot Tak ve kullan](../../iot-pnp/overview-iot-plug-and-play.md) için destek önizleme aşamasındadır ve yalnızca seçili bölgelerde desteklenir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * DCM kullanarak IoT Tak ve Kullan (Önizleme) cihazı oluşturmak için Visual Studio Code kullanın.
> * Windows 'da Cihaz kodunu çalıştırın ve IoT Central uygulamanıza bağlanma bölümüne bakın.
> * Cihazın gönderdiği sanal Telemetriyi görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

Özel uygulama **> özel uygulama** şablonunu kullanarak bir IoT Central uygulaması oluşturmak için [Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central.md) hızlı başlangıcı ' nı doldurun.

Bu öğreticiyi tamamlayabilmeniz için yerel makinenize aşağıdaki yazılımı yüklemeniz gerekir:

* **C++ derleme araçları** ve **NuGet Paket Yöneticisi bileşen** Iş yükleri Ile [Visual Studio için derleme araçları](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) . Ya da [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 veya 2015 aynı iş yükleri yüklüyken zaten yüklüyse.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/) - **CMake**'ı yüklediğinizde, **sistem yoluna CMake Ekle**seçeneğini belirleyin.
* [Visual Studio Code](https://code.visualstudio.com/).
* [Node.js](https://nodejs.org/)
* `dps-keygen` Yardımcı program:

    ```cmd/sh
    npm i -g dps-keygen
    ```

### <a name="install-azure-iot-tools"></a>Azure IoT araçları 'nı yükler

VS Code ' de Azure IoT araçları uzantı paketini yüklemek için aşağıdaki adımları kullanın:

1. VS Code, **Uzantılar** sekmesini seçin.
1. **Azure IoT araçlarını**arayın.
1. **Yükle**’yi seçin.

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu öğreticide, Azure IoT C cihaz SDK 'sını geliştirme ortamınıza yüklemek için [Vcpkg](https://github.com/microsoft/vcpkg) kitaplığı yöneticisini kullanırsınız.

1. Bir komut istemi açın. Vcpkg yüklemek için şu komutu yürütün:

    ```cmd
    git clone https://github.com/Microsoft/vcpkg.git
    cd vcpkg

    .\bootstrap-vcpkg.bat
    ```

    Ardından, Kullanıcı genelinde [tümleştirmeyi](https://github.com/microsoft/vcpkg/blob/master/docs/users/integration.md)bağlamak için aşağıdaki komutu çalıştırın. Bu komutu ilk kez çalıştırdığınızda yönetici hakları gerekir:

    ```cmd
    .\vcpkg.exe integrate install
    ```

1. Azure IoT C cihaz SDK 'Sı Vcpkg 'yi yükler:

    ```cmd
    .\vcpkg.exe install azure-iot-sdk-c[public-preview,use_prov_client]
    ```

## <a name="generate-device-key"></a>Cihaz anahtarı oluştur

Bir cihazı IoT Central uygulamasına bağlamak için bir cihaz anahtarına ihtiyacınız vardır. Bir cihaz anahtarı oluşturmak için:

1. [Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central.md) hızlı başlangıcı ' nda **özel uygulama** şablonunu kullanarak oluşturduğunuz IoT Central uygulamasında oturum açın.

1. **Yönetim** sayfasına gidin ve **cihaz bağlantısı**' nı seçin.

1. Anahtar **kapsamını** ve **görüntüleme anahtarlarını**seçtiğinizde gördüğünüz **birincil anahtarı** bir yere göz atın. Bu değerleri daha sonra bu öğreticide kullanacaksınız.

    ![Cihaz bağlantısı](./media/tutorial-connect-pnp-device/device-connection.png)

1. Bir komut istemi açın ve bir cihaz anahtarı oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dps-keygen -di:mxchip-001 -mk:{Primary Key from previous step}
    ```

    Oluşturulan _Cihaz anahtarını_bir yere getirin, bu değeri bu öğreticideki sonraki bir adımda kullanın.

## <a name="download-your-model"></a>Modelinizi indirin

Bu öğreticide, bir Mxyonga IoT DevKit cihazı için genel DCM kullanılır. Kodu çalıştırmak için gerçek bir DevKit cihazına ihtiyacınız yoktur. Bu öğreticide, Windows 'da çalıştırmak için kodu derleyebilirsiniz.

1. Adlı `central_app` bir klasör oluşturun ve vs Code açın.

1. Komut paletini açmak için **CTRL + SHIFT + P** tuşlarını kullanın, **IoT Tak ve kullan**girin ve **model deposunu aç**' ı seçin. **Ortak depoyu**seçin. VS Code, genel model deposundaki DCMs 'lerin bir listesini gösterir.

1. KIMLIĞI `urn:mxchip:mxchip_iot_devkit:1`olan **Mxyongaıot devkit** DCM öğesini seçin. Ardından **İndir**' i seçin. Artık `central_app` klasörde DCM 'nin bir kopyasına sahipsiniz.

![Model deposu ve DCM](./media/tutorial-connect-pnp-device/public-repository.png)

> [!NOTE]
> IoT Central çalışmak için, cihaz yetenek modelinde aynı dosyada satır içi tanımlanmış tüm arabirimlerin olması gerekir.

## <a name="generate-the-c-code-stub"></a>C kodu saplaması oluşturma

Artık **Mxyonu IoT DevKit** DCM ve ilişkili arabirimlerinizde, modeli uygulayan Cihaz kodunu oluşturabilirsiniz. VS Code 'da C kodu saplaması oluşturmak için:

1. DCM dosyaları açık olan klasörde, **CTRL + SHIFT + P** tuşlarına basarak komut paletini açın, **IoT Tak ve kullan**girin ve **cihaz kodu saplama oluştur**' u seçin.

    > [!NOTE]
    > IoT Tak ve Kullan kod Oluşturucu yardımcı programını ilk kez kullandığınızda, indirmesi birkaç saniye sürer.

1. Az önce indirdiğiniz **Mxyongaıot DevKit** DCM dosyasını seçin.

1. **Devkit_device**proje adını girin.

1. Diliniz olarak **ANSI C** 'yi seçin.

1. Bağlantı yöntemi olarak, **DPS ile (cihaz sağlama hizmeti) simetrik anahtar** arasında seçim yapın.

1. Proje türü olarak **Windows üzerinde CMake projesi** seçin. **Mxyongaıot devkit projesi**seçmeyin, bu seçenek, gerçek bir devkit cihazınız olduğunda yöneliktir.

1. SDK 'yı dahil etme yöntemi olarak **Vcpkg aracılığıyla** seçim yapın.

1. VS Code, `devkit_device` klasöründe oluşturulan cihaz kodu saplama dosyaları ile yeni bir pencere açar.

![Oluşturulan cihaz kodu](./media/tutorial-connect-pnp-device/generated-code.png)

## <a name="build-the-code"></a>Kodu oluşturma

Oluşturulan cihaz kodu saplaması oluşturmak için cihaz SDK 'sını kullanın. Oluşturduğunuz uygulama bir **Mxyonga IoT DevKit** cihazına benzetir ve IoT Central uygulamanıza bağlanır. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. Komut isteminde `cmake` `devkit_device` klasörde bir alt dizin oluşturun ve bu klasöre gidin:

    ```cmd
    mkdir cmake
    cd cmake
    ```

1. Oluşturulan kod Saplaması oluşturmak için aşağıdaki komutları çalıştırın. `<directory of your Vcpkg repo>` Yer tutucusunu **vcpkg** deposunun kopyasına olan yol ile değiştirin:

    ```cmd
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"

    cmake --build . -- /p:Configuration=Release
    ```

    Visual Studio 2017 veya 2015 kullanıyorsanız, kullanmakta olduğunuz yapı araçlarına göre CMake oluşturucuyu belirtmeniz gerekir:

    ```cmd
    # Either
    cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"
    # or
    cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="<directory of your Vcpkg repo>\scripts\buildsystems\vcpkg.cmake"
    ```

1. Derleme başarıyla tamamlandıktan sonra, aynı komut isteminde uygulamanızı çalıştırın. Ve `<scopeid>` `<devicekey>` değerlerini daha önce not ettiğiniz değerlerle değiştirin:

    ```cmd
    .\Release\devkit_device.exe mxchip-001 <scopeid> <devicekey>
    ```

1. Cihaz uygulaması IoT Hub veri göndermeye başlar. Bazen önceki komutu ilk kez `Error registering device for DPS` çalıştırdığınızda hatayı görürsünüz. Bu hatayı görürseniz, komutunu yeniden deneyin.

## <a name="view-the-device"></a>Cihazı görüntüleme

Cihaz kodunuz IoT Central bağlandıktan sonra, gönderdiği özellikleri ve Telemetriyi görüntüleyebilirsiniz:

1. IoT Central uygulamanızda, **cihazlar** sayfasına gidin ve **mxyonga-01** cihazını seçin. Cihaz kodu bağlandığında bu cihaz otomatik olarak eklendi:

    ![Genel Bakış sayfası](./media/tutorial-connect-pnp-device/overview-page.png)

    Birkaç dakika sonra, bu sayfada cihazın gönderdiği telemetritin grafikleri gösterilir.

1. Aygıtın gönderildiği özellik değerlerini görmek için **hakkında** sayfasını seçin.

1. Cihazdaki komutları çağırmak için **Komutlar** sayfasını seçin. Cihaz kodunu çalıştıran komut isteminde cihazın yanıt verdiğini görebilirsiniz.

1. Ortak depodaki DCM 'ten IoT Central oluşturulan şablonu görmek için **cihaz şablonları** sayfasına gidin:

    ![Cihaz şablonları sayfası](./media/tutorial-connect-pnp-device/device-template.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, genel model deposundaki bir DCM 'den oluşturulan IoT Tak ve Kullan (Önizleme) cihazını bağlamayı öğrendiniz.

DCMs hakkında daha fazla bilgi edinmek ve kendi modellerinizi oluşturmak için nasıl yapılır kılavuzuna ilerleyin:

> [!div class="nextstepaction"]
> [Yeni bir IoT cihaz türü tanımla](./howto-set-up-template.md)
