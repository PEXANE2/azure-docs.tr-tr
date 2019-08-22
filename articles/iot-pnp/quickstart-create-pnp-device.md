---
title: Azure IoT Tak ve Kullan önizleme cihazı oluşturma | Microsoft Docs
description: Cihaz kodu oluşturmak için bir cihaz yetenek modeli kullanın. Ardından, cihaz kodunu çalıştırın ve cihazın IoT Hub bağlanmasına bakın.
author: miagdp
ms.author: miag
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 7cfa6e63f74233e9a3fab8f235584fdbe01e67d9
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878218"
---
# <a name="quickstart-use-a-device-capability-model-to-create-an-iot-plug-and-play-device"></a>Hızlı Başlangıç: IoT Tak ve Kullan cihazı oluşturmak için cihaz yetenek modeli kullanma

Bir _cihaz yetenek modeli_ (DCM) IoT Tak ve kullan cihazının yeteneklerini açıklar. DCM, genellikle bir Ürün SKU 'SU ile ilişkilendirilir. DCM 'de tanımlanan yetenekler yeniden kullanılabilir arabirimler halinde düzenlenmiştir. Bir DCM 'den iskelet cihaz kodu oluşturabilirsiniz. Bu hızlı başlangıçta, bir DCM kullanarak IoT Tak ve Kullan cihazı oluşturmak için VS Code nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için yerel makinenize aşağıdaki yazılımı yüklemeniz gerekir:

* [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) -Visual Studio 'yu yüklerken **NuGet Paket Yöneticisi** bileşenini ve iş yüküyle **Masaüstü geliştirmeyi C++**  eklediğinizden emin olun.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).
* [Visual Studio Code](https://code.visualstudio.com/).

### <a name="install-azure-iot-device-workbench"></a>Azure IoT cihaz çalışma ekranı 'nı yükler

Azure IoT cihaz çalışma ekranı uzantısını VS Code yüklemek için aşağıdaki adımları kullanın:

1. VS Code, **Uzantılar** sekmesini seçin.
1. **Azure IoT cihaz çalışma ekranı**için arama yapın.
1. **Yükle**’yi seçin.

### <a name="install-the-azure-iot-explorer"></a>Azure IoT Gezginini yüklemeyi

Azure IoT gezgin aracını [en son sürüm](https://github.com/Azure/azure-iot-explorer/releases) sayfasından indirin ve yükleyin.

### <a name="get-the-connection-string-for-your-company-model-repository"></a>Şirket modeli deponuzun bağlantı dizesini alın

Bir Microsoft iş veya okul hesabıyla oturum açtığınızda veya varsa Microsoft iş ortağı KIMLIĞINIZLE _Şirket modeli deposu Bağlantı dizenizi_ [Azure Sertifikalı for IoT Portal](https://preview.catalog.azureiotsolutions.com) portalında bulabilirsiniz. Oturum açtıktan sonra **Şirket deposu** ' nu ve ardından **bağlantı dizelerini**seçin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-an-iot-hub"></a>IoT Hub 'ı hazırlama

Ayrıca, bu hızlı başlangıcı tamamlayabilmeniz için Azure aboneliğinizde bir Azure IoT Hub 'ınız olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Azure CLı için Microsoft Azure IoT uzantısını ekleyin:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

IoT Hub 'ınızda cihaz kimliğini oluşturmak için aşağıdaki komutu çalıştırın. **Youriothubname** ve **yourdevice** yer tutucuları gerçek adlarınızla değiştirin. Bir IoT Hub yoksa, [oluşturmak için aşağıdaki yönergeleri](../iot-hub/iot-hub-create-using-cli.md)izleyin:

```azurecli-interactive
az iot hub device-identity create --hub-name [YourIoTHubName] --device-id [YourDevice]
```

Yeni kaydettiğiniz cihazın _Cihaz bağlantı dizesini_ almak için aşağıdaki komutları çalıştırın:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name [YourIoTHubName] --device-id [YourDevice] --output table
```

Hub 'ınız için _IoT Hub bağlantı dizesini_ almak için aşağıdaki komutları çalıştırın:

```azurecli-interactive
az iot hub show-connection-string --hub-name [YourIoTHubName] --output table
```

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

### <a name="get-azure-iot-device-sdk-for-c"></a>C için Azure IoT cihaz SDK 'sını alın

Bu hızlı başlangıçta, Azure IoT C cihaz SDK 'sını klonlamak ve derlemek için kullanabileceğiniz bir geliştirme ortamı hazırlarsınız.

1. Bir komut istemi açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Deponun yerel `pnp_app` kopyasının kökünde bir alt dizin oluşturun. Bu klasörü cihaz modeli dosyaları ve cihaz kodu saplaması için kullanırsınız.

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir pnp_app
    ```

## <a name="author-your-model"></a>Modelinizi yazma

Bu hızlı başlangıçta, var olan bir örnek cihaz yetenek modeli ve ilişkili arabirimleri kullanırsınız.

1. [Cihaz yetenek modeli](https://github.com/Azure/IoTPlugandPlay/blob/master/samples/SampleDevice.capabilitymodel.json) ve [arabirim örneğini](https://github.com/Azure/IoTPlugandPlay/blob/master/samples/EnvironmentalSensor.interface.json) indirin ve dosyaları `pnp_app` klasöre kaydedin.

    > [!TIP]
    > GitHub 'dan bir dosyayı indirmek için dosyasına gidin, **RAW**' a sağ tıklayın ve ardından **Bağlantıyı farklı kaydet**' i seçin.

1. Klasörü `pnp_app` vs Code açın. IntelliSense ile dosyaları görüntüleyebilirsiniz:

    ![Cihaz yetenek modeli](media/quickstart-create-pnp-device/dcm.png)

1. İndirdiğiniz dosyalarda, `<YOUR_COMPANY_NAME_HERE>` `@id` ve `schema` alanlarında öğesini benzersiz bir değerle değiştirin. Yalnızca a-z, A-Z, 0-9 ve alt çizgi karakterlerini kullanın. Daha fazla bilgi için bkz. [Digital ikizi Identifier Format](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format).

## <a name="generate-the-c-code-stub"></a>C kodu saplaması oluşturma

Artık bir DCM ve ilişkili arabirimlerinizin olduğu, modeli uygulayan Cihaz kodunu oluşturabilirsiniz. VS Code 'da C kodu saplaması oluşturmak için:

1. DCM dosyaları açık olan klasörde, **CTRL + SHIFT + P** tuşlarına basarak komut paletini açın, **IoT Tak ve kullan**girin ve **cihaz kodu saplama oluştur**' u seçin.

    > [!NOTE]
    > IoT Tak ve Kullan kod Oluşturucu yardımcı programını ilk kez kullandığınızda, indirmesi birkaç saniye sürer.

1. Cihaz kodu saplaması oluşturmak için kullanmak istediğiniz DCM dosyasını seçin.

1. **Sample_device**proje adını girin, cihaz uygulamanızın adı olacaktır.

1. Diliniz olarak **ANSI C** 'yi seçin.

1. Proje türü olarak **CMake projesini** seçin.

1. Bağlantı yöntemi olarak **IoT Hub cihaz bağlantı dizesi aracılığıyla** seçim yapın.

1. VS Code, oluşturulan cihaz kodu saplama dosyaları ile yeni bir pencere açar.
    ![Cihaz kodu](media/quickstart-create-pnp-device/device-code.png)

## <a name="build-the-code"></a>Kodu oluşturma

Oluşturulan cihaz kodu saplaması oluşturmak için cihaz SDK 'sını kullanın. Oluşturduğunuz uygulama, IoT Hub 'ına bağlanan bir cihaza benzetir. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. VS Code ' de, `CMakeLists.txt` cihaz SDK 'sı kök klasöründe açın.

1. Derleme sırasında cihaz kodu saplama klasörünü dahil etmek için `CMakeLists.txt` dosyanın altına aşağıdaki satırı ekleyin:

    ```txt
    add_subdirectory(pnp_app/sample_device)
    ```

1. Cihaz SDK 'Sı kök klasöründe bir CMake alt dizini oluşturun ve bu klasöre gidin:

    ```cmd\sh
    mkdir cmake
    cd cmake
    ```

1. Cihaz SDK 'sını ve üretilen kod Saplaması oluşturmak için aşağıdaki komutları çalıştırın:

    ```cmd\sh
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > CMake derleyicinizi C++ bulamazsa, önceki komutu çalıştırdığınızda derleme hataları alırsınız. Bu durumda, [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)bu komutu çalıştırmayı deneyin.

1. Oluşturma işlemi başarıyla tamamlandıktan sonra, IoT Hub cihaz bağlantı dizesini parametre olarak geçirerek uygulamanızı çalıştırın.

    ```cmd\sh
    cd azure-iot-sdk-c\cmake\pnp_app\sample_device\Release\
    sample_device.exe "[IoT Hub device connection string]"
    ```

1. Cihaz uygulaması IoT Hub veri göndermeye başlar.

    ![Cihaz uygulaması çalışıyor](media/quickstart-create-pnp-device/device-app-running.png)

## <a name="validate-the-code"></a>Kodu doğrulama

### <a name="publish-device-model-files-to-model-repository"></a>Cihaz modeli dosyalarını model deposuna Yayımla

**Azure IoT Explorer**ile cihaz kodunu doğrulamak için dosyaları model deposuna yayımlamanız gerekir.

1. DCM dosyaları açık olan klasörde, **CTRL + SHIFT + P** tuşlarına basarak komut paletini açın, yazıp IoT eklentisi & oynat ' **ı seçin: Dosyaları model deposuna**gönderme.

1. `SampleDevice.capabilitymodel.json` Ve`EnvironmentalSensor.interface.json` dosyalarını seçin.

1. Şirket modeli deposu Bağlantı dizenizi girin.

    > [!NOTE]
    > Bağlantı dizesi yalnızca depoya ilk kez bağlandığınızda gereklidir.

1. VS Code çıkış penceresinde ve bildiriminde, dosyaların başarıyla yayımlandığını kontrol edebilirsiniz.

    > [!NOTE]
    > Cihaz modeli dosyalarını yayımlarken hata alırsanız, komut **IoT Tak ve kullan kullanmayı deneyebilirsiniz: Oturumu kapatmak için model** deposunu kapatın ve adımları yeniden deneyin.

### <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanın

1. Azure IoT Gezginini açın, **uygulama yapılandırma** sayfasını görürsünüz.

1. IoT Hub bağlantı dizenizi girin ve **Bağlan**' a tıklayın.

1. Bağlandıktan sonra, cihaza genel bakış sayfasını görürsünüz.

1. Şirket deponuzu eklemek için **Ayarlar**' ı ve sonra **+ Yeni**' yi ve ardından **Şirket deposu**' nu seçin.

1. Şirket modeli deposu Bağlantı dizenizi ekleyin. **Bağlan**’ı seçin.

1. Cihaza Genel Bakış sayfasında, daha önce oluşturduğunuz cihaz kimliğini bulun ve daha fazla ayrıntı görüntülemek için seçin.

1. IoT Tak ve Kullan temel özellikler, komutlar ve Telemetriyi görmek için, KIMLIK **urn: azureiot: Environmentalalgılayıcı: 1** olan arabirimi genişletin.

1. Cihazın gönderdiği telemetri verilerini görüntülemek için **telemetri** sayfasını seçin.

1. Cihaz tarafından raporlanan yazılabilir olmayan özellikleri görüntülemek için **Özellikler (yazılabilir değil)** sayfasını seçin.

1. Güncelleştirebilir yazılabilir özellikleri görüntülemek için **Özellikler (yazılabilir)** sayfasını seçin.

1. Özellik **adı**' nı genişletin, yeni bir adla güncelleştirin ve **yazılabilir özelliği Güncelleştir**' i seçin. 
2. Yeni adın **bildirilen özellik** sütununda görünür olduğunu görmek için sayfanın üst kısmındaki **Yenile** düğmesine tıklayın.

1. Cihazın desteklediği tüm komutları görüntülemek için **komut** sayfasını seçin.

1. **Yanıp sönen** komutunu genişletin ve yeni bir yanıp sönen zaman aralığı ayarlayın. Cihazda komutu çağırmak için **Gönder komutunu** seçin.

1. Komutun beklenen şekilde yürütüldüğünü doğrulamak için sanal cihaza gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, DCM kullanarak IoT Tak ve Kullan cihazı oluşturmayı öğrendiniz.

IoT Tak ve Kullan hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [Visual Studio Code kullanarak cihaz yetenek modeli oluşturma ve test etme](tutorial-pnp-visual-studio-code.md)
