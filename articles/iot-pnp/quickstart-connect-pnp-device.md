---
title: IoT Tak ve Kullan Preview örnek cihaz kodunu IoT Hub 'ye bağlama (Windows) | Microsoft Docs
description: IoT Hub 'ına bağlanan Windows üzerinde IoT Tak ve Kullan önizleme örnek cihaz kodu oluşturun ve çalıştırın. Cihaz tarafından hub 'a gönderilen bilgileri görüntülemek için Azure IoT gezgin aracını kullanın.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 2dd5d197851b0090ac1af7bbde5a1ad1b951c785
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73569908"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-windows-to-iot-hub"></a>Hızlı başlangıç: Windows üzerinde çalışan örnek bir IoT Tak ve Kullan önizleme cihaz uygulamasını IoT Hub 'a bağlama

Bu hızlı başlangıçta, örnek bir IoT Tak ve Kullan cihaz uygulamasının nasıl oluşturulacağı, IoT Hub 'ınıza nasıl bağlanacağı ve hub 'a gönderdiği bilgileri görüntülemek için Azure IoT gezgin aracının nasıl kullanılacağı gösterilmektedir. Örnek uygulama C dilinde yazılır ve C için Azure IoT cihaz SDK 'sına dahildir. Bir çözüm geliştiricisi, herhangi bir cihaz kodunu görüntülemeye gerek olmadan IoT Tak ve Kullan cihazının yeteneklerini anlamak için Azure IoT gezgin aracını kullanabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için yerel makinenize aşağıdaki yazılımı yüklemeniz gerekir:

* Visual Studio  **C++ için derleme araçları ve** **NuGet Paket Yöneticisi bileşen** iş yükleri [için derleme araçları](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) . Ya da [Visual Studio (Community, Professional veya Enterprise)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 veya 2015 aynı iş yükleri yüklüyken zaten yüklüyse.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="install-the-azure-iot-explorer"></a>Azure IoT Gezginini yüklemeyi

Azure IoT gezgin aracını [en son sürüm](https://github.com/Azure/azure-iot-explorer/releases) sayfasından indirin ve yükleyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-an-iot-hub"></a>IoT Hub 'ı hazırlama

Ayrıca, bu hızlı başlangıcı tamamlayabilmeniz için Azure aboneliğinizde bir Azure IoT Hub 'ınız olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Genel Önizleme sırasında IoT Tak ve Kullan özellikleri yalnızca **Orta ABD**, **Kuzey Avrupa**ve **Japonya Doğu** bölgelerinde oluşturulan IoT Hub 'larında kullanılabilir.

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

Bir komut istemi açın. Aşağıdaki komutu yürüterek [Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunu kopyalayın:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

## <a name="build-the-code"></a>Kodu oluşturma

Oluşturduğunuz uygulama, IoT Hub 'ına bağlanan bir cihaza benzetir. Uygulama telemetri ve Özellikler gönderir ve komutları alır.

1. Cihaz SDK 'Sı kök klasöründe bir `cmake` alt dizini oluşturun ve bu klasöre gidin:

    ```cmd\sh
    cd <root folder>\azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Cihaz SDK 'sını ve üretilen kod Saplaması oluşturmak için aşağıdaki komutları çalıştırın:

    ```cmd\sh
    cmake ..
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > CMake derleyicinizi C++ bulamazsa, önceki komutu çalıştırdığınızda derleme hataları alırsınız. Bu durumda, [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs)bu komutu çalıştırmayı deneyin.

## <a name="run-the-device-sample"></a>Cihaz örneğini çalıştırma

IoT Hub cihazı bağlantı dizesini parametre olarak geçirerek uygulamanızı çalıştırın.

```cmd\sh
cd digitaltwin_client\samples\digitaltwin_sample_device\Release
copy ..\EnvironmentalSensor.interface.json .
digitaltwin_sample_device.exe "[IoT Hub device connection string]"
```

Cihaz uygulaması IoT Hub veri göndermeye başlar.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Kodu doğrulamak için Azure IoT gezginini kullanın

1. Azure IoT Gezginini açın, **uygulama yapılandırma** sayfasını görürsünüz.

1. IoT Hub bağlantı dizenizi girin ve **Bağlan**' a tıklayın.

1. Bağlandıktan sonra, cihaza genel bakış sayfasını görürsünüz.

1. Şirket deponuzu eklemek için **Ayarlar**' ı ve sonra **+ Yeni**' yi ve ardından **bağlı cihaz**' ı seçin.

1. Cihaza Genel Bakış sayfasında, daha önce oluşturduğunuz cihaz kimliğini bulun ve daha fazla ayrıntı görüntülemek için seçin.

1. IoT Tak ve Kullan temel özellikler, komutlar ve Telemetriyi görmek için, KIMLIK **urn: YOUR_COMPANY_NAME_HERE: Environmentalalgılayıcı: 1** olan arabirimi genişletin.

1. Cihazın gönderdiği telemetri verilerini görüntülemek için **telemetri** sayfasını seçin.

1. Cihaz tarafından raporlanan yazılabilir olmayan özellikleri görüntülemek için **Özellikler (yazılabilir değil)** sayfasını seçin.

1. Güncelleştirebilir yazılabilir özellikleri görüntülemek için **Özellikler (yazılabilir)** sayfasını seçin.

1. Özellik **adı**' nı genişletin, yeni bir adla güncelleştirin ve **yazılabilir özelliği Güncelleştir**' i seçin. 

1. Yeni adın **bildirilen özellik** sütununda görünür olduğunu görmek için sayfanın üst kısmındaki **Yenile** düğmesine tıklayın.

1. Cihazın desteklediği tüm komutları görüntülemek için **komut** sayfasını seçin.

1. **Yanıp sönen** komutunu genişletin ve yeni bir yanıp sönen zaman aralığı ayarlayın. Cihazda komutu çağırmak için **Gönder komutunu** seçin.

1. Komutun beklenen şekilde yürütüldüğünü doğrulamak için sanal cihaza gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta IoT Hub 'ına IoT Tak ve Kullan cihazını bağlamayı öğrendiniz. IoT Tak ve Kullan cihazlarınızla etkileşim kuran bir çözüm oluşturma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Nasıl yapılır: IoT Tak ve Kullan önizleme cihazına bağlanma ve bunlarla etkileşim kurma](howto-develop-solution.md)
