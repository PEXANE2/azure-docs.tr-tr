---
title: IoT Tak ve Çalıştır Önizleme cihazları oluşturmak için Visual Studio ve Visual Studio Code'u kullanın | Microsoft Dokümanlar
description: IoT Tak ve Çalıştır aygıt modellerinin yazılmasıve cihaz kodunun uygulanmasını hızlandırmak için Visual Studio ve Visual Studio Code'u kullanın.
author: liydu
ms.author: liydu
ms.date: 12/26/2019
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 91e7b1c0be9a38c3d79440f07d944d182980dc10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159243"
---
# <a name="use-visual-studio-and-visual-studio-code-to-build-iot-plug-and-play-devices"></a>IoT Tak ve Çalıştır aygıtları oluşturmak için Visual Studio ve Visual Studio Code'u kullanın

Visual Studio Code için Azure IoT Tools, aygıt yetenek modellerine (DCM) ve arabirimleri yazar, depoları modellemek için yayımlamak ve aygıt uygulamasını uygulamak için iskelet C kodu oluşturmak için entegre bir ortam sağlar.

Bu makale, şunları nasıl yapacağınızı gösterir:

- Aygıt kodu ve uygulama projesi oluşturun.
- Aygıt projenizde oluşturulan kodu kullanın.
- İskelet kodunu yenileyerek tekrarlayın.

IoT aygıtları geliştirmek için VS Kodunu kullanma [https://github.com/microsoft/vscode-iot-workbench](https://github.com/microsoft/vscode-iot-workbench)hakkında daha fazla bilgi edinmek için bkz.

## <a name="prerequisites"></a>Ön koşullar

[Visual Studio Kodunu](https://code.visualstudio.com/)Yükleyin.

Uzantısı paketini VS Kodu'na yüklemek için aşağıdaki adımları kullanın.

1. VS Kodu'nda **Uzantılar** sekmesini seçin.
1. **Azure IoT Araçlarını** pazardan arayın ve yükleyin.

## <a name="generate-device-code-and-project"></a>Aygıt kodu ve proje oluşturma

VS Code'da komut paletini açmak için **Ctrl+Shift+P** kullanın, **IoT Tak ve Çalıştır**girin ve iskelet kodunu ve proje türünü yapılandırmak için **Aygıt Kodu Saplaması Oluştur'u** seçin. Aşağıdaki liste her adımı ayrıntılı olarak açıklar:

- **Kodu oluşturmak için kullanılacak DCM dosyası.** İskelet aygıt kodunu oluşturmak için, DCM ve uyguladığı arabirim dosyalarını içeren klasörü açın. Kod üreteci DCM'nin gerektirdiği bir arabirimi bulamazsa, gerektiğinde model deposundan karşıdan yüklenir.

- **Aygıt kodu dili**. Şu anda, kod jeneratörü sadece ANSI C destekler.

- **Proje adı**. Proje adı, oluşturulan kod ve diğer proje dosyalarının klasör adı olarak kullanılır. Klasör, varsayılan olarak DCM dosyasının yanına yerleştirilir. DCM'nizi her güncellediğinizde ve aygıt kodunu yeniden oluşturduğunuzda oluşturulan kod klasörünü el ile kopyalamak zorunda kalmamak için DCM dosyanızı proje klasörüyle aynı klasörde tutun.

- **Azure IoT'ye bağlanma yöntemi.** Oluşturulan dosyalar, aygıtı Azure IoT Hub'ına bağlanacak şekilde yapılandırmak için kod da içerir. Doğrudan [Azure IoT](https://docs.microsoft.com/azure/iot-hub) Hub'ına bağlanmayı seçebilir veya [Aygıt Sağlama Hizmetini](https://docs.microsoft.com/azure/iot-dps)kullanabilirsiniz.

    - **Via IoT Hub aygıt bağlantı dizesi**: aygıt uygulamasının doğrudan IoT Hub'a bağlanması için aygıt bağlantı dizesini belirtin.
    - **DPS simetrik tuşu ile**: DPS kullanarak IoT Hub veya IoT Central'a bağlanmak için gereken cihaz uygulaması için **Kimlik Kapsamı,** **Simetrik Anahtar** ve Aygıt **Kimliği'ni** belirtin.

- **Proje türü.** Kod jeneratörü de bir CMake veya Arduino proje üretir. Şu anda desteklenen proje türleri şunlardır:

    - **Windows'da CMake Project**: Windows'da yapı sistemi olarak [CMake](https://cmake.org/) kullanan bir aygıt projesi için. Bu seçenek, `CMakeLists.txt` C koduyla aynı klasördeki aygıt SDK yapılandırmaları ile birlikte oluşturur.
    - **Linux'ta CMake Project**: Linux'ta yapı sistemi olarak [CMake](https://cmake.org/) kullanan bir cihaz projesi için. Bu seçenek, `CMakeLists.txt` C koduyla aynı klasördeki aygıt SDK yapılandırmaları ile birlikte oluşturur.
    - **MXChip IoT DevKit projesi**: [MXChip IoT DevKit](https://aka.ms/iot-devkit) cihazı yla çalışan bir cihaz projesi için. Bu seçenek, bir IoT DevKit aygıtı oluşturmak ve çalıştırmak için [VS Kodu'nda](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) veya Arduino IDE'de kullanabileceğiniz bir Arduino projesi oluşturur.

- **Cihaz SDK türü**. CMake'i proje türü olarak seçerseniz, oluşturulan kodun Azure IoT C aygıtı SDK'yı aşağıdakilere nasıl dahil edeceğini yapılandırma `CMakeLists.txt`adımıdır:

    - **Via Source Code**: oluşturulan kod, [sdk kaynak kodunun](https://github.com/Azure/azure-iot-sdk-c) dahil edilmesine ve birlikte oluşturulmasına dayanır. Bu, aygıt SDK kaynak kodunu özelleştirdiğinizde önerilir.
    - **Via Vcpkg**: oluşturulan kod, [SDK Vcpkg cihazının](https://github.com/microsoft/vcpkg/tree/master/ports/azure-iot-sdk-c) dahil edilmesine ve birlikte oluşturulmasına dayanır. Windows, Linux veya macOS çalıştıran aygıtlar için önerilen yol budur.

    > [!NOTE]
    > Azure IoT C cihazı SDK Vcpkg için macOS desteği devam ediyor.

Kod üreteci, yerel klasörde bulunan DCM ve arabirim dosyalarını kullanmaya çalışır. Arabirim dosyaları yerel klasörde değilse, kod üreteci bunları ortak model deposunda veya şirket modeli deposunda arar. [Ortak arabirim dosyaları](./concepts-common-interfaces.md) ortak model deposunda depolanır.

Kod oluşturma bittikten sonra, uzantı kodla birlikte yeni bir VS Kodu penceresi açar. **main.c**gibi oluşturulan bir dosyayı açarsanız, IntelliSense'in C SDK kaynak dosyalarını açamayabileceğini bildirdiğini görebilirsiniz. Doğru IntelliSense ve kod gezintisini etkinleştirmek için C SDK kaynağını eklemek için aşağıdaki adımları kullanın:

1. VS Code'da, komut paletini açmak için **Ctrl+Shift+P'yi** kullanın, **c_cpp_properties.json** dosyasını açmak için **C/C++: Düzenleme Yapılandırmaları (JSON)** yazın ve seçin.

1. Aygıt SDK'nın yolunu bölüme `includePath` ekleyin:

    ```json
    "includePath": [
        "${workspaceFolder}/**",
        "{path_of_device_c_sdk}/**"
    ]
    ```

1. Dosyayı kaydedin.

## <a name="use-the-generated-code"></a>Oluşturulan kodu kullanma

Aşağıdaki yönergeler, kendi aygıt projenizde oluşturulan kodun farklı geliştirme makinesi platformlarında nasıl kullanılacağını açıklar. Yönergeler, oluşturulan kodla birlikte bir IoT Hub aygıt bağlantı dizesini kullandığınızı varsayar:

### <a name="linux"></a>Linux

Ubuntu veya Debian gibi bir Linux ortamında CMake kullanarak C SDK Vcpkg cihazı ile birlikte aygıt kodunu oluşturmak için:

1. Terminal uygulamasını açın.

1. Komutu kullanarak **GCC,** **Git,** `cmake`ve `apt-get` tüm bağımlılıkları yükleyin:

    ```bash
    sudo apt-get update
    sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
    ```

    `cmake` **2.8.12'nin** üzerinde ve **GCC'nin** sürümünün **4.4.7'nin**üzerinde olduğunu doğrulayın.

    ```bash
    cmake --version
    gcc --version
    ```

1. Vcpkg yükleyin:

    ```bash
    git clone https://github.com/Microsoft/vcpkg.git
    cd vcpkg

    ./bootstrap-vcpkg.sh
    ```

    Daha sonra, kullanıcı çapında [tümleştirme](https://github.com/microsoft/vcpkg/blob/master/docs/users/integration.md)bağlamak için çalıştırın:

    ```bash
    ./vcpkg integrate install
    ```

1. Azure IoT C cihazı SDK Vcpkg'ı yükleyin:

    ```bash
    ./vcpkg install azure-iot-sdk-c[public-preview,use_prov_client]
    ```

1. Klasörde `cmake` bir alt dizin oluşturun oluşturulan kod saplaması içerir ve bu klasöre gidin:

    ```bash
    mkdir cmake
    cd cmake
    ```

1. Aygıt SDK ve oluşturulan kod saplaması oluşturmak için CMake kullanmak için aşağıdaki komutları çalıştırın:

    ```bash
    cmake .. -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}/scripts/buildsystems/vcpkg.cmake"

    cmake --build .
    ```

1. Yapı başarılı olduktan sonra, IoT Hub aygıt bağlantı dizesini parametre olarak belirten uygulamayı çalıştırın.

    ```bash
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

### <a name="windows"></a>Windows

Komut satırındaki CMake ve Visual Studio C/C++ derleyicilerini kullanarak Windows'daki C SDK cihazıyla birlikte aygıt kodunu oluşturmak için [IoT Tak ve Hızlı Başlat'ı](./quickstart-create-pnp-device-windows.md)görün. Aşağıdaki adımlar, Visual Studio'da CMake projesi olarak C SDK Vcpkg cihazıyla birlikte aygıt kodunu nasıl oluşturabileceğinizi gösterir.

1. Vcpkg ile C için Azure IoT aygıtı SDK'yı yüklemek için [hızlı başlatma](https://docs.microsoft.com/azure/iot-pnp/quickstart-create-pnp-device-windows#prepare-the-development-environment) adımlarını izleyin.

1. [Visual Studio 2019'u (Topluluk, Profesyonel veya Kurumsal)](https://visualstudio.microsoft.com/downloads/) yükleyin - **NuGet paket yöneticisi** bileşenini ve C++ iş **yüküyle Masaüstü Geliştirme'yi** eklediğinizden emin olun.

1. Visual Studio'yu açın, Dosya > Açık `CMakeLists.txt` > **CMake...** klasörünü açmak için oluşturulan kodu seçin.

1. **Genel** araç çubuğunda, **Configurations** açılır düşüşünü bulun. Projeniz için CMake ayarını eklemek için **Yapılandırmayı Yönet'i** seçin.

    ![Yapılandırmayı yönetme](media/howto-develop-with-vs-vscode/vs-manage-config.png)

1. **CMake Ayarları'nda**yeni bir yapılandırma ekleyin ve hedef olarak **x86-Hata Ayıklama'yı** seçin.

1. **CMake Komut Bağımsız Değişkenler,** aşağıdaki satırı ekleyin:

    ```txt
    -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    ```

1. Dosyayı kaydedin.

1. **Configurations** açılır geçişinde **x86-Hata** Ayıklama'ya geçin. CMake bunun için önbellek oluşturmak için birkaç saniye gerekir. İlerlemeyi görmek için Çıktı penceresini görüntüleyin.

    ![CMake Çıktı](media/howto-develop-with-vs-vscode/vs-cmake-output.png)

1. Çözüm **Gezgini'nde,** kök `CMakeLists.txt` klasöründeki negerektin sağ tıklatın ve Aygıt SDK ile oluşturulan kod saplaması oluşturmak için bağlam menüsünden **Oluştur'u** seçin.

1. Yapı başarılı olduktan sonra, komut isteminde, ioT Hub aygıt bağlantı dizesini parametre olarak belirten uygulamayı çalıştırın.

    ```cmd
    .\out\build\x86-Debug\{generated_code_project_name}.exe "[IoT Hub device connection string]"
    ```

> [!TIP]
> Visual Studio'da CMake'i kullanma hakkında daha fazla bilgi edinmek için [Bkz.](https://docs.microsoft.com/cpp/build/cmake-projects-in-visual-studio?view=vs-2019#building-cmake-projects)

### <a name="macos"></a>macOS

Aşağıdaki adımlar, CMake kullanarak macOS'taki C SDK kaynak koduyla birlikte aygıt kodunu nasıl oluşturabileceğinizi gösterir:

1. Terminal uygulamasını açın.

1. Tüm bağımlılıkları yüklemek için [Homebrew'i](https://homebrew.sh) kullanın:

    ```bash
    brew update
    brew install git cmake pkgconfig openssl ossp-uuid
    ```

1. [CMake](https://cmake.org/) en az sürüm **2.8.12**olduğunu doğrulayın:

    ```bash
    cmake --version
    ```

1. [Patch CURL](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#upgrade-curl-on-mac-os) en son sürümü kullanılabilir.

1. Oluşturulan kodu içeren klasörde Azure [IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) deposunu kopyalayın:

    ```bash
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Oluşturulan kodu `cmake` içeren klasörün altında çağrılan bir klasör oluşturun ve bu klasöre gidin.

    ```bash
    mkdir cmake
    cd cmake
    ```

1. Aygıt SDK ve oluşturulan kod saplaması oluşturmak için CMake kullanmak için aşağıdaki komutları çalıştırın:

    ```bash
    cmake -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DOPENSSL_ROOT_DIR:PATH=/usr/local/opt/openssl ..
    cmake --build .
    ```

1. Yapı başarılı olduktan sonra, IoT Hub aygıt bağlantı dizesini parametre olarak belirten uygulamayı çalıştırın.

    ```bash
    cd {generated_code_folder_name}/cmake/
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

## <a name="iterate-by-regenerating-the-skeleton-code"></a>İskelet kodunu yenileyerek iterate

DCM veya arabirim dosyalarınızı güncellerseniz kod oluşturucu kodu yeniden oluşturabilir. Kodu yeniden oluşturmak için aygıt kodunuzu bir DCM dosyasından zaten oluşturduğunuzu varsayarsak:

1. DCM dosyaları açık olan klasör açıkken, komut paletini açmak için **Ctrl+Shift+P'yi** kullanın, **IoT Tak ve Çalıştır'ı**girin ve **Aygıt Kodu Saplaması Oluştur'u**seçin.

1. Güncellediğiniz DCM dosyasını seçin.

1. **{proje adı} için Yeniden Kodu Oluştur'u**seçin.

1. Kod oluşturucu, yapılandırdığınız önceki ayarı kullanır ve kodu yeniden oluşturur. Ancak, gibi `main.c` kullanıcı kodu ve `{project_name}_impl.c`.

> [!NOTE]
> Arayüz dosyanızdaki URN kimliğini güncellerseniz, yeni bir arabirim olarak kabul edilir. Kodu yeniden oluşturduğunuzda, kod oluşturucu arabirim için kod oluşturur, ancak `{project_name}_impl.c` dosyadaki orijinalin üzerine yazmaz.

## <a name="problems-and-feedback"></a>Sorunlar ve Geri Bildirim

Azure IoT Tools, GitHub'da açık kaynaklı bir projedir. Herhangi bir sorun ve özellik istekleri [için, GitHub'da bir sorun oluşturabilirsiniz.](https://github.com/microsoft/vscode-azure-iot-tools/issues/new)

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılsa makalede, aygıt uygulamasını uygulamak için iskelet C kodu oluşturmak için Visual Studio ve Visual Studio Code'u nasıl kullanacağınızı öğrendiniz. Önerilen bir sonraki adım, [Azure IoT explorer](./howto-install-iot-explorer.md) aracını nasıl yükleyip kullanacağınızı öğrenmektir.
