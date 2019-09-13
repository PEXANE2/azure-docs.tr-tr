---
title: IoT Tak ve Kullan önizleme aygıtları oluşturmak için Visual Studio ve Visual Studio Code kullanma | Microsoft Docs
description: IoT Tak ve Kullan cihaz modellerini yazmayı ve cihaz kodunu uygulamayı hızlandırmak için Visual Studio ve Visual Studio Code kullanın.
author: liydu
ms.author: liydu
ms.date: 09/10/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: d68a3f99096ca64b357239f61cf7ff17d6fc3f83
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935735"
---
# <a name="use-azure-iot-tools-for-visual-studio-code"></a>Visual Studio Code için Azure IoT araçları 'nı kullanma

Visual Studio Code için Azure IoT araçları, cihaz yeteneği modellerini (DCM) ve arabirimlerini yazmak, model depolarında yayımlamak ve cihaz uygulamasını uygulamak için iskelet C kodu oluşturmak üzere tümleşik bir ortam sağlar.

Bu makalede nasıl yapılacağı gösterilmektedir:

- Cihaz kodu ve uygulama projesi oluşturun.
- Cihaz projenizde oluşturulan kodu kullanın.
- İskelet kodunu yeniden oluşturarak yineleyin.

IoT cihazları geliştirmek için VS Code kullanma hakkında daha fazla bilgi için bkz [https://github.com/microsoft/vscode-iot-workbench](https://github.com/microsoft/vscode-iot-workbench).

## <a name="prerequisites"></a>Önkoşullar

[Visual Studio Code](https://code.visualstudio.com/)'i yükler.

VS Code ' ye uzantı paketini yüklemek için aşağıdaki adımları kullanın.

1. VS Code, **Uzantılar** sekmesini seçin.
1. Market 'ten **Azure IoT araçları** 'nı arayın ve bunları yükler.

## <a name="generate-device-code-and-project"></a>Cihaz kodu ve proje oluştur

VS Code ' de, **Ctrl + Shift + P** tuşlarına basarak komut paletini açın, **IoT Tak ve kullan**girin ve iskelet kodunu ve proje türünü yapılandırmak Için **cihaz kodu saplama oluştur** ' u seçin. Aşağıdaki listede her bir adım ayrıntılı açıklanmaktadır:

- **Kodu oluşturmak için kullanılacak dcm dosyası**. İskelet Cihaz kodunu oluşturmak için, uyguladığı DCM ve arabirim dosyalarını içeren klasörü açın. Kod Oluşturucu bir DCM 'in gerektirdiği bir arabirim bulamazsa, gerektiğinde model deposundan indirir.

- **Cihaz kodu dili**. Şu anda kod Oluşturucu yalnızca ANSI C 'yi destekliyor.

- **Proje adı**. Proje adı, oluşturulan kod ve diğer proje dosyaları için klasör adı olarak kullanılır. Klasör, varsayılan olarak DCM dosyasının yanına yerleştirilir. DCM Dosyanızı güncelleştirdiğinizde ve cihaz kodunu yeniden oluşturduğunuzda oluşturulan kod klasörünü el ile kopyalamak zorunda kalmamak için, DCM Dosyanızı proje klasörüyle aynı klasörde saklayın.

- **Proje türü**. Kod Oluşturucu Ayrıca, kodu kendi projenizle veya cihaz SDK projesiyle tümleştirilebilen bir proje dosyası da oluşturur. Şu anda desteklenen proje türleri şunlardır:

    - **CMake projesi**: [CMake](https://cmake.org/) yapı sistemi olarak kullanan bir cihaz projesi için. Bu seçenek C koduyla `CMakeLists.txt` aynı klasörde bir dosya oluşturur.
    - **Mxyongaıot devkit projesi**: [Mxyonga IoT devkit](https://aka.ms/iot-devkit) cihazında çalışan bir cihaz projesi için. Bu seçenek, IoT DevKit cihazında derlemek ve çalıştırmak için [vs Code](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) veya ARDUINO IDE 'de kullanabileceğiniz bir Arduino projesi oluşturur.

- **Azure IoT 'ye bağlanma yöntemi**. Oluşturulan dosyalar Ayrıca, cihazı Azure IoT Hub bağlanacak şekilde yapılandırmak için kod içerir. Doğrudan [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) bağlanmayı veya [cihaz sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps)'ni kullanmayı seçebilirsiniz.

    - **IoT Hub cihaz bağlantı dizesi aracılığıyla**: cihaz uygulaması için doğrudan IoT Hub bağlanacak cihaz bağlantı dizesini belirtin.
    - **DPS simetrik anahtarı aracılığıyla**: IoT Hub veya DPS kullanarak IoT Central bağlanmak için gereken cihaz uygulaması IÇIN **Kapsam KIMLIĞI**, **kayıt kimliği**ve **SAS anahtarını** belirtin.

Kod Oluşturucu, yerel klasörde bulunan DCM ve arabirim dosyalarını kullanmayı dener. Arabirim dosyaları yerel klasörde değilse, kod Oluşturucu bunları ortak model deposunda veya şirket modeli deposunda arar. Ortak [arabirim dosyaları](./concepts-common-interfaces.md) ortak model deposunda depolanır.

Kod oluşturma işlemi tamamlandıktan sonra uzantı, kodla yeni bir VS Code penceresi açar. **Main. c**gibi oluşturulmuş bir dosyayı açarsanız, IntelliSense 'ın c SDK kaynak dosyalarını açmadığı hakkında bilgi edinebilirsiniz. Doğru IntelliSense ve kod gezintisini etkinleştirmek için, C SDK kaynağını eklemek için aşağıdaki adımları kullanın:

1. VS Code ' de, **CTRL + SHIFT + P** ' yi kullanarak komut paletini açın, yazın ve **şunu seçinC++: **C_cpp_properties. JSON** dosyasını açmak** için konfigürasyonları düzenleyin (JSON).

1. Cihaz SDK 'sının `includePath` yolunu bölümüne ekleyin:

    ```json
    "includePath": [
        "${workspaceFolder}/**",
        "{path_of_device_c_sdk}/**"
    ]
    ```

1. Dosyayı kaydedin.

## <a name="use-the-generated-code"></a>Oluşturulan kodu kullan

Aşağıdaki yönergelerde, farklı geliştirme makinesi platformlarında kendi cihaz projenizde oluşturulan kodun nasıl kullanılacağı açıklanır. Yönergeler, oluşturulan kodla IoT Hub bir cihaz bağlantı dizesi kullandığınızı varsayar:

### <a name="linux"></a>Linux

Ubuntu veya de, gibi bir Linux ortamında CMake kullanarak cihaz C SDK 'Sı ile birlikte cihaz kodu oluşturmak için:

1. Bir Terminal uygulaması açın.

1. Komutunu kullanarak `cmake`GCC, git, ve tüm bağımlılıkları yükler: `apt-get`

    ```sh
    sudo apt-get update
    sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
    ```

    Öğesinin `cmake` sürümünün **2.8.12** üzerinde olduğunu ve **GCC** 'nin sürümünün **4.4.7**'in üzerinde olduğunu doğrulayın.

    ```sh
    cmake --version
    gcc --version
    ```

1. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) deposunu kopyalayın:

    ```sh
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Oluşturulan kodu içeren klasörü cihaz SDK 'Sı kök klasörüne kopyalayın.

1. VS Code, `CMakeLists.txt` dosyayı cihaz SDK 'sı kök klasöründe açın.

1. SDK 'yı derlerken cihaz kodu saplama klasörünü dahil etmek `CMakeLists.txt` için dosyanın sonuna aşağıdaki satırı ekleyin:

    ```txt
    add_subdirectory({generated_code_folder_name})
    ```

1. Cihaz SDK 'sı kök `cmake` klasöründe adlı bir klasör oluşturun ve bu klasöre gidin.

    ```sh
    mkdir cmake
    cd cmake
    ```

1. Aşağıdaki komutları çalıştırarak, CMake 'i kullanarak cihaz SDK 'sını ve oluşturulan kod Saplaması 'nı oluşturun:

    ```cmd\sh
    cmake -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON ..
    cmake --build .
    ```

1. Yapı başarılı olduktan sonra, IoT Hub cihaz bağlantı dizesini parametre olarak belirterek uygulamayı çalıştırın.

    ```cmd\sh
    cd azure-iot-sdk-c/cmake/{generated_code_folder_name}/
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

### <a name="windows"></a>Windows

Komut satırında CMake ve Visual Studio C/C++ derleyicileri kullanarak Windows 'Daki DEVICE c SDK ile birlikte cihaz kodunu derlemek için bkz. [IoT Tak ve kullan hızlı başlangıç](./quickstart-create-pnp-device.md). Aşağıdaki adımlarda, Visual Studio 'da CMake projesi olarak cihaz C SDK 'Sı ile birlikte cihaz kodunun nasıl oluşturulacağı gösterilmektedir.

1. [Visual Studio 2019 'yi (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) yükledikten sonra, iş yüküyle **NuGet Paket Yöneticisi** bileşenini ve **Masaüstü geliştirmeyi C++**  eklediğinizden emin olun.

1. Visual Studio 'yu açın ve **Başlarken** sayfasında **kopya veya**kullanıma alma kodu ' nu seçin:

1. **Depo konumu**' nda [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) deposunu kopyalayın:

    ```txt
    https://github.com/Azure/azure-iot-sdk-c
    ```

    Bu işlemin tamamlanması birkaç dakika sürer, ilerleme durumunu **Takım Gezgini** bölmesinde görebilirsiniz.

1. Takım Gezgini **Azure-IoT-SDK-c** deposunu açın, **dallar**' ı seçin, **genel önizleme** dalını arayın ve kontrol edin.

    ![Genel önizleme](media/howto-develop-with-vs-vscode/vs-public-preview.png)

1. Oluşturulan kodu içeren klasörü cihaz SDK 'Sı kök klasörüne kopyalayın.

1. `azure-iot-sdk-c` Klasörünü vs 'de açın.

1. `CMakeLists.txt` Dosyayı cihaz SDK 'sı kök klasöründe açın.

1. SDK 'yı derlerken cihaz kodu saplama klasörünü dahil etmek `CMakeLists.txt` için dosyanın sonuna aşağıdaki satırı ekleyin:

    ```txt
    add_subdirectory({generated_code_folder_name})
    ```

1. **Genel** araç çubuğunda, **Konfigürasyonlar** açılan listesini bulun. Projenizin CMake ayarını eklemek için **yapılandırmayı Yönet** ' i seçin.

    ![Yapılandırmayı Yönet](media/howto-develop-with-vs-vscode/vs-manage-config.png)

1. **CMake ayarlarında**yeni bir yapılandırma ekleyin ve **x64-Release** as Target ' ı seçin.

1. **CMake komut bağımsız değişkenlerinde**aşağıdaki satırı ekleyin:

    ```txt
    -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON
    ```

1. Dosyayı kaydedin.

1. **Çözüm Gezgini**, kök klasörde öğesine sağ tıklayın ve ardından `CMakeLists.txt` bağlam menüsünden **Oluştur** ' u seçerek cihaz SDK 'sı ve üretilen kod Saplaması oluşturun.

1. Oluşturma başarılı olduktan sonra komut isteminde, IoT Hub cihaz bağlantı dizesini parametre olarak belirten uygulamayı çalıştırın.

    ```cmd
    cd %USERPROFILE%\CMakeBuilds\{workspaceHash}\build\x64-Release\{generated_code_folder_name}\
    {generated_code_project_name}.exe "[IoT Hub device connection string]"
    ```

> [!TIP]
> Visual Studio 'da CMake 'i kullanma hakkında daha fazla bilgi için bkz. [CMake projesi oluşturma](https://docs.microsoft.com/cpp/build/cmake-projects-in-visual-studio?view=vs-2019#building-cmake-projects) .

### <a name="macos"></a>Mac OS

Aşağıdaki adımlarda, CMake kullanarak macOS 'ta Device C SDK 'Sı ile birlikte cihaz kodunun nasıl oluşturulacağı gösterilmektedir:

1. Terminal uygulamasını açın.

1. Tüm bağımlılıkları yüklemek için [homebrew](https://homebrew.sh) kullanın:

    ```bash
    brew update
    brew install git cmake pkgconfig openssl ossp-uuid
    ```

1. [CMake](https://cmake.org/) 'in en az sürüm **2.8.12**olduğunu doğrulayın:

    ```bash
    cmake --version
    ```

1. [Düzeltme Eki](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#upgrade-curl-on-mac-os) , kullanılabilir en son sürüme kıvır.

1. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) deposunu kopyalayın:

    ```bash
    git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

1. Oluşturulan kodu içeren klasörü cihaz SDK 'Sı kök klasörüne kopyalayın.

1. VS Code, `CMakeLists.txt` dosyayı cihaz SDK 'sı kök klasöründe açın.

1. SDK 'yı derlerken cihaz kodu saplama klasörünü dahil etmek `CMakeLists.txt` için dosyanın sonuna aşağıdaki satırı ekleyin:

    ```txt
    add_subdirectory({generated_code_folder_name})
    ```

1. Cihaz SDK 'sı kök `cmake` klasöründe adlı bir klasör oluşturun ve bu klasöre gidin.

    ```bash
    mkdir cmake
    cd cmake
    ```

1. Aşağıdaki komutları çalıştırarak, CMake 'i kullanarak cihaz SDK 'sını ve oluşturulan kod Saplaması 'nı oluşturun:

    ```bash
    cmake -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DOPENSSL_ROOT_DIR:PATH=/usr/local/opt/openssl ..
    cmake --build .
    ```

1. Yapı başarılı olduktan sonra, IoT Hub cihaz bağlantı dizesini parametre olarak belirterek uygulamayı çalıştırın.

    ```bash
    cd azure-iot-sdk-c/cmake/{generated_code_folder_name}/
    ./{generated_code_project_name} "[IoT Hub device connection string]"
    ```

## <a name="iterate-by-regenerating-the-skeleton-code"></a>İskelet kodunu yeniden oluşturarak yineleme

DCM veya arabirim dosyalarınızı güncelleştirirseniz kod Oluşturucu kodu yeniden oluşturabilir. Kodu yeniden oluşturmak için bir DCM dosyasındaki cihaz kodunuzu zaten oluşturmuş olduğunuz varsayılarak:

1. DCM dosyaları açık olan klasörde, **CTRL + SHIFT + P** tuşlarına basarak komut paletini açın, **IoT Tak ve kullan**girin ve **cihaz kodu saplama oluştur**' u seçin.

1. Güncelleştirdiğiniz DCM dosyasını seçin.

1. **{Project Name} için kodu yeniden oluştur**' u seçin.

1. Kod Oluşturucu yapılandırdığınız önceki ayarı kullanır ve kodu yeniden oluşturur. Ancak, `main.c` ve `{project_name}_impl.c`gibi Kullanıcı kodu içerebilen dosyaların üzerine yazmaz.

> [!NOTE]
> Arabirim dosyanızdaki URN kimliğini güncelleştirirseniz, yeni bir arabirim olarak kabul edilir. Kodu yeniden oluşturduğunuzda, kod Oluşturucu arabirim için kod üretir ancak `{project_name}_impl.c` dosyanın özgün olanın üzerine yazmaz.

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Azure IoT araçları, GitHub 'da açık kaynaklı bir projem. Herhangi bir sorun ve özellik isteği için GitHub ile ilgili [bir sorun oluşturabilirsiniz](https://github.com/microsoft/vscode-azure-iot-tools/issues/new).

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde, Visual Studio 'Yu kullanmayı ve Visual Studio Code cihaz uygulamasını uygulamak için iskelet C kodu oluşturmayı öğrendiniz. Önerilen bir sonraki adım, [Azure IoT gezgin aracını yüklemeyi ve kullanmayı](./howto-install-iot-explorer.md) öğrenirsiniz.
