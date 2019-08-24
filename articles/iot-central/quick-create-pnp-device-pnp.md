---
title: Azure IoT Tak ve Kullan önizleme cihazını IoT Central 'a bağlama | Microsoft Docs
description: Cihaz kodu oluşturmak için bir cihaz yetenek modeli kullanın. Ardından Cihaz kodunu çalıştırın, cihaz IoT Central uygulamasına bağlanma sayfasına bakın ve otomatik olarak oluşturulan görünümleri kullanın.
author: dominicbetts
ms.author: dobett
ms.date: 08/08/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 48161f56a8885239525587b764c4a116ffc8539f
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997727"
---
# <a name="quickstart-use-a-device-capability-model-to-create-an-iot-plug-and-play-device-and-connect-it-to-your-iot-central-application"></a>Hızlı Başlangıç: Bir IoT Tak ve Kullan cihazı oluşturmak ve IoT Central uygulamanıza bağlamak için bir cihaz yetenek modeli kullanın

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bir _cihaz yetenek modeli_ (DCM) [IoT Tak ve kullan](../iot-pnp/overview-iot-plug-and-play.md) cihazının yeteneklerini açıklar. IoT Central, cihaz ilk kez bağlandığında cihaz için bir cihaz şablonu ve görselleştirmeler oluşturmak üzere bir DCM kullanabilir. Bu hızlı başlangıçta şu işlemleri nasıl yapacağınız gösterilir:

* DCM kullanarak IoT Tak ve Kullan cihazı oluşturmak için Visual Studio Code kullanın.
* Windows 'da Cihaz kodunu çalıştırın ve IoT Central uygulamanıza bağlanma bölümüne bakın.
* Cihazın gönderdiği sanal Telemetriyi görüntüleyin.

## <a name="prerequisites"></a>Önkoşullar

**Önizleme uygulaması** şablonunu kullanarak bir IoT Central uygulaması oluşturmak için [Azure IoT Central uygulaması oluşturma (Önizleme özellikleri)](./quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hızlı başlangıcını doldurun.

Bu hızlı başlangıcı tamamlayabilmeniz için yerel makinenize aşağıdaki yazılımı yüklemeniz gerekir:

* [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) -Visual Studio 'yu yüklerken **NuGet Paket Yöneticisi** bileşenini ve iş yüküyle **Masaüstü geliştirmeyi C++**  eklediğinizden emin olun.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/) -CMake 'iyüklediğinizde, **sistem yoluna CMake Ekle**seçeneğini belirleyin.
* [Visual Studio Code](https://code.visualstudio.com/).
* [Node.js](https://nodejs.org/)
* `dps-keygen` Yardımcı program:

    ```cmd/sh
    npm i -g dps-keygen
    ```

### <a name="install-azure-iot-device-workbench"></a>Azure IoT cihaz çalışma ekranı 'nı yükler

Azure IoT cihaz çalışma ekranı uzantısını VS Code yüklemek için aşağıdaki adımları kullanın:

1. VS Code, **Uzantılar** sekmesini seçin.
1. **Azure IoT cihaz çalışma ekranı**için arama yapın.
1. **Yükle**’yi seçin.

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

### <a name="get-azure-iot-device-sdk-for-c"></a>C için Azure IoT cihaz SDK 'sını alın

Azure IoT C cihaz SDK 'sını oluşturmak için kullanabileceğiniz bir geliştirme ortamı hazırlayın.

1. Bir komut istemi açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Deponun yerel `central_app` kopyasının kökünde bir klasör oluşturun. Bu klasörü cihaz modeli dosyaları ve cihaz kodu saplaması için kullanırsınız.

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir central_app
    ```

## <a name="generate-device-key"></a>Cihaz anahtarı oluştur

Bir cihazı IoT Central uygulamasına bağlamak için bir cihaz anahtarına ihtiyacınız vardır. Bir cihaz anahtarı oluşturmak için:

1. Önceki hızlı başlangıçta oluşturduğunuz IoT Central uygulamasında oturum açın.

1. **Yönetim** sayfasına gidin ve **cihaz bağlantısı**' nı seçin.

1. **Kapsam kimliği** ve **birincil anahtarı**bir yere göz önünde yapın. Bu değerleri daha sonra bu hızlı başlangıçta kullanacaksınız.

    ![Cihaz bağlantısı](./media/quick-create-pnp-device-pnp/device-connection.png)

1. Bir komut istemi açın ve bir cihaz anahtarı oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dps-keygen  -di:mxchip-01 -mk:{Primary Key from previous step}
    ```

    Oluşturulan _Cihaz anahtarını_bir yere getirin, bu değeri bu hızlı başlangıçtaki sonraki bir adımda kullanırsınız.

## <a name="download-your-model"></a>Modelinizi indirin

Bu hızlı başlangıçta, bir Mxyonga IoT DevKit cihazı için genel DCM 'yi kullanırsınız. Kodu çalıştırmak için gerçek bir DevKit cihazına ihtiyacınız yoktur. bu hızlı başlangıçta, Windows 'da çalıştırmak için kodu derleyebilirsiniz.

1. Klasörü `azure-iot-sdk-c\central_app` vs Code açın.

1. Komut paletini açmak için **CTRL + SHIFT + P** tuşlarını kullanın, **IoT Tak ve kullan**girin ve **model deposunu aç**' ı seçin. **Ortak depoyu**seçin. VS Code, genel model deposundaki DCMs 'lerin bir listesini gösterir.

1. Kimliği`urn:mxchip:mxchip_iot_devkit:1`olan **Mxyongaıot devkit** DCM öğesini seçin. Ardından **İndir**' i seçin. Artık `central_app` klasörde DCM 'nin bir kopyasına sahipsiniz.

![Model deposu ve DCM](./media/quick-create-pnp-device-pnp/public-repository.png)

> [!NOTE]
> IoT Central çalışmak için, cihaz yetenek modelinde aynı dosyada satır içi tanımlanmış tüm arabirimlerin olması gerekir.

## <a name="generate-the-c-code-stub"></a>C kodu saplaması oluşturma

Artık **Mxyonu IoT DevKit** DCM ve ilişkili arabirimlerinizde, modeli uygulayan Cihaz kodunu oluşturabilirsiniz. VS Code 'da C kodu saplaması oluşturmak için:

1. DCM dosyaları açık olan klasörde, **CTRL + SHIFT + P** tuşlarına basarak komut paletini açın, **IoT Tak ve kullan**girin ve **cihaz kodu saplama oluştur**' u seçin.

    > [!NOTE]
    > IoT Tak ve Kullan kod Oluşturucu yardımcı programını ilk kez kullandığınızda, indirmesi birkaç saniye sürer.

1. Az önce indirdiğiniz **Mxyongaıot DevKit** DCM dosyasını seçin.

1. Proje adı **devkit_device**girin.

1. Diliniz olarak **ANSI C** 'yi seçin.

1. Proje türü olarak **CMake projesini** seçin. **Mxyongaıot devkit projesi**seçmeyin, bu seçenek, gerçek bir devkit cihazınız olduğunda yöneliktir.

1. Bağlantı yöntemi olarak, **DPS ile (cihaz sağlama hizmeti) simetrik anahtar** arasında seçim yapın.

1. VS Code, `devkit_device` klasöründe oluşturulan cihaz kodu saplama dosyaları ile yeni bir pencere açar.

![Oluşturulan cihaz kodu](./media/quick-create-pnp-device-pnp/generated-code.png)

Oluşturulan cihaz koduna bağlantı bilgilerini eklemek için:

1. Oluşturulan C kodunu içeren VS Code penceresinde. `main.c` dosyasını açın.

1. Daha `[DPS Id Scope]` önce bir notunuz yaptığınız **kapsam kimliğiyle** değiştirin.

1. Önceki `[DPS symmetric key]` bir adımda oluşturduğunuz cihaz anahtarıyla değiştirin.

1. `[device registration Id]` yerine `mxchip-01` yazın.

1. Yaptığınız değişiklikleri kaydedin.

## <a name="build-the-code"></a>Kodu oluşturma

Oluşturulan cihaz kodu saplaması oluşturmak için cihaz SDK 'sını kullanın. Oluşturduğunuz uygulama bir **Mxyonga IoT DevKit** cihazına benzetir ve IoT Central uygulamanıza bağlanır. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. VS Code `CMakeLists.txt` dosyasında dosyasını `azure-iot-sdk-c` klasöründe açın. `CMakeLists.txt` Dosyasını`azure-iot-sdk-c` klasöründe`devkit_device` değil, klasöründe açmış olduğunuzdan emin olun.

1. Derleme sırasında cihaz kodu saplama klasörünü dahil etmek için `CMakeLists.txt` dosyanın altına aşağıdaki satırı ekleyin:

    ```txt
    add_subdirectory(central_app/devkit_device)
    ```

1. `azure-iot-sdk-c` Klasörde bir `cmake` klasör oluşturun ve komut isteminde bu klasöre gidin:

    ```cmd\sh
    mkdir cmake
    cd cmake
    ```

1. Cihaz SDK 'sını ve üretilen kod Saplaması oluşturmak için aşağıdaki komutları çalıştırın:

    ```cmd\sh
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    cmake --build . -- /m /p:Configuration=Release
    ```

1. Derleme başarıyla tamamlandıktan sonra, uygulamanızı çalıştırmak için aynı komut isteminde şu komutu çalıştırın:

    ```cmd\sh
    .\central_app\devkit_device\Release\devkit_device.exe
    ```

1. Cihaz uygulaması, IoT Central uygulamanıza veri göndermeye başlar.

## <a name="view-the-device"></a>Cihazı görüntüleme

Cihaz kodunuz IoT Central bağlandıktan sonra, gönderdiği özellikleri ve Telemetriyi görüntüleyebilirsiniz:

1. IoT Central uygulamanızda, **cihazlar** sayfasına gidin ve **mxyonga-01** cihazını seçin. Cihaz kodu bağlandığında bu cihaz otomatik olarak eklendi:

    ![Genel Bakış sayfası](./media/quick-create-pnp-device-pnp/overview-page.png)

    Birkaç dakika sonra, bu sayfada cihazın gönderdiği telemetritin grafikleri gösterilir.

1. Aygıtın gönderildiği özellik değerlerini görmek için **hakkında** sayfasını seçin.

1. Cihazdaki komutları çağırmak için **Komutlar** sayfasını seçin. Cihaz kodunu çalıştıran komut isteminde cihazın yanıt verdiğini görebilirsiniz.

1. Ortak depodaki DCM 'ten IoT Central oluşturulan şablonu görmek için **cihaz şablonları** sayfasına gidin:

    ![Cihaz şablonları sayfası](./media/quick-create-pnp-device-pnp/device-template.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel model deposundaki bir DCM 'den oluşturulan IoT Tak ve Kullan cihazını bağlamayı öğrendiniz.

DCMs hakkında daha fazla bilgi edinmek ve kendi modellerinizi oluşturmak için nasıl yapılır kılavuzuna ilerleyin:

> [!div class="nextstepaction"]
> [Bir cihaz şablonu ayarlama ve yönetme](./howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
