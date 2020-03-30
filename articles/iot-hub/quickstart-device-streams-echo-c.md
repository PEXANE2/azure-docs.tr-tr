---
title: Azure IoT Hub aygıt akışları yla C'deki aygıt uygulamasıyla iletişim kurun
description: Bu hızlı başlatmada, bir aygıt akışı üzerinden bir IoT aygıtıyla iletişim kuran bir C aygıt tarafı uygulaması çalıştırın.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 08/20/2019
ms.author: robinsh
ms.openlocfilehash: 52f9e6529329c5bb1abb176082294dc26e64baa3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78675537"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Quickstart: IoT Hub aygıt akışları (önizleme) üzerinden C'deki bir aygıt uygulamasıyla iletişim kurma

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IoT Hub şu anda bir [önizleme özelliği](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)olarak aygıt akışlarını destekler.

[IoT Hub aygıt akışları,](iot-hub-device-streams-overview.md) hizmet ve aygıt uygulamalarının güvenli ve güvenlik duvarı dostu bir şekilde iletişim kurmasına olanak tanır. Genel önizleme sırasında C SDK yalnızca aygıt tarafındaki aygıt akışlarını destekler. Sonuç olarak, bu hızlı başlatma yalnızca aygıt tarafındaki uygulamayı çalıştırmak için yönergeleri kapsar. İlgili hizmet tarafı uygulamasını çalıştırmak için şu makalelere bakın:

* [IoT Hub cihaz akışları aracılığıyla C#'daki aygıt uygulamalarıyla iletişim kurun](./quickstart-device-streams-echo-csharp.md)

* [IoT Hub cihaz akışları aracılığıyla Düğüm.js'deki aygıt uygulamalarına iletişim kurun](./quickstart-device-streams-echo-nodejs.md)

Bu hızlı başlatmadaki aygıt tarafındaki C uygulaması aşağıdaki işlevselliği içerir:

* Bir IoT aygıtına aygıt akışı kurun.

* Hizmet tarafındaki uygulamadan gönderilen verileri alın ve geri döndürün.

Kod, bir aygıt akışının başlatma işlemini ve veri göndermek ve almak için nasıl kullanılacağını gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşullara ihtiyacınız vardır:

* C++ iş yükü etkinleştirilmiş **Masaüstü geliştirme özelliğiyle** Visual Studio [2019'u](https://www.visualstudio.com/vs/) yükleyin.

* En son [Git](https://git-scm.com/download/) sürümünü yükleyin.

* Bulut Kabuğu örneğinize Azure CLI için Azure IoT Uzantısı'nı eklemek için aşağıdaki komutu çalıştırın. IOT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS)'ye özel komutları ekler.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

   [!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

Aygıt akışlarının önizlemesi şu anda yalnızca aşağıdaki bölgelerde oluşturulan IoT hub'ları için desteklenir:

  * Orta ABD
  * ORTA ABD EUAP
  * Kuzey Avrupa
  * Güneydoğu Asya

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama

Bu hızlı başlangıç [için, C için Azure IoT aygıtı SDK'yı](iot-hub-device-sdk-c-intro.md)kullanırsınız. GitHub'dan [Azure IoT C SDK'yı](https://github.com/Azure/azure-iot-sdk-c) klonlamak ve oluşturmak için kullanılan bir geliştirme ortamı hazırlarsınız. GitHub'daki SDK, bu hızlı başlatmada kullanılan örnek kodu içerir.

   > [!NOTE]
   > Bu yordamı başlatmadan önce Visual Studio'nun **C++** iş yüküyle Masaüstü geliştirme ile yüklü olduğundan emin olun.

1. CMake [yapı sistemini](https://cmake.org/download/) indirme sayfasında açıklandığı gibi yükleyin.

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

   * Windows'da Visual Studio için Geliştirici [Komut Komut Ustem'i](/dotnet/framework/tools/developer-command-prompt-for-vs)açın. Visual Studio sürümünüz için komutu çalıştırın. Bu hızlı başlangıç Visual Studio 2019'u kullanır. Bu komutlar *cmake* dizininde simüle edilen aygıt için bir Visual Studio çözümü oluşturur.

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

Bağlanabilmesi için bir aygıtı IoT hub'ınıza kaydettirmelisiniz. Bu bölümde, simüle edilmiş bir aygıtı kaydetmek için [IoT Uzantılı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) Azure Bulut Kabuğu'nu kullanırsınız.

1. Aygıt kimliğini oluşturmak için Bulut Kabuğu'nda aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > * *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.
   > * Kaydolmakta olduğunuz aygıtın adı için *MyDevice'ın* gösterildiği gibi kullanılması önerilir. Aygıtınız için farklı bir ad seçerseniz, bu makale boyunca bu adı kullanın ve bunları çalıştırmadan önce örnek uygulamalardaki aygıt adını güncelleştirin.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDevice
    ```

1. Az önce kaydettiğiniz aygıtın *aygıt bağlantı dizesini* almak için Bulut Kabuğu'nda aşağıdaki komutu çalıştırın:

   > [!NOTE]
   > *YourIoTHubName* yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDevice --output table
    ```

    Bu hızlı başlatmada daha sonra kullanmak üzere döndürülen aygıt bağlantı dizesini not edin. Aşağıdaki örneğe benzer şekilde görünür:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Cihaz akışları üzerinden cihaz ve hizmet arasında iletişim kurma

Bu bölümde, hem aygıt tarafı uygulamasını hem de hizmet tarafı uygulamasını çalıştırın ve ikisi arasında iletişim kurarsınız.

### <a name="run-the-device-side-application"></a>Aygıt tarafındaki uygulamayı çalıştırma

Aygıt tarafı uygulamasını çalıştırmak için aşağıdaki adımları izleyin:

1. `iothub_client/samples/iothub_client_c2d_streaming_sample` Klasördeki **iothub_client_c2d_streaming_sample.c** kaynak dosyasını düzenleyerek ve aygıt bağlantı dizenizi ekleyerek aygıt kimlik bilgilerinizi sağlayın.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "{DeviceConnectionString}";
   ```

1. Kodu aşağıdaki komutlarla derle:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

1. Derlenen programı çalıştırın:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>Hizmet tarafındaki uygulamayı çalıştırma

Daha önce de belirtildiği gibi, IoT Hub C SDK yalnızca aygıt tarafındaki aygıt akışlarını destekler. Eşlik eden servis tarafı uygulamasını oluşturmak ve çalıştırmak için aşağıdaki hızlı başlangıçlardan birinde yönergeleri izleyin:

* [IoT Hub cihaz akışları aracılığıyla C#'daki bir cihaz uygulamasıyla iletişim kurun](./quickstart-device-streams-echo-csharp.md)

* [IoT Hub cihaz akışları aracılığıyla Node.js'deki bir cihaz uygulamasıyla iletişim kurun](./quickstart-device-streams-echo-nodejs.md)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT hub'ı kurdunuz, bir aygıtı kaydettiniz, aygıttaki C uygulaması yla hizmet tarafındaki başka bir uygulama arasında bir aygıt akışı oluşturdunuz ve uygulamalar arasında veri göndermek için akışı kullandınız.

Aygıt akışları hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Cihaz akışlarına genel bakış](./iot-hub-device-streams-overview.md)
