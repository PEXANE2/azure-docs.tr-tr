---
title: Azure IoT Hub’a telemetri gönderme hızlı başlangıcı (C) | Microsoft Docs
description: Bu hızlı başlangıçta bir IoT hub’a sanal telemetri göndermek ve bulutta işlemek üzere IoT hub’dan gelen telemetriyi okumak için iki örnek C uygulaması çalıştırırsınız.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/10/2019
ms.author: wesmc
ms.openlocfilehash: cbd32be2caefc9b84c2db4255df298cfd0766c01
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241139"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-c"></a>Hızlı başlatma: Bir aygıttan bir IoT hub'ına telemetri gönderin ve arka uç uygulamasıyla (C) okuyun

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlangıçta, bir simülasyon cihazı uygulamasından bir arka uç uygulamasına işlenmek üzere IoT Hub aracılığıyla telemetri gönderirsiniz.

Bu hızlı başlangıçta, IoT hub’ına telemetri verileri göndermek için [C için Azure IoT cihaz SDK](iot-hub-device-sdk-c-intro.md)’sındaki bir C örnek uygulaması kullanılmaktadır. Azure IoT cihaz SDK’ları, taşınabilirlik ve geniş platform uyumluluğu için [ANSI C (C99)](https://wikipedia.org/wiki/C99) ile yazılır. Örnek kodu çalıştırmadan önce bir IoT hub’ı oluşturur ve simülasyon cihazını o hub’a kaydedersiniz.

Bu makale Windows için yazılmıştır, ancak Linux'ta da bu hızlı başlatmayı tamamlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019'u](https://www.visualstudio.com/vs/) ['C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yüküyle masaüstü geliştirme özelliğiyle yükleyin.

* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu hızlı başlatmadaki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.


* Bulut Kabuğu örneğinize Azure CLI için Microsoft Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IoT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS) özel komutlarını ekler.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıç için, C [için Azure IoT aygıtı SDK'yı](iot-hub-device-sdk-c-intro.md)kullanıyor olacaksınız. 

Aşağıdaki ortamlar için, bu paketleri ve kitaplıkları yükleyerek SDK'yı kullanabilirsiniz:

* **Linux**: apt-get paketleri Ubuntu 16.04 ve 18.04 için aşağıdaki CPU mimarileri kullanılarak kullanılabilir: amd64, arm64, armhf ve i386. Daha fazla bilgi için bkz. [Ubuntu’da C cihaz istemcisi oluşturmak için apt-get kullanma](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/ubuntu_apt-get_sample_setup.md).

* **mbed**: mbed platformunda cihaz uygulamaları oluşturan geliştiriciler için, birkaç dakika içinde witH Azure IoT Hub'ı başlatacak bir kitaplık ve örnekler yayınladık. Daha fazla bilgi için bkz. [Mbed kitaplığını kullanma](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#mbed).

* **Arduino**: Arduino'da geliştiriyorsanız, Arduino IDE kitaplık yöneticisinde bulunan Azure IoT kitaplığından yararlanabilirsiniz. Daha fazla bilgi için bkz. [Arduino için Azure IoT Hub kitaplığı](https://github.com/azure/azure-iot-arduino).

* **iOS**: IoT Hub Cihaz SDK’sı, Mac ve iOS cihaz geliştirmesi için CocoaPods olarak kullanılabilir. Daha fazla bilgi için bkz. [Microsoft Azure IoT için iOS Örnekleri](https://cocoapods.org/pods/AzureIoTHubClient).

Ancak, bu hızlı başlangıçta, GitHub'dan [Azure IoT C SDK'yı](https://github.com/Azure/azure-iot-sdk-c) klonlamak ve oluşturmak için kullanılan bir geliştirme ortamı hazırlasınız. GitHub üzerindeki SDK, bu hızlı başlangıçta yer alan örnek kodu içerir.

1. [CMake yapı sistemini](https://cmake.org/download/)indirin.

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. SDK'nın [en son sürümü](https://github.com/Azure/azure-iot-sdk-c/releases/latest) için etiket adını bulun.

3. Komut istemini veya Git Bash kabuğunu açın. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunun en son sürümünde klonlamak için aşağıdaki komutları çalıştırın. Önceki adımda bulduğunuz etiketi `-b` parametre nin değeri olarak kullanın:

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

4. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. Dizinden aşağıdaki komutları `azure-iot-sdk-c` çalıştırın:

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

5. Geliştirme istemciplatformunuza özel Bir SDK sürümünü oluşturmak için aşağıdaki komutu çalıştırın. `cmake` dizininde simülasyon cihazı için bir Visual Studio çözümü de oluşturulur.

    ```cmd
    cmake ..
    ```

    `cmake` C++ derleyicinizi bulamazsa, yukarıdaki komutu çalıştırırken yapı hataları alabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin. 

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```cmd/sh
    $ cmake ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, simüle edilmiş bir aygıtı kaydetmek için [IoT uzantılı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) Azure Bulut Kabuğu'nu kullanırsınız.

1. Aygıt kimliğini oluşturmak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın.

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

   **MyCDevice**: Bu, kaydettiğiniz aygıtın adıdır. Gösterildiği gibi **MyCDevice** kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanmanız ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirmeniz gerekir.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyCDevice
    ```

2. Kaydolduğunuz aygıtın _aygıt bağlantı dizesini_ almak için Azure Cloud Shell'de aşağıdaki komutu çalıştırın:

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyCDevice --output table
    ```

    Şu ifadeye benzer şekilde görünen cihaz bağlantı dizesini not edin:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyCDevice;SharedAccessKey={YourSharedAccessKey}`

    Bu değeri daha sonra hızlı bir şekilde kullanırsınız.

## <a name="send-simulated-telemetry"></a>Sanal telemetri gönderme

Simülasyon cihazı uygulaması, IoT hub’ınız üzerindeki cihaza özgü bir uç noktaya bağlanır ve sanal telemetri olarak bir dize gönderir.

1. Bir metin düzenleyicisi kullanarak iothub_convenience_sample.c kaynak dosyasını açın ve telemetri verileri göndermek için örnek kodu gözden geçirin. Dosya, Azure IoT C SDK'yı klonladığınız çalışma dizininin altında aşağıdaki konumda yer alır:

    ```
    azure-iot-sdk-c\iothub_client\samples\iothub_convenience_sample\iothub_convenience_sample.c
    ```

2. `connectionString` sabitinin bildirimini bulun:

    ```C
    /* Paste in your device connection string  */
    static const char* connectionString = "[device connection string]";
    ```

    `connectionString` Sabitin değerini daha önce not aldığınız aygıt bağlantı dizesiyle değiştirin. Ardından **iothub_convenience_sample.c** üzerindeki değişikliklerinizi kaydedin.

3. Yerel terminal penceresinde, Azure IoT C SDK’sında oluşturduğunuz CMake dizininde yer alan *iothub_convenience_sample* proje dizinine gidin. Çalışma dizininizden aşağıdaki komutu girin:

    ```cmd/sh
    cd azure-iot-sdk-c/cmake/iothub_client/samples/iothub_convenience_sample
    ```

4. CMake öğesini yerel terminal pencerenizde çalıştırarak örneği güncelleştirilmiş `connectionString` değeriyle derleyin:

    ```cmd/sh
    cmake --build . --target iothub_convenience_sample --config Debug
    ```

5. Yerel terminal pencerenizde, benzetilen aygıt uygulamasını çalıştırmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    Debug\iothub_convenience_sample.exe
    ```

    Aşağıdaki ekran görüntüsünde, simülasyon cihazı uygulaması, IoT hub’ınıza telemetri verilerini gönderdiğinde oluşan çıktı gösterilmektedir:

    ![Simülasyon cihazını çalıştırma](media/quickstart-send-telemetry-c/simulated-device-app.png)

## <a name="read-the-telemetry-from-your-hub"></a>Hub’ınızdan telemetri okuma

Bu bölümde, benzetilen aygıt tarafından gönderilen aygıt iletilerini izlemek için [IoT uzantılı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) Azure Bulut Kabuğu'nu kullanırsınız.

1. Azure Cloud Shell'i kullanarak, IoT hub’ınızdan gelen iletilere bağlanmak ve bu iletileri okumak için aşağıdaki komutu çalıştırın:

   **YourIoTHubName**: Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub monitor-events --hub-name {YourIoTHubName} --output table
    ```

    ![Azure CLI kullanarak cihaz iletilerini okuma](media/quickstart-send-telemetry-c/read-device-to-cloud-messages-app.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub'ı kurup bir aygıtı kaydettiniz, C uygulamasını kullanarak hub'a benzetilen telemetriyi gönderdiniz ve Azure Bulut Su şurunu kullanarak hub'dan telemetriyi okudunuz.

Azure IoT Hub C SDK’sı ile geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki Nasıl yapılır kılavuzuyla devam edin:

> [!div class="nextstepaction"]
> [Azure IoT Hub C SDK’sı kullanarak geliştirme](iot-hub-devguide-develop-for-constrained-devices.md)
