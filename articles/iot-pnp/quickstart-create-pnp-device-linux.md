---
title: Azure IoT Tak ve Çalıştır Önizleme aygıtı (Linux) oluşturma | Microsoft Dokümanlar
description: Aygıt kodu oluşturmak için bir aygıt yeteneği modeli kullanın. Ardından aygıt kodunu çalıştırın ve aygıtın IoT Hub'ınıza bağlandığını görün.
author: dominicbetts
ms.author: dobett
ms.date: 12/27/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: d2cc440572d6f33480972c15f5c498cc384cb2e3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75550545"
---
# <a name="quickstart-use-a-device-capability-model-to-create-an-iot-plug-and-play-preview-device-linux"></a>Quickstart: Bir IoT Tak ve Çalıştır Önizleme aygıtı (Linux) oluşturmak için bir aygıt yeteneği modelini kullanın

[!INCLUDE [iot-pnp-quickstarts-1-selector.md](../../includes/iot-pnp-quickstarts-1-selector.md)]

_Aygıt özelliği modeli_ (DCM), bir IoT Tak ve Çalıştır aygıtının özelliklerini açıklar. Bir DCM genellikle bir ürün SKU ile ilişkilidir. DCM'de tanımlanan özellikler yeniden kullanılabilir arabirimler halinde düzenlenir. Bir DCM'den iskelet aygıt kodu oluşturabilirsiniz. Bu hızlı başlangıç, Bir DCM kullanarak bir IoT Tak ve Çalıştır aygıtı oluşturmak için Ubuntu Linux'ta VS Code'un nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatma, Ubuntu Linux'u bir masaüstü ortamıyla kullandığınızı varsayar. Bu öğreticideki adımlar Ubuntu 18.04 kullanılarak test edilmiştir.

Bu hızlı başlatmayı tamamlamak için, yerel Linux makinenize aşağıdaki yazılımı yüklemeniz gerekir:

* **Install GCC**, **Git**, **cmake**, `apt-get` ve tüm bağımlılıkları komutunu kullanarak:

    ```sh
    sudo apt-get update
    sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
    ```

    `cmake` **2.8.12'nin** üzerinde ve **GCC'nin** sürümünün **4.4.7'nin**üzerinde olduğunu doğrulayın.

    ```sh
    cmake --version
    gcc --version
    ```

* [Görsel Stüdyo Kodu](https://code.visualstudio.com/).

### <a name="install-azure-iot-tools"></a>Azure IoT Araçlarını Yükleme

VS Kodu uzantısı paketi [için Azure IoT Araçlarını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) yüklemek için aşağıdaki adımları kullanın:

1. VS Kodu'nda **Uzantılar** sekmesini seçin.
1. Azure **IoT Araçlarını**arayın.
1. **Yükle**’yi seçin.

### <a name="get-the-connection-string-for-your-company-model-repository"></a>Şirket model deponuz için bağlantı dizesini alın

Bir Microsoft çalışması veya okul hesabıyla oturum açtığınızda Azure [Sertifikalı IoT portalı portalında](https://preview.catalog.azureiotsolutions.com) veya varsa Microsoft İş Ortağı Kimliğinizde _şirket modeli depo bağlantı dizenizi_ bulabilirsiniz. Oturum açmadan sonra **Şirket deposunu** ve ardından **Bağlantı dizelerini**seçin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıçta, Geliştirme ortamınıza Azure IoT C aygıtı SDK'yı yüklemek için [Vcpkg](https://github.com/microsoft/vcpkg) kitaplık yöneticisini kullanırsınız.

Bir kabuk açın. Vcpkg yüklemek için aşağıdaki komutu çalıştırın:

```bash
cd ~
git clone https://github.com/microsoft/vcpkg
cd vcpkg
./bootstrap-vcpkg.sh
./vcpkg install azure-iot-sdk-c[public-preview,use_prov_client]
```

Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

## <a name="author-your-model"></a>Modelinizi yazar

Bu hızlı başlatmada, varolan bir örnek aygıt yetenek modeli ve ilişkili arabirimleri kullanırsınız.

1. Yerel `pnp_app` sürücünüzde bir dizin oluşturun. Bu klasörü aygıt modeli dosyaları ve aygıt kodu saplaması için kullanırsınız.

    ```bash
    cd ~
    mkdir pnp_app
    ```

1. Aygıt yetenek modelini ve arayüz `pnp_app` örnek dosyalarını klasöre indirin.

    ```bash
    cd pnp_app
    curl -O -L https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/SampleDevice.capabilitymodel.json
    curl -O -L https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensor.interface.json
    ```

1. VS `pnp_app` Kodu ile klasörü açın. Dosyaları IntelliSense ile görüntüleyebilirsiniz:

    ![Cihaz yetenek modeli](media/quickstart-create-pnp-device-linux/dcm.png)

1. İndirdiğiniz dosyalarda, benzersiz `<YOUR_COMPANY_NAME_HERE>` bir `@id` `schema` değere sahip alanları ve alanları değiştirin. Yalnızca a-z, A-Z, 0-9 karakterlerini kullanın ve altını çizin. Daha fazla bilgi için [Bkz. Dijital İkiz tanımlayıcı biçimi.](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format)

## <a name="generate-the-c-code-stub"></a>C kodu saplamasını oluşturma

Artık bir DCM ve ilişkili arabirimleriniz olduğuna göre, modeli uygulayan aygıt kodunu oluşturabilirsiniz. VS Kodu'nda C kodu saplaması oluşturmak için:

1. VS `pnp_app` Kodu'nda klasör açıkken, komut paletini açmak için **Ctrl+Shift+P'yi** kullanın, **IoT Tak ve Çalıştır'ı**girin ve **Aygıt Kodu Saplama**oluştur'u seçin.

    > [!NOTE]
    > IoT Tak ve Çalıştır Kodu Jeneratör yardımcı programını ilk kez kullandığınızda, otomatik olarak indirip yüklemek birkaç saniye sürer.

1. Aygıt kodu saplaması oluşturmak için kullanılacak **SampleDevice.capabilitymodel.json** dosyasını seçin.

1. Proje adını **sample_device**girin. Bu, aygıt uygulamanızın adı olacaktır.

1. Diliniz olarak **ANSI C'yi** seçin.

1. Bağlantı yöntemi olarak **Via IoT Hub aygıt bağlantı dizesini** seçin.

1. Proje şablonu olarak **Linux'ta CMake Project'i** seçin.

1. SDK cihazını eklemenin yolu olarak **Via Vcpkg'ı** seçin.

1. **sample_device** adlı yeni bir klasör DCM dosyasıyla aynı konumda oluşturulur ve içinde oluşturulan aygıt kodu saplama dosyaları bulunur. VS Code bunları görüntülemek için yeni bir pencere açar.
    ![Cihaz kodu](media/quickstart-create-pnp-device-linux/device-code.png)

## <a name="build-and-run-the-code"></a>Kodu oluşturma ve çalıştırma

Oluşturulan aygıt kodu saplaması oluşturmak için aygıt SDK kaynak kodunu kullanırsınız. Oluşturduğunuz uygulama, bir IoT hub'ına bağlanan bir aygıtı simüle eder. Uygulama telemetri ve özellikleri gönderir ve komutları alır.

1. **sample_device** uygulaması için bir **CMake** yapı klasörü oluşturun:

    ```bash
    cd ~/pnp_app/sample_device
    mkdir cmake
    cd cmake
    ```

1. Uygulamanızı SDK ile oluşturmak için CMake'i çalıştırın. Aşağıdaki komut, ev klasörünüze **vcpkg** yüklediğinizi varsayar:

    ```bash
    cmake .. -DCMAKE_TOOLCHAIN_FILE=~/vcpkg/scripts/buildsystems/vcpkg.cmake -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    cmake --build .
    ```

1. Yapı başarıyla tamamlandıktan sonra, uygulamanızı IoT hub aygıt bağlantı dizesini parametre olarak geçirerek çalıştırın.

    ```sh
    cd ~/pnp_app/sample_device/cmake
    ./sample_device "<YourDeviceConnectionString>"
    ```

1. Aygıt uygulaması IoT Hub'a veri göndermeye başlar.

    ![Cihaz uygulaması çalışıyor](media/quickstart-create-pnp-device-linux/device-app-running.png)

## <a name="validate-the-code"></a>Kodu doğrulama

### <a name="publish-device-model-files-to-model-repository"></a>Aygıt modeli dosyalarını model deposuna yayımlama

Aygıt kodunu **az** CLI ile doğrulamak için dosyaları model deposuna yayımlamanız gerekir.

1. `pnp_app` KLASÖR VS kodunda açıkken, komut paletini açmak için **Ctrl+Shift+P** kullanın, IoT Fiş & Play'i yazın ve **seçin: Dosyaları Model Deposu'na gönderin.**

1. Seçin `SampleDevice.capabilitymodel.json` `EnvironmentalSensor.interface.json` ve dosyaları.

1. Şirket model depo bağlantı dizenizi girin.

    > [!NOTE]
    > Bağlantı dizesi yalnızca depoya ilk bağlandığınızda gereklidir.

1. VS Kodu çıkış penceresinde ve bildirimde, dosyaların başarıyla yayımlandığını denetleyebilirsiniz.

    > [!NOTE]
    > Aygıt modeli dosyalarını yayımlamada hata lar alırsanız, komut **IoT Tak ve Çalıştır:** Oturum açma ve adımları tekrar gözden geçirmek için Model Deposu'nu oturum alabilen komutunu kullanmayı deneyebilirsiniz.

### <a name="use-the-azure-iot-cli-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT CLI'yi kullanın

Aygıt istemcisi örneği başladıktan sonra Azure CLI ile çalışıp çalışmadığını kontrol edebilirsiniz.

Örnek aygıtın gönderdiği telemetriyi görüntülemek için aşağıdaki komutu kullanın. Çıktıda herhangi bir telemetri görmeden önce bir veya iki dakika beklemeniz gerekebilir:

```azurecli-interactive
az iot dt monitor-events --hub-name <YourIoTHubNme> --device-id <YourDeviceID>
```

Aygıt tarafından gönderilen tüm özellikleri görüntülemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az iot dt list-properties --device-id <YourDeviceID> --hub-name <YourIoTHubNme> --source private --repo-login "<YourCompanyModelRepositoryConnectionString>"
```

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, Bir DCM kullanarak bir IoT Tak ve Çalıştır aygıtı oluşturmayı öğrendiniz.

DCM'ler ve kendi modellerinizi nasıl oluşturabilirsiniz hakkında daha fazla bilgi edinmek için öğreticiye devam edin:

> [!div class="nextstepaction"]
> [Öğretici: Visual Studio Code kullanarak bir aygıt yetenek modeli oluşturma ve test etme](tutorial-pnp-visual-studio-code.md)
