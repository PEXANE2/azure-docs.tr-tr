---
title: Azure IoT Hub aygıtı, SSH ve RDP için C hızlı başlat'ı akışı sağlar
description: Bu hızlı başlatmada, IoT Hub aygıt akışları üzerinden SSH ve RDP senaryolarını etkinleştirmek için proxy görevi gören örnek bir C uygulaması çalıştırın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: af303928490d2570fa9430bd4b9d30c4a49df304
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675485"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-c-proxy-application-preview"></a>Quickstart: C proxy uygulaması (önizleme) kullanarak Bir IoT Hub aygıt akışı üzerinden SSH ve RDP'yi etkinleştirme

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Azure IoT Hub şu anda bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)olarak aygıt akışlarını destekler.

[IoT Hub aygıt akışları,](./iot-hub-device-streams-overview.md) hizmet ve aygıt uygulamalarının güvenli ve güvenlik duvarı dostu bir şekilde iletişim kurmasına olanak tanır. Kurulumun genel görünümü için [Yerel Proxy Örneği sayfasına](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp)bakın.

Bu hızlı başlatma, Aygıt akışları üzerinden Secure Shell (SSH) trafiğini (bağlantı noktası 22 kullanarak) tünelleme kurulumlarını açıklar. Uzak Masaüstü Protokolü (RDP) trafiği için kurulum benzer ve basit bir yapılandırma değişikliği gerektirir. Aygıt akışları uygulama ve protokol agnostik olduğundan, bu hızlı başlatmayı diğer uygulama trafiği türlerini barındıracak şekilde değiştirebilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Aşağıdaki şekil, aygıt ve hizmet yerel proxy programlarının SSH istemcisi ile SSH daemon işlemleri arasında uçlardan uca bağlantıyı nasıl etkinleştirdüğünü göstermektedir. Genel önizleme sırasında C SDK yalnızca aygıt tarafındaki aygıt akışlarını destekler. Sonuç olarak, bu hızlı başlatma yalnızca aygıt yerel proxy uygulamasını çalıştırmak için yönergeleri kapsar. Eşlik eden servis tarafı uygulamasını oluşturmak ve çalıştırmak için aşağıdaki hızlı başlangıçlardan birinde yönergeleri izleyin:

* [C# proxy kullanarak IoT Hub cihaz akışları üzerinden SSH/RDP](./quickstart-device-streams-proxy-csharp.md)
* [NodeJS proxy kullanarak IoT Hub aygıt akışları üzerinden SSH/RDP.](./quickstart-device-streams-proxy-nodejs.md)

![Yerel proxy kurulumu](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. Hizmet yerel proxy IoT hub'ına bağlanır ve hedef aygıta bir aygıt akışı başlatır.

2. Aygıt yerel proxy akışı başlatma el sıkışma tamamlar ve hizmet tarafına IoT hub'ın akış bitiş noktası üzerinden uçtan uca akış tüneli kurar.

3. Aygıt yerel proxy, aygıttaki 22 bağlantı noktasından dinleyen SSH daemon'a bağlanır. Bu ayar, "Aygıt yerel proxy uygulamasını çalıştır" bölümünde açıklandığı gibi yapılandırılabilir.

4. Hizmet yerel proxy, bu durumda bağlantı noktası 2222 olan belirlenmiş bir bağlantı noktası üzerinde dinleyerek bir kullanıcıdan yeni SSH bağlantıları bekler. Bu ayar, "Aygıt yerel proxy uygulamasını çalıştır" bölümünde açıklandığı gibi yapılandırılabilir. Kullanıcı SSH istemcisi üzerinden bağlandığında, tünel SSH uygulama trafiğinin SSH istemcisi ile sunucu programları arasında aktarılmasını sağlar.

> [!NOTE]
> Aygıt akışı üzerinden gönderilen SSH trafiği, doğrudan servis ve aygıt arasında gönderilmek yerine IoT hub'ın akış bitiş noktasından tünellenir. Daha fazla bilgi [için, Iot Hub aygıt akışlarını kullanmanın yararlarına](iot-hub-device-streams-overview.md#benefits)bakın. Ayrıca, şekil, aygıt yerel proxy ile aynı aygıtta (veya makinede) çalışan SSH daemon'u göstermektedir. Bu hızlı başlatmada, SSH daemon IP adresinin sağlanması, aygıt-yerel proxy ve daemon'un farklı makinelerde de çalışmasını sağlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Aygıt akışlarının önizlemesi şu anda yalnızca aşağıdaki bölgelerde oluşturulan IoT hub'ları için desteklenir:

  * Orta ABD
  * ORTA ABD EUAP
  * Kuzey Avrupa
  * Güneydoğu Asya

* C++ iş yükü etkinleştirilmiş [Masaüstü geliştirme özelliğiyle](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) Visual Studio [2019'u](https://www.visualstudio.com/vs/) yükleyin.
* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

* Bulut Kabuğu örneğinize Azure CLI için Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IOT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS)'ye özel komutları ekler.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıç [için, C için Azure IoT aygıtı SDK'yı](iot-hub-device-sdk-c-intro.md)kullanırsınız. GitHub'dan [Azure IoT C SDK'yı](https://github.com/Azure/azure-iot-sdk-c) klonlamak ve oluşturmak için kullanılan bir geliştirme ortamı hazırlarsınız. GitHub'daki SDK, bu hızlı başlatmada kullanılan örnek kodu içerir.

1. [CMake yapı sistemini](https://cmake.org/download/)indirin.

    CMake yüklemesini başlatmadan *önce* Visual Studio ön koşullarının (Visual Studio ve C++ iş *yüküne sahip Masaüstü geliştirme)* makinenize yüklenmesi önemlidir. Ön koşullar yerleştirildikten ve karşıdan yükleme doğrulandıktan sonra CMake build sistemini yükleyebilirsiniz.

1. Komut istemini veya Git Bash kabuğunu açın. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu klonlamak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    git clone -b public-preview https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlem birkaç dakika sürer.

1. Git deposunun kök dizininde bir *cmake* alt dizini oluşturun ve bu klasöre gidin. *Azure-iot-sdk-c* dizininden aşağıdaki komutları çalıştırın:

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

1. Geliştirme istemci platformunuza özel Bir SDK sürümünü oluşturmak için *cmake* dizininden aşağıdaki komutları çalıştırın.

   * Linux'ta:

      ```bash
      cmake ..
      make -j
      ```

   * Windows'da, Visual Studio 2015 veya 2017 için Geliştirici Komut Komut Ustem komutu komutlarını çalıştırın. Cmake dizininde simüle edilen aygıt için *cmake* bir Visual Studio çözümü oluşturulacaktır.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Or for VS2019
      cmake .. -G "Visual Studio 16 2019"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Cihaz kaydetme

Bir cihazın bağlanabilmesi için IoT hub’ınıza kaydedilmesi gerekir. Bu bölümde, simüle edilmiş bir aygıtı kaydetmek için [IoT uzantılı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) Azure Bulut Kabuğu'nu kullanırsınız.

1. Aygıt kimliğini oluşturmak için Bulut Kabuğu'nda aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.
   > * Kaydolmakta olduğunuz aygıtın adı için *MyDevice'ın* gösterildiği gibi kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDevice
    ```

1. Az önce kaydettiğiniz aygıtın *aygıt bağlantı dizesini* almak için Bulut Kabuğu'nda aşağıdaki komutları çalıştırın:

   > [!NOTE]
   > *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDevice --output table
    ```

    Bu hızlı başlatmada daha sonra kullanmak üzere döndürülen aygıt bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

## <a name="ssh-to-a-device-via-device-streams"></a>Aygıt akışları üzerinden bir cihaza SSH

Bu bölümde, tünel SSH trafiğine uçlardan uca bir akış kurarsınız.

### <a name="run-the-device-local-proxy-application"></a>Aygıt-yerel proxy uygulamasını çalıştırma

1. Klasördeki `iothub_client/samples/iothub_client_c2d_streaming_proxy_sample`kaynak dosya **iothub_client_c2d_streaming_proxy_sample.c'yi** düzenleyin ve aygıt bağlantı dizenizi, hedef aygıtınızı IP/hostname'yi ve SSH bağlantı noktası 22'yi sağlayın:

   ```C
   /* Paste in your device connection string  */
   static const char* connectionString = "{DeviceConnectionString}";
   static const char* localHost = "{IP/Host of your target machine}"; // Address of the local server to connect to.
   static const size_t localPort = 22; // Port of the local server to connect to.
   ```

1. Örneği derle:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to cmake at root of repository
   cmake --build . -- /m /p:Configuration=Release
   ```

1. Derlenen programı aygıtta çalıştırın:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
   ./iothub_client_c2d_streaming_proxy_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_proxy_sample\Release
   iothub_client_c2d_streaming_proxy_sample.exe
   ```

### <a name="run-the-service-local-proxy-application"></a>Hizmet yerel proxy uygulamasını çalıştırma

"Nasıl çalışır" bölümünde ele alındığı gibi, Tünel SSH trafiğine uçlardan uca bir akış oluşturmak için her iki uçta (hem hizmet hem de cihaz taraflarında) yerel bir proxy gerekir. Genel önizleme sırasında, IoT Hub C SDK yalnızca aygıt tarafındaki aygıt akışlarını destekler. Hizmet yerel proxy oluşturmak ve çalıştırmak için aşağıdaki quickstarts birinde yönergeleri izleyin:

   * [C# proxy uygulamalarını kullanarak IoT Hub cihaz akışları üzerinden SSH/RDP](./quickstart-device-streams-proxy-csharp.md)
   * [Node.js proxy uygulamalarını kullanarak IoT Hub cihaz akışları üzerinden SSH/RDP](./quickstart-device-streams-proxy-nodejs.md)

### <a name="establish-an-ssh-session"></a>Bir SSH oturumu oluşturma

Hem aygıt hem de hizmet yerel proxy'leri çalıştırdıktan sonra, SSH istemci programınızı kullanın ve 2222 bağlantı noktasındaki servis yerel proxy'sine bağlanın (doğrudan SSH daemon yerine).

```cmd/sh
ssh {username}@localhost -p 2222
```

Bu noktada, SSH oturum açma penceresi kimlik bilgilerinizi girmenizi ister.

Aşağıdaki resimde, SSH daemon'a bağlanan aygıt yerel proxy'deki `IP_address:22`konsol çıktısı aşağıdaki gibidir:

![Aygıt-yerel proxy çıktısı](./media/quickstart-device-streams-proxy-c/device-console-output.png)

Aşağıdaki resimde SSH istemci programının konsol çıktısı gösterilmektedir. SSH istemcisi, hizmet yerel proxy'nin dinlediği bağlantı noktası 22'ye bağlanarak SSH daemon'a iletişim kurar:

![SSH istemci çıktısı](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub'ı kurdunuz, bir aygıtı kaydettiniz, IoT Hub üzerinden bir aygıt akışı oluşturmak için bir aygıt ve hizmet yerel proxy programı dağıttınız ve proxy'leri SSH trafiğini tünellemek için kullandınız.

Aygıt akışları hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışlarına genel bakış](./iot-hub-device-streams-overview.md)
